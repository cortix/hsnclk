---
title: "Algoritmalar Bölüm 1 - Seçme Sıralaması(Selection Sort)"
comments: false
excerpt: "Bu bölümde, seçme sıralaması algoritmasının ne olduğunu ve performansının en kötü, en iyi ve ortalama durum senaryosunda nasıl davrandığını açıklamaya çalışacağım."
header:
  #teaser: "/assets/images/2022-11-11-algorithms-part1-selection-sort/selection-sort.png"
  #og_image: /assets/images/page-header-og-image.png
  #og_image: /assets/images/2022-11-11-algorithms-part1-selection-sort/selection-sort.png
  teaser: "/assets/images/svg-book14.svg"
  og_image: /assets/images/svg-book14.svg
  overlay_image: /assets/images/svg-book14.svg
  overlay_color: "#000"
  overlay_filter: "0.3"
  caption: "background by [SVGBackgrounds.com](https://www.svgbackgrounds.com/)"
  #cta_label: "More Info"
  #cta_url: "https://unsplash.com"
categories:
  - algorithms
tags:
  - Algorithms
  - Selection Sort algorithm
last_modified_at: 2022-02-23T15:12:19-04:00
toc: true
toc_sticky: true
toc_label: "SAYFA İÇERİĞİ"
#classes: wide
---



**ÖNEMLİ :** Kendim için aldığım notlar. Umarım size de bir faydası olur. Kullanılan her bir makale referans olarak eklenmiştir.
{: .notice}

## Seçme Sıralaması (Selection Sort)

Aşağıdaki kart oyunu, **seçme algoritmasının**(selection algorithm) nasıl çalıştığını basitçe açıklamaktadır. Oyun çok basit; **dağınık kartları** <u>küçükten büyüğe</u> olacak şekilde birbirleriyle karşılaştırarak sıralamak istiyoruz.

Bunu yapmak için; öncelikle **ilk konumdaki** öğeyle başlamalı ve **en küçük** öğeyi bulmak için bu öğeyi diğer öğelerle karşılaştırmalıyız. Bu durumda ilk öğemiz, varsa, listedeki en küçük öğenin yerini alacağı için bizim referans değerimiz olacaktır. <u>Her neyse, listedeki ilk konum dışındaki en küçük öğeyi bulursak, onu ilk öğeyle <b>değiştirmemiz</b> gerekir(tabii ki bulunan bu değer ilk öğeden küçükse!).</u> Listedeki en küçük öğeyi ilk pozisyona yerleştirdikten sonra benzer işlemleri 2. pozisyondan başlayarak devam edeceğiz. Bu durumda ise referansımız 2.öğe olur. Karşılaştırma, ve varsa yer değiştirme işlemleri bittikten sonra, benzer işlemleri sırasıyla listenin **sondan bir önceki** elemanına kadar devam ettirmemiz gerekiyor. Özetle seçme sıralaması algoritması bu şekilde gerçekleşir.

<!-- <div class="notice--warning" markdown="1">
<h4 class="no_toc"><i class="fas fa-comment"></i> Not:</h4>
---
Eğer algoritmanın nasıl çalıştığını aşama aşama görmek istiyorsanız, **STEP** butonuna basabilirsiniz.

Ya da kart oyununu kendi kendine çalıştırmak için **PLAY** butonuna basın.

Son olarak, kartları tekrar karıştırmak ve oyunu yeniden başlatmak isterseniz **SHUFFLE** düğmesine basabilirsiniz.

</div> -->

<!-- <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2022-11-11-algorithms-part1-selection-sort/card.gif" srcset="{{ site.url }}{{ site.baseurl }}/assets/images/2022-11-11-algorithms-part1-selection-sort/card-small.gif 480w, {{ site.url }}{{ site.baseurl }}/assets/images/2022-11-11-algorithms-part1-selection-sort/card.gif 1080w" sizes="50vw" width="420px" height="100%" class="align-center" loading="lazy" alt="Selection Sort Algorithm"> -->

