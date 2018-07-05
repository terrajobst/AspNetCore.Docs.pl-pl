---
uid: mvc/overview/older-versions-1/security/preventing-javascript-injection-attacks-vb
title: Zapobieganie atakom iniekcji JavaScript (VB) | Dokumentacja firmy Microsoft
author: StephenWalther
description: 'Zapobiec atakom iniekcji JavaScript i atakami skryptów między witrynami dla Ciebie. W tym samouczku Walther Autor: Stephen wyjaśnia, jak można łatwo de...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: 9274a72e-34dd-4dae-8452-ed733ae71377
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/security/preventing-javascript-injection-attacks-vb
msc.type: authoredcontent
ms.openlocfilehash: 6c022847462239624d5b84816999918ec77f8337
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402344"
---
<a name="preventing-javascript-injection-attacks-vb"></a>Zapobieganie atakom iniekcji JavaScript (VB)
====================
przez [Walther Autor: Stephen](https://github.com/StephenWalther)

[Pobierz plik PDF](http://download.microsoft.com/download/8/4/8/84843d8d-1575-426c-bcb5-9d0c42e51416/ASPNET_MVC_Tutorial_06_VB.pdf)

> Zapobiec atakom iniekcji JavaScript i atakami skryptów między witrynami dla Ciebie. W tym samouczku Walther Autor: Stephen wyjaśnia, jak łatwo można pokonać tego rodzaju ataki przeprowadzane przez kodowania zawartości HTML.


Celem tego samouczka jest wyjaśniają, jak można zapobiec wstrzyknięciu kodu JavaScript w aplikacjach ASP.NET MVC. W tym samouczku omówiono dwa podejścia do obrony przed ataku polegającego na iniekcji JavaScript witryny sieci Web. Dowiesz się, jak zapobiegającą wstrzyknięciu kodu JavaScript, przez kodowanie danych, którą można wyświetlać. Poznasz również sposób zapobiegającą wstrzyknięciu kodu JavaScript przez kodowanie danych, który oznacza akceptację.

## <a name="what-is-a-javascript-injection-attack"></a>Co to jest ataku polegającego na iniekcji JavaScript?

Zawsze, gdy użytkownik akceptuje dane wejściowe użytkownika i ponownie wyświetlić dane wejściowe użytkownika, możesz otworzyć witryny sieci Web na wstrzyknięciu kodu JavaScript. Przeanalizujmy konkretnych aplikacji, która jest otwarta na wstrzyknięciu kodu JavaScript.

Wyobraź sobie, że utworzono witrynę sieci Web opinii klientów (patrz rysunek 1). Klienci mogą odwiedź witrynę internetową i wprowadź informacje zwrotne o swoich doświadczeniach z używania produktów. Gdy klient wyśle ich opinie, opinii, zostanie wyświetlony ponownie na stronie opinii.


[![Opinie klientów witryny sieci Web](preventing-javascript-injection-attacks-vb/_static/image2.png)](preventing-javascript-injection-attacks-vb/_static/image1.png)

**Rysunek 01**: witryny sieci Web klienta opinii ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](preventing-javascript-injection-attacks-vb/_static/image3.png))


Witryna internetowa opinii klientów używa `controller` w ofercie 1. To `controller` zawiera dwa działania o nazwie `Index()` i `Create()`.

**1 — Lista `HomeController.vb`**

[!code-vb[Main](preventing-javascript-injection-attacks-vb/samples/sample1.vb)]

`Index()` Metoda Wyświetla `Index` widoku. Ta metoda przekazuje wszystkich wcześniejszych informacji zwrotnych klienta, aby `Index` widoku przez pobranie opinii z bazy danych (za pomocą LINQ do kwerendy SQL).

