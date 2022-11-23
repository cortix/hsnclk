---
title: "Java'da Hafıza Modeli 4 - Kapsam(Scope)"
comments: false
excerpt: "Kapsam, değişkenin erişilebilir olduğu bölümüdür. Bu derste, Java hafıza modelini scope kavramını da dahil ederek bir bütün halinde ele alacağız."
header:
  teaser: "assets/images/equality.webp"
  og_image: /assets/images/equality.webp
  overlay_image: /assets/images/unsplash-image-33.webp
  overlay_filter: 0.5 #rgba(255, 0, 0, 0.5)
  caption: "Photo by [ian dooley](https://unsplash.com/photos/TevqnfbI0Zc) on Unsplash"
  #cta_label: "More Info"
  #cta_url: "https://unsplash.com"
categories:
  - java-hafiza-yonetimi
tags:
  - Scope(kapsam)
last_modified_at: 2020-06-06T15:12:19-04:00
toc: true
toc_label: "SAYFA İÇERİĞİ"
toc_sticky: true
---

**ÖNEMLİ :** Kendim için aldığım notlar. Umarım size de bir faydası olur. Kullanılan her bir makale referans olarak eklenmiştir.
{: .notice}

## ÖRNEK 1

Önceki bölümlerde scope kavramına gireceğimizden bahsetmiştik. Şimdi ise hafıza modelimizi bir adım daha öteye taşımanın zamanı geldi diye düşünüyorum.

## Bu Dersin Sonunda Yapabilecekleriniz
Hafıza modelleri ile ilgili diğer yazılarım iyi anlaşıldıysa, bu dersin sonunda, scope(kapsam) kavramını ve Java'nın scope ile ilgili temel kurallarını açıklayabileceksiniz. Aynı zamanda scope'u da dahil ederek hafıza modellerini çizmeyi öğreneceksiniz. Şu ana kadar scope'u dahil etmediğimiz için bazı şeyler havada kalmıştı. En azından bu kafa karışıklığını gidermiş olacak ve kod takibini belirli kurallar çerçevesinde daha iyi gerçekleştireceğiz.


## Scope(Kapsam) Giriş

Derse geçmeden önce aşağıdaki kodlarda kaç adet değişken olduğunu bulmanızı istiyorum.


