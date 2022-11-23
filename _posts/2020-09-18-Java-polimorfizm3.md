---
title: "Java'da Polimorfizm 3 - Casting"
comments: false
excerpt: "Bu bölümde casting işleminin ne olduğuna ve hangi durumlarda bu işleme ihtiyaç duyulduğuna bakacağız. Aynı zamanda bazı casting türleri hakkında bilgi verecek ve casting işleminin derleme ve çalışma zamanlarına etkileri hakkında konuşacağız."
header:
  teaser: "assets/images/equality.webp"
  og_image: /assets/images/equality.webp
  overlay_image: /assets/images/unsplash-image-54.webp
  overlay_filter: 0.5 #rgba(255, 0, 0, 0.5)
  caption: "Photo by [Serge Kutuzov](https://unsplash.com/photos/_FPy-FUndok) on Unsplash"
  #cta_label: "More Info"
  #cta_url: "https://unsplash.com"
categories:
  - java-kalitim-polimorfizm
tags:
  - polimorfizm
  - casting
  - Widening Casting
  - Narrowing Casting
  - instanceof
last_modified_at: 2020-02-19T15:12:19-04:00
toc: true
toc_label: "SAYFA İÇERİĞİ"
toc_sticky: true
---



**ÖNEMLİ :** Kendim için aldığım notlar. Umarım size de bir faydası olur. Kullanılan her bir makale referans olarak eklenmiştir.
{: .notice}


Bazı durumlarda derleme ve çalışma zamanı kararlarını değiştirmek için derleyiciye yardım etmek gerekebilir. Bu gibi durumlarda casting işlemini uygularız. Ne demek istediğimi bir öndeki bölümde yarım bıraktığımız kod üzerinden göstermek istiyorum.


<img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-06-29-Java-polimorfizm2/uml1.webp"  width="200px" height="100%" class="align-center" loading="lazy" alt="polimorfizm">


```java
Person s = new Student("Hasan", 1111);
s.getSID();
```
Şu şekilde bir hata alırız: ``Compile time error: Cannot resolve method 'getSID' in 'Person'``

Hatırlayacağınız üzere derleme zamanında, derleyici **SADECE** referans tipini bildiğinden ve metot çağrıları için sadece referans tipinin sınıfına bakacağından, Person sınıfında ``getSID`` metodunu bulamayacaktır. Ama biz bu metodun Student sınıfında olduğunu biliyoruz. Student sınıfına da ancak **çalışma zamanında** erişebileceğimiz için, ya derleme zamanı hatasını almaya razı olacağız ya da derleyiciye bir **taahhüt** vererek bu metodun aslında Student sınıfında bulunduğunu söyleyeceğiz. Şayet 2. yöntemi seçersek, casting denen işlemi uygulamamız gerekmektedir.

Öncelikli olarak casting çeşitleri hakkında bilgi vermek istiyorum. Sonrasında yukarıdaki sorunun çözümünü ele alırız.

1. **Widening(genişletme) Casting (otomatik olarak gerçekleşir)** - daha küçük bir türü daha büyük bir türe dönüştürme işlemine denir.

    * ``byte`` -> ``short`` -> ``char`` -> ``int`` -> ``long`` -> ``float`` -> ``double``
    *  Subclass -> Superclass
    *  Superclass ref = new Subclass();

    gibi veyahut

    ```java
    public class TestClass {
      public static void main(String[] args) {
        int sampleInt = 2;
        double sampleDouble = sampleInt; // otomatik casting: int'den double'a

        System.out.println(sampleInt);      // Çıktı 2
        System.out.println(sampleDouble);   // Çıktı 2.0
      }
    }
    ```
    Dikkat ederseniz ekstra bir şey yazmadan java otomatik olarak int türünü double türüne dönüştürdü.

