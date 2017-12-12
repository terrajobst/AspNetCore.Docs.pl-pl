---
uid: mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-cs
title: "Za pomocą klasy TagBuilder do tworzenia pomocników HTML (C#) | Dokumentacja firmy Microsoft"
author: StephenWalther
description: "Stephen Walther przedstawiono klasy przydatne narzędzia w nazwie klasy TagBuilder platformę ASP.NET MVC. Można użyć klasy TagBuilder można łatwo..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 3975a52f-bd15-4edd-8f3d-1df93672515b
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-cs
msc.type: authoredcontent
ms.openlocfilehash: ddc4e91bb14082c7c5e889d064d29d2bf91f7329
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="using-the-tagbuilder-class-to-build-html-helpers-c"></a>Za pomocą klasy TagBuilder do tworzenia pomocników HTML (C#)
====================
przez [Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther przedstawiono klasy przydatne narzędzia w nazwie klasy TagBuilder platformę ASP.NET MVC. Klasa TagBuilder umożliwia łatwe tworzenie tagów HTML.


Platforma ASP.NET MVC zawiera klasę przydatne narzędzie o nazwie klasy TagBuilder, której można użyć podczas kompilowania pomocników HTML. Klasa TagBuilder, jak nazwa klasy sugeruje, umożliwia łatwe tworzenie tagów HTML. W tym samouczku krótki podano przegląd klasy TagBuilder i jak przy użyciu tej klasy, podczas tworzenia prostych pomocnika kodu HTML, który renderuje HTML &lt;img&gt; tagów.

## <a name="overview-of-the-tagbuilder-class"></a>Przegląd klasy TagBuilder

Klasa TagBuilder znajduje się w przestrzeni nazw System.Web.Mvc. Składa się z pięciu metod:

- AddCssClass() — umożliwia dodanie nowego *klasy = ""* atrybut do tagu.
- GenerateId() — umożliwia dodanie atrybutu id znacznika. Ta metoda automatycznie zastępuje kropki w identyfikatorze (domyślnie okresów zastępuje znaki podkreślenia)
- MergeAttribute() — pozwala dodać atrybuty do tagu. Istnieje wiele przeciążenie tej metody.
- SetInnerText() — pozwala ustawić tekst wewnętrzny tagu. Tekst wewnętrzny jest automatycznie kodowanie HTML.
- ToString() — umożliwia renderowania tagu. Można określić, czy chcesz utworzyć tag normalne, tagu początkowego, tagu końcowego lub tagu samozamykającego.
  

Klasa TagBuilder ma cztery ważne właściwości:

- Atrybuty — reprezentuje wszystkie atrybuty tagu.
- Idatributedotreplacement ma wartość - reprezentuje znak używany przez metodę GenerateId() do zastępowania kropki (domyślnie jest to znak podkreślenia).
- InnerHTML - reprezentuje wewnętrzny zawartości znacznika. Przypisanie do tej właściwości ciąg *nie* ciąg kodowanie HTML.
- TagName - reprezentuje nazwę znacznika.

Te metody i właściwości do wszystkich podstawowe metody i właściwości, które są potrzebne do zbudowania tagu HTML. Naprawdę nie trzeba używać klasy TagBuilder. Zamiast tego można użyć klasy StringBuilder. Jednak klasy TagBuilder ułatwia życie nieco.

## <a name="creating-an-image-html-helper"></a>Tworzenie obrazu pomocnika kodu HTML

Podczas tworzenia wystąpienia klasy TagBuilder przekazujesz nazwa tagu, który chcesz utworzyć TagBuilder konstruktora. Następnie można wywołać metody, takie jak AddCssClass i MergeAttribute() metody służące do modyfikowania atrybutów tagu. Na koniec należy wywołać metodę ToString() do renderowania tagu.

Na przykład 1 Lista zawiera pomocnika kodu HTML z obrazu. Pomocnik obrazu jest implementowana wewnętrznie przy użyciu TagBuilder, który reprezentuje HTML &lt;img&gt; tagu.

**Wyświetlanie listy 1 - Helpers\ImageHelper.cs**

[!code-csharp[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample1.cs)]

Klasa w 1 Lista zawiera dwa statycznego przeciążonych metod o nazwie obrazu. Po wywołaniu metody Image() można przekazać do obiektu, który reprezentuje zestaw atrybutów HTML, czy nie.

Zwróć uwagę, jak metoda TagBuilder.MergeAttribute() jest używana do dodawania poszczególnych atrybutów, takich jak atrybutu src do TagBuilder. Zwróć uwagę, ponadto jak metoda TagBuilder.MergeAttributes() jest używana do dodawania Kolekcja atrybutów do TagBuilder. Metoda MergeAttributes() przyjmuje słownik&lt;string, object&gt; parametru. Klasa RouteValueDictionary służy do konwertowania obiekt reprezentujący kolekcję atrybutów do słownika&lt;string, object&gt;.

Po utworzeniu pomocy dotyczącej obrazów, można użyć pomocnika, w sekcji Widoki ASP.NET MVC, podobnie jak inne standardowe pomocników HTML. Widok wyświetlania 2 używa pomocy dotyczącej obrazów do wyświetlania dwa razy ten sam obraz Xbox (zobacz rysunek 1). Pomocnik Image() nazywa się zarówno z i bez Kolekcja atrybutów HTML.

**Wyświetlanie listy 2 - Home\Index.aspx**

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample2.aspx)]


[![Okno dialogowe nowego projektu](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.jpg)](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.png)

**Rysunek 01**: przy użyciu Pomocnika obrazu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image2.png))


Zwróć uwagę, importowania skojarzone z pomocy dotyczącej obrazów w górnej części widoku Index.aspx przestrzeni nazw. Pomocnik jest importowany z dyrektywą następujące:

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample3.aspx)]

>[!div class="step-by-step"]
[Poprzednie](creating-custom-html-helpers-cs.md)
[dalej](creating-page-layouts-with-view-master-pages-cs.md)
