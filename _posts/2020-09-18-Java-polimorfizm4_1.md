---
title: "Java'da Polimorfizm 4.1 - Statik ve Dinamik Bağlanma 1"
comments: false
excerpt: "Bu derste hem Java'da statik ve dinamik bağlanma arasındaki farkları ele alacağız. Diğer bir ifade şekliyle derleme ve çalışma zamanı polimorfizmi olarak da bilinir. Bunun yanı sıra dolaylı final metotlar, metot saklamanın ne olduğu da bu ders içinde anlatılacaktır"
header:
  teaser: "assets/images/equality.webp"
  og_image: /assets/images/equality.webp
  overlay_image: /assets/images/unsplash-image-55.webp
  overlay_filter: 0.5 #rgba(255, 0, 0, 0.5)
  caption: "Photo by [majd altaifi](https://unsplash.com/photos/rVAvAxQSmGI) on Unsplash"
  #cta_label: "More Info"
  #cta_url: "https://unsplash.com"
categories:
  - java-kalitim-polimorfizm
tags:
  - java polimorfizm
  - static/early binding(statik/erken bağlanma)
  - dynamic/late binding(dinamik/geç bağlanma)
  - overriding metot
  - overloading metot
  - ezici metot
  - geçersiz kılma
  - aşırı yükleme
  - metot saklama (method hiding)
  - metot gizleme
  - dolaylı final metotlar
last_modified_at: 2020-02-19T15:12:19-04:00
toc: true
toc_label: "SAYFA İÇERİĞİ"
toc_sticky: true
---



**ÖNEMLİ :** Kendim için aldığım notlar. Umarım size de bir faydası olur. Kullanılan her bir makale referans olarak eklenmiştir.
{: .notice}


Bu ve hemen sonrasında gelecek bölüm bir önceki bölümle ve Java'da Kalıtım 3.1 - Static ve Dinamik Tür bölümüyle doğrudan bağlantılıdır. Bu bölümde özellikle, Java'nın Dil Şartnamesi dökümanından da aldığım önemli notları paylaşacağım. Yer yer bazı tanımlamaları olduğu gibi sunmayı planlıyorum. Ek olarak bundan sonra paylaşacağım **Statik ve Dinamik Bağlanma 2** konusuna da kesinlikle bakmanızı öneririm. Bu konunun daha iyi anlaşılmasını sağlayacak bilgiler içermektedir.


Aslında derleme ve çalışma zamanı polimorfizmi tanımlarına ek olarak aşağıdaki tanımları da kullanmamızda herhangi bir sakınca yoktur.

1. Statik/ Erken bağlanma/ Derleme zamanı polimorfizmi
2. Dinamik/ Geç bağlanma/ Çalışma zamanı polimorfizmi

Çünkü yazdığınız kod bir polimorfizm sergiliyorsa, koda ait metot ve değişkenlerin bazılarının derleme zamanında, bazılarınınsa çalışma zamanında bağlanması gerekmektedir. O halde şöyle bir genelleme yapabiliriz.

* Eğer bu haritalama derleme zamanında gerçekleşirse, statik/erken bağlanma,
* Eğer bu çözülme çalışma zamanında gerçekleşiyorsa, dinamik/geç bağlanma  

gerçekleşir diyebiliriz.



## Statik/Erken Bağlanma (Static/Early Binding)

Statik, yani erken bağlanma, derleme zamanında meydana gelen olayları ifade eder. Özetle, bir işlevi çağırmak için gereken tüm bilgiler derleme zamanında biliniyorsa, erken bağlanma gerçekleşir.

> Tüm ``static``, ``private`` ve ``final`` yöntemlerin bağlanması derleme zamanında gerçekleşmektedir.

Derleyici, bu yöntemlerin geçersiz kılınamayacağını bilir. Aynı zamanda bu yöntemlere yerel sınıf(local class) nesnesi tarafından erişilebileceğinin de farkındadır. Bu nedenle derleyici "yerel sınıfın" nesnesini belirlemekte herhangi bir zorluk yaşamaz. Bu tür yöntemlerin bağlanmalarının "statik" olmasının bir nedeni de budur. **Çünkü statik bağlanma daha iyi bir performans sağlar.**

