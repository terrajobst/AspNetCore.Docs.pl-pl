---
title: Metodyka DevOps z platformą ASP.NET Core i platformy Azure | Następne kroki
author: CamSoper
description: Przewodnik, który dostarcza wskazówki end-to-end na tworzeniu potoku metodyki DevOps dla aplikacji ASP.NET Core hostowanych na platformie Azure.
ms.author: casoper
ms.custom: mvc
ms.date: 10/24/2018
uid: azure/devops/next-steps
ms.openlocfilehash: b82e7251b507f8d141930673d50722cfaa576db5
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50089883"
---
# <a name="next-steps"></a>Następne kroki

W tym przewodniku opisano tworzenie potoku metodyki DevOps dla przykładowej aplikacji platformy ASP.NET Core. Gratulacje! Mamy nadzieję, że podoba Ci się najbardziej uczenia publikowania aplikacji sieci web platformy ASP.NET Core w usłudze Azure App Service i automatyzowanie ciągłej integracji zmian.

Poza hosting sieci web i metodyki DevOps platforma Azure oferuje szeroką gamę usług Platform-as-a-Service (PaaS) jest przydatny dla deweloperów platformy ASP.NET Core. Ta sekcja zawiera krótkie omówienie niektórych najczęściej używanych usług.

## <a name="storage-and-databases"></a>Magazynowi i bazom danych

[Pamięć podręczna redis](/azure/redis-cache/) to dane o wysokiej przepływności, małego opóźnienia, buforowanie dostępna jako usługa. Może służyć do buforowania danych wyjściowych strony, zmniejszenie żądań bazy danych i zapewnianie stanu sesji platformy ASP.NET Core w wielu wystąpieniach aplikacji.

[Usługa Azure Storage](/azure/storage/) jest magazyn w chmurze skalowalności platformy Azure. Deweloperzy mogą korzystać z [usługi Queue Storage](/azure/storage/queues/storage-queues-introduction) niezawodnej usługi kolejkowania i [Table Storage](/azure/storage/tables/table-storage-overview) jest parach klucz wartość NoSQL, zaprojektowane do szybkiego opracowywania ogromnych, częściową strukturą zestawów danych.

[Usługa Azure SQL Database](/azure/sql-database/) zapewnia funkcje znanych relacyjnej bazy danych jako usługa korzystająca z aparatu Microsoft SQL Server.

[Usługa cosmos DB](/azure/cosmos-db/) globalnie dystrybuowana, wielomodelowa usługa bazy danych NoSQL. Wiele interfejsów API dostępnych w tym interfejsu API SQL (dawniej nazywanych bazy danych DocumentDB), bazy danych Cassandra i bazy danych MongoDB.

## <a name="identity"></a>Tożsamość

[Usługa Azure Active Directory](/azure/active-directory/) i [usługi Azure Active Directory B2C](/azure/active-directory-b2c/) są obie te usługi tożsamości. Usługa Azure Active Directory jest przeznaczona dla scenariuszy dla przedsiębiorstw i do pracy zespołowej usługi Azure AD B2B (business-to-business), podczas gdy usługi Azure Active Directory B2C jest zamierzony scenariuszy biznesowych do klienta, w tym logowania się w sieci społecznościowej.

## <a name="mobile"></a>Telefon komórkowy

[Usługa Notification Hubs](/azure/notification-hubs/) jest aparatem wieloplatformowe i skalowalne powiadomienia wypychane, aby szybko wysyłać miliony wiadomości do aplikacji działających na urządzeniach różnych typów.

## <a name="web-infrastructure"></a>Infrastruktura sieci Web

[Usługa Azure Container Service](/azure/aks/) zarządza hostowanym środowiskiem Kubernetes, pozwalając szybko i łatwo wdrażać i zarządzać konteneryzowanych aplikacji bez doświadczenia z organizowaniem kontenerów.

[Usługa Azure Search](/azure/search/) służy do tworzenia wyszukiwania przedsiębiorstwa za pośrednictwem prywatną, heterogeniczną zawartość.

[Usługa Service Fabric](/azure/service-fabric/) jest to platforma systemów rozproszonych ułatwiająca pakowanie, wdrażanie i zarządzanie skalowalnych i niezawodnych mikrousług i kontenerów.
