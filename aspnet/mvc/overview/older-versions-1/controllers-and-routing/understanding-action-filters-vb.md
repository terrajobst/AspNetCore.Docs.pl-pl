---
uid: mvc/overview/older-versions-1/controllers-and-routing/understanding-action-filters-vb
title: "Opis akcji filtrów (VB) | Dokumentacja firmy Microsoft"
author: microsoft
description: "Celem tego samouczka jest wyjaśnienie filtrów akcji. Filtr akcji jest atrybut, który można zastosować do akcji kontrolera — lub całego kontrolera..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/16/2008
ms.topic: article
ms.assetid: e83812f2-c53e-4a43-a7c1-d64c59ecf694
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/understanding-action-filters-vb
msc.type: authoredcontent
ms.openlocfilehash: 483133ec5db27c2fa1ed4b463e37e17efab12e0f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="understanding-action-filters-vb"></a>Opis akcji filtrów (VB)
====================
przez [firmy Microsoft](https://github.com/microsoft)

[Pobierz plik PDF](http://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_14_VB.pdf)

> Celem tego samouczka jest wyjaśnienie filtrów akcji. Filtr akcji jest atrybut, który można zastosować do akcji kontrolera — lub całego kontrolera — które modyfikuje sposób, w którym akcja jest wykonywana.


## <a name="understanding-action-filters"></a>Opis filtry akcji

Celem tego samouczka jest wyjaśnienie filtrów akcji. Filtr akcji jest atrybut, który można zastosować do akcji kontrolera — lub całego kontrolera — które modyfikuje sposób, w którym akcja jest wykonywana. Platforma ASP.NET MVC zawiera kilka filtrów akcji:

- OutputCache — ten filtr akcji przechowuje w pamięci podręcznej danych wyjściowych akcji kontrolera dla określonego przedziału czasu.
- HandleError — ten filtr akcji obsługuje błędy wywoływane, gdy wykonuje akcji kontrolera.
- Autoryzowanie — ten filtr akcji pozwala ograniczyć dostęp do określonego użytkownika lub roli.

Można również tworzyć filtry akcji niestandardowej. Na przykład można utworzyć filtr akcji niestandardowych w celu wdrożenia systemu uwierzytelniania niestandardowego. Można też utworzyć filtr akcji, który modyfikuje widok danych zwróconych przez akcji kontrolera.

W tym samouczku Dowiedz się jak tworzenie filtr akcji od podstaw. Utworzymy filtru akcji dziennika, który rejestruje różne etapy przetwarzania akcji w oknie programu Visual Studio danych wyjściowych.

### <a name="using-an-action-filter"></a>Przy użyciu filtru akcji

Filtr akcji jest atrybutem. Większość filtrów akcji można zastosować do akcji kontrolera poszczególnych lub całego kontrolera.

Na przykład kontroler danych w 1 lista przedstawia akcji o nazwie `Index()` zwracającą bieżącego czasu. Ta akcja zostanie nadany `OutputCache` filtru akcji. Ten filtr powoduje, że wartość zwracana przez akcję pamięci podręcznej przez 10 sekund.

**1 — Lista`Controllers\DataController.vb`**

[!code-vb[Main](understanding-action-filters-vb/samples/sample1.vb)]

Jeśli wielokrotnie wywoływać `Index()` akcji, wprowadzając adres URL/danych/indeksem w pasku adresu przeglądarki i naciśnięcie klawisza odświeżania przycisk wiele razy, a następnie pojawi się jednocześnie na 10 sekund. Dane wyjściowe `Index()` akcji jest buforowana przez 10 sekund (zobacz rysunek 1).


[![Czas pamięci podręcznej](understanding-action-filters-vb/_static/image2.png)](understanding-action-filters-vb/_static/image1.png)

**Rysunek 01**: buforowane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-action-filters-vb/_static/image3.png))


Wyświetlanie 1 filtru jednej akcji — `OutputCache` filtr akcji — jest stosowany do `Index()` metody. Należy do tego samego działania można zastosować wiele filtrów akcji. Na przykład możesz chcieć zastosowanie zarówno `OutputCache` i `HandleError` filtry akcji do tego samego działania.

Wyświetlanie listy 1 `OutputCache` filtr akcji jest stosowany do `Index()` akcji. Można także zastosować dla tego atrybutu `DataController` samej klasy. W takim przypadku wynik zwracany przez działanie udostępnianych przez kontroler będzie buforowana przez 10 sekund.

### <a name="the-different-types-of-filters"></a>Różne typy filtrów

Platforma ASP.NET MVC obsługuje cztery różne typy filtrów:

1. Filtry autoryzacji — implementuje `IAuthorizationFilter` atrybutu.
2. Filtry działań — implementuje `IActionFilter` atrybutu.
3. Powoduje filtry — implementuje `IResultFilter` atrybutu.
4. Filtry wyjątków — implementuje `IExceptionFilter` atrybutu.

Filtry są uruchamiane w kolejności podanej powyżej. Na przykład filtry autoryzacji są zawsze wykonywane przed filtry akcji, a filtry wyjątków są zawsze wykonywane po każdego typu filtru.

Filtry autoryzacji są używane do implementowania uwierzytelniania i autoryzacji dla akcji kontrolera. Na przykład filtr autoryzacji jest przykładem filtr autoryzacji.

