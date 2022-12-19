---
title: "Java'da Kalıtım 1 - Kalıtımı Neden Kullanırız? Kalıtımı Sağlamak İçin Asgari Şartlar Nelerdir?"
comments: false
excerpt: "Bu derste Java'daki kalıtım(inheritance) ve polimorfizm kavramlarını genel olarak ele alacak ve bununla birlikte kalıtımı sağlamak için asgari hedeflerin neler olduğunu işleyeceğiz."
header:
  teaser: "assets/images/equality.png"
  og_image: /assets/images/equality.png
  overlay_image: /assets/images/unsplash-image-43.jpeg
  overlay_filter: 0.5 #rgba(255, 0, 0, 0.5)
  caption: "Photo by [Possessed Photography](https://unsplash.com/photos/sVOF3ONKvfU) on Unsplash"
  #cta_label: "More Info"
  #cta_url: "https://unsplash.com"
categories:
  - java-kalitim-polimorfizm
tags:
  - Java inheritance
  - Java spagetti kodlama
  - Java'da kalıtımı sağlamak için asgari hedefler
last_modified_at: 2020-06-06T15:12:19-04:00
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
5. Java’da Kalıtım 4 - [Görünürlük Değiştiricileri](/java-kalitim-polimorfizm/Java-inheritance4/)
6. Java’da Kalıtım 5 - [Java’da Nesne Oluşturma](/java-kalitim-polimorfizm/Java-inheritance5/)
7. Java’da Kalıtım 6 - [Sınıf İnşası için Derleyici Kuralları](/java-kalitim-polimorfizm/Java-inheritance6/)
8. Java’da Kalıtım 7 - [Sınıf Hiyerarşisinde Değişken İlklendirme](/java-kalitim-polimorfizm/Java-inheritance7/)
9. Java’da Kalıtım 8 - [Alıştırmalar](/java-kalitim-polimorfizm/Java-inheritance8/)
10. Java’da Kalıtım 9 - [Overriding(Ezici) Metotlar](/java-kalitim-polimorfizm/Java-inheritance9/)
11. Java’da Kalıtım 10 - [Overloading(Aşırı Yükleme) Metotlar](/java-kalitim-polimorfizm/Java-inheritance10/)
</div>
## Kalıtımı Neden Kullanırız?

**Kalıtım**(inheritance) ve **polimorfizm**, nesneye dayalı bir programlama dilinde inanılmaz derecede güçlü özelliklerdir. **Loop** ve **conditional**'ların aksine, kodunuzun çalışması için genellikle kalıtım ve polimorfizm kullanmanız gerekmez. Yani önemini, sahip olduğunuz kod devasa bir hale gelmeden anlayamazsınız. Çünkü kodunuz büyüdükçe yöntemleri ve daha sonra sınıfları kullanmaya başlarsınız. Aynı şekilde, projenizin karmaşıklığı arttıkça ve büyük yazılım tasarım projeleri üzerinde çalışmaya başladığınızda, projenin karmaşıklığını kaldırabilmek için **kalıtım** ve **polimorfizm** kullanmaya başlarsınız.

### Örnek

Bir örnekle konuya girmek istiyorum. Diyelim ki elimizde bir **Person**(kişi) sınıfı var. Ancak yazılım tasarım ekibiniz size, artık bir sınıfın yetmediğini, öğrenci ve öğretim üyeleri olmak üzere **2 farklı sınıfa** daha ihtiyacın olduğunu söylüyor.

``` java
public class Person {
  private String name;
  ....
}
```

Şimdi ortada bir sorun var. Varolan **Person** sınıfının üstüne bir de **Student** ve **Faculty** isminde iki sınıf daha oluşturmamız isteniyor. Peki ne yapmamız gerekiyor? Bu sorunu ele almak için ilk olarak **2 tane kötü** çözüm yöntemi sunmak istiyorum. İlk olarak bu ihtimalleri görmeniz kalıtımın önemini anlamak açısından daha yerinde olacaktır.

