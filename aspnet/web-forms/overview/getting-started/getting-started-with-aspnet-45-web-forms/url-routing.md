---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing
title: Routing adresów URL | Dokumentacja firmy Microsoft
author: Erikre
description: Tej serii samouczków obejmuje podstawy tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu platformy ASP.NET 4.5 i programu Microsoft Visual Studio Express 2013 for firma Microsoft...
ms.author: aspnetcontent
ms.date: 09/08/2014
ms.assetid: 4f4bf092-c400-471f-a876-78fda0417890
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing
msc.type: authoredcontent
ms.openlocfilehash: 968517100275822071e2101a4cfb978320f222f2
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37822721"
---
<a name="url-routing"></a>Routing adresów URL
====================
przez [Erik Reitan](https://github.com/Erikre)

[Pobierz Wingtip Toys przykładowego projektu (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) lub [Pobierz książkę elektroniczną (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> W tej serii samouczków obejmuje podstawy tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu platformy ASP.NET 4.5 i programu Microsoft Visual Studio Express 2013 for Web. Visual Studio 2013 [projektu za pomocą kodu źródłowego języka C#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) jest dostępny dla tej serii samouczków towarzyszą.


W tym samouczku zmodyfikujesz przykładowej aplikacji Wingtip Toys w celu obsługi routingu adresów URL. Routing umożliwia aplikację sieci web, aby używała adresów URL, które są przyjazne, łatwiejsze do zapamiętania i lepszego obsługiwane przez aparaty wyszukiwania. Ten samouczek opiera się na poprzednim samouczku "Członkostwo i Administracja" i jest częścią serii samouczków firmy Wingtip Toys.

## <a name="what-youll-learn"></a>Zawartość:

- Jak zarejestrować tras dla aplikacji formularzy sieci Web ASP.NET.
- Jak dodać trasy do strony sieci web.
- Jak wybrać dane z bazy danych w celu obsługi trasy.

## <a name="aspnet-routing-overview"></a>Omówienie routingu platformy ASP.NET

Routing adresów URL pozwala skonfigurować aplikację tak, aby akceptował żądania z adresów URL, które nie są mapowane na pliki fizyczne. Adres URL żądania jest po prostu adres URL, które są podawane w przeglądarce, aby znaleźć stronę w witrynie sieci web. Używasz routingu do definiowania adresów URL, które są semantycznie zrozumiałe dla użytkowników i które może pomóc w optymalizacji dla aparatów wyszukiwania (SEO).

Domyślnie szablon formularzy sieci Web zawiera [przyjazne adresy URL platformy ASP.NET](http://www.nuget.org/packages/Microsoft.AspNet.FriendlyUrls/). Większość podstawowych zadań routingu są realizowane za pomocą *przyjazne adresy URL*. Jednak w tym samouczku dodasz dostosowane możliwości routingu.

Przed rozpoczęciem dostosowywania, routing adresów URL, do przykładowej aplikacji Wingtip Toys można połączyć z produktu przy użyciu następującego adresu URL:

`https://localhost:44300/ProductDetails.aspx?productID=2`

Dostosowując routingu adresów URL przykładowej aplikacji Wingtip Toys połączy się z tego samego produktu przy użyciu łatwiej odczytać adresu URL:

`https://localhost:44300/Product/Convertible%20Car`

### <a name="routes"></a>Trasy

Trasa jest wzorzec adresu URL, który jest zamapowany program obsługi. Program obsługi może być plikiem fizycznym, takie jak plik .aspx w aplikacji formularzy sieci Web. Program obsługi może być również klasę, która przetwarza żądanie. Aby zdefiniować trasę, należy utworzyć wystąpienia klasy trasy określając wzorzec adresu URL, program obsługi i opcjonalnie nazwy trasy.

Dodać trasę do aplikacji, dodając `Route` obiektu statycznej `Routes` właściwość `RouteTable` klasy. Właściwość trasy jest `RouteCollection` obiekt, który przechowuje wszystkie trasy dla aplikacji.

### <a name="url-patterns"></a>Wzorce adresów URL

Wzorzec URL może zawierać wartości literałów i zmiennych symbolami zastępczymi (określane jako parametry adresu URL). Literały i symbole zastępcze znajdują się w segmentach adresu URL, które są rozdzielane znakami ukośnika (`/`) znaków.

Po wysłaniu żądania do aplikacji sieci web, adres URL jest analizowany na segmenty i symbole zastępcze i wartości zmiennych są dostarczane do obsługi żądania. Ten proces jest podobny sposób dane w ciągu zapytania zostanie przeanalizowana i przekazywane do programu obsługi żądania. W obu przypadkach informacji o zmiennej jest uwzględniona w adresie URL i przekazywane do programu obsługi w postaci par klucz wartość. Dla ciągów zapytań klucze i wartości są w adresie URL. W przypadku tras klucze są zdefiniowane we wzorcu adres URL nazwy symbolu zastępczego, a tylko wartości są w adresie URL.

We wzorcu adresu URL zdefiniować symbole zastępcze, umieszczając je w nawiasach klamrowych ( `{` i `}` ). Można zdefiniować więcej niż jeden symbol zastępczy w segmencie, ale symbole zastępcze muszą być rozdzielone wartości literału. Na przykład `{language}-{country}/{action}` jest wzorzec prawidłową trasę. Jednak `{language}{country}/{action}` nie jest prawidłowy wzorzec, ponieważ nie ma wartość literału lub ogranicznik między symbole zastępcze. W związku z tym routingu nie może ustalić, gdzie można oddzielić wartość symbolu zastępczego języka od wartość symbolu zastępczego kraju.

### <a name="mapping-and-registering-routes"></a>Mapowanie i rejestrując trasy

Przed dołączeniem trasy do stron przykładowej aplikacji Wingtip Toys, należy zarejestrować trasy, podczas uruchamiania aplikacji. Aby zarejestrować trasy, należy zmodyfikować `Application_Start` programu obsługi zdarzeń.

1. W **Eksploratora rozwiązań**programu Visual Studio, należy znaleźć i otworzyć *Global.asax.cs* pliku.
2. Dodaj kod wyróżniony na żółto do *Global.asax.cs* pliku w następujący sposób:   

    [!code-csharp[Main](url-routing/samples/sample1.cs?highlight=30-31,34-46)]

Kiedy Wingtip Toys przykładowy uruchamiania aplikacji, wywołuje `Application_Start` programu obsługi zdarzeń. Na końcu tej obsługi zdarzeń `RegisterCustomRoutes` metoda jest wywoływana. `RegisterCustomRoutes` Metoda dodaje każdej trasy przez wywołanie metody `MapPageRoute` metody `RouteCollection` obiektu. Trasy są definiowane przy użyciu nazwy trasy adresu URL trasy i fizyczny adres URL.

Pierwszym parametrem ("`ProductsByCategoryRoute`") jest nazwą trasy. Służy do wywołania trasy, gdy jest to konieczne. Drugim parametrem ("`Category/{categoryName}`") definiuje przyjazna zastępuje adres URL, który może być dynamiczny na podstawie kodu. Za pomocą tej trasy jest wypełnianie kontrolki danych wraz z łączami, które są generowane na podstawie danych. Trasy zostały przedstawione w następujący sposób:

[!code-csharp[Main](url-routing/samples/sample2.cs)]

Drugi parametr trasy zawiera wartość dynamiczna określony przez nawiasy klamrowe (`{ }`). W tym przypadku `categoryName` to zmienna, która będzie służyć do określenia prawidłowego ścieżki routingu.

> [!NOTE] 
> 
> **Optional**
> 
> Użytkownik może być łatwiejsze do zarządzania kodem, przenosząc `RegisterCustomRoutes` metodę do osobnej klasy. W *logiki* folderze utwórz osobne `RouteActions` klasy. Przenieś powyżej `RegisterCustomRoutes` metody z *Global.asax.cs* pliku do nowego `RoutesActions` klasy. Użyj `RoleActions` klasy i `createAdmin` metody, na przykład sposób wywoływania `RegisterCustomRoutes` metody z *Global.asax.cs* pliku.


Należy również zauważyć `RegisterRoutes` przy użyciu wywołania metody `RouteConfig` obiektu na początku `Application_Start` programu obsługi zdarzeń. To wywołanie jest wprowadzone do zaimplementowania domyślny routing. Została włączona jako domyślny kod podczas tworzenia aplikacji przy użyciu szablonu formularzy sieci Web programu Visual Studio.

## <a name="retrieving-and-using-route-data"></a>Pobieranie i korzystanie z danych trasy

Jak wspomniano powyżej, można zdefiniować trasy. Kod, który został dodany do `Application_Start` programu obsługi zdarzeń w *Global.asax.cs* trasy definiowane przez załadowaniu pliku.

### <a name="setting-routes"></a>Ustawienie trasy

Trasy trzeba dodać dodatkowy kod. W tym samouczku użyjesz powiązanie modelu, aby pobrać `RouteValueDictionary` obiekt, który jest używany podczas generowania tras przy użyciu danych z kontrolki danych. `RouteValueDictionary` Obiektu będzie zawierać listę nazw produktów, które należą do określonej kategorii produktów. Łącze jest tworzony dla każdego produktu, na podstawie danych i trasy.

#### <a name="enable-routes-for-categories-and-products"></a>Włącz trasy dla kategorii i produkty

Następnie zaktualizujesz aplikację do korzystania z `ProductsByCategoryRoute` określenie poprawne trasy obejmujący dla każdego linku kategorii produktów. Zostaną również zaktualizowane *ProductList.aspx* strony, aby uwzględnić łącze routingu dla każdego produktu. Linki pojawi się znajdowały się przed zmianą, jednak łącza będą teraz korzystały z routingu adresów URL.

1. W **Eksploratora rozwiązań**, otwórz *Site.Master* strony, jeśli nie jest już otwarty.
2. Aktualizacja **ListView** formantu o nazwie "`categoryList`" ze zmianami wyróżniony na żółto, więc znaczników wygląda następująco:   

    [!code-aspx[Main](url-routing/samples/sample3.aspx?highlight=7-9)]
3. W **Eksploratora rozwiązań**, otwórz *ProductList.aspx* strony.
4. Aktualizacja `ItemTemplate` elementu *ProductList.aspx* strony za pomocą aktualizacji wyróżniony na żółto, więc znaczników pojawia się w następujący sposób:   

    [!code-aspx[Main](url-routing/samples/sample4.aspx?highlight=6-9,14-16)]
5. Otwórz związanym kodzie *ProductList.aspx.cs* i dodaj następujące przestrzeni nazw jako wyróżniane na żółty:  

    [!code-csharp[Main](url-routing/samples/sample5.cs?highlight=9)]
6. Zastąp `GetProducts` metoda kodu powiązanego (*ProductList.aspx.cs*) z następującym kodem:   

    [!code-csharp[Main](url-routing/samples/sample6.cs)]

#### <a name="add-code-for-product-details"></a>Dodaj kod, aby uzyskać szczegółowe informacje o produkcie

Teraz zaktualizuj kodu powiązanego (*ProductDetails.aspx.cs*) dla *ProductDetails.aspx* strony na korzystanie z danych trasy. Należy zauważyć, że nowe `GetProduct` metoda akceptuje także wartość ciągu zapytania, dla której użytkownik ma link zakładek przypadek, który używa starszej adresu URL innych niż przyjazne, nietrasowanego.

1. Zastąp `GetProduct` metoda kodu powiązanego (*ProductDetails.aspx.cs*) z następującym kodem:   

    [!code-csharp[Main](url-routing/samples/sample7.cs)]

## <a name="running-the-application"></a>Uruchamianie aplikacji

Można uruchomić aplikację teraz, aby zobaczyć zaktualizowane trasy.

1. Naciśnij klawisz **F5** uruchamianie przykładowej aplikacji Wingtip Toys.  
 Przeglądarki otwiera się i pokazuje *Default.aspx* strony.
2. Kliknij przycisk **produktów** widocznego u góry strony.  
 Wszystkie produkty są wyświetlane na *ProductList.aspx* strony. Następujący adres URL (przy użyciu numeru portu) zostanie wyświetlona w przeglądarce:  
    `https://localhost:44300/ProductList`
3. Następnie kliknij przycisk **samochodów** łącze kategorii w górnej części strony.  
 Tylko samochodów są wyświetlane na *ProductList.aspx* strony. Następujący adres URL (przy użyciu numeru portu) zostanie wyświetlona w przeglądarce:  
    `https://localhost:44300/Category/Cars`
4. Kliknij link, zawierające nazwę pierwszego samochodu wymienione na stronie ("**samochodu przekonwertować**") aby wyświetlić szczegóły produktu.  
 Następujący adres URL (przy użyciu numeru portu) zostanie wyświetlona w przeglądarce:  
    `https://localhost:44300/Product/Convertible%20Car`
5. Następnie wprowadź następujący nietrasowanego adres URL (przy użyciu numeru portu) do przeglądarki:  
    `https://localhost:44300/ProductDetails.aspx?productID=2`  
 Kod nadal rozpoznaje adres URL, który zawiera ciąg zapytania w przypadku, gdy użytkownik ma link zakładek.

## <a name="summary"></a>Podsumowanie

W tym samouczku zostały dodane trasy dla kategorii i produktów. Wiesz, jak trasy można zintegrować z formantami danych, korzystające z wiązania modelu. W następnym samouczku wdroży obsługi błędów globalnego.

## <a name="additional-resources"></a>Dodatkowe zasoby

[ASP.NET, przyjazne adresy URL](http://www.nuget.org/packages/Microsoft.AspNet.FriendlyUrls/)  
[Wdrażanie aplikacji formularzy bezpiecznej sieci Web platformy ASP.NET z członkostwa, uwierzytelnianiem OAuth i bazą danych SQL w usłudze Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Microsoft Azure — bezpłatna wersja próbna](https://azure.microsoft.com/pricing/free-trial/)

> [!div class="step-by-step"]
> [Poprzednie](membership-and-administration.md)
> [dalej](aspnet-error-handling.md)
