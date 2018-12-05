---
title: Składniki widoków w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak składniki widoków są używane w programie ASP.NET Core oraz dodać je do aplikacji.
ms.author: riande
ms.date: 12/03/2018
uid: mvc/views/view-components
ms.openlocfilehash: 5812abad80cd906d6b9a7175bd7cdefd03a99eb3
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861332"
---
# <a name="view-components-in-aspnet-core"></a>Składniki widoków w programie ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))

## <a name="view-components"></a>Składniki widoków

Składniki widoków są podobne do widoków częściowych, ale są one znacznie bardziej wydajne. Składniki widoków nie używaj wiązania modelu i tylko zależą od podanych podczas wywoływania do niego danych. W tym artykule został napisany, za pomocą platformy ASP.NET Core MVC, ale wyświetlania składników również Praca ze stronami Razor.

Składnik widoku:

* Renderuje fragment, a nie całej odpowiedzi.
* Obejmuje takie same separacji z uwagi i korzyści z testowania znaleziono między kontrolerem a widokiem.
* Może mieć parametrów i logiki biznesowej.
* Zazwyczaj jest wywoływane ze strony układu.

Składniki widoków mają na celu dowolnym miejscu mieć logikę renderowania wielokrotnego użytku, która jest zbyt złożone dla widoku częściowego, takich jak:

* Menu dynamiczne nawigacji
* Obłoku (gdzie zapytań bazy danych)
* Panel logowania
* Koszyk
* Ostatnio opublikowane artykuły
* Zawartość paska bocznego na blogu typowe
* Panel logowania, który będzie renderowany na każdej stronie i Pokaż łącza Wyloguj się lub zaloguj się w zależności od tego, w dzienniku w stan użytkownika

Składnik Widok składa się z dwóch części: klasy (zazwyczaj uzyskiwane ze [ViewComponent](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponent)), a wynik zwraca (zazwyczaj widok). Np. kontrolery, składnik widok może być POCO, ale większość programistów będą chcieli korzystać z zalet metody i właściwości dostępne przez pochodząca od `ViewComponent`.

## <a name="creating-a-view-component"></a>Tworzenie widoku składnika

Ta sekcja zawiera ogólne wymagania dotyczące tworzenia widoku składnika. W dalszej części tego artykułu utworzymy Sprawdź każdy krok szczegółowo i tworzenie widoku składnika.

### <a name="the-view-component-class"></a>Widok klasy składników

Widok klasy składnika mogą być tworzone według dowolnej z następujących czynności:

* Wyprowadzanie z *ViewComponent*
* Urządzanie klasy z `[ViewComponent]` atrybutu lub pochodząca od klasy z `[ViewComponent]` atrybutu
* Tworzenie klasy, której nazwa kończy się sufiksem *ViewComponent*

Jak kontrolerów widok składniki muszą być publiczne, -nested i nieabstrakcyjnej klasy. Nazwa składnika widok jest nazwą klasy, wraz z sufiksem "ViewComponent" usunięte. Jego można również jawnie określać z użyciem `ViewComponentAttribute.Name` właściwości.

Widok klasy składnika:

* W pełni obsługuje konstruktora [wstrzykiwanie zależności](../../fundamentals/dependency-injection.md)

* Nie brać udział w cyklu życia kontrolera, co oznacza, nie można użyć [filtry](../controllers/filters.md) w składniku widoku

### <a name="view-component-methods"></a>Wyświetlanie składnika metod

Składnik widok definiuje swojej logiki w `InvokeAsync` metodę, która zwraca `Task<IViewComponentResult>` lub synchronicznego `Invoke` metodę, która zwraca `IViewComponentResult`. Parametry pochodzą bezpośrednio z wywołania części widoku, nie z wiązania modelu. Składnik widok nigdy nie obsługuje bezpośrednio żądania. Zazwyczaj składnikiem widoku inicjuje modelu i przekazuje je do widoku przez wywołanie metody `View` metody. Podsumowując wyświetlić metody składników:

* Zdefiniuj `InvokeAsync` metodę, która zwraca `Task<IViewComponentResult>` lub synchronicznego `Invoke` metodę, która zwraca `IViewComponentResult`.
* Zazwyczaj inicjuje modelu i przekazuje je do widoku, wywołując `ViewComponent` `View` metody.
* Parametry pochodzą z wywołania metody, a nie HTTP. Nie istnieje żadne wiązanie modelu.
* Nie są dostępne bezpośrednio jako punkt końcowy HTTP. Są one wywoływane w kodzie (zwykle w widoku). Składnik widok nigdy nie obsługuje żądania.
* Są przeciążone dla podpisu, a nie wszystkie szczegóły z bieżącego żądania HTTP.

### <a name="view-search-path"></a>Ścieżka wyszukiwania widoku

