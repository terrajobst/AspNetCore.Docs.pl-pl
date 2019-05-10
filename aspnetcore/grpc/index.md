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
# <a name="introduction-to-grpc-on-aspnet-core"></a>Wprowadzenie do gRPC programu ASP.NET Core

Przez [Luo Jan](https://github.com/juntaoluo)

[gRPC](https://grpc.io/docs/guides/) to struktura języka niezależny, wysokowydajnych zdalnego wywołania procedury (RPC). Aby uzyskać więcej informacji na podstawy gRPC zobacz [stronę z dokumentacją dotyczącą gRPC](https://grpc.io/docs/).

Najważniejsze zalety gRPC są:
* Nowoczesne o wysokiej wydajności uproszczonego RPC framework.
* Wdrażanie interfejsu API z wymogiem wcześniejszego zawarcia kontraktu, przy użyciu Protocol Buffers domyślnie, co zapewnia implementacji niezależny od języka.
* Narzędzia dostępne dla wielu języków wygenerować silnie typizowaną serwerów i klientów.
* Obsługuje wywołania przesyłania strumieniowego dwukierunkowej, serwerów i klientów.
* Użycie niższych sieci przy użyciu formatu Protobuf serializacji binarnej.

Te korzyści idealnym rozwiązaniem gRPC do:
* Uproszczone mikrousług, gdzie wydajność ma kluczowe znaczenie.
* Polyglot systemy, w których języki są wymagane do tworzenia aplikacji.
* Punkt-punkt w czasie rzeczywistym usługi, które wymagają obsługi przesyłania strumieniowego żądań lub odpowiedzi.

Gdy C# implementacja jest obecnie dostępna w oficjalnej [strony gRPC](https://grpc.io/docs/quickstart/csharp.html), bieżąca implementacja jest uzależniona od natywną bibliotekę w języku C (gRPC [C-core](https://grpc.io/blog/grpc-stacks)). Praca jest obecnie w toku, aby zapewnić w przypadku nowego wdrożenia na podstawie serwera Kestrel HTTP i stos platformy ASP.NET Core, który jest w pełni zarządzana. Poniższe dokumenty zawierają wprowadzenie do tworzenia usług gRPC za pomocą tego nowego wdrożenia.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:grpc/basics>
* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/aspnetcore>
* <xref:grpc/migration>