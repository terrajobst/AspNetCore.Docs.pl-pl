---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing
title: Adres URL routingu | Dokumentacja firmy Microsoft
author: Erikre
description: Ten samouczek serii uczy podstawowe informacje dotyczące tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu platformy ASP.NET 4.5 i programu Microsoft Visual Studio Express 2013 dla możemy...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 4f4bf092-c400-471f-a876-78fda0417890
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing
msc.type: authoredcontent
ms.openlocfilehash: a195b36517bcae4bbeaf43fe7386e7787fd00212
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="url-routing"></a>Routingu adresów URL
====================
Przez [Erik Reitan](https://github.com/Erikre)

[Pobierz Wingtip Toys przykładowy projekt (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) lub [Pobierz E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Ten samouczek serii nauczyć podstawowe informacje dotyczące tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu platformy ASP.NET 4.5 i programu Microsoft Visual Studio Express 2013 for Web. Visual Studio 2013 [projektu z kodu źródłowego C#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) jest dostępna towarzyszące tej serii samouczka.


W tym samouczku należy zmodyfikować Wingtip Toys przykładowej aplikacji do obsługi routingu adresów URL. Routing pozwala używać adresów URL, które są przyjazną, lepiej jest obsługiwana przez aparaty wyszukiwania i łatwiejsze do zapamiętania aplikacji sieci web. W tym samouczku jest oparty na poprzednich samouczek "Członkostwa i Administracja" i jest częścią serii samouczek Wingtip Toys.

## <a name="what-youll-learn"></a>Zawartość:

- Jak zarejestrować tras dla aplikacji formularzy sieci Web ASP.NET.
- Jak dodać trasy do strony sieci web.
- Jak wybrać dane z bazy danych do obsługi trasy.

## <a name="aspnet-routing-overview"></a>Omówienie routingu platformy ASP.NET

Routingu adresów URL umożliwia konfigurowanie aplikacji do akceptowania żądań adresów URL, które nie są mapowane na pliki fizyczne. Adres URL żądania jest po prostu adres URL, które użytkownik wprowadzi w przeglądarce, aby znaleźć strony w witrynie sieci web. Aby zdefiniować adresy URL, które są semantycznie zrozumiały dla użytkowników i które mogą pomóc z optymalizacji dla aparatów wyszukiwania (SEO) używać routingu.

Domyślnie szablon formularzy sieci Web zawiera [przyjazne adresy URL platformy ASP.NET](http://www.nuget.org/packages/Microsoft.AspNet.FriendlyUrls/). Większość podstawowych zadań routingu są realizowane za pomocą *przyjazne adresy URL*. Jednak w tym samouczku spowoduje dodanie dostosowanych funkcji routingu.

Przed rozpoczęciem dostosowywania routingu adresów URL, do aplikacji przykładowej Wingtip Toys można połączyć z produktu przy użyciu następującego adresu URL:

`https://localhost:44300/ProductDetails.aspx?productID=2`

Dostosowując routingu adresów URL Wingtip Toys przykładowej aplikacji połączy się z tego samego produktu za pomocą czytelność adresu URL:

`https://localhost:44300/Product/Convertible%20Car`

### <a name="routes"></a>Trasy

Trasa jest wzorca adresu URL, który jest mapowany do programu obsługi. Program obsługi może być plik fizyczny, takich jak plik .aspx w aplikacji formularzy sieci Web. Program obsługi może być również klasę, która przetwarza żądanie. Aby zdefiniować trasy, należy utworzyć wystąpienia klasy trasy przez określenie wzorca adresu URL, program obsługi i opcjonalnie nazwy trasy.

Dodaj trasę do aplikacji przez dodanie `Route` obiektu do statycznego `Routes` właściwość `RouteTable` klasy. Właściwość tras jest `RouteCollection` obiekt, który przechowuje wszystkie tras dla aplikacji.

### <a name="url-patterns"></a>Wzorców adresów URL

Wzorzec URL może zawierać wartości literałów i zmiennej symbole zastępcze (określanych jako parametry adresu URL). Literały i symbole zastępcze znajdują się w segmenty adresu URL, które są rozdzielane ukośnik (`/`) znaków.

Po wysłaniu żądania do aplikacji sieci web, adres URL jest analizowana na segmenty i symbole zastępcze i wartości zmiennych są przekazywane do programu obsługi żądania. Ten proces jest podobny do sposobu danych w ciągu zapytania jest analizowana i przekazywane do obsługi żądania. W obu przypadkach zmiennej informacje są zawarte w adresie URL i przekazywane do programu obsługi w postaci pary klucz wartość. Ciągi zapytań zarówno klucze i wartości są w adresie URL. W przypadku tras klucze są nazwy symbolu zastępczego zdefiniowane we wzorcu adres URL i są tylko wartości w adresie URL.

We wzorcu adres URL, należy zdefiniować symbole zastępcze ujęte w nawiasy klamrowe ( `{` i `}` ). Można zdefiniować więcej niż jeden symbol zastępczy w segmencie, ale symbole zastępcze muszą być oddzielone wartość literału. Na przykład `{language}-{country}/{action}` to wzorzec prawidłowej trasy. Jednak `{language}{country}/{action}` nie jest prawidłowy wzorzec, ponieważ nie istnieje wartość literału lub separator symbole zastępcze. W związku z tym routingu nie może ustalić, gdzie można oddzielić wartość symbolu zastępczego języka od wartości dla symbolu zastępczego kraju.

### <a name="mapping-and-registering-routes"></a>Mapowanie i rejestrowanie tras

Przed dołączeniem trasy do stron Wingtip Toys przykładowej aplikacji, należy zarejestrować trasy podczas uruchamiania aplikacji. Aby zarejestrować trasy, należy zmodyfikować `Application_Start` obsługi zdarzeń.

1. W **Eksploratora rozwiązań**programu Visual Studio, znajdowanie i otwieranie *Global.asax.cs* pliku.
2. Dodaj kod wyróżnione kolorem żółtym do *Global.asax.cs* plików w następujący sposób:   

    [!code-csharp[Main](url-routing/samples/sample1.cs?highlight=30-31,34-46)]

Kiedy Wingtip Toys przykładowej aplikacji, wywołuje `Application_Start` obsługi zdarzeń. Na końcu tej obsługi zdarzeń `RegisterCustomRoutes` metoda jest wywoływana. `RegisterCustomRoutes` Metoda dodaje każdej trasy przez wywołanie metody `MapPageRoute` metody `RouteCollection` obiektu. Trasy są definiowane przy użyciu nazwy trasy, adres URL trasy i fizycznego adresu URL.

Pierwszy parametr ("`ProductsByCategoryRoute`") jest nazwą trasy. Służy do wywołania trasy, gdy jest to potrzebne. Drugi parametr ("`Category/{categoryName}`") określa przyjazną zastępuje adres URL, który może być dynamiczny na podstawie kodu. Za pomocą tej trasy są wypełnianie formantu danych z łącza, które są generowane na podstawie danych. Trasa jest wyświetlany w następujący sposób:

[!code-csharp[Main](url-routing/samples/sample2.cs)]

Drugi parametr trasy zawiera określoną wartość dynamiczna klamrowym (`{ }`). W takim przypadku `categoryName` jest zmienna, która będzie służyć do określenia prawidłowego ścieżkę routingu.

> [!NOTE] 
> 
> **Optional**
> 
> Użytkownik może łatwiej zarządzać kodem przenosząc `RegisterCustomRoutes` metody do osobnej klasy. W *logiki* folderu, Utwórz oddzielne `RouteActions` klasy. Przenieś powyższych `RegisterCustomRoutes` metody z *Global.asax.cs* pliku do nowej `RoutesActions` klasy. Użyj `RoleActions` klasy i `createAdmin` metody, na przykład sposób wywoływania `RegisterCustomRoutes` metody z *Global.asax.cs* pliku.


Należy również zauważyć `RegisterRoutes` przy użyciu wywołania metody `RouteConfig` obiektu na początku `Application_Start` obsługi zdarzeń. To wywołanie dotyczące implementowania routingu domyślnego. Została włączona jako domyślny kod po utworzeniu aplikacji przy użyciu szablonu formularzy sieci Web programu Visual Studio.

## <a name="retrieving-and-using-route-data"></a>Trwa pobieranie i przy użyciu danych trasy

Jak wspomniano powyżej, można zdefiniować trasy. Kod, który został dodany do `Application_Start` obsługi zdarzeń w *Global.asax.cs* plik ładuje zdefiniowanych tras.

### <a name="setting-routes"></a>Ustawienie tras

Trasy trzeba dodać dodatkowy kod. W tym samouczku użyjesz wiązania modelu, aby pobrać `RouteValueDictionary` obiekt, który jest używany podczas generowania trasy przy użyciu danych z formantu danych. `RouteValueDictionary` Obiektu będzie zawierać listę nazw produktów, które należą do jednej konkretnej kategorii produktów. Link jest tworzony dla każdego produktu w oparciu o dane i trasy.

#### <a name="enable-routes-for-categories-and-products"></a>Włącz trasy dla kategorii i produkty

Następna, będzie aktualizacja aplikacji umożliwiająca użycie `ProductsByCategoryRoute` do określenia poprawnej trasy do uwzględnienia w ramach poszczególnych łączy kategorii produktów. Zostanie również zaktualizować *ProductList.aspx* stronę, aby uwzględnić łącze do routingiem dla każdego produktu. Łącza zostanie wyświetlony jako znajdowały się przed zmianą, jednak łącza będą używać routingu adresów URL.

1. W **Eksploratora rozwiązań**, otwórz *Site.Master* strony, jeśli nie jest już otwarty.
2. Aktualizacja **ListView** formantu o nazwie "`categoryList`" ze zmianami wyróżnione kolorem żółtym, więc znaczników wygląda następująco:   

    [!code-aspx[Main](url-routing/samples/sample3.aspx?highlight=7-9)]
3. W **Eksploratora rozwiązań**, otwórz *ProductList.aspx* strony.
4. Aktualizacja `ItemTemplate` elementu *ProductList.aspx* strony z aktualizacjami wyróżnione kolorem żółtym, więc znaczników wygląda następująco:   

    [!code-aspx[Main](url-routing/samples/sample4.aspx?highlight=6-9,14-16)]
5. Otwórz kodem z *ProductList.aspx.cs* i dodaj następującą przestrzeń nazw jako wyróżniane na żółty:  

    [!code-csharp[Main](url-routing/samples/sample5.cs?highlight=9)]
6. Zastąp `GetProducts` metody kodu powiązanego (*ProductList.aspx.cs*) z następującym kodem:   

    [!code-csharp[Main](url-routing/samples/sample6.cs)]

#### <a name="add-code-for-product-details"></a>Dodaj kod, aby uzyskać szczegółowe informacje o produkcie

Teraz zaktualizuj kodu powiązanego (*ProductDetails.aspx.cs*) dla *ProductDetails.aspx* stronę, aby użyć danych trasy. Zwróć uwagę, że nowe `GetProduct` metoda również przyjmuje wartość ciągu kwerendy dla przypadku, gdy użytkownik ma łącze zakładki używa starszej URL innych niż przyjazne, bez routingu.

1. Zastąp `GetProduct` metody kodu powiązanego (*ProductDetails.aspx.cs*) z następującym kodem:   

    [!code-csharp[Main](url-routing/samples/sample7.cs)]

## <a name="running-the-application"></a>Uruchamianie aplikacji

Możesz uruchomić aplikację teraz, aby zobaczyć zaktualizowane trasy.

1. Naciśnij klawisz **F5** do uruchomienia aplikacji przykładowej Wingtip Toys.  
 W przeglądarce zostanie otwarty i przedstawia *Default.aspx* strony.
2. Kliknij przycisk **produktów** łącze w górnej części strony.  
 Wszystkie produkty są wyświetlane na *ProductList.aspx* strony. Następujący adres URL (przy użyciu numeru portu) są wyświetlane dla przeglądarki:  
    `https://localhost:44300/ProductList`
3. Następnie kliknij przycisk **samochodów** łącze kategorii w górnej części strony.  
 Tylko samochodów są wyświetlane na *ProductList.aspx* strony. Następujący adres URL (przy użyciu numeru portu) są wyświetlane dla przeglądarki:  
    `https://localhost:44300/Category/Cars`
4. Kliknij łącze zawierające nazwę pierwszego samochodu na liście na stronie ("**samochodu możliwe do przekonwertowania**"), aby wyświetlić szczegóły produktu.  
 Następujący adres URL (przy użyciu numeru portu) są wyświetlane dla przeglądarki:  
    `https://localhost:44300/Product/Convertible%20Car`
5. Następnie wprowadź poniższy nietrasowanego adres URL (przy użyciu numeru portu) w przeglądarce:  
    `https://localhost:44300/ProductDetails.aspx?productID=2`  
 Kod nadal rozpoznaje adres URL, który zawiera ciąg zapytania dla przypadku, gdy użytkownik ma łącze zakładki.

## <a name="summary"></a>Podsumowanie

W tym samouczku można dodać trasy dla kategorii i produktów. Wiesz już, jak trasy można zintegrować z formantów danych, które używają wiązania modelu. W następnym samouczku zaimplementowaniem obsługi błędów globalnego.

## <a name="additional-resources"></a>Dodatkowe zasoby

[ASP.NET, przyjazne adresy URL](http://www.nuget.org/packages/Microsoft.AspNet.FriendlyUrls/)  
[Wdrażanie aplikacji formularzy bezpiecznej sieci Web platformy ASP.NET z członkostwa, OAuth i bazy danych SQL Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Microsoft Azure — bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/)

> [!div class="step-by-step"]
> [Poprzednie](membership-and-administration.md)
> [dalej](aspnet-error-handling.md)
