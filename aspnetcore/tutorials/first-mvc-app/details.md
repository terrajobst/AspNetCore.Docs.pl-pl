---
title: Sprawdź szczegóły i usunięcie metod aplikacji ASP.NET Core
author: rick-anderson
description: Dowiedz się więcej o szczegóły metody kontrolera i wyświetlanie w podstawowej aplikacji ASP.NET Core MVC.
ms.author: riande
ms.date: 12/13/2018
uid: tutorials/first-mvc-app/details
ms.openlocfilehash: f674ca1761f85ce127121603286c97d5936f6716
ms.sourcegitcommit: c47d7c131eebbcd8811e31edda210d64cf4b9d6b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55236331"
---
# <a name="examine-the-details-and-delete-methods-of-an-aspnet-core-app"></a>Sprawdź szczegóły i usunięcie metod aplikacji ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

Otwórz kontrolera film i zbadaj `Details` metody:

[!code-csharp[](start-mvc/sample/MvcMovie22/Controllers/MoviesController.cs?name=snippet_details)]

Aparat tworzenia szkieletów MVC, utworzony z tą metodą akcji dodaje komentarz przedstawiający żądanie HTTP, który wywołuje tę metodę. W tym przypadku jest żądanie GET z trzech segmenty adresu URL, `Movies` kontrolera, `Details` metody i `id` wartość. Te segmenty są definiowane w odwołania *Startup.cs*.

[!code-csharp[](start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

EF ułatwia wyszukiwanie danych przy użyciu `FirstOrDefaultAsync` metody. Ważna funkcja zabezpieczeń wbudowanych w metodzie jest kod sprawdza, ta metoda wyszukiwania wykryła filmu przed ponowną próbą podejmować żadnych działań z nim. Na przykład haker może spowodować błędy do witryny, zmieniając adres URL utworzony przez łącza z `http://localhost:xxxx/Movies/Details/1` na wartość podobną `http://localhost:xxxx/Movies/Details/12345` (lub inną wartość, która nie zawiera rzeczywistych filmu). Jeśli zaznaczono film o wartości null aplikacji będzie zgłaszają wyjątek.

Sprawdź `Delete` i `DeleteConfirmed` metody.

[!code-csharp[](start-mvc/sample/MvcMovie22/Controllers/MoviesController.cs?name=snippet_delete)]

Należy pamiętać, że `HTTP GET Delete` metoda nie powoduje usunięcia określonego filmu, zwraca widok filmu dokąd wysyłać (HttpPost) usuwania. Wykonywanie operacji usuwania w odpowiedzi na polecenie GET żądania (lub służącego wykonywania operacji Edytuj, Utwórz operacji lub innej operacji, które zmieniają dane) otwiera lukę w zabezpieczeniach.

`[HttpPost]` Nosi nazwę metody, która powoduje usunięcie danych `DeleteConfirmed` zapewnienie metodą HTTP POST unikatowy podpis lub nazwy. Poniżej przedstawiono podpisy dwóch metod:

[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete2)]

[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete3)]

Środowisko uruchomieniowe języka wspólnego (CLR) wymaga przeciążonej metody ma unikatowy parametr podpisu (tej samej nazwie metoda, ale inną listę parametrów). Jednak w tym miejscu należy dwa `Delete` metody — jeden dla GET--i jeden dla wpisu, czy obie pozycje mają taki sam podpis parametru. (Obaj użytkownicy muszą zaakceptować pojedyncze liczby całkowite jako parametr.)

Dostępne są dwie opcje tego problemu, jednym jest nadać różne nazwy metody. To mechanizm tworzenia szkieletów została w poprzednim przykładzie. Jednak wprowadza mały problem: ASP.NET mapuje segmentów adresu URL do metody akcji według nazwy, a jeśli zmienisz metodę, routing zwykle nie można znaleźć tej metody. Rozwiązanie jest widoczny w tym przykładzie jest dodanie `ActionName("Delete")` atrybutu `DeleteConfirmed` metody. Ten atrybut wykonuje mapowanie systemu routingu, aby znaleźć adres URL, który zawiera /Delete/ dla żądania POST `DeleteConfirmed` metody.

Inny wspólnej obejście dla metod, które mają identyczne nazwy i wzory podpisów jest sztucznie Zmień podpis metody POST w celu uwzględnienia dodatkowych parametrów (nieużywane). To, co zrobiliśmy w poprzednim wpisie podczas dodaliśmy `notUsed` parametru. Możesz to zrobić to samo w tym miejscu dla `[HttpPost] Delete` metody:

```csharp
// POST: Movies/Delete/6
[ValidateAntiForgeryToken]
public async Task<IActionResult> Delete(int id, bool notUsed)
```

### <a name="publish-to-azure"></a>Publikowanie na platformie Azure

Aby uzyskać informacje na temat wdrażania na platformie Azure, zobacz [samouczka: Tworzenie aplikacji internetowej platformy .NET Core i SQL Database w usłudze Azure App Service](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb).

> [!div class="step-by-step"]
> [Poprzednie](validation.md)
