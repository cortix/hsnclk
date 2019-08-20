---
title: "Java Paralel Programlama - Bölüm 4"
comments: true
excerpt: "Java Paralel Programlama - Çok İşlemci Zamanlama, Paralel Hızlandırma"
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

Bir önceki bölümde hesaplama grafiklerini görmüştük. Şimdi ise gerçek çok çekirdekli bilgisayarlarda nasıl haritalandıklarını görebiliriz. Bir önceki bölümde resmettiğimiz hesaplama grafiğini tekrar göz önüne getirelim ve her bir işleme bazı yürütme zamanlarını verelim. S6 dışında bütün işlemlere 1 birim çalışma zamanı verdiğimizi varsayalım. S6 ise 10 olsun.

<figure >
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2019-08-12-Java-paralel-programlama4/comp_graph4.jpeg" alt="hesaplama grafiği">
  <figcaption>Şekil 1 - https://www.lucidchart.com da hazırlanmıştır.</figcaption>
</figure>

Burada ilgilendiğimiz şey, P kadar işlemci olduğunda T yürütme zamanını bilmek istiyoruz. Yani, P işlemcili çok çekirdekli bir makinemiz varsa: Örn; 2, 4, 8, 16 çekirdekli, verilen bir hesaplama grafiği için hangi işlem zamanını alabiliriz?

**T<sub>p</sub> = P işlemcilerinde yürütme süresidir (Execution time on P processors)**

Hesaplama grafiğindeki bu adımların aslında işlemcilerde nasıl zamanlandığını düşünecek olursak, aşağıdaki gibi bir gösterim yapabiliriz. Fakat bunu bilgisayarlar arka planda kendi yaptıkları için endişelenmemizi gerektirecek bir durum yoktur. Hesaplama grafiğindeki adımları işlemciler arasında programlamak, çalışma zamanı yazılımı ve donanımının işidir.

2 işlemcili bir durumu ele aldığımızı varsayalım ve bu adımların nasıl programlanabileceğini görelim.


<figure >
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2019-08-12-Java-paralel-programlama4/multiprocessor1.png" alt="hesaplama grafiğinin işlemciler üzerinde gösterimi">
  <figcaption>Şekil 2 - https://www.lucidchart.com da hazırlanmıştır.</figcaption>
</figure>


Dolayısıyla, ne olursa olsun S1'in ilk önce gerçekleştirilmesi gerektiğini görüyoruz, çünkü grafiğin asıl amacı, sıralama ilişkilerini gösterir ve S1 işini bitirene kadar başka hiçbir şey çalışmaz. S1'i rastgele olarak P<sub>0</sub>'da başlattığımızı varsayalım. Sonrasında S2 ve S4 ve S6 hepsi çalıştırılabilir. Ama iki işlemci olduğu için 3 işlemden 2 sini seçmemiz gerekli. Farzedelim ki, S2 ve S4'ü seçtik. P<sub>0</sub> S2 yi, P<sub>1</sub> ise S4'ü çalıştırdı. Şimdi S2 ve S4 birbirleriyle paralel olarak çalışıyorlar. Akabinde, P<sub>0</sub> S3'ü, P<sub>1</sub> ise S5'i hesaplama grafiğindeki gibi çalıştırmaya devam eder. Şimdi, bundan sonra S7'yi uygulayamıyoruz çünkü S6 hala bitirilmeyi bekliyor. Her iki işlemci de boşa çıktığı için ikisinden birinde S6 çalıştırılabilir. Farzedelim ki bu görevi P<sub>0</sub> aldı. Artık son işlem olan S7 ye geçebiliriz. Bunu da S6'da olduğu gibi boşta olan herhangi bir işlemci alabilir. Yine P<sub>0</sub>'ın aldığını varsayalım. Yukarıda görüldüğü üzere işlemciler üzerinde ilgili planlama yapıldı. Şimdi ise bu işlemcilerin çalışma zamanlarını üzerinde tekrar düşünebiliriz.

<figure >
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2019-08-12-Java-paralel-programlama4/multiprocessor2.png" alt="hesaplama grafiğinin işlemciler üzerinde gösterimi">
  <figcaption>Şekil 3 - https://www.lucidchart.com da hazırlanmıştır.</figcaption>
</figure>

Planlama bu şekilde olduğunda 2 işlemcideki çalışma süresini 14(yani T<sub>2</sub> = 14) olarak hesapladık. Dikkat edilecek olursa 2 adet de IDLE(atıl) slot göze çarpmaktadır. Yani bu slotların boşta olduğunu, yapacak bir işlerinin olmadığını göstermektedir. Ancak, grafiği programlamanın tek yolu bu değildir. Başka bir yaklaşım da izleyebiliriz. Çünkü S6 bir darboğaz oluşturuyor ve P1 işlemcisinin belirli durumlarda boşta(IDLE) kalmasına neden oluyor. Dolayısıyla, şimdi ikinci planlamadaki hedefimiz S6'yı mümkün olan en kısa sürede çalıştırmak olacaktır.

<figure >
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2019-08-12-Java-paralel-programlama4/multiprocessor3.png" alt="hesaplama grafiğinin işlemciler üzerinde gösterimi">
  <figcaption>Şekil 4 - https://www.lucidchart.com da hazırlanmıştır.</figcaption>
</figure>

