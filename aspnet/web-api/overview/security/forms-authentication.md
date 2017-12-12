---
uid: web-api/overview/security/forms-authentication
title: "Uwierzytelnianie formularzy w składniku ASP.NET Web API | Dokumentacja firmy Microsoft"
author: MikeWasson
description: "W tym artykule opisano, w interfejsie API sieci Web ASP.NET przy użyciu uwierzytelniania formularzy."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/12/2012
ms.topic: article
ms.assetid: 9f06c1f2-ffaa-4831-94a0-2e4a3befdf07
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/forms-authentication
msc.type: authoredcontent
ms.openlocfilehash: 9027d76bcf8854fc85f11906d3651511f350cd32
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="forms-authentication-in-aspnet-web-api"></a>Uwierzytelnianie formularzy w składniku ASP.NET Web API
====================
przez [Wasson Jan](https://github.com/MikeWasson)

Uwierzytelnianie formularzy używa formularza HTML do wysyłania poświadczeń użytkownika do serwera. Nie jest standardem Internet. Uwierzytelnianie formularzy jest tylko dla interfejsów API, które są nazywane z aplikacji sieci web w sieci web, dzięki czemu użytkownik może interakcyjnie formularza HTML.

| Zalety | Wady |
| --- | --- |
| -Łatwa do wdrożenia: wbudowane w program ASP.NET. -Używa dostawcy członkostwa ASP.NET, która ułatwia zarządzanie kontami użytkowników. | -Nie standardowe HTTP mechanizm uwierzytelniania; używa plików cookie protokołu HTTP zamiast standardowy nagłówek autoryzacji. -Wymaga, aby klient przeglądarki. -Poświadczenia są wysyłane w postaci zwykłego tekstu. -Narażone na fałszowanie żądań między witrynami (CSRF); wymaga anti-CSRF środków. -Trudny do wykorzystania z nonbrowser klientów. Logowanie wymaga przeglądarki. -Poświadczenia użytkownika są wysyłane w żądaniu. -Niektórzy użytkownicy wyłączają plików cookie. |

Krótko mówiąc uwierzytelnianie formularzy w programie ASP.NET przebiega następująco:

1. Klient żąda zasobu, który wymaga uwierzytelniania.
2. Jeśli użytkownik nie jest uwierzytelniony, serwer zwraca HTTP 302 (Found) i przekierowuje do strony logowania.
3. Użytkownik wprowadza poświadczenia i przesyła formularz.
4. Serwer zwraca innego HTTP 302, który przekierowuje oryginalnego identyfikatora URI. Ta odpowiedź zawiera pliku cookie uwierzytelniania.
5. Klient żąda zasobu ponownie. Żądanie zawiera pliku cookie uwierzytelniania, dlatego serwer przyznaje żądania.

![](forms-authentication/_static/image1.png)

Aby uzyskać więcej informacji, zobacz [omówienie uwierzytelniania formularzy.](../../../web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-cs.md)

## <a name="using-forms-authentication-with-web-api"></a>Korzystanie z uwierzytelniania formularzy z interfejsu API sieci Web

Aby utworzyć aplikację, która korzysta z uwierzytelniania formularzy, wybierz szablon "Aplikacji internetowej" w Kreatorze projektu MVC 4. Ten szablon tworzy kontrolerów MVC dla konta administracyjnego. Można również użyć szablonu "Jednej strony aplikacji", dostępne w aktualizacji 2012 spadek ASP.NET.

W kontrolerach interfejsu API sieci web można ograniczyć dostęp za pomocą `[Authorize]` atrybutu, zgodnie z opisem w [za pomocą atrybutu [Authorize]](authentication-and-authorization-in-aspnet-web-api.md#auth3).

Uwierzytelnianie formularzy używa pliku cookie sesji do uwierzytelniania żądań. Przeglądarek automatycznie wysyłaj wszystkie odpowiednie pliki cookie do docelowej witryny sieci web. Ta funkcja umożliwia uwierzytelnianie formularzy potencjalnie narażone na żądanie, zobacz ataków sfałszowaniem (CSRF) między lokacjami [ataków uniemożliwia Cross-Site żądania Międzywitrynowego (CSRF)](preventing-cross-site-request-forgery-csrf-attacks.md).

Uwierzytelnianie formularzy nie szyfruje poświadczenia użytkownika. W związku z tym uwierzytelnianie formularzy nie jest bezpieczne, chyba że używana przy użyciu protokołu SSL. Zobacz [Praca z protokołem SSL w składniku Web API](working-with-ssl-in-web-api.md).