Bunların yanı sıra **overloading(aşırı yüklenmiş)** metotların da bağlanması derleme zamanında gerçekleşir.
{: .notice--warning}

### Dolaylı final yöntemler(implicit final methods) ve metot saklama(method hiding)

Bir üst sınıfta bulunan `final` metot, bu üst sınıfı miras alan bir alt sınıf tarafından kesinlikle **override**(geçersiz kılma) edilemez. Bu, `final` yöntem implementasyonunun hiyerarşideki, doğrudan ve dolaylı tüm alt sınıflar tarafından kullanılacağını garanti eder. Aynı zamanda `private` olarak deklare edilmiş metotlar da, bir alt sınıfta bunları **override** etmek mümkün olmadığından dolaylı olarak(implicitly) `final` sayılır. Bunun yanı sıra `final` olarak deklare edilen bir `final` sınıf bir üst sınıf olamaz (yani, bir sınıf bir `final` sınıfı extend edemez). Sonuç olarak bir `final` sınıfındaki tüm yöntemler dolaylı olarak `final`'dır.

Bu arada `statik` yöntemler de geçersiz kılınamayacağından(yani override edilemeyeceğinden), **dolaylı olarak** `final`'dır. Evet garip geldiğinin farkındayım. Çünkü `static` yöntemler **override** edilmeye çalışıldığında aslında gerçekleşen **metot saklama(method hiding)** olayıdır. Bir alt sınıf, üst sınıftaki bir `static` yöntemle aynı imzaya sahip bir `static` yöntem tanımlarsa, alt sınıftaki yöntem, üst sınıftaki yöntemi gizler.

<div class="notice--success" markdown="1">
<h4 class="no_toc"><i class="fas fa-comment"></i> Note:</h4>
---
Statik bir yöntemi gizleme ve bir örnek(instance) yöntemini geçersiz kılma arasındaki farkın önemli sonuçları vardır:

* Birincisi, çağrılan, override edilmiş instance metodun versiyonu her zaman alt sınıfta olandır.
* İkincisi, çağrılan, saklanmış static metodun versiyonu ise üst sınıftan mı yoksa alt sınıftan mı çağrıldığına bağlıdır.
</div>
Aşağıdaki kodla bu iki durumu izah etmek istiyorum.

```java
public class Animal {
    public static void testClassMethod() {
        System.out.println("The static method in Animal");
    }
    public void testInstanceMethod() {
        System.out.println("The instance method in Animal");
    }
}
```

```java
public class Cat extends Animal {
    public static void testClassMethod() {
        System.out.println("The static method in Cat");
    }
    public void testInstanceMethod() {
        System.out.println("The instance method in Cat");
    }

    public static void main(String[] args) {
        Cat myCat = new Cat(); //1
        Animal myAnimal = myCat; //2

        Animal.testClassMethod(); //3
        myAnimal.testInstanceMethod(); //4

        myAnimal.testClassMethod(); //5
        myCat.testClassMethod(); //6

        Animal.testClassMethod(); //7
        Cat.testClassMethod(); //8
    }
}
```

**Cat** sınıfı görüleceği üzere **Animal** sınıfı içindeki instance metodunu geçersiz kılıyor ve **Animal** sınıfı içindeki statik yöntemi, aynı metot deklerasyonunu yapmak suretiyle gizliyor. Buradan sonraki aşamaları kod üzerinde belirttiğim numaralarla göstermek istiyorum..