`Create()` Metoda tworzy nowy element opinii i dodaje go do bazy danych. Komunikat, który klient wprowadza w formularzu jest przekazywany do `Create()` metody w parametrze wiadomości. Element opinii jest tworzony i komunikat jest przypisany do elementu opinii `Message` właściwości. Element opinii jest przesyłany do bazy danych o `DataContext.SubmitChanges()` wywołania metody. Ponadto użytkownik jest przekierowany z powrotem do `Index` widoku, gdzie wyświetlone wszystkie opinie.

`Index` Widok znajduje się w ofercie 2.

**2 — Lista `Index.aspx`**

[!code-aspx[Main](preventing-javascript-injection-attacks-vb/samples/sample2.aspx)]

`Index` Widok zawiera dwie sekcje. Górna sekcja zawiera formularz opinii klientów rzeczywiste. Dolną sekcję zawiera For … Każdej pętli w pętli wszystkich poprzednich elementów opinii klientów i wyświetlenie właściwości EntryDate i komunikat dla każdego elementu opinii.

Witryna internetowa z opinii klientów jest prostą witrynę sieci Web. Niestety witryna sieci Web jest otwarty na wstrzyknięciu kodu JavaScript.

Wyobraź sobie, wprowadź następujący tekst do formularza opinii klientów:

[!code-html[Main](preventing-javascript-injection-attacks-vb/samples/sample3.html)]

Ten tekst reprezentuje skryptu JavaScript, która wyświetla okno komunikatu alertu. Po trafi tego skryptu do opinii formularzu komunikat <em>coś!</em> pojawi się w każdym przypadku, gdy każdy użytkownik odwiedza witrynę sieci Web z opinii klientów w przyszłości (patrz rysunek 2).


[![Iniekcja kodu JavaScript](preventing-javascript-injection-attacks-vb/_static/image5.png)](preventing-javascript-injection-attacks-vb/_static/image4.png)

**Rysunek 02**: iniekcji JavaScript ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](preventing-javascript-injection-attacks-vb/_static/image6.png))


Teraz Twoje pierwszą odpowiedzią na wstrzyknięciu kodu JavaScript może być apathy. Może się wydawać, że wstrzyknięciu kodu JavaScript są po prostu rodzaj *wystarcza* ataku. Może być uważa, że nikt Akcja naprawdę nic przez zatwierdzenie ataku polegającego na iniekcji JavaScript.

Niestety, haker może wykonanie niektórych naprawdę bardzo Akcja rzeczy przez iniekcję kodu JavaScript do witryny sieci Web. Ataku polegającego na iniekcji JavaScript służy do wykonywania ataków skryptów między witrynami (XSS). Atak z użyciem skryptów między witrynami służy do wykradania informacji poufnych użytkownika i wysyłać je do innej witryny sieci Web.

Na przykład haker, można użyć ataku polegającego na iniekcji JavaScript do wykradania wartości pliki cookie przeglądarki od innych użytkowników. Jeśli poufnych informacji — takie jak hasła, numerów kart kredytowych lub numerów ubezpieczenia społecznego — jest przechowywany w plikach cookie przeglądarki, haker może użyć ataku polegającego na iniekcji JavaScript do wykradania informacji. Lub, jeśli użytkownik wprowadzi poufnych informacji w polu formularza zawartych na stronie, którego bezpieczeństwo zostało naruszone przy użyciu ataku JavaScript, wówczas haker używać wprowadzonego kodu JavaScript dane formularza i wysyłać je do innej witryny sieci Web.

*Należy Przerażony*. Poważnego wstrzyknięciu kodu JavaScript i ochrony informacji poufnych użytkownika. W dwóch następnych sekcjach omówiono dwie techniki, które można chronić swoje aplikacje platformy ASP.NET MVC na wstrzyknięciu kodu JavaScript.

## <a name="approach-1-html-encode-in-the-view"></a>Podejście #1: Kodowanie HTML w widoku

Wszystkie dane wprowadzone przez użytkowników witryny sieci Web, gdy użytkownik ponownie wyświetlić dane w widoku jednego z prostą metodę zapobieganie wstrzyknięciu kodu JavaScript w formacie HTML kodowania. Zaktualizowany interfejs `Index` widoku w ofercie 3 poniżej tego podejścia.

