---
title: "Java Sınıf Deklarasyonu ve Değiştiriciler"
comments: false
excerpt: "Java sınıf deklarasyonu nasıl yapılır? Değiştiriciler hakkında bilmeniz gerekenler nelerdir gibi soruları bu bölümde bulabilirsiniz."
header:
  teaser: "assets/images/equality.webp"
  og_image: /assets/images/equality.webp
  overlay_image: /assets/images/unsplash-image-4.webp
  overlay_filter: 0.5 #rgba(255, 0, 0, 0.5)
  caption: "Photo credit: [**Unsplash**](https://unsplash.com)"
  #cta_label: "More Info"
  #cta_url: "https://unsplash.com"
categories:
  - java
tags:
  - sınıf deklarasyonu
  - değiştiriciler
last_modified_at: 2018-06-06T15:12:19-04:00
toc: true
toc_label: "SAYFA İÇERİĞİ"
toc_sticky: true
---



**Not :** Kendim için aldığım notlar. Umarım size de bir faydası olur. Kullanılan her bir makale referans olarak eklenmiştir.
{: .notice}

## Java Sınıf Deklarasyonu

Köşeli parantezler arasındaki alan (yani sınıf gövdesi), sınıftan yaratılan nesnelerin yaşam döngüsünü sağlayan tüm kodu içerir:
* yeni nesneleri başlatmak için kurucular(**constructor**),
* sınıfın ve bu sınıfın nesnelerinin durumunu(**state**) sağlayan alanlar(**fields**) için deklarasyonlar ve sınıfın ve bu sınıfın nesnelerinin davranışlarını uygulamak için yöntemler.

Basit haliyle bir sınıfın deklarasyonu şu şekildedir.

{% highlight java %}
class MyClass {
    // field, constructor, and
    // method declarations
}
}
{% endhighlight %}

Java kuralları gereği bir sınıfın isminin **baş harfi** büyük olmak zorundadır. Yukarıdaki örnek en yalın haliyle bir sınıf tanımıdır. Bu tanımı genişletmekte mümkündür.

{% highlight java %}
class MyClass extends MySuperClass implements YourInterface {
    // field, constructor, and
    // method declarations
}
{% endhighlight %}

Bu, **MyClass**'ın **MySuperClass**'ın bir alt sınıfı olduğu ve **YourInterface** arayüzünü uyguladığı anlamına gelir.

Yukarıda gördüğümüz örnekler üst düzey sınıf deklarasyon örnekleridir. Bunlara ek olarak Java, **nested classes** veya **inner classes** olarak bilinen diğer sınıf kategorilerini de sağlar.

<div class="notice--success" markdown="1">
<h4 class="no_toc"><i class="fas fa-lightbulb"></i> Modifiers</h4>
---
Yukarıdaki kodun derlenmesinde bir sıkıntı olmaz fakat kodu daha işlevsel hale getirmekte değiştiricilerle(**modifiers**) mümkündür. Genel olarak modifiers **2 kategoriye** ayrılır. Bunlar;

* **Access modifiers** (public, protected, private)
* **Nonaccess modifiers** (strictfp, final ve abstract)

Erişim kontrolü (**access control**) normalde <u>4 kategoriye</u> ayrılır fakat **access modifiers** <u>3 tanedir</u>. 4. erişim kontrolü varsayılan(**default**) ya da **package access** paket erişimi olarak bilinir.

Aslında üç erişim değiştiriciden hiçbirini kullanmadığınızda bir nevi bu erişim kontrolünü uygulamış olursunuz.
</div>

Her dört erişim kontrolünün (yani, her üç değiştirici) çoğu yöntem ve değişken beyanı için çalışmasına rağmen, bir sınıf sadece **public** veya varsayılan(**default**) erişim ile bildirilebilir. Diğer iki erişim seviyesi bir sınıf tanımı için pek mantıklı değildir.

Java, paket merkezli bir dildir; Geliştiriciler, iyi organizasyon ve isim belirleme için tüm sınıflarını paketlerde saklarlar.

## Sınıf erişimi

Bir sınıfa erişmek ne anlama geliyor? Bir sınıftan (A sınıfı) kodun başka bir sınıfa (sınıf B) erişimi olduğunu söylediğimizde, A sınıfı üç şeyden birini yapabilir:

* Sınıf B'nin örneği(**instance**) oluşturmak.
* B sınıfını miras almak(**extend**) (diğer bir deyişle, B sınıfının bir alt sınıfını oluşturmak)
* Yöntem ve değişkenlerin erişim kontrolüne bağlı olarak B sınıfı içindeki bazı yöntem ve değişkenlere erişim.

Aslında, erişim görünürlük(**visibility**) anlamına gelir. A sınıfı B sınıfı göremiyorsa, B sınıfı içindeki yöntem ve değişkenlerin erişim düzeyi önemli değildir; A sınıfının bu yöntemlere ve değişkenlere erişmenin herhangi bir yolu yoktur.

