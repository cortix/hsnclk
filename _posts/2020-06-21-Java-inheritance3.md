---
title: "Java'da Kalıtım 3 - Referans ve Nesne Tipleri"
comments: true
excerpt: "Bu derste Java'daki Referans ve Nesne Tipleri ele alacağız. Aynı zamanda is-a ilişkisinin ne anlama geldiğini yine bu derste işleyeceğiz. Bunun yanı sıra, derleme zamanı ve çalışma zamanı kararları alınırken nelerin dikkate alındığından ve bir önceki derste yarım kalan, kalıtımı sağlamak için asgari hedefler nelerdir? başlıklı sorumuzun son maddesinden de bahsedeceğiz."
header:
  teaser: "assets/images/equality.png"
  og_image: /assets/images/page-header-og-image.png
  overlay_image: /assets/images/unsplash-image-45.jpg
  overlay_filter: 0.5 #rgba(255, 0, 0, 0.5)
  caption: "Photo by [Ricardo Gomez Angel](https://unsplash.com/photos/52NJbPMuGuU) on Unsplash"
  #cta_label: "More Info"
  #cta_url: "https://unsplash.com"
categories:
  - java-kalitim-polimorfizm
tags:
  - is-a ilişkisi
  - referans ve nesne tipleri
  - derleme ve çalışma zamanı kararları
  - kalıtımı sağlamak için asgari hedefler
last_modified_at: 2020-06-06T15:12:19-04:00
toc: true
toc_label: "SAYFA İÇERİĞİ"
---

**ÖNEMLİ :** Kendim için aldığım notlar. Umarım size de bir faydası olur. Kullanılan her bir makale referans olarak eklenmiştir.
{: .notice}

## Genel Bakış

Buradaki amacımız, sınıflar arasındaki "is a" ilişkisinin ne anlama geldiğini anlamak ve bir önceki derste yarım bıraktığımız 3. koşulu yerine getirmek olacaktır.

1. Bütün ortak davranışları bir sınıfta tutmak,
2. Farklı davranışa sahip olanları ise farklı sınıflara ayırmak
3. **Tüm bu nesneleri tek bir veri yapısında tutmak.**

Bir önceki ders ilk iki koşulu extends anahtar kelimesini kullanarak sağlamıştık. Ortak kodları parent sınıfta, farklı kodları ise child sınıflarda tutarak gerekli koşulları sağlamıştık. Şimdi ise 3. koşulu anlamaya çalışalım.


## Referans ve Nesne Türleri

<figure style="width: 600px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-06-21-Java-inheritance3/isa.png" alt="uml diagram">
  <figcaption></figcaption>
</figure>

