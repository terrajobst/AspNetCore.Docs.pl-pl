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
# <a name="next-steps"></a><span data-ttu-id="90ed4-103">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="90ed4-103">Next steps</span></span>

<span data-ttu-id="90ed4-104">W tym przewodniku opisano tworzenie potoku metodyki DevOps dla przykładowej aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="90ed4-104">In this guide, you created a DevOps pipeline for an ASP.NET Core sample app.</span></span> <span data-ttu-id="90ed4-105">Gratulacje!</span><span class="sxs-lookup"><span data-stu-id="90ed4-105">Congratulations!</span></span> <span data-ttu-id="90ed4-106">Mamy nadzieję, że podoba Ci się najbardziej uczenia publikowania aplikacji sieci web platformy ASP.NET Core w usłudze Azure App Service i automatyzowanie ciągłej integracji zmian.</span><span class="sxs-lookup"><span data-stu-id="90ed4-106">We hope you enjoyed learning to publish ASP.NET Core web apps to Azure App Service and automate the continuous integration of changes.</span></span>

<span data-ttu-id="90ed4-107">Poza hosting sieci web i metodyki DevOps platforma Azure oferuje szeroką gamę usług Platform-as-a-Service (PaaS) jest przydatny dla deweloperów platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="90ed4-107">Beyond web hosting and DevOps, Azure has a wide array of Platform-as-a-Service (PaaS) services useful to ASP.NET Core developers.</span></span> <span data-ttu-id="90ed4-108">Ta sekcja zawiera krótkie omówienie niektórych najczęściej używanych usług.</span><span class="sxs-lookup"><span data-stu-id="90ed4-108">This section gives a brief overview of some of the most commonly used services.</span></span>

## <a name="storage-and-databases"></a><span data-ttu-id="90ed4-109">Magazynowi i bazom danych</span><span class="sxs-lookup"><span data-stu-id="90ed4-109">Storage and databases</span></span>

<span data-ttu-id="90ed4-110">[Redis Cache](/azure/redis-cache/) to buforowanie danych o wysokiej przepływności i małych opóźnieniach dostępne jako usługa.</span><span class="sxs-lookup"><span data-stu-id="90ed4-110">[Redis Cache](/azure/redis-cache/) is high-throughput, low-latency data caching available as a service.</span></span> <span data-ttu-id="90ed4-111">Może służyć do buforowania danych wyjściowych strony, zmniejszenie żądań bazy danych i zapewnianie stanu sesji platformy ASP.NET Core w wielu wystąpieniach aplikacji.</span><span class="sxs-lookup"><span data-stu-id="90ed4-111">It can be used for caching page output, reducing database requests, and providing ASP.NET Core session state across multiple instances of an app.</span></span>

<span data-ttu-id="90ed4-112">[Usługa Azure Storage](/azure/storage/) to wysoce skalowalny magazyn w chmurze na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="90ed4-112">[Azure Storage](/azure/storage/) is Azure's massively scalable cloud storage.</span></span> <span data-ttu-id="90ed4-113">Deweloperzy mogą korzystać z [queue storage](/azure/storage/queues/storage-queues-introduction) dla niezawodnej usługi kolejkowania komunikatów, a [Table Storage](/azure/storage/tables/table-storage-overview) to NoSQL klucz-wartość magazynu, który umożliwia szybkie tworzenie aplikacji przy użyciu ogromnych zestawów danych z częściową strukturą.</span><span class="sxs-lookup"><span data-stu-id="90ed4-113">Developers can take advantage of [Queue Storage](/azure/storage/queues/storage-queues-introduction) for reliable message queuing, and [Table Storage](/azure/storage/tables/table-storage-overview) is a NoSQL key-value store designed for rapid development using massive, semi-structured data sets.</span></span>

<span data-ttu-id="90ed4-114">[Azure SQL Database](/azure/sql-database/) zapewnia znane funkcje relacyjnej bazy danych jako usługi korzystające z aparatu Microsoft SQL Server.</span><span class="sxs-lookup"><span data-stu-id="90ed4-114">[Azure SQL Database](/azure/sql-database/) provides familiar relational database functionality as a service using the Microsoft SQL Server Engine.</span></span>

