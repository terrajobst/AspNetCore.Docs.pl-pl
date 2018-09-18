---
title: Sprawdź szczegóły i usunięcie metod aplikacji ASP.NET Core
author: rick-anderson
description: Dowiedz się więcej o szczegóły metody kontrolera i wyświetlanie w podstawowej aplikacji ASP.NET Core MVC.
ms.author: riande
ms.date: 03/07/2017
uid: tutorials/first-mvc-app/details
ms.openlocfilehash: c3fb7ae9507d13f3b8c7d366e333151dc7a7a6d3
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011439"
---
# <a name="examine-the-details-and-delete-methods-of-an-aspnet-core-app"></a>Sprawdź szczegóły i usunięcie metod aplikacji ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

Otwórz kontrolera film i zbadaj `Details` metody:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](start-mvc/sample/MvcMovie21/Controllers/MoviesController.cs?name=snippet_details)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_details)]

::: moniker-end

Aparat tworzenia szkieletów MVC, utworzony z tą metodą akcji dodaje komentarz przedstawiający żądanie HTTP, który wywołuje tę metodę. W tym przypadku jest żądanie GET z trzech segmenty adresu URL, `Movies` kontrolera, `Details` metody i `id` wartość. Te segmenty są definiowane w odwołania *Startup.cs*.

[!code-csharp[](start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

EF ułatwia wyszukiwanie danych przy użyciu `SingleOrDefaultAsync` metody. Ważna funkcja zabezpieczeń wbudowanych w metodzie jest kod sprawdza, ta metoda wyszukiwania wykryła filmu przed ponowną próbą podejmować żadnych działań z nim. Na przykład haker może spowodować błędy do witryny, zmieniając adres URL utworzony przez łącza z `http://localhost:xxxx/Movies/Details/1` na wartość podobną `http://localhost:xxxx/Movies/Details/12345` (lub inną wartość, która nie zawiera rzeczywistych filmu). Jeśli zaznaczono film o wartości null aplikacji będzie zgłaszają wyjątek.

Sprawdź `Delete` i `DeleteConfirmed` metody.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](start-mvc/sample/MvcMovie21/Controllers/MoviesController.cs?name=snippet_delete)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete)]

::: moniker-end

Należy pamiętać, że `HTTP GET Delete` metoda nie powoduje usunięcia określonego filmu, zwraca widok filmu dokąd wysyłać (HttpPost) usuwania. Wykonywanie operacji usuwania w odpowiedzi na polecenie GET żądania (lub służącego wykonywania operacji Edytuj, Utwórz operacji lub innej operacji, które zmieniają dane) otwiera lukę w zabezpieczeniach.

`[HttpPost]` Nosi nazwę metody, która powoduje usunięcie danych `DeleteConfirmed` zapewnienie metodą HTTP POST unikatowy podpis lub nazwy. Poniżej przedstawiono podpisy dwóch metod:

[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete2)]

[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete3)]


Środowisko uruchomieniowe języka wspólnego (CLR) wymaga przeciążonej metody ma unikatowy parametr podpisu (tej samej nazwie metoda, ale inną listę parametrów). Jednak w tym miejscu należy dwa `Delete` metody — jeden dla GET--i jeden dla wpisu, czy obie pozycje mają taki sam podpis parametru. (Obaj użytkownicy muszą zaakceptować pojedyncze liczby całkowite jako parametr.)

Dostępne są dwie opcje tego problemu, jednym jest nadać różne nazwy metody. To mechanizm tworzenia szkieletów została w poprzednim przykładzie. Jednak wprowadza mały problem: ASP.NET mapuje segmentów adresu URL do metody akcji według nazwy i metodę w przypadku zmiany nazwy, routing zwykle nie będzie mógł odnaleźć tej metody. Rozwiązanie jest widoczny w tym przykładzie jest dodanie `ActionName("Delete")` atrybutu `DeleteConfirmed` metody. Ten atrybut wykonuje mapowanie systemu routingu, aby znaleźć adres URL, który zawiera /Delete/ dla żądania POST `DeleteConfirmed` metody.

Inny wspólnej obejście dla metod, które mają identyczne nazwy i wzory podpisów jest sztucznie Zmień podpis metody POST w celu uwzględnienia dodatkowych parametrów (nieużywane). To, co zrobiliśmy w poprzednim wpisie podczas dodaliśmy `notUsed` parametru. Możesz to zrobić to samo w tym miejscu dla `[HttpPost] Delete` metody:

```csharp
// POST: Movies/Delete/6
[ValidateAntiForgeryToken]
public async Task<IActionResult> Delete(int id, bool notUsed)
```

### <a name="publish-to-azure"></a>Publikowanie na platformie Azure

Zobacz [publikowania aplikacji sieci web ASP.NET Core w usłudze Azure App Service przy użyciu programu Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) instrukcje dotyczące sposobu publikowania tej aplikacji na platformie Azure przy użyciu programu Visual Studio.  Aplikację można także publikować z [wiersza polecenia](xref:tutorials/publish-to-azure-webapp-using-cli).

> [!div class="step-by-step"]
> [Poprzednie](validation.md)
