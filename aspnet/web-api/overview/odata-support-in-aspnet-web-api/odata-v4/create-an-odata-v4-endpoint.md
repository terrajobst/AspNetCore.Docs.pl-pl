---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
title: "Tworzenie punktu końcowego OData v4 używanie składnika ASP.NET Web API 2.2 | Dokumentacja firmy Microsoft"
author: MikeWasson
description: "Open Data Protocol (OData) to protokół dostępu do danych w sieci Web. OData zapewnia jednolity sposób, w celu wykonywania zapytań i manipulowania zestawów danych za pośrednictwem operacji CRUD..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/24/2014
ms.topic: article
ms.assetid: 1e1927c0-ded1-4752-80fd-a146628d2f09
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
msc.type: authoredcontent
ms.openlocfilehash: a3f94818f9674b0e1e9a45b2a6cc9455edc79726
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="create-an-odata-v4-endpoint-using-aspnet-web-api-22"></a>Tworzenie punktu końcowego OData v4 używanie składnika ASP.NET Web API 2.2
====================
przez [Wasson Jan](https://github.com/MikeWasson)

> Open Data Protocol (OData) to protokół dostępu do danych w sieci Web. OData zapewnia jednolity sposób, w celu wykonywania zapytań i manipulowania zestawów danych za pośrednictwem operacji CRUD (tworzenia, odczytu, aktualizacji i usuwania).
> 
> Interfejs API sieci Web platformy ASP.NET obsługuje zarówno w wersji 3, jak i w wersji 4 protokołu. Można także ustawić punkt końcowy v4 uruchomioną side-by-side z punktem końcowym v3.
> 
> W tym samouczku przedstawiono sposób tworzenia punktu końcowego v4 OData, która obsługuje operacje CRUD.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Używane w samouczku wersje oprogramowania
> 
> 
> - 2.2 interfejsu API sieci Web
> - Protokołu OData v4
> - [Visual Studio 2013 Update 2](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - Entity Framework 6
> - .NET 4.5
> 
> 
> ## <a name="tutorial-versions"></a>Samouczek wersji
> 
> Dla OData w wersji 3, zobacz [tworzenia punktu końcowego OData v3](../odata-v3/creating-an-odata-endpoint.md).


## <a name="create-the-visual-studio-project"></a>Tworzenie projektu programu Visual Studio

W programie Visual Studio z **pliku** menu, wybierz opcję **nowy** &gt; **projektu**.

Rozwiń węzeł **zainstalowana** &gt; **szablony** &gt; **Visual C#** &gt; **Web**i wybierz  **Aplikacja sieci Web ASP.NET** szablonu. Nazwij projekt &quot;ProductService&quot;.

[![](create-an-odata-v4-endpoint/_static/image2.png)](create-an-odata-v4-endpoint/_static/image1.png)

W **nowy projekt** okno dialogowe, wybierz opcję **pusty** szablonu. W obszarze &quot;. Dodaj foldery i podstawowe odwołania... &quot;, kliknij przycisk **interfejsu API sieci Web**. Kliknij przycisk **OK**.

[![](create-an-odata-v4-endpoint/_static/image4.png)](create-an-odata-v4-endpoint/_static/image3.png)

## <a name="install-the-odata-packages"></a>Zainstaluj pakiety OData

Z **narzędzia** menu, wybierz opcję **Menedżera pakietów NuGet** &gt; **Konsola Menedżera pakietów**. W oknie Konsola Menedżera pakietów wpisz:

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample1.cmd)]

To polecenie powoduje zainstalowanie najnowszych pakietów OData NuGet.

## <a name="add-a-model-class"></a>Dodaj klasę modelu

A *modelu* jest obiekt, który reprezentuje jednostkę danych w aplikacji.

W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy folder modeli. Wybierz z menu kontekstowego **Dodaj** &gt; **klasy**.

[![](create-an-odata-v4-endpoint/_static/image6.png)](create-an-odata-v4-endpoint/_static/image5.png)

> [!NOTE]
> Według Konwencji klasy modeli są umieszczane w folderze modele, ale nie trzeba wykonywać tę Konwencję do własnych projektów.


Nazwa klasy `Product`. W pliku Product.cs Zastąp schematyczny kod poniżej:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample2.cs)]

`Id` Właściwość jest kluczem jednostki. Klienci mogą wykonywać kwerendę jednostek według klucza. Na przykład, aby uzyskać produktu o identyfikatorze 5, identyfikator URI jest `/Products(5)`. `Id` Właściwości będzie również klucza podstawowego w bazie danych zaplecza.

## <a name="enable-entity-framework"></a>Włączanie programu Entity Framework

W tym samouczku użyjemy Entity Framework (EF) Code First można utworzyć bazy danych zaplecza.

> [!NOTE]
> Web API OData nie wymaga EF. Użyj dowolnej warstwy dostępu do danych, która może dokonywać translacji jednostki bazy danych do modeli.


Najpierw zainstaluj pakiet NuGet dla EF. Z **narzędzia** menu, wybierz opcję **Menedżera pakietów NuGet** &gt; **Konsola Menedżera pakietów**. W oknie Konsola Menedżera pakietów wpisz:

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample3.cmd)]

