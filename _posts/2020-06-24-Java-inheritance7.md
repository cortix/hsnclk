---
title: "Java'da Kalıtım 7 - Sınıf Hiyerarşisinde Değişken İlklendirme"
comments: true
excerpt: "Nesneleri oluştururken, bulunduğumuz alt sınıf kurucuları, hiyerarşik sırada en üstte bulunan Object sınıfına ulaşana kadar içten dışa doğru çağrıldığından bahsetmiştik. Bu derste ise en dıştaki sınıfın üye değişkenlerinden başlayıp, bulunduğumuz sınıfa kadar üye değişkenlerinin nasıl ilklendirildiğini göreceğiz."
header:
  teaser: "assets/images/equality.png"
  og_image: /assets/images/page-header-og-image.png
  overlay_image: /assets/images/unsplash-image-49.jpg
  overlay_filter: 0.5 #rgba(255, 0, 0, 0.5)
  caption: "Photo by [Vino Li](https://unsplash.com/photos/_xvuM4kAIHM) on Unsplash"
  video:
    id: cR9uwtMQt-g
    provider: youtube
  #cta_label: "More Info"
  #cta_url: "https://unsplash.com"
categories:
  - java-kalitim-polimorfizm
tags:
  - nesne oluşumu
  - Kurucu(constructor)
  - derleyici kuralları
  - değişken ilklendirme
  - this keyword in Java
last_modified_at: 2020-06-06T15:12:19-04:00
toc: true
toc_label: "SAYFA İÇERİĞİ"
---

**ÖNEMLİ :** Kendim için aldığım notlar. Umarım size de bir faydası olur. Kullanılan her bir makale referans olarak eklenmiştir.
{: .notice}

## Genel Bakış

Son derslerde Java'da sınıf oluşturmanın veya nesne oluşturmanın nasıl gerçekleştiğine, özellikle kalıtımın nesne oluşturmaya nasıl uygulandığına bakmıştık.

Son iki derste gördüklerimiz özetle şunlardı. Öncelikli olarak tüm nesnelerin içten dışa doğru yaratıldığını söylerek başladık. Son derste derleyici kurallarının bunu nasıl gerçekleştirdiğini gördük. Bu arada bu kuralları bilmek istememizin nedeni, kodumuzu rahatlıkla izlememize yardımcı olmasıdır. Aslında, bu derste, kodumuzda belli başlı hatalar göreceğiz. Bu hataların da bu kuralların uygulanmaması nedeniyle ortaya çıktığını anlayacaksınız.

Şu ana kadar yaptığımız, aslında hiçbir üye değişkeni başlatmadan/ilklendirmeden bir dizi varsayılan kurucu(constructor) oluşturmak oldu. Bu kurucular yaratılırken, alt sınıftan başlayarak, kalıtım hiyerarşisinde en üst sınıf olan ``Object`` sınıfının kurucusuna kadar, belli bir sırayla ilerlediğini gördük. Şimdi ise yarım bıraktığımız iş olan değişkenleri ilklendirme işlemine odaklanacağız.

Muhtemelen bir kurucuda daha önce bir değişken başlattınız(yani ilklendirdiniz). Yalnız bu derste odaklandığımız şey kalıtım için geçerli olan unsurlardır. Ayrıca sınıf oluşturmaya yardımcı olması için aynı sınıf(same-class) kurucuları ve üst sınıf(superclass) kurucuları kullanacağız.

Geçtiğimiz derslerde verdiğimiz örnek sınıflar olan ``Person`` ve ``Student`` sınıflarını bu derste de kullanalım istiyorum.

<figure style="width: 400px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-06-24-Java-inheritance6/student.png" alt="derleyici kural">
  <figcaption></figcaption>
</figure>

<figure style="width: 400px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-06-24-Java-inheritance6/rule4.png" alt="derleyici kural">
  <figcaption></figcaption>
</figure>

