---
title: Dane ogólne rozporządzenie o ochronie danych (GDPR) obsługi w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak uzyskać dostęp do punktów rozszerzenia RODO w aplikacji sieci web platformy ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 07/11/2019
uid: security/gdpr
ms.openlocfilehash: 01d2f8943c0995c1400122b89c4ca7c459a85279
ms.sourcegitcommit: bee530454ae2b3c25dc7ffebf93536f479a14460
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67724567"
---
# <a name="eu-general-data-protection-regulation-gdpr-support-in-aspnet-core"></a><span data-ttu-id="c4941-103">Obsługa Unii Europejskiej ogólnego danych (GDPR Protection Regulation) w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c4941-103">EU General Data Protection Regulation (GDPR) support in ASP.NET Core</span></span>

<span data-ttu-id="c4941-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c4941-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c4941-105">Platformy ASP.NET Core udostępnia interfejsów API i szablony, które ułatwiają korzystanie z niektórych [Unii Europejskiej ogólnego danych (GDPR Protection Regulation)](https://www.eugdpr.org/) wymagania:</span><span class="sxs-lookup"><span data-stu-id="c4941-105">ASP.NET Core provides APIs and templates to help meet some of the [EU General Data Protection Regulation (GDPR)](https://www.eugdpr.org/) requirements:</span></span>

::: moniker range=">= aspnetcore-3.0"

* <span data-ttu-id="c4941-106">Szablony projektów obejmują punktów rozszerzeń i przekształcone na wycinki kod znaczników, który można zastąpić prywatności i plików cookie zasady.</span><span class="sxs-lookup"><span data-stu-id="c4941-106">The project templates include extension points and stubbed markup that you can replace with your privacy and cookie use policy.</span></span>
* <span data-ttu-id="c4941-107">*Pages/Privacy.cshtml* strony lub *Views/Home/Privacy.cshtml* widok zawiera stronę, aby szczegółowo opisują zasady zachowania poufności informacji witryny.</span><span class="sxs-lookup"><span data-stu-id="c4941-107">The *Pages/Privacy.cshtml* page or *Views/Home/Privacy.cshtml* view provides a page to detail your site's privacy policy.</span></span>

<span data-ttu-id="c4941-108">Aby włączyć funkcję zgody plik cookie domyślne Krewny, który znalezione w szablony ASP.NET Core 2.2 w aplikacji ASP.NET Core 3.0 wygenerowanego szablonu:</span><span class="sxs-lookup"><span data-stu-id="c4941-108">To enable the default cookie consent feature like that found in the ASP.NET Core 2.2 templates in an ASP.NET Core 3.0 template generated app:</span></span>

* <span data-ttu-id="c4941-109">Dodaj [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) zbyt `Startup.ConfigureServices` i [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) do `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="c4941-109">Add [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) too `Startup.ConfigureServices` and [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) to `Startup.Configure`:</span></span>

  [!code-csharp[Main](gdpr/sample/RP3.0/Startup.cs?name=snippet1&highlight=12-19,38)]

* <span data-ttu-id="c4941-110">Dodaj częściowego zgody pliku cookie do *_Layout.cshtml* pliku:</span><span class="sxs-lookup"><span data-stu-id="c4941-110">Add the cookie consent partial to the *_Layout.cshtml* file:</span></span>

  [!code-cshtml[Main](gdpr/sample/RP3.0/Pages/Shared/_Layout.cshtml?name=snippet&highlight=4)]

* <span data-ttu-id="c4941-111">Dodaj  *\_CookieConsentPartial.cshtml* pliku do projektu:</span><span class="sxs-lookup"><span data-stu-id="c4941-111">Add the *\_CookieConsentPartial.cshtml* file to the project:</span></span>

  [!code-cshtml[Main](gdpr/sample/RP3.0/Pages/Shared/_CookieConsentPartial.cshtml)]

* <span data-ttu-id="c4941-112">Wybierz wersję platformy ASP.NET Core 2.2 w tym artykule na temat funkcji wyrażania zgody pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="c4941-112">Select the ASP.NET Core 2.2 version of this article to read about the cookie consent feature.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

* <span data-ttu-id="c4941-113">Szablony projektów obejmują punktów rozszerzeń i przekształcone na wycinki kod znaczników, który można zastąpić prywatności i plików cookie zasady.</span><span class="sxs-lookup"><span data-stu-id="c4941-113">The project templates include extension points and stubbed markup that you can replace with your privacy and cookie use policy.</span></span>
* <span data-ttu-id="c4941-114">Funkcja zgody plik cookie umożliwia poprosić o (i śledzenia) zgody od użytkowników do przechowywania informacji osobistych.</span><span class="sxs-lookup"><span data-stu-id="c4941-114">A cookie consent feature allows you to ask for (and track) consent from your users for storing personal information.</span></span> <span data-ttu-id="c4941-115">Jeśli użytkownik nie wyraził zgodę na zbieranie danych, a aplikacja ma [CheckConsentNeeded](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.checkconsentneeded) równa `true`, inne niż niezbędne pliki cookie nie są wysyłane do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="c4941-115">If a user hasn't consented to data collection and the app has [CheckConsentNeeded](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.checkconsentneeded) set to `true`, non-essential cookies aren't sent to the browser.</span></span>
* <span data-ttu-id="c4941-116">Pliki cookie mogą być oznaczane jako podstawowe.</span><span class="sxs-lookup"><span data-stu-id="c4941-116">Cookies can be marked as essential.</span></span> <span data-ttu-id="c4941-117">Podstawowe pliki cookie są wysyłane do przeglądarki, nawet wtedy, gdy użytkownik nie wyraził zgodę i śledzenie jest wyłączone.</span><span class="sxs-lookup"><span data-stu-id="c4941-117">Essential cookies are sent to the browser even when the user hasn't consented and tracking is disabled.</span></span>
* <span data-ttu-id="c4941-118">[Pliki cookie sesji i TempData](#tempdata) nie są funkcjonalności, gdy śledzenie jest wyłączone.</span><span class="sxs-lookup"><span data-stu-id="c4941-118">[TempData and Session cookies](#tempdata) aren't functional when tracking is disabled.</span></span>
* <span data-ttu-id="c4941-119">[Zarządzanie tożsamościami](#pd) strona zawiera również link do pobierające i usuwające dane użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c4941-119">The [Identity manage](#pd) page provides a link to download and delete user data.</span></span>

<span data-ttu-id="c4941-120">[Przykładową aplikację](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) umożliwia testowanie większość punktów rozszerzenia RODO i interfejsów API dodano szablony platformy ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="c4941-120">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) allows you test most of the GDPR extension points and APIs added to the ASP.NET Core 2.1 templates.</span></span> <span data-ttu-id="c4941-121">Zobacz [ReadMe](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) testowania instrukcje w pliku.</span><span class="sxs-lookup"><span data-stu-id="c4941-121">See the [ReadMe](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) file for testing instructions.</span></span>

<span data-ttu-id="c4941-122">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c4941-122">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="aspnet-core-gdpr-support-in-template-generated-code"></a><span data-ttu-id="c4941-123">ASP.NET Core RODO obsługi w kodzie wygenerowany szablon</span><span class="sxs-lookup"><span data-stu-id="c4941-123">ASP.NET Core GDPR support in template-generated code</span></span>

<span data-ttu-id="c4941-124">Strony razor i MVC projekty utworzone za pomocą szablonów projektu obejmują obsługę RODO:</span><span class="sxs-lookup"><span data-stu-id="c4941-124">Razor Pages and MVC projects created with the project templates include the following GDPR support:</span></span>

* <span data-ttu-id="c4941-125">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) i [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) są ustawiane w `Startup` klasy.</span><span class="sxs-lookup"><span data-stu-id="c4941-125">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) and [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) are set in the `Startup` class.</span></span>
* <span data-ttu-id="c4941-126">*\_CookieConsentPartial.cshtml* [widoku częściowego](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="c4941-126">The *\_CookieConsentPartial.cshtml* [partial view](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper).</span></span> <span data-ttu-id="c4941-127">**Akceptuj** przycisk znajduje się w tym pliku.</span><span class="sxs-lookup"><span data-stu-id="c4941-127">An **Accept** button is included in this file.</span></span> <span data-ttu-id="c4941-128">Kiedy użytkownik kliknie **Akceptuj** przycisk, wyrażanie zgody do przechowywania plików cookie znajduje się.</span><span class="sxs-lookup"><span data-stu-id="c4941-128">When the user clicks the **Accept** button, consent to store cookies is provided.</span></span>
* <span data-ttu-id="c4941-129">*Pages/Privacy.cshtml* strony lub *Views/Home/Privacy.cshtml* widok zawiera stronę, aby szczegółowo opisują zasady zachowania poufności informacji witryny.</span><span class="sxs-lookup"><span data-stu-id="c4941-129">The *Pages/Privacy.cshtml* page or *Views/Home/Privacy.cshtml* view provides a page to detail your site's privacy policy.</span></span> <span data-ttu-id="c4941-130">*\_CookieConsentPartial.cshtml* plik generuje łącze do strony ochrony prywatności.</span><span class="sxs-lookup"><span data-stu-id="c4941-130">The *\_CookieConsentPartial.cshtml* file generates a link to the Privacy page.</span></span>
* <span data-ttu-id="c4941-131">W przypadku aplikacji utworzonych za pomocą indywidualnych kont użytkowników, na stronie Zarządzanie zawiera łącza do pobierające i usuwające [dane osobiste użytkownika](#pd).</span><span class="sxs-lookup"><span data-stu-id="c4941-131">For apps created with individual user accounts, the Manage page provides links to download and delete [personal user data](#pd).</span></span>

### <a name="cookiepolicyoptions-and-usecookiepolicy"></a><span data-ttu-id="c4941-132">CookiePolicyOptions i UseCookiePolicy</span><span class="sxs-lookup"><span data-stu-id="c4941-132">CookiePolicyOptions and UseCookiePolicy</span></span>

<span data-ttu-id="c4941-133">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) są inicjowane w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="c4941-133">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) are initialized in `Startup.ConfigureServices`:</span></span>

[!code-csharp[Main](gdpr/sample/Startup.cs?name=snippet1&highlight=14-20)]

<span data-ttu-id="c4941-134">[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) jest wywoływana w `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="c4941-134">[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) is called in `Startup.Configure`:</span></span>

[!code-csharp[](gdpr/sample/Startup.cs?name=snippet1&highlight=51)]

### <a name="cookieconsentpartialcshtml-partial-view"></a><span data-ttu-id="c4941-135">\_CookieConsentPartial.cshtml partial view</span><span class="sxs-lookup"><span data-stu-id="c4941-135">\_CookieConsentPartial.cshtml partial view</span></span>

<span data-ttu-id="c4941-136">*\_CookieConsentPartial.cshtml* widoku częściowego:</span><span class="sxs-lookup"><span data-stu-id="c4941-136">The *\_CookieConsentPartial.cshtml* partial view:</span></span>

[!code-html[](gdpr/sample/RP2.2/Pages/Shared/_CookieConsentPartial.cshtml)]

<span data-ttu-id="c4941-137">Tej części:</span><span class="sxs-lookup"><span data-stu-id="c4941-137">This partial:</span></span>

* <span data-ttu-id="c4941-138">Pobiera stan śledzenia dla użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c4941-138">Obtains the state of tracking for the user.</span></span> <span data-ttu-id="c4941-139">Jeśli aplikacja jest skonfigurowana do wymagają zgody, użytkownik musi wyrazić zgodę przed pliki cookie mogą być śledzone.</span><span class="sxs-lookup"><span data-stu-id="c4941-139">If the app is configured to require consent, the user must consent before cookies can be tracked.</span></span> <span data-ttu-id="c4941-140">Jeśli wymagana jest zgoda, panel zgody plik cookie jest ustalony na górnej części paska nawigacyjnego, utworzone przez  *\_Layout.cshtml* pliku.</span><span class="sxs-lookup"><span data-stu-id="c4941-140">If consent is required, the cookie consent panel is fixed at top of the navigation bar created by the *\_Layout.cshtml* file.</span></span>
* <span data-ttu-id="c4941-141">Udostępnia kodu HTML `<p>` element, aby podsumować, prywatności i plików cookie przy użyciu zasad.</span><span class="sxs-lookup"><span data-stu-id="c4941-141">Provides an HTML `<p>` element to summarize your privacy and cookie use policy.</span></span>
* <span data-ttu-id="c4941-142">Zawiera również link do strony prywatności lub widok, gdzie możesz szczegółowo opisują zasady zachowania poufności informacji witryny.</span><span class="sxs-lookup"><span data-stu-id="c4941-142">Provides a link to Privacy page or view where you can detail your site's privacy policy.</span></span>

## <a name="essential-cookies"></a><span data-ttu-id="c4941-143">Podstawowe pliki cookie</span><span class="sxs-lookup"><span data-stu-id="c4941-143">Essential cookies</span></span>

<span data-ttu-id="c4941-144">Jeśli zgody do przechowywania plików cookie udzielona, tylko pliki cookie oznaczone niezbędne są wysyłane do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="c4941-144">If consent to store cookies hasn't been provided, only cookies marked essential are sent to the browser.</span></span> <span data-ttu-id="c4941-145">Poniższy kod sprawia, że plik cookie jest podstawowe:</span><span class="sxs-lookup"><span data-stu-id="c4941-145">The following code makes a cookie essential:</span></span>

[!code-csharp[Main](gdpr/sample/RP2.2/Pages/Cookie.cshtml.cs?name=snippet1&highlight=5)]

<a name="tempdata"></a>

### <a name="tempdata-provider-and-session-state-cookies-arent-essential"></a><span data-ttu-id="c4941-146">TempData dostawcy sesji stanu plików cookie i nie są istotne</span><span class="sxs-lookup"><span data-stu-id="c4941-146">TempData provider and session state cookies aren't essential</span></span>

<span data-ttu-id="c4941-147">[Dostawcy TempData](xref:fundamentals/app-state#tempdata) pliku cookie nie jest niezbędne.</span><span class="sxs-lookup"><span data-stu-id="c4941-147">The [TempData provider](xref:fundamentals/app-state#tempdata) cookie isn't essential.</span></span> <span data-ttu-id="c4941-148">Jeśli śledzenie jest wyłączone, dostawca TempData nie jest funkcjonalności.</span><span class="sxs-lookup"><span data-stu-id="c4941-148">If tracking is disabled, the TempData provider isn't functional.</span></span> <span data-ttu-id="c4941-149">Aby umożliwić dostawcy TempData, gdy śledzenie jest wyłączone, Oznacz plik cookie TempData za istotne w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="c4941-149">To enable the TempData provider when tracking is disabled, mark the TempData cookie as essential in `Startup.ConfigureServices`:</span></span>

[!code-csharp[Main](gdpr/sample/RP2.2/Startup.cs?name=snippet1)]

<span data-ttu-id="c4941-150">[Stan sesji](xref:fundamentals/app-state) pliki cookie nie są istotne.</span><span class="sxs-lookup"><span data-stu-id="c4941-150">[Session state](xref:fundamentals/app-state) cookies are not essential.</span></span> <span data-ttu-id="c4941-151">Stan sesji nie jest funkcjonalnych, jeśli śledzenie jest wyłączone.</span><span class="sxs-lookup"><span data-stu-id="c4941-151">Session state isn't functional when tracking is disabled.</span></span> <span data-ttu-id="c4941-152">Poniższy kod sprawia, że pliki cookie z sesji podstawowe:</span><span class="sxs-lookup"><span data-stu-id="c4941-152">The following code makes session cookies essential:</span></span>

[!code-csharp[](gdpr/sample/RP2.2/Startup.cs?name=snippet2)]

<a name="pd"></a>

## <a name="personal-data"></a><span data-ttu-id="c4941-153">Dane osobowe</span><span class="sxs-lookup"><span data-stu-id="c4941-153">Personal data</span></span>

<span data-ttu-id="c4941-154">Aplikacje platformy ASP.NET Core utworzonych za pomocą indywidualnych kont użytkowników, to kod, aby pobrać i usunięcie danych osobowych.</span><span class="sxs-lookup"><span data-stu-id="c4941-154">ASP.NET Core apps created with individual user accounts include code to download and delete personal data.</span></span>

<span data-ttu-id="c4941-155">Wybierz nazwę użytkownika, a następnie wybierz pozycję **danych osobowych**:</span><span class="sxs-lookup"><span data-stu-id="c4941-155">Select the user name and then select **Personal data**:</span></span>

![Zarządzanie stroną danych osobowych](gdpr/_static/pd.png)

<span data-ttu-id="c4941-157">Uwagi:</span><span class="sxs-lookup"><span data-stu-id="c4941-157">Notes:</span></span>

* <span data-ttu-id="c4941-158">Aby wygenerować `Account/Manage` kod, zobacz [tożsamości szkieletu](xref:security/authentication/scaffold-identity).</span><span class="sxs-lookup"><span data-stu-id="c4941-158">To generate the `Account/Manage` code, see [Scaffold Identity](xref:security/authentication/scaffold-identity).</span></span>
* <span data-ttu-id="c4941-159">**Usuń** i **Pobierz** łącza działać tylko w przypadku domyślnych danych tożsamości.</span><span class="sxs-lookup"><span data-stu-id="c4941-159">The **Delete** and **Download** links only act on the default identity data.</span></span> <span data-ttu-id="c4941-160">Aplikacje tworzone danych niestandardowych użytkownika musi zostać rozszerzony do usuwania/pobierania danych niestandardowych użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c4941-160">Apps that create custom user data must be extended to delete/download the custom user data.</span></span> <span data-ttu-id="c4941-161">Aby uzyskać więcej informacji, zobacz [Dodawanie, pobieranie i usuwanie danych niestandardowych użytkownika tożsamości](xref:security/authentication/add-user-data).</span><span class="sxs-lookup"><span data-stu-id="c4941-161">For more information, see [Add, download, and delete custom user data to Identity](xref:security/authentication/add-user-data).</span></span>
* <span data-ttu-id="c4941-162">Zapisano tokeny dla danego użytkownika, które są przechowywane w tabeli bazy danych tożsamości `AspNetUserTokens` są usuwane po usunięciu użytkownika za pomocą kaskadowych zachowanie dotyczące usuwania ze względu na [klucz obcy](https://github.com/aspnet/Identity/blob/release/2.1/src/EF/IdentityUserContext.cs#L152).</span><span class="sxs-lookup"><span data-stu-id="c4941-162">Saved tokens for the user that are stored in the Identity database table `AspNetUserTokens` are deleted when the user is deleted via the cascading delete behavior due to the [foreign key](https://github.com/aspnet/Identity/blob/release/2.1/src/EF/IdentityUserContext.cs#L152).</span></span>
* <span data-ttu-id="c4941-163">[Uwierzytelnianie zewnętrznego dostawcy](xref:security/authentication/social/index), takie jak Facebook i Google, nie jest dostępna przed zaakceptowaniu zasad pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="c4941-163">[External provider authentication](xref:security/authentication/social/index), such as Facebook and Google, isn't available before the cookie policy is accepted.</span></span>

::: moniker-end

## <a name="encryption-at-rest"></a><span data-ttu-id="c4941-164">Szyfrowanie w spoczynku</span><span class="sxs-lookup"><span data-stu-id="c4941-164">Encryption at rest</span></span>

<span data-ttu-id="c4941-165">Niektóre bazy danych i mechanizmy magazynu umożliwia szyfrowanie danych magazynowanych.</span><span class="sxs-lookup"><span data-stu-id="c4941-165">Some databases and storage mechanisms allow for encryption at rest.</span></span> <span data-ttu-id="c4941-166">Szyfrowanie danych magazynowanych:</span><span class="sxs-lookup"><span data-stu-id="c4941-166">Encryption at rest:</span></span>

* <span data-ttu-id="c4941-167">Automatycznie szyfruje dane przechowywane.</span><span class="sxs-lookup"><span data-stu-id="c4941-167">Encrypts stored data automatically.</span></span>
* <span data-ttu-id="c4941-168">Szyfruje bez konfiguracji, programowania lub innych zadań dotyczących oprogramowania, który uzyskuje dostęp do danych.</span><span class="sxs-lookup"><span data-stu-id="c4941-168">Encrypts without configuration, programming, or other work for the software that accesses the data.</span></span>
* <span data-ttu-id="c4941-169">Jest to najprostszy i najbezpieczniejszy opcja.</span><span class="sxs-lookup"><span data-stu-id="c4941-169">Is the easiest and safest option.</span></span>
* <span data-ttu-id="c4941-170">Umożliwia zarządzanie kluczami i szyfrowania w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="c4941-170">Allows the database to manage keys and encryption.</span></span>

<span data-ttu-id="c4941-171">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="c4941-171">For example:</span></span>

* <span data-ttu-id="c4941-172">Program Microsoft SQL i Azure SQL udostępniają [funkcji Transparent Data Encryption](/sql/relational-databases/security/encryption/transparent-data-encryption) (TDE).</span><span class="sxs-lookup"><span data-stu-id="c4941-172">Microsoft SQL and Azure SQL provide [Transparent Data Encryption](/sql/relational-databases/security/encryption/transparent-data-encryption) (TDE).</span></span>
* [<span data-ttu-id="c4941-173">Bazy danych w SQL Azure są szyfrowane domyślnie</span><span class="sxs-lookup"><span data-stu-id="c4941-173">SQL Azure encrypts the database by default</span></span>](https://azure.microsoft.com/updates/newly-created-azure-sql-databases-encrypted-by-default/)
* <span data-ttu-id="c4941-174">[Azure obiektów blob, plików, tabela i Queue Storage są domyślnie szyfrowane](https://azure.microsoft.com/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/).</span><span class="sxs-lookup"><span data-stu-id="c4941-174">[Azure Blobs, Files, Table, and Queue Storage are encrypted by default](https://azure.microsoft.com/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/).</span></span>

<span data-ttu-id="c4941-175">W przypadku baz danych, które nie udostępniają wbudowane szyfrowanie danych magazynowanych można udostępnić taką samą ochronę za pomocą szyfrowania dysków.</span><span class="sxs-lookup"><span data-stu-id="c4941-175">For databases that don't provide built-in encryption at rest, you may be able to use disk encryption to provide the same protection.</span></span> <span data-ttu-id="c4941-176">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="c4941-176">For example:</span></span>

* [<span data-ttu-id="c4941-177">Funkcja BitLocker dla systemu Windows Server</span><span class="sxs-lookup"><span data-stu-id="c4941-177">BitLocker for Windows Server</span></span>](/windows/security/information-protection/bitlocker/bitlocker-how-to-deploy-on-windows-server)
* <span data-ttu-id="c4941-178">W systemie Linux:</span><span class="sxs-lookup"><span data-stu-id="c4941-178">Linux:</span></span>
  * [<span data-ttu-id="c4941-179">eCryptfs</span><span class="sxs-lookup"><span data-stu-id="c4941-179">eCryptfs</span></span>](https://launchpad.net/ecryptfs)
  * <span data-ttu-id="c4941-180">[EncFS](https://github.com/vgough/encfs).</span><span class="sxs-lookup"><span data-stu-id="c4941-180">[EncFS](https://github.com/vgough/encfs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c4941-181">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="c4941-181">Additional resources</span></span>

* [<span data-ttu-id="c4941-182">Microsoft.com/GDPR</span><span class="sxs-lookup"><span data-stu-id="c4941-182">Microsoft.com/GDPR</span></span>](https://www.microsoft.com/trustcenter/Privacy/GDPR)
