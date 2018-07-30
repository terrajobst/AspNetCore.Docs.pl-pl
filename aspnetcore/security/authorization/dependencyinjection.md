---
title: Wstrzykiwanie zależności w programach obsługi wymagań w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak wstawić programach obsługi wymagań autoryzacji do aplikacji ASP.NET Core przy użyciu iniekcji zależności.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/dependencyinjection
ms.openlocfilehash: 71d563e11d308a95c08e6d012d3a071f4697d2de
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342117"
---
# <a name="dependency-injection-in-requirement-handlers-in-aspnet-core"></a>Wstrzykiwanie zależności w programach obsługi wymagań w programie ASP.NET Core

<a name="security-authorization-di"></a>

[Programy obsługi autoryzacji musi być zarejestrowana](xref:security/authorization/policies#handler-registration) w kolekcji usługi podczas konfigurowania (przy użyciu [wstrzykiwanie zależności](xref:fundamentals/dependency-injection)).

Załóżmy, że masz repozytorium reguł, który chcesz ocenić wewnątrz procedury obsługi autoryzacji i tego repozytorium został zarejestrowany w kolekcji usługi. Autoryzacja i rozpoznawania wstrzyknąć, do konstruktora.

Na przykład, jeśli chcesz użyć ASP. NET przez rejestrowanie infrastruktury, które chcesz wstawić `ILoggerFactory` do programu obsługi. Program obsługi może wyglądać jak:

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

Czy zarejestrować program obsługi przy użyciu `services.AddSingleton()`:

```csharp
services.AddSingleton<IAuthorizationHandler, LoggingAuthorizationHandler>();
```

Wystąpienie zostanie obsługi można utworzyć podczas uruchamiania aplikacji i będzie DI wstrzyknąć zarejestrowaną `ILoggerFactory` do konstruktora.

> [!NOTE]
> Programy obsługi, które używają programu Entity Framework nie może zostać zarejestrowana jako pojedynczych wystąpień.
