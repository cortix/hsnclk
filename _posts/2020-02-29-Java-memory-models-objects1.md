---
title: "Java'da Hafıza Modeli 3 - Nesneler"
comments: false
excerpt: "Java'da Nesneler Bellekte Nasıl Saklanır? Farklı Örnekler"
header:
  teaser: "assets/images/equality.png"
  og_image: /assets/images/equality.png
  overlay_image: /assets/images/unsplash-image-35.jpeg
  overlay_filter: 0.5 #rgba(255, 0, 0, 0.5)
  caption: "Photo by [redcharlie](https://unsplash.com/photos/k2zWqv_yfNM) on Unsplash"
  #cta_label: "More Info"
  #cta_url: "https://unsplash.com"
categories:
  - java-hafiza-yonetimi
tags:
  - Java memory model
  - Nesneler İçin Hafıza Modeli
last_modified_at: 2020-02-19T15:12:19-04:00
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
2. Java’da Hafıza Modeli 2 - [Nesneler](/java-hafiza-yonetimi/Java-memory-models-objects/)
3. **Java’da Hafıza Modeli 3 - Nesneler**
4. Java’da Hafıza Modeli 4 - [Kapsam(Scope)](/java-hafiza-yonetimi/Java-memory-models-scope/)
5. Java’da Hafıza Modeli 5 - [Pass By Value / Pass By Reference](/java-hafiza-yonetimi/Java-memory-models-pass-by-value-reference/)
6. Java’da Hafıza Modeli 6 - [Java’da Statik ve Statik Olmayan Değişken ve Metotların Hafıza Yönetimi](/java-hafiza-yonetimi/Java-memory-models-static-nonstatic-members/)
7. Java’da Hafıza Modeli 7 - [String Interning Nedir, String Pool Nedir, == operatörü ve equals metodu Arasındaki Fark](/java-hafiza-yonetimi/string-interning-string-pool/)
</div>

## Java Hafıza Modeli Örnek 1

Dilerseniz bir önceki yazıda değindiğimiz örneği bir adım öteye taşıyalım.

 ```java
 public class Sample {
     public static void main(String[] args) {
         int m = 11;
         SampleTest sample1 = new SampleTest(1,m);
         SampleTest sample2 = new SampleTest(3,sample1.y);
     }
 }
 ```

 ```java
 public class SampleTest {
     private int x;
     private int y;

     public SampleTest(int xx, int yy){
         this.x = xx;
         this.y = yy;
     }
     // ....
 }
 ```
---
Sizce bu sorunun cevabı hangisi olurdu?

{% picture 2020-02-29-Java-memory-models-objects1/sample8.png --alt Java reference variable assignment(java referans değişkeni atama) --img width="100%" height="100%" %}

{% picture 2020-02-29-Java-memory-models-objects1/sample9.png --alt Java reference variable assignment(java referans değişkeni atama) --img width="100%" height="100%" %}

Görüleceği üzere 2 adet **new**, bize <u>heap alanında 2 farklı nesnenin oluştuğunu</u> söylüyor. Burası önemli!!! Her yeni bir **new** anahtar kelimesi bize yeni bir nesneyi işaret etmektedir. Bu sebepten ötürü **A şıkkını** elemeliyiz. <u>Çünkü iki nesne de heap de tek bir nesneyi işaret etmektedir</u>. Halbuki bize 2 farklı nesne gerekiyor.

**B ve D seçeneklerinde** de bir terslik var. Bir önceki dersten de hatırlarsanız, biz, okla gösterimi heap alanındaki nesneyi stack'te kimin temsil ettiğini göstermek için kullanıyorduk. Yani **@ işareti** ile başlayan nesnenin id'si stack'teki referansın içinde tutmak yerine, bu gösterimi ok ile yapmıştık. Ama bu seçeneklerde ilkel bir tipe sahip olan **m** değişkeninin değerini heap'te yarattığımız nesnelere vermeye çalışıyoruz. Yani bir ilkel değişken atama söz konusu! Bu sebeple **C seçeneği** <u>doğru</u> seçenektir.

Doğru seçenek olan **C cevabını** scope dersine geçmeden önce bir adım öteye taşıyarak açıklamak istiyorum. Birinci kod bloğunun 3.satırında **m** isminde bir değişkene bir **int ilkel tipli** bir sayı atıyoruz. Buraya kadar her şey normal! 4.satırda ise aslında bildiğimiz gibi <u>heap'de bir nesne yaratma işlemi</u> vardır. Yalnız burada bir şeye değinmek istiyorum. Nesne oluştururken nesnenin aldığı parametreler için hiç görsel sunmadık. **SampleTest** sınıfı constructor'ına iki tane parametre almaktadır. Bunlar **xx** ve **yy** parametreleridir. 4.satırda nesne oluştururken parametrelerden **yy** olanına dikkat ettiyseniz **m** değişkeninin sahip olduğu değeri atamışız. Diğerine ise normal bir sayı!!! Yani bu değerlere stack alanında atama yapıldıktan sonra, constructor aracılığı ile heap'de yer alan nesnenin **x** ve **y** değerleri ilklendirilmektedir. Sonrasında ise bu **xx** ve **yy** parametreleri stack'den silinir. Aslında bu kısım tam olarak <u>böyle değil</u>, scope konusunda, kurucuları(constructor) da işin içine katıp daha farklı bir görsel resim sunmak istiyorum. Buradaki **xx** ve **yy** parametreleri **geçici** değişkenlerdir. Bazen **intermediate değişkenler** olarak da anılır. Ben ise bazen **kullan-at değişkenleri** diyorum. Çünkü asıl işleri olan constructor ilklendirmesini yaptıktan sonra yok olacaklardır. Scope konusunda henüz geçmediğimiz için şimdilik böyle olduğunu varsayın.

