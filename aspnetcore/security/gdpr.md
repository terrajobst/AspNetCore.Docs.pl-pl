---
title: Dane ogólne rozporządzenie o ochronie danych (GDPR) obsługi w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak uzyskać dostęp do punktów rozszerzenia RODO w aplikacji sieci web platformy ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/29/2018
uid: security/gdpr
ms.openlocfilehash: bbb3b8e091b5a0be8e852d70ba1a5d7100782ba3
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665379"
---
# <a name="eu-general-data-protection-regulation-gdpr-support-in-aspnet-core"></a>Obsługa Unii Europejskiej ogólnego danych (GDPR Protection Regulation) w programie ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

Platformy ASP.NET Core udostępnia interfejsów API i szablony, które ułatwiają korzystanie z niektórych [Unii Europejskiej ogólnego danych (GDPR Protection Regulation)](https://www.eugdpr.org/) wymagania:

* Szablony projektów obejmują punktów rozszerzeń i przekształcone na wycinki kod znaczników, który można zastąpić prywatności i plików cookie zasady.
* Funkcja zgody plik cookie umożliwia poprosić o (i śledzenia) zgody od użytkowników do przechowywania informacji osobistych. Jeśli użytkownik nie wyraził zgodę na zbieranie danych, a aplikacja ma [CheckConsentNeeded](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.checkconsentneeded) równa `true`, inne niż niezbędne pliki cookie nie są wysyłane do przeglądarki.
* Pliki cookie mogą być oznaczane jako podstawowe. Podstawowe pliki cookie są wysyłane do przeglądarki, nawet wtedy, gdy użytkownik nie wyraził zgodę i śledzenie jest wyłączone.
* [Pliki cookie sesji i TempData](#tempdata) nie są funkcjonalności, gdy śledzenie jest wyłączone.
* [Zarządzanie tożsamościami](#pd) strona zawiera również link do pobierające i usuwające dane użytkownika.

[Przykładową aplikację](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) umożliwia testowanie większość punktów rozszerzenia RODO i interfejsów API dodano szablony platformy ASP.NET Core 2.1. Zobacz [ReadMe](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) testowania instrukcje w pliku.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))

## <a name="aspnet-core-gdpr-support-in-template-generated-code"></a>Obsługa rozporządzenia RODO platformy ASP.NET Core w szablonie wygenerowanego kodu

Strony razor i MVC projekty utworzone za pomocą szablonów projektu obejmują obsługę RODO:

* [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) i [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) są ustawiane w `Startup`.
* *_CookieConsentPartial.cshtml* [widoku częściowego](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper).
* *Pages/Privacy.cshtml* strony lub *Views/Home/Privacy.cshtml* widok zawiera stronę, aby szczegółowo opisują zasady zachowania poufności informacji witryny. *_CookieConsentPartial.cshtml* plik generuje łącze do strony ochrony prywatności.
* W przypadku aplikacji utworzonych za pomocą indywidualnych kont użytkowników, na stronie Zarządzanie zawiera łącza do pobierające i usuwające [dane osobiste użytkownika](#pd).

### <a name="cookiepolicyoptions-and-usecookiepolicy"></a>CookiePolicyOptions i UseCookiePolicy

[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) są inicjowane w `Startup.ConfigureServices`:

[!code-csharp[Main](gdpr/sample/Startup.cs?name=snippet1&highlight=14-20)]

[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) jest wywoływana w `Startup.Configure`:

[!code-csharp[](gdpr/sample/Startup.cs?name=snippet1&highlight=51)]

### <a name="cookieconsentpartialcshtml-partial-view"></a>_CookieConsentPartial.cshtml partial view

The *_CookieConsentPartial.cshtml* partial view:

[!code-html[](gdpr/sample/RP/Pages/Shared/_CookieConsentPartial.cshtml)]

Tej części:

* Pobiera stan śledzenia dla użytkownika. Jeśli aplikacja jest skonfigurowana do wymagają zgody, użytkownik musi wyrazić zgodę przed pliki cookie mogą być śledzone. Jeśli wymagana jest zgoda, panel zgody plik cookie jest ustalony na górnej części paska nawigacyjnego, utworzone przez *_Layout.cshtml* pliku.
* Udostępnia kodu HTML `<p>` element, aby podsumować, prywatności i plików cookie przy użyciu zasad.
* Zawiera również link do strony prywatności lub widok, gdzie możesz szczegółowo opisują zasady zachowania poufności informacji witryny.

## <a name="essential-cookies"></a>Podstawowe pliki cookie

Jeśli nie udzielono zgody, tylko pliki cookie oznaczone niezbędne są wysyłane do przeglądarki. Poniższy kod sprawia, że plik cookie jest podstawowe:

[!code-csharp[Main](gdpr/sample/RP/Pages/Cookie.cshtml.cs?name=snippet1&highlight=5)]

<a name="tempdata"></a>

## <a name="tempdata-provider-and-session-state-cookies-are-not-essential"></a>Tempdata dostawcy sesji stanu plików cookie i nie są istotne

[Dostawcy Tempdata](xref:fundamentals/app-state#tempdata) pliku cookie nie jest niezbędne. Jeśli śledzenie jest wyłączone, dostawca Tempdata nie jest funkcjonalności. Aby umożliwić dostawcy Tempdata, gdy śledzenie jest wyłączone, Oznacz plik cookie TempData za istotne w `Startup.ConfigureServices`:

[!code-csharp[Main](gdpr/sample/RP/Startup.cs?name=snippet1)]

[Stan sesji](xref:fundamentals/app-state) pliki cookie nie są istotne. Stan sesji nie jest funkcjonalnych, jeśli śledzenie jest wyłączone. Poniższy kod sprawia, że pliki cookie z sesji podstawowe:

[!code-csharp[](gdpr/sample/RP/Startup.cs?name=snippet2)]

<a name="pd"></a>

## <a name="personal-data"></a>Dane osobowe

Aplikacje platformy ASP.NET Core utworzonych za pomocą indywidualnych kont użytkowników, to kod, aby pobrać i usunięcie danych osobowych.

Wybierz nazwę użytkownika, a następnie wybierz pozycję **danych osobowych**:

![Zarządzanie stroną danych osobowych](gdpr/_static/pd.png)

Uwagi:

* Aby wygenerować `Account/Manage` kod, zobacz [tożsamości szkieletu](xref:security/authentication/scaffold-identity).
* **Usuń** i **Pobierz** łącza działać tylko w przypadku domyślnych danych tożsamości. Aplikacje tworzone danych niestandardowych użytkownika musi zostać rozszerzony do usuwania/pobierania danych niestandardowych użytkownika. Aby uzyskać więcej informacji, zobacz [Dodawanie, pobieranie i usuwanie danych niestandardowych użytkownika tożsamości](xref:security/authentication/add-user-data).
* Zapisano tokeny dla danego użytkownika, które są przechowywane w tabeli bazy danych tożsamości `AspNetUserTokens` są usuwane po usunięciu użytkownika za pomocą kaskadowych zachowanie dotyczące usuwania ze względu na [klucz obcy](https://github.com/aspnet/Identity/blob/release/2.1/src/EF/IdentityUserContext.cs#L152).
* [Uwierzytelnianie zewnętrznego dostawcy](xref:security/authentication/social/index), takie jak Facebook i Google, nie jest dostępna przed zaakceptowaniu zasad pliku cookie.

## <a name="encryption-at-rest"></a>Szyfrowanie w spoczynku

Niektóre bazy danych i mechanizmy magazynu umożliwia szyfrowanie danych magazynowanych. Szyfrowanie danych magazynowanych:

* Automatycznie szyfruje dane przechowywane.
* Szyfruje bez konfiguracji, programowania lub innych zadań dotyczących oprogramowania, który uzyskuje dostęp do danych.
* Jest to najprostszy i najbezpieczniejszy opcja.
* Umożliwia zarządzanie kluczami i szyfrowania w bazie danych.

Na przykład:

* Program Microsoft SQL i Azure SQL udostępniają [funkcji Transparent Data Encryption](/sql/relational-databases/security/encryption/transparent-data-encryption) (TDE).
* [Bazy danych w SQL Azure są szyfrowane domyślnie](https://azure.microsoft.com/updates/newly-created-azure-sql-databases-encrypted-by-default/)
* [Azure obiektów blob, plików, tabela i Queue Storage są domyślnie szyfrowane](https://azure.microsoft.com/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/).

W przypadku baz danych, które nie udostępniają wbudowane szyfrowanie danych magazynowanych można udostępnić taką samą ochronę za pomocą szyfrowania dysków. Na przykład:

* [Funkcja BitLocker dla systemu Windows Server](/windows/security/information-protection/bitlocker/bitlocker-how-to-deploy-on-windows-server)
* W systemie Linux:
  * [eCryptfs](https://launchpad.net/ecryptfs)
  * [EncFS](https://github.com/vgough/encfs).

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Microsoft.com/GDPR](https://www.microsoft.com/trustcenter/Privacy/GDPR)
