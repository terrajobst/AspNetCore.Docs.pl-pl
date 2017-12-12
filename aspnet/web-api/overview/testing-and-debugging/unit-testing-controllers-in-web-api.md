---
uid: web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
title: "Jednostka testowania kontrolerów w składniku ASP.NET Web API 2 | Dokumentacja firmy Microsoft"
author: MikeWasson
description: "W tym temacie opisano niektóre określone techniki testowania kontrolerów w sieci Web API 2 jednostek. Przed przeczytaniem tego tematu, należy przeczytać samouczek jednostki..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/11/2014
ms.topic: article
ms.assetid: 43a6cce7-a3ef-42aa-ad06-90d36d49f098
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 167cd24d27977c3652f6a8903054654f5edf7756
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="unit-testing-controllers-in-aspnet-web-api-2"></a>Jednostka testowania kontrolerów w składniku ASP.NET Web API 2
====================
przez [Wasson Jan](https://github.com/MikeWasson)

> W tym temacie opisano niektóre określone techniki testowania kontrolerów w sieci Web API 2 jednostek. Przed przeczytaniem tego tematu, należy przeczytać samouczek [jednostek testowania ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), który wskazuje, jak dodać projektu testu jednostkowego do rozwiązania.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Używane w samouczku wersje oprogramowania
> 
> - [Visual Studio 2017 r.](https://www.visualstudio.com/vs/)
> - Składnik Web API 2
> - [Moq](https://github.com/Moq) 4.5.30

> [!NOTE]
> Po użyciu Moq, ale sam pomysł ma zastosowanie do dowolnej mocking architektury. Moq 4.5.30 (i nowszych) obsługuje Visual Studio 2017 r, Roslyn i .NET 4.5 i nowszymi wersjami.

Wspólnego wzorca w testach jednostkowych jest &quot;Rozmieść act-assert&quot;:

- Rozmieść: Konfigurowanie wymagań wstępnych dla testów do uruchomienia.
- Działanie: Wykonywania testu.
- Assert: Sprawdź, czy testu zakończyło się pomyślnie.

W kroku Rozmieść będą często używane makiety lub stub obiektów. Który minimalizuje liczbę zależności, więc badanie koncentruje się na rzecz testowania.

Poniżej przedstawiono niektóre czynności, które powinny testu jednostkowego w kontrolerach interfejsu API sieci Web:

- Akcja zwraca poprawny typ odpowiedzi.
- Nieprawidłowe parametry zwraca odpowiedź Napraw błąd.
- Akcja wywołuje metodę poprawne na warstwie repozytorium lub usługi.
- Jeśli odpowiedź zawiera model domeny, sprawdź typ modelu.

Oto niektóre ogólne czynności, aby przetestować, ale szczegółowe informacje są zależne od implementacji kontrolera. W szczególności ułatwia istotną różnicą czy zwracać akcji kontrolera **HttpResponseMessage** lub **IHttpActionResult**. Aby uzyskać więcej informacji na temat tych typów, zobacz [wyniki akcji w sieci Web Api 2](../getting-started-with-aspnet-web-api/action-results.md).

## <a name="testing-actions-that-return-httpresponsemessage"></a>Testowanie akcji, które zwracają HttpResponseMessage

Oto przykład kontrolera którego powrotu akcje **HttpResponseMessage**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample1.cs)]

Powiadomienie kontrolera używa iniekcji zależności można wstrzyknąć `IProductRepository`. Dzięki temu kontroler więcej testować, ponieważ może wstrzyknąć zasymulować repozytorium. Następujące testu jednostkowego sprawdza, czy `Get` zapisy metody `Product` do treści odpowiedzi. Przyjęto założenie, że `repository` jest makiety `IProductRepository`.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample2.cs)]

Ważne jest, aby ustawić **żądania** i **konfiguracji** na kontrolerze. W przeciwnym razie test zakończy się niepowodzeniem z **argumentnullexception —** lub **InvalidOperationException**.

## <a name="testing-link-generation"></a>Testowanie generowania łącza

`Post` Wywołania metody **UrlHelper.Link** do utworzenia łącza w odpowiedzi. Wymaga nieco więcej ustawień w testu jednostkowego:

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample3.cs)]

