---
title: Rozwiązywanie problemów z lokalizacji platformy ASP.NET Core
author: hishamco
description: Dowiedz się, jak diagnozować problemy z lokalizacją w aplikacji platformy ASP.NET Core.
ms.author: riande
ms.date: 01/24/2019
uid: fundamentals/troubleshoot-aspnet-core-localization
ms.openlocfilehash: c76732c1a0389818f8f9efae8fe384ca0f9ca308
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087377"
---
# <a name="troubleshoot-aspnet-core-localization"></a><span data-ttu-id="aba01-103">Rozwiązywanie problemów z lokalizacji platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="aba01-103">Troubleshoot ASP.NET Core Localization</span></span>

<span data-ttu-id="aba01-104">Przez [Ateya Hisham pojemnika](https://github.com/hishamco)</span><span class="sxs-lookup"><span data-stu-id="aba01-104">By [Hisham Bin Ateya](https://github.com/hishamco)</span></span>

<span data-ttu-id="aba01-105">Ten artykuł zawiera instrukcje na temat diagnozowania problemów z lokalizacji aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="aba01-105">This article provides instructions on how to diagnose ASP.NET Core app localization issues.</span></span>

## <a name="localization-configuration-issues"></a><span data-ttu-id="aba01-106">Problemy z konfiguracją lokalizacja</span><span class="sxs-lookup"><span data-stu-id="aba01-106">Localization configuration issues</span></span>

<span data-ttu-id="aba01-107">**Kolejność oprogramowania pośredniczącego lokalizacji**</span><span class="sxs-lookup"><span data-stu-id="aba01-107">**Localization middleware order**</span></span>  
<span data-ttu-id="aba01-108">Aplikacja nie mogą lokalizować, ponieważ oprogramowanie pośredniczące lokalizacji nie jest określona, zgodnie z oczekiwaniami.</span><span class="sxs-lookup"><span data-stu-id="aba01-108">The app may not localize because the localization middleware isn't ordered as expected.</span></span>

<span data-ttu-id="aba01-109">Aby rozwiązać ten problem, upewnij się, że oprogramowanie pośredniczące tej lokalizacji jest zarejestrowana przed MVC oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="aba01-109">To resolve this issue, ensure that localization middleware is registered before MVC middleware.</span></span> <span data-ttu-id="aba01-110">W przeciwnym razie oprogramowania pośredniczącego lokalizacji nie jest stosowane.</span><span class="sxs-lookup"><span data-stu-id="aba01-110">Otherwise, the localization middleware isn't applied.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddLocalization(options => options.ResourcesPath = "Resources");

    services.AddMvc();
}
```

<span data-ttu-id="aba01-111">**Nie można odnaleźć ścieżki zasobów w lokalizacji**</span><span class="sxs-lookup"><span data-stu-id="aba01-111">**Localization resources path not found**</span></span>

<span data-ttu-id="aba01-112">**Obsługiwanych kultur w RequestCultureProvider nie jest zgodny z zarejestrowani**</span><span class="sxs-lookup"><span data-stu-id="aba01-112">**Supported Cultures in RequestCultureProvider don't match with registered once**</span></span>  

## <a name="resource-file-naming-issues"></a><span data-ttu-id="aba01-113">Zagadnienia dotyczące nazewnictwa plików zasobów</span><span class="sxs-lookup"><span data-stu-id="aba01-113">Resource file naming issues</span></span>

<span data-ttu-id="aba01-114">Platforma ASP.NET Core ma wstępnie zdefiniowane zasady i wytyczne dotyczące lokalizacji nazywania plików zasobów, które zostały szczegółowo opisane w [tutaj](xref:fundamentals/localization?view=aspnetcore-2.2#resource-file-naming).</span><span class="sxs-lookup"><span data-stu-id="aba01-114">ASP.NET Core has predefined rules and guidelines for localization resources file naming, which are described in detail [here](xref:fundamentals/localization?view=aspnetcore-2.2#resource-file-naming).</span></span>

## <a name="missing-resources"></a><span data-ttu-id="aba01-115">Brak zasobów</span><span class="sxs-lookup"><span data-stu-id="aba01-115">Missing resources</span></span>

<span data-ttu-id="aba01-116">Typowe przyczyny nie odnaleziono zasobów:</span><span class="sxs-lookup"><span data-stu-id="aba01-116">Common causes of resources not being found include:</span></span>

- <span data-ttu-id="aba01-117">Nazwy zasobów jest błędnie wpisana w jednym `resx` plik lub żądanie lokalizatorowi.</span><span class="sxs-lookup"><span data-stu-id="aba01-117">Resource names are misspelled in either the `resx` file or the localizer request.</span></span>
- <span data-ttu-id="aba01-118">Brakuje zasobu `resx` w przypadku niektórych języków, ale istnieje w innym.</span><span class="sxs-lookup"><span data-stu-id="aba01-118">The resource is missing from the `resx` for some languages, but exists in others.</span></span>
- <span data-ttu-id="aba01-119">Jeśli nadal występują problemy, sprawdź komunikaty dziennika lokalizacji (które znajdują się na `Debug` poziom dziennika) Aby uzyskać więcej informacji o brakujących zasobów.</span><span class="sxs-lookup"><span data-stu-id="aba01-119">If you're still having trouble, check the localization log messages (which are at `Debug` log level) for more details about the missing resources.</span></span>

<span data-ttu-id="aba01-120">_**Wskazówka:** Korzystając z `CookieRequestCultureProvider`, sprawdź apostrofy nie są używane przy użyciu kultur wewnątrz lokalizacji wartość pliku cookie. Na przykład `c='en-UK'|uic='en-US'` jest nieprawidłowa wartość pliku cookie, podczas gdy `c=en-UK|uic=en-US` jest prawidłowy._</span><span class="sxs-lookup"><span data-stu-id="aba01-120">_**Hint:** When using `CookieRequestCultureProvider`, verify single quotes are not used with the cultures inside the localization cookie value. For example, `c='en-UK'|uic='en-US'` is an invalid cookie value, while `c=en-UK|uic=en-US` is a valid._</span></span>

## <a name="resources--class-libraries-issues"></a><span data-ttu-id="aba01-121">Problemy z zasobami i bibliotek klas</span><span class="sxs-lookup"><span data-stu-id="aba01-121">Resources & Class Libraries issues</span></span>

<span data-ttu-id="aba01-122">Platforma ASP.NET Core domyślnie zapewnia sposób umożliwić bibliotek klas znaleźć ich pliki zasobów za pomocą [ResourceLocationAttribute](/dotnet/api/microsoft.extensions.localization.resourcelocationattribute?view=aspnetcore-2.1).</span><span class="sxs-lookup"><span data-stu-id="aba01-122">ASP.NET Core by default provides a way to allow the class libraries to find their resource files via [ResourceLocationAttribute](/dotnet/api/microsoft.extensions.localization.resourcelocationattribute?view=aspnetcore-2.1).</span></span>

<span data-ttu-id="aba01-123">Typowe problemy z biblioteki klas obejmują:</span><span class="sxs-lookup"><span data-stu-id="aba01-123">Common issues with class libraries include:</span></span>
- <span data-ttu-id="aba01-124">Brak `ResourceLocationAttribute` w klasie uniemożliwi biblioteki `ResourceManagerStringLocalizerFactory` od odnajdowania zasobów.</span><span class="sxs-lookup"><span data-stu-id="aba01-124">Missing the `ResourceLocationAttribute` in a class library will prevent `ResourceManagerStringLocalizerFactory` from discovering the resources.</span></span>
- <span data-ttu-id="aba01-125">Nazywanie pliku zasobów.</span><span class="sxs-lookup"><span data-stu-id="aba01-125">Resource file naming.</span></span> <span data-ttu-id="aba01-126">Aby uzyskać więcej informacji, zobacz [nazewnictwa problemy pliku zasobów](#resource-file-naming-issues) sekcji.</span><span class="sxs-lookup"><span data-stu-id="aba01-126">For more information, see [Resource file naming issues](#resource-file-naming-issues) section.</span></span>
- <span data-ttu-id="aba01-127">Zmiana główna przestrzeń nazw biblioteki klas.</span><span class="sxs-lookup"><span data-stu-id="aba01-127">Changing the root namespace of the class library.</span></span> <span data-ttu-id="aba01-128">Aby uzyskać więcej informacji, zobacz [wystawia głównego Namespace](#root-namespace-issues) sekcji.</span><span class="sxs-lookup"><span data-stu-id="aba01-128">For more information, see [Root Namespace issues](#root-namespace-issues) section.</span></span>

## <a name="customrequestcultureprovider-doesnt-work-as-expected"></a><span data-ttu-id="aba01-129">CustomRequestCultureProvider nie działa zgodnie z oczekiwaniami</span><span class="sxs-lookup"><span data-stu-id="aba01-129">CustomRequestCultureProvider doesn't work as expected</span></span>

<span data-ttu-id="aba01-130">`RequestLocalizationOptions` Klasa ma trzy domyślnych dostawców:</span><span class="sxs-lookup"><span data-stu-id="aba01-130">The `RequestLocalizationOptions` class has three default providers:</span></span>

1. `QueryStringRequestCultureProvider`
2. `CookieRequestCultureProvider`
3. `AcceptLanguageHeaderRequestCultureProvider`

<span data-ttu-id="aba01-131">[CustomRequestCultureProvider](/dotnet/api/microsoft.aspnetcore.localization.customrequestcultureprovider?view=aspnetcore-2.1) pozwala dostosować sposób kulturę lokalizacji znajduje się w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="aba01-131">The [CustomRequestCultureProvider](/dotnet/api/microsoft.aspnetcore.localization.customrequestcultureprovider?view=aspnetcore-2.1) allows you to customize how the localization culture is provided in your app.</span></span> <span data-ttu-id="aba01-132">`CustomRequestCultureProvider` Jest używany podczas domyślnych dostawców nie spełniają Twoich wymagań.</span><span class="sxs-lookup"><span data-stu-id="aba01-132">The `CustomRequestCultureProvider` is used when the default providers don't meet your requirements.</span></span>

- <span data-ttu-id="aba01-133">Typową przyczyną niestandardowego dostawcy nie działają prawidłowo jest, że nie jest pierwszym dostawcą usług w `RequestCultureProviders` listy.</span><span class="sxs-lookup"><span data-stu-id="aba01-133">A common reason custom provider don't work properly is that it isn't the first provider in the `RequestCultureProviders` list.</span></span> <span data-ttu-id="aba01-134">Aby rozwiązać ten problem:</span><span class="sxs-lookup"><span data-stu-id="aba01-134">To resolve this issue:</span></span>

- <span data-ttu-id="aba01-135">Wstawianie niestandardowego dostawcy na pozycji 0 w `RequestCultureProviders` listy zgodnie z poniższymi:</span><span class="sxs-lookup"><span data-stu-id="aba01-135">Insert the custom provider at the position 0 in the `RequestCultureProviders` list as the following:</span></span>

```csharp
options.RequestCultureProviders.Insert(0, new CustomRequestCultureProvider(async context =>
    {
        // My custom request culture logic
        return new ProviderCultureResult("en");
    }));
