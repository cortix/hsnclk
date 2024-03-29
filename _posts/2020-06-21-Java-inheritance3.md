---
title: "Java'da Kalıtım 3 - Referans ve Nesne Tipleri"
comments: false
excerpt: "Bu bölümde java'da referans ve nesne tiplerini, is-a ilişkisinin ne anlama geldiğini, bunun yanı sıra derleme zamanı ve çalışma zamanı kararları işleyeceğiz."
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
  - Is-a ilişkisi
  - Java referans ve nesne tipleri
  - Derleme ve çalışma zamanı kararları
  - Kalıtımı sağlamak için asgari hedefler
#last_modified_at: 2023-01-07T15:12:19-04:00
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
3. **Java’da Kalıtım 3 - Referans ve Nesne Tipleri**
4. Java’da Kalıtım 3.1 - [Static ve Dinamik Tür](/java-kalitim-polimorfizm/Java-inheritance3_1/)
5. Java’da Kalıtım 4 - [Görünürlük/Erişim Değiştiricileri](/java-kalitim-polimorfizm/Java-inheritance4/)
6. Java’da Kalıtım 5 - [Java’da Nesne Oluşturma](/java-kalitim-polimorfizm/Java-inheritance5/)
7. Java’da Kalıtım 6 - [Sınıf İnşası için Derleyici Kuralları](/java-kalitim-polimorfizm/Java-inheritance6/)
8. Java’da Kalıtım 7 - [Sınıf Hiyerarşisinde Değişken İlklendirme](/java-kalitim-polimorfizm/Java-inheritance7/)
9. Java’da Kalıtım 8 - [Alıştırmalar](/java-kalitim-polimorfizm/Java-inheritance8/)
10. Java’da Kalıtım 9 - [Overriding(Ezici) Metotlar](/java-kalitim-polimorfizm/Java-inheritance9/)
11. Java’da Kalıtım 10 - [Overloading(Aşırı Yükleme) Metotlar](/java-kalitim-polimorfizm/Java-inheritance10/)
</div>

## Genel Bakış

Buradaki amacımız, sınıflar arasındaki "**is-a**" ilişkisinin ne anlama geldiğini anlamak ve bir önceki bölümde yarım bıraktığımız **3. koşulu** yerine getirmek olacaktır.

<div class="notice--success" markdown="1">
<h4 class="no_toc"><i class="fas fa-lightbulb"></i> Kodda tutarlılığı sağlamak ve veri yapısını tek bir sınıfta toplamak için hedefler:</h4>
---
1. [x] Bütün ortak davranışları bir sınıfta tutmak,
2. [x] Farklı davranışa sahip olanları ise farklı sınıflara ayırmak
3. [x] **Tüm bu nesneleri tek bir veri yapısında tutmak.**
</div>

Kodda tutarlılığı sağlamak ve veri yapısını tek bir sınıfta toplamak için ilk iki koşulu, bir önceki bölümde java'da **extends** anahtar kelimesi kullanarak sağlamıştık. Ortak kodları **parent** sınıfta, farklı kodları ise **child** sınıflarda tutarak gerekli koşulları yerine getirmiştik. Şimdi ise 3. koşulu anlamaya çalışalım.


## Referans ve Nesne Türleri

<br/>{% picture 2020-06-21-Java-inheritance3/isa.png --alt java reference and object type, java is-a relationship (java referans ve obje tipi, java is-a ilişkisi) --img width="100%" height="100%" %}<br/>

