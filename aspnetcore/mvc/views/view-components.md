---
title: Wyświetl składniki w ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak składniki widoku są używane w ASP.NET Core i jak dodawać je do aplikacji.
ms.author: riande
ms.custom: mvc
ms.date: 05/14/2019
uid: mvc/views/view-components
ms.openlocfilehash: e6990368519857a27b291d7d565c09072f23f1b0
ms.sourcegitcommit: 7001657c00358b082734ba4273693b9b3ed35d2a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/31/2019
ms.locfileid: "68670084"
---
# <a name="view-components-in-aspnet-core"></a>Wyświetl składniki w ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/views/view-components/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))

## <a name="view-components"></a>Składniki widoku

Składniki widoku są podobne do widoków częściowych, ale są znacznie bardziej wydajne. Składniki widoku nie używają powiązania modelu i są zależne od danych, które są dostępne podczas wywoływania. Ten artykuł został zapisany przy użyciu kontrolerów i widoków, ale składniki widoków również współpracują z Razor Pages.

Składnik widoku:

* Renderuje fragment, a nie całą odpowiedź.
* Obejmuje te same rozbarwienia i korzyści z zakresu możliwości testowania, które znajdują się między kontrolerem i widokiem.
* Może mieć parametry i logikę biznesową.
* Jest zazwyczaj wywoływany ze strony układu.

Składniki widoku są zamierzone wszędzie tam, gdzie można ponownie używać logiki renderowania, która jest zbyt złożona dla widoku częściowego, na przykład:

* Dynamiczne menu nawigacji
* Tag Cloud (gdzie wysyła zapytanie do bazy danych)
* Panel logowania
* Koszyk
* Ostatnio opublikowane artykuły
* Zawartość paska bocznego w typowym blogu
* Panel logowania, który będzie renderowany na każdej stronie i pokazuje linki do wylogowania lub zalogowania, w zależności od stanu logowania użytkownika

Składnik widoku składa się z dwóch części: klasy (zwykle pochodnej od [ViewComponent](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponent)) i wynik zwracanego (zazwyczaj widok). Podobnie jak kontrolery, składnik widoku może być POCO, ale większość deweloperów chce skorzystać z metod i właściwości dostępnych w `ViewComponent`wyniku.

Biorąc pod uwagę, czy składniki widoku spełniają wymagania dotyczące aplikacji, zamiast tego Rozważ użycie składników Razor. Składniki Razor również łączą znaczniki C# z kodem, aby utworzyć jednostki interfejsu użytkownika wielokrotnego użytku. Składniki Razor są przeznaczone do produktywności deweloperów podczas udostępniania logiki interfejsu użytkownika po stronie klienta. Aby uzyskać więcej informacji, zobacz <xref:blazor/components>.

## <a name="creating-a-view-component"></a>Tworzenie składnika widoku

Ta sekcja zawiera wymagania wysokiego poziomu dotyczące tworzenia składnika widoku. W dalszej części tego artykułu sprawdzimy szczegółowo poszczególne kroki i utworzysz składnik widoku.

### <a name="the-view-component-class"></a>Klasa widoku

Klasę składnika widoku można utworzyć przy użyciu dowolnego z następujących elementów:

* Wyprowadzanie z *ViewComponent*
* Dekorowania nazwy klasę z `[ViewComponent]` atrybutem lub pochodną klasy `[ViewComponent]` z atrybutem
* Tworzenie klasy, w której następuje nazwa z sufiksem *ViewComponent*

Podobnie jak kontrolery, składniki widoku muszą być publiczne, niezagnieżdżone i nieabstrakcyjne. Nazwa składnika widoku to nazwa klasy z usuniętym sufiksem "ViewComponent". Można go również jawnie określić przy użyciu `ViewComponentAttribute.Name` właściwości.

Klasa składnika widoku:

* W pełni obsługuje [iniekcję zależności](../../fundamentals/dependency-injection.md) konstruktora

* Nie uczestniczy w cyklu życia kontrolera, co oznacza, że nie można używać [filtrów](../controllers/filters.md) w składniku widoku

