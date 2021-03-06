---
title: "Java'da Hafıza Modeli 5 - Pass By Value / Pass By Reference"
comments: true
excerpt: "Aslında bu bölümde anlatılacaklar şu ana kadar öğrendiklerimizin bir tekrarı gibi düşünülebilir. Yalnız bu bölümde bu öğrendiklerimizi farklı programlama dilleri ile karşılaştırarak değerlendireceğiz. Metotlara parametre geçirilirken kullanılan 2 farklı yaklaşımı ele alacağız. Passing By Value/Passing By Reference(Değer veya Referans İle Parametre Geçirmek)"
header:
  teaser: "assets/images/equality.png"
  og_image: /assets/images/page-header-og-image.png
  overlay_image: /assets/images/unsplash-image-33.jpg
  overlay_filter: 0.5 #rgba(255, 0, 0, 0.5)
  caption: "Photo by [ian dooley](https://unsplash.com/photos/TevqnfbI0Zc) on Unsplash"
  #cta_label: "More Info"
  #cta_url: "https://unsplash.com"
categories:
  - java-hafiza-yonetimi
tags:
  - pass by value
  - pass by reference
last_modified_at: 2020-06-06T15:12:19-04:00
toc: true
toc_label: "SAYFA İÇERİĞİ"
---

**ÖNEMLİ :** Kendim için aldığım notlar. Umarım size de bir faydası olur. Kullanılan her bir makale referans olarak eklenmiştir.
{: .notice}

## Değişken Geçirme(Passing a Variable)

**"Değişken geçirme"** terimi, bir metot daha önce tanımladığınız bir değişkenle çağrıldığında kullanılır.

```java
int a = 2;
calTotal(a);
```
**a** değişkeni ``calTotal`` yönteminde kullanılmak üzere, bu yönteme geçirilmiştir. Bu yöntemin de zaten bir ``int`` değeri alacak şekilde önceden tanımlandığını anlıyoruz.

Programlama dilleri metotlara parametre aktarılırken 2 farklı yaklaşım kullanır. **Pass by value(değere göre geçirme)** ve **pass by reference(referansa göre geçirme)** yaklaşımları değişkenlerin metotlara nasıl aktarıldığını tanımlamak için kullanılan 2 farklı tekniktir. Kısaca izah edecek olursak, **değere göre geçişte**, metoda gerçek değerin geçirildiği anlamına gelir. Referansla geçişte, değerin nerede saklandığını tanımlayan bir işaretçinin (bu, geçirilen değişkenin hafızadaki adresi olarak düşünülebilir) geçirildiği anlamına gelir.

Önceki derslerde ilkel ve ilkel olmayan tiplerin hafızada nerelerde ve nasıl saklandığını zaten bildiğinizi varsayarak, hibrit bir hafıza şekli ile bu durumu anlatmak istiyorum. Buradaki amaç heap ve stack gibi hafıza birimlerine etraflıca girmeden konuyu daha genel bir çerçeveden ele almaktır.

<figure style="width: 200px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-03-01-Java-memory-models-pass-by-value-reference/1.png" alt="how memory works">
  <figcaption>Kaynak:penjee.com</figcaption>
</figure>

Basitleştirmek için, hafızayı yan yana sıralanmış bloklar olarak düşünebilirsiniz. Ve her bir bloğun da veri saklayan bir alanı temsil ettiğini hayal edin. **Gri rakamlar** herbir bloğun hafızadaki adresini, **mavi** ve **kırmızı** rakamlar ise bu hafıza bloklarında saklanan gerçek değerleri temsil etmektedir.

## Pass by Value (C ve C++ dillerinde)

*"Pass by Value"* yaklaşımı uygulandığında, metotun içine aldığı parametrenin değeri, belleğin başka bir yerine kopyalanır. Şayet metodun değişkenine erişmek veyahut bu değişkeni değiştirmek isterseniz, yalnızca kopyaya erişilir/değiştirilir, orijinal değere dokunulmaz. Aşağıdaki örnekte ``myAge`` değişkeninin orjinal değeri **106** nolu blokta saklanmaktadır ve bu değer **14**'tür. Bu değerin ``calBirthYear`` metoduna geçen kopyasının değeri ise **152** nolu blokta tutulmaktadır.

