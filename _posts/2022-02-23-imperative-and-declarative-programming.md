---
title: "Imperative and Declarative Style Programming (Programming Style Part 1)"
comments: false
excerpt: "In this article I am going to try to explain the difference between imperative and declarative style programming"
header:
  teaser: "assets/images/2022-02-23-imperative-and-declarative-programming/imp.webp"
  #og_image: /assets/images/page-header-og-image.png
  og_image: /assets/images/2022-02-23-imperative-and-declarative-programming/imp.webp
  overlay_image: /assets/images/unsplash-image-60.webp
  overlay_filter: 0.5 #rgba(255, 0, 0, 0.5)
  caption: "Photo by [Omid Armin](https://unsplash.com/photos/edANaB0ZFVo) on Unsplash"
  #cta_label: "More Info"
  #cta_url: "https://unsplash.com"
categories:
  - programming-style
tags:
  - programming-style
  - declarative
  - imperative
last_modified_at: 2022-02-23T15:12:19-04:00
toc: true
toc_label: "CONTENT"
---



**IMPORTANT :** The notes that I took for myself. I hope they will help you, too. Each resource that I used is added as reference at the end of the page.
{: .notice}

## Imperative Style Programming

According to the wiki definition, **imperative programming style** focuses on describing how a program operates. Actually it sounds good but not very descriptive. I have found a better one at a live event I attended via O'reilly.

According to Venkat Subramaniam, Imperative Style Programming tells us **what a program does and also how it does it**. I want to summarize this definition like Venkat did ->

### What to do - How to do it

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

**How is it doing it? :** it is doing it by using a standard for-loop. It defines a variable in the for-loop and does this by incrementing it one at a time and comparing it to the name it is looking for.

In summary, we show step by step *how we do* the necessary actions to find this name(*what we do*).

### Note:

At the core of the familiar imperative style are **mutability** and **command-driven** programming. We create variables or objects and modify their state along the way. We also provide detailed commands or instructions to execute, such as create a loop index, increment its value, check if we reached the end, update the nth element of an array, and so on.

## Declarative Style Programming

The declarative programming style focuses on what the program should accomplish without specifying all the details of how the program should achieve the result. Again according to Venkat;

It tells us **what a program does but not how to do it**. Again, I would like to summarize this definition like this ->

### What to do - NOT How to do it

For instance;

```java
List<String> listNames = Arrays.asList("hasan", "tom", "phil", "bev");
if (listNames.contains("hasan")) {
    System.out.println("it is found");
}
```
**What is it doing :** well, it is trying to find a special name in the array like previous one.

**NOT "how is it doing it" :** The difference out of the imperative programming style, declarative style hides the details of "**how to do**" parts. As you can see, the *contains* method does the whole job for us and hides the complexity.

## Comparison of enhanced and standard loops in terms of programming styles

In which classification should we evaluate **enhanced** and **standard** for loops? Let's brainstorm together...

In both cases, it's obvious what we're going to do. Our goal is to print the elements of the array to the console. What about "**how to do**" parts???

**In the standard for loop;** A counter, *i*, is declared and initialized to 0. A boolean expression compares *i* with the length of the *names* array. if *i<names.length*, the code block is executed. *i* is incremented by one at the end of each code block. Within the code block, *i* is used as the array index. As you can see, We explain **how to do** it step by step.

**In the enhanced for loop;** A string variable, *name*, is declared to hold each element of the array. But we don't know **how** this loop does all these operations.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/2022-02-23-imperative-and-declarative-programming/forloops.webp"  width="100%" height="100%" loading="lazy" alt="for loops example">

Actually both the **standard for loop** *(for i = 0...)* and the **enhanced for loop** *(for var x : ...)* are **imperative style**. The **enhanced for loop** is really a wrapper around
`iterator.hasNext()` and `iterator.next()`. That is, under the hood this form of iteration uses the `Iterator` interface and calls into its `hasNext` and `next` methods. Furthermore, from the **enhanced for loop** we can do `break` and `continue` with an if condition, and that is where we see the **imperative** nature being
enhanced clearly.

> ENHANCED for loop is more declarative than the STANDARD for loop

We can say that the enhanced for loop moves the needle towards **declarative style**. However, it is not entirely in the land of **declarative style**. Both the **standard loop** and the **enhanced loops** are external iterators which are **imperative** in nature. In both the loops we control the flow using if `break` or if `continue`. We do a little bit less of the controlling in enhanced for loop compared to the standard for loop. Nevertheless both allow for imperative control.


## Reference:
* [Imperative programming](https://en.wikipedia.org/wiki/Imperative_programming)
* [Declarative programming](https://en.wikipedia.org/wiki/Declarative_programming)
* [Understanding Functional Programming - Venkat Subramaniam](https://learning.oreilly.com/live-events/understanding-functional-programming/0636920457435/0636920058831/)
* [Functional Programming in Java](https://learning.oreilly.com/library/view/functional-programming-in/9781941222690/)
