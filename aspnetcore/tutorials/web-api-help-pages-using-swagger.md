---
title: ASP.NET Core Web API help pages w strukturze Swagger / interfejsu OpenAPI
author: rsuter
description: Ten samouczek zawiera wskazówki dotyczące dodawania struktury Swagger, aby generować dokumentację i strony dla aplikacji interfejsu API sieci Web pomocy.
ms.author: scaddie
ms.custom: mvc
ms.date: 09/20/2018
uid: tutorials/web-api-help-pages-using-swagger
ms.openlocfilehash: d7a6ed158dcb464bb80c83773ed7d455b25ce44b
ms.sourcegitcommit: 49faca2644590fc081d86db46ea5e29edfc28b7b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/09/2018
ms.locfileid: "53121547"
---
# <a name="aspnet-core-web-api-help-pages-with-swagger--openapi"></a>Strony sieci web platformy ASP.NET Core pomocy interfejsu API, w strukturze Swagger / interfejsu OpenAPI

Przez [Christoph Nienaber](https://twitter.com/zuckerthoben) i [Rico Suter](http://rsuter.com)

Podczas korzystania z internetowego interfejsu API, informacje o jego różne metody może stanowić wyzwanie dla dewelopera. [Struktury swagger](https://swagger.io/), znane również jako [OpenAPI](https://www.openapis.org/), rozwiązuje problem Generowanie przydatne stron pomocy i dokumentacji dla interfejsów API sieci Web. Zapewnia korzyści, takich jak dokumentacja interaktywne, generowanie zestawów SDK klienta i odnajdywania interfejsu API.

W tym artykule [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) i [NSwag](https://github.com/RSuter/NSwag) implementacji .NET Swagger są pokazywane:

* **Swashbuckle.AspNetCore** to projekt typu open source do generowania dokumentów struktury Swagger dla interfejsów API sieci Web programu ASP.NET Core.

* **NSwag** inny projekt open source do generowania dokumentów struktury Swagger i integrowanie [interfejs użytkownika struktury Swagger](https://swagger.io/swagger-ui/) lub [ReDoc](https://github.com/Rebilly/ReDoc) do platformy ASP.NET Core internetowych interfejsów API. Ponadto NSwag oferuje podejścia, aby wygenerować C# i TypeScript kodu klienta dla interfejsu API.

## <a name="what-is-swagger--openapi"></a>Co to jest struktury Swagger / OpenAPI?

Struktury swagger jest niezależny od języka opisu [REST](https://en.wikipedia.org/wiki/Representational_state_transfer) interfejsów API. Projekt struktury Swagger zostało przekazanych [inicjatywy OpenAPI](https://www.openapis.org/), gdzie go jest teraz nazywana interfejsu OpenAPI. Obie nazwy są używane zamiennie. preferowane jest jednak interfejsu OpenAPI. Umożliwia on zarówno komputerów, jak i ludzi, aby zapoznać się z funkcjami usługi bez żadnych bezpośredni dostęp do implementacji (kod źródłowy, dostępu do sieci, dokumentacji). Jeden cel jest minimalizacja ilości pracy wymaganej do nawiązywania połączenia z usługami usunięte skojarzenia. Innym celem jest skrócenie czasu wymaganego do dokładnie dokumentu usługi.

## <a name="swagger-specification-swaggerjson"></a>Specyfikacja swagger (swagger.json)

Core do usługi flow struktury Swagger jest specyfikacja Swagger&mdash;domyślnie o nazwie dokumentu *swagger.json*. Jest ona generowana przez struktury Swagger narzędzie łańcucha (lub innych implementacji go) na podstawie Twojej usługi. Opisuje funkcje interfejsu API i uzyskiwania dostępu do niego za pośrednictwem protokołu HTTP. Jej dyski interfejsu użytkownika programu Swagger i jest używany przez łańcuch narzędzi, aby umożliwić generowanie kodu klienta wykrywania i. Oto przykład specyfikacji Swagger, zmniejszone dla zwięzłości:

```json
{
   "swagger": "2.0",
   "info": {
       "version": "v1",
       "title": "API V1"
   },
   "basePath": "/",
   "paths": {
       "/api/Todo": {
           "get": {
               "tags": [
                   "Todo"
               ],
               "operationId": "ApiTodoGet",
               "consumes": [],
               "produces": [
                   "text/plain",
                   "application/json",
                   "text/json"
               ],
               "responses": {
                   "200": {
                       "description": "Success",
                       "schema": {
                           "type": "array",
                           "items": {
                               "$ref": "#/definitions/TodoItem"
                           }
                       }
                   }
                }
           },
           "post": {
               ...
           }
       },
       "/api/Todo/{id}": {
           "get": {
               ...
           },
           "put": {
               ...
           },
           "delete": {
               ...
   },
   "definitions": {
       "TodoItem": {
           "type": "object",
            "properties": {
                "id": {
                    "format": "int64",
                    "type": "integer"
                },
                "name": {
                    "type": "string"
                },
                "isComplete": {
                    "default": false,
                    "type": "boolean"
                }
            }
       }
   },
   "securityDefinitions": {}
}
```

## <a name="swagger-ui"></a>Swagger UI

[Interfejs użytkownika struktury swagger](https://swagger.io/swagger-ui/) oferuje oparte na sieci web interfejs użytkownika, który zawiera informacje dotyczące usługi, za pomocą wygenerowanego specyfikacją struktury Swagger. Zarówno pakiet Swashbuckle, jak i NSwag obejmują wbudowana wersja interfejs użytkownika struktury Swagger, dzięki czemu mogą być hostowane w aplikacji platformy ASP.NET Core przy użyciu wywołania rejestracji oprogramowania pośredniczącego. Internetowy interfejs użytkownika wygląda następująco:

![Swagger UI](web-api-help-pages-using-swagger/_static/swagger-ui.png)

W interfejsie użytkownika można przetestować każdej metody akcji publicznych w kontrolerach. Kliknij nazwę metody, aby rozwinąć sekcję. Dodaj wszystkie niezbędne parametry, a następnie kliknij przycisk **wypróbuj!**.

![Przykład testu pobrać programu Swagger](web-api-help-pages-using-swagger/_static/get-try-it-out.png)

> [!NOTE]
> Wersja interfejs użytkownika struktury Swagger, umożliwiający zrzuty ekranu jest w wersji 2. Na przykład w wersji 3, zobacz [przykład Petstore](http://petstore.swagger.io/).

## <a name="next-steps"></a>Następne kroki

* [Wprowadzenie do pakietu Swashbuckle](xref:tutorials/get-started-with-swashbuckle)
* [Wprowadzenie do łańcucha narzędzi NSwag](xref:tutorials/get-started-with-nswag)
