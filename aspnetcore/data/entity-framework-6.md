---
title: Rozpoczynanie pracy z platformą ASP.NET Core i Entity Framework 6
author: rick-anderson
description: W tym artykule pokazano, jak używać platformy Entity Framework 6 w aplikacji ASP.NET Core.
ms.author: tdykstra
ms.custom: mvc
ms.date: 10/24/2018
uid: data/entity-framework-6
ms.openlocfilehash: 50f51a72341cbad017872df581da214211b9920a
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64900616"
---
# <a name="get-started-with-aspnet-core-and-entity-framework-6"></a>Rozpoczynanie pracy z platformą ASP.NET Core i Entity Framework 6

Przez [Paweł Grudzień](https://github.com/pgrudzien12), [Damien Pontifex](https://github.com/DamienPontifex), i [Tom Dykstra](https://github.com/tdykstra)

W tym artykule pokazano, jak używać platformy Entity Framework 6 w aplikacji ASP.NET Core.

## <a name="overview"></a>Omówienie

Aby korzystać z platformy Entity Framework 6, projekt ma kompilowanie z użyciem platformy .NET Framework, zgodnie z platformy Entity Framework 6 nie obsługuje platformy .NET Core. Jeśli potrzebujesz funkcji dla wielu platform należy uaktualnić do [Entity Framework Core](/ef/).

Zalecanym sposobem użycia platformy Entity Framework 6 w aplikacji ASP.NET Core jest umieszczenie kontekstu EF6 i klasy modeli w bibliotece klas projektu przeznaczonego pełny framework. Dodaj odwołanie do biblioteki klas z projektów ASP.NET Core. Zobacz przykład [rozwiązania Visual Studio z projektami EF6 i ASP.NET Core](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/entity-framework-6/sample/).

Nie można umieścić uprawnieniami EF6 w projektach programu ASP.NET Core, ponieważ projektów .NET Core nie obsługują wszystkie funkcje, które EF6 polecenia, takie jak *Enable-Migrations* wymagają.

Niezależnie od typu projektu, w którym możesz znaleźć kontekstu EF6 tylko narzędzia wiersza polecenia platformy EF6 pracować z uprawnieniami EF6. Na przykład `Scaffold-DbContext` jest dostępna tylko w programie Entity Framework Core. Jeśli zachodzi potrzeba odtwarzanie bazy danych do modelu EF6, zobacz [Code First istniejącą bazę danych](https://msdn.microsoft.com/jj200620).

## <a name="reference-full-framework-and-ef6-in-the-aspnet-core-project"></a>Dokumentacja pełny framework i EF6 w projekcie platformy ASP.NET Core

Projekt platformy ASP.NET Core musi odwoływać się do środowiska .NET framework i EF6. Na przykład *.csproj* plik projektu programu ASP.NET Core będzie wyglądać podobnie do poniższego przykładu (wyświetlane są tylko odpowiednie części pliku).

[!code-xml[](entity-framework-6/sample/MVCCore/MVCCore.csproj?range=3-9&highlight=2)]

Podczas tworzenia nowego projektu, użyj **aplikacja sieci Web programu ASP.NET Core (.NET Framework)** szablonu.

## <a name="handle-connection-strings"></a>Obsługa parametrów połączenia

EF6 narzędzia wiersza polecenia, których można używać w projekcie biblioteki klas platformy EF6 wymagają domyślnego konstruktora, dzięki czemu można utworzyć wystąpienie kontekstu. Jednak prawdopodobnie będziesz chciał określić parametry połączenia do użycia w projekcie platformy ASP.NET Core, w którym to przypadku konstruktora kontekście musi mieć parametr, który umożliwia przekazywanie parametrów połączenia. Oto przykład.

[!code-csharp[](entity-framework-6/sample/EF6/SchoolContext.cs?name=snippet_Constructor)]

Ponieważ kontekst EF6 nie ma konstruktora bez parametrów, projekt EF6 musi dostarczyć implementację [IDbContextFactory](https://msdn.microsoft.com/library/hh506876). Narzędzia wiersza polecenia platformy EF6 znajdzie i użyć tej implementacji, dzięki czemu można utworzyć wystąpienie kontekstu. Oto przykład.

[!code-csharp[](entity-framework-6/sample/EF6/SchoolContextFactory.cs?name=snippet_IDbContextFactory)]

W tym przykładowym kodzie `IDbContextFactory` implementacji przekazuje ciągów połączeń zakodowanych. Jest to ciąg połączenia, który będzie używać narzędzi wiersza polecenia. Należy zaimplementować strategię, aby upewnić się, że biblioteka klas używa jednych parametrach połączenia, który używa aplikacji wywołującej. Na przykład można uzyskać wartość ze zmiennej środowiskowej w obu projektach.

## <a name="set-up-dependency-injection-in-the-aspnet-core-project"></a>Konfigurowanie wstrzykiwanie zależności w projekcie platformy ASP.NET Core

W projekcie Core *Startup.cs* plików, skonfiguruj kontekst EF6 wstrzykiwanie zależności (DI) w `ConfigureServices`. Obiektów kontekstu EF powinien być ograniczony okres istnienia danego żądania.

[!code-csharp[](entity-framework-6/sample/MVCCore/Startup.cs?name=snippet_ConfigureServices&highlight=5)]

Następnie można pobrać wystąpienia kontekstu w kontrolerach przy użyciu DI. Kod jest podobny do piszesz dla kontekstu programu EF Core:

[!code-csharp[](entity-framework-6/sample/MVCCore/Controllers/StudentsController.cs?name=snippet_ContextInController)]

## <a name="sample-application"></a>Przykładowa aplikacja

Pracy przykładowej aplikacji, zobacz [przykładowe rozwiązanie programu Visual Studio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/entity-framework-6/sample/) dołączony w tym artykule.

W tym przykładzie można tworzyć od podstaw przez następujące kroki w programie Visual Studio:

* Tworzenie rozwiązań.

* **Dodaj** > **nowy projekt** > **Web** > **aplikacji sieci Web platformy ASP.NET Core**
  * W oknie dialogowym wyboru szablonów projektu wybierz na liście rozwijanej interfejsu API i .NET Framework

* **Dodaj** > **nowy projekt** > **pulpitu Windows** > **klasy biblioteki (.NET Framework)**

* W **Konsola Menedżera pakietów** (PMC) dla obu projektów, uruchom polecenie `Install-Package Entityframework`.

* W projekcie biblioteki klas Tworzenie klas modelu danych i klasy kontekstu i implementację `IDbContextFactory`.

* W konsoli zarządzania Pakietami dla projektu biblioteki klas, Uruchom polecenia `Enable-Migrations` i `Add-Migration Initial`. Jeśli ustawisz projekt platformy ASP.NET Core jako projekt startowy, Dodaj `-StartupProjectName EF6` tych poleceń.

* W projekcie podstawowe należy dodać odwołanie projektu do projektu biblioteki klas.

* W projekcie podstawowe w *Startup.cs*, zarejestruj DI kontekstu.

* W projekcie podstawowe w *appsettings.json*, Dodaj parametry połączenia.

* W projekcie Core Dodaj kontrolera i widoki, aby sprawdzić, czy może odczytywać i zapisywać dane. (Zwróć uwagę, że tworzenia szkieletu ASP.NET Core MVC nie będzie działać w kontekście platformy EF6 stanowiący odwołanie z biblioteki klas).

## <a name="summary"></a>Podsumowanie

Ten artykuł ma podano podstawowe wskazówki dotyczące korzystania z platformy Entity Framework 6 w aplikacji ASP.NET Core.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Entity Framework - konfiguracja na podstawie kodu](https://msdn.microsoft.com/data/jj680699.aspx)
