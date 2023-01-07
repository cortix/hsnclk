---
title: "Java'da Kalıtım 1 - Kalıtımı Neden Kullanırız? Kalıtımı Sağlamak İçin Asgari Şartlar Nelerdir?"
comments: false
excerpt: "Bu bölümde Java'daki kalıtım(inheritance) ve polimorfizm kavramlarını ele alacak ve kalıtımı sağlamak için asgari hedeflerin neler olduğunu işleyeceğiz."
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
  - Java spagetti kodlama
  - Java'da kalıtımı sağlamak için asgari hedefler
last_modified_at: 2023-01-06T15:12:19-04:00
toc: true
toc_label: "SAYFA İÇERİĞİ"
toc_sticky: true
---

**ÖNEMLİ :** Kendim için aldığım notlar. Umarım size de bir faydası olur. Kullanılan her bir makale referans olarak eklenmiştir.
{: .notice}

<div class="notice--success" markdown="1">
<h4 class="no_toc"><i class="fas fa-lightbulb"></i> Java’da Kalıtım(inheritance) ve Java’da Polimorfizm Serisi</h4>
---
1. **Java’da Kalıtım 1 - Kalıtımı Neden Kullanırız? Kalıtımı Sağlamak İçin Asgari Şartlar Nelerdir?**
2. Java’da Kalıtım 2- [Extends](/java-kalitim-polimorfizm/Java-inheritance2/)
3. Java’da Kalıtım 3 - [Referans ve Nesne Tipleri](/java-kalitim-polimorfizm/Java-inheritance3/)
4. Java’da Kalıtım 3.1 - [Static ve Dinamik Tür](/java-kalitim-polimorfizm/Java-inheritance3_1/)
5. Java’da Kalıtım 4 - [Görünürlük/Erişim Değiştiricileri](/java-kalitim-polimorfizm/Java-inheritance4/)
6. Java’da Kalıtım 5 - [Java’da Nesne Oluşturma](/java-kalitim-polimorfizm/Java-inheritance5/)
7. Java’da Kalıtım 6 - [Sınıf İnşası için Derleyici Kuralları](/java-kalitim-polimorfizm/Java-inheritance6/)
8. Java’da Kalıtım 7 - [Sınıf Hiyerarşisinde Değişken İlklendirme](/java-kalitim-polimorfizm/Java-inheritance7/)
9. Java’da Kalıtım 8 - [Alıştırmalar](/java-kalitim-polimorfizm/Java-inheritance8/)
10. Java’da Kalıtım 9 - [Overriding(Ezici) Metotlar](/java-kalitim-polimorfizm/Java-inheritance9/)
11. Java’da Kalıtım 10 - [Overloading(Aşırı Yükleme) Metotlar](/java-kalitim-polimorfizm/Java-inheritance10/)
</div>
## Kalıtımı Neden Kullanırız?

**Kalıtım**(inheritance) ve **polimorfizm**, nesneye dayalı bir programlama dilinde inanılmaz derecede güçlü özelliklerdir. **Loop** ve **conditional**'ların aksine, kodunuzun çalışması için genellikle kalıtım ve polimorfizm kullanmanız gerekmez. Yani önemini, sahip olduğunuz kod devasa bir hale gelmeden anlayamazsınız. Çünkü kodunuz büyüdükçe yöntemleri ve daha sonra sınıfları kullanmaya başlarsınız. Aynı şekilde, projenizin karmaşıklığı arttıkça ve büyük yazılım tasarım projeleri üzerinde çalışmaya başladığınızda, projenin karmaşıklığını kaldırabilmek için **kalıtım** ve **polimorfizm** kullanmaya başlarsınız.

## Java'da Kalıtım Kullanmadan Oluşturulan Örnek Kodlar

Bir örnekle konuya girmek istiyorum. Diyelim ki elimizde bir **Person**(kişi) sınıfı var. Ancak yazılım tasarım ekibiniz size, artık bir sınıfın yetmediğini, öğrenci ve öğretim üyeleri olmak üzere **2 farklı sınıfa** daha ihtiyacın olduğunu söylüyor.

``` java
public class Person {
  private String name;
  ....
}
```

<br/>Varolan **Person** sınıfının üstüne bir de **Student** ve **Faculty** isminde iki sınıf daha oluşturmamız isteniyor. Peki ne yapmamız gerekiyor? Bu sorunu ele almak için ilk olarak **2 tane kötü** çözüm yöntemi sunmak istiyorum. İlk olarak bu ihtimalleri görmeniz kalıtımın önemini anlamak açısından daha yerinde olacaktır.

### Birinci Önerilmeyen Yöntem

İlk çözüm, olan biteni sadece **Person** sınıfında tutmaya çalışmak olsun. Yani demek istediğim aşağıdaki gibi bir kod;

<div class="notice--info" markdown="1">
<h4 class="no_toc"><i class="fas fa-lightbulb"></i> Birinci kötü çözüm:</h4>
---

``` java
public class Person {
  private String name;
  private boolean student;

  public Person(boolean s){
    this.student = s;
  }
  ....
}
```

