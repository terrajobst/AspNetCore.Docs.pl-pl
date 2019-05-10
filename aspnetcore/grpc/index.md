---
title: Wprowadzenie do gRPC programu ASP.NET Core
author: juntaoluo
description: Więcej informacji na temat usług gRPC przy użyciu serwera Kestrel i stosu platformy ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 02/26/2019
uid: grpc/index
ms.openlocfilehash: dd1c42744bfda965df91ea1fcc0b71814317b969
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65085565"
---
# <a name="introduction-to-grpc-on-aspnet-core"></a><span data-ttu-id="b6584-103">Wprowadzenie do gRPC programu ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b6584-103">Introduction to gRPC on ASP.NET Core</span></span>

<span data-ttu-id="b6584-104">Przez [Luo Jan](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="b6584-104">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="b6584-105">[gRPC](https://grpc.io/docs/guides/) to struktura języka niezależny, wysokowydajnych zdalnego wywołania procedury (RPC).</span><span class="sxs-lookup"><span data-stu-id="b6584-105">[gRPC](https://grpc.io/docs/guides/) is a language agnostic, high-performance Remote Procedure Call (RPC) framework.</span></span> <span data-ttu-id="b6584-106">Aby uzyskać więcej informacji na podstawy gRPC zobacz [stronę z dokumentacją dotyczącą gRPC](https://grpc.io/docs/).</span><span class="sxs-lookup"><span data-stu-id="b6584-106">For more on gRPC fundamentals, see the [gRPC documentation page](https://grpc.io/docs/).</span></span>

<span data-ttu-id="b6584-107">Najważniejsze zalety gRPC są:</span><span class="sxs-lookup"><span data-stu-id="b6584-107">The main benefits of gRPC are:</span></span>
* <span data-ttu-id="b6584-108">Nowoczesne o wysokiej wydajności uproszczonego RPC framework.</span><span class="sxs-lookup"><span data-stu-id="b6584-108">Modern high-performance lightweight RPC framework.</span></span>
* <span data-ttu-id="b6584-109">Wdrażanie interfejsu API z wymogiem wcześniejszego zawarcia kontraktu, przy użyciu Protocol Buffers domyślnie, co zapewnia implementacji niezależny od języka.</span><span class="sxs-lookup"><span data-stu-id="b6584-109">Contract-first API development, using Protocol Buffers by default, allowing for language agnostic implementations.</span></span>
* <span data-ttu-id="b6584-110">Narzędzia dostępne dla wielu języków wygenerować silnie typizowaną serwerów i klientów.</span><span class="sxs-lookup"><span data-stu-id="b6584-110">Tooling available for many languages to generate strongly-typed servers and clients.</span></span>
* <span data-ttu-id="b6584-111">Obsługuje wywołania przesyłania strumieniowego dwukierunkowej, serwerów i klientów.</span><span class="sxs-lookup"><span data-stu-id="b6584-111">Supports client, server, and bi-directional streaming calls.</span></span>
* <span data-ttu-id="b6584-112">Użycie niższych sieci przy użyciu formatu Protobuf serializacji binarnej.</span><span class="sxs-lookup"><span data-stu-id="b6584-112">Reduced network usage with Protobuf binary serialization.</span></span>

<span data-ttu-id="b6584-113">Te korzyści idealnym rozwiązaniem gRPC do:</span><span class="sxs-lookup"><span data-stu-id="b6584-113">These benefits make gRPC ideal for:</span></span>
* <span data-ttu-id="b6584-114">Uproszczone mikrousług, gdzie wydajność ma kluczowe znaczenie.</span><span class="sxs-lookup"><span data-stu-id="b6584-114">Lightweight microservices where efficiency is critical.</span></span>
* <span data-ttu-id="b6584-115">Polyglot systemy, w których języki są wymagane do tworzenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b6584-115">Polyglot systems where multiple languages are required for development.</span></span>
* <span data-ttu-id="b6584-116">Punkt-punkt w czasie rzeczywistym usługi, które wymagają obsługi przesyłania strumieniowego żądań lub odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="b6584-116">Point-to-point real-time services that need to handle streaming requests or responses.</span></span>

<span data-ttu-id="b6584-117">Gdy C# implementacja jest obecnie dostępna w oficjalnej [strony gRPC](https://grpc.io/docs/quickstart/csharp.html), bieżąca implementacja jest uzależniona od natywną bibliotekę w języku C (gRPC [C-core](https://grpc.io/blog/grpc-stacks)).</span><span class="sxs-lookup"><span data-stu-id="b6584-117">While a C# implementation is currently available on the official [gRPC page](https://grpc.io/docs/quickstart/csharp.html), the current implementation relies on the native library written in C (gRPC [C-core](https://grpc.io/blog/grpc-stacks)).</span></span> <span data-ttu-id="b6584-118">Praca jest obecnie w toku, aby zapewnić w przypadku nowego wdrożenia na podstawie serwera Kestrel HTTP i stos platformy ASP.NET Core, który jest w pełni zarządzana.</span><span class="sxs-lookup"><span data-stu-id="b6584-118">Work is currently in progress to provide a new implementation based on the Kestrel HTTP server and the ASP.NET Core stack that is fully managed.</span></span> <span data-ttu-id="b6584-119">Poniższe dokumenty zawierają wprowadzenie do tworzenia usług gRPC za pomocą tego nowego wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="b6584-119">The following documents provide an introduction to building gRPC services with this new implementation.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b6584-120">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="b6584-120">Additional resources</span></span>

* <xref:grpc/basics>
* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/aspnetcore>
* <xref:grpc/migration>