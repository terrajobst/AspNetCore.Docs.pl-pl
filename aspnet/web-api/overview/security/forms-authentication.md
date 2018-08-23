---
uid: web-api/overview/security/forms-authentication
title: Uwierzytelnianie formularzy we wzorcu ASP.NET Web API | Dokumentacja firmy Microsoft
author: MikeWasson
description: W tym artykule opisano, za pomocą uwierzytelniania formularzy programu ASP.NET Web API.
ms.author: riande
ms.date: 12/12/2012
ms.assetid: 9f06c1f2-ffaa-4831-94a0-2e4a3befdf07
msc.legacyurl: /web-api/overview/security/forms-authentication
msc.type: authoredcontent
ms.openlocfilehash: 35d62a83382553085ed8a728dcdcdae0e93090b8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41757182"
---
<a name="forms-authentication-in-aspnet-web-api"></a>Uwierzytelnianie formularzy we wzorcu ASP.NET Web API
====================
przez [Mike Wasson](https://github.com/MikeWasson)

Uwierzytelnianie formularzy używa formularza HTML w celu wysłania poświadczeń użytkownika do serwera. Nie jest standardem Internet. Uwierzytelnianie formularzy jest tylko internetowych interfejsów API, które są wywoływane z poziomu aplikacji internetowej tak, aby użytkownik może korzystać z formularza HTML.

| Zalety | Wady |
| --- | --- |
| -Łatwe do wdrożenia: wbudowane w platformy ASP.NET. -Używa dostawcy członkostwa platformy ASP.NET, która ułatwia zarządzanie kontami użytkowników. | -Nie standardowego protokołu HTTP mechanizm uwierzytelniania; używa plików cookie protokołu HTTP zamiast standardowy nagłówek autoryzacji. — Wymagają klienta przeglądarki. -Poświadczenia są wysyłane jako zwykły tekst. -Narażone na fałszerstwo żądania międzywitrynowego (CSRF); wymaga anti-CSRF miar bez ograniczeń. -Trudne do użycia z nonbrowser klientów. Logowanie wymaga przeglądarki. — Poświadczenia użytkownika są wysyłane w żądaniu. — Niektórzy użytkownicy wyłączają plików cookie. |

Krótko mówiąc uwierzytelnianie formularzy w programie ASP.NET: przebiega następująco:

1. Klient żąda zasobu, który wymaga uwierzytelniania.
2. Jeśli użytkownik nie jest uwierzytelniony, serwer zwraca HTTP 302 (Found) i wykonuje przekierowanie do strony logowania.
3. Użytkownik wprowadza poświadczenia i przesyła formularz.
4. Serwer zwraca inną HTTP 302, który przekierowuje oryginalnego identyfikatora URI. Ta odpowiedź zawiera pliku cookie uwierzytelniania.
5. Klient żąda zasobu ponownie. Żądanie zawiera pliku cookie uwierzytelniania, więc serwer nie udzieli żądania.

![](forms-authentication/_static/image1.png)

Aby uzyskać więcej informacji, zobacz [omówienie uwierzytelniania formularzy.](../../../web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-cs.md)

## <a name="using-forms-authentication-with-web-api"></a>Korzystanie z uwierzytelniania formularzy z interfejsu API sieci Web

Aby utworzyć aplikację, która korzysta z uwierzytelniania formularzy, wybierz szablon "Aplikacji internetowej" w Kreatorze projektu MVC 4. Ten szablon umożliwia utworzenie kontrolerów MVC w celu zarządzania kontami. Można również użyć szablonu "Aplikacja jednostronicowa" dostępnych w ASP.NET Fall 2012 Update.

W kontrolerach internetowego interfejsu API możesz ograniczyć dostęp za pomocą `[Authorize]` atrybutu, zgodnie z opisem w [przy użyciu atrybutu [Authorize]](authentication-and-authorization-in-aspnet-web-api.md#auth3).

Uwierzytelnianie formularzy używa pliku cookie sesji, do uwierzytelniania żądań. Przeglądarek automatycznie wysyła wszystkie odpowiednie pliki cookie do docelowej witryny sieci web. Ta funkcja sprawia, że uwierzytelnianie formularzy są potencjalnie narażone na fałszerstwo żądania międzywitrynowego (CSRF) ataki zobacz [ataków zapobieganie Cross-Site żądania Międzywitrynowego Międzywitrynowych](preventing-cross-site-request-forgery-csrf-attacks.md).

Uwierzytelnianie formularzy nie szyfruje poświadczenia użytkownika. W związku z tym uwierzytelnianie formularzy nie jest bezpieczny, chyba że używana przy użyciu protokołu SSL. Zobacz [Praca z protokołem SSL w składniku Web API](working-with-ssl-in-web-api.md).
