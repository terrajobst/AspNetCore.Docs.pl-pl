---
title: Dojście do żądań z kontrolerami platformy ASP.NET Core MVC
author: ardalis
description: ''
ms.author: riande
ms.date: 07/03/2017
uid: mvc/controllers/actions
ms.openlocfilehash: 952e4dbb2c4343ca87ace1535e4a5968faf088cf
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64902485"
---
# <a name="handle-requests-with-controllers-in-aspnet-core-mvc"></a>Dojście do żądań z kontrolerami platformy ASP.NET Core MVC

Przez [Steve Smith](https://ardalis.com/) i [Scott Addie](https://github.com/scottaddie)

Kontrolerów, akcji i wyników akcji są integralną częścią jak deweloperom tworzyć aplikacje przy użyciu platformy ASP.NET Core MVC.

## <a name="what-is-a-controller"></a>Co to jest kontrolerem?

Kontroler jest używany do definiowania i grupy działań. Akcja (lub *metody akcji*) jest metodą na kontrolerze, który obsługuje żądania. Kontrolery logicznie pogrupować podobne działania. Ta agregacja akcje umożliwia wspólne zestawów reguł, takich jak routing, buforowanie i autoryzacji, mają być stosowane zbiorczo. Żądanie jest mapowane na operacje za pomocą [routingu](xref:mvc/controllers/routing).

Zgodnie z Konwencją klas kontrolera:
* Znajdują się w katalogu głównego projektu na poziomie *kontrolerów* folderu
* Dziedzicz `Microsoft.AspNetCore.Mvc.Controller`

Kontroler jest tworzone jako wystąpienia klasy, co najmniej jeden z następujących warunków jest spełniony:
* Nazwa klasy jest sufiks "Controller"
* Klasa dziedziczy z klasy, których nazwa jest sufiks "Controller"
* Klasa zostanie nadany `[Controller]` atrybutu

Klasa kontrolera nie może mieć skojarzoną `[NonController]` atrybutu.

Należy przestrzegać kontrolerów [jawne zależności zasady](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies). Istnieje kilka sposobów implementowania tej zasady. Jeśli wiele akcji kontrolera wymagają tej samej usługi, należy wziąć pod uwagę przy użyciu [iniekcji konstruktora](xref:mvc/controllers/dependency-injection#constructor-injection) do żądania tych zależności. Jeśli usługa jest wymagana przez metodę jedną akcję, należy rozważyć użycie [iniekcji akcji](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices) do żądania z zależności.

W ramach **M**odelu -**V**idok -**C**wzorzec ontroller kontrolera jest odpowiedzialny za początkowe przetwarzania żądania i wystąpienia modelu. Ogólnie rzecz biorąc decyzje biznesowe powinny być wykonywane w ramach modelu.

Kontroler pobiera wynik modelu przetwarzania (jeśli istnieje) i zwraca odpowiedniego widoku i jego skojarzonego widoku danych lub wynik wywołania interfejsu API. Dowiedz się więcej o [omówienie platformy ASP.NET Core MVC](xref:mvc/overview) i [wprowadzenie do ASP.NET Core MVC i programu Visual Studio](xref:tutorials/first-mvc-app/start-mvc).

Jest to kontroler *interfejsu użytkownika na poziomie* abstrakcji. Jego obowiązki są aby upewnij się, dane żądania są prawidłowe i wybrać, które widoku (lub wyniku dla interfejsu API) ma zostać zwrócony. W aplikacjach dobrze uwarunkowaną bezpośrednio nie zawiera danych programu access lub logikę biznesową. Zamiast tego kontroler deleguje do usług obsługi obowiązków.

## <a name="defining-actions"></a>Definiowanie akcji

Metody publiczne na kontrolerze, z wyjątkiem ozdobione `[NonAction]` atrybutu, akcji. Parametry na działania są powiązane dane żądania i są weryfikowane przy użyciu [wiązanie modelu](xref:mvc/models/model-binding). Sprawdzanie poprawności modelu występuje wszystko, co jest powiązany z modelu. `ModelState.IsValid` Wartość właściwości wskazuje na to, czy wiązania modelu i sprawdzanie poprawności zakończyło się pomyślnie.

Metody akcji powinna zawierać logikę do zamapowania na żądanie na znaczenie biznesowe. Potencjalne problemy biznesowe, zazwyczaj powinna być reprezentowana jako usługi, do których kontroler, który uzyskuje dostęp za pośrednictwem [wstrzykiwanie zależności](xref:mvc/controllers/dependency-injection). Akcje są mapowane następnie wynik monitorowanej akcji biznesowych do stanu aplikacji.

Akcje zwraca niczego, ale często zwrócenia wystąpienia `IActionResult` (lub `Task<IActionResult>` do metod asynchronicznych) daje odpowiedzi. Metoda akcji jest odpowiedzialny za wybranie *jakiego rodzaju odpowiedzi*. Wynik akcji *jest odpowiada*.

### <a name="controller-helper-methods"></a>Metody pomocnicze kontrolera

Dziedzicz zwykle kontrolery [kontrolera](/dotnet/api/microsoft.aspnetcore.mvc.controller), chociaż nie jest to wymagane. Wyprowadzanie z `Controller` zapewnia dostęp do metody pomocnika trzy kategorie:

#### <a name="1-methods-resulting-in-an-empty-response-body"></a>1. Metody skutkuje pusta treść odpowiedzi

Nie `Content-Type` nagłówka odpowiedzi HTTP jest uwzględniona, ponieważ treść odpowiedzi nie ma zawartości, aby opisać.

Istnieją dwa typy wyników w ramach tej kategorii: Przekierowania i kod stanu HTTP.

* **Kod stanu HTTP**

    Ten typ zwracany jest kod stanu HTTP. Kilka metody pomocnika tego typu są `BadRequest`, `NotFound`, i `Ok`. Na przykład `return BadRequest();` generuje kod stanu 400, podczas wykonywania. Gdy metody takie jak `BadRequest`, `NotFound`, i `Ok` są przeciążone, już nie kwalifikują się jako obiektów odpowiadających kod stanu HTTP, ponieważ negocjacje zawartości odbywa się.

* **Przekierowanie**

    Ten typ zwraca przekierowania do akcji lub docelowym (przy użyciu `Redirect`, `LocalRedirect`, `RedirectToAction`, lub `RedirectToRoute`). Na przykład `return RedirectToAction("Complete", new {id = 123});` przekierowuje do `Complete`, przekazując obiektu anonimowego.

    Typ wyniku przekierowania, który różni się od typu kod stanu HTTP, przede wszystkim podczas dodawania `Location` nagłówka odpowiedzi HTTP.

#### <a name="2-methods-resulting-in-a-non-empty-response-body-with-a-predefined-content-type"></a>2. Metody skutkuje treści odpowiedzi pusty wstępnie zdefiniowanego typu zawartości

Większość metod pomocnika z tej kategorii należą `ContentType` właściwości, co pozwala ustawić `Content-Type` nagłówek odpowiedzi, aby opisać treść odpowiedzi.

Istnieją dwa typy wyników w ramach tej kategorii: [Widok](xref:mvc/views/overview) i [sformatowane odpowiedzi](xref:web-api/advanced/formatting).

* **Widok**

    Ten typ zwraca widok, który korzysta z modelu do renderowania elementów HTML. Na przykład `return View(customer);` przekazuje modelu do widoku dla powiązania danych.

* **Sformatowana odpowiedzi**

    Ten typ zwraca JSON lub podobny format wymiany danych do reprezentowania obiektu w szczególny sposób. Na przykład `return Json(customer);` serializuje podany obiekt do formatu JSON.
    
    Inne typowe metody tego typu to `File` i `PhysicalFile`. Na przykład `return PhysicalFile(customerFilePath, "text/xml");` zwraca [PhysicalFileResult](/dotnet/api/microsoft.aspnetcore.mvc.physicalfileresult).

#### <a name="3-methods-resulting-in-a-non-empty-response-body-formatted-in-a-content-type-negotiated-with-the-client"></a>3. Metody skutkuje treści odpowiedzi pusty sformatowane typu zawartości negocjowane za pomocą klienta programu

Ta kategoria jest lepiej znany jako **negocjacje zawartości**. [Negocjowanie zawartości](xref:web-api/advanced/formatting#content-negotiation) ma zastosowanie przy każdym zwraca akcję [ObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.objectresult) typu albo coś innego niż [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult) implementacji. Akcja, która zwraca innej niż`IActionResult` implementacji (na przykład `object`) również zwraca odpowiedź sformatowany.

Niektóre metody pomocnika tego typu obejmują `BadRequest`, `CreatedAtRoute`, i `Ok`. Przykłady te metody `return BadRequest(modelState);`, `return CreatedAtRoute("routename", values, newobject);`, i `return Ok(value);`, odpowiednio. Należy pamiętać, że `BadRequest` i `Ok` przeprowadzania negocjacji zawartości tylko wtedy, gdy przekazana wartość; bez przekazywana wartość, zamiast tego służą jako typy wyników kod stanu HTTP. `CreatedAtRoute` Metody, z drugiej strony, zawsze przeprowadza negocjowanie zawartości od momentu jej przeciążeń wszystkie wymagają przekazania wartości.

### <a name="cross-cutting-concerns"></a>Cross-Cutting Concerns

Aplikacje zazwyczaj mają części przepływu pracy. Przykłady obejmują aplikację, która wymaga uwierzytelnienia dostępu do koszyka zakupów lub aplikację, która przechowuje dane na kilka stron. Aby wykonać logikę przed lub po metody akcji, użyj *filtru*. Za pomocą [filtry](xref:mvc/controllers/filters) odciąż przekrojowe zagadnienia może zmniejszyć dublowania.

Najbardziej filtrowanie atrybutów, takich jak `[Authorize]`, można zastosować na poziomie kontroler lub akcję, zależnie od żądanego poziomu szczegółowości.

Obsługa błędów i buforowanie odpowiedzi są często odciąż przekrojowe zagadnienia:
* [Obsługa błędów](xref:mvc/controllers/filters#exception-filters)
* [Buforowanie odpowiedzi](xref:performance/caching/response)

Wiele odciąż przekrojowe zagadnienia można obsługiwać przy użyciu filtrów lub niestandardowe [oprogramowania pośredniczącego](xref:fundamentals/middleware/index).
