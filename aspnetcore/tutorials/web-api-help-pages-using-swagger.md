---
title: ASP.NET Core Web API help pages w strukturze Swagger / interfejsu OpenAPI
author: RicoSuter
description: Ten samouczek zawiera wskazówki dotyczące dodawania struktury Swagger, aby generować dokumentację i strony dla aplikacji interfejsu API sieci Web pomocy.
ms.author: scaddie
ms.custom: mvc
ms.date: 12/07/2019
uid: tutorials/web-api-help-pages-using-swagger
ms.openlocfilehash: 4408e02996b958bf009903aa1e4eeda9ad4f457c
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78658474"
---
# <a name="aspnet-core-web-api-help-pages-with-swagger--openapi"></a>Strony sieci web platformy ASP.NET Core pomocy interfejsu API, w strukturze Swagger / interfejsu OpenAPI

[Christoph Nienaber](https://twitter.com/zuckerthoben) i [Portoryko Suter](https://blog.rsuter.com/)

Podczas korzystania z internetowego interfejsu API, informacje o jego różne metody może stanowić wyzwanie dla dewelopera. Struktura [Swagger](https://swagger.io/), znana również jako [openapi](https://www.openapis.org/), rozwiązuje problem związany z generowaniem użytecznej dokumentacji i stron pomocy dla interfejsów API sieci Web. Zapewnia korzyści, takich jak dokumentacja interaktywne, generowanie zestawów SDK klienta i odnajdywania interfejsu API.

W tym artykule opisano implementacje [Swashbuckle. AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) i [NSwag](https://github.com/RicoSuter/NSwag) .NET Swagger:

* **Swashbuckle. AspNetCore** to projekt Open Source służący do generowania dokumentów struktury Swagger dla ASP.NET Core interfejsów API sieci Web.

* **NSwag** jest innym projektem Open Source na potrzeby generowania dokumentów struktury Swagger i INTEGROWANIA [interfejsu użytkownika struktury Swagger](https://swagger.io/swagger-ui/) lub [ReDoc](https://github.com/Rebilly/ReDoc) do ASP.NET Core interfejsów API sieci Web. Ponadto NSwag oferuje podejścia, aby wygenerować C# i TypeScript kodu klienta dla interfejsu API.

## <a name="what-is-swagger--openapi"></a>Co to jest struktury Swagger / OpenAPI?

Swagger to specyfikacja języka niezależny od do opisywania interfejsów API [rest](https://en.wikipedia.org/wiki/Representational_state_transfer) . Projekt Swagger został przekazano do [inicjatywy openapi](https://www.openapis.org/), w której jest teraz określany jako openapi. Obie nazwy są używane zamiennie. preferowane jest jednak interfejsu OpenAPI. Umożliwia on zarówno komputerów, jak i ludzi, aby zapoznać się z funkcjami usługi bez żadnych bezpośredni dostęp do implementacji (kod źródłowy, dostępu do sieci, dokumentacji). Jeden cel jest minimalizacja ilości pracy wymaganej do nawiązywania połączenia z usługami usunięte skojarzenia. Innym celem jest skrócenie czasu wymaganego do dokładnie dokumentu usługi.

## <a name="swagger-specification-swaggerjson"></a>Specyfikacja swagger (swagger.json)

Rdzeń do przepływu Swagger to specyfikacja struktury Swagger&mdash;domyślnie dokument o nazwie *Swagger. JSON*. Jest ona generowana przez struktury Swagger narzędzie łańcucha (lub innych implementacji go) na podstawie Twojej usługi. Opisuje funkcje interfejsu API i uzyskiwania dostępu do niego za pośrednictwem protokołu HTTP. Jej dyski interfejsu użytkownika programu Swagger i jest używany przez łańcuch narzędzi, aby umożliwić generowanie kodu klienta wykrywania i. Oto przykład specyfikacji Swagger, zmniejszone dla zwięzłości:

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

[Interfejs użytkownika struktury Swagger](https://swagger.io/swagger-ui/) oferuje interfejs użytkownika oparty na sieci Web, który zawiera informacje o usłudze, przy użyciu wygenerowanej specyfikacji struktury Swagger. Zarówno pakiet Swashbuckle, jak i NSwag obejmują wbudowana wersja interfejs użytkownika struktury Swagger, dzięki czemu mogą być hostowane w aplikacji platformy ASP.NET Core przy użyciu wywołania rejestracji oprogramowania pośredniczącego. Internetowy interfejs użytkownika wygląda następująco:

![Swagger UI](web-api-help-pages-using-swagger/_static/swagger-ui.png)

W interfejsie użytkownika można przetestować każdej metody akcji publicznych w kontrolerach. Kliknij nazwę metody, aby rozwinąć sekcję. Dodaj wszelkie niezbędne parametry i kliknij przycisk **Wypróbuj!** .

![Przykład testu pobrać programu Swagger](web-api-help-pages-using-swagger/_static/get-try-it-out.png)

> [!NOTE]
> Wersja interfejs użytkownika struktury Swagger, umożliwiający zrzuty ekranu jest w wersji 2. Aby zapoznać się z wersją 3 przykład, zobacz [przykład petstore](https://petstore.swagger.io/).

## <a name="next-steps"></a>Następne kroki

* [Wprowadzenie do pakietu Swashbuckle](xref:tutorials/get-started-with-swashbuckle)
* [Wprowadzenie do łańcucha narzędzi NSwag](xref:tutorials/get-started-with-nswag)
