---
title: "Java Paralel Programlama - Bölüm 1"
comments: true
excerpt: "Java Paralel Programlama - Görev oluşturma ve sonlandırma kavramları(Async, Finish)"
header:
  teaser: "assets/images/equality.png"
  og_image: /assets/images/page-header-og-image.png
  overlay_image: /assets/images/unsplash-image-5.jpg
  overlay_filter: 0.5 #rgba(255, 0, 0, 0.5)
  caption: "Photo credit: [**Unsplash**](https://unsplash.com)"
  #cta_label: "More Info"
  #cta_url: "https://unsplash.com"
categories:
  - Java
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

Bilgisayar bilimlerinde, sıralı bir algoritma(``sequential algorithm``) veya seri algoritma(``serial algorithm``), eşzamanlı(``concurrently``) veya paralel'in(``parallel``) aksine sırayla (başka bir işlem çalıştırmadan baştan sona kadar bir kez) yürütülen bir algoritmadır. Terim öncelikle eşzamanlı algoritma veya paralel algoritma ile zıtlık oluşturmak için kullanılır; çoğu standart bilgisayar algoritması, sıralı algoritmalardır ve sıralılık bir arka plan varsayımı olduğu için özel olarak tanımlanmamıştır.

Bir algoritma bir işlem sırasına sahiptir. Bunları **S1, S2, S3, S4** şeklinde sıralayabiliriz. Çok çekirdekli işlemciler için paralel programlamanın arkasındaki temel fikir, bu adımlardan hangisinin birbirleriyle paralel çalışabileceğini belirlemektir. Kısacası asıl önemli olan bu paralelliklerinin nasıl koordine edilmesi gerektiğidir.

Basit bir örnek ile başlamak gerekirse, integer içeren bir dizi olduğunu varsayalım. Amacımız array'in sahip olduğu elementlerin toplamını hesaplamak olsun. Şimdi bunu yapmanın bir yolu, onu iki yarıya bölmek ve alt yarı ile üst yarıdaki toplamı ayrı ayrı hesaplamak. Böylece ``SUM1 = alt yarının toplamı``, ``SUM2 = üst yarı toplamı`` olabilir. Sonra bu ikisini birleştirerek son toplamı alabiliriz.

<figure >
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/async-finish00.jpeg" alt="async-finish example">
  <figcaption>https://www.lucidchart.com da hazırlanmıştır.</figcaption>
</figure>

Ektra bir işlem yapmaz isek, yukarıdaki adımlar sıralı bir şekilde ``sequential algorithm`` kullanılarak yapılır. Peki bunu paralel olarak nasıl yapabiliriz? Burada kullanacağımız notasyon asenkron(``async``) denilen bu kelimedir. Async'ın amacı, bir statement tarafından takip edilen `async` notasyonuna sahip ifadenin zaman uyumsuz olarak çalışması gerektiğidir. Görüleceği üzere SUM1 async olarak işaretlenmiştir. Bunun anlamı, bu hesaplama, SUM1(alt yarının toplamı), takip eden işlem ne olursa olsun asenkron olarak devam etmelidir. ASYNC olarak işaretli SUM1 işlemi, SUM2 işleminden önce de gerçekleşebilir, sonra da gerçekleşebilir, hatta paralel olarak da gerçekleşebilir.

<figure >
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/async-finish0.jpeg" alt="async-finish example">
  <figcaption>https://www.lucidchart.com da hazırlanmıştır.</figcaption>
</figure>

Şimdi farzedelim ki bu iki işlem paralel olarak ilerliyor ve sonrasında SUM2'yi hesaplıyoruz; buradaki kilit nokta, ikisini bir araya getirmeden, yani SUM1 ve SUM2 yi toplamadan önce, bu "zaman uyumsuz(async)" görevin nihayete erip SUM1 değerini elde etmemiz gerekmektedir. Tam da bu noktada bu işlemi gerçekleştirebilmek için bitiş(``finish``) adında başka bir notasyonumuz vardır.

<figure >
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/async-finish1.jpeg" alt="async-finish example">
  <figcaption>https://www.lucidchart.com da hazırlanmıştır.</figcaption>
</figure>

