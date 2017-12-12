---
uid: web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
title: "Wprowadzenie do składnika ASP.NET Web API 2 (C#)"
author: MikeWasson
description: "HTTP nie jest po prostu wysyłaniu stron sieci web. Istnieje również zaawansowane platformę do tworzenia interfejsów API, który ujawnia usług i danych. HTTP jest prosty i elastyczny i ubiq..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/28/2017
ms.topic: article
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
msc.type: authoredcontent
ms.openlocfilehash: fa1cd068a7466e0b6b6fe7716090c8a7afd2a4d5
ms.sourcegitcommit: ec9371e2fbfcb8d62e7e7cae69e7752f3f205385
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/23/2017
---
<a name="getting-started-with-aspnet-web-api-2-c"></a>Wprowadzenie do składnika ASP.NET Web API 2 (C#)
====================
przez [Wasson Jan](https://github.com/MikeWasson)

[Pobieranie ukończone projektu](https://code.msdn.microsoft.com/Sample-code-of-Getting-c56ccb28)

HTTP nie jest po prostu wysyłaniu stron sieci web. HTTP jest również zaawansowane platformę do tworzenia interfejsów API, który ujawnia usług i danych. HTTP jest proste, elastyczne i uniwersalnych. Prawie każdego platformy, które można traktować ma biblioteki HTTP, więc usług HTTP może korzystać szeroka gama klientów, takich jak przeglądarki, urządzeń przenośnych i tradycyjnych aplikacji klasycznych.
 
Interfejs API sieci Web platformy ASP.NET to platforma do tworzenia interfejsów API na programie .NET Framework w sieci web. W tym samouczku użyjesz interfejsu API sieci Web platformy ASP.NET można utworzyć składnika web API, które zwraca listę produktów.

 
 ## <a name="software-versions-used-in-the-tutorial"></a>Używane w samouczku wersje oprogramowania
  
 - [Visual Studio 2017 r.](https://www.visualstudio.com/downloads/)
 - Składnik Web API 2

Zobacz [Tworzenie składnika web API platformy ASP.NET Core i Visual Studio dla systemu Windows](https://docs.microsoft.com/aspnet/core/tutorials/first-web-api) jest dostępna nowsza wersja tego samouczka.

## <a name="create-a-web-api-project"></a>Utwórz projekt interfejsu API sieci Web

W tym samouczku użyjesz interfejsu API sieci Web platformy ASP.NET można utworzyć składnika web API, które zwraca listę produktów. Strony sieci web frontonu używa jQuery, aby wyświetlić wyniki.

![](tutorial-your-first-web-api/_static/image1.png)

Uruchom program Visual Studio i wybierz **nowy projekt** z **Start** strony. Lub z **pliku** menu, wybierz opcję **nowy** , a następnie **projektu**.

W **szablony** okienku wybierz **zainstalowane szablony** i rozwiń **Visual C#** węzła. W obszarze **Visual C#**, wybierz pozycję **Web**. Na liście szablony projektów, wybierz **aplikacji sieci Web ASP.NET**. Nazwij projekt "ProductsApp", a następnie kliknij przycisk **OK**.

![](tutorial-your-first-web-api/_static/image2.png)

W **nowy projekt ASP.NET** okno dialogowe, wybierz opcję **pusty** szablonu. W obszarze &quot;. Dodaj foldery i podstawowe odwołania dla&quot;, sprawdź **interfejsu API sieci Web**. Kliknij przycisk **OK**.

![](tutorial-your-first-web-api/_static/image3.png)

> [!NOTE]
> Można również utworzyć projekt interfejsu API sieci Web za pomocą &quot;interfejsu API sieci Web&quot; szablonu. Szablon interfejsu API sieci Web używa platformy ASP.NET MVC, aby zapewnić strony pomocy interfejsu API. Używam programu pustego szablonu w tym samouczku, ponieważ chcę wyświetlić interfejs API sieci Web bez MVC. Ogólnie rzecz biorąc nie trzeba znać ASP.NET MVC można użyć interfejsu API sieci Web.


## <a name="adding-a-model"></a>Dodawanie modelu

A *modelu* jest obiekt, który reprezentuje dane w aplikacji. Interfejs API sieci Web platformy ASP.NET można automatycznie serializuj model do formatu JSON, XML lub niektóre inny format, a następnie wpisz dane serializowane w treści komunikatu odpowiedzi HTTP. Tak długo, jak długo klient może odczytywać format serializacji, może wykonywać deserializację obiektu. Większość klientów może przeanalizować składni XML lub JSON. Ponadto klienta można wskazać, który format chce przez ustawienie nagłówek Accept w komunikacie żądania HTTP.

Zacznijmy od utworzenia prostego modelu, który reprezentuje produkt.

Jeśli w Eksploratorze rozwiązań nie jest widoczny, kliknij przycisk **widoku** menu i wybierz **Eksploratora rozwiązań**. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy folder modeli. Wybierz z menu kontekstowego **Dodaj** następnie wybierz **klasy**.

![](tutorial-your-first-web-api/_static/image4.png)

Nazwa klasy &quot;produktu&quot;. Dodaj następujące właściwości `Product` klasy.

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample1.cs)]

## <a name="adding-a-controller"></a>Dodawanie kontrolera

W składniku Web API *kontrolera* jest obiekt, który obsługuje żądania HTTP. Dodamy kontrolera, które może zwracać listę produktów lub jeden produkt określony przez identyfikator.

> [!NOTE]
> Użycie programu ASP.NET MVC znasz już kontrolerów. Kontrolery interfejsu API sieci Web są podobne do kontrolerów MVC, ale dziedziczą **klasy ApiController** klasy zamiast **kontrolera** klasy.

W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy folder kontrolery. Wybierz **Dodaj** , a następnie wybierz **kontrolera**.

![](tutorial-your-first-web-api/_static/image5.png)

W **Dodawanie szkieletu** okno dialogowe, wybierz opcję **kontrolera interfejsu API sieci Web — pusty**. Kliknij przycisk **Dodaj**.

![](tutorial-your-first-web-api/_static/image6.png)

W **Dodaj kontroler** okna dialogowego, nazwy kontrolera &quot;ProductsController&quot;. Kliknij przycisk **Dodaj**.

![](tutorial-your-first-web-api/_static/image7.png)

Rusztowania tworzy plik o nazwie ProductsController.cs w folderze kontrolerów.

![](tutorial-your-first-web-api/_static/image8.png)

> [!NOTE]
> Nie trzeba umieścić kontrolerów w folderze o nazwie kontrolerów. Nazwa folderu jest po prostu wygodny sposób organizowania plików źródłowych.


Jeśli ten plik nie jest jeszcze otwarty, kliknij dwukrotnie plik, aby go otworzyć. Zastąp kod w tym pliku z następujących czynności:

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample2.cs)]

Aby zachować prosty przykład, produkty są przechowywane w tablicy o ustalonym wewnątrz klasy kontrolera. Oczywiście w rzeczywistej aplikacji, czy zapytanie dotyczące bazy danych lub użyj innego źródła danych zewnętrznych.

Kontroler definiuje dwie metody, które zwracają produktów:

- `GetAllProducts` Metoda zwraca całą listę produktów jako **IEnumerable&lt;produktu&gt;**  typu.
- `GetProduct` Metody wyszukuje pojedynczy produkt przez jego identyfikator.

To już wszystko! Masz pracy składnika web API. Każda metoda na kontrolerze odnosi się do co najmniej jeden identyfikator URI:

| Kontroler — metoda | Identyfikator URI |
| --- | --- |
| GetAllProducts | / api/produktów |
| GetProduct | /API/produkty/*id* |

Aby uzyskać `GetProduct` metody *identyfikator* w identyfikatorze URI to symbol zastępczy. Na przykład, aby uzyskać produktu o identyfikatorze 5, identyfikator URI jest `api/products/5`.

Aby uzyskać więcej informacji dotyczących sposobu interfejsu API sieci Web kieruje żądania HTTP do metod kontrolera, zobacz [routingu na platformie ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).

## <a name="calling-the-web-api-with-javascript-and-jquery"></a>Wywołanie interfejsu API sieci Web z Javascript i jQuery

W tej sekcji dodamy strona HTML, która używa technologii AJAX do wywołania interfejsu API sieci web. Użyjemy jQuery wywołań AJAX i aktualizowanie strony z wynikami.

W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj**, a następnie wybierz pozycję **nowy element**.

![](tutorial-your-first-web-api/_static/image9.png)

W **Dodaj nowy element** okno dialogowe, wybierz opcję **sieci Web** węźle **Visual C#**, a następnie wybierz **strony HTML** elementu. Nazwa strony &quot;index.html&quot;.

![](tutorial-your-first-web-api/_static/image10.png)

Zastąp wszystkie elementy w tym pliku następujące czynności:

[!code-html[Main](tutorial-your-first-web-api/samples/sample3.html)]

Istnieje kilka sposobów, aby uzyskać jQuery. W tym przykładzie używany [Microsoft Ajax CDN](../../../ajax/cdn/overview.md). Można również pobrać ją z [http://jquery.com/](http://jquery.com/)i ASP.NET "Interfejsu API sieci Web" szablon projektu zawiera również jQuery.

### <a name="getting-a-list-of-products"></a>Pobieranie listy produktów

Aby uzyskać listę produktów, Wyślij żądanie HTTP GET do &quot;produkty/api/&quot;.

JQuery [getJSON](http://api.jquery.com/jQuery.getJSON/) funkcja wysyła żądanie AJAX. Odpowiedź zawiera tablicę obiektów JSON. `done` Funkcja określa wywołania zwrotnego, która jest wywoływana, gdy żądanie zakończy się pomyślnie. Wywołanie zwrotne możemy zaktualizować modelu DOM z informacji o produkcie.

[!code-html[Main](tutorial-your-first-web-api/samples/sample4.html)]

### <a name="getting-a-product-by-id"></a>Uzyskiwanie produktu według Identyfikatora

Aby uzyskać produktu przez identyfikator, Wyślij żądanie HTTP GET do &quot;/api/produkty/*identyfikator*&quot;, gdzie *identyfikator* jest identyfikatora produktu.

[!code-javascript[Main](tutorial-your-first-web-api/samples/sample5.js)]

Nadal nazywamy `getJSON` do wysłania żądania AJAX, ale tym razem testujemy identyfikator w identyfikatorze URI żądania. Odpowiedź z tym żądaniem jest reprezentacja JSON jeden produkt.

## <a name="running-the-application"></a>Uruchamianie aplikacji

Naciśnij klawisz F5, aby rozpocząć debugowania aplikacji. Strony sieci web powinien wyglądać następująco:

![](tutorial-your-first-web-api/_static/image11.png)

Uzyskanie produktu przez identyfikator, wprowadź identyfikator, a następnie kliknij przycisk wyszukiwania:

![](tutorial-your-first-web-api/_static/image12.png)

Jeśli wprowadzisz nieprawidłowy identyfikator, serwer zwraca błąd HTTP:

![](tutorial-your-first-web-api/_static/image13.png)

## <a name="using-f12-to-view-the-http-request-and-response"></a>Aby wyświetlić żądania HTTP i odpowiedzi przy użyciu F12

Podczas pracy z usługą HTTP, może być bardzo przydatne zobaczyć żądania HTTP i żądania wiadomości. Można to zrobić za pomocą narzędzi deweloperskich F12 w programie Internet Explorer 9. Internet Explorer 9, naciśnij klawisze **F12** można otworzyć narzędzia. Kliknij przycisk **sieci** kartę i naciśnij klawisz **Start Przechwytywanie**. Teraz przejdź do strony sieci web i naciśnij klawisz **F5** ponowne załadowanie tej strony sieci web. Program Internet Explorer będzie przechwytywać ruchu HTTP między przeglądarką a serwerem sieci web. Widok podsumowania przedstawia cały ruch sieciowy dla strony:

![](tutorial-your-first-web-api/_static/image14.png)

Znajdź wpis dla względnego identyfikatora URI "produkty/api /". Wybierz tę pozycję, a następnie kliknij przycisk **przejdź do widoku szczegółowe**. W widoku szczegółów istnieją karty, aby wyświetlić nagłówki żądań i odpowiedzi i treść. Na przykład, jeśli klikniesz przycisk **nagłówki żądań** karcie widać, że klient żądał &quot;application/json&quot; w nagłówku Accept.

![](tutorial-your-first-web-api/_static/image15.png)

Jeśli klikniesz kartę treść odpowiedzi widać, jak lista produktów została wykonana serializacja do formatu JSON. Inne przeglądarki mają podobną funkcjonalność. Inne przydatne narzędzie jest [Fiddler](http://www.fiddler2.com/fiddler2/), debugowanie serwera proxy sieci web. Służy Fiddler do wyświetlania ruchu HTTP i tworzą żądania HTTP, co pozwala na pełną kontrolę nad nagłówków HTTP w żądaniu.

## <a name="see-this-app-running-on-azure"></a>Zobacz tej aplikacji działających na platformie Azure

Czy chcesz w witrynie Zakończono uruchomione jako aplikacji sieci web? Pełną wersję aplikacji można wdrożyć do konta platformy Azure w celu wystarczy kliknąć poniższy przycisk.

[![](http://azuredeploy.net/deploybutton.png)](https://deploy.azure.com/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/WebAPI-ProductsApp#/form/setup)

Potrzebujesz konta platformy Azure, aby wdrożyć to rozwiązanie do platformy Azure. Jeśli nie masz już konto, dostępne są następujące opcje:

- [Otworzyć bezpłatne konto platformy Azure](https://azure.microsoft.com/en-us/pricing/free-trial/?WT.mc_id=A443DD604) — otrzymasz kredyt służy do wypróbowania płatnych usług Azure, a nawet po wyczerpaniu kredytu możesz zachować konto i korzystać z bezpłatnych usług platformy Azure.
- [Aktywować korzyści dla subskrybentów MSDN](https://azure.microsoft.com/en-us/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -subskrypcji Your MSDN otrzymasz kredyt, co miesiąc, używanego programu płatnych usług Azure.

## <a name="next-steps"></a>Następne kroki

- Aby bardziej szczegółowy przykład usługi HTTP, która obsługuje operacje POST, PUT i DELETE i zapisuje do bazy danych, zobacz [przy użyciu sieci Web API 2 z programu Entity Framework 6](../data/using-web-api-with-entity-framework/part-1.md).
- Aby uzyskać więcej informacji na temat tworzenia aplikacji sieci web płynne i szybkie ponad usługą HTTP, zobacz [aplikacji jednej strony ASP.NET](../../../single-page-application/index.md).
- Aby uzyskać informacje dotyczące wdrażania projektu sieci web programu Visual Studio w usłudze Azure App Service, zobacz [tworzenie aplikacji sieci web platformy ASP.NET w usłudze Azure App Service](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-get-started/).
