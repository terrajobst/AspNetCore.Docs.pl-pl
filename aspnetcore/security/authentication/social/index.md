---
title: "Włączanie uwierzytelniania za pomocą usługi Facebook, Google i innych dostawców zewnętrznych"
author: rick-anderson
description: "W tym samouczku pokazano, jak kompilacji platformy ASP.NET Core 2.x aplikacji przy użyciu protokołu OAuth 2.0 przy pomocy dostawcy uwierzytelniania zewnętrznego."
keywords: "Platformy ASP.NET Core, uwierzytelnianie, społecznych, dostawców uwierzytelniania, google, facebook, twitter, konta microsoft"
ms.author: riande
manager: wpickett
ms.date: 11/01/2016
ms.topic: article
ms.assetid: eda7ee17-f38c-462e-8d1d-63f459901cf3
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/social/index
ms.openlocfilehash: 9cc637f469dcb7097ee1b3996fde8a4ebac8d7ff
ms.sourcegitcommit: 198fb0488e961048bfa376cf58cb853ef1d1cb91
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/14/2017
---
# <a name="enabling-authentication-using-facebook-google-and-other-external-providers"></a>Włączanie uwierzytelniania za pomocą usługi Facebook, Google i innych dostawców zewnętrznych

<a name="security-authentication-social-logins"></a>

