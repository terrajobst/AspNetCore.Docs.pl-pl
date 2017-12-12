---
title: "Iniekcji zależności w obsłudze wymaganie"
author: rick-anderson
description: "W tym dokumencie przedstawiono sposób dodanie obsługi wymagań autoryzacji do aplikacji platformy ASP.NET Core za pomocą iniekcji zależności."
keywords: "Platformy ASP.NET Core iniekcji zależności, program obsługi autoryzacji"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 5fb6625c-173a-4feb-8380-73c9844dc23c
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/dependencyinjection
ms.openlocfilehash: b5e590cc63387553af7385b611cdf8cd6b255db7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
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