Referans ve obje tipleri konusuna şu [derste](/java-hafiza-yonetimi/Java-memory-models-objects/#referans-ve-nesne-türleri) biraz değinmiştik. Devam etmeden önce göz gezdirmenizde yarar var.

Yukarıdaki şekili biraz yorumlamaya çalışalım istiyorum. Önceki derste belirttiğimiz gibi referans ve nesne türleri de her zaman aynı olmayabilir. Heap alanında oluşan nesnenin tipi ile, aynı nesneyi stack alanında temsil eden değişkenin, yani referansın tipi farklı olabilir. Yukarıdaki örnekte ``Person`` bir parent(ana) sınıftır. ``Student`` ve ``Faculty`` sınıfları ise birer child(çocuk) sınıftır ve Person sınıfını ``extends`` anahtar kelimesi ile miras almıştır. Bu sebepten ötürü stack alanında bulunan değişkenin/referansın tipini parent sınıf olan ``Person`` yapabiliriz. Bir üst sınıf olduğu için stack alanında ``Student`` ve ``Faculty`` sınıflarını temsil edebilir. Yalnız önceki derste verdiğimiz ``Map/HashMap`` ilişkisine benzetmeyin. Çünkü ``Map`` bir soyut sınıftır. ``Person`` sınıfı her ne kadar bir parent sınıf olsa da, somut bir sınıftır. ``Person`` sınıfını bu yüzden heap alanında somutlaştırabiliriz.


## IS-A İlişkisi

Is-a kelime anlamı doğrudan yoktur ama bildiğiniz gibi yardımcı bir fiildir. A'de bir anlamındadır.

* A Person **is a** Person : Bir kişi bir kişidir.
* A Student **is a** Student : Bir öğrenci bir öğrencidir.
* A Student **is a** Person : Bir öğrenci bir bireydir.
* A Person **is a** Student : Bir birey bir öğrencidir.

İlk üçü okunduğundan kulağa gayet mantıklı gelmektedir. Fakat sonuncusunda bir gariplik vardır. Çünkü her bir birey bir öğrenci midir???? Öğretim görevlisi de olabilir. Bu sebepten ötürü sonuncu ilişki yanlıştır. Yani ``Student s = new Person();`` doğru bir ilişkiye sahip değildir. Bu cümleyi şu şekilde de okuyabilirsiniz. new anahtar kelimesi ile somutlaşan her bir kişi(Person) aslında bir öğrenci(Student) midir? Tabii ki değildir. Doğru ifade şekli tabii ki ``Person p = new Student();`` şeklinde olur. Veyahut ``Person p = new Faculty();`` şeklinde!!!

O halde somutlaştırdığımız her bir Faculty ve Student sınıfı nesnelerini, stack alanında Person sınıfını kullanarak temsil edebilirim.

``` java
Person[] p= new Person[3];
p[0] = new Person();
p[1] = new Student();
p[2] = new Faculty();
```

Yani bir person dizisi hem Student hem de Faculty objelerini saklayabilir. Dersin başında bahsettiğimiz son şartı da bu şekilde sağlamış olduk. Yani **tüm bu nesneleri tek bir veri yapısında tutmaktan** bahsediyorum. Hem Student, hem Faculty hem de Person objelerini tek bir veri yapısı olan Person veri yapısında saklamış olduk.

### Örnek

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

* Önce hata alınmayanlardan başlayalım istiyorum. ``s`` değişkeninin tipi ``Student`` sınıfıdır. ``Student`` sınıfı normalde ``getName()`` yöntemine sahip değildir fakat ``Person`` sınıfını miras aldığı için ``getName()`` metoduna ulaşabilir.

* ``p = s;`` atama işlemini ele alacak olursak; ``p`` değişkeninin tipi ``Person`` sınıfıdır. ``s`` değişkenini zaten az önce ele almıştık. Burada yapılmak istenen şey, ``p`` değişkeninin heap alanında temsil ettiği objenin referansını, ``s`` değişkeninin heap alanında temsil ettiği objenin referansı ile değiştirmektir. Yani ``p`` referansı artık ``s`` değişkeninin işaret ettiği objeyi işaret edecektir. Peki bu mümkün müdür? Hemen bakalım. Yapılmak istenen özünde şöyle bir işlemdir. ``Person p = new Student();`` Yani bir **is-a** ilişkisi vardır. **A Student is a Person**.... Bir öğrenci bir kişi midir? Cevap: Evet. Her bir öğrenci aslında bir kişidir. Bu yüzden bu atamada bir sakınca yoktur.


* int m = p.getID(); hata alırız, peki neden? Bildiğimiz gibi ``p`` değişkeninin tipi ``Person`` sınıfıdır. Person sınıfının da böyle bir metodu olmadığından hata mesajı alırız. Ama şunu söyleyebilirsiniz. Bir önceki satırda p referansını/değişkenini artık heap alanında s referansının işaret ettiği Student objesine atamıştık. Evet tam olarak bunu yaptık. Fakat derleyici bunu bilmiyor. Derleyici bunu bir Person nesnesi olarak görüyor çünkü başlangıçta bu şekilde ayarlanmış. Derleyici p'nin bir Student referası olduğunu ancak çalışma zamanında(run-time) görecektir. <u><i>Çünkü referanslar ile ilgili kararlar derleme zamanında, objeler/nesneler ile ilgili kararlar ise çalışma zamanında alınır. Bu yüzden derleyici çalışma zamanına geçmeden obje tiplerini bilmeyecektir.</i></u> Programı daha çalıştırmadığımız için, derleme sırasında hata almış olacağız. Haliyle java p referasını halen bir Person olarak düşünecektir. Alınan hata şu şekildedir. **Cannot resolve method 'getID' in 'Person'.** Aslında derleme zamanında bu hata cast işlemi yapılarak çözülebilir. ``int m = ((Student) p).getID();`` yaptığımızda derleme hatasını bir bakıma engellemiş oluruz. Ama amacımız burada hatanın ne olduğunu göstermek, çözüm yolu aramak değil. Yani Hemen altta derleme zamanı ve çalışma zamanı kararları başlığı altında bu konuya biraz değineceğiz.

* ``f = q;`` Şayet tekraradan **is-a** ilişkisinden yola çıkacak olursak, ``f`` değişkeni/referansı stack alanında ``Faculty`` tipinde bulunmaktadır. Heap alanında ise yine ``Faculty`` tipinde bir nesneye sahiptir. Ama burada ``f`` değişkeni, ``q`` değişkeninin referans aldığı nesneye eşitlenmek isteniyor. Bu arada ``q`` değişkeninin/referansının tipi ``Person`` sınıfıdır. Peki bu eşitleme mümkün mü? Yapılmak istenen özünde tam olarak şudur.. ``Faculty f = new Person();`` **is-a** ilişkisindeki karşılığı **A Person is a Faculty.**. Yani her bir kişi bir öğretim üyesi midir? Tabii ki hayır. Mantıksal açıdan bu zaten mümkün değildir. Ama şunu da belirtmek isterim. Burada bir derleme hatası alıyoruz. Alınan hata tam olarak şudur. **Error: java: incompatible types: Person cannot be converted to Faculty.** Yani java ilk olarak, çalışma zamanında heap alanında olacaklardan çok, derleme zamanında var olanlara bakar. Burada da bir ``Faculty`` ile bir ``Person`` sınıfının atama işlemi vardır. Tam tersi olsaydı, yani ``Person f = new  Faculty();`` her bir öğretim üyesi bir kişidir diyebilirdik ama bu şekilde bir atama burada söz konusu değildir.

* ``o = s;`` atama işlemini ele alacak olursak; ``o`` değişkeninin tipi ``Object`` sınıfıdır. ``s`` değişkenini zaten az önce ele almıştık. Şu bilgiyi sanırım herkes vakıftır. Java'da her nesne bir objedir. Yani her nesne ``Object`` sınıfını miras alır. Yani ``Object`` sınıfını ``extends`` eder. **is-a** ilişkisinden yola çıkacak olursak, ``Object o = new Student();``  her bir öğrenci bir objedir diyebilir.

Sonuç olarak aşağıdaki iki atama işlemi derleme zamanı hatası alır.

``` java
f = q;
int m = p.getID();
```
Aşağıda deneyerek görebilirsiniz. Derleme hatası aldığımız yerleri yorum satırı içine aldığımı farkedeceksiniz. Yorum satırını kaldırıp deneyebilirsiniz.

<iframe width="800" height="500" frameborder="0" src="http://pythontutor.com/iframe-embed.html#code=public%20class%20TestFaculty%20%7B%0A%20%20%20%20public%20static%20void%20main%28String%5B%5D%20args%29%20%7B%0A%20%20%20%20%20%20Student%20s%20%3D%20new%20Student%28%29%3B%0A%20%20%20%20%20%20Person%20p%20%3D%20new%20Person%28%29%3B%0A%20%20%20%20%20%20Person%20q%20%3D%20new%20Person%28%29%3B%0A%20%20%20%20%20%20Faculty%20f%20%3D%20new%20Faculty%28%29%3B%0A%20%20%20%20%20%20Object%20o%20%3D%20new%20Faculty%28%29%3B%0A%20%20%20%20%20%20%0A%20%20%20%20%20%20String%20n%20%3D%20s.getName%28%29%3B%0A%20%20%20%20%20%20p%20%3D%20s%3B%0A%20%20%20%20%20%20//int%20m%20%3D%20p.getID%28%29%3B%20%0A%20%20%20%20%20%20//f%20%3D%20q%3B%20%0A%20%20%20%20%20%20o%20%3D%20s%3B%0A%20%20%20%20%7D%0A%7D%0A%0Aclass%20Person%20%7B%0A%20%20%20%20private%20String%20name%3B%0A%20%20%20%20public%20String%20getName%28%29%20%7Breturn%20name%3B%7D%0A%7D%0A%0Aclass%20Student%20extends%20Person%20%7B%0A%20%20%20%20private%20int%20id%3B%0A%20%20%20%20public%20int%20getID%28%29%20%7Breturn%20id%3B%7D%0A%7D%0A%0Aclass%20Faculty%20extends%20Person%20%7B%0A%20%20%20%20private%20String%20id%3B%0A%20%20%20%20public%20String%20getID%28%29%20%7Breturn%20id%3B%7D%0A%7D&codeDivHeight=400&codeDivWidth=350&cumulative=false&curInstr=0&heapPrimitives=nevernest&origin=opt-frontend.js&py=java&rawInputLstJSON=%5B%5D&textReferences=false"> </iframe>

## Derleme Zamanı ve Çalışma Zamanı Kararları

<figure style="width: 600px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-06-21-Java-inheritance3/compile-runtime.png" alt="uml diagram">
  <figcaption></figcaption>
</figure>

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
Yukarıdaki koda baktığımızda görünüşte bir sıkıntının olmadığını söyleyebilirsiniz. 3.satırda  ``SampleTest`` tipinde bir nesne yaratılıyor ve bu nesnenin ``calculate()`` metodu çağırılarak işlem yapılıyor. Buraya kadar her şey normal fakat 5.satırda bir fark var. Burada referans tipi olarak ``Object`` sınıfı tercih edilmiş. Aslında gayet normal diyebilirsiniz. Çünkü şunu biliyoruz ki java'da her şey bir nesnedir ve ``Object`` sınıfını miras alır. Bu yüzden de bu satırda herhangi bir derleme hatası almayız. Asıl sorun 6.satırda gerçekleşecektir. Çünkü Java derleme zamanında referans tiplerine bakar. Referans tipimiz olan ``Object`` sınıfının da ``calculate()`` isminde bir metodu olmadığı için derleme hatası(compile time error) alırız. Özetle java, çalışma zamanına(run-time) geçmeden heap alanında hangi nesneyi referans aldığını bilemez.

İleriki konularda referans türüne göre derleme zamanında hangi kararların alındığı ve nesne türüne göre çalışma zamanında hangi kararların alındığı hakkında konuşacağız. Ama öncesinde bu konuyla çok bağlantılı olan bir konuyu ele alacağız. Statik ve dinamik türler nelerdir? Bundan sonraki derste bu konuyu ele almak istiyorum.

## Referanslar
* [https://en.wikipedia.org/wiki/Is-a](https://en.wikipedia.org/wiki/Is-a)
* [https://www.w3resource.com/java-tutorial/inheritance-composition-relationship.php](https://www.w3resource.com/java-tutorial/inheritance-composition-relationship.php)
* [https://www.baeldung.com/java-inheritance-composition](https://www.baeldung.com/java-inheritance-composition)
* [https://docs.oracle.com/javase/tutorial/java/generics/inheritance.html](https://docs.oracle.com/javase/tutorial/java/generics/inheritance.html)
* [https://www.coursera.org/learn/object-oriented-java/lecture/h75Dw/core-reference-vs-object-type](https://www.coursera.org/learn/object-oriented-java/lecture/h75Dw/core-reference-vs-object-type)
