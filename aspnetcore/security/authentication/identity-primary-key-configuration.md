---
title: "Konfigurowanie tożsamości typu danych kluczy podstawowych"
author: AdrienTorris
description: "W tym artykule opisano kroki związane z konfigurowaniem typ żądanych danych, używany dla tożsamości ASP.NET Core klucza podstawowego."
ms.author: scaddie
manager: wpickett
ms.date: 09/28/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity-primary-key-configuration
ms.openlocfilehash: da46aa90a434a978a55467da982d746eb2d959ee
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="configure-the-aspnet-core-identity-primary-key-data-type"></a>Konfigurowanie typu danych kluczy podstawowych ASP.NET Core Identity

Tożsamość platformy ASP.NET Core pozwala na skonfigurowanie typ danych używany do reprezentowania klucza podstawowego. Używa tożsamości `string` domyślnie — typ danych. Możesz zmienić to zachowanie.

## <a name="customize-the-primary-key-data-type"></a>Dostosowanie typu danych kluczy podstawowych

1. Utwórz niestandardową implementację [IdentityUser](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuser-1) klasy. Reprezentuje typ służący do tworzenia obiektów użytkownika. W poniższym przykładzie, domyślnie `string` typu jest zastępowany `Guid`.

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationUser.cs?highlight=4&range=7-13)]

1. Utwórz niestandardową implementację [IdentityRole](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityrole-1) klasy. Reprezentuje typ służący do tworzenia obiektów roli. W poniższym przykładzie, domyślnie `string` typu jest zastępowany `Guid`.
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationRole.cs?highlight=3&range=7-12)]
    
1. Utwórz klasę kontekstu w niestandardowej bazie danych. Dziedziczy z klasy kontekstu bazy danych programu Entity Framework używany dla tożsamości. `TUser` i `TRole` argumenty odwołania klas niestandardowych użytkownika i roli odpowiednio utworzony w poprzednim kroku. `Guid` — Typ danych jest zdefiniowany dla klucza podstawowego.

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Data/ApplicationDbContext.cs?highlight=3&range=9-26)]
    
1. Podczas dodawania usługa tożsamości w klasie uruchamiania aplikacji, należy zarejestrować klasy kontekstu w niestandardowej bazie danych.

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)
    
    `AddEntityFrameworkStores` Nie obsługuje metody `TKey` argumentu w postaci, w jakiej zostały w ASP.NET Core 1.x. Klucz podstawowy typ danych jest wywnioskowany analizując `DbContext` obiektu.
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=6-8&range=25-37)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)
    
    `AddEntityFrameworkStores` Metoda przyjmuje `TKey` wskazuje klucz podstawowy typ danych argumentu.
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=9-11&range=39-55)]
    
    ---

## <a name="test-the-changes"></a>Testowanie zmian

Po zakończeniu zmiany konfiguracji właściwość reprezentujące klucz podstawowy odzwierciedla nowy typ danych. W poniższym przykładzie pokazano, uzyskiwanie dostępu do właściwości w kontroler MVC.

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Controllers/AccountController.cs?name=snippet_GetCurrentUserId&highlight=6)]