Przez [Valeriy Novytskyy](https://github.com/01binary) i [Rick Anderson](https://twitter.com/RickAndMSFT)

W tym samouczku przedstawiono sposób kompilacji platformy ASP.NET Core 2.x aplikacji, która umożliwia użytkownikom zalogowanie się przy użyciu protokołu OAuth 2.0 z poświadczeń zewnętrzni dostawcy uwierzytelniania.

[Facebook](facebook-logins.md), [Twitter](twitter-logins.md), [Google](google-logins.md), i [Microsoft](microsoft-logins.md) dostawców są przedstawione w poniższych sekcjach. Inni dostawcy są dostępne w pakiety innych firm takich jak [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) i [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).

![Ikony mediów społecznościowych dla usługi Facebook, Twitter, Google plus i systemu Windows](index/_static/social.png)

Umożliwienie użytkownikom logowania się przy użyciu swoich istniejących poświadczeń jest wygodne dla użytkowników i przenosi wiele złożoności Zarządzanie procesem logowania na innych firm. Przykłady jak społecznościowych logowania można dysku konwersje typów ruchu i klienta, można znaleźć przypadków przez [Facebook](https://www.facebook.com/unsupportedbrowser) i [Twitter](https://dev.twitter.com/resources/case-studies).

Uwaga: Przedstawionych w tym miejscu pakiety abstrakcyjnej dużą złożoność przepływ uwierzytelniania OAuth, ale opis szczegóły mogą okazać się konieczne podczas rozwiązywania problemów. Wiele zasobów są dostępne; na przykład, zobacz [wprowadzenie do OAuth 2](https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2) lub [2 OAuth opis](http://www.bubblecode.net/2016/01/22/understanding-oauth2/). Niektóre problemy można rozwiązać, analizując [kod źródłowy platformy ASP.NET Core dla pakietów dostawcy](https://github.com/aspnet/Security/tree/dev/src).

## <a name="create-a-new-aspnet-core-project"></a>Utwórz nowy projekt platformy ASP.NET Core

* W programie Visual Studio 2017 r, Utwórz nowy projekt z stronę początkową lub za pośrednictwem **Plik > Nowy > Projekt**.

* Wybierz **aplikacji sieci Web platformy ASP.NET Core** dostępne w szablonie **Visual C# > .NET Core** kategorii:

![Okno dialogowe nowego projektu](index/_static/new-project.png)

* Wybierz **aplikacji sieci Web** i sprawdź **uwierzytelniania** ustawiono **indywidualnych kont użytkowników**:

![Okno dialogowe nowego aplikacji sieci Web](index/_static/select-project.png)

Uwaga: Ten samouczek dotyczy wersji platformy ASP.NET Core SDK 2.0, który można wybrać w górnej części kreatora.

## <a name="apply-migrations"></a>Zastosuj migracje

* Uruchom aplikację i wybierz **Zaloguj** łącza.
* Wybierz **Zarejestruj się jako nowy użytkownik** łącza.
* Wprowadź adres e-mail i hasło dla nowego konta, a następnie wybierz **zarejestrować**.
* Postępuj zgodnie z instrukcjami, aby zastosować migracji.

## <a name="require-ssl"></a>Wymagaj protokołu SSL

OAuth 2.0 wymaga korzystania z protokołu SSL do uwierzytelniania za pośrednictwem protokołu HTTPS.

Uwaga: Projekty utworzone za pomocą **aplikacji sieci Web** lub **interfejsu API sieci Web** szablonów projektu dla platformy ASP.NET Core 2.x są automatycznie konfigurowane do włączenia protokołu SSL, a następnie uruchom z adresem URL https, jeśli **poszczególnych Konta użytkowników** została zaznaczona opcja **okno dialogowe Zmienianie uwierzytelniania** w Kreatorze projektu, jak pokazano powyżej.

* Wymagaj protokołu SSL w witrynie, wykonując kroki opisane w [Wymuszanie protokołu SSL w aplikacji platformy ASP.NET Core](xref:security/enforcing-ssl) tematu.

## <a name="use-secretmanager-to-store-tokens-assigned-by-login-providers"></a>SecretManager używany do przechowywania tokenów przypisany przez dostawców logowania

Przypisz dostawców logowania społecznościowych **identyfikator aplikacji** i **klucz tajny aplikacji** tokeny podczas procesu rejestracji (dokładne nazewnictwa jest zależna od dostawcy).

Te wartości są wydajnie *nazwy użytkownika* i *hasło* aplikacja korzysta z dostępu do interfejsu API i stanowią "kluczy tajnych" połączone za pomocą konfiguracji aplikacji **Manager klucz tajny** zamiast przechowywania ich w plikach konfiguracji bezpośrednio lub kodować je.

Postępuj zgodnie z instrukcjami [bezpiecznego magazynu kluczy tajnych aplikacji w czasie opracowywania w ASP.NET Core](xref:security/app-secrets) tematu, dzięki czemu można przechowywać tokeny przypisany przez każdy dostawca logowania poniżej.

## <a name="setup-login-providers-required-by-your-application"></a>Dostawców logowania Instalatora wymagane przez aplikację

Aby skonfigurować aplikację do używania odpowiednich dostawców, należy użyć następujące tematy:

* [Facebook](facebook-logins.md) instrukcje
* [W usłudze Twitter](twitter-logins.md) instrukcje
* [Google](google-logins.md) instrukcje
* [Microsoft](microsoft-logins.md) instrukcje
* [Inny dostawca](other-logins.md) instrukcje

## <a name="optionally-set-password"></a>Opcjonalnie Ustaw hasło.

Podczas rejestrowania przy użyciu dostawcy logowania zewnętrznego, nie masz hasła zarejestrowany z aplikacją. Pozwala to uniknąć z tworzeniem i zapamiętywanie hasła dla lokacji, ale zapewnia także możesz zależny od dostawcy logowania zewnętrznego. Jeśli dostawcy logowania zewnętrznego jest niedostępny, nie będzie można logować się do witryny sieci web.

Aby utworzyć hasło i zaloguj się przy użyciu poczty e-mail ustawioną podczas logowania w procesie z zewnętrznych źródeł:

* Wybierz **Hello <email alias>**  łącze w prawym górnym rogu, aby przejść do **Zarządzaj** widoku.

![Widok Zarządzaj aplikacji sieci Web](index/_static/pass1a.png)

* Wybierz **tworzenie**

![Ustawianie hasła strony](index/_static/pass2a.png)

* Ustawione prawidłowe hasło i umożliwia to Zaloguj się przy użyciu poczty e-mail.

## <a name="next-steps"></a>Następne kroki

* W tym artykule wprowadzono uwierzytelniania zewnętrznego i wyjaśniono wymagania wstępne dotyczące dodawania logowań zewnętrznych do aplikacji platformy ASP.NET Core.

* Odwołanie do strony specyficznego dla dostawcy można skonfigurować logowania dla dostawcy wymagane przez aplikację.
