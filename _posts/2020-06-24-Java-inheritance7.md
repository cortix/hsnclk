---
title: "Java'da Kalıtım 7 - Sınıf Hiyerarşisinde Değişken İlklendirme (Java Variable Initialization)"
comments: false
excerpt: "Bu bölümde java'da en dıştaki sınıfın üye değişkenlerinden başlayıp, içteki sınıfa kadar üye değişkenlerinin nasıl ilklendirildiğini göreceğiz."
header:
  teaser: "/assets/images/svg-book10.svg"
  og_image: /assets/images/svg-book10.svg
  overlay_image: /assets/images/svg-book10.svg
  overlay_color: "#000"
  overlay_filter: "0.3"
  caption: "background by [SVGBackgrounds.com](https://www.svgbackgrounds.com/)"
#  video:
#    id: cR9uwtMQt-g
#    provider: youtube
  #cta_label: "More Info"
  #cta_url: "https://unsplash.com"
categories:
  - java-kalitim-polimorfizm
tags:
  - Java inheritance
  - Java nesne oluşumu
  - Java Kurucu(constructor)
  - Java derleyici kuralları
  - Java değişken ilklendirme
  - Java this keyword
#last_modified_at: 2023-01-06T15:12:19-04:00
last_modified_at:
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
8. **Java’da Kalıtım 7 - Sınıf Hiyerarşisinde Değişken İlklendirme**
9. Java’da Kalıtım 8 - [Alıştırmalar](/java-kalitim-polimorfizm/Java-inheritance8/)
10. Java’da Kalıtım 9 - [Overriding(Ezici) Metotlar](/java-kalitim-polimorfizm/Java-inheritance9/)
11. Java’da Kalıtım 10 - [Overloading(Aşırı Yükleme) Metotlar](/java-kalitim-polimorfizm/Java-inheritance10/)
</div>

## Genel Bakış

Son derslerde Java'da sınıf oluşturmanın veya nesne oluşturmanın nasıl gerçekleştiğine, özellikle kalıtımın nesne oluşturmaya nasıl uygulandığına bakmıştık.

Son iki bölümde gördüklerimiz özetle şunlardı. Öncelikli olarak tüm nesnelerin içten dışa doğru yaratıldığını söyleyerek başladık. Son bölümde derleyici kurallarının bunu nasıl gerçekleştirdiğini gördük. Bu arada bu kuralları bilmek istememizin nedeni, kodumuzu rahatlıkla izlememize yardımcı olmasıdır. Aslında bu bölümde, kodumuzda belli başlı hatalar göreceğiz. Bu hataların da bu kuralların uygulanmaması nedeniyle ortaya çıktığını anlayacaksınız.

Şu ana kadar yaptığımız, aslında hiçbir üye değişkeni başlatmadan/ilklendirmeden bir dizi varsayılan kurucu(constructor) oluşturmak oldu. Bu kurucular yaratılırken, alt sınıftan başlayarak, kalıtım hiyerarşisinde en üst sınıf olan **Object** sınıfının kurucusuna kadar, belli bir sırayla ilerlediğini gördük. Şimdi ise yarım bıraktığımız iş olan değişkenleri ilklendirme işlemine odaklanacağız.

Muhtemelen bir kurucuda daha önce bir değişken başlattınız(yani ilklendirdiniz). Yalnız bu bölümde odaklandığımız şey kalıtım için geçerli olan unsurlardır. Ayrıca sınıf oluşturmaya yardımcı olması için aynı sınıf(same-class) kurucuları ve üst sınıf(superclass) kurucuları kullanacağız.

Geçtiğimiz derslerde verdiğimiz örnek sınıflar olan **Person** ve **Student** sınıflarını bu bölümde de kullanalım istiyorum.

<br/>{% picture 2020-06-24-Java-inheritance6/student.png --alt Java compiler rule(java derleyici kuralı) --img width="100%" height="100%" %}<br/>

<br/>{% picture 2020-06-24-Java-inheritance6/rule4.png --alt Java compiler rule(java derleyici kuralı) --img width="100%" height="100%" %}<br/>

