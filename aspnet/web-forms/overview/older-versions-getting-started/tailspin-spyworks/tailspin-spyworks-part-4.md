---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
title: "Część 4: Wyświetlanie listy produktów | Dokumentacja firmy Microsoft"
author: JoeStagner
description: "Ta seria samouczek zawiera szczegóły dotyczące wszystkich kroków kompilacji Tailspin Spyworks przykładowej aplikacji. Wyświetlanie listy produktów z widoku GridView zysk obejmuje część 4."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 4fab47d5-a6ec-4fdc-91f0-651a093a24b9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
msc.type: authoredcontent
ms.openlocfilehash: 420cdbcc002bcbfff619d399a7a374009999d754
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="part-4-listing-products"></a>Część 4: Lista produktów
====================
przez [Stagner Jan](https://github.com/JoeStagner)

> Tailspin Spyworks pokazano, jak bardzo proste jest tworzenie zaawansowanych, skalowalnych aplikacji dla platformy .NET. Przedstawia on poza jak nowe, fantastyczne funkcje programu ASP.NET 4 do tworzenia sklepu online, łącznie z zakupów, wyewidencjonowania i administracji.
> 
> Ta seria samouczek zawiera szczegóły dotyczące wszystkich kroków kompilacji Tailspin Spyworks przykładowej aplikacji. Część 4 zawiera listę produktów za pomocą formantu widoku GridView.


## <a id="_Toc260221670"></a>Wyświetlanie listy produktów z formantem widoku GridView

Zacznijmy Implementowanie naszą stronę ProductsList.aspx "Kliknąć prawym przyciskiem myszy" na naszych rozwiązań i wybierając polecenie "Dodaj" i "Nowy element".

![](tailspin-spyworks-part-4/_static/image1.jpg)

Wybierz pozycję "Strony sieci Web formularza za pomocą wzorca" i wprowadź nazwę strony ProductsList.aspx".

Kliknij przycisk "Dodaj".

![](tailspin-spyworks-part-4/_static/image2.jpg)

Następnie wybierz folder "Style", gdzie możemy umieścić strony Site.Master i wybierz ją z okna "Zawartość folderu".

![](tailspin-spyworks-part-4/_static/image3.jpg)

Kliknij przycisk "Ok", aby utworzyć stronę.

Naszej bazie danych jest wypełniana danych produktu, jak pokazano poniżej.

![](tailspin-spyworks-part-4/_static/image4.jpg)

Po utworzeniu naszą stronę ponownie użyjemy Entity Data Source dostępu do danych tego produktu, ale w tym wystąpieniu musimy wybierz jednostek produktu i musimy ograniczanie elementów, które są zwracane tylko do tych dla wybranej kategorii.

W tym celu możemy poinformować obiektu EntityDataSource do automatycznego generowania klauzuli WHERE i firma Microsoft będzie określić WhereParameter.

Będziesz się odwołać, że gdy utworzyliśmy elementów Menu w naszym "produktu kategorii Menu" dynamicznie budujemy łącza przez dodanie CatagoryID na ciąg zapytania dla każdego łącza. Firma Microsoft powiadomi Entity Data Source pochodzić z parametru QueryString parametru WHERE.

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample1.aspx)]

Dalej skonfigurujemy formantu ListView, aby wyświetlić listę produktów. Aby utworzyć zapewnienia optymalnego działania zakupów, firma Microsoft będzie compact kilka funkcji zwięzły do każdego produktu poszczególne wyświetlane w naszym ListVew.

- Nazwa produktu będzie link do tego produktu: widok szczegółów.
- Wyświetlane są ceny produktu.
- Obraz produktu będą wyświetlane i dynamicznie wybierzemy obrazu z katalogu katalog obrazów w naszej aplikacji.
- Firma Microsoft będzie zawierać łącze do natychmiast dodać określonego produktu do koszyka.

Oto kod znaczników dla naszych wystąpienia formantu ListView.

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample2.aspx)]

Firma Microsoft są dynamiczne tworzenie kilku łączy dla każdego produktu wyświetlane.

Ponadto aby przetestować własnych nowej strony należy utworzyć struktury katalogu dla produktu katalog obrazów w następujący sposób.

![](tailspin-spyworks-part-4/_static/image1.png)

Po zatwierdzeniu dostępne obrazy naszego produktu można było przetestować naszą stronę produktu listy.

![](tailspin-spyworks-part-4/_static/image5.jpg)

Na stronie głównej witryny kliknij jeden z linków listy kategorii.

![](tailspin-spyworks-part-4/_static/image6.jpg)

Teraz musimy zaimplementować stronę ProductDetials.apsx oraz funkcje AddToCart.

Użyj pliku -&gt;nowy, aby utworzyć nazwę strony ProductDetails.aspx przy użyciu witryny strony wzorcowej, jak robiliśmy wcześniej.

Ponownie używamy formantu obiektu EntityDataSource uzyskać dostęp do określonego produktu rekordu w bazie danych i użyjemy kontrolkę ASP.NET FormView do wyświetlania danych produktu w następujący sposób.

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample3.aspx)]

Nie martw się, jeśli formatowanie wygląda nieco Rady do Ciebie. Kod znaczników powyżej pozostawia miejsce w układzie wyświetlania z kilku funkcji, które będą później wprowadzania.

Koszyku będzie reprezentować bardziej złożonej logice w naszej aplikacji. Aby rozpocząć pracę, należy użyć pliku -&gt;nowy, aby utworzyć stronę o nazwie MyShoppingCart.aspx.

Należy pamiętać, że firma Microsoft nie wybierania nazwy ShoppingCart.aspx.

Nasze baza danych zawiera tabelę o nazwie "ShoppingCart". Gdy firma Microsoft wygenerowany modelu Entity Data Model klasy został utworzony dla każdej tabeli w bazie danych. W związku z tym modelu Entity Data Model wygenerowane klasy jednostki o nazwie "ShoppingCart". Firma Microsoft może edytować model, tak, aby firma Microsoft może używać tej nazwy naszych stosowania koszyka zakupów lub jej rozszerzenia na naszych potrzeb, ale firma Microsoft będzie wybierze zamiast tego po prostu wybierz nazwę, która pozwoli uniknąć konfliktu.

Warto również zauważyć, że firma Microsoft będzie Tworzenie prostego koszyk i osadzanie logiki koszyka zakupów z ekranem koszyka zakupów. Firma Microsoft może też do zaimplementowania naszego koszyka sklepowego całkowicie osobnej warstwie Business.

>[!div class="step-by-step"]
[Poprzednie](tailspin-spyworks-part-3.md)
[dalej](tailspin-spyworks-part-5.md)
