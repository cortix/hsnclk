---
title: "Java'da Polimorfizm 5 - Soyut(Abstract) Sınıflar ve Arayüzler(Interfaces)"
comments: true
excerpt: "Bu bölümde abstract sınıfları ve arayüzleri ele almak istiyorum. Özellikle neler olduğu, hangi durumlarda hangisinin tercih edilmesi gerektiği hakkında konuşacağız. Bunun yanı sıra uygulama kalıtımı(**inheritance of implementation**) ve arayüzün kalıtımı(**inheritance of interface**) kavramlarının neler olduğunu karşılaştıracağız."
header:
  teaser: "assets/images/equality.png"
  og_image: /assets/images/page-header-og-image.png
  overlay_image: /assets/images/unsplash-image-58.jpeg
  overlay_filter: 0.5 #rgba(255, 0, 0, 0.5)
  caption: "Photo by [zero take](https://unsplash.com/photos/jSB9PWaxhXo) on Unsplash"
  #cta_label: "More Info"
  #cta_url: "https://unsplash.com"
categories:
  - java-kalitim-polimorfizm
tags:
  - polimorfizm
  - inheritance of implementation
  - inheritance of interface
  - abstract class
  - interface
last_modified_at: 2020-02-19T15:12:19-04:00
toc: true
toc_label: "SAYFA İÇERİĞİ"
---



**ÖNEMLİ :** Kendim için aldığım notlar. Umarım size de bir faydası olur. Kullanılan her bir makale referans olarak eklenmiştir.
{: .notice}

Aslında **abstract sınıf** ve **interface** özünde ikisi de soyut kavramlardır. Tabii aralarında hem kullanım hem de yapısal bazı farklılıklar bulunmaktadır. Bu derste de bu farklılıkları ele alacağız. Ek olarak şu [makaleme](/java/Java-class-access/) de bakmanızı öneririm. Abstract sınıf ve interface konularına orada da değinmiştim. 

Normalde bir üst sınıfım ve onu miras alan bir alt sınıfım oluyordu. Burada yapmak istediğim şey aslında yine kalıtımı bazı kısıtlamalar getirerek uygulamak olacak. Hemen bir örnek vermek istiyorum.


<figure style="width: 400px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-09-18-Java-polimorfizm5/uml1.png" alt="inheritance">
  <figcaption></figcaption>
</figure>


Normalde bir sınıfı miras aldığımızda o sınıfın bütün davranışlarını da miras almış olurduk. Yani metotları ve özelliklerini(fields) kastediyorum. Alt sınıflar da eğer farklı bir şey yapmak isterlerse metotları kendilerine göre geçersiz kılabiliyorlardı.

Yalnız yukarıdaki örnekte ben Person sınıfıyla bir obje oluşturabiliyorum. Yani demek istediğim ``Person person = new Person();`` şeklinde bir obje yarattığımda bunun bir öğrenci(Student) mi yoksa öğretim üyesi(Faculty) mi olduğunu bilmemiz gerekecek. Peki obje oluştururken şöyle bir zorunluluk getirsek sizce nasıl olur?

* Referans tipi olarak yine üst sınıfı kullanabileyim ama gerçek nesne için beni alt sınıflarda obje oluşturmaya zorlasın....
* Benzer davranışları üst sınıfta tutup, farklı davranışları alt sınıflara özel kılacak soyut metotlar oluşturmak...

Bunların hepsini tek bir sınıfta yapmak için abstract sınıf kullanırız. Bir sınıfı **abstract** yapmak için, ilgili sınıfın içindeki tek bir metodunu **abstract** anahtar kelimesi ile işaretlememiz yeterlidir. İlgili sınıf sırf tek bir metot **abstract** anahtar kelimesine sahip olduğu için otomatikman **abstract**(yani soyut) olur. Ve bu sınıfın abstract metotlarını onu miras alan sınıflar override etmek zorunda kalır. Aslında bu, alt sınıfları belirli yöntemlere sahip olmaya zorlamanın güzel bir yoludur.

```java
public abstract class Person {
  public abstract void doWork();
}
```

**ÖNEMLİ NOT:** Abstract yapılan sınıfın objesi oluşturulamaz.. Ancak ve ancak onu miras alan bir concrete(somut) sınıf ile bir nesne(obje) oluşturabiliriz.

**public abstract void doWork()** metodu, onu ezen(override) alt sınıflar tarafından kullanılabilir. Yani ilgili metodu işlevsel hale getirecek, yine nesnesini oluşturabildiğimiz somut(concrete) sınıflar olacaktır.

Bir abstract sınıf size kalıtımın iki şeyini sunar: Soyut bir sınıfla iki şey elde edersiniz. Hem uygulamayı(implementation) hem de arayüzü(interface) miras alırsınız.

* bir uygulama(**implementation**) ile, esasen herhangi bir `Person` sınıfının sahip olması gereken ortak davranışı tanımlayan örnek değişkenleri veya yöntemleri elde edersiniz,
* ikinci olarak sınıfın sahip olduğu arayüzleri(**interface**) miras alırsınız.. Özetle, alt sınıflarımda hangi yöntemlerin geçersiz kılınması gerektiğini söylüyorsunuz. Yani soyut metot imzaları!!!