İlk çözüm, olan biteni sadece **Person** sınıfında tutmaya çalışmak olsun. Yani ne demek istiyorum?


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
---
Yukarıda görüldüğü gibi karşılaştığım sorunları yine **Person** sınıfı içinde bir ``if-else`` conditional'ı kullanarak halledebilirim. Her seferinde bu kişi bir öğrenci mi yoksa fakülte üyesi mi şeklinde bir soru sormam yeterli olacaktır. Burada basit gibi gözükse de üye değişkenleri fazlalaştığında bu durum içinden çıkılamayacak bir hal alabilir.

Fakat farklı öğrencilerin farklı davranışları da olabilir. Aynı durum fakülte üyeleri için de geçerlidir. Okulda mezun öğrenciler de, mezun olmamışlar da olabilir. Her seferinde `if-else` kullanarak bu sorunu çözemeyiz.

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

Bu tarz kodlamaya **spagetti kodlama** da denir. Peki diğer kötü çözüm yöntemimiz nedir? Bu da **Person** sınıfın **Student** ve **Faculty** isminde 2 farklı kopyasını oluşturmak. Yani **Person** sınıfının içeriğini doğrudan kopyalayıp bu yeni sınıflara yapıştırmış olduk.

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
Peki buradan ne gibi problemler çıkardı? Cevabınız, tutarlılıkla(**consistency**) veya bütün nesneleri tek bir veri yapısında saklama özelliğiyle ilgiliyse, doğru yoldasınız demektir. Şimdi her ikisinde de konuşalım. Buradaki problem aslında tam olarak şudur. Örneğin **Person** sınıfının kodunda değişiklik yapmak istediğinizde, bu değişiklikleri **Student** ve **Faculty** sınıflarına da uygulamamız gerekecektir. Peki bunu her seferinde kopyala-yapıştır şeklinde mi yapacağız? Böyle yaparsak ortak kodu **tutarlı** tutmak gerçekten zor olacak.

Veyahut **Person[]** tipinde bir dizi oluşturmak istiyoruz. Bu dizinin öğrenci mi yoksa fakülte üyesi mi olduğuna nasıl karar vereceğim? Diyelim ki bunu sadece öğrenciler ve sadece fakülte üyeleri şeklinde iki farklı diziye ayırırsam ne olur? **Student[]** ve **Faculty[]** .... O zaman da kişileri temsil eden diziyi bir daha asla kullanamam. Sonuç olarak öğrenciler ve fakülte üyeleri için iki farklı dizi tutmam gerekecek.

Tüm insanlar için tek bir veri yapısı tutmanın bir yolu olmadığını da görüyorum. <u>Peki bu neden önemlidir?</u> Yani sadece **Person** şeklinde tutmanın ne önemi olacak? Belkide amacımız, okuldaki insanları, öğrenci veya öğretim görevlisi ayrımı yapmadan tek bir çatı altında kayıt altına almak olabilirdi. Yalnız bu durumda bunu yapmak gerçekten zor olurdu. Öğrencileri potansiyel olarak sıralayabilirim, öğretim görevlilerini de sıralayabilirim, ama bunları nasıl birleştirebilirim? Esasen, bu iki örneğe bakarak ne yapmamız gerektiğini az çok anladık.

<div class="notice--success" markdown="1">
<h4 class="no_toc"><i class="fas fa-lightbulb"></i> Not:</h4>
---
Buradaki **hedefler**,
1. Bütün ortak davranışları bir sınıfta tutmak,
2. Farklı davranışa sahip olanları ise farklı sınıflara ayırmak
3. Tüm bu nesneleri tek bir veri yapısında tutmak.
</div>

İşin güzel yanı bütün bunları kalıtım(inheritance) ile yapabiliriz.


## Referanslar
* [Inheritance](https://docs.oracle.com/javase/tutorial/java/IandI/subclasses.html)
* [Object Oriented Programming in Java](https://www.coursera.org/learn/object-oriented-java?specialization=java-object-oriented)