2. **Narrowing(daraltma) Casting (elle belirtmeniz gerekmektedir)** - daha büyük bir türü daha küçük boyutlu bir türe dönüştürme işlemidir.

    * ``double`` -> ``float`` -> ``long`` -> ``int`` -> ``char`` -> ``short`` -> ``byte``
    * Superclass -> Subclass
    * Subclass ref = (Subclass) superRef;

    ```java
    public class TestClass {
      public static void main(String[] args) {
        double sampleDouble = 2.66;
        int sampleInt = (int) sampleDouble; // Manuel casting: double'dan int'e

        System.out.println(sampleDouble);   // Çıktı 2.66
        System.out.println(sampleInt);      // Çıktı 2
      }
    }
    ```

    Burada da sampleDouble değişkeni bir double olduğu için, bu türü int türünü daraltırken daraltacağınız türü bu şekilde (int) cast ettiğinizi belirtmeniz gerekmektedir. Yani bu casting türünde elle uygulayacağınız türü belirtmeniz gerekmektedir.

    Bu casting türünde(bu, bölümün ta en başta verdiğimiz örnek oluyor) çok dikkatli olmamız gerekmektedir. Çünkü bunu yaparak bir nevi derleyiciye size güvenmesini söylüyorsunuz. **"Derleyici bana güvenmen gerekiyor, çünkü bunun çalışma zamanında bir Alt Sınıf referansı olacağını biliyorum," diyorsunuz.**

Şimdi en baştaki soruya geri dönelim. Şayet ilgili metot çağırma işlemini şu şekilde yaparsam derleme zamanındaki sorunu çözmüş olurum.

```java
((Student)s ).getSID();
```

Peki kodu şu şekilde değiştirsek sizce ne olurdu?


```java
Person s = new Person("Hasan");
((Student)s ).getSID();
```

Çalışma zamanında aşağıdaki gibi bir hata alırdık.

``Exception in thread "main" java.lang.ClassCastException: Person cannot be cast to Student``

Daha ilk başta yaptığımız hata; çalışma zamanında Person nesnesinin olduğunu bildiğimiz halde, Person sınıfını Student sınıfına gönüştürmeye çalışmamızdır. Bir bakıma derleyicinin güvenini sarstık. Özetle derleyiciye, **"bunun çalışma zamanında bir Student sınıfı olacağını ve bu sınıfın da içinde bir getSID metodu barındıracağını taahhüt ettik."** Daha çalışma zamanındaki sınıfı bile tutturamadık.

```java
Person s = new Student("Bilal", 1234);
((Student) s).getSID();
```
Kodu bu şekilde güncellersek sorunu çözmüş oluruz. Yalnız bu durumu test edebilseydik gerçekten güzel olabilirdi. Yani cast yapmadan önce bunun bir Student sınıfı olup olmadığını test etme şansımız olsaydı demek istiyorum. Bunu yapabilmemizin yolu ``instanceof`` operatörünü  kullanmaktır. Bu operatör bize ``is-a`` ilişkisini **çalışma zamanında** test etme imkanı verir.

```java
if(s instanceof Student){
  ((Student) s).getSID();
}
```
``s instanceof Student`` anlamı ``s`` Student sınıfının bir örneği midir? şeklinde bir soru soruyorsunuz.. Diğer bir şekilde ifade edecek olursak, ``s`` çalışma zamanında Student ile bir ilişki içinde mi? Yalnız bu operatörün çalışma zamanında test ettiğini unutmayın.

Aslında polimorfizm uyguladığımızda casting yapmamıza da gerek kalmaz. Çünkü java çalışma zamanında hangi yöntemi uygulayacağını bilir. Bu yüzden bu bir tercih meselesidir.



## Referanslar
* [Java Type Casting](https://www.w3schools.com/java/java_type_casting.asp)
* [Equality, Relational, and Conditional Operators](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/op2.html)
* [Pattern Matching for the instanceof Operator](https://docs.oracle.com/en/java/javase/14/language/pattern-matching-instanceof-operator.html)
* [Object Oriented Programming in Java](https://www.coursera.org/learn/object-oriented-java?specialization=java-object-oriented)
