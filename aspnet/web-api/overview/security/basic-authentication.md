---
uid: web-api/overview/security/basic-authentication
title: Uwierzytelnianie podstawowe w interfejsie Web API platformy ASP.NET | Dokumentacja firmy Microsoft
author: MikeWasson
description: W tym artykule opisano, w interfejsie API sieci Web platformy ASP.NET przy użyciu uwierzytelniania podstawowego.
ms.author: riande
ms.date: 10/02/2014
ms.assetid: 41423767-0021-47c3-9e53-0021b457c39f
msc.legacyurl: /web-api/overview/security/basic-authentication
msc.type: authoredcontent
ms.openlocfilehash: 7afafb6b7851f0d955d1f4292318f64d2a068a45
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754086"
---
<a name="basic-authentication-in-aspnet-web-api"></a>Uwierzytelnianie podstawowe we wzorcu ASP.NET Web API
====================
przez [Mike Wasson](https://github.com/MikeWasson)

Uwierzytelnianie podstawowe jest zdefiniowany w [RFC 2617, uwierzytelnianie HTTP: podstawowe i uwierzytelnianie dostępu szyfrowanego](http://www.ietf.org/rfc/rfc2617.txt).

Wady

- Poświadczenia użytkownika są wysyłane w żądaniu.
- Poświadczenia są wysyłane jako zwykły tekst.
- Poświadczenia są wysyłane z każdym żądaniem.
- Sposób logowania, z wyjątkiem poprzez zakończenie sesji przeglądarki.
- Narażone na fałszerstwo żądania międzywitrynowego (CSRF); wymaga anti-CSRF miar bez ograniczeń.

Zalety

- Standardy internetowe.
- Obsługiwane przez wszystkie popularne przeglądarki.
- Protokół stosunkowo proste.

Uwierzytelnianie podstawowe działa w następujący sposób:

1. Jeśli żądanie wymaga uwierzytelnienia, serwer zwraca wartość 401 (bez autoryzacji). Odpowiedź zawiera nagłówka WWW-Authenticate, wskazujący, że serwer obsługuje uwierzytelnianie podstawowe.
2. Klient wysyła kolejne żądanie przy użyciu poświadczeń klienta w nagłówku autoryzacji. Poświadczenia są sformatowane jako ciąg "Nazwa: hasło", algorytmem Base64. Poświadczenia nie są szyfrowane.

Uwierzytelnianie podstawowe jest wykonywane w kontekście parametru "realm." Serwer zawiera nazwę obszaru nagłówka WWW-Authenticate. Poświadczenia użytkownika są prawidłowe w ramach tego obszaru. Dokładny zakres obszaru jest definiowany przez serwer. Na przykład można zdefiniować kilka obszarów w kolejności do partycji zasobów.

![](basic-authentication/_static/image1.png)

Ponieważ poświadczenia są wysyłane niezaszyfrowane, uwierzytelnianie podstawowe jest tylko bezpieczne za pośrednictwem protokołu HTTPS. Zobacz [Praca z protokołem SSL w składniku Web API](working-with-ssl-in-web-api.md).

Uwierzytelnianie podstawowe jest również narażone na ataki CSRF. Po użytkownik wprowadza poświadczenia, przeglądarka automatycznie wysyła je dla kolejnych żądań do tej samej domeny, na czas trwania sesji. W tym wysyłanie żądań AJAX. Zobacz [zapobieganie atakom na fałszerstwo żądania Międzywitrynowego (CSRF)](preventing-cross-site-request-forgery-csrf-attacks.md).

## <a name="basic-authentication-with-iis"></a>Uwierzytelnianie podstawowe w usługach IIS

Usługi IIS obsługuje uwierzytelnianie podstawowe, ale istnieje Ostrzeżenie: użytkownik jest uwierzytelniany względem poświadczeń Windows. Oznacza to, że użytkownik musi mieć konto w domenie serwera. Dla witryny sieci web publicznego zazwyczaj chcesz uwierzytelniać dostawcy członkostwa platformy ASP.NET.

Aby włączyć uwierzytelnianie podstawowe, za pomocą usług IIS, ustaw tryb uwierzytelniania "Windows" w pliku Web.config projektu programu ASP.NET:

[!code-xml[Main](basic-authentication/samples/sample1.xml)]

W tym trybie usług IIS przy użyciu poświadczeń Windows do uwierzytelniania. Ponadto należy włączyć uwierzytelnianie podstawowe w usługach IIS. W Menedżerze usług IIS, przejdź do widoku funkcji, wybierz opcję Uwierzytelnianie i włączyć uwierzytelnianie podstawowe.

![](basic-authentication/_static/image2.png)

W projekcie interfejsu API sieci Web Dodaj `[Authorize]` atrybutu dla wszystkich działań kontrolera, które wymagają uwierzytelniania.

Klient uwierzytelnia, ustawiając nagłówek autoryzacji w żądaniu. Klientów w przeglądarkach, które automatycznie wykonać ten krok. Klienci nonbrowser należy ustawić nagłówek.

## <a name="basic-authentication-with-custom-membership"></a>Uwierzytelnianie podstawowe z członkostwem niestandardowe

Jak wspomniano wcześniej, uwierzytelnianie podstawowe, wbudowanych usług IIS przy użyciu poświadczeń Windows. Oznacza to, że należy utworzyć konta dla użytkowników na serwerze hostingu. Jednak dla aplikacji internetowych, konta użytkowników są zazwyczaj przechowywane w zewnętrznej bazie danych.

Poniższy kod, w jaki sposób moduł HTTP, który wykonuje uwierzytelnianie podstawowe. Możesz łatwo podłączyć dostawcy członkostwa platformy ASP.NET, zastępując `CheckPassword` metody, która jest metodą fikcyjnego z rolą w tym przykładzie.

W sieci Web API 2, należy rozważyć pisania [filtr uwierzytelniania](authentication-filters.md) lub [oprogramowania pośredniczącego OWIN](../../../aspnet/overview/owin-and-katana/index.md), zamiast moduł protokołu HTTP.

[!code-csharp[Main](basic-authentication/samples/sample2.cs)]

Aby włączyć moduł HTTP, Dodaj następujący element do pliku web.config w **system.webServer** sekcji:

[!code-xml[Main](basic-authentication/samples/sample3.xml?highlight=4)]

Zastąp "YourAssemblyName" Nazwa zestawu (z wykluczeniem rozszerzenie "dll").

Należy wyłączyć inne schematy uwierzytelniania, takich jak uwierzytelniania formularzy lub Windows