```java
public class Sample {
    public static void main(String[] args) {
        SampleTest sample1 = new SampleTest(1,2);
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
Toplamda **6 tane** değişken bulunmaktadır. Bunları aşağıdaki şekilde görebilirsiniz.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-02-29-Java-memory-models-scope/var.webp"  width="100%" height="100%"  loading="lazy" alt="java variables">

Belki **this** anahtar kelimesini de değişken olarak tanımlamış olabilirsiniz.. Ama aslında pek değişken sayılmaz. Fakat değişken gibi davrandığını söyleyebiliriz. Birazdan scope konusuna geçince bu this anahtar sözcüğünü de ele alacağız. Bu yüzden toplamda **6 değişkenimiz** vardır. Bu değişkenlerin isimlerini düzgün bir şekilde tanımlayacak olursak, **xx** ve **yy** kurucumuzun(constructor) parametreleridir. Main metotumuzun içinde yer alan ve SampleTest nesnesini referans alan **sample1** de bir diğer değişkenimizdir. Diğer bir ifade şekliyle referansımız. **sample1** referansı(değişkeni) main metodu içinde yer aldığı için local(yerel) değişken olarak da ifade edilebilir.

Bu arada local değişkenler sadece bulundukları metotlar içinde yaşarlar. Scope konusuna başladığımızda ne demek istediğimi daha net anlayacaksınız. Bir diğer değişken ise **main** metodun içinde bulunan **args** parametresidir. Çoğu zaman unutulur ama bu da bir parametredir. Komut satırı parametreleri ile ilgili detaylı bilgiye şu [yazımdan](/java/Java-main-method/#komut-satırı-argümanlarıcommand-line-arguments) ulaşabilirsiniz. **x** ve **y** değişkenleri ise üye/örnek değişkenleri(member/instance variables) olarak geçer. Yani sınıftan türetilen herbir nesne bu değişkenlere sahip olacak ve bu değişkenler herbir nesne için özel olacaktır.

Peki mevcut kodumuzun içinde aşağıdaki gibi bir atama işlemi yapsak ne olurdu?

```java
public class Sample {
    public static void main(String[] args) {
        SampleTest sample1 = new SampleTest(1,2);
        x = 1;
    }
}
```
Burada bir derleme hatası alırdık. Çünkü **main** yönteminin sahip olduğu kapsamda(scope) önceden declare edilen bir **x** değişkeni yoktur. Peki kapsam(scope) nedir? <u><i>Bir değişkenin kapsamı, değişkenin bir değere sahip olması için tanımlandığı ve aynı zamanda değişkenin erişilebilir olduğu alandır.</i></u> Görüldüğü üzere **main** metotu içerisinde **x** değişkeni tanımlanmadan, atama işlemi gerçekleştirilmeye çalışılmıştır. Aslında **x** değişkeninin **SampleTest** sınıfında tanımlandığını ve bir üye değişkeni(member variable) olduğunu az önce öğrenmiştik. Bu yüzden ona main metotu içinde bu şekilde erişemeyiz.

<!-- Değişkenin kapsamı, kodda değişkenin belirli bir değere sahip olarak tanımlandığı alandır. -->
<!-- The scope of a variable is the area in the code where it's defined to have a particular value. The scope is the area where it is defined to have a value. -->

Şimdi birkaç farklı değişken türü ve bu değişkenler için kapsam belirleme kuralları hakkında konuşalım. Bahsetmek istediğim ilk değişken **yerel değişkendir(local variable)**. Yukarıdaki kod bloğunda **sample1** değişkenini/referansını görmektesiniz. Bu bir yerel değişkendir. Yerel değişkenler sadece bir metot içinde deklare edilirler. Bu değişken yöntemin içinde, "**if deyimi**" veya "**while döngüsü**" gibi herhangi bir bloğun dışında bildirilirse, çok rahat kullanılabilir ve bu yöntem boyunca ayarladığınız belirli bir değere sahiptir. Bu sebepten ötürü **sample1** değişkenini **main** metodun içinde herhangi bir yerde kullanabilirim, ancak **main** yönteminin dışında bir yerde kullanamam.

Bahsetmek istediğim ikinci tür değişken **parametrelerdir**. (Yukarıdaki kod bloğundan da hatırlayacağınız üzere **xx** ve **yy** birer parametredir) Parametreler, yöntemlere aktardığımız argümanlardır ve temel olarak yerel değişkenler gibi davranırlar. Onları yöntemin içinde herhangi bir yerde kullanabilirsiniz, isterseniz değerlerini de değiştirebilirsiniz. Aslında parametrelere local değişkenler de denilir.

Üçüncü değişken türü ise **üye değişkenleridir**. Artık üye değişkenleri(instance variable) zaten biliyorsunuz. Bunlar bir sınıftan yaratılan nesnelere ait değişkenlerdir(tabii member variable ile class variable farkını biliyorsunuz. İkiside aslında sınıfa ait değişkenlerdir ama üye değişkenleri ait olduğu sınıftan türetilen her bir nesne için özelken, sınıf değişkenleri ait olduğu sınıfın her bir nesnesi için tektir.)

Herhangi bir yöntemde yerel(local) olarak bildirilmek yerine, sınıftaki herhangi bir yöntemin dışında bildirilirler. Ve bu, değişkenlerin kapsamlarının aslında tüm sınıfın kendisi olduğu anlamına gelir. Bu nedenle, yukarıdaki kodda yer alan **x** ve **y** değişkenlerine sınıfın içerisindeki herhangi bir yöntemden ulaşabilirsiniz. Bu, yerel değişkenlerden daha geniş bir kapsama sahip olduklarını göstermektedir.

```java
public class Sample {
    public static void main(String[] args) {
        double m = 1.0;
        SampleTest sample1 = new SampleTest(m, 2);
    }
}
```

Yukarıdaki kod bloğunda gerçekleşenleri adım adım resmedecek olsak, önceki yazılardan da hatırlayacağınız üzere ilk önce **m** değişkeni için bir kutu çizip bu değişkene atama işlemi yapıyorduk. Yalnız bu işlemleri yaparken, değişkenlerin görülebilirliklerini de dikkate alarak kapsam(scope) kavramına giriş yapmak istiyorum. Kapsamı(scope) değişkenlerin etki alanı veyahut görülebildiği alan olarak düşünebilirsiniz.

**m** değişkeni görüleceği üzere **main** yönteminin içinde yaratılmıştır. Bu da bize bu değişkenin **main** kapsamı(scope) içinde yaşayabileceğini ve bu metodunun dışında bir yerden erişilemeyeceğini söyler. Aşağıdaki şekilden de görüleceği üzere artık tanımladığımız değişkenleri bir de içinde bulundukları kapsamları(scope) belirterek çizmek istiyorum.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-02-29-Java-memory-models-scope/scope1.webp"  width="100%" height="100%"  loading="lazy" alt="scope in java">

Bir sonraki satırda **SampleTest** sınıfı tipinde bir değişkenimizi(yani referans) oluşturuyoruz. Dikkat ederseniz bu değişkenin de kapsamı **main** yöntemidir.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-02-29-Java-memory-models-scope/scope2.webp"  width="100%" height="100%"  loading="lazy" alt="scope in java">

Şu ana kadar işlediklerimizi, yani scope dışında olan bölümleri önceki derslerde de görmüştük. **sample1** değişkeni/referansı bir **object type** değişkendir. **new** anahtar kelimesi ile bir nesne yaratılacağı sırada işler biraz karışmaktadır.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-02-29-Java-memory-models-scope/scope3.webp"  width="100%" height="100%"  loading="lazy" alt="scope in java">

Görüleceği üzere **2 üye değişkeni** ile birlikte nesnemiz heap alanında oluşturulmuştur. <u>Üye değişkenleri her zaman nesnenin içinde ve heap alanında yer alırlar.</u> Nesne oluşturulurken ilk etapda ilgili sınıfın constructor yöntemi çağrılır ve bu üye değişkenleri ilklendirilir(initialize). İlklendirilmekten kasıt değişken atama işlemidir. Bu ilklendirme işlemini de kurucunun aldığı parametreler sayesinde gerçekleştiririz. **Kurucu(constructor)** çağrılırken şu şekilde bir işlem gerçekleşir. Bir yandan ilgili sınıfı da görmekte yarar var.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-02-29-Java-memory-models-scope/scope4.webp"  width="100%" height="100%"  loading="lazy" alt="scope in java">

Constructor parametre olarak aldığı **xx** ve **yy** değişkenlerini nesnenin üye değişkenleri olan **x** ve **y** değişkenlerini ilklendirmek için kullanır. Constructor'un içindeki **this** anahtar kelimesi heap alanındaki ilgili nesneyi temsil etmektedir. Oku takip ederek üye değişkenlerinin nasıl ilklendirildiğini görebilirsiniz.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-02-29-Java-memory-models-scope/scope5.webp"  width="100%" height="100%"  loading="lazy" alt="scope in java">

Şimdi, **constructor** ile işimiz bitti. Peki constructor kapsamına ne olacak? Bir yöntem sona erdiğinde kapsamı da kaybolur. Böylece tüm bu yerel değişkenler, parametreler kaybolur. Onları bir daha asla kullanamazsınız. Yani içinde sahip olduğu her şey kapsamın yaşam ömrü kadardır. Kurucunun kapsamı silindikten sonra **main** yönteminin kapsamına geri dönebiliriz. Son olarak **sample1** değişkenine heap alanındaki nesnenin referansı verilir. Böylece kodda yer alan bütün adımları teker teker gerçekleştirmiş olduk.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-02-29-Java-memory-models-scope/scope6.webp"  width="100%" height="100%"  loading="lazy" alt="scope in java">


Burada **this** anahtar kelimesi ile ilgili ek bir bilgi vermek istiyorum. **this** opsiyoneldir. Siz belirtmeseniz de Java arka planda mantıksal bir çıkarım yaparak bu işlemi sizin yerinize zaten gerçekleştirir. Aşağıdaki kodu **this** anahtar kelimesi olmadan güncellemeyi deneyebilirsiniz.

```java
class SampleTest {
	    public double x;
	    public int y;

