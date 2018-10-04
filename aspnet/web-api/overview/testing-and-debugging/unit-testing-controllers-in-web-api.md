---
uid: web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
title: Kontrolery testów jednostkowych we wzorcu ASP.NET Web API 2 | Dokumentacja firmy Microsoft
author: MikeWasson
description: W tym temacie opisano niektóre określone techniki kontrolery testów jednostkowych WE Web API 2. Przed odczytaniem w tym temacie, możesz chcieć przeczytaj samouczek jednostki...
ms.author: riande
ms.date: 06/11/2014
ms.assetid: 43a6cce7-a3ef-42aa-ad06-90d36d49f098
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: e1bb1aa120ced95db7674eae1831f2a2c7356fc0
ms.sourcegitcommit: 7890dfb5a8f8c07d813f166d3ab0c263f893d0c6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/04/2018
ms.locfileid: "48794827"
---
<a name="unit-testing-controllers-in-aspnet-web-api-2"></a>Kontrolery testów jednostkowych we wzorcu ASP.NET Web API 2
====================
przez [Mike Wasson](https://github.com/MikeWasson)

> W tym temacie opisano niektóre określone techniki kontrolery testów jednostkowych WE Web API 2. Przed odczytaniem w tym temacie, możesz chcieć przeczytaj samouczek [jednostki testowania wzorca ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), który pokazuje, jak dodać projekt testu jednostkowego do rozwiązania.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używanego w tym samouczku
>
> - [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - Składnik Web API 2
> - [Moq](https://github.com/Moq) 4.5.30

> [!NOTE]
> Po użyciu Moq, ale ten sam pomysł stosuje się do dowolnej architektury pozorowania. Moq 4.5.30 (i nowszych) obsługuje program Visual Studio 2017, Roslyn i .NET 4.5 i nowsze wersje.

Typowym w testach jednostkowych &quot;Rozmieść act asercja&quot;:

- Rozmieść: Konfigurowanie wymagań wstępnych dla testu do uruchomienia.
- Ustawa: Wykonywania testu.
- Asercja: Sprawdź, czy test powiodło się.

W kroku Rozmieść będą często używane pozorny lub namiastki obiektów. Liczba zależności, które minimalizuje tak testu koncentruje się na testowanie jedno.

Oto kilka rzeczy, które powinno testu jednostkowego w kontrolerach interfejsu API sieci Web:

- Akcja zwraca poprawny typ odpowiedzi.
- Nieprawidłowe parametry zwracania odpowiedzi usunąć błąd.
- Akcja wywołuje metodę poprawne na warstwie repozytorium lub usługi.
- Jeśli odpowiedź zawiera model domeny, należy sprawdzić typ modelu.

Poniżej przedstawiono niektóre ogólne czynności, aby przetestować, ale szczegółowe informacje są zależne od implementacji kontrolera. W szczególności, to sprawia, że różnica czy zwrócić akcji kontrolera **obiektu HttpResponseMessage** lub **IHttpActionResult**. Aby uzyskać więcej informacji na temat tych typów, zobacz [wyniki akcji w sieci Web Api 2](../getting-started-with-aspnet-web-api/action-results.md).

## <a name="testing-actions-that-return-httpresponsemessage"></a>Testowanie akcji, które zwracają element HttpResponseMessage

Oto przykład kontrolera którego zwracany akcje **obiektu HttpResponseMessage**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample1.cs)]

Zwróć uwagę, kontroler używa wstrzykiwanie zależności do dodania `IProductRepository`. Dzięki temu kontroler bardziej sprawdzalnego działa zgodnie, ponieważ może wprowadzać makiety repozytorium. Następujący test jednostkowy sprawdza, czy `Get` zapisów metoda `Product` na treść odpowiedzi. Przyjęto założenie, że `repository` jest pozorny `IProductRepository`.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample2.cs)]

Jest ważne, aby ustawić **żądania** i **konfiguracji** na kontrolerze. W przeciwnym razie niepowodzenie testu **ArgumentNullException** lub **InvalidOperationException**.

## <a name="testing-link-generation"></a>Testowanie Generowanie konsolidacji

`Post` Wywołania metody **UrlHelper.Link** do tworzenia łączy w odpowiedzi. Ta migracja wymaga nieco więcej instalacji w test jednostkowy:

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample3.cs)]

