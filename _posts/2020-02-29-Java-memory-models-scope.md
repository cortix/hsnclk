---
title: "Java'da Hafıza Modeli 4 - Kapsam(Scope)"
comments: false
excerpt: "Java'da kapsam(scope), değişkenin erişilebilir olduğu bölümüdür. Bu bölümde, Java hafıza modelini, kapsam(scope) kavramını da dahil ederek ele alacağız."
header:
  teaser: "/assets/images/svg-book26.svg"
  og_image: /assets/images/svg-book26.svg
  overlay_image: /assets/images/svg-book26.svg
  overlay_color: "#000"
  overlay_filter: "0.3"
  caption: "background by [SVGBackgrounds.com](https://www.svgbackgrounds.com/)"
  #cta_label: "More Info"
  #cta_url: "https://unsplash.com"
categories:
  - java-hafiza-yonetimi
tags:
  - Java memory model
  - Java Scope(kapsam)
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
2. Java’da Hafıza Modeli 2 - [Nesneler](/java-hafiza-yonetimi/Java-memory-models-objects/)
3. Java’da Hafıza Modeli 3 - [Nesneler](/java-hafiza-yonetimi/Java-memory-models-objects1/)
4. **Java’da Hafıza Modeli 4 - Kapsam(Scope)**
5. Java’da Hafıza Modeli 5 - [Pass By Value / Pass By Reference](/java-hafiza-yonetimi/Java-memory-models-pass-by-value-reference/)
6. Java’da Hafıza Modeli 6 - [Java’da Statik ve Statik Olmayan Değişken ve Metotların Hafıza Yönetimi](/java-hafiza-yonetimi/Java-memory-models-static-nonstatic-members/)
7. Java’da Hafıza Modeli 7 - [String Interning Nedir, String Pool Nedir, == operatörü ve equals metodu Arasındaki Fark](/java-hafiza-yonetimi/string-interning-string-pool/)
8. Java’da Hafıza Modeli 8 - [Java’da Static Initializer nedir? - Java statik ilklendirici](/java-hafiza-yonetimi/Java-statik-ilklendirici-1/)
9. Java’da Hafıza Modeli 9 - [Java’da Instance Initializer Nedir? - Java örnek ilklendirici](/java-hafiza-yonetimi/Java-ornek-ilklendirici-2/)
10. Java’da Hafıza Modeli 10 - [Java’da Neden BAZI durumlarda metot kullanmak yerine initializer’ı tercih ederiz?](/java-hafiza-yonetimi/Java-metot-yerine-ilklendirici-kullanmak-3/)
</div>

## ÖRNEK 1

Önceki bölümlerde **kapsam(scope)** kavramına gireceğimizden bahsetmiştik. Şimdi ise hafıza modelimizi bir adım daha öteye taşımanın zamanı geldi diye düşünüyorum.

## Bu Dersin Sonunda Yapabilecekleriniz
Java hafıza modelleri ile ilgili diğer yazılarım iyi anlaşıldıysa, bu dersin sonunda, scope(kapsam) kavramını ve Java'nın scope ile ilgili temel kurallarını açıklayabileceksiniz. Aynı zamanda scope'u da dahil ederek hafıza modellerini çizmeyi öğreneceksiniz. Şu ana kadar scope'u dahil etmediğimiz için bazı şeyler havada kalmıştı. En azından bu kafa karışıklığını gidermiş olacak ve kod takibini belirli kurallar çerçevesinde daha iyi gerçekleştireceğiz.


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

<br/>{% picture 2020-02-29-Java-memory-models-scope/var.png --alt Java variable(java değişkeni) --img width="100%" height="100%" %}<br/>

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

