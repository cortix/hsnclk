---
title: "Higher Order Functions (Programming Style Part 2)"
comments: false
excerpt: "In this article, I am going to try to explain what the higher-order functions are, first-class citizen and first-class function"
header:
  teaser: "/assets/images/2022-02-25-higher-order-functions/higher.webp"
  #og_image: /assets/images/page-header-og-image.png
  og_image: /assets/images/2022-02-25-higher-order-functions/higher.webp
  overlay_image: /assets/images/2022-02-25-higher-order-functions/higher_f.webp
  overlay_filter: 0.5 #rgba(255, 0, 0, 0.5)
  caption: "Photo by [Etienne Girardet](https://unsplash.com/photos/Xh6BpT-1tXo) on Unsplash"
  #cta_label: "More Info"
  #cta_url: "https://unsplash.com"
categories:
  - programming-style
tags:
  - programming-style
  - higher order functions
  - first-class citizens
  - first-class functions
last_modified_at: 2022-02-23T15:12:19-04:00
toc: true
toc_label: "CONTENT"
---



**IMPORTANT :** The notes that I took for myself. I hope they will help you too.. Each resource that I used is added as reference at the end of the page.
{: .notice}

First things first, we have to know some definitions to understand higher order functions. I would like to start with the term of "**first-class citizen**"

## First-class citizen

According to the wiki definition, in programming language design, a **first-class citizen** (also type, object, entity, or value) in a given programming language is an entity which supports all the operations generally available to other entities. These operations typically include being **passed as an argument**, **returned from a function**, and **assigned to a variable**.

According to this definition, anything can be *first-class citizen* if it supports the **highlighted operations** in the above. I want to put a comma here and move on to the next definition which is **first-class function**

## First-class function

 We have a little bit idea what the *first-class citizen* is. We can say that a programming language is said to have **first-class functions** if it treats functions as *first-class citizens*.

## Higher Order Functions

In normal **functions** in general;

* We can pass objects to functions
* We can create objects in functions
* We can also return objects from functions

In **higher order functions**;

* We can pass functions to functions
* We can create functions in functions
* We can also return functions from functions

I would like to share two code examples to clarify.

For instance;

```java
Thread t1 = new Thread(new Runnable() {
    @Override
    public void run() {
        System.out.println("This is a thread");
    }
});

t1.start();
```

In the above example, we pass a `Runnable` object to the `Thread` constructor. In here, an **object** treats as *first-class citizen*.

```java
Thread t2 = new Thread( () -> System.out.println("Another thread"));

t2.start();
```

But, we can treat functions or codes as *first class citizens* as well. In this example we pass a **function** which is *lambda expression*(anonymous) as an argument to the `Thread` constructor. This makes it *higher order function* because it receives another function as its parameter.



## Reference:
* [First-class citizen](https://en.wikipedia.org/wiki/First-class_citizen)
* [First-class function](https://en.wikipedia.org/wiki/First-class_function)
* [Higher-order function](https://en.wikipedia.org/wiki/Higher-order_function)
* [Anonymous function](https://en.wikipedia.org/wiki/Anonymous_function)
* [Understanding Functional Programming - Venkat Subramaniam](https://learning.oreilly.com/live-events/understanding-functional-programming/0636920457435/0636920058831/)
