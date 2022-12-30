---
title: "Java'da Kalıtım 10 - Overloading(Aşırı Yükleme) Metotlar"
comments: false
excerpt: "Java'da aşırı yükleme(overloading) ne anlama gelmektedir? Neden constructor'larda overloading metotlara ihtiyaç duyarız ve overload yaparken uymamız gereken bir kural var mıdır gibi soruları cevaplamaya çalışacağız."
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
  - Java overloading metot
  - Java aşırı yükleme
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
2. Java’da Kalıtım 2- [Extends](/java-kalitim-polimorfizm/Java-inheritance2/)
3. Java’da Kalıtım 3 - [Referans ve Nesne Tipleri](/java-kalitim-polimorfizm/Java-inheritance3/)
4. Java’da Kalıtım 3.1 - [Static ve Dinamik Tür](/java-kalitim-polimorfizm/Java-inheritance3_1/)
5. Java’da Kalıtım 4 - [Görünürlük/Erişim Değiştiricileri](/java-kalitim-polimorfizm/Java-inheritance4/)
6. Java’da Kalıtım 5 - [Java’da Nesne Oluşturma](/java-kalitim-polimorfizm/Java-inheritance5/)
7. Java’da Kalıtım 6 - [Sınıf İnşası İçin Derleyici Kuralları](/java-kalitim-polimorfizm/Java-inheritance6/)
8. Java’da Kalıtım 7 - [Sınıf Hiyerarşisinde Değişken İlklendirme](/java-kalitim-polimorfizm/Java-inheritance7/)
9. Java’da Kalıtım 8 - [Alıştırmalar](/java-kalitim-polimorfizm/Java-inheritance8/)
10. Java’da Kalıtım 9 - [Overriding(Ezici) Metotlar](/java-kalitim-polimorfizm/Java-inheritance9/)
11. **Java’da Kalıtım 10 - Overloading(Aşırı Yükleme) Metotlar**
12. Java’da Polimorfizm 1 - [Amaç?](/java-kalitim-polimorfizm/Java-polimorfizm1/)

Java’da Kalıtım serisinden sonra Java’da Polimorfizm başlıklı seriye başlamanızı öneririm.
</div>

## Genel Bakış

Java programlama dili aşırı yükleme yöntemlerini destekler ve farklı imzalarına sahip yöntemler arasında ayrım yapabilir. Bu, bir sınıf içindeki yöntemlerin farklı parametre listeleri varsa aynı ada sahip olabileceği anlamına gelir.

### Overloading neden yararlıdır?
Hem bir sınıftan nesne yaratırken hem de sınıfın sahip olduğu bir methodu kullanırken çeşitlilik sağlar. Aşağıdaki örneklerle tanımlamak istediğimiz şeyi daha net ifade edebiliriz.

Constructor, ilgili sınıftan bir nesne oluşturabilmek için reçetedir. O sınıfı kullanabilmek için bir kullanım kılavuzudur. Aşağıdaki iki constructor örneği de beraber çalışabilir. Overloading için güzel bir örnektir.

```java
public class Deneme
{
  //...
  // 1.constructor - parametresiz constructor
  // (default constructor)
  public Deneme()
  {
    //
  }
  // 2.constructor - parametreli constructor
  public Deneme(int a, int b)
  {
    //
  }

}
```
<br/>

Örneğin yukarıdaki **Deneme** sınıfını ele alacak olursak, 1.constructor parametresiz bir kurucudur(constructor). Siz belirtmeseniz de java sizin için bunu arka planda oluşturacaktır. Her neyse, bu constructorı oluşturabilmek için new anahtar sözcüğü ile `new deneme()` yazmanız bu sınıfın bir nesnesini oluşturacaktır. Bu şu demek oluyor, artık bu sınıfı kullanabilmek için elimde bir nesne var demektir. 2. constructor ise parametreli constructor'dır.

Burada aslında bu sınıfın nesnesini oluşturmak için bir ön koşul koymuş oluyorsunuz. Ancak ve ancak 2 parametre aldığınız sürece - bu parametreler belirtildiği üzere integer olmak zorunda - bu sınıfa ait bir nesne oluşturabilirsiniz. Kısacası, bu tarz çeşitlilikler ile nesneyi oluşturmadan önce nesnenin sahip olmasını istediğiniz ön şartları da burada belirtebilirsiniz. Bu ön koşul belki sınıfın instance variable'larını ilklendirmek için parametre veyahut constructor içinde yapılmasını istediğiniz bir işlem için gereken başka bir parametre olabilir.

### Peki overload yaparken uymamız gereken bir kural var mıdır?

**Overload** yapmak için tek şart <u>method veya constructor'ın parametlerinin farklı olmasıdır</u>. Dönüş türünün bir önemi yoktur.

* **Parametreler farklı olmak zorundadır.** (aynı method isminde olmak zorunda!! zaten farklı bir isimde olsa overload yapmış olmayız:) Bunu belirtmemiz bile gereksiz)
* Parametrelerin farklı olmasından kasıt; <u>sadece aldığı parametre sayısı değil, parametre tipi de buna dahildir.</u> Örneğin 2 parametre alan 2 tane overloaded constructor'ınız olabilir. Ama overloaded olabilmesi için bu parametrelerden en az birinin tipi farklı olmak zorundadır.  `Deneme(int a, int b)` ve `Deneme(double a, int b)` gibi..
* **Dönüş türü(Return type)** method imzasının bir parçası değildir.
* Overloaded yöntemler **farklı dönüş tiplerine** sahip olabilir.
* Overloaded için dönüş tipinin(Return type) değiştirilmesi **yeterli değildir**.

### Peki aynı parametreye sahip ama sadece dönüş tipi farklı olan methodlara sahip olsaydık ne olurdu?

Java bu gibi durumlarda derleme hatası verir. Aşağıda bu durumu örneklendirdik.

Daha önce de belirttiğimiz gibi constructor'ı overloading yapabildiğimiz gibi metotları da overload yapabiliriz.

```java
public class Deneme
{
  //...
  public void yontem(int a)
  {
  }

}
```


Yukarıdaki örnek için hangi overloading methodlar uygun olur?

1. public boolean yontem(int a)
2. public void yontem(int a, int b)
3. public void yontem(String a)

Daha önce de belirttiğimiz gibi, <u>yalnızca parametre listesini değiştirerek overloading yapabilirsiniz.</u> Dönüş türünün tek başına değiştirilmesi yönteme overloading yaptırmak için yeterli değildir, çünkü dönüş türü yöntem imzasının bir parçası değildir (yalnızca yöntem adı ve parametre listesi yöntem imzasındadır). Bu yüzden 1. seçenek overloading için geçerli değildir. 2. ve 3. seçenek ise overloading için geçerlidir. Farklı bir parametre listesine ve aynı yöntem adına sahip olmak, aşırı yöntem yüklemesidir.

## Referanslar
* [Defining Methods](https://docs.oracle.com/javase/tutorial/java/javaOO/methods.html)
