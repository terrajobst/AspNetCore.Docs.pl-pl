---
title: Zapobiegaj atakom typu "Open redirect" w ASP.NET Core
author: ardalis
description: Pokazuje, jak zapobiec atakom typu "Open redirect" w aplikacji ASP.NET Core
ms.author: riande
ms.date: 07/07/2017
uid: security/preventing-open-redirects
ms.openlocfilehash: 9d8cac8708fe9aeadba5af1287362a20df7f6bfe
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78660525"
---
# <a name="prevent-open-redirect-attacks-in-aspnet-core"></a>Zapobiegaj atakom typu "Open redirect" w ASP.NET Core

Aplikacja sieci Web, która przekierowuje do adresu URL, który jest określony za pośrednictwem żądania, takiego jak QueryString lub dane formularza, może zostać naruszona w celu przekierowania użytkowników do zewnętrznego, złośliwego adresu URL. Takie manipulowanie nazywa się atakiem typu Open przekierowania.

Za każdym razem, gdy logika aplikacji przekieruje do określonego adresu URL, należy sprawdzić, czy adres URL przekierowania nie został naruszony. ASP.NET Core ma wbudowaną funkcję ułatwiającą ochronę aplikacji przed atakami typu Open redirect (nazywanymi także otwartym przekierowaniem).

## <a name="what-is-an-open-redirect-attack"></a>Co to jest atak typu "Open redirect"?

Aplikacje sieci Web często przekierowują użytkowników na stronę logowania, gdy uzyskują dostęp do zasobów wymagających uwierzytelniania. Przekierowanie zawiera zwykle parametr `returnUrl` QueryString, dzięki czemu użytkownik może zostać zwrócony do pierwotnie żądanego adresu URL po pomyślnym zalogowaniu się. Po uwierzytelnieniu użytkownik zostanie przekierowany do adresu URL, na który pierwotnie żądał.

Ponieważ docelowy adres URL jest określony w ciągu kwerendy żądania, złośliwy użytkownik może naruszać ten ciąg. Ciąg QueryString z naruszonymi komunikatami może pozwolić witrynie na przekierowanie użytkownika do zewnętrznej, szkodliwej lokacji. Ta technika jest nazywana atakiem typu Open redirect (lub przekierowaniem).

### <a name="an-example-attack"></a>Przykład ataku

Złośliwy użytkownik może stworzyć atak przeznaczony do zezwalania złośliwemu użytkownikowi na dostęp do poświadczeń lub informacji poufnych. Aby rozpocząć atak, złośliwy użytkownik zakończył pracę użytkownika w celu kliknięcia linku do strony logowania do witryny z wartością `returnUrl` QueryString dodaną w adresie URL. Rozważmy na przykład aplikację w `contoso.com`, która obejmuje stronę logowania w `http://contoso.com/Account/LogOn?returnUrl=/Home/About`. Atakujący wykonuje następujące czynności:

1. Użytkownik klika złośliwe łącze do `http://contoso.com/Account/LogOn?returnUrl=http://contoso1.com/Account/LogOn` (drugi adres URL to "contoso**1**. com", a nie "contoso.com").
2. Logowanie użytkownika zakończyło się pomyślnie.
3. Użytkownik zostanie przekierowany (przez lokację) do `http://contoso1.com/Account/LogOn` (złośliwa witryna, która wygląda tak samo jak prawdziwa witryna).
4. Użytkownik loguje się ponownie (podając złośliwe witryny jako poświadczenia) i zostaje przekierowany z powrotem do rzeczywistej lokacji.

Użytkownik prawdopodobnie zauważa, że pierwsza próba zalogowania nie powiodła się, a druga próba zakończyła się pomyślnie. Użytkownik najprawdopodobniej nie będzie świadomy naruszenia bezpieczeństwa poświadczeń.

![Proces ataku typu Otwórz przekierowanie](preventing-open-redirects/_static/open-redirection-attack-process.png)

Oprócz stron logowania niektóre lokacje udostępniają strony lub punkty końcowe przekierowania. Wyobraź sobie, że aplikacja zawiera stronę z otwartym przekierowaniem, `/Home/Redirect`. Osoba atakująca może utworzyć na przykład link w wiadomości e-mail, który przechodzi do `[yoursite]/Home/Redirect?url=http://phishingsite.com/Home/Login`. Typowy użytkownik zobaczy adres URL i rozpocznie się po jego nazwie. Zaufanie to spowoduje kliknięcie linku. Otwarte przekierowanie spowoduje następnie wysłanie użytkownika do witryny wyłudzania informacji, która wygląda identycznie, a użytkownik może zalogować się do Twojej witryny.

## <a name="protecting-against-open-redirect-attacks"></a>Ochrona przed atakami typu Open redirect

Podczas tworzenia aplikacji sieci Web Traktuj wszystkie dane dostarczone przez użytkownika jako niezaufane. Jeśli aplikacja zawiera funkcję, która przekierowuje użytkownika na podstawie zawartości adresu URL, należy się upewnić, że takie przekierowania są wykonywane tylko lokalnie w aplikacji (lub na znanym adresie URL, nie na żadnym z adresów URL, które mogą być dostarczone w ciągu QueryString).

### <a name="localredirect"></a>LocalRedirect

Użyj metody pomocnika `LocalRedirect` z klasy podstawowej `Controller`:

```csharp
public IActionResult SomeAction(string redirectUrl)
{
    return LocalRedirect(redirectUrl);
}
```

Jeśli określony adres URL nie jest adresem lokalnym, `LocalRedirect` zgłosi wyjątek. W przeciwnym razie zachowuje się tak jak Metoda `Redirect`.

### <a name="islocalurl"></a>IsLocalUrl

Użyj metody [IsLocalUrl](/dotnet/api/Microsoft.AspNetCore.Mvc.IUrlHelper.islocalurl#Microsoft_AspNetCore_Mvc_IUrlHelper_IsLocalUrl_System_String_) , aby przetestować adresy URL przed przekierowaniem:

Poniższy przykład pokazuje, jak sprawdzić, czy adres URL jest lokalny przed przekierowaniem.

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

Metoda `IsLocalUrl` chroni użytkowników przed przypadkowym przekierowaniem do złośliwej witryny. Można rejestrować szczegółowe informacje o adresie URL, który został podany w przypadku podania nielokalnego adresu URL w sytuacji, w której oczekiwano lokalnego adresu URL. Adresy URL przekierowania rejestrowania mogą pomóc w diagnozowaniu ataków przekierowania.
