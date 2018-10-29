---
title: Migrowanie konfiguracji do platformy ASP.NET Core
author: ardalis
description: Dowiedz się, jak przeprowadzić migrację konfiguracji z projektu programu ASP.NET MVC do projektu programu ASP.NET Core MVC.
ms.author: riande
ms.date: 10/14/2016
uid: migration/configuration
ms.openlocfilehash: 5a1c4d0cbbdf74a00073c654e78a05f44948caae
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/29/2018
ms.locfileid: "50205915"
---
# <a name="migrate-configuration-to-aspnet-core"></a>Migrowanie konfiguracji do platformy ASP.NET Core

Przez [Steve Smith](https://ardalis.com/) i [Scott Addie](https://scottaddie.com)

W poprzednim artykule, firma Microsoft rozpoczęła [migracji projektu MVC programu ASP.NET do ASP.NET Core MVC](xref:migration/mvc). W tym artykule migracji konfiguracji.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/configuration/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))

## <a name="setup-configuration"></a>Konfiguracja instalacji

Platforma ASP.NET Core nie używa już *Global.asax* i *web.config* pliki, które są wykorzystywane w poprzednich wersjach programu ASP.NET. We wcześniejszych wersjach programu ASP.NET, Logika uruchamiania aplikacji zostało umieszczone w `Application_StartUp` metodę w ramach *Global.asax*. W dalszej części, ASP.NET MVC *Startup.cs* plik został uwzględniony w katalogu głównym projektu; i została wywołana po uruchomieniu aplikacji. Platforma ASP.NET Core przyjmuje takie podejście całkowicie, umieszczając całą logikę uruchamiania w *Startup.cs* pliku.

*Web.config* plik również został zastąpiony w programie ASP.NET Core. Konfiguracja samego można teraz skonfigurować, w ramach procedury uruchomienia aplikacji, które są opisane w *Startup.cs*. Konfiguracja, mogą nadal korzystać z plików XML, ale zazwyczaj projektów ASP.NET Core spowoduje umieszczenie wartości konfiguracji w pliku w formacie JSON, takich jak *appsettings.json*. Systemu konfiguracji platformy ASP.NET Core mają także dostęp do zmiennych środowiskowych, które może zapewnić [bardziej bezpieczne i niezawodne lokalizacji](xref:security/app-secrets) wartości specyficznych dla środowiska. Jest to szczególnie istotne dla wpisów tajnych, takich jak parametry połączenia i klucze interfejsu API, które nie powinny zostać sprawdzone w kontroli źródła. Zobacz [konfiguracji](xref:fundamentals/configuration/index) Aby dowiedzieć się więcej na temat konfiguracji w programie ASP.NET Core.

W tym artykule rozpoczynamy z częściowo migrowanych projektem platformy ASP.NET Core z [poprzednim artykule](xref:migration/mvc). Ustawienia konfiguracji, Dodaj następujący Konstruktor oraz właściwości *Startup.cs* plik znajdujący się w folderze głównym projektu:

[!code-csharp[](configuration/samples/WebApp1/src/WebApp1/Startup.cs?range=11-16)]

Uwaga, który w tym momencie *Startup.cs* pliku nie będzie skompilować, jak nadal należy dodać następujące `using` instrukcji:

```csharp
using Microsoft.Extensions.Configuration;
```

Dodaj *appsettings.json* pliku w folderze głównym projektu przy użyciu szablonu odpowiedni element:

![Dodaj AppSettings JSON](configuration/_static/add-appsettings-json.png)

## <a name="migrate-configuration-settings-from-webconfig"></a>Przeprowadzić migrację ustawień konfiguracji z pliku web.config

Nasz projekt składnika ASP.NET MVC uwzględnione wymagane parametry połączenia w *web.config*w `<connectionStrings>` elementu. W projekcie platformy ASP.NET Core, użyjemy do przechowywania tych informacji w *appsettings.json* pliku. Otwórz *appsettings.json*i zwróć uwagę, że już obejmuje następujące elementy:

[!code-json[](../migration/configuration/samples/WebApp1/src/WebApp1/appsettings.json?highlight=4)]

W wyróżniony wiersz przedstawione powyżej, Zmień nazwę bazy danych z **_CHANGE_ME** nazwę bazy danych.

## <a name="summary"></a>Podsumowanie

Platforma ASP.NET Core umieszcza całą logikę uruchamiania aplikacji w jednym pliku, w którym wymagane usługi i zależności można definiować i skonfigurowane. Zastępuje on programy *web.config* pliku z funkcją Elastyczna konfiguracja, która może korzystać z wielu różnych formatów plików, takich jak JSON, a także zmienne środowiskowe.
