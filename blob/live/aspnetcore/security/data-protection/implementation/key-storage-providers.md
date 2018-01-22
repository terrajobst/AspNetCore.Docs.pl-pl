---
title: Dostawcy magazynu kluczy
author: rick-anderson
description: Dostawcy magazynu kluczy
ms.author: riande
manager: wpickett
ms.date: 01/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-storage-providers
ms.openlocfilehash: f95322c208d323c052295959e39f945700b7ec57
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="key-storage-providers"></a><span data-ttu-id="02ae1-103">Dostawcy magazynu kluczy</span><span class="sxs-lookup"><span data-stu-id="02ae1-103">Key storage providers</span></span>

<a name="data-protection-implementation-key-storage-providers"></a>

<span data-ttu-id="02ae1-104">Domyślnie system ochrony danych [wykorzystuje heurystyki](xref:security/data-protection/configuration/default-settings) ustalenie, gdzie powinien zostać utrwalony materiał kluczy kryptograficznych.</span><span class="sxs-lookup"><span data-stu-id="02ae1-104">By default the data protection system [employs a heuristic](xref:security/data-protection/configuration/default-settings) to determine where cryptographic key material should be persisted.</span></span> <span data-ttu-id="02ae1-105">Deweloper może zastąpić heurystyki i ręcznie określ lokalizację.</span><span class="sxs-lookup"><span data-stu-id="02ae1-105">The developer can override the heuristic and manually specify the location.</span></span>

> [!NOTE]
> <span data-ttu-id="02ae1-106">Jeśli określisz lokalizacja jawna trwałości klucza systemu ochrony danych wyrejestrowania szyfrowanie klucza domyślne w mechanizm rest podany heurystyki, więc klucze będzie już być szyfrowane, gdy.</span><span class="sxs-lookup"><span data-stu-id="02ae1-106">If you specify an explicit key persistence location, the data protection system will deregister the default key encryption at rest mechanism that the heuristic provided, so keys will no longer be encrypted at rest.</span></span> <span data-ttu-id="02ae1-107">Zaleca się że dodatkowo [mechanizm jawne klucza szyfrowania określony](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest-providers) przez aplikacje produkcyjne.</span><span class="sxs-lookup"><span data-stu-id="02ae1-107">It is recommended that you additionally [specify an explicit key encryption mechanism](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest-providers) for production applications.</span></span>

<span data-ttu-id="02ae1-108">System ochrony danych jest dostarczany z wielu dostawców magazynu kluczy w polu.</span><span class="sxs-lookup"><span data-stu-id="02ae1-108">The data protection system ships with several in-box key storage providers.</span></span>

## <a name="file-system"></a><span data-ttu-id="02ae1-109">System plików</span><span class="sxs-lookup"><span data-stu-id="02ae1-109">File system</span></span>

<span data-ttu-id="02ae1-110">Przewidujemy, że wiele aplikacji będzie używać repozytorium kluczy opartych na systemie plików.</span><span class="sxs-lookup"><span data-stu-id="02ae1-110">We anticipate that many apps will use a file system-based key repository.</span></span> <span data-ttu-id="02ae1-111">Aby to skonfigurować, należy wywołać [PersistKeysToFileSystem](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection/DataProtectionBuilderExtensions.cs) procedury konfiguracji, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="02ae1-111">To configure this, call the [PersistKeysToFileSystem](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection/DataProtectionBuilderExtensions.cs) configuration routine as shown below.</span></span> <span data-ttu-id="02ae1-112">Podaj `DirectoryInfo` wskazujący repozytorium, w którym można przechowywać kluczy.</span><span class="sxs-lookup"><span data-stu-id="02ae1-112">Provide a `DirectoryInfo` pointing to the repository where keys should be stored.</span></span>

```csharp
sc.AddDataProtection()
       // persist keys to a specific directory
       .PersistKeysToFileSystem(new DirectoryInfo(@"c:\temp-keys\"));
   ```