1. `main` metodu içinde de bir **Cat** objesi yaratılıyor.
2. Yaratılan **Cat** objesinin `myCat` referansının değeri, **Animal** tipindeki `myAnimal` referansına veriliyor. Hatırlarsanız compatible(yani kalıtsal olarak uygun) tiplerle, objelerin referanslarını deklare edebiliyorduk.. Aslında buradaki önemli husus şu, `myCat` referansının tipi **Cat** olduğundan, hem **Cat** sınıfı özelindeki, hem de **Cat**'in miras aldığı üst sınıflar olan **Animal** ve **Object** sınıfı özelindeki public metot ve değişkenleri görebiliyoruz. Yalnız, `myAnimal` referansının tipi **Animal** olduğu için, sadece **Animal**, ve **Animal** sınıfının dolaylı(implicitly) olarak `extends` ettiği **Object** sınıfındaki public metot ve değişkenler görülebilir olacaktır(override edilmiş olanlar ayrı!!!). Çünkü **override** edilmiş bir metot varsa, polimorfizm gereği, implementasyon her zaman alt sınıfın ezilmiş metoduna iletilir. Her neyse ufak bir hatırlatmadan sonra devam edebiliriz.
3. Burada **Animal** sınıfı üzerinden, sınıfa ait bir statik metot olan `testClassMethod()`'una erişiliyor. Statik metotlar sınıfa ait oldukları için sınıf üzerinden erişim her zaman daha doğru olacaktır. Ama instance'lar üzerinden de erişilebilir. Çünkü statik konteks bütün objelerle ortak paylaşılır. Özetle burada anlatılmak istenen şu! `testClassMethod()` metodu alt sınıf olan **Cat** sınıfında override edilmiş gibi **gözükse de**(aslında geçersiz kılınmıyor), burada gerçekleşen **metot saklama(method hiding)** olayıdır. Hatırlarsanız, normalde polimorfizm gereği, ilgili implementasyon **override** edilen alt sınıfa devrediliyordu. Statik metotlarda ise durum farklı!! Statik metotlarda, bu metodu kim çağırıyorsa o sınıfın statik metodu görünür olur. Diğeri ise saklanır. Sonuç olarak

    ```java
    Animal.testClassMethod();
    ```
  yani **Animal** sınıfının statik metodu çağrıldığı için, konsola bu metodun döndürdüğü şey yazılacaktır. Çıktıları aşağıdaki görebilirsiniz.
4. Burada ise **Animal** sınıfının bir instance metodunu(statik olmayan metotlar) görüyorsunuz. Bu `testInstanceMethod()` metodu alt sınıf olan **Cat** sınıfında override edildiği için polimorfizm gereği implementasyon bu sınıfın, yani **Cat** sınıfının `testInstanceMethod()` una aktarılır. Metodu nereden çağırdığınızın bir önemi yoktur, ki burada ilgili metot, Animal tipindeki myAnimal referansı üzerinden çağrılıyor. Normalde **Animal** sınıfının özelindeki metotları görmemiz gerekiyordu ama bir istisna dışında demiştik. O da neydi? **override** edilen metotlar dışında!!! Burada da **overide** edilen bir metot olduğu için mecburen implementasyonu alt sınıfa aktarmak durumundayız. Özetle ekrana **Cat** sınıfındaki instance metodun implementasyonu yazılacaktır.
5. Bu satırda yine statik metoda bir erişim var.. Daha önce dediğim gibi statik metoda hangi sınıf üzerinden eriştiğiniz önemlidir. Birinden biri gizlenecektir. Burada **Animal** tipindeki `myAnimal` referansı üzerinden bir erişim sağlanıyor. Bu referans hatırlarsanız 2. satırda `myCat` referasının tuttuğu objeye bağlanıyordu. Yani özetle instance üzerinden bir erişim sağlanıyor. Doğrudan sınıf üzerinden değil!! Her neyse `myAnimal`'ın tipi **Animal** olduğu için, bu referans ilk olarak **Animal** özelindeki metotları görecektir. Çağrılan bu metot instance bir metot olmadığı için polimorfik düşünmemize gerek yok. Çağrılan metot statik bir metot olduğu için, çağrıldığı sınıfın statik metodu gözükecek, bu sınıfı miras alan **Cat** sınıftaki benzer statik metot ise gizlenecektir. Özetle **Animal** sınıfındaki statik metodun logic'i çalışacaktır.
6. Bu satırda ise `myCat` referansı üzerinden, yani yine bir instance üzerinden statik metoda erişim gerçekleşmektedir. Ama bu sefer `myCat` referansının tipi **Cat** olduğu için, **Cat** sınıfındaki statik `testClassMethod()` metodu çalışacak, **Animal** sınıfındaki `testClassMethod()` metodu gizlenecektir.
7. Bu satırda ise statik metoda erişimi, olması gerektiği gibi sınıf üzerinden yapıyoruz. Her neyse burada statik metoda erişim **Animal** sınıfı üzerinden gerçekleştiği için, bu sınıf içindeki `testClassMethod()` statik metodu çalışacak, **Cat** sınıfındaki gizlenecektir.
8. Bu satırda ise **Cat** sınıfı üzerinden statik metoda erişildiğinden, bu sınıftaki `testClassMethod()` statik metodu çalışacak, **Animal** sınıfındaki `testClassMethod()` metodu gizlenecektir.



