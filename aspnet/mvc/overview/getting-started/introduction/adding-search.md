---
uid: mvc/overview/getting-started/introduction/adding-search
title: Wyszukiwanie | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/22/2015
ms.topic: article
ms.assetid: df001954-18bf-4550-b03d-43911a0ea186
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-search
msc.type: authoredcontent
ms.openlocfilehash: 116f681e14af0a09a4eb1502ef9f057c5db2f97d
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="search"></a>Wyszukaj
====================
Przez [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE[Tutorial Note](sample/code-location.md)]

## <a name="adding-a-search-method-and-search-view"></a>Dodawanie metody wyszukiwania i widoku wyszukiwania

W tej sekcji dodasz możliwość wyszukiwania `Index` metody akcji, która umożliwia wyszukiwanie filmów genre lub nazwę.

## <a name="updating-the-index-form"></a>Formularz indeksu

Uruchom aktualizując `Index` metody akcji do istniejącej `MoviesController` klasy. Oto kod:

[!code-csharp[Main](adding-search/samples/sample1.cs?highlight=1,6-9)]

W pierwszym wierszu `Index` metoda tworzy następujące [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) zapytanie, aby wybrać filmy:

[!code-csharp[Main](adding-search/samples/sample2.cs)]

Zapytanie jest zdefiniowany w tym momencie, ale nie został jeszcze uruchomiony w bazie danych.

Jeśli `searchString` parametru zawiera ciąg, filmy zapytania są modyfikowane w celu filtrowania wartość ciągu wyszukiwania, używając następującego kodu:

[!code-csharp[Main](adding-search/samples/sample3.cs)]

