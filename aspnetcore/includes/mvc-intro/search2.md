---
ms.openlocfilehash: b90767ce8e3d703cc5e3144feb07bdf5cd519775
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58207885"
---
<!--
[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull)]

![Index view](~/tutorials/first-mvc-app/search/_static/ghost.png)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

--> 

Poprzedni `Index` metody:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1,8&name=snippet_1stSearch)]

Zaktualizowany interfejs `Index` metody z `id` parametru:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1,8&name=snippet_SearchID)]

Tytuł wyszukiwania można teraz przekazywać jako dane trasy (segment adresu URL) zamiast jako wartość ciągu zapytania.

![Widok indeksu z ghost word dodawany do adresu Url i zwrócony filmu listę filmów Ghostbusters i Ghostbusters 2](~/tutorials/first-mvc-app/search/_static/g2.png)

Jednak nie można oczekiwać od użytkowników, aby zmodyfikować adres URL, za każdym razem, gdy chcą wyszukiwania filmów. Teraz należy dodać elementy interfejsu użytkownika, aby pomóc im filtrowanie filmów. Jeśli zmienisz podpis `Index` metodę, aby przetestować sposób przekazywania trasy powiązanym `ID` parametr, zmień je z powrotem, aby przyspieszyć parametr o nazwie `searchString`:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1&name=snippet_1stSearch)]

Otwórz *Views/Movies/Index.cshtml* pliku i Dodaj `<form>` znaczników, które przedstawiono poniżej:

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexForm1.cshtml?highlight=10-16&range=4-21)]

Kod HTML `<form>` tagów używa [Pomocnik tagu formularza](xref:mvc/views/working-with-forms), więc gdy prześlesz formularz, aby opublikowaniu ciąg filtru `Index` akcji kontrolera filmów. Zapisz zmiany, a następnie przetestować filtr.

![Widok indeksu z ghost słowa wpisane w polu tekstowym filtru tytułu](~/tutorials/first-mvc-app/search/_static/filter.png)

Istnieje nie `[HttpPost]` przeciążenia `Index` metody można by oczekiwać. Nie są potrzebne, ponieważ metoda nie jest zmiana stanu aplikacji, po prostu filtrowania danych.

Można dodać następujące `[HttpPost] Index` metody.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1&name=snippet_SearchPost)]

`notUsed` Parametr jest używany do tworzenia przeciążenie dla elementu `Index` metody. Omówimy, w dalszej części tego samouczka.

Jeśli dodasz tej metody, wywołujący akcji będzie odpowiadać `[HttpPost] Index` metody i `[HttpPost] Index` metoda może działać, jak pokazano na poniższej ilustracji.

![Okno przeglądarki z odpowiedzi aplikacji z indeksu HttpPost: Filtr ghost](~/tutorials/first-mvc-app/search/_static/fo.png)

Jednak nawet w przypadku dodania to `[HttpPost]` wersję `Index` metody, jest to ograniczenie, w jak to wszystko została zaimplementowana. Wyobraź sobie, że chcesz utworzyć zakładkę określonego wyszukiwania lub chcesz wysłać link do znajomych, mogą kliknąć umożliwiający zobaczenie tej samej listy filtrowane filmów. Należy zauważyć, że adres URL żądania HTTP POST jest taki sam jak adres URL dla żądania GET (localhost:xxxxx/filmy/indeksu) — Brak wyszukiwania informacji w adresie URL. Informacje o parametrach wyszukiwania są wysyłane do serwera jako [stanowią wartość pola](https://developer.mozilla.org/docs/Learn/HTML/Forms/Sending_and_retrieving_form_data). Możesz sprawdzić, czy za pomocą narzędzia deweloperskie przeglądarki lub doskonałą [narzędzie Fiddler](http://www.telerik.com/fiddler). Na poniższym obrazie przedstawiono narzędzia deweloperskie przeglądarki Chrome:

![Karta Sieć narzędzi dla deweloperów w programie Microsoft Edge wyświetlanie treść żądania z wartością Ciągwyszukiwania ghost](~/tutorials/first-mvc-app/search/_static/f12_rb.png)

Możesz zobaczyć parametru wyszukiwania i [XSRF](xref:security/anti-request-forgery) tokenu w treści żądania. Należy pamiętać, zgodnie z opisem w poprzednim samouczku [Pomocnik tagu formularza](xref:mvc/views/working-with-forms) generuje [XSRF](xref:security/anti-request-forgery) token zabezpieczający przed sfałszowaniem. Dane, nie masz zostać zmodyfikowana, więc nie potrzebujemy przeprowadzić walidacji tokenu w metodzie kontrolera.

Ponieważ parametr wyszukiwania znajduje się w treści żądania, a nie jej adres URL, nie można przechwycić informacje wyszukiwania do zakładki lub udostępnienia innym osobom. Firma Microsoft będzie rozwiązać ten problem, określając żądanie powinno być `HTTP GET`.