```java
The static method in Animal //1
The instance method in Cat //2
The static method in Animal //3
The static method in Cat //4
The static method in Animal //5
The static method in Cat //6
```

Özetle bir **final** yöntemin deklarasyonu asla değişemez, bu nedenle tüm alt sınıflar aynı yöntem uygulamasını kullanır ve **final** yöntemlere yapılan çağrılar **derleme zamanında** çözümlenir ve bu, **statik bağlama** olarak bilinir.

## Dinamik/Geç Bağlanma (Dynamic/Late Binding)

Geç bağlanma(late binding), çalışma zamanına(run-time) kadar çözümlenmeyen işlev çağrılarını ifade eder. Geç bağlanma veya başka bir ifadeyle dinamik bağlanmada, derleyici çağrılacak yönteme karar vermez. Sanal(virtual) fonksiyonlar geç bağlanmayı sağlamak için kullanılır. Bu arada java'da sanal(virtual) fonksiyonlar var mıdır?

> Java'da, sanal işlev kavramı çok basittir, çünkü **static**, **private** veya **final** dışındaki tüm işlevler varsayılan(default) olarak sanal(virtual) işlevlerdir. Bazı programlama dillerinde işlevlerin sanal olduğunu belirtmek için **virtual** anahtar kelimesi kullanılır. Java'da yukarıda da belirtildiği gibi tüm **üye işlevler(member functions)** varsayılan olarak sanaldır.

--------

Yukarıdaki örnekte parent sınıfın referansıyla alt sınıfın nesnesini temsil edebileceğimizi görmüştük. Yani;

```java
Animal myAnimal = new Cat();
```
Parent sınıfın referansından yöntemleri çağırabiliriz. Ancak, gerçek yöntem çağırma işlemi, bu örnekteki Animal sınıfı referansı tarafından işaret edilen nesnenin dinamik türüne bağlıdır. Bu örnekten yola çıkarsak, Animal referansının türü nesnenin **statik türü** olarak bilinir, ve çalışma zamanında bu referans tarafından işaret edilen gerçek nesne ise nesnenin **dinamik türü** olarak tanımlanır. Derleyici, statik tür üzerinden, yani myAnimal referansından bir yöntem çağrıldığını görürse, ve **bu yöntem geçersiz kılınabilir bir yöntemse(statik olmayan ve final olmayan)**, derleyici gerçek yöntemi belirlemeyi erteler. Bu işleme çalışma zamanı bağlanması(geç bağlanma) denir. Çalışma zamanında, nesnenin gerçek dinamik türüne bağlı olarak uygun bir yöntem çağrılır. Bu mekanizma, dinamik yöntem çözünürlüğü veya dinamik yöntem çağırma olarak bilinir.

<div class="notice--success" markdown="1">
<h4 class="no_toc"><i class="fas fa-comment"></i> Note:</h4>
---
> Bu arada abstract metotlar da çalışma zamanı polimorfizmini kullanır. Yani dinamik/geç bağlanmaya maruz kalırlar.

