---
title: Obsługa ochrony rozporządzenia (GDPR) ogólne dane w ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak uzyskać dostęp do punktów rozszerzenia GDPR w aplikacji sieci web platformy ASP.NET Core.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/29/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/gdpr
ms.openlocfilehash: eb9173bfe554b8b00ef8deb255e8347a534e7ba3
ms.sourcegitcommit: 9a35906446af7ffd4ccfc18daec38874b5abbef7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/18/2018
ms.locfileid: "35725799"
---
# <a name="eu-general-data-protection-regulation-gdpr-support-in-aspnet-core"></a><span data-ttu-id="4571c-103">Obsługa interfejsów UE ogólne dane ochrony rozporządzenia (GDPR) w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4571c-103">EU General Data Protection Regulation (GDPR) support in ASP.NET Core</span></span>

<span data-ttu-id="4571c-104">przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="4571c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="4571c-105">Platformy ASP.NET Core udostępnia interfejsy API i szablony, które ułatwiają korzystanie z niektórych [interfejsów UE ogólne dane ochrony rozporządzenia (GDPR)](https://www.eugdpr.org/) wymagania:</span><span class="sxs-lookup"><span data-stu-id="4571c-105">ASP.NET Core provides APIs and templates to help meet some of the [EU General Data Protection Regulation (GDPR)](https://www.eugdpr.org/) requirements:</span></span>

