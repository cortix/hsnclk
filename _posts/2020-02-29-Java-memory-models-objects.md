---
title: "Java'da Nesneler İçin Hafıza Modeli 1"
comments: true
excerpt: "Java'da Nesneler Bellekte Nasıl Saklanır? Bu durumun net anlaşılması için nasıl simüle edebiliriz?"
header:
  teaser: "assets/images/equality.png"
  og_image: /assets/images/page-header-og-image.png
  overlay_image: /assets/images/unsplash-image-7.jpg
  overlay_filter: 0.5 #rgba(255, 0, 0, 0.5)
  caption: "Photo credit: [**Unsplash**](https://unsplash.com)"
  #cta_label: "More Info"
  #cta_url: "https://unsplash.com"
categories:
  - java-hafiza-yonetimi
tags:
  - Nesneler İçin Hafıza Modeli
last_modified_at: 2020-02-19T15:12:19-04:00
toc: true
toc_label: "SAYFA İÇERİĞİ"
---

**ÖNEMLİ :** Kendim için aldığım notlar. Umarım size de bir faydası olur. Kullanılan her bir makale referans olarak eklenmiştir.
{: .notice}

## Genel Bakış

Şu ana kadar gördüğümüz ilkel veri tipleri, hafıza modeli nasıl olur sorusuna bir yanıttı! Şimdi ise komplex veri tiplerinden(ilkel olmayan veri tipleri), yani bir başka ifadeyle nesneler için bir hafıza modeli oluşturmaya çalışalım.

> Referans türleri/ ilkel olmayan veri türleri/kompleks veri türleri diye bahsederken nesneleri kastettiğimizi unutmayalım. Neden kompleks ve referans olarak nitelediğimizi birazdan daha net anlayacaksınız.

| İlkel Veri Türleri(Primitive Types) 	| Referans Türleri(Object/Reference Types)      	|
|-----------------	|--------------------	|
|   int,   *boolean* , *byte* , *char* , *short* , *long* , *float* ve *double*           	| Diziler(Arrays) ve Sınıflar(Classes) 	|

Bir önceki makalede de belirttiğim üzere Java 2 çeşit veri türüne sahiptir. Bunlardan ilki, ilkel veri türleridir(*primitive data types* yani: *int*, *boolean* , *byte* , *char* , *short* , *long* , *float* ve *double*). İlkel veri türleri dışında kalan her şey de bir nesnedir. Yani nesne türü(*object types*) de diyebiliriz. Bu nesne türleri, diziler(arrays), kullanıcı-tanımlı(user-defined) sınıf veyahut bir kütüphane içindeki bir sınıf da olabilir. Anlayacağınız ilkel veri türleri dışında kalan herşey nesnedir diyebiliriz.

Açıkçası, bellek modellerimize nasıl nesne ekleneceğini öğrenmek, bir kod hakkında akıl yürütme açısından gerçekten önemli olacak.

```java
int deg1 = 1;
SampleTest sample = new SampleTest(1,2);
```

Yukarıdaki örneğimizde ilkel ve ilkel olmayan veri tiplerine birer örnek görmektesiniz. Bir önceki dersten `int` ilkel veri tipine aşinayız. İkinci satıda ise ilkel olmayan veri tipine bir örnek verilmiştir. `SampleTest` isminde bir sınıftan oluşturulmuş bir nesne ve o nesneyi temsil eden `sample` adında bir değişken!!!

## İlkel Olmayan Veri Türleri İçin Hafıza Modeli

Dilerseniz bir önceki makalede olduğu gibi bir örnekle başlayalım.

 ```java
 int deg1 = 5;
 SampleTest sample1;
 sample1 = new SampleTest(1,2);
 SampleTest sample2 = new SampleTest(3,4);
 sample2.x = 5;
 ```

 ```java
 public class SampleTest{
   private int x;
   private int y;
   ....
 }
 ```


Adım adım yukarıdaki kod bloğu nasıl çalışır, resmetmeye çalışalım;

<figure style="width: 250px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-02-29-Java-memory-models-objects/deg1.png" alt="async-variable">
  <figcaption></figcaption>
</figure>