Bazı durumlarda ikisini birden miras almak istemezsiniz.. Sadece arayüzü(interface) miras almak isteyebilirsiniz. Bu gibi durumlarda arayüzler(interface) işinizi görecektir. Yalnız şunu belirtmekte yarar var.. Arayüzler(yani interface) bir sınıf değildir.


Örneğimize geri dönecek olursak, abstract(soyut) bir sınıfın az önce bahsettiğimiz varsayıma uygun olduğuna gerçekten inanıyorum. Ancak, `Person` sınıfında ortak bir kodunuz olmadığını varsayalım. O zaman interface kullanmak mantıklı bir tercih olacaktır. Bir interface bize alt sınıfları bir yönteme sahip olmaya zorlama, ``Person person = new Person();`` olduğu gibi gerçek `Person` nesnelerine sahip olmayı durdurma yeteneği verirken, Person sınıfı gibi üst sınıfların(parent class) referanslarına sahip olmaya devam etme imkanı verecektir.

Arayüzler(interface) sadece gerekli metodları tanımlar ve sınıflar aslında birden çok arayüzden miras alabilir(bu miras alma işlemini **implements** anahtar kelimesi ile gerçekleştiririz. Yani **extends** değil!!!).

Hepsi tek bir sınıf içinde olmak üzere, bir sınıf sadece bir tane **abstract sınıfı** miras alabilirken, birden çok **interface**'i miras alabilir.

```java
package org.hasancelik.core.test;

import java.util.Comparator;

public abstract class Car implements Comparable<Car>, Comparator<Car> {
    private String name;
    private String engine;
    public abstract void consumeFuel();
}
```
```java
package org.hasancelik.core.test;

public class BMW extends Car{
    @Override
    public void consumeFuel() {

    }

    @Override
    public int compareTo(Car o) {
        return 0;
    }

    @Override
    public int compare(Car o1, Car o2) {
        return 0;
    }
}
```
```java
package org.hasancelik.core.test;

public class Mercedes extends Car{
    @Override
    public void consumeFuel() {

    }

    @Override
    public int compareTo(Car o) {
        return 0;
    }


    @Override
    public int compare(Car o1, Car o2) {
        return 0;
    }
}
```

Yukarıdaki kod bloklarından da anlaşılacağı üzere bir sınıf sadece tek bir abstract sınıfı miras alabilirken, birden fazla interface'i miras alabilir. `BMW` ve `Mercedes` sınıfları sırf **abstract** `Car` sınıfını miras aldıkları için `consumeFuel()` metodunu **override** etmek zorunda bırakılmışlardır. Bununla beraber, `Car` sınıfı iki farklı arayüzü **implement** ettiğini göreceksiniz. Haliyle bu arayüzler sahip oldukları 2 farklı **abstract** metodu, kendilerini **implement** eden alt sınıflara uygulatmak zorunda bırakacaklardır. Bu metotlardan `compareTo` metodu `Comparable` arayüzünden, `compare` metodu ise `Comparator` arayüzünden **override** edilen metotlardır.


Abstract sınıfla interface her ne kadar ikisi de soyut kavramını temsil etseler de miras verdikleri sınıflar arasındaki organik bağ tam olarak aynı sayılmaz. Farklı bir örnekten yola çıkacak olursak, `Animal` isminde bir sınıf ile `Dog` isminde sınıfın arasındaki organik bağ, `Dog` sınıfının implement ettiği `public interface Comparable<T>` arayüzü arasındaki organik bağdan farklıdır. Yani bir köpeğin sadece bir tane üst familyası olabilir. Ama `Comparable` arayüzünü miras aldığı gibi birden fazla farklı davranışa sahip olabilir. Arayüzleri, ilgili sınıfa kalıtım hiyerarşisi dışında özellikler vermek için kullanıyoruz da diyebiliriz. Tabii ki bu durum yine sizin hayal gücünüze kalmış bir şey!!!

### Özet

Yani bir sınıf hiyerarşisini tasarlamaya çalışırken bunu bir arayüz mü yoksa soyut bir sınıf mı yapmalıyım diye düşünebilirsiniz? Ve genel kural şudur, eğer sadece metotlara ihtiyaç duyuyorsanız, bu bir arayüz(interface) olmalıdır. Ancak metotlara olduğu kadar ve ortak bir davranışlara da ihtiyaç duyuyorsanız, işte burada soyut bir sınıf(yani abstract sınıflar)devreye girer.

### Referanslar

* [https://docs.oracle.com/javase/8/docs/api/java/util/Comparator.html](https://docs.oracle.com/javase/8/docs/api/java/util/Comparator.html)
* [https://docs.oracle.com/javase/8/docs/api/java/lang/Comparable.html](https://docs.oracle.com/javase/8/docs/api/java/lang/Comparable.html)
* [https://docs.oracle.com/javase/7/docs/api/java/lang/Object.html#toString()](https://docs.oracle.com/javase/7/docs/api/java/lang/Object.html#toString())
* [https://docs.oracle.com/javase/tutorial/java/IandI/abstract.html](https://docs.oracle.com/javase/tutorial/java/IandI/abstract.html)
* [https://www.coursera.org/learn/object-oriented-java?specialization=java-object-oriented](https://www.coursera.org/learn/object-oriented-java?specialization=java-object-oriented)
