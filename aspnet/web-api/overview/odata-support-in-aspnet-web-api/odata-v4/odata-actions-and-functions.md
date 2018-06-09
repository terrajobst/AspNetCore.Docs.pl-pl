---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
title: Akcje i funkcje protokołu OData v4 używanie składnika ASP.NET Web API 2.2 | Dokumentacja firmy Microsoft
author: MikeWasson
description: W protokole OData akcje i funkcje są sposobem dodania zachowania po stronie serwera, które łatwo nie są zdefiniowane jako operacje CRUD na jednostkach. Ten samouczek pokazuje, jak...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/27/2014
ms.topic: article
ms.assetid: 0e6fb03c-b16d-4bb0-ab0b-552bd2b6ece1
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
msc.type: authoredcontent
ms.openlocfilehash: 532362f0c0faaaf0cb0c04726856f0497e5261b5
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "26566774"
---
<a name="actions-and-functions-in-odata-v4-using-aspnet-web-api-22"></a>Akcje i funkcje w wersji 4 OData przy użyciu interfejsu API 2.2 sieci Web ASP.NET
====================
przez [Wasson Jan](https://github.com/MikeWasson)

> W protokole OData akcje i funkcje są sposobem dodania zachowania po stronie serwera, które łatwo nie są zdefiniowane jako operacje CRUD na jednostkach. W tym samouczku przedstawiono sposób dodawania akcje i funkcje na OData v4 punkt końcowy, za pomocą 2.2 interfejsu API sieci Web. Samouczek opiera się na samouczka [tworzenia dodatku Using ASP.NET Web API 2 OData v4 punktu końcowego](create-an-odata-v4-endpoint.md)
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Używane w samouczku wersje oprogramowania
> 
> 
> - 2.2 interfejsu API sieci Web
> - Protokołu OData v4
> - [Visual Studio 2013 Update 2](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - .NET 4.5
> 
> 
> ## <a name="tutorial-versions"></a>Samouczek wersji
> 
> Dla OData w wersji 3, zobacz [akcji OData w ASP.NET Web API 2](../odata-v3/odata-actions.md).


Różnica między *akcje* i *funkcje* akcje może mieć efekty uboczne, a nie w funkcji. Zarówno akcje i funkcje mogą zwracać dane. Niektóre zastosowania dla działania obejmują:

- Złożone transakcje.
- Manipulowanie jednocześnie kilka jednostek.
- Stosowanie aktualizacji tylko do niektórych właściwości jednostki.
- Wysyłanie danych, która nie jest jednostką.

Funkcje są przydatne do zwracania informacji, który nie odpowiada bezpośrednio do jednostki lub kolekcji.

Akcja (lub funkcji) celem może być pojedynczą jednostką lub kolekcji. W terminologii OData jest *powiązania*. Może także zawierać &quot;niezwiązanego&quot; akcje/funkcje, które są nazywane jako statyczne operacji dla usługi.

## <a name="example-adding-an-action"></a>Przykład: Dodawanie operacji

Umożliwia zdefiniowanie akcji, aby ocenić produktu.

> [!NOTE]
> W tym samouczku opiera się na samouczka [tworzenia dodatku Using ASP.NET Web API 2 OData v4 punktu końcowego](create-an-odata-v4-endpoint.md)


Najpierw dodaj `ProductRating` modelu do reprezentowania klasyfikacji.

[!code-csharp[Main](odata-actions-and-functions/samples/sample1.cs)]

Dodaj również **DbSet** do `ProductsContext` klasy, tak aby EF spowoduje utworzenie tabeli klasyfikacje w bazie danych.

[!code-csharp[Main](odata-actions-and-functions/samples/sample2.cs)]

### <a name="add-the-action-to-the-edm"></a>Dodaj akcję do EDM

W WebApiConfig.cs Dodaj następujący kod:

[!code-csharp[Main](odata-actions-and-functions/samples/sample3.cs)]

**EntityTypeConfiguration.Action** metody Dodawanie akcji do modelu entity data model (EDM). **Parametr** metody określa parametr określonego dla akcji.

Ten kod także ustawia obszar nazw dla EDM. Przestrzeń nazw jest ważna, ponieważ akcji w pełni kwalifikowana nazwa zawiera identyfikator URI dla akcji:

[!code-console[Main](odata-actions-and-functions/samples/sample4.cmd)]

> [!NOTE]
> W typowej konfiguracji usług IIS kropki (.) w tym adresem URL spowoduje, że usługi IIS zwracają błąd 404. Można go rozwiązać, dodając następującą sekcję do pliku Web.Config:

[!code-xml[Main](odata-actions-and-functions/samples/sample5.xml)]

### <a name="add-a-controller-method-for-the-action"></a>Dodaj metodę kontrolera dla akcji

Aby włączyć &quot;szybkość&quot; akcji, dodaj następującą metodę do `ProductsController`:

[!code-csharp[Main](odata-actions-and-functions/samples/sample6.cs)]

Zwróć uwagę, czy nazwa metody jest zgodna z nazwą akcji. **[HttpPost]** atrybut określa metoda jest metodą HTTP POST.

Aby wywołać akcję, klient wysyła żądanie HTTP POST, takie jak następujące:

[!code-console[Main](odata-actions-and-functions/samples/sample7.cmd)]

&quot;Szybkość&quot; akcji jest powiązana z wystąpieniami produktu, więc identyfikator URI dla akcji jest nazwa akcji pełną dołączany do jednostki identyfikatora URI. (Odwołać możemy ustawić przestrzeni nazw EDM &quot;ProductService&quot;, więc akcji w pełni kwalifikowana nazwa jest &quot;ProductService.Rate&quot;.)

Treść żądania zawiera parametry akcji jako ładunek JSON. Interfejs API sieci Web automatycznie konwertuje ładunek JSON do **ODataActionParameters** obiektu, który jest ze słownika wartości parametrów. Umożliwia dostęp do parametrów w metodę kontrolera tego słownika.

Jeśli klient wysyła parametry akcji w niewłaściwy format, wartość **ModelState.IsValid** ma wartość false. Sprawdź tę flagę w metodę kontrolera i zwraca błąd, jeśli **IsValid** ma wartość false.

[!code-csharp[Main](odata-actions-and-functions/samples/sample8.cs)]

## <a name="example-adding-a-function"></a>Przykład: Dodawanie funkcji

Teraz możemy dodać funkcję OData, która zwraca najbardziej kosztownych produktu. Ponieważ przed, pierwszym krokiem jest dodanie funkcji do EDM. W WebApiConfig.cs Dodaj następujący kod.

[!code-csharp[Main](odata-actions-and-functions/samples/sample9.cs)]

W takim przypadku funkcja jest powiązana z kolekcji produktów, a nie poszczególnych wystąpień produktu. Klienci wywołanie funkcji, wysyłając żądanie GET:

[!code-console[Main](odata-actions-and-functions/samples/sample10.cmd)]

Oto metoda kontrolera dla tej funkcji:

[!code-csharp[Main](odata-actions-and-functions/samples/sample11.cs)]

Zwróć uwagę, czy nazwa metody jest zgodna z nazwą funkcji. **[HttpGet]** atrybut określa metoda jest metodą HTTP GET.

Poniżej przedstawiono odpowiedzi HTTP:

[!code-console[Main](odata-actions-and-functions/samples/sample12.cmd)]

## <a name="example-adding-an-unbound-function"></a>Przykład: Dodawanie niepowiązanych — funkcja

Poprzedni przykład była funkcja powiązane z kolekcją. W tym przykładzie dalej utworzymy *niezwiązanego* funkcji. Niezwiązane funkcje są nazywane jako statyczne operacji dla usługi. Funkcja w tym przykładzie zwraca podatek dla danego kod pocztowy.

W pliku WebApiConfig należy dodać funkcję do EDM:

[!code-csharp[Main](odata-actions-and-functions/samples/sample13.cs)]

Należy zauważyć, że wywołania **funkcja** bezpośrednio na **element ODataModelBuilder**, zamiast typu jednostki lub kolekcji. Informuje konstruktora modeli, czy funkcja jest niepowiązanych.

Oto metoda kontrolera, który implementuje funkcję:

[!code-csharp[Main](odata-actions-and-functions/samples/sample14.cs)]

Nie ma znaczenia, który można umieścić tę metodę w kontroler interfejsu API sieci Web. Można go umieścić `ProductsController`, lub zdefiniuj osobnego kontrolera. **[ODataRoute]** atrybut określa szablon identyfikatora URI dla funkcji.

Poniżej przedstawiono przykładowe żądanie klienta:

[!code-console[Main](odata-actions-and-functions/samples/sample15.cmd)]

Odpowiedź HTTP:

[!code-console[Main](odata-actions-and-functions/samples/sample16.cmd)]