``Person`` sınfını ele alacak olursak, derleyici, sınıfın miras aldığı bir başka sınıf yoksa arka planda otomatik olarak ``Object`` sınıfını miras alıyor ve sınıfın bir kurucusu yoksa bir de sınıfa bir kurucu(constructor) yerleştiriyordu. Hatta 3.derleyici kuralımızı hatırlarsanız, bu kurucu içine bir de ``super();`` metodu çağrısı da eklemişti. Ben bu değişiklikler, sanki derleyici tarafından açıkça eklemiş gibi aşağıdaki kodda belirteceğim. Yalnız bunların derleyici tarafından arka planda eklendiğini unutmayın.

```java
public class Person extends Object {
  private String name;
  public Person(){
    super();
  }
}
```

Şimdi yapacağımız şey, Person sınıfı içinde bulunan private ``name`` değişkenini başlatmak(yani ilklendirmek) için bu varsayılan(default) kurucuyu değiştirmek olacak. Yani değişken başlatmayı, kurucuları oluşturduktan sonra en uygun nasıl başlatabiliriz, ona bakacağız.

Aşağıdaki kodda da görüleceği üzere Person kurucusunu, değişken ilklendirmeye olanak verecek şekilde değiştirdik. Yapıcıya(constructor) ``n`` isminde bir ``String`` argümanı ekledik. Daha sonra kurucu içinde sınıfın üye değişkenine bu argümanı ``this.name = n`` ifadesiyle atadık. <u><i>Yalnız burada derleyici bir hata verecektir.</i></u>


<figure style="width: 300px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-06-24-Java-inheritance7/var_init1.png" alt="değişken ilklendirme">
  <figcaption></figcaption>
</figure>

Derleyici kurallarından **3.kuralı** hatırlayacak olursak, <u><i>kurucunun ilk satırı mutlaka aynı sınıf(same-class) yapıcısına veya bir üst sınıf(superclass) yapıcısına bir çağrı olmalıdır.</i></u> O halde ``super();`` çağrısının yerini değiştirerek bu sorunu çözebiliriz.

<figure style="width: 300px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-06-24-Java-inheritance7/var_init2.png" alt="değişken ilklendirme">
  <figcaption></figcaption>
</figure>

Şimdi hem üst sınıf kurucumu doğru bir şekilde çağırabiliyorum hem de üye değişkenimi başlatabiliyorum. Buradan aslında şu çıkarımı da yapabilirsiniz. Bir sınıftan bir objeyi oluşturmadan veyahut sınıfın kendisini yüklemeden önce derleyici hep bir üst sınıfın sorunlarını çözmemiz için bizi zorlar.

Şimdi benzer değişiklikleri Student sınıfında değişkenleri başlatmak için yapalım istiyorum.  Dersin başında Student ve Person sınıflarının derleyici kuralları kapsamında nasıl değiştiğini gösteren 2 şekil göstermiştim. Person sınıfını az önce ele aldığımız için şimdi Student sınıfına odaklanacağız. Hatırlarsanız, derleyici arka planda bizim yerimize eklediği kurucu ve kurucu içindeki super(); üst sınıf çağısırını yapmıştı. Şimdi bu varsayılan(default) Student kurucusunu ele alıp, üzerinde biraz değişiklikler yapacağım. Haliyle değişiklik yapacağımız için bu kurucunun artık bize özel bir kurucu olacağını unutmayın. Bildiğiniz gibi derleyici, sınıf içinde bir kurucu olmadığı zaman müdahele ediyordu.

<figure style="width: 400px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-06-24-Java-inheritance7/var_init3.png" alt="değişken ilklendirme">
  <figcaption></figcaption>
</figure>

``Student`` kurucusunun içinde, ``Person`` kurucusunda yaptığımız gibi benzer ilklendirme işlemini yapalım istiyorum. Eş zamanlı olarak bu kodu bir editörde denediğinizi varsayarak, alacağınız derleme hatalarını birlikte irdeleyebiliriz. İlk gözüme çarpan ``Person`` sınıfının içinde argümansız bir kurucunun olmayışı!! İkincisi ise ``name`` üye değişkeninin ``private`` olması... Hatırlarsanız ``private`` değişkenlere **getter** ve **setter** yöntemlerle erişebiliyorduk. Yalnız ``Person`` sınıfında ``public`` olan bir **getter/setter** yöntemin olmadığını görüyorum. Peki bu durumda ne yapabiliriz? Yani **getter** ve **setter** olmadan ``name`` isimli üye değişkenini nasıl ilklendirebiliriz? Aşağıdaki gibi bir düzenleme ile bu mümkün...

