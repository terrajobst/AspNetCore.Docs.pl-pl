---
uid: mvc/overview/older-versions-1/security/preventing-javascript-injection-attacks-vb
title: Zapobieganie atakom iniekcji JavaScript (VB) | Dokumentacja firmy Microsoft
author: StephenWalther
description: Zapobiec atakom iniekcji JavaScript i skryptów przed atakami opartymi na Cross-Site do Ciebie. W tym samouczku Stephen Walther wyjaśniono, jak łatwo można de...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: 9274a72e-34dd-4dae-8452-ed733ae71377
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/security/preventing-javascript-injection-attacks-vb
msc.type: authoredcontent
ms.openlocfilehash: cb19236b22abd455472621ce74a8cddf9752d6c5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871236"
---
<a name="preventing-javascript-injection-attacks-vb"></a>Zapobieganie atakom iniekcji JavaScript (VB)
====================
przez [Stephen Walther](https://github.com/StephenWalther)

[Pobierz plik PDF](http://download.microsoft.com/download/8/4/8/84843d8d-1575-426c-bcb5-9d0c42e51416/ASPNET_MVC_Tutorial_06_VB.pdf)

> Zapobiec atakom iniekcji JavaScript i skryptów przed atakami opartymi na Cross-Site do Ciebie. W tym samouczku Stephen Walther wyjaśniono, jak łatwo pokonać takich typów ataków przez kodowania zawartości HTML.


Celem tego samouczka jest wyjaśnienie, jak można zapobiec atakom iniekcji JavaScript w aplikacjach ASP.NET MVC. W tym samouczku opisano dwa podejścia do obrony witryny sieci Web przed ataku polegającego na iniekcji JavaScript. Sposób przed atakami iniekcji JavaScript kodując dane, które można wyświetlić. Możesz również sposób zapobiegać atakom iniekcji JavaScript kodując dane, które możesz zaakceptować.

## <a name="what-is-a-javascript-injection-attack"></a>Co to jest atak iniekcji JavaScript?

Akceptowanie danych wejściowych użytkownika, a następnie ponownie wyświetlić dane wejściowe użytkownika, możesz otworzyć witryny sieci Web na ataki iniekcji JavaScript. Przeanalizujmy konkretnych aplikacji, która jest otwarta na ataki iniekcji JavaScript.

Załóżmy, że utworzono witrynę sieci Web opinie klientów (zobacz rysunek 1). Klientów można odwiedzić witrynę, a następnie wprowadź opinii na ich doświadczenia w używaniu produktów. Gdy klient przesyła ich opinie, opinii zostanie wyświetlony ponownie na stronie opinii.


[![Opinie klientów witryny sieci Web](preventing-javascript-injection-attacks-vb/_static/image2.png)](preventing-javascript-injection-attacks-vb/_static/image1.png)

**Rysunek 01**: Opinia klienta witryny sieci Web ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](preventing-javascript-injection-attacks-vb/_static/image3.png))


Opinie klientów witryny sieci Web używa `controller` wyświetlania 1. To `controller` zawiera dwa działania o nazwie `Index()` i `Create()`.

**1 — Lista `HomeController.vb`**

[!code-vb[Main](preventing-javascript-injection-attacks-vb/samples/sample1.vb)]

`Index()` Metoda Wyświetla `Index` widoku. Ta metoda przekazuje wszystkie poprzednie opinie klientów do `Index` widoku pobierając opinii z bazy danych (przy użyciu zapytania składnika LINQ to SQL).

`Create()` — Metoda tworzy nowy element opinii i dodaje go do bazy danych. Komunikat, który klient przechodzi w formularzu są przekazywane do `Create()` metody w parametrze wiadomości. Utworzono element opinii i wiadomość jest przypisana do elementu opinii `Message` właściwości. Element opinii jest przesyłany do bazy danych z `DataContext.SubmitChanges()` wywołania metody. Na koniec obiekt odwiedzający jest przekierowany z powrotem do `Index` widok, w którym wyświetlane wszystkie opinii.

`Index` Widoku znajduje się lista 2.

**2 — Lista `Index.aspx`**

[!code-aspx[Main](preventing-javascript-injection-attacks-vb/samples/sample2.aspx)]

`Index` Widok zawiera dwie sekcje. Pierwsza sekcja zawiera rzeczywiste klienta formularza opinii. Na dole zawiera For... Każdej pętli, który przetwarza w pętli wszystkich poprzednich elementów opinie klientów i wyświetla właściwości EntryDate i komunikat dla każdego elementu opinii.

Opinie klientów witryny sieci Web jest prosty witryny sieci Web. Niestety witryna sieci Web jest otwarty na ataki iniekcji JavaScript.

Wyobraź sobie wprowadzić następujący tekst w formularzu informacji zwrotnych klienta:

[!code-html[Main](preventing-javascript-injection-attacks-vb/samples/sample3.html)]

Ten tekst reprezentuje skryptu JavaScript, która wyświetla komunikat alertu. Po trafi tego skryptu do opinii formularzu komunikat <em>coś!</em> będą wyświetlane zawsze, gdy każdy użytkownik odwiedza witryny opinii klientów w przyszłości (patrz rysunek 2).


[![Iniekcji JavaScript](preventing-javascript-injection-attacks-vb/_static/image5.png)](preventing-javascript-injection-attacks-vb/_static/image4.png)

**Rysunek 02**: iniekcji JavaScript ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](preventing-javascript-injection-attacks-vb/_static/image6.png))


Teraz początkową odpowiedź na ataki iniekcji JavaScript może być apathy. Może się okazać, że ataki iniekcji JavaScript są po prostu typu *atak skutkujący zmianą zawartości* ataku. Może być uważasz, że nikt naprawdę Akcja niczego przez zatwierdzenie ataku polegającego na iniekcji JavaScript.

