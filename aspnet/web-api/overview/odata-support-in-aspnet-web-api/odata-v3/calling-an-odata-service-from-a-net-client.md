---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
title: Wywoływanie usługi protokołu OData z klienta programu .NET (C#) | Dokumentacja firmy Microsoft
author: MikeWasson
description: W tym samouczku pokazano, jak wywołać usługę OData z poziomu aplikacji klienckiej języka C#. Wersje oprogramowania używany w samouczku programu Visual Studio 2013 (działa Visual S...
ms.author: aspnetcontent
ms.date: 02/26/2014
ms.assetid: 6f448917-ad23-4dcc-9789-897fad74051b
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: 72dca7dc2ec27f15c9f0526621a713267f5835f8
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37819558"
---
<a name="calling-an-odata-service-from-a-net-client-c"></a>Wywoływanie usługi protokołu OData z klienta programu .NET (C#)
====================
przez [Mike Wasson](https://github.com/MikeWasson)

[Pobieranie ukończone projektu](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> W tym samouczku pokazano, jak wywołać usługę OData z poziomu aplikacji klienckiej języka C#.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używanego w tym samouczku
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (w programach Visual Studio 2012)
> - [Biblioteka klienta usług danych WCF](https://msdn.microsoft.com/library/cc668772.aspx)
> - Interfejsu Web API 2. (Przykład usługi OData został skompilowany przy użyciu Web API 2, ale aplikacja kliencka nie zależy od interfejsu API sieci Web).


W ramach tego samouczka I zawiera opis kroków tworzenia aplikacji klienckiej, która wywołuje usługę OData. Usługa OData przedstawia następujących elementach:

- `Product`
- `Supplier`
- `ProductRating`

![](calling-an-odata-service-from-a-net-client/_static/image1.png)

Następujące artykuły zawierają instrukcje dotyczące wdrażania usługi OData w interfejsie API sieci Web. (Nie trzeba ich zrozumienie tego samouczka, jednak odczytać).

- [Tworzenie punktu końcowego OData w interfejsie Web API 2](creating-an-odata-endpoint.md)
- [Relacje jednostek OData w interfejsie Web API 2](working-with-entity-relations.md)
- [Akcje protokołu OData w interfejsie Web API 2](odata-actions.md)

## <a name="generate-the-service-proxy"></a>Generowanie usługi serwera Proxy

Pierwszym krokiem jest generowanie usługi serwera proxy. Serwer proxy usługi jest klasą .NET, która definiuje metody uzyskiwania dostępu do usługi OData. Serwer proxy tłumaczy wywołania metody na żądania HTTP.

![](calling-an-odata-service-from-a-net-client/_static/image2.png)

Rozpocznij, otwierając projekt usługi OData w programie Visual Studio. Naciśnij klawisze CTRL + F5, aby uruchomić usługę lokalnie w usługach IIS Express. Należy pamiętać adresów lokalnych, w tym numer portu, który przypisuje programu Visual Studio. Podczas tworzenia serwera proxy, należy ten adres.

Następnie otwórz inne wystąpienie programu Visual Studio i Utwórz projekt aplikacji konsoli. Aplikacja konsoli będzie naszej aplikacji klienckiej OData. (Możesz również dodać projekt do tego samego rozwiązania co usługa.)

> [!NOTE]
> Pozostałe kroki można znaleźć projektu konsoli.


W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **odwołania** i wybierz **Dodaj odwołanie do usługi**.

![](calling-an-odata-service-from-a-net-client/_static/image3.png)

W **Dodaj odwołanie do usługi** okno dialogowe, wpisz adres usługi OData:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample1.cmd)]

gdzie *portu* jest numerem portu.

[![](calling-an-odata-service-from-a-net-client/_static/image5.png)](calling-an-odata-service-from-a-net-client/_static/image4.png)

Aby uzyskać **Namespace**, wpisz "ProductService". Ta opcja określa przestrzeń nazw, klasy proxy.

Kliknij przycisk **Przejdź**. Program Visual Studio odczytuje dokument metadanych OData, aby odnaleźć obiekty usługi.

[![](calling-an-odata-service-from-a-net-client/_static/image7.png)](calling-an-odata-service-from-a-net-client/_static/image6.png)

Kliknij przycisk **OK** można dodać klasy serwera proxy do projektu.

![](calling-an-odata-service-from-a-net-client/_static/image8.png)

## <a name="create-an-instance-of-the-service-proxy-class"></a>Utwórz wystąpienie klasy serwera Proxy usługi

Wewnątrz swojej `Main` metody, Utwórz nowe wystąpienie klasy serwera proxy w następujący sposób:

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample2.cs)]

Ponownie użyj numeru portu rzeczywiste gdzie usługa jest uruchomiona. Podczas wdrażania usługi, zostanie użyty identyfikator URI usługi na żywo. Nie potrzebujesz zaktualizować serwer proxy.

Poniższy kod dodaje program obsługi zdarzeń, który wyświetla żądanie identyfikatorów URI w oknie konsoli. Ten krok nie jest wymagana, ale co ciekawe wyświetlić identyfikatory URI, dla każdego zapytania.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample3.cs)]

