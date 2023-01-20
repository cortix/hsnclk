---
title: "Java'da Hafıza Modeli 2 - Nesneler"
comments: false
excerpt: "Java'da Nesneler Bellekte Nasıl Saklanır? Bu durumu net anlaşılması için nasıl simüle edebiliriz?"
header:
  teaser: "/assets/images/svg-book23.svg"
  og_image: /assets/images/svg-book23.svg
  overlay_image: /assets/images/svg-book23.svg
  overlay_color: "#000"
  overlay_filter: "0.3"
  caption: "background by [SVGBackgrounds.com](https://www.svgbackgrounds.com/)"
  #cta_label: "More Info"
  #cta_url: "https://unsplash.com"
categories:
  - java-hafiza-yonetimi
tags:
  - Java memory model
  - Java'da Nesneler İçin Hafıza Modeli
#last_modified_at: 2023-01-06T15:12:19-04:00
last_modified_at:
toc: true
toc_label: "SAYFA İÇERİĞİ"
toc_sticky: true
---

**ÖNEMLİ :** Kendim için aldığım notlar. Umarım size de bir faydası olur. Kullanılan her bir makale referans olarak eklenmiştir.
{: .notice}

<div class="notice--success" markdown="1">
<h4 class="no_toc"><i class="fas fa-lightbulb"></i> Java'da Hafıza Modeli Serisi</h4>
---

1. Java'da Hafıza Modeli 1 - [İlkel Veri Tipleri(Primitive Types)](/java-hafiza-yonetimi/Java-memory-models-primitive-types/)
2. **Java’da Hafıza Modeli 2 - Nesneler**
3. Java’da Hafıza Modeli 3 - [Nesneler](/java-hafiza-yonetimi/Java-memory-models-objects1/)
4. Java’da Hafıza Modeli 4 - [Kapsam(Scope)](/java-hafiza-yonetimi/Java-memory-models-scope/)
5. Java’da Hafıza Modeli 5 - [Pass By Value / Pass By Reference](/java-hafiza-yonetimi/Java-memory-models-pass-by-value-reference/)
6. Java’da Hafıza Modeli 6 - [Java’da Statik ve Statik Olmayan Değişken ve Metotların Hafıza Yönetimi](/java-hafiza-yonetimi/Java-memory-models-static-nonstatic-members/)
7. Java’da Hafıza Modeli 7 - [String Interning Nedir, String Pool Nedir, == operatörü ve equals metodu Arasındaki Fark](/java-hafiza-yonetimi/string-interning-string-pool/)
</div>

## Genel Bakış

Şu ana kadar gördüğümüz ilkel veri tipleri, hafıza modeli nasıl olur sorusuna bir yanıttı! Şimdi ise kompleks veri tiplerinden(ilkel olmayan veri tipleri), yani bir başka ifadeyle nesneler için bir hafıza modeli oluşturmaya çalışalım.

> **Referans türleri/ ilkel olmayan veri türleri/kompleks veri türleri** diye bahsederken nesneleri kastettiğimizi unutmayalım. Neden kompleks ve referans olarak nitelediğimizi birazdan daha net anlayacaksınız.

| İlkel Veri Türleri(Primitive Types) 	| Referans Türleri(Object/Reference Types)      	|
|-----------------	|--------------------	|
|   int,   *boolean* , *byte* , *char* , *short* , *long* , *float* ve *double*           	| Diziler(Arrays) ve Sınıflar(Classes) 	|

<div class="notice--success" markdown="1">
<h4 class="no_toc"><i class="fas fa-lightbulb"></i> İlkel Türler ve Referans Türleri</h4>
---

Bir önceki makalede de belirttiğim üzere Java **2 çeşit veri türüne** sahiptir.

* Bunlardan ilki, **ilkel veri türleridir(primitive data types** yani: **int**, **boolean**, **byte**, **char**, **short**, **long**, **float** ve **double**).

* <u>İlkel veri türleri dışında kalan her şey de bir nesnedir</u>. Yani nesne türü(**object types**) de diyebiliriz. Bu nesne türleri, **diziler(arrays)**, **kullanıcı-tanımlı(user-defined) sınıf** veyahut bir **kütüphane içindeki bir sınıf** da olabilir. Anlayacağınız ilkel veri türleri dışında kalan her şey nesnedir diyebiliriz.
</div>
Açıkçası, bellek modellerimize nasıl nesne ekleneceğini öğrenmek, bir kod hakkında akıl yürütme açısından gerçekten önemli olacak.

