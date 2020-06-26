---
title: "Java'da Kalıtım 2- Extends"
comments: true
excerpt: "Bu derste Java'daki kalıtım ve polimorfizm kavramlarını ele almaya devam edeceğiz. Java'da extends anahtar kelimesinin ne anlama geldiğini ve kullanımını, superclass ve subclass kavramlarını, sınıf hiyerarşisinde UML diyagramı kullanımını göreceğiz. Bununla birlikte bir önceki derste belirlediğimiz en temel kalıtım kurallarından ilk ikisini ele alacağız."
header:
  teaser: "assets/images/equality.png"
  og_image: /assets/images/page-header-og-image.png
  overlay_image: /assets/images/unsplash-image-44.jpg
  overlay_filter: 0.5 #rgba(255, 0, 0, 0.5)
  caption: "Photo by [Mitchell Luo](https://unsplash.com/photos/hhdxtwlaAsM) on Unsplash"
  #cta_label: "More Info"
  #cta_url: "https://unsplash.com"
categories:
  - java-kalitim-polimorfizm
tags:
  - Uml diagram
  - extends
  - miras alma
  - kalıtım kuralları
last_modified_at: 2020-06-06T15:12:19-04:00
toc: true
toc_label: "SAYFA İÇERİĞİ"
---

**ÖNEMLİ :** Kendim için aldığım notlar. Umarım size de bir faydası olur. Kullanılan her bir makale referans olarak eklenmiştir.
{: .notice}

Bir önceki dersten de hatırlayacağımız üzere belirlediğimiz temel hedefler şunlardı;

1. Bütün ortak davranışları bir sınıfta tutmak,
2. Farklı davranışa sahip olanları ise farklı sınıflara ayırmak
3. Tüm bu nesneleri tek bir veri yapısında tutmak.

İlk ikisini bu derste ele almaya çalışacağız.

**Ortak Kod**

``` java
public class Person {
  private String name;
  ....
}
```

**Farklı Sınıflara Ayrılan Kod**

``` java
public class Student {
  private String name;
  ....
}

public class Faculty {
  private String name;
  ....
}
```

Aslında 1 ve 2. seçenekleri tamamlamamız için gereken sihirli bir kelimeye ihtiyacımız bulunmaktadır. Çünkü halen ortak değişkenler tek bir sınıfta yer almamaktadır. Bu kelime ``extends`` anahtar kelimesidir.

**Ortak Kod**

``` java
public class Person {
  private String name;
  ....
}
```

**Farklı Sınıflara Ayrılan Kod**

``` java
public class Student extends Person {
  ....
}

public class Faculty extends Person {
  ....
}
```


Görüleceği üzere ortak değişkenimiz olan name, sadece üst sınıfımız olan Person sınıfında yer almaktadır. Extend kelimesininin kelime anlamı genişletmektir. Yalnız programlama jargonundaki tam karşılı ise miras almaktır. Yani Student ve Faculty sınıflarını Person sınıfına extends ettiğimizde aslında Person sınıfını miras almış oluruz.

Böylelikle Person sınıfımız bizim base/super/parent sınıfımız olurken, Student ve Faculty sınıfları ise ana sınıftan türeyen derived/subclass/child olur.  


Peki Parent Sınıftan ne Miras Alınır?

Tabii ki bütün özellikerini değil.

* public örnek değişkenleri(instance variables
* public metotları,
* private örnek değişkenleri(instance variables)???? Aslında bir bakıma private instance variable'ları da bir bakıma extends ederiz. Ama nasıl olduğuyla ilgili detayı başka bir derste veririz.



``` java
public class Person {
  private String name;
  ....
}

public class Student extends Person {
  private String name; // ???????????
  ...
}

```



Gerçekten hala **name** adında bir Student değişkenine sahip olmanız gerekiyor mu? Aslında hayır... Buna gizli değişken veya gölge değişken de denir ve bu hangi değişkeni, hangi name değişkeninden bahsettiğinizi fark etmek zordur. Student mı yoksa Person mı???? Yani aslında buna sahip olmazsınız, çünkü onu otomatik olarak Person'dan miras aldınız.Yalnız ufak bir sorun var. public olmayan değişkenlere erişim sadece public metodlar üzerinden sağlanabilir. getter ve setter metodlar buna güzel bir örnektir.


``` java
public class Person {
  private String name;
  public String getName(){
    return name;
  }
  ....
}

public class Student extends Person {
  ...
}

```

### UML Diagram

Artık mirası nasıl kullanabileceğimiz hakkında bir fikrimiz olduğuna göre, bir miras hiyerarşisini nasıl tasarlayabileceğimiz hakkında konuşmaya başlayalım. Şimdi bir tasarım ekibi ile bir beyaz tahta üzerinde çalışıyorsanız, tüm sınıfı yazmak istemeyeceksiniz. Bir sınıfı çok küçük bir şekilde temsil etmek isteyeceksiniz.

Kalıtım hiyerarşisi şu şekildedir.



<figure style="width: 600px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-06-20-Java-inheritance2/uml1.png" alt="uml diagram">
  <figcaption></figcaption>
</figure>

* Grade Point Average (Not Ortalaması)

* Salary(Maaş)

* Gender(Cinsiyet)

Diyelim ki yukarıdaki 2 değişkeni bu sınıflara eklemek istiyorsunuz. Sizce hangisi hangi sınıfa daha uygun olur? Örneğin Student sınıfına ait olup ama Faculty sınıfında olmayan değişken ne olabilir? Aynı soruyu Faculty sınıfına ait olup ama Student sınıfında olmayan şeklinde tersten de sorabiliriz.

Cinsiyet değişkeni genel bir değişken olduğundan Person sınıfında olması daha doğru olacaktır. Ama maaş ve not ortalaması değişkenleri biraz daha spesifiktir. Yani öğrencinin maaş alamayacağını ve öğretim görevlilerinin ise not ortalamasına sahip olamayacağını biliyoruz. Bu yüzden bu değişkenleri bu sınıflara özel olarak tanımlayabiliriz.

<figure style="width: 600px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-06-20-Java-inheritance2/uml2.png" alt="uml diagram">
  <figcaption></figcaption>
</figure>

Dersin başında belirttiğimiz 3 şarttan ilk ikisini gerçekleştirdik. Sonuncuyu ise bir sonraki derste ele alacağız.