### Default Access
Sınıf beyanında bir değiştirici yazmazsanız bu **default access**(varsayılan erişim) anlamına gelir. Yani sadece aynı paket içerisinde bulunan sınıflar birbirlerine görünürdür. Bu yüzden **package access** paket erişimi de denilmektedir.

### Public Access
*public* anahtar kelimesiyle yapılan bir sınıf bildirimi, tüm paketlerdeki tüm sınıfların bu public sınıfa erişmesini sağlar. Diğer bir deyişle, Java ekosistemi içindeki tüm sınıfların bir *public* sınıfa erişimi vardır. Yani bütün sınıflar tarafından görünür ve kullanılabilir durumdadır.

### Other (Non-access) Class Modifiers

``final``, ``abstract`` veya ``strictfp`` anahtar sözcüğünü kullanarak bir sınıf beyanını değiştirebilirsiniz. Bu değiştiriciler, sınıfın erişim kontrolü ne olursa olsun, yani bir sınıfı hem ``public`` hem de ``final`` olarak deklare edebilirsiniz. Ancak, **non-access** değiştiricileri her zaman karıştıramazsınız. Örneğin, ``final`` ile kombinasyon halinde ``strictfp``'i kullanmakta özgürsünüz, ama asla ve asla, bir sınıfı hem ``final`` hem de ``abstract`` olarak işaretleyemezsiniz.

### Final Classes
Bir sınıf beyanında kullanıldığında, ``final`` anahtar kelimesi sınıfın alt sınıflara ayrılamayacağı anlamına gelir. Başka bir deyişle, başka hiçbir sınıf bir ``final`` sınıfını genişletemez (yani miras alamaz) ve bunu yapmak istediğinizde bir derleyici hatası alırsınız.

Peki neden bir ``final`` işaretini kullanalım? Ne de olsa, OO'nun miras anlayışını ihlal etmiyor mu? Bir ``final`` sınıf, ancak o sınıfa ait yöntemlerin hiçbirinin geçersiz kılınmayacağına dair mutlak bir garantiye ihtiyacınız varsa, yapmalısınız. Java'daki çekirdek kütüphanelerinde ``final`` olan birçok sınıf görebilirsiniz. Örneğin, String sınıfının bir alt sınıfı oluşturulamaz.

### Abstract Classes
Soyut(**Abstract**) bir sınıf asla örneklenemez. Tek amacı, yaşamdaki misyonu, genişletilmesidir (**alt sınıf oluşturulması**). Soyut yöntemlerin sonu, köşeli parantez yerine noktalı virgülle işaretlendiğine dikkat edin.

{% highlight java %}
abstract class XClass {
  private double price;
  protected String speed;
  private String year;
  protected abstract void goFast();
  public abstract void goUpHill();
}
{% endhighlight %}

Bir sınıfın içindeki sadece bir metodun bile soyut olması - bir arayüzün aksine - sınıfın da soyut olarak işaretlenmesi anlamına gelir. Bununla birlikte, soyut olan bir sınıfta, soyut olmayan metotları yerleştirebilirsiniz. Soyut olmayan bir metoda soyut bir sınıfta yer verdiğinizde, bu **abstract** sınıfı **extend** eden tüm somut(**concrete**) alt sınıflara (**concrete** soyut olmayan anlamına gelir.) kalıtsal yöntem uygulamalarını verirsiniz.

* Bir sınıfı hem ``abstract`` hem de ``final`` olarak işaretleyemezsiniz. Neredeyse karşıt anlamlara sahiptirler. Bir **abstract** sınıfı alt sınıflara ayrılabilirken, bir **final** sınıfı ise alt sınıflanamaz. Bir sınıf veya yöntem bildirimi için kullanılan ``abstract`` ve ``final`` değiştiricilerin bu birleşimini görürseniz, kod derlenmez.

{% highlight java %}
XClass x = new Xclass(); // hata oluşur
{% endhighlight %}

En başta söylediğimiz gibi bir **abstract** sınıftan ``new`` anahtar kelimesi kullanarak instance oluşturamazsınız. Yani örneklendirilemez. Yapılırsa hata alırsınız.

## Interface Deklarasyonu

### Java 8'e kadar Interface yapısı

Genel olarak, bir arayüz oluşturduğunuzda, bir sınıfın yapabileceği şeyler için, nasıl yapılacağı hakkında bir şey söylemeden bir sözleşme tanımlayabilirsiniz. Yani bu şu anlama gelir. Bu arayüzü implement eden sınıflar bu sözleşmenin maddelerine bağlı kalmak zorundadırlar. Java 8'e kadar interface'ler ile ilgili %100 **abstract** tanımını yapabiliyorduk. Fakat ilk olarak Java 8'e kadar olan interface yapısından bahsetmek istiyorum.

{% highlight java %}
public abstract interface MyInterface  {
    public int call(int a, int b);
    abstract void bounce();
}
{% endhighlight %}