Aslında statik ve dinamik bağlanmaya verilebilecek en belirgin örnekler; statik bağlama için **overloading(aşırı yüklenmiş)** metotlar, dinamik bağlama için ise **overriding(geçersiz kılınan)** metotlardır. Örneğin aşağıdaki kod bloğunu ele alabiliriz.
</div>
Aşağıdaki örnekte hem overloading hem de overriding metot örneklerini göreceksiniz. Derleyici ``Car`` sınıfındaki bu overloading metotları ve  metotların kodlarını derleme zamanında çözecektir. Bu bir statik bağlama örneğidir. Fakat durum overriding metotlar için farklıdır. Onların bağlanması çalışma zamanında olacaktır. ``BMW`` sınıfındaki ``getCarBrand`` metodu buna bir örnektir.

```java
public class Car {
    public String getCarBrand(String name){
        return name;
    }
    public String getCarBrand(String name, int year){
        return name +" " +year;
    }
}
```

```java
public class BMW extends Car {

    @Override
    public String getCarBrand(String name){
        return name;
    }
}
```

```java
public class Features {
  public static void isElectricCar(Car car){
      System.out.println("No, there is not enough information");
  }
  public static void isElectricCar(BMW bmw){
      System.out.println("Yes, it is");
  }
}
```

```java
public class Main {
    public static void main(String[] args) {
        Car car = new Car();
        Car bmw = new BMW();

        System.out.println(car.getCarBrand("mercedes"));
        System.out.println(car.getCarBrand("ford", 1960));
        System.out.println(bmw.getCarBrand("bmw"));

        Features.isElectricCar(car);
        Features.isElectricCar(bmw);
    }
}
```

**Çıktı:**

```java
mercedes
ford 1960
bmw
No, there is not enough information
No, there is not enough information
```

Görüleceği üzere statik metotlar statik bağlanmaya maruz kalırlar. ``isElectricCar`` statik metodu içine aldığı ``bmw`` referansını, statik bağlabnaya maruz kaldığı için ``Car`` olarak algılar ve **"No, there is not enough information"** sonucunu ekrana yazdırır.



## Statik Bağlanmada Field Erişimi (Örnek 1)


Statik bağlanmada **field** erişimini örnek üzerinden anlatmaya çalışacağım;

```java
class S { int x = 0;}

class T extends S { int x = 1; }

public class Test1 {
    public static void main(String[] args) {
        T t = new T();
        System.out.println("t.x=" + t.x + when("t", t));

        S s = new S();
        System.out.println("s.x=" + s.x + when("s", s));

        s = t;
        System.out.println("s.x=" + s.x + when("s", s));
    }

    static String when(String name, Object t) {
        return " when " + name + " holds a "
                        + t.getClass() + " at run time.";
    }
}
```

Çıktı şu şekildedir:

```
t.x=1 when t holds a class T at run time.
s.x=0 when s holds a class S at run time.
s.x=0 when s holds a class T at run time.
```


Son satır, aslında erişilmeye çalışılan field'ın, başvurulan nesnenin çalışma zamanı sınıfına **bağlı olmadığını** gösterir; s değişkeni, T sınıfından bir nesnenin bir referansını tutsa bile, **s.x** ifadesi, **S** sınıfının **x** field'ını ifade eder, çünkü **s** ifadesinin türü **S**'dir. T sınıfının nesneleri, biri **T** sınıfı için ve biri de üst sınıfı olan **S** olmak üzere **x** isimli iki field içerir.

>Alan erişimlerine yönelik bu dinamik arama eksikliği, programların basit uygulamalarla verimli bir şekilde çalıştırılmasına izin verir.


<img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-09-18-Java-polimorfizm4_1/1.webp"  width="400px" height="100%" class="align-center" loading="lazy" alt="Static Binding for Field Access">


## Generic Öncesi ve Sonrası Dönemde Çalışma ve Derleme Zamanı Polimorfizmi

Aslında bu bölümde konuyu farklı açıdan ele almaya çalışacağım. Yani generic türler öncesinde ve sonrasında statik ve dinamik bağlanma nasıl gerçekleşiyordu. Yine örnek üzerinden ilerlemenin faydası var.

