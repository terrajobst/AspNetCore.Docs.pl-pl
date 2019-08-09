---
title: Sprawdzanie metod Details i DELETE aplikacji ASP.NET Core
author: rick-anderson
description: Dowiedz się więcej na temat metody i widoku szczegółów kontrolera w podstawowej aplikacji ASP.NET Core MVC.
ms.author: riande
ms.date: 12/13/2018
uid: tutorials/first-mvc-app/details
ms.openlocfilehash: d19e8cdb63da2bb9c66db1943dfcec183d432401
ms.sourcegitcommit: 2719c70cd15a430479ab4007ff3e197fbf5dfee0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/09/2019
ms.locfileid: "68862973"
---
# <a name="examine-the-details-and-delete-methods-of-an-aspnet-core-app"></a>Sprawdzanie metod Details i DELETE aplikacji ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

Otwórz kontroler filmu i Przeanalizuj `Details` metodę:

[!code-csharp[](start-mvc/sample/MvcMovie22/Controllers/MoviesController.cs?name=snippet_details)]

Aparat tworzenia szkieletu MVC, który utworzył tę metodę akcji, dodaje komentarz zawierający żądanie HTTP, które wywołuje metodę. W tym przypadku jest to żądanie Get z trzema segmentami adresów URL `Movies` , kontrolerem `Details` , metodą i `id` wartością. Wycofaj te segmenty są zdefiniowane w *Startup.cs*.

[!code-csharp[](start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

EF ułatwia wyszukiwanie danych przy użyciu `FirstOrDefaultAsync` metody. Ważna funkcja zabezpieczeń wbudowana w metodę polega na tym, że kod sprawdza, czy metoda wyszukiwania znalazła film przed podjęciem próby wykonania jakichkolwiek czynności. Na przykład haker może wprowadzić błędy do witryny przez zmianę adresu URL utworzonego przez linki z `http://localhost:{PORT}/Movies/Details/1` do czegoś takiego jak `http://localhost:{PORT}/Movies/Details/12345` (lub innej wartości, która nie reprezentuje rzeczywistego filmu). Jeśli nie zaznaczono filmu o wartości null, aplikacja zgłosi wyjątek.

Przejrzyj metody `DeleteConfirmed`i. `Delete`

[!code-csharp[](start-mvc/sample/MvcMovie22/Controllers/MoviesController.cs?name=snippet_delete)]

Należy zauważyć, `HTTP GET Delete` że metoda nie usuwa określonego filmu, zwraca widok filmu, w którym można przesłać (HTTPPOST) usunięcie. Wykonanie operacji usuwania w odpowiedzi na żądanie GET (lub w tym przypadku wykonanie operacji edycji, operacji tworzenia lub jakiejkolwiek innej operacji, która zmienia dane) powoduje otwarcie otworu zabezpieczeń.

Metoda, która usuwa dane, ma nazwę `DeleteConfirmed` , aby nadać metodzie post protokołu HTTP unikatowy podpis lub nazwę. `[HttpPost]` Poniżej przedstawiono dwie sygnatury metod:

[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete2)]

[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete3)]

Środowisko uruchomieniowe języka wspólnego (CLR) wymaga, aby przeciążone metody miały unikatowy podpis parametru (taka sama nazwa metody, ale inna lista parametrów). Jednak w tym miejscu wymagane są `Delete` dwie metody — jeden dla elementu get i jeden dla elementu post--oba mają taki sam podpis parametru. (Oba muszą akceptować jedną liczbę całkowitą jako parametr).

Istnieją dwa podejścia do tego problemu, jedną z nich jest nadanie metodom różnych nazw. To właśnie mechanizm tworzenia szkieletu w poprzednim przykładzie. W ten sposób wprowadzono jednak niewielki problem: ASP.NET mapuje segmenty adresu URL na metody akcji według nazwy, a jeśli zmienisz nazwę metody, routing zwykle nie będzie mógł znaleźć tej metody. To rozwiązanie jest widoczne w przykładzie, który polega na dodaniu `ActionName("Delete")` atrybutu `DeleteConfirmed` do metody. Ten atrybut wykonuje mapowanie dla systemu routingu w taki sposób, aby adres URL, który zawiera/Delete/dla żądania post, `DeleteConfirmed` znajdował metodę.

Inna częsta obejście dla metod, które mają identyczne nazwy i podpisy, polega na sztucznej zmianie sygnatury metody POST w celu uwzględnienia dodatkowego parametru (nieużywane). To właśnie zrobiono w poprzednim wpisie po dodaniu `notUsed` parametru. Tę samą czynność można wykonać w tym miejscu dla `[HttpPost] Delete` metody:

```csharp
// POST: Movies/Delete/6
[HttpPost]
[ValidateAntiForgeryToken]
public async Task<IActionResult> Delete(int id, bool notUsed)
```

### <a name="publish-to-azure"></a>Publikowanie na platformie Azure

Aby uzyskać informacje na temat wdrażania na platformie [Azure, zobacz Samouczek: Tworzenie aplikacji internetowej platformy .NET Core i SQL Database w Azure App Service](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb).

> [!div class="step-by-step"]
> [Poprzednie](validation.md)
