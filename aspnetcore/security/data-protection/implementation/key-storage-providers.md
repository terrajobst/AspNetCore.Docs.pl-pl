---
title: Dostawcy magazynu kluczy
author: rick-anderson
description: Dostawcy magazynu kluczy
keywords: Encryption,ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 01/14/2017
ms.topic: article
ms.assetid: 423e0a79-2f34-44c4-aaf3-146a53c39251
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-storage-providers
ms.openlocfilehash: d4b286dc47f8d66e6d09c3e0f48e6326139c8e1e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="key-storage-providers"></a><span data-ttu-id="07cb3-104">Dostawcy magazynu kluczy</span><span class="sxs-lookup"><span data-stu-id="07cb3-104">Key storage providers</span></span>

<a name="data-protection-implementation-key-storage-providers"></a>

<span data-ttu-id="07cb3-105">Domyślnie system ochrony danych [wykorzystuje heurystyki](xref:security/data-protection/configuration/default-settings) ustalenie, gdzie powinien zostać utrwalony materiał kluczy kryptograficznych.</span><span class="sxs-lookup"><span data-stu-id="07cb3-105">By default the data protection system [employs a heuristic](xref:security/data-protection/configuration/default-settings) to determine where cryptographic key material should be persisted.</span></span> <span data-ttu-id="07cb3-106">Deweloper może zastąpić heurystyki i ręcznie określ lokalizację.</span><span class="sxs-lookup"><span data-stu-id="07cb3-106">The developer can override the heuristic and manually specify the location.</span></span>

> [!NOTE]
> <span data-ttu-id="07cb3-107">Jeśli określisz lokalizacja jawna trwałości klucza systemu ochrony danych wyrejestrowania szyfrowanie klucza domyślne w mechanizm rest podany heurystyki, więc klucze będzie już być szyfrowane, gdy.</span><span class="sxs-lookup"><span data-stu-id="07cb3-107">If you specify an explicit key persistence location, the data protection system will deregister the default key encryption at rest mechanism that the heuristic provided, so keys will no longer be encrypted at rest.</span></span> <span data-ttu-id="07cb3-108">Zaleca się że dodatkowo [mechanizm jawne klucza szyfrowania określony](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest-providers) przez aplikacje produkcyjne.</span><span class="sxs-lookup"><span data-stu-id="07cb3-108">It is recommended that you additionally [specify an explicit key encryption mechanism](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest-providers) for production applications.</span></span>

<span data-ttu-id="07cb3-109">System ochrony danych jest dostarczany z wielu dostawców magazynu kluczy w polu.</span><span class="sxs-lookup"><span data-stu-id="07cb3-109">The data protection system ships with several in-box key storage providers.</span></span>

## <a name="file-system"></a><span data-ttu-id="07cb3-110">System plików</span><span class="sxs-lookup"><span data-stu-id="07cb3-110">File system</span></span>

<span data-ttu-id="07cb3-111">Przewidujemy, że wiele aplikacji będzie używać repozytorium kluczy opartych na systemie plików.</span><span class="sxs-lookup"><span data-stu-id="07cb3-111">We anticipate that many apps will use a file system-based key repository.</span></span> <span data-ttu-id="07cb3-112">Aby to skonfigurować, należy wywołać [PersistKeysToFileSystem](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection/DataProtectionBuilderExtensions.cs) procedury konfiguracji, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="07cb3-112">To configure this, call the [PersistKeysToFileSystem](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection/DataProtectionBuilderExtensions.cs) configuration routine as shown below.</span></span> <span data-ttu-id="07cb3-113">Podaj `DirectoryInfo` wskazujący repozytorium, w którym można przechowywać kluczy.</span><span class="sxs-lookup"><span data-stu-id="07cb3-113">Provide a `DirectoryInfo` pointing to the repository where keys should be stored.</span></span>

```csharp
sc.AddDataProtection()
       // persist keys to a specific directory
       .PersistKeysToFileSystem(new DirectoryInfo(@"c:\temp-keys\"));
   ```

