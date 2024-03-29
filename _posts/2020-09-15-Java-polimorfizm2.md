---
title: "Java'da Polimorfizm 2 - İzlenecek Kurallar Nelerdir?"
comments: false
excerpt: "Java'da Derleme Zamanı Kararları(Compile-time Decision) Çalışma Zamanı Kararları(Runtime Decision) ne anlama gelmektedir?"
header:
  teaser: "/assets/images/svg-book11.svg"
  og_image: /assets/images/svg-book11.svg
  overlay_image: /assets/images/svg-book11.svg
  overlay_color: "#000"
  overlay_filter: "0.3"
  caption: "background by [SVGBackgrounds.com](https://www.svgbackgrounds.com/)"
  #cta_label: "More Info"
  #cta_url: "https://unsplash.com"
categories:
  - java-kalitim-polimorfizm
tags:
  - Java polimorfizm
  - Java compile-time decision
  - Java runtime decision
#last_modified_at: 2022-12-29T15:12:19-04:00
last_modified_at:
toc: true
toc_label: "SAYFA İÇERİĞİ"
toc_sticky: true
---



**ÖNEMLİ :** Kendim için aldığım notlar. Umarım size de bir faydası olur. Kullanılan her bir makale referans olarak eklenmiştir.
{: .notice}

<div class="notice--info" markdown="1">
<h4 class="no_toc"><i class="fas fa-lightbulb"></i> Java’da Kalıtım(inheritance) ve Java’da Polimorfizm Serisi</h4>
---

12. Java’da Polimorfizm 1 - [Amaç?](/java-kalitim-polimorfizm/Java-polimorfizm1/)
13. **Java’da Polimorfizm 2 - İzlenecek Kurallar Nelerdir?**
14. Java’da Polimorfizm 3 - [Casting](/java-kalitim-polimorfizm/Java-polimorfizm3/)
15. Java’da Polimorfizm 4.1 - [Statik ve Dinamik Bağlanma 1](/java-kalitim-polimorfizm/Java-polimorfizm4_1/)
16. Java’da Polimorfizm 4.2 - [Statik ve Dinamik Bağlanma 2](/java-kalitim-polimorfizm/Java-polimorfizm4_2/)
17. Java’da Polimorfizm 4.3 - [Dinamik Bağlanma Örnek](/java-kalitim-polimorfizm/Java-polimorfizm4_3/)
18. Java’da Polimorfizm 5 - [Soyut(Abstract) Sınıflar ve Arayüzler(Interfaces)](/java-kalitim-polimorfizm/Java-polimorfizm5/)
</div>

## Genel Bakış

Katıldığım online bir etkinlikte **object-oriented** nedir şeklinde sorulan bir soruya **Robert C. Martin** şu şekilde yanıt vermişti.

> "A language is object-oriented if it supports dynamic polymorphism" ("Bir dil, dinamik polimorfizmi destekliyorsa nesne yönelimlidir")

Kısa ve öz bir tanım.

Anlaşılacağı üzere polimorfizm nesne yönelimli bir dilin merkezindedir. Bu bölümde ise **derleme zamanı** ve **çalışma zamanı** kurallarına odaklanacağız. Ve bunu bir kez anladığınızda, polimorfizmin çalışma mantığını çok iyi anlayacağınızı temin ederim.

> Derleyici gibi düşün, çalışma zamanı ortamı gibi davran (Rick Ord)

Bu sözün aslında altında yatan mantık çok açıktır. Çünkü bir kodu her çalıştırdığımızda gerçekleşen iki şey vardır. Birincisi, bir derleyicinin yazdığınız kodu yorumlaması gerektiğidir. İkinci şey ise, çalışma zamanı ortamı bu yorumlanan alıntıyı çalıştırır. Polimorfizm hakkında düşüneceksek şayet bunları göz önüne almamız gerekmektedir. O halde, polimorfizm söz konusu olduğunda derleme zamanı kurallarına ve çalışma zamanı kurallarına bakarak devam edelim.

## Derleme Zamanı Kuralları(Compile-time Decision)

