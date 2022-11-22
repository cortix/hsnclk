---
title: "Algorithms Part 1 - Selection Sort"
comments: false
excerpt: "In this section, I am going to explain what the selection sort algorithm is and how its performance behaves in worst, best and average case scenario"
header:
  teaser: "/assets/images/2022-11-11-algorithms-part1-selection-sort/selection-sort.webp"
  #og_image: /assets/images/page-header-og-image.png
  og_image: /assets/images/2022-11-11-algorithms-part1-selection-sort/selection-sort.webp
  overlay_image: /assets/images/unsplash-image-58.webp
  overlay_filter: 0.5 #rgba(255, 0, 0, 0.5)
  caption: "Photo by [zero take](https://unsplash.com/photos/jSB9PWaxhXo) on Unsplash"
  #cta_label: "More Info"
  #cta_url: "https://unsplash.com"
categories:
  - algorithms
tags:
  - selection Sort algorithm
last_modified_at: 2022-02-23T15:12:19-04:00
toc: false
toc_label: "CONTENT"
classes: wide
---



**IMPORTANT :** The notes that I took for myself. I hope they will help you too.. Each resource that I used is added as reference at the end of the page.
{: .notice}

# Selection Sort
---
The following card game simply explains how the **selection algorithm** works. The game is very simple; we want to sort the **scattered cards** <u>from smallest to largest</u> by comparing them with each other.

To do this; first we need to start with the element in the **first position** and compare this element with other elements to find the **smallest** element. <u>If we find the smallest element in the part of the list other than the first position, we need to <b>swap</b> it with the first element.</u> Then we can continue the similar process up to the **2nd**, **3rd** ..... and till the **penultimate** element of the list, respectively.

<div class="notice--warning" markdown="1">
<h4 class="no_toc"><i class="fas fa-comment"></i> Note:</h4>
---
If you want to see how it works step by step, you can press the **STEP** button.

Or click the **PLAY** button to run the game by itself.

Finally, if you want to shuffle the cards again and restart the game, you can press the **SHUFFLE** button.

</div>

<iframe sandbox="allow-popups allow-same-origin allow-scripts allow-top-navigation" src="https://www.khanacademy.org/computer-programming/program/4808854910533632/embedded?embed=yes&amp;author=no&amp;editor=no&amp;width=688&amp;buttons=no&amp;settings=%7B%22sortType%22%3A%22selection%22%7D" class="perseus-scratchpad" allowfullscreen="" style="height: 450px; width: 100%; border-top-width: 0px;
border-right-width: 0px;
border-bottom-width: 0px;
border-left-width: 0px;"></iframe>

## An Example of Selection Sort

For example we have an unsorted list like this;

<h1><span style="color: blue">9 - 4 - 3 - 1</span></h1>
{: .text-left}

### Our purpose
Our plan is to sort from smallest to largest by comparing the numbers with each other.

### How to apply selection sort algorithm

1. I need **nested for loops** to apply the **selection sort** to my list. My **outer loop** starts from the beginning of the list, keeping track of <u>the position I want to hold</u>. At the very beginning of the algorithm, this position is of course "**0**". On the other hand, my **inner loop** checks the other items other than the item in the position that the outer loop holds **to find the smallest item**.  
2. For each new smallest value found, int "**smallest**" value will be updated throughout the inner loop.
3. You will notice that the values for "**smallest**" and "**hasSmallestBeenFounded**" are updated only before you get inside the inner loop. This is because when analyzing the first value of the outer loop, we will already find the smallest value. Our goal is to find the next smallest value. Therefore, it is necessary to update these values according to the next value in the loop.
4. If even one smallest value is found during this time, the boolean value "**hasSmallestBeenFounded**" value is marked as `true`.
5. This part also crutial. There is an if statement inside in the inner loop. **if (list[j + 1] < list[smallest])**. Here we compare the value **after** smallest with the **smallest value** itself. If we encounter a smaller value, we update this value to the new smallest.
6. If the inner loop finds even one smallest value, it enters the **if statement** outside the inner loop, which replaces the smallest value found with the value in the first position(which is of course the position that I hold via outer loop).
7. When the first position of my outer loop is replaced by the smallest(if any), we repeat the same process with the 2nd position of my outer loop by increasing the **i** number by **one**. Then, the same operations continue until the **penultimate position(list.length-1)** of my outer loop. As you can see, `int` **firstPosition** value is updated each time by increasing the i number by one.

---

```java
public class SelectionSort {
    public static void main(String[] args) {
        int[] list = {9,4,3,1};
        int smallest = 0;
        int firstPosition = 0;
        boolean hasSmallestBeenFounded = false;
        for(int i=0; i< list.length-1; i++) {
            hasSmallestBeenFounded=false;
            smallest = i;
            for(int j=i; j< list.length-1; j++){
                if (list[j + 1] < list[smallest]) {
                    smallest = j + 1;
                    hasSmallestBeenFounded = true;
                }
            }
            if(hasSmallestBeenFounded){
                firstPosition = list[i];
                list[i] = list[smallest];
                list[smallest] = firstPosition;
            }
        }
    }
}
```


<div class="notice--warning" markdown="1">
<h4 class="no_toc"><i class="fas fa-comment"></i> Note:</h4>
---
If you want to run the code in the online tool step by step, you can click the link below.

