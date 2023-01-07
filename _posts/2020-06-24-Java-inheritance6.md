---
title: "Java'da Kalıtım 6 - Sınıf İnşası İçin Derleyici Kuralları"
comments: false
excerpt: "Bu bölümde java'da sınıf inşaası sırasında derleyicinin nasıl çalıştığından ve belli başlı derleyici kurallarından bahsedeceğiz."
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
  - Java Virtual Machine(JVM)
  - Javac
#last_modified_at: 2023-01-06T15:12:19-04:00
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
7. **Java’da Kalıtım 6 - Sınıf İnşası İçin Derleyici Kuralları**
8. Java’da Kalıtım 7 - [Sınıf Hiyerarşisinde Değişken İlklendirme](/java-kalitim-polimorfizm/Java-inheritance7/)
9. Java’da Kalıtım 8 - [Alıştırmalar](/java-kalitim-polimorfizm/Java-inheritance8/)
10. Java’da Kalıtım 9 - [Overriding(Ezici) Metotlar](/java-kalitim-polimorfizm/Java-inheritance9/)
11. Java’da Kalıtım 10 - [Overloading(Aşırı Yükleme) Metotlar](/java-kalitim-polimorfizm/Java-inheritance10/)
</div>

## Genel Bakış

Bir önceki bölümde Java'da nesnelerin içten dışa nasıl inşa edildiğini gördük. Bu bölümde ise bu durumun neden bu şekilde gerçekleştiğine bakacağız.

Hatırlarsanız bir önceki ders, ``Object`` sınıfını ``extends`` etmediğimiz halde java bunu nasıl biliyor? şeklinde bir soru sormuştuk. Bu bölümde bunun cevabını bulmaya çalışalım.

Aslında bu şekilde olmasının nedeni tamamen Java derleyici kurallarından kaynaklanmaktadır. Nesnenin içten dışa oluşturulmasını sağlamak için bu derleyici kurallarının ne olduğu ve nasıl çalıştığı hakkında konuşalım istiyorum.

Öncelikli olarak, bir önceki bölümde ne yaptığımızı ve en son nerede kaldığımız hatırlayalım istiyorum. Elimizde bir Student sınıfı vardı. Bu sınıf Person isimli bir başka sınıfı miras alıyordu. Person sınıfı ise biz belirlemesekte java tarafından Object sınıfını miras almaya maruz bırakılıyordu.

<br/>{% picture 2020-06-23-Java-inheritance5/hierarchy5.png --alt Java class hierarchy or java inheritance tree (java sınıf hiyerarşisi veya java kalıtım ağacı) --img width="100%" height="100%" %}<br/>

Tam da bu noktada, böyle bir planımız olmadığı halde, Object sınıfını neden miras almak zorunda olduğumuzu sormuştuk. Dilerseniz bunun öncesinde java'nın nasıl çalıştığı hakkında bilgi sahibi olalım istiyorum.

## Java Nasıl Çalışır?

Java programlama dilinde, tüm kaynak kodu önce ``.java`` uzantısıyla biten düz metin dosyalarında yazılır. Bu kaynak kodu bir editör aracılığıyla yazdığımız java programlama dilidir ve okunabilir(human-readeble) bir koddur. Yalnız java ile yazılan bir uygulamanın JVM'de çalıştırılabilmesi için önce uygulamanın Java kaynak kodunu JVM'nin tanıdığı bir biçime dönüştürmesi gerekir.

> Bu arada JVM'den kısaca bahsetmek istiyorum. Java Sanal Makinesi, Java programlama dilinin merkezinde yer alır. Aslında, Java Sanal Makinesi uygulamasını çalıştırmadan bir Java programını çalıştıramazsınız. Java Sanal Makinesi, aslında bir Java programını çalıştıran motorun adıdır ve taşınabilirliği, verimliliği ve güvenliği de dahil olmak üzere Java'nın birçok özelliğinin anahtarıdır.