### <a name="view-component-methods"></a>Wyświetlanie metod składników

Składnik `InvokeAsync` widoku definiuje swoją logikę w metodzie, która `Task<IViewComponentResult>` zwraca lub w metodzie `Invoke` `IViewComponentResult`synchronicznej, która zwraca wartość. Parametry pochodzą bezpośrednio z wywołania składnika widoku, a nie z powiązania modelu. Składnik widoku nigdy nie obsługuje bezpośrednio żądania. Zazwyczaj składnik widoku inicjuje model i przekazuje go do widoku przez wywołanie `View` metody. Podsumowując, Wyświetl metody składników:

* Zdefiniuj metodę zwracającą `Invoke` lub metodę synchroniczną, która zwraca obiekt `IViewComponentResult`. `Task<IViewComponentResult>` `InvokeAsync`
* Zazwyczaj inicjuje model i przekazuje go do widoku przez wywołanie `ViewComponent` `View` metody.
* Parametry pochodzą z metody wywołującej, a nie HTTP. Brak powiązania modelu.
* Nie są dostępne bezpośrednio jako punkt końcowy HTTP. Są wywoływane z kodu (zazwyczaj w widoku). Składnik widoku nigdy nie obsługuje żądania.
* Są przeciążone w sygnaturze, a nie żadne szczegóły z bieżącego żądania HTTP.

### <a name="view-search-path"></a>Wyświetl ścieżkę wyszukiwania

Środowisko uruchomieniowe wyszukuje widok w następujących ścieżkach:

* Nazwa/Views/{Controller}/Components/{View nazwa składnika}/{View Name}
* Nazwa składnika/Views/Shared/Components/{View}/{View Name}
* Nazwa składnika/Pages/Shared/Components/{View}/{View Name}

Ścieżka wyszukiwania ma zastosowanie do projektów korzystających z kontrolerów i widoków oraz Razor Pages.

Domyślna nazwa widoku dla składnika widoku jest *Domyślna*, co oznacza, że plik widoku będzie zazwyczaj nazwany *default. cshtml*. Możesz określić inną nazwę widoku podczas tworzenia wyniku składnika widoku lub podczas wywoływania `View` metody.

Zalecamy, aby nazwa pliku widoku *default. cshtml* i użyć ścieżki *views/Shared/Components/{View nazwa składnika}/{View Name}* . Składnik widoku używany w tym przykładzie używa *widoków/Shared/Components/PriorityList/default. cshtml* dla widoku składnika widoku. `PriorityList`

## <a name="invoking-a-view-component"></a>Wywoływanie składnika widoku

Aby użyć składnika widoku, wywołaj następujące elementy w widoku:

```cshtml
@await Component.InvokeAsync("Name of view component", {Anonymous Type Containing Parameters})
```

Parametry zostaną przesłane do `InvokeAsync` metody. Składnik widoku utworzony w artykule jest wywoływany z pliku widoku *widoków/zadania/index. cshtml.* `PriorityList` W poniższej `InvokeAsync` tabeli Metoda jest wywoływana z dwoma parametrami:

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/ToDo/IndexFinal.cshtml?range=35)]

::: moniker range=">= aspnetcore-1.1"

## <a name="invoking-a-view-component-as-a-tag-helper"></a>Wywoływanie składnika widoku jako pomocnika tagów

W przypadku ASP.NET Core 1,1 i wyższych można wywołać składnik widoku jako [pomocnika tagów](xref:mvc/views/tag-helpers/intro):

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/ToDo/IndexTagHelper.cshtml?range=37-38)]

Klasy z wielkością liter w języku Pascal i parametry metody dla pomocników tagów są tłumaczone na ich [Kebab](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101). Pomocnik tagu do wywołania składnika widoku używa `<vc></vc>` elementu. Składnik widoku jest określony w następujący sposób:

```cshtml
<vc:[view-component-name]
  parameter1="parameter1 value"
  parameter2="parameter2 value">
</vc:[view-component-name]>
```