Środowisko uruchomieniowe wyszukuje widoku w następujących ścieżkach:

* /Pages/składniki / {Nazwa widoku składnika} / {Nazwa widoku}
* /Components/ /views/ {nazwa kontrolera} {Nazwa widoku składnika} / {Nazwa widoku}
* / Widoków/Shared/Components / {View nazwa składnika} / {Nazwa widoku}

Domyślna nazwa widoku składnika widoku to *domyślne*, co oznacza, że plik widoku zazwyczaj będzie miała nazwę *Default.cshtml*. Można określić nazwę innego widoku, tworząc wynik widoku składnika lub podczas wywoływania `View` metody.

Firma Microsoft zaleca, nazwij plik widoku *Default.cshtml* i użyj *widoków/Shared/Components / {Nazwa widoku składnika} / {Nazwa widoku}* ścieżki. `PriorityList` Składnik widoku używane w tym przykładzie używa *Views/Shared/Components/PriorityList/Default.cshtml* widoku składnika widoku.

## <a name="invoking-a-view-component"></a>Wywoływanie składnika widoku

Aby użyć widoku składnika, wywołaj następujące wewnątrz widoku:

```cshtml
@await Component.InvokeAsync("Name of view component", {Anonymous Type Containing Parameters})
```

Parametry, które zostaną przekazane do `InvokeAsync` metody. `PriorityList` Widoku składnika opracowanych w artykule jest wywoływany z *Views/Todo/Index.cshtml* plik widoku. Poniższa `InvokeAsync` metoda jest wywoływana z dwoma parametrami:

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

::: moniker range=">= aspnetcore-1.1"

## <a name="invoking-a-view-component-as-a-tag-helper"></a>Wywoływanie składnika widok jako pomocnika tagów

Dla platformy ASP.NET Core 1.1 lub nowszym, można wywołać składnika widok jako [Pomocnik tagu](xref:mvc/views/tag-helpers/intro):

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

Pascal — z uwzględnieniem wielkości liter parametry klasy i metody pomocników tagów są tłumaczone na ich [obniżyć przypadek kebab](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101). Pomocnik tagu do wywoływania składnika widoku używa `<vc></vc>` elementu. Składnik widoku określono w następujący sposób:

```cshtml
<vc:[view-component-name]
  parameter1="parameter1 value"
  parameter2="parameter2 value">
</vc:[view-component-name]>
```

Aby użyć widoku składnika jako pomocnika tagów, zarejestruj zestawu zawierającego za pomocą składnika widoku `@addTagHelper` dyrektywy. Jeśli składnik widoku znajduje się w zestawie o nazwie `MyWebApp`, Dodaj następujące dyrektywy *_ViewImports.cshtml* pliku:

```cshtml
@addTagHelper *, MyWebApp
```