**Wyświetlanie listy 3 — `Index.aspx` (kodowania HTML)**

[!code-aspx[Main](preventing-javascript-injection-attacks-vb/samples/sample4.aspx)]

Należy zauważyć, że wartość `feedback.Message` ma format HTML zakodowane przed wyświetleniem wartość następującym kodem:

[!code-aspx[Main](preventing-javascript-injection-attacks-vb/samples/sample5.aspx)]

Jaki jest średnia w formacie HTML kodowanie ciągu? Podczas HTML kodowania ciągu, niebezpiecznych znaków takich jak `<` i `>` są zastępowane przez odwołań do jednostek kodu HTML, takich jak `&lt;` i `&gt;`. W takim przypadku ciąg `<script>alert("Boo!")</script>` ma format HTML zakodowane, jego są konwertowane na `&lt;script&gt;alert(&quot;Boo!&quot;)&lt;/script&gt;`. Zakodowany ciąg nie jest już wykonuje jako skrypt JavaScript, gdy interpretowany przez przeglądarkę. Zamiast tego możesz pobrać nieszkodliwe strony na rysunku 3.


[![Bezcelowe ataku JavaScript](preventing-javascript-injection-attacks-vb/_static/image8.png)](preventing-javascript-injection-attacks-vb/_static/image7.png)

**Rysunek 03**: bezcelowe ataku JavaScript ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](preventing-javascript-injection-attacks-vb/_static/image9.png))


Należy zauważyć, że w `Index` tylko wartości przeglądać w ofercie 3 `feedback.Message` jest zaszyfrowana. Wartość `feedback.EntryDate` nie jest zaszyfrowana. Należy zakodować dane wprowadzane przez użytkownika. Ponieważ wartość EntryDate został wygenerowany w kontrolerze, nie należy do formatu HTML kodowania tę wartość.

## <a name="approach-2-html-encode-in-the-controller"></a>Podejście #2: Kodowanie HTML w kontrolerze

Zamiast kodowania danych, gdy dane są wyświetlane w widoku HTML, możesz HTML zakodować dane przed przesyłania danych do bazy danych. Ta druga metoda jest wykonywane w przypadku programu `controller` w ofercie 4.

**Wyświetlanie listy 4 – `HomeController.cs` (kodowania HTML)**

[!code-vb[Main](preventing-javascript-injection-attacks-vb/samples/sample6.vb)]

Zauważ, że wartość wiadomości HTML zakodowane przed przesłaniem do bazy danych w ramach wartość `Create()` akcji. Gdy komunikat zostanie wyświetlony ponownie, w widoku, wiadomości ma kodowania HTML i dowolnego języka JavaScript, wprowadzony w komunikacie nie jest wykonywany.

Zazwyczaj powinien Preferuj pierwszego podejścia omówionych w tym samouczku za pośrednictwem tego drugiego podejścia. Problem w drugim przypadku tej metody polega na tym, że na końcu HTML z zakodowanych danych w bazie danych. Innymi słowy danych bazy danych jest dirtied zabawnych wyglądających znaków.

Dlaczego jest to nieodpowiedni? Jeśli potrzebujesz wsparcia wyświetlania danych w bazie danych w innych jednostkach niż strony sieci web, następnie będziesz mieć problemy. Można na przykład, nie będzie łatwo wyświetlić dane w aplikacji Windows Forms.

## <a name="summary"></a>Podsumowanie

Celem tego samouczka było przestraszyć możesz o Perspektywa ataku polegającego na iniekcji JavaScript. W tym samouczku omówiono dwa podejścia do obrony przed atakami polegającymi na iniekcji JavaScript aplikacji ASP.NET MVC: można albo HTML kodowanie użytkownika przesłane dane w widoku lub można HTML kodowanie użytkownika przesłane dane w kontrolerze.

> [!div class="step-by-step"]
> [Poprzednie](authenticating-users-with-windows-authentication-vb.md)
