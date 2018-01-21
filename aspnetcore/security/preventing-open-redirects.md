---
title: Zapobieganie atakom przekierowania otwartych w aplikacji platformy ASP.NET Core | Dokumentacja firmy Microsoft
author: ardalis
description: "Pokazuje, jak zapobiegać atakom Otwórz przekierowania dla aplikacji platformy ASP.NET Core"
ms.author: riande
manager: wpickett
ms.date: 07/07/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/preventing-open-redirects
ms.openlocfilehash: e57ae429e9af54ade74485361ba591cb75c16752
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="preventing-open-redirect-attacks-in-an-aspnet-core-app"></a>Zapobieganie atakom przekierowania otwartych w aplikacji platformy ASP.NET Core

Aplikacja sieci web, który przekierowuje do adresu URL, który został określony przy użyciu żądania, takich jak dane formularza lub ciągu kwerendy może potencjalnie niepowołane przekierowuje użytkowników zewnętrznych, złośliwy adresu URL. Naruszeniu ten nosi nazwę atak przekierowania otwarte.

Zawsze, gdy logiki aplikacji przekierowuje pod określony adres URL, należy sprawdzić, czy adres URL przekierowania nie została zmieniona. Platformy ASP.NET Core ma wbudowaną funkcję do ochrony aplikacji przed atakami Otwórz przekierowania (znanej także jako Otwórz przekierowania).

## <a name="what-is-an-open-redirect-attack"></a>Co to jest atak przekierowania Otwórz?

Podczas uzyskiwania dostępu do zasobów, które wymagają uwierzytelniania aplikacji sieci Web często przekierować użytkowników do strony logowania. Obejmuje typlically przekierowania `returnUrl` parametr querystring, dzięki czemu użytkownik może być zwracany do pierwotnie żądanego adresu URL po ich pomyślnym zalogowaniu. Po uwierzytelnia użytkownika, zostanie przekierowany do były one pierwotnie żądanego adresu URL.

Docelowy adres URL, ponieważ określono w ciąg zapytania żądania złośliwy użytkownik może manipulować ciąg zapytania. Zmodyfikowany querystring umożliwia witrynie przekierowanie użytkownika do witryny zewnętrznej, złośliwe. Ta metoda jest wywoływana Otwórz atak przekierowania (lub przekierowania).

### <a name="an-example-attack"></a>Na przykład ataki

Złośliwy użytkownik może utworzyć atak ma umożliwić złośliwemu użytkownikowi dostęp do poświadczeń użytkownika lub poufnych informacji w aplikacji. Aby rozpocząć atak, ich przekonać użytkownika do kliknij łącze do strony logowania w witrynie, z `returnUrl` wartości querystring dodany do adresu URL. Na przykład [NerdDinner.com](http://nerddinner.com) przykładowej aplikacji (przeznaczone dla platformy ASP.NET MVC) zawiera takie strony logowania tutaj: ``http://nerddinner.com/Account/LogOn?returnUrl=/Home/About``. Atak następnie obejmuje następujące kroki:

1. Użytkownik kliknie łącze do ``http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn`` (należy pamiętać, drugi adres URL jest nerddi**n**era, nie nerddi**nn**ERA).
2. Użytkownik, który loguje się pomyślnie.
3. Użytkownik zostaje przekierowany (przez witrynę) do ``http://nerddiner.com/Account/LogOn`` (złośliwa witryna przypominającą rzeczywistej lokacji).
4. Użytkownik loguje się ponownie (podając złośliwego lokacji poświadczeń) i jest przekierowywany do rzeczywistego witryny.

Użytkownik prawdopodobnie będą sądziły pierwsza próba logowania nie powiodła się, a ich drugi zakończyło się pomyślnie. Prawdopodobnie będziesz pozostają bez "świadomości" swoje poświadczenia zostały naruszone.

![Otwórz przekierowania ataku procesu](preventing-open-redirects/_static/open-redirection-attack-process.png)

Oprócz strony logowania niektóre Lokacje przekierowania strony lub punktów końcowych. Wyobraź sobie aplikacja ma stronę Otwórz przekierowania ``/Home/Redirect``. Osoba atakująca może utworzyć na przykład łącze w wiadomości e-mail, który jest przesyłany do ``[yoursite]/Home/Redirect?url=http://phishingsite.com/Home/Login``. Typowy użytkownik będzie wyglądać pod adresem URL i znaleźć zaczyna się od nazwy lokacji. Który ufające, będzie kliknij łącze. Otwórz przekierowania następnie może wysłać do użytkownika do witryny wyłudzaniem informacji, która jest taka sama jak należy do Ciebie, a użytkownik będzie prawdopodobnie będą uważali logowanie jest witryną.

## <a name="protecting-against-open-redirect-attacks"></a>Ochrona przed atakami Otwórz przekierowania

Podczas tworzenia aplikacji sieci web, należy traktować wszystkie dane dostarczone przez użytkownika jako niezaufane. Jeśli aplikacja ma funkcji, która przekierowuje użytkownika na podstawie zawartości adresu URL, upewnij się, że takie przekierowania tylko są wykonywane lokalnie w Twojej aplikacji (lub znany adres URL nie każdy adres URL, które mogą być dostarczane w zmiennej querystring).

### <a name="localredirect"></a>LocalRedirect

Użyj ``LocalRedirect`` metody pomocnika od podstawy `Controller` klasy:

```csharp
public IActionResult SomeAction(string redirectUrl)
{
    return LocalRedirect(redirectUrl);
}
```

``LocalRedirect``spowoduje zgłoszenie wyjątku, jeśli określono adres URL nie lokalnego. W przeciwnym razie wartość działa tak samo jak ``Redirect`` metody.

### <a name="islocalurl"></a>IsLocalUrl

Użyj [IsLocalUrl](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.iurlhelper#Microsoft_AspNetCore_Mvc_IUrlHelper_IsLocalUrl_System_String_) metody do testowania przekierowywał adresów URL:

Poniższy przykład przedstawia sposób sprawdzania, czy adres URL jest lokalny, przed przekierowaniem.

```csharp
private IActionResult RedirectToLocal(string returnUrl)
{
    if (Url.IsLocalUrl(returnUrl))
    {
        return Redirect(returnUrl);
    }
    else
    {
        return RedirectToAction(nameof(HomeController.Index), "Home");
    }
}
```

`IsLocalUrl` — Metoda uniemożliwia przypadkowo nastąpi przekierowanie do witryny złośliwych użytkowników. Możesz zalogować się szczegóły adres URL, który został podany, gdy adres URL-local znajduje się w sytuacji, gdy oczekiwano lokalny adres URL. Rejestrowanie przekierowania adresów URL może pomóc w zdiagnozowaniu ataków przekierowania.
