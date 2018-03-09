---
title: Co to jest nowe w programie ASP.NET 2.0 Core
author: rick-anderson
description: Co to jest nowe w programie ASP.NET 2.0 Core
manager: wpickett
ms.author: riande
ms.date: 07/10/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: aspnetcore-2.0
ms.openlocfilehash: 81d24088d53a7e37d17bb7c57892c98efb06ca6f
ms.sourcegitcommit: 53ee14b9c8200f44705d8997c3619fa874192d45
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/08/2018
---
# <a name="whats-new-in-aspnet-core-20"></a>Co to jest nowe w programie ASP.NET 2.0 Core

W tym artykule omówiono najbardziej znaczących zmian w programie ASP.NET 2.0 Core, wraz z łączami do odpowiedniej dokumentacji.

## <a name="razor-pages"></a>Stron razor

Stron razor to nowa funkcja platformy ASP.NET Core MVC umożliwia kodowanie strony scenariusze łatwiejsze i bardziej wydajnej pracy.

Aby uzyskać więcej informacji zobacz wprowadzenie i samouczek:

* [Wprowadzenie do stron Razor](xref:mvc/razor-pages/index)
* [Wprowadzenie do korzystania ze stron Razor](xref:tutorials/razor-pages/razor-pages-start)

## <a name="aspnet-core-metapackage"></a>Metapackage platformy ASP.NET Core

Nowe metapackage platformy ASP.NET Core obejmuje wszystkie pakiety wprowadzone i obsługiwane przez zespoły programu Entity Framework Core i ASP.NET Core, wraz z ich zależności wewnętrzne i 3rd firm. Nie trzeba wybrać poszczególne platformy ASP.NET Core funkcje przez pakiet. Wszystkie funkcje dostępne w [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) pakietu. Szablony domyślne, użyj tego pakietu.

Aby uzyskać więcej informacji, zobacz [metapackage Microsoft.AspNetCore.All dla programu ASP.NET 2.0 Core](xref:fundamentals/metapackage).

## <a name="runtime-store"></a>Środowisko uruchomieniowe magazynu

Aplikacje używające `Microsoft.AspNetCore.All` metapackage automatycznie korzystać z nowego magazynu środowiska uruchomieniowego .NET Core. Magazyn zawiera wszystkie zasoby środowiska uruchomieniowego potrzebne do uruchomienia aplikacji platformy ASP.NET Core 2.0. Jeśli używasz `Microsoft.AspNetCore.All` metapackage, nie zasoby, z którym związane są odwołania pakietów platformy ASP.NET Core NuGet są wdrażane w aplikacji, ponieważ są one już przechowywane na komputerze docelowym. Zasoby w magazynie środowiska uruchomieniowego są również wstępnie skompilowanym skrócić czas uruchomienia aplikacji.