Otwórz plik Web.config i dodaj następującą sekcję wewnątrz **konfiguracji** elementu, po **configSections** elementu.

[!code-xml[Main](create-an-odata-v4-endpoint/samples/sample4.xml?highlight=6)]

To ustawienie dodaje ciąg połączenia bazy danych LocalDB. Ta baza danych będzie można użyć, gdy lokalne uruchamianie aplikacji.

Następnie Dodaj klasę o nazwie `ProductsContext` do folderu modeli:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample5.cs)]

W Konstruktorze `"name=ProductsContext"` nadaje nazwę parametrów połączenia.

## <a name="configure-the-odata-endpoint"></a>Konfigurowanie punktu końcowego OData

Otwórz plik aplikacji\_Start/WebApiConfig.cs. Dodaj następujące **przy użyciu** instrukcji:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample6.cs)]

Następnie dodaj następujący kod, aby **zarejestrować** metody:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample7.cs)]

Ten kod wykonuje dwie czynności:

- Tworzy modelu Entity Data Model (EDM).
- Dodaje trasę.

EDM jest abstrakcyjny modelu danych. EDM służy do tworzenia dokumentu metadanych usługi. **ODataConventionModelBuilder** klasy tworzy EDM przy użyciu domyślnych konwencji nazewnictwa. Ta metoda wymaga co najmniej kodu. Jeśli chcesz mieć większą kontrolę nad EDM, możesz użyć **element ODataModelBuilder** klasy można utworzyć przez dodanie jawnie właściwości, kluczy i właściwości nawigacji EDM.

A *trasy* informuje interfejsu API sieci Web, jak można przekierować żądania HTTP do punktu końcowego. Aby utworzyć trasę OData v4, należy wywołać **MapODataServiceRoute** — metoda rozszerzenia.

Jeśli aplikacja ma wiele punktów końcowych OData, Utwórz oddzielne trasy dla każdego. Nadaj każdej trasy trasy unikatową nazwę i prefiksu.

## <a name="add-the-odata-controller"></a>Dodawanie kontrolera OData

A *kontrolera* jest klasa, która obsługuje żądania HTTP. Możesz utworzyć oddzielne kontrolera dla każdego obiektu w usłudze OData. W tym samouczku utworzysz jeden kontroler dla `Product` jednostki.

W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy folder kontrolery, a następnie wybierz **Dodaj** &gt; **klasy**. Nazwa klasy `ProductsController`.

> [!NOTE]
> Wersja tego samouczka dla OData v3 używa **Dodaj kontroler** szkieletów. Obecnie nie są nie funkcją szkieletów dla protokołu OData v4.


Zamień schematyczny kod w ProductsController.cs poniżej.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample8.cs)]

Używa kontrolera `ProductsContext` klasy dostęp do bazy danych przy użyciu EF. Należy zauważyć, że kontroler zastępuje **Dispose** metody zlikwidować **ProductsContext**.

To jest punkt początkowy dla kontrolera. Następnie dodamy metod dla wszystkich operacji CRUD.

## <a name="querying-the-entity-set"></a>Zapytanie dotyczące zestawu jednostek

Dodaj następujące metody umożliwiające `ProductsController`.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample9.cs)]

Wersja bez parametrów `Get` metoda zwraca całą kolekcję produktów. `Get` Metody z *klucza* parametru wyszukuje produktu według jego klucza (w tym przypadku `Id` właściwości).

**[EnableQuery]** atrybutu umożliwia klientom zmodyfikować zapytanie, za pomocą opcji zapytania $filter, $sort i $page. Aby uzyskać więcej informacji, zobacz [obsługi opcji zapytania OData](../supporting-odata-query-options.md).

## <a name="adding-an-entity-to-the-entity-set"></a>Dodawanie jednostki do zestawu jednostek

Aby umożliwić klientom dodawanie nowego produktu do bazy danych, dodaj następującą metodę do `ProductsController`.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample10.cs)]

## <a name="updating-an-entity"></a>Aktualizowanie jednostki

OData obsługuje dwa semantykę różną aktualizowania jednostki, poprawki i PUT.

- POPRAWKA wykonuje częściowej aktualizacji. Klient określa tylko właściwości do zaktualizowania.
- Umieść zastępuje całej jednostki.

Wadą PUT jest klient musi wysłać wartości dla wszystkich właściwości w obiekcie, w tym wartości, które nie są zmieniane. [Specyfikację OData](http://docs.oasis-open.org/odata/odata/v4.0/os/part1-protocol/odata-v4.0-os-part1-protocol.html#_Toc372793719) stwierdza, że poprawka jest preferowana.

W każdym przypadku Oto kod dla metody zarówno poprawki i PUT:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample11.cs)]

W przypadku poprawek, używa kontrolera **Delta&lt;T&gt;**  typ do śledzenia zmian.

## <a name="deleting-an-entity"></a>Usuwanie jednostki

Aby umożliwić klientom usuwanie produktu z bazy danych, dodaj następującą metodę do `ProductsController`.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample12.cs)]
