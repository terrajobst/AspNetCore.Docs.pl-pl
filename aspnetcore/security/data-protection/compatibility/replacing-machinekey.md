---
title: "Zastępowanie `<machineKey>` w programie ASP.NET"
author: rick-anderson
description: "Zastępowanie `<machineKey>` w programie ASP.NET"
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/compatibility/replacing-machinekey
ms.openlocfilehash: 865c949a526e07d41ac4627fdf0411d5422adc16
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2018
---
# <a name="replacing-machinekey-in-aspnet"></a><span data-ttu-id="3cccb-103">Zastępowanie `<machineKey>` w programie ASP.NET</span><span class="sxs-lookup"><span data-stu-id="3cccb-103">Replacing `<machineKey>` in ASP.NET</span></span>

<a name="compatibility-replacing-machinekey"></a>

<span data-ttu-id="3cccb-104">Implementacja `<machineKey>` elementu w programie ASP.NET [jest wymienny](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/).</span><span class="sxs-lookup"><span data-stu-id="3cccb-104">The implementation of the `<machineKey>` element in ASP.NET [is replaceable](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/).</span></span> <span data-ttu-id="3cccb-105">Dzięki temu większość wywołania procedur kryptograficznych ASP.NET go przekierować przez mechanizm wymiany danych ochrony, łącznie z nowego systemu ochrony danych.</span><span class="sxs-lookup"><span data-stu-id="3cccb-105">This allows most calls to ASP.NET cryptographic routines to be routed through a replacement data protection mechanism, including the new data protection system.</span></span>

## <a name="package-installation"></a><span data-ttu-id="3cccb-106">Instalacja pakietu</span><span class="sxs-lookup"><span data-stu-id="3cccb-106">Package installation</span></span>

> [!NOTE]
> <span data-ttu-id="3cccb-107">Nowy system ochrony danych można tylko zainstalowana w istniejącej aplikacji programu ASP.NET przeznaczonych dla platformy .NET 4.5.1 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="3cccb-107">The new data protection system can only be installed into an existing ASP.NET application targeting .NET 4.5.1 or higher.</span></span> <span data-ttu-id="3cccb-108">Instalacja będzie się niepowodzeniem, jeśli aplikacja jest przeznaczony dla platformy .NET 4.5 lub niższy.</span><span class="sxs-lookup"><span data-stu-id="3cccb-108">Installation will fail if the application targets .NET 4.5 or lower.</span></span>

<span data-ttu-id="3cccb-109">Aby zainstalować nowy system ochrony danych do istniejącego projektu 4.5.1+ ASP.NET, należy zainstalować pakiet Microsoft.AspNetCore.DataProtection.SystemWeb.</span><span class="sxs-lookup"><span data-stu-id="3cccb-109">To install the new data protection system into an existing ASP.NET 4.5.1+ project, install the package Microsoft.AspNetCore.DataProtection.SystemWeb.</span></span> <span data-ttu-id="3cccb-110">To spowoduje utworzenie wystąpienia użycie systemu ochrony danych [domyślnej konfiguracji](xref:security/data-protection/configuration/default-settings) ustawienia.</span><span class="sxs-lookup"><span data-stu-id="3cccb-110">This will instantiate the data protection system using the [default configuration](xref:security/data-protection/configuration/default-settings) settings.</span></span>

<span data-ttu-id="3cccb-111">Po zainstalowaniu pakietu do wstawienia wiersza do *Web.config* , który zawiadamia ASP.NET, aby użyć go do [większości operacji kryptograficznych](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/), w tym uwierzytelniania formularzy, stan widoku i wywołań MachineKey.Protect.</span><span class="sxs-lookup"><span data-stu-id="3cccb-111">When you install the package, it inserts a line into *Web.config* that tells ASP.NET to use it for [most cryptographic operations](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/), including forms authentication, view state, and calls to MachineKey.Protect.</span></span> <span data-ttu-id="3cccb-112">Wiersz wstawiony odczytuje w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="3cccb-112">The line that's inserted reads as follows.</span></span>

```xml
<machineKey compatibilityMode="Framework45" dataProtectorType="..." />
```

>[!TIP]
> <span data-ttu-id="3cccb-113">Można określić, czy nowy system ochrony danych jest aktywna, sprawdzając pól `__VIEWSTATE`, którego powinny rozpoczynać się od "CfDJ8" tak jak w poniższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="3cccb-113">You can tell if the new data protection system is active by inspecting fields like `__VIEWSTATE`, which should begin with "CfDJ8" as in the example below.</span></span> <span data-ttu-id="3cccb-114">"CfDJ8" jest reprezentacja base64 nagłówek magic "09 F0 C9 F0", który identyfikuje ładunku chronionego przez system ochrony danych.</span><span class="sxs-lookup"><span data-stu-id="3cccb-114">"CfDJ8" is the base64 representation of the magic "09 F0 C9 F0" header that identifies a payload protected by the data protection system.</span></span>