<span data-ttu-id="02ae1-113">`DirectoryInfo` Może wskazywać na katalog na komputerze lokalnym lub może wskazywać folderu w udziale sieciowym.</span><span class="sxs-lookup"><span data-stu-id="02ae1-113">The `DirectoryInfo` can point to a directory on the local machine, or it can point to a folder on a network share.</span></span> <span data-ttu-id="02ae1-114">Jeśli wskazuje katalog na komputerze lokalnym (oraz scenariusz jest, że tylko aplikacje na komputerze lokalnym będą musieli używać tego repozytorium), należy rozważyć użycie [interfejsu DPAPI systemu Windows](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) do szyfrowania kluczy w stanie spoczynku.</span><span class="sxs-lookup"><span data-stu-id="02ae1-114">If pointing to a directory on the local machine (and the scenario is that only applications on the local machine will need to use this repository), consider using [Windows DPAPI](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) to encrypt the keys at rest.</span></span> <span data-ttu-id="02ae1-115">W przeciwnym razie należy rozważyć użycie [certyfikatu X.509](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) do szyfrowania kluczy w stanie spoczynku.</span><span class="sxs-lookup"><span data-stu-id="02ae1-115">Otherwise consider using an [X.509 certificate](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) to encrypt keys at rest.</span></span>

## <a name="azure-and-redis"></a><span data-ttu-id="02ae1-116">Azure i pamięci podręcznej Redis</span><span class="sxs-lookup"><span data-stu-id="02ae1-116">Azure and Redis</span></span>

<span data-ttu-id="02ae1-117">`Microsoft.AspNetCore.DataProtection.AzureStorage` i `Microsoft.AspNetCore.DataProtection.Redis` pakietów umożliwia przechowywanie kluczy ochrony danych w usłudze Azure Storage lub w pamięci podręcznej Redis.</span><span class="sxs-lookup"><span data-stu-id="02ae1-117">The `Microsoft.AspNetCore.DataProtection.AzureStorage` and `Microsoft.AspNetCore.DataProtection.Redis` packages allow storing your data protection keys in Azure Storage or a Redis cache.</span></span> <span data-ttu-id="02ae1-118">Klucze mogą być współużytkowane przez wiele wystąpień aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="02ae1-118">Keys can be shared across several instances of a web app.</span></span> <span data-ttu-id="02ae1-119">Aplikacji platformy ASP.NET Core mogą udostępniać pliki cookie uwierzytelniania lub ochrony CSRF na wielu serwerach.</span><span class="sxs-lookup"><span data-stu-id="02ae1-119">Your ASP.NET Core app can share authentication cookies or CSRF protection across multiple servers.</span></span> <span data-ttu-id="02ae1-120">Aby skonfigurować na platformie Azure, wywoływanie jednego z [PersistKeysToAzureBlobStorage](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection.AzureStorage/AzureDataProtectionBuilderExtensions.cs) overloads, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="02ae1-120">To configure on Azure, call one of the [PersistKeysToAzureBlobStorage](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection.AzureStorage/AzureDataProtectionBuilderExtensions.cs) overloads as shown below.</span></span>

```
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blob URI including SAS token>"));

    services.AddMvc();
}
```

<span data-ttu-id="02ae1-121">Zobacz też [kod testu Azure](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/samples/AzureBlob/Program.cs).</span><span class="sxs-lookup"><span data-stu-id="02ae1-121">See also the [Azure test code](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/samples/AzureBlob/Program.cs).</span></span>

<span data-ttu-id="02ae1-122">Aby skonfigurować w pamięci podręcznej Redis, wywoływanie jednego z [PersistKeysToRedis](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection.Redis/RedisDataProtectionBuilderExtensions.cs) overloads, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="02ae1-122">To configure on Redis, call one of the [PersistKeysToRedis](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection.Redis/RedisDataProtectionBuilderExtensions.cs) overloads as shown below.</span></span>

```
public void ConfigureServices(IServiceCollection services)
{
    // Connect to Redis database.
    var redis = ConnectionMultiplexer.Connect("<URI>");
    services.AddDataProtection()
        .PersistKeysToRedis(redis, "DataProtection-Keys");

    services.AddMvc();
}
```

