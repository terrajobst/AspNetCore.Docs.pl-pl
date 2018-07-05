---
uid: mvc/overview/security/preventing-open-redirection-attacks
title: Zapobieganie atakom na otwarte przekierowywanie (C#) | Dokumentacja firmy Microsoft
author: jongalloway
description: W tym samouczku wyjaśniono, jak zapobiegać atakom na otwarte przekierowywanie w aplikacjach ASP.NET MVC. W tym samouczku omówiono zmiany, które zostały wprowadzone...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 69fb02e0-f5b7-4c35-878c-fa87164fc785
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/security/preventing-open-redirection-attacks
msc.type: authoredcontent
ms.openlocfilehash: 27921e23d38d34332b81fb85dcc795c8f9ff0352
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37375473"
---
<a name="preventing-open-redirection-attacks-c"></a>Zapobieganie atakom na otwarte przekierowywanie (C#)
====================
przez [Galloway'em Jon](https://github.com/jongalloway)

> W tym samouczku wyjaśniono, jak zapobiegać atakom na otwarte przekierowywanie w aplikacjach ASP.NET MVC. W tym samouczku omówiono zmiany wprowadzone w elementu AccountController w ASP.NET MVC 3 i pokazuje, jak te zmiany mogą stosować w aplikacjach 2 i Twojego istniejącego programu ASP.NET MVC 1.0.


## <a name="what-is-an-open-redirection-attack"></a>Co to jest atak przekierowania Open?

Dowolnej aplikacji sieci web, która przekierowuje do adresu URL, który jest określony za pomocą żądania, takie jak dane formularza lub querystring może potencjalnie naruszone przekierowywać użytkowników zewnętrznych, złośliwy adres URL. Tym naruszeniu nosi nazwę na otwarte przekierowywanie ataki.

Zawsze, gdy logika aplikacji przekierowuje pod określony adres URL, należy sprawdzić, czy adres URL przekierowania nie została zmieniona. Nazwa logowania używana w domyślnej elementu AccountController zarówno dla programu ASP.NET MVC 1.0 i ASP.NET MVC 2 jest narażony na otwarcie przekierowania ataków. Na szczęście jest łatwe do zaktualizowania istniejącej aplikacji poprawki w ASP.NET MVC 3 w wersji zapoznawczej.

Aby poznać luki w zabezpieczeniach, Przyjrzyjmy się jak działa przekierowanie logowania w projekcie aplikacji sieci Web programu ASP.NET MVC 2 domyślne. W tej aplikacji próba można znaleźć akcji kontrolera, który ma atrybut [Authorize] nastąpi przekierowanie nieautoryzowanych użytkowników do widoku /Account/LogOn. Przekierowanie do /Account/LogOn będzie zawierać parametr querystring returnUrl, tak aby użytkownika mogą być zwrócone do pierwotnie żądanego adresu URL, po ich pomyślnym zalogowaniu.

Na poniższym zrzucie ekranu okaże się, że próba dostępu do widoku /Account/ChangePassword, gdy nie jest zalogowany powoduje przekierowanie do /Account/LogOn? ReturnUrl = % 2fAccount % 2fChangePassword % 2f.

[![](preventing-open-redirection-attacks/_static/image2.png)](preventing-open-redirection-attacks/_static/image1.png)

**Rysunek 01**: strony logowania za pomocą otwarte przekierowywanie

Ponieważ parametr querystring ReturnUrl nie zostanie zweryfikowana, osoba atakująca ją zmodyfikować, aby wstawić dowolny adres URL do parametru do przeprowadzenia ataku na otwarte przekierowywanie. Aby to wykazać, firma Microsoft Zmodyfikuj parametr ReturnUrl [ http://bing.com ](http://bing.com), dzięki czemu będzie wynikowy adres URL logowania/konta/logowania? ReturnUrl =<http://www.bing.com/>. Po pomyślnym zalogowaniu się do witryny, firma Microsoft są przekierowywane do [ http://bing.com ](http://bing.com). Ponieważ przekierowanie nie zostanie zweryfikowana, zamiast tego można wskazywać złośliwych witryn, który próbuje zachęcające użytkownika.

### <a name="a-more-complex-open-redirection-attack"></a>Bardziej złożone Otwórz atak przekierowania

Atakom na otwarte przekierowywanie są szczególnie niebezpieczne, ponieważ osoba atakująca zna osobę, którą próbujemy Zaloguj się do określonej witryny sieci Web, co pozwala nam na [ataku](https://www.microsoft.com/protect/fraud/phishing/symptoms.aspx). Na przykład osoba atakująca może wysłać złośliwy wiadomości e-mail do użytkowników witryny sieci Web w celu przechwycenia haseł. Przyjrzyjmy się jak działa to w witrynie NerdDinner. (Zwróć uwagę, że w działającej witrynie NerdDinner został zaktualizowany do ochrony przed atakom na otwarte przekierowywanie).

Po pierwsze osoba atakująca wysyła nam łącze do strony logowania na NerdDinner obejmującą przekierowania do ich sfałszowanych strony:

[http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn](http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn)

Należy pamiętać, że zwrotny adres URL wskazuje nerddiner.com, której brakuje "n" z obiad programu word. W tym przykładzie jest domena, który kontroluje osoba atakująca. Podczas dostępu do powyższego linku, firma Microsoft nastąpi przekierowanie do strony logowania NerdDinner.com uzasadnione.

[![](preventing-open-redirection-attacks/_static/image4.png)](preventing-open-redirection-attacks/_static/image3.png)

**Rysunek 02**: NerdDinner strony logowania za pomocą otwarte przekierowywanie

Firma Microsoft prawidłowo po zalogowaniu się, działania logowania elementu platformy ASP.NET MVC AccountController przekierowuje nam do adresu URL określonego w parametrze querystring returnUrl. W tym przypadku jest adres URL, który wprowadził osoba atakująca, która jest [ http://nerddiner.com/Account/LogOn ](http://nerddiner.com/Account/LogOn). Chyba, że jesteśmy bardzo watchful, jest bardzo prawdopodobne, firma Microsoft nie będzie Zwróć uwagę, szczególnie w przypadku, ponieważ osoba atakująca została musisz upewnić się, że ich sfałszowanych strona wygląda dokładnie strony logowania autoryzowanych. Ta strona logowania zawiera błąd komunikat żądania, możemy ponownie się zalogować. Clumsy NAS, firma Microsoft musi być błędnie wpisane hasło naszych.

[![](preventing-open-redirection-attacks/_static/image6.png)](preventing-open-redirection-attacks/_static/image5.png)

**Rysunek 03**: ekran logowania sfałszowanych NerdDinner

Gdy firma Microsoft umożliwia ponowne wpisanie naszych nazwy użytkownika i hasła, na stronie logowania sfałszowanych zapisuje informacje i wysyła nam do legalnych witryny NerdDinner.com. W tym momencie witryny NerdDinner.com ma już uwierzytelniony, aby przekierować strony logowania sfałszowanych bezpośrednio do tej strony. Wynik końcowy jest, że osoba atakująca ma naszych nazwę użytkownika i hasło, a jesteśmy świadomości, że udostępniliśmy ją do nich.

## <a name="looking-at-the-vulnerable-code-in-the-accountcontroller-logon-action"></a>Patrząc na ataki kodu w akcji elementu AccountController logowania

Poniżej przedstawiono kod dla działań logowania w aplikacji ASP.NET MVC 2. Należy pamiętać, że po pomyślnym logowaniu ten kontroler zwraca przekierowania do returnUrl. Aby zobaczyć, czy Weryfikacja nie jest akurat wykonywane względem parametru returnUrl.

**1 — działania logowania programu ASP.NET MVC 2 w ofercie `AccountController.cs`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample1.cs)]

Teraz Przyjrzyjmy się zmian do działania logowania programu ASP.NET MVC 3. Ten kod został zmieniony w celu zweryfikowania parametru returnUrl przez wywołanie metody nową metodę o nazwie klasy pomocnika System.Web.Mvc.Url `IsLocalUrl()`.

**2 — Akcja platformy ASP.NET MVC 3 logowania w ofercie `AccountController.cs`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample2.cs)]

Zostało to zmienione aby zweryfikować parametr zwrotnego adresu URL, wywołując nową metodę w klasie pomocnika System.Web.Mvc.Url `IsLocalUrl()`.

## <a name="protecting-your-aspnet-mvc-10-and-mvc-2-applications"></a>Ochrona ASP.NET MVC 1.0 i MVC 2 aplikacji

Będziemy korzystać z zalet zmiany ASP.NET MVC 3 w naszych istniejących programu ASP.NET MVC 1.0 i 2 aplikacji przez dodanie metody pomocnika IsLocalUrl() i aktualizowanie akcji logowania w celu zweryfikowania parametru returnUrl.

Metoda UrlHelper IsLocalUrl() w rzeczywistości po prostu wywoływanie metody w System.Web.WebPages w ramach tej weryfikacji także jest używane przez aplikacje ASP.NET Web Pages.

**Wyświetlanie listy 3 — metoda IsLocalUrl() z ASP.NET MVC 3 UrlHelper `class`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample3.cs)]

