---
title: What's new in ASP.NET Core 2.0
author: rick-anderson
description: Dowiedz się więcej o nowych funkcjach w programie ASP.NET Core 2.0.
ms.author: riande
ms.custom: mvc
ms.date: 05/28/2019
uid: aspnetcore-2.0
ms.openlocfilehash: dac5a48042a71774cb884e30a166be68bee69691
ms.sourcegitcommit: f5762967df3be8b8c868229e679301f2f7954679
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67048110"
---
# <a name="whats-new-in-aspnet-core-20"></a>What's new in ASP.NET Core 2.0

W tym artykule przedstawiono najbardziej znaczących zmian w programie ASP.NET Core 2.0 wraz z łączami do odpowiedniej dokumentacji.

## <a name="razor-pages"></a>Razor Pages

Strony razor jest to nowa funkcja usługi ASP.NET Core MVC, która sprawia, że kodowania skoncentrowane na stronie scenariuszy łatwiejsze i bardziej wydajne.

Aby uzyskać więcej informacji zobacz wprowadzenie i samouczków:

* [Wprowadzenie do produktu Razor Pages](xref:razor-pages/index)
* [Wprowadzenie do korzystania ze stron Razor](xref:tutorials/razor-pages/razor-pages-start)

## <a name="aspnet-core-metapackage"></a>Meta Microsoft.aspnetcore.all platformy ASP.NET Core

