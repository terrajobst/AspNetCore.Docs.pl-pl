---
title: "Rozwiązywanie problemów z platformy ASP.NET Core w usługach IIS"
author: guardrex
description: "Dowiedz się, jak diagnozować problemy z wdrożeniami usług IIS aplikacji platformy ASP.NET Core."
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 03/13/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: host-and-deploy/iis/troubleshoot
ms.openlocfilehash: 2e42d5904e2be8c031c5c6d4bbc104a251b699f2
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/11/2018
---
# <a name="troubleshoot-aspnet-core-on-iis"></a>Rozwiązywanie problemów z platformy ASP.NET Core w usługach IIS

Przez [Luke Latham](https://github.com/guardrex)

Do diagnozowania problemów z wdrożeniami usług IIS:

* Badanie wyników przeglądarki.
* Sprawdź system **aplikacji** Zaloguj się za pośrednictwem **Podgląd zdarzeń**.
* Włącz `stdout` rejestrowania. **Moduł platformy ASP.NET Core** dziennika znajduje się w podanej w ścieżce *stdoutLogFile* atrybutu `<aspNetCore>` element *web.config*. Wszystkie foldery w ścieżce w wartości atrybutu musi istnieć w ramach wdrożenia. Ustaw *stdoutLogEnabled* do `true`. Aplikacje używające `Microsoft.NET.Sdk.Web` zestaw SDK, aby utworzyć *web.config* plik domyślny *stdoutLogEnabled* ustawienie `false`, ręcznie podaj *web.config* pliku lub zmodyfikowania pliku, aby włączyć `stdout` rejestrowania.

Użyj informacji z tych trzech źródeł z [typowe błędy odwołania tematu](xref:host-and-deploy/azure-iis-errors-reference) problemu. Wykonaj zalecenia dotyczące rozwiązywania problemów do rozwiązania problemu.

Niektóre typowe błędy nie są wyświetlane w przeglądarce, dziennik aplikacji i ASP.NET Core modułu dziennika do modułu *startupTimeLimit* (domyślne: 120 sekund) i *startupRetryCount* (domyślne: 2) są spełnione. W związku z tym Zaczekaj pełne 6 minut przed wydedukowania które module nie można uruchomić procesu aplikacji.

Jest szybkim sposobem Ustal, czy aplikacja działa prawidłowo do uruchomienia aplikacji bezpośrednio na Kestrel. Jeśli aplikacja została opublikowana jako [wdrożenia zależne od framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd), wykonać `dotnet <assembly_name>.dll` w folderze wdrożenia, który jest ścieżka fizyczna programu IIS do aplikacji. Jeśli aplikacja została opublikowana jako [niezależne wdrożenia](/dotnet/core/deploying/#self-contained-deployments-scd)Uruchom aplikację do pliku wykonywalnego bezpośrednio z wiersza polecenia, `<assembly_name>.exe`, w folderze wdrożenia. Jeśli Kestrel nasłuchuje na porcie domyślnym 5000, aplikacja powinna być dostępne pod adresem `http://localhost:5000/`. Jeśli aplikacja zazwyczaj odpowiada pod adresem punktu końcowego Kestrel, problem jest bardziej prawdopodobne, związane z konfiguracją zwrotnego serwera proxy i mniej prawdopodobne w aplikacji.

Jednym ze sposobów ustalenia, czy zwrotny serwer proxy działa poprawnie jest wykonanie żądania prostego pliku statycznego do arkusza stylów, skryptów lub obraz z plików statycznych dla aplikacji w *wwwroot* przy użyciu [oprogramowanie pośredniczące plików statycznych](xref:fundamentals/static-files). Jeśli aplikacja może obsługiwać pliki statyczne, ale innych punktów końcowych i widoków MVC kończą się niepowodzeniem, problem jest mniej prawdopodobne związane z konfiguracją zwrotnego serwera proxy i bardziej prawdopodobne w aplikacji (na przykład routingu MVC lub 500 Wewnętrzny błąd serwera).

Gdy Kestrel uruchamia się normalnie za usług IIS, ale aplikacja nie mogą być uruchamiane na komputerze po pomyślnym uruchomieniu lokalnie, wartość zmiennej środowiskowej może tymczasowo dodane do *web.config* można ustawić `ASPNETCORE_ENVIRONMENT` do `Development`. Umożliwia ustawienie zmiennej środowiskowej dopóki środowisko nie została zastąpiona przy uruchamianiu aplikacji, [strona wyjątek dewelopera](xref:fundamentals/error-handling) pojawią się po uruchomieniu aplikacji. Ustawienie zmiennej środowiskowej dla `ASPNETCORE_ENVIRONMENT` w ten sposób jest zalecane tylko dla serwerów przemieszczania/testowania, które nie są dostępne w Internecie. Pamiętaj usunąć zmienną środowiskową z *web.config* pliku po zakończeniu. Aby uzyskać informacje na temat ustawiania zmiennych środowiskowych za pośrednictwem *web.config*, zobacz [environmentVariables elementem podrzędnym aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).

W większości przypadków Włączanie rejestrowania aplikacji pomaga w rozwiązywaniu problemów z aplikacji lub zwrotnego serwera proxy. Zobacz [rejestrowanie](xref:fundamentals/logging/index) Aby uzyskać więcej informacji.

Ostatni porady dotyczące rozwiązywania problemów odnosi się do aplikacji, których nie można uruchomić po uaktualnieniu albo .NET Core SDK w wersjach programowanie maszyny lub pakietu w aplikacji. W niektórych przypadkach niespójne pakiety mogą być dzielone aplikacji podczas przeprowadzania uaktualnienia głównych. Większość te problemy można rozwiązać przez:

* Usuwanie `bin` i `obj` foldery w projekcie.
* Czyszczenie pakietu buforuje na `%UserProfile%\.nuget\packages\` i `%LocalAppData%\Nuget\v3-cache`.
* Przywracanie i ponownie skompilować projekt.
* Potwierdzenie, że poprzedniego wdrożenia na serwerze zostaną całkowicie usunięte przed ponownym wdrażaniem aplikacji.

> [!TIP]
> Jest to wygodny sposób czyszczenia pamięci podręcznych pakietu do wykonania `dotnet nuget locals all --clear` z wiersza polecenia.
> 
> Wyczyszczenie pamięci podręcznej pakiet może być również wykonywane przy użyciu [nuget.exe](https://www.nuget.org/downloads) narzędzie i wykonywania polecenia `nuget locals all -clear`. *nuget.exe* nie jest powiązane instalacji z systemem Windows 10 i trzeba je instalować osobno w witrynie NuGet.
<!--
> [!TIP]
> A convenient way to clear package caches is to:
>
> * Obtain the *NuGet.exe* tool from [NuGet.org](https://www.nuget.org/).
> * Add the path to *NuGet.exe* to the system PATH.
> * Execute `nuget locals all -clear` from a command prompt.
>
> Alternatively, execute `dotnet nuget locals all --clear` from a command prompt without obtaining *NuGet.exe*. -->

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Typowe błędy odwołania dla usługi Azure App Service i IIS z platformy ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference)
* [Konfiguracja modułu Core programu ASP.NET](xref:host-and-deploy/aspnet-core-module)
