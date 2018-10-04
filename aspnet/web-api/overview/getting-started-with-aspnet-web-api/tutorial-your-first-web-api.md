---
uid: web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
title: Wprowadzenie do wzorca ASP.NET Web API 2 (C#)
author: MikeWasson
description: Protokół HTTP nie jest dostępne tylko dla wysyłaniu stron sieci web. Ponadto jest to zaawansowana platforma do tworzenia interfejsów API, które udostępniają dane i usługi. Protokół HTTP jest proste, elastyczne i ubiq...
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

[Pobieranie ukończone projektu](https://code.msdn.microsoft.com/Sample-code-of-Getting-c56ccb28)

Protokół HTTP nie jest dostępne tylko dla wysyłaniu stron sieci web. Protokół HTTP, również jest to zaawansowana platforma do tworzenia interfejsów API, które udostępniają dane i usługi. Protokół HTTP jest proste, elastyczne i uniwersalnych. Niemal dowolną platformą, która można traktować ma biblioteki HTTP, więc usług HTTP można dotrzeć do szerokiej gamy klientów, w tym przeglądarek i urządzeń przenośnych, tradycyjne aplikacje komputerowe.

Web API platformy ASP.NET to architektura służąca do tworzenia interfejsów API w programie .NET Framework w sieci web. W tym samouczku użyjesz interfejsu API sieci Web platformy ASP.NET do tworzenia internetowego interfejsu API, które zwraca listę produktów.

## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używanego w tym samouczku

- [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
- Składnik Web API 2

Zobacz [Tworzenie internetowego interfejsu API za pomocą platformy ASP.NET Core i Visual Studio for Windows](https://docs.microsoft.com/aspnet/core/tutorials/first-web-api) jest dostępna nowsza wersja tego samouczka.

## <a name="create-a-web-api-project"></a>Utwórz projekt internetowego interfejsu API

W tym samouczku użyjesz interfejsu API sieci Web platformy ASP.NET do tworzenia internetowego interfejsu API, które zwraca listę produktów. Strony sieci web frontonu używa jQuery, aby wyświetlić wyniki.

![](tutorial-your-first-web-api/_static/image1.png)

Uruchom program Visual Studio i wybierz **nowy projekt** z **Start** strony. Lub z **pliku** menu, wybierz opcję **New** i następnie **projektu**.

W **szablony** okienku wybierz **zainstalowane szablony** i rozwiń **Visual C#** węzła. W obszarze **Visual C#**, wybierz opcję **Web**. Na liście szablonów projektu wybierz **aplikacji sieci Web ASP.NET**. Nadaj projektowi nazwę "ProductsApp", a następnie kliknij przycisk **OK**.

![](tutorial-your-first-web-api/_static/image2.png)

W **nowy projekt ASP.NET** okno dialogowe, wybierz opcję **pusty** szablonu. W obszarze &quot;. Dodaj foldery i podstawowe odwołania dla&quot;, sprawdź **interfejsu API sieci Web**. Kliknij przycisk **OK**.

![](tutorial-your-first-web-api/_static/image3.png)

> [!NOTE]
> Można również utworzyć projekt interfejsu API sieci Web za pomocą &quot;interfejsu API sieci Web&quot; szablonu. Szablon interfejsu API sieci Web używa platformy ASP.NET MVC w celu zapewnienia stron pomocy interfejsu API. Używam pustego szablonu na potrzeby tego samouczka ponieważ chcę pokazać interfejs API sieci Web bez MVC. Ogólnie rzecz biorąc nie trzeba znać platformy ASP.NET MVC w celu korzystania z interfejsu API sieci Web.


## <a name="adding-a-model"></a>Dodawanie modelu

A *modelu* jest obiektem, który reprezentuje dane w aplikacji. ASP.NET Web API może automatycznie serializuj model do formatu JSON, XML lub innym formacie, a następnie wpisz dane serializowane w treści komunikatu odpowiedzi HTTP. Tak długo, jak długo klient może odczytywać format serializacji, może wykonywać deserializację obiektu. Większość klientów może przeanalizować, XML lub JSON. Ponadto klient może wskazywać formatu, który chce, ustawiając nagłówek Accept w komunikacie żądania HTTP.

Zacznijmy od utworzenia prostego modelu, który reprezentuje produkt.

Eksplorator rozwiązań nie jest jeszcze widoczny, kliknij przycisk **widoku** menu, a następnie wybierz **Eksploratora rozwiązań**. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy folderu modeli. Z menu kontekstowego wybierz **Dodaj** polecenie **klasy**.

![](tutorial-your-first-web-api/_static/image4.png)

Nazwa klasy &quot;produktu&quot;. Dodaj następujące właściwości do `Product` klasy.

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample1.cs)]

## <a name="adding-a-controller"></a>Dodawanie kontrolera

W interfejsie API sieci Web *kontrolera* jest obiektem, który obsługuje żądania HTTP. Dodamy kontroler, który może zwrócić listę produktów lub jednego produktu, określonego przez identyfikator.

> [!NOTE]
> Jeśli używasz platformy ASP.NET MVC, znasz już kontrolerów. Interfejs API sieci Web kontrolery są podobne do kontrolerów MVC, ale dziedziczą **klasy ApiController** klasy zamiast **kontrolera** klasy.

W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy folder kontrolerów. Wybierz **Dodaj** , a następnie wybierz **kontrolera**.

![](tutorial-your-first-web-api/_static/image5.png)

W **Dodawanie szkieletu** okno dialogowe, wybierz opcję **kontroler internetowego interfejsu API — pusty**. Kliknij przycisk **Dodaj**.

![](tutorial-your-first-web-api/_static/image6.png)

W **Dodaj kontroler** okno dialogowe, nazwy kontrolera &quot;ProductsController&quot;. Kliknij przycisk **Dodaj**.

![](tutorial-your-first-web-api/_static/image7.png)

Szkieletu tworzy plik o nazwie ProductsController.cs w folderze kontrolerów.

![](tutorial-your-first-web-api/_static/image8.png)

> [!NOTE]
> Nie ma potrzeby kontrolerach należy umieścić w folderze o nazwie kontrolerów. Nazwa folderu jest po prostu wygodny sposób organizowania plików źródłowych.


Jeśli ten plik nie jest jeszcze otwarty, kliknij dwukrotnie plik, aby go otworzyć. Zastąp kod w tym pliku następujących czynności:

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample2.cs)]