<video autoplay loop muted playsinline width="380px" height="100%" class="align-center" title="Selection Sort Algorithm">
  <source src="{{ site.url }}{{ site.baseurl }}/assets/images/2022-11-11-algorithms-part1-selection-sort/selection-sort.webm" type="video/webm">
  <source src="{{ site.url }}{{ site.baseurl }}/assets/images/2022-11-11-algorithms-part1-selection-sort/selection-sort.mp4" type="video/mp4">
</video>

<!-- <iframe sandbox="allow-popups allow-same-origin allow-scripts allow-top-navigation" src="https://www.khanacademy.org/computer-programming/program/4808854910533632/embedded?embed=yes&amp;author=no&amp;editor=no&amp;width=688&amp;buttons=no&amp;settings=%7B%22sortType%22%3A%22selection%22%7D" class="perseus-scratchpad" allowfullscreen="" style="height: 450px; width: 100%; border-top-width: 0px;
border-right-width: 0px;
border-bottom-width: 0px;
border-left-width: 0px;" title="Selection Sort Algorithm"></iframe> -->

### Seçme Sıralamasına Bir Örnek

Örneğin, bunun gibi sıralanmamış bir listemiz var;

<span style="black: blue"><b>9 - 4 - 3 - 1</b></span>
{: .text-left}

#### Amacımız

Sayıları birbiriyle karşılaştırarak küçükten büyüğe doğru sıralamak.

#### Seçme sıralama algoritması nasıl uygulanır
---

```java
public class SelectionSort {
    public static void main(String[] args) {
        int[] list = {9,4,3,1};
        for(int i=0; i< list.length-1; i++) {
            boolean hasSmallestBeenFounded = false;
            int smallest = i;
            for(int j=i; j< list.length-1; j++){
                if (list[j + 1] < list[smallest]) {
                    smallest = j + 1;
                    hasSmallestBeenFounded = true;
                }
            }
            if(hasSmallestBeenFounded){
                int tempValue = list[i];
                list[i] = list[smallest];
                list[smallest] = tempValue;
            }
        }
        //Arrays.stream(list).forEach(value -> System.out.print(value + " - "));
    }
}
```
---

1. **Seçme sıralamasını** listeme uygulamak için iki tane iç içe geçmiş **for döngüsüne** ihtiyacım var(elbette for döngüsü kullanmak şart değildir). **Dıştaki for döngüm**, <u>tutmak istediğim konumu</u> takip ederek listenin başından başlar(yani yukarıdaki tanıma göre bizim referans değerimiz oluyor). Algoritmanın en başında bu konum elbette "**0**" olacaktır. Öte yandan, **içteki for döngüm** ise, **en küçük öğeyi** bulmak için dıştaki for döngüsünün tuttuğu konumdaki öğe dışındaki diğer öğeleri kontrol eder.
2. Bulunan her yeni en küçük değer için int “**smallest**” değeri içteki for döngüsü boyunca güncellenecektir.
3. "**smallest**" ve "**hasSmallestBeenFounded**" değerlerinin siz içteki for döngüsüne girmeden hemen önce güncellendiğini fark edeceksiniz. Çünkü içteki for döngüsünün amacı en küçük değeri bulmak ve referans değerle karşılaştırmaktır. Şayet içteki döngünün dışına çıktığımızda, bu değerlerin görevlerini yerine getirmiş olduğunu varsayarak, bu iki değeri sonraki en küçük değeri bulmak için sıfırlarız.
4. İçteki for döngüsü çalışmasını bitirdiğinde, en azından bir tane bile en küçük değer bulunsa, "**hasSmallestBeenFounded**" boolean değeri `true` olarak işaretlenir. Çünkü içteki döngü boyunca **smallest** değeri değişebilir. Şayet, en küçük değer bulunmazsa, "**hasSmallestBeenFounded**" değeri `false` olarak kalır ve döngünün dışındaki "**if**" ifadesinin içine girilmez.
5. İçteki döngünün içinde bir "**if**" ifadesi var, **if (list[j + 1] < list[smallest])**. Burada, **smallest** değer ile smallest değerin hemen sonrasındaki değer karşılaştırılır. Daha küçük bir değerle karşılaşırsak, bu değeri yeni en küçük değere güncelleriz.
6. İçteki döngü, bir tane bile en küçük değer bulsa, içteki döngünün dışında bulunan "**if**" ifadesinin içine girer, bu da bulunan en küçük değeri ilk konumdaki değerle değiştirir (ki bu elbette dış döngü aracılığıyla tuttuğum konumdur)).
7. Dış döngümün ilk konumu bulunan en küçük değerle(varsa) değiştirildikten sonra, **i** sayısını **bir** artırarak dış döngümün 2. konumu ile aynı işlemi tekrarlarız. Daha sonra dış döngümün **sondan bir önceki konumuna(list.length-1)** kadar aynı işlemler devam eder.