<span data-ttu-id="07cb3-114">`DirectoryInfo` Może wskazywać na katalog na komputerze lokalnym lub może wskazywać folderu w udziale sieciowym.</span><span class="sxs-lookup"><span data-stu-id="07cb3-114">The `DirectoryInfo` can point to a directory on the local machine, or it can point to a folder on a network share.</span></span> <span data-ttu-id="07cb3-115">Jeśli wskazuje katalog na komputerze lokalnym (oraz scenariusz jest, że tylko aplikacje na komputerze lokalnym będą musieli używać tego repozytorium), należy rozważyć użycie [interfejsu DPAPI systemu Windows](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) do szyfrowania kluczy w stanie spoczynku.</span><span class="sxs-lookup"><span data-stu-id="07cb3-115">If pointing to a directory on the local machine (and the scenario is that only applications on the local machine will need to use this repository), consider using [Windows DPAPI](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) to encrypt the keys at rest.</span></span> <span data-ttu-id="07cb3-116">W przeciwnym razie należy rozważyć użycie [certyfikatu X.509](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) do szyfrowania kluczy w stanie spoczynku.</span><span class="sxs-lookup"><span data-stu-id="07cb3-116">Otherwise consider using an [X.509 certificate](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) to encrypt keys at rest.</span></span>

## <a name="azure-and-redis"></a><span data-ttu-id="07cb3-117">Azure i pamięci podręcznej Redis</span><span class="sxs-lookup"><span data-stu-id="07cb3-117">Azure and Redis</span></span>

<span data-ttu-id="07cb3-118">`Microsoft.AspNetCore.DataProtection.AzureStorage` i `Microsoft.AspNetCore.DataProtection.Redis` pakietów umożliwia przechowywanie kluczy ochrony danych w usłudze Azure Storage lub w pamięci podręcznej Redis.</span><span class="sxs-lookup"><span data-stu-id="07cb3-118">The `Microsoft.AspNetCore.DataProtection.AzureStorage` and `Microsoft.AspNetCore.DataProtection.Redis` packages allow storing your data protection keys in Azure Storage or a Redis cache.</span></span> <span data-ttu-id="07cb3-119">Klucze mogą być współużytkowane przez wiele wystąpień aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="07cb3-119">Keys can be shared across several instances of a web app.</span></span> <span data-ttu-id="07cb3-120">Aplikacji platformy ASP.NET Core mogą udostępniać pliki cookie uwierzytelniania lub ochrony CSRF na wielu serwerach.</span><span class="sxs-lookup"><span data-stu-id="07cb3-120">Your ASP.NET Core app can share authentication cookies or CSRF protection across multiple servers.</span></span> <span data-ttu-id="07cb3-121">Aby skonfigurować na platformie Azure, wywoływanie jednego z [PersistKeysToAzureBlobStorage](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection.AzureStorage/AzureDataProtectionBuilderExtensions.cs) overloads, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="07cb3-121">To configure on Azure, call one of the [PersistKeysToAzureBlobStorage](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection.AzureStorage/AzureDataProtectionBuilderExtensions.cs) overloads as shown below.</span></span>

```
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blob URI including SAS token>"));

    services.AddMvc();
}
```

<span data-ttu-id="07cb3-122">Zobacz też [kod testu Azure](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/samples/AzureBlob/Program.cs).</span><span class="sxs-lookup"><span data-stu-id="07cb3-122">See also the [Azure test code](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/samples/AzureBlob/Program.cs).</span></span>

<span data-ttu-id="07cb3-123">Aby skonfigurować w pamięci podręcznej Redis, wywoływanie jednego z [PersistKeysToRedis](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection.Redis/RedisDataProtectionBuilderExtensions.cs) overloads, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="07cb3-123">To configure on Redis, call one of the [PersistKeysToRedis](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection.Redis/RedisDataProtectionBuilderExtensions.cs) overloads as shown below.</span></span>

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

<span data-ttu-id="07cb3-124">Zobacz następujące tematy, aby uzyskać więcej informacji:</span><span class="sxs-lookup"><span data-stu-id="07cb3-124">See the following for more information:</span></span>

