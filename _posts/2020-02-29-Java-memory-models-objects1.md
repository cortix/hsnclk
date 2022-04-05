---
title: "Java'da Hafıza Modeli 3 - Nesneler"
comments: true
excerpt: "Java'da Nesneler Bellekte Nasıl Saklanır? Farklı Örnekler"
header:
  teaser: "assets/images/equality.png"
  og_image: /assets/images/page-header-og-image.png
  overlay_image: /assets/images/unsplash-image-35.jpg
  overlay_filter: 0.5 #rgba(255, 0, 0, 0.5)
  caption: "Photo by [redcharlie](https://unsplash.com/photos/k2zWqv_yfNM) on Unsplash"
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

## ÖRNEK 1

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
Sizce bu sorunun cevabı hangisi olurdu?

<figure style="width: 600px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-02-29-Java-memory-models-objects1/sample8.png" alt="async-variable">
  <figcaption></figcaption>
</figure>
<figure style="width: 600px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-02-29-Java-memory-models-objects1/sample9.png" alt="async-variable">
  <figcaption></figcaption>
</figure>

Görüleceği üzere 2 adet ``new``, bize heap alanında 2 farklı nesnenin oluştuğunu söylüyor. Burası önemli!!! Her yeni bir ``new`` anahtar kelimesi bize yeni bir nesneyi işaret etmektedir. Bu sebepten ötürü A şıkkkını elemeliyiz. Çünkü iki nesne de heap de tek bir nesneyi işaret etmektedir. Halbuki bize 2 farklı nesne gerekiyor.

B ve D şeçeneklerinde de bir terslik var. Bir önceki dersten de hatırlarsanız, biz, okla gösterimi heap alanındaki nesneyi stack'te kimin temsil ettiğini göstermek için kullanıyorduk. Yani @ işareti ile başlayan nesnenin id'si stack'teki referansın içinde tutmak yerine, bu gösterimi ok ile yapmıştık. Ama bu seçeneklerde ilkel bir tipe sahip olan **m** değişkeninin değerini heap'te yarattığımız nesnelere vermeye çalışıyoruz. Yani bir ilkel değişken atama söz konusu! Bu sebeple C seçeneği doğru seçenektir.

Doğru seçenek olan C cevabını scope dersine geçmeden önce bir adım öteye taşıyarak açıklamak istiyorum. Birinci kod bloğunun 3.satırında m isminde bir değişkene bir int ilkel tipli bir sayı atıyoruz. Buraya kadar her şey normal! 4.satırda ise aslında bildiğimiz gibi heapde bir nesne yaratma işlemi vardır. Yalnız burada bir şeye değinmek istiyorum. Nesne oluştururken nesnenin aldığı parametreler için hiç görsel sunmadık. ``SampleTest`` sınıfı constructor'ına iki tane parametre almaktadır. Bunlar ``xx`` ve ``yy`` parametreleridir. 4.satırda nesne oluştururken parametrelerden ``yy`` olanına dikkat ettiyseniz m değişkeninin sahip olduğu değeri atamışız. Diğerine ise normal bir sayı!!! Dikkat ederseniz bu değerlere stack alanında atama yapıldıktan sonra, constructor aracılığı ile heap'de yer alan nesnenin ``x`` ve ``y`` değerleri ilklendirilmektedir. Sonrasında ise bu ``xx`` ve ``yy`` parametreleri stack'den silinir. Aslında bu kısım tam olarak böyle değil, scope konusunda, kurucuları(constructor) da işin içine katıp daha farklı bir görsel resim sunmak istiyorum. Buradaki ``xx`` ve ``yy`` parametreleri geçici değişkenlerdir. Bazen intermediate değişkenler olarak da anılır. Ben ise bazen kullan-at değişkenleri diyorum. Çünkü asıl işleri olan constructor ilklendirmesini yaptıktan sonra yok olacaklardır. Scope konusunda henüz geçmediğimiz için şimdilik böyle olduğunu varsayın.

<figure style="width: 600px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-02-29-Java-memory-models-objects1/sample9-1.png" alt="async-variable">
  <figcaption></figcaption>
</figure>

5.satırda da benzer şekilde bir nesne oluşturma işlemi gerçekleşmektedir. Aynı şekilde ``SampleTest`` sınıfı constructor'ına iki tane parametre almaktadır. Bunlar bildiğiniz gibi ``xx`` ve ``yy`` parametreleridir. Yalnız bu sefer ``yy`` parametresi, ``sample1`` değişkeninin/referansının işaret ettiği nesnenin ``y`` değikeninin sahip olduğu değeri almaktadır. Yalnız yeşil okla gösterdiğim hafıza modelimizin içinde yer almamaktadır. Buradaki amacım, constructor'da ilklendirme yapıldığını göstermek ve bu işlem sonrasında ``xx`` ve ``yy`` değerlerinin silindiğini belirtmektir. Aslında scope konusuna giriş yapınca bazı şeyler kafanızda daha da netleşecektir. Şimdilik aşağıdaki şekilden ne söylemek istediğimi daha net anlayabilirsiniz.

