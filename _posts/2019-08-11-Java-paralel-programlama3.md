---
title: "Java Paralel Programlama - Bölüm 3"
comments: false
excerpt: "Java Paralel Programlama - Computation Graphs, Work, Span"
header:
  teaser: "assets/images/equality.webp"
  og_image: /assets/images/equality.webp
  overlay_image: /assets/images/unsplash-image-39.webp
  overlay_filter: 0.5 #rgba(255, 0, 0, 0.5)
  caption: "Photo by [Jenn.jpeg](https://unsplash.com/photos/gQCzAT6vyN4) on Unsplash"
  #cta_label: "More Info"
  #cta_url: "https://unsplash.com"
categories:
  - java-paralel-programlama
tags:
  - java paralel programlama
  - java threads
  - java fork/join
  - kritik yol uzunluğu
  - java data race(veri yarışı)
  - java ideal parallelism
  - java work
  - java span
  - java continue edges
  - java fork edges
  - java join edges
last_modified_at: 2018-06-06T15:12:19-04:00
toc: true
toc_label: "SAYFA İÇERİĞİ"
toc_sticky: true
---

**ÖNEMLİ :** Kendim için aldığım notlar. Umarım size de bir faydası olur. Kullanılan her bir makale referans olarak eklenmiştir. Rice Üniversitesi'nin hazırladığı eğitsel bir Framework olan PCDP bu ve sonraki bölümlerde kullanılacaktır. async ve finish notasyonları bu Framework'de yer almaktadır.
{: .notice}

<div class="notice--success" markdown="1">
<h4 class="no_toc"><i class="fas fa-lightbulb"></i> Java Paralel Programlama Serisi</h4>
---

1. Java Paralel Programlama - [Bölüm 1](/java-paralel-programlama/Java-paralel-programlama1/)
2. Java Paralel Programlama - [Bölüm 2](/java-paralel-programlama/Java-paralel-programlama2/)
3. **Java Paralel Programlama - Bölüm 3**
4. Java Paralel Programlama - [Bölüm 4](/java-paralel-programlama/Java-paralel-programlama4/)

</div>

## Genel Bakış

Şu ana kadar gördüklerimizi özetlemek gerekirse, aşağıdaki görsel yeterli olacaktır.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/2019-08-11-Java-paralel-programlama3/async-finish4.webp" srcset="{{ site.url }}{{ site.baseurl }}/assets/images/2019-08-11-Java-paralel-programlama3/async-finish4-small.webp 480w, {{ site.url }}{{ site.baseurl }}/assets/images/2019-08-11-Java-paralel-programlama3/async-finish4.webp 1080w" sizes="50vw" width="100%" height="100%" loading="lazy" alt="async-finish example">


Aslında bu bölümde bunun gibi paralel programları modellemek için **hesaplama grafiği(computation graph)** adı verilen bir kavramı göstermek istiyorum.


<img src="{{ site.url }}{{ site.baseurl }}/assets/images/2019-08-11-Java-paralel-programlama3/comp_graph1.webp" srcset="{{ site.url }}{{ site.baseurl }}/assets/images/2019-08-11-Java-paralel-programlama3/comp_graph1-small.webp 480w, {{ site.url }}{{ site.baseurl }}/assets/images/2019-08-11-Java-paralel-programlama3/comp_graph1.webp 1080w" sizes="50vw" width="100%" height="100%" loading="lazy" alt="computation graph example">


Şekilde görüldüğü gibi **S<sub>1</sub>**'den sonra **S<sub>2</sub>** **fork** edilerek yeni bir branch de çalışması sağlanıyor. **S<sub>2</sub>**'ye paralel olarak **S<sub>1</sub>**, **S<sub>3</sub>** olarak yeni bir işleme devam ediyor. Buna **continue** işlemi denir. **S<sub>3</sub>**'ten sonra da aynı görev **S<sub>4</sub>**'te devam etmek istiyor. Ama burada bu **join** işlemi var. Bunun için **join edge** adı verilen farklı bir kenarımız var.

<div class="notice--success" markdown="1">
<h4 class="no_toc"><i class="fas fa-lightbulb"></i> Not:</h4>
---
<b>Fork–join</b> programları için, kenarları üç bölüme ayırmak yararlı olur:

- Bir görevdeki adımların sırasını yakalayan **continue edges**'ler',
- Child task'ların ilk adımına bir fork işlemi bağlayan **fork edges**'ler'.
- Bir görevin son adımını o görevdeki tüm **join** işlemlerine bağlayan **join edges**'ler'.
</div>

Dolayısıyla, bu üç çeşit kenarla, bir paralel programın çalışmasını modelleyebileceğimizi görüyoruz. Bu yönlendirilmiş grafiğin her tepe noktası veya düğümü, bir adım olarak adlandırdığımız bir ardışık alt hesaplamayı temsil eder. Ve her **edge** bir sıralama kısıtlamasına karşılık gelir. Eğer **fork** ve **join** olmadan normal bir sıralı programa sahip olsaydınız, grafiğiniz sadece **continue edge**'lere sahip düz bir çizgi olurdu.

## Data Race Nedir?

