---
title: Zastąp machineKey platformy ASP.NET w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak zastąpić elementu machineKey w programie ASP.NET, aby zezwolić na korzystanie z systemu ochrony danych na nowe i bardziej bezpieczne.
ms.author: riande
ms.date: 04/06/2019
uid: security/data-protection/compatibility/replacing-machinekey
ms.openlocfilehash: 2317cb50cfe63226baf336ebfc5d681d1cebe5c6
ms.sourcegitcommit: 948e533e02c2a7cb6175ada20b2c9cabb7786d0b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/10/2019
ms.locfileid: "59468569"
---
# <a name="replace-the-aspnet-machinekey-in-aspnet-core"></a><span data-ttu-id="c1550-103">Zastąp machineKey platformy ASP.NET w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c1550-103">Replace the ASP.NET machineKey in ASP.NET Core</span></span>

<a name="compatibility-replacing-machinekey"></a>

<span data-ttu-id="c1550-104">Implementacja `<machineKey>` elementu w programie ASP.NET: [wymienną](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/).</span><span class="sxs-lookup"><span data-stu-id="c1550-104">The implementation of the `<machineKey>` element in ASP.NET [is replaceable](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/).</span></span> <span data-ttu-id="c1550-105">Dzięki temu większość wywołań procedur kryptograficznych platformy ASP.NET będą kierowane za pośrednictwem mechanizm ochrony danych zastąpienia, w tym nowy system ochrony danych.</span><span class="sxs-lookup"><span data-stu-id="c1550-105">This allows most calls to ASP.NET cryptographic routines to be routed through a replacement data protection mechanism, including the new data protection system.</span></span>

## <a name="package-installation"></a><span data-ttu-id="c1550-106">Instalacja pakietu</span><span class="sxs-lookup"><span data-stu-id="c1550-106">Package installation</span></span>

> [!NOTE]
> <span data-ttu-id="c1550-107">Nowy system ochrony danych może być tylko zainstalowany do istniejącej aplikacji ASP.NET przeznaczonych dla platformy .NET 4.5.1 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="c1550-107">The new data protection system can only be installed into an existing ASP.NET application targeting .NET 4.5.1 or later.</span></span> <span data-ttu-id="c1550-108">Instalacja zostanie zakończona niepowodzeniem, jeśli aplikacja jest przeznaczony dla platformy .NET 4.5 lub starszą.</span><span class="sxs-lookup"><span data-stu-id="c1550-108">Installation will fail if the application targets .NET 4.5 or lower.</span></span>

<span data-ttu-id="c1550-109">Aby zainstalować nowy system ochrony danych w istniejącym projekcie 4.5.1+ ASP.NET, należy zainstalować pakiet Microsoft.AspNetCore.DataProtection.SystemWeb.</span><span class="sxs-lookup"><span data-stu-id="c1550-109">To install the new data protection system into an existing ASP.NET 4.5.1+ project, install the package Microsoft.AspNetCore.DataProtection.SystemWeb.</span></span> <span data-ttu-id="c1550-110">Spowoduje to wystąpienia dane ochrony systemu za pomocą [domyślnej konfiguracji](xref:security/data-protection/configuration/default-settings) ustawienia.</span><span class="sxs-lookup"><span data-stu-id="c1550-110">This will instantiate the data protection system using the [default configuration](xref:security/data-protection/configuration/default-settings) settings.</span></span>

<span data-ttu-id="c1550-111">Podczas instalowania pakietu do wstawienia wiersza do *Web.config* który informuje platformę ASP.NET w celu zastosowania [większości operacji kryptograficznych](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/), w tym uwierzytelnianie formularzy, stan widoku i wywołania MachineKey.Protect.</span><span class="sxs-lookup"><span data-stu-id="c1550-111">When you install the package, it inserts a line into *Web.config* that tells ASP.NET to use it for [most cryptographic operations](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/), including forms authentication, view state, and calls to MachineKey.Protect.</span></span> <span data-ttu-id="c1550-112">Wiersz, który jest wstawiany odczytuje w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="c1550-112">The line that's inserted reads as follows.</span></span>

```xml
<machineKey compatibilityMode="Framework45" dataProtectorType="..." />
```

>[!TIP]
> <span data-ttu-id="c1550-113">Można stwierdzić, czy nowy system ochrony danych jest aktywna, sprawdzając pól, takich jak `__VIEWSTATE`, który powinien zaczynać się od "CfDJ8" tak jak w poniższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="c1550-113">You can tell if the new data protection system is active by inspecting fields like `__VIEWSTATE`, which should begin with "CfDJ8" as in the example below.</span></span> <span data-ttu-id="c1550-114">"CfDJ8" jest reprezentacji base64 nagłówka magic "09 F0 C9 F0", który identyfikuje ładunek chroniona przez system ochrony danych.</span><span class="sxs-lookup"><span data-stu-id="c1550-114">"CfDJ8" is the base64 representation of the magic "09 F0 C9 F0" header that identifies a payload protected by the data protection system.</span></span>

```html
<input type="hidden" name="__VIEWSTATE" id="__VIEWSTATE" value="CfDJ8AWPr2EQPTBGs3L2GCZOpk...">
```