```java
int myAge = 14;
calBirthYear(myAge);
```

<figure style="width: 400px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-03-01-Java-memory-models-pass-by-value-reference/2.png" alt="pass by value">
  <figcaption>Kaynak:penjee.com</figcaption>
</figure>

Şayet bir işlem yapılmak istendiğinde gerekli işlem orjinal değere değil, kopyalanan değere uygulanır. Aslında bunun izahını bir önceki bölüm olan scope(kapsam) konusunda farklı bir şekilde ele almıştık.

Yukarıdaki örneği daha açık hale getirmek için, metot içindeki değişken bu örnekte **age** olarak adlandırılmıştır. Yani **152** nolu blokta yer alan **age** değişkeni aslında **myAge** değişkeninden klonlanan değerinin ta kendisidir. Bütün işlemler **age** üzerinde gerçekleşecektir. ``myAge`` değişkeni bu işlemlerden etkilenmez çünkü **myAge** ``calBirthYear`` metodunun kapsamı(scope) dışındadır. Aşağıdaki c++ programlama dilinde hazırlanmıştır. Kodun bütün halini şu [linkte](http://www.pythontutor.com/cpp.html#code=int%20CURRENT_YEAR%20%3D%202020%3B%0Aint%20calBirthYear%28int%20yourAge%29%20%7B%0A%20%20%20%20int%20birthYear%20%3D%20CURRENT_YEAR%20-%20yourAge%3B%0A%20%20%20%20return%20birthYear%3B%0A%7D%0Aint%20main%28%29%20%7B%0A%20%20int%20myAge%20%3D%2014%3B%0A%20%20int%20res%20%3D%20calBirthYear%28myAge%29%3B%0A%20%20return%20res%3B%0A%7D&curInstr=8&mode=display&origin=opt-frontend.js&py=cpp&rawInputLstJSON=%5B%5D) görebilirsiniz.

```java
int calBirthYear(int age) {
	int birthYear = CURRENT_YEAR – age;
	return birthYear;
}
```

Peki, *pass by value* yaklaşımı uygulandığında, metot kapsamı dışındaki değişkeni nasıl değiştiririz? Bir değişkeni değere göre iletirken, kaynak değişkeni güncellemenin tek yolu metodun dönen değerini kullanmaktır. Bu, metot dışında yalnızca bir değerin değiştirilebileceği anlamına gelir.

```java
int myAge = 14;
myAge = increaseAge(myAge);

int increaseAge(int age) {
	return age + 1;
}
```

<figure style="width: 200px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-03-01-Java-memory-models-pass-by-value-reference/3.png" alt="pass by value">
  <figcaption>Kaynak:penjee.com</figcaption>
</figure>

Görüleceği üzere **myAge** değeri ancak bu şekilde değişir. 15 olarak bu değer güncellenir.

## Pass by Reference (C ve C++ dillerinde)

Referans ile geçirme, değişkenin hafıza adresinin ilgili metoda iletildiği anlamına gelir. Yani hafızada ilgili değişkenin değerini saklayan bloğun adresi, metoda geçirilir.


**C++ programlama dili ile,**
```java
int increaseAgeByRef(int &age) {
    age = age + 1;
  }
int main() {
  int myAge = 14;
  increaseAgeByRef(myAge);
  return 0;
}
```

**C programlama dili ile,**
```java
int increaseAgeByRef(int *age) {
    *age = *age + 1;
  }
int main() {
  int myAge = 14;
  increaseAgeByRef(&myAge);
  return 0;
}
```


Yukarıdaki örnek hem C hem de C++ ile gösterilmiştir. Konuyu **java** ile ele almadan önce farklı dillerde bu yaklaşımların nasıl gerçekleştiğini anlamak önemlidir.

Önceki örneklerden de bildiğimiz üzere ``myAge`` değişkeni hafızada **106 nolu** blokta tutulmaktadır. Yani bu değişkenin hafızadaki adresi **106**'dır. Bu örneklerde de, bu değişkenin tuttuğu değer yerine, hafızadaki adresi olan **106** numarasının ``increaseAgeByRef`` metoduna geçirilmesi sağlanıyor. Anlaşılacağı üzere ``increaseAgeByRef`` metodu ``int`` tipinde, ``age`` isminde bir argüman aldığını deklare ediyor. C++ ve C gibi dillerde, değişkenin önünde **&** sembolü geldiğinde, girilen parametrenin hafızadaki adresine işaret ettiğini anlaşılır. Bu örnekte ``myAge`` değişkeninin adresinin **106** nolu blok olduğunu biliyoruz. Bu yüzden bu adresin işaret ettiği bloğun değeri üzerinde işlem yapılacağını anlayabiliriz. <u><i>Yalnız C programlama dilinde ekstra bir işaretçiye daha ihtiyacımız vardır.</i></u> Dikkat ederseniz C ile yazdığımız kodda, metot tanımı içinde, ``age`` değişkeninin hemen önünde bir **( * )** yıldız sembolü bulunmaktadır. Bu sembolün aslında bir anlamı vardır. **&** Sembolü bir değişkenin adresini gösterirken, **( * )** sembolü ise **bu adreste** depolanan *değeri* döndürür. Yani bir nevi **&** sembolünü tersine çevirir. Özetle adres yerine, adresin işaret ettiği bloktaki değeri verir.

Bu sayede ``myAge`` değişkeninin orjinal değeri olan **14** doğrudan değiştirilmiş olur. Referansa göre geçirme sayesinde doğrudan orjinal değer üzerinde istediğimiz manipülasyonu yapabiliriz. Burada ``age`` ve ``myAge`` değişkenleri aynı adrese işaret ettiği için, ``age`` değişkeninin değerini değiştirmemiz ``myAge`` değişkenini de etkileyecektir. Burada **14** değerine **1** ekleniyor. Yani **106 nolu** blokta bulunan orjinal değeri **15** olarak güncelliyoruz.

Sonuç olarak **pass by value**'da olduğu gibi değer, ayrı bir bloğa kopyalanmadı. Doğrudan orginal değer üzerinde gerekli işlemler gerçekleşmiş oldu.

<figure style="width: 700px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-03-01-Java-memory-models-pass-by-value-reference/4.png" alt="pass by reference">
  <figcaption>Kaynak:penjee.com</figcaption>
</figure>

Referans ile geçirme yaklaşımı ile, metot parametresinde verilen birden fazla değişkeni değiştirmek mümkündür. Bu gibi durumlarda fonksiyon dönüş türünü **true** veya **false** şeklinde yaparak genel durumun başarılı/başarısız olduğunu belirten bir şey veyahut önemli bir değişken döndürülebilir.

Örneğin;


**C programlama dili ile,**
```java
#include <stdbool.h>
#include <stdio.h>

bool calculate(int *c1, int *c2, int *c3) {
    *c1 = *c1 + 1;
    *c2 = *c2 + 1;
    *c3 = *c3 + 1;
    return true;
}

int main() {
  int c1 = 1;
  int c2 = 2;
  int c3 = 3;
  calculate(&c1,&c2,&c3);
  printf("C1 is %d, C2 is %d and C3 is %d\n", c1, c2, c3);
  return 0;
}
```
Kodu bu [linkte](http://www.pythontutor.com/c.html#code=%23include%20%3Cstdbool.h%3E%0A%23include%20%3Cstdio.h%3E%0A%0Abool%20calculate%28int%20*c1,%20int%20*c2,%20int%20*c3%29%20%7B%0A%20%20%20%20*c1%20%3D%20*c1%20%2B%201%3B%0A%20%20%20%20*c2%20%3D%20*c2%20%2B%201%3B%0A%20%20%20%20*c3%20%3D%20*c3%20%2B%201%3B%0A%20%20%20%20return%20true%3B%0A%7D%0A%0Aint%20main%28%29%20%7B%0A%20%20int%20c1%20%3D%201%3B%0A%20%20int%20c2%20%3D%202%3B%0A%20%20int%20c3%20%3D%203%3B%0A%20%20calculate%28%26c1,%26c2,%26c3%29%3B%0A%20%20printf%28%22C1%20is%20%25d,%20C2%20is%20%25d%20and%20C3%20is%20%25d%5Cn%22,%20c1,%20c2,%20c3%29%3B%0A%20%20return%200%3B%0A%7D%0A&curInstr=0&mode=display&origin=opt-frontend.js&py=c&rawInputLstJSON=%5B%5D) çalıştırabilirsiniz.

**C++ programlama dili ile,**
```java
#include <stdio.h>

bool calculate(int &c1, int &c2, int &c3) {
    c1 = c1 + 1;
    c2 = c2 + 1;
    c3 = c3 + 1;
    return true;
}

int main() {
  int c1 = 1;
  int c2 = 2;
  int c3 = 3;
  calculate(c1,c2,c3);
  printf("C1 is %d, C2 is %d and C3 is %d\n", c1, c2, c3);
  return 0;
}
```
Kodu bu [linkte](http://www.pythontutor.com/cpp.html#code=%23include%20%3Cstdio.h%3E%0A%0Abool%20calculate%28int%20%26c1,%20int%20%26c2,%20int%20%26c3%29%20%7B%0A%20%20%20%20c1%20%3D%20c1%20%2B%201%3B%0A%20%20%20%20c2%20%3D%20c2%20%2B%201%3B%0A%20%20%20%20c3%20%3D%20c3%20%2B%201%3B%0A%20%20%20%20return%20true%3B%0A%7D%0A%0Aint%20main%28%29%20%7B%0A%20%20int%20c1%20%3D%201%3B%0A%20%20int%20c2%20%3D%202%3B%0A%20%20int%20c3%20%3D%203%3B%0A%20%20calculate%28c1,c2,c3%29%3B%0A%20%20printf%28%22C1%20is%20%25d,%20C2%20is%20%25d%20and%20C3%20is%20%25d%5Cn%22,%20c1,%20c2,%20c3%29%3B%0A%20%20return%200%3B%0A%7D%0A%0A&curInstr=10&mode=display&origin=opt-frontend.js&py=cpp&rawInputLstJSON=%5B%5D) çalıştırabilirsiniz.


Pass by reference veya pass by value'dan birini kullanmaya karar vermek için iki basit genel kural vardır:

* Bir işlev tek bir değer döndürüyorsa: **pass by value** kullanılabilir,
* Bir işlev iki veya daha fazla farklı değer döndürüyorsa: **pass by reference** kullanmak daha isabetli olabilir. Son verdiğimiz iki örnek bu madde için uygundur. Dönüş türü boolean olan bir metot içinde birden fazla referans değeri değiştirilmektedir.  

> **Not:** Referans yoluyla geçirme genellikle diziler veya obje gibi yapılar kullanılarak önlenebilir.

## Pass by Value(Değer ile Geçirme) - JAVA

Java üst düzey bir programlama dilidir. Bu, normal şartlar altında, bellekte ne olduğu konusunda endişelenmeniz gerekmediği anlamına gelir. Çünkü java hafıza yönetimini arka planda kendi halleder.

Java'da da ilkel veri tipleri (int, double vb.) her zaman **değere göre iletilir**, yani bütün işlem aslında metoda geçirilen değişkenin değerin bir kopyası üzerinden gerçekleşir. C ve C++ üzerinden konuyu anlatırken oluşturduğumuz ilastrasyon burada da geçerlidir. Bu sebepten ötürü tekrardan resmetmek istemedim.

Peki ilkel olmayanlar türler için bu durum sizce nasıl gerçekleşir? C ve C++'da olduğu gibi Java'da da, referansın hafıza adreslerini almak için **&** operatörü yoktur. İlkel olmayan veri tiplerinde değişkenler genellikle bir objede “paketlenir” ve böylece nesne/obje değişkenleri elden ele aktarılır. Java'da değişken geçirme konusuyla ilgili olarak, basit ve genele bir ifade vardır:

> Java'da **HER ZAMAN** pass by value yaklaşımı uygulanır.

Yani referansa göre geçme durumu Java'da söz konusu değildir. Bu durumu izah etmek için şu şekilde bir örneğimizin olduğunu düşünelim.

```java
public class JSample {
    public static void main(String[] args) {
      SomeObject someObject = new SomeObject("object 1");
      System.out.println(someObject);
      testMethod(someObject);
      System.out.println(someObject);
    }
    public static void testMethod(SomeObject someObjectX) {
        someObjectX = new SomeObject("object 2");
    }
}
 class SomeObject {
    public SomeObject(String x){
    }
}
```

Kodu online bir editör olan pythontutor'da çalıştırmak isterseniz, bu [linki](http://www.pythontutor.com/java.html#code=public%20class%20JSample%20%7B%0A%20%20%20%20public%20static%20void%20main%28String%5B%5D%20args%29%20%7B%0A%20%20%20%20%20%20SomeObject%20someObject%20%3D%20new%20SomeObject%28%22object%201%22%29%3B%0A%20%20%20%20%20%20System.out.println%28someObject%29%3B%0A%20%20%20%20%20%20testMethod%28someObject%29%3B%0A%20%20%20%20%20%20System.out.println%28someObject%29%3B%0A%20%20%20%20%7D%0A%20%20%20%20public%20static%20void%20testMethod%28SomeObject%20someObjectX%29%20%7B%0A%20%20%20%20%20%20%20%20someObjectX%20%3D%20new%20SomeObject%28%22object%202%22%29%3B%0A%20%20%20%20%7D%0A%7D%0A%20class%20SomeObject%20%7B%0A%20%20%20%20public%20SomeObject%28String%20x%29%7B%0A%20%20%20%20%7D%0A%7D&cumulative=false&curInstr=0&heapPrimitives=nevernest&mode=display&origin=opt-frontend.js&py=java&rawInputLstJSON=%5B%5D&textReferences=false) ziyaret edebilirsiniz.


> Bir yöntem içindeki parametre değişkenine bir değer (veya nesne) atarsanız, yöntemin dışındaki değişken yine aynı değere (veya nesneye) sahiptir.

Görüleceği üzere ``someObject`` referansı/değişkeni **testMethod** yöntemine geçiriliyor. Dikkat ederseniz bu değişken heap alanında bir objeyi işaret ediyor. Bu kodu çalıştırdığımızda, değişkenin **testMethod** yöntemi içinde çeşitli işlemlere maruz kaldığını göreceksiniz. Yalnız yöntemden çıktıktan sonra, bu değişkenin yönteme girmeden önce sahip olduğu değeri muhafaza ettiği de anlaşılıyor. Peki değişen tam olarak nedir ve burada **pass by value** ve **pass by reference** yaklaşımlarından hangisi uygulanıyor?

İşin içine ilkel olmayan veri türleri de girse java'da uygulanan yaklaşım her zaman **pass by value**'dur. Aslında yöntemin içine girildiğinde ``someObject`` değişkeninin bir kopyası yönteme geçer. Bu *kopya-referans* da, orjinal referans gibi hafızada bir yer kaplar ve tıpkı orjinal referans gibi heap alanında aynı objeye işaret eder. Haliyle bir *kopya referans* da olsa, *kopya referans* üzerinde yapılan işlemler, değişkenin heap alanında işaret ettiği nesneyi de etkileyecektir. Ama değişkenin sahip olduğu değeri, yani heap alanında işaret ettiği nesnenin adresini etkilemeyecektir.

Yukarıdaki bu basit örnek, ``someObject`` değişkeninin referansa göre değil, değere göre iletildiğini gösterir.

> Yalnız, aşağıdaki örnekte olduğu gibi, bazı durumlarda, nesne özelliklerinin yöntem içinde değiştirilebilmesi, programcıları, Java'nın referans ile geçtiğini yanılgısına düşürebiliyor.

Örneğin:

```java
public class JSample {
    public static void main(String[] args) {
      SomeObject someObject = new SomeObject("object 1");
      System.out.println(someObject);
      testMethod(someObject);
      System.out.println(someObject);
    }
    public static void testMethod(SomeObject someObject) {
        someObject.setName("o1");
        someObject = new SomeObject("object 2");
        someObject.setName("o2");
    }
}
 class SomeObject {
   private String name;
   public SomeObject(String x){

   }

   public void setName(String name){
     this.name = name;
   }
}
```

Kodu online bir editör olan pythontutor'da çalıştırmak isterseniz, bu [linki](http://www.pythontutor.com/java.html#code=public%20class%20JSample%20%7B%0A%20%20%20%20public%20static%20void%20main%28String%5B%5D%20args%29%20%7B%0A%20%20%20%20%20%20SomeObject%20someObject%20%3D%20new%20SomeObject%28%22object%201%22%29%3B%0A%20%20%20%20%20%20System.out.println%28someObject%29%3B%0A%20%20%20%20%20%20testMethod%28someObject%29%3B%0A%20%20%20%20%20%20System.out.println%28someObject%29%3B%0A%20%20%20%20%20%20%0A%20%20%20%20%20%20%0A%20%20%20%20%7D%0A%20%20%20%20public%20static%20void%20testMethod%28SomeObject%20someObject%29%20%7B%0A%20%20%20%20%20%20%20%20someObject.setName%28%22o1%22%29%3B%0A%20%20%20%20%20%20%20%20someObject%20%3D%20new%20SomeObject%28%22object%202%22%29%3B%0A%20%20%20%20%20%20%20%20someObject.setName%28%22o2%22%29%3B%0A%20%20%20%20%7D%0A%7D%0A%20class%20SomeObject%20%7B%0A%20%20%20private%20String%20name%3B%0A%20%20%20public%20SomeObject%28String%20x%29%7B%0A%20%20%20%20%20%20%0A%20%20%20%7D%0A%20%20%20%0A%20%20%20public%20void%20setName%28String%20name%29%7B%0A%20%20%20%20%20this.name%20%3D%20name%3B%0A%20%20%20%7D%0A%7D%0A%0A&cumulative=false&curInstr=28&heapPrimitives=nevernest&mode=display&origin=opt-frontend.js&py=java&rawInputLstJSON=%5B%5D&textReferences=false) ziyaret edebilirsiniz.

buradaki ``setName`` işlevi, ``someObject`` değişkeninin değerini değil, referans aldığı nesnenin özelliğini(property/field) değiştirir. Yani özellikten kasıt, ilgili nesnenin örnek değişkeni(instance variable) olan ``name`` değişkenidir. Bu arada ``someObject`` değişkeninin değeri aynı kalır. Çünkü metoda geçirilen değişken aslında ``someObject`` değişkeninin bir kopyasıdır.

Aşağıdaki kod bloğu az önce vermiş olduğumuz örneğin hemen hemen aynısıdır. Sadece özellikle belirtmek için metot içine aldığımız parametreyi farklı göstermek için ismini ``someObjectX`` olarak değiştirdim. Yukarıdaki örnekte de bu değişikliği dilerseniz yapabilirsiniz.


```java
SomeObject someObject = new SomeObject("object 1");
testMethod(someObject);

public static void testMethod(SomeObject someObjectX) {
    someObjectX.setName("o1");
    someObjectX = new SomeObject("object 2");
    someObjectX.setName("o2");
}
```

Farz edelim ki referansın heap alanıdaki adresi **121** rakamı olsun.

<figure style="width: 500px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-03-01-Java-memory-models-pass-by-value-reference/5_1.png" alt="java object Variable">
  <figcaption></figcaption>
</figure>

* **1.satırda:** heap alanında ``new`` anahtar kelimesi yardımıyla bir ``SomeObject`` objesi yaratılır. Bu objeyi stack'da ``someObject`` değişkeni temsil etmektedir. Hayali verdiğimiz **121** rakamı(yani objenin heap alanıdaki adresi) bu değişkene **değer** olarak geçirilir. (Bir üstteki şekil)

<figure style="width: 500px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-03-01-Java-memory-models-pass-by-value-reference/5_2.png" alt="java object Variable">
  <figcaption></figcaption>
</figure>

* **2.satırda:** ise ``someObject`` değişkeni **testMethod** yöntemine **pass by value** yaklaşımı ile geçer. Yani aslında bu değişkenin bir kopyası **testMethod** yöntemine geçecektir. (Bir üstteki şekil)

* **4.satırda:** burada ``someObjectX`` isminde bir *kopya-referans* oluşturulur. Bu referans/değişken ``someObject`` değişkeninde olduğu gibi **121** değerine sahiptir. Her ne kadar *kopya-referans* olsa da heap alanında yine aynı objeyi işaret edeceğini unutmayın. (Bir üstteki şekil)

<figure style="width: 500px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-03-01-Java-memory-models-pass-by-value-reference/5_3.png" alt="java object Variable">
  <figcaption></figcaption>
</figure>

* **5.satırda:** ise ``someObjectX`` değişkeninin heap alanında işaret ettiği nesnenin ``name`` özelliği(yani nesnenin üye değişkeni) **o1** olarak güncelleniyor. ``someObject`` değişkeni de aynı nesneyi işaret ettiği için haliyle bu güncellemeden dolaylı yoldan etkilenmiş olur ama sahip olduğu değerde(yani **121**'de) bir değişiklik olmaz. (Bir üstteki şekil)

<figure style="width: 500px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-03-01-Java-memory-models-pass-by-value-reference/5_4.png" alt="java object Variable">
  <figcaption></figcaption>
</figure>

* **6.satırda:** bu satırda yeni bir``SomeObject`` objesi yaratılır ve *kopya-referansımız* olan ``someObjectX`` değişkeni bu nesneyi işaret etmeye başlar. Burada bir başka değişen şey ise ``someObjectX`` değişkeninin değeridir. Bu değişken yeni objenin adresi olan **119** rakamını saklamaya başlar. Buna karşın ``someObject`` değişkeni ise halen **121** adresini muhafaza etmektedir. (Bir üstteki şekil)


<figure style="width: 500px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-03-01-Java-memory-models-pass-by-value-reference/5_5.png" alt="java object Variable">
  <figcaption></figcaption>
</figure>

* **7.satırda:** 7.satırda ise ``someObjectX`` değişkeninin işaret ettiği nesnenin ``name`` özelliği(yani nesnenin üye değişkeni) **o2** olarak güncellenmektedir. (Bir üstteki şekil)

<figure style="width: 500px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-03-01-Java-memory-models-pass-by-value-reference/5_6.png" alt="java object Variable">
  <figcaption></figcaption>
</figure>

* **3.satırda:** Son olarak bir ek bilgi daha verecek olursak, **testMethod** yönteminden çıktıktan sonra, yani bu yöntemin kapsamı dışına çıktıktan sonra, hem ``someObjectX`` değişkeni, hem de bu değişkenin işaret ettiği nesne ortadan kalkacak, sadece ``someObject`` değişkeni ve bu değişkenin işaret ettiği nesne kalacaktır. (Gerekli temizleme işlemleri garbage collector tarafından gerçekleştirilir.)


Anlaşılacağı üzere, ``someObject`` değişkeni nesnenin kendisini tutmuyor, değişkenin değeri sadece nesneyi tanımlayan bir işaretçidir(yani nesnenin heap'deki adresidir). <u><i>121 sayısı aslında yönteme geçirilir ve "referansla geçirme" gibi anlaşılabilir.</i></u> Yalnız java'da, diğer ilkel veri tiplerinde olduğu gibi referans tiplerinde de geçişler **pass by value** şeklinde gerçekleşir. Yani metoda aktarılan, değişkenin bir kopyasıdır. Yani özelte şu saptamayı yapabiliriz;

> Java'da nesne referansları(işaretçiler) da pass by value(değere göre geçme) yaklaşımı ile geçirilir.


<!-- Başvuru yoluyla iletme(Passing By Reference), "çağıran metodun" içindeki bir argümanın adresini "çağrılan metodun" içindeki karşılık gelen bir parametreye iletme yöntemini ifade eder.

Reference by Passing, "calling-function" daki bir argümanın adresini "called function"daki karşılık gelen bir parametreye geçirme yöntemini ifade eder.

Referans yoluyla geçirme, değişkenin bellek adresinin metoda iletildiği anlamına gelir. -->


## Referanslar

* [https://blog.penjee.com/passing-by-value-vs-by-reference-java-graphical/](https://blog.penjee.com/passing-by-value-vs-by-reference-java-graphical/)
* [https://www.geeksforgeeks.org/passing-by-pointer-vs-passing-by-reference-in-c/](https://www.geeksforgeeks.org/passing-by-pointer-vs-passing-by-reference-in-c/)
* [https://www.ibm.com/support/knowledgecenter/en/ssw_ibm_i_74/rzarg/cplr233.htm](https://www.ibm.com/support/knowledgecenter/en/ssw_ibm_i_74/rzarg/cplr233.htm)
* [Pass by value vs. pass by reference](https://www.educative.io/edpresso/pass-by-value-vs-pass-by-reference#:~:text=Pass%20by%20value%20means%20that,not%20visible%20to%20the%20caller.&text=Changes%20made%20to%20the%20passed%20variable%20do%20not%20affect%20the%20actual%20value.)