<figure style="width: 600px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-02-29-Java-memory-models-objects1/sample9-2.png" alt="async-variable">
  <figcaption></figcaption>
</figure>


## ÖRNEK 2

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

Birinci kod bloğunun 3. ve 4. satırlarında hafıza modeli aşağıdaki gibidir.

<figure style="width: 600px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-02-29-Java-memory-models-objects1/sample10.png" alt="async-variable">
  <figcaption></figcaption>
</figure>

Yalnız 5. satırda bir atama işlemi yapılmaktadır. Yani **sample1** referansının sahip olduğu *id* **sample2**'nin id'si ile değiştiriyoruz. Yani heap alanında **sample1** referansının işaret ettiği nesne 4.satırda oluşturduğumuz nesneyi işaret etmektedir. Yani okla göstermeden önce @ işareti ile başlayan id ile gösterelim.

<figure style="width: 600px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-02-29-Java-memory-models-objects1/sample11.png" alt="async-variable">
  <figcaption></figcaption>
</figure>

Id ile gösterimi gözünüzde canlandırabildiyseniz, okla gösterimden devam edebiliriz. Ama tekrar hatırlatmakta yarar var. Aslında okla gösterim az önceki **@** işareti ile başlayan sayıyının ne anlama geldiğini resmetmektedir. Yani bu sayı bir nevi objenin heap alanındaki lokasyonunu bize verir. Gerçekte sayı ile gösterim daha doğrudur. Ama okla yaptığımız gösterim, arka planda olanı daha net anlamanıza yardımcı olacağını düşünüyorum.

<figure style="width: 600px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-02-29-Java-memory-models-objects1/sample12.png" alt="async-variable">
  <figcaption></figcaption>
</figure>

Buraya kadar olan biteni anladığınızı umuyorum. Artık 2 referans da tek bir nesneyi işaret etmektedir. Haliyle ilk oluşturduğumuz nesne heap alanında boşta kalacaktır ve tam da bu noktada **garbage collector** devreye girer. Hatırlarsanız garbage collector'un görevi boşta kalan bu nesneleri temizlemekti.

Kod bloğunun ``sample1.x = 20;`` 6.satırı olan bu yerde ise yeni bir atama işlemi ile karşılaşıyoruz. **sample1** referansı artık **sample2** referansı ile **aynı nesneyi** işaret ettiğine göre ``sample.x`` ile yaptığımız aslında aşağıdaki işlemdir. Yani ikinci nesnenin **x** değeridir.

<figure style="width: 600px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-02-29-Java-memory-models-objects1/sample13.png" alt="async-variable">
  <figcaption></figcaption>
</figure>

Son satırda ise **sample2** referansının işaret ettiği nesnenin **x** ve **y** değerlerini ekrana yazdırma işlemi bulunmaktadır. Bir önceki satırda yaptığımız işlem aslında **sample2** referansının tuttuğu nesneyi de etkilediği için ekrana yazdıracağımız sonuç aşağıdaki gibi olacaktır.

```java
20, 4
```

## Referanslar
* Bütün şekiller [https://www.lucidchart.com](https://www.lucidchart.com)'da tarafımdan hazırlanmıştır.
* [https://docs.oracle.com/javase/tutorial/java/nutsandbolts/datatypes.html](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/datatypes.html)
* [https://www.w3schools.com/java/java_data_types.asp](https://www.w3schools.com/java/java_data_types.asp)
* [https://www.baeldung.com/java-initialization](https://www.baeldung.com/java-initialization)
* [https://www.baeldung.com/java-primitives](https://www.baeldung.com/java-primitives)
* [https://www.baeldung.com/java-stack-heap](https://www.baeldung.com/java-stack-heap)
* [https://dzone.com/articles/stack-vs-heap-understanding-java-memory-allocation](https://dzone.com/articles/stack-vs-heap-understanding-java-memory-allocation)
* [https://www.coursera.org/learn/object-oriented-java?specialization=java-object-oriented](https://www.coursera.org/learn/object-oriented-java?specialization=java-object-oriented)
* [https://www.journaldev.com/4098/java-heap-space-vs-stack-memory](https://www.journaldev.com/4098/java-heap-space-vs-stack-memory)
* [https://blogs.oracle.com/jonthecollector/presenting-the-permanent-generation](https://blogs.oracle.com/jonthecollector/presenting-the-permanent-generation)
* [https://www.journaldev.com/797/what-is-java-string-pool](https://www.journaldev.com/797/what-is-java-string-pool)
* [https://www.baeldung.com/java-string-pool](https://www.baeldung.com/java-string-pool)
