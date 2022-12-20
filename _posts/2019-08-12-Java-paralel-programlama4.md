---
title: "Java Paralel Programlama - Bölüm 4"
comments: false
excerpt: "Java Paralel Programlama - Çok İşlemci Zamanlama, Paralel Hızlandırma"
header:
  teaser: "/assets/images/svg-book7.svg"
  og_image: //assets/images/svg-book7.svg
  overlay_image: /assets/images/svg-book7.svg
  overlay_color: "#000"
  overlay_filter: "0.3"
  caption: "background by [SVGBackgrounds.com](https://www.svgbackgrounds.com/)"
  #cta_label: "More Info"
  #cta_url: "https://unsplash.com"
categories:
  - java-paralel-programlama
tags:
  - Java paralel programlama
  - Java threads
  - Java fork/join
last_modified_at: 2018-06-06T15:12:19-04:00
toc: true
toc_label: "SAYFA İÇERİĞİ"
#classes: wide
---

**ÖNEMLİ :** Kendim için aldığım notlar. Umarım size de bir faydası olur. Kullanılan her bir makale referans olarak eklenmiştir. Rice Üniversitesi'nin hazırladığı eğitsel bir Framework olan PCDP bu ve sonraki bölümlerde kullanılacaktır. async ve finish notasyonları bu Framework'de yer almaktadır.
{: .notice}

<div class="notice--success" markdown="1">
<h4 class="no_toc"><i class="fas fa-lightbulb"></i> Java Paralel Programlama Serisi</h4>
---

1. Java Paralel Programlama - [Bölüm 1](/java-paralel-programlama/Java-paralel-programlama1/)
2. Java Paralel Programlama - [Bölüm 2](/java-paralel-programlama/Java-paralel-programlama2/)
3. Java Paralel Programlama - [Bölüm 3](/java-paralel-programlama/Java-paralel-programlama3/)
4. **Java Paralel Programlama - Bölüm 4**

</div>

## Genel Bakış

Bir önceki bölümde hesaplama grafiklerini görmüştük. Şimdi ise gerçek **çok çekirdekli bilgisayarlarda** nasıl haritalandıklarını görebiliriz. Bir önceki bölümde resmettiğimiz hesaplama grafiğini tekrar göz önüne getirelim ve her bir işleme bazı yürütme zamanlarını verelim. **S<sub>6</sub>** dışında bütün işlemlere <u>1 birim</u> çalışma zamanı verdiğimizi varsayalım. **S<sub>6</sub>** ise **10** olsun.


<br/>{% picture 2019-08-12-Java-paralel-programlama4/comp_graph4.png --alt Java hesaplama grafiği(java computation graph) --img width="100%" height="100%" %}<br/>

Burada ilgilendiğimiz şey, **P** kadar işlemci olduğunda **T yürütme zamanını** bilmek istiyoruz. Yani, **P işlemcili** çok çekirdekli bir makinemiz varsa: Örn; **2, 4, 8, 16** çekirdekli, verilen bir hesaplama grafiği için hangi işlem zamanını alabiliriz?

* **T<sub>p</sub> = P işlemcilerinde yürütme süresidir (Execution time on P processors)**

Hesaplama grafiğindeki bu adımların aslında işlemcilerde nasıl zamanlandığını düşünecek olursak, aşağıdaki gibi bir gösterim yapabiliriz. Fakat bunu bilgisayarlar arka planda kendi yaptıkları için endişelenmemizi gerektirecek bir durum yoktur. Hesaplama grafiğindeki adımları işlemciler arasında programlamak, çalışma zamanı yazılımı ve donanımının işidir.

**2 işlemcili** bir durumu ele aldığımızı varsayalım ve bu adımların nasıl programlanabileceğini görelim.

<br/>{% picture 2019-08-12-Java-paralel-programlama4/multiprocessor1.png --alt Java hesaplama grafiğinin(java computation graph) işlemciler üzerinde gösterimi --img width="100%" height="100%" %}<br/>

Dolayısıyla, ne olursa olsun **S<sub>1</sub>**'in ilk önce gerçekleştirilmesi gerektiğini görüyoruz, çünkü grafiğin asıl amacı, sıralama ilişkilerini gösterir ve **S<sub>1</sub>** işini bitirene kadar başka hiçbir şey çalışmaz. **S<sub>1</sub>**'i rastgele olarak **P<sub>0</sub>**'da başlattığımızı varsayalım. Sonrasında **S<sub>2</sub>** ve **S<sub>4</sub>** ve **S<sub>6</sub>** hepsi çalıştırılabilir. Ama iki işlemci olduğu için **3** işlemden **2**'sini seçmemiz gerekli.

