---
title: Następne kroki — metodyki DevOps z platformą ASP.NET Core i platformy Azure
author: CamSoper
description: Dodatkowe ścieżki szkoleniowe dotyczące metodyki DevOps z platformą ASP.NET Core i platformy Azure.
ms.author: casoper
ms.custom: mvc, seodec18
ms.date: 10/24/2018
uid: azure/devops/next-steps
ms.openlocfilehash: a775dc42551a43bcce72b5f9ca364ed00b1dc4e6
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78659475"
---
# <a name="next-steps"></a>Następne kroki

W tym przewodniku opisano tworzenie potoku metodyki DevOps dla przykładowej aplikacji platformy ASP.NET Core. Gratulacje! Mamy nadzieję, że podoba Ci się najbardziej uczenia publikowania aplikacji sieci web platformy ASP.NET Core w usłudze Azure App Service i automatyzowanie ciągłej integracji zmian.

Poza hosting sieci web i metodyki DevOps platforma Azure oferuje szeroką gamę usług Platform-as-a-Service (PaaS) jest przydatny dla deweloperów platformy ASP.NET Core. Ta sekcja zawiera krótkie omówienie niektórych najczęściej używanych usług.

## <a name="storage-and-databases"></a>Magazynowi i bazom danych

[Redis Cache](/azure/redis-cache/) to buforowanie danych o wysokiej przepływności i małych opóźnieniach dostępne jako usługa. Może służyć do buforowania danych wyjściowych strony, zmniejszenie żądań bazy danych i zapewnianie stanu sesji platformy ASP.NET Core w wielu wystąpieniach aplikacji.

[Usługa Azure Storage](/azure/storage/) to wysoce skalowalny magazyn w chmurze na platformie Azure. Deweloperzy mogą korzystać z [queue storage](/azure/storage/queues/storage-queues-introduction) dla niezawodnej usługi kolejkowania komunikatów, a [Table Storage](/azure/storage/tables/table-storage-overview) to NoSQL klucz-wartość magazynu, który umożliwia szybkie tworzenie aplikacji przy użyciu ogromnych zestawów danych z częściową strukturą.

[Azure SQL Database](/azure/sql-database/) zapewnia znane funkcje relacyjnej bazy danych jako usługi korzystające z aparatu Microsoft SQL Server.

[Cosmos DB](/azure/cosmos-db/) globalnie dystrybuowana, wielomodelowa usługa bazy danych NoSQL. Wiele interfejsów API dostępnych w tym interfejsu API SQL (dawniej nazywanych bazy danych DocumentDB), bazy danych Cassandra i bazy danych MongoDB.

## <a name="identity"></a>Tożsamość

[Azure Active Directory](/azure/active-directory/) i [Azure Active Directory B2C](/azure/active-directory-b2c/) są usługami tożsamości. Usługa Azure Active Directory jest przeznaczona dla scenariuszy dla przedsiębiorstw i do pracy zespołowej usługi Azure AD B2B (business-to-business), podczas gdy usługi Azure Active Directory B2C jest zamierzony scenariuszy biznesowych do klienta, w tym logowania się w sieci społecznościowej.

## <a name="mobile"></a>Komórkowy

[Notification Hubs](/azure/notification-hubs/) to wieloplatformowy, Skalowalny aparat powiadomień wypychanych umożliwiający szybkie wysyłanie milionów komunikatów do aplikacji działających na różnych typach urządzeń.

## <a name="web-infrastructure"></a>Infrastruktura sieci Web

[Azure Container Service](/azure/aks/) zarządza hostowanym środowiskiem Kubernetes, dzięki czemu można szybko i łatwo wdrażać aplikacje z kontenerami bez wiedzy z zakresu aranżacji kontenerów i zarządzać nimi.

[Azure Search](/azure/search/) służy do tworzenia rozwiązania wyszukiwania dla przedsiębiorstw za pośrednictwem prywatnej, heterogenicznych zawartości.

[Service Fabric](/azure/service-fabric/) to platforma systemów rozproszonych ułatwiająca pakowanie i wdrażanie skalowalnych i niezawodnych mikrousług i kontenerów oraz zarządzanie nimi.