----

<div class="notice--warning" markdown="1">
<h4 class="no_toc"><i class="fas fa-comment"></i> Not:</h4>
---
Çevrimiçi araçta kodu adım adım çalıştırmak isterseniz aşağıdaki bağlantıya tıklayabilirsiniz.

[link](https://pythontutor.com/visualize.html#code=public%20class%20SelectionSort%20%7B%0A%20%20%20%20public%20static%20void%20main%28String%5B%5D%20args%29%20%7B%0A%20%20%20%20%20%20%20%20int%5B%5D%20list%20%3D%20%7B9,4,3,1%7D%3B%0A%20%20%20%20%20%20%20%20for%28int%20i%3D0%3B%20i%3C%20list.length-1%3B%20i%2B%2B%29%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20boolean%20hasSmallestBeenFounded%20%3D%20false%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20int%20smallest%20%3D%20i%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20for%28int%20j%3Di%3B%20j%3C%20list.length-1%3B%20j%2B%2B%29%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20if%20%28list%5Bj%20%2B%201%5D%20%3C%20list%5Bsmallest%5D%29%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20smallest%20%3D%20j%20%2B%201%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20hasSmallestBeenFounded%20%3D%20true%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20if%28hasSmallestBeenFounded%29%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20int%20tempValue%20%3D%20list%5Bi%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20list%5Bi%5D%20%3D%20list%5Bsmallest%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20list%5Bsmallest%5D%20%3D%20tempValue%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%0A%7D&cumulative=false&curInstr=2&heapPrimitives=nevernest&mode=display&origin=opt-frontend.js&py=java&rawInputLstJSON=%5B%5D&textReferences=false)

Yukarıdaki kodda, hem sıralanmış hem de sıralanmamış liste kullanmak yerine sadece bir liste kullandım. Yani kendi içinde tek bir listeyi sıralayarak çözüme ulaştım. Dilerseniz bu şekilde 2 alt listeye ayırabilirsiniz;

| Sıralanmış alt-liste | Sıralanmamış alt-liste | Sıralanmamış listedeki en küçük öğe |
|:--------|:-------:|--------:|
| ()   | (9,4,3,1)   | 1   |
| (1)   | (9,4,3)   | 3   |
| (1,3)   | (9,4)   | 4   |   
| (1,3,4)   | (9)   | 9   |   
| (1,3,4,9)   | ()   |    |   
{: rules="groups"}

</div>

<!-- But sometimes embedded code may crash due to **massive traffic overload**, if you cannot see the code properly, you can click the  -->

<!-- <iframe width="100%" height="800" frameborder="0" src="https://pythontutor.com/iframe-embed.html#code=public%20class%20SelectionSort%20%7B%0A%20%20%20%20public%20static%20void%20main%28String%5B%5D%20args%29%20%7B%0A%20%20%20%20%20%20%20%20int%5B%5D%20list%20%3D%20%7B9,4,3,1%7D%3B%0A%20%20%20%20%20%20%20%20int%20smallest%20%3D%200%3B%0A%20%20%20%20%20%20%20%20int%20firstPosition%20%3D%200%3B%0A%20%20%20%20%20%20%20%20boolean%20hasSmallestBeenFounded%20%3D%20false%3B%0A%20%20%20%20%20%20%20%20for%28int%20i%3D0%3B%20i%3C%20list.length-1%3B%20i%2B%2B%29%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20hasSmallestBeenFounded%3Dfalse%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20smallest%20%3D%20i%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20for%28int%20j%3Di%3B%20j%3C%20list.length-1%3B%20j%2B%2B%29%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20if%20%28list%5Bj%20%2B%201%5D%20%3C%20list%5Bsmallest%5D%29%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20smallest%20%3D%20j%20%2B%201%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20hasSmallestBeenFounded%20%3D%20true%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20if%28hasSmallestBeenFounded%29%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20firstPosition%20%3D%20list%5Bi%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20list%5Bi%5D%20%3D%20list%5Bsmallest%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20list%5Bsmallest%5D%20%3D%20firstPosition%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%0A%7D&codeDivHeight=1100&codeDivWidth=510&cumulative=false&curInstr=2&heapPrimitives=nevernest&origin=opt-frontend.js&py=java&rawInputLstJSON=%5B%5D&textReferences=false"> </iframe> -->

## Seçme Sıralamasının Zaman Karmaşıklığı Nedir? (Time Complexity of Selection Sort?)

Seçme sıralamasının zaman verimliliği ikinci derecedendir(quadratic).

### Seçme Sıralamasının En İyi Durum Zaman Karmaşıklığı

\\(O(n^{2})\\) karşılaştırma, \\( O(1) \\) yer değiştirme.

En iyi durum zaman karmaşıklığında, listenin zaten sıralı olduğunu düşünürüz. Yer değiştirme olmayacağı için **O(n)** **1** olur. Ancak listenin sıralı olup olmadığını öğrenmek için her durumda **karşılaştırma** olacaktır. Bu **quadratic** zaman karmaşıklığını beraberinde getirir, yani, \\(O(n^{2})\\). Çünkü **2** tane iç içe **for** döngümüz bulunmaktadır.

### Seçme Sıralamasının En Kötü Durum Zaman Karmaşıklığı

\\(O(n^{2})\\) karşılaştırma, \\( O(n) \\) yer değiştirme.

Yazılım geliştiriciler genellikle sadece **en kötü durumun çalışma zamanını** bulmak üzerine yoğunlaşır çünkü **n** boyutlu herhangi bir girdi için en uzun çalışma zamanı odur. Tıpkı en iyi durum zaman karmaşıklığında olduğu gibi, **karşılaştırma** ikinci dereceden(**quadratic**) zaman karmaşıklığında gerçekleşir. Ama en kötü senaryoda elbette listemiz **sıralı olmayacak**. Çünkü en kötü senaryo bunu gerektirir. Bu yüzden **yer değiştirme** \\( O(n) \\) zamanda gerçekleşir.

### Seçme Sıralamasının Ortalama Durum Zaman Karmaşıklığı

\\(O(n^{2})\\) karşılaştırma, \\( O(n) \\) yer değiştirme.

Ortalama süredeki adım sayısı en kötü durumun **yarısı** olsa bile sabitler(constants) formülasyonda dikkate alınmayacağından sonuç yine en kötü durumla aynı olacaktır.

<div class="notice--warning" markdown="1">
<h4 class="no_toc"><i class="fas fa-comment"></i> Note:</h4>
---
Bir algoritmanın **en kötü durum çalışma zamanı** bize herhangi bir girdinin çalışma zamanı hakkında bir **üst sınır(upper bound)** verir. Bu, algoritmanın asla sürmeyeceği zamanı bilmeyi garanti eder.

Bunun yanı sıra, en iyi, en kötü ve ortalama durum zaman karmaşıklıklarını tanımlarken, \\(O(n^{2})\\) karşılaştırma, \\( O(n) \\) yer değiştirme, şeklinde ifade etsek de, her zaman **dominant terim** dikkate alınır. Bu yüzden;

\\(O(n^{2})\\) karşılaştırma, \\( O(n) \\) yer değiştirme için dominant terim: \\(O(n^{2})\\)

</div>

## Referanslar:

* [selection sort pseudocode](https://www.khanacademy.org/computing/computer-science/algorithms/sorting-algorithms/a/selection-sort-pseudocode)
* [selection sort wiki](https://en.wikipedia.org/wiki/Selection_sort)
* [Introduction to Algorithms third edition](https://en.wikipedia.org/wiki/Introduction_to_Algorithms)
