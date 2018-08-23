---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
title: 'Część 3: Układ i Menu kategorii | Dokumentacja firmy Microsoft'
author: JoeStagner
description: W tej serii samouczków zawiera szczegóły wszystkich kroków kompilacji Przykładowa aplikacja Tailspin Spyworks. Część 3 obejmuje dodawanie układ i menu kategorii.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 94ea1a70-a9bc-4241-8f36-08366d64bab9
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
msc.type: authoredcontent
ms.openlocfilehash: f55b29a271dbdb72d3e2249ed74517b77d78cf5e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755764"
---
<a name="part-3-layout-and-category-menu"></a>Część 3: Układ i Menu kategorii
====================
przez [Stagner Jan](https://github.com/JoeStagner)

> Tailspin Spyworks pokazuje, jak bardzo łatwo jest tworzyć zaawansowane, skalowalne aplikacje dla platformy .NET. Przedstawia on poza sposób użycia wspaniałych nowych funkcjach w ASP.NET 4 do tworzenia sklep online, m.in. zakupy wyewidencjonowanie i Administracja.
> 
> W tej serii samouczków zawiera szczegóły wszystkich kroków kompilacji Przykładowa aplikacja Tailspin Spyworks. Część 3 obejmuje dodawanie układ i menu kategorii.


## <a id="_Toc260221669"></a>  Dodanie niektórych układ i Menu kategorii

Strona wzorcowa naszej witryny polega na dodaniu div kolumny po lewej stronie, która będzie zawierać nasze menu kategorii produktów.

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample1.aspx)]

Należy pamiętać, że żądane wyrównanie i inne elementy formatowania będzie świadczona przez klasy CSS, która dodaliśmy do naszego pliku Style.css.

[!code-css[Main](tailspin-spyworks-part-3/samples/sample2.css)]

Menu kategorii produktu zostanie dynamicznie utworzony w czasie wykonywania, badając Commerce bazy danych dla istniejącej kategorii produktów oraz tworzenie elementów menu i odpowiadające łączy.

W tym celu użyjemy dwóch ASP. Formanty zaawansowanych danych przez sieć. Formantem "Źródła danych jednostki" i "ListView".

![](tailspin-spyworks-part-3/_static/image1.jpg)

Teraz przejdź do "W widoku projektu" i umożliwia konfigurowanie kontrolek naszych pomocników.

![](tailspin-spyworks-part-3/_static/image2.jpg)

Teraz ustaw właściwość Identyfikatora EntityDataSource do zewnętrznego źródła danych\_kategorii\_Menu i kliknij przycisk "Konfiguruj źródła danych".

![](tailspin-spyworks-part-3/_static/image3.jpg)

Wybierz połączenie CommerceEntities, który został utworzony dla nas podczas tworzenia modelu źródła danych jednostki dla naszych Commerce bazy danych, a następnie kliknij przycisk "Dalej".

![](tailspin-spyworks-part-3/_static/image4.jpg)

Wybierz jednostki "Kategorie" Nazwa zestawu i pozostaw resztę opcji jako domyślny. Kliknij przycisk "Zakończ".

Teraz możemy ustawić właściwość identyfikator wystąpienie kontrolki ListView, który możemy umieścić na naszej stronie ListView\_ProductsMenu i uaktywnić jego pomocy.

![](tailspin-spyworks-part-3/_static/image5.jpg)

Mimo że moglibyśmy użyć różnych opcji kontroli do formatowania wyświetlania elementu danych i formatowania, nasze menu tworzenia będzie wymagać tylko proste znaczników, dzięki czemu możemy wprowadzić kod w widoku źródła.

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample3.aspx)]

Należy pamiętać, instrukcji "Eval": &lt;% # Eval("CategoryName") %&gt;

Składnia ASP.NET &lt;% # %&gt; jest Konwencja skrót, który powoduje, że środowisko uruchomieniowe do wykonania, niezależnie od rodzaju znajduje się w obrębie i zapisuje wyniki "w wierszu".

Instrukcja Eval("CategoryName") powoduje, że, bieżącego wpisu w powiązanej kolekcji elementów danych, pobrać wartości nazw elementów modelu Entity "CatagoryName". Jest to zwarta składnia bardzo zaawansowaną funkcją.

Uruchomimy aplikację teraz.

![](tailspin-spyworks-part-3/_static/image6.jpg)

Pamiętaj, że nasze menu kategorii produktu są wyświetlane i kiedy jednak umieścimy kursor nad jednym z elementów menu kategorii widać prowadzi link element menu na stronie mamy jeszcze do wdrożenia o nazwie ProductsList.aspx a, stworzyliśmy argumentu ciągu zapytania dynamicznego, zawiera  Identyfikator kategorii.

> [!div class="step-by-step"]
> [Poprzednie](tailspin-spyworks-part-2.md)
> [dalej](tailspin-spyworks-part-4.md)
