---
uid: mvc/overview/security/preventing-open-redirection-attacks
title: "Zapobieganie atakom Otwórz przekierowania (C#) | Dokumentacja firmy Microsoft"
author: jongalloway
description: "W tym samouczku wyjaśniono, jak można zapobiec ataków Otwórz przekierowania w aplikacjach ASP.NET MVC. W tym samouczku opisano zmiany, które zostały wprowadzone..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 69fb02e0-f5b7-4c35-878c-fa87164fc785
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/preventing-open-redirection-attacks
msc.type: authoredcontent
ms.openlocfilehash: 17944c0600a174176e3e9940f414b34f0835b800
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2018
---
<a name="preventing-open-redirection-attacks-c"></a>Zapobieganie atakom Otwórz przekierowania (C#)
====================
przez [Galloway Jan](https://github.com/jongalloway)

> W tym samouczku wyjaśniono, jak można zapobiec ataków Otwórz przekierowania w aplikacjach ASP.NET MVC. W tym samouczku opisano zmiany wprowadzone w elementu AccountController w ASP.NET MVC 3 i pokazuje, jak zastosować te zmiany w Twoje istniejące 1.0 MVC ASP.NET i aplikacji 2.


## <a name="what-is-an-open-redirection-attack"></a>Co to jest atak przekierowania Otwórz?

Dowolnej aplikacji sieci web, która przekierowuje do adresu URL, który został określony przy użyciu żądania, takich jak dane querystring lub formularza może potencjalnie niepowołane przekierowuje użytkowników zewnętrznych, złośliwy adresu URL. Naruszeniu ten nosi nazwę atak przekierowania otwarte.

Zawsze, gdy logiki aplikacji przekierowuje pod określony adres URL, należy sprawdzić, czy adres URL przekierowania nie została zmieniona. Nazwa logowania używana w domyślnym elementu AccountController dla platformy ASP.NET MVC 1.0 i ASP.NET MVC 2 jest narażony na Otwórz ataków przekierowania. Na szczęście są łatwe do aktualizowania istniejących aplikacji ma używać poprawek w wersji zapoznawczej programu ASP.NET MVC 3.

Aby poznać lukę w zabezpieczeniach, Przyjrzyjmy się jak działa przekierowanie logowania w domyślny projekt aplikacji sieci Web programu ASP.NET MVC 2. W tej aplikacji próby odwiedź akcji kontrolera, który ma atrybut [Authorize] nastąpi przekierowanie nieautoryzowanych użytkowników do widoku /Account/LogOn. Przekierowanie do /Account/LogOn będzie zawierać parametr querystring returnUrl, dzięki czemu użytkownik może być zwracany do pierwotnie żądanego adresu URL po ich pomyślnym zalogowaniu.

Na poniższym zrzucie ekranu widać, że próba dostępu do widoku /Account/ChangePassword, gdy nie jest zalogowany powoduje przekierowanie do /Account/LogOn? ReturnUrl = % 2fAccount % 2fChangePassword % 2f.

[![](preventing-open-redirection-attacks/_static/image2.png)](preventing-open-redirection-attacks/_static/image1.png)

**Rysunek 01**: strony logowania z otwartych przekierowania

Ponieważ parametr querystring ReturnUrl nie została zweryfikowana, osoba atakująca ją zmodyfikować, aby wprowadzić dowolny adres URL do parametru do przeprowadzenia ataku Otwórz przekierowania. Aby to wykazać, firma Microsoft Zmodyfikuj parametr ReturnUrl [http://bing.com](http://bing.com), więc będzie wynikowy adresu URL logowania/Account/logowania? ReturnUrl = http://www.bing.com/. Po pomyślnym zalogowaniu do lokacji, możemy są przekierowywane do [http://bing.com](http://bing.com). Ponieważ przekierowanie nie została zweryfikowana, zamiast tego można wskazać niebezpiecznej witryny podejmowanych w celu nakłonienia użytkownika.

### <a name="a-more-complex-open-redirection-attack"></a>Bardziej złożone Otwórz atak przekierowania

Otwórz przekierowania ataki są szczególnie niebezpieczne, ponieważ atakujący zna próbujemy do zalogowania się do określonej witryny sieci Web, dzięki czemu nam narażony na ataki [ataku phishing](https://www.microsoft.com/protect/fraud/phishing/symptoms.aspx). Na przykład osoba atakująca może wysłać złośliwy wiadomości e-mail do użytkowników witryny sieci Web w celu przechwycenia haseł. Oto jak to będzie działać w witrynie NerdDinner. (Należy pamiętać, że działającą witrynę NerdDinner została zaktualizowana w celu ochrony przed atakami Otwórz przekierowania).

Po pierwsze osoba atakująca wysyła nam łącze do strony logowania na NerdDinner, która obejmuje przekierowanie do ich sfałszowanego strony:

[http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn](http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn)

Należy pamiętać, że zwrotny adres URL wskazuje nerddiner.com, której brakuje "n" z obiad programu word. W tym przykładzie jest domena, która kontroluje, osoba atakująca. Gdy firma Microsoft dostęp do tego łącza, firma Microsoft jest podjęcie uzasadnionych NerdDinner.com strony logowania.

[![](preventing-open-redirection-attacks/_static/image4.png)](preventing-open-redirection-attacks/_static/image3.png)

**Rysunek 02**: NerdDinner strony logowania z otwartych przekierowania

Gdy poprawnie rejestrowane, elementu ASP.NET MVC AccountController logowania akcji przekierowuje nam na adres URL określony w parametrze returnUrl querystring. W takim przypadku jest adres URL wprowadzony osoba atakująca, która jest [http://nerddiner.com/Account/LogOn](http://nerddiner.com/Account/LogOn). O ile nie jest bardzo watchful, jest bardzo prawdopodobne, nie będzie zauważymy, szczególnie w przypadku, ponieważ atakujący został uważać, aby upewnić się, że ich sfałszowanego strona wygląda dokładnie strony logowania autoryzowanych. Ta strona logowania zawiera komunikat błędu żądania, że możemy zalogować się ponownie. Clumsy USA, firma Microsoft musi zostać błędnie nasze hasło.

[![](preventing-open-redirection-attacks/_static/image6.png)](preventing-open-redirection-attacks/_static/image5.png)

**Rysunek 03**: ekran logowania sfałszowane NerdDinner

Gdy firma Microsoft ponownie naszych nazwy użytkownika i hasła, strony logowania sfałszowanego zapisuje informacje i wysyła nam do witrynę NerdDinner.com. W tym momencie lokacji NerdDinner.com ma już uwierzytelniony us, więc strony logowania sfałszowanego można przekierowywać bezpośrednio do tej strony. W rezultacie jest ma naszych nazwę użytkownika i hasło, czy możemy wie, że udostępniliśmy ją dla nich.

## <a name="looking-at-the-vulnerable-code-in-the-accountcontroller-logon-action"></a>Spojrzenie na niebezpieczny kod w akcji elementu AccountController logowania

Poniżej przedstawiono kod dla działań logowania w aplikacji ASP.NET MVC 2. Należy pamiętać, że po pomyślnym logowaniu, kontrolera zwraca przekierowanie do returnUrl. Widać, że weryfikacja nie jest wykonywana przed parametru returnUrl.

**Wyświetlanie listy 1 – logowania programu ASP.NET MVC 2 akcji w`AccountController.cs`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample1.cs)]

Teraz Przyjrzyjmy się zmiany do akcji logowania programu ASP.NET MVC 3. Ten kod został zmieniony na sprawdzanie poprawności parametru returnUrl przez wywołanie nowej metody w klasie System.Web.Mvc.Url o nazwie `IsLocalUrl()`.

**Wyświetlanie listy 2 — akcji logowania programu ASP.NET MVC 3`AccountController.cs`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample2.cs)]

Zostało to zmienione aby zweryfikować parametr zwrotnego adresu URL, wywołując nowej metody w klasie System.Web.Mvc.Url, `IsLocalUrl()`.

## <a name="protecting-your-aspnet-mvc-10-and-mvc-2-applications"></a>Ochrona ASP.NET MVC 1.0 i MVC 2 aplikacji

Firma Microsoft może korzystać z zmiany ASP.NET MVC 3 w naszym istniejących 1.0 MVC ASP.NET i aplikacji 2 przez dodanie metody pomocnika IsLocalUrl() i aktualizowanie akcji logowania w celu zweryfikowania parametru returnUrl.

Metoda UrlHelper IsLocalUrl() faktycznie tylko wywoływanie metody w System.Web.WebPages jako tej weryfikacji jest również używane przez aplikacje ASP.NET Web Pages.

**Wyświetlanie listy 3 — metody IsLocalUrl() z UrlHelper ASP.NET MVC 3`class`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample3.cs)]

