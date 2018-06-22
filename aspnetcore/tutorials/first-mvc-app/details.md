---
title: Sprawdź szczegóły i metody zostaną usunięte z aplikacji platformy ASP.NET Core
author: rick-anderson
description: Więcej informacji na temat metody kontrolera szczegóły i wyświetlanie Podstawowa aplikacja platformy ASP.NET Core MVC.
ms.author: riande
ms.date: 03/07/2017
uid: tutorials/first-mvc-app/details
ms.openlocfilehash: f7f9a369e3e612542140fcf1091b21037e530a91
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278651"
---
# <a name="examine-the-details-and-delete-methods-of-an-aspnet-core-app"></a>Sprawdź szczegóły i metody zostaną usunięte z aplikacji platformy ASP.NET Core

przez [Rick Anderson](https://twitter.com/RickAndMSFT)

Otwórz kontrolera filmu i sprawdź, czy `Details` metody:

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](start-mvc/sample/MvcMovie21/Controllers/MoviesController.cs?name=snippet_details)]

::: moniker-end
::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_details)]

::: moniker-end

Aparat szkieletów MVC utworzony tą metodą akcji dodaje komentarz przedstawiający żądanie HTTP, która wywołuje metodę. W tym przypadku jest to żądanie GET z trzech segmenty adresu URL, `Movies` kontrolera, `Details` — metoda i `id` wartość. Te segmenty są definiowane w odwołaniu *Startup.cs*.

[!code-csharp[](start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

EF ułatwia wyszukiwanie danych przy użyciu `SingleOrDefaultAsync` metody. Ważna funkcja zabezpieczeń wbudowanych w metodzie jest, czy kod sprawdza metody search znalazł filmu przed ponowną próbą podejmować żadnych działań z nim. Na przykład haker może wprowadzić błędy do witryny, zmieniając adres URL utworzony przez łącza z `http://localhost:xxxx/Movies/Details/1` podobną `http://localhost:xxxx/Movies/Details/12345` (lub inne wartości nie reprezentuje rzeczywisty film). Jeśli nie wybierzesz null filmu aplikacji spowoduje zgłoszenie wyjątku.

Sprawdź `Delete` i `DeleteConfirmed` metody.

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](start-mvc/sample/MvcMovie21/Controllers/MoviesController.cs?name=snippet_delete)]

::: moniker-end
::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete)]

::: moniker-end

Należy pamiętać, że `HTTP GET Delete` — metoda nie powoduje usunięcia określonego filmu, zwraca widok filmu którego (HttpPost) można przesłać usunięcia. Wykonywanie operacji usuwania w odpowiedzi na polecenie GET żądania (lub dla tej sprawy wykonywania operacji edycji, Utwórz operację lub innej operacji, które zmienia dane) otwiera luka w zabezpieczeniach.

`[HttpPost]` Nosi nazwę metody, która powoduje usunięcie danych `DeleteConfirmed` umożliwiają metodą HTTP POST unikatowego podpisu lub nazwę. Poniżej przedstawiono podpisy dwóch metod:

[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete2)]

[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete3)]


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

> [!div class="step-by-step"]
> [Poprzednie](validation.md)
