---
title: "Java Paralel Programlama - Bölüm 3"
comments: true
excerpt: "Java Paralel Programlama - Computation Graphs, Work, Span"
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

Şu ana kadar gördüklerimizi özetlemek gerekirse, aşağıdaki görsel yeterli olacaktır.

<figure >
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2019-08-11-Java-paralel-programlama3/async-finish4.jpeg" alt="async-finish example">
  <figcaption>Şekil 1 - https://www.lucidchart.com da hazırlanmıştır.</figcaption>
</figure>

Aslında bu bölümde bunun gibi paralel programları modellemek için hesaplama grafiği(computation graph) adı verilen bir kavramı göstermek istiyorum.

<figure >
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2019-08-11-Java-paralel-programlama3/comp_graph1.jpeg" alt="computation graph example">
  <figcaption>Şekil 2 - https://www.lucidchart.com da hazırlanmıştır.</figcaption>
</figure>

Şekilde görüldüğü gibi S1'den sonra S2 *fork* edilerek yeni bir branch de çalışması sağlanıyor. S2'ye paralel olarak S1, S3 olarak yeni bir işleme devam ediyor. Buna **continue** işlemi denir. S3'ten sonra da aynı görev S4'te devam etmek istiyor. Ama burada bu *join* işlemi var. Bunun için **join edge** adı verilen farklı bir kenarımız var.

fork–join programları için, kenarları üç bölüme ayırmak yararlı olur:

- Bir görevdeki adımların sırasını yakalayan **continue edges**'ler',
- Child task'ların ilk adımına bir fork işlemi bağlayan **fork edges**'ler'.
- Bir görevin son adımını o görevdeki tüm *join* işlemlerine bağlayan **join edges**'ler'.

Dolayısıyla, bu üç çeşit kenarla, bir paralel programın çalışmasını modelleyebileceğimizi görüyoruz. Bu yönlendirilmiş grafiğin her tepe noktası veya düğümü, bir adım olarak adlandırdığımız bir ardışık alt hesaplamayı temsil eder. Ve her edge bir sıralama kısıtlamasına karşılık gelir. Eğer fork ve join olmadan normal bir sıralı programa sahip olsaydınız, grafiğiniz sadece continue edge'lere sahip düz bir çizgi olurdu.

## Data Race Nedir?
İş parçacığı Çözümleyicisi, çok iş parçacıklı bir işlem yürütülürken oluşan veri yarışlarını algılar. Bir veri yarışması şu durumlarda gerçekleşir:

- Tek bir işlemde iki veya daha fazla iş parçacığı aynı anda aynı bellek konumuna erişir ve
- erişimlerin en az biri yazmak içindir ve
- iş parçacığı bu belleğe erişimlerini denetlemek için özel kilitler kullanmaz.

Bu üç koşul geçerli olduğunda, erişimlerin sırası belirleyici değildir ve hesaplama, o sıraya bağlı olarak çalıştırmadan farklı sonuçlar verebilir. Bazı veri yarışları iyi huylu olabilir (örneğin, hafıza erişimi meşgul olarak beklemek için kullanıldığında), ancak programdaki birçok veri yarışları hatalıdır.

Örneğin, eğer S3’te yanlışlıkla, S2’nin hesapladığı bazı toplamları okumaya çalıştığımızı farzedelim. Ve diyelim ki o esnada S2'de bu alan toplamına yazmaya başlıyoruz. İşte o zaman, okuma ve yazma işlemleri S2'de paralel gidebileceğinden ve haliyle S3'ün hesaplama grafiğinde S2 ile bir bağlantısı olmadığından dolayı bir hata alırız. Paralel programlamada bu çok zararlı bir tür hatadır. Buna **data race(yani veri yarışı)** denir.   

## Performans Ölçümü, Work, Span ve Ideal Paralellik
Hesaplama grafiklerinin bir başka ilginç özelliği de, paralel programınızın performansını düşünmek için onları kullanabilmemizdir. Varsayalım ki, S1 ve S4, 1 birim zaman alacak, S3 ve S2 ise 10 birim zaman alacak iş yapsın.

<figure >
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2019-08-11-Java-paralel-programlama3/comp_graph2.jpeg" alt="computation graph performans ölçümü">
  <figcaption>Şekil 3 - https://www.lucidchart.com da hazırlanmıştır.</figcaption>
</figure>

Performansla ilgili olarak çalışacağımız iki önemli ölçüm bulunmaktadır. İlki **WORK** olarak adlandırılır. Aslında bu sadece tüm düğümlerin yürütme zamanlarının toplamıdır. Yani bu durumda, 1 artı 10 artı 10 artı 1 olur. Yani 22. Gerçekten önemli olan başka bir ölçüm ise **SPAN** olarak adlandırılıyor. Ve bu en uzun yolun uzunluğudur. Programcılar ayrıca bunu *kritik yol uzunluğu* olarak da adlandırırlar.

<figure >
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2019-08-11-Java-paralel-programlama3/comp_graph3.jpeg" alt="span : kritik yol uzunluğu">
  <figcaption>Şekil 4 - https://www.lucidchart.com da hazırlanmıştır.</figcaption>
</figure>

Bu iki ölçüm, programdaki paralellik hakkında akıl yürütmemize yardımcı olur. Örneğin bu iki ölçümü kullanarak **ideal parelellik** kavramını öğreneceğiz.

**Ideal Parallelism = WORK / SPAN**

**Ideal Parallelism** = 22/12
                  = 1.83

Bu, hesaplama grafiğinde ne kadar paralellik olduğuna dair çok somut bir ölçü vermektedir. Sıralı bir allgoritma için, bu sadece 1 olacaktır, çünkü span ile work aynı olacaktır(herhangi bir çatallanma olmayacağından). İdeal paralellik, hesaplama grafiğinde düğümlerin paralel olarak yürütülmesinden elde edilebilecek hızlanma faktörü üzerindeki üst sınırdır. İdeal paralelliğin sadece paralel programın bir işlevi olduğunu ve fiziksel bilgisayarda mevcut olan gerçek paralelliğe bağlı olmadığını unutmayın.

**Referanslar :**

1. [https://docs.oracle.com/cd/E19205-01/820-0619/geojs/index.html](https://docs.oracle.com/cd/E19205-01/820-0619/geojs/index.html)
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
