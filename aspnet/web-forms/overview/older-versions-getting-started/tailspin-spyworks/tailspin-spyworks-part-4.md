---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
title: 'Część 4: Wyświetlanie listy produktów | Dokumentacja firmy Microsoft'
author: JoeStagner
description: W tej serii samouczków zawiera szczegóły wszystkich kroków kompilacji Przykładowa aplikacja Tailspin Spyworks. Część 4 obejmuje tworzenie listy produktów z zysk GridView...
ms.author: aspnetcontent
ms.date: 07/21/2010
ms.assetid: 4fab47d5-a6ec-4fdc-91f0-651a093a24b9
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
msc.type: authoredcontent
ms.openlocfilehash: ec349a2564a63fd950ca5f6a171e6ffd1199c28a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37813078"
---
<a name="part-4-listing-products"></a>Część 4: Tworzenie listy produktów
====================
przez [Stagner Jan](https://github.com/JoeStagner)

> Tailspin Spyworks pokazuje, jak bardzo łatwo jest tworzyć zaawansowane, skalowalne aplikacje dla platformy .NET. Przedstawia on poza sposób użycia wspaniałych nowych funkcjach w ASP.NET 4 do tworzenia sklep online, m.in. zakupy wyewidencjonowanie i Administracja.
> 
> W tej serii samouczków zawiera szczegóły wszystkich kroków kompilacji Przykładowa aplikacja Tailspin Spyworks. Część 4 obejmuje tworzenie listy produktów za pomocą kontrolki GridView.


## <a id="_Toc260221670"></a>  Tworzenie listy produktów za pomocą kontrolki GridView

Zacznijmy implementacji naszą stronę ProductsList.aspx przez "Kliknięcie prawym przyciskiem myszy" na nasze rozwiązanie i wybierając pozycję "Dodaj" i "Nowy element".

![](tailspin-spyworks-part-4/_static/image1.jpg)

Wybierz pozycję "Formularza przy użyciu wzorca stronę internetową" i wprowadź nazwę strony ProductsList.aspx".

Kliknij przycisk "Dodaj".

![](tailspin-spyworks-part-4/_static/image2.jpg)

Następnie wybierz folder "Style", w którym firma Microsoft umieszczone na stronie Site.Master i wybierz ją z okna "Zawartość folderu".

![](tailspin-spyworks-part-4/_static/image3.jpg)

Kliknij przycisk "Ok", aby utworzyć stronę.

Naszej bazie danych jest wypełniana danymi produktów, jak pokazano poniżej.

![](tailspin-spyworks-part-4/_static/image4.jpg)

Po naszej stronie ponownie użyjemy źródła danych jednostki do dostępu do tych danych produktu, ale w tym wystąpieniu musimy wybrać jednostki produktu i należy ograniczyć elementy, które są zwracane tylko do tych dla wybranej kategorii.

W tym celu będzie o EntityDataSource automatycznej klauzuli WHERE i określamy będzie WhereParameter.

Dowiesz się odwołać, podczas tworzenia elementów Menu w naszym "produktu kategorii Menu" dynamicznie zbudowaliśmy łącze, dodając CatagoryID do ciągu kwerendy dla każdego łącza. Firma Microsoft poinformuje źródła danych jednostki do wyprowadzenia parametr gdzie z tego parametru QueryString.

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample1.aspx)]

Następnie skonfigurujemy formantu ListView, aby wyświetlić listę produktów. Aby utworzyć zapewnienia optymalnego działania zakupów, firma Microsoft będzie compact kilka zwięzłe funkcji do każdego produktu wyświetlane w naszej ListVew.

- Nazwa produktu będzie łącze do widoku szczegółów produktu.
- Zostanie wyświetlona cena produktu.
- Obraz produktu będzie wyświetlany, i dynamicznie wybierzemy obrazu z katalogu katalog obrazów w naszej aplikacji.
- Firma Microsoft będzie zawierać link do natychmiast dodać określonego produktu do koszyka.

Oto znaczniki dla naszych wystąpienie kontrolki ListView.

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample2.aspx)]

Dynamicznie tworzymy kilka linków dla każdego produktu wyświetlane.

Ponadto przed przetestowanie własnych nową stronę musimy utworzyć katalog obrazów struktury katalogów dla produktu w następujący sposób.

![](tailspin-spyworks-part-4/_static/image1.png)

Po nasze obrazy produktów są dostępne możemy przetestować naszą stronę listy produktu.

![](tailspin-spyworks-part-4/_static/image5.jpg)

Na stronie głównej witryny kliknij jeden z linków listy kategorii.

![](tailspin-spyworks-part-4/_static/image6.jpg)

Teraz musimy zaimplementować stronę ProductDetials.apsx i funkcjonalność AddToCart.

Użyj pliku -&gt;nowy, aby utworzyć nazwę strony ProductDetails.aspx przy użyciu witryny stronę wzorcową, ile My mieliśmy wcześniej.

Ponownie użyjemy kontrolkę EntityDataSource uzyskać dostęp do rekordu określonego produktu w bazie danych, a następnie użyjemy kontrolkę ASP.NET FormView do wyświetlania danych produktu w następujący sposób.

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample3.aspx)]

Nie martw się, jeśli formatowanie wygląda nieco Rady do Ciebie. Powyższe znaczników pozostawia miejsce w układzie wyświetlana przez kilka funkcji, które firma Microsoft będzie implementowana w późniejszym czasie na.

Zawartość koszyka będzie reprezentować bardziej złożonej logiki w naszej aplikacji. Aby rozpocząć pracę, należy użyć pliku -&gt;nowy, aby utworzyć stronę o nazwie MyShoppingCart.aspx.

Należy pamiętać, że firma Microsoft nie wybierają nazwę ShoppingCart.aspx.

Nasze baza danych zawiera tabelę o nazwie "ShoppingCart". Gdy firma Microsoft wygenerowany model Entity Data Model klasy został utworzony dla każdej tabeli w bazie danych. W związku z tym modelu Entity Data Model wygenerować klasę jednostki o nazwie "ShoppingCart". Firma Microsoft można edytować model, dzięki czemu firma Microsoft może używać tej nazwy dla naszej zakupów implementacji koszyka lub rozszerzania jej do naszych potrzeb, ale firma Microsoft będzie zoptymalizowany pod kątem zamiast tego można po prostu wybierz nazwę, która pozwoli uniknąć konfliktu.

Warto również zauważyć, że utworzymy prostą koszyka zakupów i osadzanie logikę koszyka zakupów z wyświetlaniem koszyka zakupów. Firma Microsoft może też zaimplementować naszego koszyka w oddzielną warstwy biznesowej.

> [!div class="step-by-step"]
> [Poprzednie](tailspin-spyworks-part-3.md)
> [dalej](tailspin-spyworks-part-5.md)