* **1.satır:** Bu ilk satır bir önceki derste yaptığımız gibi değişken tanımlama işlemidir. Java'ya, tamsayı(int) tipinde bir değeri saklamak için yeni bir alan istiyorum, diyorsunuz. Ve o alanı(space) deg1 etiketiyle etiketleyeceğinizi belirtiyorsunuz. Şimdi hafıza modelimizde bunu göz önünde canlandırabilmek için bir kutu çizeceğiz. Bu kutu değişkeni temsil etmem gereken alanı(space) işaret ediyor. Sonrasında bu değişkeni deg1 ismiyle etiketliyorum. Bir önceki dersteki işlemin aynısı!!!

<figure style="width: 250px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-02-29-Java-memory-models-objects/sample1.png" alt="async-variable">
  <figcaption></figcaption>
</figure>

* **2.satır:** Aslında ikinci satırda yaptığımız işlemin birinci satırda olandan bir farklı yok. `sample1` isminde bir değişken oluşturup, onun için de özel bir kutu çizeceğiz. Sadece değişkenin tipi `int` yerine `SampleTest` isminde bir sınıf olduğunu unutmayalım.

<figure style="width: 600px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-02-29-Java-memory-models-objects/sample2.png" alt="async-variable">
  <figcaption></figcaption>
</figure>

* **3.satır:** Bu satırda işler biraz daha komplike bir hal almaya başlıyor. Aslında 1. satırda olduğu gibi bir atama işlemi var. Yalnız tek farkı; atanan değerin bir nesne olması. Burada `new` anahtar kelimesini kullanarak `SampleTest` sınıfından bir nesne yaratma ve yaratılan bu nesneyi de az önce oluşturduğumuz değişkene atama işlemi var. Şimdi burada gerçekleşen işlemleri teker teker ele alalım istiyorum. `new` anahtar kelimesini kullandığımızda Java'ya bilgisayarın hafızasında, heap denilen alanda özel bir alan oluşturmasını ve bu alanda yaratılan nesneyi saklamasını söylüyorsunuz. Yani `deg1` ve `sample1` değişkenlerini yarattığımız yığından(stack) farklı, özel bir birimden bahsediyorum. Sonuç olarak Java bu nesneyi heap'de bir lokasyona koyar ve bize sadece bu lokasyonun adresini verir. Tabii Java'nın, bu nesneyi oluşturmak için heap'de tam olarak nereyi seçtiğini bilmiyoruz. O yüzden bize verdiği adresle yetinmek zorundayız. Verilen adresin başında `@` işareti vardır. Bunu bir editörde debug modda deneyerek görebilirsiniz. Peki burada ne oluyor? Aslında Java heap'de yarattığı nesneyi sample1 kutusunun içine kopyalamak yerine, bu nesneye ulaşabilmek için stack'de yer alan bu değişkene bir referans verir. Başka bir deyişle, nesnenin heap içinde saklandığı yerin adresini sample1 değişkeninin kutusuna koyar. Bu, referans verme işlemi olarak adlandırılır. Yani **@** işareti ile başlayan sayı bir nevi heap'de yer alan objenin stack alanındaki referansıdır. Nesnelere referans türleri denmesinin sebebi de aslında budur. Bu referansı stack'da yer alan sample1 değişkeninin içine yerleştirdiğimizi hayal edin. Aslında arka planda tam olarak buna benzer bir işlem olmaktadır.

<figure style="width: 600px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-02-29-Java-memory-models-objects/sample3.png" alt="async-variable">
  <figcaption></figcaption>
</figure>

Bu numarayı silip okla göstermek istiyorum. Bu ok az önceki **@** işareti ile başlayan sayıyının ne anlama geldiğini resmetmektedir. Yani bu sayı bir nevi objenin heap alanındaki lokasyonunu bize verir. Gerçekte sayı ile gösterim daha doğrudur. Ama okla yaptığımız gösterim, arka planda olanı daha net anlamanıza yardımcı olacağını düşünüyorum. Bu ok aslında sample1 değişkeninin değerini bize göstermektedir. Yani aşağıdaki gibi bir sonuç bizi beklemektedir.

<figure style="width: 600px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-02-29-Java-memory-models-objects/sample4.png" alt="async-variable">
  <figcaption></figcaption>
</figure>

Şimdi olan biteni daha detaylı anlamak için objenin heap alanında nasıl oluştuğuna bakalım. Haliyle sınıfın kendine özel değişkenleri de olabilir. Buna nesne değişkenleri(member variables) da denmektedir. Bu değişkenler bu sınıftan türetilen herbir nesnenin içinde yer almaktadır. Şimdi yukarıdaki kodda yer alan sınıfımızın aşağıdaki gibi 2 nesne değişkenine sahip olduğunu hayal edin. Bunlar heap alanında oluşurken resmetmeye çalışacağım.

