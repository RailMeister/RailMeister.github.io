---
layout: post
title: How to Extend JavaScript with Your Own String Prototypes
---

Everybody wishes that Javascript can do more than it already can, and then thinks, well that's gonna be way too hard. News flash! It's actually quite easy! On that note, I will walk you through creating an `endsWith()` string method. It will check if the string ends with the specified substring, hence the name _endsWith_.

### Making the Prototype:
First you will want to make sure JS knows that this is to be added to the String type. Do so like this:

```javascript
String.prototype.endsWith = (substring) => {
```

Then, if the specified substring has the same length as the string we're operating on, we want to stop in our tracks. Inside the function, do this:

```javascript
    if(substring.length > this.length) return false;
```

Then if it passes that test, we want to go ahead and return our True or false value and end the function:

```javascript
    return this.substr(this.length - substring.length) === substring;
};
```

## The completed code:

```javascript
String.prototype.endsWith = (substring) => {
    if(substring.length > this.length) return false;
    return this.substr(this.length - substring.length) === substring;
};
```

### Final thoughts:
This is very handy if you want to do something like check the filetype of a file you're programattically editing in Node or something like that. I used it in my [Downloader CLI](https://www.npmjs.com/package/downloader-cli), which is a CLI for downloading any file on the web. If you use it, my only request is that you don't use it illegally. Thanks for reading!