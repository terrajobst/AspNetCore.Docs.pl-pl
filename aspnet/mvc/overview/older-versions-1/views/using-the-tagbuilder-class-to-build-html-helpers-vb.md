---
uid: mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-vb
title: Za pomocą klasa TagBuilder do tworzenia pomocników HTML (VB) | Dokumentacja firmy Microsoft
author: StephenWalther
description: 'Autor: Stephen Walther wprowadza do klasy przydatne narzędzia w platformę ASP.NET MVC, nazwana klasa TagBuilder. Klasa TagBuilder do mogą używać łatwo...'
ms.author: riande
ms.date: 03/02/2009
ms.assetid: ec26f264-d0ea-4031-9943-825505a3ac4b
msc.legacyurl: /mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-vb
msc.type: authoredcontent
ms.openlocfilehash: 783c5f73709de37f79c472e10c79e284cf25f8fd
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754712"
---
<a name="using-the-tagbuilder-class-to-build-html-helpers-vb"></a>Za pomocą klasa TagBuilder do tworzenia pomocników HTML (VB)
====================
przez [Walther Autor: Stephen](https://github.com/StephenWalther)

> Autor: Stephen Walther wprowadza do klasy przydatne narzędzia w platformę ASP.NET MVC, nazwana klasa TagBuilder. Klasa TagBuilder umożliwia łatwe tworzenie tagów HTML.


Platforma ASP.NET MVC zawiera klasę przydatne narzędzie nazwana klasa TagBuilder, używanego podczas tworzenia pomocników HTML. Klasa TagBuilder jak sugeruje nazwa klasy, można w łatwy sposób tworzyć tagi HTML. W tym samouczku krótki podano klasa TagBuilder do przeglądu i dowiesz się, jak używać tej klasy, podczas tworzenia prostego pomocnika kodu HTML, który renderuje HTML &lt;img&gt; tagów.

## <a name="overview-of-the-tagbuilder-class"></a>Omówienie klasa TagBuilder

Klasa TagBuilder znajduje się w przestrzeni nazw System.Web.Mvc. Ma pięć metod:

- AddCssClass() — umożliwia dodanie nowego *klasy = ""* atrybut w tagu.
- GenerateId() — umożliwia dodanie atrybutu identyfikator tagu. Ta metoda automatycznie zastępuje kropki w identyfikatorze (domyślnie parametr kropki są zastępowane przez znaki podkreślenia)
- MergeAttribute() — umożliwia dodanie atrybutów tagu. Istnieje wiele przeciążeń z tej metody.
- SetInnerText() — umożliwia ustawianie tekst zawarty wewnątrz tagu. Tekst wewnętrzny jest automatycznie kodowanie HTML.
- ToString() — umożliwia renderowania tagu. Można określić, czy chcesz utworzyć tag normalne, tagu początkowego, tagu końcowego lub tagu samozamykającego.
  

Klasa TagBuilder ma cztery właściwości ważne:

- Atrybuty — reprezentuje wszystkie atrybuty znacznika.
- Idatributedotreplacement — reprezentuje znak używany przez metodę GenerateId() do zastępowania kropki (wartość domyślna to znaku podkreślenia).
- InnerHTML — reprezentuje wewnętrzny zawartości znacznika. Przypisywanie ciąg do tej właściwości *nie* ciąg kodowanie HTML.
- TagName — reprezentuje nazwę tagu.

Te metody i właściwości zapewniają wszystkie podstawowe metody i właściwości, które są potrzebne do zbudowania potraktowane jak tag HTML. Tak naprawdę nie trzeba używać klasa TagBuilder. Można zamiast tego użyj klasy StringBuilder. Jednak klasa TagBuilder sprawia, że Twoja praca była nieco łatwiejsza.

## <a name="creating-an-image-html-helper"></a>Tworzenie obrazu pomocnika kodu HTML

Podczas tworzenia wystąpienia klasy TagBuilder możesz przekazywać nazwę jednostki tag, który chcesz skompilować do konstruktora TagBuilder. Następnie możesz wywołać metod, takich jak metody AddCssClass i MergeAttribute(), aby zmodyfikować atrybuty znacznika. Na koniec możesz wywołać metodę ToString() do renderowania tagu.

Na przykład wyświetlanie listy 1 zawiera pomocnika kodu HTML z obrazu. Pomocnik obrazu jest implementowane wewnętrznie z TagBuilder, który reprezentuje HTML &lt;img&gt; tagu.

**Wyświetlanie listy 1 – Helpers\ImageHelper.vb**

[!code-vb[Main](using-the-tagbuilder-class-to-build-html-helpers-vb/samples/sample1.vb)]

Moduł w ofercie 1 zawiera dwie przeciążone metody o nazwie Image(). Po wywołaniu metody Image(), można przekazać do obiektu, który reprezentuje zestaw atrybutów HTML, czy nie.

Zwróć uwagę, jak metoda TagBuilder.MergeAttribute() służy do Dodaj poszczególne atrybuty, takie jak atrybut src do TagBuilder. Zwróć uwagę, co więcej, jak metoda TagBuilder.MergeAttributes() służy do dodawania Kolekcja atrybutów do TagBuilder. Metoda MergeAttributes() akceptuje słownik&lt;ciąg, obiekt&gt; parametru. Klasa RouteValueDictionary jest używana do konwersji obiekt reprezentujący kolekcję atrybutów do słownika&lt;ciąg, obiekt&gt;.

Po utworzeniu Pomocnik obrazu, można użyć pomocnika, w sekcji Widoki ASP.NET MVC, podobnie jak inne standardowe pomocników HTML. Wyświetl w ofercie 2 użyto Pomocnik obrazu, aby wyświetlić dwa razy ten sam obraz Xbox (patrz rysunek 1). Pomocnik Image() nazywa się zarówno z i bez Kolekcja atrybutów HTML.

**Wyświetlanie listy 2 — Home\Index.aspx**

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-vb/samples/sample2.aspx)]


[![Okno dialogowe Nowy projekt](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image1.jpg)](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image1.png)

**Rysunek 01**: przy użyciu Pomocnika obrazu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image2.png))


Zwróć uwagę, należy zaimportować skojarzone z elementem pomocniczym obrazu w górnej części widoku Index.aspx przestrzeni nazw. Pomocnik jest importowany z następującą dyrektywę:

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-vb/samples/sample3.aspx)]

W aplikacji Visual Basic domyślny obszar nazw jest taka sama jak nazwa aplikacji.

> [!div class="step-by-step"]
> [Poprzednie](creating-custom-html-helpers-vb.md)
> [dalej](creating-page-layouts-with-view-master-pages-vb.md)
