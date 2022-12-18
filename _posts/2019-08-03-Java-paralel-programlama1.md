---
title: "Java Paralel Programlama - Bölüm 1"
comments: false
excerpt: "Java Paralel Programlama - Görev oluşturma ve sonlandırma kavramları(Async, Finish)"
header:
  teaser: "assets/images/equality.png"
  og_image: /assets/images/equality.png
  overlay_image: /assets/images/unsplash-image-62.jpeg
  overlay_filter: 0.5 #rgba(255, 0, 0, 0.5)
  caption: "Photo by [Hal Gatewood](https://unsplash.com/photos/tZc3vjPCk-Q) on Unsplash"
  #cta_label: "More Info"
  #cta_url: "https://unsplash.com"
categories:
  - java-paralel-programlama
tags:
  - java paralel programlama
  - java threads
  - java fork/join
last_modified_at: 2018-06-06T15:12:19-04:00
toc: false
toc_label: "SAYFA İÇERİĞİ"
toc_sticky: true
classes: wide
---



**ÖNEMLİ :** Kendim için aldığım notlar. Umarım size de bir faydası olur. Kullanılan her bir makale referans olarak eklenmiştir. Rice Üniversitesi'nin hazırladığı eğitsel bir Framework olan PCDP bu ve sonraki bölümlerde kullanılacaktır. async ve finish notasyonları bu Framework'de yer almaktadır.
{: .notice}

<div class="notice--success" markdown="1">
<h4 class="no_toc"><i class="fas fa-lightbulb"></i> Java Paralel Programlama Serisi</h4>
---

1. **Java Paralel Programlama - Bölüm 1**
2. Java Paralel Programlama - [Bölüm 2](/java-paralel-programlama/Java-paralel-programlama2/)
3. Java Paralel Programlama - [Bölüm 3](/java-paralel-programlama/Java-paralel-programlama3/)
4. Java Paralel Programlama - [Bölüm 4](/java-paralel-programlama/Java-paralel-programlama4/)

</div>

## Genel Bakış

Bilgisayar bilimlerinde, sıralı bir algoritma(**sequential algorithm**) veya seri algoritma(**serial algorithm**), eşzamanlı(**concurrently**) veya paralel'in(**parallel**) aksine sırayla (<u>başka bir işlem çalıştırmadan baştan sona kadar bir kez</u>) yürütülen bir algoritmadır. Terim öncelikle eşzamanlı algoritma veya paralel algoritma ile zıtlık oluşturmak için kullanılır; çoğu standart bilgisayar algoritması, sıralı algoritmalardır ve sıralılık bir arka plan varsayımı olduğu için özel olarak tanımlanmamıştır.

Bir algoritma bir işlem sırasına sahiptir. Bunları **S<sub>1</sub>, S<sub>2</sub>, S<sub>3</sub>, S<sub>4</sub>** şeklinde sıralayabiliriz. Çok çekirdekli işlemciler için paralel programlamanın arkasındaki temel fikir, bu adımlardan hangisinin birbirleriyle paralel çalışabileceğini belirlemektir. Kısacası asıl önemli olan bu paralelliklerinin nasıl koordine edilmesi gerektiğidir.

<div class="notice--success" markdown="1">
<h4 class="no_toc"><i class="fas fa-lightbulb"></i> Örnek</h4>
---
Basit bir örnek ile başlamak gerekirse, integer içeren bir dizi olduğunu varsayalım. Amacımız array'in sahip olduğu elementlerin toplamını hesaplamak olsun. Şimdi bunu yapmanın bir yolu, onu iki yarıya bölmek ve alt yarı ile üst yarıdaki toplamı ayrı ayrı hesaplamak. Böylece;

* **SUM<sub>1</sub> = alt yarının toplamı**,
* **SUM<sub>2</sub> = üst yarı toplamı**

olabilir. Sonra bu ikisini birleştirerek son toplamı alabiliriz.
</div>

{% picture 2019-08-03-Java-paralel-programlama1/async-finish00.png --alt Java async-finish example --img width="100%" height="100%" %}

Ekstra bir işlem yapmaz isek, yukarıdaki adımlar sıralı bir şekilde **sequential algorithm** kullanılarak yapılır. Peki bunu paralel olarak nasıl yapabiliriz? Burada kullanacağımız notasyon **asenkron**(``async``) denilen bu kelimedir. **Async**'nin amacı, bir statement tarafından takip edilen `async` notasyonuna sahip ifadenin **asenkron(zaman uyumsuz)** olarak çalışması gerektiğidir. Görüleceği üzere **SUM<sub>1</sub>** ``async`` olarak işaretlenmiştir. Bunun anlamı, bu hesaplama, **SUM<sub>1</sub>(alt yarının toplamı)**, takip eden işlem ne olursa olsun asenkron olarak devam etmelidir. **ASYNC** olarak işaretli **SUM<sub>1</sub>** işlemi, **SUM<sub>2</sub>** işleminden önce de gerçekleşebilir, sonra da gerçekleşebilir, hatta **paralel** olarak da gerçekleşebilir.