**UrlHelper** klasa musi żądanie adresu URL i dane trasy, więc nie można ustawić wartości dla nich testu. Innym rozwiązaniem jest makiety lub stub **UrlHelper**. Takie podejście, należy zastąpić wartość domyślną [ApiController.Url](https://msdn.microsoft.com/en-us/library/system.web.http.apicontroller.url.aspx) na wersję makiety lub stub zwracające wartość stałą.

Umożliwia ponowne zapisywanie adresów testu za pomocą [Moq](https://github.com/Moq) framework. Zainstaluj `Moq` pakietu NuGet w projekcie testowym.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample4.cs)]

W tej wersji, nie trzeba skonfigurować dowolne dane trasy, ponieważ makiety **UrlHelper** zwraca ciąg stałej.


## <a name="testing-actions-that-return-ihttpactionresult"></a>Testowanie akcji, które zwracają IHttpActionResult

W sieci Web API 2, mogą zwracać akcji kontrolera **IHttpActionResult**, który jest odpowiednikiem **ActionResult** na platformie ASP.NET MVC. **IHttpActionResult** interfejs definiuje polecenie wzorzec do tworzenia odpowiedzi HTTP. Zamiast tworzyć bezpośrednio odpowiedzi, zwraca kontrolera **IHttpActionResult**. Później, wywołuje potok **IHttpActionResult** do tworzenia odpowiedzi. Takie podejście ułatwia pisanie testów jednostkowych, ponieważ można pominąć wiele ustawień, który jest wymagany dla **HttpResponseMessage**.

Oto przykład controller którego powrotu akcje **IHttpActionResult**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample5.cs)]

W tym przykładzie przedstawiono niektóre typowe wzorce przy użyciu **IHttpActionResult**. Zobaczmy, jak do jednostki je przetestować.

### <a name="action-returns-200-ok-with-a-response-body"></a>Akcja zwraca 200 (OK) z treści odpowiedzi

`Get` Wywołania metody `Ok(product)` Jeśli zostanie znaleziony produkt. Upewnij się, jest zwracany typ w testu jednostkowego **OkNegotiatedContentResult** i zwrócony produkt ma prawo identyfikatora.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample6.cs)]

Należy zauważyć, że testu jednostkowego nie jest wykonywana wyniku akcji. Można założyć, że poprawnie tworzy wynik akcji odpowiedzi HTTP. (Dlatego strukturę interfejsu API sieci Web ma własną testów jednostkowych!)

### <a name="action-returns-404-not-found"></a>Akcja zwraca 404 (nie znaleziono)

`Get` Wywołania metody `NotFound()` Jeśli produktu nie zostanie znaleziony. Dla tego przypadku testu jednostkowego sprawdza tylko jeśli typ zwracany jest **NotFoundResult**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample7.cs)]

### <a name="action-returns-200-ok-with-no-response-body"></a>Akcja zwraca 200 (OK) z treści odpowiedzi

`Delete` Wywołania metody `Ok()` aby zwrócić pustą odpowiedź 200 protokołu HTTP. Tak jak w poprzednim przykładzie testu jednostkowego sprawdza zwracany typ w tym przypadku **OkResult**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample8.cs)]

### <a name="action-returns-201-created-with-a-location-header"></a>Akcja zwraca 201 (utworzono) z nagłówkiem lokalizacji

`Post` Wywołania metody `CreatedAtRoute` do zwrócenia odpowiedź 201 protokołu HTTP z identyfikatora URI w nagłówku lokalizacji. Sprawdź, czy akcja ustawia poprawne wartości routingu w testu jednostkowego.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample9.cs)]

### <a name="action-returns-another-2xx-with-a-response-body"></a>Akcja zwraca innego 2xx z treści odpowiedzi

`Put` Wywołania metody `Content` do zwracania odpowiedzi HTTP 202 (zaakceptowane) z treści odpowiedzi. Ten przypadek jest podobny do zwracania 200 (OK), ale testu jednostkowego należy także sprawdzić kod stanu.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample10.cs)]

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Mocking programu Entity Framework podczas testowania składnika ASP.NET Web API 2 jednostek](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md)
- [Pisanie testów dla usługi interfejsu API sieci Web platformy ASP.NET](https://blogs.msdn.com/b/youssefm/archive/2013/01/28/writing-tests-for-an-asp-net-webapi-service.aspx) (wpis w blogu przez Youssef Moussaoui).
- [Debugowanie interfejsu API sieci Web ASP.NET za pomocą Route Debugger](https://blogs.msdn.com/b/webdev/archive/2013/04/04/debugging-asp-net-web-api-with-route-debugger.aspx)
