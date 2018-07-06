---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
title: Obsługa akcji OData we wzorcu ASP.NET Web API 2 | Dokumentacja firmy Microsoft
author: MikeWasson
description: 'W protokole OData akcje to sposób, aby dodać zachowania po stronie serwera, które łatwo nie są zdefiniowane jako operacje CRUD na jednostkach. Do niektórych zastosowań akcje obejmują: Implementowanie...'
ms.author: aspnetcontent
ms.date: 02/25/2014
ms.assetid: 2d7b3aa2-aa47-4e6e-b0ce-3d65a1c6fe02
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
msc.type: authoredcontent
ms.openlocfilehash: e02ab21b864e328fe6892a00e5d5aca3f88eb9a2
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37837775"
---
<a name="supporting-odata-actions-in-aspnet-web-api-2"></a>Obsługa akcji OData we wzorcu ASP.NET Web API 2
====================
przez [Mike Wasson](https://github.com/MikeWasson)

[Pobieranie ukończone projektu](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> W protokole OData *akcje* to sposób, aby dodać zachowania po stronie serwera, które łatwo nie są zdefiniowane jako operacje CRUD na jednostkach. Do niektórych zastosowań akcje obejmują:
> 
> - Implementowanie złożonych transakcji.
> - Manipulowanie kilka jednostek naraz.
> - Zezwalanie na aktualizacje tylko dla niektórych właściwości jednostki.
> - Wysyłanie informacji do serwera, który nie jest zdefiniowany w jednostce.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używanego w tym samouczku
> 
> 
> - Składnik Web API 2
> - OData w wersji 3
> - Entity Framework 6


## <a name="example-rating-a-product"></a>Przykład: Ocena produktu

W tym przykładzie chcemy umożliwić użytkownikom oceniać produkty, a następnie udostępnić średnią ocen dla każdego produktu. W bazie danych będą przechowywane listy klasyfikacji, opartych na kluczach produktów.

Poniżej przedstawiono model, może być używana do reprezentowania klasyfikacje platformy Entity Framework:

[!code-csharp[Main](odata-actions/samples/sample1.cs)]

Ale nie chcemy, aby klienci POST `ProductRating` obiektu do kolekcji "Klasyfikacje". Intuicyjne ocena jest skojarzona z tą kolekcją produktów, a klient powinien wystarczy wpis wartość oceny.

W związku z tym zamiast korzystać z normalnych operacji CRUD, definiujemy akcję, które klient może wywołać nad produktem. W terminologii OData, akcja jest *powiązany* jednostek produktu.

>Akcji ma efekty uboczne, na serwerze. Z tego powodu są wywoływane, za pomocą żądania HTTP POST. Akcje mogą mieć parametry i zwracane typy, które są opisane w metadanych usługi. Klient wysyła parametrów w treści żądania, a serwer wysyła wartości zwracanej w treści odpowiedzi. Aby wywołać akcję "Współczynnik produkt", klient wysyła WPIS do identyfikatora URI, jak pokazano poniżej:

[!code-console[Main](odata-actions/samples/sample2.cmd)]

Dane w żądaniu POST jest po prostu ocena produktu:

[!code-console[Main](odata-actions/samples/sample3.cmd)]

## <a name="declare-the-action-in-the-entity-data-model"></a>Zadeklaruj akcji w modelu Entity Data Model

W konfiguracji interfejsu API sieci Web Dodaj do modelu entity data model (EDM struktury) akcję:

[!code-csharp[Main](odata-actions/samples/sample4.cs)]

Ten kod definiuje "RateProduct" jako akcję, które mogą być wykonywane na jednostkach produktu. Również deklaruje, że akcja jest wykonywana **int** parametr o nazwie "Ocena" i zwraca **int** wartość.

## <a name="add-the-action-to-the-controller"></a>Dodawanie akcji do kontrolera

Akcja "RateProduct" jest powiązany z jednostki produktu. Aby zastosować akcję, Dodaj metodę o nazwie `RateProduct` kontrolerowi produktów:

[!code-csharp[Main](odata-actions/samples/sample5.cs)]

Należy zauważyć, że nazwa metody odpowiada nazwie w akcji EDM. Metoda ma dwa parametry:

- *klucz*: klucz produktu do szybkości.
- *Parametry*: słownika wartości parametrów akcji.

Jeśli używasz domyślnych Konwencji tras parametr klucza musi mieć nazwę "klucza". Warto również uwzględnić **[FromOdataUri]** atrybutu, jak pokazano. Ten atrybut informuje reguły składni OData używane, gdy jej analizuje klucza z identyfikatora URI żądania interfejsu API sieci Web.

Użyj *parametry* słownika można pobrać parametrów akcji:

[!code-csharp[Main](odata-actions/samples/sample6.cs)]

Jeśli klient wysyła parametry akcji w poprawny format, wartość **ModelState.IsValid** ma wartość true. W takim przypadku można użyć **ODataActionParameters** słownika do uzyskania wartości parametrów. W tym przykładzie `RateProduct` akcji przyjmuje jeden parametr o nazwie "Ocena".

## <a name="action-metadata"></a>Metadane akcji

Aby wyświetlić metadane usługi, Wyślij żądanie Pobierz do /odata/$ metadanych. Poniżej przedstawiono część metadanych, który deklaruje `RateProduct` akcji:

[!code-xml[Main](odata-actions/samples/sample7.xml)]

**FunctionImport** elementu deklaruje akcji. Większość pól jest oczywista, ale dwa są warte odnotowania:

- **Może być powiązana** oznacza, że akcja może być wywołana na jednostkę docelową co najmniej pewien czas.
- **Funkcja IsAlwaysBindable** oznacza, że akcja zawsze może być wywołana na jednostkę docelową.

Różnica polega na niektóre akcje są zawsze dostępne dla klientów, że inne akcje mogą zależeć od stan jednostki. Na przykład załóżmy, że definiowania akcji "Kup". Można kupić tylko elementu, który znajduje się w magazynie. Jeśli element znajduje się na stanie, klient nie może wywołać tę akcję.

Podczas definiowania EDM, **akcji** metoda tworzy zawsze powiązać akcji:

[!code-csharp[Main](odata-actions/samples/sample8.cs?highlight=1)]

Omówię nie zawsze — możliwości powiązania akcji (nazywane również *przejściowy* akcji) w dalszej części tego tematu.

## <a name="invoking-the-action"></a>Wywoływania akcji

Teraz zobaczmy, jak klient powodowałoby wywołanie pliku wykonywalnego tej akcji. Załóżmy, że klient chce udzielić ma klasyfikację od 2 do produktu o identyfikatorze = 4. Oto przykładowy komunikat żądania, przy użyciu formatu JSON w treści żądania:

[!code-console[Main](odata-actions/samples/sample9.cmd)]

Oto komunikat odpowiedzi:

[!code-console[Main](odata-actions/samples/sample10.cmd)]

## <a name="binding-an-action-to-an-entity-set"></a>Powiązania akcji do zbioru jednostek

W poprzednim przykładzie, akcja jest powiązany z pojedynczej jednostki: klient ocenia jednego produktu. Możesz również powiązać akcję do kolekcji jednostek. Po prostu wprowadź następujące zmiany:

W EDM, Dodaj akcję w jednostce **kolekcji** właściwości.

[!code-csharp[Main](odata-actions/samples/sample11.cs?highlight=1)]

W przypadku metody kontrolera pominąć *klucz* parametru.

[!code-csharp[Main](odata-actions/samples/sample12.cs)]

Teraz klient wywołuje akcję w zestawie jednostek produktów:

[!code-console[Main](odata-actions/samples/sample13.cmd)]

## <a name="actions-with-collection-parameters"></a>Akcje z parametrami kolekcji

Akcje może mieć parametrów, które przyjmują kolekcję wartości. W EDM, użyj **CollectionParameter&lt;T&gt;**  Aby zadeklarować parametr.

[!code-csharp[Main](odata-actions/samples/sample14.cs)]

To deklaruje parametr o nazwie "Klasyfikacje", które z kolekcji **int** wartości. W przypadku metody kontrolera będzie nadal się pojawiać wartość parametru **ODataActionParameters** obiektu, ale teraz wartość **ICollection&lt;int&gt;**  wartość:

[!code-csharp[Main](odata-actions/samples/sample15.cs)]

## <a name="transient-actions"></a>Akcje przejściowe

W tym przykładzie "RateProduct" użytkownicy zawsze ocenić produktu, tak akcja jest zawsze dostępna. Jednak niektóre akcje są zależne od stanu jednostki. Na przykład w usłudze wideo wypożyczeń akcji "Wyewidencjonowania" nie zawsze jest dostępne. (Jest to uzależnione czy kopię tego wideo jest dostępna.) Ten typ akcji jest wywoływana *przejściowy* akcji.

W metadanych usługi ma przejściowy akcji **IsAlwaysBindable** równa false. Który jest faktycznie wartość domyślna, więc metadane będzie wyglądać następująco:

[!code-xml[Main](odata-actions/samples/sample16.xml)]

Oto dlaczego jest to istotne: Jeśli akcja jest przejściowy, serwer musi sprawdzić klientowi, gdy akcja jest dostępna. Jest to możliwe dzięki tym link do akcji w jednostce. Oto przykład dla jednostki film:

[!code-console[Main](odata-actions/samples/sample17.cmd)]

Właściwość "#CheckOut" zawiera łącze do Akcja CheckOut. Jeśli akcja nie jest dostępna, serwer pomija łącze.

Aby zadeklarować przejściowy akcji w EDM, należy wywołać **TransientAction** metody:

[!code-csharp[Main](odata-actions/samples/sample18.cs)]

Ponadto należy podać funkcji, która zwraca łącze akcji dla danej jednostki. Ustaw tę funkcję, wywołując **HasActionLink**. Funkcję można napisać jako wyrażenie lambda:

[!code-csharp[Main](odata-actions/samples/sample19.cs)]

Jeśli akcja jest dostępna, wyrażenie lambda zwraca link do akcji. Reprezentuje serializator OData zawiera ten link, gdy to szereguje jednostki. Jeśli akcja nie jest dostępna, funkcja zwraca `null`.

## <a name="additional-resources"></a>Dodatkowe zasoby

[Przykład akcji OData](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v3/ODataActionsSample/)
