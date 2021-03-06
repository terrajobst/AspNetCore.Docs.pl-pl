---
title: Zastąp ASP.NET machineKey w ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak zastąpić machineKey w ASP.NET, aby umożliwić korzystanie z nowego i bezpieczniejszego systemu ochrony danych.
ms.author: riande
ms.date: 04/06/2019
uid: security/data-protection/compatibility/replacing-machinekey
ms.openlocfilehash: 2317cb50cfe63226baf336ebfc5d681d1cebe5c6
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78667987"
---
# <a name="replace-the-aspnet-machinekey-in-aspnet-core"></a>Zastąp ASP.NET machineKey w ASP.NET Core

<a name="compatibility-replacing-machinekey"></a>

Implementacja `<machineKey>` elementu w ASP.NET [jest wymienna](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/). Umożliwia to większości wywołań ASP.NET procedur kryptograficznych, które mają być kierowane przez mechanizm ochrony danych zamiennych, w tym nowy system ochrony danych.

## <a name="package-installation"></a>Instalacja pakietu

> [!NOTE]
> Nowy system ochrony danych można zainstalować tylko w istniejącej aplikacji ASP.NET, w której znajduje się program .NET 4.5.1 lub nowszy. Instalacja zakończy się niepowodzeniem, jeśli aplikacja jest przeznaczona dla platformy .NET 4,5 lub niższej.

Aby zainstalować nowy system ochrony danych w istniejącym projekcie ASP.NET 4.5.1 +, zainstaluj pakiet Microsoft. AspNetCore. dataprotection. SystemWeb. Spowoduje to utworzenie wystąpienia systemu ochrony danych przy użyciu [domyślnych ustawień konfiguracji](xref:security/data-protection/configuration/default-settings) .

Po zainstalowaniu pakietu wstawia wiersz do *pliku Web. config* , który informuje ASP.NET o tym, aby użyć go do [większości operacji kryptograficznych](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/), w tym uwierzytelniania formularzy, stanu widoku i wywołań machineKey. Protect. Wstawiony wiersz odczytuje się w następujący sposób.

```xml
<machineKey compatibilityMode="Framework45" dataProtectorType="..." />
```

>[!TIP]
> Możesz sprawdzić, czy nowy system ochrony danych jest aktywny, sprawdzając pola, takie jak `__VIEWSTATE`, które powinny rozpoczynać się od ciągu "CfDJ8", jak w poniższym przykładzie. "CfDJ8" to reprezentacja Base64 nagłówka Magic "09 F0 C9 F0", który identyfikuje ładunek chroniony przez system ochrony danych.

```html
<input type="hidden" name="__VIEWSTATE" id="__VIEWSTATE" value="CfDJ8AWPr2EQPTBGs3L2GCZOpk...">
```

## <a name="package-configuration"></a>Konfiguracja pakietu

System ochrony danych jest inicjowany z domyślną konfiguracją instalacji zerowej. Ponieważ jednak domyślnie klucze są utrwalane w lokalnym systemie plików, nie będzie to konieczne w przypadku aplikacji wdrożonych w farmie. Aby rozwiązać ten problem, można zapewnić konfigurację, tworząc typ, który podklasy DataProtectionStartup i przesłania metodę ConfigureServices.

Poniżej znajduje się przykład niestandardowego typu uruchamiania ochrony danych, który został skonfigurowany zarówno w przypadku, gdy klucze są utrwalane, jak i w sposób szyfrowany. Zastępuje ono również domyślne zasady izolacji aplikacji, podając własną nazwę aplikacji.

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
> Zamiast jawnie wywołać metody setapplicationname, można również użyć `<machineKey applicationName="my-app" ... />`. Jest to wygodny mechanizm pozwalający uniknąć wymuszania tworzenia przez dewelopera typu DataProtectionStartup-pochodnego, jeśli wszystko, co chciało skonfigurować, została ustawiona nazwa aplikacji.

Aby włączyć tę konfigurację niestandardową, Wróć do pliku Web. config i poszukaj elementu `<appSettings>`, który został dodany przez pakiet w celu dodania do niego. Będzie wyglądać następująco:

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

Wypełnij wartość pustą, podając kwalifikowaną nazwę zestawu DataProtectionStartup typu pochodnego. Jeśli nazwa aplikacji jest DataProtectionDemo, będzie wyglądać tak jak poniżej.

```xml
<add key="aspnet:dataProtectionStartupType"
     value="DataProtectionDemo.MyDataProtectionStartup, DataProtectionDemo" />
```

Nowo skonfigurowany system ochrony danych jest teraz gotowy do użycia w aplikacji.