```html
<input type="hidden" name="__VIEWSTATE" id="__VIEWSTATE" value="CfDJ8AWPr2EQPTBGs3L2GCZOpk..." />
```

## <a name="package-configuration"></a><span data-ttu-id="3cccb-115">Konfiguracja pakietu</span><span class="sxs-lookup"><span data-stu-id="3cccb-115">Package configuration</span></span>

<span data-ttu-id="3cccb-116">System ochrony danych zostanie uruchomiony z domyślnej konfiguracji instalacji zero.</span><span class="sxs-lookup"><span data-stu-id="3cccb-116">The data protection system is instantiated with a default zero-setup configuration.</span></span> <span data-ttu-id="3cccb-117">Jednak ponieważ domyślnie klucze są zapisywane w lokalnym systemie plików, to nie będzie działać dla aplikacji, które zostały wdrożone w farmie.</span><span class="sxs-lookup"><span data-stu-id="3cccb-117">However, since by default keys are persisted to the local file system, this won't work for applications which are deployed in a farm.</span></span> <span data-ttu-id="3cccb-118">Aby rozwiązać ten problem, można podać konfigurację przez tworzenie podklas, które DataProtectionStartup typu i zastępuje jego metodę ConfigureServices.</span><span class="sxs-lookup"><span data-stu-id="3cccb-118">To resolve this, you can provide configuration by creating a type which subclasses DataProtectionStartup and overrides its ConfigureServices method.</span></span>

<span data-ttu-id="3cccb-119">Poniżej znajduje się przykład typ uruchomienia ochrony danych niestandardowych, który skonfigurowano zarówno której klucze są zachowywane i sposób ich są szyfrowane, gdy.</span><span class="sxs-lookup"><span data-stu-id="3cccb-119">Below is an example of a custom data protection startup type which configured both where keys are persisted and how they're encrypted at rest.</span></span> <span data-ttu-id="3cccb-120">Zastępuje również domyślne zasady izolacji aplikacji, zapewniając własną nazwę aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3cccb-120">It also overrides the default app isolation policy by providing its own application name.</span></span>

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
> <span data-ttu-id="3cccb-121">Można również użyć `<machineKey applicationName="my-app" ... />` zamiast jawnym wywołaniem SetApplicationName.</span><span class="sxs-lookup"><span data-stu-id="3cccb-121">You can also use `<machineKey applicationName="my-app" ... />` in place of an explicit call to SetApplicationName.</span></span> <span data-ttu-id="3cccb-122">Jest to mechanizm wygody, aby uniknąć wymuszania deweloperom tworzenie typu pochodnego DataProtectionStartup, jeśli został wszystkich chcieli skonfigurować ustawienie nazwy aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3cccb-122">This is a convenience mechanism to avoid forcing the developer to create a DataProtectionStartup-derived type if all they wanted to configure was setting the application name.</span></span>

<span data-ttu-id="3cccb-123">Aby włączyć to Konfiguracja niestandardowa, przejdź wstecz do pliku Web.config i wyszukaj `<appSettings>` element zainstalować pakiet dodane do pliku konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="3cccb-123">To enable this custom configuration, go back to Web.config and look for the `<appSettings>` element that the package install added to the config file.</span></span> <span data-ttu-id="3cccb-124">Będzie wyglądać następujący kod:</span><span class="sxs-lookup"><span data-stu-id="3cccb-124">It will look like the following markup:</span></span>

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

<span data-ttu-id="3cccb-125">Wypełnij puste wartości z kwalifikowaną dla zestawu nazwę typu pochodnego DataProtectionStartup, utworzony.</span><span class="sxs-lookup"><span data-stu-id="3cccb-125">Fill in the blank value with the assembly-qualified name of the DataProtectionStartup-derived type you just created.</span></span> <span data-ttu-id="3cccb-126">Jeśli nazwa aplikacji jest DataProtectionDemo, będzie wyglądać podobnie jak poniżej.</span><span class="sxs-lookup"><span data-stu-id="3cccb-126">If the name of the application is DataProtectionDemo, this would look like the below.</span></span>

```xml
<add key="aspnet:dataProtectionStartupType"
     value="DataProtectionDemo.MyDataProtectionStartup, DataProtectionDemo" />
```

<span data-ttu-id="3cccb-127">System ochrony danych nowo skonfigurowane jest teraz gotowa do użycia w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3cccb-127">The newly-configured data protection system is now ready for use inside the application.</span></span>
