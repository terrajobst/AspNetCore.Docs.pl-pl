---
title: "Iniekcji zależności do widoków"
author: ardalis
description: 
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/dependency-injection
ms.openlocfilehash: cade61b1ebdb2b845b07117384475638c0227f7f
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="dependency-injection-into-views"></a>Iniekcji zależności do widoków

Przez [Steve Smith](https://ardalis.com/)

Obsługuje platformy ASP.NET Core [iniekcji zależności](xref:fundamentals/dependency-injection) do widoków. Może to być przydatne w przypadku usług specyficzne dla widoku, takich jak lokalizacja lub wymagane tylko w przypadku wypełnianie elementy widoku danych. Staraj się zachować [separacji](http://deviq.com/separation-of-concerns/) między widoków i kontrolerów. Większość danych, które widoków wyświetlić powinien zostać przekazany w z kontrolera.

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/dependency-injection/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

## <a name="a-simple-example"></a>Prosty przykład

Usługi można wstawić do widoku przy użyciu `@inject` dyrektywy. Można potraktować `@inject` jako Dodawanie właściwości do widoku i wypełniania właściwości, używając Podpisane.

Składnia `@inject`:`@inject <type> <name>`

Przykład `@inject` w akcji:

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/ToDo/Index.cshtml?highlight=4,5,15,16,17)]

Ten widok przedstawia listę `ToDoItem` wystąpień, wraz z podsumowaniem przedstawiający ogólne statystyki. Podsumowanie jest wypełniana z wprowadzonym `StatisticsService`. Ta usługa jest zarejestrowana iniekcji zależności w `ConfigureServices` w *Startup.cs*:

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Startup.cs?highlight=6,7&range=15-22)]

`StatisticsService` Wykonuje obliczenia na zbiór `ToDoItem` wystąpienia, które uzyskuje dostęp do za pośrednictwem repozytorium:

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/StatisticsService.cs?highlight=15,20,26)]

Repozytorium przykładowej korzysta z kolekcji w pamięci. Implementacja pokazanym powyżej (który działa na wszystkich danych w pamięci) nie jest zalecane dla dużych, zdalny dostęp do zestawów danych.

Próbki są wyświetlane dane z modelu, powiązany z widoku i usługi do widoku:

![Aby wyświetlić listę całkowita liczba elementów ukończone elementy, średni priorytet i Lista zadań z ich poziomy priorytetu i wskazujący ukończenia wartości logiczne.](dependency-injection/_static/screenshot.png)

## <a name="populating-lookup-data"></a>Wypełnianie wyszukiwanie danych

Widok iniekcji mogą być przydatne do wypełniania opcji w elementów interfejsu użytkownika, takich jak list rozwijanych. Należy wziąć pod uwagę formularz profilu użytkownika, która obejmuje opcje określenie płci, stanu i inne preferencje. Renderowanie formularza przy użyciu standardowej metody MVC wymaga kontrolera w celu żądania usługi dostępu do danych dla każdego z tych zestawów opcje, a następnie wypełnij modelu lub `ViewBag` każdy zestaw opcji może być powiązane.

Informacje o innym podejściu injects usług bezpośrednio w widoku, aby uzyskać odpowiednie opcje. Pozwala to zmniejszyć ilość kodu wymagane przez administratora, przeniesienie tego widoku elementu konstrukcji logiki do samego widoku. Akcji kontrolera do wyświetlania formularza edycji profilu musi tylko Przekaż wystąpienie profilu formularza:

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Controllers/ProfileController.cs?highlight=9,19)]

Formularz HTML używane do aktualizowania tych preferencji zawiera list rozwijanych trzy właściwości:

![Aktualizowanie widoku profilu dla formularza, umożliwiając wpis nazwy, płci, stanu i kolor ulubionych.](dependency-injection/_static/updateprofile.png)

Te listy są wypełniane przez usługę, które zostały dodane do widoku:

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Profile/Index.cshtml?highlight=4,16,17,21,22,26,27)]

`ProfileOptionsService` Jest usługą interfejsu użytkownika na poziomie zapewnia tylko te dane, które są wymagane dla tego formularza:

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/ProfileOptionsService.cs?highlight=7,13,24)]

>[!TIP]
> Nie zapomnij zarejestrować typy będzie żądać za pomocą iniekcji zależności w `ConfigureServices` metody w *Startup.cs*.

## <a name="overriding-services"></a>Zastępowanie usług

Oprócz wstrzyknięcie nowych usług, ta metoda mogą służyć do zastępowania poprzednio wprowadzony usług na stronie. Na poniższej ilustracji przedstawiono wszystkie pola, które są dostępne na stronie używany w pierwszym przykładzie:

![Menu kontekstowe IntelliSense na typu wyświetlania pola Html, składnik StatsService i adres Url symbol @](dependency-injection/_static/razor-fields.png)

Jak widać, domyślne pola zawierają `Html`, `Component`, i `Url` (a także `StatsService` który możemy wprowadzić). Jeśli na przykład chcesz zastąpić domyślne pomocników HTML własnym, możesz łatwo to zrobić przy użyciu `@inject`:

[!code-html[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Helper/Index.cshtml?highlight=3,11)]

Jeśli chcesz rozszerzyć istniejących usług, po prostu służy ta technika podczas dziedziczenia z lub zawijania istniejącej implementacji własnymi.

## <a name="see-also"></a>Zobacz też

* Blog Timms Simona: [pobierania danych odnośników do widoku](http://blog.simontimms.com/2015/06/09/getting-lookup-data-into-you-view/)
