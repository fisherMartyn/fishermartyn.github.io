---
layout: post
title: iOS中的单例
categories: blog
excerpt: 单例模式实现记录
tags: [iOS]
---

ios中的单例无处不在，在设计单例的时候需要考虑其他入口拦截等。

在这里把实现的代码记录一下：

头文件：

{% highlight Objective-C %}

#import 

@interface MTiMerLogin : NSObject
+(MTiMerLogin *)sharedInstance;
-(MTiMerLogin *)sharedInstance;
@end

{% endhighlight %}

实现文件：


{% highlight Objective-C %}
#import "MTiMerLogin.h"

@implementation MTiMerLogin
+(MTiMerLogin *)sharedInstance
{
    static MTiMerLogin *singleton;
    static dispatch_once_t singletoken;
    dispatch_once(&singletoken,^{
        singleton = [[self alloc] initSingle];
    });
    return singleton;
}

-(MTiMerLogin *)sharedInstance
{
    return [MTiMerLogin sharedInstance];
}

-(instancetype) initSingle
{
    self = [super init];
    return self;
}

-(instancetype) init
{
    return [MTiMerLogin sharedInstance];
}
@end
{% endhighlight %}

需要注意的是，需要重写init来防止对alloc init这种方式的调用。


