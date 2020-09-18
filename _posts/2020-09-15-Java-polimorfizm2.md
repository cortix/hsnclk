---
title: "Java'da Polimorfizm 2 - İzlenecek Kurallar Nelerdir?"
comments: true
excerpt: "Java'da aşırı yükleme ne anlama gelmektedir? Neden constructor'larda overloading metotlara ihtiyaç duyarız ve overload yaparken uymamız gereken bir kural var mıdır gibi soruları cevaplamaya çalışacağız."
header:
  teaser: "assets/images/equality.png"
  og_image: /assets/images/page-header-og-image.png
  overlay_image: /assets/images/unsplash-image-53.jpg
  overlay_filter: 0.5 #rgba(255, 0, 0, 0.5)
  caption: "Photo by [Andras Vas](https://unsplash.com/photos/l2uv7YZcYDE) on Unsplash"
  #cta_label: "More Info"
  #cta_url: "https://unsplash.com"
categories:
  - java-kalitim-polimorfizm
tags:
  - polimorfizm
last_modified_at: 2020-02-19T15:12:19-04:00
toc: true
toc_label: "SAYFA İÇERİĞİ"
---



**ÖNEMLİ :** Kendim için aldığım notlar. Umarım size de bir faydası olur. Kullanılan her bir makale referans olarak eklenmiştir.
{: .notice}


Bu bölümde derleme zamanı ve çalışma zamanı kurallarına odaklanacağız. Ve bunu bir kez anladığınızda, polimorfizm ile ilişkili görünebilecek tüm sihir kaybolur.

> Derleyici gibi düşün, çalışma zamanı ortamı gibi davran (Rick Ord)

Bu sözün aslında altında yatan mantık çok açıktır. Çünkü bir kodu her çalıştırdığımızda gerçekleşen iki şey vardır. Birincisi, bir derleyicinin yazdığınız kodu yorumlaması gerektiğidir. İkinci şey ise, çalışma zamanı ortamı bu yorumlanan alıntıyı çalıştırır. Polimorfizm hakkında düşüneceksek şayet bunları göz önüne almamız gerekmektedir. O halde, polimorfizm söz konusu olduğunda derleme zamanı kurallarına ve çalışma zamanı kurallarına bakarak devam edelim.

## Derleme Zamanı Kuralları

1. **Derleyici SADECE referans tipini bilir:** (yani nesnenin çalışma zamanı tipini bilmez. Derleyicinin amacı, daha sonra çalışma zamanında yürütülecek olan bir yöntem imzası çıkarmaktır.)

    <figure style="width: 200px" class="align-center">
      <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-06-29-Java-polimorfizm2/uml1.png" alt="polimorfizm">
      <figcaption></figcaption>
    </figure>

    ```java
    Person s = new Student("Hasan", 1111);
    s.toString();
    ```

      Yukarıdaki örnekte bir adet Person, bir adet de Student sınıfımız bulunmaktadır.  Görüleceği üzere Person sınıf tipinde s referansımız Student nesnesine işaret ediyor. Planımız her ne kadar Student nesnesinde toString yöntemini çağırmak gibi görünse de(aslında bir bütün olarak baktığımızda öyle!), javanın kodu bu şekilde yorumlamadığını bilmemizde yarar var. Java kurallarını derleme ve çalışma zamanı olarak ikiye ayırır. Kurala göre de derleyicinin sadece Person referansını bildiğini unutmayalım. Yani derleme zamanında javanın bu koddan algıladığı tek şey s referansının bir Person olduğudur.

2. **Derleyici metot çağrıları için SADECE referans tipinin sınıfına bakar:** s referansı ile toString'i çağırmayı denediğinizde, java **derleme zamanında** ilk olarak Person sınıfına bakacak ve bu toString yöntemini bulacaktır.

    <figure style="width: 600px" class="align-center">
      <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-06-29-Java-polimorfizm2/uml2.png" alt="polimorfizm">
      <figcaption></figcaption>
    </figure>


3. **Derleyici bir yöntem imzası çıkarır:** Sonrasında derleme zamanında gerçekleşen bir diğer şey, çalışma zamanında yürütülecek parametresiz bir toString yöntemi imzasının derleyici tarafından yayımlanmasıdır.

    <figure style="width: 600px" class="align-center">
      <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-06-29-Java-polimorfizm2/uml3.png" alt="polimorfizm">
      <figcaption></figcaption>
    </figure>


## Çalışma Zamanı Kuralları

1. **Çalışma zamanında, ilgili yöntemi bulmak için java, nesnenin çalışma zamanı türünü izler**
2. **Derleme zamanında yayımlanan yöntem imzasını gerçek çalışma zamanı sınıfındaki uygun yöntemle eşleştirir:** Yukarıdaki örnekten yola çıkarsak, ``s.toString`` yapmaya çalıştığımızda, artık Java çalışma zamanında, **s**'in aslında bir Student nesnesi olduğunu bilir. Yani Student sınıfında toString yöntemini bulacak ve çalışma zamanında çalıştıracaktır.

    <figure style="width: 600px" class="align-center">
      <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-06-29-Java-polimorfizm2/uml3.png" alt="polimorfizm">
      <figcaption></figcaption>
    </figure>

