---
title: Dostawcy magazynu kluczy w programie ASP.NET Core
author: rick-anderson
description: Więcej informacji o dostawcy magazynu kluczy na platformie ASP.NET Core i jak skonfigurować lokalizacje magazynu kluczy.
ms.author: riande
ms.date: 06/11/2019
uid: security/data-protection/implementation/key-storage-providers
ms.openlocfilehash: 49b068480fe7ba0a9b338aa5f5b7fc19fb98528f
ms.sourcegitcommit: f5762967df3be8b8c868229e679301f2f7954679
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67048100"
---
# <a name="key-storage-providers-in-aspnet-core"></a>Dostawcy magazynu kluczy w programie ASP.NET Core

System ochrony danych [wykorzystuje mechanizm wykrywania domyślnie](xref:security/data-protection/configuration/default-settings) ustalenie, gdzie utrwalone kluczy kryptograficznych. Deweloper można zastąpić domyślny mechanizm odnajdywania i ręcznie określ lokalizację.

> [!WARNING]
> Jeśli określisz lokalizacji jawne trwałość klucza, system ochrony danych deregisters domyślne szyfrowanie kluczy podczas mechanizmu rest, więc klucze nie są szyfrowane w stanie spoczynku. Zalecamy, aby można dodatkowo [mechanizm jawne klucza szyfrowania określony](xref:security/data-protection/implementation/key-encryption-at-rest) we wdrożeniach produkcyjnych.

## <a name="file-system"></a>System plików

Aby skonfigurować repozytorium kluczy opartych na systemie plików, należy wywołać [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) procedury konfiguracji, jak pokazano poniżej. Podaj [DirectoryInfo](/dotnet/api/system.io.directoryinfo) wskazującego repozytorium, gdzie można przechowywać kluczy:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"c:\temp-keys\"));
}
```

`DirectoryInfo` Może wskazywać katalog na komputerze lokalnym lub może wskazywać do folderu w udziale sieciowym. Jeśli wskazuje katalog na komputerze lokalnym i scenariusza jest to, że tylko aplikacje na komputerze lokalnym wymaga dostępu do używania tego repozytorium, należy rozważyć użycie [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest) (na Windows) do szyfrowania kluczy w stanie spoczynku. W przeciwnym razie należy wziąć pod uwagę przy użyciu [certyfikat X.509](xref:security/data-protection/implementation/key-encryption-at-rest) do szyfrowania kluczy w stanie spoczynku.

## <a name="azure-storage"></a>Azure Storage

[Microsoft.AspNetCore.DataProtection.AzureStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.AzureStorage/) pakiet umożliwia przechowywanie kluczy ochrony danych w usłudze Azure Blob Storage. Klucze mogą być współużytkowane przez wiele wystąpień aplikacji sieci web. Aplikacje mogą udostępniać pliki cookie uwierzytelniania lub CSRF ochronę na wielu serwerach.

Aby skonfigurować dostawcę usługi Azure Blob Storage, należy wywołać jedną z [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage) przeciążenia. 

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blob URI including SAS token>"));
}
```

Jeśli aplikacja sieci web działa jako usługa platformy Azure, tokeny uwierzytelniania mogą być automatycznie tworzone za pomocą [ Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication/). 

```csharp
var tokenProvider = new AzureServiceTokenProvider();
var token = await tokenProvider.GetAccessTokenAsync("https://storage.azure.com/");
var credentials = new StorageCredentials(new TokenCredential(token));
var storageAccount = new CloudStorageAccount(credentials, "mystorageaccount", "core.windows.net", useHttps: true);
var client = storageAccount.CreateCloudBlobClient();
var container = client.GetContainerReference("my-key-container");

// optional - provision the container automatically
await container.CreateIfNotExistsAsync();

services.AddDataProtection()
    .PersistKeysToAzureBlobStorage(container, "keys.xml");
```

Zobacz [więcej szczegółów na temat konfigurowania usługi do uwierzytelniania.](/azure/key-vault/service-to-service-authentication)

## <a name="redis"></a>Redis Cache

::: moniker range=">= aspnetcore-2.2"

[Microsoft.AspNetCore.DataProtection.StackExchangeRedis](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.StackExchangeRedis/) pakiet umożliwia przechowywanie kluczy ochrony danych w pamięci podręcznej Redis. Klucze mogą być współużytkowane przez wiele wystąpień aplikacji sieci web. Aplikacje mogą udostępniać pliki cookie uwierzytelniania lub CSRF ochronę na wielu serwerach.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

[Microsoft.AspNetCore.DataProtection.Redis](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Redis/) pakiet umożliwia przechowywanie kluczy ochrony danych w pamięci podręcznej Redis. Klucze mogą być współużytkowane przez wiele wystąpień aplikacji sieci web. Aplikacje mogą udostępniać pliki cookie uwierzytelniania lub CSRF ochronę na wielu serwerach.

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

