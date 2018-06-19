---
uid: web-api/overview/error-handling/exception-handling
title: Obsługa wyjątków w ASP.NET Web API | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/12/2012
ms.topic: article
ms.assetid: cbebeb37-2594-41f2-b71a-f4f26520d512
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/error-handling/exception-handling
msc.type: authoredcontent
ms.openlocfilehash: c65ddcca012840d70ab5a33af92edb30041be971
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
ms.locfileid: "26566393"
---
<a name="exception-handling-in-aspnet-web-api"></a>Obsługa wyjątków w Web API platformy ASP.NET
====================
przez [Wasson Jan](https://github.com/MikeWasson)

W tym artykule opisano błąd i obsługa wyjątków w interfejsu API sieci Web platformy ASP.NET.

- [HttpResponseException](#httpresponserexception)
- [Filtry wyjątków](#exception_filters)
- [Rejestrowanie filtry wyjątków](#registering_exception_filters)
- [HttpError](#httperror)

<a id="httpresponserexception"></a>
## <a name="httpresponseexception"></a>HttpResponseException

Co się stanie, jeśli kontroler Web API zgłasza nieprzechwycony wyjątek? Domyślnie większość wyjątki są przekształcane na odpowiedzi HTTP z kodem stanu 500, wewnętrzny błąd serwera.

**HttpResponseException** szczególnych przypadkach jest typu. Ten wyjątek zwraca do kod stanu HTTP, określonego w Konstruktorze wyjątku. Na przykład następująca metoda zwraca 404, nie można odnaleźć, jeśli *identyfikator* parametr jest nieprawidłowy.

[!code-csharp[Main](exception-handling/samples/sample1.cs)]

Uzyskać większą kontrolę nad odpowiedzi, możesz również utworzyć komunikat całej odpowiedzi i dołącz ją z **HttpResponseException:** 

[!code-csharp[Main](exception-handling/samples/sample2.cs)]

<a id="exception_filters"></a>
## <a name="exception-filters"></a>Filtry wyjątków

Można dostosować sposób obsługi wyjątków interfejsu API sieci Web pisząc *filtru wyjątków*. Filtra wyjątku jest wykonywany podczas kontrolera metodę nieobsługiwany wyjątek, który jest *nie* **HttpResponseException** wyjątku. **HttpResponseException** typu jest szczególnych przypadkach, ponieważ został zaprojektowany specjalnie z myślą o zwracanie odpowiedzi HTTP.

Filtry wyjątków zaimplementować **System.Web.Http.Filters.IExceptionFilter** interfejsu. Najprostszym sposobem pisanie filtra wyjątku jest pochodzić z **System.Web.Http.Filters.ExceptionFilterAttribute** klasy i zastąpić **OnException** metody.

> [!NOTE]
> Filtry wyjątków w interfejsie API sieci Web platformy ASP.NET są podobne do tych na platformie ASP.NET MVC. Jednak zostały zgłoszone w oddzielnych przestrzeni nazw i funkcja oddzielnie. W szczególności **atrybutu HandleErrorAttribute** klasa używana w MVC nie zapewnia obsługi wyjątków zgłaszanych przez kontrolery interfejsu API sieci Web.


Oto filtr, który konwertuje **notimplementedexception —** 501 Nie zaimplementowano kodu wyjątków do stanu HTTP:

[!code-csharp[Main](exception-handling/samples/sample3.cs)]

**Odpowiedzi** właściwość **HttpActionExecutedContext** obiekt zawiera komunikat odpowiedzi HTTP, które zostaną wysłane do klienta.

<a id="registering_exception_filters"></a>
## <a name="registering-exception-filters"></a>Rejestrowanie filtry wyjątków

Istnieje kilka sposobów, aby zarejestrować filtru wyjątków interfejsu API sieci Web:

- Przez akcję
- Kontroler
- Globalny

Aby zastosować filtr do określonej akcji, należy dodać filtr jako atrybut do akcji:

[!code-csharp[Main](exception-handling/samples/sample4.cs)]

Aby zastosować filtr do wszystkich akcji w kontrolerze, Dodaj filtr jako atrybut do klasy kontrolera:

[!code-csharp[Main](exception-handling/samples/sample5.cs)]

Aby zastosować filtr globalnie do wszystkich kontrolerów interfejsu API sieci Web, należy dodać wystąpienia filtr, aby **GlobalConfiguration.Configuration.Filters** kolekcji. Filtry wyjątkiem w tej kolekcji mają zastosowanie do dowolnej akcji kontrolera interfejsu API sieci Web.

[!code-csharp[Main](exception-handling/samples/sample6.cs)]

Użycie szablonu projektu "Platformy ASP.NET MVC 4 aplikacji sieci Web" Aby utworzyć projekt, umieść kod konfiguracji interfejsu API sieci Web wewnątrz `WebApiConfig` klasy, która znajduje się w aplikacji\_folder początkowy:

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

<a id="httperror"></a>
## <a name="httperror"></a>HttpError

**HttpError** obiekt zapewnia spójny sposób, aby zwrócić informacje o błędzie w treści odpowiedzi. Poniższy przykład przedstawia sposób zwrócenia kod stanu HTTP 404 (nie znaleziono) z **HttpError** w treści odpowiedzi.

[!code-csharp[Main](exception-handling/samples/sample8.cs)]

**CreateErrorResponse** — metoda rozszerzenia jest zdefiniowany w **System.Net.Http.HttpRequestMessageExtensions** klasy. Wewnętrznie **CreateErrorResponse** tworzy **HttpError** wystąpienia, a następnie tworzy **HttpResponseMessage** zawierający **HttpError**.

W tym przykładzie Jeśli metoda zakończy się pomyślnie, zwraca produktu w odpowiedzi HTTP. Ale jeśli nie odnaleziono żądanego produktu, odpowiedź HTTP zawiera **HttpError** w treści żądania. Odpowiedź może wyglądać następująco:

[!code-console[Main](exception-handling/samples/sample9.cmd)]

Zwróć uwagę, że **HttpError** została wykonana serializacja JSON, w tym przykładzie. Jedną z zalet przy użyciu **HttpError** jest, że przechodzi ona przez taki sam [negocjowanie zawartości](../formats-and-model-binding/content-negotiation.md) i serializacji przetwarzania jak wszystkie inne silnie typizowanym modelem.

### <a name="httperror-and-model-validation"></a>HttpError i weryfikacja modelu

Do weryfikacji modelu, można przekazać stan modelu do **CreateErrorResponse**, aby uwzględnić w odpowiedzi na błędy sprawdzania poprawności:

[!code-csharp[Main](exception-handling/samples/sample10.cs)]

W tym przykładzie może zwrócić następującą odpowiedź:

[!code-console[Main](exception-handling/samples/sample11.cmd)]

Aby uzyskać więcej informacji o weryfikacji modelu, zobacz [weryfikacji modelu w ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).

### <a name="using-httperror-with-httpresponseexception"></a>Korzystanie z HttpResponseException HttpError

Zwraca poprzednich przykładach **HttpResponseMessage** komunikat z akcji kontrolera, ale można również użyć **HttpResponseException** do zwrócenia **HttpError**. Dzięki temu można zwrócić silnie typizowanym modelem w przypadku powodzenia normalne, podczas zwracania nadal **HttpError** Jeśli występuje błąd:

[!code-csharp[Main](exception-handling/samples/sample12.cs)]
