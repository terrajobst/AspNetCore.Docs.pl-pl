---
title: Migrowanie konfiguracji do platformy ASP.NET Core
author: ardalis
description: "Dowiedz się, jak przeprowadzić migrację konfiguracji z projektu programu ASP.NET MVC do projektu programu ASP.NET Core MVC."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/configuration
ms.openlocfilehash: 6c72b324de49a03a3b2c4e96ba8886d1ed249103
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/15/2018
---
# <a name="migrating-configuration-to-aspnet-core"></a><span data-ttu-id="0594e-103">Migrowanie konfiguracji do platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0594e-103">Migrating Configuration to ASP.NET Core</span></span>

<span data-ttu-id="0594e-104">Przez [Steve Smith](https://ardalis.com/) i [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="0594e-104">By [Steve Smith](https://ardalis.com/) and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="0594e-105">W poprzednim artykule, możemy rozpoczęcia [migracji projektu programu ASP.NET MVC do platformy ASP.NET Core MVC](mvc.md).</span><span class="sxs-lookup"><span data-stu-id="0594e-105">In the previous article, we began [migrating an ASP.NET MVC project to ASP.NET Core MVC](mvc.md).</span></span> <span data-ttu-id="0594e-106">W tym artykule firma Microsoft Migruj konfigurację.</span><span class="sxs-lookup"><span data-stu-id="0594e-106">In this article, we migrate configuration.</span></span>

<span data-ttu-id="0594e-107">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/configuration/samples) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0594e-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/configuration/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="setup-configuration"></a><span data-ttu-id="0594e-108">W konfiguracji</span><span class="sxs-lookup"><span data-stu-id="0594e-108">Setup Configuration</span></span>

<span data-ttu-id="0594e-109">Już korzysta z platformy ASP.NET Core *Global.asax* i *web.config* pliki, które są używane w poprzednich wersjach programu ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="0594e-109">ASP.NET Core no longer uses the *Global.asax* and *web.config* files that previous versions of ASP.NET utilized.</span></span> <span data-ttu-id="0594e-110">We wcześniejszych wersjach programu ASP.NET, Logika uruchamiania aplikacji została umieszczona w `Application_StartUp` metodę *Global.asax*.</span><span class="sxs-lookup"><span data-stu-id="0594e-110">In earlier versions of ASP.NET, application startup logic was placed in an `Application_StartUp` method within *Global.asax*.</span></span> <span data-ttu-id="0594e-111">Później, w programie ASP.NET MVC *Startup.cs* pliku została uwzględniona w katalogu głównym projektu; i została wywołana po uruchomieniu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0594e-111">Later, in ASP.NET MVC, a *Startup.cs* file was included in the root of the project; and, it was called when the application started.</span></span> <span data-ttu-id="0594e-112">Platformy ASP.NET Core przyjmuje takie podejście całkowicie przez umieszczenie wszystkich Logika uruchamiania w *Startup.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="0594e-112">ASP.NET Core has adopted this approach completely by placing all startup logic in the *Startup.cs* file.</span></span>

<span data-ttu-id="0594e-113">*Web.config* plik również został zastąpiony w ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0594e-113">The *web.config* file has also been replaced in ASP.NET Core.</span></span> <span data-ttu-id="0594e-114">Konfiguracja samego można teraz skonfigurować, w ramach procedury uruchomienia aplikacji, które są opisane w sekcji *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="0594e-114">Configuration itself can now be configured, as part of the application startup procedure described in *Startup.cs*.</span></span> <span data-ttu-id="0594e-115">Konfiguracja nadal mogą korzystać z plików XML, ale zazwyczaj projektów platformy ASP.NET Core umieści wartości konfiguracji w formacie JSON pliku, takich jak *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="0594e-115">Configuration can still utilize XML files, but typically ASP.NET Core projects will place configuration values in a JSON-formatted file, such as *appsettings.json*.</span></span> <span data-ttu-id="0594e-116">System konfiguracji platformy ASP.NET Core można również łatwo uzyskiwać dostęp do zmiennych środowiskowych, które zapewniają [bardziej bezpieczne i niezawodne lokalizacji](xref:security/app-secrets) wartości określonego środowiska.</span><span class="sxs-lookup"><span data-stu-id="0594e-116">ASP.NET Core's configuration system can also easily access environment variables, which can provide a [more secure and robust location](xref:security/app-secrets) for environment-specific values.</span></span> <span data-ttu-id="0594e-117">Jest to szczególnie istotne dla kluczy tajnych, takich jak parametry połączenia i klucze interfejsu API, które nie powinny być wyewidencjonowany do kontroli źródła.</span><span class="sxs-lookup"><span data-stu-id="0594e-117">This is especially true for secrets like connection strings and API keys that shouldn't be checked into source control.</span></span> <span data-ttu-id="0594e-118">Zobacz [konfiguracji](xref:fundamentals/configuration/index) Aby dowiedzieć się więcej na temat konfiguracji w programie ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0594e-118">See [Configuration](xref:fundamentals/configuration/index) to learn more about configuration in ASP.NET Core.</span></span>

