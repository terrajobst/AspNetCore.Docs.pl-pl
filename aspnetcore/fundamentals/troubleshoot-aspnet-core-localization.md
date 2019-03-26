---
title: Rozwiązywanie problemów z lokalizacji platformy ASP.NET Core
author: hishamco
description: Dowiedz się, jak diagnozować problemy z lokalizacją w aplikacji platformy ASP.NET Core.
ms.date: 01/24/2019
uid: fundamentals/troubleshoot-aspnet-core-localization
ms.openlocfilehash: 73405f539c89d79096e7b536407cd9730679d478
ms.sourcegitcommit: 687ffb15ebe65379f75c84739ea851d5a0d788b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58489008"
---
# <a name="troubleshoot-aspnet-core-localization"></a>Rozwiązywanie problemów z lokalizacji platformy ASP.NET Core

Przez [Ateya Hisham pojemnika](https://github.com/hishamco)

Ten artykuł zawiera instrukcje na temat diagnozowania problemów z lokalizacji aplikacji platformy ASP.NET Core.

## <a name="localization-configuration-issues"></a>Problemy z konfiguracją lokalizacja

**Kolejność oprogramowania pośredniczącego lokalizacji**  
Aplikacja nie mogą lokalizować, ponieważ oprogramowanie pośredniczące lokalizacji nie jest określona, zgodnie z oczekiwaniami.

Aby rozwiązać ten problem, upewnij się, że oprogramowanie pośredniczące tej lokalizacji jest zarejestrowana przed MVC oprogramowania pośredniczącego. W przeciwnym razie oprogramowania pośredniczącego lokalizacji nie jest stosowane.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddLocalization(options => options.ResourcesPath = "Resources");

    services.AddMvc();
}
```

**Nie można odnaleźć ścieżki zasobów w lokalizacji**

**Obsługiwanych kultur w RequestCultureProvider nie jest zgodny z zarejestrowani**  

## <a name="resource-file-naming-issues"></a>Zagadnienia dotyczące nazewnictwa plików zasobów

Platforma ASP.NET Core ma wstępnie zdefiniowane zasady i wytyczne dotyczące lokalizacji nazywania plików zasobów, które zostały szczegółowo opisane w [tutaj](xref:fundamentals/localization?view=aspnetcore-2.2#resource-file-naming).

## <a name="missing-resources"></a>Brak zasobów

Typowe przyczyny nie odnaleziono zasobów:

- Nazwy zasobów jest błędnie wpisana w jednym `resx` plik lub żądanie lokalizatorowi.
- Brakuje zasobu `resx` w przypadku niektórych języków, ale istnieje w innym.
- Jeśli nadal występują problemy, sprawdź komunikaty dziennika lokalizacji (które znajdują się na `Debug` poziom dziennika) Aby uzyskać więcej informacji o brakujących zasobów.

_**Wskazówka:** Korzystając z `CookieRequestCultureProvider`, sprawdź apostrofy nie są używane przy użyciu kultur wewnątrz lokalizacji wartość pliku cookie. Na przykład `c='en-UK'|uic='en-US'` jest nieprawidłowa wartość pliku cookie, podczas gdy `c=en-UK|uic=en-US` jest prawidłowy._

## <a name="resources--class-libraries-issues"></a>Problemy z zasobami i bibliotek klas

Platforma ASP.NET Core domyślnie zapewnia sposób umożliwić bibliotek klas znaleźć ich pliki zasobów za pomocą [ResourceLocationAttribute](/dotnet/api/microsoft.extensions.localization.resourcelocationattribute?view=aspnetcore-2.1).

Typowe problemy z biblioteki klas obejmują:
- Brak `ResourceLocationAttribute` w klasie uniemożliwi biblioteki `ResourceManagerStringLocalizerFactory` od odnajdowania zasobów.
- Nazywanie pliku zasobów. Aby uzyskać więcej informacji, zobacz [nazewnictwa problemy pliku zasobów](#resource-file-naming-issues) sekcji.
- Zmiana główna przestrzeń nazw biblioteki klas. Aby uzyskać więcej informacji, zobacz [wystawia głównego Namespace](#root-namespace-issues) sekcji.

## <a name="customrequestcultureprovider-doesnt-work-as-expected"></a>CustomRequestCultureProvider nie działa zgodnie z oczekiwaniami

`RequestLocalizationOptions` Klasa ma trzy domyślnych dostawców:

1. `QueryStringRequestCultureProvider`
2. `CookieRequestCultureProvider`
3. `AcceptLanguageHeaderRequestCultureProvider`

[CustomRequestCultureProvider](/dotnet/api/microsoft.aspnetcore.localization.customrequestcultureprovider?view=aspnetcore-2.1) pozwala dostosować sposób kulturę lokalizacji znajduje się w aplikacji. `CustomRequestCultureProvider` Jest używany podczas domyślnych dostawców nie spełniają Twoich wymagań.

- Typową przyczyną niestandardowego dostawcy nie działają prawidłowo jest, że nie jest pierwszym dostawcą usług w `RequestCultureProviders` listy. Aby rozwiązać ten problem:

- Wstawianie niestandardowego dostawcy na pozycji 0 w `RequestCultureProviders` listy zgodnie z poniższymi:

```csharp
options.RequestCultureProviders.Insert(0, new CustomRequestCultureProvider(async context =>
    {
        // My custom request culture logic
        return new ProviderCultureResult("en");
    }));
```

- Użyj `AddInitialRequestCultureProvider` metodę rozszerzenia, aby ustawić niestandardowego dostawcy jako początkowej dostawcy.

## <a name="root-namespace-issues"></a>Namespace głównych problemów

Gdy główna przestrzeń nazw zestawu jest inna niż nazwa zestawu, lokalizacja nie działa domyślnie. Aby uniknąć tego problemu użyj [RootNamespace](/dotnet/api/microsoft.extensions.localization.rootnamespaceattribute?view=aspnetcore-2.1), który został szczegółowo opisany w [tutaj](xref:fundamentals/localization?view=aspnetcore-2.2#resource-file-naming)

## <a name="resources--build-action"></a>Akcja kompilacji & zasobów

Jeśli używasz plików zasobów do lokalizacji, ważne jest, do których mają odpowiednie działania. Powinny one być **zasób osadzony**, w przeciwnym razie `ResourceStringLocalizer` nie będzie mógł odnaleźć te zasoby.