Bu yüzden finish'in arkasındaki temel fikir, çalışan bir dizi asenkron görev alabileceğiniz bir iş kapsamıdır. Görüleceği üzere async notasyonu sadece SUM1'i kapsarken, finish notasyonu hem SUM1 hem de SUM2'yi kapsamaktadır. Aslında finish scope'unun(kapsamının) sonunda bütün görevlerin tamamlanacağını garanti edersinir. Yani finish scope'unun içindeki asekron görevler dahil bütün görevler bitmeden bir sonraki işlem ele alınmaz. Artık SUM1 işleminin finish notasyonundan sonra uygun olacağından emin olabiliriz. finish notasyonundan sonra SUM1 ve SUM2 için toplama işlemi gerçekleştirebiliriz.

<figure >
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/async-finish2.jpeg" alt="async-finish example">
  <figcaption>https://www.lucidchart.com da hazırlanmıştır.</figcaption>
</figure>

Yukarıdaki resimde de görüleceği üzere, eğer pentium dual bir işlemciye sahipseniz, işlemlerden biri(yani SUM1) core0 çekirdeğinde, işlemlerden diğeri(yani SUM2) core1 çekirdeğinde gerçekleşecektir. Dolayısıyla, buradaki prensibe bakarsanız, herhangi bir sıralı algoritmayı alabilir ve paralellik için fırsat gördüğümüz her yerde asyncs ile önek ekleyebiliriz.

<figure >
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/async-finish3.jpeg" alt="async-finish example">
  <figcaption>https://www.lucidchart.com da hazırlanmıştır.</figcaption>
</figure>

Paralel olarak çalışmasını istediğiniz işlemlerin bulunduğu yere bir finish scope u ekleyin. Yukarıdaki örnekteki gibi S2,S3,S4 birbirleriyle paralel olarak çalışacaktır. Fakat S5 işlemi, finish scope'u tamamlanıncaya kadar bekleyecektir.

## Özet
Bu bölümde bir array kullanarak görev oluşturma(task creation) ve görev sonlandırma(task termination) konseplerini öğrendik.

{% highlight java %}
finish {
  async S1; // Asekron olarak dizinin alt yarısının toplamını hesapla
  S2;       // S1 ile paralel array'in üst yarısının toplamını hesapla
}
S3; // S1 ve S2 tamamlandıktan sonra iki kısmi toplamı birleştirir.
{% endhighlight %}

- Görev oluşturma(task creation) için zaman uyumsuz(asekron) gösterimini öğrendik: async SUM1 --> async notasyonu parent task'ın bir child task oluşturmasına sebep olur. async notasyonuna sahip task, parent task ile asekron bir şekilde işleme alınır. Burada işlem parent task'ın öncesinde sonrasında veyahut parelel bir şekilde gerçekleşebilir.
- finish notasyonunu kullanarak görev sonlandırma(task termination) gösterimini öğrendik. finish notasyonu, parent task'ın çalışmasını ve finish scope içinde oluşturulan tüm asekron görevlerin tamamlanmasını bekler. async ve finish yapıları isteğe bağlı olarak yerleştirilmiş olabilir.




Referanslar :

1. [https://en.wikipedia.org/wiki/Sequential_algorithm](https://en.wikipedia.org/wiki/Sequential_algorithm)
2. [https://en.wikipedia.org/wiki/Asynchronous_method_invocation](https://en.wikipedia.org/wiki/Asynchronous_method_invocation)
3. [https://en.wikipedia.org/wiki/Analysis_of_parallel_algorithms](https://en.wikipedia.org/wiki/Analysis_of_parallel_algorithms)
4. [https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/RecursiveAction.html](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/RecursiveAction.html)
5. [http://docs.oracle.com/javase/8/docs/api/?java/util/concurrent/RecursiveTask.html](http://docs.oracle.com/javase/8/docs/api/?java/util/concurrent/RecursiveTask.html)
6. [https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ForkJoinPool.html](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ForkJoinPool.html)
7. [https://docs.oracle.com/javase/8/docs/api/java/util/stream/package-summary.html](https://docs.oracle.com/javase/8/docs/api/java/util/stream/package-summary.html)
8. [https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html)
9. [https://docs.oracle.com/javase/tutorial/essential/concurrency/forkjoin.html](https://docs.oracle.com/javase/tutorial/essential/concurrency/forkjoin.html)
10. [http://docs.oracle.com/javase/7/docs/technotes/guides/visualvm/](http://docs.oracle.com/javase/7/docs/technotes/guides/visualvm/)
11. [https://www.coursera.org/learn/parallel-programming-in-java/home/welcome](https://www.coursera.org/learn/parallel-programming-in-java/home/welcome)
12. [https://habanero-rice.github.io/PCDP/](https://habanero-rice.github.io/PCDP/)