## <a name="query-the-service"></a>Zapytanie usługi

Poniższy kod umożliwia pobranie listy produktów z usługi OData.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample4.cs)]

Należy zauważyć, że nie trzeba pisać kodu do wysyłania żądań HTTP lub przeanalizować odpowiedzi. Klasy serwera proxy jest to automatycznie podczas wyliczania `Container.Products` kolekcji w **foreach** pętli.

Po uruchomieniu aplikacji, dane wyjściowe powinny wyglądać następująco:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample5.cmd)]

Aby uzyskać jednostki według Identyfikatora, należy użyć `where` klauzuli.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample6.cs)]

Dla pozostałej części tego tematu, I nie będzie zawierać całą `Main` działać tylko kod potrzebny do wywołania tej usługi.

## <a name="apply-query-options"></a>Stosowanie opcji zapytania

Usługa OData definiuje [opcje kwerendy](../supporting-odata-query-options.md) który może służyć do filtrowania, sortowania, dane strony i tak dalej. Na serwerze proxy usługi można stosować te opcje przy użyciu różnych wyrażenia LINQ.

W tej sekcji I pokazano przykładowe krótki. Aby uzyskać więcej informacji, zobacz temat [zagadnienia dotyczące LINQ (WCF Data Services)](https://msdn.microsoft.com/library/ee622463.aspx) w witrynie MSDN.

### <a name="filtering-filter"></a>Filtrowanie ($filter)

Aby filtrować, użyj `where` klauzuli. Następujące przykładowe filtry według kategorii produktów.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample7.cs)]

Ten kod odnosi się do następującego zapytania OData.

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample8.cmd)]

Należy zauważyć, że serwer proxy konwertuje `where` klauzuli do OData `$filter` wyrażenia.

### <a name="sorting-orderby"></a>Sortowanie ($orderby)

Aby posortować, należy użyć `orderby` klauzuli. Poniższy przykład sortowania ceny, od najwyższego do najniższego.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample9.cs)]

Oto odpowiednie żądania OData.

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample10.cmd)]

### <a name="client-side-paging-skip-and-top"></a>Po stronie klienta stronicowania ($skip i $top)

Dla zestawów dużych jednostek klient chcieć ograniczyć liczbę wyników. Na przykład klient może wyświetlać 10 wpisów w danym momencie. Jest to nazywane *stronicowania po stronie klienta*. (Dostępna jest również [stronicowania po stronie serwera](../supporting-odata-query-options.md#server-paging), gdy serwer ogranicza liczbę wyników.) Aby wykonać stronicowania po stronie klienta, należy użyć LINQ **Pomiń** i **zająć** metody. Poniższy przykład pomija najpierw 40 wyniki i pobiera następny 10.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample11.cs)]

Oto odpowiednie żądania OData:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample12.cmd)]

### <a name="select-select-and-expand-expand"></a>Wybierz ($select) i rozwiń ($expand)

Aby dołączyć powiązanych jednostek, należy użyć `DataServiceQuery<t>.Expand` metody. Na przykład w celu uwzględnienia `Supplier` dla każdego `Product`:

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample13.cs)]

Oto odpowiednie żądania OData:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample14.cmd)]

Aby zmienić kształt odpowiedzi, należy użyć LINQ **wybierz** klauzuli. Poniższy przykład pobiera tylko nazwę każdego produktu, bez innych właściwości.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample15.cs)]

Oto odpowiednie żądania OData:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample16.cmd)]