Referans ve obje tipleri konusuna şu [bölümde](/java-hafiza-yonetimi/Java-memory-models-objects/#referans-ve-nesne-türleri) biraz değinmiştik. Devam etmeden önce göz gezdirmenizde yarar var.

**NOT :** Bu arada referans tipleri, deklare edilen tipler(**declared type**) olarak da tanımlanır.
{: .notice--success}

Yukarıdaki şekili biraz yorumlamaya çalışalım istiyorum. Önceki bölümde belirttiğimiz gibi <u>referans ve nesne türleri de her zaman aynı olmayabilir</u>. Heap alanında oluşan nesnenin tipi ile, aynı nesneyi stack alanında temsil eden değişkenin, yani referansın tipi farklı olabilir. Yukarıdaki örnekte **Person** bir parent(ana) sınıftır. **Student** ve **Faculty** sınıfları ise birer child(çocuk) sınıftır ve Person sınıfını **extends** anahtar kelimesi ile miras almıştır.

Bu sebepten ötürü stack alanında bulunan değişkenin/referansın tipini parent sınıf olan **Person** yapabiliriz. Bir üst sınıf olduğu için stack alanında **Student** ve **Faculty** sınıflarını temsil edebilir. Yalnız önceki bölümde verdiğimiz **Map/HashMap** ilişkisine benzetmeyin. Çünkü **Map** bir soyut sınıftır. **Person** sınıfı her ne kadar bir parent sınıf olsa da, <u>somut bir sınıftır</u>. Somut sınıftan kasıt, **Person** sınıfının istenirse heap alanında somutlaştırabiliyor olmasıdır. Yani **new** anahtar kelimesi vasıtasıyla bu sınıftan bir obje oluşturabiliriz.


## IS-A İlişkisi

**Is-a**'nin kelime anlamı doğrudan yoktur ama bildiğiniz gibi **is** yardımcı bir fiildir. İngilizce okunuşuyla **a** ise <u>bir anlamındadır</u>.

* A Person **is a** Person : Bir kişi bir kişidir.
* A Student **is a** Student : Bir öğrenci bir öğrencidir.
* A Student **is a** Person : Bir öğrenci bir bireydir.
* A Person **is a** Student : Bir birey bir öğrencidir.

**Not:** person'ı bazen kişi bazen de birey çevirebilirim. Lütfen buna takılmayın.
{: .notice--warning}

İlk üçü okunduğundan kulağa gayet mantıklı gelmektedir. <u>Fakat sonuncusunda mantıksal bir gariplik vardır</u>. Gerçekten de her bir birey bir öğrenci midir???? Öğretim görevlisi de olabilir. Bu sebepten ötürü sonuncu mantıksal olarak yanlıştır. Yani ``Student s = new Person();`` doğru bir **is-a** ilişkisine sahip değildir. Bu cümleyi şu şekilde de okuyabilirsiniz. **new** anahtar kelimesi ile somutlaşan her bir **kişi(Person)** aslında bir **öğrenci(Student)** midir? <u>Tabii ki değildir</u>.

Doğru ifade şekli tabii ki ``Person p = new Student();`` şeklinde olur. Veyahut ``Person p = new Faculty();`` şeklinde!!!

O halde somutlaştırdığımız her bir **Faculty** ve **Student** sınıfı nesnelerini, stack alanında **Person** sınıfını kullanarak temsil edebilirim.

``` java
Person[] p= new Person[3];
p[0] = new Person();
p[1] = new Student();
p[2] = new Faculty();
```

Yani bir **Person** dizisi hem **Student** hem de **Faculty** objelerini saklayabilir. Yazımın başında bahsettiğim son şartı da bu şekilde sağlamış olduk. Yani **tüm bu nesneleri tek bir veri yapısında tutmaktan** bahsediyorum. Hem **Student**, hem **Faculty** hem de **Person** objelerini tek bir veri yapısı olan **Person** veri yapısında(data structure) saklamış olduk.

### Java'da IS-A İlişkisine Örnek

Şu ana kadar öğrendiklerimizi uygulayacak olursak, aşağıdaki kod çalıştırıldığında nerelerde hata alırız? Bir editörde çalıştırmadan, yani bakarak çözmeye çalışın istiyorum.

``` java
public class Person {
    private String name;
    public String getName() {return name;}
}
```

``` java
public class Student extends Person {
    private int id;
    public int getID() {return id;}
}
```

``` java
public class Faculty extends Person {
    private String id;
    public String getID() {return id;}
}
```

``` java
public class TestFaculty {
    public static void main(String[] args) {
      Student s = new Student();
      Person p = new Person();
      Person q = new Person();
      Faculty f = new Faculty();
      Object o = new Faculty();

      String n = s.getName();
      p = s;
      int m = p.getID();
      f = q;
      o = s;


    }
}
```

* Önce hata almadıklarımızdan başlayalım istiyorum. ``s`` değişkeninin(referansının) tipi **Student** sınıfıdır. **Student** sınıfı normalde ``getName()`` yöntemine sahip değildir fakat **Person** sınıfını miras aldığı için ``getName()`` metoduna ulaşabilir.

* ``p = s;`` atama işlemini ele alacak olursak; ``p`` değişkeninin(referansının) tipi **Person** sınıfıdır. ``s`` değişkenini(referansını) zaten az önce ele almıştık. Burada yapılmak istenen şey, ``p`` değişkeninin(referansının) heap alanında temsil ettiği objeyi, ``s`` değişkeninin(referansının) heap alanında temsil ettiği obje ile değiştirmektir. Yani ``p`` referansı artık ``s`` referansının işaret ettiği objeyi işaret edecektir. Peki bu mümkün müdür? Hemen bakalım. Yapılmak istenen özünde şöyle bir işlemdir. ``Person p = new Student();`` Yani bir **is-a** ilişkisi vardır. **A Student is a Person**.... Bir öğrenci bir kişi midir? Cevap: Evet. Her bir öğrenci aslında bir kişidir. Bu yüzden bu atamada bir sakınca yoktur.(Bu arada okumayı soldan sağa yapmadığımızı farkettiğinizi umuyorum. Cümleye `new` anahtar kelimesinin olduğu yerden başladım. Yani ``new Student()`` bir Person mıdır? Kafanız karıştıysa sorabilirsiniz)


* `int m = p.getID();` hata alırız, peki neden? Bildiğimiz gibi ``p`` değişkeninin tipi **Person** sınıfıdır. **Person** sınıfının da böyle bir metodu olmadığından hata mesajı alırız. Ama şunu söyleyebilirsiniz. İyi de biz bir önceki satırda ``p`` referansını/değişkenini artık heap alanında ``s`` referansının işaret ettiği **Student** objesine atamıştık. Artık sahip olması lazım!!!! diye düşünebilirsiniz. Evet tam olarak bunu yaptık. Fakat derleyici bunu bilmiyor. Derleyici bunu bir **Person** nesnesi olarak görüyor çünkü başlangıçta bu şekilde ayarlanmış. Derleyici **p**'nin bir **Student** referası olduğunu ancak çalışma zamanında(run-time) görecektir. <u>Çünkü <b>referanslar</b> ile ilgili kararlar derleme zamanında, <b>objeler/nesneler</b> ile ilgili kararlar ise çalışma zamanında alınır. Bu yüzden derleyici çalışma zamanına geçmeden <b>obje tiplerini</b> bilmeyecektir.</u>

  Programı daha çalıştırmadığımız için, derleme sırasında hata almış olacağız. Haliyle java **p** referasını hâlen bir **Person** olarak düşünecektir. Alınan hata şu şekildedir. **Cannot resolve method 'getID' in 'Person'.** Aslında derleme zamanında bu hata cast işlemi yapılarak çözülebilir. ``int m = ((Student) p).getID();`` yaptığımızda derleme hatasını bir bakıma engellemiş oluruz. Ama amacımız burada hatanın ne olduğunu göstermek, çözüm yolu aramak değil. Yani Hemen altta derleme zamanı ve çalışma zamanı kararları başlığı altında bu konuya biraz değineceğiz.

  <div class="notice--success" markdown="1">
  <h4 class="no_toc"><i class="fas fa-lightbulb"></i> Not:</h4>
  ---
  Yalnız şunu da belirtmekte yarar var. Peki **p** referansı çalışma zamanına(runtime) geçtiğinde bu metodu görebilecek mi? Cevap hayır..

  Çünkü bir obje heap alanında oluşturulduğunda <u>hem nesne tipinin instance değişkenlerini hem de parent sınıflarının instance değişkenlerini saklar</u>.

  Fakat parent **sınıfın tipiyle stack'den heap'teki bu objeye erişmek istediğimizde**, stack'teki bu referans sadece kendi instance metot ve değişkenlerini ve parent sınıflarının instance metot ve değişkenlerini görür.

  Yani kendi üstündeki parent sınıfların bütün özelliklerini görür ama altındakileri göremez. Her ne kadar çalışma zamanında buradaki bir durum olsa da.. İllaki görmek istiyorsanız cast işlemini uygulamanız gerekmektedir. (Aslında bunu da bir şekil üzerinde anlatmam gerekiyor.) Her neyse bu konuya şimdilik burada bir nokta koymak istiyorum.
  </div>
* ``f = q;`` Şayet tekraradan **is-a** ilişkisinden yola çıkacak olursak, ``f`` değişkeni/referansı stack alanında **Faculty** tipinde bulunmaktadır. Heap alanında ise yine **Faculty** tipinde bir nesneye sahiptir. Ama burada ``f`` değişkeni, ``q`` değişkeninin referans aldığı nesneye eşitlenmek isteniyor. Bu arada ``q`` değişkeninin/referansının tipi **Person** sınıfıdır. Peki bu eşitleme mümkün mü? Yapılmak istenen özünde tam olarak şudur.. ``Faculty f = new Person();`` **is-a** ilişkisindeki karşılığı **A Person is a Faculty.**. Yani her bir kişi bir öğretim üyesi midir? Tabii ki hayır. Mantıksal açıdan bu zaten mümkün değildir. Ama şunu da belirtmek isterim. Burada bir derleme hatası alıyoruz. Alınan hata tam olarak şudur. **Error: java: incompatible types: Person cannot be converted to Faculty.** Yani java ilk olarak, çalışma zamanında heap alanında olacaklardan çok, derleme zamanında var olanlara bakar. Burada da bir **Faculty** ile bir **Person** sınıfının atama işlemi vardır. Tam tersi olsaydı, yani ``Person f = new  Faculty();`` her bir öğretim üyesi bir kişidir diyebilirdik ama bu şekilde bir atama burada söz konusu değildir.

* ``o = s;`` atama işlemini ele alacak olursak; ``o`` değişkeninin tipi ``Object`` sınıfıdır. ``s`` değişkenini zaten az önce ele almıştık. Şu bilgiyi sanırım herkes vakıftır. Java'da her nesne bir objedir. Yani her nesne ``Object`` sınıfını miras alır. Yani ``Object`` sınıfını dolaylı olarak miras alır(**extends** eder). **is-a** ilişkisinden yola çıkacak olursak, ``Object o = new Student();``  yani her bir öğrenci bir objedir diyebiliriz.

Sonuç olarak aşağıdaki iki atama işlemi derleme zamanı hatası alır.

``` java
f = q;
int m = p.getID();
```
Aşağıda kodu bu [linke](http://pythontutor.com/java.html#code=public%20class%20TestFaculty%20%7B%0A%20%20%20%20public%20static%20void%20main%28String%5B%5D%20args%29%20%7B%0A%20%20%20%20%20%20Student%20s%20%3D%20new%20Student%28%29%3B%0A%20%20%20%20%20%20Person%20p%20%3D%20new%20Person%28%29%3B%0A%20%20%20%20%20%20Person%20q%20%3D%20new%20Person%28%29%3B%0A%20%20%20%20%20%20Faculty%20f%20%3D%20new%20Faculty%28%29%3B%0A%20%20%20%20%20%20Object%20o%20%3D%20new%20Faculty%28%29%3B%0A%0A%20%20%20%20%20%20String%20n%20%3D%20s.getName%28%29%3B%0A%20%20%20%20%20%20%20%20%20%20p%20%3D%20s%3B%0A%20%20%20%20%20%20%20%20%20%20//int%20m%20%3D%20p.getID%28%29%3B%0A%20%20%20%20%20%20%20%20%20%20//f%20%3D%20q%3B%0A%20%20%20%20%20%20%20%20%20%20o%20%3D%20s%3B%0A%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%0A%0A%20%20%20%20class%20Person%20%7B%0A%20%20%20%20%20%20%20%20private%20String%20name%3B%0A%20%20%20%20%20%20%20%20public%20String%20getName%28%29%20%7Breturn%20name%3B%7D%0A%20%20%20%20%7D%0A%0A%20%20%20%20class%20Student%20extends%20Person%20%7B%0A%20%20%20%20%20%20%20%20private%20int%20id%3B%0A%20%20%20%20%20%20%20%20public%20int%20getID%28%29%20%7Breturn%20id%3B%7D%0A%20%20%20%20%7D%0A%0A%20%20%20%20class%20Faculty%20extends%20Person%20%7B%0A%20%20%20%20%20%20%20%20private%20String%20id%3B%0A%20%20%20%20%20%20%20%20public%20String%20getID%28%29%20%7Breturn%20id%3B%7D%0A%20%20%7D&cumulative=false&heapPrimitives=nevernest&mode=edit&origin=opt-frontend.js&py=java&rawInputLstJSON=%5B%5D&textReferences=false) ulaşarak da görüntüleyebilirsiniz. Derleme hatası aldığımız yerleri yorum satırı içine aldığımı farkedeceksiniz. Yorum satırını kaldırıp siz de deneyebilirsiniz.

```java
public class TestFaculty {
    public static void main(String[] args) {
      Student s = new Student();
      Person p = new Person();
      Person q = new Person();
      Faculty f = new Faculty();
      Object o = new Faculty();

      String n = s.getName();
	      p = s;
	      //int m = p.getID();
	      //f = q;
	      o = s;
	    }
	}

	class Person {
	    private String name;
	    public String getName() {return name;}
	}

	class Student extends Person {
	    private int id;
	    public int getID() {return id;}
	}

	class Faculty extends Person {
	    private String id;
	    public String getID() {return id;}
	}
```


## Derleme Zamanı ve Çalışma Zamanı Kararları

<br/>{% picture 2020-06-21-Java-inheritance3/compile-runtime.png --alt java reference and object type, java compile time decisions and runtime decisions (java referans ve obje tipi, java derleme zamanı ve çalışma zamanı kararları) --img width="100%" height="100%" %}<br/><br/><br/>

Stack ve heap alanında görselleştirdiklerimizden anlaşılacağı üzere referanslar ön tarafta gerçekleşen işlemleri, objeler ise arka tarafta gerçekleşen işlemleri temsil etmektedir. Ön taraf için derleme zamanı, arka taraf, yani merkezde olan işlemler için ise çalışma zamanını düşünebilirsiniz.


``` java
public class SampleTest {

    public SampleTest(){
    }
    public int calculate(int m, int z){
        return m+z;
    }
    // ....
}
```

``` java
public class Sample {
    public static void main(String[] args) {
        SampleTest o = new SampleTest();
        o.calculate(1,2);
        Object o = new SampleTest();
        o.calculate(1,2);
    }
}
```
Yukarıdaki koda baktığımızda görünüşte bir sıkıntının olmadığını söyleyebilirsiniz. 3.satırda  ``SampleTest`` tipinde bir nesne yaratılıyor ve bu nesnenin ``calculate()`` metodu çağırılarak işlem yapılıyor. Buraya kadar her şey normal fakat 5.satırda bir fark var. Burada referans tipi olarak ``Object`` sınıfı tercih edilmiş. Aslında gayet normal diyebilirsiniz. Çünkü şunu biliyoruz ki java'da her şey bir nesnedir ve ``Object`` sınıfını miras alır. Bu yüzden de bu satırda herhangi bir derleme hatası almayız. Asıl sorun 6.satırda gerçekleşecektir. Çünkü Java derleme zamanında referans tiplerine bakar. Referans tipimiz olan ``Object`` sınıfının da ``calculate()`` isminde bir metodu olmadığı için derleme hatası(compile time error) alırız. Fakat yukarıda da değindiğimiz gibi çalışma zamanına geçtiğimizde de bu metot(calculate) yine **Object o** tarafından görünür olmayacaktı. Görünür kılmak için cast işlemi uygulamak şarttır.

İleriki konularda referans türüne göre derleme zamanında hangi kararların alındığı ve nesne türüne göre çalışma zamanında hangi kararların alındığı hakkında konuşacağız. Ama öncesinde bu konuyla çok bağlantılı olan bir konuyu ele alacağız. Statik ve dinamik türler nelerdir? Bundan sonraki bölümde bu konuyu ele almak istiyorum.

## Referanslar
* [Is-a](https://en.wikipedia.org/wiki/Is-a)
* [Inheritance (IS-A) vs. Composition (HAS-A) Relationship](https://www.w3resource.com/java-tutorial/inheritance-composition-relationship.php)
* [Inheritance and Composition (Is-a vs Has-a relationship) in Java](https://www.baeldung.com/java-inheritance-composition)
* [Generics, Inheritance, and Subtypes](https://docs.oracle.com/javase/tutorial/java/generics/inheritance.html)
* [Object Oriented Programming in Java](https://www.coursera.org/learn/object-oriented-java?specialization=java-object-oriented)