**UrlHelper** klasy musi żądanie adresu URL i dane trasy, więc nie można ustawić wartości dla tych testów. Innym rozwiązaniem jest pozorny ani klas zastępczych **UrlHelper**. W przypadku tej metody, zastąp wartość domyślną [ApiController.Url](https://msdn.microsoft.com/library/system.web.http.apicontroller.url.aspx) pozorny ani klas zastępczych wersji, która zwraca wartość stałą.

Teraz Zapisz ponownie test za pomocą [Moq](https://github.com/Moq) framework. Zainstaluj `Moq` pakietu NuGet w projekcie testowym.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample4.cs)]

W tej wersji, nie trzeba skonfigurować wszelkie dane trasy, ponieważ pozorny **UrlHelper** zwraca ciąg stałej.


## <a name="testing-actions-that-return-ihttpactionresult"></a>Testowanie akcji, które zwracają IHttpActionResult

W sieci Web API 2, można powrócić do akcji kontrolera **IHttpActionResult**, który jest odpowiednikiem **ActionResult** we wzorcu ASP.NET MVC. **IHttpActionResult** interfejs definiuje wzorzec polecenia do tworzenia odpowiedzi HTTP. Zamiast tworzyć odpowiedź bezpośrednio, ten kontroler zwraca **IHttpActionResult**. Później, potok wywołuje **IHttpActionResult** do tworzenia odpowiedzi. Takie podejście ułatwia pisanie testów jednostkowych, ponieważ możesz pominąć wiele ustawień, które są potrzebne do **obiektu HttpResponseMessage**.

Oto przykład controller którego zwracany akcje **IHttpActionResult**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample5.cs)]

W tym przykładzie przedstawiono niektóre typowe wzorce przy użyciu **IHttpActionResult**. Zobaczmy, jak do jednostki przetestować je.

### <a name="action-returns-200-ok-with-a-response-body"></a>Akcja zwraca 200 (OK) z treści odpowiedzi

`Get` Wywołania metody `Ok(product)` Jeśli zostanie znaleziony produktu. Upewnij się, jest zwracany typ w test jednostkowy **OkNegotiatedContentResult** i zwrócony produkt ma prawo identyfikatora.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample6.cs)]

Należy zauważyć, że testów jednostkowych nie jest wykonywany wynik akcji. Można założyć, że poprawnie tworzy wynik akcji odpowiedzi HTTP. (Dlatego strukturę interfejsu API sieci Web ma swoje własne testy jednostkowe!)

### <a name="action-returns-404-not-found"></a>Akcja zwraca 404 (nie znaleziono)

`Get` Wywołania metody `NotFound()` Jeśli produktu nie zostanie znaleziony. Dla tego przypadku testów jednostkowych sprawdza tylko, jeśli typ zwracany jest **NotFoundResult**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample7.cs)]

### <a name="action-returns-200-ok-with-no-response-body"></a>Akcja zwraca 200 (OK) bez treści odpowiedzi

`Delete` Wywołania metody `Ok()` zwrócić pustą odpowiedź HTTP 200. Podobnie jak w poprzednim przykładzie test jednostkowy sprawdza, czy zwracany typ, w tym przypadku **OkResult**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample8.cs)]

### <a name="action-returns-201-created-with-a-location-header"></a>Akcja zwraca 201 (utworzono) przy użyciu nagłówka lokalizacji

`Post` Wywołania metody `CreatedAtRoute` zwrócić odpowiedź 201 protokołu HTTP z identyfikatorem URI w nagłówku Location. Testy jednostkowe Sprawdź, czy akcja ustawia poprawne wartości routingu.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample9.cs)]

### <a name="action-returns-another-2xx-with-a-response-body"></a>Akcja zwraca inną 2xx z treści odpowiedzi

`Put` Wywołania metody `Content` zwrócić odpowiedź HTTP 202 (zaakceptowano) z treści odpowiedzi. Ten przypadek jest podobny do zwracania 200 (OK), ale test jednostkowy należy także sprawdzić kod stanu.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample10.cs)]

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Pozorowanie programu Entity Framework podczas testowania ASP.NET Web API 2 jednostek](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md)
- [Pisanie testów dla usługi interfejsu API sieci Web platformy ASP.NET](https://blogs.msdn.com/b/youssefm/archive/2013/01/28/writing-tests-for-an-asp-net-webapi-service.aspx) (wpis na blogu autorstwa Youssef Moussaoui).
- [Debugowanie ASP.NET Web API za pomocą debugera trasy](https://blogs.msdn.com/b/webdev/archive/2013/04/04/debugging-asp-net-web-api-with-route-debugger.aspx)
