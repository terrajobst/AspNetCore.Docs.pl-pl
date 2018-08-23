---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
title: Tworzenie punktu końcowego OData v4 przy użyciu wzorca ASP.NET Web API 2.2 | Dokumentacja firmy Microsoft
author: MikeWasson
description: Open Data Protocol (OData) to protokół dostępu do danych w sieci Web. Protokół OData zapewnia jednolity sposób, w celu wykonywania zapytań i manipulowania zestawami danych przy użyciu operacji CRUD...
ms.author: riande
ms.date: 06/24/2014
ms.assetid: 1e1927c0-ded1-4752-80fd-a146628d2f09
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
msc.type: authoredcontent
ms.openlocfilehash: 7f2d0b8fa8ac290e5018cb5237b1fedb5f40eeb0
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752703"
---
<a name="create-an-odata-v4-endpoint-using-aspnet-web-api-22"></a>Tworzenie punktu końcowego OData v4 przy użyciu wzorca ASP.NET Web API 2.2
====================
przez [Mike Wasson](https://github.com/MikeWasson)

> Open Data Protocol (OData) to protokół dostępu do danych w sieci Web. Protokół OData zapewnia jednolity sposób, w celu wykonywania zapytań i manipulowania zestawami danych przy użyciu operacji CRUD (tworzenia, odczytu, aktualizacji i usuwania).
> 
> Web API platformy ASP.NET obsługuje zarówno w wersji 3 i 4 protokołu. Można nawet mieć punktu końcowego w wersji 4, który uruchamia side-by-side z punktem końcowym v3.
> 
> W tym samouczku przedstawiono sposób tworzenia punktu końcowego OData v4 obsługującego operacje CRUD.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używanego w tym samouczku
> 
> 
> - Składnik Web API 2.2
> - Protokołu OData v4
> - [Visual Studio 2013 Update 2](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - Entity Framework 6
> - .NET 4.5
> 
> 
> ## <a name="tutorial-versions"></a>Samouczek wersji
> 
> Dla protokołu OData w wersji 3, zobacz [Tworzenie punktu końcowego OData v3](../odata-v3/creating-an-odata-endpoint.md).


## <a name="create-the-visual-studio-project"></a>Tworzenie projektu programu Visual Studio

W programie Visual Studio z **pliku** menu, wybierz opcję **New** &gt; **projektu**.

Rozwiń **zainstalowane** &gt; **szablony** &gt; **Visual C#** &gt; **Web**i wybierz  **Aplikacja sieci Web ASP.NET** szablonu. Nadaj projektowi nazwę &quot;ProductService&quot;.

[![](create-an-odata-v4-endpoint/_static/image2.png)](create-an-odata-v4-endpoint/_static/image1.png)

W **nowy projekt** okno dialogowe, wybierz opcję **pusty** szablonu. W obszarze &quot;. Dodaj foldery i podstawowe odwołania... &quot;, kliknij przycisk **interfejsu API sieci Web**. Kliknij przycisk **OK**.

[![](create-an-odata-v4-endpoint/_static/image4.png)](create-an-odata-v4-endpoint/_static/image3.png)

## <a name="install-the-odata-packages"></a>Instalowanie pakietów OData

Z **narzędzia** menu, wybierz opcję **Menedżera pakietów NuGet** &gt; **Konsola Menedżera pakietów**. W oknie Konsola Menedżera pakietów wpisz polecenie:

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample1.cmd)]

To polecenie powoduje zainstalowanie najnowszych pakietów OData NuGet.

## <a name="add-a-model-class"></a>Dodawanie klasy modelu

A *modelu* jest obiektem, który reprezentuje jednostki danych w aplikacji.

W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy folderu modeli. Z menu kontekstowego wybierz **Dodaj** &gt; **klasy**.

[![](create-an-odata-v4-endpoint/_static/image6.png)](create-an-odata-v4-endpoint/_static/image5.png)

> [!NOTE]
> Zgodnie z Konwencją klasy modelu są umieszczane w folderze modeli, ale nie musisz przestrzegać tej Konwencji we własnych projektach.


Nazwa klasy `Product`. W pliku Product.cs Zastąp standardowy kod następujących czynności:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample2.cs)]

`Id` Właściwość jest kluczem jednostki. Klienci mogą wysyłać zapytania jednostek według klucza. Na przykład, aby uzyskać produkt o identyfikatorze 5, identyfikator URI jest `/Products(5)`. `Id` Właściwość będzie również klucz podstawowy w wewnętrznej bazy danych.

## <a name="enable-entity-framework"></a>Włączanie programu Entity Framework

W tym samouczku użyjemy Entity Framework (EF) Code First platformy do utworzenia wewnętrznej bazy danych.

> [!NOTE]
> Web API OData nie wymaga EF. Użyj dowolnej warstwy dostępu do danych, która może dokonywać translacji jednostek bazy danych modeli.


Najpierw zainstaluj pakiet NuGet dla platformy EF. Z **narzędzia** menu, wybierz opcję **Menedżera pakietów NuGet** &gt; **Konsola Menedżera pakietów**. W oknie Konsola Menedżera pakietów wpisz polecenie:

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample3.cmd)]

