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
# <a name="migrate-configuration-to-aspnet-core"></a><span data-ttu-id="922b1-103">Migrowanie konfiguracji do platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="922b1-103">Migrate configuration to ASP.NET Core</span></span>

<span data-ttu-id="922b1-104">Przez [Steve Smith](https://ardalis.com/) i [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="922b1-104">By [Steve Smith](https://ardalis.com/) and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="922b1-105">W poprzednim artykule, firma Microsoft rozpoczęła [migracji projektu MVC programu ASP.NET do ASP.NET Core MVC](xref:migration/mvc).</span><span class="sxs-lookup"><span data-stu-id="922b1-105">In the previous article, we began [migrate an ASP.NET MVC project to ASP.NET Core MVC](xref:migration/mvc).</span></span> <span data-ttu-id="922b1-106">W tym artykule migracji konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="922b1-106">In this article, we migrate configuration.</span></span>

<span data-ttu-id="922b1-107">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/configuration/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="922b1-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/configuration/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="setup-configuration"></a><span data-ttu-id="922b1-108">Konfiguracja instalacji</span><span class="sxs-lookup"><span data-stu-id="922b1-108">Setup configuration</span></span>

<span data-ttu-id="922b1-109">Platforma ASP.NET Core nie używa już *Global.asax* i *web.config* pliki, które są wykorzystywane w poprzednich wersjach programu ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="922b1-109">ASP.NET Core no longer uses the *Global.asax* and *web.config* files that previous versions of ASP.NET utilized.</span></span> <span data-ttu-id="922b1-110">We wcześniejszych wersjach programu ASP.NET, Logika uruchamiania aplikacji zostało umieszczone w `Application_StartUp` metodę w ramach *Global.asax*.</span><span class="sxs-lookup"><span data-stu-id="922b1-110">In earlier versions of ASP.NET, application startup logic was placed in an `Application_StartUp` method within *Global.asax*.</span></span> <span data-ttu-id="922b1-111">W dalszej części, ASP.NET MVC *Startup.cs* plik został uwzględniony w katalogu głównym projektu; i została wywołana po uruchomieniu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="922b1-111">Later, in ASP.NET MVC, a *Startup.cs* file was included in the root of the project; and, it was called when the application started.</span></span> <span data-ttu-id="922b1-112">Platforma ASP.NET Core przyjmuje takie podejście całkowicie, umieszczając całą logikę uruchamiania w *Startup.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="922b1-112">ASP.NET Core has adopted this approach completely by placing all startup logic in the *Startup.cs* file.</span></span>

<span data-ttu-id="922b1-113">*Web.config* plik również został zastąpiony w programie ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="922b1-113">The *web.config* file has also been replaced in ASP.NET Core.</span></span> <span data-ttu-id="922b1-114">Konfiguracja samego można teraz skonfigurować, w ramach procedury uruchomienia aplikacji, które są opisane w *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="922b1-114">Configuration itself can now be configured, as part of the application startup procedure described in *Startup.cs*.</span></span> <span data-ttu-id="922b1-115">Konfiguracja, mogą nadal korzystać z plików XML, ale zazwyczaj projektów ASP.NET Core spowoduje umieszczenie wartości konfiguracji w pliku w formacie JSON, takich jak *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="922b1-115">Configuration can still utilize XML files, but typically ASP.NET Core projects will place configuration values in a JSON-formatted file, such as *appsettings.json*.</span></span> <span data-ttu-id="922b1-116">Systemu konfiguracji platformy ASP.NET Core mają także dostęp do zmiennych środowiskowych, które może zapewnić [bardziej bezpieczne i niezawodne lokalizacji](xref:security/app-secrets) wartości specyficznych dla środowiska.</span><span class="sxs-lookup"><span data-stu-id="922b1-116">ASP.NET Core's configuration system can also easily access environment variables, which can provide a [more secure and robust location](xref:security/app-secrets) for environment-specific values.</span></span> <span data-ttu-id="922b1-117">Jest to szczególnie istotne dla wpisów tajnych, takich jak parametry połączenia i klucze interfejsu API, które nie powinny zostać sprawdzone w kontroli źródła.</span><span class="sxs-lookup"><span data-stu-id="922b1-117">This is especially true for secrets like connection strings and API keys that shouldn't be checked into source control.</span></span> <span data-ttu-id="922b1-118">Zobacz [konfiguracji](xref:fundamentals/configuration/index) Aby dowiedzieć się więcej na temat konfiguracji w programie ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="922b1-118">See [Configuration](xref:fundamentals/configuration/index) to learn more about configuration in ASP.NET Core.</span></span>

<span data-ttu-id="922b1-119">W tym artykule rozpoczynamy z częściowo migrowanych projektem platformy ASP.NET Core z [poprzednim artykule](xref:migration/mvc).</span><span class="sxs-lookup"><span data-stu-id="922b1-119">For this article, we are starting with the partially migrated ASP.NET Core project from [the previous article](xref:migration/mvc).</span></span> <span data-ttu-id="922b1-120">Ustawienia konfiguracji, Dodaj następujący Konstruktor oraz właściwości *Startup.cs* plik znajdujący się w folderze głównym projektu:</span><span class="sxs-lookup"><span data-stu-id="922b1-120">To setup configuration, add the following constructor and property to the *Startup.cs* file located in the root of the project:</span></span>

[!code-csharp[](configuration/samples/WebApp1/src/WebApp1/Startup.cs?range=11-16)]

<span data-ttu-id="922b1-121">Uwaga, który w tym momencie *Startup.cs* pliku nie będzie skompilować, jak nadal należy dodać następujące `using` instrukcji:</span><span class="sxs-lookup"><span data-stu-id="922b1-121">Note that at this point, the *Startup.cs* file won't compile, as we still need to add the following `using` statement:</span></span>

```csharp
using Microsoft.Extensions.Configuration;
```

<span data-ttu-id="922b1-122">Dodaj *appsettings.json* pliku w folderze głównym projektu przy użyciu szablonu odpowiedni element:</span><span class="sxs-lookup"><span data-stu-id="922b1-122">Add an *appsettings.json* file to the root of the project using the appropriate item template:</span></span>

![Dodaj AppSettings JSON](configuration/_static/add-appsettings-json.png)

## <a name="migrate-configuration-settings-from-webconfig"></a><span data-ttu-id="922b1-124">Przeprowadzić migrację ustawień konfiguracji z pliku web.config</span><span class="sxs-lookup"><span data-stu-id="922b1-124">Migrate configuration settings from web.config</span></span>

<span data-ttu-id="922b1-125">Nasz projekt składnika ASP.NET MVC uwzględnione wymagane parametry połączenia w *web.config*w `<connectionStrings>` elementu.</span><span class="sxs-lookup"><span data-stu-id="922b1-125">Our ASP.NET MVC project included the required database connection string in *web.config*, in the `<connectionStrings>` element.</span></span> <span data-ttu-id="922b1-126">W projekcie platformy ASP.NET Core, użyjemy do przechowywania tych informacji w *appsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="922b1-126">In our ASP.NET Core project, we are going to store this information in the *appsettings.json* file.</span></span> <span data-ttu-id="922b1-127">Otwórz *appsettings.json*i zwróć uwagę, że już obejmuje następujące elementy:</span><span class="sxs-lookup"><span data-stu-id="922b1-127">Open *appsettings.json*, and note that it already includes the following:</span></span>

[!code-json[](../migration/configuration/samples/WebApp1/src/WebApp1/appsettings.json?highlight=4)]

<span data-ttu-id="922b1-128">W wyróżniony wiersz przedstawione powyżej, Zmień nazwę bazy danych z **_CHANGE_ME** nazwę bazy danych.</span><span class="sxs-lookup"><span data-stu-id="922b1-128">In the highlighted line depicted above, change the name of the database from **_CHANGE_ME** to the name of your database.</span></span>

## <a name="summary"></a><span data-ttu-id="922b1-129">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="922b1-129">Summary</span></span>

<span data-ttu-id="922b1-130">Platforma ASP.NET Core umieszcza całą logikę uruchamiania aplikacji w jednym pliku, w którym wymagane usługi i zależności można definiować i skonfigurowane.</span><span class="sxs-lookup"><span data-stu-id="922b1-130">ASP.NET Core places all startup logic for the application in a single file, in which the necessary services and dependencies can be defined and configured.</span></span> <span data-ttu-id="922b1-131">Zastępuje on programy *web.config* pliku z funkcją Elastyczna konfiguracja, która może korzystać z wielu różnych formatów plików, takich jak JSON, a także zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="922b1-131">It replaces the *web.config* file with a flexible configuration feature that can leverage a variety of file formats, such as JSON, as well as environment variables.</span></span>