{% picture 2019-08-03-Java-paralel-programlama1/async-finish0.png --alt Java async-finish example --img width="100%" height="100%" %}

Şimdi farzedelim ki bu iki işlem paralel olarak ilerliyor ve sonrasında **SUM<sub>2</sub>**'yi hesaplıyoruz; buradaki kilit nokta, ikisini bir araya getirmeden, yani **SUM<sub>1</sub>** ve **SUM<sub>2</sub>**'yi toplamadan önce, bu "**zaman uyumsuz**(``async``)" görevin nihayete erip **SUM<sub>1</sub>** değerini elde etmemiz gerekmektedir. Tam da bu noktada bu işlemi gerçekleştirebilmek için bitiş(``finish``) adında başka bir notasyonumuz vardır.

{% picture 2019-08-03-Java-paralel-programlama1/async-finish1.png --alt Java async-finish example --img width="100%" height="100%" %}

Bu yüzden ``finish``'in arkasındaki temel fikir, <u>çalışan bir dizi asenkron görev alabileceğiniz bir iş kapsamıdır</u>. Görüleceği üzere ``async`` notasyonu sadece **SUM<sub>1</sub>**'i kapsarken, ``finish`` notasyonu hem **SUM<sub>1</sub>** hem de **SUM<sub>2</sub>**'yi kapsamaktadır. Aslında ``finish`` scope'unun(kapsamının) sonunda bütün görevlerin tamamlanacağını garanti edersiniz. Yani ``finish`` scope'unun içindeki asenkron görevler dahil bütün görevler bitmeden bir sonraki işlem ele alınmaz. Artık **SUM<sub>1</sub>** işleminin ``finish`` notasyonundan sonra uygun olacağından emin olabiliriz. ``finish`` notasyonundan sonra **SUM<sub>1</sub>** ve **SUM<sub>2</sub>** için toplama işlemi gerçekleştirebiliriz.

{% picture 2019-08-03-Java-paralel-programlama1/async-finish2.png --alt Java async-finish example --img width="100%" height="100%" %}

Yukarıdaki resimde de görüleceği üzere, eğer **pentium dual** bir işlemciye sahipseniz, işlemlerden biri(yani **SUM<sub>1</sub>**) ``core0`` çekirdeğinde, işlemlerden diğeri(yani **SUM<sub>2</sub>**) ``core1`` çekirdeğinde gerçekleşecektir. Dolayısıyla, buradaki prensibe bakarsanız, herhangi bir sıralı algoritmayı alabilir ve paralellik için fırsat gördüğümüz her yerde ``asyncs`` ile önek ekleyebiliriz.

{% picture 2019-08-03-Java-paralel-programlama1/async-finish3.png --alt Java async-finish example --img width="100%" height="100%" %}

Paralel olarak çalışmasını istediğiniz işlemlerin bulunduğu yere bir ``finish`` scope'u ekleyin. Yukarıdaki örnekteki gibi **S<sub>2</sub>, S<sub>3</sub>, S<sub>4</sub>** birbirleriyle **paralel** olarak çalışacaktır. Fakat **S<sub>5</sub>** işlemi, ``finish`` scope'u tamamlanıncaya kadar bekleyecektir.

**Not :** Bu bölümde gördüğümüz **async** ve **finish** notasyonları eğitsel ve gösterimsel kavramlardır. Fakat Java’daki karşılıkları tabii ki bu şekilde değildir. Bir sonraki bölümde ise aynı konseptin Java’daki karşılıklarına bakacağız.  
{: .notice--warning}

## Özet
Bu bölümde bir array kullanarak görev oluşturma(**task creation**) ve görev sonlandırma(**task termination**) konseptlerini öğrendik.

{% highlight java %}
finish {
  async S1; // Asenkron olarak dizinin alt yarısının toplamını hesapla
  S2;       // S1 ile paralel array'in üst yarısının toplamını hesapla
}
S3; // S1 ve S2 tamamlandıktan sonra iki kısmi toplamı birleştirir.
{% endhighlight %}

- Görev oluşturma(**task creation**) için zaman uyumsuz(**asenkron**) gösterimini öğrendik: ``async`` **S<sub>1</sub>** --> ``async`` notasyonu parent task'ın bir child task oluşturmasına sebep olur. ``async`` notasyonuna sahip task, parent task ile asenkron bir şekilde işleme alınır. Burada işlem parent task'ın öncesinde sonrasında veyahut parelel bir şekilde gerçekleşebilir.
- ``finish`` notasyonunu kullanarak görev sonlandırma(**task termination**) gösterimini öğrendik. ``finish`` notasyonu, parent task'ın çalışmasını ve ``finish`` scope içinde oluşturulan tüm asenkron görevlerin tamamlanmasını bekler. ``async`` ve ``finish`` yapıları isteğe bağlı olarak yerleştirilmiş olabilir.


## Referanslar :

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
12. [PCDP parallel programming framework](https://habanero-rice.github.io/PCDP/)
13. Şekil 1,2,3,4,5 - [lucidchart](https://www.lucidchart.com) ile hazırlanmıştır.