Metoda IsUrlLocalToHost zawiera logikę weryfikacji rzeczywistych, jak pokazano w ofercie 4.

**Wyświetlanie listy 4 – IsUrlLocalToHost() metody z klasy System.Web.WebPages RequestExtensions**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample4.cs)]

W naszym programu ASP.NET MVC 1.0 lub aplikacji, 2 dodamy metoda IsLocalUrl() do elementu AccountController, ale chętnie chcesz dodać do osobnej klasie pomocy, jeśli jest to możliwe. Podejmiemy dwóch niewielkie zmiany do wersji ASP.NET MVC 3 IsLocalUrl(), aby działały w ramach elementu AccountController. Po pierwsze zmienimy go z publiczną metodę do metody prywatnej, ponieważ metod publicznych w kontrolerach może być dostępna jako akcji kontrolera. Po drugie zmodyfikujemy wywołanie, które sprawdza, czy host adresu URL dla hosta aplikacji. Czy wywołania korzysta z lokalnych RequestContext pole w klasie UrlHelper. Zamiast tego. RequestContext.HttpContext.Request.Url.Host, użyjemy go. Request.Url.Host. Poniższy kod przedstawia modyfikowana metoda IsLocalUrl() do użytku z klasy controller w aplikacjach 2 i programu ASP.NET MVC 1.0.

