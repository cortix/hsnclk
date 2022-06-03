---
title: "Java'da Kalıtım 6 - Sınıf İnşası için Derleyici Kuralları"
comments: false
excerpt: "Bu derste Java'da sınıf inşaası sırasında derleyicinin yazdığımız kodu nasıl modifiye edip Jvm'e gönderdiğinden ve belli başlı derleyici kurallarından bahsedeceğiz. Bunun yanı sıra Java'nın arka planda nasıl çalıştığını da ele alacağız."
header:
  teaser: "assets/images/equality.png"
  og_image: /assets/images/page-header-og-image.png
  overlay_image: /assets/images/unsplash-image-48.jpg
  overlay_filter: 0.5 #rgba(255, 0, 0, 0.5)
  caption: "Photo by [Daniel Salgado](https://unsplash.com/photos/1eTc_d3sdHs) on Unsplash"
#  video:
#    id: cR9uwtMQt-g
#    provider: youtube
  #cta_label: "More Info"
  #cta_url: "https://unsplash.com"
categories:
  - java-kalitim-polimorfizm
tags:
  - nesne oluşumu
  - Kurucu(constructor)
  - derleyici kuralları
  - Java Virtual Machine(jvm)
  - Javac

last_modified_at: 2020-06-06T15:12:19-04:00
toc: true
toc_label: "SAYFA İÇERİĞİ"
---

**ÖNEMLİ :** Kendim için aldığım notlar. Umarım size de bir faydası olur. Kullanılan her bir makale referans olarak eklenmiştir.
{: .notice}

## Genel Bakış

Bir önceki derste Java'da nesnelerin içten dışa nasıl inşa edildiğini gördük. Bu derste ise bu durumun neden bu şekilde gerçekleştiğine bakacağız.

Hatırlarsanız bir önceki ders, ``Object`` sınıfını ``extends`` etmediğimiz halde java bunu nasıl biliyor? şeklinde bir soru sormuştuk. Bu bölümde bunun cevabını bulmaya çalışalım.

Aslında bu şekilde olmasının nedeni tamamen Java derleyici kurallarından kaynaklanmaktadır. Nesnenin içten dışa oluşturulmasını sağlamak için bu derleyici kurallarının ne olduğu ve nasıl çalıştığı hakkında konuşalım istiyorum.

Öncelikli olarak, bir önceki derste ne yaptığımızı ve en son nerede kaldığımız hatırlayalım istiyorum. Elimizde bir Student sınıfı vardı. Bu sınıf Person isimli bir başka sınıfı miras alıyordu. Person sınıfı ise biz belirlemesekte java tarafından Object sınıfını miras almaya maruz bırakılıyordu.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-06-23-Java-inheritance5/hierarchy5.png" alt="hierarchy">

Tam da bu noktada, böyle bir planımız olmadığı halde, Object sınıfını neden miras almak zorunda olduğumuzu sormuştuk. Dilerseniz bunun öncesinde java'nın nasıl çalıştığı hakkında bilgi sahibi olalım istiyorum.

## Java Nasıl Çalışır?

Java programlama dilinde, tüm kaynak kodu önce ``.java`` uzantısıyla biten düz metin dosyalarında yazılır. Bu kaynak kodu bir editör aracılığıyla yazdığımız java programlama dilidir ve okunabilir(human-readeble) bir koddur. Yalnız java ile yazılan bir uygulamanın JVM'de çalıştırılabilmesi için önce uygulamanın Java kaynak kodunu JVM'nin tanıdığı bir biçime dönüştürmesi gerekir.

> Bu arada JVM'den kısaca bahsetmek istiyorum. Java Sanal Makinesi, Java programlama dilinin merkezinde yer alır. Aslında, Java Sanal Makinesi uygulamasını çalıştırmadan bir Java programını çalıştıramazsınız. Java Sanal Makinesi, aslında bir Java programını çalıştıran motorun adıdır ve taşınabilirliği, verimliliği ve güvenliği de dahil olmak üzere Java'nın birçok özelliğinin anahtarıdır.


