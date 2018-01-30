---
title: Migrowanie konfiguracji
author: ardalis
description: 
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/configuration
ms.openlocfilehash: 23b96ad11201f9b82cbd9fb832757d905407d228
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2018
---
# <a name="migrating-configuration"></a>Migrowanie konfiguracji

Przez [Steve Smith](https://ardalis.com/) i [Scott Addie](https://scottaddie.com)

W poprzednim artykule, możemy rozpoczęcia [migracji projektu programu ASP.NET MVC do platformy ASP.NET Core MVC](mvc.md). W tym artykule firma Microsoft Migruj konfigurację.

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/configuration/samples) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

## <a name="setup-configuration"></a>W konfiguracji

Już korzysta z platformy ASP.NET Core *Global.asax* i *web.config* pliki, które są używane w poprzednich wersjach programu ASP.NET. We wcześniejszych wersjach programu ASP.NET, Logika uruchamiania aplikacji została umieszczona w `Application_StartUp` metodę *Global.asax*. Później, w programie ASP.NET MVC *Startup.cs* pliku została uwzględniona w katalogu głównym projektu; i została wywołana po uruchomieniu aplikacji. Platformy ASP.NET Core przyjmuje takie podejście całkowicie przez umieszczenie wszystkich Logika uruchamiania w *Startup.cs* pliku.

*Web.config* plik również został zastąpiony w ASP.NET Core. Konfiguracja samego można teraz skonfigurować, w ramach procedury uruchomienia aplikacji, które są opisane w sekcji *Startup.cs*. Konfiguracja nadal mogą korzystać z plików XML, ale zazwyczaj projektów platformy ASP.NET Core umieści wartości konfiguracji w formacie JSON pliku, takich jak *appsettings.json*. System konfiguracji platformy ASP.NET Core można również łatwo uzyskiwać dostęp do zmiennych środowiskowych, które zapewniają bardziej bezpieczne i niezawodne lokalizacji dla określonego środowiska wartości. Jest to szczególnie istotne dla kluczy tajnych, takich jak parametry połączenia i klucze interfejsu API, które nie powinny być wyewidencjonowany do kontroli źródła. Zobacz [konfiguracji](xref:fundamentals/configuration/index) Aby dowiedzieć się więcej na temat konfiguracji w programie ASP.NET Core.

W tym artykule, zostanie użyty z projektem platformy ASP.NET Core częściowo migrowane z [poprzednim artykule](mvc.md). Ustawienia konfiguracji, Dodaj następujący Konstruktor i właściwości *Startup.cs* plik znajdujący się w katalogu głównym projektu:

[!code-csharp[Main](configuration/samples/WebApp1/src/WebApp1/Startup.cs?range=11-21)]

Uwaga to w tym momencie *Startup.cs* pliku nie będzie skompilować, jak nadal trzeba dodać następujące `using` instrukcji:

```csharp
using Microsoft.Extensions.Configuration;
```

Dodaj *appsettings.json* plik do katalogu głównego projektu przy użyciu szablonu odpowiedni element:

![Dodaj AppSettings JSON](configuration/_static/add-appsettings-json.png)

## <a name="migrate-configuration-settings-from-webconfig"></a>Migracja ustawień konfiguracji z pliku web.config

Nasze projektu programu ASP.NET MVC zawarte w ciągu połączenia bazy danych wymagane *web.config*w `<connectionStrings>` elementu. W naszym projektem platformy ASP.NET Core zamierzamy przechowywać tych informacji w *appsettings.json* pliku. Otwórz *appsettings.json*i należy pamiętać, że już obejmuje następujące elementy:

[!code-json[Main](../migration/configuration/samples/WebApp1/src/WebApp1/appsettings.json?highlight=4)]


W wyróżnionych wiersza przedstawione powyżej, Zmień nazwę bazy danych z **_CHANGE_ME** do nazwy bazy danych.

## <a name="summary"></a>Podsumowanie

Platformy ASP.NET Core umieszcza wszystkie Logika uruchamiania aplikacji w jednym pliku, w którym wymagane usługi i zależności można zdefiniowane i skonfigurowane. Zastępuje *web.config* plików przy użyciu funkcji elastyczne konfiguracji, które mogą korzystać z szerokiej gamy formatów plików, takich jak JSON, a także zmienne środowiskowe.
