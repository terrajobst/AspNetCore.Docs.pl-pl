---
title: Wstrzykiwanie zależności w programach obsługi wymagań w ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak wstrzyknąć programy obsługi wymagań autoryzacji do aplikacji ASP.NET Core przy użyciu iniekcji zależności.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/dependencyinjection
ms.openlocfilehash: 71d563e11d308a95c08e6d012d3a071f4697d2de
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78666090"
---
# <a name="dependency-injection-in-requirement-handlers-in-aspnet-core"></a>Wstrzykiwanie zależności w programach obsługi wymagań w ASP.NET Core

<a name="security-authorization-di"></a>

[Procedury obsługi autoryzacji muszą być zarejestrowane](xref:security/authorization/policies#handler-registration) w kolekcji usług podczas konfiguracji (przy użyciu [iniekcji zależności](xref:fundamentals/dependency-injection)).

Załóżmy, że masz repozytorium reguł, które chcesz oszacować wewnątrz procedury obsługi autoryzacji i że repozytorium zostało zarejestrowane w kolekcji usług. Autoryzacja zostanie rozwiązany i wstrzyknąć do konstruktora.

Na przykład jeśli chciałeś użyć ASP. Infrastruktura rejestrowania w sieci, którą chcesz wstrzyknąć `ILoggerFactory` do procedury obsługi. Taka procedura obsługi może wyglądać następująco:

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

Procedurę obsługi należy zarejestrować w `services.AddSingleton()`:

```csharp
services.AddSingleton<IAuthorizationHandler, LoggingAuthorizationHandler>();
```

Wystąpienie programu obsługi zostanie utworzone, gdy aplikacja zostanie uruchomiona, a funkcja DI sprawdzi zarejestrowane `ILoggerFactory` w konstruktorze.

> [!NOTE]
> Programy obsługi, które używają Entity Framework nie powinny być rejestrowane jako pojedyncze.