Farzedelim ki, **S<sub>2</sub>** ve **S<sub>4</sub>**'ü seçtik. **P<sub>0</sub>**, **S<sub>2</sub>**'yi, **P<sub>1</sub>** ise **S<sub>4</sub>**'ü çalıştırdı. Şimdi **S<sub>2</sub>** ve **S<sub>4</sub>** birbirleriyle paralel olarak çalışıyorlar. Akabinde, **P<sub>0</sub>**, **S<sub>3</sub>**'ü, **P<sub>1</sub>** ise **S<sub>5</sub>**'i hesaplama grafiğindeki gibi çalıştırmaya devam eder. Şimdi, bundan sonra **S<sub>7</sub>**'yi uygulayamıyoruz çünkü **S<sub>6</sub>** hala bitirilmeyi bekliyor.

Her iki işlemci de boşa çıktığı için ikisinden birinde **S<sub>6</sub>** çalıştırılabilir. Farzedelim ki bu görevi **P<sub>0</sub>** aldı. Artık son işlem olan **S<sub>7</sub>**'ye geçebiliriz. Bunu da **S<sub>6</sub>**'da olduğu gibi boşta olan herhangi bir işlemci alabilir. Yine **P<sub>0</sub>**'ın aldığını varsayalım. Yukarıda görüldüğü üzere işlemciler üzerinde ilgili planlama yapıldı. Şimdi ise bu işlemcilerin çalışma zamanlarını üzerinde tekrar düşünebiliriz.

<br/>{% picture 2019-08-12-Java-paralel-programlama4/multiprocessor2.png --alt Java hesaplama grafiğinin(java computation graph) işlemciler üzerinde gösterimi --img width="100%" height="100%" %}<br/>


Planlama bu şekilde olduğunda 2 işlemcideki çalışma süresini **14**(yani **T<sub>2</sub>** = 14) olarak hesapladık. Dikkat edilecek olursa **2** adet de **IDLE**(atıl) slot göze çarpmaktadır. Yani bu slotların boşta olduğunu, yapacak bir işlerinin olmadığını göstermektedir. Ancak, grafiği programlamanın tek yolu bu değildir. Başka bir yaklaşım da izleyebiliriz. Çünkü **S<sub>6</sub>** bir darboğaz oluşturuyor ve **P<sub>1</sub>** işlemcisinin belirli durumlarda **boşta(IDLE)** kalmasına neden oluyor. Dolayısıyla, şimdi ikinci planlamadaki hedefimiz **S<sub>6</sub>**'yı mümkün olan en kısa sürede çalıştırmak olacaktır.

<br/>{% picture 2019-08-12-Java-paralel-programlama4/multiprocessor3.png --alt Java hesaplama grafiğinin(java computation graph) işlemciler üzerinde gösterimi --img width="100%" height="100%" %}<br/>


Bu planlama örneğinde **S<sub>1</sub>**'i yine ilk olarak başlatmak zorundayız. Tabii **S<sub>1</sub>** başladığında **P<sub>1</sub>** bir öncekinde olduğu gibi başlangıçta boş kalır. **S<sub>1</sub>** işlemi bittiğinde, **P<sub>0</sub>**'da **S<sub>6</sub>**'yı önceliklendirebiliriz. Böylece **S<sub>6</sub>**, <u>10 birim</u> zaman boyunca **P<sub>0</sub>** üzerinde çalışırken, **P<sub>1</sub>**'de **S<sub>2</sub>**, **S<sub>3</sub>**, **S<sub>4</sub>**, **S<sub>5</sub>** işlemlerini rahatlıkla ele alabiliriz. <u>Sıra önemli değil çünkü bunlar, hangi sırayla yapılırsa yapsınlar toplamda dört iş birimi ile çalışırlar</u>. Yani **10 birim** zamana sahip **S<sub>6</sub>**'yı düşünürsek, işlemlerini **S<sub>6</sub>**'dan önce bitirecekleri kesindir. **S<sub>6</sub>**'da işlemini sonlandırdıktan sonra **S<sub>7</sub>**'yi işleme alabiliriz. **S<sub>7</sub>** herhangi bir işlemci de başlayabilir. Varsayalım ki **P<sub>0</sub>**'da başladı. Peki, şimdi bu planlama için yürütme süresi ne oldu?. Burada **S<sub>1</sub>**'in **1**, **S<sub>6</sub>**'nın **10** ve **S<sub>7</sub>**'nin <u>1 birim</u> süresi olduğu için toplamda <u>12 birim</u> çalışma süresi olduğunu görüyoruz.

