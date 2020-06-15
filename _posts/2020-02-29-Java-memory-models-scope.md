---
title: "Java'da Hafıza Modeli 4 - Kapsam(Scope)"
comments: true
excerpt: "Java'da Nesneler Bellekte Nasıl Saklanır? Farklı Örnekler"
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
  - scope(kapsam)
tags:
  - Nesneler İçin Hafıza Modeli
last_modified_at: 2020-06-06T15:12:19-04:00
toc: true
toc_label: "SAYFA İÇERİĞİ"
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
Toplamda 6 tane değişken bulunmaktadır. Bunları aşağıdaki şekilde görebilirsiniz.

<figure style="width: 650px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-02-29-Java-memory-models-scope/var.png" alt="variables">
  <figcaption>Captured by using Jing</figcaption>
</figure>

Belki **this** anahtar kelimesini de değişken olarak tanımlamış olabilirsiniz.. Ama aslında pek değişken sayılmaz. Fakat değişken gibi davrandığını söyleyebiliriz. Birazdan scope konusuna geçince bu this anahtar sözcüğünü de ele alacağız. Bu yüzden toplamda 6 değişkenimiz vardır. Bu değişkenlerin isimlerini düzgün bir şekilde tanımlayacak olursak, **xx** ve **yy** kurucumuzun(constructor) parametreleridir. Main metotumuzun içinde yer alan ve SampleTest nesnesini referans alan ``sample1`` de bir diğer değişkenimizdir. ``sample1`` değişkeni yerel değişken olarak tanımlanır. Yerel değişkenler sadece bulundukları metotlar içinde yaşarlar. Scope konusuna başladığımızda ne demek istediğimi daha net anlayacaksınız. Bir diğer değişken ise main metodun içinde bulunan ``args`` parametresidir. Çoğu zaman unutulur ama bu da bir parametredir. Komut satırı parametreleri ile ilgili detaylı bilgiye şu [yazımdan](/java/Java-main-method/#komut-satırı-argümanlarıcommand-line-arguments) ulaşabilirsiniz. **x** ve **y** değişkenleri ise üye/örnek değişkenleri(member/instance variables) olarak geçer. Yani sınıftan türetilen herbir nesne bu değişkenlere sahip olacak ve bu değişkenler herbir nesne için özel olacaktır.

Peki mevcut kodumuzun içinde aşağıdaki gibi bir atama işlemi yapsak ne olurdu?

```java
public class Sample {
    public static void main(String[] args) {
        SampleTest sample1 = new SampleTest(1,2);
        x = 1;
    }
}
```
Burada bir derleme hatası alırdık. Çünkü ``main`` yönteminin sahip olduğu kapsamda(scope) **x** değişkeni yoktur. Peki kapsam(scope) nedir? <u><i>Bir değişkenin kapsamı, değişkenin bir değere sahip olması için tanımlandığı alandır.</i></u> Görüldüğü üzere main metotu içerisinde x değişkeni tanımlanmadan, atama işlemi gerçekleştirilmeye çalışılmıştır. Aslında x değişkeninin SampleTest sınıfında tanımlandığını ve bir üye değişkeni(member variable) olduğunu az önce öğrenmiştik. Bu yüzden ona main metotu içinde bu şekilde erişemeyiz.

<!-- Değişkenin kapsamı, kodda değişkenin belirli bir değere sahip olarak tanımlandığı alandır. -->
<!-- The scope of a variable is the area in the code where it's defined to have a particular value. The scope is the area where it is defined to have a value. -->

Şimdi birkaç farklı değişken türü ve bu değişkenler için kapsam belirleme kuralları hakkında konuşalım. Bahsetmek istediğim ilk değişken yerel değişkendir(local variable). Yukarıdaki kod bloğunda ``sample1`` değişkenini görmektesiniz. Bu bir yerel değişkendir. Yerel değişkenler sadece bir metot içinde deklare edilirler. Bu değişken yöntemin içinde, "if deyimi" veya "while döngüsü" gibi herhangi bir bloğun dışında bildirilirse, çok rahat kullanılabilir ve bu yöntem boyunca ayarladığınız belirli bir değere sahiptir. Bu sebepten ötürü ``sample1`` değişkenini ``main`` metodun içinde herhangi bir yerde kullanabilirim, ancak ``main`` yönteminin dışında bir yerde kullanamam.

Bahsetmek istediğim ikinci tür değişken parametrelerdir. (Yukarıdaki kod bloğundan da hatırlayacağınız üzere **xx** ve **yy** birer parametredir) Parametreler, yöntemlere aktardığımız argümanlardır ve temel olarak yerel değişkenler gibi davranırlar. Onları yöntemin içinde herhangi bir yerde kullanabilirsiniz, isterseniz değerlerini de değiştirebilirsiniz.

Üçüncü değişken türü ise üye değişkenlerdir. Artık üye değişkenleri zaten biliyorsunuz. Bunlar bir sınıfa ait değişkenlerdir(tabii member variable ile class variable farkını biliyorsunuz. İkiside aslında sınıfa ait değişkenlerdir ama üye değişkenleri ait olduğu sınıftan türetilen her bir nesne için özelken, sınıf değişkenleri ait olduğu sınıfın her bir nesnesi için tektir.) Herhangi bir yöntemde yerel olarak bildirilmek yerine, sınıftaki herhangi bir yöntemin dışında bildirilirler. Ve bu, değişkenlerin kapsamlarının aslında tüm sınıfın kendisi olduğu anlamına gelir. Bu nedenle, yukarıdaki kodda yer alan **x** ve **y** değişkenlerine sınıfın içerisindeki herhangi bir yöntemden ulaşabilirsiniz. Bu, yerel değişkenlerden daha geniş bir kapsama sahip olduklarını göstermektedir.

```java
public class Sample {
    public static void main(String[] args) {
        double m = 1.0;
        SampleTest sample1 = new SampleTest(m, 2);
    }
}
```

Yukarıdaki kod bloğunda gerçekleşenleri adım adım resmedecek olsak, önceki yazılardan da hatırlayacağınız üzere ilk önce m değişkeni için bir kutu çizip bu değişkene atama işlemi yapıyorduk. Yalnız bu işlemleri yaparken, değişkenlerin görülebilirliklerini de dikkate alarak kapsam(scope) kavramına giriş yapmak istiyorum. Kapsamı(scope) değişkenlerin etki alanı veyahut görülebildiği alan olarak düşünebilirsiniz.

m değişkeni görüleceği üzere ``main`` yönteminin içinde yaratılmıştır. Bu da bize bu değişkenin ``main`` kapsamı(scope) içinde yaşayabileceğini ve bu metodunun dışında bir yerden erişilemeyeceğini söyler. Aşağıdaki şekilden de görüleceği üzere artık tanımladığımız değişkenleri bir de içinde bulundukları kapsamları(scope) belirterek çizmek istiyorum.

<figure style="width: 650px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-02-29-Java-memory-models-scope/scope1.png" alt="scope">
  <figcaption>Lucidchart'da hazırlanmıştır</figcaption>
</figure>

Bir sonraki satırda SampleTest sınıfı tipinde bir değişkenimizi oluşturuyoruz. Dikkat ederseniz bu değişkenin de kapsamı main yöntemidir.


<figure style="width: 650px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-02-29-Java-memory-models-scope/scope2.png" alt="scope">
  <figcaption>Lucidchart'da hazırlanmıştır</figcaption>
</figure>

Şu ana kadar işlediklerimizi, yani scope dışında olan bölümleri önceki derslerde de görmüştük. sample1 değişkeni bir object type değişkendir. new anahtar kelimesi ile bir nesne yaratılacağı sırada işler biraz karışmaktadır.

<figure style="width: 650px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-02-29-Java-memory-models-scope/scope3.png" alt="scope">
  <figcaption>Lucidchart'da hazırlanmıştır</figcaption>
</figure>

Görüleceği üzere 2 üye değişkeni ile birlikte nesnemiz heap alanında oluşturulmuştur. Üye değişkenleri her zaman nesnenin içinde ve heap alanında yer alırlar. Nesne oluşturulurken ilk etapda ilgili sınıfın constructor yöntemi çağrılır ve bu üye değişkenleri ilklendirilir. İlklendirilmekten kasıt değişken atama işlemidir. Bu ilklendirme işlemini de kurucunun aldığı parametreler sayesinde gerçekleştiririz. Kurucu(constructor) çağrılırken şu şekilde bir işlem gerçekleşir. Bir yandan ilgili sınıfı da görmekte yarar var.

<figure style="width: 650px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-02-29-Java-memory-models-scope/scope4.png" alt="scope">
  <figcaption>Lucidchart'da hazırlanmıştır</figcaption>
</figure>

Constructor parametre olarak aldığı **xx** ve **yy** değişkenlerini nesnenin üye değişkenleri olan **x** ve **y** değişkenlerini ilklendirmek için kullanır. Constructor'un içindeki **this** anahtar kelimesi heap alanındaki nesneyi temsil etmektedir. Oku takip ederek üye değişkenlerinin nasıl ilklendirildiğini görebilirsiniz.

<figure style="width: 650px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-02-29-Java-memory-models-scope/scope5.png" alt="scope">
  <figcaption>Lucidchart'da hazırlanmıştır</figcaption>
</figure>

Şimdi, constructor ile işimiz bitti. Peki constructor kapsamına ne olacak? Bir yöntem sona erdiğinde kapsamı da kaybolur. Böylece tüm bu yerel değişkenler, parametreler kaybolur. Onları bir daha asla kullanamazsınız. Yani içinde sahip olduğu her şey kapsamın yaşam ömrü kadardır. Kurucunun kapsamı silindikten sonra main yönteminin kapsamına geri dönebiliriz. Son olarak ``sample1`` değişkenine heap alanındaki nesnenin referansı verilir. Böylece kodda yer alan bütün adımları teker teker gerçekleştirmiş olduk.

<figure style="width: 650px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-02-29-Java-memory-models-scope/scope6.png" alt="scope">
  <figcaption>Lucidchart'da hazırlanmıştır</figcaption>
</figure>

Burada ``this`` anahtar kelimesi ile ilgili ek bir bilgi vermek istiyorum. ``this`` opsiyoneldir. Siz belirtmeseniz de Java arka planda mantıksal bir çıkarım yaparak bu işlemi sizin yerinize zaten gerçekleştirir. Aşağıdaki kodu ``this`` anahtar kelimesi olmadan güncellemeyi deneyebilirsiniz. Kodun biraz altında bulunan **Prev** ve **Next** butonlarını kullanarak kodu satır satır ilerletebilir ve hafızada nasıl oluştuğunu gözlemleyebilirsiniz.

<iframe width="800" height="500" frameborder="0" src="http://pythontutor.com/iframe-embed.html#code=class%20SampleTest%20%7B%0A%20%20%20%20public%20double%20x%3B%0A%20%20%20%20public%20int%20y%3B%0A%0A%20%20%20%20public%20SampleTest%28double%20xx,%20int%20yy%29%7B%0A%20%20%20%20%20%20%20%20this.x%20%3D%20xx%3B%0A%20%20%20%20%20%20%20%20this.y%20%3D%20yy%3B%0A%20%20%20%20%7D%0A%20%20%20%20//%20....%0A%7D%0A%0A%0Apublic%20class%20Sample%20%7B%0A%20%20%20%20public%20static%20void%20main%28String%5B%5D%20args%29%20%7B%0A%20%20%20%20%20%20%20%20double%20x%20%3D%201.0%3B%0A%20%20%20%20%20%20%20%20SampleTest%20sample1%20%3D%20new%20SampleTest%28x,2%29%3B%0A%20%20%20%20%7D%0A%7D&codeDivHeight=400&codeDivWidth=350&cumulative=false&curInstr=3&heapPrimitives=nevernest&origin=opt-frontend.js&py=java&rawInputLstJSON=%5B%5D&textReferences=false"> </iframe>


> **Not:** Üye değişkenleri ingilizce olarak member variables veyahut instance variables olarak da geçer. Yani sınıftan üretilen herbir nesneye özel olan değişkenlerdir. 

## Referanslar
* Bütün şekiller [https://www.lucidchart.com](https://www.lucidchart.com)'da tarafımdan hazırlanmıştır.
* [https://docs.oracle.com/javase/specs/jls/se8/html/jls-6.html#jls-6.3](https://docs.oracle.com/javase/specs/jls/se8/html/jls-6.html#jls-6.3)
* [https://www.journaldev.com/4098/java-heap-space-vs-stack-memory](https://www.journaldev.com/4098/java-heap-space-vs-stack-memory)
* [https://www.iitk.ac.in/esc101/05Aug/tutorial/java/nutsandbolts/scope.html](https://www.iitk.ac.in/esc101/05Aug/tutorial/java/nutsandbolts/scope.html)
* [https://www.coursera.org/learn/object-oriented-java/lecture/mqFZ3/core-introduction-to-scope](https://www.coursera.org/learn/object-oriented-java/lecture/mqFZ3/core-introduction-to-scope)
* [http://archive.oreilly.com/oreillyschool/courses/csharp2/csharp214.html](http://archive.oreilly.com/oreillyschool/courses/csharp2/csharp214.html)
* [https://docs.oracle.com/javase/realtime/doc_2.2u1/release/rtsj-docs/javax/realtime/ScopedMemory.html](https://docs.oracle.com/javase/realtime/doc_2.2u1/release/rtsj-docs/javax/realtime/ScopedMemory.html)
* [https://docs.oracle.com/javase/realtime/doc_2.2u1/release/rtsj-docs/javax/realtime/MemoryArea.html](https://docs.oracle.com/javase/realtime/doc_2.2u1/release/rtsj-docs/javax/realtime/MemoryArea.html)
* [https://docs.oracle.com/javase/realtime/doc_2.2u1/release/rtsj-docs/javax/realtime/HeapMemory.html](https://docs.oracle.com/javase/realtime/doc_2.2u1/release/rtsj-docs/javax/realtime/HeapMemory.html)
* Real-Time Java Platform Programming