**Person** sınfını ele alacak olursak, derleyici, sınıfın miras aldığı bir başka sınıf yoksa arka planda otomatik olarak **Object** sınıfını miras alıyor ve sınıfın bir kurucusu yoksa bir de sınıfa bir **kurucu(constructor)** yerleştiriyordu. Hatta 3.derleyici kuralımızı hatırlarsanız, bu kurucu içine bir de ``super();`` metodu çağrısı da eklemişti. Ben bu değişiklikler, sanki derleyici tarafından açıkça eklemiş gibi aşağıdaki kodda belirteceğim. Yalnız bunların derleyici tarafından arka planda eklendiğini unutmayın.

```java
public class Person extends Object {
  private String name;
  public Person(){
    super();
  }
}
```

Şimdi yapacağımız şey, Person sınıfı içinde bulunan private **name** değişkenini başlatmak(yani ilklendirmek) için bu varsayılan(default) kurucuyu değiştirmek olacak. Yani değişken başlatmayı, kurucuları oluşturduktan sonra en uygun nasıl başlatabiliriz, ona bakacağız.

Aşağıdaki kodda da görüleceği üzere Person kurucusunu, değişken ilklendirmeye olanak verecek şekilde değiştirdik. Yapıcıya(constructor) ``n`` isminde bir **String** argümanı ekledik. Daha sonra kurucu içinde sınıfın üye değişkenine bu argümanı ``this.name = n`` ifadesiyle atadık. <u>Yalnız burada derleyici bir hata verecektir.</u>



<br/>{% picture 2020-06-24-Java-inheritance7/var_init1.png --alt Java Variable Initialization(java değişken ilklendirme) --img width="100%" height="100%" %}<br/>



Derleyici kurallarından **3.kuralı** hatırlayacak olursak, <u>kurucunun ilk satırı mutlaka aynı sınıf(same-class) yapıcısına veya bir üst sınıf(superclass) yapıcısına bir çağrı olmalıdır.</u> O halde ``super();`` çağrısının yerini değiştirerek bu sorunu çözebiliriz.


<br/>{% picture 2020-06-24-Java-inheritance7/var_init2.png --alt Java Variable Initialization(java değişken ilklendirme) --img width="100%" height="100%" %}<br/>


Şimdi hem üst sınıf kurucumu doğru bir şekilde çağırabiliyorum hem de üye değişkenimi başlatabiliyorum. Buradan aslında şu çıkarımı da yapabilirsiniz. Bir sınıftan bir objeyi oluşturmadan veyahut sınıfın kendisini yüklemeden önce derleyici hep bir üst sınıfın sorunlarını çözmemiz için bizi zorlar.

Şimdi benzer değişiklikleri Student sınıfında değişkenleri başlatmak için yapalım istiyorum.  Dersin başında **Student** ve **Person** sınıflarının derleyici kuralları kapsamında nasıl değiştiğini gösteren **2 şekil** göstermiştim. **Person** sınıfını az önce ele aldığımız için şimdi **Student** sınıfına odaklanacağız.

Hatırlarsanız, derleyici arka planda bizim yerimize eklediği kurucu ve kurucu içindeki `super();` üst sınıf çağrısını yapmıştı. Şimdi bu varsayılan(default) **Student** kurucusunu ele alıp, üzerinde biraz değişiklikler yapacağım. Haliyle değişiklik yapacağımız için bu kurucunun artık bize özel bir kurucu olacağını unutmayın. Bildiğiniz gibi derleyici, sınıf içinde bir kurucu olmadığı zaman müdahale ediyordu.

<br/>{% picture 2020-06-24-Java-inheritance7/var_init3.png --alt Java Variable Initialization(java değişken ilklendirme) --img width="100%" height="100%" %}<br/>

**Student** kurucusunun içinde, **Person** kurucusunda yaptığımız gibi benzer ilklendirme işlemini yapalım istiyorum. Eş zamanlı olarak bu kodu bir editörde denediğinizi varsayarak, alacağınız derleme hatalarını birlikte irdeleyebiliriz. İlk gözüme çarpan **Person** sınıfının içinde argümansız bir kurucunun olmayışı!! İkincisi ise **name** üye değişkeninin **private** olması... Hatırlarsanız **private** değişkenlere **getter** ve **setter** yöntemlerle erişebiliyorduk. Yalnız **Person** sınıfında **public** olan bir **getter/setter** yöntemin olmadığını görüyorum. Peki bu durumda ne yapabiliriz? Yani **getter** ve **setter** olmadan **name** isimli üye değişkenini nasıl ilklendirebiliriz? Aşağıdaki gibi bir düzenleme ile bu mümkün...