```java
int deg1 = 1;
SampleTest sample = new SampleTest(1,2);
```

Yukarıdaki örneğimizde **ilkel** ve **ilkel olmayan** veri tiplerine birer örnek görmektesiniz. Bir önceki bölümden `int` ilkel veri tipine aşinayız. İkinci satırda ise ilkel olmayan veri tipine bir örnek verilmiştir. **SampleTest** isminde bir sınıftan oluşturulmuş bir nesne ve o nesneyi temsil eden **sample** adında bir değişken(bundan sonraki bölümlerde değişken yerine referans kullanmaya çalışacağım. Aslında ikisi de doğru fakat **stack**'de objeyi temsil ettiği için bu değişkenin referans olarak ifade edilmesi daha doğru olacaktır.)!!!

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


<br/>{% picture 2020-02-29-Java-memory-models-objects/deg1.png --alt Java variable assignment(java değişken atama) --img width="100%" height="100%" %}<br/>

* **1.satır:** Bu ilk satır bir önceki bölümde yaptığımız gibi değişken tanımlama işlemidir. Java'ya, tamsayı(int) tipinde bir değeri saklamak için yeni bir alan istiyorum, diyorsunuz. Ve o alanı(space) **deg1** etiketiyle etiketleyeceğinizi belirtiyorsunuz. Şimdi hafıza modelimizde bunu göz önünde canlandırabilmek için bir kutu çizeceğiz. Bu kutu değişkeni temsil etmem gereken alanı(space) işaret ediyor. Sonrasında bu değişkeni **deg1** ismiyle etiketliyorum. Bir önceki bölümde bulunan işlemin aynısı!!!


<br/>{% picture 2020-02-29-Java-memory-models-objects/sample1.png --alt Java reference variable assignment(java referans değişkeni atama) --img width="100%" height="100%" %}<br/>

* **2.satır:** Aslında ikinci satırda yaptığımız işlemin birinci satırda olandan bir farklı yok. `sample1` isminde bir değişken oluşturup, onun için de özel bir kutu çizeceğiz. Sadece değişkenin tipi `int` yerine **SampleTest** isminde bir sınıf olduğunu unutmayalım.

<br/>{% picture 2020-02-29-Java-memory-models-objects/sample2.png --alt Java reference variable assignment(java referans değişkeni atama) --img width="100%" height="100%" %}<br/>

* **3.satır:** Bu satırda işler biraz daha komplike bir hal almaya başlıyor. Aslında 1. satırda olduğu gibi bir atama işlemi var. Yalnız tek farkı; atanan değerin bir nesne olması. Burada **new** anahtar kelimesini kullanarak **SampleTest** sınıfından bir nesne yaratma ve yaratılan bu nesneyi de az önce oluşturduğumuz değişkene(yani referansa) atama işlemi var(<u>Az önce sırf bu yüzden bir açıklama getirmiştim. Bu değişken artık ilkel bir veriyi tutan bir değişkenin aksine heap alanında bir objeyi işaret eden bir referanstır.</u> Yine değişken olarak da ifade edebilirsiniz fakat farkını bilmenizde yarar var). **new** anahtar kelimesini kullandığımızda Java'ya bilgisayarın hafızasında, heap denilen bölgede özel bir alan oluşturmasını ve bu alanda yaratılan nesneyi saklamasını söylüyorsunuz. Yani `deg1` ve `sample1` değişkenlerini yarattığımız yığından(stack) farklı, özel bir birimden bahsediyorum. Sonuç olarak Java bu nesneyi **heap**'de bir lokasyona koyar ve bize sadece bu lokasyonun **id**'sini(başka programlama dillerinde bu id adres olarak da geçer) verir. Tabii Java'nın, bu nesneyi oluşturmak için **heap**'de tam olarak nereyi seçtiğini bilmiyoruz. O yüzden bize verdiği **id** ile yetinmek zorundayız. Verilen id'nin başında **@** işareti vardır. Bunu bir editörde debug modda deneyerek görebilirsiniz. Peki burada ne oluyor? Aslında java heap'de yarattığı nesneyi **sample1** kutusunun içine kopyalamak yerine, bu nesneye ulaşabilmek için stack'de yer alan bu **değişkene/referansa** bir id verir. Klişe olan şekilde ifade etmek gerekirse, nesnenin heap içinde saklandığı yerin adresini(aslında adres değil :))) **sample1** değişkeninin/referansının kutusuna yazar. Bu, referans verme işlemi olarak adlandırılır. Yani **@** işareti ile başlayan sayı bir nevi heap'de yer alan objenin stack alanındaki referansıdır. Nesnelere referans türleri denmesinin sebebi de aslında budur.