Aby skonfigurować w pamięci podręcznej Redis, należy wywołać jedną z [PersistKeysToStackExchangeRedis](/dotnet/api/microsoft.aspnetcore.dataprotection.stackexchangeredisdataprotectionbuilderextensions.persistkeystostackexchangeredis) przeciążenia:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    var redis = ConnectionMultiplexer.Connect("<URI>");
    services.AddDataProtection()
        .PersistKeysToStackExchangeRedis(redis, "DataProtection-Keys");
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Aby skonfigurować w pamięci podręcznej Redis, należy wywołać jedną z [PersistKeysToRedis](/dotnet/api/microsoft.aspnetcore.dataprotection.redisdataprotectionbuilderextensions.persistkeystoredis) przeciążenia:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    var redis = ConnectionMultiplexer.Connect("<URI>");
    services.AddDataProtection()
        .PersistKeysToRedis(redis, "DataProtection-Keys");
}
```

::: moniker-end

Więcej informacji znajduje się w następujących tematach:

* [StackExchange.Redis ConnectionMultiplexer](https://github.com/StackExchange/StackExchange.Redis/blob/master/docs/Basics.md)
* [Usługi Azure Redis Cache](/azure/redis-cache/cache-dotnet-how-to-use-azure-redis-cache#connect-to-the-cache)
* [Przykłady ASPNET/DataProtection](https://github.com/aspnet/AspNetCore/tree/2.2.0/src/DataProtection/samples)

## <a name="registry"></a>Rejestr

**Dotyczy tylko wdrożenia Windows.**

Czasami aplikacja ma uprawnienia do zapisu w systemie plików. Rozważmy scenariusz, w którym aplikacja jest uruchomiona jako konto usługi wirtualnej (takie jak *w3wp.exe*dla tożsamości puli aplikacji). W takich przypadkach administrator może aprowizować klucz rejestru, który jest dostępny za pomocą tożsamości konta usługi. Wywołaj [PersistKeysToRegistry](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystoregistry) metodę rozszerzenia, jak pokazano poniżej. Podaj [RegistryKey](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository.registrykey) wskazuje lokalizację przechowywania kluczy kryptograficznych:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToRegistry(Registry.CurrentUser.OpenSubKey(@"SOFTWARE\Sample\keys"));
}
```

> [!IMPORTANT]
> Firma Microsoft zaleca używanie [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest) do szyfrowania kluczy w stanie spoczynku.

::: moniker range=">= aspnetcore-2.2"

## <a name="entity-framework-core"></a>Entity Framework Core

[Microsoft.AspNetCore.DataProtection.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.EntityFrameworkCore/) pakietu udostępnia mechanizm do przechowywania danych ochrony kluczy z bazą danych przy użyciu platformy Entity Framework Core. `Microsoft.AspNetCore.DataProtection.EntityFrameworkCore` Pakietu NuGet należy dodać do pliku projektu nie jest częścią [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).

Z tego pakietu kluczy mogą być współużytkowane przez wiele wystąpień aplikacji sieci web.

Aby skonfigurować dostawcę programu EF Core, należy wywołać [ `PersistKeysToDbContext<TContext>` ](/dotnet/api/microsoft.aspnetcore.dataprotection.entityframeworkcoredataprotectionextensions.persistkeystodbcontext) metody:

[!code-csharp[Main](key-storage-providers/sample/Startup.cs?name=snippet&highlight=13-15)]

Parametr ogólny, `TContext`, musi dziedziczyć [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) i zaimplementować [IDataProtectionKeyContext](/dotnet/api/microsoft.aspnetcore.dataprotection.entityframeworkcore.idataprotectionkeycontext):

[!code-csharp[Main](key-storage-providers/sample/MyKeysContext.cs)]

Utwórz `DataProtectionKeys` tabeli. 

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Wykonaj następujące polecenia w **Konsola Menedżera pakietów** okna (PMC):

```PowerShell
Add-Migration AddDataProtectionKeys -Context MyKeysContext
Update-Database -Context MyKeysContext
```

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Wykonaj następujące polecenia w powłoce poleceń:

```console
dotnet ef migrations add AddDataProtectionKeys --context MyKeysContext
dotnet ef database update --context MyKeysContext
```

---

`MyKeysContext` jest `DbContext` zdefiniowane w poprzednim przykładzie kodu. Jeśli używasz `DbContext` pod inną nazwą podstaw swoje `DbContext` nazwę `MyKeysContext`.

`DataProtectionKeys` Klasy na jednostkę przyjmuje strukturę pokazano w poniższej tabeli.

| Właściwości/pola | Typ CLR | Typ SQL              |
| -------------- | -------- | --------------------- |
| `Id`           | `int`    | `int`, Klucz podstawowy, nie ma wartości null   |
| `FriendlyName` | `string` | `nvarchar(MAX)`, o wartości null |
| `Xml`          | `string` | `nvarchar(MAX)`, o wartości null |

::: moniker-end

## <a name="custom-key-repository"></a>Niestandardowe repozytorium klucza

Jeśli mechanizmy wewnętrzne nie są odpowiednie, deweloper może określić ich własny mechanizm trwałość klucza, podając własne [IXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.ixmlrepository).