W celu uproszczenia w przykładzie produkty są przechowywane w tablicy o stałym wewnątrz klasy kontrolera. Oczywiście w rzeczywistej aplikacji, czy zapytanie dotyczące bazy danych lub użyj innego źródła danych zewnętrznych.

Kontroler definiuje dwie metody, które zwracają produktów:

- `GetAllProducts` Metoda zwraca całą listę produktów jako **IEnumerable&lt;produktu&gt;**  typu.
- `GetProduct` Metoda odwołuje się do jednego produktu za pomocą jego identyfikatora.

To wszystko! Masz pracy internetowego interfejsu API. Każda metoda na kontrolerze odnosi się do jednego lub więcej identyfikatorów URI:

| Metoda kontrolera | Identyfikator URI |
| --- | --- |
| GetAllProducts | / api/produktów |
| GetProduct | / InterfejsAPI/produkty/*identyfikator* |

Aby uzyskać `GetProduct` metody *identyfikator* w identyfikatorze URI jest symbolem zastępczym. Na przykład, aby uzyskać produkt o identyfikatorze 5, identyfikator URI jest `api/products/5`.

Aby uzyskać więcej informacji na temat sposobu kierowania żądań HTTP interfejsu API sieci Web do metod kontrolera, zobacz [routingu ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).

## <a name="calling-the-web-api-with-javascript-and-jquery"></a>Wywoływanie interfejsu API sieci Web za pomocą języka Javascript i jQuery

W tej sekcji dodamy strona HTML, która używa technologii AJAX w celu wywołania interfejsu API sieci web. Użyjemy jQuery wykonywanie wywołań AJAX, a także do aktualizacji strony z wynikami.

W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj**, a następnie wybierz **nowy element**.

![](tutorial-your-first-web-api/_static/image9.png)

W **Dodaj nowy element** okno dialogowe, wybierz opcję **Web** węzeł w węźle **Visual C#**, a następnie wybierz pozycję **strony HTML** elementu. Nazwij stronę &quot;index.html&quot;.

![](tutorial-your-first-web-api/_static/image10.png)

Zamień wszystko, co w tym pliku następujące czynności:

[!code-html[Main](tutorial-your-first-web-api/samples/sample3.html)]

Istnieje kilka sposobów uzyskania jQuery. W tym przykładzie używany [Microsoft Ajax CDN](../../../ajax/cdn/overview.md). Można również pobrać go z [ http://jquery.com/ ](http://jquery.com/)i programu ASP.NET "Interfejs API sieci Web" szablon projektu obejmuje także jQuery.

### <a name="getting-a-list-of-products"></a>Pobieranie listy produktów

Aby uzyskać listę produktów, Wyślij żądanie HTTP GET do &quot;/api/produktów&quot;.

JQuery [getJSON](http://api.jquery.com/jQuery.getJSON/) funkcja wysyła żądanie AJAX. Odpowiedź zawiera tablicę obiektów JSON. `done` Określa funkcja wywołania zwrotnego, która jest wywołana, jeśli żądanie zakończy się pomyślnie. Wywołanie zwrotne firma Microsoft aktualizuje modelu DOM przy użyciu informacji o produkcie.

[!code-html[Main](tutorial-your-first-web-api/samples/sample4.html)]

### <a name="getting-a-product-by-id"></a>Pobieranie produktu według Identyfikatora

Aby uzyskać produkt za pomocą Identyfikatora, Wyślij żądanie HTTP GET do &quot;/interfejs API/produkty/*identyfikator*&quot;, gdzie *identyfikator* jest identyfikator produktu.

[!code-javascript[Main](tutorial-your-first-web-api/samples/sample5.js)]

Firma Microsoft wciąż wywołać `getJSON` Aby wysłać żądanie AJAX, ale tym razem utworzona Państwu identyfikator w identyfikatorze URI żądania. Odpowiedź z tym żądaniem jest reprezentacja JSON jednego produktu.

## <a name="running-the-application"></a>Uruchamianie aplikacji

Naciśnij klawisz F5, aby rozpocząć debugowanie aplikacji. Strony sieci web powinna wyglądać następująco:

![](tutorial-your-first-web-api/_static/image11.png)

Aby uzyskać produkt za pomocą Identyfikatora, wprowadź identyfikator, a następnie kliknij przycisk wyszukiwania:

![](tutorial-your-first-web-api/_static/image12.png)

Jeśli wprowadzisz nieprawidłowy identyfikator, serwer zwraca błąd HTTP:

![](tutorial-your-first-web-api/_static/image13.png)

## <a name="using-f12-to-view-the-http-request-and-response"></a>Za pomocą F12, aby wyświetlić żądania HTTP i odpowiedzi

Podczas pracy z usługą HTTP może być bardzo przydatne zobaczyć żądania HTTP i żądania wiadomości. Można to zrobić przy użyciu narzędzi deweloperskich F12 w programie Internet Explorer 9. Z programu Internet Explorer 9, naciśnij klawisz **F12** można otworzyć narzędzia. Kliknij przycisk **sieci** kartę, a następnie naciśnij klawisz **Rozpocznij przechwytywanie**. Teraz wróć do strony sieci web i naciśnij klawisz **F5** ponowne załadowanie strony sieci web. Program Internet Explorer będzie przechwytywać ruch HTTP między przeglądarką a serwerem sieci web. Widok podsumowania przedstawia cały ruch sieciowy dla strony:

![](tutorial-your-first-web-api/_static/image14.png)

Zlokalizuj wpis dla względny identyfikator URI "interfejsu api/produkty /". Wybierz ten wpis, a następnie kliknij przycisk **przejdź do widoku szczegółowym**. W widoku szczegółów istnieją karty, aby wyświetlić żądanie i odpowiedź nagłówki i treść. Na przykład jeśli klikniesz **nagłówki żądań** karcie widać, że klient zażądał &quot;application/json&quot; w nagłówku Accept.

![](tutorial-your-first-web-api/_static/image15.png)

Jeśli klikniesz kartę treść odpowiedzi, zostanie wyświetlony, jak lista produktów został wydany do formatu JSON. Inne przeglądarki mają podobne funkcje. Inne przydatne narzędzie jest [Fiddler](http://www.fiddler2.com/fiddler2/), internetowy serwer proxy debugowania. Służy narzędzia Fiddler do wyświetlania ruchu HTTP, a także do tworzenia żądań HTTP, co daje pełną kontrolę nad nagłówków HTTP żądania.

## <a name="see-this-app-running-on-azure"></a>Zobacz tej aplikacji działających na platformie Azure

Czy chcesz w witrynie Zakończono działającego jako aplikacja sieci web? Pełną wersję aplikacji można wdrożyć do konta platformy Azure, po prostu kliknąć poniższy przycisk.

[![](http://azuredeploy.net/deploybutton.png)](https://deploy.azure.com/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/WebAPI-ProductsApp#/form/setup)

Potrzebujesz konta platformy Azure, aby wdrożyć to rozwiązanie na platformie Azure. Jeśli nie masz już konto, masz następujące opcje:

- [Otworzyć bezpłatne konto platformy Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) — otrzymasz kredyt służy do wypróbowania płatnych usług platformy Azure, a nawet w przypadku, po ich wyczerpaniu nawet możesz zachować konto i korzystać z bezpłatnych usług platformy Azure.
- [Aktywować korzyści dla subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -ramach subskrypcji MSDN daje środki na korzystanie z każdego miesiąca, używanego do płatne usługi platformy Azure.

## <a name="next-steps"></a>Następne kroki

- Aby uzyskać bardziej szczegółowy przykład usługi HTTP, która obsługuje operacje POST, PUT i DELETE i zapisuje je do bazy danych, zobacz [przy użyciu Web API 2 przy użyciu platformy Entity Framework 6](../data/using-web-api-with-entity-framework/part-1.md).
- Aby uzyskać więcej informacji o tworzeniu aplikacji sieci web płynne i szybkie działanie na podstawie usługi HTTP, zobacz [aplikacji jednostronicowej ASP.NET](../../../single-page-application/index.md).
- Aby uzyskać informacje o sposobie wdrażania projektu sieci web programu Visual Studio w usłudze Azure App Service, zobacz [tworzenie aplikacji sieci web platformy ASP.NET w usłudze Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).