<br/>{% picture 2020-06-24-Java-inheritance7/var_init4.png --alt Java Variable Initialization(java değişken ilklendirme) --img width="100%" height="100%" %}<br/>

Şimdi Student sınıfına tamamen keyfi olarak bir argümansız kurucu eklemek istiyorum. Person sınıfında argümansız bir kurucu olmadığı için super("Student") çağrımızın içine bir string eklemem gerekiyor. **name** değişkenini ilklendirebilmek için **"Student"** isminde varsayılan bir değer ekliyorum. Bu kod bu şekilde çalışır.

<br/>{% picture 2020-06-24-Java-inheritance7/var_init5.png --alt Java Variable Initialization(java değişken ilklendirme) --img width="100%" height="100%" %}<br/>

Daha iyi bir yaklaşım sizce var mı? Evet var.. Çünkü Student sınıfının içinde, argümanlı bir kurucu içinden zaten superclass çağrısı yapılıyor. Tekrardan benzer bir çağrı yapmanın gereği yoktur. Bu gibi durumlarda, aynı sınıf kurucularımı kullanabilirim. Belki de aynı sınıf kurucularımdan biri, tam olarak bu isteğime dayalı bir talebi karşılıyordur. Aşağıdaki kodda yaptığım değişiklik görüleceği üzere, üst sınıf çağrımız olan ``super("Student");`` çağrısını **this("Student")** yapmak oldu. Burada **this** kullanarak **bulunduğumuz sınıfın kurucusunu(constructor)** kastettiğimizi anlamışsınızdır.

<br/>{% picture 2020-06-24-Java-inheritance7/var_init6.png --alt Java Variable Initialization(java değişken ilklendirme) --img width="100%" height="100%" %}<br/>

Bazı durumlarda bu tarz sınıf içi kuruculara ihtiyaç duyulabilir. Örneğin **Person** sınıfının ilklendirmesini dışarıdan gelen parametrelerle değil de kendiniz belirlemek isteyebilirsiniz. Örneğin argümanlı kurucunuzu **private** yapıp, bu kurucuya erişimi sadece argümansız kurucuyla yapabildiğinizi hayal edin. Bu gibi durumlarda **this("")** çağrısı ile sınıf içi bir kurucuya erişebilirsiniz.

Bu arada **this("")** ile **this.name** kullanımı arasında fark vardır. İlkinde kurucuyu çağırdığınızı belirtiyorsunuz. İkincisinde ise mevcut nesne üzerinde işlem yaptığınızı ifade ediyorsunuz.

Sonuç olarak değişken ilklendirme kısmını da hallettiğimize göre, Java'da nesne oluşturmanın nasıl gerçekleştiğini bir bütün olarak görüyorsunuz.

<div class="notice--success" markdown="1">
<h4 class="no_toc"><i class="fas fa-lightbulb"></i> Not:</h4>
---
**Not :** Hazırladığım **java'da kalıtım serisini** sıralı takip etmiyorsanız bazı şeyler havada kalacağı için aşağıdaki videoyu izlemenizi öneririm.

Aşağıdaki hazırladığım java eğitim [videosunda](https://www.youtube.com/watch?v=cR9uwtMQt-g), **main** metodunu da kapsayan bir örnek kod üzerinde, statik ve statik olmayan değişken ve metotların hafıza yönetim modelini ele aldım.
</div>




## Referanslar
* [Using the this Keyword](https://docs.oracle.com/javase/tutorial/java/javaOO/thiskey.html)
* [this keyword in Java](https://www.javatpoint.com/this-keyword)
* [Declaring Classes](https://docs.oracle.com/javase/tutorial/java/javaOO/classdecl.html)
* [Object Oriented Programming in Java](https://www.coursera.org/learn/object-oriented-java?specialization=java-object-oriented)