1. **Derleyici SADECE referans tipini bilir:** (yani derleyici nesnenin çalışma zamanı tipini bilmez. Derleyicinin amacı, daha sonra çalışma zamanında yürütülecek olan bir yöntem imzası çıkarmaktır.)


    <br/>{% picture 2020-06-29-Java-polimorfizm2/uml1.png --alt Java Polymorphism(java polimorfizm) --img width="100%" height="100%" %}<br/>


    ```java
    Person s = new Student("Hasan", 1111);
    s.toString();
    ```
    Yukarıdaki örnekte bir adet **Person**, bir adet de **Student** sınıfımız bulunmaktadır.  Görüleceği üzere **Person** sınıf tipinde **s** referansımız, **Student** nesnesine işaret ediyor. Planımız her ne kadar **Student** nesnesinde `toString` yöntemini çağırmak gibi görünse de(aslında bir bütün olarak baktığımızda öyle!), javanın kodu bu şekilde yorumlamadığını bilmemizde yarar var. Java kurallarını derleme ve çalışma zamanı olarak ikiye ayırır. Kurala göre de derleyicinin sadece **Person** referansını bildiğini unutmayalım. Yani derleme zamanında javanın bu koddan algıladığı tek şey **s** referansının bir **Person** olduğudur.

2. **Derleyici metot çağrıları için SADECE referans tipinin sınıfına bakar:** *s* referansı ile `toString`'i çağırmayı denediğinizde, java **derleme zamanında** ilk olarak **Person** sınıfına bakacak ve bu `toString` yöntemini bulacaktır.

    <br/>{% picture 2020-06-29-Java-polimorfizm2/uml2.png --alt Java Polymorphism(java polimorfizm) --img width="100%" height="100%" %}<br/>

3. **Derleyici bir yöntem imzası yayımlar:** Derleyicinin bir diğer amacı ise daha sonra çalışma zamanında yürütülecek olan bir **yöntem imzası** çıkarmaktır. Yani parametresiz bir toString yöntem imzası derleyici tarafından daha sonra çalışma zamanında yürütülmek üzere yayımlanır.

    <br/>{% picture 2020-06-29-Java-polimorfizm2/uml3_0.png --alt Java Polymorphism(java polimorfizm) --img width="100%" height="100%" %}<br/>

## Çalışma Zamanı Kuralları(Runtime Decision)

1. **Çalışma zamanında, ilgili yöntemi bulmak için java, nesnenin çalışma zamanı türünü izler**
2. **Derleme zamanında yayımlanan yöntem imzasını gerçek çalışma zamanı sınıfındaki uygun yöntemle eşleştirir:** Yukarıdaki örnekten yola çıkarsak, ``s.toString`` yapmaya çalıştığımızda java, artık çalışma zamanında **s**'in aslında bir **Student** nesnesi olduğunu bilir ve bu metodu ilk bu sınıfın içinde arar. Yukarıdaki koddan yola çıkarsak, java **Student** sınıfında ``toString`` yöntemini bulacak ve çalışma zamanında çalıştıracaktır.

    <br/>{% picture 2020-06-29-Java-polimorfizm2/uml4_0.png --alt Java Polymorphism(java polimorfizm) --img width="100%" height="100%" %}<br/>

    **ÖNEMLİ NOT :** Buraya kadar olanlar ilgili yöntem(yani ``toString`` metodunu kastediyorum) **geçersiz kılınmışsa** gerçekleşecek şeylerdir. Çünkü ilgili yöntem geçersiz kılınmışsa, polimorfizm gereği doğrudan geçersiz kılınan yöntem çalıştırılır. Yani bu örnekte yöntemin geçersiz kılındığını görüyoruz.

3. Peki ilgili yöntem **geçersiz kılınmamışsa** ne olur? Bu durumda ise java çalışma zamanı sınıfında uygun bir yöntem bulamadığı için referans tipinin sınıfındaki yöntemle eşleşir. Bu kısım çok önemlidir. Farzedelim ki yukarıdaki örnek şu şekilde olsaydı(Student sınıfına odaklanmanızı istiyorum. Dikkat ederseniz ``toString`` metodu override edilmemiş)

    <br/>{% picture 2020-06-29-Java-polimorfizm2/uml4.png --alt Java Polymorphism(java polimorfizm) --img width="100%" height="100%" %}<br/>

    ``s.toString`` yazarak metodu çağırmaya çalıştığımızda, **Person** sınıfındaki ``toString`` metodu çalışacaktır.


Aslında polimorfizm, **derleme zamanı** ve **çalışma zamanı polimorfizmi** olarak ikiye ayırabiliriz. Derleme zamanında alınan kararlar sonucu gerçekleşen bağlanmalara **statik/erken bağlanma(static or early binding)**, çalışma zamanında alınan kararlar sonucu gerçekleşen bağlanmalara da **dinamik/geç bağlanma(dynamic or late binding)** denir. Bununla ilgili aslında ayrı bir bölüm açmayı planlıyorum. Çünkü burada bahsedilenlerin dışında da değineceğim bilgiler var.

