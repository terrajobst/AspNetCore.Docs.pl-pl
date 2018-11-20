---
title: Przegląd zabezpieczeń platformy ASP.NET Core
author: tdykstra
description: Poznaj podstawowe informacje dotyczące uwierzytelniania, autoryzacji i zabezpieczeń w programie ASP.NET Core.
ms.author: tdykstra
ms.custom: mvc
ms.date: 10/24/2018
uid: security/index
ms.openlocfilehash: 579e472e01efd08bbafe949e37a3b655a42a5b46
ms.sourcegitcommit: 04b55a5ce9d649ff2df926157ec28ae47afe79e2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/20/2018
ms.locfileid: "52156922"
---
# <a name="overview-of-aspnet-core-security"></a>Przegląd zabezpieczeń platformy ASP.NET Core

Platforma ASP.NET Core umożliwia deweloperom łatwe konfigurowanie i zarządzanie zabezpieczeniami dla swoich aplikacji. Platforma ASP.NET Core zawiera funkcje zarządzania uwierzytelniania, autoryzacji, ochrony danych, Wymuszanie protokołu SSL, wpisy tajne aplikacji, ochrona przed fałszerstwem żądań ochrony i zarządzanie mechanizmu CORS. Te funkcje zabezpieczeń pozwalają na tworzenie niezawodnych, ale zabezpieczanie aplikacji platformy ASP.NET Core.

## <a name="aspnet-core-security-features"></a>Funkcje zabezpieczeń platformy ASP.NET Core

Platforma ASP.NET Core udostępnia wiele narzędzi i bibliotek, aby zabezpieczyć swoje aplikacje, w tym Wbudowani dostawcy tożsamości, ale można użyć 3rd usług tożsamości innych firm, takich jak Facebook, Twitter i LinkedIn. Platforma ASP.NET Core umożliwia łatwe zarządzanie wpisy tajne aplikacji, które służą do przechowywania i korzystania z poufnych informacji bez ujawniania ich w kodzie.

## <a name="authentication-vs-authorization"></a>Uwierzytelnianie programu vs. Autoryzacja

Uwierzytelnianie jest procesem, w którym użytkownik podaje poświadczenia, które następnie są porównywane te, przechowywane w systemie operacyjnym, bazy danych, aplikacji lub zasobów. Jeśli są zgodne, użytkownikom pomyślnie uwierzytelnione i można wykonać akcje, które otrzymali oni autoryzację pozwalającą, podczas procesu autoryzacji. Autoryzacja odnosi się do procesu, który określa, jakie użytkownik może wykonywać.

Innym sposobem, aby traktować uwierzytelniania jest należy wziąć pod uwagę jako sposób wprowadź spację, np. serwera, bazy danych, aplikacji lub zasobów, podczas gdy autoryzacja to akcje, które może wykonywać użytkownik, do których obiektów wewnątrz tego miejsca (serwer, baza danych lub aplikacji).

## <a name="common-vulnerabilities-in-software"></a>Typowe luk w zabezpieczeniach oprogramowania

Platforma ASP.NET Core i programem EF zawiera funkcje, które ułatwiają zabezpieczanie aplikacji i zapobieganie naruszeniom bezpieczeństwa. Poniższa lista łącza spowoduje przejście do dokumentacji techniki takimi szczegółami, jak uniknąć typowych luk w zabezpieczeniach w usłudze web apps:

* [Ataki z użyciem skryptów między witrynami](xref:security/cross-site-scripting)
* [Ataki polegające na iniekcji SQL](/ef/core/querying/raw-sql)
* [Cross-Site Request Forgery (CSRF)](xref:security/anti-request-forgery)
* [Atakom na otwarte przekierowywanie](xref:security/preventing-open-redirects)

Ma więcej luk w zabezpieczeniach, które należy wiedzieć. Aby uzyskać więcej informacji, zobacz artykuły w **zabezpieczenia i tożsamość** części spisu treści.
