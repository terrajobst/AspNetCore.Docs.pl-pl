---
title: Zapobieganie atakom na otwarte przekierowywanie w programie ASP.NET Core
author: ardalis
description: Pokazuje, jak zapobiegać atakom na otwarte przekierowywanie względem aplikacji ASP.NET Core
ms.author: riande
ms.date: 07/07/2017
uid: security/preventing-open-redirects
ms.openlocfilehash: 0896189d2caaccb19647eb7c6d57f29dfc0290dd
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64898300"
---
# <a name="prevent-open-redirect-attacks-in-aspnet-core"></a>Zapobieganie atakom na otwarte przekierowywanie w programie ASP.NET Core

Aplikację internetową, która przekierowuje do adresu URL, który jest określony za pomocą żądania, takie jak dane formularza lub querystring może potencjalnie naruszone przekierowywać użytkowników zewnętrznych, złośliwy adres URL. Tym naruszeniu nosi nazwę na otwarte przekierowywanie ataki.

Zawsze, gdy logika aplikacji przekierowuje pod określony adres URL, należy sprawdzić, czy adres URL przekierowania nie została zmieniona. Platforma ASP.NET Core ma wbudowaną funkcję, aby chronić aplikacje przed atakom na otwarte przekierowywanie (znany także jako otwarte przekierowywanie).

## <a name="what-is-an-open-redirect-attack"></a>Co to jest atak na otwarte przekierowywanie?

Aplikacje sieci Web często przekierowywać użytkowników do strony logowania w przypadku uzyskiwania dostępu do zasobów, które wymagają uwierzytelniania. Zwykle obejmują przekierowanie `returnUrl` parametr querystring tak, aby użytkownika mogą być zwrócone do pierwotnie żądanego adresu URL, po ich pomyślnym zalogowaniu. Po użytkownik jest uwierzytelniany, są one przekierowanie do adresu URL, pierwszy poprosił.

Ponieważ docelowy adres URL jest określona w zmiennej querystring żądania, złośliwy użytkownik może manipulować ciąg zapytania. Zmodyfikowany querystring umożliwiającej witryny przekierować użytkownika do witryny zewnętrznej, złośliwy. Technika ta jest zwana Otwórz atak przekierowania (lub przekierowania).

### <a name="an-example-attack"></a>Na przykład ataki

Złośliwy użytkownik może tworzyć ataku, aby umożliwić złośliwemu użytkownikowi dostęp do poświadczeń użytkownika lub informacji poufnych. Aby rozpocząć atak, złośliwy użytkownik convinces użytkownikowi kliknij link do strony logowania w witrynie za pomocą `returnUrl` wartości querystring dodany do adresu URL. Rozważmy na przykład aplikacji `contoso.com` zawierającą stronę logowania w `http://contoso.com/Account/LogOn?returnUrl=/Home/About`. Atak obejmuje następujące kroki:

1. Użytkownik kliknie to łącze do `http://contoso.com/Account/LogOn?returnUrl=http://contoso1.com/Account/LogOn` (drugi adres URL to "contoso**1**.com", nie "contoso.com").
2. Użytkownik, który loguje się pomyślnie.
3. Użytkownik jest przekierowywany (przez witrynę) do `http://contoso1.com/Account/LogOn` (złośliwych witryn, który wygląda tak samo jak real lokacji).
4. Użytkownik loguje się ponownie (dzięki czemu złośliwego lokacji poświadczeń) i jest przekierowywany do rzeczywistego witryny.

Prawdopodobnie użytkownik uważa, że pierwsza próba logowania nie powiodło się i że ich druga próba zakończyła się powodzeniem. Prawdopodobnie użytkownik pozostaje świadomości, że ich poświadczenia są naruszone.

![Proces ataku otwarte przekierowywanie](preventing-open-redirects/_static/open-redirection-attack-process.png)

Oprócz strony logowania niektóre witryny zapewniają przekierowania strony lub punktów końcowych. Wyobraź sobie, Twoja aplikacja ma stronę z otwarte przekierowywanie `/Home/Redirect`. Osoba atakująca może utworzyć na przykład łącze w wiadomości e-mail, który prowadzi do `[yoursite]/Home/Redirect?url=http://phishingsite.com/Home/Login`. Typowy użytkownik odczyta adres URL i zobacz zaczyna się od nazwy lokacji. Zaufanie, który, będzie kliknij łącze. Otwarte przekierowywanie będzie wysłać użytkownika do witryny wyłudzania informacji, który będzie taka sama jak należy do Ciebie, a użytkownik będzie prawdopodobnie Zaloguj się do będą uważali jest witryny.

## <a name="protecting-against-open-redirect-attacks"></a>Ochrona przed atakom na otwarte przekierowywanie

Podczas tworzenia aplikacji sieci web, wszystkie dane dostarczane przez użytkownika należy traktować jako niezaufana. Jeśli aplikacja ma funkcji, która przekierowuje użytkownika, w oparciu o zawartość adresu URL, upewnij się, że takie przekierowuje tylko są wykonywane lokalnie w aplikacji (lub znany adres URL nie dowolnego adresu URL, które mogą być dostarczane w zmiennej querystring).

### <a name="localredirect"></a>LocalRedirect

Użyj `LocalRedirect` metody pomocnika od podstawy `Controller` klasy:

```csharp
public IActionResult SomeAction(string redirectUrl)
{
    return LocalRedirect(redirectUrl);
}
```

`LocalRedirect` spowoduje zgłoszenie wyjątku, jeśli określono adres URL inną niż lokalna. W przeciwnym razie działa podobnie jak `Redirect` metody.

### <a name="islocalurl"></a>IsLocalUrl

Użyj [IsLocalUrl](/dotnet/api/Microsoft.AspNetCore.Mvc.IUrlHelper?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_IUrlHelper_IsLocalUrl_System_String_) metodę, aby przetestować, zanim zostaną przekierowywanie adresów URL:

Poniższy przykład pokazuje, jak sprawdzić, czy adres URL jest lokalny, przed przekierowaniem.

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

`IsLocalUrl` Metoda ochronę przypadkowo nastąpi przekierowanie do złośliwych witryn przez użytkowników. Można rejestrować szczegóły adresu URL, który został podany, gdy adres URL inną niż lokalna jest podany w sytuacji, gdy oczekiwany lokalny adres URL. Rejestrowanie przekierowania adresów URL może pomóc w diagnozowaniu ataków przekierowania.
