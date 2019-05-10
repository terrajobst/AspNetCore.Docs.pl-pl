---
title: Migracja z Microsoft.Extensions.Logging 2.1 lub 2.2 lub 3.0
author: pakrym
description: Dowiedz się, jak przeprowadzić migrację aplikacji bez platformy ASP.NET Core, która używa Microsoft.Extensions.Logging z 2.1 2.2 lub 3.0.
ms.author: pakrym
ms.custom: mvc
ms.date: 01/04/2019
uid: migration/logging-nonaspnetcore
ms.openlocfilehash: 2519ddc02cee5978483bcaef4341a52aad3ba2a6
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64898840"
---
# <a name="migrate-from-microsoftextensionslogging-21-to-22-or-30"></a>Migracja z Microsoft.Extensions.Logging 2.1 lub 2.2 lub 3.0

W tym artykule przedstawiono typowe kroki dotyczące migrowania aplikacji bez platformy ASP.NET Core, która używa `Microsoft.Extensions.Logging` z 2.1 2.2 lub 3.0.

## <a name="21-to-22"></a>Z wersji 2.1 do 2.2

Ręczne tworzenie `ServiceCollection` i wywołać `AddLogging`.

przykład 2.1:

```csharp
using (var loggerFactory = new LoggerFactory())
{
    loggerFactory.AddConsole();

    // use loggerFactory
}
```

przykład 2,2:

```csharp
var serviceCollection = new ServiceCollection();
serviceCollection.AddLogging(builder => builder.AddConsole());

using (var serviceProvider = serviceCollection.BuildServiceProvider())
using (var loggerFactory = serviceProvider.GetService<ILoggerFactory>())
{
    // use loggerFactory
}
```

## <a name="21-to-30"></a>2.1 i 3.0

3.0, należy użyć `LoggingFactory.Create`.

przykład 2.1:

```csharp
using (var loggerFactory = new LoggerFactory())
{
    loggerFactory.AddConsole();

    // use loggerFactory
}
```

przykład 3.0:

```csharp
using (var loggerFactory = LoggerFactory.Create(builder => builder.AddConsole()))
{
    // use loggerFactory
}
```

## <a name="additional-resources"></a>Dodatkowe zasoby

<xref:fundamentals/logging/index>