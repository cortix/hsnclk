---
title: "Java'da Kalıtım 2- Extends"
comments: false
excerpt: "Java'da extends anahtar kelimesinin ne olduğunu, kullanımını, superclass ve subclass kavramlarını ve sınıf hiyerarşisinde UML diyagramı kullanımını göreceğiz."
header:
  teaser: "/assets/images/svg-book10.svg"
  og_image: /assets/images/svg-book10.svg
  overlay_image: /assets/images/svg-book10.svg
  overlay_color: "#000"
  overlay_filter: "0.3"
  caption: "background by [SVGBackgrounds.com](https://www.svgbackgrounds.com/)"
  #cta_label: "More Info"
  #cta_url: "https://unsplash.com"
categories:
  - java-kalitim-polimorfizm
tags:
  - Java inheritance
  - Java Uml diagram
  - Java extends
  - Java'da miras alma
  - Java'da kalıtımı sağlamak için asgari hedefler
  - Java gizli değişken
  - Java gölge değişken
last_modified_at: 2022-12-29T15:12:19-04:00
toc: true
toc_label: "SAYFA İÇERİĞİ"
toc_sticky: true
---

**ÖNEMLİ :** Kendim için aldığım notlar. Umarım size de bir faydası olur. Kullanılan her bir makale referans olarak eklenmiştir.
{: .notice}

<div class="notice--success" markdown="1">
<h4 class="no_toc"><i class="fas fa-lightbulb"></i> Java’da Kalıtım(inheritance) ve Java’da Polimorfizm Serisi</h4>
---

1. Java’da Kalıtım 1 - [Kalıtımı Neden Kullanırız? Kalıtımı Sağlamak İçin Asgari Şartlar Nelerdir?](/java-kalitim-polimorfizm/Java-inheritance1/)
2. **Java’da Kalıtım 2- Extends**
3. Java’da Kalıtım 3 - [Referans ve Nesne Tipleri](/java-kalitim-polimorfizm/Java-inheritance3/)
4. Java’da Kalıtım 3.1 - [Static ve Dinamik Tür](/java-kalitim-polimorfizm/Java-inheritance3_1/)
5. Java’da Kalıtım 4 - [Görünürlük/Erişim Değiştiricileri](/java-kalitim-polimorfizm/Java-inheritance4/)
6. Java’da Kalıtım 5 - [Java’da Nesne Oluşturma](/java-kalitim-polimorfizm/Java-inheritance5/)
7. Java’da Kalıtım 6 - [Sınıf İnşası için Derleyici Kuralları](/java-kalitim-polimorfizm/Java-inheritance6/)
8. Java’da Kalıtım 7 - [Sınıf Hiyerarşisinde Değişken İlklendirme](/java-kalitim-polimorfizm/Java-inheritance7/)
9. Java’da Kalıtım 8 - [Alıştırmalar](/java-kalitim-polimorfizm/Java-inheritance8/)
10. Java’da Kalıtım 9 - [Overriding(Ezici) Metotlar](/java-kalitim-polimorfizm/Java-inheritance9/)
</div>

## Genel Bakış

Bir önceki bölümden de hatırlayacağımız üzere yazdığımız kodda tutarlılığı sağlamak ve veri yapısını tek bir sınıfta toplamak için belirlediğimiz temel hedefler şunlardı;

<div class="notice--success" markdown="1">
<h4 class="no_toc"><i class="fas fa-lightbulb"></i> Kodda tutarlılığı sağlamak ve veri yapısını tek bir sınıfta toplamak için hedefler:</h4>
---
1. [x] **Bütün ortak davranışları bir sınıfta tutmak,**
2. [x] **Farklı davranışa sahip olanları ise farklı sınıflara ayırmak**
3. [ ] Tüm bu nesneleri tek bir veri yapısında tutmak.
</div>

İlk iki hedefi bu bölümde ele almaya çalışacağız. Hatırlayacağınız üzere, bunu java'da kalıtım(inheritance) kullanarak çözmeye çalışacağımızı belirtmiştik.

### Ortak Kod

``` java
public class Person {
  private String name;
  ....
}
```

### Farklı Sınıflara Ayrılan Kod

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

Aslında 1 ve 2. seçenekleri tamamlamamız için sihirli bir anahtar kelimesine ihtiyacımız bulunmaktadır. Çünkü ortak değişkenler hâlen tek bir sınıfta ikamet ediyor. Tahmin edeceğiniz üzere bu kelime **extends** anahtar kelimesidir.

### Ortak Kod

``` java
public class Person {
  private String name;
  ....
}
```

### Farklı Sınıflara Ayrılan Kod

``` java
public class Student extends Person {
  ....
}

public class Faculty extends Person {
  ....
}
```


Görüleceği üzere ortak değişkenimiz olan **name**, sadece üst sınıfımız olan **Person** sınıfında yer almaktadır. Ama private erişim değiştiricisine sahip!!! Bu arada **extend** anahtar kelimesinin anlamı genişletmektir. Yalnız programlama jargonundaki tam karşılı ise **miras almaktır**. Yani **Student** ve **Faculty** sınıflarını **Person** sınıfına **extends** ettiğimizde aslında **Person** sınıfını miras almış oluruz.

<div class="notice--success" markdown="1">
<h4 class="no_toc"><i class="fas fa-lightbulb"></i> Peki Parent Sınıftan ne Miras Alınır?</h4>
---

