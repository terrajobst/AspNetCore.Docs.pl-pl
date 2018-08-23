---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
title: Akcje i funkcje w protokole OData v4 przy użyciu wzorca ASP.NET Web API 2.2 | Dokumentacja firmy Microsoft
author: MikeWasson
description: W protokole OData akcje i funkcje są w sposób, aby dodać zachowania po stronie serwera, które łatwo nie są zdefiniowane jako operacje CRUD na jednostkach. Ten samouczek pokazuje, jak...
ms.author: riande
ms.date: 06/27/2014
ms.assetid: 0e6fb03c-b16d-4bb0-ab0b-552bd2b6ece1
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
msc.type: authoredcontent
ms.openlocfilehash: 4e61c6b4bf59792b6570e32e6d24635d4f5e5ac3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41751986"
---
<a name="actions-and-functions-in-odata-v4-using-aspnet-web-api-22"></a>Akcje i funkcje w protokole OData v4 przy użyciu wzorca ASP.NET Web API 2.2
====================
przez [Mike Wasson](https://github.com/MikeWasson)

> W protokole OData akcje i funkcje są w sposób, aby dodać zachowania po stronie serwera, które łatwo nie są zdefiniowane jako operacje CRUD na jednostkach. W tym samouczku przedstawiono sposób dodawania akcji i funkcji do punktu końcowego OData v4, przy użyciu Web API 2.2. Samouczek opiera się na samouczka [tworzenia protokołu OData v4 punktu końcowego Używanie wzorca ASP.NET Web API 2](create-an-odata-v4-endpoint.md)
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używanego w tym samouczku
> 
> 
> - Składnik Web API 2.2
> - Protokołu OData v4
> - [Visual Studio 2013 Update 2](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - .NET 4.5
> 
> 
> ## <a name="tutorial-versions"></a>Samouczek wersji
> 
> Dla protokołu OData w wersji 3, zobacz [akcje protokołu OData w programie ASP.NET Web API 2](../odata-v3/odata-actions.md).


Różnica między *akcje* i *funkcje* akcje mogą mieć skutki uboczne, a nie obsługują funkcji. Akcje i funkcje mogą zwracać dane. Do niektórych zastosowań akcje obejmują:

- Złożone transakcji.
- Manipulowanie kilka jednostek naraz.
- Zezwalanie na aktualizacje tylko dla niektórych właściwości jednostki.
- Wysyłanie danych, który nie jest jednostką.

Funkcje są przydatne do zwracania informacji, które nie odpowiadają bezpośrednio do jednostki lub kolekcji.

Akcja (lub funkcji) można wskazać pojedynczą jednostkę lub kolekcję. W terminologii OData jest *powiązania*. Mogą też istnieć &quot;niepowiązanych&quot; akcji/funkcje, które są wywoływane jako statyczne operacje w usłudze.

## <a name="example-adding-an-action"></a>Przykład: Dodawanie operacji

Czynnością jest zdefiniowanie akcję, aby ocenić produktu.

> [!NOTE]
> Ten samouczek opiera się na samouczka [tworzenia protokołu OData v4 punktu końcowego Używanie wzorca ASP.NET Web API 2](create-an-odata-v4-endpoint.md)


Najpierw dodaj `ProductRating` modelu do reprezentowania klasyfikacji.

[!code-csharp[Main](odata-actions-and-functions/samples/sample1.cs)]

Również dodać **DbSet** do `ProductsContext` klasy, tak aby EF opisano tworzenie tabeli oceny w bazie danych.

[!code-csharp[Main](odata-actions-and-functions/samples/sample2.cs)]

### <a name="add-the-action-to-the-edm"></a>Dodawanie akcji do EDM

W WebApiConfig.cs Dodaj następujący kod:

[!code-csharp[Main](odata-actions-and-functions/samples/sample3.cs)]

**EntityTypeConfiguration.Action** metoda dodaje akcję do modelu entity data model (EDM struktury). **Parametr** metody określa typizowany parametr dla akcji.

Ten kod ustawia też przestrzeni nazw dla EDM. Przestrzeń nazw ma znaczenie, ponieważ identyfikator URI akcji zawiera nazwę akcji w pełni kwalifikowana:

[!code-console[Main](odata-actions-and-functions/samples/sample4.cmd)]

> [!NOTE]
> W typowej konfiguracji usług IIS kropki (.) w tym adresie URL spowoduje, że usługi IIS, aby zwrócić błąd 404. Możesz rozwiązać ten problem, dodając następującą sekcję do pliku Web.Config:

[!code-xml[Main](odata-actions-and-functions/samples/sample5.xml)]

### <a name="add-a-controller-method-for-the-action"></a>Dodaj metodę kontrolera dla akcji

Aby włączyć &quot;współczynnik&quot; działania, dodaj następującą metodę do `ProductsController`:

[!code-csharp[Main](odata-actions-and-functions/samples/sample6.cs)]

Należy zauważyć, że nazwa metody odpowiada nazwie akcji. **[HttpPost]** atrybut określa, metoda jest metodą HTTP POST.

Aby wywołać akcję, klient wysyła żądanie HTTP POST, jak pokazano poniżej:

[!code-console[Main](odata-actions-and-functions/samples/sample7.cmd)]

&quot;Współczynnik&quot; akcji jest powiązany z wystąpień produktu, dlatego identyfikator URI akcji jest nazwa akcji w pełni kwalifikowaną, dołączany do jednostki identyfikatora URI. (Odwołać, możemy ustawić przestrzeni nazw EDM na &quot;ProductService&quot;, więc jest nazwa akcji z w pełni kwalifikowana &quot;ProductService.Rate&quot;.)

Treść żądania zawiera parametry akcji jako ładunek JSON. Interfejs API sieci Web automatycznie konwertuje ładunek JSON do **ODataActionParameters** obiektu, który jest po prostu słownika wartości parametrów. Umożliwia dostęp do parametrów w metodzie kontrolera tego słownika.

Jeśli klient wysyła parametry akcji w nieprawidłowa formatowania, wartości **ModelState.IsValid** ma wartość false. Sprawdź tę flagę w metodzie kontrolera i zwraca błędu, jeśli **IsValid** ma wartość false.

[!code-csharp[Main](odata-actions-and-functions/samples/sample8.cs)]

## <a name="example-adding-a-function"></a>Przykład: Dodawanie funkcji

Teraz możemy dodać funkcję OData, która zwraca iloczyn najbardziej kosztowne. Jak wcześniej, pierwszym krokiem jest dodanie funkcji do EDM. W WebApiConfig.cs Dodaj następujący kod.

[!code-csharp[Main](odata-actions-and-functions/samples/sample9.cs)]

W takim przypadku funkcja jest powiązana z kolekcji produktów, a nie poszczególnych wystąpień produktu. Klienci wywołać funkcję, wysyłając żądanie GET:

[!code-console[Main](odata-actions-and-functions/samples/sample10.cmd)]

Poniżej przedstawiono metody kontrolera dla tej funkcji:

[!code-csharp[Main](odata-actions-and-functions/samples/sample11.cs)]

Należy zauważyć, że nazwa metody odpowiada nazwie funkcji. **[HttpGet]** atrybut określa, metoda jest metodą HTTP GET.

Poniżej przedstawiono odpowiedzi HTTP:

[!code-console[Main](odata-actions-and-functions/samples/sample12.cmd)]

## <a name="example-adding-an-unbound-function"></a>Przykład: Dodawanie niepowiązanych — funkcja

Poprzedni przykład jest funkcja powiązana z kolekcją. W następnym przykładzie utworzymy *niepowiązanych* funkcji. Niezwiązane funkcje są wywoływane jako statyczne operacje w usłudze. Funkcja w tym przykładzie zwraca podatku od sprzedaży dla podanego kodu pocztowego.

W pliku WebApiConfig Dodaj funkcję do EDM:

[!code-csharp[Main](odata-actions-and-functions/samples/sample13.cs)]

Należy zauważyć, że wywołania **funkcja** bezpośrednio na **element ODataModelBuilder**, a nie typu jednostki lub kolekcji. Informuje konstruktora modeli, że funkcja ta jest niepowiązanych.

Poniżej przedstawiono metody kontrolera, który implementuje funkcję:

[!code-csharp[Main](odata-actions-and-functions/samples/sample14.cs)]

Nie ma znaczenia, który kontroler interfejsu API sieci Web umieścić tę metodę w. Możesz umieścić go w `ProductsController`, lub zdefiniować osobnego kontrolera. **[ODataRoute]** atrybut definiuje szablon identyfikatora URI dla tej funkcji.

Oto przykładowe żądanie klienta:

[!code-console[Main](odata-actions-and-functions/samples/sample15.cmd)]

Odpowiedź HTTP:

[!code-console[Main](odata-actions-and-functions/samples/sample16.cmd)]