```java
public class SampleTest{
  private int x;
  private int y;
  ....
}
```

Heap alanında nesne yaratıldığında tıpkı stack alanında yaptığımız gibi nesnenin içinde de nesnenin sahip olduğu değişkenleri temsil eden iki tane kutu çizeceğiz. Bu kutular nesne yaratıldığında, nesnenin **x ve y değerleri** saklayacaktır. İlk paylaştığımız kod bloğunun 3.satırında ``new SampleTest(1,2);`` görüldüğü üzere 2 parametre almaktadır. Bu parametreler aslında bu x ve y değerlerini ilklendirmek için nesne yaratıldığında tanımlanmıştır. Constructor(yapılandırıcı) scope'unun nasıl oluştuğu ile ilgili örneğe scope'lara giriş konusunda değiniriz. Burada böyle bir constructor olduğunu varsayarak bu değerleri heap alanında oluşturduğumuz nesne içindeki değişken kutucuklarına ekleyeceğim. Yani şöyle bir şeyle karşılaşırız.

<figure style="width: 600px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-02-29-Java-memory-models-objects/sample5.png" alt="async-variable">
  <figcaption></figcaption>
</figure>


* **4.satır:**
Bu adımda ``SampleTest sample2 = new SampleTest(3,4);`` 3. adımdakine benzer şekilde heap alanında tekrardan yeni bir nesne yaratıyoruz. Aynı zamanda stack alanında da sample2 isminde yeni bir değişken oluşturuyoruz. Sonrasında bu değişkene bu nesnenin referansını veriyoruz. **@** işareti ile başlayan sayı yerine yine okla göstereceğim.

<figure style="width: 600px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-02-29-Java-memory-models-objects/sample6.png" alt="async-variable">
  <figcaption></figcaption>
</figure>

* **5.satır**
Son satırda ise ``sample2.x = 5;`` sample2 değişkeninin referans aldığı nesnenin **x** değişkenine yeni bir değerin atanması işlemi vardır. Yani sample2 değişkeninin işaret ettiği nesneye gidip ilgili değeri değiştireceğiz.

<figure style="width: 600px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-02-29-Java-memory-models-objects/sample7.png" alt="async-variable">
  <figcaption></figcaption>
</figure>

Yukarıdaki kod bloğumuzun hafıza modeli kısaca bu şekildedir. Bu resmi bir sonraki ders **scope** konusu ile bir adım öteye taşıyacağız.

## Sözlük

* **statically-typed:** değişken tiplerinin açıkça bildirildiği ve derleme zamanında belirlendiği bir programlama dili karakteristiğidir. Bu, derleyicinin belirli bir değişkenin kendisinden istenen eylemleri gerçekleştirip gerçekleştiremeyeceğine karar vermesini sağlar.


## Referanslar
* Bütün şekiller [https://www.lucidchart.com](https://www.lucidchart.com)'da tarafımdan hazırlanmıştır.
* [https://docs.oracle.com/javase/tutorial/java/nutsandbolts/datatypes.html](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/datatypes.html)
* [https://www.w3schools.com/java/java_data_types.asp](https://www.w3schools.com/java/java_data_types.asp)
* [https://www.baeldung.com/java-initialization](https://www.baeldung.com/java-initialization)
* [https://www.baeldung.com/java-primitives](https://www.baeldung.com/java-primitives)
* [https://www.baeldung.com/java-stack-heap](https://www.baeldung.com/java-stack-heap)
* [https://dzone.com/articles/stack-vs-heap-understanding-java-memory-allocation](https://dzone.com/articles/stack-vs-heap-understanding-java-memory-allocation)
* [https://www.coursera.org/lecture/object-oriented-java/core-drawing-memory-models-with-primitive-data-H0TCb](https://www.coursera.org/lecture/object-oriented-java/core-drawing-memory-models-with-primitive-data-H0TCb)
* [https://www.journaldev.com/4098/java-heap-space-vs-stack-memory](https://www.journaldev.com/4098/java-heap-space-vs-stack-memory)
* [https://blogs.oracle.com/jonthecollector/presenting-the-permanent-generation](https://blogs.oracle.com/jonthecollector/presenting-the-permanent-generation)
* [https://www.journaldev.com/797/what-is-java-string-pool](https://www.journaldev.com/797/what-is-java-string-pool)
* [https://www.baeldung.com/java-string-pool](https://www.baeldung.com/java-string-pool)
