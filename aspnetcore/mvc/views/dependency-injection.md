---
title: Wstrzykiwanie zależności do widoków w programie ASP.NET Core
author: ardalis
description: Dowiedz się, jak platforma ASP.NET Core obsługuje wstrzykiwanie zależności do widoków MVC.
ms.author: riande
ms.date: 10/14/2016
uid: mvc/views/dependency-injection
ms.openlocfilehash: b411b164bfea81f82c5c9fc1052e0ecfe65f0bc2
ms.sourcegitcommit: 3376f224b47a89acf329b2d2f9260046a372f924
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/10/2019
ms.locfileid: "65517051"
---
# <a name="dependency-injection-into-views-in-aspnet-core"></a>Wstrzykiwanie zależności do widoków w programie ASP.NET Core

Przez [Steve Smith](https://ardalis.com/)

Obsługuje platformy ASP.NET Core [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) do widoków. Może to być przydatne w przypadku usługi specyficzne dla widoku, takie jak lokalizacja lub wymagane tylko w przypadku wypełnianie elementy widoku danych. Należy starać się utrzymać [separacji](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) między widoków i kontrolerów. Większość danych, których wyświetlanie widoków powinien być przekazywany w z kontrolera.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/views/dependency-injection/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))

## <a name="configuration-injection"></a>Iniekcja konfiguracji

*appSettings.JSON* wartości może wprowadzone bezpośrednio w widoku.

Przykład *appsettings.json* pliku:

```json
{
   "root": {
      "parent": {
         "child": "myvalue"
      }
   }
}
```

Składnia `@inject`: `@inject <type> <name>`

Przykłady dotyczące używania `@inject`:

```csharp
@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration
@{
   string myValue = Configuration["root:parent:child"];
   ...
}
```

## <a name="service-injection"></a>Uruchomienie usługi

Usługa może zostać wprowadzona w widoku, używając `@inject` dyrektywy. Można potraktować `@inject` Dodawanie właściwości do widoku i wypełnienie właściwości, używając DI.

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/ToDo/Index.cshtml?highlight=4,5,15,16,17)]

Ten widok przedstawia listę `ToDoItem` wystąpień, wraz z podsumowaniem przedstawiająca ogólne statystyki. Podsumowanie jest wypełniana od wprowadzonego `StatisticsService`. Ta usługa jest zarejestrowana dla wstrzykiwanie zależności w `ConfigureServices` w *Startup.cs*:

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Startup.cs?highlight=6,7&range=15-22)]

`StatisticsService` Wykonywania niektórych obliczeń dla zestawu `ToDoItem` wystąpień, które uzyskuje dostęp za pośrednictwem repozytorium:

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/StatisticsService.cs?highlight=15,20,25)]

Przykładowe repozytorium korzysta z kolekcji w pamięci. Implementacja powyżej (który działa na wszystkich danych w pamięci) nie jest zalecane w przypadku dużych, zdalny dostęp do zestawów danych.

Przykład wyświetla dane z modelu powiązany z widoku i usługi, które są wstrzykiwane do widoku:

![Aby wyświetlić listę łączna liczba elementów ukończone elementy, średni priorytet i Lista zadań wraz z ich poziomy priorytetów i wartości logiczne, wskazując ukończenia.](dependency-injection/_static/screenshot.png)

## <a name="populating-lookup-data"></a>Wypełnianie danych wyszukiwania

Iniekcja widok może być przydatne do wypełniania opcje w elementach interfejsu użytkownika, takich jak listy rozwijanej. Należy wziąć pod uwagę formularz profilu użytkownika, która obejmuje opcje określania płeć, stan i inne preferencje. Renderowanie formularza przy użyciu standardowego podejścia MVC wymagałoby kontrolera w celu żądania usługi dostępu do danych dla każdego z tych zestawów opcji, a następnie wypełnij modelu lub `ViewBag` z każdym zestawem opcji, aby powiązać.

Alternatywne podejście wprowadza services bezpośrednio w widoku, aby uzyskać opcje. Zmniejsza to ilość kodu wymaganą przez kontroler przeniesienie tę logikę budowy elementu widoku do samego widoku. Akcji kontrolera, aby wyświetlić formularz edycji profilu wystarczy przekazać formularz wystąpienia profilu:

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Controllers/ProfileController.cs?highlight=9,19)]

Formularza HTML, używane do aktualizowania tych preferencji zawiera listy rozwijane dla trzech właściwości:

![Zaktualizuj widoku profilu użytkownika z formularzem, umożliwiając wprowadzanie nazwy, płeć, stanu i ulubionych kolorów.](dependency-injection/_static/updateprofile.png)

Te listy są wypełniane przez usługę, która ma został wprowadzony w widoku:

[!code-cshtml[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Profile/Index.cshtml?highlight=4,16,17,21,22,26,27)]

`ProfileOptionsService` Jest usługą poziomu interfejsu użytkownika, zaprojektowana w celu zapewnienia tylko dane, które są wymagane dla tego formularza:

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/ProfileOptionsService.cs?highlight=7,13,24)]

> [!IMPORTANT]
> Nie należy zapominać zarejestrować typy żądań za pośrednictwem wstrzykiwanie zależności w `Startup.ConfigureServices`. Niezarejestrowany typ zgłasza wyjątek w czasie wykonywania, ponieważ usługodawcy wewnętrznie otrzymaniu kwerendy za pośrednictwem [GetRequiredService](/dotnet/api/microsoft.extensions.dependencyinjection.serviceproviderserviceextensions.getrequiredservice).

## <a name="overriding-services"></a>Zastępowanie usług

Oprócz wprowadza nowe usługi, ta technika może również zastąpić uprzednio wprowadzony usług na stronie. Na poniższym rysunku przedstawiono wszystkie pola, które są dostępne na stronie używany w pierwszym przykładzie:

![Menu kontekstowe funkcji IntelliSense na typizowaną listę pól Html, składnika, StatsService i adres Url symbol @](dependency-injection/_static/razor-fields.png)

Jak widać, pól domyślnych obejmują `Html`, `Component`, i `Url` (także `StatsService` , firma Microsoft wprowadzony). Jeśli na przykład chcesz zastąpić domyślny pomocników HTML swoją własną, użytkownik może łatwo to zrobić za pomocą `@inject`:

[!code-cshtml[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Helper/Index.cshtml?highlight=3,11)]

Jeśli chcesz rozszerzyć istniejące usługi, możesz po prostu użyć tej techniki podczas dziedziczących lub zawijania istniejącego wdrożenia za pomocą własnych.

## <a name="see-also"></a>Zobacz też

* Simon Timms Blog: [Pobieranie danych wyszukiwania do widoku](http://blog.simontimms.com/2015/06/09/getting-lookup-data-into-you-view/)
