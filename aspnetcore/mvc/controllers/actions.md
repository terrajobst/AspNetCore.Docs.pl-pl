---
title: Dojście żądań z kontrolerami w programie ASP.NET MVC Core
author: ardalis
description: ''
ms.author: riande
ms.date: 07/03/2017
uid: mvc/controllers/actions
ms.openlocfilehash: 3f3f565021d484b69401a3e03a2a966c92764a49
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36275663"
---
# <a name="handle-requests-with-controllers-in-aspnet-core-mvc"></a>Dojście żądań z kontrolerami w programie ASP.NET MVC Core

Przez [Steve Smith](https://ardalis.com/) i [Scott Addie](https://github.com/scottaddie)

Kontrolery, akcje i wyniki akcji są integralną częścią jak deweloperom tworzenie aplikacji korzystających z platformy ASP.NET Core MVC.

## <a name="what-is-a-controller"></a>Co to jest kontrolerem?

Kontroler służy do definiowania i grupy działań. Akcja (lub *metody akcji*) jest metodą na kontrolerze, który obsługuje żądania. Kontrolery logicznie grupują podobnych działań. Ta agregacja akcje umożliwia wspólne zestawy reguł, takich jak routing, buforowanie i autoryzacji, mają być stosowane razem. Żądanie jest mapowane na akcji za pomocą [routingu](xref:mvc/controllers/routing).

Według Konwencji klasy kontrolera:
* Znajdują się w katalogu głównego projektu na poziomie *kontrolerów* folderu
* Dziedzicz `Microsoft.AspNetCore.Mvc.Controller`

Kontroler jest tworzone jako wystąpienia klasy co najmniej jeden z następujących warunków jest spełniony:
* Nazwa klasy jest kończyły się słowem "Controller"
* Klasa dziedziczy z klasy o nazwie kończyły się słowem "Controller"
* Klasa zostanie nadany `[Controller]` atrybutu

Klasa kontrolera nie może mieć skojarzone `[NonController]` atrybutu.

Należy stosować kontrolerów [jawne zależności zasady](http://deviq.com/explicit-dependencies-principle/). Istnieją dwa podejścia do wdrażania tej zasady. Jeśli wiele akcji kontrolera wymagają tej samej usługi, rozważ użycie [iniekcji konstruktora](xref:mvc/controllers/dependency-injection#constructor-injection) żądania tych zależności. Jeśli usługa jest wymagana przez metodę jednej akcji, należy rozważyć użycie [iniekcji akcji](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices) żądania zależności.

W ramach **M**odelu -**V**rzeglądaj -**C**wzorzec ontroller kontrolera jest odpowiedzialny za początkowego przetwarzania żądania i wystąpienia modelu. Ogólnie rzecz biorąc decyzje biznesowe powinny być wykonywane w ramach modelu.

Kontroler ma wynik przetwarzania modelu (jeśli istnieje) i zwraca prawidłowego widoku i jego skojarzony widok danych lub wynik wywołania interfejsu API. Dowiedz się więcej o [Przegląd platformy ASP.NET Core MVC](xref:mvc/overview) i [wprowadzenie do platformy ASP.NET Core MVC i Visual Studio](xref:tutorials/first-mvc-app/start-mvc).

Jest to kontroler *interfejsu użytkownika na poziomie* abstrakcji. Jego obowiązki jest zapewnienie dane żądania są prawidłowe oraz wybrać, które widoku (lub wynik dla interfejsu API) ma zostać zwrócony. W aplikacjach dobrze factored bezpośrednio nie zawiera danych dostępu lub logikę biznesową. Zamiast tego kontrolera deleguje do obsługi obowiązki tych usług.

## <a name="defining-actions"></a>Definiowanie akcji

Metody publiczne na kontrolerze, z wyjątkiem ozdobione `[NonAction]` atrybutu, akcje. Parametry akcjami powiązanych do żądania danych i są weryfikowane przy użyciu [modelu powiązania](xref:mvc/models/model-binding). Weryfikacja modelu występuje wszystko, co jest powiązane z modelu. `ModelState.IsValid` Wartość właściwości wskazuje, czy wiązanie modelu i sprawdzanie poprawności zakończyło się pomyślnie.

Metody akcji powinna zawierać logikę do mapowania żądania znaczenie biznesowe. Problemy biznesowe zwykle powinny być reprezentowane jako usługi, które kontrolera uzyskuje dostęp do za pośrednictwem [iniekcji zależności](xref:mvc/controllers/dependency-injection). Akcje są mapowane następnie wynik akcji biznesowych do stanu aplikacji.

Akcje zwraca niczego, ale często zwrócić wystąpienia `IActionResult` (lub `Task<IActionResult>` dla metod asynchronicznych) daje odpowiedzi. Metoda akcji jest odpowiedzialny za wybranie *jakiego rodzaju odpowiedzi*. Wynik akcji *jest odpowiada*.

### <a name="controller-helper-methods"></a>Metody pomocnicze kontrolera

Dziedzicz zwykle kontrolery [kontrolera](/dotnet/api/microsoft.aspnetcore.mvc.controller), chociaż nie jest to wymagane. Wyprowadzanie z `Controller` zapewnia dostęp do metody pomocnicze trzy kategorie:

#### <a name="1-methods-resulting-in-an-empty-response-body"></a>1. Metod co w treści odpowiedzi pusty

Nie `Content-Type` nagłówka odpowiedzi HTTP jest uwzględnione, ponieważ treść odpowiedzi nie ma zawartości do opisu.

Istnieją dwa typy wyników w ramach tej kategorii: Przekierowanie i kod stanu HTTP.

* **Kod stanu HTTP**

    Ten typ zwraca kod stanu HTTP. Kilka metody pomocnicze tego typu są `BadRequest`, `NotFound`, i `Ok`. Na przykład `return BadRequest();` generuje kod stanu 400 podczas wykonywania. Gdy metod, takich jak `BadRequest`, `NotFound`, i `Ok` są przeciążone, już nie kwalifikują się jako obiekty kod stanu HTTP odpowiadające, ponieważ negocjacje zawartości ma miejsce.

* **przekierowania**

    Ten typ zwraca przekierowanie do akcji lub docelowym (przy użyciu `Redirect`, `LocalRedirect`, `RedirectToAction`, lub `RedirectToRoute`). Na przykład `return RedirectToAction("Complete", new {id = 123});` przekierowuje do `Complete`, przekazywanie obiektu anonimowego.

    Typ wyniku przekierowania różni się od typu kod stanu HTTP głównie w przypadku dodawania `Location` nagłówka odpowiedzi HTTP.

#### <a name="2-methods-resulting-in-a-non-empty-response-body-with-a-predefined-content-type"></a>2. Metod co w treści odpowiedzi pusty z wstępnie zdefiniowany typ zawartości

Większość metody pomocnika w tej kategorii obejmują `ContentType` właściwości, dzięki czemu można ustawić `Content-Type` nagłówek odpowiedzi do opisywania treść odpowiedzi.

Istnieją dwa typy wyników w ramach tej kategorii: [widoku](xref:mvc/views/overview) i [sformatowany odpowiedzi](xref:web-api/advanced/formatting).

* **Widok**

    Ten typ zwraca widoku, który korzysta z modelu do renderowania elementów HTML. Na przykład `return View(customer);` przekazuje model widoku dla powiązania danych.

* **Sformatowany odpowiedzi**

    Ten typ zwraca JSON lub podobny format wymiany danych, do reprezentowania obiektu w określonym czasie. Na przykład `return Json(customer);` serializuje podany obiekt do formatu JSON.
    
    Inne typowe metody tego typu to `File`, `PhysicalFile`, i `VirtualFile`. Na przykład `return PhysicalFile(customerFilePath, "text/xml");` zwraca opisanego przez plik XML `Content-Type` wartości nagłówka odpowiedzi "text/xml".

#### <a name="3-methods-resulting-in-a-non-empty-response-body-formatted-in-a-content-type-negotiated-with-the-client"></a>3. Metod co w treści odpowiedzi pusty sformatowane typu zawartości negocjowane z klienta

Ta kategoria jest lepiej znane jako **negocjacje zawartości**. [Negocjowanie zawartości](xref:web-api/advanced/formatting#content-negotiation) ma zastosowanie, gdy akcja zwraca [ObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.objectresult) typu lub coś innego niż [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult) implementacji. Akcja, która zwraca niż`IActionResult` implementacji (na przykład `object`) również zwraca odpowiedź sformatowany.

Niektóre metody pomocnika tego typu obejmują `BadRequest`, `CreatedAtRoute`, i `Ok`. Przykłady te metody `return BadRequest(modelState);`, `return CreatedAtRoute("routename", values, newobject);`, i `return Ok(value);`odpowiednio. Należy pamiętać, że `BadRequest` i `Ok` przeprowadzania negocjacji zawartości tylko wtedy, gdy przekazana wartość; nie przekazano wartość, zamiast tego służą jako typy wyników kod stanu HTTP. `CreatedAtRoute` Metody z drugiej strony, zawsze przeprowadza negocjacje zawartości od momentu jego przeciążenia wszystkie wymagają, aby otrzymać wartość.

### <a name="cross-cutting-concerns"></a>Dotyczy kompleksowymi

Aplikacje zazwyczaj współużytkować części przepływu pracy. Przykładami aplikacji, która wymaga uwierzytelnienia do koszyka lub aplikację, która przechowuje dane na kilka stron. Aby wykonać logiki przed lub po metody akcji, należy użyć *filtru*. Przy użyciu [filtry](xref:mvc/controllers/filters) dotyczy kompleksowymi można zmniejszyć duplikatów, umożliwiając wykonaj [zasady nie powtarzaj samodzielnie (BIBUŁĄ)](http://deviq.com/don-t-repeat-yourself/).

Najbardziej filtrowanie atrybutów, takich jak `[Authorize]`, można zastosować na poziomie kontrolera lub akcji, w zależności od żądany poziom szczegółowości.

Obsługa błędów i buforowanie odpowiedzi są często dotyczy kompleksowymi:
   * [Obsługa błędów](xref:mvc/controllers/filters#exception-filters)
   * [Buforowanie odpowiedzi](xref:performance/caching/response)

Wiele problemów kompleksowymi można obsługiwać przy użyciu filtrów lub niestandardowe [oprogramowanie pośredniczące](xref:fundamentals/middleware/index).
