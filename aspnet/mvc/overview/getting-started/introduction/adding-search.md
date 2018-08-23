---
uid: mvc/overview/getting-started/introduction/adding-search
title: Wyszukiwanie | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 05/22/2015
ms.assetid: df001954-18bf-4550-b03d-43911a0ea186
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-search
msc.type: authoredcontent
ms.openlocfilehash: 70a9b08a76a07350bac696f1c74e6397d2aa6ee0
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752666"
---
<a name="search"></a>Wyszukaj
====================
Przez [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

## <a name="adding-a-search-method-and-search-view"></a>Dodawanie metody wyszukiwania i widoku wyszukiwania

W tej sekcji dodasz możliwości wyszukiwania do `Index` metody akcji, która umożliwia wyszukiwanie filmów według gatunku lub nazwy.

## <a name="updating-the-index-form"></a>Aktualizowanie indeksu formularza

Rozpocznij, aktualizując `Index` metody akcji do istniejących `MoviesController` klasy. Oto kod:

[!code-csharp[Main](adding-search/samples/sample1.cs?highlight=1,6-9)]

W pierwszym wierszu `Index` metoda tworzy następujące [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) zapytanie, aby wybrać filmów:

[!code-csharp[Main](adding-search/samples/sample2.cs)]

Zapytanie jest zdefiniowany w tym momencie, ale nie zostało jeszcze uruchomione w bazie danych.

Jeśli `searchString` parametru zawiera ciąg, zapytanie filmy zostanie zmodyfikowany na potrzeby filtrowania na wartość ciągu wyszukiwania, używając następującego kodu:

[!code-csharp[Main](adding-search/samples/sample3.cs)]

`s => s.Title` Powyższy kod jest [wyrażenia Lambda](https://msdn.microsoft.com/library/bb397687.aspx). Wyrażenia lambda są używane w oparte na metodzie [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) jako argumenty do standardowych metod operatorów kwerendy takich zapytań jak [gdzie](https://msdn.microsoft.com/library/system.linq.enumerable.where.aspx) metodę użytą w kodzie powyżej. Zapytania LINQ nie są wykonywane, gdy są one definiowane lub modyfikacji przez wywołanie metody, takie jak `Where` lub `OrderBy`. Zamiast tego, wykonanie zapytania jest odroczone, co oznacza, że wyniku obliczenia wyrażenia zostanie opóźnione, dopóki wartość zrealizowane faktycznie jest powtarzana lub [ `ToList` ](https://msdn.microsoft.com/library/bb342261.aspx) metoda jest wywoływana. W `Search` przykładzie zapytanie jest wykonywane w *Index.cshtml* widoku. Aby uzyskać więcej informacji na temat wykonywania zapytań z opóźnieniem, zobacz [wykonywania zapytania](https://msdn.microsoft.com/library/bb738633.aspx).

> [!NOTE]
> [Zawiera](https://msdn.microsoft.com/library/bb155125.aspx) metoda jest uruchomiona w bazie danych, a nie kodu c# powyżej. W bazie danych [zawiera](https://msdn.microsoft.com/library/bb155125.aspx) mapuje [SQL LIKE](https://msdn.microsoft.com/library/ms179859.aspx), która jest uwzględniana wielkość liter.

Teraz możesz zaktualizować `Index` widok, w którym będą wyświetlane na formularzu do użytkownika.

Uruchom aplikację, a następnie przejdź do */filmy/indeks*. Dołącz ciąg zapytania, takie jak `?searchString=ghost` do adresu URL. Wyświetlane są filtrowane filmów.

![SearchQryStr](adding-search/_static/image1.png)

Jeśli zmienisz podpis `Index` metoda będzie miała parametru o nazwie `id`, `id` parametr będzie odpowiadał `{id}` symbolu zastępczego dla domyślnej kieruje zestawu w *aplikacji\_Start\ RouteConfig.cs* pliku.

[!code-json[Main](adding-search/samples/sample4.json)]

Oryginalny `Index` metoda wygląda następująco:

[!code-csharp[Main](adding-search/samples/sample5.cs)]

Zmodyfikowanego `Index` metoda wyglądałby następująco:

[!code-csharp[Main](adding-search/samples/sample6.cs?highlight=1,3)]

Tytuł wyszukiwania można teraz przekazywać jako dane trasy (segment adresu URL) zamiast jako wartość ciągu zapytania.

![](adding-search/_static/image2.png)

Jednak nie można oczekiwać od użytkowników, aby zmodyfikować adres URL, za każdym razem, gdy chcą wyszukiwania filmów. Teraz możesz dodasz interfejs użytkownika, aby pomóc im filtrowanie filmów. Jeśli zmienisz podpis `Index` metodę, aby przetestować sposób przekazywania parametru ID powiązane z tras, zmień ją tak, że Twoje `Index` metoda przyjmuje jako parametr ciągu o nazwie `searchString`:

[!code-csharp[Main](adding-search/samples/sample7.cs)]

Otwórz *Views\Movies\Index.cshtml* pliku, a tylko po `@Html.ActionLink("Create New", "Create")`, Dodaj znaczników formularza, które przedstawiono poniżej:

[!code-cshtml[Main](adding-search/samples/sample8.cshtml?highlight=12-15)]

`Html.BeginForm` Pomocnika tworzy otwierający `<form>` tagu. `Html.BeginForm` Pomocnika powoduje, że formularz do publikowania do samego siebie, gdy użytkownik przesyła formularz, klikając **filtru** przycisku.

Visual Studio 2013 ma nieuprzywilejowany poprawy jakości, podczas wyświetlania i edytowania plików widoku. Po uruchomieniu aplikacji z plikiem widok Otwórz program Visual Studio 2013 wywołuje metody akcji kontrolera poprawne, aby wyświetlić widok.

![](adding-search/_static/image3.png)

Przy użyciu widoku indeksu Otwórz w programie Visual Studio (jak pokazano na ilustracji powyżej), naciśnij przycisk kont F5 lub F5, aby uruchomić aplikację, a następnie spróbuj wyszukać film.

![](adding-search/_static/image4.png)

Istnieje nie `HttpPost` przeciążenia `Index` metody. Nie są potrzebne, ponieważ metoda nie jest zmiana stanu aplikacji, po prostu filtrowania danych.

Można dodać następujące `HttpPost Index` metody. W takiej sytuacji będzie odpowiadać element wywołujący akcji `HttpPost Index` metody i `HttpPost Index` metoda może działać, jak pokazano na poniższej ilustracji.

[!code-csharp[Main](adding-search/samples/sample9.cs)]

![SearchPostGhost](adding-search/_static/image5.png)

Jednak nawet w przypadku dodania to `HttpPost` wersję `Index` metody, jest to ograniczenie, w jak to wszystko została zaimplementowana. Wyobraź sobie, że chcesz utworzyć zakładkę określonego wyszukiwania lub chcesz wysłać link do znajomych, mogą kliknąć umożliwiający zobaczenie tej samej listy filtrowane filmów. Należy zauważyć, że adres URL żądania HTTP POST jest taki sam jak adres URL dla żądania GET (localhost:xxxxx/filmy/indeksu) — nie ma żadnych informacji wyszukiwania w adresie URL, sam. Po prawej stronie teraz informacje o parametrach wyszukiwania jest wysyłany do serwera jako wartość pola formularza. Oznacza to, że nie można przechwycić informacje wyszukiwania do zakładki lub wysłać znajomym w adresie URL.

Rozwiązaniem jest używanie przeciążenia `BeginForm` Określa, że żądanie POST, należy dodać informacje dotyczące wyszukiwania do adresu URL i że powinny być kierowane do `HttpGet` wersję `Index` metody. Zastąp istniejące bez parametrów `BeginForm` metody za pomocą następujących znaczników:

[!code-cshtml[Main](adding-search/samples/sample10.cshtml)]

![BeginFormPost_SM](adding-search/_static/image6.png)

Teraz gdy prześlesz wyszukiwania, adres URL zawiera ciąg zapytania wyszukiwania. Wyszukiwanie będzie także przejść do `HttpGet Index` metody akcji, nawet jeśli masz `HttpPost Index` metody.

![IndexWithGetURL](adding-search/_static/image7.png)

## <a name="adding-search-by-genre"></a>Dodawanie wyszukiwania według gatunku

Jeśli dodano `HttpPost` wersję `Index` metody, usuń ją teraz.

Następnie dodasz funkcję, aby umożliwić użytkownikom wyszukiwanie filmów według gatunku. Zastąp `Index` metoda następującym kodem:

[!code-csharp[Main](adding-search/samples/sample11.cs)]

Ta wersja `Index` metoda przyjmuje dodatkowy parametr, to znaczy `movieGenre`. Tworzenie pierwszych kilka wierszy kodu `List` obiekt do przechowywania gatunki film z bazy danych.

Poniższy kod jest zapytanie LINQ, która pobiera wszystkie gatunki z bazy danych.

[!code-csharp[Main](adding-search/samples/sample12.cs)]

Kod używa `AddRange` metody ogólnej `List` kolekcję, aby dodać różne gatunki do listy. (Bez `Distinct` modyfikator, zostaną dodane zduplikowane gatunki — na przykład, zostaną dodane Komedia dwukrotnie w naszym przykładzie). Kod następnie przechowuje listę gatunki w `ViewBag.MovieGenre` obiektu. Przechowywanie danych kategorii (takie gatunku filmu firmy) jako [SelectList](https://msdn.microsoft.cus/library/system.web.mvc.selectlist(v=vs.108).aspx) obiektu `ViewBag`, uzyskiwanie dostępu do danych kategorii, w polu listy rozwijanej jest typowym podejściem dla aplikacji MVC.

Poniższy kod przedstawia sposób sprawdzić `movieGenre` parametru. Jeśli nie jest pusty, kod dodatkowo ogranicza zapytanie filmy, aby ograniczyć wybranych filmów na określonego rodzaju.

[!code-csharp[Main](adding-search/samples/sample13.cs)]

Jak wspomniano wcześniej, zapytania nie jest uruchamiane na bazę danych, aż Lista filmów jest powtarzana (który sytuacja występuje w widoku `Index` metoda akcji zwraca).

## <a name="adding-markup-to-the-index-view-to-support-search-by-genre"></a>Dodawanie znaczników do widoku indeksu, aby obsługiwać wyszukiwanie według gatunku

Dodaj `Html.DropDownList` element pomocniczy służący do *Views\Movies\Index.cshtml* pliku tuż przed `TextBox` pomocnika. Ukończone znaczników jest pokazany poniżej:

[!code-cshtml[Main](adding-search/samples/sample14.cshtml?highlight=11)]

W poniższym kodzie:

[!code-cshtml[Main](adding-search/samples/sample15.cshtml)]

Parametr "MovieGenre" udostępnia klucz dla `DropDownList` pomocnika, aby znaleźć `IEnumerable<SelectListItem>` w `ViewBag`. `ViewBag` Zostało wypełnione w metodzie akcji:

[!code-csharp[Main](adding-search/samples/sample16.cs?highlight=10)]

Parametr "All" zawiera etykiety opcji. Jeśli wybór sprawdzić się w przeglądarce, zobaczysz, że jego atrybut "value" jest pusty. Ponieważ kontrolera tylko filtry `if` ciąg nie jest `null` lub jest pusta, pusta wartość dla przesyłania `movieGenre` pokazuje wszystkie gatunki.

Można również ustawić opcję, aby domyślnie zaznaczone. Jeśli chce się "Dokument" jako opcjach domyślny, należy zmienić kod w kontrolerze w następujący sposób:

[!code-cshtml[Main](adding-search/samples/sample17.cshtml)]

Uruchom aplikację, a następnie przejdź do */filmy/indeks*. Spróbuj wyszukiwania według gatunku, tytuł filmu i oba kryteria.

![](adding-search/_static/image8.png)

W tej sekcji zostały utworzone metody akcji wyszukiwania i widoku, które umożliwiają użytkownikom wyszukiwanie tytuł filmu i gatunku. W następnej sekcji zostanie przyjrzymy się sposób dodawania właściwości do `Movie` modelu oraz sposób dodawania inicjatora, które automatycznie utworzy test bazy danych.

> [!div class="step-by-step"]
> [Poprzednie](examining-the-edit-methods-and-edit-view.md)
> [dalej](adding-a-new-field.md)