``` java
if (student){
  //code for students
}else{
  //code for faculty members
}
  ....
```
</div>

Yukarıda görüldüğü gibi karşılaştığım sorunları yine **Person** sınıfı içinde bir ``if-else`` conditional'ı kullanarak halledebilirim. Her seferinde bu kişi bir öğrenci mi? yoksa bir fakülte üyesi mi? şeklinde bir soru sormam yeterli olacaktır. Buradaki örnek basit gibi gözükse de, üye değişkenleri fazlalaştığında, bu durum içinden çıkılamayacak bir hâl alacaktır.

Fakat farklı öğrencilerin farklı davranışları da olabilir. Aynı durum fakülte üyeleri için de geçerlidir. Okulda mezun öğrenciler olduğu gibi, henüz mezun olmamış öğrenciler de olabilir. Her seferinde `if-else` kullanarak bu sorunu çözemeyiz.

<div class="notice--info" markdown="1">
<h4 class="no_toc"><i class="fas fa-lightbulb"></i> Birinci kötü çözümün devamı:</h4>
---

``` java
public class Person {
  private String name;
  private boolean student;
  private boolean graduate;
  private boolean undergraduate;

  public Person(boolean s){
    this.student = s;
  }
  ....
}
```

``` java
if (student){
  if(graduate){
    //...
  }else if(undergraduate){
    //....
  }
  //code for students
}else{
  //code for faculty members
}
  ....
```
</div>

Bu tarz kodlamaya **spagetti kodlama** da denir.

### İkinci Önerilmeyen Yöntem

Peki diğer kötü çözüm yöntemimiz nedir? Bu da, **Person** sınıfının **Student** ve **Faculty** isminde 2 farklı kopyasını oluşturmak olabilir. Yani **Person** sınıfının içeriğini doğrudan kopyalayıp bu yeni sınıflara yapıştırmaktan bahsediyorum;

<div class="notice--info" markdown="1">
<h4 class="no_toc"><i class="fas fa-lightbulb"></i> İkinci kötü çözüm:</h4>
---

``` java
public class Student {
  private String name;
  ....
}
```

``` java
public class Faculty {
  private String name;
  ....
}
```

</div>

Peki buradan ne gibi problemler çıkardı? Cevabınız, tutarlılıkla(**consistency**) veya veri yapısının dağınıklığı ile ilgiliyse, doğru yoldasınız demektir. Şimdi her ikisini de konuşalım istiyorum.

Buradaki problem aslında tam olarak şudur. Örneğin **Person** sınıfının kodunda değişiklik yapmak istediğinizde, bu değişiklikleri **Student** ve **Faculty** sınıflarına da uygulamamız gerekecektir. <u>Peki bunu her seferinde kopyala-yapıştır şeklinde mi yapacağız?</u> Şayet böyle yaparsak ortak kodu **tutarlı** tutmak gerçekten zor olacaktır.

Veyahut **Person[]** tipinde bir dizi oluşturmak istediğimizi varsayalım. Bu dizinin öğrenci mi yoksa fakülte üyesi mi olduğuna nasıl karar vereceğiz? Veyahut bunu, sadece öğrenciler ve sadece fakülte üyeleri şeklinde iki farklı diziye ayırırsak ne olur? **Student[]** ve **Faculty[]** .... O zaman da kişileri temsil eden diziyi bir daha asla kullanamayız! Sonuç olarak öğrenciler ve fakülte üyeleri için iki farklı dizi tutmam gerekecek.

Tüm insanlar için tek bir veri yapısı tutmanın bir yolu olmadığını da görüyorum. <u>Peki bu neden önemlidir?</u> Yani sadece **Person** şeklinde tutmanın ne önemi olacak? Belkide amacımız, okuldaki insanları, öğrenci veya öğretim görevlisi ayrımı yapmadan, tek bir çatı altında kayıt altına almak olabilirdi. Yalnız bu durumda bunu yapmak gerçekten zor olurdu. Öğrencileri potansiyel olarak sıralayabilirim, öğretim görevlilerini de sıralayabilirim, ama bunları nasıl birleştirebilirim? Esasen, bu iki örneğe bakarak ne yapmamız gerektiğini az çok anladık.

## Kodda tutarlılığı sağlamak ve veri yapısını tek bir sınıfta toplamak

Buradaki **hedefler**,

1. Bütün ortak davranışları bir sınıfta tutmak,
2. Farklı davranışa sahip olanları ise farklı sınıflara ayırmak
3. Tüm bu nesneleri tek bir veri yapısında tutmak.

İşin güzel yanı bütün bunları kalıtım(inheritance) ile yapabiliriz. Sonraki [yazımda](/java-kalitim-polimorfizm/Java-inheritance2/) bu hedeflerden ilk ikisini ele aldım, dilerseniz bakabilirsiniz.


## Referanslar
* [Inheritance](https://docs.oracle.com/javase/tutorial/java/IandI/subclasses.html)
* [Object Oriented Programming in Java](https://www.coursera.org/learn/object-oriented-java?specialization=java-object-oriented)