Można zarejestrować składnika widok jako pomocnika tagów do każdego pliku, który odwołuje się do składnika widoku. Zobacz [Zarządzanie zakresem pomocnika tagów](xref:mvc/views/tag-helpers/intro#managing-tag-helper-scope) Aby uzyskać więcej informacji o sposobie rejestrowania pomocników tagów.

`InvokeAsync` Metodę używaną w ramach tego samouczka:

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

W znacznikach Pomocnik tagu:

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

W przykładzie powyżej `PriorityList` widoku składnika staje się `priority-list`. Parametry do składnika widoku są przekazywane jako atrybuty w małe litery kebab.

::: moniker-end

### <a name="invoking-a-view-component-directly-from-a-controller"></a>Wywoływanie składnika widoku bezpośrednio za pomocą kontrolera

Składniki widoków są zwykle wywoływani z widoku, ale można go wywołać bezpośrednio z metody kontrolera. Podczas wyświetlania składników nie Definiuj punktów końcowych, takich jak kontrolerów, akcji kontrolera, która zwraca treść można łatwo zaimplementować `ViewComponentResult`.

W tym przykładzie składnik ten widok jest wywoływany bezpośrednio z kontrolera:

[!code-csharp[](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

## <a name="walkthrough-creating-a-simple-view-component"></a>Wskazówki: Tworzenie składnika Widok prosty

[Pobierz](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample), tworzyć i testować kod startowy. Jest to prosty projekt za pomocą `Todo` kontrolera, który wyświetla listę *Todo* elementów.

![Lista zadań do wykonania](view-components/_static/2dos.png)

### <a name="add-a-viewcomponent-class"></a>Dodaj klasę ViewComponent

Tworzenie *ViewComponents* folderze i dodaj następującą `PriorityListViewComponent` klasy:

[!code-csharp[](view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponent1.cs?name=snippet1)]

Uwagi dotyczące kodu:

* Widok klas składników mogą być zawarte w **wszelkie** folderu w projekcie.
* Klasa name PriorityList**ViewComponent** kończy się sufiksem **ViewComponent**, środowisko uruchomieniowe będzie używać ciągu "PriorityList" podczas odwoływania się do składnika klasy z widoku. Czy mogę wyjaśnię, które bardziej szczegółowo później.
* `[ViewComponent]` Atrybutu można zmienić nazwę używaną do się odwoływać do składnika widoku. Na przykład firma Microsoft może już o nazwie klasy `XYZ` i stosowane `ViewComponent` atrybutu:

  ```csharp
  [ViewComponent(Name = "PriorityList")]
     public class XYZ : ViewComponent
     ```

* `[ViewComponent]` Atrybut powyżej informuje wybór składników widok, aby użyć nazwy `PriorityList` podczas wyszukiwania dla widoków skojarzonych ze składnikiem oraz użyć ciągu "PriorityList" podczas odwoływania się do składnika klasy z widoku. Czy mogę wyjaśnię, które bardziej szczegółowo później.
* Używany przez składnik [wstrzykiwanie zależności](../../fundamentals/dependency-injection.md) Aby udostępnić kontekst danych.
* `InvokeAsync` udostępnia metody, która może zostać wywołana z widoku, a może zająć dowolnej liczby argumentów.
* `InvokeAsync` Metoda zwraca zestaw elementów `ToDo` elementów, które spełniają `isDone` i `maxPriority` parametrów.

### <a name="create-the-view-component-razor-view"></a>Utwórz widok widoku Razor dla składnika

* Tworzenie *widoków/Shared/Components* folderu. Ten folder **musi** nosić *składniki*.

* Tworzenie *widoków/Shared/składniki/PriorityList* folderu. Ta nazwa folderu musi odpowiadać Nazwa klasy składnika widoku lub nazwa klasy minus sufiks (jeśli możemy stosowana Konwencja *ViewComponent* sufiksu w nazwie klasy). Jeśli użyto `ViewComponent` atrybutu, nazwa klasy będzie muszą być zgodne z nazwy atrybutu.

* Tworzenie *Views/Shared/Components/PriorityList/Default.cshtml* widoku Razor: [!code-cshtml[](view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/Default1.cshtml)]
    
   Widok Razor przyjmuje listę `TodoItem` i wyświetla je. Jeśli składnik widoku `InvokeAsync` metoda nie zakończy się pomyślnie Nazwa widoku (jak w naszym przykładzie), *domyślne* jest używana jako nazwa widoku, zgodnie z Konwencją. W dalszej części tego samouczka I opisano sposób przekazywania nazwy widoku. Aby zastąpić stylem domyślnym dla określonego kontrolera, Dodaj widok do folderu określonego kontrolera widoku (na przykład *Views/Todo/Components/PriorityList/Default.cshtml)*.
    
    Jeśli składnik widok jest specyficzne dla kontrolera, można dodać go do folderu określonego kontrolera (*Views/Todo/Components/PriorityList/Default.cshtml*).

* Dodaj `div` zawierającym wywołanie składnika Lista priorytetu do dołu *Views/Todo/index.cshtml* pliku:

    [!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexFirst.cshtml?range=34-38)]

Znaczniki `@await Component.InvokeAsync` pokazuje składnię do wywoływania składniki widoków. Pierwszy argument jest nazwa składnika, który chcemy, aby wywołać lub wywołania. Kolejne parametry są przekazywane do składnika. `InvokeAsync` może być dowolną liczbę argumentów.

Testowanie aplikacji. Na poniższej ilustracji przedstawiono lista czynności do wykonania i elementów o priorytecie:

![elementy listy i priorytet zadań do wykonania](view-components/_static/pi.png)

Składnik widoku można również wywołać bezpośrednio z kontrolera:

[!code-csharp[](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

![priorytet elementów z IndexVC akcji](view-components/_static/indexvc.png)

### <a name="specifying-a-view-name"></a>Określanie nazwy widoku

Składnik złożonego widoku może być konieczne określić widok innych niż domyślne, w niektórych warunkach. Poniższy kod przedstawia sposób określania widok "PVC" z `InvokeAsync` metody. Aktualizacja `InvokeAsync` method in Class metoda `PriorityListViewComponent` klasy.

[!code-csharp[](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponentFinal.cs?highlight=4,5,6,7,8,9&range=28-39)]

Kopiuj *Views/Shared/Components/PriorityList/Default.cshtml* pliku do widoku o nazwie *Views/Shared/Components/PriorityList/PVC.cshtml*. Dodaj nagłówek, aby wskazać, że jest używany widok PVC.

[!code-cshtml[](../../mvc/views/view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/PVC.cshtml?highlight=3)]

Aktualizacja *Views/TodoList/Index.cshtml*:

<!-- Views/TodoList/Index.cshtml is never imported, so change to test tutorial -->

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

Uruchom aplikację i sprawdź widok PVC.

![Priorytet widoku składnika](view-components/_static/pvc.png)

Jeśli nie jest renderowany widok PVC, sprawdź, czy są wywoływania składnika widoku z priorytetem 4 lub nowszy.

### <a name="examine-the-view-path"></a>Sprawdź ścieżkę widoku

* Zmień parametr priorytet do trzech lub mniej, więc Wyświetl priorytet nie jest zwracany.
* Tymczasowo zmień nazwę *Views/Todo/Components/PriorityList/Default.cshtml* do *1Default.cshtml*.
* Testowanie aplikacji, zostanie wyświetlony następujący błąd:

   ```
   An unhandled exception occurred while processing the request.
   InvalidOperationException: The view 'Components/PriorityList/Default' wasn't found. The following locations were searched:
   /Views/ToDo/Components/PriorityList/Default.cshtml
   /Views/Shared/Components/PriorityList/Default.cshtml
   EnsureSuccessful
   ```

* Kopiuj *Views/Todo/Components/PriorityList/1Default.cshtml* do *Views/Shared/Components/PriorityList/Default.cshtml*.
* Dodaj kilka znaczników w celu *Shared* Todo widoku składnika Widok, aby wskazać, w widoku pochodzi z *Shared* folderu.
* Test **Shared** widok składnika.

![Dane wyjściowe zadań do wykonania, przy użyciu widoku składnika współużytkowanego](view-components/_static/shared.png)

### <a name="avoiding-magic-strings"></a>Unikanie magic ciągów

Jeśli chcesz skompilować bezpieczeństwa czasu, można zastąpić nazwy składnika ustaloną widoku nazwą klasy. Utwórz składnik widoku bez sufiksu "ViewComponent":

[!code-csharp[](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityList.cs?highlight=10&range=5-35)]

Dodaj `using` instrukcję, aby Twoje Razor wyświetlanie plików i używanie `nameof` operator:

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexNameof.cshtml?range=1-6,35-)]

## <a name="perform-synchronous-work"></a>Wykonaj Praca synchroniczna

Struktura obsługuje wywoływanie synchronicznej `Invoke` metody, jeśli nie trzeba wykonywać pracę asynchroniczną. Poniższa metoda tworzy synchronicznego `Invoke` widoku składnika:

```csharp
public class PriorityList : ViewComponent
{
    public IViewComponentResult Invoke(int maxPriority, bool isDone)
    {
        var items = new List<string> { $"maxPriority: {maxPriority}", $"isDone: {isDone}" };
        return View(items);
    }
}
```

Składnik widoku Razor plik listy ciągi przekazywane do `Invoke` — metoda (*Views/Home/Components/PriorityList/Default.cshtml*):

```cshtml
@model List<string>

<h3>Priority Items</h3>
<ul>
    @foreach (var item in Model)
    {
        <li>@item</li>
    }
</ul>
```

::: moniker range=">= aspnetcore-1.1"

Składnik widok został wywołany w pliku Razor (na przykład *Views/Home/Index.cshtml*) przy użyciu jednej z następujących metod:

* <xref:Microsoft.AspNetCore.Mvc.IViewComponentHelper>
* [Pomocnik tagu](xref:mvc/views/tag-helpers/intro)

Aby użyć <xref:Microsoft.AspNetCore.Mvc.IViewComponentHelper> podejście, wywołaj `Component.InvokeAsync`:

::: moniker-end

::: moniker range="< aspnetcore-1.1"

Składnik widok został wywołany w pliku Razor (na przykład *Views/Home/Index.cshtml*) przy użyciu <xref:Microsoft.AspNetCore.Mvc.IViewComponentHelper>.

Wywołaj `Component.InvokeAsync`:

::: moniker-end

```cshtml
@await Component.InvokeAsync(nameof(PriorityList), new { maxPriority = 4, isDone = true })
```

::: moniker range=">= aspnetcore-1.1"

Aby użyć pomocnika tagów, należy zarejestrować zestaw zawierający przy użyciu widoku składnika `@addTagHelper` — dyrektywa (składnik widoku znajduje się w zestawie o nazwie `MyWebApp`):

```cshtml
@addTagHelper *, MyWebApp
```

Użyj widoku składnika Pomocnik tagu w pliku znaczników Razor:

```cshtml
<vc:priority-list max-priority="999" is-done="false">
</vc:priority-list>
```
::: moniker-end

Podpis metody `PriorityList.Invoke` jest synchroniczna, ale Razor znajduje i wywołuje metodę z `Component.InvokeAsync` w pliku znaczników.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Wstrzykiwanie zależności do widoków](xref:mvc/views/dependency-injection)
