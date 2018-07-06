---
uid: web-api/overview/error-handling/exception-handling
title: Obsługa wyjątków w Web API platformy ASP.NET | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 03/12/2012
ms.assetid: cbebeb37-2594-41f2-b71a-f4f26520d512
msc.legacyurl: /web-api/overview/error-handling/exception-handling
msc.type: authoredcontent
ms.openlocfilehash: 2db00c1ab241e0dad051c2a3bcd3b0e4bbf4d5c4
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807664"
---
<a name="exception-handling-in-aspnet-web-api"></a>Obsługa wyjątków w Web API platformy ASP.NET
====================
przez [Mike Wasson](https://github.com/MikeWasson)

W tym artykule opisano, błędów i obsługa wyjątków w Web API platformy ASP.NET.

- [HttpResponseException](#httpresponserexception)
- [Filtry wyjątków](#exception_filters)
- [Rejestrowanie filtry wyjątków](#registering_exception_filters)
- [HttpError](#httperror)

<a id="httpresponserexception"></a>
## <a name="httpresponseexception"></a>HttpResponseException

Co się stanie, jeśli kontroler Web API zgłosi nieprzechwycony wyjątek? Domyślnie większość wyjątków są tłumaczone na odpowiedź HTTP z kodem stanu 500, wewnętrzny błąd serwera.

**HttpResponseException** typu jest przypadkiem szczególnym. Ten wyjątek zwraca do kod stanu HTTP, który określisz w Konstruktorze wyjątku. Na przykład poniższa metoda zwraca 404, nie można odnaleźć, jeśli *identyfikator* parametr jest nieprawidłowy.

[!code-csharp[Main](exception-handling/samples/sample1.cs)]

Większą kontrolę nad odpowiedzi, można także skonstruować komunikat całej odpowiedzi i dołącz go za pomocą **HttpResponseException:** 

[!code-csharp[Main](exception-handling/samples/sample2.cs)]

<a id="exception_filters"></a>
## <a name="exception-filters"></a>Filtry wyjątków

Można dostosować, jak internetowy interfejs API obsługuje wyjątki, pisząc *filtra wyjątku*. Filtra wyjątku jest wykonywany, gdy metoda kontrolera zgłasza wyjątek nieobsługiwany wyjątek, który jest *nie* **HttpResponseException** wyjątku. **HttpResponseException** typ jest to szczególny przypadek, ponieważ jest on zaprojektowany specjalnie w celu zwrócenia odpowiedź HTTP.

Filtry wyjątków zaimplementować **System.Web.Http.Filters.IExceptionFilter** interfejsu. Najprostszym sposobem pisanie filtra wyjątku jest pochodną **System.Web.Http.Filters.ExceptionFilterAttribute** klasy, a także Przesłoń **OnException** metody.

> [!NOTE]
> Filtry wyjątków w interfejsie API sieci Web platformy ASP.NET są podobne do tych we wzorcu ASP.NET MVC. Jednak są deklarowane w oddzielnych przestrzeni nazw, a funkcja oddzielnie. W szczególności **atrybutu HandleErrorAttribute** klasa używana w MVC nie obsługuje wyjątki wyrzucane przez kontrolerów internetowych interfejsów API.


Oto filtr, który konwertuje **NotImplementedException** wyjątki w przypadku stanu HTTP kodu 501, nie zaimplementowano:

[!code-csharp[Main](exception-handling/samples/sample3.cs)]

**Odpowiedzi** właściwość **HttpActionExecutedContext** obiekt zawiera komunikat odpowiedzi HTTP, który zostanie wysłany do klienta.

<a id="registering_exception_filters"></a>
## <a name="registering-exception-filters"></a>Rejestrowanie filtry wyjątków

Istnieje kilka sposobów, aby zarejestrować filtra wyjątku interfejsu API sieci Web:

- Za akcję
- Kontroler
- Globalnie

Aby zastosować filtr do określonej akcji, Dodaj filtr jako atrybut do akcji:

[!code-csharp[Main](exception-handling/samples/sample4.cs)]

Aby zastosować filtr do wszystkich akcji w kontrolerze, Dodaj filtr jako atrybut do klasy kontrolera:

[!code-csharp[Main](exception-handling/samples/sample5.cs)]

Aby zastosować filtr globalnie do wszystkich kontrolerów składnika Web API, należy dodać wystąpienia filtru w celu **GlobalConfiguration.Configuration.Filters** kolekcji. Filtry wyjątku w tej kolekcji są stosowane do dowolnej akcji kontrolera interfejsu API sieci Web.

[!code-csharp[Main](exception-handling/samples/sample6.cs)]

Jeśli używasz szablonu projektu "Aplikacja sieci Web 4 MVC ASP.NET" do tworzenia projektu, umieść kod konfiguracji interfejsu API sieci Web wewnątrz `WebApiConfig` klasy, która znajduje się w aplikacji\_folder początkowy:

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

<a id="httperror"></a>
## <a name="httperror"></a>HttpError

**HttpError** obiekt zapewnia spójny sposób, aby zwrócić informacje o błędzie w treści odpowiedzi. Poniższy przykład pokazuje, jak zwrócić kod stanu HTTP 404 (nie znaleziono) za pomocą **HttpError** w treści odpowiedzi.

[!code-csharp[Main](exception-handling/samples/sample8.cs)]

**CreateErrorResponse** metodę rozszerzającą zdefiniowano **System.Net.Http.HttpRequestMessageExtensions** klasy. Wewnętrznie **CreateErrorResponse** tworzy **HttpError** wystąpienia, a następnie tworzy **obiektu HttpResponseMessage** zawierający **HttpError**.

W tym przykładzie Jeśli metoda zakończy się pomyślnie, zwracany jest produkt w odpowiedzi HTTP. Ale jeśli żądana produktu nie zostanie znaleziony, odpowiedź HTTP zawiera **HttpError** w treści żądania. Odpowiedź może wyglądać następująco:

[!code-console[Main](exception-handling/samples/sample9.cmd)]

Należy zauważyć, że **HttpError** został wydany do formatu JSON, w tym przykładzie. Jedną z zalet za pomocą **HttpError** jest, że przechodzi on przez takie same [negocjacji zawartości](../formats-and-model-binding/content-negotiation.md) i serializacji przetwarzania jak wszystkie inne z silnie typizowanym modelem.

### <a name="httperror-and-model-validation"></a>HttpError i sprawdzanie poprawności modelu

Do weryfikacji modelu, można przekazać stan modelu do **CreateErrorResponse**, aby uwzględnić błędy sprawdzania poprawności w odpowiedzi:

[!code-csharp[Main](exception-handling/samples/sample10.cs)]

W tym przykładzie może zwrócić następującą odpowiedź:

[!code-console[Main](exception-handling/samples/sample11.cmd)]

Aby uzyskać więcej informacji o weryfikacji modelu, zobacz [weryfikacji modelu programu ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).

### <a name="using-httperror-with-httpresponseexception"></a>Przy użyciu HttpResponseException HttpError

Poprzednie przykłady zwrócą **obiektu HttpResponseMessage** wiadomości z akcji kontrolera, ale można również użyć **HttpResponseException** do zwrócenia **HttpError**. Dzięki temu można zwrócić silnie typizowanym modelem w przypadku powodzenia normalne, podczas zwracania nadal **HttpError** Jeśli występuje błąd:

[!code-csharp[Main](exception-handling/samples/sample12.cs)]