<!-- An interface is something that indicates that we're dealing with an abstract data type. It's like a promise between the Programmer and the person using the library's code.  -->

> **Interface aslında bir sözleşmedir.**

Interface'ler herhangi bir sınıf tarafından ``implements`` anahtar kelimesi ile uygulanır. Özünde arayüzlerde **abstract**'tır. <u>Fakat interface deklarasyonunda abstract yazmanıza gerek yoktur.</u>

Yazmasanızda kullandığınız **IDE**'ler derleme anında interface'i abstract olarak görür. Ayrıca anstract sınıflarda olduğu gibi interface içinde de **abstract metodlar** tanımlanabilir. <u>Fakat abstract bir sınıf hem soyut hem de soyut olmayan yöntemleri tanımlayabilse de, genel olarak bir interface(arayüz) sadece soyut yöntemlere sahiptir.</u>

### Java 8 itibariyle Interface yapısı

Java 8 itibariyle, sözleşme maddelerinin nasıl yapılacağını da belirleyeblirsiniz. default ve static anahtar kelimeleri Java 8 ile gelen yeni özelliklerdir. Bu sayede interface içinde somut yöntemler oluşturulabilir. Biraz karışık gözükebilir. Örneklerle daha net anlaşılacaktır.

* Bir Interface'in metodları, ``default`` veya ``static`` olarak bildirilmedikçe, dolaylı olarak ``public`` ve ``abstract``dır. Diğer bir deyişle, metod deklarasyonunda aslında ``public`` ve ``abstract`` değiştiricilerini yazmanıza gerek yoktur, derleyici yazılan metodları derleme anında ``public`` ve ``abstract`` olarak algılar. Aşağıdaki görselde ne demek istediğimi daha net anlayacaksınız. İlk kod bloğunu sizin yazdığınız varsayalım. Kodu yazdığınızda derleyici yazdığınız kodu 2. kod bloğundaki gibi algılar. 3. kod bloğunda ise bir interface sınıfını implement eden yani uygulayan bir sınıf ele alınmıştır. Görüleceği üzere bu sınıf interface'in bütün metodlarını uygulamak zorunda kalmıştır.

    <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2018-07-28-Java-class-access/f0024-01.webp"  width="100%" height="100%"  loading="lazy" alt="java interface example">

* Bir interface'de tanımlanan tüm değişkenler(**variables**), ``public``, ``static`` ve ``final`` olmalıdır, diğer bir deyişle, interface instance variables değil, yalnızca **constants(sabit)** deklare edebilir.
* Interface yöntemleri ``final``, ``strictfp`` veya ``native`` olarak işaretlenemez. Zaten ``final`` ve ``abstract`` yan yana düşünülemez.
* Bir interface bir veya daha fazla interface'i genişletebilir(yani **extends** anahtar kelimesi ile miras alabilir).

{% highlight java %}
public interface MyInterface extends Z,M  {
    //methods
}
{% endhighlight %}

{% highlight java %}
public interface M {
}
{% endhighlight %}
{% highlight java %}
public interface Z {
}
{% endhighlight %}


* Fakat bir interface, bir arayüzden başka bir şey extend edemez.
* Bir interface başka bir arayüzü veya sınıfı implement edemez.

Yukarıda da bahsettiğimiz gibi interface deklarasyonunda **abstract** anahtar kelimesi gereksiz gözükebilir. Ama interface zaten dolaylı olarak abstract, yani soyuttur. Siz belirtsenizde belirtmesenizde!!! Aşağıdaki iki interface deklarasyonu da benzerdir.

{% highlight java %}
public abstract interface M { }
public interface M { }
{% endhighlight %}

Interface'in package access(**default access**) yerine herkese açık olmasını istiyorsanız, ``public`` değiştirici işareti gereklidir.

Aşağıdaki beş metod deklarasyonu, kendi arayüzleri içinde beyan edildiyse, yasal ve özdeştir! Aşağıdaki metod deklarasyonları farklı gibi gözükse de hepsi aynıdır.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/2018-07-28-Java-class-access/interface1.webp"  width="100%" height="100%"  loading="lazy" alt="java interface example">

Ekran görüntüsünden de anlaşılacağı üzere **IDE**'miz kodu derlediğinde beyan edilen metodların aynı olduğunu bize söylemekte ve hata vermektedir. Birde dikkat ederseniz, **public** ve **abstract** olan kısımlar gri gösterilmektedir. Burada **IDE** bu anahtar kelimeleri yazmanıza gerek yok uyarısı yapmaktadır.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/2018-07-28-Java-class-access/interface2.webp"  width="100%" height="100%"  loading="lazy" alt="java interface example">



## Referanslar:  

1. [Declaring Classes](https://docs.oracle.com/javase/tutorial/java/javaOO/classdecl.html)
2. [OCA Java SE 8 Programmer I Exam Guide (Exams 1Z0-808)](https://www.amazon.com/Java-Programmer-Guide-Exams-1Z0-808/dp/1260011399)
