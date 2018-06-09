---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
title: Wywoływanie usługi OData z klienta programu .NET (C#) | Dokumentacja firmy Microsoft
author: MikeWasson
description: Ten samouczek pokazuje sposób wywoływania usługi OData od aplikacji klienckiej C#. Wersje oprogramowania używany w samouczek Visual Studio 2013 (działa Visual S...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/26/2014
ms.topic: article
ms.assetid: 6f448917-ad23-4dcc-9789-897fad74051b
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: 497102cfa98680f2156a56ff9e36d84b7c820020
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "28042397"
---
<a name="calling-an-odata-service-from-a-net-client-c"></a>Wywoływanie usługi OData z klienta programu .NET (C#)
====================
przez [Wasson Jan](https://github.com/MikeWasson)

[Pobieranie ukończone projektu](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> Ten samouczek pokazuje sposób wywoływania usługi OData od aplikacji klienckiej C#.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Używane w samouczku wersje oprogramowania
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (współpracuje z programu Visual Studio 2012)
> - [Biblioteka klienta usług danych WCF](https://msdn.microsoft.com/library/cc668772.aspx)
> - Składnik Web API 2. (Przykład usługi OData jest utworzony przy użyciu 2 interfejsu API sieci Web, ale aplikacja kliencka nie zależy od interfejsu API sieci Web).


W tym samouczku I będzie przeprowadzenie tworzenia aplikacji klienckiej, która wywołuje usługi OData. Usługa OData udostępnia następujących obiektów:

- `Product`
- `Supplier`
- `ProductRating`

![](calling-an-odata-service-from-a-net-client/_static/image1.png)

Następujące artykuły opisano sposób wdrażania usługi OData w składniku Web API. (Nie trzeba ich zrozumienie tego samouczka, jednak odczytać).

- [Tworzenie punktu końcowego OData w składniku Web API 2](creating-an-odata-endpoint.md)
- [Relacji z jednostką OData w składniku Web API 2](working-with-entity-relations.md)
- [Akcje protokołu OData w interfejsie Web API 2](odata-actions.md)

## <a name="generate-the-service-proxy"></a>Generowanie serwera Proxy usługi

Pierwszym krokiem jest generowanie usługi serwera proxy. Serwer proxy usługi jest klasą .NET, który definiuje metody do uzyskiwania dostępu do usługi OData. Serwer proxy tłumaczy wywołania metody do żądania HTTP.

![](calling-an-odata-service-from-a-net-client/_static/image2.png)

Rozpocznij od otwierania projektu usługi OData w programie Visual Studio. Naciśnij klawisze CTRL + F5, aby uruchomić usługę lokalnie w usługach IIS Express. Należy zwrócić uwagę adresu lokalnego, w tym numer portu, który przypisuje Visual Studio. Podczas tworzenia serwera proxy, należy ten adres.

Następnie otwórz inne wystąpienie programu Visual Studio i utworzyć projekt aplikacji konsoli. Aplikacja konsoli będzie naszej aplikacji klienckiej OData. (Można również dodać projekt do tego samego rozwiązania co usługa.)

> [!NOTE]
> Pozostałe kroki można znaleźć projektu konsoli.


W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **odwołania** i wybierz **Dodaj odwołanie do usługi**.

![](calling-an-odata-service-from-a-net-client/_static/image3.png)

W **Dodaj odwołanie do usługi** okna dialogowego, wpisz adres usługi OData:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample1.cmd)]

gdzie *portu* numer portu.

[![](calling-an-odata-service-from-a-net-client/_static/image5.png)](calling-an-odata-service-from-a-net-client/_static/image4.png)

Aby uzyskać **Namespace**, wpisz "ProductService". Ta opcja definiuje przestrzeń nazw, klasy proxy.

Kliknij przycisk **Przejdź**. Visual Studio odczytuje dokument metadanych OData do odnajdywania obiektów w usłudze.

[![](calling-an-odata-service-from-a-net-client/_static/image7.png)](calling-an-odata-service-from-a-net-client/_static/image6.png)

Kliknij przycisk **OK** do dodania do projektu klasy serwera proxy.

![](calling-an-odata-service-from-a-net-client/_static/image8.png)

## <a name="create-an-instance-of-the-service-proxy-class"></a>Utwórz wystąpienie klasy serwera Proxy usługi

Wewnątrz sieci `Main` metody, Utwórz nowe wystąpienie klasy serwera proxy w następujący sposób:

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample2.cs)]

Użyj numeru portu rzeczywiste którym usługa jest uruchomiona. Podczas wdrażania usługi, zostanie użyty identyfikator URI usługi na żywo. Nie trzeba zaktualizować serwera proxy.

Poniższy kod dodaje obsługi zdarzeń, który Wyświetla identyfikatory URI żądania w oknie konsoli. Ten krok nie jest wymagane, ale jest interesujące wyświetlić identyfikator URI dla każdego zapytania.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample3.cs)]

