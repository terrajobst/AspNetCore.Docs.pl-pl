---
uid: web-api/overview/security/basic-authentication
title: "Uwierzytelnianie podstawowe w składniku ASP.NET Web API | Dokumentacja firmy Microsoft"
author: MikeWasson
description: "W tym artykule opisano, w interfejsie API sieci Web ASP.NET przy użyciu uwierzytelniania podstawowego."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/02/2014
ms.topic: article
ms.assetid: 41423767-0021-47c3-9e53-0021b457c39f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/basic-authentication
msc.type: authoredcontent
ms.openlocfilehash: 4b8e6410668b2db289488bb4b6cd26d881e70f4c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="basic-authentication-in-aspnet-web-api"></a>Uwierzytelnianie podstawowe w składniku ASP.NET Web API
====================
przez [Wasson Jan](https://github.com/MikeWasson)

Uwierzytelnianie podstawowe jest zdefiniowany w [RFC 2617, uwierzytelnianie HTTP: Basic i uwierzytelniania szyfrowanego dostępu](http://www.ietf.org/rfc/rfc2617.txt).

Wady

- Poświadczenia użytkownika są wysyłane w żądaniu.
- Poświadczenia są wysyłane w postaci zwykłego tekstu.
- Poświadczenia są wysyłane z każdym żądaniem.
- Nie można wylogować się, chyba że przez kończenie sesji przeglądarki.
- Narażone na krzyżowych żądania międzywitrynowego (CSRF); wymaga anti-CSRF środków.

Zalety

- Standardy internetowe.
- Obsługuje wszystkie główne przeglądarki.
- Protokół stosunkowo proste.

Uwierzytelnianie podstawowe działa w następujący sposób:

1. Jeśli żądanie wymaga uwierzytelnienia, serwer zwraca 401 (bez autoryzacji). Odpowiedź zawiera nagłówek WWW-Authenticate, wskazujący, że serwer obsługuje uwierzytelnianie podstawowe.
2. Klient wysyła żądanie innego, przy użyciu poświadczeń klienta w nagłówku autoryzacji. Poświadczenia są sformatowane jako ciąg "Nazwa: hasło", algorytmem Base64. Poświadczenia nie są szyfrowane.

Uwierzytelnianie podstawowe jest wykonywane w kontekście "obszar". Serwer zawiera nazwę obszaru nagłówka WWW-Authenticate. Poświadczenia użytkownika są prawidłowe w ramach tego obszaru. Dokładne zakresu obszaru jest zdefiniowany przez serwer. Na przykład można zdefiniować kilku obszarów w celu zasobów partycji.

![](basic-authentication/_static/image1.png)

Ponieważ poświadczenia są wysyłane niezaszyfrowane, uwierzytelnianie podstawowe jest tylko bezpieczne za pośrednictwem protokołu HTTPS. Zobacz [Praca z protokołem SSL w składniku Web API](working-with-ssl-in-web-api.md).

Uwierzytelnianie podstawowe jest również narażony na ataki CSRF. Po użytkownik wprowadza poświadczenia, przeglądarka automatycznie wysyła je na kolejnych żądań do tej samej domeny, czas trwania sesji. W tym żądania AJAX. Zobacz [zapobieganie Fałszerstwie żądania Międzywitrynowego (CSRF)](preventing-cross-site-request-forgery-csrf-attacks.md).

## <a name="basic-authentication-with-iis"></a>Uwierzytelnianie podstawowe w usługach IIS

Usługi IIS obsługują uwierzytelnianie podstawowe, ale istnieje Ostrzeżenie: użytkownik jest uwierzytelniany na podstawie swoich poświadczeń systemu Windows. Oznacza to, że użytkownik musi mieć konto w domenie serwera. Witryny sieci web publicznych zwykle można uwierzytelniać na dostawcy członkostwa ASP.NET.

Aby włączyć uwierzytelnianie podstawowe, za pomocą usług IIS, ustaw tryb uwierzytelniania "Windows" w pliku Web.config projektu programu ASP.NET:

[!code-xml[Main](basic-authentication/samples/sample1.xml)]

W tym trybie usługi IIS używają poświadczeń systemu Windows do uwierzytelniania. Ponadto należy włączyć uwierzytelnianie podstawowe w usługach IIS. W Menedżerze usług IIS przejdź do widoku funkcji, wybierz uwierzytelnianie, a następnie włączyć uwierzytelnianie podstawowe.

![](basic-authentication/_static/image2.png)

W projekcie interfejsu API sieci Web, Dodaj `[Authorize]` atrybutu dla każdej akcji kontrolera, które wymagają uwierzytelniania.

Klient samodzielnie przeprowadza uwierzytelnianie przez ustawienie nagłówek autoryzacji w żądaniu. Klientów w przeglądarkach automatycznego wykonania tego kroku. Klienci nonbrowser należy ustawić nagłówka.

## <a name="basic-authentication-with-custom-membership"></a>Uwierzytelnianie podstawowe z członkostwem niestandardowych

Jak wspomniano, uwierzytelnianie podstawowe zapewnia wbudowaną usług IIS korzysta z poświadczeń systemu Windows. Oznacza to, że należy utworzyć konta dla użytkowników na serwerze hostingu. Ale aplikację internetową kont użytkowników zwykle są przechowywane w zewnętrznej bazy danych.

Poniższy kod, w jaki sposób moduł protokołu HTTP, który przeprowadza uwierzytelnianie podstawowe. Dostawcy członkostwa ASP.NET mogą łatwo dodatku, zastępując `CheckPassword` metodę, która jest atrapa metoda w tym przykładzie.

W sieci Web API 2, należy rozważyć zapisu [filtr uwierzytelniania](authentication-filters.md) lub [oprogramowanie pośredniczące OWIN](../../../aspnet/overview/owin-and-katana/index.md), zamiast modułu HTTP.

[!code-csharp[Main](basic-authentication/samples/sample2.cs)]

Aby włączyć moduł protokołu HTTP, Dodaj następujący plik web.config w **system.webServer** sekcji:

[!code-xml[Main](basic-authentication/samples/sample3.xml?highlight=4)]

Zamień na nazwę zestawu (nie z rozszerzeniem "dll") "YourAssemblyName".

Należy wyłączyć inne schematy uwierzytelniania, takich jak uwierzytelniania formularzy lub systemu Windows