	    public SampleTest(double xx, int yy){
	        this.x = xx;
	        this.y = yy;
	    }
	    // ....
	}


	public class Sample {
	    public static void main(String[] args) {
	        double x = 1.0;
	        SampleTest sample1 = new SampleTest(x,2);
	    }
	}
```

> **Not:** Üye değişkenleri ingilizce olarak **member variables** veyahut **instance variables** olarak da geçer. Yani sınıftan üretilen herbir nesneye özel olan değişkenlerdir.

## Referanslar
* Bütün şekiller [https://www.lucidchart.com](https://www.lucidchart.com)'da tarafımdan hazırlanmıştır.
* [6.3. Scope of a Declaration](https://docs.oracle.com/javase/specs/jls/se8/html/jls-6.html#jls-6.3)
* [Java Heap Space vs Stack – Memory Allocation in Java](https://www.journaldev.com/4098/java-heap-space-vs-stack-memory)
* [Scope](https://www.iitk.ac.in/esc101/05Aug/tutorial/java/nutsandbolts/scope.html)
* [Object Oriented Programming in Java](https://www.coursera.org/learn/object-oriented-java?specialization=java-object-oriented)
* [Class ScopedMemory](https://docs.oracle.com/javase/realtime/doc_2.2u1/release/rtsj-docs/javax/realtime/ScopedMemory.html)
* [Class MemoryArea](https://docs.oracle.com/javase/realtime/doc_2.2u1/release/rtsj-docs/javax/realtime/MemoryArea.html)
* [Class HeapMemory](https://docs.oracle.com/javase/realtime/doc_2.2u1/release/rtsj-docs/javax/realtime/HeapMemory.html)