Metoda IsUrlLocalToHost zawiera logikę rzeczywista weryfikacja pokazane na listę 4.

**Wyświetlanie listy 4 — metody IsUrlLocalToHost() z klasy System.Web.WebPages RequestExtensions**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample4.cs)]

W naszym ASP.NET MVC w wersji 1.0 lub 2 aplikacji metoda IsLocalUrl() zostanie dodany do elementu AccountController, ale użytkownik ma zaleca, aby dodać go do klasę pomocy oddzielnych Jeśli to możliwe. Firma Microsoft wprowadzi dwóch niewielkich zmian do wersji platformy ASP.NET MVC 3 IsLocalUrl(), dzięki czemu będzie ona działać wewnątrz elementu AccountController. Najpierw zmienimy go z publiczną metodę do metody prywatnej, ponieważ są dostępne metody publiczne kontrolery akcji kontrolera. Po drugie firma Microsoft będzie modyfikować połączenia, który sprawdza hostem adresu URL dla hosta aplikacji. Czy połączenie korzysta z lokalnej RequestContext pole w klasie UrlHelper. Zamiast tego. RequestContext.HttpContext.Request.Url.Host, użyjemy go. Request.Url.Host. Poniższy kod przedstawia zmodyfikowane metody IsLocalUrl() do użytku z klasy controller w aplikacjach 2 i ASP.NET MVC w wersji 1.0.