Dolayısıyla, <u>bir öncekine kıyasla daha küçük bir yürütme zamanımız var</u>. Bu nedenle, **P** kadar işlemcide yürütme süresinin bu tanımı aslında tasarlanan bu plana bağlıdır. Ancak bu yürütme süresi hakkında belirleyebileceğimiz belirli özellikler vardır. Birincisi, **P --> 1**'e eşit olsaydı ne olurdu? <u>Eğer sadece 1 işlemcimiz olsaydı, <b>T<sub>1</sub>'in --> WORK</b>'e eşit olduğunu iddia edebilirdik.</u> Bunu neden söylüyoruz? Çünkü bir işlemcide çalışıyorsanız, temel olarak bu işlemcinin, hesaplama grafiğindeki tüm işleri yapması gerekir. Bu işlem, tüm yürütme zamanlarını eklediğinizde elde ettiğiniz şeydir.

* **P = 1 ---> T1 = WORK**


Örneği düşünecek olursak tüm çalışma zamanlarının toplamı;

{% highlight java %}
S1 + S2 + S3 + S4 + S5 + S6 + S7 = ?
1  + 1  + 1  + 1  + 1  + 10 + 1  = 16
T1 = 16
{% endhighlight %}

Düşünebileceğimiz diğer bir şey ise, pratikte olmasa da, sonsuz sayıda işlemcimiz olsaydı ne olurdu? Cevap daha önce öğrendiğimiz, yani hesaplama grafiğindeki en uzun yolun uzunluğunu temsil eden **SPAN** olurdu. Çünkü yeterli işlemcimiz varsa, en uzun yolda olmak ve kendinden sonra gelecek işlemleri beklemek dışında bir adım dahi beklemenin bir sebebi yoktur.

Şekil 1'e bakacak olursak, başta hesaplama grafiğini çizdiğimizde **3 tane paralel** çalışan işlemi düşünmüştük. İşlemin uzunluğu da haliyle bu dallanmalardan en son hangisi biterse ona göre belirlenir. Çünkü diğer işlemler önce bitse de, en son **join işlemi** yapılacağı zaman birbirlerini beklemek zorundalar. Paralel çalışacak yeterli sayıda işlemcimiz olacağı için **3 dallanmaya** da yeterli sayıda işlemci olacaktır. **S<sub>3</sub>**, **S<sub>2</sub>**'den sonra gelen bir işlem, **S<sub>5</sub>** de **S<sub>4</sub>**'den sonra gelen bir işlem olduğu için onlar yeni bir işlemci de çalışmaz.

<div class="notice--success" markdown="1">
<h4 class="no_toc"><i class="fas fa-lightbulb"></i> Not:</h4>
---
Yani sonsuz sayıda da işlemciniz olsa hesaplama grafiğine göre en fazla 3 tane dallanmanız olacaktır. Kısacası en son biten işlem size **SPAN** değerini de verecektir. Bu durumda **SPAN 1+10+1 = 12** olur. O halde, herhangi bir **P** sayıda işlemciye bakıldığında, aşağıdaki aralıkta olmamız gerektiğini biliyoruz;

* **T<sub>1</sub> = WORK = 16**

* **T<sub>$$\infty$$</sub> = SPAN = 12**

* **T<sub>$$\infty$$</sub> $$\le$$ T<sub>p</sub> $$\le$$ T<sub>1</sub>**
</div>

Paralel programlar hakkında konuşurken çok ilginç bir diğer kavram ise hızlanmadır(**SPEEDUP**). Bu yüzden, paralelliğin asıl amacı, donanım üreticilerinin bize verdiği tüm bu çekirdeklerle programınızın daha hızlı çalışmasını sağlamaktır.

* **SPEEDUP = T<sub>1</sub> / T<sub>P</sub>**

Öyleyse bunu düşünelim. **T<sub>1</sub>** sıralı yürütme süresidir. **T<sub>P</sub>**, **P** işlemcide aldığımız yürütme süresidir. Ve bu oran, paralel versiyonun ne kadar hızlı çalışabildiğinin faktörü olacaktır. Bu yüzden, hızlanmanın **P**'ye küçük eşit olması gerektiğini görebiliyoruz.


* **SPEEDUP $$\le$$ P**

Speedup(P) must be ≤ the number of processors P.
{: .notice--info}


Aynı şekilde, **SPEEDUP** aşağıdaki gibi de olmalı;


* **SPEEDUP $$\le$$ WORK/SPAN = IDEAL PARALLELISM**

Speedup(P) must be ≤ the ideal parallelism, WORK/SPAN.
{: .notice--info}


Dolayısıyla, hızlanmanın elbette kaç işlemcinin mevcut olduğuna bağlı olduğunu ve aynı zamanda "**ideal paralellik**" olan hesaplama grafiğinin bu gerçekten önemli özelliği ile de sınırlandığını görüyoruz. <u>Paralel algoritmalarda hedefimiz, sahip olduğunuz işlemci sayısından çok daha büyük olan ideal paralelliğe sahip hesaplama grafikleri oluşturmaktır</u>, böylece, bu paralel programı çok sayıda işlemcide çalıştırma esnekliğine sahip olursunuz.

* **IDEAL PARALLELISM $$\ge$$ P**


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