Bu referansı stack'da yer alan **sample1** değişkeninin içine yerleştirdiğimizi hayal edin. Aslında arka planda tam olarak buna benzer bir işlem olmaktadır.

<br/>{% picture 2020-02-29-Java-memory-models-objects/sample3.png --alt Java reference variable assignment(java referans değişkeni atama) --img width="100%" height="100%" %}<br/>

Bu id'yi silip okla göstermek istiyorum. Bu ok az önceki **@** işareti ile başlayan id'nin ne anlama geldiğini resmetmektedir. Gerçekte **id** ile gösterim daha doğrudur. Ama okla yaptığımız gösterim, arka planda olanı daha net anlamanıza yardımcı olacağını düşünüyorum. Bu ok aslında **sample1** değişkeninin/referansının heap'te ne tuttuğunu bize işaret etmektedir. Yani aşağıdaki gibi bir sonuç bizi beklemektedir.

<br/>{% picture 2020-02-29-Java-memory-models-objects/sample4.png --alt Java reference variable assignment(java referans değişkeni atama) --img width="100%" height="100%" %}<br/>


Şimdi olan biteni daha detaylı anlamak için objenin **heap** alanında nasıl oluştuğuna bakalım. Haliyle sınıfın kendine özel değişkenleri de olabilir. Buna **nesne değişkenleri(member variables/statik olmayan değişkenler/instance variables**) da denmektedir. <u>Bu değişkenler bu sınıftan türetilen herbir nesnenin içinde yer almaktadır</u>. Şimdi yukarıdaki kodda yer alan sınıfımızın aşağıdaki gibi **2 nesne değişkenine** sahip olduğunu hayal edin. Bunlar heap alanında oluşurken resmetmeye çalışacağım.

```java
public class SampleTest{
    private int x;
    private int y;

    public SampleTest(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```

Heap alanında nesne yaratıldığında tıpkı stack alanında yaptığımız gibi nesnenin içinde de nesnenin sahip olduğu değişkenleri temsil eden iki tane kutu çizeceğiz. Bu kutular nesne yaratıldığında, nesnenin **x ve y değerlerini** saklayacaktır. İlk paylaştığımız kod bloğunun 3.satırında ``new SampleTest(1,2);`` görüldüğü üzere <u>2 parametre</u> almaktadır. Bu parametreler aslında bu **x** ve **y** değerlerini ilklendirmek için nesne yaratıldığında tanımlanmıştır. **Constructor(yapılandırıcı)** scope'unun nasıl oluştuğu ile ilgili örneğe scope'lara giriş konusunda değiniriz. Burada böyle bir constructor olduğunu varsayarak bu değerleri heap alanında oluşturduğumuz nesne içindeki değişken kutucuklarına ekleyeceğim. Yani şöyle bir şeyle karşılaşırız.

<br/>{% picture 2020-02-29-Java-memory-models-objects/sample5.png --alt Java reference variable assignment(java referans değişkeni atama) --img width="100%" height="100%" %}<br/>

* **4.satır:**
Bu adımda ``SampleTest sample2 = new SampleTest(3,4);`` 3. adımdakine benzer şekilde heap alanında tekrardan yeni bir nesne yaratıyoruz. Aynı zamanda stack alanında da **sample2** isminde yeni bir değişken/referans oluşturuyoruz. Sonrasında bu referansa bu nesnenin id'sini veriyoruz. **@** işareti ile başlayan id yerine yine okla gösterimi tercih edeceğim.

<br/>{% picture 2020-02-29-Java-memory-models-objects/sample6.png --alt Java reference variable assignment(java referans değişkeni atama) --img width="100%" height="100%" %}<br/>

* **5.satır**
Son satırda ise ``sample2.x = 5;`` **sample2** referansının işaret ettiği nesnenin **x** değişkenine yeni bir değerin atanması işlemi vardır. Yani **sample2** referansının işaret ettiği nesneye gidip ilgili değeri değiştireceğiz.

<br/>{% picture 2020-02-29-Java-memory-models-objects/sample7.png --alt Java reference variable assignment(java referans değişkeni atama) --img width="100%" height="100%" %}<br/>

