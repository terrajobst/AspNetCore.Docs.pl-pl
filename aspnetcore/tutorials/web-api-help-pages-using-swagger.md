---
title: "Strony pomocy Core interfejsu API sieci Web ASP.NET przy użyciu programu Swagger / Otwórz interfejs API"
author: rsuter
description: "Ten samouczek zawiera wskazówki dodawania programu Swagger do generowania dokumentacji i strony dla aplikacji interfejsu API sieci Web pomocy."
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 03/09/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/web-api-help-pages-using-swagger
ms.openlocfilehash: f0e2d97111c95d44269a2023a3349d86ccbf8bad
ms.sourcegitcommit: 9622bdc6326c28c3322c70000468a80ef21ad376
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/12/2018
---
# <a name="aspnet-core-web-api-help-pages-using-swagger--open-api"></a>Strony pomocy Core interfejsu API sieci Web ASP.NET przy użyciu programu Swagger / Otwórz interfejs API

Przez [Christoph Nienaber](https://twitter.com/zuckerthoben) i [Rico Suter](http://rsuter.com)

W przypadku uzyskiwania dostępu przez interfejs API sieci Web, opis jego różnych metod może być wyzwaniem dla deweloperów. [Swagger](https://swagger.io/), znanej także jako otwarty interfejs API, rozwiązuje problem Generowanie przydatne stron dokumentacji i pomocy dla interfejsów API sieci Web. Zapewnia korzyści, takich jak interakcyjne dokumentacji, generowanie zestawów SDK klienta i odnajdowanie interfejsu API.

W tym artykule [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) i [NSwag](https://github.com/RSuter/NSwag) są pokazywane implementacji programu Swagger .NET:

* **Swashbuckle.AspNetCore** to projekt open source służący do generowania dokumentów programu Swagger dla interfejsów API platformy ASP.NET Core sieci Web.

* **NSwag** jest inny projekt typu open source do integracji [interfejs użytkownika programu Swagger](https://swagger.io/swagger-ui/) lub [ReDoc](https://github.com/Rebilly/ReDoc) do interfejsów API platformy ASP.NET Core sieci Web. Zapewnia ona podejścia do generowania C# i TypeScript kodu klienta interfejsu API.

## <a name="what-is-swagger--open-api"></a>Co to jest Swagger / Otwórz interfejsu API?

Struktury swagger jest specyfikacją niezależny od języka opisu [REST](https://en.wikipedia.org/wiki/Representational_state_transfer) interfejsów API. Projekt struktury Swagger została przekazana na [inicjatywy OpenAPI](https://www.openapis.org/), gdzie jest ona teraz określana jako otwarty interfejs API. Obie nazwy są używane zamiennie. preferowane jest jednak otwarty interfejs API. Umożliwia on zarówno komputerów, jak i ludzi, aby zapoznać się z funkcjami usługi bez żadnych bezpośredni dostęp do wykonania (kod źródłowy, dostępu do sieci, dokumentacji). Jeden celem jest aby zminimalizować ilość pracy wymagane do połączenia usługi już. Innym celem jest aby skrócić czas potrzebny na dokładnie dokumentu usługi.

## <a name="swagger-specification-swaggerjson"></a>Specyfikacja swagger (swagger.json)

Podstawowe z przepływem struktury Swagger jest specyfikacją Swagger&mdash;domyślnie dokumentu o nazwie *swagger.json*. Jest ona generowana przez Swagger narzędzie łańcucha (lub innych firm implementacje go) oparte na usłudze. Opisuje funkcje interfejsu API i jak do niej dostęp przy użyciu protokołu HTTP. Dyski interfejsu użytkownika programu Swagger, a jest używany przez łańcucha narzędzi, aby umożliwić generowanie kodu klienta wykrywania i. Oto przykład specyfikacji Swagger zmniejszona skrócenia:

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

[Interfejs użytkownika struktury swagger](https://swagger.io/swagger-ui/) oferuje opartych na sieci web interfejsu użytkownika, który zawiera informacje dotyczące usług, przy użyciu specyfikacji wygenerowanego struktury Swagger. Zarówno Swashbuckle, jak i NSwag zawierają osadzone wersję interfejsu użytkownika programu Swagger, dzięki czemu mogą być hostowane w aplikacji platformy ASP.NET Core za pomocą oprogramowania pośredniczącego wywołanie rejestracji. Witryna sieci web UI wygląda następująco:

![Swagger UI](web-api-help-pages-using-swagger/_static/swagger-ui.png)

Każda metoda akcji publicznego w kontrolerach można przetestować w interfejsie użytkownika. Kliknij nazwę metody, aby rozwinąć sekcję. Dodaj wszystkie niezbędne parametry, a następnie kliknij przycisk **Wypróbuj ją!**.

![Przykład struktury Swagger UZYSKAĆ testu](web-api-help-pages-using-swagger/_static/get-try-it-out.png)

> [!NOTE]
> Wersja programu Swagger interfejsu użytkownika używana do zrzuty ekranu jest w wersji 2. Na przykład w wersji 3, zobacz [przykład Petstore](http://petstore.swagger.io/).

## <a name="next-steps"></a>Następne kroki

* [Rozpoczynanie pracy z Swashbuckle](xref:tutorials/get-started-with-swashbuckle)
* [Rozpoczynanie pracy z NSwag](xref:tutorials/get-started-with-nswag)