Generic kavramı hayatımıza Java SE 5'ten sonra girmiştir. Haliyle java'nın ilk sürümünden bu sürüme kadar işlerimizi generic'siz bir şekilde halletmeye çalışıyorduk. Her neyse, ilk örneğimiz bize generic olmadan gerçekleşen bir kod örneğini gösteriyor. Görüleceği üzere Test sınıfı hem String hem de Integer tipinde iki değeri saklamaya çalışıyor. Diyelim ki Test sınıfının setValue metodu deklare edildiğinde buna müsade etsin. Yani bu metodun deklarasyonunun şu şekilde olduğunu hayal edin.

```java
public void setValue(Object value){
  this.value = value;
}
```
Sonrasında bu değerleri dışarı çıkarmak istediğimizde her seferinde test etmemiz gerekecek. Buradaki **instanceof** anahtarı çalışma zamanında nesnenin dinamik türünün ne olduğunu sorgular. Eğer if clause ile doğru nesneyi bulursak, downcasting yaparak doğru nesneye işaret ettiğimizi söyleyebiliriz. Görüleceği üzere hem instanceof hem de downcasting gibi iki işlemi yapmak durumunda kalıyoruz. Özetle generic öncesinde çalışma zamanı polimorfizmi vardı. Yani gerçek nesnenin tipini çalışma zamanında görebiliyorduk.

```java
Test test = new Test();
test.setValue(new String("Hello World"));
test.setValue(new Integer(3));

Object value = test.getValue();
if (value instanceof String) {
  String s = (String)value;
}
if (value instanceof Integer) {
  Integer i = (Integer)value;
}
```
Yalnız generic sonrasında bu durum değişti. Yani derleme zamanında gerçek nesnenin tipini belirleme imkanı sunuldu. Örneğimizden de anlaşılacağı üzere Test sınıfının sadece String tipinde değerler saklayacağını deklare ediyoruz. Böylelikle derleme zamanı tür güvenliğini sağlamış oluyoruz.

```java
Test<String> test = new Test<>();
test.setValue("Hello World");
test.setValue(3); // compile time error occurs
```
3.satırda int bir değer eklemeye çalıştığımızda daha derleme zamanında hata alırız. Bu da bize generic türlerin derleme zamanı polimorfizmi uyguladığını gösterir.

Başka bir örnek daha paylaşmak istiyorum.

```java
public void sample(List<String> list) {
    System.put.println(list.size());
}
```

sample metodunun argümanına baktığımızda bir List arayüzü(interface) ile kaşılaşıyoruz. Bildiğiniz üzere List bir abstract(yani soyut) sınıftır. Haliyle interface'leri somut(yani concrete) bir sınıf olmadan yaratamayız. Yani;

```java
List s1 = new List()
```
yapmaya çalıştığınızda alacağınız hata şu şekilde bir derleme hatası olacaktır. **'List' is abstract; cannot be instantiated**

```java
List<String> s1 = new ArrayList<String>();
List<String> s2 = new LinkedList<String>();
List<String> s3 = new Vector<String>();
```

Peki sorulması gereken soru şu? Bu List arayüzü hangi somut sınıfı temsil ediyor? ArrayList? LinkedList?  Derleme zamanında bunu bilme şansımız var mı? Şu ana kadar öğrendiklerimizden yola çıkacak olursak, List arayüzünün somut türünün ne olduğunu derleme sırasında bilmenin bir yolu yoktur. Yalnızca çalışma zamanında öğrenebilirsiniz. Dolayısıyla, size() yönteminden hangi somut sınıftan çağrılacağına yalnızca çalışma zamanında karar verilebilir.

-----

## Statik Bağlanmada Field Erişimi (Örnek 2)

Alanlara(fields) erişmek için örnek yöntemlerin(instance methods) kullanıldığı benzer bir örneği ele alacak olursak;


```java
class S           { int x = 0; int z() { return x; } }
class T extends S { int x = 1; int z() { return x; } }
class Test2 {
    public static void main(String[] args) {
        T t = new T();
        System.out.println("t.z()=" + t.z() + when("t", t));
        S s = new S();
        System.out.println("s.z()=" + s.z() + when("s", s));
        s = t;
        System.out.println("s.z()=" + s.z() + when("s", s));
    }
    static String when(String name, Object t) {
        return " when " + name + " holds a "
                        + t.getClass() + " at run time.";
    }
}
```