Filtry akcji zawiera logikę, która jest wykonywana przed i po wykonaniu akcji kontrolera. Filtr akcji można użyć na przykład, aby zmodyfikować wyświetlić dane, które zwraca akcji kontrolera.

Filtry wyników zawiera logikę, która jest wykonywana przed i po wykonaniu wyniku widoku. Na przykład można zmodyfikować wyniku widoku kliknij prawym przyciskiem myszy przed wyświetleniem widok w przeglądarce.

Filtry wyjątków są ostatniego typu filtru. Do obsługi błędów zgłaszane przez akcji kontrolera lub wyników akcji kontrolera, można użyć filtru wyjątków. Możesz również użyć filtry wyjątków do dziennika błędów.

Każdy typ filtru jest wykonywany w określonej kolejności. Jeśli chcesz kontrolować kolejność wykonywania filtrów tego samego typu można ustawić właściwości kolejności filtrów.

Klasa podstawowa dla wszystkich filtrów akcji jest `System.Web.Mvc.FilterAttribute` klasy. Chcąc do zaimplementowania konkretnego typu filtru, należy utworzyć klasę, która dziedziczy po klasie podstawowej filtru i implementuje co najmniej jeden IAuthorizationFilter, IActionFilter, IResultFilter lub ExceptionFilter interfejsy.

### <a name="the-base-actionfilterattribute-class"></a>Klasa podstawowa ActionFilterAttribute

Aby ułatwić implementuje filtr akcji niestandardowej, platforma ASP.NET MVC zawiera podstawowej `ActionFilterAttribute` klasy. Ta klasa implementuje zarówno `IActionFilter` i `IResultFilter` i interfejsy dziedziczy `Filter` klasy.

Terminologia w tym miejscu nie jest całkowicie zgodne. Z technicznego punktu widzenia klasy, która dziedziczy po klasie ActionFilterAttribute jest filtr akcji i filtrowania wyników. Jednak w tym sensie, utracić, filtr akcji programu word jest używana do odwoływania się do dowolnego typu filtru w platformę ASP.NET MVC.

Klasa podstawowa ActionFilterAttribute ma następujące metody, które można zastąpić:

- OnActionExecuting — ta metoda jest wywoływana przed wykonaniem akcji kontrolera.
- OnActionExecuted — ta metoda jest wywoływana po wykonaniu akcji kontrolera.
- OnResultExecuting — ta metoda jest wywoływana przed wykonaniem wyniku akcji kontrolera.
- OnResultExecuted — ta metoda jest wywoływana po wykonaniu wyniku akcji kontrolera.

W następnej sekcji przedstawiono będzie implementacji każdej z tych różnych metod.

### <a name="creating-a-log-action-filter"></a>Utworzenie filtru akcji dziennika

Aby zilustrować, jak można utworzyć filtr akcji niestandardowej, utworzymy filtr akcji niestandardowej, który rejestruje etapów przetwarzania akcji kontrolera, w oknie programu Visual Studio danych wyjściowych. Nasze `LogActionFilter` znajduje się lista 2.

**2 — Lista`ActionFilters\LogActionFilter.vb`**

[!code-vb[Main](understanding-action-filters-vb/samples/sample2.vb)]

Wyświetlanie 2 `OnActionExecuting()`, `OnActionExecuted()`, `OnResultExecuting()`, i `OnResultExecuted()` wywołania metody `Log()` metody. Nazwa metody i bieżące dane trasy są przekazywane do `Log()` metody. `Log()` Metody zapisuje komunikat w oknie programu Visual Studio danych wyjściowych (patrz rysunek 2).


[![Zapisywanie w oknie programu Visual Studio danych wyjściowych.](understanding-action-filters-vb/_static/image5.png)](understanding-action-filters-vb/_static/image4.png)

**Rysunek 02**: zapisywanie w oknie programu Visual Studio danych wyjściowych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-action-filters-vb/_static/image6.png))


Kontrolera głównej w wyświetlania 3 przedstawiono, jak filtr akcji dziennika można stosować do klasy całego kontrolera. Zawsze, gdy wszystkie akcje udostępnianych przez kontrolera głównej są wywoływane — albo `Index()` metody lub `About()` metodą — etapy przetwarzania akcji są rejestrowane w oknie programu Visual Studio danych wyjściowych.

**3 — lista`Controllers\HomeController.vb`**

[!code-vb[Main](understanding-action-filters-vb/samples/sample3.vb)]

### <a name="summary"></a>Podsumowanie

W tym samouczku zostały wprowadzone do platformy ASP.NET MVC filtry akcji. Przedstawiono cztery różne typy filtrów: filtry autoryzacji, filtry akcji, filtry wyników i filtry wyjątków. Przedstawiono również o podstawowym `ActionFilterAttribute` klasy.

Ponadto przedstawiono sposób wykonania prostego filtru akcji. Utworzyliśmy filtru akcji dziennika, który rejestruje etapów przetwarzania akcji kontrolera, w oknie programu Visual Studio danych wyjściowych.

>[!div class="step-by-step"]
[Poprzednie](asp-net-mvc-routing-overview-vb.md)
[dalej](improving-performance-with-output-caching-vb.md)
