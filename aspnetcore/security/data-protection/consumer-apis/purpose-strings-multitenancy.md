---
title: Hierarchia przeznaczenia i Wielodostępność w ASP.NET Core
author: rick-anderson
description: Dowiedz się więcej o hierarchii ciągów przeznaczenie i wielu dzierżawcach, które odnoszą się do ASP.NET Core interfejsów API ochrony danych.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/purpose-strings-multitenancy
ms.openlocfilehash: 1133d40e7b325d58b3f70e7387494dae36ff8ac9
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78664753"
---
# <a name="purpose-hierarchy-and-multi-tenancy-in-aspnet-core"></a>Hierarchia przeznaczenia i Wielodostępność w ASP.NET Core

Ponieważ `IDataProtector` jest również niejawnie `IDataProtectionProvider`, cele mogą być łańcucha ze sobą. W tym sensie `provider.CreateProtector([ "purpose1", "purpose2" ])` jest równoznaczny z `provider.CreateProtector("purpose1").CreateProtector("purpose2")`.

Pozwala to na niektóre interesujące relacje hierarchiczne za pomocą systemu ochrony danych. W poprzednim przykładzie elementu [contoso. Messaging. SecureMessage](xref:security/data-protection/consumer-apis/purpose-strings#data-protection-contoso-purpose)składnik SecureMessage może wywoływać `provider.CreateProtector("Contoso.Messaging.SecureMessage")` po utworzeniu i zbuforowaniu wyniku w polu `_myProvider` prywatnym. Przyszłe funkcje ochrony można następnie tworzyć za pomocą wywołań do `_myProvider.CreateProtector("User: username")`, a te funkcje ochrony zostaną użyte do zabezpieczenia poszczególnych komunikatów.

Można to również przerzucić. Rozważ użycie pojedynczej aplikacji logicznej, która hostuje wiele dzierżawców (w przypadku usługi CMS jest to uzasadnione), a każda dzierżawa może być skonfigurowana z własnym systemem zarządzania uwierzytelnianiem i stanem. Aplikacja parasola ma jednego dostawcę głównego i wywołuje `provider.CreateProtector("Tenant 1")` i `provider.CreateProtector("Tenant 2")`, aby przyznać każdej dzierżawy swój odizolowany wycink systemu ochrony danych. Dzierżawcy mogą następnie utworzyć własne osoby chroniące przed własnymi potrzebami, ale bez względu na to, w jaki sposób nie mogą oni tworzyć funkcji ochrony, które kolidują z innymi dzierżawcami w systemie. Graficznie jest to reprezentowane poniżej.

![Cele z wielu dzierżawców](purpose-strings-multitenancy/_static/purposes-multi-tenancy.png)

>[!WARNING]
> Przyjęto założenie, że aplikacja parasola kontroluje, które interfejsy API są dostępne dla poszczególnych dzierżawców, a dzierżawcy nie mogą wykonać dowolnego kodu na serwerze. Jeśli dzierżawca może wykonać dowolny kod, może wykonać prywatne odbicie, aby przerwać gwarancje izolacji lub bezpośrednio odczytać główny materiał kluczający, a także utworzyć wszystkie podklucze, których chcą.

System ochrony danych w rzeczywistości używa sortowania wielu dzierżawców w domyślnej konfiguracji wbudowanej. Domyślnie główny materiał klucza jest przechowywany w folderze profilu użytkownika konta procesu roboczego (lub w rejestrze dla tożsamości puli aplikacji IIS). Jest to jednak dość powszechne, aby użyć jednego konta do uruchamiania wielu aplikacji, a w związku z tym wszystkie te aplikacje spowodują utworzenie głównego materiału klucza. Aby rozwiązać ten problem, system ochrony danych automatycznie wstawia unikatowy identyfikator dla poszczególnych aplikacji jako pierwszy element w łańcuchu ogólnego przeznaczenia. To niejawne przeznaczenie służy do [izolowania poszczególnych aplikacji](xref:security/data-protection/configuration/overview#per-application-isolation) przez efektywne traktowanie każdej aplikacji jako unikatowej dzierżawy w systemie, a proces tworzenia funkcji ochrony jest identyczny z powyższym obrazem.