**Listę 5 — metoda IsLocalUrl(), który jest modyfikowany do użytku z klasą kontroler MVC**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample5.cs)]

Teraz, gdy metoda IsLocalUrl() jest w miejscu, możemy wywołać go z naszej akcji logowania w celu zweryfikowania parametru returnUrl, jak pokazano w poniższym kodzie

**Wyświetlanie listy 6 — metoda zaktualizowano logowania, która sprawdza poprawność parametru returnUrl**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample6.cs)]

Teraz możemy przetestować ataku na otwarte przekierowywanie próbując zalogować się przy użyciu zewnętrznego adresu URL zwracany. Użyjmy /Account/LogOn? ReturnUrl =<http://www.bing.com/> ponownie.

[![](preventing-open-redirection-attacks/_static/image8.png)](preventing-open-redirection-attacks/_static/image7.png)

**Rysunek 04**: testowanie zaktualizowano akcję logowania

Po pomyślnym zalogowaniu możemy są przekierowywane do akcji kontrolera Home/Index, a nie na zewnętrzny adres URL.

[![](preventing-open-redirection-attacks/_static/image10.png)](preventing-open-redirection-attacks/_static/image9.png)

**Rysunek 05**: ataku otwarte przekierowywanie udaremnione

## <a name="summary"></a>Podsumowanie

Przekierowywanie adresów URL są przekazywane jako parametry w adresie URL aplikacji, mogą pojawić się atakom na otwarte przekierowywanie. ASP.NET MVC 3, szablon zawiera kod, aby zapewnić ochronę przed Otwórz ataków przekierowania. Możesz dodać ten kod z wprowadzając pewne modyfikacje, ASP.NET MVC 1.0 i 2 aplikacjami. Aby zapewnić ochronę przed atakom na otwarte przekierowywanie podczas logowania się do platformy ASP.NET w wersji 1.0 i 2 aplikacjami, Dodaj metodę IsLocalUrl() i sprawdzanie poprawności parametru returnUrl działanie logowania.
