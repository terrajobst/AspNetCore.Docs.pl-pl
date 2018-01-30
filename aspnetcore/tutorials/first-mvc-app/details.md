---
title: "Badanie szczegóły i metody zostaną usunięte"
author: rick-anderson
description: "Szczegóły metody kontrolera i widoku w Podstawowa aplikacja platformy ASP.NET Core MVC."
manager: wpickett
ms.author: riande
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/details
ms.openlocfilehash: 4a0004fc79f8e1d334e3acb96b28b2954d19f0a1
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2018
---
# <a name="examining-the-details-and-delete-methods"></a>Badanie szczegóły i metody zostaną usunięte

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

Otwórz kontrolera filmu i sprawdź, czy `Details` metody:

[!code-csharp[Main](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_details)]

Aparat szkieletów MVC utworzony tą metodą akcji dodaje komentarz przedstawiający żądanie HTTP, która wywołuje metodę. W tym przypadku jest to żądanie GET z trzech segmenty adresu URL, `Movies` kontrolera, `Details` — metoda i `id` wartość. Te segmenty są definiowane w odwołaniu *Startup.cs*.

[!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

EF ułatwia wyszukiwanie danych przy użyciu `SingleOrDefaultAsync` metody. Ważna funkcja zabezpieczeń wbudowanych w metodzie jest, czy kod sprawdza metody search znalazł filmu przed ponowną próbą podejmować żadnych działań z nim. Na przykład haker może wprowadzić błędy do witryny, zmieniając adres URL utworzony przez łącza z `http://localhost:xxxx/Movies/Details/1` podobną `http://localhost:xxxx/Movies/Details/12345` (lub inne wartości nie reprezentuje rzeczywisty film). Jeśli nie wybierzesz null filmu aplikacji spowoduje zgłoszenie wyjątku.

Sprawdź `Delete` i `DeleteConfirmed` metody.

[!code-csharp[Main](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete)]

Należy pamiętać, że `HTTP GET Delete` — metoda nie powoduje usunięcia określonego filmu, zwraca widok filmu którego (HttpPost) można przesłać usunięcia. Wykonywanie operacji usuwania w odpowiedzi na polecenie GET żądania (lub dla tej sprawy wykonywania operacji edycji, Utwórz operację lub innej operacji, które zmienia dane) otwiera luka w zabezpieczeniach.

`[HttpPost]` Nosi nazwę metody, która powoduje usunięcie danych `DeleteConfirmed` umożliwiają metodą HTTP POST unikatowego podpisu lub nazwę. Poniżej przedstawiono podpisy dwóch metod:

[!code-csharp[Main](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete2)]

[!code-csharp[Main](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete3)]


Środowisko uruchomieniowe języka wspólnego (CLR) wymaga przeciążonej metody ma unikatowy parametr podpisu (tej samej nazwy metody, ale lista różnych parametrów). Jednak w tym miejscu należy dwa `Delete` metody — jeden dla GET - i jeden dla żądania POST, że mają taką samą sygnaturę parametru. (Oba muszą zaakceptować pojedynczego całkowitą jako parametr.)

Istnieją dwa podejścia do tego problemu, co jest zapewniają różne nazwy metody. To mechanizm szkieletów został w poprzednim przykładzie. Jednak powstaje mały problem: ASP.NET mapuje segmentów adresu URL do metody akcji według nazwy i zmiana metody routingu zwykle nie będą mogli odnaleźć tej metody. Rozwiązanie, to zostanie wyświetlony w tym przykładzie jest dodanie `ActionName("Delete")` atrybutu `DeleteConfirmed` metody. Ten atrybut wykonuje mapowanie systemu routingu, aby znaleźć adres URL, który zawiera /Delete/ dla żądania POST `DeleteConfirmed` metody.

Inny wspólnej obejść dla metod, które mają identyczne nazwy i podpisy jest sztucznie Zmiana podpisu metody POST, aby uwzględnić dodatkowy parametr (i nieużywanych). To, co opisano w poprzedniej operacji publikowania podczas dodaliśmy `notUsed` parametru. Można tak samo postąpić tutaj dla `[HttpPost] Delete` metody:

```csharp
// POST: Movies/Delete/6
[ValidateAntiForgeryToken]
public async Task<IActionResult> Delete(int id, bool notUsed)
```

### <a name="publish-to-azure"></a>Publikowanie na platformie Azure

Zobacz [publikowania aplikacji sieci web platformy ASP.NET Core w usłudze Azure App Service przy użyciu programu Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) instrukcje dotyczące sposobu publikowania tej aplikacji na platformie Azure przy użyciu programu Visual Studio.  Aplikacja może również być publikowane z [wiersza polecenia](xref:tutorials/publish-to-azure-webapp-using-cli).

Dziękujemy za korzystanie to wprowadzenie do platformy ASP.NET Core MVC. Dziękujemy za wszelkie komentarze, które pozostaną. [Wprowadzenie do programu MVC i podstawowe EF](xref:data/ef-mvc/intro) jest doskonałym uzupełnianie w tym samouczku.

>[!div class="step-by-step"]
[Poprzednie](validation.md)
