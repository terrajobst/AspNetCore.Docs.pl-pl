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
# <a name="replace-the-aspnet-machinekey-in-aspnet-core"></a>Zastąp machineKey platformy ASP.NET w programie ASP.NET Core

<a name="compatibility-replacing-machinekey"></a>

Implementacja `<machineKey>` elementu w programie ASP.NET: [wymienną](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/). Dzięki temu większość wywołań procedur kryptograficznych platformy ASP.NET będą kierowane za pośrednictwem mechanizm ochrony danych zastąpienia, w tym nowy system ochrony danych.

## <a name="package-installation"></a>Instalacja pakietu

> [!NOTE]
> Nowy system ochrony danych może być tylko zainstalowany do istniejącej aplikacji ASP.NET przeznaczonych dla platformy .NET 4.5.1 lub nowszej. Instalacja zostanie zakończona niepowodzeniem, jeśli aplikacja jest przeznaczony dla platformy .NET 4.5 lub starszą.

Aby zainstalować nowy system ochrony danych w istniejącym projekcie 4.5.1+ ASP.NET, należy zainstalować pakiet Microsoft.AspNetCore.DataProtection.SystemWeb. Spowoduje to wystąpienia dane ochrony systemu za pomocą [domyślnej konfiguracji](xref:security/data-protection/configuration/default-settings) ustawienia.

Podczas instalowania pakietu do wstawienia wiersza do *Web.config* który informuje platformę ASP.NET w celu zastosowania [większości operacji kryptograficznych](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/), w tym uwierzytelnianie formularzy, stan widoku i wywołania MachineKey.Protect. Wiersz, który jest wstawiany odczytuje w następujący sposób.

```xml
<machineKey compatibilityMode="Framework45" dataProtectorType="..." />
```

>[!TIP]
> Można stwierdzić, czy nowy system ochrony danych jest aktywna, sprawdzając pól, takich jak `__VIEWSTATE`, który powinien zaczynać się od "CfDJ8" tak jak w poniższym przykładzie. "CfDJ8" jest reprezentacji base64 nagłówka magic "09 F0 C9 F0", który identyfikuje ładunek chroniona przez system ochrony danych.

```html
<input type="hidden" name="__VIEWSTATE" id="__VIEWSTATE" value="CfDJ8AWPr2EQPTBGs3L2GCZOpk...">
```

## <a name="package-configuration"></a>Konfiguracja pakietu

System ochrony danych jest utworzone za pomocą domyślnej konfiguracji instalacji zero. Jednak ponieważ domyślnie klucze zostaną utrwalone w lokalnym systemie plików, to nie będzie działać dla aplikacji, które są wdrażane w farmie. Aby rozwiązać ten problem, może zapewnić konfiguracji przez utworzenie typu, który podklasy DataProtectionStartup i przesłania jej metodę ConfigureServices.

Poniżej znajduje się przykład typ uruchomienia ochrony danych niestandardowych, które skonfigurowane, gdzie klucze są zachowywane i jak są szyfrowane w stanie spoczynku. Zastępuje również domyślne zasady izolacji aplikacji, podając własną nazwę aplikacji.

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
> Można również użyć `<machineKey applicationName="my-app" ... />` zamiast jawnym wywołaniem SetApplicationName. Jest to mechanizm wygody, aby uniknąć wymuszania dla deweloperów, aby utworzyć DataProtectionStartup stosowanego typu pochodnego, jeśli został wszystko, czego użytkownik chce skonfigurować ustawienie nazwy aplikacji.

Aby włączyć ten niestandardowej konfiguracji, wróć do pliku Web.config i poszukaj `<appSettings>` element, który zainstaluj pakiet dodawane do pliku konfiguracji. Będzie to wyglądać następujące znaczniki:

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

Wprowadź wartość pustą nazwą kwalifikowaną dla zestawu typu pochodnego DataProtectionStartup, właśnie utworzony. Jeśli nazwa aplikacji jest DataProtectionDemo, to będzie wyglądać poniżej.

```xml
<add key="aspnet:dataProtectionStartupType"
     value="DataProtectionDemo.MyDataProtectionStartup, DataProtectionDemo" />
```

System ochrony danych nowo skonfigurowane jest teraz gotowy do użycia w aplikacji.