Kaldığımız yerden devam edecek olursak, bu kaynak dosyalar daha sonra ``javac`` derleyicisi(compiler) tarafından ``.class`` dosyalarına derlenir. ``javac`` gibi derleyiciler genellikle Java kaynak kodunu Java sınıfı(yani ``.class``) dosya biçiminde derlemek için kullanılır. Bir ``.class`` dosyası, bilgisayar işlemcinize özgü makine kodu(native code) içermez; bunun yerine bunun yerine ``bytecodes`` içerir. ``bytecode`` Java Sanal Makinesi'nin(JVM) makine dilidir. JVM ise bu ``bytecode`` yorumlar(interpret). Daha sonrasında ``java`` başlatıcı aracı, uygulamanızı Java Virtual Machine örneğiyle çalıştırır.

> javac, Oracle'ın Java Geliştirme Kiti'nde (JDK) bulunan birincil bir Java derleyicisidir(compiler). Derleyici, Java dil şartnamesine (Java language specification-JLS) uygun kaynak kodunu kabul eder ve Java Sanal Makine Şartnamesi'ne (JVMS-Java Virtual Machine Specification) uygun Java bayt kodu üretir.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-06-24-Java-inheritance6/getStarted-compiler.gif" alt="get started compiler">

Çok basit şekliyle javanın çalışma şekli bu şekildedir. Hatta bunu bir editörde(Netbeans,Eclipse vb.) denemek yerine .java uzantılı bir dosya yaratarak deneyebilirsiniz.

## Java Derleyici Kuralları Nedir?

Hatırlarsanız yukarıda, derleyici kurallarından bahsetmiştik. Peki bu karallar nelerdir? Az önce çizdiğimiz şeklin bir başka versiyonunu göstermek istiyorum.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-06-24-Java-inheritance6/jvm.png" alt="java'nın çalışma şekli">

Yukarıdaki şekilde odaklanmanızı istediğim bölüm aslında derleyicinin kod ekleme şartları kapsamında yaptığı eklemelerdir. Evet derleyici kodumuzu ``bytecode``'a çevirirken belli kurallar çerçevesinde çeşitli komut eklemeleri yapar. Biz burada bütün bu komut eklemelerinden bizim için önemli olan 3 tanesine bakacağız.

Peki, Java derleyicisi ne yapıyor ve bu kurallar nelerdir? Yaptığı şey aslında yazdığımız kodu, jvm'nin anlayacağı şekilde değiştirmektir. Önceki örnek koddan yola çıkarak ``Person`` sınıfı üzerinde bu değişiklikleri göstermeye çalışalım.

### Kural 1

Birinci kural: eğer bir üst sınıfınız yoksa, derleyici size bir tane verecektir. Bu sınıf da daha önce bahsettiğimiz ``Object`` sınıfıdır. Böylelikle Object sınıfının nereden geldiğini anlamış bulunuyoruz.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-06-24-Java-inheritance6/rule1.png" alt="derleyici kural1">

``Person`` sınıfının ``Object`` sınıfını miras aldığını artık biliyoruz. Ama bu sınıflara ait "kurucuların" nereden çağrıldığını bilmiyoruz? O zaman şu soruyu sorabiliriz... ``Person()`` ve daha sonra ``Object()`` kurucularını nerede çağırdık? Yani ``Person()`` ya da ``Object()`` kurucusu olarak adlandırdığımız yer neresi?  2.kuralımızın ortaya çıktığı yer de tam olarak burasıdır.

### Kural 2

İkinci kural: eğer bir kurucunuz yoksa, Java derleyicisi size bir tane verecektir. Verilen kurucu varsayılan(default) bir kurucu olacağı için, argüman almaz.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-06-24-Java-inheritance6/rule2.png" alt="derleyici kural2">

### Kural 3

Ve sonra tüm kurucularla(constructor) ilişkili başka bir kurallar dizisi uygulanacaktır. Bu kurallar dizisi veya diğer bir deyişle üçüncü kuralımız:

* Kurucunun 1. satırında ya aynı sınıf kurucusunu çağıran bir **this(args<sub>opt</sub>)** ifadesi olacak ya da bir üst sınıf kurucusunu çağıran bir **super(args<sub>opt</sub>)** ifadesi olacaktır.
* Bunların ikisi de yoksa java derleyicisi şu şekilde bir ifade ekler: **super();**

Şekilde görüldüğü gibi Java derleyicisi, ``Person`` sınıfının varsayılan(default) kurucusuna **super()** olarak tanımlanan bir çağrı ekleyecektir.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-06-24-Java-inheritance6/rule3.png" alt="derleyici kural3">

## Özet

