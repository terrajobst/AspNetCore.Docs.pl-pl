---
title: Wzorzec repozytorium za pomocą programu ASP.NET Core
author: ardalis
description: Dowiedz się, jak zaimplementować wzorzec projektowania aplikacji w repozytorium w aplikacji ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 07/02/2018
uid: fundamentals/repository-pattern
ms.openlocfilehash: d64c3d090f62c580ebc05a6bbc2744a4508bb9bc
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342754"
---
# <a name="repository-pattern-with-aspnet-core"></a>Wzorzec repozytorium za pomocą programu ASP.NET Core

Przez [Steve Smith](https://ardalis.com/), [Scott Addie](https://scottaddie.com), i [Luke Latham](https://github.com/guardrex)

*Wzorca repozytorium* jest szablon projektu, który izoluje dostęp do danych za abstrakcji interfejsu. Połączenie z bazą danych i manipulowania obiektów magazynu danych odbywa się przy użyciu metod dostarczonych przez implementację interfejsu. W związku z tym nie ma potrzeby wywoływania kodu radzenia sobie z obaw bazy danych, takie jak połączenia, poleceń i czytników.

Implementacja wzorca repozytorium za pomocą programu ASP.NET Core oferuje następujące korzyści:

* Organizacja aplikacji jest mniej złożona przy użyciu nie bezpośrednie interdependency między działalności biznesowej i warstwy dostępu do danych.
* Jest łatwiejsze do ponownego użycia kodu dostępu do bazy danych, ponieważ kod jest zarządzane centralnie przez jeden lub więcej repozytoriów.
* Domeny biznesowej mogą być niezależne jednostki przetestowane z warstwy bazy danych.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/repository-pattern/samples) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

[Przykładową aplikację](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/repository-pattern/samples) zastosowanie wzorca repozytorium umożliwia inicjowanie i wyświetlić listę nazw znaków filmu. Ta aplikacja używa [Entity Framework Core](/ef/core/) i `ApplicationDbContext` klasy dla jej funkcji trwałości danych, ale infrastrukturą bazy danych nie jest widoczny w przypadku, gdy dane są dostępne. Data access i obiektów bazy danych zostały wyabstrahowane za [repozytorium](https://martinfowler.com/eaaCatalog/repository.html).

## <a name="repository-interface"></a>Interfejs repozytorium

Interfejs repozytorium definiuje właściwości i metody dla implementacji. W przykładowej aplikacji interfejsu repozytorium danych znakowych film jest `ICharacterRepository`. `ICharacterRepository` definiuje `ListAll` i `Add` metod wymaganych do pracy z `Character` wystąpień w aplikacji:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Interfaces/ICharacterRepository.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Interfaces/ICharacterRepository.cs?name=snippet1)]

::: moniker-end

`Character` definiuje się następująco:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Models/Character.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Models/Character.cs?name=snippet1)]

::: moniker-end

## <a name="repository-concrete-type"></a>Konkretny typ repozytorium

Interfejs jest implementowany przez konkretnego typu. W przykładowej aplikacji `CharacterRepository` zarządza kontekst bazy danych i implementuje `ListAll` i `Add` metody:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Models/CharacterRepository.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Models/CharacterRepository.cs?name=snippet1)]

::: moniker-end

## <a name="register-the-repository-service"></a>Zarejestruj usługę repozytorium

Kontekst repozytorium i bazy danych są zarejestrowane w usłudze container service w `Startup.ConfigureServices`. W przykładowej aplikacji `ApplicationDbContext` skonfigurowano przy użyciu wywołania metody rozszerzenia [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext). `ICharacterRepository` jest zarejestrowany jako usługa o określonym zakresie:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Startup.cs?name=snippet1&highlight=4-6,18)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Startup.cs?name=snippet1&highlight=4-6,12)]

::: moniker-end

## <a name="inject-an-instance-of-the-repository"></a>Wstrzykiwanie wystąpienie repozytorium

W klasie, w której wymagana jest dostępu do bazy danych wystąpienie repozytorium jest za pośrednictwem konstruktora i przypisane do pola prywatnego, do użytku przez metody klasy. W przykładowej aplikacji `ICharacterRepository` służy do:

* Wypełniania bazy danych, jeśli żadne znaki nie istnieje.
* Uzyskaj listę znaków do wyświetlenia.

Zwróć uwagę, jak kod wywołujący tylko współdziała z implementacją interfejsu `CharacterRepository`. Wywoływanie kodu nie używa `ApplicationDbContext` bezpośrednio:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Pages/Index.cshtml.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Controllers/HomeController.cs?name=snippet1)]

::: moniker-end

## <a name="generic-repository-interface"></a>Interfejs ogólny repozytorium

Ten temat i jego przykładową aplikację pokazują najprostszej implementacji wzorca repozytorium, w których jedno repozytorium jest tworzony dla każdego obiektu firm. Jeśli aplikacja przekroczy kilka obiektów *interfejsu ogólnego repozytorium* może zmniejszyć ilość kodu wymaganą do zaimplementowania wzorca repozytorium. Aby uzyskać więcej informacji, zobacz [DevIQ: wzorzec repozytorium: ogólny interfejs repozytorium](http://deviq.com/repository-pattern/).

## <a name="additional-resources"></a>Dodatkowe zasoby

* [DevIQ: Wzorzec repozytorium](https://deviq.com/repository-pattern/)
* [Wstrzykiwanie zależności](xref:fundamentals/dependency-injection)
* [Wstrzykiwanie zależności do widoków](xref:mvc/views/dependency-injection)
* [Wstrzykiwanie zależności do kontrolerów](xref:mvc/controllers/dependency-injection)
* [Wstrzykiwanie zależności w programach obsługi wymagań](xref:security/authorization/dependencyinjection)
* [Inwersja kontroli kontenerów i wzorzec wstrzykiwania zależności](https://www.martinfowler.com/articles/injection.html)