## <a name="query-the-service"></a>Zapytanie usługi

Poniższy kod umożliwia pobranie listy produktów z usługi OData.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample4.cs)]

Zwróć uwagę, że nie należy do pisania kodu do wysyłania żądań HTTP i przeanalizować odpowiedzi. Klasy serwera proxy jest to automatycznie podczas wyliczania `Container.Products` kolekcji w **foreach** pętli.

Po uruchomieniu aplikacji, dane wyjściowe powinny wyglądać następująco:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample5.cmd)]

Aby uzyskać jednostki według Identyfikatora, należy użyć `where` klauzuli.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample6.cs)]

W pozostałej części tego tematu, I nie będzie zawierać całą `Main` działać tylko kod potrzebne do wywołania tej usługi.

## <a name="apply-query-options"></a>Stosowanie opcji zapytania

Definiuje OData [opcje kwerendy](../supporting-odata-query-options.md) który może służyć do filtrowania, sortowania, dane strony i tak dalej. Na serwerze proxy usług można stosować te opcje przy użyciu różnych wyrażenia LINQ.

W tej sekcji opisano I krótki przykłady. Aby uzyskać więcej informacji, zobacz temat [zagadnienia dotyczące LINQ (usługi danych WCF)](https://msdn.microsoft.com/library/ee622463.aspx) w witrynie MSDN.

### <a name="filtering-filter"></a>Filtrowania ($filter)

Aby filtrować, użyj `where` klauzuli. Następujące przykładowe filtry według kategorii produktów.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample7.cs)]

Ten kod odnosi się do następującego zapytania OData.

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample8.cmd)]

Należy zauważyć, że serwer proxy konwertuje `where` klauzuli do OData `$filter` wyrażenia.

### <a name="sorting-orderby"></a>Sortowanie ($orderby)

Aby posortować, użyj `orderby` klauzuli. Poniższy przykład sortowania cen w kolejności malejącej.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample9.cs)]

Oto odpowiedniego żądania OData.

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample10.cmd)]

### <a name="client-side-paging-skip-and-top"></a>Stronicowania po stronie klienta ($skip ani $top)

Dla zestawów duża jednostka klienta mogą chcieć ograniczyć liczbę wyników. Na przykład klienta mogą być wyświetlane wpisy 10 naraz. Ta metoda jest wywoływana *stronicowania po stronie klienta*. (Dostępne są także [stronicowania po stronie serwera](../supporting-odata-query-options.md#server-paging), gdy serwer ogranicza liczbę wyników.) Aby wykonać stronicowania po stronie klienta, należy użyć LINQ **Pomiń** i **zająć** metody. Poniższy przykład pomija najpierw 40 wyników i przejście dalej 10.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample11.cs)]

Oto odpowiedniego żądania OData:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample12.cmd)]

### <a name="select-select-and-expand-expand"></a>Wybierz ($select) i rozwiń ($expand)

Aby dołączyć powiązanych jednostek, należy użyć `DataServiceQuery<t>.Expand` metody. Na przykład, aby uwzględnić `Supplier` dla każdego `Product`:

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample13.cs)]

Oto odpowiedniego żądania OData:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample14.cmd)]

Aby zmienić kształt odpowiedzi, należy użyć LINQ **wybierz** klauzuli. Poniższy przykład pobiera tylko nazwę każdego produktu bez innych właściwości.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample15.cs)]

Oto odpowiedniego żądania OData:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample16.cmd)]

Klauzula select może zawierać powiązanych jednostek. W takim przypadku nie wywołuj **rozwiń**; w takim przypadku serwer proxy rozszerzenie zawiera automatycznie. Poniższy przykład pobiera nazwę i dostawcy każdego produktu.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample17.cs)]