<figure style="width: 400px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-06-24-Java-inheritance7/var_init4.png" alt="değişken ilklendirme">
  <figcaption></figcaption>
</figure>

Şimdi Student sınıfına tamamen keyfi olarak bir argümansız kurucu eklemek istiyorum. Person sınıfında argümansız bir kurucu olmadığı için super("Student") çağrımızın içine bir string eklemem gerekiyor. ``name`` değişkenini ilklendirebilmek için **"Student"** isminde varsayılan bir değer ekliyorum. Bu kod bu şekilde çalışır.

<figure style="width: 400px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-06-24-Java-inheritance7/var_init5.png" alt="değişken ilklendirme">
  <figcaption></figcaption>
</figure>

Daha iyi bir yaklaşım sizce var mı? Evet var.. Çünkü Student sınıfının içinde, argümanlı bir kurucu içinden zaten superclass çağrısı yapılıyor. Tekrardan benzer bir çağrı yapmanın gereği yoktur. Bu gibi durumlarda, aynı sınıf kurucularımı kullanabilirim. Belki de aynı sınıf kurucularımdan biri, tam olarak bu isteğime dayalı bir talebi karşılıyordur. Aşağıdaki kodda yaptığım değişiklik görüleceği üzere, üst sınıf çağrımız olan ``super("Student");`` çağrısını `this("Student")` yapmak oldu. Burada ``this`` kullanarak **bulunduğumuz sınıfın kurucusunu(constructor)** kastettiğimizi anlamışsınızdır.

<figure style="width: 400px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-06-24-Java-inheritance7/var_init6.png" alt="değişken ilklendirme">
  <figcaption></figcaption>
</figure>

Bazı durumlarda bu tarz sınıf içi kuruculara ihtiyaç duyulabilir. Örneğin ``Person`` sınıfının ilklendirmesini dışarıdan gelen parametrelerle değil de kendiniz belirlemek isteyebilirsiniz. Örneğin argümanlı kurucunuzu ``private`` yapıp, bu kurucuya erişimi sadece argümansız kurucuyla yapabildiğinizi hayal edin. Bu gibi durumlarda **this("")** çağrısı ile sınıf içi bir kurucuya erişebilirsiniz.

Bu arada **this("")** ile **this.name** kullanımı arasında fark vardır. İlkinde kurucuyu çağırdığınızı belirtiyorsunuz. İkincisinde ise mevcut nesne üzerinde işlem yaptığınızı ifade ediyorsunuz.

Sonuç olarak değişken ilklendirme kısmını da hallettiğimize göre, Java'da nesne oluşturmanın nasıl gerçekleştiğini bir bütün olarak görüyorsunuz.

**Not :** Hazırladığım **java'da kalıtım serisini** sıralı takip etmiyorsanız bazı şeyler havada kalacağı için aşağıdaki videoyu izlemenizi öneririm. Aşağıdaki hazırladığım java eğitim videosunda, main metodunu da kapsayan bir örnek kod üzerinde, statik ve statik olmayan değişken ve metotların hafıza yönetim modelini ele aldım. Bu video konunun daha iyi anlaşılmasını sağlayacaktır.

{% include video id="cR9uwtMQt-g" provider="youtube" %}


## Referanslar
* [Using the this Keyword](https://docs.oracle.com/javase/tutorial/java/javaOO/thiskey.html)
* [this keyword in Java](https://www.javatpoint.com/this-keyword)
* [Declaring Classes](https://docs.oracle.com/javase/tutorial/java/javaOO/classdecl.html)
* [Object Oriented Programming in Java](https://www.coursera.org/learn/object-oriented-java?specialization=java-object-oriented)