{% picture 2020-02-29-Java-memory-models-objects1/sample9-1.png --alt Java reference variable assignment(java referans değişkeni atama) --img width="100%" height="100%" %}

5.satırda da benzer şekilde bir nesne oluşturma işlemi gerçekleşmektedir. Aynı şekilde **SampleTest** sınıfı constructor'ına iki tane parametre almaktadır. Bunlar bildiğiniz gibi **xx** ve **yy** parametreleridir. Yalnız bu sefer **yy** parametresi, **sample1** değişkeninin/referansının işaret ettiği nesnenin **y** değikeninin sahip olduğu değeri almaktadır. Yalnız yeşil okla gösterdiğim hafıza modelimizin içinde yer almamaktadır. Buradaki amacım, constructor'da ilklendirme yapıldığını göstermek ve bu işlem sonrasında **xx** ve **yy** değerlerinin silindiğini belirtmektir. Aslında scope konusuna giriş yapınca bazı şeyler kafanızda daha da netleşecektir. Şimdilik aşağıdaki şekilden ne söylemek istediğimi daha net anlayabilirsiniz.

{% picture 2020-02-29-Java-memory-models-objects1/sample9-2.png --alt Java reference variable assignment(java referans değişkeni atama) --img width="100%" height="100%" %}

## Java Hafıza Modeli Örnek 2

Benzer bir soruyu çözmeye çalışalım. Sizce cevap nedir?

```java
public class Sample {
    public static void main(String[] args) {
        SampleTest sample1 = new SampleTest(1,2);
        SampleTest sample2 = new SampleTest(3,4);
        sample1 = sample2;
        sample1.x = 20;
        System.out.println(sample2.x + ", "+ sample2.y);
    }
}
```

```java
public class SampleTest {
    private int x;
    private int y;

    public SampleTest(int xx, int yy){
        this.x = xx;
        this.y = yy;
    }
    // ....
}
```
---
Birinci kod bloğunun 3. ve 4. satırlarında hafıza modeli aşağıdaki gibidir.

{% picture 2020-02-29-Java-memory-models-objects1/sample10.png --alt Java reference variable assignment(java referans değişkeni atama) --img width="100%" height="100%" %}

Yalnız 5. satırda bir atama işlemi yapılmaktadır. Yani **sample1** referansının sahip olduğu *id* **sample2**'nin *id*'si ile değiştiriyoruz. Yani heap alanında **sample1** referansının işaret ettiği nesne 4.satırda oluşturduğumuz nesneyi işaret etmektedir. Yani okla göstermeden önce **@ işareti** ile başlayan id ile gösterelim.

{% picture 2020-02-29-Java-memory-models-objects1/sample11.png --alt Java reference variable assignment(java referans değişkeni atama) --img width="100%" height="100%" %}

Id ile gösterimi gözünüzde canlandırabildiyseniz, okla gösterimden devam edebiliriz. Ama tekrar hatırlatmakta yarar var. Aslında okla gösterim az önceki **@** işareti ile başlayan sayıyının ne anlama geldiğini resmetmektedir. <u>Yani bu sayı bir nevi objenin heap alanındaki lokasyonunu</u> bize verir. Gerçekte sayı ile gösterim daha doğrudur. Ama okla yaptığımız gösterim, arka planda olanı daha net anlamanıza yardımcı olacağını düşünüyorum.

{% picture 2020-02-29-Java-memory-models-objects1/sample12.png --alt Java reference variable assignment(java referans değişkeni atama) --img width="100%" height="100%" %}

Buraya kadar olan biteni anladığınızı umuyorum. <u>Artık 2 referans da tek bir nesneyi işaret etmektedir.</u> Haliyle ilk oluşturduğumuz nesne heap alanında boşta kalacaktır ve tam da bu noktada **garbage collector** devreye girer. Hatırlarsanız garbage collector'un görevi boşta kalan bu nesneleri temizlemekti.

Kod bloğunun ``sample1.x = 20;`` 6.satırı olan bu yerde ise yeni bir atama işlemi ile karşılaşıyoruz. **sample1** referansı artık **sample2** referansı ile **aynı nesneyi** işaret ettiğine göre ``sample.x`` ile yaptığımız aslında aşağıdaki işlemdir. Yani ikinci nesnenin **x** değeridir.

{% picture 2020-02-29-Java-memory-models-objects1/sample13.png --alt Java reference variable assignment(java referans değişkeni atama) --img width="100%" height="100%" %}

Son satırda ise **sample2** referansının işaret ettiği nesnenin **x** ve **y** değerlerini ekrana yazdırma işlemi bulunmaktadır. Bir önceki satırda yaptığımız işlem aslında **sample2** referansının tuttuğu nesneyi de etkilediği için ekrana yazdıracağımız sonuç aşağıdaki gibi olacaktır.

```java
20, 4
```

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