Kaldığımız yerden devam edecek olursak, bu kaynak dosyalar daha sonra ``javac`` derleyicisi(compiler) tarafından ``.class`` dosyalarına derlenir. ``javac`` gibi derleyiciler genellikle Java kaynak kodunu Java sınıfı(yani ``.class``) dosya biçiminde derlemek için kullanılır. Bir ``.class`` dosyası, bilgisayar işlemcinize özgü makine kodu(native code) içermez; bunun yerine bunun yerine ``bytecodes`` içerir. ``bytecode`` Java Sanal Makinesi'nin(JVM) makine dilidir. JVM ise bu ``bytecode`` yorumlar(interpret). Daha sonrasında ``java`` başlatıcı aracı, uygulamanızı Java Virtual Machine örneğiyle çalıştırır.

> javac, Oracle'ın Java Geliştirme Kiti'nde (JDK) bulunan birincil bir Java derleyicisidir(compiler). Derleyici, Java dil şartnamesine (Java language specification-JLS) uygun kaynak kodunu kabul eder ve Java Sanal Makine Şartnamesi'ne (JVMS-Java Virtual Machine Specification) uygun Java bayt kodu üretir.

<br/>{% picture 2020-06-24-Java-inheritance6/getStarted-compiler.png --alt How java compiler works, javac ( java derleyicisi nasıl çalışır, javac) --img width="100%" height="100%" %}<br/>

Çok basit şekliyle javanın çalışma şekli bu şekildedir. Hatta bunu bir editörde(Netbeans,Eclipse vb.) denemek yerine .java uzantılı bir dosya yaratarak deneyebilirsiniz.

## Java Derleyici Kuralları Nedir?

Hatırlarsanız yukarıda, derleyici kurallarından bahsetmiştik. Peki bu karallar nelerdir? Az önce çizdiğimiz şeklin bir başka versiyonunu göstermek istiyorum.

