---
title: "Imperative and Declarative Style Programming"
comments: true
excerpt: "In this article, I am going to try to explain the difference between imperative and declarative style programming"
header:
  teaser: "assets/images/equality.png"
  og_image: /assets/images/page-header-og-image.png
  overlay_image: /assets/images/unsplash-image-60.jpeg
  overlay_filter: 0.5 #rgba(255, 0, 0, 0.5)
  caption: "Photo by [Omid Armin](https://unsplash.com/photos/edANaB0ZFVo) on Unsplash"
  #cta_label: "More Info"
  #cta_url: "https://unsplash.com"
categories:
  - programming-style
tags:
  - declarative
  - imperative
last_modified_at: 2022-02-23T15:12:19-04:00
toc: true
toc_label: "CONTENT"
---



**ÖNEMLİ :** The notes that I took for myself. I hope they will help you too.. Each resource that I used is added as reference at the end of the page.
{: .notice}

## Imperative Style Programming

According to the wiki definition, **imperative programming style** focuses on describing how a program operates. Actually it sounds good but not very descriptive. I have found a better one at a live event I attended via O'reilly.

According to Venkat Subramaniam, Imperative Style Programming tells us **what a program do and also how to do it**. I want to summarize this definition like this ->

### What to do - How to do

For instance;

```java
String[] names = {"hasan", "tom", "phil", "bev", "mike"};
for (int i = 0; i < names.length; i++) {
    if (names[i].equals("hasan")) {
        System.out.println("it is found");
    }
}
```
In the above example, we can easily understand what this code does.

**What is it doing :** it is trying to find a special name in the array.

**How is it doing :** it is doing by using a traditional for-loop. It defines a variable in the body of the for-loop and does this by incrementing it one at a time and comparing it to the name it is looking for.

In summary, we show step by step *how we do* the necessary actions while finding this name(*what we do*).

## Declarative Style Programming

The declarative programming style focuses on what the program should accomplish without specifying all the details of how the program should achieve the result. Again according to Venkat Subramaniam;

It tells us **what a program do but not how to do it**.

### What to do - NOT How to do

For instance;

```java
List<String> listNames = Arrays.asList("hasan", "tom", "phil", "bev");
if (listNames.contains("hasan")) {
    System.out.println("it is found");
}
```
**What is it doing :** it is trying to find a special name in the array.

**NOT "how is it doing" :** The difference out of the imperative programming style, declarative style hides the details of "**how to do**" parts. As you can see, *contains* method does whole jobs for us and hides the complexity.

## Reference:
* [https://en.wikipedia.org/wiki/Imperative_programming](https://en.wikipedia.org/wiki/Imperative_programming)
* [https://en.wikipedia.org/wiki/Declarative_programming](https://en.wikipedia.org/wiki/Declarative_programming)
* [https://learning.oreilly.com/live-events/understanding-functional-programming/0636920457435/0636920058831/](https://learning.oreilly.com/live-events/understanding-functional-programming/0636920457435/0636920058831/)
