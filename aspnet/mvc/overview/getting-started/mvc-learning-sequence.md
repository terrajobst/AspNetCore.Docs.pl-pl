---
uid: mvc/overview/getting-started/mvc-learning-sequence
title: Zalecane MVC, samouczki i artykuły dotyczące | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: Ta strona zawiera linki do samouczki platformy ASP.NET MVC i sugerowane sekwencji z nich.
ms.author: aspnetcontent
ms.date: 05/22/2015
ms.assetid: 8513a57a-2d45-4d6b-881c-15a01c5cbb1c
msc.legacyurl: /mvc/overview/getting-started/mvc-learning-sequence
msc.type: authoredcontent
ms.openlocfilehash: 7b9431123ea670b87949ed756aaf8e6d80011bc5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37827019"
---
<a name="mvc-recommended-tutorials-and-articles"></a>Zalecane samouczki i artykuły dotyczące MVC
====================
Przez [Rick Anderson](https://github.com/Rick-Anderson)

<a id="pwd"></a>
## <a name="getting-started"></a>Wprowadzenie

- [Wprowadzenie do ASP.NET MVC 5](introduction/getting-started.md) serii 11 ten jest dobrym miejscem do rozpoczęcia.
- [W witrynie Pluralsight platformy ASP.NET MVC 5 podstawy](https://pluralsight.com/training/Player?author=scott-allen&amp;name=aspdotnet-mvc5-fundamentals-m1-introduction&amp;mode=live&amp;clip=0&amp;course=aspdotnet-mvc5-fundamentals) (kurs wideo)
- [Wprowadzenie do platformy ASP.NET MVC](https://www.microsoftvirtualacademy.com/training-courses/introduction-to-asp-net-mvc) Jon Galloway'em i Christopher Harrison
- [Cykl życia aplikacji ASP.NET MVC 5](lifecycle-of-an-aspnet-mvc-5-application.md) dokument PDF, który wykresy cyklem życia aplikacji ASP.NET MVC 5.

<a id="con"></a>
## <a name="working-with-data"></a>Praca z danymi

- [Wprowadzenie do programów EF 6 Code First wykorzystaniem MVC 5](getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) Tom Dykstra nagradzanych serii zagłębił się szczegółowo EF.

<a id="wj"></a>
## <a name="security"></a>Zabezpieczenia

- [Tworzenie aplikacji ASP.NET MVC z uwierzytelnianiem i bazą danych SQL i wdrażanie na platformie Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) ten popularnych samouczek przedstawia proces tworzenia prostej aplikacji i dodawanie członkostwa i ról.
- [Tworzenie aplikacji platformy ASP.NET MVC 5 za pomocą usługi Facebook, Twitter, LinkedIn i Google OAuth2 logowanie jednokrotne](../security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) w tym samouczku dowiesz się, jak utworzyć aplikację sieci web ASP.NET MVC 5, która pozwala użytkownikom na logowanie przy użyciu poświadczeń z zewnętrznego uwierzytelniania przy użyciu protokołu OAuth 2.0 Dostawca, takich jak Facebook, Twitter, LinkedIn, Microsoft lub Google.
- [Tworzenie bezpiecznej aplikacji sieci web ASP.NET MVC 5 z logowaniem, adres e-mail potwierdzenia i resetowaniem hasła](../security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) najpierw w serii tożsamości, zawiera kod [Wyślij ponownie link do potwierdzenia](../security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md#rsend).
- [Aplikacja ASP.NET MVC 5 z wiadomości SMS i wiadomości e-mail uwierzytelniania dwuskładnikowego](../security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md) drugiej serii tożsamości.
- [Najlepsze rozwiązania dotyczące wdrażania haseł i innych poufnych danych na platformie ASP.NET i w usłudze Azure App Service](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md)
- [Uwierzytelnianie dwuskładnikowe za pomocą wiadomości SMS i wiadomości e-mail w produkcie ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) `isPersistent` i plik cookie zabezpieczeń kod wymaga od użytkownika posiadania konta zatwierdzonych wiadomości e-mail przed ich zalogowaniem się, jak SignInManager sprawdza, czy wymaganie uwierzytelniania 2FA i nie tylko.
- [Potwierdzenie konta i odzyskiwanie hasła w produkcie ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) zawiera szczegółowe informacje o tożsamości, nie można odnaleźć w [tworzenie bezpiecznej aplikacji sieci web ASP.NET MVC 5 z logowaniem, adres e-mail potwierdzenia i resetowaniem hasła](../security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) takie jak umożliwić Użytkownicy zresetować zapomniane hasła.

<a id="da"></a>
## <a name="azure"></a>Azure

- [Tworzenie aplikacji internetowej ASP.NET na platformie Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/) samouczek krótka i prosta do wdrożenia na platformie Azure.
- [Tworzenie aplikacji ASP.NET MVC z uwierzytelnianiem i bazą danych SQL i wdrażanie na platformie Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)

<a id="perf"></a>
## <a name="performance-and-debugging"></a>W zakresie wydajności debugowania

- [Profilowanie i debugowanie aplikacji ASP.NET MVC za pomocą pakietów Glimpse](../performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse.md)