Aslında polimorfizm derleme zamanı ve çalışma zamanı polimorfizmi olarak ikiye ayırabiliriz. Derleme zamanında alınan kararlar sonucu gerçekleşen bağlanmalara statik/erken bağlanma(static or early binding), çalışma zamanında alınan kararlar sonucu gerçekleşen bağlanmalara da dinamik/geç bağlanma(dynamic or late binding) denir. Bununla ilgili aslında ayrı bir bölüm açmayı planlıyorum. Çünkü burada bahsedilenlerin dışında da değineceğim bilgiler var.

Şimdi yukarıdaki kuralları düşünerek aşağıdaki soruya cevap bulmaya çalışalım.

## Örnek

Bu sefer s referansı ile getSID() metoduna ulaşmaya çalışacağız. Yukarıda yazdığım için tekrardan Student ve Person sınıflarını yazma gereği duymadım. Göreceğiniz üzere bu metot Student sınıfının içinde yer almaktadır. Sizce bu kod çalışır mı?

```java
Person s = new Student("Hasan", 1234);
s.getSID();
```

Aslında ilk bakışta kodun çalışacağını düşünebiliriz. Ama çalışma zamanı dışında bir de derleme zamanı kararlarına uymamız gerekmektedir. Sırayla ilerleyecek olursak kodun çalışması için ilk önce derleme zamanı kurallarını karşılaması gerekmektedir. Koda tekrar bakacak olursak, derleyici yalnızca Person içindeki yöntemleri bilecektir. Çünkü derleyiciye göre s bir Person referansıdır. Dolayısıyla, derleyici Person sınıfında ``getSID`` yöntemini bulmaya çalışacaktır. Fakat bu yöntemin Person sınıfında olmadığını biliyoruz. Bu sebepten ötürü bir derleme hatası alırız. Yani kodun çalışma zamanına geçme şansı olmayacaktır.

Yalnız koda baktığımızda bu metodun Student sınıfında bulunduğunu biliyoruz. Peki bizim bu bildiğimizi derleyicinin bilmesini nasıl sağlarız. Bunun yöntemi **casting**'dir. Bir sonraki ders bu konu üzerine konuşacağız.


## Referanslar
* [https://docs.oracle.com/javase/specs/jls/se7/html/jls-5.html](https://docs.oracle.com/javase/specs/jls/se7/html/jls-5.html)
* [https://en.wikipedia.org/wiki/Polymorphism_(biology)](https://en.wikipedia.org/wiki/Polymorphism_(biology))
* [https://en.wikipedia.org/wiki/Polymorphism_(computer_science)](https://en.wikipedia.org/wiki/Polymorphism_(computer_science))
* [https://inst.eecs.berkeley.edu/~cs61bl/su15/materials/guides/static-dynamic.pdf](https://inst.eecs.berkeley.edu/~cs61bl/su15/materials/guides/static-dynamic.pdf)
* [Dynamic typing vs. static typing](https://docs.oracle.com/cd/E57471_01/bigData.100/extensions_bdd/src/cext_transform_typing.html#:~:text=First%2C%20dynamically%2Dtyped%20languages%20perform,type%20checking%20at%20compile%20time.&text=If%20a%20script%20written%20in,the%20errors%20have%20been%20fixed.)
* [https://howtoprogramwithjava.com/dynamic-typing-vs-static-typing/](https://howtoprogramwithjava.com/dynamic-typing-vs-static-typing/)
* [https://stackoverflow.com/questions/20504714/difference-between-dynamic-and-static-type-assignments-in-java/20505326](https://stackoverflow.com/questions/20504714/difference-between-dynamic-and-static-type-assignments-in-java/20505326)
* [https://www.oracle.com/technical-resources/articles/javase/dyntypelang.html](https://www.oracle.com/technical-resources/articles/javase/dyntypelang.html)
* [Dynamic Class Loading Example](https://examples.javacodegeeks.com/core-java/dynamic-class-loading-example/)
* [https://www.clear.rice.edu/comp310/JavaResources/dynamic_class_load.html](https://www.clear.rice.edu/comp310/JavaResources/dynamic_class_load.html)
* [http://tutorials.jenkov.com/java-reflection/dynamic-class-loading-reloading.html](http://tutorials.jenkov.com/java-reflection/dynamic-class-loading-reloading.html)
* [https://dzone.com/articles/fully-dynamic-classes-with-asm](https://dzone.com/articles/fully-dynamic-classes-with-asm)
* [https://docs.oracle.com/javase/tutorial/java/IandI/polymorphism.html](https://docs.oracle.com/javase/tutorial/java/IandI/polymorphism.html)
* [https://www.coursera.org/learn/object-oriented-java?specialization=java-object-oriented](https://www.coursera.org/learn/object-oriented-java?specialization=java-object-oriented)
