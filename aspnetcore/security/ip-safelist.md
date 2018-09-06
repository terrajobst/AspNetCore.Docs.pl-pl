---
title: Dla platformy ASP.NET Core, bezpiecznej liście adresów IP klienta
author: damienbod
description: Dowiedz się, jak pisać oprogramowanie pośredniczące lub akcji filtry, aby sprawdzić poprawność zdalnych adresów IP na liście zatwierdzonych adresów IP.
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/31/2018
uid: security/ip-safelist
ms.openlocfilehash: 40fe7b67359efd1692490099c3fb529ba4a6148f
ms.sourcegitcommit: 08bf41d4b3e696ab512b044970e8304816f8cc56
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/06/2018
ms.locfileid: "44040124"
---
# <a name="client-ip-safelist-for-aspnet-core"></a>Dla platformy ASP.NET Core, bezpiecznej liście adresów IP klienta

Przez [linkach Damien](https://twitter.com/damien_bod) i [Tom Dykstra](https://github.com/tdykstra)
 
W tym artykule przedstawiono dwa sposoby zaimplementowania bezpiecznej liście adresów IP (Lista dozwolonych):

* Za pomocą platformy ASP.NET Core oprogramowania pośredniczącego do sprawdzenia zdalny adres IP każdego żądania.
* Za pomocą platformy ASP.NET Core filtry akcji do sprawdzenia zdalny adres IP żądań dla danego działania metod.

Przykładowa aplikacja pokazano oba podejścia. W każdym przypadku ciąg zawierający adresy IP zatwierdzone klienta są przechowywane w ustawieniu aplikacji. Oprogramowanie pośredniczące lub filtr analizuje ciąg w postaci listy i sprawdza, czy zdalny adres IP na liście. W przeciwnym razie zostanie zwrócony kod stanu HTTP 403 — Dostęp zabroniony.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/ip-safelist/samples/2.x/ClientIpAspNetCore) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

## <a name="the-safelist"></a>Bezpiecznej liście

Lista jest skonfigurowana w *appsettings.json* pliku. Jest rozdzielaną średnikami listę i może zawierać adresów IPv4 i IPv6.

[!code-json[](ip-safelist/samples/2.x/ClientIpAspNetCore/appsettings.json?highlight=2)]

## <a name="middleware"></a>Oprogramowanie pośredniczące

`Configure` Metoda dodaje oprogramowanie pośredniczące i przekazuje do niego ciąg bezpiecznej liście parametr konstruktora.

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_Configure&highlight=7)]

Oprogramowanie pośredniczące analizuje ciąg do tablicy, a także szuka zdalny adres IP w tablicy. Jeśli zdalny adres IP nie zostanie znaleziony, oprogramowanie pośredniczące zwraca HTTP 401 — Dostęp zabroniony. Ten proces sprawdzania poprawności jest pomijana dla żądań HTTP Get.

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/AdminSafeListMiddleware.cs?name=snippet_ClassOnly)]

## <a name="action-filter"></a>Filtr akcji

Chcąc bezpiecznej liście tylko dla określonych kontrolery lub metody akcji, użyj filtru akcji. Oto przykład: 

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Filters/ClientIdCheckFilter.cs)]

Filtr akcji zostanie dodany do kontenera usług.

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

Następnie można filtr dla metody kontrolera lub akcji.

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Controllers/ValuesController.cs?name=snippet_Filter&highlight=1)]

Przykładowa aplikacja, jest stosowany filtr do `Get` metody. Tak, podczas testowania aplikacji, wysyłając `Get` żądania interfejsu API, ten atrybut jest sprawdzanie poprawności adresu IP klienta. Podczas testowania, wywołując interfejs API przy użyciu dowolnej metody HTTP, oprogramowanie pośredniczące jest sprawdzanie poprawności adresu IP klienta.

## <a name="razor-pages-filter"></a>Filtrowanie stron razor 

Chcąc bezpiecznej liście stron Razor aplikacji, użyj filtru stron Razor. Oto przykład: 

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Filters/ClientIdCheckPageFilter.cs)]

Ten filtr jest włączona, dodając ją do kolekcji filtrów platformy MVC.

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServices&highlight=7-9)]

Po uruchomieniu aplikacji i żądania strony Razor, filtru strony Razor jest sprawdzanie poprawności adresu IP klienta.

## <a name="next-steps"></a>Następne kroki

[Dowiedz się więcej na temat platformy ASP.NET Core w oprogramowaniu pośredniczącym](xref:fundamentals/middleware/index).
