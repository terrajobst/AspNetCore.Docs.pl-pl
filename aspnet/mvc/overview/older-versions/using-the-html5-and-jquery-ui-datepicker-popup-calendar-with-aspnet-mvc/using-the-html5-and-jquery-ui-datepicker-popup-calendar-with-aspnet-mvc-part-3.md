---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
title: Używanie kalendarza podręcznego selektora daty interfejsu użytkownika jQuery i HTML5 platformie ASP.NET MVC — część 3 | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: W tym samouczku uczy podstaw sposób pracy z szablonami edytora, szablonów wyświetlania i kalendarza podręcznego selektora daty interfejsu użytkownika jQuery, w MV ASP.NET...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/29/2011
ms.topic: article
ms.assetid: 8f5f91ae-12d7-4cf3-ac09-4bb53d07ee60
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
msc.type: authoredcontent
ms.openlocfilehash: fd1ae746f4f134b779c7eee50cf6c840bbb7068e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-3"></a>Używanie kalendarza podręcznego selektora daty interfejsu użytkownika jQuery i HTML5 platformie ASP.NET MVC — część 3
====================
przez [Rick Anderson](https://github.com/Rick-Anderson)

> W tym samouczku uczy podstaw sposób pracy z szablonami edytora, szablonów wyświetlania i kalendarza podręcznego selektora daty interfejsu użytkownika jQuery, w aplikacji sieci Web programu ASP.NET MVC.


## <a name="working-with-complex-types"></a>Praca z typów złożonych

W tej sekcji możesz utworzyć klasę adresu i Dowiedz się, jak utworzyć szablon, aby go wyświetlić.

W *modele* folderu, Utwórz nowy plik klasy o nazwie *Person.cs* których będzie umieścić dwa typy: `Person` klasy i `Address` klasy. `Person` Klasy będzie zawierać właściwość, która zostanie wpisany jako `Address`. `Address` Typ jest typem złożonym, co oznacza nie jest jedną z wbudowanych typów, takie jak `int`, `string`, lub `double`. Zamiast tego ma kilka właściwości. Kod dla nowych klas wygląda następująco:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample1.cs)]

W `Movie` kontroler, Dodaj następujący `PersonDetail` akcji, aby wyświetlić wystąpienia osoba:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample2.cs)]

Następnie dodaj następujący kod, aby `Movie` kontrolera do wypełnienia `Person` modelu z przykładowymi danymi:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample3.cs)]

Otwórz *Views\Movies\PersonDetail.cshtml* i Dodaj następujący kod znaczników dla `PersonDetail` widoku.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample4.cshtml)]

Naciśnij klawisze Ctrl + F5, aby uruchomić aplikację i przejdź do *filmy/PersonDetail*.

`PersonDetail` Widok nie zawiera `Address` typu złożonego jako użytkownik widzi na tym zrzucie ekranu pokazano. (Adres nie jest widoczny).

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image1.png)

`Address` Modelu danych nie jest wyświetlane, ponieważ jest typem złożonym. Aby wyświetlić informacje o adresach, otwórz *Views\Movies\PersonDetail.cshtml* ponownie i Dodaj następujący kod znaczników.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample5.cshtml)]

Pełny kod znaczników dla `PersonDetail` teraz widoku wygląda następująco:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample6.cshtml)]

Uruchom aplikację ponownie i wyświetlić `PersonDetail` widoku. Informacje o adresie jest teraz wyświetlone:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image2.png)

### <a name="creating-a-template-for-a-complex-type"></a>Tworzenie szablonu typu złożonego

W tej sekcji utworzysz szablon, który będzie używany do renderowania `Address` typu złożonego. Po utworzeniu szablonu dla `Address` typu, ASP.NET MVC automatycznie służy do formatu adresu modelu w dowolnym miejscu aplikacji. Zapewnia to pozwala sterować renderowanie `Address` typu z tylko w jednym miejscu w aplikacji.

W *Views\Shared\DisplayTemplates* folderu, Utwórz silnie typizowanego widoku częściowego o nazwie **adres**:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image3.png)

Kliknij przycisk **Dodaj**, a następnie otwórz nową *Views\Shared\DisplayTemplates\Address.cshtml* pliku. Nowy widok zawiera następujące wygenerowany kod znaczników:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample7.cshtml)]

Uruchom aplikację i wyświetlić `PersonDetail` widoku. Teraz, `Address` właśnie utworzony szablon jest używany do wyświetlania `Address` typu złożonego, więc wyświetlanie wygląda następująco:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image4.png)

### <a name="summary-ways-to-specify-the-model-display-format-and-template"></a>Podsumowanie: Określ szablon i Format wyświetlania modelu metody

W tym samouczku można określić formatu lub szablon dla właściwości modelu w przy użyciu następujących metod:

- Stosowanie `DisplayFormat` atrybutu właściwości w modelu. Na przykład następujący kod powoduje, że data będzie wyświetlana bez czasu:

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample8.cs)]
- Stosowanie [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) atrybutu właściwości w modelu i określenie typu danych. Na przykład następujący kod powoduje, że data będzie wyświetlana bez czasu.

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample9.cs)]

    Jeśli aplikacja zawiera *date.cshtml* szablonu w *Views\Shared\DisplayTemplates* folderu lub *Views\Movies\DisplayTemplates* folder, w tym szablonie będzie używany do renderowania `DateTime` właściwości. W przeciwnym razie wbudowanych systemu tworzenia szablonów ASP.NET Wyświetla właściwość jako data.
- Tworzenie szablonu ekranu w *Views\Shared\DisplayTemplates* folderu lub *Views\Movies\DisplayTemplates* folder o nazwie zgodny z typem danych, które chcesz sformatować. Na przykład, które widać *Views\Shared\DisplayTemplates\DateTime.cshtml* został użyty do renderowania `DateTime` właściwości w modelu, bez dodawania atrybutu w modelu i bez dodawania żadnych znaczników do widoków.
- Przy użyciu [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) atrybutu w modelu, aby określić szablonu, aby wyświetlić właściwości modelu.
- Jawne Dodawanie nazwę wyświetlaną szablonu do [Html.DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) wywołań w widoku.

Podejście, którego używasz, zależy od tego, co należy zrobić w aplikacji. Nie jest rzadko kombinację obu rozwiązań można pobrać dokładnie rodzaj formatowania potrzebne.

W następnej sekcji, można przełączać narzędzi połowowych nieco i Przenieś możliwości dostosowywania wyświetlania danych do dostosowywania sposobu wprowadzania. Będzie Podłączanie selektora daty jQuery z widokami edycji w aplikacji w celu zapewnienia slick możliwość określenia daty.

> [!div class="step-by-step"]
> [Poprzednie](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
> [dalej](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4.md)
