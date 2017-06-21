---
layout: post
title: Angular and the Internet Explorer!
---

<img src="{{ site.baseurl }}/images/angular-IE/mycode-or-IE.jpg" alt="Is it my code or IE?" title="Is it my code or IE?">



When it comes to web development, we all agree to the fact that we all hate the Internet Explorer. It is not due to the fact that it is sluggish. But due to the fact that It has lot of compatibility issues. First issue which I came during my internship at Wavenet was the directives issue, in which we faced a trouble in passing URLs to a directive.

### Background

The issue was only with the Internet Explorer, It worked smoothly in both Chrome and Firefox. Since the product was intended for use by people who use Internet Explorer, solving the issue was a MUST.

The particular directive contained an embed tag and source for embed tag content had to be passed through the angular app dynamically. The issue prevailed only during dynamic URL passing. Once we had used a hard-coded URL, we had no issue.

### Diving more ...

First we checked for the compatibility for the embed tag and for any syntax issue. What we found was that it was supported by Internet Explorer 9 and all subsequent versions. Further, since we needed to use an audio, we tried the using the <audio> tag, which gave the same result with no progress.

After more testing we were able to realize the fact that issue was only for URLs, if we pass some other content, for instance a dummy text it would update the DOM and the content would be available as required.


``` javascript

var app = angular.module('test', ['PlayerModule']);

var controller = angular.controller('testController', function($scope){
	$scope.name
});

var PlayerModule = angular.module('PlayerModule', []);
PlayerModule.directive('player', function(){
    return {
        scope : {
            audio : @
        },
        template : "<embed url='{{ audio }}'> </embed>   "
    }
})
```

The issue became evident and we realized the fact that it was due to IE not trusting the content passed through javascript and restricting any DOM updates. So we had to WIN THEIR TRUST.

### Winning the trust

Angular has a Strict Contextual Escaping ($sce) module for rendering only the trusted values in the DOM which is aimed at enhancing security of the Angular based applications. By default Angular is shipped with $sce enabled default. Disabling $sce would make your awesome app  vulnerable to any attacks. (XSS and stuff I guess)

The complete documentation can be found at
[Strict Contextual Escaping - angular docs](https://docs.angularjs.org/api/ng/service/$sce) . For the purpose of the blog post, I would only demonstrate the way to fix the issue. ;)

For winning the trust, all we have to do is indicate the app to trust the value passed as the parameter to be a trusted entity. It brings the directive code down to

``` javascript

var PlayerModule = angular.module('PlayerModule', []);
PlayerModule.directive('player', function($sce){
    return {
        scope : {
            audio : @
        },
        template : "<embed url='{{ $sce.trustAsUrl(audio) }}'> </embed>   "
    }
})

```

### Conclusion


Even we naturally tend to hate Internet Explorer ( and also Edge), I think it is a good feature to include security mechanism to the web applications running on your browser. Even though we hate it, It would automatically protect our application from any vulnerabilities and also would make us to worry about possible security issues and vulnerabilities.

<img src="{{ site.baseurl }}/images/angular-IE/thats-ok.jpg" alt="Thats Ok. :|" title="Thats Ok. :|">


Cheers !!!

**mahanama94**