Klauzula select może obejmować powiązanych jednostek. W takiej sytuacji nie należy wywoływać metody **rozwiń**; w takim przypadku serwer proxy się automatycznie uwzględnia rozszerzenie. Poniższy przykład pobiera nazwę i dostawcy każdego produktu.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample17.cs)]

Oto odpowiednie żądania OData. Należy zauważyć, że zawiera on **$expand** opcji.

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample18.cmd)]

Aby uzyskać więcej informacji na temat $select i $rozwiń, zobacz [przy użyciu $select, $expand and $value w sieci Web API 2](../using-select-expand-and-value.md).

## <a name="add-a-new-entity"></a>Dodaj nową jednostkę

Aby dodać nową jednostkę do zbioru jednostek, należy wywołać `AddToEntitySet`, gdzie *EntitySet* to nazwa zestawu jednostek. Na przykład `AddToProducts` dodaje nowy `Product` do `Products` zestawu jednostek. Podczas generowania serwera proxy usług danych WCF automatycznie tworzy następujące silnie typizowane **AddTo** metody.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample19.cs)]

Aby dodać łącze między dwiema jednostkami, użyj **AddLink** i **SetLink** metody. Poniższy kod dodaje nowego dostawcy i nowego produktu, a następnie tworzy łącza między nimi.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample20.cs)]

Użyj **AddLink** po właściwości nawigacji kolekcji. W tym przykładzie dodamy produktu do `Products` kolekcji na dostawcy.

Użyj **SetLink** gdy właściwość nawigacji jest pojedynczą jednostką. W tym przykładzie firma Microsoft ustawienie `Supplier` właściwość nad produktem.

## <a name="update--patch"></a>Aktualizacji / poprawek

Aby zaktualizować jednostkę, należy wywołać **UpdateObject** metody.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample21.cs)]

Aktualizacja jest wykonywana po wywołaniu **SaveChanges**. Domyślnie program WCF wysyła żądanie HTTP scalania. **PatchOnUpdate** przekazuje WCF, zamiast wysyłać HTTP PATCH.

> [!NOTE]
> Dlaczego PATCH i MERGE? Pierwotną specyfikację protokołu HTTP 1.1 ([RCF 2616](http://tools.ietf.org/html/rfc2616)) nie definiują dowolną metodę HTTP, za pomocą semantyki "częściową aktualizację". Aby obsługiwać aktualizacje częściowe, specyfikacją OData zdefiniowane MERGE — metoda. W 2010 r. [RFC 5789](http://tools.ietf.org/html/rfc5789) zdefiniowane metody PATCH częściowej aktualizacji. Niektóre z historii, w tym przeczytasz [wpis w blogu](https://blogs.msdn.com/b/astoriateam/archive/2008/05/20/merge-vs-replace-semantics-for-update-operations.aspx) na blogu usługi danych WCF. Już dziś poprawki jest preferowana nad scalania. Kontrolera OData, utworzonego przez tworzenie szkieletu interfejsu API sieci Web obsługuje obie metody.


Jeśli chcesz zamienić całej jednostki (PUT semantyki), należy określić **ReplaceOnUpdate** opcji. Powoduje to WCF możesz wysłać żądanie HTTP PUT.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample22.cs)]

## <a name="delete-an-entity"></a>Usunięcie jednostki

Aby usunąć jednostkę, wywołaj **DeleteObject**.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample23.cs)]

## <a name="invoke-an-odata-action"></a>Wywołaj akcję OData

W protokole OData [akcje](odata-actions.md) to sposób, aby dodać zachowania po stronie serwera, które łatwo nie są zdefiniowane jako operacje CRUD na jednostkach.

Mimo że ten dokument metadanych OData w tym artykule opisano czynności, klasę proxy nie powoduje utworzenia wszelkie silnie typizowane metody dla nich. Nadal możesz wywołać akcję OData przy użyciu ogólnej **Execute** metody. Jednakże należy znać typów danych parametrów i zwracanej wartości.

Na przykład `RateProduct` akcja ma parametr o nazwie "Ocena" o typie `Int32` i zwraca `double`. Poniższy kod przedstawia sposób wywołania tej akcji.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample24.cs)]

Aby uzyskać więcej informacji, zobacz[wywoływanie operacji usługi i akcje](https://msdn.microsoft.com/library/hh230677.aspx).

Jedną z opcji jest rozszerzenie **kontenera** Aby klasa zapewniała silnie typizowane metody, która wywołuje akcję:

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample25.cs)]