```

- <span data-ttu-id="aba01-136">Użyj `AddInitialRequestCultureProvider` metodę rozszerzenia, aby ustawić niestandardowego dostawcy jako początkowej dostawcy.</span><span class="sxs-lookup"><span data-stu-id="aba01-136">Use `AddInitialRequestCultureProvider` extension method to set the custom provider as initial provider.</span></span>

## <a name="root-namespace-issues"></a><span data-ttu-id="aba01-137">Namespace głównych problemów</span><span class="sxs-lookup"><span data-stu-id="aba01-137">Root Namespace issues</span></span>

<span data-ttu-id="aba01-138">Gdy główna przestrzeń nazw zestawu jest inna niż nazwa zestawu, lokalizacja nie działa domyślnie.</span><span class="sxs-lookup"><span data-stu-id="aba01-138">When the root namespace of an assembly is different than the assembly name, localization doesn't work by default.</span></span> <span data-ttu-id="aba01-139">Aby uniknąć tego problemu użyj [RootNamespace](/dotnet/api/microsoft.extensions.localization.rootnamespaceattribute?view=aspnetcore-2.1), który został szczegółowo opisany w [tutaj](xref:fundamentals/localization?view=aspnetcore-2.2#resource-file-naming)</span><span class="sxs-lookup"><span data-stu-id="aba01-139">To avoid this issue use [RootNamespace](/dotnet/api/microsoft.extensions.localization.rootnamespaceattribute?view=aspnetcore-2.1), which is described in detail [here](xref:fundamentals/localization?view=aspnetcore-2.2#resource-file-naming)</span></span>

## <a name="resources--build-action"></a><span data-ttu-id="aba01-140">Akcja kompilacji & zasobów</span><span class="sxs-lookup"><span data-stu-id="aba01-140">Resources & Build Action</span></span>

<span data-ttu-id="aba01-141">Jeśli używasz plików zasobów do lokalizacji, ważne jest, do których mają odpowiednie działania.</span><span class="sxs-lookup"><span data-stu-id="aba01-141">If you use resource files for localization, it's important that they have an appropriate build action.</span></span> <span data-ttu-id="aba01-142">Powinny one być **zasób osadzony**, w przeciwnym razie `ResourceStringLocalizer` nie będzie mógł odnaleźć te zasoby.</span><span class="sxs-lookup"><span data-stu-id="aba01-142">They should be **Embedded Resource**, otherwise the `ResourceStringLocalizer` is not able to find these resources.</span></span>