Aby użyć składnika widoku jako pomocnika tagów, zarejestruj zestaw zawierający składnik widoku za pomocą `@addTagHelper` dyrektywy. Jeśli składnik widoku znajduje się w zestawie o nazwie `MyWebApp`, Dodaj następującą dyrektywę do pliku *_ViewImports. cshtml* :

```cshtml
@addTagHelper *, MyWebApp
```

Składnik widoku można zarejestrować jako pomocnika tagów do każdego pliku, który odwołuje się do składnika widoku. Zobacz temat [Zarządzanie zakresem pomocnika tagów](xref:mvc/views/tag-helpers/intro#managing-tag-helper-scope) , aby uzyskać więcej informacji na temat rejestrowania pomocników tagów.

`InvokeAsync` Metoda używana w tym samouczku:

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/ToDo/IndexFinal.cshtml?range=35)]

Znaczniki pomocnika tagów:

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/ToDo/IndexTagHelper.cshtml?range=37-38)]

W powyższym `PriorityList` przykładzie składnik widoku zmieni się `priority-list`. Parametry składnika widoku są przenoszone jako atrybuty w przypadku Kebab.

::: moniker-end

### <a name="invoking-a-view-component-directly-from-a-controller"></a>Wywoływanie składnika widoku bezpośrednio z kontrolera

Składniki widoku są zwykle wywoływane z widoku, ale można je wywołać bezpośrednio z metody kontrolera. Chociaż składniki widoku nie definiują punktów końcowych, takich jak kontrolery, można łatwo zaimplementować akcję kontrolera, która zwraca `ViewComponentResult`zawartość.

W tym przykładzie składnik widoku jest wywoływany bezpośrednio z kontrolera:

