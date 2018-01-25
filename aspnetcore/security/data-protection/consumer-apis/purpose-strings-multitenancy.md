---
title: "Cel ciągów platformy ASP.NET Core"
author: rick-anderson
description: "W tym dokumencie przedstawiono cel ciąg hierarchii i obsługi wielu dzierżawców w powiązaniu z interfejsami API ochrony danych platformy ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/purpose-strings-multitenancy
ms.openlocfilehash: e7b08df32069cf1243630accda82eb6250594c53
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
# <a name="purpose-hierarchy-and-multi-tenancy-in-aspnet-core"></a>Cel hierarchii i obsługi wielu dzierżawców w ASP.NET Core

Ponieważ `IDataProtector` jest również niejawnie `IDataProtectionProvider`, celów można połączyć ze sobą. W tym sensie `provider.CreateProtector([ "purpose1", "purpose2" ])` jest odpowiednikiem `provider.CreateProtector("purpose1").CreateProtector("purpose2")`.

Dzięki temu niektóre ciekawe hierarchicznych za pośrednictwem systemu ochrony danych. W przykładzie wcześniejszych [Contoso.Messaging.SecureMessage](purpose-strings.md#data-protection-contoso-purpose), składnik SecureMessage można wywołać `provider.CreateProtector("Contoso.Messaging.SecureMessage")` raz początkowych i zbuforuj wynik do prywatnej `_myProvide` pola. Przyszłe funkcje ochrony można będzie utworzyć za pomocą wywołania `_myProvider.CreateProtector("User: username")`, a te funkcje ochrony mają być używane do zabezpieczania poszczególnych wiadomości.

To również można odwrócić. Należy rozważyć jednej aplikacji logicznej hosty, które można skonfigurować wiele dzierżaw (rozsądne wydaje system CMS) i każdego dzierżawcy z własnym systemem zarządzania uwierzytelniania i stanu. Aplikacja parasola ma jednego dostawcy wzorca i wywołuje `provider.CreateProtector("Tenant 1")` i `provider.CreateProtector("Tenant 2")` własną izolowanego wycinek systemu ochrony danych umożliwiają każdego dzierżawcy. Dzierżawcy może następnie pochodzi własnych indywidualnych funkcji ochrony na podstawie własnych potrzeb, ale niezależnie od tego, jaką próbują oni nie można utworzyć funkcji ochrony, które powodują kolizję z innymi dzierżawami w systemie. Graficznie jest reprezentowane jako poniżej.

![Obsługa wielu dzierżawców celów](purpose-strings-multitenancy/_static/purposes-multi-tenancy.png)

>[!WARNING]
> Przy założeniu parasola formanty aplikacji, które interfejsy API są dostępne dla poszczególnych dzierżawców i dzierżawcy nie wykonania dowolnego kodu na serwerze. Dzierżawca może uruchomić kod z dowolnego, można wykonują prywatnej odbicia przerwanie gwarancje izolacji, czy można po prostu bezpośrednio odczytywać materiał klucza głównego i pochodzić niezależnie od podkluczy chcą.

System ochrony danych faktycznie używa sortowania wielu dzierżawców w domyślnej konfiguracji poza pole. Domyślnie materiał klucza głównego są przechowywane w folderze profilu konta procesu roboczego użytkownika (lub w rejestrze, dla tożsamości puli aplikacji usług IIS). Ale faktycznie dość często, aby używać jednego konta do uruchamiania wielu aplikacji, a w związku z tym te aplikacje pojawiłyby udostępnianie wzorca materiału klucza. Aby rozwiązać ten problem, system ochrony danych automatycznie wstawia identyfikator unikatowy dla poszczególnych aplikacji jako pierwszy element w łańcuchu ogólnego przeznaczenia. Ten cel niejawne służy do [izolowania aplikacji poszczególnych](xref:security/data-protection/configuration/overview#per-application-isolation) od siebie nawzajem skutecznie traktując każdej aplikacji jako taki sam jak powyższy obraz wygląda unikatowy dzierżawy w systemie i proces tworzenia ochrony.