Yukarıdaki kod bloğumuzun **hafıza modeli** kısaca bu şekildedir. Bu resmi bir sonraki ders **scope** konusu ile bir adım öteye taşıyacağız.

### Referans ve Nesne Türleri

Burada küçük bir parantez açıp bir şeyi izah etmek istiyorum. Aslında **nesne türleri** ve **referans türleri** arasındaki çok ufak bir farkı göstermek istiyorum. Heap alanındaki nesneleri stack alanında temsil etmelerinden ötürü ikisinin de aynı olduğunu düşünebilirsiniz. <u>Ama bir nesneyi stack'de temsil eden referansın türüyle, bu nesnenin heap'teki türü her zaman aynı olmayabilir.</u> Aslında şunu söylemek istiyorum... Nesneyi stack alanında temsil eden değişkenin, yani referansın tipi **somut(concrete)** bir sınıf olmayabilir. [Kalıtım](/java-hafiza-yonetimi/Java-inheritance3/) konusuna geçince bunun ne anlama geldiğini daha net anlayabilirsiniz.

Aşağıdaki şekilden de görüleceği üzere **referans** ve **nesne(obje)** tiplerinin neler olduğunu görüyorsunuz. [Hashmap](https://docs.oracle.com/javase/8/docs/api/java/util/HashMap.html) sınıfı aslında **Map arayüzünü(interface)** uygular. [Map](https://docs.oracle.com/javase/8/docs/api/java/util/Map.html) <u>soyut bir sınıftır</u>.

<div class="notice--danger" markdown="1">
<h4 class="no_toc"><i class="fas fa-lightbulb"></i> Önemli Hatırlatma</h4>
---
Soyut sınıflar **new** anahtar kelimesi ile oluşturulamazlar. Oluşturmaya kalkıştığınızda şu şekilde bir hata alırsınız.

```java
'Map' is abstract; cannot be instantiated.
```
</div>

**Abstract**'ın kelime anlamı <u>soyut</u>, **instantiated** ise <u>somutlaştırmadır</u>. Yani bu hata mesajı bize, <u>Map soyuttur, somutlaştırılamaz demek istemektedir</u>. Yani, nesne türü olarak ifade ettiğimizde, sanki stack'teki değişkenin(referansın) tipinin karşılığı da, heap'teki somut nesnenin tipiyle aynı olduğu kanısı oluşabilir. <u>Ama bu örnekten de anlaşılacağı üzere stack'te her zaman somut bir sınıfın türü objeyi temsil etmeyebilir.</u> Bu tür burada olduğu gibi bir **interface** de olabilir. Bu yüzden referans türü diye ifade ettiğimizde nesneyi de kapsadığı için daha genel bir tanım olacaktır.

Özetle, <u>soyut sınıflar ve arayüzler(abstract classes and interfaces) heap alanında somut bir sınıf(concrete class) tarafından temsil edilmedikçe</u> **var olamazlar**. `HashMap` ise **somut(concrete)** bir sınıftır.

**NOT :** Bu arada referans tipleri, deklare edilen tipler(declared type) olarak da tanımlanır.
{: .notice--warning}

<br/>{% picture 2020-02-29-Java-memory-models-objects/ref-obj-type.png --alt Java difference between reference and object variable(java referans ve obje tipi arasındaki fark) --img width="100%" height="100%" %}<br/>


## Referanslar
* Bütün şekiller [lucidchart](https://www.lucidchart.com)'da tarafımdan hazırlanmıştır.
* [Primitive Data Types](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/datatypes.html)
* [Java Data Types](https://www.w3schools.com/java/java_data_types.asp)
* [A Guide to Creating Objects in Java](https://www.baeldung.com/java-initialization)
* [Introduction to Java Primitives](https://www.baeldung.com/java-primitives)
* [Stack Memory and Heap Space in Java](https://www.baeldung.com/java-stack-heap)
* [Stack vs. Heap: Understanding Java Memory Allocation](https://dzone.com/articles/stack-vs-heap-understanding-java-memory-allocation)
* [Object Oriented Programming in Java](https://www.coursera.org/learn/object-oriented-java?specialization=java-object-oriented)
* [Java Heap Space vs Stack – Memory Allocation in Java](https://www.journaldev.com/4098/java-heap-space-vs-stack-memory)
* [What is Java String Pool?](https://www.journaldev.com/797/what-is-java-string-pool)
* [Guide to Java String Pool](https://www.baeldung.com/java-string-pool)