**Listę 5 — metoda IsLocalUrl(), który jest zmodyfikowany służących do klasy kontrolera MVC**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample5.cs)]

Teraz, metoda IsLocalUrl() znajduje się w miejscu, można nazywamy go od naszych działań logowania do zweryfikowania parametru returnUrl, jak pokazano w poniższym kodzie.

**Wyświetlanie listy 6 — metodę logowania zaktualizowane, która weryfikuje parametru returnUrl**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample6.cs)]

Teraz można było przetestować atak przekierowania otwarte przez próby Zaloguj się za pomocą zewnętrznego adresu URL zwracany. Użyjmy/Account/logowania? ReturnUrl = http://www.bing.com/ ponownie.

[![](preventing-open-redirection-attacks/_static/image8.png)](preventing-open-redirection-attacks/_static/image7.png)

**Rysunek 04**: testowanie zaktualizowane akcji logowania

Po pomyślnym zalogowaniu możemy są przekierowywane do akcji kontrolera głównej/indeksu, a nie jako zewnętrzny adres URL.

[![](preventing-open-redirection-attacks/_static/image10.png)](preventing-open-redirection-attacks/_static/image9.png)

**Rysunek 05**: atak przekierowania Otwórz udaremnione

## <a name="summary"></a>Podsumowanie

Otwórz przekierowania ataków może wystąpić, gdy przekierowania adresów URL są przekazywane jako parametry w adresie URL dla aplikacji. ASP.NET MVC 3 szablon zawiera kod, aby zapewnić ochronę przed Otwórz ataków przekierowania. Możesz dodać ten kod z niektórych modyfikację ASP.NET MVC w wersji 1.0 oraz 2 aplikacji. Ochronę przed atakami Otwórz przekierowania, podczas logowania się do platformy ASP.NET w wersji 1.0 oraz 2 aplikacji, Dodaj metodę IsLocalUrl() i sprawdzanie poprawności parametru returnUrl w operacji logowania.