Soldaki kod bloğu bizim yazdığımız kodu temsil etmektedir. Sağdaki ise derleyicinin bizim yazdıklarımızdan anladığıdır:) Yani **mavi bölümleri** derleyi kendi ekleyecektir.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-06-24-Java-inheritance6/rule4.png" alt="derleyici kural">

### Örnek

Student sınıfının Person sınıfını miras aldığını önceki dersten biliyoruz. Peki sizce aşağıdaki kodu derleyici nasıl değiştirir?

```java
public class Student extends Person {

}
```

Cevap: İlk kuralımız, sınıfın bir üst sınıfı olmadığında, derleyicinin bize bir tane vereceğini söylüyordu. Burada ``Student`` sınıfı zaten ``Person`` sınıfını miras aldığı için bizim bir şey yapmamıza gerek kalmıyor. Ama ikinci ve üçüncü kuralları uygulayabiliriz. Örneğin görünürde bir kurucu(constructor) yok. O halde derleyici bir tane ekleyebilir. Aynı şekilde derleyici tarafından constructor içine bir de ``super()`` çağrısını eklenecek olursa, 3.kuralımızı da gerçekleştirmiş olacağız.

```java
public class Student extends Person {
  public Student(){
    super();
  }
}

```

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-06-24-Java-inheritance6/student.png" alt="derleyici kural">

Bu süreçleri derleyici sizden bağımsız bir şekilde arka planda gerçekleştirecektir. Ama sürecin nasıl ilerlediğini bilmekte yarar.

Şu ana kadar gerçekleştirdiğimiz şeyler kurucuları alt sınıftan hiyerarşinin üst sınıfı olan Object'e kadar götürmek oldu. Fakat, hatırlarsanız geri dönerken üye değişkenleri ilklendiriyorduk. Henüz üye değişkenlerini ilklendirmedik(initialize). Bir sonraki ders bu konuyu ele alacağız.

Sizce neden derleyicinin kodunuzu derlerken uyguladığı kuralları öğreniyoruz? Çünkü kodda hata ayıklamak için bir noktada bunları bilmeniz gerekebilir. Kodunuzu daha iyi anlamanıza yardımcı olmak ve daha da önemlisi, bu kuralları bilmediğinizde süreci çözümsüzlüğe götürecek hataları ayıklayabilmeniz için bu ayrıntılara dalıyoruz. Örneğin bu kuralları bilmediğinizde, şu şekilde kafa karıştırıcı bir soru aklınızda kalabilir. Ben default bir üst sınıf constructor çağırmadım, neden bu kod yürütülüyor? Bir sonraki ders biraz daha detaya gireceğiz.

**Not :** Hazırladığım **java'da kalıtım serisini** sıralı takip etmiyorsanız bazı şeyler havada kalacağı için aşağıdaki videoyu izlemenizi öneririm. Aşağıdaki hazırladığım java eğitim [videosunda](https://www.youtube.com/watch?v=cR9uwtMQt-g), main metodunu da kapsayan bir örnek kod üzerinde, statik ve statik olmayan değişken ve metotların hafıza yönetim modelini ele aldım. Bu video konunun daha iyi anlaşılmasını sağlayacaktır.



## Referanslar
* [About the Java Technology](https://docs.oracle.com/javase/tutorial/getStarted/intro/definition.html)
* [List of Java bytecode instructions](https://en.wikipedia.org/wiki/Java_bytecode_instruction_listings)
* [Java virtual machine](https://en.wikipedia.org/wiki/Java_virtual_machine)
* [How	Java	Works](https://www.cs.cornell.edu/courses/JavaAndDS/files/howJavaWorks.pdf)
* [How	Java	Works](https://www2.cs.duke.edu/courses/cps108/fall99/slides/slides14.pdf)
* [How	Java	Works](http://www.cs.cmu.edu/~jcarroll/15-100-s05/supps/basics/history.html)
* [Java Programming Environment and the Java Runtime Environment (JRE)](https://docs.oracle.com/cd/E19455-01/806-3461/6jck06gqd/index.html)
* [Declaring Classes](https://docs.oracle.com/javase/tutorial/java/javaOO/classdecl.html)
* [Object Oriented Programming in Java](https://www.coursera.org/learn/object-oriented-java?specialization=java-object-oriented)
* [javac](https://en.wikipedia.org/wiki/Javac)
* [javac](https://docs.oracle.com/javase/8/docs/technotes/tools/windows/javac.html)
* JAVA Virtual Machine - Jon Meyer & Troy Downing
