---
title: Migrowanie z Microsoft. Extensions. Logging 2,1 do 2,2 lub 3,0
author: pakrym
description: Dowiedz się, jak przeprowadzić migrację aplikacji non-ASP.NET Core, która korzysta z Microsoft. Extensions. rejestrowanie z 2,1 do 2,2 lub 3,0.
ms.author: pakrym
ms.custom: mvc
ms.date: 01/04/2019
uid: migration/logging-nonaspnetcore
ms.openlocfilehash: 2519ddc02cee5978483bcaef4341a52aad3ba2a6
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78659321"
---
# <a name="migrate-from-microsoftextensionslogging-21-to-22-or-30"></a>Migrowanie z Microsoft. Extensions. Logging 2,1 do 2,2 lub 3,0

W tym artykule opisano typowe kroki migrowania aplikacji non-ASP.NET Core, która używa `Microsoft.Extensions.Logging` od 2,1 do 2,2 lub 3,0.

## <a name="21-to-22"></a>Z wersji 2.1 do 2.2

Ręcznie Utwórz `ServiceCollection` i Wywołaj `AddLogging`.

2,1 przykład:

```csharp
using (var loggerFactory = new LoggerFactory())
{
    loggerFactory.AddConsole();

    // use loggerFactory
}
```

2,2 przykład:

```csharp
var serviceCollection = new ServiceCollection();
serviceCollection.AddLogging(builder => builder.AddConsole());

using (var serviceProvider = serviceCollection.BuildServiceProvider())
using (var loggerFactory = serviceProvider.GetService<ILoggerFactory>())
{
    // use loggerFactory
}
```

## <a name="21-to-30"></a>2,1 do 3,0

W 3,0 użyj `LoggingFactory.Create`.

2,1 przykład:

```csharp
using (var loggerFactory = new LoggerFactory())
{
    loggerFactory.AddConsole();

    // use loggerFactory
}
```

3,0 przykład:

```csharp
using (var loggerFactory = LoggerFactory.Create(builder => builder.AddConsole()))
{
    // use loggerFactory
}
```

## <a name="additional-resources"></a>Dodatkowe zasoby

<xref:fundamentals/logging/index>