Nowe meta Microsoft.aspnetcore.all platformy ASP.NET Core obejmuje wszystkie pakiety wprowadzone i obsługiwane przez zespoły platformy ASP.NET Core i Entity Framework Core, wraz z ich zależnościami wewnętrznych i innych firm 3. Nie trzeba wybrać poszczególne platformy ASP.NET Core funkcje przez pakiet. Wszystkie funkcje są uwzględnione w [pakiet](https://www.nuget.org/packages/Microsoft.AspNetCore.All) pakietu. Szablony domyślne używają tego pakietu.

Aby uzyskać więcej informacji, zobacz [pakiet meta Microsoft.aspnetcore.all dla programu ASP.NET Core 2.0](xref:fundamentals/metapackage).

## <a name="runtime-store"></a>Środowisko uruchomieniowe Store

Aplikacje, które używają `Microsoft.AspNetCore.All` meta Microsoft.aspnetcore.all automatycznie wykorzystywać nowe Store środowiska uruchomieniowego programu .NET Core. Store zawiera wszystkie zasoby środowiska uruchomieniowego, wymagane do uruchamiania aplikacji ASP.NET Core 2.0. Kiedy używasz `Microsoft.AspNetCore.All` meta Microsoft.aspnetcore.all, żadne zasoby z przywoływanych pakietów dla platformy ASP.NET Core NuGet są wdrażane przy użyciu aplikacji, ponieważ są one już przechowywane w systemie docelowym. Zasoby w Store środowiska uruchomieniowego są również wstępnie skompilowanych, aby poprawić czas uruchamiania aplikacji.

Aby uzyskać więcej informacji, zobacz [magazynu środowiska uruchomieniowego](/dotnet/core/deploying/runtime-store)

## <a name="net-standard-20"></a>.NET Standard 2.0

Pakiety platformy ASP.NET Core 2.0 docelowych .NET Standard 2.0. Pakiety mogą być przywoływane przez inne biblioteki .NET Standard 2.0 i mogą być uruchamiane na .NET Standard 2.0 zgodne implementacji .NET, w tym .NET Core 2.0 i .NET Framework 4.6.1. 

`Microsoft.AspNetCore.All` Meta Microsoft.aspnetcore.all jest przeznaczony dla platformy .NET Core 2.0, ponieważ jest ono przeznaczone do użycia z Store środowiska uruchomieniowego programu .NET Core 2.0.

## <a name="configuration-update"></a>Aktualizacja konfiguracji

`IConfiguration` Wystąpienia zostanie dodany do kontenera usług, domyślnie w programie ASP.NET Core 2.0. `IConfiguration` w usługach kontenerów ułatwia dla aplikacji, które można pobrać wartości konfiguracji z kontenera.

Aby uzyskać informacje o stanie planowanej dokumentacji, zobacz [problem w usłudze GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/3387).

## <a name="logging-update"></a>Rejestrowanie aktualizacji

W programie ASP.NET Core 2.0 rejestrowania jest włączona w systemie (DI) iniekcji zależności domyślnie. Dodawanie dostawcy i Konfigurowanie filtrowania w *Program.cs* zamiast w pliku *Startup.cs* pliku. I domyślnego `ILoggerFactory` obsługuje filtrowanie w sposób, który pozwala na użytek jeden elastyczne podejście filtrowanie krzyżowe dostawcy i filtrowanie określonego dostawcy.

Aby uzyskać więcej informacji, zobacz [wprowadzenie do rejestrowania](xref:fundamentals/logging/index).

## <a name="authentication-update"></a>Aktualizacja uwierzytelniania

Nowy model uwierzytelniania ułatwia skonfigurowanie uwierzytelniania dla aplikacji przy użyciu DI.

Nowe szablony są dostępne do konfigurowania uwierzytelniania dla aplikacji sieci web i interfejsów API sieci web przy użyciu [usługi Azure AD B2C](https://azure.microsoft.com/services/active-directory-b2c/).

Aby uzyskać informacje o stanie planowanej dokumentacji, zobacz [problem w usłudze GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/3054).

## <a name="identity-update"></a>Aktualizacja tożsamości

Wprowadziliśmy je łatwiej tworzyć bezpieczne internetowych interfejsów API przy użyciu tożsamości w programie ASP.NET Core 2.0. Możesz uzyskać tokenów dostępu do uzyskiwania dostępu do sieci web API za pomocą [Microsoft Authentication Library (MSAL)](https://www.nuget.org/packages/Microsoft.Identity.Client).

Aby uzyskać więcej informacji na temat zmian uwierzytelniania w wersji 2.0 zobacz następujące zasoby:

* [Potwierdzenie konta i odzyskiwanie hasła w programie ASP.NET Core](xref:security/authentication/accconfirm)
* [Włączanie generowania kodu QR dla wystawcy uwierzytelnienia aplikacji w programie ASP.NET Core](xref:security/authentication/identity-enable-qrcodes)
* [Migrowanie do platformy ASP.NET Core 2.0 uwierzytelniania i tożsamości](xref:migration/1x-to-2x/identity-2x)

## <a name="spa-templates"></a>Szablonów aplikacji JEDNOSTRONICOWYCH

Pojedynczy szablony projektu strona aplikacji (SPA) szablonów Angular, Aurelia, struktura Knockout.js, React.js i React.js z kontenera Redux są dostępne. Platformy Angular szablon został uaktualniony do Angular 4. Szablony usługi Angular i języka React domyślnie są dostępne; Aby dowiedzieć się, jak uzyskać innych szablonów, zobacz [Utwórz nowy projekt SPA](xref:client-side/spa-services#create-a-new-project). Aby uzyskać informacje dotyczące sposobu tworzenia SPA w programie ASP.NET Core, zobacz <xref:client-side/spa-services>.

## <a name="kestrel-improvements"></a>Ulepszenia kestrel

Serwer sieci web Kestrel ma nowe funkcje, które lepiej przystosować na serwerze dostępnym z Internetu. Liczba opcji konfiguracji ograniczenia serwera zostały dodane w `KestrelServerOptions` klasy przez nowe `Limits` właściwości. Dodaj następujące limity:

* Maksymalna liczba połączeń klientów
* Rozmiar treści żądania maksymalna
* Szybkość danych treści żądania minimalne

Aby uzyskać więcej informacji, zobacz [implementacji serwera sieci web Kestrel w programie ASP.NET Core](xref:fundamentals/servers/kestrel).

## <a name="weblistener-renamed-to-httpsys"></a>Zmieniono nazwę pliku HTTP.sys WebListener

Pakiety `Microsoft.AspNetCore.Server.WebListener` i `Microsoft.Net.Http.Server` została scalona nowy pakiet `Microsoft.AspNetCore.Server.HttpSys`. Przestrzenie nazw zostały zaktualizowane do dopasowania.

Aby uzyskać więcej informacji, zobacz [implementacji serwera sieci web HTTP.sys, w programie ASP.NET Core](xref:fundamentals/servers/httpsys).

## <a name="enhanced-http-header-support"></a>Ulepszona obsługa nagłówków HTTP

Podczas przesyłania przy użyciu platformy MVC `FileStreamResult` lub `FileContentResult`, masz teraz możliwość ustawienia `ETag` lub `LastModified` daty dla zawartości, przesyłania. Można ustawić następujące wartości w zwracanej zawartości przy użyciu kodu podobne do następujących:

```csharp
var data = Encoding.UTF8.GetBytes("This is a sample text from a binary array");
var entityTag = new EntityTagHeaderValue("\"MyCalculatedEtagValue\"");
return File(data, "text/plain", "downloadName.txt", lastModified: DateTime.UtcNow.AddSeconds(-5), entityTag: entityTag);
```

Pliku zwrócone do odwiedzających będzie posiadać odpowiednie nagłówki HTTP dla `ETag` i `LastModified` wartości.

Jeśli aplikacja obiektu odwiedzającego zażąda zawartości z nagłówkiem żądania zakresu, ASP.NET Core rozpoznaje żądania i obsługuje nagłówka. Żądanej zawartości mogą być dostarczane częściowo, ASP.NET Core odpowiednio pomija i zwraca tylko żądany zestaw bajtów. Nie musisz tworzyć żadnych specjalnych obsługi do Twojej metody, aby dostosować lub obsługi tej funkcji; odbywa się automatycznie dla Ciebie.

## <a name="hosting-startup-and-application-insights"></a>Uruchamianie hostingu i usługi Application Insights

Środowisk hostingowych teraz wstrzykiwanie zależności dodatkowych pakietów i wykonanie kodu podczas uruchamiania aplikacji bez zastosowania konieczności jawnego wiedziały lub wywołanie dowolnej metody. Tę funkcję można włączyć niektórych środowisk "światła w górę" funkcji unikatowych dla danego środowiska bez aplikacji musi wiedzieć z wyprzedzeniem. 

W programie ASP.NET Core 2.0, ta funkcja jest używana, można automatycznie włączyć diagnostykę usługi Application Insights podczas debugowania w programie Visual Studio i (po zgodzie na rozwiązanie) podczas uruchamiania w usłudze Azure App Services. W rezultacie szablony projektów nie jest już dodać pakiety usługi Application Insights i kod domyślnie.

Aby uzyskać informacje o stanie planowanej dokumentacji, zobacz [problem w usłudze GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/3389).

## <a name="automatic-use-of-anti-forgery-tokens"></a>Automatyczne użycie tokeny zabezpieczające przed fałszerstwem

Platforma ASP.NET Core zawsze pomogła kodowanie HTML zawartości domyślnie, ale z nową wersją dodatkowego kroku jest podjęte w celu pomocy atakami fałszerstwo żądania międzywitrynowego (XSRF). Platforma ASP.NET Core teraz emitować tokeny zabezpieczające przed fałszerstwem domyślnie i sprawdzić ich poprawność akcje POST formularza i stron bez dodatkowej konfiguracji.

Aby uzyskać więcej informacji, zobacz [zapobiec Cross-Site Request Forgery (XSRF/CSRF) ataki](xref:security/anti-request-forgery).

## <a name="automatic-precompilation"></a>Automatyczne wstępnej kompilacji

Wstępna kompilacja widoku razor jest domyślnie podczas publikowania, zmniejszając rozmiar danych wyjściowych publikowania i aplikacji czas uruchamiania.

Aby uzyskać więcej informacji, zobacz [kompilacja widoku Razor i wstępnej kompilacji w programie ASP.NET Core](xref:mvc/views/view-compilation).

## <a name="razor-support-for-c-71"></a>Obsługa razor dla języka C# 7.1

Aparat widoku Razor został zaktualizowany do pracy z nowy kompilator Roslyn. Obejmuje to obsługę funkcji języka C# 7.1 jak domyślne wyrażeń, wywnioskowane nazwy krotki i dopasowania do wzorca za pomocą typów ogólnych. Aby korzystać z języka C# 7.1 w projekcie, dodaj następującą właściwość w pliku projektu, a następnie załaduj ponownie rozwiązanie:

```xml
<LangVersion>latest</LangVersion>
```

Aby uzyskać informacje o stanie funkcji języka C# 7.1 zobacz [repozytorium GitHub platformy Roslyn](https://github.com/dotnet/roslyn/blob/master/docs/Language%20Feature%20Status.md).

## <a name="other-documentation-updates-for-20"></a>Inne aktualizacje dokumentacji dla wersji 2.0

* [Program Visual Studio publikowania profile na potrzeby wdrażania aplikacji platformy ASP.NET Core](xref:host-and-deploy/visual-studio-publish-profiles)
* [Zarządzanie kluczami](xref:security/data-protection/implementation/key-management)
* [Konfigurowanie uwierzytelniania serwisu Facebook](xref:security/authentication/facebook-logins)
* [Konfigurowanie uwierzytelniania usługi Twitter](xref:security/authentication/twitter-logins)
* [Konfigurowanie uwierzytelniania serwisu Google](xref:security/authentication/google-logins)
* [Konfigurowanie uwierzytelniania Account firmy Microsoft](xref:security/authentication/microsoft-logins)

## <a name="migration-guidance"></a>Wskazówki dotyczące migracji

Aby uzyskać wskazówki dotyczące sposobu przeprowadzenia migracji platformy ASP.NET Core 1.x aplikacji ASP.NET Core 2.0 zobacz następujące zasoby:

* [Migracja z programu ASP.NET Core 1.x do platformy ASP.NET Core 2.0](xref:migration/1x-to-2x/index)
* [Migrowanie do platformy ASP.NET Core 2.0 uwierzytelniania i tożsamości](xref:migration/1x-to-2x/identity-2x)

## <a name="additional-information"></a>Dodatkowe informacje

Aby uzyskać pełną listę zmian, zobacz [platformy ASP.NET Core w wersji 2.0 — informacje o wersji](https://github.com/aspnet/Home/releases/tag/2.0.0).

Aby połączyć się z postępach i planach zespołu projektowego ASP.NET Core, obejrzyj Konferencję [ASP.NET Community Standup](https://live.asp.net/).
