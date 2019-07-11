---
title: ASP.NET Core Web API help pages w strukturze Swagger / interfejsu OpenAPI
author: rsuter
description: Ten samouczek zawiera wskazówki dotyczące dodawania struktury Swagger, aby generować dokumentację i strony dla aplikacji interfejsu API sieci Web pomocy.
ms.author: scaddie
ms.custom: mvc
ms.date: 09/20/2018
uid: tutorials/web-api-help-pages-using-swagger
ms.openlocfilehash: 456e1dcac583705d815c7578c0dba5c52c5dbb3a
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67815401"
---
# <a name="aspnet-core-web-api-help-pages-with-swagger--openapi"></a><span data-ttu-id="d44fa-103">Strony sieci web platformy ASP.NET Core pomocy interfejsu API, w strukturze Swagger / interfejsu OpenAPI</span><span class="sxs-lookup"><span data-stu-id="d44fa-103">ASP.NET Core web API help pages with Swagger / OpenAPI</span></span>

<span data-ttu-id="d44fa-104">Przez [Christoph Nienaber](https://twitter.com/zuckerthoben) i [Rico Suter](http://rsuter.com)</span><span class="sxs-lookup"><span data-stu-id="d44fa-104">By [Christoph Nienaber](https://twitter.com/zuckerthoben) and [Rico Suter](http://rsuter.com)</span></span>

<span data-ttu-id="d44fa-105">Podczas korzystania z internetowego interfejsu API, informacje o jego różne metody może stanowić wyzwanie dla dewelopera.</span><span class="sxs-lookup"><span data-stu-id="d44fa-105">When consuming a Web API, understanding its various methods can be challenging for a developer.</span></span> <span data-ttu-id="d44fa-106">[Struktury swagger](https://swagger.io/), znane również jako [OpenAPI](https://www.openapis.org/), rozwiązuje problem Generowanie przydatne stron pomocy i dokumentacji dla interfejsów API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="d44fa-106">[Swagger](https://swagger.io/), also known as [OpenAPI](https://www.openapis.org/), solves the problem of generating useful documentation and help pages for Web APIs.</span></span> <span data-ttu-id="d44fa-107">Zapewnia korzyści, takich jak dokumentacja interaktywne, generowanie zestawów SDK klienta i odnajdywania interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="d44fa-107">It provides benefits such as interactive documentation, client SDK generation, and API discoverability.</span></span>

<span data-ttu-id="d44fa-108">W tym artykule [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) i [NSwag](https://github.com/RicoSuter/NSwag) implementacji .NET Swagger są pokazywane:</span><span class="sxs-lookup"><span data-stu-id="d44fa-108">In this article, the [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) and [NSwag](https://github.com/RicoSuter/NSwag) .NET Swagger implementations are showcased:</span></span>

* <span data-ttu-id="d44fa-109">**Swashbuckle.AspNetCore** to projekt typu open source do generowania dokumentów struktury Swagger dla interfejsów API sieci Web programu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d44fa-109">**Swashbuckle.AspNetCore** is an open source project for generating Swagger documents for ASP.NET Core Web APIs.</span></span>

* <span data-ttu-id="d44fa-110">**NSwag** inny projekt open source do generowania dokumentów struktury Swagger i integrowanie [interfejs użytkownika struktury Swagger](https://swagger.io/swagger-ui/) lub [ReDoc](https://github.com/Rebilly/ReDoc) do platformy ASP.NET Core internetowych interfejsów API.</span><span class="sxs-lookup"><span data-stu-id="d44fa-110">**NSwag** is another open source project for generating Swagger documents and integrating [Swagger UI](https://swagger.io/swagger-ui/) or [ReDoc](https://github.com/Rebilly/ReDoc) into ASP.NET Core web APIs.</span></span> <span data-ttu-id="d44fa-111">Ponadto NSwag oferuje podejścia, aby wygenerować C# i TypeScript kodu klienta dla interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="d44fa-111">Additionally, NSwag offers approaches to generate C# and TypeScript client code for your API.</span></span>

## <a name="what-is-swagger--openapi"></a><span data-ttu-id="d44fa-112">Co to jest struktury Swagger / OpenAPI?</span><span class="sxs-lookup"><span data-stu-id="d44fa-112">What is Swagger / OpenAPI?</span></span>

<span data-ttu-id="d44fa-113">Struktury swagger jest niezależny od języka opisu [REST](https://en.wikipedia.org/wiki/Representational_state_transfer) interfejsów API.</span><span class="sxs-lookup"><span data-stu-id="d44fa-113">Swagger is a language-agnostic specification for describing [REST](https://en.wikipedia.org/wiki/Representational_state_transfer) APIs.</span></span> <span data-ttu-id="d44fa-114">Projekt struktury Swagger zostało przekazanych [inicjatywy OpenAPI](https://www.openapis.org/), gdzie go jest teraz nazywana interfejsu OpenAPI.</span><span class="sxs-lookup"><span data-stu-id="d44fa-114">The Swagger project was donated to the [OpenAPI Initiative](https://www.openapis.org/), where it's now referred to as OpenAPI.</span></span> <span data-ttu-id="d44fa-115">Obie nazwy są używane zamiennie. preferowane jest jednak interfejsu OpenAPI.</span><span class="sxs-lookup"><span data-stu-id="d44fa-115">Both names are used interchangeably; however, OpenAPI is preferred.</span></span> <span data-ttu-id="d44fa-116">Umożliwia on zarówno komputerów, jak i ludzi, aby zapoznać się z funkcjami usługi bez żadnych bezpośredni dostęp do implementacji (kod źródłowy, dostępu do sieci, dokumentacji).</span><span class="sxs-lookup"><span data-stu-id="d44fa-116">It allows both computers and humans to understand the capabilities of a service without any direct access to the implementation (source code, network access, documentation).</span></span> <span data-ttu-id="d44fa-117">Jeden cel jest minimalizacja ilości pracy wymaganej do nawiązywania połączenia z usługami usunięte skojarzenia.</span><span class="sxs-lookup"><span data-stu-id="d44fa-117">One goal is to minimize the amount of work needed to connect disassociated services.</span></span> <span data-ttu-id="d44fa-118">Innym celem jest skrócenie czasu wymaganego do dokładnie dokumentu usługi.</span><span class="sxs-lookup"><span data-stu-id="d44fa-118">Another goal is to reduce the amount of time needed to accurately document a service.</span></span>

## <a name="swagger-specification-swaggerjson"></a><span data-ttu-id="d44fa-119">Specyfikacja swagger (swagger.json)</span><span class="sxs-lookup"><span data-stu-id="d44fa-119">Swagger specification (swagger.json)</span></span>

<span data-ttu-id="d44fa-120">Core do usługi flow struktury Swagger jest specyfikacja Swagger&mdash;domyślnie o nazwie dokumentu *swagger.json*.</span><span class="sxs-lookup"><span data-stu-id="d44fa-120">The core to the Swagger flow is the Swagger specification&mdash;by default, a document named *swagger.json*.</span></span> <span data-ttu-id="d44fa-121">Jest ona generowana przez struktury Swagger narzędzie łańcucha (lub innych implementacji go) na podstawie Twojej usługi.</span><span class="sxs-lookup"><span data-stu-id="d44fa-121">It's generated by the Swagger tool chain (or third-party implementations of it) based on your service.</span></span> <span data-ttu-id="d44fa-122">Opisuje funkcje interfejsu API i uzyskiwania dostępu do niego za pośrednictwem protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="d44fa-122">It describes the capabilities of your API and how to access it with HTTP.</span></span> <span data-ttu-id="d44fa-123">Jej dyski interfejsu użytkownika programu Swagger i jest używany przez łańcuch narzędzi, aby umożliwić generowanie kodu klienta wykrywania i.</span><span class="sxs-lookup"><span data-stu-id="d44fa-123">It drives the Swagger UI and is used by the tool chain to enable discovery and client code generation.</span></span> <span data-ttu-id="d44fa-124">Oto przykład specyfikacji Swagger, zmniejszone dla zwięzłości:</span><span class="sxs-lookup"><span data-stu-id="d44fa-124">Here's an example of a Swagger specification, reduced for brevity:</span></span>

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

## <a name="swagger-ui"></a><span data-ttu-id="d44fa-125">Swagger UI</span><span class="sxs-lookup"><span data-stu-id="d44fa-125">Swagger UI</span></span>

<span data-ttu-id="d44fa-126">[Interfejs użytkownika struktury swagger](https://swagger.io/swagger-ui/) oferuje oparte na sieci web interfejs użytkownika, który zawiera informacje dotyczące usługi, za pomocą wygenerowanego specyfikacją struktury Swagger.</span><span class="sxs-lookup"><span data-stu-id="d44fa-126">[Swagger UI](https://swagger.io/swagger-ui/) offers a web-based UI that provides information about the service, using the generated Swagger specification.</span></span> <span data-ttu-id="d44fa-127">Zarówno pakiet Swashbuckle, jak i NSwag obejmują wbudowana wersja interfejs użytkownika struktury Swagger, dzięki czemu mogą być hostowane w aplikacji platformy ASP.NET Core przy użyciu wywołania rejestracji oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="d44fa-127">Both Swashbuckle and NSwag include an embedded version of Swagger UI, so that it can be hosted in your ASP.NET Core app using a middleware registration call.</span></span> <span data-ttu-id="d44fa-128">Internetowy interfejs użytkownika wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="d44fa-128">The web UI looks like this:</span></span>

![Swagger UI](web-api-help-pages-using-swagger/_static/swagger-ui.png)

<span data-ttu-id="d44fa-130">W interfejsie użytkownika można przetestować każdej metody akcji publicznych w kontrolerach.</span><span class="sxs-lookup"><span data-stu-id="d44fa-130">Each public action method in your controllers can be tested from the UI.</span></span> <span data-ttu-id="d44fa-131">Kliknij nazwę metody, aby rozwinąć sekcję.</span><span class="sxs-lookup"><span data-stu-id="d44fa-131">Click a method name to expand the section.</span></span> <span data-ttu-id="d44fa-132">Dodaj wszystkie niezbędne parametry, a następnie kliknij przycisk **wypróbuj!** .</span><span class="sxs-lookup"><span data-stu-id="d44fa-132">Add any necessary parameters, and click **Try it out!**.</span></span>

![Przykład testu pobrać programu Swagger](web-api-help-pages-using-swagger/_static/get-try-it-out.png)

> [!NOTE]
> <span data-ttu-id="d44fa-134">Wersja interfejs użytkownika struktury Swagger, umożliwiający zrzuty ekranu jest w wersji 2.</span><span class="sxs-lookup"><span data-stu-id="d44fa-134">The Swagger UI version used for the screenshots is version 2.</span></span> <span data-ttu-id="d44fa-135">Na przykład w wersji 3, zobacz [przykład Petstore](https://petstore.swagger.io/).</span><span class="sxs-lookup"><span data-stu-id="d44fa-135">For a version 3 example, see [Petstore example](https://petstore.swagger.io/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="d44fa-136">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="d44fa-136">Next steps</span></span>

* [<span data-ttu-id="d44fa-137">Wprowadzenie do pakietu Swashbuckle</span><span class="sxs-lookup"><span data-stu-id="d44fa-137">Get started with Swashbuckle</span></span>](xref:tutorials/get-started-with-swashbuckle)
* [<span data-ttu-id="d44fa-138">Wprowadzenie do łańcucha narzędzi NSwag</span><span class="sxs-lookup"><span data-stu-id="d44fa-138">Get started with NSwag</span></span>](xref:tutorials/get-started-with-nswag)