Aby uzyskać więcej informacji, zobacz [magazynu środowiska wykonawczego](https://docs.microsoft.com/dotnet/core/deploying/runtime-store)

## <a name="net-standard-20"></a>.NET Standard 2.0

Pakiety programu ASP.NET 2.0 Core docelowe .NET 2.0 standardowa. Pakiety mogą odwoływać się inne biblioteki .NET 2.0 standardowych i mogą być uruchamiane na standardowe .NET 2.0 zgodne implementacje .NET, w tym .NET Core 2.0 i .NET Framework 4.6.1. 

`Microsoft.AspNetCore.All` Metapackage dotyczy tylko .NET Core 2.0, ponieważ jest on przeznaczony do użycia ze sklepem środowiska uruchomieniowego .NET Core 2.0.

## <a name="configuration-update"></a>Aktualizacja konfiguracji

`IConfiguration` Wystąpienia jest domyślnie w programie ASP.NET 2.0 Core dodawany do kontenera usług. `IConfiguration` w usługach kontenera ułatwia tworzenie aplikacji można pobrać wartości konfiguracji z kontenera.

Aby uzyskać informacje o stanie planowanej dokumentacji, zobacz [problem GitHub](https://github.com/aspnet/Docs/issues/3387).

## <a name="logging-update"></a>Rejestrowanie aktualizacji

W programie ASP.NET 2.0 Core rejestrowanie jest domyślnie włączona do systemu (Podpisane) iniekcji zależności. Dodawanie dostawcy i skonfigurować filtrowanie w *Program.cs* zamiast w pliku *Startup.cs* pliku. Wartością domyślną `ILoggerFactory` obsługuje filtrowanie w taki sposób, który pozwala używać jednego elastyczne podejście zarówno dla filtrowania krzyżowego dostawcy i filtrowanie określonego dostawcy.

Aby uzyskać więcej informacji, zobacz [wprowadzenie do rejestrowania](xref:fundamentals/logging/index).

## <a name="authentication-update"></a>Aktualizacja uwierzytelniania

Nowy model uwierzytelniania ułatwia konfigurowanie uwierzytelniania dla aplikacji przy użyciu Podpisane.

Nowe szablony są dostępne do konfigurowania uwierzytelniania dla aplikacji sieci web i interfejsów API przy użyciu [usługi Azure AD B2C] sieci web (https://azure.microsoft.com/services/active-directory-b2c/).

Aby uzyskać informacje o stanie planowanej dokumentacji, zobacz [problem GitHub](https://github.com/aspnet/Docs/issues/3054).

## <a name="identity-update"></a>Aktualizacja tożsamości

Ułatwiliśmy ułatwia tworzenie bezpiecznej przy użyciu tożsamości w programie ASP.NET 2.0 podstawowych interfejsów API sieci web. W przypadku uzyskania tokenów dostępu do uzyskiwania dostępu do sieci web API przy użyciu [biblioteki uwierzytelniania firmy Microsoft (MSAL)](https://www.nuget.org/packages/Microsoft.Identity.Client).

Aby uzyskać więcej informacji na temat uwierzytelniania zmian w 2.0 zobacz następujące zasoby:

* [Potwierdzenie konta i hasła odzyskiwania w ASP.NET Core](xref:security/authentication/accconfirm)
* [Włączenie generowania kodu QR uwierzytelniania aplikacji w ASP.NET Core](xref:security/authentication/identity-enable-qrcodes)
* [Migrowanie uwierzytelnianie i tożsamość platformy ASP.NET Core 2.0](xref:migration/1x-to-2x/identity-2x)

## <a name="spa-templates"></a>Szablony SPA

Jednej strony aplikacji JEDNOSTRONICOWEJ szablonów projektu dla kątową, Aurelia Knockout.js, React.js i React.js z Redux są dostępne. Dyrektywy Angular szablon został uaktualniony do dyrektywy Angular 4. Szablony kątową i bibliotece React. domyślnie są dostępne; Aby uzyskać informacje o sposobie pobrania innych szablonów, zobacz [tworzenia nowego projektu SPA](xref:client-side/spa-services#creating-a-new-project). Aby uzyskać informacje o sposobie budowania SPA w ASP.NET Core, zobacz [przy użyciu JavaScriptServices tworzenie aplikacji jednej strony](xref:client-side/spa-services).

## <a name="kestrel-improvements"></a>Ulepszenia kestrel

Serwer sieci web Kestrel ma nowe funkcje ułatwiające odpowiedniejsze jako serwer internetowy. Dodano wiele opcji konfiguracji serwera ograniczenia w `KestrelServerOptions` na nowe klasy `Limits` właściwości. Dodaj następujące limity:

- Maksymalna liczba połączeń klientów
- Żądanie maksymalny rozmiar treści
- Szybkość danych treści żądania minimalna

Aby uzyskać więcej informacji, zobacz [Kestrel implementacja serwera sieci web platformy ASP.NET Core](xref:fundamentals/servers/kestrel).

## <a name="weblistener-renamed-to-httpsys"></a>WebListener zmieniona do pliku HTTP.sys

Pakiety `Microsoft.AspNetCore.Server.WebListener` i `Microsoft.Net.Http.Server` zostały scalone w nowym pakiecie `Microsoft.AspNetCore.Server.HttpSys`. Przestrzenie nazw zostały zaktualizowane do dopasowania.

Aby uzyskać więcej informacji, zobacz [HTTP.sys implementacja serwera sieci web platformy ASP.NET Core](xref:fundamentals/servers/httpsys).

## <a name="enhanced-http-header-support"></a>Ulepszona obsługa nagłówka HTTP

W przypadku używania MVC do przesyłania `FileStreamResult` lub `FileContentResult`, masz teraz opcję, aby ustawić `ETag` lub `LastModified` Data przesyłania zawartości. Można ustawić te wartości zwracane zawartości z kodem podobny do następującego:

```csharp
var data = Encoding.UTF8.GetBytes("This is a sample text from a binary array");
var entityTag = new EntityTagHeaderValue("\"MyCalculatedEtagValue\"");
return File(data, "text/plain", "downloadName.txt", lastModified: DateTime.UtcNow.AddSeconds(-5), entityTag: entityTag);
```

Powrót do odwiedzających plik zostanie ozdobione odpowiednie nagłówki HTTP dla `ETag` i `LastModified` wartości.

Jeśli obiekt odwiedzający żądań zawartości aplikacji przy użyciu nagłówka żądania zakresu, ASP.NET będzie uznają, że i obsługiwać tego nagłówka. Jeśli żądanej zawartości mogą być dostarczane częściowo, ASP.NET odpowiednio Pomiń i zwrócić tylko żądany zestaw bajtów.  Nie trzeba zapisać wszelkie specjalne obsługi do metody zmienić lub obsługi tej funkcji; odbywa się automatycznie za Ciebie.

## <a name="hosting-startup-and-application-insights"></a>Hosting uruchamiania i usługi Application Insights

Środowiskach hostingu można teraz wstrzyknięcia zależności dodatkowych pakietów i wykonanie kodu podczas uruchamiania aplikacji bez konieczności jawnie zależności lub wywołać żadnych metod aplikacji. Ta funkcja umożliwia włączanie niektórych środowiskach do funkcji "światła w górę" unikatowe dla tego środowiska bez dokładnej znajomości wcześniejsze aplikacji. 

W programie ASP.NET 2.0 Core, ta funkcja jest używana, można automatycznie włączyć usługi Application Insights diagnostyczne podczas debugowania w programie Visual Studio i (po skorzystaniu z rozwiązania) uruchomionej w usłudze Azure App Services. W związku z tym szablony projektów nie jest już Dodawanie pakietów usługi Application Insights i kodu domyślnie.

Aby uzyskać informacje o stanie planowanej dokumentacji, zobacz [problem GitHub](https://github.com/aspnet/Docs/issues/3389).

## <a name="automatic-use-of-anti-forgery-tokens"></a>Automatyczne stosowanie tokenów zabezpieczających przed sfałszowaniem

Platformy ASP.NET Core zawsze pomogła kodowanie HTML zawartości domyślnie, ale z nową wersją dodatkowego kroku jest podjęte w celu zapobieżenia fałszerstwie żądania międzywitrynowego (XSRF). Platformy ASP.NET Core teraz Emituj tokenów zabezpieczających przed sfałszowaniem domyślnie i weryfikacji ich na stronach bez dodatkowej konfiguracji i akcji POST formularza.

Aby uzyskać więcej informacji, zobacz [zapobiec Cross-Site żądania Międzywitrynowego (XSRF/CSRF) przed atakami opartymi na](xref:security/anti-request-forgery).

## <a name="automatic-precompilation"></a>Automatyczne wstępnej kompilacji

Wstępna kompilacja widoku razor jest domyślnie podczas publikowania, zmniejszenie jego rozmiar dane wyjściowe publikowania i aplikacji czas uruchamiania.

Aby uzyskać więcej informacji, zobacz [kompilacji widoku Razor i wstępnej kompilacji w ASP.NET Core](xref:mvc/views/view-compilation).

## <a name="razor-support-for-c-71"></a>Obsługa razor 7.1 C#

Do pracy z nowy kompilator Roslyn został zaktualizowany przez aparat widoku Razor. Która obejmuje obsługę funkcji C# 7.1 jak domyślne wyrażenia, wywnioskować nazwy spójnej kolekcji i dopasowywanie do wzorca z ogólnymi. Aby użyć 7.1 C# w projekcie, dodaj następującą właściwość w pliku projektu, a następnie ponownie załaduj rozwiązania:

```xml
<LangVersion>latest</LangVersion>
```

Aby uzyskać informacje o stanie funkcji 7.1 C#, zobacz [repozytorium Roslyn GitHub](https://github.com/dotnet/roslyn/blob/master/docs/Language%20Feature%20Status.md).

## <a name="other-documentation-updates-for-20"></a>Inne aktualizacje dokumentacji 2.0

* [Profilów dla wdrożenia aplikacji platformy ASP.NET Core publikowania programu Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles)
* [Zarządzanie kluczami](xref:security/data-protection/implementation/key-management)
* [Konfigurowanie uwierzytelniania usługi Facebook](xref:security/authentication/facebook-logins)
* [Konfigurowanie uwierzytelniania usługi Twitter](xref:security/authentication/twitter-logins)
* [Konfigurowanie uwierzytelniania serwisu Google](xref:security/authentication/google-logins)
* [Konfigurowanie uwierzytelniania Account firmy Microsoft](xref:security/authentication/microsoft-logins)

## <a name="migration-guidance"></a>Wskazówki dotyczące migracji

Aby uzyskać wskazówki dotyczące sposobu przeprowadzenia migracji platformy ASP.NET Core 1.x aplikacji platformy ASP.NET Core 2.0 zobacz następujące zasoby:

* [Migrowanie z platformy ASP.NET Core 1.x do platformy ASP.NET Core 2.0](xref:migration/1x-to-2x/index)
* [Migrowanie uwierzytelnianie i tożsamość platformy ASP.NET Core 2.0](xref:migration/1x-to-2x/identity-2x)

## <a name="additional-information"></a>Dodatkowe informacje

Aby uzyskać pełną listę zmian, zobacz [2.0 informacje o wersji platformy ASP.NET Core](https://github.com/aspnet/Home/releases/tag/2.0.0).

Aby połączyć się z postęp i plany zespół deweloperów platformy ASP.NET Core, dostroić [Standup społeczności ASP.NET](https://live.asp.net/).