<br/>{% picture 2020-02-29-Java-memory-models-scope/scope1.png --alt Java scope(java'da kapsam) --img width="100%" height="100%" %}<br/>

Bir sonraki satırda **SampleTest** sınıfı tipinde bir değişkenimizi(yani referans) oluşturuyoruz. Dikkat ederseniz bu değişkenin de kapsamı **main** yöntemidir.

<br/>{% picture 2020-02-29-Java-memory-models-scope/scope2.png --alt Java scope(java'da kapsam) --img width="100%" height="100%" %}<br/>

Şu ana kadar işlediklerimizi, yani scope dışında olan bölümleri önceki derslerde de görmüştük. **sample1** değişkeni/referansı bir **object type** değişkendir. **new** anahtar kelimesi ile bir nesne yaratılacağı sırada işler biraz karışmaktadır.

<br/>{% picture 2020-02-29-Java-memory-models-scope/scope3.png --alt Java scope(java'da kapsam) --img width="100%" height="100%" %}<br/>

Görüleceği üzere **2 üye değişkeni** ile birlikte nesnemiz heap alanında oluşturulmuştur. <u>Üye değişkenleri her zaman nesnenin içinde ve heap alanında yer alırlar.</u> Nesne oluşturulurken ilk etapda ilgili sınıfın constructor yöntemi çağrılır ve bu üye değişkenleri ilklendirilir(initialize). İlklendirilmekten kasıt değişken atama işlemidir. Bu ilklendirme işlemini de kurucunun aldığı parametreler sayesinde gerçekleştiririz. **Kurucu(constructor)** çağrılırken şu şekilde bir işlem gerçekleşir. Bir yandan ilgili sınıfı da görmekte yarar var.

<br/>{% picture 2020-02-29-Java-memory-models-scope/scope4.png --alt Java scope(java'da kapsam) --img width="100%" height="100%" %}<br/>

Constructor parametre olarak aldığı **xx** ve **yy** değişkenlerini nesnenin üye değişkenleri olan **x** ve **y** değişkenlerini ilklendirmek için kullanır. Constructor'un içindeki **this** anahtar kelimesi heap alanındaki ilgili nesneyi temsil etmektedir. Oku takip ederek üye değişkenlerinin nasıl ilklendirildiğini görebilirsiniz.

<br/>{% picture 2020-02-29-Java-memory-models-scope/scope5.png --alt Java scope(java'da kapsam) --img width="100%" height="100%" %}<br/>

Şimdi, **constructor** ile işimiz bitti. Peki constructor kapsamına ne olacak? Bir yöntem sona erdiğinde kapsamı da kaybolur. Böylece tüm bu yerel değişkenler, parametreler kaybolur. Onları bir daha asla kullanamazsınız. Yani içinde sahip olduğu her şey kapsamın yaşam ömrü kadardır. Kurucunun kapsamı silindikten sonra **main** yönteminin kapsamına geri dönebiliriz. Son olarak **sample1** değişkenine heap alanındaki nesnenin referansı verilir. Böylece kodda yer alan bütün adımları teker teker gerçekleştirmiş olduk.

<br/>{% picture 2020-02-29-Java-memory-models-scope/scope6.png --alt Java scope(java'da kapsam) --img width="100%" height="100%" %}<br/>

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

* Bütün şekiller [lucidchart](https://www.lucidchart.com)'da tarafımdan hazırlanmıştır.
* [6.3. Scope of a Declaration](https://docs.oracle.com/javase/specs/jls/se8/html/jls-6.html#jls-6.3)
* [Java Heap Space vs Stack – Memory Allocation in Java](https://www.journaldev.com/4098/java-heap-space-vs-stack-memory)
* [Scope](https://www.iitk.ac.in/esc101/05Aug/tutorial/java/nutsandbolts/scope.html)
* [Object Oriented Programming in Java](https://www.coursera.org/learn/object-oriented-java?specialization=java-object-oriented)
* [Class ScopedMemory](https://docs.oracle.com/javase/realtime/doc_2.2u1/release/rtsj-docs/javax/realtime/ScopedMemory.html)
* [Class MemoryArea](https://docs.oracle.com/javase/realtime/doc_2.2u1/release/rtsj-docs/javax/realtime/MemoryArea.html)
* [Class HeapMemory](https://docs.oracle.com/javase/realtime/doc_2.2u1/release/rtsj-docs/javax/realtime/HeapMemory.html)
