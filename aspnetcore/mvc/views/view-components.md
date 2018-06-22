---
title: Widok składniki platformy ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak widok składniki są używane w ASP.NET Core i sposobu dodawania ich do aplikacji.
ms.author: riande
ms.date: 02/14/2017
uid: mvc/views/view-components
ms.openlocfilehash: 2b196d8d46942604d1c85eb5f2f073661e5acb30
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278365"
---
# <a name="view-components-in-aspnet-core"></a>Widok składniki platformy ASP.NET Core

przez [Rick Anderson](https://twitter.com/RickAndMSFT)

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

## <a name="view-components"></a>Składniki w widoku

Widok składniki są podobne do widoków częściowych, ale są one bardziej wydajne. Składniki w widoku nie używaj wiązania modelu i tylko zależą od dostarczonych podczas wywoływania metody w nim danych. Ten artykuł dotyczy programu ASP.NET Core MVC, ale wyświetlania składników również współpracować z stron Razor.

Składnik widoku:

* Renderuje fragmentu, a nie całej odpowiedzi.
* Obejmuje takie same separacji z uwagi i korzyści z testowania znaleziono między kontrolerem a widokiem.
* Może mieć parametrów i logiki biznesowej.
* Zazwyczaj jest wywoływane ze strony układu.

Składniki w widoku mają gdziekolwiek się, że masz logiki renderowania wielokrotnego użytku, które jest zbyt złożony widoku częściowego, takich jak:

* Menu dynamiczne nawigacji
* Chmura znaczników (gdzie zapytanie bazy danych)
* Panel logowania
* Koszyk
* Ostatnio opublikowanych artykułów.
* Zawartość paska bocznego w typowych blogu
* Panel logowania, który będzie renderowany na każdej stronie i Pokaż łącza Wyloguj się lub zaloguj w zależności od dziennika w stanie użytkownika

Składnik widoku składa się z dwóch części: klasy (zwykle pochodną [ViewComponent](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponent)) i wynik zwraca (zazwyczaj widok). Podobnie jak kontrolerów składnika Widok może być POCO, ale większość deweloperów będą korzystać z metod i właściwości dostępne przez wynikających z `ViewComponent`.

## <a name="creating-a-view-component"></a>Tworzenie widoku składnika

Ta sekcja zawiera ogólne wymagania można utworzyć składnika widoku. W dalszej części tego artykułu możemy Sprawdź każdy krok szczegółowo i utworzyć składnika widoku.

### <a name="the-view-component-class"></a>Widok klasy składnika

Widok klasy składnika mogą być tworzone przez jedną z następujących czynności:

* Wyprowadzanie z *ViewComponent*
* Dekoracji klasy z `[ViewComponent]` atrybutu lub tworzenia klasy pochodnej z klasy z `[ViewComponent]` atrybutu
* Tworzenie klasy, której nazwa kończy się sufiksem *ViewComponent*

Podobnie jak kontrolerów widok składniki muszą być publiczne, -nested i nieabstrakcyjnej klasy. Nazwa składnika widoku jest nazwą klasy wraz z sufiksem "ViewComponent" usunięte. Można również ją jawnie określić przy użyciu `ViewComponentAttribute.Name` właściwości.

Widok klasy składnika:

* W pełni obsługuje konstruktora [iniekcji zależności](../../fundamentals/dependency-injection.md)

* Nie uczestniczy w cyklu życia kontrolera, co oznacza, nie można użyć [filtry](../controllers/filters.md) w składniku widoku

### <a name="view-component-methods"></a>Wyświetlanie składnika metod

Składnik widoku definiuje swojej logiki w `InvokeAsync` metodę zwracającą `IViewComponentResult`. Parametry pochodzi bezpośrednio z wywołania składnika widoku, nie z wiązania modelu. Składnik widoku nigdy nie obsługuje bezpośrednio żądanie. Zazwyczaj składnikiem widoku inicjuje modelu i przekazuje je do widoku, wywołując `View` metody. Podsumowując Wyświetl metody składników:

* Zdefiniuj `InvokeAsync` metodę zwracającą `IViewComponentResult`
* Zazwyczaj inicjuje modelu i przekazuje je do widoku, wywołując `ViewComponent` `View` — metoda
* Parametry pochodzą z wywołania metody HTTP nie znajduje się nie wiązanie modelu
* To nie jest dostępny bezpośrednio jako punkt końcowy HTTP, ich jest wywoływany z kodu (zazwyczaj w widoku). Składnik widoku nigdy nie obsługuje żądania
* Są przeciążone w sygnaturze, a nie wszystkie szczegóły z bieżącego żądania HTTP

### <a name="view-search-path"></a>Ścieżki wyszukiwania widoku

Środowisko uruchomieniowe wyszukuje widoku w następujących ścieżkach:

   * Views/\<controller_name>/Components/\<view_component_name>/\<view_name>
   * Widoki/Shared/składniki/\<view_component_name > /\<view_name >

Domyślna nazwa widoku dla składnika widoku to *domyślne*, co oznacza, że plik widoku zazwyczaj będzie miała nazwę *Default.cshtml*. Można określić nazwę inny widok, podczas tworzenia składnika wynik widoku lub podczas wywoływania metody `View` metody.

Firma Microsoft zaleca, nazwa pliku widoku *Default.cshtml* i użyj *widoków/Shared/składniki/\<view_component_name > /\<view_name >* ścieżki. `PriorityList` Składnik widoku używany w tym przykładzie używa *Views/Shared/Components/PriorityList/Default.cshtml* widoku składnika widoku.

## <a name="invoking-a-view-component"></a>Wywoływanie składnika widoku

Aby użyć widoku składnika, wywołaj następujące wewnątrz widoku:

```cshtml
@Component.InvokeAsync("Name of view component", <anonymous type containing parameters>)
```

Parametry zostaną przekazane do `InvokeAsync` metody. `PriorityList` Składnika widoku opracowane w artykule jest wywoływany z *Views/Todo/Index.cshtml* widoku pliku. Poniższa `InvokeAsync` metoda jest wywoływana z dwoma parametrami:

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

## <a name="invoking-a-view-component-as-a-tag-helper"></a>Wywoływanie składnika widoku jako pomocnika tagów

Dla platformy ASP.NET Core 1.1 i wyższych, można wywołać składnika widoku jako [pomocnika tagów](xref:mvc/views/tag-helpers/intro):

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

Pascal — z uwzględnieniem wielkości liter klasy i metody parametrów dla pomocników tagów są przekształcane na ich [obniżyć przypadku kebab](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101). Używa pomocnika tagów do wywołania składnika widoku `<vc></vc>` elementu. Składnik widoku określono w następujący sposób:

```cshtml
<vc:[view-component-name]
  parameter1="parameter1 value"
  parameter2="parameter2 value">
</vc:[view-component-name]>
```

Uwaga: Aby można było używać składnika widoku jako pomocnika tagów, należy zarejestrować zestawu zawierającego przy użyciu składnika widoku `@addTagHelper` dyrektywy. Na przykład, jeśli składnik widoku znajduje się w zestawie o nazwie "MyWebApp", Dodaj następujące dyrektywy `_ViewImports.cshtml` pliku:

```cshtml
@addTagHelper *, MyWebApp
```

Można zarejestrować składnika widoku jako pomocnika tagów do każdego pliku, który odwołuje się do składnika widoku. Zobacz [Zarządzanie zakresu pomocnika tagów](xref:mvc/views/tag-helpers/intro#managing-tag-helper-scope) Aby uzyskać więcej informacji na temat rejestrowania pomocników tagów.

`InvokeAsync` Metodę używaną w tym samouczku:

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

W znaczniku pomocnika tagów:

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

W powyższym przykładowym `PriorityList` staje się widok składnika `priority-list`. Parametry do składnika widoku są przekazywane jako atrybuty w przypadku kebab niższa.

### <a name="invoking-a-view-component-directly-from-a-controller"></a>Wywoływanie składnika widoku bezpośrednio z kontrolerem

Widok składniki zwykle są wywoływane z widoku, ale można ich wywoływać bezpośrednio z metody kontrolera. Podczas wyświetlania składników nie punkty końcowe, takich jak kontrolerów, można łatwo zaimplementować akcji kontrolera, która zwraca zawartość `ViewComponentResult`.

W tym przykładzie składnik widoku jest wywoływany bezpośrednio z kontrolerem:

[!code-csharp[](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

## <a name="walkthrough-creating-a-simple-view-component"></a>Wskazówki: Tworzenie składnika prosty widok

[Pobierz](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample), tworzenia i testowania kodu początkowego. Jest proste projektu z `Todo` kontrolera, który wyświetla listę *Todo* elementów.

![Lista ToDos](view-components/_static/2dos.png)

### <a name="add-a-viewcomponent-class"></a>Dodaj klasę ViewComponent

Utwórz *ViewComponents* folderu i dodaj następującą `PriorityListViewComponent` klasy:

[!code-csharp[](view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponent1.cs?name=snippet1)]

Uwagi o kodzie:

* Widok klas składników mogą być zawarte w **żadnych** folderu w projekcie.
* Klasa name PriorityList**ViewComponent** kończy się sufiksem **ViewComponent**, środowisko wykonawcze będzie używać ciągu "PriorityList" podczas odwoływania się do składnika klasy z widoku. I będzie wyjaśnić, że bardziej szczegółowo później.
* `[ViewComponent]` Atrybutu można zmienić nazwę używaną do odwołania składnika widoku. Na przykład firma Microsoft może już o nazwie klasy `XYZ` i stosowane `ViewComponent` atrybutu:

  ```csharp
  [ViewComponent(Name = "PriorityList")]
     public class XYZ : ViewComponent
     ```

* `[ViewComponent]` Atrybut powyżej informuje selektor składnika Widok, aby użyć nazwy `PriorityList` podczas wyszukiwania dla widoków skojarzone ze składnikiem i aby użyć ciągu "PriorityList" podczas odwoływania się do składnika klasy z widoku. I będzie wyjaśnić, że bardziej szczegółowo później.
* Używany przez składnik [iniekcji zależności](../../fundamentals/dependency-injection.md) udostępnić kontekstu danych.
* `InvokeAsync` udostępnia metodę, która może zostać wywołana z widoku, a może zająć dowolnej liczby argumentów.
* `InvokeAsync` Metoda zwraca zbiór `ToDo` elementów, które spełniają `isDone` i `maxPriority` parametrów.

### <a name="create-the-view-component-razor-view"></a>Tworzenie widoku Razor składnika widoku

* Utwórz *widoków/Shared/składniki* folderu. Ten folder **musi** nosić *składniki*.

* Utwórz *widoków/Shared/składniki/PriorityList* folderu. Nazwa tego folderu muszą być zgodne, nazwa klasy części widoku lub nazwę klasy minus sufiks (jeśli mamy po Konwencji i używana *ViewComponent* sufiksu w nazwie klasy). Jeśli używasz `ViewComponent` atrybutu, nazwa klasy musi do dopasowania nazwy atrybutu.

* Utwórz *Views/Shared/Components/PriorityList/Default.cshtml* widoku Razor: [!code-cshtml[](view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/Default1.cshtml)]
    
   W widoku Razor przyjmuje listę `TodoItem` i wyświetla je. Jeśli składnik widoku `InvokeAsync` — metoda nie przeszło nazwę widoku (w naszym przykładzie) *domyślne* jest używany dla nazwy widoku przez Konwencję. W dalszej części samouczka I opisano sposób przekazywania nazwy widoku. Aby zastąpić stylem domyślnym dla określonego kontrolera, Dodaj widok do folderu określonego kontrolera widoku (na przykład *Views/Todo/Components/PriorityList/Default.cshtml)*.
    
    Jeśli składnik widoku jest specyficzne dla kontrolera, można dodać go do folderu kontrolera specyficznych (*Views/Todo/Components/PriorityList/Default.cshtml*).

* Dodaj `div` zawierającym wywołanie składnika listy Priorytet do dołu *Views/Todo/index.cshtml* pliku:

    [!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexFirst.cshtml?range=34-38)]

Kod znaczników `@await Component.InvokeAsync` przedstawiono składnię wywoływania wyświetlania składników. Pierwszy argument jest nazwa składnika, którą chcemy udostępnić wywołania lub zadzwoń. Kolejne parametry są przekazywane do składnika. `InvokeAsync` może być dowolną liczbę argumentów.

Testowanie aplikacji. Na poniższej ilustracji przedstawiono listy rzeczy do zrobienia, a elementy o priorytecie:

![elementy listy i priorytet ToDo](view-components/_static/pi.png)

Możesz także wywołać bezpośrednio z kontrolera składnika widoku:

[!code-csharp[](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

![priorytet elementy z IndexVC akcji](view-components/_static/indexvc.png)

### <a name="specifying-a-view-name"></a>Określanie nazwy widoku

Składnik złożonego widoku może należy określić widok z systemem innym niż domyślny, w niektórych warunkach. Poniższy kod przedstawia sposób określania widoku "PVC" z `InvokeAsync` metody. Aktualizacja `InvokeAsync` metoda `PriorityListViewComponent` klasy.

[!code-csharp[](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponentFinal.cs?highlight=4,5,6,7,8,9&range=28-39)]

Kopiuj *Views/Shared/Components/PriorityList/Default.cshtml* pliku do widoku o nazwie *Views/Shared/Components/PriorityList/PVC.cshtml*. Dodaj nagłówek wskazuje, że widok PVC jest używany.

[!code-cshtml[](../../mvc/views/view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/PVC.cshtml?highlight=3)]

Aktualizacja *Views/TodoList/Index.cshtml*:

<!-- Views/TodoList/Index.cshtml is never imported, so change to test tutorial -->

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

Uruchom aplikację i zweryfikować PVC widoku.

![Priorytet widoku składnika](view-components/_static/pvc.png)

Jeśli nie jest renderowany widok PVC, sprawdź, czy są wywoływanie składnika widoku priorytet wynosi 4 lub nowszej.

### <a name="examine-the-view-path"></a>Sprawdź ścieżkę widoku

* Zmień parametr priorytet do trzech lub mniej, aby nie jest zwracana w widoku priorytet.
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
* Niektóre znaczników, aby dodać *Shared* Todo widoku składnika widoku, aby wskazać widoku pochodzi z *Shared* folderu.
* Test **Shared** widok składnika.

![Dane wyjściowe zadania z widoku składnika współużytkowanego](view-components/_static/shared.png)

### <a name="avoiding-magic-strings"></a>Unikanie magic ciągów

Jeśli chcesz skompilować bezpieczeństwa czasu, nazwy składnika ustalony widoku można zastąpić nazwę klasy. Utwórz widok składnika sufiksu "ViewComponent":

[!code-csharp[](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityList.cs?highlight=10&range=5-35)]

Dodaj `using` oświadczenie do użytkownika Razor wyświetlanie plików i używanie `nameof` operator:

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexNameof.cshtml?range=1-6,35-)]

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Wstrzykiwanie zależności do widoków](xref:mvc/views/dependency-injection)