- [<span data-ttu-id="07cb3-125">ConnectionMultiplexer programie StackExchange.Redis</span><span class="sxs-lookup"><span data-stu-id="07cb3-125">StackExchange.Redis ConnectionMultiplexer</span></span>](https://github.com/StackExchange/StackExchange.Redis/blob/master/docs/Basics.md)
- [<span data-ttu-id="07cb3-126">Pamięć podręczna Azure Redis</span><span class="sxs-lookup"><span data-stu-id="07cb3-126">Azure Redis Cache</span></span>](https://docs.microsoft.com/azure/redis-cache/cache-dotnet-how-to-use-azure-redis-cache#connect-to-the-cache)
- <span data-ttu-id="07cb3-127">[Kod testu redis](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/samples/Redis/Program.cs).</span><span class="sxs-lookup"><span data-stu-id="07cb3-127">[Redis test code](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/samples/Redis/Program.cs).</span></span>

## <a name="registry"></a><span data-ttu-id="07cb3-128">Rejestr</span><span class="sxs-lookup"><span data-stu-id="07cb3-128">Registry</span></span>

<span data-ttu-id="07cb3-129">Czasami aplikacja może nie mieć uprawnienia do zapisu w systemie plików.</span><span class="sxs-lookup"><span data-stu-id="07cb3-129">Sometimes the app might not have write access to the file system.</span></span> <span data-ttu-id="07cb3-130">Rozważmy scenariusz, w którym aplikacja działa jako konto usługi wirtualnych (takich jak tożsamość puli aplikacji w w3wp.exe).</span><span class="sxs-lookup"><span data-stu-id="07cb3-130">Consider a scenario where an app is running as a virtual service account (such as w3wp.exe's app pool identity).</span></span> <span data-ttu-id="07cb3-131">W takim przypadku administrator może przygotowana klucz rejestru, który jest odpowiedni ACLed tożsamości konta usługi.</span><span class="sxs-lookup"><span data-stu-id="07cb3-131">In these cases, the administrator may have provisioned a registry key that is appropriate ACLed for the service account identity.</span></span> <span data-ttu-id="07cb3-132">Wywołanie [PersistKeysToRegistry](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection/DataProtectionBuilderExtensions.cs) procedury konfiguracji, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="07cb3-132">Call the [PersistKeysToRegistry](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection/DataProtectionBuilderExtensions.cs) configuration routine as shown below.</span></span> <span data-ttu-id="07cb3-133">Podaj `RegistryKey` wskazuje lokalizację przechowywania kluczy kryptograficznych/wartości.</span><span class="sxs-lookup"><span data-stu-id="07cb3-133">Provide a `RegistryKey` pointing to the location where cryptographic keys/values should be stored.</span></span>

```csharp
   sc.AddDataProtection()
       // persist keys to a specific location in the system registry
       .PersistKeysToRegistry(Registry.CurrentUser.OpenSubKey(@"SOFTWARE\Sample\keys"));
   ```

<span data-ttu-id="07cb3-134">Jeśli używasz rejestru systemowego jako mechanizmu stanu trwałego, rozważ użycie [interfejsu DPAPI systemu Windows](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) do szyfrowania kluczy w stanie spoczynku.</span><span class="sxs-lookup"><span data-stu-id="07cb3-134">If you use the system registry as a persistence mechanism, consider using [Windows DPAPI](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) to encrypt the keys at rest.</span></span>

## <a name="custom-key-repository"></a><span data-ttu-id="07cb3-135">Niestandardowe repozytorium klucza</span><span class="sxs-lookup"><span data-stu-id="07cb3-135">Custom key repository</span></span>

<span data-ttu-id="07cb3-136">Jeśli mechanizmów w polu nie są odpowiednie, deweloper można określić własne mechanizmu stanu trwałego klucza zapewniając niestandardowego `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="07cb3-136">If the in-box mechanisms are not appropriate, the developer can specify their own key persistence mechanism by providing a custom `IXmlRepository`.</span></span>
