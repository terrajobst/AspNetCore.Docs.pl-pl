---
title: "Iniekcji zależności w obsłudze wymaganie"
author: rick-anderson
description: "W tym dokumencie przedstawiono sposób dodanie obsługi wymagań autoryzacji do aplikacji platformy ASP.NET Core za pomocą iniekcji zależności."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/dependencyinjection
ms.openlocfilehash: 1b7506b49109264a8c628ea2e39ded9f5ace95d3
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2018
---
# <a name="dependency-injection-in-requirement-handlers"></a>Iniekcji zależności w obsłudze wymaganie

<a name="security-authorization-di"></a>

[Programy obsługi autoryzacji musi być zarejestrowana](policies.md#handler-registration) w kolekcji usługi podczas konfigurowania (przy użyciu [iniekcji zależności](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection)).

Załóżmy, że masz repozytorium reguł, który chcesz ocenić wewnątrz obsługi autoryzacji i tego repozytorium został zarejestrowany w kolekcji usługi. Autoryzacja i rozpoznawania iniekcję który do Twojej konstruktora.

Na przykład, jeśli chcesz użyć ASP. NET do rejestrowania infrastruktury chcesz wstawić `ILoggerFactory` do programu obsługi. Program obsługi może wyglądać tak:

```csharp
public class LoggingAuthorizationHandler : AuthorizationHandler<MyRequirement>
   {
       ILogger _logger;

       public LoggingAuthorizationHandler(ILoggerFactory loggerFactory)
       {
           _logger = loggerFactory.CreateLogger(this.GetType().FullName);
       }

       protected override Task HandleRequirementAsync(AuthorizationHandlerContext context, MyRequirement requirement)
       {
           _logger.LogInformation("Inside my handler");
           // Check if the requirement is fulfilled.
           return Task.CompletedTask;
       }
   }
   ```

Może zarejestrować programu obsługi z `services.AddSingleton()`:

```csharp
services.AddSingleton<IAuthorizationHandler, LoggingAuthorizationHandler>();
```

Wystąpienie zostanie obsługi można utworzyć podczas uruchamiania aplikacji i zostanie Podpisane wstrzyknąć zarejestrowaną `ILoggerFactory` do Twojej konstruktora.

> [!NOTE]
> Programy obsługi, które używają programu Entity Framework nie powinien być zarejestrowany jako pojedynczych wystąpień.
