---
title: Safelist IP klienta dla ASP.NET Core
author: damienbod
description: Dowiedz się, jak napisać oprogramowanie pośredniczące lub filtry akcji, aby zweryfikować zdalne adresy IP w odniesieniu do listy zatwierdzonych adresów IP.
ms.author: riande
ms.custom: mvc
ms.date: 08/31/2018
uid: security/ip-safelist
ms.openlocfilehash: d25c375f7e659168ab8cc9d8e11753cb7dfde831
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78659776"
---
# <a name="client-ip-safelist-for-aspnet-core"></a>Safelist IP klienta dla ASP.NET Core

Autorzy [Damien Bowden](https://twitter.com/damien_bod) i [Tomasz Dykstra](https://github.com/tdykstra)
 
W tym artykule przedstawiono trzy sposoby implementacji Safelist IP (znanego również jako dozwolonych) w aplikacji ASP.NET Core. Możesz użyć:

* Oprogramowanie pośredniczące do sprawdzenia zdalnego adresu IP każdego żądania.
* Filtry akcji do sprawdzania zdalnego adresu IP żądań dla określonych kontrolerów lub metod akcji.
* Razor Pages filtrów, aby sprawdzić zdalny adres IP żądań dla stron Razor.

W każdym przypadku ciąg zawierający zatwierdzone adresy IP klienta jest przechowywany w ustawieniu aplikacji. Oprogramowanie pośredniczące lub Filtr analizuje ciąg w postaci listy i sprawdza, czy zdalny adres IP znajduje się na liście. W przeciwnym razie zwracany jest kod stanu zabroniony protokołu HTTP 403.

[Wyświetl lub pobierz przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/ip-safelist/samples/2.x/ClientIpAspNetCore) ([jak pobrać](xref:index#how-to-download-a-sample))

## <a name="the-safelist"></a>Safelist

Lista jest konfigurowana w pliku *appSettings. JSON* . Jest to rozdzielana średnikami lista i może zawierać adresy IPv4 i IPv6.

[!code-json[](ip-safelist/samples/2.x/ClientIpAspNetCore/appsettings.json?highlight=2)]

## <a name="middleware"></a>Oprogramowanie pośredniczące

Metoda `Configure` dodaje oprogramowanie pośredniczące i przekazuje do niego ciąg Safelist w parametrze konstruktora.

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_Configure&highlight=10)]

Oprogramowanie pośredniczące analizuje ciąg w tablicę i wyszukuje zdalny adres IP w tablicy. Jeśli zdalny adres IP nie zostanie znaleziony, oprogramowanie pośredniczące zwróci niedozwolony protokół HTTP 401. Ten proces sprawdzania poprawności jest pomijany dla żądań HTTP GET.

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/AdminSafeListMiddleware.cs?name=snippet_ClassOnly)]

## <a name="action-filter"></a>Filtr akcji

Jeśli chcesz, aby Safelist tylko dla określonych kontrolerów lub metod akcji, Użyj filtru akcji. Oto przykład: 

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Filters/ClientIpCheckFilter.cs)]

Filtr akcji zostanie dodany do kontenera usługi.

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

Filtr może być następnie używany na kontrolerze lub metodzie akcji.

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Controllers/ValuesController.cs?name=snippet_Filter&highlight=1)]

W przykładowej aplikacji filtr jest stosowany do metody `Get`. Dlatego podczas testowania aplikacji przez wysłanie żądania interfejsu API `Get` ten atrybut sprawdza poprawność adresu IP klienta. Podczas testowania przez wywołanie interfejsu API z dowolną inną metodą HTTP, oprogramowanie pośredniczące sprawdza poprawność adresu IP klienta.

## <a name="razor-pages-filter"></a>Filtr Razor Pages 

Jeśli chcesz Safelist dla aplikacji Razor Pages, Użyj filtru Razor Pages. Oto przykład: 

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Filters/ClientIpCheckPageFilter.cs)]

Ten filtr jest włączony przez dodanie go do kolekcji filtrów MVC.

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServices&highlight=7-9)]

Po uruchomieniu aplikacji i zażądaniu strony Razor filtr Razor Pages sprawdza poprawność adresu IP klienta.

## <a name="next-steps"></a>Następne kroki

[Dowiedz się więcej na temat ASP.NET Core oprogramowania pośredniczącego](xref:fundamentals/middleware/index).
