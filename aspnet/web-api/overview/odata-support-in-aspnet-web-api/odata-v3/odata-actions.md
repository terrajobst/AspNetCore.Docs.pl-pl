---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
title: Obsługa akcji OData w składniku ASP.NET Web API 2 | Dokumentacja firmy Microsoft
author: MikeWasson
description: 'W protokole OData akcje służą do dodawania zachowań po stronie serwera, które łatwo nie są zdefiniowane jako operacje CRUD na jednostkach. Niektóre zastosowania dla działania obejmują: Implementowanie...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/25/2014
ms.topic: article
ms.assetid: 2d7b3aa2-aa47-4e6e-b0ce-3d65a1c6fe02
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
msc.type: authoredcontent
ms.openlocfilehash: acb369ca8f1bab8d7cad14c15f46cfd44beb9fdd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="supporting-odata-actions-in-aspnet-web-api-2"></a>Obsługa akcji OData w składniku ASP.NET Web API 2
====================
przez [Wasson Jan](https://github.com/MikeWasson)

[Pobieranie ukończone projektu](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> W protokole OData *akcje* służą do dodawania zachowań po stronie serwera, które łatwo nie są zdefiniowane jako operacje CRUD na jednostkach. Niektóre zastosowania dla działania obejmują:
> 
> - Wdrażanie złożonych transakcji.
> - Manipulowanie jednocześnie kilka jednostek.
> - Stosowanie aktualizacji tylko do niektórych właściwości jednostki.
> - Wysyłanie informacji do serwera, który nie jest zdefiniowany w jednostce.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Używane w samouczku wersje oprogramowania
> 
> 
> - Składnik Web API 2
> - OData w wersji 3
> - Entity Framework 6


## <a name="example-rating-a-product"></a>Przykład: Ocen produktu

W tym przykładzie chcemy umożliwić użytkownikom sklasyfikować produktów, a następnie uwidaczniaj średnią klasyfikację dla każdego produktu. W bazie danych przechowujemy listę oceny i kluczami z produktami.

Oto modelu, który można ich używać do reprezentowania klasyfikacje Entity Framework:

[!code-csharp[Main](odata-actions/samples/sample1.cs)]

Ale nie chcemy, aby klienci POST `ProductRating` obiektu do kolekcji "Klasyfikacje". Intuicyjnie Klasyfikacja jest skojarzony z tą kolekcją produktów, a klient wystarcza tylko po wartości klasyfikacji.

Dlatego zamiast normalnych operacji CRUD definiujemy akcji, które klient może wywołać na produkt. W terminologii OData, akcja jest *powiązany* ENTITIES produktu.

>Akcji ma efekty uboczne na serwerze. Z tego powodu ich wywołania za pomocą żądania HTTP POST. Akcje może mieć parametry i zwracane typy, które zostały opisane w metadanych usługi. Klient wysyła parametrów w treści żądania, a serwer wysyła wartości zwracanej w treści odpowiedzi. Wywołanie akcji "Szybkość produktu", klient wysyła POST do identyfikatora URI, takich jak następujące:

[!code-console[Main](odata-actions/samples/sample2.cmd)]

Dane w żądaniu POST jest po prostu ocenę produktu:

[!code-console[Main](odata-actions/samples/sample3.cmd)]

## <a name="declare-the-action-in-the-entity-data-model"></a>Deklarowanie akcji w modelu danych jednostki

W konfiguracji interfejsu API sieci Web należy dodać akcję do modelu entity data model (EDM):

[!code-csharp[Main](odata-actions/samples/sample4.cs)]

Ten kod definiuje "RateProduct" jako akcji, które mogą być wykonywane na jednostkach produktu. Również deklaruje, że przez akcję **int** parametr o nazwie "Klasyfikacji" i zwraca **int** wartość.

## <a name="add-the-action-to-the-controller"></a>Dodaj akcję do kontrolera

Akcja "RateProduct" jest powiązane z jednostkami produktu. Aby zastosować akcję, Dodaj metodę o nazwie `RateProduct` do kontrolera produktów:

[!code-csharp[Main](odata-actions/samples/sample5.cs)]

Zwróć uwagę, że w nazwie metody odpowiada nazwie w akcji EDM. Metoda ma dwa parametry:

- *klucz*: klucz produktu do szybkości.
- *Parametry*: słownik wartości parametru akcji.

Jeśli korzystasz z domyślnych Konwencji tras, parametr klucza muszą nosić nazwy "klucza". Należy również uwzględnić **[FromOdataUri]** atrybutu, jak pokazano. Ten atrybut informuje interfejsu API sieci Web do używania reguły składni OData po przeanalizowaniu klucza z identyfikatora URI żądania.

Użyj *parametry* słownika można pobrać parametrów akcji:

[!code-csharp[Main](odata-actions/samples/sample6.cs)]

Jeśli klient wysyła parametry akcji w poprawny format, wartość **ModelState.IsValid** ma wartość true. W takim przypadku można użyć **ODataActionParameters** słownika do uzyskania wartości parametrów. W tym przykładzie `RateProduct` akcji przyjmuje jeden parametr o nazwie "Klasyfikacji".

## <a name="action-metadata"></a>Akcja metadanych

Aby wyświetlić metadane usługi, należy wysłać żądanie GET /odata/$ metadanych. Poniżej przedstawiono część metadanych, który deklaruje `RateProduct` akcji:

[!code-xml[Main](odata-actions/samples/sample7.xml)]

**FunctionImport** element deklaruje akcji. Większość pól nie wymaga wyjaśnień, ale są dwa warto zauważyć:

- **Powiązana** oznacza, że akcja może być wywoływana na obiekcie docelowym co najmniej pewien czas działanie.
- **Funkcja IsAlwaysBindable** oznacza, że akcja zawsze może być wywoływana na obiekcie docelowym.

Różnica polega na tym że niektóre akcje są zawsze dostępne dla klientów, ale inne działania może być zależna od stanu jednostki. Na przykład załóżmy, że definiowania akcji "Zakupu". Można kupić tylko elementu, który znajduje się w magazynie. Jeśli element znajduje się na stanie, klient nie można wywołać tego działania.

Podczas definiowania EDM, **akcji** metoda tworzy akcję zawsze powiązania:

[!code-csharp[Main](odata-actions/samples/sample8.cs?highlight=1)]

Będzie omówienia nie — wiązanie jest zawsze — możliwe akcje (nazywane również *przejściowa* akcje) dalszej części tego tematu.

## <a name="invoking-the-action"></a>Wywoływania akcji

Teraz zobaczmy, jak klient powodowałoby wywołanie tej akcji. Załóżmy, że klient chce zapewnić co najmniej 2 do produktu o identyfikatorze = 4. Oto przykładowy komunikat żądania, przy użyciu formatu JSON w treści żądania:

[!code-console[Main](odata-actions/samples/sample9.cmd)]

Oto komunikat odpowiedzi:

[!code-console[Main](odata-actions/samples/sample10.cmd)]

## <a name="binding-an-action-to-an-entity-set"></a>Powiązania działań do zbioru jednostek

W poprzednim przykładzie, akcja jest powiązany z pojedynczej jednostki: klient ocenia jeden produkt. Może także powiązać akcji do kolekcji jednostek. Wystarczy wprowadzić następujące zmiany:

W modelu EDM, Dodaj akcję z jednostką **kolekcji** właściwości.

[!code-csharp[Main](odata-actions/samples/sample11.cs?highlight=1)]

W metodzie kontrolera, Pomiń *klucza* parametru.

[!code-csharp[Main](odata-actions/samples/sample12.cs)]

Teraz klient wywołuje akcję na zestaw jednostek produktów:

[!code-console[Main](odata-actions/samples/sample13.cmd)]

## <a name="actions-with-collection-parameters"></a>Akcje z kolekcji parametrów

Akcje może mieć parametrów, które przyjmują kolekcję wartości. W modelu EDM, użyj **CollectionParameter&lt;T&gt;**  Aby zadeklarować parametr.

[!code-csharp[Main](odata-actions/samples/sample14.cs)]

Deklaruje to parametr o nazwie "Klasyfikacje", która przyjmuje kolekcję **int** wartości. W metodzie kontrolera nadal pobrana wartość parametru **ODataActionParameters** obiektu, ale teraz wartość jest **ICollection&lt;int&gt;**  wartość:

[!code-csharp[Main](odata-actions/samples/sample15.cs)]

## <a name="transient-actions"></a>Akcje przejściowej

W tym przykładzie "RateProduct" użytkownicy zawsze można klasyfikować produktu, tak akcja jest zawsze dostępna. Jednak niektóre akcje są zależne od stanu jednostki. Na przykład w usługę dzierżawa wideo, Akcja "CheckOut" nie jest zawsze dostępna. (Jest ono zależne czy kopię wideo jest dostępny.) Działania tego typu jest nazywana *przejściowa* akcji.

W metadanych usługi ma akcji przejściowej **IsAlwaysBindable** równa false. Faktycznie wartość domyślną, który jest więc metadanych będzie wyglądać następująco:

[!code-xml[Main](odata-actions/samples/sample16.xml)]

W tym dlaczego jest to istotne: Jeśli akcja jest przejściowe, serwer musi sprawdzić klienta, jeśli akcja jest dostępna. Jest to możliwe dzięki tym łącze do działania w jednostce. Oto przykład dla jednostki film:

[!code-console[Main](odata-actions/samples/sample17.cmd)]

Właściwość "#CheckOut" zawiera łącze do Akcja CheckOut. Jeśli akcja nie jest dostępna, serwer pomija łącza.

Aby zadeklarować przejściowej akcji w EDM, należy wywołać **TransientAction** metody:

[!code-csharp[Main](odata-actions/samples/sample18.cs)]

Ponadto należy podać funkcja, która zwraca łącze akcji dla danej jednostki. Ustawiony przez wywołanie tej funkcji **HasActionLink**. Funkcja można zapisać jako wyrażenia lambda:

[!code-csharp[Main](odata-actions/samples/sample19.cs)]

Jeśli akcja jest dostępna, wyrażenia lambda zwraca łącze do akcji. Serializator OData dotyczy również ten link jej serializuje jednostki. Jeśli akcja nie jest dostępna, funkcja zwraca `null`.

## <a name="additional-resources"></a>Dodatkowe zasoby

[Przykładowe akcji OData](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v3/ODataActionsSample/)
