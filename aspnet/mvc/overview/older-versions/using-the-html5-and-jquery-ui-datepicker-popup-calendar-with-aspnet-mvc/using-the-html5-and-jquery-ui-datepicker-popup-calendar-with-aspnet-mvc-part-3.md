---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
title: Używanie kalendarza podręcznego selektora daty interfejsu użytkownika jQuery i HTML5 z ASP.NET MVC — część 3 | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: Ta seria samouczków obejmuje podstawowe informacje dotyczące korzystania z edytora szablonów, szablony i kalendarza podręcznego selektora daty interfejsu użytkownika jQuery, w MV ASP.NET...
ms.author: riande
ms.date: 08/29/2011
ms.assetid: 8f5f91ae-12d7-4cf3-ac09-4bb53d07ee60
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
msc.type: authoredcontent
ms.openlocfilehash: 184d93323ac6f2cb7f14d32b401cf6a400eb79a6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41757192"
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-3"></a>Używanie kalendarza podręcznego selektora daty interfejsu użytkownika jQuery i HTML5 z ASP.NET MVC — część 3
====================
Przez [Rick Anderson](https://github.com/Rick-Anderson)

> Ta seria samouczków obejmuje podstawowe informacje dotyczące korzystania z edytora szablonów, szablony i kalendarza podręcznego selektora daty interfejsu użytkownika jQuery, w aplikacji sieci Web platformy ASP.NET MVC.


## <a name="working-with-complex-types"></a>Praca z typów złożonych

W tej sekcji możesz utworzyć klasę adres i Dowiedz się, jak utworzyć szablon, aby go wyświetlić.

W *modeli* folderu, Utwórz nowy plik klasy o nazwie *osoba.cs* którym zostanie umieszczony dwa typy: `Person` klasy i `Address` klasy. `Person` Klasa będzie zawierać właściwości, która jest `Address`. `Address` Typ to typ złożony, co oznacza, nie jest jedną z wbudowanych typów, takich jak `int`, `string`, lub `double`. Zamiast tego ma kilka właściwości. Kod dla nowych klas wygląda następująco:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample1.cs)]

W `Movie` kontroler, Dodaj następujący kod `PersonDetail` akcji w celu wyświetlenia wystąpienia osobę:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample2.cs)]

Następnie dodaj następujący kod do `Movie` kontrolera, aby wypełnić `Person` modelu z pewnymi przykładowymi danymi:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample3.cs)]

Otwórz *Views\Movies\PersonDetail.cshtml* pliku i Dodaj następujący kod znaczników dla `PersonDetail` widoku.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample4.cshtml)]

Naciśnij klawisze Ctrl + F5, aby uruchomić aplikację, a następnie przejdź do *filmy/PersonDetail*.

`PersonDetail` Widok nie zawiera `Address` typu złożonego, jak można sprawdzić, w tym zrzucie ekranu. (Brak adresu znajduje się).

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image1.png)

`Address` Modelu danych nie jest wyświetlana, ponieważ jest typem złożonym. Aby wyświetlić informacje o adresach, otwórz *Views\Movies\PersonDetail.cshtml* ponownie i Dodaj następujący kod znaczników.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample5.cshtml)]

Kompletny kod znaczników dla `PersonDetail` teraz widoku wygląda następująco:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample6.cshtml)]

Uruchom ponownie aplikację i wyświetlić `PersonDetail` widoku. Informacje o adresie jest teraz wyświetlany:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image2.png)

### <a name="creating-a-template-for-a-complex-type"></a>Tworzenie szablonu dla typu złożonego

W tej sekcji utworzysz szablon, który zostanie użyty do renderowania `Address` typu złożonego. Po utworzeniu szablonu dla `Address` typu, ASP.NET MVC automatycznie służy do formatowania modelu adresu w dowolnym miejscu aplikacji. Daje to możliwość kontrolowania renderowania `Address` typu na podstawie tylko w jednym miejscu w aplikacji.

W *folder Views\Shared\DisplayTemplates* folderze utwórz silnie typizowanego widoku częściowego, o nazwie **adres**:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image3.png)

Kliknij przycisk **Dodaj**, a następnie otwórz nowy *Views\Shared\DisplayTemplates\Address.cshtml* pliku. Nowy widok zawiera następujące wygenerowane znaczniki:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample7.cshtml)]

Uruchom aplikację i wyświetlić `PersonDetail` widoku. Tym razem `Address` właśnie utworzony szablon służy do wyświetlania `Address` typu złożonego, aby ekran wygląda podobnie do poniższego:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image4.png)

### <a name="summary-ways-to-specify-the-model-display-format-and-template"></a>Podsumowanie: Sposoby, aby określić Format wyświetlania modelu i szablonu

W tym samouczku można określić format lub szablon dla właściwości modelu w przy użyciu następujących metod:

- Stosowanie `DisplayFormat` atrybutu do właściwości w modelu. Na przykład poniższy kod powoduje, że data jest wyświetlana bez czasu:

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample8.cs)]
- Stosowanie [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) atrybutu do właściwości w modelu i określania typu danych. Na przykład poniższy kod powoduje, że data jest wyświetlana bez czasu.

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample9.cs)]

    Jeśli aplikacja zawiera *date.cshtml* szablonu w *folder Views\Shared\DisplayTemplates* folderu lub *Views\Movies\DisplayTemplates* folder, w tym szablonie będzie używany do renderowania `DateTime` właściwości. W przeciwnym razie wbudowany system szablonów ASP.NET Wyświetla właściwość jako wartość typu date.
- Tworzenie szablonu ekranu w *folder Views\Shared\DisplayTemplates* folderu lub *Views\Movies\DisplayTemplates* folder, którego nazwa odpowiada typ danych, który chcesz sformatować. Na przykład, że widoczny *Views\Shared\DisplayTemplates\DateTime.cshtml* został użyty do renderowania `DateTime` właściwości w modelu, bez konieczności dodawania atrybutów do modelu i bez dodawania żadnych znaczników do widoków.
- Za pomocą [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) atrybutu w modelu, aby określić szablon, aby wyświetlić właściwości modelu.
- Jawne dodanie nazwę wyświetlaną szablonu w celu [Html.DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) wywołań w widoku.

Podejścia, którego używasz, zależy od tego, co musisz zrobić w aplikacji. Nie jest niczym niezwykłym kombinację obu rozwiązań, aby uzyskać dokładnie rodzaj formatowania, które należy.

W następnej sekcji można przełączać gears nieco i Przenieś możliwości dostosowywania wyświetlania danych umożliwiające dostosowanie sposobu wprowadzania. Będziesz podpiąć datepicker jQuery z widokami Edytuj w aplikacji w celu zapewnienia sprawny sposób, aby określić daty.

> [!div class="step-by-step"]
> [Poprzednie](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
> [dalej](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4.md)
