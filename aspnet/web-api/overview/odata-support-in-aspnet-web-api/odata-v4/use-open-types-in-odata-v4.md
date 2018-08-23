---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
title: Otwórz typy w protokole OData v4 Web API platformy ASP.NET | Dokumentacja firmy Microsoft
author: microsoft
description: W protokole OData v4 otwarty typ jest typem stuctured, który zawiera właściwości dynamicznych, oprócz żadnych właściwości, które są zadeklarowane w definicji typu. Otwórz...
ms.author: riande
ms.date: 09/15/2014
ms.assetid: f25f5ac5-4800-4950-abe5-c97750a27fc6
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: 77771d85532b8b622c2ad4ca219a38990e474c9c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756902"
---
<a name="open-types-in-odata-v4-with-aspnet-web-api"></a>Otwórz typy w protokole OData v4 Web API platformy ASP.NET
====================
przez [firmy Microsoft](https://github.com/microsoft)

> W protokole OData v4 *Otwórz typ* jest typem stuctured, który zawiera właściwości dynamicznych, oprócz żadnych właściwości, które są zadeklarowane w definicji typu. Typy otwarte umożliwiają bardziej elastyczne modeli danych. W tym samouczku pokazano, jak używać typy otwarte w programie ASP.NET Web API OData.
> 
> Ten samouczek zakłada się, czy znasz już sposób tworzenia punktu końcowego OData w interfejsie API sieci Web platformy ASP.NET. Jeśli nie, należy zacząć od przeczytania [utworzenie punktu końcowego OData v4](create-an-odata-v4-endpoint.md) pierwszy.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używanego w tym samouczku
> 
> 
> - Web API OData 5.3
> - Protokołu OData v4


Pierwszy, niektóre terminologii OData:

- Typ jednostki: typem strukturalnym przy użyciu klucza.
- Typ złożony: typem strukturalnym bez klucza.
- Otwórz typ: typ z właściwości dynamicznych. Typy jednostek i typów złożonych może być otwarty.

Wartość właściwości dynamicznych może być typ pierwotny, typu złożonego lub typ wyliczenia; lub kolekcja każdego z tych typów. Aby uzyskać więcej informacji na temat otwartych typów, zobacz [specyfikacji v4 OData](http://www.odata.org/documentation/odata-version-4-0/).

## <a name="install-the-web-odata-libraries"></a>Instalowanie bibliotek OData w sieci Web

Użyj Menedżera pakietów NuGet do zainstalowania najnowszej biblioteki Web API OData. W oknie Konsola Menedżera pakietów:

[!code-console[Main](use-open-types-in-odata-v4/samples/sample1.cmd)]

## <a name="define-the-clr-types"></a>Definiowanie typów CLR

Rozpocznij, definiując modele EDM jako typy CLR.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample2.cs)]

Po utworzeniu Entity Data Model (EDM)

- `Category` jest typem wyliczenia.
- `Address` jest to typ złożony. (Go nie ma klucza, więc nie jest typem jednostki.)
- `Customer` nie jest typem jednostki. (Ma klucz).
- `Press` to otwarty typ złożony.
- `Book` nie jest typem jednostki open.

Aby utworzyć typu otwartego, typ CLR musi mieć właściwość typu `IDictionary<string, object>`, który zawiera właściwości dynamicznych.

## <a name="build-the-edm-model"></a>Budowanie modelu EDM

Jeśli używasz **ODataConventionModelBuilder** utworzyć EDM, `Press` i `Book` są automatycznie dodawane jako typy otwarte na podstawie obecności `IDictionary<string, object>` właściwości.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample3.cs)]

Możesz również tworzyć EDM jawnie, przy użyciu **element ODataModelBuilder**.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample4.cs)]

## <a name="add-an-odata-controller"></a>Dodawanie kontrolera OData

Następnie dodaj kontrolerze OData. W tym samouczku użyjemy uproszczony kontroler, że obsługuje tylko pobieranie POST żądań i używa listy w pamięci na przechowywanie podmioty.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample5.cs)]

Należy zauważyć, że pierwszy `Book` wystąpienia nie ma żadnych właściwości dynamicznych. Drugi `Book` wystąpienia ma następujące właściwości dynamicznych:

- "Opublikowany": typ pierwotny
- "Autorzy": kolekcja typów podstawowych
- "OtherCategories": kolekcja typów wyliczeń.

Ponadto `Press` właściwość, która `Book` wystąpienia ma następujące właściwości dynamicznych:

- "Blog": typ pierwotny
- "Address": typ złożony

## <a name="query-the-metadata"></a>Zapytanie metadanych

Do pobierania dokumentu metadanych OData, Wyślij żądanie Pobierz do `~/$metadata`. Treść odpowiedzi powinna wyglądać mniej więcej tak:

[!code-xml[Main](use-open-types-in-odata-v4/samples/sample6.xml?highlight=5,21)]

Z dokumentu metadanych możesz zobaczyć, który:

- Aby uzyskać `Book` i `Press` typów wartości `OpenType` atrybut ma wartość true. `Customer` i `Address` typów nie ma tego atrybutu.
- `Book` Typ jednostki ma trzy właściwości zadeklarowanych: ISBN, tytuł, a następnie naciśnij klawisz. Nie ma metadanych OData `Book.Properties` właściwości z klasy CLR.
- Podobnie `Press` typ złożony ma tylko dwie właściwości zadeklarowanych: nazwą i kategorią. Metadane nie zawierają `Press.DynamicProperties` właściwości z klasy CLR.

## <a name="query-an-entity"></a>Zapytanie jednostki

Aby uzyskać książki z ISBN jest równa "978-0-7356-7942-9", Wyślij żądanie Pobierz do `~/Books('978-0-7356-7942-9')`. Treść odpowiedzi powinny wyglądać podobnie do poniższego. (Wcięty, aby był bardziej czytelny.)

[!code-console[Main](use-open-types-in-odata-v4/samples/sample7.cmd?highlight=8-13,15-23)]

Zauważ, że właściwości dynamicznych są dołączone jako śródwierszowego zadeklarowane właściwości.

## <a name="post-an-entity"></a>WPIS w jednostce

Aby dodać jednostkę książki, Wyślij żądanie POST `~/Books`. Klienta można ustawić właściwości dynamicznych w ładunku żądania.

Oto przykładowe żądanie. Należy pamiętać, właściwości "Price" i "Opublikowano".

[!code-console[Main](use-open-types-in-odata-v4/samples/sample8.cmd?highlight=10)]

Jeśli ustawisz punkt przerwania w metodzie kontrolera widać, że internetowego interfejsu API dodane tych właściwości, aby `Properties` słownika.

![](use-open-types-in-odata-v4/_static/image1.png)

## <a name="additional-resources"></a>Dodatkowe zasoby

[Przykładowe typu OData Open](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataOpenTypeSample/ReadMe.txt)
