---
uid: mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-cs
title: Za pomocą klasa TagBuilder do tworzenia pomocników HTML (C#) | Dokumentacja firmy Microsoft
author: StephenWalther
description: 'Autor: Stephen Walther wprowadza do klasy przydatne narzędzia w platformę ASP.NET MVC, nazwana klasa TagBuilder. Klasa TagBuilder do mogą używać łatwo...'
ms.author: aspnetcontent
ms.date: 03/02/2009
ms.assetid: 3975a52f-bd15-4edd-8f3d-1df93672515b
msc.legacyurl: /mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-cs
msc.type: authoredcontent
ms.openlocfilehash: 9990fc7ad8093643a564a5e02ff65264d4a7fe15
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37813229"
---
<a name="using-the-tagbuilder-class-to-build-html-helpers-c"></a>Za pomocą klasa TagBuilder do tworzenia pomocników HTML (C#)
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

**Wyświetlanie listy 1 - Helpers\ImageHelper.cs**

[!code-csharp[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample1.cs)]

Klasa w ofercie 1 zawiera dwa statyczne przeciążone metody o nazwie obrazu. Po wywołaniu metody Image(), można przekazać do obiektu, który reprezentuje zestaw atrybutów HTML, czy nie.

Zwróć uwagę, jak metoda TagBuilder.MergeAttribute() służy do Dodaj poszczególne atrybuty, takie jak atrybut src do TagBuilder. Zwróć uwagę, co więcej, jak metoda TagBuilder.MergeAttributes() służy do dodawania Kolekcja atrybutów do TagBuilder. Metoda MergeAttributes() akceptuje słownik&lt;ciąg, obiekt&gt; parametru. Klasa RouteValueDictionary jest używana do konwersji obiekt reprezentujący kolekcję atrybutów do słownika&lt;ciąg, obiekt&gt;.

Po utworzeniu Pomocnik obrazu, można użyć pomocnika, w sekcji Widoki ASP.NET MVC, podobnie jak inne standardowe pomocników HTML. Wyświetl w ofercie 2 użyto Pomocnik obrazu, aby wyświetlić dwa razy ten sam obraz Xbox (patrz rysunek 1). Pomocnik Image() nazywa się zarówno z i bez Kolekcja atrybutów HTML.

**Wyświetlanie listy 2 - Home\Index.aspx**

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample2.aspx)]


[![Okno dialogowe Nowy projekt](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.jpg)](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.png)

**Rysunek 01**: przy użyciu Pomocnika obrazu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image2.png))


Zwróć uwagę, należy zaimportować skojarzone z elementem pomocniczym obrazu w górnej części widoku Index.aspx przestrzeni nazw. Pomocnik jest importowany z następującą dyrektywę:

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample3.aspx)]

> [!div class="step-by-step"]
> [Poprzednie](creating-custom-html-helpers-cs.md)
> [dalej](creating-page-layouts-with-view-master-pages-cs.md)
