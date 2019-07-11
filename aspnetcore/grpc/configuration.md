---
title: gRPC konfiguracji platformy ASP.NET Core
author: jamesnk
description: Dowiedz się, jak skonfigurować gRPC dla aplikacji platformy ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 05/30/2019
uid: grpc/configuration
ms.openlocfilehash: e269d701f45c0b852a9006107f0162cc5af2c38a
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67814924"
---
# <a name="grpc-for-aspnet-core-configuration"></a><span data-ttu-id="ef62c-103">gRPC konfiguracji platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ef62c-103">gRPC for ASP.NET Core configuration</span></span>

## <a name="configure-services-options"></a><span data-ttu-id="ef62c-104">Skonfiguruj opcje usługi</span><span class="sxs-lookup"><span data-stu-id="ef62c-104">Configure services options</span></span>

<span data-ttu-id="ef62c-105">W poniższej tabeli opisano opcje dotyczące konfigurowania usługi gRPC:</span><span class="sxs-lookup"><span data-stu-id="ef62c-105">The following table describes options for configuring gRPC services:</span></span>

| <span data-ttu-id="ef62c-106">Opcja</span><span class="sxs-lookup"><span data-stu-id="ef62c-106">Option</span></span> | <span data-ttu-id="ef62c-107">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="ef62c-107">Default Value</span></span> | <span data-ttu-id="ef62c-108">Opis</span><span class="sxs-lookup"><span data-stu-id="ef62c-108">Description</span></span> |
| ------ | ------------- | ----------- |
| `SendMaxMessageSize` | `null` | <span data-ttu-id="ef62c-109">Maksymalny rozmiar wiadomości w bajtach, które mogą być wysyłane z serwera.</span><span class="sxs-lookup"><span data-stu-id="ef62c-109">The maximum message size in bytes that can be sent from the server.</span></span> <span data-ttu-id="ef62c-110">Podjęto próbę wysłać komunikat, który przekracza skutkuje rozmiar skonfigurowany maksymalny komunikat o wyjątku.</span><span class="sxs-lookup"><span data-stu-id="ef62c-110">Attempting to send a message that exceeds the configured maximum message size results in an exception.</span></span> |
| `ReceiveMaxMessageSize` | <span data-ttu-id="ef62c-111">4 MB</span><span class="sxs-lookup"><span data-stu-id="ef62c-111">4 MB</span></span> | <span data-ttu-id="ef62c-112">Maksymalny rozmiar wiadomości w bajtów odebranych przez serwer.</span><span class="sxs-lookup"><span data-stu-id="ef62c-112">The maximum message size in bytes that can be received by the server.</span></span> <span data-ttu-id="ef62c-113">Jeśli serwer odbiera komunikat, który przekracza ten limit, zgłasza wyjątek.</span><span class="sxs-lookup"><span data-stu-id="ef62c-113">If the server receives a message that exceeds this limit, it throws an exception.</span></span> <span data-ttu-id="ef62c-114">Zwiększenie tej wartości umożliwia serwer do odbierania komunikatów większy, ale może mieć negatywny wpływ na zużycie pamięci.</span><span class="sxs-lookup"><span data-stu-id="ef62c-114">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="ef62c-115">Jeśli `true`, szczegółowe komunikaty o wyjątkach są zwracane do klientów, gdy wyjątek jest zgłaszany w metodzie usług.</span><span class="sxs-lookup"><span data-stu-id="ef62c-115">If `true`, detailed exception messages are returned to clients when an exception is thrown in a service method.</span></span> <span data-ttu-id="ef62c-116">Wartość domyślna to `false`.</span><span class="sxs-lookup"><span data-stu-id="ef62c-116">The default is `false`.</span></span> <span data-ttu-id="ef62c-117">Ustawienie `EnableDetailedErrors` do `true` można przecieku informacji poufnych.</span><span class="sxs-lookup"><span data-stu-id="ef62c-117">Setting `EnableDetailedErrors` to `true` can leak sensitive information.</span></span> |
| `CompressionProviders` | <span data-ttu-id="ef62c-118">Gzip</span><span class="sxs-lookup"><span data-stu-id="ef62c-118">gzip</span></span> | <span data-ttu-id="ef62c-119">Kolekcja dostawców kompresji, które umożliwiają kompresję i dekompresję wiadomości.</span><span class="sxs-lookup"><span data-stu-id="ef62c-119">A collection of compression providers used to compress and decompress messages.</span></span> <span data-ttu-id="ef62c-120">Można tworzyć i dodawać je do kolekcji dostawców niestandardowych kompresji.</span><span class="sxs-lookup"><span data-stu-id="ef62c-120">Custom compression providers can be created and added to the collection.</span></span> <span data-ttu-id="ef62c-121">Domyślnie skonfigurowany dostawca obsługuje **gzip** kompresji.</span><span class="sxs-lookup"><span data-stu-id="ef62c-121">The default configured provider supports **gzip** compression.</span></span> |
| `ResponseCompressionAlgorithm` | `null` | <span data-ttu-id="ef62c-122">Algorytm kompresji używany do skompresowania komunikatów wysyłanych z serwera.</span><span class="sxs-lookup"><span data-stu-id="ef62c-122">The compression algorithm used to compress messages sent from the server.</span></span> <span data-ttu-id="ef62c-123">Algorytm musi być zgodny dostawca kompresji w `CompressionProviders`.</span><span class="sxs-lookup"><span data-stu-id="ef62c-123">The algorithm must match a compression provider in `CompressionProviders`.</span></span> <span data-ttu-id="ef62c-124">Dla algorytmu kompresowały odpowiedzi, klient musi wskazywać obsługuje algorytm przez wysłanie go **grpc zaakceptować encoding** nagłówka.</span><span class="sxs-lookup"><span data-stu-id="ef62c-124">For the algorithm to compress a response, the client must indicate it supports the algorithm by sending it in the **grpc-accept-encoding** header.</span></span> |
| `ResponseCompressionLevel` | `null` | <span data-ttu-id="ef62c-125">Poziom kompresji, używany do skompresowania komunikatów wysyłanych z serwera.</span><span class="sxs-lookup"><span data-stu-id="ef62c-125">The compress level used to compress messages sent from the server.</span></span> |

