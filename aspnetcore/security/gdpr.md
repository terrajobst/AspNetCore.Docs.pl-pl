---
title: Obsługa ochrony rozporządzenia (GDPR) ogólne dane w ASP.NET Core
author: rick-anderson
description: Pokazuje, jak punkty rozszerzeń GDPR w ASP.NET Core dostępu do aplikacji sieci web.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 5/29/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/gdpr
ms.openlocfilehash: dc1724e8a78c25d3697d14ad784ce853737681f2
ms.sourcegitcommit: 1b94305cc79843e2b0866dae811dab61c21980ad
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/24/2018
---
# <a name="eu-general-data-protection-regulation-gdpr-support-in-aspnet-core"></a>Obsługa interfejsów UE ogólne dane ochrony rozporządzenia (GDPR) w ASP.NET Core

przez [Rick Anderson](https://twitter.com/RickAndMSFT)

Platformy ASP.NET Core udostępnia interfejsy API i szablony, które ułatwiają korzystanie z niektórych [interfejsów UE ogólne dane ochrony rozporządzenia (GDPR)](https://www.eugdpr.org/) wymagania:

* Szablony projektu to punkty rozszerzenia i zastępczych znaczników, który można zastąpić prywatności i plików cookie użyj zasad.
* Funkcja zgody plik cookie umożliwia podanie (i śledzenia) zgody od użytkowników do przechowywania informacji osobistych. Jeśli użytkownik nie ma zgodę na zbieranie danych i aplikacji została skonfigurowana z [CheckConsentNeeded](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.checkconsentneeded?view=aspnetcore-2.1#Microsoft_AspNetCore_Builder_CookiePolicyOptions_CheckConsentNeeded) do `true`, nieistotne pliki cookie nie będą wysyłane do przeglądarki.
* Pliki cookie może być oznaczony jako ważne. Niezbędne pliki cookie są wysyłane do przeglądarki, nawet wtedy, gdy użytkownik zgodził się nie śledzenie jest wyłączone.
* [Pliki cookie sesji i TempData](#tempdata) nie działają podczas śledzenia jest wyłączona.
* [Zarządzanie tożsamościami](#pd) strona zawiera również link do pobierania i usuwania danych użytkownika.

[Przykładowa aplikacja](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) umożliwia testowanie większość punktów rozszerzenia GDPR i dodane do szablonów platformy ASP.NET Core 2.1 interfejsów API. Zobacz [ReadMe](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) pliku do testowania instrukcje.

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

## <a name="aspnet-core-gdpr-support-in-template-generated-code"></a>Obsługa GDPR platformy ASP.NET Core w szablonie wygenerowanego kodu

Stron razor i MVC projektów utworzonych za pomocą szablonów projektu obejmują obsługę GDPR:

* [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions?view=aspnetcore-2.0) i [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_CookiePolicyAppBuilderExtensions_UseCookiePolicy_Microsoft_AspNetCore_Builder_IApplicationBuilder_) są ustawione w `Startup`.
* *_CookieConsentPartial.cshtml* [widoku częściowego](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper).
* *Pages/Privacy.cshtml* lub *Home/rivacy.cshtml* widok zawiera strona szczegółów zasady zachowania poufności informacji witryny. *_CookieConsentPartial.cshtml* plik generuje łącze do strony prywatności.
* Dla aplikacji utworzonych za pomocą indywidualne konta użytkowników, na stronie Zarządzanie zawiera łącza do pobierania i usuwania [dane osobiste użytkownika](#pd).

### <a name="cookiepolicyoptions-and-usecookiepolicy"></a>CookiePolicyOptions i UseCookiePolicy

[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions?view=aspnetcore-2.0) są inicjowane w `Startup` klasy `ConfigureServices` metody:

[!code-csharp[Main](gdpr/sample/Startup.cs?name=snippet1&highlight=14-20)]

[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_CookiePolicyAppBuilderExtensions_UseCookiePolicy_Microsoft_AspNetCore_Builder_IApplicationBuilder_) jest wywoływana `Startup` klasy `Configure` metody:

[!code-csharp[Main](gdpr/sample/Startup.cs?name=snippet1&highlight=49)]

### <a name="cookieconsentpartialcshtml-partial-view"></a>Widok częściowy _CookieConsentPartial.cshtml

*_CookieConsentPartial.cshtml* widoku częściowego:

[!code-html[Main](gdpr/sample/RP/Pages/Shared/_CookieConsentPartial.cshtml)]

Tej części:

* Pobiera stan śledzenia dla użytkownika. Aplikację skonfigurowano wymaganie zgody użytkownik musi wyrazić zgodę przed plików cookie mogą być śledzone. Jeśli wymagana jest zgoda, chrome zgody plik cookie jest ustalone na pasku nawigacyjnym utworzone w *Pages/Shared/_Layout.cshtml* pliku.
* Udostępnia HTML `<p>` element, aby podsumować prywatności i plików cookie przy użyciu zasad.
* Link do *Pages/Privacy.cshtml* gdzie można szczegółowe zasady zachowania poufności informacji witryny.

## <a name="essential-cookies"></a>Niezbędne pliki cookie

Jeśli nie udzielono zgody, tylko pliki cookie oznaczone jako ważne są wysyłane do przeglądarki. Poniższy kod sprawia, że plik cookie jest istotne:

[!code-csharp[Main](gdpr/sample/RP/Pages/Cookie.cshtml.cs?name=snippet1&highlight=5)]

<a name="tempdata"></a>

## <a name="tempdata-provider-and-session-state-cookies-are-not-essential"></a>Tempdata dostawcy i sesji stanu pliki cookie nie są istotne

[Dostawcy Tempdata](xref:fundamentals/app-state#tempdata) pliku cookie nie jest konieczne. Jeśli śledzenie jest wyłączone, dostawca Tempdata nie działa. Aby włączyć dostawcy Tempdata, gdy śledzenie jest wyłączone, Oznacz plik cookie TempData za istotne w `ConfigureServices`:

[!code-csharp[Main](gdpr/sample/RP/Startup.cs?name=snippet1)]

[Stan sesji](xref:fundamentals/app-state) pliki cookie nie są istotne. Stan sesji nie działa po wyłączeniu śledzenia.

<a name="pd"></a>

## <a name="personal-data"></a>Dane osobowe

Utworzone za pomocą indywidualne konta użytkowników aplikacji platformy ASP.NET Core obejmują kod, aby pobrać i usunięcia danych osobowych.

Wybierz nazwę użytkownika, a następnie wybierz **danych osobowych**:

![Zarządzanie strony danych osobowych](gdpr/_static/pd.png)

Uwagi:

* Aby wygenerować `Account/Manage` kodu, zobacz [tożsamości szkieletu](xref:security/authentication/scaffold-identity).
* Usuwanie i Pobierz wpływ domyślnych danych tożsamości. Utwórz użytkownika niestandardowego danych aplikacji musi zostać rozszerzony do usuwania/pobierania danych użytkownika. Problem GitHub [sposobu dodawania/usuwania danych użytkownika niestandardowego do tożsamości](https://github.com/aspnet/Docs/issues/6226) śledzi proponowanych artykuł na temat tworzenia niestandardowych/usuwanie/pobieranie danych użytkownika. Jeśli chcesz znaleźć w tym temacie priorytety, pozostaw Kciuki zapasowej reakcji w kwestii.
* Zapisano tokeny dla danego użytkownika, które są przechowywane w tabeli bazy danych tożsamości `AspNetUserTokens` są usuwane po usunięciu użytkownika przy użyciu kaskadowych zachowanie Usuń ze względu na [klucz obcy](https://github.com/aspnet/Identity/blob/b4fc72c944e0589a7e1f076794d7e5d8dcf163bf/src/EF/IdentityUserContext.cs#L152).

## <a name="encryption-at-rest"></a>Szyfrowanie magazynowanych

Niektóre bazy danych i magazynu mechanizmów umożliwiają szyfrowanie w stanie spoczynku. Szyfrowanie magazynowanych:

* Automatycznie szyfruje dane przechowywane.
* Szyfruje bez konfiguracji, programowania lub inne zadania dla oprogramowania, które uzyskuje dostęp do danych.
* Jest to opcja najprostszym i najbezpieczniejsze.
* Umożliwia zarządzanie kluczy i szyfrowania bazy danych.

Na przykład:

* Microsoft SQL i Azure SQL udostępniają [przezroczystego szyfrowania danych](https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/transparent-data-encryption?view=sql-server-2017) (funkcji TDE).
* [Usługi SQL Azure szyfruje bazy danych domyślnie](https://azure.microsoft.com/en-us/updates/newly-created-azure-sql-databases-encrypted-by-default/)
* [Domyślnie są szyfrowane Azure obiektów blob, plików, tabel i magazyn kolejek](https://azure.microsoft.com/en-us/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/).

Dla baz danych, które nie zawierają wbudowane szyfrowanie magazynowane można używać szyfrowania dysku w celu zapewnienia ochrony tej samej. Na przykład:

* [Funkcja BitLocker dla systemu windows server](https://docs.microsoft.com/en-us/windows/security/information-protection/bitlocker/bitlocker-how-to-deploy-on-windows-server)
* Linux:
  * [eCryptfs](https://launchpad.net/ecryptfs)
  * [EncFS](https://github.com/vgough/encfs).

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Microsoft.com/GDPR](https://www.microsoft.com/en-us/trustcenter/Privacy/GDPR)