Niestety, haker można wykonać niektóre naprawdę, naprawdę Akcja rzeczy przez wstrzykiwanie JavaScript do witryny sieci Web. Ataku polegającego na iniekcji JavaScript służy do wykonywania ataku polegającego na wykonywanie skryptów między witrynami (XSS). Atak skryptów krzyżowych służy do wykradania informacji poufnych użytkownika i wysyła je do innej witryny sieci Web.

Na przykład haker służy ataku polegającego na iniekcji JavaScript do wykradania wartości pliki cookie przeglądarki od innych użytkowników. Jeśli informacji poufnych — takich jak hasła, numery kart kredytowych lub numerów ubezpieczenia społecznego — są przechowywane w plikach cookie przeglądarki, haker można użyć ataku polegającego na iniekcji JavaScript do wykradania informacji. Lub, jeśli użytkownik wprowadzi poufne informacje w polu formularza zawartych na stronie, który został złamany z atak JavaScript, wówczas haker używać wprowadzony kod JavaScript do pobrania danych formularza i wysyłania go do innej witryny sieci Web.

*Należy Przerażony*. Poważnego ataków iniekcji JavaScript i ochrony informacji poufnych użytkownika. W dwóch następnych sekcjach omówiono dwie metody, których można chronić przed atakami iniekcji JavaScript aplikacji ASP.NET MVC.

## <a name="approach-1-html-encode-in-the-view"></a>Podejście #1: Kodowanie HTML w widoku

Wszystkie dane wprowadzone przez użytkowników witryny sieci Web, po ponownym danych w widoku jednego łatwa metoda zapobieganie atakom iniekcji JavaScript HTML kodowania. Zaktualizowany interfejs `Index` widoku w 3 wyświetlania następuje tej metody.

**Wyświetlanie listy 3 – `Index.aspx` (kodowania HTML)**

[!code-aspx[Main](preventing-javascript-injection-attacks-vb/samples/sample4.aspx)]

Zwróć uwagę, że wartość `feedback.Message` jest HTML zakodowane przed wyświetleniem wartość następującym kodem:

[!code-aspx[Main](preventing-javascript-injection-attacks-vb/samples/sample5.aspx)]

Jaki jest średnią do formatu HTML zakodować ciąg? Podczas HTML kodowania ciąg, niebezpiecznych znaków takich jak `<` i `>` zastępuje odwołań do jednostek kodu HTML, takie jak `&lt;` i `&gt;`. W takim przypadku ciąg `<script>alert("Boo!")</script>` jest HTML zakodowane, pobiera go przekonwertować na `&lt;script&gt;alert(&quot;Boo!&quot;)&lt;/script&gt;`. Ciąg kodowany jako już nie wykonuje jako skryptu JavaScript, gdy interpretowany przez przeglądarkę. Zamiast tego należy pobrać nieszkodliwe strony na rysunku 3.


[![Bezcelowe ataku JavaScript](preventing-javascript-injection-attacks-vb/_static/image8.png)](preventing-javascript-injection-attacks-vb/_static/image7.png)

**Rysunek 03**: bezcelowe ataku JavaScript ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](preventing-javascript-injection-attacks-vb/_static/image9.png))


Zwróć uwagę, że w `Index` tylko wartości przeglądać listę 3 `feedback.Message` jest zaszyfrowana. Wartość `feedback.EntryDate` nie jest zaszyfrowana. Należy zakodować danych wprowadzonych przez użytkownika. Ponieważ wartość EntryDate został wygenerowany w kontrolerze, nie należy do formatu HTML kodowania tej wartości.

## <a name="approach-2-html-encode-in-the-controller"></a>Podejście #2: Kodowanie HTML w kontrolerze

Zamiast kodowania danych, gdy dane są wyświetlane w widoku HTML, możesz HTML zakodować dane tuż przed przesyłania danych do bazy danych. Takie podejście drugi jest pobierana w odniesieniu `controller` w listę 4.

**Wyświetlanie listy 4 — `HomeController.cs` (kodowania HTML)**

[!code-vb[Main](preventing-javascript-injection-attacks-vb/samples/sample6.vb)]

Zauważ, że wartość komunikatu jest HTML zakodowane przed przesłaniem do bazy danych w ramach wartość `Create()` akcji. Jeśli komunikat zostanie wyświetlony ponownie w widoku, wiadomość ma kodowania HTML i wszelkie JavaScript wprowadzonym w komunikacie nie została wykonana.

Zazwyczaj powinien Preferuj pierwszym sposobem omówione w tym samouczku za pośrednictwem tej drugiej metody. Ta druga podejścia przy rozwiązywaniu problemu jest, że na końcu danych HTML zakodowane w bazie danych. Innymi słowy danych bazy danych jest dirtied z dziwne wyglądającej znaki.

Dlaczego jest to nieodpowiedni? Jeśli trzeba do wyświetlania danych w bazie danych w inną niż strony sieci web, zostanie występują problemy. Na przykład nie jest już przeglądania danych w aplikacji formularzy systemu Windows.

## <a name="summary"></a>Podsumowanie

Celem tego samouczka było przestraszyć możesz o Perspektywa ataku polegającego na iniekcji JavaScript. W tym samouczku opisano dwa podejścia do obrony przed atakami iniekcji JavaScript aplikacji ASP.NET MVC: można albo HTML kodowania użytkownika przesłane dane w widoku lub użytkownik może HTML kodowania użytkownika przesłane dane w kontrolerze.

> [!div class="step-by-step"]
> [Poprzednie](authenticating-users-with-windows-authentication-vb.md)