Şimdi yukarıdaki kuralları düşünerek aşağıdaki soruya cevap bulmaya çalışalım.

## Örnek

Bu sefer **s** referansı ile **getSID()** metoduna ulaşmaya çalışacağız. Yukarıda yazdığım için tekrardan **Student** ve **Person** sınıflarını yazma gereği duymadım. Göreceğiniz üzere bu metot **Student** sınıfının içinde yer almaktadır. Sizce bu kod çalışır mı?

```java
Person s = new Student("Hasan", 1234);
s.getSID();
```

Aslında ilk bakışta kodun çalışacağını düşünebiliriz. Ama çalışma zamanı dışında bir de derleme zamanı kararlarına uymamız gerekmektedir. Sırayla ilerleyecek olursak kodun çalışması için ilk önce derleme zamanı kurallarını karşılaması gerekmektedir. Koda tekrar bakacak olursak, derleyici yalnızca **Person** içindeki yöntemleri bilecektir. Çünkü derleyiciye göre **s** bir **Person** referansıdır. Dolayısıyla, derleyici **Person** sınıfında ``getSID`` yöntemini bulmaya çalışacaktır. Fakat bu yöntemin **Person** sınıfında olmadığını biliyoruz. Bu sebepten ötürü bir **derleme hatası** alırız. Yani kodun çalışma zamanına geçme şansı olmayacaktır.

Yalnız koda baktığımızda bu metodun **Student** sınıfında bulunduğunu biliyoruz. Peki bizim bu bildiğimizi derleyicinin bilmesini nasıl sağlarız. Bunun yöntemi **casting**'dir. Bir sonraki ders bu konu üzerine konuşacağız.

## Özet
Java'da polimorfizm konusundan bahsederken sıklıkla derleme ve çalışma zamanı kararlarından bahsettik. Aslında polimorfizmi tanımsal olarak rahatlıkla ikiye ayırabiliriz.

1. Statik veya derleme zamanı polimorfizmi
2. Dinamik veya çalışma zamanı polimorfizmi

Anlaşılacağı üzere statik polimorfizm derleme zamanında gerçekleşirken, dinamik polimorfizm çalışma zamanında gerçekleşir.

## Referanslar
* [Chapter 5. Conversions and Promotions](https://docs.oracle.com/javase/specs/jls/se7/html/jls-5.html)
* [Polymorphism (biology)](https://en.wikipedia.org/wiki/Polymorphism_(biology))
* [Polymorphism (computer science)](https://en.wikipedia.org/wiki/Polymorphism_(computer_science))
* [Static vs. Dynamic Type](https://inst.eecs.berkeley.edu/~cs61bl/su15/materials/guides/static-dynamic.pdf)
* [Dynamic typing vs. static typing](https://docs.oracle.com/cd/E57471_01/bigData.100/extensions_bdd/src/cext_transform_typing.html#:~:text=First%2C%20dynamically%2Dtyped%20languages%20perform,type%20checking%20at%20compile%20time.&text=If%20a%20script%20written%20in,the%20errors%20have%20been%20fixed.)
* [Static Typing vs Dynamic Typing](https://www.coderscampus.com/static-typing-vs-dynamic-typing/)
* [Difference between Dynamic and Static type assignments in Java](https://stackoverflow.com/questions/20504714/difference-between-dynamic-and-static-type-assignments-in-java/20505326)
* [New JDK 7 Feature: Support for Dynamically Typed Languages in the Java Virtual Machine](https://www.oracle.com/technical-resources/articles/javase/dyntypelang.html)
* [Dynamic Class Loading Example](https://examples.javacodegeeks.com/core-java/dynamic-class-loading-example/)
* [Dynamic Class Loading](https://www.clear.rice.edu/comp310/JavaResources/dynamic_class_load.html)
* [Java Reflection - Dynamic Class Loading and Reloading](http://tutorials.jenkov.com/java-reflection/dynamic-class-loading-reloading.html)
* [Fully Dynamic Classes With ASM](https://dzone.com/articles/fully-dynamic-classes-with-asm)
* [Polymorphism](https://docs.oracle.com/javase/tutorial/java/IandI/polymorphism.html)
* [Object Oriented Programming in Java](https://www.coursera.org/learn/object-oriented-java?specialization=java-object-oriented)
* [Java: Is method name/signature resolution done statically (compile-time)?](https://stackoverflow.com/questions/9223938/java-is-method-name-signature-resolution-done-statically-compile-time)