Oto odpowiedniego żądania OData. Należy zauważyć, że zawiera on **rozwiń $** opcji.

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample18.cmd)]

Aby uzyskać więcej informacji na temat $select i $expand Rozwiń, zobacz [przy użyciu $select, rozwinąć $ i $value w sieci Web API 2](../using-select-expand-and-value.md).

## <a name="add-a-new-entity"></a>Dodaj nową jednostkę

Aby dodać nową jednostkę do zbioru jednostek, należy wywołać `AddToEntitySet`, gdzie *EntitySet* jest nazwą zestawu jednostek. Na przykład `AddToProducts` dodaje nowy `Product` do `Products` zestawu jednostek. Podczas generowania serwera proxy usługi danych WCF automatycznie tworzy następujące jednoznacznie **AddTo** metody.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample19.cs)]

Aby dodać łącze między dwiema jednostkami, należy użyć **metody AddLink** i **SetLink** metody. Poniższy kod dodaje nowego dostawcy i nowego produktu, a następnie tworzy linki między nimi.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample20.cs)]

Użyj **metody AddLink** przypadku właściwości nawigacji kolekcji. W tym przykładzie dodajemy produkt `Products` kolekcji na dostawcy.

Użyj **SetLink** gdy właściwość nawigacji jest pojedynczą jednostką. W tym przykładzie konfigurujemy ustawienia `Supplier` właściwości dla produktu.

## <a name="update--patch"></a>Aktualizacji lub poprawki

Aby zaktualizować jednostkę, należy wywołać **UpdateObject** metody.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample21.cs)]

Aktualizacja jest wykonywana po wywołaniu **SaveChanges**. Domyślnie program WCF wysyła żądanie HTTP scalania. **PatchOnUpdate** opcja nakazuje WCF, aby zamiast wysyłania HTTP PATCH.

> [!NOTE]
> Dlaczego PATCH i MERGE? Pierwotnej specyfikacji protokołu HTTP 1.1 ([RCF 2616](http://tools.ietf.org/html/rfc2616)) nie zdefiniowano żadnych metody HTTP z semantyki "częściowej aktualizacji". Aby obsługiwać aktualizacje częściowe, specyfikację OData zdefiniowane MERGE — metoda. W 2010 r. [RFC 5789](http://tools.ietf.org/html/rfc5789) zdefiniowane poprawki metody częściowej aktualizacji. Można znaleźć niektórych historii w tym [wpis w blogu](https://blogs.msdn.com/b/astoriateam/archive/2008/05/20/merge-vs-replace-semantics-for-update-operations.aspx) w blogu usługi danych WCF. Obecnie poprawka jest preferowana przez scalania. Kontrolera OData utworzonego przez funkcję szkieletów interfejsu API sieci Web obsługuje obie metody.


Jeśli chcesz zamienić całej jednostki (PUT semantyki), określ **ReplaceOnUpdate** opcji. Powoduje to WCF można wysłać żądania HTTP PUT.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample22.cs)]

## <a name="delete-an-entity"></a>Usuwanie jednostki

Aby usunąć jednostkę, wywołaj **DeleteObject**.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample23.cs)]

## <a name="invoke-an-odata-action"></a>Wywołaj akcję OData

W protokole OData [akcje](odata-actions.md) służą do dodawania zachowań po stronie serwera, które łatwo nie są zdefiniowane jako operacje CRUD na jednostkach.

Mimo że ten dokument metadanych OData opisano akcje, klasy serwera proxy nie tworzy żadnych metod jednoznacznie dla nich. Nadal można wywołać akcję OData przy użyciu ogólnej **Execute** metody. Należy znać typów danych parametrów i zwracanych wartości.

Na przykład `RateProduct` akcja ma parametr o nazwie "Klasyfikacji" typu `Int32` i zwraca `double`. Poniższy kod przedstawia sposób wywołania tej akcji.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample24.cs)]

Aby uzyskać więcej informacji, zobacz[wywoływanie operacji usługi i działania](https://msdn.microsoft.com/library/hh230677.aspx).

Jedną z opcji jest rozszerzenie **kontenera** klasy zapewnienie silnie typizowane metody, która wywołuje akcję:

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample25.cs)]