`s => s.Title` Kod powyżej [wyrażenia Lambda](https://msdn.microsoft.com/library/bb397687.aspx). Wyrażenia lambda są używane w oparte na metodzie [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) wysyła zapytanie jako argumenty do metod operator standardowej kwerendy, takich jak [gdzie](https://msdn.microsoft.com/library/system.linq.enumerable.where.aspx) metodę używaną w powyższym kodzie. Zapytania LINQ nie są wykonywane, gdy są one zdefiniowane lub modyfikacji przez wywołanie metody, takie jak `Where` lub `OrderBy`. Zamiast tego wykonywania zapytania jest opóźnione, co oznacza, że obliczania wyrażenia jest opóźnione aż do jej wartość rzeczywista jest rzeczywiście iterowane za pośrednictwem lub [ `ToList` ](https://msdn.microsoft.com/library/bb342261.aspx) metoda jest wywoływana. W `Search` przykładzie zapytanie jest wykonywane w *Index.cshtml* widoku. Aby uzyskać więcej informacji na temat wykonywania zapytań odroczonych, zobacz [wykonywania zapytania](https://msdn.microsoft.com/library/bb738633.aspx).

> [!NOTE]
> [Zawiera](https://msdn.microsoft.com/library/bb155125.aspx) metody jest uruchamiany w bazie danych, a nie kodu c# powyżej. W bazie danych [zawiera](https://msdn.microsoft.com/library/bb155125.aspx) mapuje [SQL LIKE](https://msdn.microsoft.com/library/ms179859.aspx), która jest uwzględniana wielkość liter.

Teraz możesz zaktualizować `Index` widok, który będzie wyświetlany formularz dla użytkownika.

Uruchom aplikację i przejdź do */filmów/indeksu*. Dołącz ciąg zapytania, takie jak `?searchString=ghost` do adresu URL. Filtrowane filmów są wyświetlane.

![SearchQryStr](adding-search/_static/image1.png)

Jeśli zmienisz podpis `Index` metoda ma parametr o nazwie `id`, `id` parametr będzie odpowiadać `{id}` symbolu zastępczego dla domyślnej kieruje zestawu w *aplikacji\_Start\ RouteConfig.cs* pliku.

[!code-json[Main](adding-search/samples/sample4.json)]

Oryginalna `Index` metody wygląda następująco:

[!code-csharp[Main](adding-search/samples/sample5.cs)]

Zmodyfikowane `Index` metoda będzie wyglądać następująco:

[!code-csharp[Main](adding-search/samples/sample6.cs?highlight=1,3)]

Tytuł wyszukiwania mogą być obecnie przekazywane jako dane trasy (segment adresu URL) zamiast jako wartość ciągu zapytania.

![](adding-search/_static/image2.png)

Nie można jednak spodziewać się użytkowników, aby zmodyfikować adres URL, za każdym razem, gdy chcą, aby wyszukać filmu. Tak, teraz możesz dodasz interfejsu użytkownika w celu ich filtrowania filmów. Zmiana podpisu `Index` metody do testowania sposób przekazywania parametru Identyfikatora trasy wiązaniem, zmień ją tak, że Twoje `Index` metoda przyjmuje parametr ciągu o nazwie `searchString`:

[!code-csharp[Main](adding-search/samples/sample7.cs)]

Otwórz *Views\Movies\Index.cshtml* pliku, a tylko po `@Html.ActionLink("Create New", "Create")`, Dodaj znacznika formularza wskazanych poniżej:

[!code-cshtml[Main](adding-search/samples/sample8.cshtml?highlight=12-15)]

`Html.BeginForm` Pomocnika tworzy otwierania `<form>` tagu. `Html.BeginForm` Pomocnika powoduje, że formularz publikowania do samej siebie, gdy użytkownik przesyła formularz, klikając **filtru** przycisku.

Visual Studio 2013 ma nieuprzywilejowany poprawy podczas wyświetlania i edytowania plików widoku. Po uruchomieniu aplikacji przy użyciu pliku widoku otwarte, Visual Studio 2013 wywołuje metodę akcji kontrolera poprawne, aby wyświetlić widok.

![](adding-search/_static/image3.png)

Z widoku indeksu Otwórz w programie Visual Studio (jak pokazano na ilustracji powyżej), wybierz przycisk F5 ewidencyjne lub F5, aby uruchomić aplikację, a następnie spróbuj wyszukiwanie filmu.

![](adding-search/_static/image4.png)

Brak nie `HttpPost` przeciążenia z `Index` metody. Nie jest konieczne, ponieważ metoda nie jest zmiana stanu aplikacji, po prostu filtrowania danych.

Można dodać następujące `HttpPost Index` metody. W takim przypadku element wywołujący akcji spowoduje dopasowanie `HttpPost Index` metody i `HttpPost Index` metoda może działać, jak pokazano na poniższej ilustracji.

[!code-csharp[Main](adding-search/samples/sample9.cs)]

![SearchPostGhost](adding-search/_static/image5.png)

Jednak nawet jeśli dodasz to `HttpPost` wersji `Index` metody, jest to ograniczenie, w jaki sposób to wszystko zostało zaimplementowane. Załóżmy, że chcesz utworzyć zakładkę określonego wyszukiwania lub chcesz wysłać łącze do znajomych, mogą kliknąć w celu wyświetlenia tego samego filtrowane listy filmów. Zwróć uwagę, że adres URL żądania HTTP POST jest taki sam jak adres URL dla żądania GET (localhost:xxxxx/filmów/indeksu) — nie ma żadnych informacji wyszukiwania w adresie URL, do samej siebie. Prawo teraz, wyszukaj ciąg informacje są wysyłane do serwera jako wartość pola formularza. Oznacza to, że nie można przechwycić informacje wyszukiwania zakładki lub wysyłać do znajomych w adresie URL.

Rozwiązaniem jest użycie przeciążenia `BeginForm` , który określa żądanie POST należy dodać informacje dotyczące wyszukiwania na adres URL i że powinny być kierowane do `HttpGet` wersji `Index` metody. Zamień istniejące bez parametrów `BeginForm` metody z następujący kod:

[!code-cshtml[Main](adding-search/samples/sample10.cshtml)]

![BeginFormPost_SM](adding-search/_static/image6.png)

Teraz podczas przesyłania wyszukiwania adres URL zawiera ciąg zapytania wyszukiwania. Wyszukiwanie będzie także przejść do `HttpGet Index` metody akcji, nawet jeśli masz `HttpPost Index` metody.

![IndexWithGetURL](adding-search/_static/image7.png)

## <a name="adding-search-by-genre"></a>Dodawanie wyszukiwania według rodzaju

Jeśli dodano `HttpPost` wersji `Index` metody, usuń go teraz.

Następnie dodasz funkcji, aby umożliwić użytkownikom wyszukiwanie filmów według rodzaju. Zastąp `Index` metodę z następującym kodem:

[!code-csharp[Main](adding-search/samples/sample11.cs)]

Ta wersja `Index` metoda przyjmuje dodatkowych parametrów, a mianowicie `movieGenre`. Tworzenie pierwszego kilku wierszy kodu `List` obiektu do przechowywania genres film z bazy danych.

Następujący kod jest zapytania LINQ, która pobiera wszystkie genres z bazy danych.

[!code-csharp[Main](adding-search/samples/sample12.cs)]

W kodzie użyto `AddRange` metody ogólnej `List` kolekcji można dodać do listy wszystkich unikatowych genres. (Bez `Distinct` modyfikator, zostanie dodany genres zduplikowane — na przykład zostanie dodany Komedia dwa razy w naszym przykładzie). Kod następnie przechowuje listę gatunkami muzyki w `ViewBag.MovieGenre` obiektu. Przechowywanie danych kategorii (takie określonego filmu rodzaju firmy) jako [SelectList](https://msdn.microsoft.cus/library/system.web.mvc.selectlist(v=vs.108).aspx) obiektu w `ViewBag`, a następnie uzyskiwanie dostępu do danych kategorii w polu listy rozwijanej jest podejście typowej aplikacji MVC.

Poniższy kod przedstawia sposób sprawdzania `movieGenre` parametru. Jeśli nie jest pusta, kodu ogranicza kwerendę filmy i ograniczyć wybranego filmów do określonego rodzaju.

[!code-csharp[Main](adding-search/samples/sample13.cs)]

Jak wspomniano wcześniej, zapytanie nie jest uruchamiane na bazę danych, aż do listy filmów jest iterowane za pośrednictwem (który odbywa się w widoku, po `Index` metoda akcji zwraca).

## <a name="adding-markup-to-the-index-view-to-support-search-by-genre"></a>Dodawanie znaczników do obsługi wyszukiwania według rodzaju widoku indeksu

Dodaj `Html.DropDownList` Pomocnika *Views\Movies\Index.cshtml* pliku tuż przed `TextBox` pomocnika. Poniżej przedstawiono wypełniony kod znaczników:

[!code-cshtml[Main](adding-search/samples/sample14.cshtml?highlight=11)]

W poniższym kodzie:

[!code-cshtml[Main](adding-search/samples/sample15.cshtml)]

Parametr "MovieGenre" udostępnia klucz dla `DropDownList` pomocnika można znaleźć `IEnumerable<SelectListItem>` w `ViewBag`. `ViewBag` Został wypełniony w metodzie akcji:

[!code-csharp[Main](adding-search/samples/sample16.cs?highlight=10)]

Parametr "All" zawiera etykiety opcji. Jeśli wybór ten można sprawdzić w przeglądarce, zobaczysz, że jego atrybut "value" jest pusty. Ponieważ kontrolera tylko filtry `if` ciąg nie jest `null` lub jest pusta, przesyłanie pustą wartość dla `movieGenre` pokazuje wszystkie genres.

Można również ustawić opcję należy wybrać domyślnie. Jeśli potrzebujesz "Komedia" możliwość domyślna zmieniłby kodu w kontrolerze w następujący sposób:

[!code-cshtml[Main](adding-search/samples/sample17.cshtml)]

Uruchom aplikację i przejdź do */filmów/indeksu*. Spróbować przeprowadzić wyszukiwanie według rodzaju, nazwa filmu i oba kryteria.

![](adding-search/_static/image8.png)

W tej sekcji utworzono metody akcji wyszukiwania i widok, który zezwala użytkownikom na wyszukiwanie według tytuł filmu i rodzaju. W następnej sekcji, możesz wyjaśniono, jak dodać właściwości do `Movie` modelu oraz sposobu dodawania inicjatora automatycznie utworzy testowej bazy danych.

>[!div class="step-by-step"]
[Poprzednie](examining-the-edit-methods-and-edit-view.md)
[dalej](adding-a-new-field.md)