<span data-ttu-id="ef62c-126">Można skonfigurować opcje dla wszystkich usług, zapewniając delegat opcje do `AddGrpc` wywołania `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="ef62c-126">Options can be configured for all services by providing an options delegate to the `AddGrpc` call in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](~/grpc/configuration/sample/GrcpService/Startup.cs?name=snippet)]

<span data-ttu-id="ef62c-127">Opcje dla jednej usługi zastępują opcje globalne w `AddGrpc` i może być konfigurowana przy użyciu `AddServiceOptions<TService>`:</span><span class="sxs-lookup"><span data-stu-id="ef62c-127">Options for a single service override the global options provided in `AddGrpc` and can be configured using `AddServiceOptions<TService>`:</span></span>

[!code-csharp[](~/grpc/configuration/sample/GrcpService/Startup2.cs?name=snippet)]

## <a name="configure-client-options"></a><span data-ttu-id="ef62c-128">Skonfiguruj opcje klienta</span><span class="sxs-lookup"><span data-stu-id="ef62c-128">Configure client options</span></span>

<span data-ttu-id="ef62c-129">Poniższy kod ustawia maksymalna liczba wysyłanych klienta i wyświetlany rozmiar wiadomości:</span><span class="sxs-lookup"><span data-stu-id="ef62c-129">The following code sets the client maximum send and receive message size:</span></span>

[!code-csharp[](~/grpc/configuration/sample/Program.cs?name=snippet&highlight=3-6)]

## <a name="additional-resources"></a><span data-ttu-id="ef62c-130">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="ef62c-130">Additional resources</span></span>

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
