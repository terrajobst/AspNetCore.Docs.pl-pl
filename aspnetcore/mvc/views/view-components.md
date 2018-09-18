---
title: Składniki widoków w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak składniki widoków są używane w programie ASP.NET Core oraz dodać je do aplikacji.
ms.author: riande
ms.date: 02/14/2017
uid: mvc/views/view-components
ms.openlocfilehash: 0410e2025019bae45d941e61f556f4b2b57bd30f
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46010913"
---
# <a name="view-components-in-aspnet-core"></a>Składniki widoków w programie ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

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

Składnik widok definiuje swojej logiki w `InvokeAsync` metodę, która zwraca `IViewComponentResult`. Parametry pochodzą bezpośrednio z wywołania części widoku, nie z wiązania modelu. Składnik widok nigdy nie obsługuje bezpośrednio żądania. Zazwyczaj składnikiem widoku inicjuje modelu i przekazuje je do widoku przez wywołanie metody `View` metody. Podsumowując wyświetlić metody składników:

* Zdefiniuj `InvokeAsync` metodę, która zwraca `IViewComponentResult`
* Zazwyczaj inicjuje modelu i przekazuje je do widoku, wywołując `ViewComponent` `View` — metoda
* Parametry pochodzą z wywołania metody HTTP nie znajduje się żadne wiązanie modelu
* To nie jest dostępny bezpośrednio jako punkt końcowy HTTP, są one wywoływane w kodzie (zwykle w widoku). Składnik widok nigdy nie obsługuje żądania
* Są przeciążone dla podpisu, a nie wszystkie szczegóły z bieżącego żądania HTTP

### <a name="view-search-path"></a>Ścieżka wyszukiwania widoku

Środowisko uruchomieniowe wyszukuje widoku w następujących ścieżkach:

* /Pages/składniki/\<view_component_name > /\<view_name >
* /Views/\<controller_name > /Components/\<view_component_name > /\<view_name >
* / Widoków/Shared/Components/\<view_component_name > /\<view_name >

Domyślna nazwa widoku składnika widoku to *domyślne*, co oznacza, że plik widoku zazwyczaj będzie miała nazwę *Default.cshtml*. Można określić nazwę innego widoku, tworząc wynik widoku składnika lub podczas wywoływania `View` metody.

Firma Microsoft zaleca, nazwij plik widoku *Default.cshtml* i użyj *widoków/Shared/Components/\<view_component_name > /\<view_name >* ścieżki. `PriorityList` Składnik widoku używane w tym przykładzie używa *Views/Shared/Components/PriorityList/Default.cshtml* widoku składnika widoku.

## <a name="invoking-a-view-component"></a>Wywoływanie składnika widoku

Aby użyć widoku składnika, wywołaj następujące wewnątrz widoku:

```cshtml
@Component.InvokeAsync("Name of view component", <anonymous type containing parameters>)
```

Parametry, które zostaną przekazane do `InvokeAsync` metody. `PriorityList` Widoku składnika opracowanych w artykule jest wywoływany z *Views/Todo/Index.cshtml* plik widoku. Poniższa `InvokeAsync` metoda jest wywoływana z dwoma parametrami:

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

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

Uwaga: Aby można było używać składnika widok jako pomocnika tagów, musisz się zarejestrować, zestaw zawierający przy użyciu widoku składnika `@addTagHelper` dyrektywy. Na przykład, jeśli składnik widoku znajduje się w zestawie o nazwie "MyWebApp", Dodaj następujące dyrektywy do `_ViewImports.cshtml` pliku:

```cshtml
@addTagHelper *, MyWebApp
```

Można zarejestrować składnika widok jako pomocnika tagów do każdego pliku, który odwołuje się do składnika widoku. Zobacz [Zarządzanie zakresem pomocnika tagów](xref:mvc/views/tag-helpers/intro#managing-tag-helper-scope) Aby uzyskać więcej informacji o sposobie rejestrowania pomocników tagów.

`InvokeAsync` Metodę używaną w ramach tego samouczka:

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

W znacznikach Pomocnik tagu:

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

W przykładzie powyżej `PriorityList` widoku składnika staje się `priority-list`. Parametry do składnika widoku są przekazywane jako atrybuty w małe litery kebab.

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

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Wstrzykiwanie zależności do widoków](xref:mvc/views/dependency-injection)