Tabii ki bütün özelliklerini değil.

* **public** örnek değişkenleri(instance variables) miras alırız,
* **public** metotları miras alırız,
* **private** örnek değişkenleri de teknik olarak miras alınır ama bu değişkenlere erişim için public metotlara ihtiyacımız olacaktır. Aşağıda bunu izah etmeye çalışacağım.

</div>

**Not:** Dikkat edecek olursanız, üst sınıfımız olan **Person** sınıfında bulunan **name** ismindeki üye değişkenimiz **private** erişim değiştiricisine sahip olduğu için alt sınıflar tarafından miras alınsa da bu değişkene erişim şu an için mümkün gözükmemektedir.
{: .notice--warning}

Kaldığımız yerden devam edecek olursak, **Person** sınıfımız bizim **base/super/parent** sınıfımız olurken, **Student** ve **Faculty** sınıfları ise ana sınıftan türeyen **derived/subclass/child** olur.  

### Gizli değişken (Hidden Variable) | Gölge değişken (shadow variable / variable shadowing)

{% highlight java mark_lines="7" %}
public class Person {
  private String name;
  ....
}

public class Student extends Person {
  private String name; // ???????????
  ...
}
{% endhighlight %}



Bu örnekte ise hem parent hem de child sınıfta, **name** isminde benzer üye değişkenlerimiz bulunmaktadır. Görüleceği üzere, her ikisi de **private** erişim değiştiricisine sahip. Sizce **Student** sınıfında **name** adında private bir üye değişkenine gerçekten de sahip olmanız gerekiyor mu?

Aslında hayır... Hatta bu kötü bir pratiktir. Çünkü, **Student** sınıfı içindeki private **name** değişkeni, **gizli değişken** veya **gölge değişken**(variable shadowing) olarak ifade ettiğimiz şekilde davranır. Yani hangi sınıfın **name** üye değişkeninden bahsettiğinizi belirlemek zordur. **Student** mı yoksa **Person** mı???? Bu yüzden Student sınıfı içinde tekrardan aynı isimli bir değişkeni tanımlamamız anlamsızdır, çünkü bu değişkeni otomatik olarak **Person** sınıfından zaten miras alırsınız.

<div class="notice--success" markdown="1">
<h4 class="no_toc"><i class="fas fa-lightbulb"></i> Private örnek değişkenine erişim</h4>
---

* Peki **private** olan bu örnek değişkenlerini miras alabiliyorsak bunlara nasıl erişeceğiz?

**private** örnek değişkenlerine doğrudan erişim sağlayamasak da **public** metotlar üzerinden bu değişkenlere erişim sağlayabiliriz.

**getter** ve **setter** metodlar buna güzel bir örnektir. Hatta bu erişim yöntemi şiddetle tavsiye edilir.
</div>

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

## Java Miras Hiyerarşisinin UML Diagramı

Artık mirası nasıl kullanabileceğimiz hakkında bir fikrimiz olduğuna göre, bir miras hiyerarşisini nasıl tasarlayabileceğimiz hakkında konuşmaya başlayalım. Şimdi bir tasarım ekibi ile bir beyaz tahta üzerinde çalışıyorsanız, tüm sınıfı yazmak istemeyeceksiniz. Bir sınıfı çok küçük bir şekilde temsil etmek isteyeceksiniz.

Kalıtım(inheritance) hiyerarşisi şu şekildedir.

<br/>{% picture 2020-06-20-Java-inheritance2/uml1.png --alt java class hierarchy or inheritance tree uml diagram (java sınıf hiyerarşisi veya kalıtım ağacı uml diyagramı) --img width="100%" height="100%" %}<br/>

* Grade Point Average (Not Ortalaması)

* Salary(Maaş)

* Gender(Cinsiyet)

Diyelim ki yukarıdaki 3 değişkeni bu sınıflara eklemek istiyorsunuz. Sizce hangisi hangi sınıfa daha uygun olur? Örneğin **Student** sınıfına ait olup ama **Faculty** sınıfında olmayan değişken ne olabilir? Aynı soruyu **Faculty** sınıfına ait olup ama **Student** sınıfında olmayan şeklinde tersten de sorabiliriz.

Cinsiyet değişkeni genel bir değişken olduğundan **Person** sınıfında olması daha doğru olacaktır. Ama **maaş** ve **not ortalaması** değişkenleri biraz daha spesifiktir. Yani öğrencinin maaş alamayacağını ve öğretim görevlilerinin ise not ortalamasına sahip olamayacağını biliyoruz. Bu yüzden bu değişkenleri bu sınıflara özel olarak tanımlayabiliriz.

<br/>{% picture 2020-06-20-Java-inheritance2/uml2.png --alt java class hierarchy or inheritance tree uml diagram (java sınıf hiyerarşisi veya kalıtım ağacı uml diyagramı) --img width="100%" height="100%" %}<br/>

Bölümün başında belirttiğimiz 3 şarttan ilk ikisini gerçekleştirdik. Sonuncuyu ise bir sonraki bölümde ele alacağız.

## Referanslar
* [Inheritance](https://docs.oracle.com/javase/tutorial/java/IandI/subclasses.html)
* [Object Oriented Programming in Java](https://www.coursera.org/learn/object-oriented-java?specialization=java-object-oriented)
* [Variable shadowing](https://en.wikipedia.org/wiki/Variable_shadowing)
