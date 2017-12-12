---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
title: "Obsługa relacjami jednostek w OData v3 z składnika Web API 2 | Dokumentacja firmy Microsoft"
author: MikeWasson
description: "Większość zestawów danych definiować relacje między obiektami: klienci mają zleceń; książki ma autorów; produkty mają dostawców. Używanie protokołu OData, klientów można przechodzić przez..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/26/2014
ms.topic: article
ms.assetid: 1e4c2eb4-b6cf-42ff-8a65-4d71ddca0394
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
msc.type: authoredcontent
ms.openlocfilehash: dec7e10e59cc2441c967afe062df227b105106a1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="supporting-entity-relations-in-odata-v3-with-web-api-2"></a>Obsługa relacjami jednostek w OData v3 z składnika Web API 2
====================
przez [Wasson Jan](https://github.com/MikeWasson)

[Pobieranie ukończone projektu](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> Większość zestawów danych definiować relacje między obiektami: klienci mają zleceń; książki ma autorów; produkty mają dostawców. Używanie protokołu OData, klienci mogą przechodzić za pośrednictwem relacjami jednostek. Biorąc pod uwagę produktu, można znaleźć dostawcy. Można również utworzyć lub Usuń relacje. Na przykład można ustawić dostawcy produktu.
> 
> W tym samouczku przedstawiono sposób obsługi tych operacji w interfejsie API sieci Web ASP.NET. Samouczek opiera się na samouczka [tworzenia punktu końcowego OData v3 z sieci Web API 2](creating-an-odata-endpoint.md).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Używane w samouczku wersje oprogramowania
> 
> 
> - Składnik Web API 2
> - OData w wersji 3
> - Entity Framework 6


## <a name="add-a-supplier-entity"></a>Dodawanie jednostki dostawcy

Najpierw należy dodać nowy typ jednostek do naszej źródła strumieniowego OData. Dodamy `Supplier` klasy.

[!code-csharp[Main](working-with-entity-relations/samples/sample1.cs)]

Ta klasa korzysta z ciągiem klucza jednostki. W praktyce, która może być mniej typowych niż przy użyciu klucza. Jednak warto zobaczenia jak OData obsługuje innych typów kluczy oprócz liczb całkowitych.

Następnie utworzymy relacji przez dodanie `Supplier` właściwości `Product` klasy:

[!code-csharp[Main](working-with-entity-relations/samples/sample2.cs)]

Dodaj nową **DbSet** do `ProductServiceContext` klasy, tak aby uwzględni Entity Framework `Supplier` tabeli w bazie danych.

[!code-csharp[Main](working-with-entity-relations/samples/sample3.cs?highlight=9)]

W WebApiConfig.cs należy dodać do jednostki "Dostawcy" do modelu EDM:

[!code-csharp[Main](working-with-entity-relations/samples/sample4.cs?highlight=4)]

## <a name="navigation-properties"></a>Właściwości nawigacji

Aby uzyskać dostawcę produktu, klient wysyła żądanie pobrania:

[!code-console[Main](working-with-entity-relations/samples/sample5.cmd)]

W tym miejscu "Dostawca" jest właściwością nawigacji na `Product` typu. W takim przypadku `Supplier` odwołuje się do pojedynczego elementu, ale Nawigacja właściwości można także wrócić kolekcji (relacji jeden do wielu lub wiele do wielu).

Aby zapewnić obsługę tego żądania, dodaj następującą metodę do `ProductsController` klasy:

[!code-csharp[Main](working-with-entity-relations/samples/sample6.cs)]

*Klucza* parametru jest klucz produktu. Metoda zwraca obiekt pokrewny &#8212;w takim przypadku `Supplier` wystąpienia. Nazwa metody i nazwa parametru są ważne. Ogólnie rzecz biorąc Jeśli właściwość nawigacji ma nazwę "X", należy dodać metodę o nazwie "GetX". Metoda przyjmuje parametr o nazwie "*klucza*" odpowiedniego dla typu danych klucza nadrzędnego.

Należy również uwzględnić **[FromOdataUri]** atrybutu w *klucza* parametru. Ten atrybut informuje interfejsu API sieci Web do używania reguły składni OData po przeanalizowaniu klucza z identyfikatora URI żądania.

## <a name="creating-and-deleting-links"></a>Tworzenie i usuwanie łącza

OData obsługuje tworzenie lub usuwanie relacji między dwiema jednostkami. W terminologii OData relacji jest "link". Każde łącze ma identyfikator URI w formularzu *jednostki*/$links /*jednostki*. Na przykład link z produktu dostawcy wygląda następująco:

[!code-console[Main](working-with-entity-relations/samples/sample7.cmd)]

Aby utworzyć nowy link, klient wysyła żądanie POST do łącza identyfikatora URI. Treść żądania jest identyfikatorem URI obiektu docelowego. Na przykład załóżmy, że istnieje dostawcy z kluczem "CTSO". Aby utworzyć łącze z "Product(1)" do "Supplier('CTSO')", klient wysyła żądanie podobne do poniższych:

[!code-console[Main](working-with-entity-relations/samples/sample8.cmd)]

Aby usunąć łącza, klient wysyła żądanie usunięcia łącza identyfikatora URI.

**Tworzenie łączy**

Aby włączyć klienta do utworzenia łącza dostawcy produktu, Dodaj następujący kod do `ProductsController` klasy:

[!code-csharp[Main](working-with-entity-relations/samples/sample9.cs)]

Ta metoda przyjmuje trzy parametry:

- *klucz*: klucz do obiektu nadrzędnego (product)
- *Element navigationProperty*: Nazwa właściwości nawigacji. W tym przykładzie tylko prawidłową właściwością nawigacji jest "Dostawca".
- *łącze*: identyfikator URI OData powiązanej jednostki. Ta wartość jest pobierana z treści żądania. Na przykład łącze identyfikatora URI może być "`http://localhost/odata/Suppliers('CTSO')`, co oznacza dostawcy o identyfikatorze ="CTSO".

Metoda używa łącze do wyszukania dostawcy. Jeśli zostanie znaleziony zgodnego dostawcę, Ustawia metodę `Product.Supplier` właściwości i zapisuje go w bazie danych.

Część najtrudniejsze jest podczas analizowania łącza identyfikatora URI. Zasadniczo należy symulować działanie wysyłania żądania GET do tego identyfikatora URI. Następująca metoda pomocnika pokazano, jak to zrobić. Metoda wywołuje proces routingu interfejsu API sieci Web i otrzymuje w odpowiedzi **element ODataPath** wystąpienie reprezentującego przeanalizowany ścieżki OData. Łącza identyfikatora URI jednego z segmentów powinna być klucza jednostki. (Jeśli nie, klient wysłał nieprawidłowy identyfikator URI.)

[!code-csharp[Main](working-with-entity-relations/samples/sample10.cs)]

**Usunięcie łącza**

Aby usunąć łącza, Dodaj następujący kod do `ProductsController` klasy:

[!code-csharp[Main](working-with-entity-relations/samples/sample11.cs)]

W tym przykładzie właściwość nawigacji jest jeden `Supplier` jednostki. Jeśli właściwość nawigacji jest kolekcją, identyfikator URI do usunięcia łącza musi zawierać klucz powiązanej jednostki. Na przykład:

[!code-console[Main](working-with-entity-relations/samples/sample12.cmd)]

To żądanie usuwa kolejności 1 klientów 1. W tym przypadku metoda DeleteLink będzie mieć następującą sygnaturą:

[!code-csharp[Main](working-with-entity-relations/samples/sample13.cs)]

*RelatedKey* parametru zapewnia klucz powiązanej jednostki. Tak w Twojej `DeleteLink` metody wyszukiwania podstawowego jednostki według *klucza* parametru znaleźć obiektu pokrewnego przez *relatedKey* parametr, a następnie Usuń skojarzenie. W zależności od modelu danych, może być konieczne wdrożenie obie wersje `DeleteLink`. Interfejs API sieci Web będzie wywoływać poprawnej wersji oparte na identyfikator URI żądania.
