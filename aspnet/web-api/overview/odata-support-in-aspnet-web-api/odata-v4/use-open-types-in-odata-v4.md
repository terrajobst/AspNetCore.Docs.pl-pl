---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
title: "Otwórz typy w protokołu OData v4 z interfejsu API sieci Web programu ASP.NET | Dokumentacja firmy Microsoft"
author: microsoft
description: "W protokołu OData v4 otwartym typem jest typ stuctured, który zawiera właściwości dynamicznych, oprócz żadnych właściwości, które są zadeklarowane w definicji typu. Otwórz..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/15/2014
ms.topic: article
ms.assetid: f25f5ac5-4800-4950-abe5-c97750a27fc6
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: fe67b9a11a82b55d5f3e0e5f1b0cee10a58833d2
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/12/2018
---
<a name="open-types-in-odata-v4-with-aspnet-web-api"></a>Otwórz typy w protokołu OData v4 z interfejsu API sieci Web ASP.NET
====================
przez [firmy Microsoft](https://github.com/microsoft)

> W przypadku protokołu OData v4 *otwartym typem* jest typu stuctured, który zawiera właściwości dynamicznych, oprócz żadnych właściwości, które są zadeklarowane w definicji typu. Otwórz typy umożliwiają bardziej elastyczne modeli danych. Ten samouczek przedstawia sposób użycia typy otwarte w programie ASP.NET Web API OData.
> 
> Ten samouczek zakłada, że już wiesz, jak można utworzyć punktu końcowego OData w interfejsie API sieci Web ASP.NET. Jeśli nie, Rozpocznij od przeczytania [utworzyć punktu końcowego OData v4](create-an-odata-v4-endpoint.md) pierwszy.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Używane w samouczku wersje oprogramowania
> 
> 
> - Web API OData 5.3
> - Protokołu OData v4


Pierwszy, niektóre terminologii OData:

- Typ jednostki: typem strukturalnym przy użyciu klucza.
- Typ złożony: typem strukturalnym bez klucza.
- Otwórz typu: typ z właściwości dynamicznych. Zarówno typów jednostek i typów złożonych może być otwarty.

Wartość właściwości dynamicznych może być typu pierwotnego, typu złożonego lub typem wyliczenia; lub kolekcji żadnego z tych typów. Aby uzyskać więcej informacji na temat typów open zobacz [specyfikację OData v4](http://www.odata.org/documentation/odata-version-4-0/).

## <a name="install-the-web-odata-libraries"></a>Zainstaluj biblioteki OData sieci Web

Użyj Menedżera pakietów NuGet do zainstalowania najnowszej bibliotek Web API OData. W oknie Konsola Menedżera pakietów:

[!code-console[Main](use-open-types-in-odata-v4/samples/sample1.cmd)]

## <a name="define-the-clr-types"></a>Definiowanie typów CLR

Rozpocznij od zdefiniowania modelach EDM jako typów CLR.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample2.cs)]

Podczas tworzenia modelu danych jednostki (EDM)

- `Category`jest typem wyliczenia.
- `Address`jest to typ złożony. (Nie ma klucza, więc nie jest typem jednostki.)
- `Customer`nie jest typem jednostki. (Ma klucz).
- `Press`jest otwarty typ złożony.
- `Book`nie jest typem jednostki otwarte.

Aby utworzyć typu otwartego, typ CLR musi mieć właściwość typu `IDictionary<string, object>`, które zawiera właściwości dynamicznych.

## <a name="build-the-edm-model"></a>Tworzenie modelu EDM

Jeśli używasz **ODataConventionModelBuilder** utworzyć EDM, `Press` i `Book` są automatycznie dodawane jako typów open, na podstawie obecności z `IDictionary<string, object>` właściwości.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample3.cs)]

Można również tworzyć EDM jawnie, przy użyciu **element ODataModelBuilder**.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample4.cs)]

## <a name="add-an-odata-controller"></a>Dodawanie kontrolera OData

Następnie dodaj kontrolerze OData. W tym samouczku użyjemy uproszczony kontroler obsługuje tylko GET i POST żądań czy używa listy w pamięci do przechowywania obiektów.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample5.cs)]

Należy zauważyć, że pierwszy `Book` wystąpienie nie ma dynamicznych właściwości. Drugi `Book` wystąpienie ma następujące właściwości dynamicznych:

- "Opublikowane": typ pierwotny
- "Autorzy": zbiór typy pierwotne
- "OtherCategories": kolekcję typów wyliczenia.

Ponadto `Press` właściwości tego `Book` wystąpienie ma następujące właściwości dynamicznych:

- "Blog": typ pierwotny
- "Address": typ złożony

## <a name="query-the-metadata"></a>Zapytanie metadanych

Aby uzyskać ten dokument metadanych OData, Wyślij żądanie GET `~/$metadata`. Treść odpowiedzi powinny wyglądać podobnie do poniższego:

[!code-xml[Main](use-open-types-in-odata-v4/samples/sample6.xml?highlight=5,21)]

Z dokumentu metadanych można stwierdzić, że:

- Aby uzyskać `Book` i `Press` typów wartości `OpenType` atrybut ma wartość true. `Customer` i `Address` typów nie ma tego atrybutu.
- `Book` Typ jednostki ma trzy zadeklarowane właściwości: ISBN, tytuł i naciśnij klawisz. Nie ma metadanych OData `Book.Properties` właściwość z klasy CLR.
- Podobnie `Press` typu złożonego ma tylko dwie właściwości zadeklarowane: nazwą i kategorią. Nie ma metadanych `Press.DynamicProperties` właściwość z klasy CLR.

## <a name="query-an-entity"></a>Zapytanie jednostki

Aby uzyskać książkę o ISBN równa "978-0-7356-7942-9", Wyślij żądanie GET `~/Books('978-0-7356-7942-9')`. Treść odpowiedzi powinien wyglądać podobnie do następującego. (Wcięty, aby był bardziej czytelny.)

[!code-console[Main](use-open-types-in-odata-v4/samples/sample7.cmd?highlight=8-13,15-23)]

Zwróć uwagę, że właściwości dynamicznych są uwzględniane wbudowany z zadeklarowane właściwości.

## <a name="post-an-entity"></a>POST jednostki

Aby dodać jednostkę książki, Wyślij żądanie POST `~/Books`. Klienta można ustawić właściwości dynamicznych w ładunku żądania.

Oto przykładowe żądanie. Zanotuj właściwości "Price" i "Opublikowaną".

[!code-console[Main](use-open-types-in-odata-v4/samples/sample8.cmd?highlight=10)]

Jeśli ustawisz punkt przerwania w metodzie kontrolera widać dodania tych właściwości do interfejsu API sieci Web `Properties` słownika.

![](use-open-types-in-odata-v4/_static/image1.png)

## <a name="additional-resources"></a>Dodatkowe zasoby

[Przykładowe typu OData Open](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataOpenTypeSample/ReadMe.txt)