[link](https://pythontutor.com/render.html#code=public%20class%20SelectionSort%20%7B%0A%20%20%20%20public%20static%20void%20main%28String%5B%5D%20args%29%20%7B%0A%20%20%20%20%20%20%20%20int%5B%5D%20list%20%3D%20%7B9,4,3,1%7D%3B%0A%20%20%20%20%20%20%20%20int%20smallest%20%3D%200%3B%0A%20%20%20%20%20%20%20%20int%20firstPosition%20%3D%200%3B%0A%20%20%20%20%20%20%20%20boolean%20hasSmallestBeenFounded%20%3D%20false%3B%0A%20%20%20%20%20%20%20%20for%28int%20i%3D0%3B%20i%3C%20list.length-1%3B%20i%2B%2B%29%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20hasSmallestBeenFounded%3Dfalse%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20smallest%20%3D%20i%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20for%28int%20j%3Di%3B%20j%3C%20list.length-1%3B%20j%2B%2B%29%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20if%20%28list%5Bj%20%2B%201%5D%20%3C%20list%5Bsmallest%5D%29%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20smallest%20%3D%20j%20%2B%201%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20hasSmallestBeenFounded%20%3D%20true%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20if%28hasSmallestBeenFounded%29%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20firstPosition%20%3D%20list%5Bi%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20list%5Bi%5D%20%3D%20list%5Bsmallest%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20list%5Bsmallest%5D%20%3D%20firstPosition%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%0A%7D&cumulative=false&curInstr=2&heapPrimitives=nevernest&mode=display&origin=opt-frontend.js&py=java&rawInputLstJSON=%5B%5D&textReferences=false)

In the code above, I just used one list instead of using both sorted and unsorted list. That is, I reached the solution by sorting a single list within itself. If you want, you can divide it 2 sublist like that;

| Sorted sublist | Unsorted sublist | Smallest element in unsorted list |
|:--------|:-------:|--------:|
| ()   | (9,4,3,1)   | 1   |
| (1)   | (9,4,3)   | 3   |
| (1,3)   | (9,4)   | 4   |   
| (1,3,4)   | (9)   | 9   |   
| (1,3,4,9)   | ()   |    |   
{: rules="groups"}

</div>

<!-- But sometimes embedded code may crash due to **massive traffic overload**, if you cannot see the code properly, you can click the  -->

<!-- <iframe width="100%" height="800" frameborder="0" src="https://pythontutor.com/iframe-embed.html#code=public%20class%20SelectionSort%20%7B%0A%20%20%20%20public%20static%20void%20main%28String%5B%5D%20args%29%20%7B%0A%20%20%20%20%20%20%20%20int%5B%5D%20list%20%3D%20%7B9,4,3,1%7D%3B%0A%20%20%20%20%20%20%20%20int%20smallest%20%3D%200%3B%0A%20%20%20%20%20%20%20%20int%20firstPosition%20%3D%200%3B%0A%20%20%20%20%20%20%20%20boolean%20hasSmallestBeenFounded%20%3D%20false%3B%0A%20%20%20%20%20%20%20%20for%28int%20i%3D0%3B%20i%3C%20list.length-1%3B%20i%2B%2B%29%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20hasSmallestBeenFounded%3Dfalse%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20smallest%20%3D%20i%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20for%28int%20j%3Di%3B%20j%3C%20list.length-1%3B%20j%2B%2B%29%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20if%20%28list%5Bj%20%2B%201%5D%20%3C%20list%5Bsmallest%5D%29%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20smallest%20%3D%20j%20%2B%201%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20hasSmallestBeenFounded%20%3D%20true%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20if%28hasSmallestBeenFounded%29%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20firstPosition%20%3D%20list%5Bi%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20list%5Bi%5D%20%3D%20list%5Bsmallest%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20list%5Bsmallest%5D%20%3D%20firstPosition%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%0A%7D&codeDivHeight=1100&codeDivWidth=510&cumulative=false&curInstr=2&heapPrimitives=nevernest&origin=opt-frontend.js&py=java&rawInputLstJSON=%5B%5D&textReferences=false"> </iframe> -->

# What is the Time Complexity of Selection Sort?

The time efficiency of selection sort is quadratic.

## Best Case Time Complexity of Selection Sort

\\(O(n^{2})\\) comparisons, \\( O(1) \\) swaps.

In best case time complexity, we consider the list already sorted. So **O(n)** is **1** as there will be **no** swapping. But to find out whether the list is ordered or not, it would be **comparison** in any case. This brings a **quadratic** time complexity with it, that is, \\(O(n^{2})\\). Because we have **2** nested **for** loops.

## Worst Case Time Complexity of Selection Sort

\\(O(n^{2})\\) comparisons, \\( O(n) \\) swaps.

Software engineers usually concentrate on finding only the **worst-case running time**, that is, the longest running time for any input of size "**n**". Just like in the best case time complexity, the **comparison** takes place in **quadratic** time complexity. But in the worst case scenario, our list of course will **not be sorted**. Because we have to consider the worst case scenario. So the **swapping** takes place in \\( O(n) \\) time.

## Average Case Time Complexity of Selection Sort

\\(O(n^{2})\\) comparisons, \\( O(n) \\) swaps.

Even if the number of steps in the average time is **half** of the worst case, the result will still be the same as the worst case since constants will not be taken into account in the formulation. So the result will be the same as worst case.


<div class="notice--warning" markdown="1">
<h4 class="no_toc"><i class="fas fa-comment"></i> Note:</h4>
---
Note: The **worst-case running time** of an algorithm gives us an **upper bound** on the running time for any input. Knowing it provides a guarantee that the algorithm will never take any longer.
</div>

# Reference:

* [selection sort pseudocode](https://www.khanacademy.org/computing/computer-science/algorithms/sorting-algorithms/a/selection-sort-pseudocode)
* [selection sort wiki](https://en.wikipedia.org/wiki/Selection_sort)
* [Introduction to Algorithms third edition](https://en.wikipedia.org/wiki/Introduction_to_Algorithms)
