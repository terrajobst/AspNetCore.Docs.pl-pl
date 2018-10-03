---
uid: web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
title: Wprowadzenie do wzorca ASP.NET Web API 2 (C#)
author: MikeWasson
description: Protokół HTTP nie jest dostępne tylko dla wysyłaniu stron sieci web. Ponadto jest to zaawansowana platforma do tworzenia interfejsów API, które udostępniają dane i usługi. Protokół HTTP jest proste, elastyczne i ubiq...
ms.author: riande
ms.date: 11/28/2017
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
msc.type: authoredcontent
ms.openlocfilehash: 14796ce31187a0f6a6331b46477fadc77e281c3b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753493"
---
<a name="get-started-with-aspnet-web-api-2-c"></a>Wprowadzenie do wzorca ASP.NET Web API 2 (C#)
====================
przez [Mike Wasson](https://github.com/MikeWasson)

[Pobierz ukończony projekt](https://code.msdn.microsoft.com/Sample-code-of-Getting-c56ccb28)

Protokół HTTP nie jest wykorzystywany tylko i wyłącznie do przesyłania stron sieci web. Protokół HTTP to również zaawansowana platforma do tworzenia interfejsów API, które udostępniają dane i usługi. Protokół HTTP jest prosty, elastyczny i uniwersalny. Niemal każda, dowolna platforma obsługująca biblioteki HTTP może korzystać z usług HTTP dla szerokiej gamy klientów, w tym przeglądarki, urządzenia przenośne i tradycyjne aplikacje komputerowe.
 
Web API platformy ASP.NET to architektura służąca do tworzenia interfejsów API w programie .NET Framework w sieci web. W tym samouczku użyjesz interfejsu API sieci Web platformy ASP.NET do tworzenia internetowego interfejsu API, który zwraca listę produktów.

 
 ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używanego w tym samouczku
  
 - [Visual Studio 2017](https://www.visualstudio.com/downloads/)
 - Web API 2

Zobacz [Tworzenie internetowego interfejsu API za pomocą platformy ASP.NET Core i Visual Studio dla systemu Windows](https://docs.microsoft.com/aspnet/core/tutorials/first-web-api), dla nowszej wersji tego samouczka.

## <a name="create-a-web-api-project"></a>Utwórz projekt internetowego interfejsu API

W tym samouczku użyjesz interfejsu API sieci Web platformy ASP.NET do tworzenia internetowego interfejsu API, który zwraca listę produktów. Strony sieci web wykorzystują jQuery, aby wyświetlić wyniki.

![](tutorial-your-first-web-api/_static/image1.png)

Uruchom program Visual Studio i wybierz opcję **nowy projekt** z menu **Start**. Możesz również z menu **plik**, wybrać opcję **Nowy**, a następnie **projekt**.

W okienku **szablony** wybierz opcję **zainstalowane szablony** i rozwiń węzeł **Visual C#**. W obszarze **Visual C#**, wybierz opcję **Web**. Na liście szablonów projektu wybierz **aplikacji sieci Web ASP.NET**. Nadaj projektowi nazwę "ProductsApp", a następnie kliknij przycisk **OK**.

![](tutorial-your-first-web-api/_static/image2.png)

W oknie dialogowym **nowy projekt ASP.NET**, wybierz opcję szablonu **pusty**. W obszarze &quot;. Dodaj foldery i podstawowe odwołania dla&quot;, zaznacz **interfejs API sieci Web**. Kliknij przycisk **OK**.

![](tutorial-your-first-web-api/_static/image3.png)

> [!NOTE]
> Można również utworzyć projekt interfejsu API sieci Web za pomocą szablonu &quot;interfejs API sieci Web&quot;. Szablon interfejsu API sieci Web używa platformy ASP.NET MVC w celu zapewnienia pomocy przy tworzeniu stron interfejsu API. Używam pustego szablonu na potrzeby tego samouczka ponieważ chcę pokazać interfejs API sieci Web bez MVC. Ogólnie rzecz biorąc nie trzeba znać platformy ASP.NET MVC w celu korzystania z interfejsu API sieci Web.


## <a name="adding-a-model"></a>Dodawanie modelu
*Model* jest obiektem, który reprezentuje dane w aplikacji. ASP.NET Web API może automatycznie serializować model do formatu JSON, XML lub w innym formacie. Następnie wpisz dane serializowane w treści komunikatu odpowiedzi HTTP. Tak długo, jak długo klient może odczytywać format serializacji, może wykonywać deserializację obiektu. Większość klientów może przeanalizować format XML lub JSON. Ponadto klient może wskazywać formatu, który chce, ustawiając nagłówek Accept w komunikacie żądania HTTP.

Zacznijmy od utworzenia prostego modelu, który reprezentuje produkt.

Eksplorator rozwiązań nie jest jeszcze widoczny, kliknij przycisk menu **widok**, a następnie wybierz **Eksploratora rozwiązań**. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy folder modeli. Z menu kontekstowego wybierz **Dodaj**, polecenie **klasy**.

![](tutorial-your-first-web-api/_static/image4.png)

Nazwa klasy &quot;produktu&quot;. Dodaj następujące właściwości do klasy `Product`.

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample1.cs)]

## <a name="adding-a-controller"></a>Dodawanie kontrolera

W interfejsie API sieci Web *kontroler* jest obiektem, który obsługuje żądania HTTP. Dodamy kontroler, który może zwrócić listę produktów lub jednego produktu, określonego przez identyfikator.

> [!NOTE]
> Jeśli używasz platformy ASP.NET MVC, znasz już kontrolery. W interfejsie API sieci Web kontrolery są podobne do kontrolerów MVC, ale dziedziczą klasę **ApiController** zamiast klasy **kontroler**.

W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy folder kontrolerów. Wybierz **Dodaj** , a następnie wybierz **kontroler**.

![](tutorial-your-first-web-api/_static/image5.png)

W oknie dialogowym **Dodawanie szkieletu**, wybierz opcję **kontroler internetowego interfejsu API — pusty**. Kliknij przycisk **Dodaj**.

![](tutorial-your-first-web-api/_static/image6.png)

W oknie dialogowym **Dodaj kontroler**, nazwij kontroler &quot;ProductsController&quot;. Kliknij przycisk **Dodaj**.

![](tutorial-your-first-web-api/_static/image7.png)

Szkielet utworzy plik o nazwie ProductsController.cs w folderze kontrolerów.

![](tutorial-your-first-web-api/_static/image8.png)

> [!NOTE]
> Nie ma potrzeby umieścić kontrolera w folderze o nazwie kontroler. Nazwa folderu to po prostu wygodny sposób organizowania plików źródłowych.


Jeśli ten plik nie jest jeszcze otwarty, kliknij dwukrotnie, aby go otworzyć. Zastąp kod w tym pliku:

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample2.cs)]

W celu uproszczenia w przykładzie, produkty są przechowywane w stałej tablicy wewnątrz klasy kontrolera. Oczywiście w rzeczywistej aplikacji, zapytanie zostanie przesłane do bazy danych, lub innego źródła danych.

Kontroler definiuje dwie metody, które zwracają produkt:

- `GetAllProducts` Metoda zwraca całą listę produktów jako typ **IEnumerable&lt;produktu&gt;**.
- `GetProduct` Metoda odwołuje się do jednego produktu za pomocą jego identyfikatora.

To wszystko! Masz działający internetowy interfejs API. Każda metoda na kontrolerze odnosi się do jednego lub więcej identyfikatorów URI:

| Metoda kontrolera | Identyfikator URI |
| --- | --- |
| GetAllProducts | / api/produktów |
| GetProduct | / InterfejsAPI/produkty/*identyfikator* |

Aby uzyskać metodę `GetProduct` klasy *identyfikator* w identyfikatorze URI, użyj symbolu zastępczego. Na przykład, aby uzyskać produkt o identyfikatorze 5, wywołaj identyfikator URI `api/products/5`.

Aby uzyskać więcej informacji na temat sposobu kierowania żądań HTTP interfejsu API sieci Web do metod kontrolera, zobacz [routing ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).

## <a name="calling-the-web-api-with-javascript-and-jquery"></a>Wywoływanie interfejsu API sieci Web za pomocą języka Javascript i jQuery

W tej sekcji dodamy stronę HTML, która używa technologii AJAX w celu wywołania interfejsu API sieci web. Użyjemy jQuery który wykona wywołanie AJAX, a także zaktualizuje strony z wynikami.

W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj**, a następnie wybierz **nowy element**.

![](tutorial-your-first-web-api/_static/image9.png)

W oknie dialogowym **Dodaj nowy element**, wybierz opcję **Web** w węźle **Visual C#**, a następnie wybierz element **strona HTML**. Nazwij stronę &quot;index.html&quot;.

![](tutorial-your-first-web-api/_static/image10.png)

Zamień następujące elementy:

[!code-html[Main](tutorial-your-first-web-api/samples/sample3.html)]

Istnieje kilka sposobów uzyskania jQuery. W tym przykładzie używany [Microsoft Ajax CDN](../../../ajax/cdn/overview.md). Można również pobrać go z [ http://jquery.com/ ](http://jquery.com/)i programu ASP.NET "Interfejs API sieci Web". Szablon projektu obejmuje także jQuery.

### <a name="getting-a-list-of-products"></a>Pobieranie listy produktów

Aby uzyskać listę produktów, Wyślij żądanie HTTP GET do &quot;/api/produktów&quot;.

JQuery [getJSON](http://api.jquery.com/jQuery.getJSON/) wysyła żądanie AJAX. Odpowiedź zawiera tablicę obiektów JSON. `done` Określa funkcja wywołania zwrotnego, która jest wywołana, jeśli żądanie zakończy się pomyślnie. Wywołaniem zwrotnym, firma Microsoft aktualizuje modelu DOM przy użyciu informacji o produkcie.

[!code-html[Main](tutorial-your-first-web-api/samples/sample4.html)]

### <a name="getting-a-product-by-id"></a>Pobieranie produktu według Identyfikatora

Aby uzyskać produkt za pomocą Identyfikatora, Wyślij żądanie HTTP GET do &quot;/interfejs API/produkty/*identyfikator*&quot;, gdzie *identyfikator* jest identyfikatorem produktu.

[!code-javascript[Main](tutorial-your-first-web-api/samples/sample5.js)]

Firma Microsoft wciąż wywołuje `getJSON` aby wysłać żądanie AJAX, ale tym razem utworzony przez Państwo identyfikator w identyfikatorze URI żądania. Odpowiedź z tym żądaniem jest reprezentacją JSON jednego produktu.

## <a name="running-the-application"></a>Uruchamianie aplikacji

Naciśnij klawisz F5, aby rozpocząć debugowanie aplikacji. Strona sieci web powinna wyglądać następująco:

![](tutorial-your-first-web-api/_static/image11.png)

Aby uzyskać produkt za pomocą Identyfikatora, wprowadź identyfikator, a następnie kliknij przycisk wyszukiwania:

![](tutorial-your-first-web-api/_static/image12.png)

Jeśli wprowadzisz nieprawidłowy identyfikator, serwer zwraca błąd HTTP:

![](tutorial-your-first-web-api/_static/image13.png)

## <a name="using-f12-to-view-the-http-request-and-response"></a>Użycie F12, aby wyświetlić żądania i odpowiedzi HTTP

Podczas pracy z usługą HTTP może być bardzo przydatne sprawdzenie żądania i odpowiedzi HTTP. Można to zrobić przy użyciu narzędzi deweloperskich F12 w programie Internet Explorer 9. Z programu Internet Explorer 9, naciśnij klawisz **F12** aby otworzyć narzędzia. Kliknij przycisk **sieci**, a następnie naciśnij klawisz **Rozpocznij przechwytywanie**. Teraz wróć do strony sieci web i naciśnij klawisz **F5** ponowne załadowanie strony sieci web. Program Internet Explorer będzie przechwytywać ruch HTTP między przeglądarką a serwerem sieci web. Widok podsumowania przedstawia cały ruch sieciowy dla strony:

![](tutorial-your-first-web-api/_static/image14.png)

Zlokalizuj wpis dla względnego identyfikatora URI interfejsu "api/produkty /". Wybierz ten wpis, a następnie kliknij przycisk **przejdź do widoku szczegółowego**. W widoku szczegółów istnieją karty, które pozwalają wyświetlić żądanie i odpowiedź nagłówka. Na przykład jeśli klikniesz kartę **nagłówki żądań**, widać, że klient zażądał &quot;application/json&quot; w nagłówku Accept.

![](tutorial-your-first-web-api/_static/image15.png)

Jeśli klikniesz kartę **treść odpowiedzi**, zostanie wyświetlony jako lista produktów, wydany do formatu JSON. Inne przeglądarki mają podobne funkcje. Inne przydatne narzędzie to [Fiddler](http://www.fiddler2.com/fiddler2/), internetowy serwer proxy dla debugowania. Służy on do wyświetlania ruchu HTTP, a także do tworzenia żądań HTTP, co daje pełną kontrolę nad nagłówkami HTTP.

## <a name="see-this-app-running-on-azure"></a>Zobacz tą aplikacje działającą na platformie Azure

Czy chcesz zobaczyć skończony projekt jako aplikacja sieci web? Pełną wersję aplikacji można wdrożyć do konta platformy Azure, po prostu kliknij poniższy przycisk.

[![](http://azuredeploy.net/deploybutton.png)](https://deploy.azure.com/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/WebAPI-ProductsApp#/form/setup)

Potrzebujesz konta platformy Azure, aby wdrożyć to rozwiązanie na platformie Azure. Jeśli nie masz jeszcze konta, masz następujące opcje:

- [Otworzyć bezpłatne konto platformy Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) — otrzymasz kredyt służy do wypróbowania płatnych usług platformy Azure, a nawet w przypadku ich wyczerpania nawet możesz zachować konto i korzystać z bezpłatnych usług platformy Azure.
- [Aktywować korzyści dla subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - w ramach subskrypcji MSDN daje środki na korzystanie z każdego miesiąca, używanego przez płatne usługi platformy Azure.

## <a name="next-steps"></a>Następne kroki

- Aby uzyskać bardziej szczegółowy przykład usługi HTTP, która obsługuje operacje POST, PUT i DELETE i zapisuje je do bazy danych, zobacz [użycie Web API 2 przy użyciu platformy Entity Framework 6](../data/using-web-api-with-entity-framework/part-1.md).
- Aby uzyskać więcej informacji o tworzeniu aplikacji sieci web na podstawie usługi HTTP, zobacz [aplikacji jednostronicowej ASP.NET](../../../single-page-application/index.md).
- Aby uzyskać informacje o sposobie wdrażania projektu sieci web programu Visual Studio w usłudze Azure App Service, zobacz [tworzenie aplikacji sieci web platformy ASP.NET w usłudze Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).