İş parçacığı çözümleyicisi, çok iş parçacıklı bir işlem yürütülürken oluşan veri yarışlarını algılar. Bir **veri yarışması(data Race)** şu durumlarda gerçekleşir:

- Tek bir işlemde iki veya daha fazla iş parçacığı aynı anda aynı bellek konumuna erişir ve
- erişimlerin en az biri yazmak içindir ve
- iş parçacığı bu belleğe erişimlerini denetlemek için özel kilitler kullanmaz.

Bu üç koşul geçerli olduğunda, erişimlerin sırası belirleyici değildir ve hesaplama, o sıraya bağlı olarak çalıştırmadan farklı sonuçlar verebilir. <u>Bazı veri yarışları(data Race) iyi huylu olabilir</u> (örneğin, hafıza erişimi meşgul olarak beklemek için kullanıldığında), ancak programdaki birçok veri yarışları(data Race) hatalıdır.

Örneğin, eğer **S<sub>3</sub>**’te yanlışlıkla, **S<sub>2</sub>**’nin hesapladığı bazı toplamları okumaya çalıştığımızı farzedelim. Ve diyelim ki o esnada **S<sub>2</sub>**'de bu alan toplamına yazmaya başlıyoruz. İşte o zaman, okuma ve yazma işlemleri **S<sub>2</sub>**'de paralel gidebileceğinden ve haliyle **S<sub>3</sub>**'ün hesaplama grafiğinde **S<sub>2</sub>** ile bir bağlantısı olmadığından dolayı bir hata alırız. Paralel programlamada bu çok zararlı bir tür hatadır. Buna **data race(yani veri yarışı)** denir.   

## Performans Ölçümü, Work, Span ve Ideal Paralellik

Hesaplama grafiklerinin bir başka ilginç özelliği de, paralel programınızın performansını düşünmek için onları kullanabilmemizdir. Varsayalım ki, **S<sub>1</sub>** ve **S<sub>4</sub>**, <u>1 birim</u> zaman alacak, **S<sub>3</sub>** ve **S<sub>2</sub>** ise <u>10 birim</u> zaman alacak iş yapsın.


<img src="{{ site.url }}{{ site.baseurl }}/assets/images/2019-08-11-Java-paralel-programlama3/comp_graph2.webp" srcset="{{ site.url }}{{ site.baseurl }}/assets/images/2019-08-11-Java-paralel-programlama3/comp_graph2-small.webp 480w, {{ site.url }}{{ site.baseurl }}/assets/images/2019-08-11-Java-paralel-programlama3/comp_graph2.webp 1080w" sizes="50vw" width="100%" height="100%" loading="lazy" alt="computation graph performans ölçümü">


Performansla ilgili olarak çalışacağımız iki önemli ölçüm bulunmaktadır. İlki **WORK** olarak adlandırılır. Aslında bu sadece tüm düğümlerin **yürütme zamanlarının toplamıdır**. Yani bu durumda, **1 + 10 + 10 + 1** olur. Yani **22**. Gerçekten önemli olan başka bir ölçüm ise **SPAN** olarak adlandırılıyor. Ve bu <u>en uzun yolun uzunluğudur</u>. Programcılar ayrıca bunu "**kritik yol uzunluğu**" olarak da adlandırırlar.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/2019-08-11-Java-paralel-programlama3/comp_graph3.webp" srcset="{{ site.url }}{{ site.baseurl }}/assets/images/2019-08-11-Java-paralel-programlama3/comp_graph3-small.webp 480w, {{ site.url }}{{ site.baseurl }}/assets/images/2019-08-11-Java-paralel-programlama3/comp_graph3.webp 1080w" sizes="50vw" width="100%" height="100%" loading="lazy" alt="span : kritik yol uzunluğu">

<div class="notice--success" markdown="1">
<h4 class="no_toc"><i class="fas fa-lightbulb"></i> Ideal Parallelism</h4>
---
Bu iki ölçüm, programdaki paralellik hakkında akıl yürütmemize yardımcı olur. Örneğin bu iki ölçümü kullanarak **ideal parelellik** kavramını öğreneceğiz.

* **Ideal Parallelism = WORK / SPAN**

* **Ideal Parallelism** = 22/12 = 1.83
</div>

Bu, hesaplama grafiğinde ne kadar paralellik olduğuna dair çok somut bir ölçü vermektedir. Sıralı bir algoritma için, bu sadece **1** olacaktır, çünkü **span** ile **work** aynı olacaktır(<u>herhangi bir çatallanma olmayacağından</u>). İdeal paralellik, hesaplama grafiğinde düğümlerin paralel olarak yürütülmesinden elde edilebilecek hızlanma faktörü üzerindeki üst sınırdır. <u>İdeal paralelliğin sadece paralel programın bir işlevi olduğunu ve fiziksel bilgisayarda mevcut olan gerçek paralelliğe bağlı olmadığını</u> unutmayın.

## Referanslar :

1. [What is a Data Race?](https://docs.oracle.com/cd/E19205-01/820-0619/geojs/index.html)
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
13. Şekil 1,2,3,4 - [lucidchart](https://www.lucidchart.com) ile hazırlanmıştır.