```
t.z()=1 when t holds a class T at run time.
s.z()=0 when s holds a class S at run time.
s.z()=1 when s holds a class T at run time.
```

Son satır, erişilen yöntemin, başvurulan nesnenin çalışma zamanı sınıfına bağlı olduğunu gösterir; s, T sınıfına ait bir nesneye referans tuttuğunda, s.z () ifadesi, s ifadesinin türü S olmasına rağmen, T sınıfının z yöntemini ifade eder. **T sınıfının z yöntemi, S sınıfının z yöntemini geçersiz kılar.**

## Sözlük

* Bilgisayar bilimlerinde, dinamik dağıtım([Dynamic dispatch](https://en.wikipedia.org/wiki/Dynamic_dispatch)), hangi polimorfik işlemin çalışma zamanında uygulanacağını seçme işlemidir.
* Sınıf tabanlı programlamada, [downcasting](https://en.wikipedia.org/wiki/Downcasting) veya tür ayrıntılandırma(type refinement), bir temel sınıfın türetilmiş sınıflarından birine referans verme eylemidir.
* Geç bağlanma([late binding](https://en.wikipedia.org/wiki/Late_binding)), dinamik bağlanma veya dinamik bağlantı, bir nesneye çağrılan yöntemin veya bağımsız değişkenlerle çağrılan işlevin çalışma zamanında adıyla arandığı bir bilgisayar programlama mekanizmasıdır.


## Referanslar

* [Difference between Early and Late Binding in Java](https://www.geeksforgeeks.org/difference-between-early-and-late-binding-in-java/)
* [Static and Dynamic Binding in Java](https://www.baeldung.com/java-static-dynamic-binding)
* [Virtual Function in Java](https://medium.com/@namangupta01/virtual-function-in-java-vs-c-d75874d23#:~:text=In%20object%2Doriented%20programming%2C%20a,to%20provide%20the%20polymorphic%20behavior.)
* [Static Binding for Field Access](https://docs.oracle.com/javase/specs/jls/se13/html/jls-15.html#d5e25867)
* [Static vs Dynamic Binding in Java](https://www.geeksforgeeks.org/static-vs-dynamic-binding-in-java/)
* [toString metodu](https://docs.oracle.com/javase/7/docs/api/java/lang/Object.html#toString())
* [Using the this Keyword](https://docs.oracle.com/javase/tutorial/java/javaOO/thiskey.html)
* [Interface List](https://docs.oracle.com/javase/6/docs/api/java/util/List.html)
* [Classes](https://docs.oracle.com/javase/specs/jls/se6/html/classes.html)
* [Static and Dynamic Binding in Java](https://www.baeldung.com/java-static-dynamic-binding)
* [Static Vs. Dynamic Binding in Java](https://stackoverflow.com/questions/19017258/static-vs-dynamic-binding-in-java)
* [Polymorphism in Java](https://www.baeldung.com/java-polymorphism)
* [Virtual Function in Java vs. C++](https://medium.com/@namangupta01/virtual-function-in-java-vs-c-d75874d23#:~:text=In%20object%2Doriented%20programming%2C%20a,to%20provide%20the%20polymorphic%20behavior.)
* [What is Virtual Method](http://net-informations.com/faq/oops/virtual.htm)
* [Local Classes](https://docs.oracle.com/javase/tutorial/java/javaOO/localclasses.html)
* [Why does Java bind variables at compile time?](https://stackoverflow.com/questions/32422923/why-does-java-bind-variables-at-compile-time)
* [Java for Programmers 2nd Edition Deitel Developer Series](https://www.amazon.com/Java-Programmers-Deitel-Developer-Paul/dp/0132821540)
* [Oracle Certified Professional Java SE 8 Programmer Exam 1Z0-809](https://www.amazon.com/Oracle-Certified-Professional-Programmer-1Z0-809/dp/1484218353)
* [Overriding and Hiding Methods](https://docs.oracle.com/javase/tutorial/java/IandI/override.html)
