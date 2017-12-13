---
title: "Konfigurowanie tożsamości typu danych kluczy podstawowych"
author: AdrienTorris
description: "W tym artykule opisano kroki związane z konfigurowaniem typ żądanych danych, używany dla tożsamości ASP.NET Core klucza podstawowego."
keywords: "Klucz podstawowy platformy ASP.NET Core tożsamości,"
ms.author: scaddie
manager: wpickett
ms.date: 09/28/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity-primary-key-configuration
ms.openlocfilehash: 5734a9aa86fb2831bd054593ad41c3e3bda4729e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="configure-the-aspnet-core-identity-primary-key-data-type"></a><span data-ttu-id="5dbe2-104">Konfigurowanie typu danych kluczy podstawowych ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="5dbe2-104">Configure the ASP.NET Core Identity primary key data type</span></span>

<span data-ttu-id="5dbe2-105">Tożsamość platformy ASP.NET Core pozwala na skonfigurowanie typ danych używany do reprezentowania klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="5dbe2-105">ASP.NET Core Identity allows you to configure the data type used to represent a primary key.</span></span> <span data-ttu-id="5dbe2-106">Używa tożsamości `string` domyślnie — typ danych.</span><span class="sxs-lookup"><span data-stu-id="5dbe2-106">Identity uses the `string` data type by default.</span></span> <span data-ttu-id="5dbe2-107">Możesz zmienić to zachowanie.</span><span class="sxs-lookup"><span data-stu-id="5dbe2-107">You can override this behavior.</span></span>

## <a name="customize-the-primary-key-data-type"></a><span data-ttu-id="5dbe2-108">Dostosowanie typu danych kluczy podstawowych</span><span class="sxs-lookup"><span data-stu-id="5dbe2-108">Customize the primary key data type</span></span>

1. <span data-ttu-id="5dbe2-109">Utwórz niestandardową implementację [IdentityUser](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuser-1) klasy.</span><span class="sxs-lookup"><span data-stu-id="5dbe2-109">Create a custom implementation of the [IdentityUser](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuser-1) class.</span></span> <span data-ttu-id="5dbe2-110">Reprezentuje typ służący do tworzenia obiektów użytkownika.</span><span class="sxs-lookup"><span data-stu-id="5dbe2-110">It represents the type to be used for creating user objects.</span></span> <span data-ttu-id="5dbe2-111">W poniższym przykładzie, domyślnie `string` typu jest zastępowany `Guid`.</span><span class="sxs-lookup"><span data-stu-id="5dbe2-111">In the following example, the default `string` type is replaced with `Guid`.</span></span>

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationUser.cs?highlight=4&range=7-13)]

1. <span data-ttu-id="5dbe2-112">Utwórz niestandardową implementację [IdentityRole](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityrole-1) klasy.</span><span class="sxs-lookup"><span data-stu-id="5dbe2-112">Create a custom implementation of the [IdentityRole](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityrole-1) class.</span></span> <span data-ttu-id="5dbe2-113">Reprezentuje typ służący do tworzenia obiektów roli.</span><span class="sxs-lookup"><span data-stu-id="5dbe2-113">It represents the type to be used for creating role objects.</span></span> <span data-ttu-id="5dbe2-114">W poniższym przykładzie, domyślnie `string` typu jest zastępowany `Guid`.</span><span class="sxs-lookup"><span data-stu-id="5dbe2-114">In the following example, the default `string` type is replaced with `Guid`.</span></span>
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationRole.cs?highlight=3&range=7-12)]
    
1. <span data-ttu-id="5dbe2-115">Utwórz klasę kontekstu w niestandardowej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="5dbe2-115">Create a custom database context class.</span></span> <span data-ttu-id="5dbe2-116">Dziedziczy z klasy kontekstu bazy danych programu Entity Framework używany dla tożsamości.</span><span class="sxs-lookup"><span data-stu-id="5dbe2-116">It inherits from the Entity Framework database context class used for Identity.</span></span> <span data-ttu-id="5dbe2-117">`TUser` i `TRole` argumenty odwołania klas niestandardowych użytkownika i roli odpowiednio utworzony w poprzednim kroku.</span><span class="sxs-lookup"><span data-stu-id="5dbe2-117">The `TUser` and `TRole` arguments reference the custom user and role classes created in the previous step, respectively.</span></span> <span data-ttu-id="5dbe2-118">`Guid` — Typ danych jest zdefiniowany dla klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="5dbe2-118">The `Guid` data type is defined for the primary key.</span></span>

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Data/ApplicationDbContext.cs?highlight=3&range=9-26)]
    
1. <span data-ttu-id="5dbe2-119">Podczas dodawania usługa tożsamości w klasie uruchamiania aplikacji, należy zarejestrować klasy kontekstu w niestandardowej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="5dbe2-119">Register the custom database context class when adding the Identity service in the app's startup class.</span></span>

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5dbe2-120">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5dbe2-120">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)
    
    <span data-ttu-id="5dbe2-121">`AddEntityFrameworkStores` Nie obsługuje metody `TKey` argumentu w postaci, w jakiej zostały w ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="5dbe2-121">The `AddEntityFrameworkStores` method doesn't accept a `TKey` argument as it did in ASP.NET Core 1.x.</span></span> <span data-ttu-id="5dbe2-122">Klucz podstawowy typ danych jest wywnioskowany analizując `DbContext` obiektu.</span><span class="sxs-lookup"><span data-stu-id="5dbe2-122">The primary key's data type is inferred by analyzing the `DbContext` object.</span></span>
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=6-8&range=25-37)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5dbe2-123">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5dbe2-123">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
    
    <span data-ttu-id="5dbe2-124">`AddEntityFrameworkStores` Metoda przyjmuje `TKey` wskazuje klucz podstawowy typ danych argumentu.</span><span class="sxs-lookup"><span data-stu-id="5dbe2-124">The `AddEntityFrameworkStores` method accepts a `TKey` argument indicating the primary key's data type.</span></span>
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=9-11&range=39-55)]
    
    ---

## <a name="test-the-changes"></a><span data-ttu-id="5dbe2-125">Testowanie zmian</span><span class="sxs-lookup"><span data-stu-id="5dbe2-125">Test the changes</span></span>

<span data-ttu-id="5dbe2-126">Po zakończeniu zmiany konfiguracji właściwość reprezentujące klucz podstawowy odzwierciedla nowy typ danych.</span><span class="sxs-lookup"><span data-stu-id="5dbe2-126">Upon completion of the configuration changes, the property representing the primary key reflects the new data type.</span></span> <span data-ttu-id="5dbe2-127">W poniższym przykładzie pokazano, uzyskiwanie dostępu do właściwości w kontroler MVC.</span><span class="sxs-lookup"><span data-stu-id="5dbe2-127">The following example demonstrates accessing the property in an MVC controller.</span></span>

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Controllers/AccountController.cs?name=snippet_GetCurrentUserId&highlight=6)]
