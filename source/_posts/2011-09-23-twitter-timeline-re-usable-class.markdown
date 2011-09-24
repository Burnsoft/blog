---
layout: post
title: "Twitter Timeline Re-usable Class"
date: 2011-09-23 18:32
comments: true
categories: 
published: true
---

I'm developing a Twitter tool and needed to grab the latest tweets in the public timeline and chuck (technical term) in a tableview for testing.
  
I'd also appreciate any comments if anyone spots any bad coding habits or would like to improve it.
 
Copy the code into your project making sure the calling class registers for the <latestTweetDelegate> in its header file.
 
 
 
Then instantiate the class using your URL, I just use the standard Twitter Public Timeline Timeline API:
 
{% codeblock somewhere in your class%}
latestTweets *lt = [[latestTweets alloc] initWithTwitterURL:@"http://api.twitter.com/1/statuses/public_timeline.json?include_entities=true"];
lt.delegate = self;
 
[lt release];
 {% endcodeblock %}

 <!--more-->

Your calling class needs to implement the delegate call back method to capture the returned array:

{% codeblock %}
- (void)returnedArray:(NSArray*)tArray;
{
//self.yourArray = tArray;
}
{% endcodeblock %}


As it uses the JSON parser you'll need to add that to your project as well:
 
https://github.com/stig/json-framework/
 
Hope you find it useful.
 
Nik
 
 
{% codeblock latestTweets header %}
#import <Foundation/Foundation.h>
 
@protocol latestTweetsDelegate
- (void)returnedArray:(NSArray*)tArray;
@end
 
@interface latestTweets : NSObject {
        NSMutableData *responseData;
        NSMutableArray *resultsArray;
        id<latestTweetsDelegate> delegate;
}
@property (nonatomic, retain) NSMutableArray *resultsArray;
@property (retain,nonatomic) id<latestTweetsDelegate> delegate;
 
- (id)initWithTwitterURL:(NSString *)twitterURL;
 
@end
{% endcodeblock %}
 
 
 
{% codeblock latestTweets.m %}
#import "latestTweets.h"
#import "SBJson.h"
 
@implementation latestTweets
@synthesize resultsArray, delegate;
 
- (id)initWithTwitterURL:(NSString *)twitterURL;
{
    self = [super init];
    if (self) {
                responseData = [[NSMutableData data] retain];
                NSURLRequest *request = [NSURLRequest requestWithURL:[NSURL URLWithString:twitterURL]];
                [[NSURLConnection alloc] initWithRequest:request delegate:self];
        }
        return self;
}
- (void)connection:(NSURLConnection *)connection didReceiveResponse:(NSURLResponse *)response {
}
- (void)connection:(NSURLConnection *)connection didReceiveData:(NSData *)data {
                [responseData appendData:data];
}
- (void)connection:(NSURLConnection *)connection didFailWithError:(NSError *)error {
        [connection release];
        NSLog(@"Connection failed: %@", [error description]);
}
 
- (void)connectionDidFinishLoading:(NSURLConnection *)connection {
                NSString *responseString = [[NSString alloc] initWithData:responseData encoding:NSUTF8StringEncoding];
                [responseData release];
                NSArray *newData = [responseString JSONValue];
                [responseString release];      
            [self.delegate returnedArray:newData];
}
 
#pragma mark Memory Management
 
- (void)dealloc {
 
        self.resultsArray = nil;
        responseData = nil;
        [respondeData release];
        self.delegate = nil;
        [super dealloc];
}
 
@end
{% endcodeblock %}