<br/>{% picture 2020-06-24-Java-inheritance6/jvm.png --alt How java works, jvm (java'nın çalışma şekli, jvm) --img width="100%" height="100%" %}<br/>

Yukarıdaki şekilde odaklanmanızı istediğim bölüm aslında derleyicinin kod ekleme şartları kapsamında yaptığı eklemelerdir. Evet derleyici kodumuzu ``bytecode``'a çevirirken belli kurallar çerçevesinde çeşitli komut eklemeleri yapar. Biz burada bütün bu komut eklemelerinden bizim için önemli olan 3 tanesine bakacağız.

Peki, Java derleyicisi ne yapıyor ve bu kurallar nelerdir? Yaptığı şey aslında yazdığımız kodu, jvm'nin anlayacağı şekilde değiştirmektir. Önceki örnek koddan yola çıkarak ``Person`` sınıfı üzerinde bu değişiklikleri göstermeye çalışalım.

### Java Derleyici Kuralı 1

Birinci kural: eğer bir üst sınıfınız yoksa, derleyici size bir tane verecektir. Bu sınıf da daha önce bahsettiğimiz ``Object`` sınıfıdır. Böylelikle Object sınıfının nereden geldiğini anlamış bulunuyoruz.

<br/>{% picture 2020-06-24-Java-inheritance6/rule1.png --alt Java compiler rule(java derleyici kuralı) --img width="100%" height="100%" %}<br/>

``Person`` sınıfının ``Object`` sınıfını miras aldığını artık biliyoruz. Ama bu sınıflara ait "kurucuların" nereden çağrıldığını bilmiyoruz? O zaman şu soruyu sorabiliriz... ``Person()`` ve daha sonra ``Object()`` kurucularını nerede çağırdık? Yani ``Person()`` ya da ``Object()`` kurucusu olarak adlandırdığımız yer neresi?  2.kuralımızın ortaya çıktığı yer de tam olarak burasıdır.

### Java Derleyici Kuralı 2

İkinci kural: eğer bir kurucunuz yoksa, Java derleyicisi size bir tane verecektir. Verilen kurucu varsayılan(default) bir kurucu olacağı için, argüman almaz.

<br/>{% picture 2020-06-24-Java-inheritance6/rule2.png --alt Java compiler rule(java derleyici kuralı) --img width="100%" height="100%" %}<br/>

### Java Derleyici Kuralı 3

Ve sonra tüm kurucularla(constructor) ilişkili başka bir kurallar dizisi uygulanacaktır. Bu kurallar dizisi veya diğer bir deyişle üçüncü kuralımız:

* Kurucunun 1. satırında ya aynı sınıf kurucusunu çağıran bir **this(args<sub>opt</sub>)** ifadesi olacak ya da bir üst sınıf kurucusunu çağıran bir **super(args<sub>opt</sub>)** ifadesi olacaktır.
* Bunların ikisi de yoksa java derleyicisi şu şekilde bir ifade ekler: **super();**

Şekilde görüldüğü gibi Java derleyicisi, ``Person`` sınıfının varsayılan(default) kurucusuna **super()** olarak tanımlanan bir çağrı ekleyecektir.

<br/>{% picture 2020-06-24-Java-inheritance6/rule3.png --alt Java compiler rule(java derleyici kuralı) --img width="100%" height="100%" %}<br/>

## Özet

Soldaki kod bloğu bizim yazdığımız kodu temsil etmektedir. Sağdaki ise derleyicinin bizim yazdıklarımızdan anladığıdır:) Yani **mavi bölümleri** derleyi kendi ekleyecektir.

<br/>{% picture 2020-06-24-Java-inheritance6/rule4.png --alt Java compiler rule(java derleyici kuralı) --img width="100%" height="100%" %}<br/>

### Örnek

Student sınıfının Person sınıfını miras aldığını önceki bölümden biliyoruz. Peki sizce aşağıdaki kodu derleyici nasıl değiştirir?

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

<br/>{% picture 2020-06-24-Java-inheritance6/student.png --alt Java compiler rule(java derleyici kuralı) --img width="100%" height="100%" %}<br/>

Bu süreçleri derleyici sizden bağımsız bir şekilde arka planda gerçekleştirecektir. Ama sürecin nasıl ilerlediğini bilmekte yarar.

Şu ana kadar gerçekleştirdiğimiz şeyler kurucuları alt sınıftan hiyerarşinin üst sınıfı olan Object'e kadar götürmek oldu. Fakat, hatırlarsanız geri dönerken üye değişkenleri ilklendiriyorduk. Henüz üye değişkenlerini ilklendirmedik(initialize). Bir sonraki ders bu konuyu ele alacağız.

Sizce neden derleyicinin kodunuzu derlerken uyguladığı kuralları öğreniyoruz? Çünkü kodda hata ayıklamak için bir noktada bunları bilmeniz gerekebilir. Kodunuzu daha iyi anlamanıza yardımcı olmak ve daha da önemlisi, bu kuralları bilmediğinizde süreci çözümsüzlüğe götürecek hataları ayıklayabilmeniz için bu ayrıntılara dalıyoruz. Örneğin bu kuralları bilmediğinizde, şu şekilde kafa karıştırıcı bir soru aklınızda kalabilir. Ben default bir üst sınıf constructor çağırmadım, neden bu kod yürütülüyor? Bir sonraki ders biraz daha detaya gireceğiz.

<div class="notice--success" markdown="1">
<h4 class="no_toc"><i class="fas fa-lightbulb"></i> Not:</h4>
---
**Not :** Hazırladığım **java'da kalıtım serisini** sıralı takip etmiyorsanız bazı şeyler havada kalacağı için aşağıdaki videoyu izlemenizi öneririm.

Aşağıdaki hazırladığım java eğitim [videosunda](https://www.youtube.com/watch?v=cR9uwtMQt-g), **main** metodunu da kapsayan bir örnek kod üzerinde, statik ve statik olmayan değişken ve metotların hafıza yönetim modelini ele aldım.
</div>



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
