---
uid: web-pages/overview/ui-layouts-and-themes/4-working-with-forms
title: Praca z formularzami HTML w witrynach sieci Web Pages (Razor) platformy ASP.NET | Dokumentacja firmy Microsoft
author: tfitzmac
description: Formularz jest sekcja dokumentu HTML, gdzie umieścić kontrolki danych wejściowych użytkownika, takie jak pola tekstowe, pola wyboru, listy rozwijane i przyciski opcji. Używanie formularzy pytania "Wh"...
ms.author: aspnetcontent
ms.date: 02/10/2014
ms.assetid: f3f4b8c8-e8f6-4474-ad94-69228a6c01ee
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/4-working-with-forms
msc.type: authoredcontent
ms.openlocfilehash: 4658e80d63c9c3665f197071f3f3d1420362e4a3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820288"
---
<a name="working-with-html-forms-in-aspnet-web-pages-razor-sites"></a>Praca z formularzami HTML w witrynach ASP.NET Web Pages (Razor)
====================
przez [Tom FitzMacken](https://github.com/tfitzmac)

> W tym artykule opisano sposób przetwarzania formularza HTML (za pomocą pola tekstowe i przyciski) podczas pracy w witrynie internetowej ASP.NET Web Pages (Razor).
> 
> **Zawartość:** 
> 
> - Jak utworzyć formularza HTML.
> - Jak odczytać dane wejściowe użytkownika z formularza.
> - Jak sprawdzić poprawność danych wejściowych użytkownika.
> - Jak przywrócić wartości formularza po przesłaniu strony.
> 
> Poniżej przedstawiono ASP.NET programowania pojęciami opisanymi w artykule:
> 
> - `Request` Obiektu.
> - Walidacja danych wejściowych.
> - Kodowanie HTML.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używanego w tym samouczku
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> W tym samouczku współpracuje również z wzorca ASP.NET Web Pages 2.


## <a name="creating-a-simple-html-form"></a>Tworzenie prostego formularza HTML

1. Utwórz nową witrynę sieci Web.
2. W folderze głównym Utwórz stronę sieci web o nazwie *Form.cshtml* i wprowadź następujący kod:

    [!code-html[Main](4-working-with-forms/samples/sample1.html)]
3. Uruchom stronę w przeglądarce. (W programie WebMatrix w **pliki** obszaru roboczego, kliknij prawym przyciskiem myszy plik, a następnie wybierz pozycję **Uruchom w przeglądarce**.) Prosty formularz z trzech pól wejściowych i **przesyłania** wyświetlany przycisk.

    ![Zrzut ekranu przedstawiający formularz zawierający trzy pola tekstowe.](4-working-with-forms/_static/image1.jpg)

    Jeśli na tym etapie, możesz kliknąć pozycję **przesyłania** przycisk, nic się nie dzieje. Aby formularz był przydatny, trzeba dodać kodu, który będzie uruchamiany na serwerze.

## <a name="reading-user-input-from-the-form"></a>Odczytywanie danych wejściowych użytkownika z formularza

Aby przetwarzać formularza, należy dodać kod, który odczytuje wartości pól przesłane i wykonuje coś z nimi. Ta procedura pokazuje, jak odczytać pól i wyświetlić dane wejściowe użytkownika na stronie. (W przypadku aplikacji produkcyjnej należy zwykle wykonać bardziej interesujących zadań z danymi wejściowymi użytkownika. Należy to zrobić w artykule na temat pracy z bazami danych.)

1. W górnej części *Form.cshtml* pliku, wprowadź następujący kod:

    [!code-cshtml[Main](4-working-with-forms/samples/sample2.cshtml)]

    Gdy użytkownik po raz pierwszy żąda strony, zostanie wyświetlony tylko pusty formularz. (Który będzie można) użytkownik wypełnia formularz, a następnie klika przycisk **przesyłania**. Przesyła (ogłoszeń) danych wejściowych użytkownika do serwera. Domyślnie żądania prowadzi do tej samej strony (to znaczy, *Form.cshtml*).

    Po przesłaniu strony teraz wprowadzone wartości są wyświetlane nad formularza:

    ![Zrzut ekranu pokazujący wprowadzone wartości wyświetlane na stronie.](4-working-with-forms/_static/image2.jpg)

    Spójrz na kod strony. Należy najpierw użyć `IsPost` metodę pozwala ustalić, czy strona jest przesyłana &#8212; oznacza to, czy użytkownik kliknął element **przesyłania** przycisku. Jeśli jest to udzielania `IsPost` zwraca wartość true. Jest to standardowy sposób stron ASP.NET Web Pages w celu ustalenia, czy pracujesz za pomocą początkowego żądania (żądanie GET) lub zwrotu (żądania POST). (Aby uzyskać więcej informacji na temat GET i POST zobacz "Pobierz i POST oraz IsPost właściwość HTTP" paska bocznego w [wprowadzenie do platformy ASP.NET Web Pages programowania z użyciem składni Razor](https://go.microsoft.com/fwlink/?LinkId=202890#SB_HttpGetPost).)

    Następnie Pobierz wartości, które użytkownik wypełnione `Request.Form` obiektu i umieść je w zmiennych na później. `Request.Form` Obiekt zawiera wszystkie wartości, które zostały przesłane ze stroną, identyfikowanych według klucza. Klucz jest odpowiednikiem `name` atrybutu pola formularza, który chcesz odczytać. Na przykład, aby odczytać `companyname` pola (pole tekstowe), możesz użyć `Request.Form["companyname"]`.

    Wartości formularza są przechowywane w `Request.Form` obiektu jako ciągi. W związku z tym gdy trzeba pracować z wartością jako liczba lub wartość typu date lub innego typu, należy przekonwertować go z ciągu do tego typu. W tym przykładzie `AsInt` metody `Request.Form` służy do konwertowania wartości pola pracowników, (który zawiera liczba pracowników) na liczbę całkowitą.
2. Uruchom stronę w przeglądarce, wypełnij pola formularza, a następnie kliknij przycisk **przesyłania**. Zostanie wyświetlona strona wprowadzone wartości.

> [!TIP] 
> 
> <a id="SB_HTMLEncoding"></a>
> ### <a name="html-encoding-for-appearance-and-security"></a>Kodowanie wygląd i zabezpieczeń w formacie HTML
> 
> HTML ma specjalne zastosowań znaków, takich jak `<`, `>`, i `&`. Jeśli te znaki specjalne są wyświetlane, gdzie one nie oczekiwano, ich przeszukiwanie wyglądu i funkcjonalności na stronie sieci web. Na przykład przeglądarka interpretuje `<` znak (chyba że następuje spacja) jako początek elementu HTML, takie jak `<b>` lub `<input ...>`. Jeśli przeglądarka nie rozpoznaje element, po prostu odrzuca ciąg, który rozpoczyna się od `<` aż do napotkania coś, ponownie rozpoznaje. Oczywiście może to spowodować pewne renderowanie nieco dziwne, na stronie.
> 
> Kodowanie HTML zastępuje następujących znaków zastrzeżonych z kodem, który przeglądarek, następuje interpretacja jako poprawne symboli. Na przykład `<` jest zastępowany znaku `&lt;` i `>` jest zastępowany znaku `&gt;`. Przeglądarka renderuje te ciągi zastąpienie jako znaki, które mają być wyświetlane.
> 
> To dobry pomysł, do użycia w dowolnym momencie wyświetlić ciągów kodowania HTML (dane wejściowe), którą uzyskano przez użytkownika. Jeśli nie, można pobrać strony sieci web do uruchamiania złośliwych skryptów lub czymś innym, obniża bezpieczeństwa witryny lub po prostu nie ma spróbować użytkownika. (Jest to szczególnie ważne, jeśli akceptują dane wejściowe użytkownika, zapisz go w innym, a następnie Wyświetl później &#8212; na przykład, jako komentarz blog, recenzji użytkownika lub coś podobnego.)
> 
> Aby uniknąć tych problemów, ASP.NET Web Pages automatycznie koduje HTML dowolny tekst zawartości, dane wyjściowe w kodzie. Na przykład podczas wyświetlania zawartości zmiennej lub wyrażenia przy użyciu kodu, takich jak `@MyVar`, ASP.NET Web Pages automatycznie koduje dane wyjściowe.


## <a name="validating-user-input"></a>Walidacja danych wejściowych użytkownika

Użytkownicy wykonują błędów. Możesz poprosić o wypełnienie pola, zapomną, lub możesz poprosić o wprowadź liczby pracowników i ich zamiast tego wpisz nazwę. Aby upewnić się, że formularza zostało wypełnione poprawnie przed rozpoczęciem przetwarzania, sprawdzanie poprawności danych wejściowych użytkownika.

Ta procedura pokazuje, jak można zweryfikować wszystkie trzy pola do upewnij się, że użytkownik nie jest puste je. Możesz sprawdzić, czy wartość liczby pracowników jest liczbą. Jeśli występują błędy, zostanie zwrócony komunikat o błędzie komunikatu, informujący użytkownika, jakie wartości nie przeszły pomyślnie sprawdzania poprawności.

1. W *Form.cshtml* pliku, Zamień pierwszy blok kodu poniższym kodem: 

    [!code-cshtml[Main](4-working-with-forms/samples/sample3.cshtml)]

    Aby sprawdzić poprawność danych wejściowych użytkownika, należy użyć `Validation` pomocnika. Zarejestruj wymagane pola, wywołując `Validation.RequireField`. Rejestrowanie innych typów weryfikacji przez wywołanie metody `Validation.Add` i określając pole do sprawdzania poprawności i Typ weryfikacji do wykonania.

    Po uruchomieniu strony ASP.NET wykonuje wszystkie sprawdzania poprawności dla Ciebie. Sprawdź wyniki, wywołując `Validation.IsValid`, który zwraca wartość true, jeśli wszystkie elementy przekazane i wartość false, jeśli dowolne pole Weryfikacja nie powiodła się. Zazwyczaj należy wywołać `Validation.IsValid` przed wykonaniem jakiegokolwiek przetwarzania na dane wejściowe użytkownika.
2. Aktualizacja `<body>` elementu przez dodanie trzech wywołań `Html.ValidationMessage` metoda następująco:

    [!code-cshtml[Main](4-working-with-forms/samples/sample4.cshtml?highlight=8,13,18)]

    Aby wyświetlić komunikaty o błędach weryfikacji, można wywołać Html.`ValidationMessage` i przekaż mu nazwę pola, które mają w komunikacie o.
3. Uruchom stronę. Pozostaw pola puste, a następnie kliknij przycisk **przesyłania**. Możesz wyświetlić komunikaty o błędach.

    ![Zrzut ekranu pokazujący komunikaty o błędach wyświetlane, jeśli dane wejściowe użytkownika nie przeszło weryfikacji.](4-working-with-forms/_static/image3.jpg)
4. Dodaj parametry (na przykład "ABC") do **liczba pracowników** pola, a następnie kliknij przycisk **przesyłania** ponownie. Tym razem zostanie wyświetlony błąd, który wskazuje, że ten ciąg nie jest w odpowiednim formacie, to znaczy, liczbą całkowitą.

    ![Zrzut ekranu pokazujący komunikaty o błędach wyświetlane, jeśli użytkownicy wprowadzają ciąg dla pola pracowników.](4-working-with-forms/_static/image4.jpg)

Strony ASP.NET Web Pages zapewnia więcej opcji sprawdzania poprawności danych wejściowych użytkownika, w tym możliwość automatycznego wykonywania weryfikacji przy użyciu skryptu klientów tak, aby użytkownicy natychmiast otrzymać opinię w przeglądarce. Zobacz [dodatkowe zasoby](#Additional_Resources) później uzyskać więcej informacji.

## <a name="restoring-form-values-after-postbacks"></a>Przywracanie wartości formularza po ogłaszania zwrotnego

Gdy strona jest badany w poprzedniej sekcji, być może Zauważyłeś, jeśli masz błąd sprawdzania poprawności, wszystko, co wprowadzono (nie tylko. nieprawidłowe dane) został usunięty i musiał ponownie wprowadzić wartości wszystkich pól. Obrazuje to to ważny punkt: podczas przesyłania strony, przetwarzanie i renderowanie strony w ponownie strona jest ponownie utworzyć od podstaw. Co oznacza to, że wszelkie wartości, które były na stronie przesłanego zostaną utracone.

Można to naprawić, jednak. Masz dostęp do wartości, które zostały przesłane (w `Request.Form` obiektu, dzięki czemu możesz wpisać te wartości do pola formularza, gdy strona jest wyświetlana.

1. W *Form.cshtml* pliku, Zastąp `value` atrybuty `<input>` elementów za pomocą `value` atrybutu.: 

    [!code-cshtml[Main](4-working-with-forms/samples/sample5.cshtml?highlight=13,19,25)]

    `value` Atrybutu `<input>` elementów został ustawiony na dynamicznie odczytać wartości pól z `Request.Form` obiektu. Przy pierwszym żądaniu strony wartości w `Request.Form` obiektu są puste. Jest dobrym rozwiązaniem, ponieważ w ten sposób formularz jest pusty.
2. Uruchom stronę w przeglądarce, wypełnij pola formularza lub pozostawić je puste, a kliknij **przesyłania**. Zostanie wyświetlona strona, pokazujący przesyłane wartości.

    ![Formularze 5](4-working-with-forms/_static/image5.jpg)

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Dodatkowe zasoby

- [1,001 sposobów uzyskania danych wejściowych od użytkowników w sieci Web](https://msdn.microsoft.com/library/ms971057.aspx)
- [Za pomocą formularzy i przetwarzania danych wejściowych użytkownika](https://msdn.microsoft.com/library/ms525182(VS.90).aspx)
- [Weryfikacja danych wejściowych użytkownika w witrynach ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=253002)
- [Za pomocą automatycznego uzupełniania w formularzach HTML](https://msdn.microsoft.com/library/ms533032(VS.85).aspx)
