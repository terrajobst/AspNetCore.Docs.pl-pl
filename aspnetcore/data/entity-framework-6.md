---
title: Wprowadzenie do korzystania z programu Entity Framework 6 i ASP.NET Core
author: tdykstra
description: "W tym artykule przedstawiono sposób użycia Entity Framework 6 w aplikacji platformy ASP.NET Core."
ms.author: tdykstra
manager: wpickett
ms.date: 02/24/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/entity-framework-6
ms.openlocfilehash: 51445b8c110ad618aeb680148ccf4304a45ee16e
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="getting-started-with-aspnet-core-and-entity-framework-6"></a>Wprowadzenie do platformy ASP.NET Core oraz Entity Framework 6

Przez [Paweł Grudzień](https://github.com/pgrudzien12), [Damien Pontifex](https://github.com/DamienPontifex), i [Dykstra niestandardowy](https://github.com/tdykstra)

W tym artykule przedstawiono sposób użycia Entity Framework 6 w aplikacji platformy ASP.NET Core.

## <a name="overview"></a>Omówienie

Aby korzystać z programu Entity Framework 6, projekt ma Kompiluj dla środowiska .NET Framework, ponieważ Entity Framework 6 nie obsługuje platformy .NET Core. Jeśli potrzebujesz funkcji i platform musisz uaktualnić do [Entity Framework Core](https://docs.microsoft.com/ef/).

Zalecanym sposobem użycia Entity Framework 6 w aplikacji platformy ASP.NET Core jest umieszczenie kontekstu EF6 i klasy modelu w bibliotece klas projektu którego element docelowy pełna platformy. Dodaj odwołanie do biblioteki klas z projektu platformy ASP.NET Core. Zobacz przykład [rozwiązania Visual Studio z projektami EF6 i ASP.NET Core](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/).

Nie można ustawić kontekstu EF6 w projekcie platformy ASP.NET Core ponieważ projekty .NET Core nie obsługuje wszystkich funkcji, które EF6 polecenia takie jak *Enable-Migrations* wymagają.

Niezależnie od typu projektu, w którym możesz znaleźć kontekst EF6 tylko narzędzia wiersza polecenia EF6 pracować z kontekstem EF6. Na przykład `Scaffold-DbContext` jest dostępna tylko w Entity Framework Core. Jeśli potrzebujesz odtwarzanie bazy danych do modelu EF6, zobacz [Code First istniejącą bazę danych](https://msdn.microsoft.com/jj200620).

## <a name="reference-full-framework-and-ef6-in-the-aspnet-core-project"></a>Odwołanie pełna platformy i EF6 w projekcie platformy ASP.NET Core

Projekt platformy ASP.NET Core musi odwoływać się do środowiska .NET framework i EF6. Na przykład *.csproj* pliku projektu platformy ASP.NET Core będzie wyglądać podobnie do poniższego przykładu (wyświetlane są tylko odpowiednich części pliku).

[!code-xml[](entity-framework-6/sample/MVCCore/MVCCore.csproj?range=3-9&highlight=2)]

Jeśli tworzysz nowy projekt, użyj **aplikacji sieci Web platformy ASP.NET Core (.NET Framework)** szablonu.

## <a name="handle-connection-strings"></a>Obsługa parametrów połączenia

Narzędzia wiersza polecenia EF6, które będą używane w projektu biblioteki klas EF6 wymaga konstruktora domyślnego, dzięki czemu można ich wystąpienia kontekstu. Ale prawdopodobnie należy określić parametry połączenia do użycia w projekcie platformy ASP.NET Core, w którym to przypadku konstruktora z kontekstu musi mieć parametr, który umożliwia przekazywanie w parametrach połączenia. Oto przykład.

[!code-csharp[](entity-framework-6/sample/EF6/SchoolContext.cs?name=snippet_Constructor)]

Ponieważ kontekst EF6 nie ma konstruktora bez parametrów, projekt EF6 musi zapewniać implementację elementu [interfejsu IDbContextFactory](https://msdn.microsoft.com/library/hh506876). Narzędzia wiersza polecenia EF6 znajdzie i używać tej implementacji, dlatego można utworzyć wystąpienie kontekstu. Oto przykład.

[!code-csharp[](entity-framework-6/sample/EF6/SchoolContextFactory.cs?name=snippet_IDbContextFactory)]

W tym przykładowym kodzie `IDbContextFactory` implementacji przekazuje w ciągu połączenia ustalony. Jest to ciąg połączenia, który będzie używać narzędzi wiersza polecenia. Należy zaimplementować strategię, aby upewnić się, że biblioteka klas używa te same parametry połączenia, który korzysta z aplikacji wywołującej. Na przykład można pobrać wartości ze zmiennej środowiskowej w obu tych projektów.

## <a name="set-up-dependency-injection-in-the-aspnet-core-project"></a>Konfigurowanie iniekcji zależności w projekcie platformy ASP.NET Core

W projekcie Core *Startup.cs* pliku skonfigurowane w kontekście EF6 iniekcji zależności (Podpisane) w `ConfigureServices`. Obiektów kontekstu EF ma być ograniczone do istnienia danego żądania.

[!code-csharp[](entity-framework-6/sample/MVCCore/Startup.cs?name=snippet_ConfigureServices&highlight=5)]

Następnie można pobrać wystąpienia kontekstu w kontrolerach przy użyciu Podpisane. Kod jest podobny do piszesz kontekstu EF rdzeni:

[!code-csharp[](entity-framework-6/sample/MVCCore/Controllers/StudentsController.cs?name=snippet_ContextInController)]

## <a name="sample-application"></a>Przykładowa aplikacja

Przykładową aplikację pracy, zobacz [przykładowe rozwiązanie Visual Studio](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/) dołączony w tym artykule.

W tym przykładzie można utworzyć od podstaw przez następujące czynności w programie Visual Studio:

* Tworzenie rozwiązania.

* **Dodaj nowy projekt > sieci Web > Aplikacja sieci Web platformy ASP.NET Core (.NET Framework)**

* **Dodaj nowy projekt > klasycznego pulpitu systemu Windows > klasy biblioteki (.NET Framework)**

* W **Konsola Menedżera pakietów** (PMC) dla obu projektów, uruchom polecenie `Install-Package Entityframework`.

* W projektu biblioteki klas, utworzyć klasy modelu danych i klasę kontekstu i implementacja `IDbContextFactory`.

* W PMC dla projektu biblioteki klas, Uruchom polecenia `Enable-Migrations` i `Add-Migration Initial`. Jeśli ustawisz projekt platformy ASP.NET Core jako projekt startowy, Dodaj `-StartupProjectName EF6` do tych poleceń.

* W projekcie Core Dodaj odwołanie projektu do projektu biblioteki klas.

* W projekcie Core w *Startup.cs*, zarejestruj się w kontekście celu Podpisane.

* W projekcie Core w *appsettings.json*, Dodaj parametry połączenia.

* W projekcie Core Dodaj kontroler i widoków, aby sprawdzić, czy można odczytywania i zapisywania danych. (Należy pamiętać, że ASP.NET MVC podstawowych szkieletów nie będzie działać w kontekście EF6 odwołanie z biblioteki klas.)

## <a name="summary"></a>Podsumowanie

W tym artykule udostępnił podstawowe wskazówki dotyczące korzystania z programu Entity Framework 6 w aplikacji platformy ASP.NET Core.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Entity Framework - konfiguracji opartej na kodzie](https://msdn.microsoft.com/data/jj680699.aspx)