<span data-ttu-id="02ae1-123">Zobacz następujące tematy, aby uzyskać więcej informacji:</span><span class="sxs-lookup"><span data-stu-id="02ae1-123">See the following for more information:</span></span>

- [<span data-ttu-id="02ae1-124">StackExchange.Redis ConnectionMultiplexer</span><span class="sxs-lookup"><span data-stu-id="02ae1-124">StackExchange.Redis ConnectionMultiplexer</span></span>](https://github.com/StackExchange/StackExchange.Redis/blob/master/docs/Basics.md)
- [<span data-ttu-id="02ae1-125">Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="02ae1-125">Azure Redis Cache</span></span>](https://docs.microsoft.com/azure/redis-cache/cache-dotnet-how-to-use-azure-redis-cache#connect-to-the-cache)
- <span data-ttu-id="02ae1-126">[Kod testu redis](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/samples/Redis/Program.cs).</span><span class="sxs-lookup"><span data-stu-id="02ae1-126">[Redis test code](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/samples/Redis/Program.cs).</span></span>

## <a name="registry"></a><span data-ttu-id="02ae1-127">Rejestr</span><span class="sxs-lookup"><span data-stu-id="02ae1-127">Registry</span></span>

<span data-ttu-id="02ae1-128">Czasami aplikacja może nie mieć uprawnienia do zapisu w systemie plików.</span><span class="sxs-lookup"><span data-stu-id="02ae1-128">Sometimes the app might not have write access to the file system.</span></span> <span data-ttu-id="02ae1-129">Rozważmy scenariusz, w którym aplikacja działa jako konto usługi wirtualnych (takich jak tożsamość puli aplikacji w w3wp.exe).</span><span class="sxs-lookup"><span data-stu-id="02ae1-129">Consider a scenario where an app is running as a virtual service account (such as w3wp.exe's app pool identity).</span></span> <span data-ttu-id="02ae1-130">W takim przypadku administrator może przygotowana klucz rejestru, który jest odpowiedni ACLed tożsamości konta usługi.</span><span class="sxs-lookup"><span data-stu-id="02ae1-130">In these cases, the administrator may have provisioned a registry key that is appropriate ACLed for the service account identity.</span></span> <span data-ttu-id="02ae1-131">Wywołanie [PersistKeysToRegistry](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection/DataProtectionBuilderExtensions.cs) procedury konfiguracji, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="02ae1-131">Call the [PersistKeysToRegistry](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection/DataProtectionBuilderExtensions.cs) configuration routine as shown below.</span></span> <span data-ttu-id="02ae1-132">Podaj `RegistryKey` wskazuje lokalizację przechowywania kluczy kryptograficznych/wartości.</span><span class="sxs-lookup"><span data-stu-id="02ae1-132">Provide a `RegistryKey` pointing to the location where cryptographic keys/values should be stored.</span></span>

```csharp
   sc.AddDataProtection()
       // persist keys to a specific location in the system registry
       .PersistKeysToRegistry(Registry.CurrentUser.OpenSubKey(@"SOFTWARE\Sample\keys"));
   ```

<span data-ttu-id="02ae1-133">Jeśli używasz rejestru systemowego jako mechanizmu stanu trwałego, rozważ użycie [interfejsu DPAPI systemu Windows](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) do szyfrowania kluczy w stanie spoczynku.</span><span class="sxs-lookup"><span data-stu-id="02ae1-133">If you use the system registry as a persistence mechanism, consider using [Windows DPAPI](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) to encrypt the keys at rest.</span></span>

## <a name="custom-key-repository"></a><span data-ttu-id="02ae1-134">Niestandardowe repozytorium klucza</span><span class="sxs-lookup"><span data-stu-id="02ae1-134">Custom key repository</span></span>

<span data-ttu-id="02ae1-135">Jeśli mechanizmów w polu nie są odpowiednie, deweloper można określić własne mechanizmu stanu trwałego klucza zapewniając niestandardowego `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="02ae1-135">If the in-box mechanisms are not appropriate, the developer can specify their own key persistence mechanism by providing a custom `IXmlRepository`.</span></span>
