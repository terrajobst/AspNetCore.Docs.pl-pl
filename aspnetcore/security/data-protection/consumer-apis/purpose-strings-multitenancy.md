---
title: Hierarchia celów i obsługa wielu dzierżawców w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się więcej o hierarchia ciągu celów i obsługa wielu dzierżawców w odniesieniu do interfejsów API do ochrony danych usługi ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/purpose-strings-multitenancy
ms.openlocfilehash: 1133d40e7b325d58b3f70e7387494dae36ff8ac9
ms.sourcegitcommit: fd461c60b5e36c7019f81da0138cc859d0fddaa2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/10/2018
ms.locfileid: "41752460"
---
# <a name="purpose-hierarchy-and-multi-tenancy-in-aspnet-core"></a>Hierarchia celów i obsługa wielu dzierżawców w programie ASP.NET Core

Ponieważ `IDataProtector` jest również niejawnie `IDataProtectionProvider`, celów można łączyć w łańcuch. W tym sensie `provider.CreateProtector([ "purpose1", "purpose2" ])` jest odpowiednikiem `provider.CreateProtector("purpose1").CreateProtector("purpose2")`.

Dzięki temu w przypadku niektórych interesujące relacji hierarchicznych za pośrednictwem systemu ochrony danych. We wcześniejszym przykładzie z [Contoso.Messaging.SecureMessage](xref:security/data-protection/consumer-apis/purpose-strings#data-protection-contoso-purpose), można wywołać składnika SecureMessage `provider.CreateProtector("Contoso.Messaging.SecureMessage")` raz ponoszonych z góry i zbuforuj wynik do prywatnej `_myProvider` pola. Funkcje ochrony kluczy w przyszłości można będzie utworzyć za pomocą wywołania `_myProvider.CreateProtector("User: username")`, a te funkcje ochrony kluczy będzie używana do zabezpieczania poszczególnych wiadomości.

To również można odwrócić. Należy rozważyć jednej aplikacji logicznej hosta, którego można skonfigurować wiele dzierżaw (rozsądne wydaje CMS) i każdego dzierżawcy z własnym systemem uwierzytelniania i stanu zarządzania. Aplikacja parasola ma jednego dostawcy głównego i wywołuje `provider.CreateProtector("Tenant 1")` i `provider.CreateProtector("Tenant 2")` aby dać każdej dzierżawy własnego izolowanego wycinek system ochrony danych. Dzierżawcy mogą następnie pochodzić własne poszczególne funkcje ochrony kluczy na podstawie własnych potrzeb, ale niezależnie od tego, jaką użytkownik podejmie próbę nie mogą tworzyć funkcje ochrony kluczy, które kolidują z innymi dzierżawami w systemie. Graficzne jest reprezentowane poniżej.

![Obsługa wielu dzierżawców celów](purpose-strings-multitenancy/_static/purposes-multi-tenancy.png)

>[!WARNING]
> Przy założeniu, parasola kontrolki aplikacji, które interfejsy API są dostępne dla poszczególnych dzierżawców i dzierżawcy nie można wykonać dowolnego kodu na serwerze. Jeśli Dzierżawca może wykonywać dowolny kod, wykonują prywatnej odbicia przerwanie gwarancje izolacji lub można po prostu bezpośrednio odczytywać materiał klucza głównego i pochodzić z dowolnych podkluczy działanie.

System ochrony danych faktycznie używa sortowania wielu dzierżawców w domyślnej konfiguracji poza pole. Domyślnie materiał klucza głównego są przechowywane w folderze profilu użytkownika konta procesu roboczego (lub rejestru dla tożsamości puli aplikacji IIS). Ale rzeczywiście dość często jest używać jednego konta do uruchomienia wielu aplikacji, a zatem te aplikacje pojawiłyby udostępnianie wzorzec materiału klucza. Aby rozwiązać ten problem, system ochrony danych automatycznie wstawia identyfikator unikatowy dla aplikacji jako pierwszy element w łańcuchu ogólnego przeznaczenia. W tym celu niejawne służy do [izolowania aplikacji poszczególnych](xref:security/data-protection/configuration/overview#per-application-isolation) od siebie nawzajem skutecznie traktując każdej aplikacji jako unikatowy dzierżawy w ramach systemu i proces tworzenia ochrony wygląda identyczne z powyższej ilustracji.