[!code-csharp[](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

## <a name="walkthrough-creating-a-simple-view-component"></a>Przewodnik: Tworzenie prostego składnika widoku

[Pobierz](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/views/view-components/sample), skompiluj i przetestuj kod startowy. Jest to prosty projekt z `ToDo` kontrolerem, który wyświetla listę elementów do *wykonania* .

![Lista zadań do wykonania](view-components/_static/2dos.png)

### <a name="add-a-viewcomponent-class"></a>Dodaj klasę ViewComponent

Utwórz folder *ViewComponents* i Dodaj następującą `PriorityListViewComponent` klasę:

[!code-csharp[](view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponent1.cs?name=snippet1)]

Uwagi dotyczące kodu:

* Klasy składników widoku mogą być zawarte w **dowolnym** folderze w projekcie.
* Ponieważ nazwa klasy PriorityList**ViewComponent** kończąca się sufiksem **ViewComponent**, środowisko uruchomieniowe będzie używać ciągu "PriorityList" podczas odwoływania się do składnika klasy z widoku. Wyjaśnimy, że w dalszej części bardziej szczegółowo.
* Ten `[ViewComponent]` atrybut może zmienić nazwę używaną do odwoływania się do składnika widoku. Można na przykład nazwać klasę `XYZ` i `ViewComponent` zastosować atrybut:

  ```csharp
  [ViewComponent(Name = "PriorityList")]
     public class XYZ : ViewComponent
     ```

* W `[ViewComponent]` powyższym atrybucie nakazuje selektorowi składnika widoku użycie `PriorityList` nazwy podczas wyszukiwania widoków skojarzonych ze składnikiem i użycie ciągu "PriorityList" podczas odwoływania się do składnika klasy z widoku. Wyjaśnimy, że w dalszej części bardziej szczegółowo.
* Składnik używa [iniekcji zależności](../../fundamentals/dependency-injection.md) , aby udostępnić kontekst danych.
* `InvokeAsync`uwidacznia metodę, która może być wywoływana z widoku i może przyjmować dowolną liczbę argumentów.
* Metoda zwraca `isDone` zestaw elementów, które spełniają parametry i `maxPriority`. `ToDo` `InvokeAsync`

### <a name="create-the-view-component-razor-view"></a>Tworzenie widoku Razor składnika widoku

* Utwórz folder *widoki/udostępnione/składniki* . Ten folder **musi** być nazwanymi *składnikami*.

* Utwórz folder *widoki/udostępnione/składniki/PriorityList* . Nazwa folderu musi być zgodna z nazwą klasy składnika widoku lub nazwą klasy pomniejszonej o sufiks (jeśli została postosowana Konwencja i użyto sufiksu *ViewComponent* w nazwie klasy). Jeśli użyto `ViewComponent` atrybutu, nazwa klasy musi być zgodna z oznaczeniem atrybutu.

* Utwórz *widoki/Shared/Components/PriorityList/default. cshtml* Razor widok:


  [!code-cshtml[](view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/Default1.cshtml)]

   Widok Razor pobiera listę `TodoItem` i wyświetla je. Jeśli metoda widoku składnika `InvokeAsync` nie przekaże nazwy widoku (jak w naszym przykładzie), *Domyślnie* jest używana jako nazwa widoku według Konwencji. W dalszej części tego samouczka pokażę, jak przekazać nazwę widoku. Aby zastąpić domyślne style dla określonego kontrolera, Dodaj widok do folderu widoku określonego dla kontrolera (na przykład *widoki/zadania/składniki/PriorityList/default. cshtml)* .

    Jeśli składnik widoku jest specyficzny dla kontrolera, można go dodać do folderu właściwego dla kontrolera (*widoki/zadania/składniki/PriorityList/default. cshtml*).

* Dodaj element zawierający wywołanie do składnika listy priorytetu w dolnej części pliku *views/do zrobienia/index. cshtml:* `div`

    [!code-cshtml[](view-components/sample/ViewCompFinal/Views/ToDo/IndexFirst.cshtml?range=34-38)]

Znacznik `@await Component.InvokeAsync` pokazuje składnię dla wywoływanych składników widoku. Pierwszy argument jest nazwą składnika, który ma zostać wywołany lub wywołany. Kolejne parametry są przesyłane do składnika. `InvokeAsync`może przyjmować dowolną liczbę argumentów.

Przetestuj aplikację. Na poniższej ilustracji przedstawiono listę zadań do wykonania i priorytet:

![Lista zadań do wykonania i priorytetowe elementy](view-components/_static/pi.png)

Możesz również wywołać składnik widoku bezpośrednio z poziomu kontrolera:

[!code-csharp[](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

![priorytet elementów z akcji IndexVC](view-components/_static/indexvc.png)

### <a name="specifying-a-view-name"></a>Określanie nazwy widoku

Składnik widoku złożonego może wymagać określenia widoku innego niż domyślny w pewnych warunkach. Poniższy kod ilustruje sposób określania widoku "PVC" z `InvokeAsync` metody. `InvokeAsync` Zaktualizuj metodę`PriorityListViewComponent` w klasie.

[!code-csharp[](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponentFinal.cs?highlight=4,5,6,7,8,9&range=28-39)]

Skopiuj plik *views/Shared/Components/PriorityList/default. cshtml* do widoku o nazwie *views/Shared/Components/PriorityList/PVC. cshtml*. Dodaj nagłówek, aby wskazać, że widok obwodu PVC jest używany.

[!code-cshtml[](../../mvc/views/view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/PVC.cshtml?highlight=3)]

Aktualizowanie *widoków/do zrobienia/index. cshtml*:

<!-- Views/ToDo/Index.cshtml is never imported, so change to test tutorial -->

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/ToDo/IndexFinal.cshtml?range=35)]

Uruchom aplikację i sprawdź Widok obwodu PVC.

![Składnik widoku priorytetu](view-components/_static/pvc.png)

Jeśli widok obwodu PVC nie jest renderowany, sprawdź, czy wywoływany jest składnik widoku o priorytecie 4 lub wyższym.

### <a name="examine-the-view-path"></a>Sprawdzanie ścieżki widoku

* Zmień wartość parametru Priority na trzy lub mniej, aby widok priorytet nie został zwrócony.
* Tymczasowe zmiany nazwy *widoków/zadania/składniki/PriorityList/default. cshtml* na *1Default. cshtml*.
* Przetestuj aplikację, zostanie wyświetlony następujący błąd:

   ```
   An unhandled exception occurred while processing the request.
   InvalidOperationException: The view 'Components/PriorityList/Default' wasn't found. The following locations were searched:
   /Views/ToDo/Components/PriorityList/Default.cshtml
   /Views/Shared/Components/PriorityList/Default.cshtml
   EnsureSuccessful
   ```

* Kopiuj *widoki/zadania/składniki/PriorityList/1Default. cshtml* do *widoków/Shared/Components/PriorityList/default. cshtml*.
* Dodaj adiustację do widoku *udostępnionego* składnika widoku do wykonania, aby wskazać, że widok pochodzi z folderu *udostępnionego* .
* Przetestuj widok składnika współużytkowanego.

![Dane wyjściowe zadania z widokiem udostępnionego składnika](view-components/_static/shared.png)

### <a name="avoiding-hard-coded-strings"></a>Unikanie stałych zakodowanych ciągów

Jeśli chcesz uzyskać bezpieczeństwo czasu kompilowania, możesz zastąpić ustaloną nazwę składnika widoku nazwą klasy. Utwórz składnik widoku bez sufiksu "ViewComponent":

[!code-csharp[](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityList.cs?highlight=10&range=5-35)]

Dodaj instrukcję do pliku widoku Razor i `nameof` Użyj operatora: `using`

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/ToDo/IndexNameof.cshtml?range=1-6,35-)]