<span data-ttu-id="0594e-119">W tym artykule, zostanie użyty z projektem platformy ASP.NET Core częściowo migrowane z [poprzednim artykule](mvc.md).</span><span class="sxs-lookup"><span data-stu-id="0594e-119">For this article, we are starting with the partially-migrated ASP.NET Core project from [the previous article](mvc.md).</span></span> <span data-ttu-id="0594e-120">Ustawienia konfiguracji, Dodaj następujący Konstruktor i właściwości *Startup.cs* plik znajdujący się w katalogu głównym projektu:</span><span class="sxs-lookup"><span data-stu-id="0594e-120">To setup configuration, add the following constructor and property to the *Startup.cs* file located in the root of the project:</span></span>

[!code-csharp[](configuration/samples/WebApp1/src/WebApp1/Startup.cs?range=11-21)]

<span data-ttu-id="0594e-121">Uwaga to w tym momencie *Startup.cs* pliku nie będzie skompilować, jak nadal trzeba dodać następujące `using` instrukcji:</span><span class="sxs-lookup"><span data-stu-id="0594e-121">Note that at this point, the *Startup.cs* file won't compile, as we still need to add the following `using` statement:</span></span>

```csharp
using Microsoft.Extensions.Configuration;
```

<span data-ttu-id="0594e-122">Dodaj *appsettings.json* plik do katalogu głównego projektu przy użyciu szablonu odpowiedni element:</span><span class="sxs-lookup"><span data-stu-id="0594e-122">Add an *appsettings.json* file to the root of the project using the appropriate item template:</span></span>

![Dodaj AppSettings JSON](configuration/_static/add-appsettings-json.png)

## <a name="migrate-configuration-settings-from-webconfig"></a><span data-ttu-id="0594e-124">Migracja ustawień konfiguracji z pliku web.config</span><span class="sxs-lookup"><span data-stu-id="0594e-124">Migrate Configuration Settings from web.config</span></span>

<span data-ttu-id="0594e-125">Nasze projektu programu ASP.NET MVC zawarte w ciągu połączenia bazy danych wymagane *web.config*w `<connectionStrings>` elementu.</span><span class="sxs-lookup"><span data-stu-id="0594e-125">Our ASP.NET MVC project included the required database connection string in *web.config*, in the `<connectionStrings>` element.</span></span> <span data-ttu-id="0594e-126">W naszym projektem platformy ASP.NET Core zamierzamy przechowywać tych informacji w *appsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="0594e-126">In our ASP.NET Core project, we are going to store this information in the *appsettings.json* file.</span></span> <span data-ttu-id="0594e-127">Otwórz *appsettings.json*i należy pamiętać, że już obejmuje następujące elementy:</span><span class="sxs-lookup"><span data-stu-id="0594e-127">Open *appsettings.json*, and note that it already includes the following:</span></span>

[!code-json[](../migration/configuration/samples/WebApp1/src/WebApp1/appsettings.json?highlight=4)]


<span data-ttu-id="0594e-128">W wyróżnionych wiersza przedstawione powyżej, Zmień nazwę bazy danych z **_CHANGE_ME** do nazwy bazy danych.</span><span class="sxs-lookup"><span data-stu-id="0594e-128">In the highlighted line depicted above, change the name of the database from **_CHANGE_ME** to the name of your database.</span></span>

## <a name="summary"></a><span data-ttu-id="0594e-129">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="0594e-129">Summary</span></span>

<span data-ttu-id="0594e-130">Platformy ASP.NET Core umieszcza wszystkie Logika uruchamiania aplikacji w jednym pliku, w którym wymagane usługi i zależności można zdefiniowane i skonfigurowane.</span><span class="sxs-lookup"><span data-stu-id="0594e-130">ASP.NET Core places all startup logic for the application in a single file, in which the necessary services and dependencies can be defined and configured.</span></span> <span data-ttu-id="0594e-131">Zastępuje *web.config* plików przy użyciu funkcji elastyczne konfiguracji, które mogą korzystać z szerokiej gamy formatów plików, takich jak JSON, a także zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="0594e-131">It replaces the *web.config* file with a flexible configuration feature that can leverage a variety of file formats, such as JSON, as well as environment variables.</span></span>