## <a name="package-configuration"></a><span data-ttu-id="c1550-115">Konfiguracja pakietu</span><span class="sxs-lookup"><span data-stu-id="c1550-115">Package configuration</span></span>

<span data-ttu-id="c1550-116">System ochrony danych jest utworzone za pomocą domyślnej konfiguracji instalacji zero.</span><span class="sxs-lookup"><span data-stu-id="c1550-116">The data protection system is instantiated with a default zero-setup configuration.</span></span> <span data-ttu-id="c1550-117">Jednak ponieważ domyślnie klucze zostaną utrwalone w lokalnym systemie plików, to nie będzie działać dla aplikacji, które są wdrażane w farmie.</span><span class="sxs-lookup"><span data-stu-id="c1550-117">However, since by default keys are persisted to the local file system, this won't work for applications which are deployed in a farm.</span></span> <span data-ttu-id="c1550-118">Aby rozwiązać ten problem, może zapewnić konfiguracji przez utworzenie typu, który podklasy DataProtectionStartup i przesłania jej metodę ConfigureServices.</span><span class="sxs-lookup"><span data-stu-id="c1550-118">To resolve this, you can provide configuration by creating a type which subclasses DataProtectionStartup and overrides its ConfigureServices method.</span></span>

<span data-ttu-id="c1550-119">Poniżej znajduje się przykład typ uruchomienia ochrony danych niestandardowych, które skonfigurowane, gdzie klucze są zachowywane i jak są szyfrowane w stanie spoczynku.</span><span class="sxs-lookup"><span data-stu-id="c1550-119">Below is an example of a custom data protection startup type which configured both where keys are persisted and how they're encrypted at rest.</span></span> <span data-ttu-id="c1550-120">Zastępuje również domyślne zasady izolacji aplikacji, podając własną nazwę aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c1550-120">It also overrides the default app isolation policy by providing its own application name.</span></span>

```csharp
using System;
using System.IO;
using Microsoft.AspNetCore.DataProtection;
using Microsoft.AspNetCore.DataProtection.SystemWeb;
using Microsoft.Extensions.DependencyInjection;

namespace DataProtectionDemo
{
    public class MyDataProtectionStartup : DataProtectionStartup
    {
        public override void ConfigureServices(IServiceCollection services)
        {
            services.AddDataProtection()
                .SetApplicationName("my-app")
                .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\myapp-keys\"))
                .ProtectKeysWithCertificate("thumbprint");
        }
    }
}
```

>[!TIP]
> <span data-ttu-id="c1550-121">Można również użyć `<machineKey applicationName="my-app" ... />` zamiast jawnym wywołaniem SetApplicationName.</span><span class="sxs-lookup"><span data-stu-id="c1550-121">You can also use `<machineKey applicationName="my-app" ... />` in place of an explicit call to SetApplicationName.</span></span> <span data-ttu-id="c1550-122">Jest to mechanizm wygody, aby uniknąć wymuszania dla deweloperów, aby utworzyć DataProtectionStartup stosowanego typu pochodnego, jeśli został wszystko, czego użytkownik chce skonfigurować ustawienie nazwy aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c1550-122">This is a convenience mechanism to avoid forcing the developer to create a DataProtectionStartup-derived type if all they wanted to configure was setting the application name.</span></span>

<span data-ttu-id="c1550-123">Aby włączyć ten niestandardowej konfiguracji, wróć do pliku Web.config i poszukaj `<appSettings>` element, który zainstaluj pakiet dodawane do pliku konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="c1550-123">To enable this custom configuration, go back to Web.config and look for the `<appSettings>` element that the package install added to the config file.</span></span> <span data-ttu-id="c1550-124">Będzie to wyglądać następujące znaczniki:</span><span class="sxs-lookup"><span data-stu-id="c1550-124">It will look like the following markup:</span></span>

```xml
<appSettings>
  <!--
  If you want to customize the behavior of the ASP.NET Core Data Protection stack, set the
  "aspnet:dataProtectionStartupType" switch below to be the fully-qualified name of a
  type which subclasses Microsoft.AspNetCore.DataProtection.SystemWeb.DataProtectionStartup.
  -->
  <add key="aspnet:dataProtectionStartupType" value="" />
</appSettings>
```

<span data-ttu-id="c1550-125">Wprowadź wartość pustą nazwą kwalifikowaną dla zestawu typu pochodnego DataProtectionStartup, właśnie utworzony.</span><span class="sxs-lookup"><span data-stu-id="c1550-125">Fill in the blank value with the assembly-qualified name of the DataProtectionStartup-derived type you just created.</span></span> <span data-ttu-id="c1550-126">Jeśli nazwa aplikacji jest DataProtectionDemo, to będzie wyglądać poniżej.</span><span class="sxs-lookup"><span data-stu-id="c1550-126">If the name of the application is DataProtectionDemo, this would look like the below.</span></span>

```xml
<add key="aspnet:dataProtectionStartupType"
     value="DataProtectionDemo.MyDataProtectionStartup, DataProtectionDemo" />
```

<span data-ttu-id="c1550-127">System ochrony danych nowo skonfigurowane jest teraz gotowy do użycia w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c1550-127">The newly-configured data protection system is now ready for use inside the application.</span></span>
