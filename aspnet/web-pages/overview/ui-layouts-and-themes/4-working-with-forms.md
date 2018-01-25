---
uid: web-pages/overview/ui-layouts-and-themes/4-working-with-forms
title: Praca z formularzy HTML w witrynach platformy ASP.NET Web Pages (Razor) | Dokumentacja firmy Microsoft
author: tfitzmac
description: "Formularz jest sekcji dokumentu HTML, gdzie umieścić kontroli danych wejściowych użytkownika, takich jak pola tekstowe, pola wyboru, przyciski radiowe i listy rozwijane. Używanie formularzy ki..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2014
ms.topic: article
ms.assetid: f3f4b8c8-e8f6-4474-ad94-69228a6c01ee
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/4-working-with-forms
msc.type: authoredcontent
ms.openlocfilehash: 8579c444fd19d1a366349cc09f9f768de23055f8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="working-with-html-forms-in-aspnet-web-pages-razor-sites"></a>Praca z formularzy HTML w lokacjach (Razor) stron sieci Web ASP.NET
====================
przez [FitzMacken niestandardowy](https://github.com/tfitzmac)

> W tym artykule opisano sposób przetwarzania formularza HTML (z pola tekstowe i przyciski) podczas pracy w witrynie sieci Web platformy ASP.NET Web Pages (Razor).
> 
> **Zawartość:** 
> 
> - Jak utworzyć formularz HTML.
> - Jak można odczytać dane wejściowe użytkownika w formularzu.
> - Jak można sprawdzić poprawności danych wejściowych użytkownika.
> - Jak przywrócić wartości formularza po przesłaniu strony.
> 
> Są to programowania pojęciami opisanymi w artykule programu ASP.NET:
> 
> - `Request` Obiektu.
> - Sprawdzania poprawności danych wejściowych.
> - Kodowanie HTML.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Używane w samouczku wersje oprogramowania
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> W tym samouczku współdziała również z programu ASP.NET Web Pages 2.


## <a name="creating-a-simple-html-form"></a>Tworzenie prostego formularza HTML

1. Utwórz nową witrynę sieci Web.
2. W folderze głównym utworzyć stronę sieci web o nazwie *Form.cshtml* i wprowadź następujący kod:

    [!code-html[Main](4-working-with-forms/samples/sample1.html)]
3. Uruchom stronę w przeglądarce. (W programie WebMatrix, w **pliki** obszaru roboczego, kliknij prawym przyciskiem myszy plik, a następnie wybierz **Uruchom w przeglądarce**.) Proste formularza z trzech pól wejściowych i **przesyłania** wyświetlany przycisk.

    ![Zrzut ekranu przedstawiający formularza z trzech pól tekstowych.](4-working-with-forms/_static/image1.jpg)

    W tym punkcie, jeśli klikniesz przycisk **przesyłania** przycisk nic się nie dzieje. Aby formularz był przydatne, należy dodać kod, który zostanie uruchomiony na serwerze.

## <a name="reading-user-input-from-the-form"></a>Odczytywanie danych wejściowych użytkownika za pomocą formularza

Aby przetwarzania formularza, należy dodać kod odczytuje wartości pól przesłane i w jakiś z nimi. W tej procedurze przedstawiono sposób odczytać pola i wyświetlania danych wejściowych użytkownika na stronie. (W aplikacji produkcyjnej, zazwyczaj wykonasz bardziej interesujące elementy z danych wejściowych użytkownika. Należy to zrobić w artykule o pracy z bazami danych.)

1. W górnej części *Form.cshtml* pliku, wprowadź następujący kod:

    [!code-cshtml[Main](4-working-with-forms/samples/sample2.cshtml)]

    Gdy użytkownik po raz pierwszy żąda strony, zostanie wyświetlony tylko pusty formularz. Wypełnia formularz dla użytkownika (co będzie można), a następnie klika przycisk **przesyłania**. To przesyła (ogłoszeń) danych wejściowych użytkownika na serwerze. Domyślnie żądania przechodzi do tej samej stronie (to znaczy, *Form.cshtml*).

    Po przesłaniu strony teraz wprowadzonych wartości są wyświetlane nad formularza:

    ![Zrzut ekranu pokazujący wprowadzone wartości wyświetlane na stronie.](4-working-with-forms/_static/image2.jpg)

    Sprawdź kod dla strony. Należy najpierw użyć `IsPost` metodę, aby określić, czy strona jest jest przesyłana &#8212; oznacza to, czy użytkownik kliknął **przesyłania** przycisku. Jeśli jest to post, `IsPost` zwraca wartość true. Jest to standardowy sposób stron ASP.NET Web Pages można określić, czy pracy z żądania początkowego (żądanie GET) lub odświeżenie strony (żądania POST). (Aby uzyskać więcej informacji na temat GET i POST, zobacz "HTTP GET i POST i IsPost Property" paska bocznego w [wprowadzenie do platformy ASP.NET Web Pages programowania przy użyciu składni Razor](https://go.microsoft.com/fwlink/?LinkId=202890#SB_HttpGetPost).)

    Następnie Pobierz wartości, które użytkownik wypełnione `Request.Form` obiekt i umieść je w zmiennych na później. `Request.Form` Obiekt zawiera wszystkie wartości, które zostały przesłane ze stroną, identyfikowanych przy użyciu klucza. Klucz jest odpowiednikiem `name` atrybutu pola formularza, który chcesz odczytać. Na przykład, aby odczytać `companyname` pola (pole tekstowe), możesz użyć `Request.Form["companyname"]`.

    Wartości formularza są przechowywane w `Request.Form` obiektu jako ciągi. W związku z tym podczas pracy z wartość jako liczbę lub datę lub innego typu, należy przekonwertować go z ciągu dla tego typu. W tym przykładzie `AsInt` metoda `Request.Form` służy do konwertowania wartości pola pracowników (zawierający liczba pracowników) na liczbę całkowitą.
2. Uruchom stronę w przeglądarce, wypełnij pola formularza, a następnie kliknij przycisk **przesyłania**. Zostaje wyświetlona strona wprowadzonych wartości.

> [!TIP] 
> 
> <a id="SB_HTMLEncoding"></a>
> ### <a name="html-encoding-for-appearance-and-security"></a>Kodowanie wygląd i zabezpieczeń w formacie HTML
> 
> HTML ma specjalne zastosowania znaków, takich jak `<`, `>`, i `&`. Jeśli te znaki specjalne są wyświetlane, gdzie jest nie oczekiwano, ich przeszukiwanie wygląd i funkcjonalność strony sieci web. Na przykład interpretuje przeglądarki `<` znak (chyba że jest ona następuje spacja) jako początek elementu HTML tak samo, jak `<b>` lub `<input ...>`. Jeśli przeglądarka nie rozpoznaje element, po prostu odrzuca ciąg, który rozpoczyna się od `<` dopóki nie osiągnie coś który ponownie rozpoznawane. Oczywiście może to spowodować niektórych nieco dziwne renderowania strony.
> 
> Kodowanie HTML zastępuje te znaki zastrzeżone kod, który przeglądarki zinterpretować jako poprawne symbolu. Na przykład `<` znak jest zastępowany `&lt;` i `>` znak jest zastępowany `&gt;`. Przeglądarka renderuje te ciągi zamienne jako znaki, które mają być wyświetlane.
> 
> Należy dobrze do użycia w dowolnym momencie wyświetlić ciągów kodowania HTML (dane wejściowe) uzyskanego od użytkownika. Jeśli nie, użytkownik może spróbuj pobrać strony sieci web do uruchamiania skryptu złośliwego lub czegoś innego który obniża poziom bezpieczeństwa witryny lub nie ma. (Jest to szczególnie ważne w przypadku zastosowania danych wejściowych użytkownika, zapisz go w innym, a następnie Wyświetl później &#8212; na przykład jako komentarz blog, przejrzyj użytkownika, lub coś, takich jak który).
> 
> Aby uniknąć tych problemów, ASP.NET Web Pages automatycznie koduje HTML tekstu zawartości tego przypadku dane wyjściowe w kodzie. Na przykład podczas wyświetlania zawartości zmiennej lub wyrażenie, przy użyciu kodu, takich jak `@MyVar`, ASP.NET Web Pages automatycznie koduje dane wyjściowe.


## <a name="validating-user-input"></a>Walidacja danych wejściowych użytkownika

Użytkownicy popełnione. Możesz poprosić o wypełnienie pola i ich zapomnij, lub możesz poprosić o wprowadź liczbę pracowników i ich zamiast tego wpisz nazwę. Aby upewnić się, że formularz wprowadzony poprawnie przed go przetworzyć, sprawdzanie poprawności danych wejściowych użytkownika.

Ta procedura pokazuje, jak można sprawdzić poprawności wszystkie trzy pola aby upewnić się, że użytkownik nie jest pusta je. Możesz również sprawdzić, czy wartość Liczba pracowników jest liczbą. Jeśli wystąpią błędy, zostanie zwrócony komunikat o błędzie komunikat informujący użytkownika, jakie wartości nie przeszedł pomyślnie weryfikacji.

1. W *Form.cshtml* pliku, Zastąp pierwszy blok kodu poniższym kodem: 

    [!code-cshtml[Main](4-working-with-forms/samples/sample3.cshtml)]

    Aby sprawdzić poprawność danych wejściowych użytkownika, należy użyć `Validation` pomocnika. Zarejestruj pola wymagane przez wywołanie metody `Validation.RequireField`. Zarejestruj innych typów weryfikacji przez wywołanie metody `Validation.Add` i określanie pól do sprawdzania poprawności i Typ weryfikacji do wykonania.

    Po uruchomieniu strony ASP.NET wykonuje wszystkie sprawdzania poprawności dla Ciebie. Sprawdź wyniki, wywołując `Validation.IsValid`, która zwraca wartość true, jeśli wszystkie przekazane i wartość false, jeśli którekolwiek z pól nie przeszedł sprawdzania poprawności. Zazwyczaj wywołanie `Validation.IsValid` przed wykonaniem jakiegokolwiek przetwarzania danych wejściowych użytkownika.
2. Aktualizacja `<body>` elementu przez dodanie trzy wywołania `Html.ValidationMessage` metody następująco:

    [!code-cshtml[Main](4-working-with-forms/samples/sample4.cshtml?highlight=8,13,18)]

    Aby wyświetlić komunikaty o błędach weryfikacji, należy wywołać Html.`ValidationMessage` i przekaż go nazwę pola, które ma w komunikacie.
3. Uruchom strony. Pozostaw puste pola i kliknij przycisk **przesyłania**. Możesz wyświetlić komunikaty o błędach.

    ![Zrzut ekranu pokazujący wyświetlane komunikaty o błędach, jeśli dane wejściowe użytkownika nie przeszedł pomyślnie weryfikacji.](4-working-with-forms/_static/image3.jpg)
4. Dodaj ciąg (na przykład "ABC") do **liczba pracowników** a następnie kliknij przycisk **przesyłania** ponownie. Teraz zostanie wyświetlony błąd, który wskazuje, że ciąg nie jest w prawidłowym formacie, a mianowicie, liczbą całkowitą.

    ![Zrzut ekranu pokazujący wyświetlane komunikaty o błędach, jeśli użytkownicy wprowadź ciąg w polu pracowników.](4-working-with-forms/_static/image4.jpg)

Strony sieci Web ASP.NET udostępnia dodatkowe opcje dotyczące sprawdzania poprawności danych wejściowych użytkownika, w tym możliwość automatyczne wykonywanie weryfikacji przy użyciu skryptu klienta, dzięki czemu użytkownicy uzyskują natychmiast uzyskuje opinie w przeglądarce. Zobacz [dodatkowe zasoby](#Additional_Resources) później uzyskać więcej informacji.

## <a name="restoring-form-values-after-postbacks"></a>Przywracanie wartości formularza po ogłaszania zwrotnego

Testach strony w poprzedniej sekcji, można zauważyć, że jeśli masz błąd sprawdzania poprawności, wszystko wprowadził (nie tylko. nieprawidłowe dane) został usunięty i musiał ponownie wprowadzić wartości w polach. Przedstawiono to punkt ważne: Jeśli przesłanie strony, go przetworzyć, a następnie ponownie renderowania strony, strony jest odtworzona od początku. Jak widać, oznacza to, że wartości, które były na stronie przesłanego zostaną utracone.

Można to naprawić, jednak. Masz dostęp do wartości, które zostały przesłane (w `Request.Form` obiektu, więc możesz wpisać tych wartości do pola formularza, podczas renderowania strony.

1. W *Form.cshtml* pliku, Zastąp `value` atrybuty `<input>` elementów za pomocą `value` atrybutu.: 

    [!code-cshtml[Main](4-working-with-forms/samples/sample5.cshtml?highlight=13,19,25)]

    `value` Atrybutu `<input>` elementy została ustawiona jako dynamicznie odczytać wartości pola z `Request.Form` obiektu. Po raz pierwszy żąda strony, wartości `Request.Form` obiektu są puste. Jest poprawnie, ponieważ w ten sposób formularz jest pusty.
2. Uruchom stronę w przeglądarce, wypełnij pola formularza lub pozostaw je puste i kliknij przycisk **przesyłania**. Zostanie wyświetlona strona, pokazujący przesyłane wartości.

    ![forms-5](4-working-with-forms/_static/image5.jpg)

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Dodatkowe zasoby

- [1,001 sposobów uzyskania danych wejściowych od użytkowników sieci Web](https://msdn.microsoft.com/library/ms971057.aspx)
- [Za pomocą formularzy i przetwarzania danych wejściowych użytkownika](https://msdn.microsoft.com/library/ms525182(VS.90).aspx)
- [Weryfikacja danych wejściowych użytkownika w witrynach ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=253002)
- [Przy użyciu funkcji AutoComplete w formularzach HTML](https://msdn.microsoft.com/library/ms533032(VS.85).aspx)