Bu planlama örneğinde S1'i yine ilk olarak başlatmak zorundayız. Tabii S1 başladığında P1 bir öncekinde olduğu gibi başlangıçta boş kalır. S1 işlemi bittiğinde, P0'da S6'yı önceliklendirebiliriz. Böylece S6, 10 birim zaman boyunca P0 üzerinde çalışırken,  P1'de S2, S3, S4, S5 işlemlerini rahatlıkla ele alabiliriz. Sıra önemli değil çünkü bunlar, hangi sırayla yapılırsa yapsınlar toplamda dört iş birimi ile çalışırlar. Yani 10 birim zamana sahip S6'yı düşünürsek, işlemlerini S6'dan önce bitirecekleri kesindir. S6'da işlemini sonlandırdıktan sonra S7'yi işleme alabiliriz. S7 herhangi bir işlemci de başlayabilir. Varsayalım ki P0'da başladı. Peki, şimdi bu planlama için yürütme süresi ne oldu?. Burada S1'in 1, S6'nın 10 ve S7'nin 1 birim süresi olduğu için toplamda 12 birim çalışma süresi olduğunu görüyoruz. Dolayısıyla, bir öncekine kıyasla daha küçük bir yürütme zamanımız var. Bu nedenle, P kadar işlemcide yürütme süresinin bu tanımı aslında tasarlanan bu plana bağlıdır. Ancak bu yürütme süresi hakkında belirleyebileceğimiz belirli özellikler vardır. Birincisi, P --> 1'e eşit olsaydı ne olurdu? Eğer sadece 1 işlemcimiz olsaydı, T1'in --> WORK'e eşit olduğunu iddia edebilirdik. Bunu neden söylüyoruz? Çünkü bir işlemcide çalışıyorsanız, temel olarak bu işlemcinin, hesaplama grafiğindeki tüm işleri yapması gerekir. Bu işlem, tüm yürütme zamanlarını eklediğinizde elde ettiğiniz şeydir.

**P = 1 ---> T1 = WORK**

Örneği düşünecek olursak tüm çalışma zamanlarının toplamı;

{% highlight java %}
S1 + S2 + S3 + S4 + S5 + S6 + S7 = ?
1  + 1  + 1  + 1  + 1  + 10 + 1  = 16
T1 = 16
{% endhighlight %}

Düşünebileceğimiz diğer bir şey ise, pratikte olmasa da, sonsuz sayıda işlemcimiz olsaydı ne olurdu? Cevap daha önce öğrendiğimiz, yani hesaplama grafiğindeki en uzun yolun uzunluğunu temsil eden **SPAN** olurdu. Çünkü yeterli işlemcimiz varsa, en uzun yolda olmak ve kendinden sonra gelecek işlemleri beklemek dışında bir adım dahi beklemenin bir sebebi yoktur. Şekil 1'e bakacak olursak, başta hesaplama grafiğini çizdiğimizde 3 tane paralel çalışan işlemi düşünmüştük. İşlemin uzunluğu da haliyle bu dallanmalardan en son hangisi biterse ona göre belirlenir. Çünkü diğer işlemler önce bitse de, en son join işlemi yapılacağı zaman birbirlerini beklemek zorundalar. Paralel çalışak yeterli sayıda işlemcimiz olacağı için 3 dallanmaya da yeterli sayıda işlemci olacaktır. S3, S2'den sonra gelen bir işlem, S5 de S4'den sonra gelen bir işlem olduğu için onlar yeni bir işlemci de çalışmaz. Yani sonsuz sayıda da işlemciniz olsa hesaplama grafiğine göre en fazla 3 tane dallanmanız olacaktır. Kısacası en son biten işlem size **SPAN** değerini de verecektir. Bu durumda ``SPAN 1+10+1 = 12`` olur. O halde, herhangi bir P sayıda işlemciye bakıldığında, aşağıdaki aralıkta olmamız gerektiğini biliyoruz;

**T<sub>1</sub> = WORK = 16**

**T<sub>$$\infty$$</sub> = SPAN = 12**

**T<sub>$$\infty$$</sub> $$\le$$ T<sub>p</sub> $$\le$$ T<sub>1</sub>**

Paralel programlar hakkında konuşurken çok ilginç bir diğer kavram ise hızlanmadır(**SPEEDUP**). Bu yüzden, paralelliğin asıl amacı, donanım üreticilerinin bize verdiği tüm bu çekirdeklerle programınızın daha hızlı çalışmasını sağlamaktır.

**SPEEDUP = T<sub>1</sub> / T<sub>P</sub>**

Öyleyse bunu düşünelim. **T<sub>1</sub>** sıralı yürütme süresidir. **T<sub>P</sub>**, P işlemcide aldığımız yürütme süresidir. Ve bu oran, paralel versiyonun ne kadar hızlı çalışabildiğinin faktörü olacaktır. Bu yüzden, hızlanmanın P'ye küçük eşit olması gerektiğini görebiliyoruz.

**SPEEDUP $$\le$$ P** (Speedup(P) must be ≤ the number of processors P)

Aynı şekilde, SPEEDUP aşağıdaki gibi de olmalı;

**SPEEDUP $$\le$$ WORK/SPAN = IDEAL PARALLELISM** (Speedup(P) must be ≤ the ideal parallelism, WORK/SPAN)

Dolayısıyla, hızlanmanın elbette kaç işlemcinin mevcut olduğuna bağlı olduğunu, aynı zamanda ideal paralellik olan hesaplama grafiğinin bu gerçekten önemli özelliği ile sınırlandırıldığını görüyoruz.

Dolayısıyla, hızlanmanın elbette kaç işlemcinin mevcut olduğuna bağlı olduğunu ve aynı zamanda "ideal paralellik" olan hesaplama grafiğinin bu gerçekten önemli özelliği ile de sınırlandığını görüyoruz. Paralel algoritmalarda hedefimiz, sahip olduğunuz işlemci sayısından çok daha büyük olan ideal paralelliğe sahip hesaplama grafikleri oluşturmaktır, böylece, bu paralel programı çok sayıda işlemcide çalıştırma esnekliğine sahip olursunuz.

**IDEAL PARALLELISM $$\ge$$ P**


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
