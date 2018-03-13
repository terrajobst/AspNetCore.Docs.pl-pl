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
# <a name="aspnet-core-web-api-help-pages-using-swagger--open-api"></a><span data-ttu-id="43ca1-103">Strony pomocy Core interfejsu API sieci Web ASP.NET przy użyciu programu Swagger / Otwórz interfejs API</span><span class="sxs-lookup"><span data-stu-id="43ca1-103">ASP.NET Core Web API help pages using Swagger / Open API</span></span>

<span data-ttu-id="43ca1-104">Przez [Christoph Nienaber](https://twitter.com/zuckerthoben) i [Rico Suter](http://rsuter.com)</span><span class="sxs-lookup"><span data-stu-id="43ca1-104">By [Christoph Nienaber](https://twitter.com/zuckerthoben) and [Rico Suter](http://rsuter.com)</span></span>

<span data-ttu-id="43ca1-105">W przypadku uzyskiwania dostępu przez interfejs API sieci Web, opis jego różnych metod może być wyzwaniem dla deweloperów.</span><span class="sxs-lookup"><span data-stu-id="43ca1-105">When consuming a Web API, understanding its various methods can be challenging for a developer.</span></span> <span data-ttu-id="43ca1-106">[Swagger](https://swagger.io/), znanej także jako otwarty interfejs API, rozwiązuje problem Generowanie przydatne stron dokumentacji i pomocy dla interfejsów API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="43ca1-106">[Swagger](https://swagger.io/), also known as Open API, solves the problem of generating useful documentation and help pages for Web APIs.</span></span> <span data-ttu-id="43ca1-107">Zapewnia korzyści, takich jak interakcyjne dokumentacji, generowanie zestawów SDK klienta i odnajdowanie interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="43ca1-107">It provides benefits such as interactive documentation, client SDK generation, and API discoverability.</span></span>

<span data-ttu-id="43ca1-108">W tym artykule [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) i [NSwag](https://github.com/RSuter/NSwag) są pokazywane implementacji programu Swagger .NET:</span><span class="sxs-lookup"><span data-stu-id="43ca1-108">In this article, the [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) and [NSwag](https://github.com/RSuter/NSwag) .NET Swagger implementations are showcased:</span></span>

* <span data-ttu-id="43ca1-109">**Swashbuckle.AspNetCore** to projekt open source służący do generowania dokumentów programu Swagger dla interfejsów API platformy ASP.NET Core sieci Web.</span><span class="sxs-lookup"><span data-stu-id="43ca1-109">**Swashbuckle.AspNetCore** is an open source project for generating Swagger documents for ASP.NET Core Web APIs.</span></span>

* <span data-ttu-id="43ca1-110">**NSwag** jest inny projekt typu open source do integracji [interfejs użytkownika programu Swagger](https://swagger.io/swagger-ui/) lub [ReDoc](https://github.com/Rebilly/ReDoc) do interfejsów API platformy ASP.NET Core sieci Web.</span><span class="sxs-lookup"><span data-stu-id="43ca1-110">**NSwag** is another open source project for integrating [Swagger UI](https://swagger.io/swagger-ui/) or [ReDoc](https://github.com/Rebilly/ReDoc) into ASP.NET Core Web APIs.</span></span> <span data-ttu-id="43ca1-111">Zapewnia ona podejścia do generowania C# i TypeScript kodu klienta interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="43ca1-111">It offers approaches to generate C# and TypeScript client code for your API.</span></span>

## <a name="what-is-swagger--open-api"></a><span data-ttu-id="43ca1-112">Co to jest Swagger / Otwórz interfejsu API?</span><span class="sxs-lookup"><span data-stu-id="43ca1-112">What is Swagger / Open API?</span></span>

<span data-ttu-id="43ca1-113">Struktury swagger jest specyfikacją niezależny od języka opisu [REST](https://en.wikipedia.org/wiki/Representational_state_transfer) interfejsów API.</span><span class="sxs-lookup"><span data-stu-id="43ca1-113">Swagger is a language-agnostic specification for describing [REST](https://en.wikipedia.org/wiki/Representational_state_transfer) APIs.</span></span> <span data-ttu-id="43ca1-114">Projekt struktury Swagger została przekazana na [inicjatywy OpenAPI](https://www.openapis.org/), gdzie jest ona teraz określana jako otwarty interfejs API.</span><span class="sxs-lookup"><span data-stu-id="43ca1-114">The Swagger project was donated to the [OpenAPI Initiative](https://www.openapis.org/), where it's now referred to as Open API.</span></span> <span data-ttu-id="43ca1-115">Obie nazwy są używane zamiennie. preferowane jest jednak otwarty interfejs API.</span><span class="sxs-lookup"><span data-stu-id="43ca1-115">Both names are used interchangeably; however, Open API is preferred.</span></span> <span data-ttu-id="43ca1-116">Umożliwia on zarówno komputerów, jak i ludzi, aby zapoznać się z funkcjami usługi bez żadnych bezpośredni dostęp do wykonania (kod źródłowy, dostępu do sieci, dokumentacji).</span><span class="sxs-lookup"><span data-stu-id="43ca1-116">It allows both computers and humans to understand the capabilities of a service without any direct access to the implementation (source code, network access, documentation).</span></span> <span data-ttu-id="43ca1-117">Jeden celem jest aby zminimalizować ilość pracy wymagane do połączenia usługi już.</span><span class="sxs-lookup"><span data-stu-id="43ca1-117">One goal is to minimize the amount of work needed to connect disassociated services.</span></span> <span data-ttu-id="43ca1-118">Innym celem jest aby skrócić czas potrzebny na dokładnie dokumentu usługi.</span><span class="sxs-lookup"><span data-stu-id="43ca1-118">Another goal is to reduce the amount of time needed to accurately document a service.</span></span>

## <a name="swagger-specification-swaggerjson"></a><span data-ttu-id="43ca1-119">Specyfikacja swagger (swagger.json)</span><span class="sxs-lookup"><span data-stu-id="43ca1-119">Swagger specification (swagger.json)</span></span>

<span data-ttu-id="43ca1-120">Podstawowe z przepływem struktury Swagger jest specyfikacją Swagger&mdash;domyślnie dokumentu o nazwie *swagger.json*.</span><span class="sxs-lookup"><span data-stu-id="43ca1-120">The core to the Swagger flow is the Swagger specification&mdash;by default, a document named *swagger.json*.</span></span> <span data-ttu-id="43ca1-121">Jest ona generowana przez Swagger narzędzie łańcucha (lub innych firm implementacje go) oparte na usłudze.</span><span class="sxs-lookup"><span data-stu-id="43ca1-121">It's generated by the Swagger tool chain (or third-party implementations of it) based on your service.</span></span> <span data-ttu-id="43ca1-122">Opisuje funkcje interfejsu API i jak do niej dostęp przy użyciu protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="43ca1-122">It describes the capabilities of your API and how to access it with HTTP.</span></span> <span data-ttu-id="43ca1-123">Dyski interfejsu użytkownika programu Swagger, a jest używany przez łańcucha narzędzi, aby umożliwić generowanie kodu klienta wykrywania i.</span><span class="sxs-lookup"><span data-stu-id="43ca1-123">It drives the Swagger UI and is used by the tool chain to enable discovery and client code generation.</span></span> <span data-ttu-id="43ca1-124">Oto przykład specyfikacji Swagger zmniejszona skrócenia:</span><span class="sxs-lookup"><span data-stu-id="43ca1-124">Here's an example of a Swagger specification, reduced for brevity:</span></span>

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

## <a name="swagger-ui"></a><span data-ttu-id="43ca1-125">Swagger UI</span><span class="sxs-lookup"><span data-stu-id="43ca1-125">Swagger UI</span></span>

<span data-ttu-id="43ca1-126">[Interfejs użytkownika struktury swagger](https://swagger.io/swagger-ui/) oferuje opartych na sieci web interfejsu użytkownika, który zawiera informacje dotyczące usług, przy użyciu specyfikacji wygenerowanego struktury Swagger.</span><span class="sxs-lookup"><span data-stu-id="43ca1-126">[Swagger UI](https://swagger.io/swagger-ui/) offers a web-based UI that provides information about the service, using the generated Swagger specification.</span></span> <span data-ttu-id="43ca1-127">Zarówno Swashbuckle, jak i NSwag zawierają osadzone wersję interfejsu użytkownika programu Swagger, dzięki czemu mogą być hostowane w aplikacji platformy ASP.NET Core za pomocą oprogramowania pośredniczącego wywołanie rejestracji.</span><span class="sxs-lookup"><span data-stu-id="43ca1-127">Both Swashbuckle and NSwag include an embedded version of Swagger UI, so that it can be hosted in your ASP.NET Core app using a middleware registration call.</span></span> <span data-ttu-id="43ca1-128">Witryna sieci web UI wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="43ca1-128">The web UI looks like this:</span></span>

![Swagger UI](web-api-help-pages-using-swagger/_static/swagger-ui.png)

<span data-ttu-id="43ca1-130">Każda metoda akcji publicznego w kontrolerach można przetestować w interfejsie użytkownika.</span><span class="sxs-lookup"><span data-stu-id="43ca1-130">Each public action method in your controllers can be tested from the UI.</span></span> <span data-ttu-id="43ca1-131">Kliknij nazwę metody, aby rozwinąć sekcję.</span><span class="sxs-lookup"><span data-stu-id="43ca1-131">Click a method name to expand the section.</span></span> <span data-ttu-id="43ca1-132">Dodaj wszystkie niezbędne parametry, a następnie kliknij przycisk **Wypróbuj ją!**.</span><span class="sxs-lookup"><span data-stu-id="43ca1-132">Add any necessary parameters, and click **Try it out!**.</span></span>

![Przykład struktury Swagger UZYSKAĆ testu](web-api-help-pages-using-swagger/_static/get-try-it-out.png)

> [!NOTE]
> <span data-ttu-id="43ca1-134">Wersja programu Swagger interfejsu użytkownika używana do zrzuty ekranu jest w wersji 2.</span><span class="sxs-lookup"><span data-stu-id="43ca1-134">The Swagger UI version used for the screenshots is version 2.</span></span> <span data-ttu-id="43ca1-135">Na przykład w wersji 3, zobacz [przykład Petstore](http://petstore.swagger.io/).</span><span class="sxs-lookup"><span data-stu-id="43ca1-135">For a version 3 example, see [Petstore example](http://petstore.swagger.io/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="43ca1-136">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="43ca1-136">Next steps</span></span>

* [<span data-ttu-id="43ca1-137">Rozpoczynanie pracy z Swashbuckle</span><span class="sxs-lookup"><span data-stu-id="43ca1-137">Get started with Swashbuckle</span></span>](xref:tutorials/get-started-with-swashbuckle)
* [<span data-ttu-id="43ca1-138">Rozpoczynanie pracy z NSwag</span><span class="sxs-lookup"><span data-stu-id="43ca1-138">Get started with NSwag</span></span>](xref:tutorials/get-started-with-nswag)