<span data-ttu-id="90ed4-115">[Cosmos DB](/azure/cosmos-db/) globalnie dystrybuowana, wielomodelowa usługa bazy danych NoSQL.</span><span class="sxs-lookup"><span data-stu-id="90ed4-115">[Cosmos DB](/azure/cosmos-db/) globally distributed, multi-model NoSQL database service.</span></span> <span data-ttu-id="90ed4-116">Wiele interfejsów API dostępnych w tym interfejsu API SQL (dawniej nazywanych bazy danych DocumentDB), bazy danych Cassandra i bazy danych MongoDB.</span><span class="sxs-lookup"><span data-stu-id="90ed4-116">Multiple APIs are available, including SQL API (formerly called DocumentDB), Cassandra, and MongoDB.</span></span>

## <a name="identity"></a><span data-ttu-id="90ed4-117">Tożsamość</span><span class="sxs-lookup"><span data-stu-id="90ed4-117">Identity</span></span>

<span data-ttu-id="90ed4-118">[Azure Active Directory](/azure/active-directory/) i [Azure Active Directory B2C](/azure/active-directory-b2c/) są usługami tożsamości.</span><span class="sxs-lookup"><span data-stu-id="90ed4-118">[Azure Active Directory](/azure/active-directory/) and [Azure Active Directory B2C](/azure/active-directory-b2c/) are both identity services.</span></span> <span data-ttu-id="90ed4-119">Usługa Azure Active Directory jest przeznaczona dla scenariuszy dla przedsiębiorstw i do pracy zespołowej usługi Azure AD B2B (business-to-business), podczas gdy usługi Azure Active Directory B2C jest zamierzony scenariuszy biznesowych do klienta, w tym logowania się w sieci społecznościowej.</span><span class="sxs-lookup"><span data-stu-id="90ed4-119">Azure Active Directory is designed for enterprise scenarios and enables Azure AD B2B (business-to-business) collaboration, while Azure Active Directory B2C is intended business-to-customer scenarios, including social network sign-in.</span></span>

## <a name="mobile"></a><span data-ttu-id="90ed4-120">Komórkowy</span><span class="sxs-lookup"><span data-stu-id="90ed4-120">Mobile</span></span>

<span data-ttu-id="90ed4-121">[Notification Hubs](/azure/notification-hubs/) to wieloplatformowy, Skalowalny aparat powiadomień wypychanych umożliwiający szybkie wysyłanie milionów komunikatów do aplikacji działających na różnych typach urządzeń.</span><span class="sxs-lookup"><span data-stu-id="90ed4-121">[Notification Hubs](/azure/notification-hubs/) is a multi-platform, scalable push-notification engine to quickly send millions of messages to apps running on various types of devices.</span></span>

## <a name="web-infrastructure"></a><span data-ttu-id="90ed4-122">Infrastruktura sieci Web</span><span class="sxs-lookup"><span data-stu-id="90ed4-122">Web infrastructure</span></span>

<span data-ttu-id="90ed4-123">[Azure Container Service](/azure/aks/) zarządza hostowanym środowiskiem Kubernetes, dzięki czemu można szybko i łatwo wdrażać aplikacje z kontenerami bez wiedzy z zakresu aranżacji kontenerów i zarządzać nimi.</span><span class="sxs-lookup"><span data-stu-id="90ed4-123">[Azure Container Service](/azure/aks/) manages your hosted Kubernetes environment, making it quick and easy to deploy and manage containerized apps without container orchestration expertise.</span></span>

<span data-ttu-id="90ed4-124">[Azure Search](/azure/search/) służy do tworzenia rozwiązania wyszukiwania dla przedsiębiorstw za pośrednictwem prywatnej, heterogenicznych zawartości.</span><span class="sxs-lookup"><span data-stu-id="90ed4-124">[Azure Search](/azure/search/) is used to create an enterprise search solution over private, heterogenous content.</span></span>

<span data-ttu-id="90ed4-125">[Service Fabric](/azure/service-fabric/) to platforma systemów rozproszonych ułatwiająca pakowanie i wdrażanie skalowalnych i niezawodnych mikrousług i kontenerów oraz zarządzanie nimi.</span><span class="sxs-lookup"><span data-stu-id="90ed4-125">[Service Fabric](/azure/service-fabric/) is a distributed systems platform that makes it easy to package, deploy, and manage scalable and reliable microservices and containers.</span></span>