Otwórz plik Web.config i dodaj następującą sekcję wewnątrz **konfiguracji** elementu po **configSections** elementu.

[!code-xml[Main](create-an-odata-v4-endpoint/samples/sample4.xml?highlight=6)]

To ustawienie dodaje parametry połączenia dla bazy danych LocalDB. Ta baza danych będzie służyć lokalnie uruchomić aplikację.

Następnie Dodaj klasę o nazwie `ProductsContext` do folderu modeli:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample5.cs)]

W Konstruktorze `"name=ProductsContext"` nadaje nazwę parametrów połączenia.

## <a name="configure-the-odata-endpoint"></a>Skonfiguruj punkt końcowy OData

Otwórz plik App\_Start/WebApiConfig.cs. Dodaj następujący kod **przy użyciu** instrukcji:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample6.cs)]

Następnie dodaj następujący kod do **zarejestrować** metody:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample7.cs)]

Ten kod wykonuje dwie czynności:

- Tworzy model Entity Data Model (EDM struktury).
- Dodaje trasę.

EDM jest abstrakcyjny model danych. EDM jest używany do utworzenia dokumentu metadanych usługi. **ODataConventionModelBuilder** klasy tworzy EDM przy użyciu domyślnych konwencji nazewnictwa. Takie podejście wymaga co najmniej kodu. Jeśli chcesz, aby mieć większą kontrolę nad EDM, możesz użyć **element ODataModelBuilder** klasy w celu utworzenia EDM, jawnie dodając właściwości, kluczy i właściwości nawigacji.

A *trasy* zawiera informacje dotyczące określenia trasy żądań HTTP do punktu końcowego interfejsu API sieci Web. Aby utworzyć trasę OData v4, wywołaj **MapODataServiceRoute** — metoda rozszerzenia.

Jeśli aplikacja ma wiele punktów końcowych OData, Utwórz oddzielne trasy dla każdego. Nadaj każdej trasy trasy unikatową nazwę i prefiksu.

## <a name="add-the-odata-controller"></a>Dodawanie kontrolera OData

A *kontrolera* to klasa, która obsługuje żądania HTTP. Możesz utworzyć oddzielne kontrolera dla każdej jednostki w usługi OData. W tym samouczku utworzysz jednego kontrolera dla `Product` jednostki.

W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy folder kontrolerów, a następnie wybierz **Dodaj** &gt; **klasy**. Nazwa klasy `ProductsController`.

> [!NOTE]
> Wersja tego samouczka dla protokołu OData v3 używa **Dodaj kontroler** tworzenia szkieletów. Obecnie nie ma żadnych funkcją szkieletów dla protokołu OData v4.


Zamień schematyczny kod w ProductsController.cs poniżej.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample8.cs)]

Zastosowań kontrolera `ProductsContext` klasy dostępu do bazy danych przy użyciu programu EF. Należy zauważyć, że kontroler zastępuje **Dispose** metodę, aby usunąć **ProductsContext**.

Jest to punkt początkowy dla kontrolera. Następnie dodamy metody dla wszystkich operacji CRUD.

## <a name="querying-the-entity-set"></a>Zapytanie dotyczące zestawu jednostek

Dodaj następujące metody umożliwiające `ProductsController`.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample9.cs)]

Bez parametrów wersję `Get` metoda zwraca całą kolekcję produktów. `Get` Metody z *klucz* parametru wyszukuje produktu według jego klucza (w tym przypadku `Id` właściwości).

**[EnableQuery]** atrybut umożliwia klientom zmodyfikować zapytanie, za pomocą opcji zapytania $filter, $sort i $page. Aby uzyskać więcej informacji, zobacz [obsługi opcji zapytania OData](../supporting-odata-query-options.md).

## <a name="adding-an-entity-to-the-entity-set"></a>Dodawanie jednostki do zestawu jednostek

Aby umożliwić klientom dodać nowy produkt do bazy danych, dodaj następującą metodę do `ProductsController`.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample10.cs)]

## <a name="updating-an-entity"></a>Aktualizowanie jednostki

OData obsługuje dwie semantykę różną w przypadku aktualizowania jednostki, poprawki i PUT.

- POPRAWKA wykonuje częściową aktualizację. Klient określa tylko właściwości do zaktualizowania.
- Umieść zastępuje całej jednostki.

Wadą PUT jest, klient musi wysłać wartości dla wszystkich właściwości w obiekcie, łącznie z wartościami, które nie zmieniają się. [Specyfikacji protokołu OData](http://docs.oasis-open.org/odata/odata/v4.0/os/part1-protocol/odata-v4.0-os-part1-protocol.html#_Toc372793719) stwierdzający, że poprawka jest preferowana.

W każdym przypadku poniżej przedstawiono kod dla metody PUT i PATCH:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample11.cs)]

W przypadku poprawek, używa kontrolera **Delta&lt;T&gt;**  typ do śledzenia zmian.

## <a name="deleting-an-entity"></a>Usuwanie jednostki

Aby umożliwić klientom usunąć produkt z bazy danych, dodaj następującą metodę do `ProductsController`.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample12.cs)]
