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



**IMPORTANT :** The notes that I took for myself. I hope they will help you too.. Each resource that I used is added as reference at the end of the page.
{: .notice}

## Imperative Style Programming

According to the wiki definition, **imperative programming style** focuses on describing how a program operates. Actually it sounds good but not very descriptive. I have found a better one at a live event I attended via O'reilly.

According to Venkat Subramaniam, Imperative Style Programming tells us **what a program do and also how to do it**. I want to summarize this definition like Venkat did ->

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

**How is it doing :** it is doing by using a standard for-loop. It defines a variable in the for-loop and does this by incrementing it one at a time and comparing it to the name it is looking for.

In summary, we show step by step *how we do* the necessary actions while finding this name(*what we do*).

## Declarative Style Programming

The declarative programming style focuses on what the program should accomplish without specifying all the details of how the program should achieve the result. Again according to Venkat;

It tells us **what a program do but not how to do it**. Again, I would like to summarize this definition like this ->

### What to do - NOT How to do

For instance;

```java
List<String> listNames = Arrays.asList("hasan", "tom", "phil", "bev");
if (listNames.contains("hasan")) {
    System.out.println("it is found");
}
```
**What is it doing :** well, it is trying to find a special name in the array like previous one.

**NOT "how is it doing" :** The difference out of the imperative programming style, declarative style hides the details of "**how to do**" parts. As you can see, *contains* method does whole jobs for us and hides the complexity.

## An example to show the difference

Actually, standard and enhanced for loops are good examples to show difference between imperative and declarative style programming.  

In both cases, it's obvious what we're going to do. Our goal is to print the elements of the array to the console. What about "**how to do**" parts???

**In the standard for loop;** A counter, *i*, is declared and initialized to 0. A boolean expression compares *i* with the length of the *names* array. if *i<names.length*, the code block is executed. *i* is incremented by one at the end of each code block. Within the code block, *i* is used as the array index. As you can see, We explain **how to do** it step by step.

**In the enhanced for loop;** A string variable, *name*, is declared to hold each element of the array. But we don't know **how** this loop does all these operations.

<figure style="width: 600px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2022-02-23-imperative-and-declarative-programming/forloops.png" alt="for loops example">
  <figcaption>Figure 1 - created by me by using https://www.lucidchart.com </figcaption>
</figure>

## Reference:
* [https://en.wikipedia.org/wiki/Imperative_programming](https://en.wikipedia.org/wiki/Imperative_programming)
* [https://en.wikipedia.org/wiki/Declarative_programming](https://en.wikipedia.org/wiki/Declarative_programming)
* [Understanding Functional Programming - Venkat Subramaniam](https://learning.oreilly.com/live-events/understanding-functional-programming/0636920457435/0636920058831/)
