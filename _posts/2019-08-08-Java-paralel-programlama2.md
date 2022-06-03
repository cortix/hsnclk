---
title: "Java Paralel Programlama - Bölüm 2"
comments: false
excerpt: "Java Paralel Programlama - Java Fork/Join Framework"
header:
  teaser: "assets/images/equality.png"
  og_image: /assets/images/page-header-og-image.png
  overlay_image: /assets/images/unsplash-image-40.jpg
  overlay_filter: 0.5 #rgba(255, 0, 0, 0.5)
  caption: "Photo by [Tanya Trofymchuk](https://unsplash.com/photos/LsD0PaXl_U8) on Unsplash"
  #cta_label: "More Info"
  #cta_url: "https://unsplash.com"
categories:
  - java-paralel-programlama
tags:
  - threads
  - paralel programlama
  - fork/join
last_modified_at: 2018-06-06T15:12:19-04:00
toc: true
toc_label: "SAYFA İÇERİĞİ"
---



**ÖNEMLİ :** Kendim için aldığım notlar. Umarım size de bir faydası olur. Kullanılan her bir makale referans olarak eklenmiştir. Rice Üniversitesi'nin hazırladığı eğitsel bir Framework olan PCDP bu ve sonraki bölümlerde kullanılacaktır. async ve finish notasyonları bu Framework'de yer almaktadır.
{: .notice}

## Genel Bakış

Bir önceki bölümde gördüğümüz ``async`` ve ``finish`` notasyonları eğitsel ve gösterimsel kavramlardı. Fakat Java'daki karşılıkları tabii ki bu şekilde değildir. Bu bölümde ise aynı konseptin Java'daki karşılıklarına bakacağız. Java'da çok çekirdekli paralellikten yararlanmanın en popüler yollarından biri olan Java Fork/Join Framework'üdür.

Öyleyse array-sum örneğimize geri dönelim. Ve önce bir böl ve fethet algoritmasına kadar uzatabilir miyiz bir bakalım. Şimdi, daha nesne odaklı düşünelim ve bir sınıfımız olduğunu söyleyelim. Bu sınıfımızın adı ASUM olsun.

{% highlight java %}
CLASS ASUM {
  A, // an array
  LO, // lower bound
  UP, // upper bound
  SUM;
  COMPUTE(){  //compute() metodu, aslında alt ve üst sınırlar ve array göz önüne alındığında toplamı hesaplayacak bir hesaplama yöntemimizdir.
    IF(LO == UP) SUM = A[LO]; // Örneğin, eğer LO, UP ile aynıysa, sum değerini array LO'ya ata, ki set high ile aynı yaparız.
    ELSE IF (LO>UP) SUM = 0;
    ELSE {
      MID = (LO+UP)/2;
      L = NEW ASUM(A,LO,MID);
      R = NEW ASUM(A,MID+1,UP);
      L.COMPUTE();
      R.COMPUTE();
      SUM = L.SUM + R.SUM;
    }
  }
}
{% endhighlight %}

Bu bölümde, async, finish ve Fork/Join kavramlarını deneysel anlatmak için Böl ve fethet algoritması(divide and conquer algorithm) kullanılacaktır. Bu algoritmanın arkasındaki temel fikir, sorunu ikiye bölmektir. Amacımız algoritmanın detayına girmek değil. Temel hedefimiz, bu algoritmayı nasıl paralel hale getirebiliriz? Algoritma ile ilgili daha detaylı bilgiye ulaşmak için bu [linki](http://bilgisayarkavramlari.sadievrenseker.com/2008/08/09/birlestirme-siralamasi-merge-sort/) ziyaret edebilirsiniz.

Yukarıda görüleceği üzere L.COMPUTE() ve R.COMPUTE() olarak ifade edilmiş 2 işlem bulunmaktadır. Biz bu işlemlerden herhangi birini, örneğin L.COMPUTE() olanı asekron olarak işaretlersek paralellik sağlamış oluruz. finish notasyonu ile de bu iki işlemi bir kapsam içine alırsak, iki işlemin nihayete ereceği garanti altına alınmış olur. Böylelikle iki işlem bitmeden SUM işlemine geçilemez.
{% highlight java %}
FINISH {
  ASYNC L.COMPUTE() // L.FORK async ile aynı işi yapar.
        R.COMPUTE()
}
SUM = L.SUM + R.SUM;
{% endhighlight %}

finish ise join ile aynı işi yapar. Tek fark, finish scope içindeki bütün işlerin bitmesini beklerken, join sadece bir işe odaklanır. Benzer işlem async ve finish notasyonları yerine forkjoin kullanılarak ise şu şekilde yapılır.

{% highlight java %}
L.COMPUTE();
L.FORK;
R.COMPUTE()
L.JOIN();
SUM = L.SUM + R.SUM;
{% endhighlight %}

Fork/Join görevleri, Java iş parçacığı havuzu olan bir "ForkJoinPool" içinde yürütülür. Bu havuz, hem **fork** hem de **join** işlemlerini paralel bir dizi görevi yürüterek ve bunların tamamlanmasını bekleyerek birleştiren **invokeAll()** yöntemini destekler. Örneğin, **ForkJoinTask.invokeAll (sol, sağ)**, örtülü olarak sol ve sağda **fork()** işlemlerini gerçekleştirir, ardından her iki nesnede de **join()** işlemi gerçekleştirir.

Bu Framework'de, kullanıcının kendi oluşturacağı bir sınıf, FJ Framework'ü içinde bulunan **RecursiveAction** sınıfı *extend* edilip, bu sınıfın yerleşik metodlarından biri olan **compute()** metodu *override* edildikten sonra, metodun içinde istenilen görev belirlenebilir.  

{% highlight java %}
private static class ASum extends RecursiveAction {
  int[] A; // input array
  int LO, HI; // subrange
  int SUM; // return value
  ...

  @Override
  protected void compute() {
    SUM = 0;
    for (int i = LO; i <= HI; i++) SUM += A[i];
  } // compute()
}
{% endhighlight %}


**Referanslar :**

1. [Sequential algorithm](https://en.wikipedia.org/wiki/Sequential_algorithm)
2. [Asynchronous method invocation](https://en.wikipedia.org/wiki/Asynchronous_method_invocation)
3. [Analysis of parallel algorithms](https://en.wikipedia.org/wiki/Analysis_of_parallel_algorithms)
4. [Class RecursiveAction](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/RecursiveAction.html)
5. [Class RecursiveTask](http://docs.oracle.com/javase/8/docs/api/?java/util/concurrent/RecursiveTask.html)
6. [Class ForkJoinPool](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ForkJoinPool.html)
7. [Package java.util.stream](https://docs.oracle.com/javase/8/docs/api/java/util/stream/package-summary.html)
8. [Interface Stream](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html)
9. [Fork/Join](https://docs.oracle.com/javase/tutorial/essential/concurrency/forkjoin.html)
10. [Java VisualVM](http://docs.oracle.com/javase/7/docs/technotes/guides/visualvm/)
11. [Parallel Programming in Java](https://www.coursera.org/learn/parallel-programming-in-java/home/welcome)
12. [https://habanero-rice.github.io/PCDP/](https://habanero-rice.github.io/PCDP/)
