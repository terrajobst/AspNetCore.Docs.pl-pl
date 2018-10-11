---
uid: web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
title: Wprowadzenie do wzorca ASP.NET Web API 2 (C#)
author: MikeWasson
description: Protokół HTTP nie jest używany tylko do obsługi stron internetowych. Ponadto jest to zaawansowana platforma do tworzenia interfejsów API, które udostępniają dane i usługi. Protokół HTTP jest proste, elastyczne i ubiq...
ms.author: riande
ms.date: 11/28/2017
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
msc.type: authoredcontent
ms.openlocfilehash: 62e99a41ba935470c39476c9aea8ee4193543425
ms.sourcegitcommit: 7890dfb5a8f8c07d813f166d3ab0c263f893d0c6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/04/2018
ms.locfileid: "48795296"
---
<a name="get-started-with-aspnet-web-api-2-c"></a>Wprowadzenie do wzorca ASP.NET Web API 2 (C#)
====================
przez [Mike Wasson](https://github.com/MikeWasson)

[Pobierz ukończony projekt](https://code.msdn.microsoft.com/Sample-code-of-Getting-c56ccb28)

Protokół HTTP nie jest używany tylko do obsługi stron internetowych. Protokół HTTP to również zaawansowana platforma do tworzenia interfejsów API, które udostępniają dane i usługi. Protokół HTTP jest prosty, elastyczny i uniwersalny. Prawie każda platforma obsługuje bibliotekę HTTP, więc usługi HTTP mogą być stosowane dla szerokiej gamy klientów takich jak przeglądarki, urządzenia przenośne czy tradycyjne aplikacje komputerowe.

Internetowy interfejs API platformy ASP.NET to architektura służąca do tworzenia internetowych interfejsów API w programie .NET Framework. W tym samouczku użyjesz internetowego interfejsu API platformy ASP.NET do tworzenia internetowego interfejsu API, który zwraca listę produktów.

## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używanego w tym samouczku

- [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
- Internetowy interfejs API 2

Aby zapoznać się z nowszą wersją tego samouczka, zobacz [Tworzenie internetowego interfejsu API za pomocą platformy ASP.NET Core i programu Visual Studio dla systemu Windows](https://docs.microsoft.com/aspnet/core/tutorials/first-web-api).

## <a name="create-a-web-api-project"></a>Utwórz projekt internetowego interfejsu API

W tym samouczku użyjesz internetowego interfejsu API platformy ASP.NET do tworzenia internetowego interfejsu API, który zwraca listę produktów. Strony sieci web frontonu używa jQuery, aby wyświetlić wyniki.

![](tutorial-your-first-web-api/_static/image1.png)

Uruchom program Visual Studio i wybierz pozycję **Nowy projekt** na stronie **Start**. Możesz również z menu **Plik** wybrać pozycję **Nowy**, a następnie **Projekt**.

W okienku **Szablony** wybierz pozycję **Zainstalowane szablony** i rozwiń węzeł **Visual C#**. W obszarze **Visual C#**, wybierz pozycję **Sieć Web**. Na liście szablonów projektu wybierz **Aplikacja internetowa ASP.NET**. Nadaj projektowi nazwę "ProductsApp", a następnie kliknij przycisk **OK**.

![](tutorial-your-first-web-api/_static/image2.png)

W oknie dialogowym **Nowy projekt ASP.NET** wybierz szablon **Pusty**. W obszarze &quot;Dodaj foldery i podstawowe odwołania dla&quot; zaznacz opcję **Internetowy interfejs API** Kliknij przycisk **OK**.

![](tutorial-your-first-web-api/_static/image3.png)

> [!NOTE]
> Można również utworzyć projekt internetowego interfejsu API za pomocą szablonu &quot;Internetowy interfejs API&quot;. Szablon internetowego interfejsu API używa platformy ASP.NET MVC do udostępniania stron pomocy do interfejsu API. Używam pustego szablonu na potrzeby tego samouczka, ponieważ chcę pokazać internetowy interfejs API bez platformy MVC. Ogólnie rzecz biorąc, nie trzeba znać platformy ASP.NET MVC, aby korzystać z internetowego interfejsu API.


## <a name="adding-a-model"></a>Dodawanie modelu

*Model* jest obiektem, który reprezentuje dane w aplikacji. Internetowy interfejs API platformy ASP.NET może automatycznie zserializować model do formatu JSON, XML lub innego, a następnie wpisać te dane serializowane w treści komunikatu odpowiedzi HTTP. Jeśli klient może odczytać format serializacji, może również wykonywać deserializację obiektu. Większość klientów może analizować albo format XML, albo JSON. Ponadto klient może wskazać pożądany format, ustawiając nagłówek Accept w komunikacie żądania HTTP.

Zacznijmy od utworzenia prostego modelu, który reprezentuje produkt.

Jeśli Eksplorator rozwiązań nie jest jeszcze widoczny, kliknij menu **Widok**, a następnie wybierz pozycję **Eksplorator rozwiązań**. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy folder Modele. Z menu kontekstowego wybierz pozycję **Dodaj**, a następnie **Klasa**.

![](tutorial-your-first-web-api/_static/image4.png)

Nazwij klasę &quot;Product&quot;. Dodaj następujące właściwości do klasy `Product`.

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample1.cs)]

## <a name="adding-a-controller"></a>Dodawanie kontrolera

W internetowym interfejsie API *kontroler* jest obiektem, który obsługuje żądania HTTP. Dodamy kontroler, który może zwrócić listę produktów lub pojedynczy produkt określony przez identyfikator.

> [!NOTE]
> Jeśli używasz platformy ASP.NET MVC, znasz już kontrolery. W internetowym interfejsie API kontrolery są podobne do kontrolerów MVC, ale dziedziczą klasę **ApiController** zamiast klasy **Controller**.

W **Eksploratorze rozwiązań** kliknij prawym przyciskiem myszy folder kontrolerów (Controllers). Wybierz pozycję **Dodaj**, a następnie **Kontroler**.

![](tutorial-your-first-web-api/_static/image5.png)

W oknie dialogowym **Dodawanie szkieletu** wybierz pozycję **Kontroler Web API 2 — pusty**. Kliknij przycisk **Dodaj**.

![](tutorial-your-first-web-api/_static/image6.png)

W oknie dialogowym **Dodawanie kontrolera** nazwij kontrolera &quot;ProductsController&quot;. Kliknij przycisk **Dodaj**.

![](tutorial-your-first-web-api/_static/image7.png)

Funkcja tworzenia szkieletów utworzy plik o nazwie ProductsController.cs w folderze kontrolerów.

![](tutorial-your-first-web-api/_static/image8.png)

> [!NOTE]
> Kontroler nie musi być umieszczany w folderze kontrolerów (Controllers). Nazwy folderów ułatwiają tylko organizowanie plików źródłowych.


Jeśli ten plik nie jest jeszcze otwarty, kliknij dwukrotnie, aby go otworzyć. Zastąp kod w tym pliku zgodnie z poniższym przykładem:

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample2.cs)]

W tym przykładzie w celu uproszczenia produkty są przechowywane w stałej tablicy wewnątrz klasy kontrolera. Oczywiście w rzeczywistej aplikacji przesyłane jest zapytanie do bazy danych lub innego zewnętrznego źródła danych.

Kontroler definiuje dwie metody, które zwracają produkty:

- Metoda `GetAllProducts` zwraca całą listę produktów jako typ **IEnumerable&lt;produkt&gt;**.
- `GetProduct` Metoda odwołuje się do jednego produktu za pomocą jego identyfikatora.

To wszystko! Masz działający internetowy interfejs API. Każda metoda na kontrolerze odnosi się do co najmniej jednego identyfikatora URI:

| Metoda kontrolera | Identyfikator URI |
| --- | --- |
| GetAllProducts | / api/produktów |
| GetProduct | / InterfejsAPI/produkty/*identyfikator* |

W przypadku metody `GetProduct` wartość *id* w identyfikatorze URI jest symbolem zastępczym. Na przykład, aby uzyskać produkt o identyfikatorze 5, użyj identyfikatora URI:`api/products/5`.

Aby uzyskać więcej informacji na temat sposobu kierowania żądań HTTP internetowego interfejsu API do metod kontrolera, zobacz [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md) (Routing w internetowym interfejsie API platformy ASP.NET).

## <a name="calling-the-web-api-with-javascript-and-jquery"></a>Wywoływanie interfejsu API sieci Web za pomocą języka Javascript i jQuery

W tej sekcji dodamy stronę HTML używającą technologii AJAX do wywołania internetowego interfejsu API. Użyjemy technologii jQuery do wykonania wywołań AJAX oraz do zaktualizowania wyników na stronie.

W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj**, a następnie wybierz **nowy element**.

![](tutorial-your-first-web-api/_static/image9.png)

W oknie dialogowym **Dodawanie nowego elementu** wybierz węzeł **Sieć Web** w kategorii **Visual C#**, a następnie wybierz element **Strona HTML**. Nazwij stronę &quot;index.html&quot;.

![](tutorial-your-first-web-api/_static/image10.png)

Zamień zawartość pliku na następującą:

[!code-html[Main](tutorial-your-first-web-api/samples/sample3.html)]

Istnieje kilka sposobów uzyskania biblioteki jQuery. W tym przykładzie użyto w tym celu usługi [Microsoft Ajax CDN](../../../ajax/cdn/overview.md). Można również pobrać ją ze strony [ http://jquery.com/ ](http://jquery.com/) i jest zawarta w szablonie projektu „Internetowy interfejs API” platformy ASP.NET.

### <a name="getting-a-list-of-products"></a>Pobieranie listy produktów

Aby uzyskać listę produktów, Wyślij żądanie HTTP GET do &quot;/api/produktów&quot;.

Funkcja JQuery [getJSON](http://api.jquery.com/jQuery.getJSON/) wysyła żądanie AJAX. Odpowiedź zawiera tablicę obiektów JSON. Funkcja `done` określa wywołanie zwrotne, które jest wywoływane, jeśli żądanie zakończy się pomyślnie. W wywołaniu zwrotnym aktualizujemy model DOM informacjami o produkcie.

[!code-html[Main](tutorial-your-first-web-api/samples/sample4.html)]

### <a name="getting-a-product-by-id"></a>Pobieranie produktu według Identyfikatora

Aby uzyskać produkt o określonym identyfikatorze, wyślij żądanie HTTP GET do &quot;//api/products/*id*&quot;, gdzie *id* jest identyfikatorem produktu.

[!code-javascript[Main](tutorial-your-first-web-api/samples/sample5.js)]

Wciąż wywołujemy funkcję `getJSON` do wysyłania żądania AJAX, ale tym razem wstawiamy identyfikator do identyfikatora URI żądania. Odpowiedź na to żądanie jest reprezentacją JSON jednego produktu.

## <a name="running-the-application"></a>Uruchamianie aplikacji

Naciśnij klawisz F5, aby rozpocząć debugowanie aplikacji. Strona internetowa powinna wyglądać następująco:

![](tutorial-your-first-web-api/_static/image11.png)

Aby uzyskać produkt za pomocą Identyfikatora, wprowadź identyfikator, a następnie kliknij przycisk wyszukiwania:

![](tutorial-your-first-web-api/_static/image12.png)

Jeśli wprowadzisz nieprawidłowy identyfikator, serwer zwraca błąd HTTP:

![](tutorial-your-first-web-api/_static/image13.png)

## <a name="using-f12-to-view-the-http-request-and-response"></a>Używanie klawisza F12 do wyświetlania żądania i odpowiedzi HTTP

W przypadku korzystania z usługi HTTP bardzo przydatne może być wyświetlenie żądania HTTP i jego komunikatów. Można to zrobić w programie Internet Explorer 9 przy użyciu narzędzi deweloperskich uruchamianych za pomocą klawisza F12. W tym celu w programie Internet Explorer 9 naciśnij klawisz **F12**, aby otworzyć narzędzia. Następnie kliknij kartę **Sieć** i wybierz pozycję **Rozpocznij przechwytywanie**. Teraz wróć do strony internetowej i naciśnij klawisz ***F5**, aby ponowne załadować stronę. Program Internet Explorer będzie przechwytywać ruch HTTP między przeglądarką a serwerem internetowym. Widok podsumowania przedstawia cały ruch sieciowy dla strony:

![](tutorial-your-first-web-api/_static/image14.png)

Zlokalizuj wpis dla względnego identyfikatora URI "api/products//". Wybierz ten wpis, a następnie kliknij przycisk **Przejdź do widoku szczegółowego**. Widok szczegółowy zawiera karty, na których są wyświetlane nagłówki i zawartość żądań oraz odpowiedzi. Na przykład po kliknięciu karty **Nagłówki żądań** możesz zobaczyć, że klient zażądał wartości &quot;application/json&quot; w nagłówku Accept.

![](tutorial-your-first-web-api/_static/image15.png)

Po kliknięciu karty Treść odpowiedzi możesz zobaczyć, jak lista produktów została zserializowana do formatu JSON. Inne przeglądarki mają podobną funkcję. Kolejnym przydatnym narzędziem jest [Fiddler](http://www.fiddler2.com/fiddler2/) — internetowy serwer proxy do debugowania. Służy on do wyświetlania ruchu HTTP, a także do tworzenia żądań HTTP, co daje pełną kontrolę nad nagłówkami HTTP w żądaniu.

## <a name="see-this-app-running-on-azure"></a>Wyświetlanie aplikacji działającej na platformie Azure

Czy chcesz zobaczyć gotową witrynę działającą jako aplikacja internetowa na żywo? Pełną wersję aplikacji możesz wdrożyć na koncie platformy Azure — wystarczy kliknąć poniższy przycisk.

[![](http://azuredeploy.net/deploybutton.png)](https://deploy.azure.com/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/WebAPI-ProductsApp#/form/setup)

Aby wdrożyć to rozwiązanie na platformie Azure, musisz mieć konto platformy Azure. Jeśli nie masz jeszcze konta, możesz:

- [Otworzyć bezpłatne konto platformy Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) — otrzymujesz środki , które możesz użyć do wypróbowania płatnych usług platformy Azure, a potem nawet w przypadku ich wyczerpania możesz zachować konto i korzystać z bezpłatnych usług platformy Azure.
- [Aktywować korzyści dla subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - w ramach subskrypcji MSDN każdego miesiąca dostajesz środki, dzięki którym możesz korzystać z płatnych usług platformy Azure.

## <a name="next-steps"></a>Następne kroki

- Aby zapoznać się z bardziej szczegółowym przykładem usługi HTTP, która obsługuje operacje POST, PUT i DELETE oraz zapisuje je do bazy danych, zobacz [Using Web API 2 with Entity Framework 6](../data/using-web-api-with-entity-framework/part-1.md) (Używanie interfejsu Web API 2 z programem Entity Framework 6).
- Aby uzyskać więcej informacji o tworzeniu płynnie działających i responsywnych  aplikacji internetowych na podstawie usługi HTTP, zobacz [ASP.NET Single Page Application](../../../single-page-application/index.md) (Aplikacja jednostronicowa platformy ASP.NET).
- Aby uzyskać informacje o sposobie wdrażania projektu sieci web programu Visual Studio w usłudze Azure App Service, zobacz [tworzenie aplikacji sieci web platformy ASP.NET w usłudze Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).