* <span data-ttu-id="4571c-106">Szablony projektu to punkty rozszerzenia i zastępczych znaczników, który można zastąpić prywatności i plików cookie użyj zasad.</span><span class="sxs-lookup"><span data-stu-id="4571c-106">The project templates include extension points and stubbed markup you can replace with your privacy and cookie use policy.</span></span>
* <span data-ttu-id="4571c-107">Funkcja zgody plik cookie umożliwia podanie (i śledzenia) zgody od użytkowników do przechowywania informacji osobistych.</span><span class="sxs-lookup"><span data-stu-id="4571c-107">A cookie consent feature allows you to ask for (and track) consent from your users for storing personal information.</span></span> <span data-ttu-id="4571c-108">Jeśli użytkownik nie ma zgodę na zbieranie danych i aplikacji została skonfigurowana z [CheckConsentNeeded](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.checkconsentneeded) do `true`, nieistotne pliki cookie nie będą wysyłane do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="4571c-108">If a user has not consented to data collection and the app is set with [CheckConsentNeeded](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.checkconsentneeded) to `true`, non-essential cookies will not be sent to the browser.</span></span>
* <span data-ttu-id="4571c-109">Pliki cookie może być oznaczony jako ważne.</span><span class="sxs-lookup"><span data-stu-id="4571c-109">Cookies can be marked as essential.</span></span> <span data-ttu-id="4571c-110">Niezbędne pliki cookie są wysyłane do przeglądarki, nawet wtedy, gdy użytkownik zgodził się nie śledzenie jest wyłączone.</span><span class="sxs-lookup"><span data-stu-id="4571c-110">Essential cookies are sent to the browser even when the user has not consented and tracking is disabled.</span></span>
* <span data-ttu-id="4571c-111">[Pliki cookie sesji i TempData](#tempdata) nie działają podczas śledzenia jest wyłączona.</span><span class="sxs-lookup"><span data-stu-id="4571c-111">[TempData and Session cookies](#tempdata) are not functional when tracking is disabled.</span></span>
* <span data-ttu-id="4571c-112">[Zarządzanie tożsamościami](#pd) strona zawiera również link do pobierania i usuwania danych użytkownika.</span><span class="sxs-lookup"><span data-stu-id="4571c-112">The [Identity manage](#pd) page provides a link to download and delete user data.</span></span>

<span data-ttu-id="4571c-113">[Przykładowa aplikacja](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) umożliwia testowanie większość punktów rozszerzenia GDPR i dodane do szablonów platformy ASP.NET Core 2.1 interfejsów API.</span><span class="sxs-lookup"><span data-stu-id="4571c-113">The [sample app](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) lets you test most of the GDPR extension points and APIs added to the ASP.NET Core 2.1 templates.</span></span> <span data-ttu-id="4571c-114">Zobacz [ReadMe](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) pliku do testowania instrukcje.</span><span class="sxs-lookup"><span data-stu-id="4571c-114">See the [ReadMe](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) file for testing instructions.</span></span>

<span data-ttu-id="4571c-115">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4571c-115">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="aspnet-core-gdpr-support-in-template-generated-code"></a><span data-ttu-id="4571c-116">Obsługa GDPR platformy ASP.NET Core w szablonie wygenerowanego kodu</span><span class="sxs-lookup"><span data-stu-id="4571c-116">ASP.NET Core GDPR support in template generated code</span></span>

<span data-ttu-id="4571c-117">Stron razor i MVC projektów utworzonych za pomocą szablonów projektu obejmują obsługę GDPR:</span><span class="sxs-lookup"><span data-stu-id="4571c-117">Razor Pages and MVC projects created with the project templates include the following GDPR support:</span></span>

* <span data-ttu-id="4571c-118">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) i [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) są ustawione w `Startup`.</span><span class="sxs-lookup"><span data-stu-id="4571c-118">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) and [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) are set in `Startup`.</span></span>
* <span data-ttu-id="4571c-119">*_CookieConsentPartial.cshtml* [widoku częściowego](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="4571c-119">The *_CookieConsentPartial.cshtml* [partial view](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper).</span></span>
* <span data-ttu-id="4571c-120">*Pages/Privacy.cshtml* lub *Home/Privacy.cshtml* widok zawiera strona szczegółów zasady zachowania poufności informacji witryny.</span><span class="sxs-lookup"><span data-stu-id="4571c-120">The *Pages/Privacy.cshtml* or *Home/Privacy.cshtml* view provides a page to detail your site's privacy policy.</span></span> <span data-ttu-id="4571c-121">*_CookieConsentPartial.cshtml* plik generuje łącze do strony prywatności.</span><span class="sxs-lookup"><span data-stu-id="4571c-121">The *_CookieConsentPartial.cshtml* file generates a link to the privacy page.</span></span>
* <span data-ttu-id="4571c-122">Dla aplikacji utworzonych za pomocą indywidualne konta użytkowników, na stronie Zarządzanie zawiera łącza do pobierania i usuwania [dane osobiste użytkownika](#pd).</span><span class="sxs-lookup"><span data-stu-id="4571c-122">For applications created with individual user accounts, the manage page provides links to download and delete [personal user data](#pd).</span></span>

### <a name="cookiepolicyoptions-and-usecookiepolicy"></a><span data-ttu-id="4571c-123">CookiePolicyOptions i UseCookiePolicy</span><span class="sxs-lookup"><span data-stu-id="4571c-123">CookiePolicyOptions and UseCookiePolicy</span></span>

<span data-ttu-id="4571c-124">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) są inicjowane w `Startup` klasy `ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="4571c-124">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) are initialized in the `Startup` class `ConfigureServices` method:</span></span>

[!code-csharp[Main](gdpr/sample/Startup.cs?name=snippet1&highlight=14-20)]

<span data-ttu-id="4571c-125">[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) jest wywoływana `Startup` klasy `Configure` metody:</span><span class="sxs-lookup"><span data-stu-id="4571c-125">[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) is called in the `Startup` class `Configure` method:</span></span>

[!code-csharp[Main](gdpr/sample/Startup.cs?name=snippet1&highlight=49)]

### <a name="cookieconsentpartialcshtml-partial-view"></a><span data-ttu-id="4571c-126">Widok częściowy _CookieConsentPartial.cshtml</span><span class="sxs-lookup"><span data-stu-id="4571c-126">_CookieConsentPartial.cshtml partial view</span></span>

<span data-ttu-id="4571c-127">*_CookieConsentPartial.cshtml* widoku częściowego:</span><span class="sxs-lookup"><span data-stu-id="4571c-127">The *_CookieConsentPartial.cshtml* partial view:</span></span>

[!code-html[Main](gdpr/sample/RP/Pages/Shared/_CookieConsentPartial.cshtml)]

<span data-ttu-id="4571c-128">Tej części:</span><span class="sxs-lookup"><span data-stu-id="4571c-128">This partial:</span></span>

* <span data-ttu-id="4571c-129">Pobiera stan śledzenia dla użytkownika.</span><span class="sxs-lookup"><span data-stu-id="4571c-129">Gets the state of tracking for the user.</span></span> <span data-ttu-id="4571c-130">Aplikację skonfigurowano wymaganie zgody użytkownik musi wyrazić zgodę przed plików cookie mogą być śledzone.</span><span class="sxs-lookup"><span data-stu-id="4571c-130">If the application is configured to require consent the user must consent before cookies can be tracked.</span></span> <span data-ttu-id="4571c-131">Jeśli wymagana jest zgoda, chrome zgody plik cookie jest ustalone na pasku nawigacyjnym utworzone w *Pages/Shared/_Layout.cshtml* pliku.</span><span class="sxs-lookup"><span data-stu-id="4571c-131">If consent is required, the cookie consent chrome is fixed on top of the navigation bar created in the *Pages/Shared/_Layout.cshtml* file.</span></span>
* <span data-ttu-id="4571c-132">Udostępnia HTML `<p>` element, aby podsumować prywatności i plików cookie przy użyciu zasad.</span><span class="sxs-lookup"><span data-stu-id="4571c-132">Provides an HTML `<p>` element to summarize your privacy and cookie use policy.</span></span>
* <span data-ttu-id="4571c-133">Link do *Pages/Privacy.cshtml* gdzie można szczegółowe zasady zachowania poufności informacji witryny.</span><span class="sxs-lookup"><span data-stu-id="4571c-133">Provides a link to *Pages/Privacy.cshtml* where you can detail your site's privacy policy.</span></span>

## <a name="essential-cookies"></a><span data-ttu-id="4571c-134">Niezbędne pliki cookie</span><span class="sxs-lookup"><span data-stu-id="4571c-134">Essential cookies</span></span>

<span data-ttu-id="4571c-135">Jeśli nie udzielono zgody, tylko pliki cookie oznaczone jako ważne są wysyłane do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="4571c-135">If consent has not been given, only cookies marked essential are sent to the browser.</span></span> <span data-ttu-id="4571c-136">Poniższy kod sprawia, że plik cookie jest istotne:</span><span class="sxs-lookup"><span data-stu-id="4571c-136">The following code makes a cookie essential:</span></span>

[!code-csharp[Main](gdpr/sample/RP/Pages/Cookie.cshtml.cs?name=snippet1&highlight=5)]

<a name="tempdata"></a>

## <a name="tempdata-provider-and-session-state-cookies-are-not-essential"></a><span data-ttu-id="4571c-137">Tempdata dostawcy i sesji stanu pliki cookie nie są istotne</span><span class="sxs-lookup"><span data-stu-id="4571c-137">Tempdata provider and session state cookies are not essential</span></span>

<span data-ttu-id="4571c-138">[Dostawcy Tempdata](xref:fundamentals/app-state#tempdata) pliku cookie nie jest konieczne.</span><span class="sxs-lookup"><span data-stu-id="4571c-138">The [Tempdata provider](xref:fundamentals/app-state#tempdata) cookie is not essential.</span></span> <span data-ttu-id="4571c-139">Jeśli śledzenie jest wyłączone, dostawca Tempdata nie działa.</span><span class="sxs-lookup"><span data-stu-id="4571c-139">If tracking is disabled, the Tempdata provider is not functional.</span></span> <span data-ttu-id="4571c-140">Aby włączyć dostawcy Tempdata, gdy śledzenie jest wyłączone, Oznacz plik cookie TempData za istotne w `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="4571c-140">To enable the Tempdata provider when tracking is disabled, mark the TempData cookie as essential in `ConfigureServices`:</span></span>

[!code-csharp[Main](gdpr/sample/RP/Startup.cs?name=snippet1)]

<span data-ttu-id="4571c-141">[Stan sesji](xref:fundamentals/app-state) pliki cookie nie są istotne.</span><span class="sxs-lookup"><span data-stu-id="4571c-141">[Session state](xref:fundamentals/app-state) cookies are not essential.</span></span> <span data-ttu-id="4571c-142">Stan sesji nie działa po wyłączeniu śledzenia.</span><span class="sxs-lookup"><span data-stu-id="4571c-142">Session state is not functional when tracking is disabled.</span></span>

<a name="pd"></a>

## <a name="personal-data"></a><span data-ttu-id="4571c-143">Dane osobowe</span><span class="sxs-lookup"><span data-stu-id="4571c-143">Personal data</span></span>

<span data-ttu-id="4571c-144">Utworzone za pomocą indywidualne konta użytkowników aplikacji platformy ASP.NET Core obejmują kod, aby pobrać i usunięcia danych osobowych.</span><span class="sxs-lookup"><span data-stu-id="4571c-144">ASP.NET Core applications created with individual user accounts include code to download and delete personal data.</span></span>

<span data-ttu-id="4571c-145">Wybierz nazwę użytkownika, a następnie wybierz **danych osobowych**:</span><span class="sxs-lookup"><span data-stu-id="4571c-145">Select the user name and then select **Personal data**:</span></span>

![Zarządzanie strony danych osobowych](gdpr/_static/pd.png)

<span data-ttu-id="4571c-147">Uwagi:</span><span class="sxs-lookup"><span data-stu-id="4571c-147">Notes:</span></span>

* <span data-ttu-id="4571c-148">Aby wygenerować `Account/Manage` kodu, zobacz [tożsamości szkieletu](xref:security/authentication/scaffold-identity).</span><span class="sxs-lookup"><span data-stu-id="4571c-148">To generate the `Account/Manage` code, see [Scaffold Identity](xref:security/authentication/scaffold-identity).</span></span>
* <span data-ttu-id="4571c-149">Usuwanie i Pobierz wpływ domyślnych danych tożsamości.</span><span class="sxs-lookup"><span data-stu-id="4571c-149">Delete and download only impact the default identity data.</span></span> <span data-ttu-id="4571c-150">Utwórz użytkownika niestandardowego danych aplikacji musi zostać rozszerzony do usuwania/pobierania danych użytkownika.</span><span class="sxs-lookup"><span data-stu-id="4571c-150">Apps the create custom user data must be extended to delete/download the custom user data.</span></span> <span data-ttu-id="4571c-151">Problem GitHub [sposobu dodawania/usuwania danych użytkownika niestandardowego do tożsamości](https://github.com/aspnet/Docs/issues/6226) śledzi proponowanych artykuł na temat tworzenia niestandardowych/usuwanie/pobieranie danych użytkownika.</span><span class="sxs-lookup"><span data-stu-id="4571c-151">GitHub issue [How to add/delete custom user data to Identity](https://github.com/aspnet/Docs/issues/6226) tracks a proposed article on creating custom/deleting/downloading custom user data.</span></span> <span data-ttu-id="4571c-152">Jeśli chcesz znaleźć w tym temacie priorytety, pozostaw Kciuki zapasowej reakcji w kwestii.</span><span class="sxs-lookup"><span data-stu-id="4571c-152">If you'd like to see that topic prioritized, leave a thumbs up reaction in the issue.</span></span>
* <span data-ttu-id="4571c-153">Zapisano tokeny dla danego użytkownika, które są przechowywane w tabeli bazy danych tożsamości `AspNetUserTokens` są usuwane po usunięciu użytkownika przy użyciu kaskadowych zachowanie Usuń ze względu na [klucz obcy](https://github.com/aspnet/Identity/blob/release/2.1/src/EF/IdentityUserContext.cs#L152).</span><span class="sxs-lookup"><span data-stu-id="4571c-153">Saved tokens for the user that are stored in the Identity database table `AspNetUserTokens` are deleted when the user is deleted via the cascading delete behavior due to the [foreign key](https://github.com/aspnet/Identity/blob/release/2.1/src/EF/IdentityUserContext.cs#L152).</span></span>

## <a name="encryption-at-rest"></a><span data-ttu-id="4571c-154">Szyfrowanie magazynowanych</span><span class="sxs-lookup"><span data-stu-id="4571c-154">Encryption at rest</span></span>

<span data-ttu-id="4571c-155">Niektóre bazy danych i magazynu mechanizmów umożliwiają szyfrowanie w stanie spoczynku.</span><span class="sxs-lookup"><span data-stu-id="4571c-155">Some databases and storage mechanisms allow for encryption at rest.</span></span> <span data-ttu-id="4571c-156">Szyfrowanie magazynowanych:</span><span class="sxs-lookup"><span data-stu-id="4571c-156">Encryption at rest:</span></span>

* <span data-ttu-id="4571c-157">Automatycznie szyfruje dane przechowywane.</span><span class="sxs-lookup"><span data-stu-id="4571c-157">Encrypts stored data automatically.</span></span>
* <span data-ttu-id="4571c-158">Szyfruje bez konfiguracji, programowania lub inne zadania dla oprogramowania, które uzyskuje dostęp do danych.</span><span class="sxs-lookup"><span data-stu-id="4571c-158">Encrypts without configuration, programming, or other work for the software that accesses the data.</span></span>
* <span data-ttu-id="4571c-159">Jest to opcja najprostszym i najbezpieczniejsze.</span><span class="sxs-lookup"><span data-stu-id="4571c-159">Is the easiest and safest option.</span></span>
* <span data-ttu-id="4571c-160">Umożliwia zarządzanie kluczy i szyfrowania bazy danych.</span><span class="sxs-lookup"><span data-stu-id="4571c-160">Lets the database manage keys and encryption.</span></span>

<span data-ttu-id="4571c-161">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="4571c-161">For example:</span></span>

* <span data-ttu-id="4571c-162">Microsoft SQL i Azure SQL udostępniają [przezroczystego szyfrowania danych](/sql/relational-databases/security/encryption/transparent-data-encryption) (funkcji TDE).</span><span class="sxs-lookup"><span data-stu-id="4571c-162">Microsoft SQL and Azure SQL provide [Transparent Data Encryption](/sql/relational-databases/security/encryption/transparent-data-encryption) (TDE).</span></span>
* [<span data-ttu-id="4571c-163">Usługi SQL Azure szyfruje bazy danych domyślnie</span><span class="sxs-lookup"><span data-stu-id="4571c-163">SQL Azure encrypts the database by default</span></span>](https://azure.microsoft.com/updates/newly-created-azure-sql-databases-encrypted-by-default/)
* <span data-ttu-id="4571c-164">[Domyślnie są szyfrowane Azure obiektów blob, plików, tabel i magazyn kolejek](https://azure.microsoft.com/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/).</span><span class="sxs-lookup"><span data-stu-id="4571c-164">[Azure Blobs, Files, Table, and Queue Storage are encrypted by default](https://azure.microsoft.com/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/).</span></span>

<span data-ttu-id="4571c-165">Dla baz danych, które nie zawierają wbudowane szyfrowanie magazynowane można używać szyfrowania dysku w celu zapewnienia ochrony tej samej.</span><span class="sxs-lookup"><span data-stu-id="4571c-165">For databases that don't provide built-in encryption at rest you may be able to use disk encryption to provide the same protection.</span></span> <span data-ttu-id="4571c-166">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="4571c-166">For example:</span></span>

* [<span data-ttu-id="4571c-167">Funkcja BitLocker dla systemu Windows Server</span><span class="sxs-lookup"><span data-stu-id="4571c-167">BitLocker for Windows Server</span></span>](/windows/security/information-protection/bitlocker/bitlocker-how-to-deploy-on-windows-server)
* <span data-ttu-id="4571c-168">Linux:</span><span class="sxs-lookup"><span data-stu-id="4571c-168">Linux:</span></span>
  * [<span data-ttu-id="4571c-169">eCryptfs</span><span class="sxs-lookup"><span data-stu-id="4571c-169">eCryptfs</span></span>](https://launchpad.net/ecryptfs)
  * <span data-ttu-id="4571c-170">[EncFS](https://github.com/vgough/encfs).</span><span class="sxs-lookup"><span data-stu-id="4571c-170">[EncFS](https://github.com/vgough/encfs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4571c-171">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="4571c-171">Additional Resources</span></span>

* [<span data-ttu-id="4571c-172">Microsoft.com/GDPR</span><span class="sxs-lookup"><span data-stu-id="4571c-172">Microsoft.com/GDPR</span></span>](https://www.microsoft.com/en-us/trustcenter/Privacy/GDPR)