## <a name="perform-synchronous-work"></a>Wykonywanie synchronicznej pracy

Struktura obsługuje wywoływanie metody synchronicznej `Invoke` , jeśli nie trzeba wykonywać operacji asynchronicznej. W poniższej metodzie jest tworzony `Invoke` składnik widoku synchronicznego:

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

Plik Razor składnika widoku Wyświetla listę ciągów przekazaną do `Invoke` metody (*przegląda/Home/Components/PriorityList/default. cshtml*):

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

Składnik widoku jest wywoływany w pliku Razor (na przykład *widoki/Home/index. cshtml*) przy użyciu jednej z następujących metod:

* <xref:Microsoft.AspNetCore.Mvc.IViewComponentHelper>
* [Pomocnik tagów](xref:mvc/views/tag-helpers/intro)

Aby użyć <xref:Microsoft.AspNetCore.Mvc.IViewComponentHelper> podejścia, wywołaj `Component.InvokeAsync`:

::: moniker-end

::: moniker range="< aspnetcore-1.1"

Składnik widoku jest wywoływany w pliku Razor (na przykład Przeglądas */Home/index. cshtml*) z <xref:Microsoft.AspNetCore.Mvc.IViewComponentHelper>.

Wywołanie `Component.InvokeAsync`:

::: moniker-end

```cshtml
@await Component.InvokeAsync(nameof(PriorityList), new { maxPriority = 4, isDone = true })
```

::: moniker range=">= aspnetcore-1.1"

Aby użyć pomocnika tagów, zarejestruj zestaw zawierający składnik widoku przy użyciu `@addTagHelper` dyrektywy (składnik widoku znajduje się w zestawie o nazwie `MyWebApp`):

```cshtml
@addTagHelper *, MyWebApp
```

Użyj pomocnika tagów składnika w pliku znaczników Razor:

```cshtml
<vc:priority-list max-priority="999" is-done="false">
</vc:priority-list>
```

::: moniker-end

Sygnatura `PriorityList.Invoke` metody jest synchroniczna, ale Razor znajduje i wywołuje metodę z `Component.InvokeAsync` w pliku znaczników.

## <a name="all-view-component-parameters-are-required"></a>Wszystkie parametry składnika widoku są wymagane

Każdy parametr w składniku widoku jest atrybutem wymaganym. Zobacz [ten problem](https://github.com/aspnet/AspNetCore/issues/5011)w serwisie GitHub. Jeśli dowolny parametr zostanie pominięty:

* Sygnatura `InvokeAsync` metody nie jest zgodna, dlatego metoda nie zostanie wykonana.
* ViewComponent nie renderuje żadnych znaczników.
* Nie zostaną zgłoszone żadne błędy.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Wstrzykiwanie zależności do widoków](xref:mvc/views/dependency-injection)
