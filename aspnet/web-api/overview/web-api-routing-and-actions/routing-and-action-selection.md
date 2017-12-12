---
uid: web-api/overview/web-api-routing-and-actions/routing-and-action-selection
title: "Wybieranie routingu i akcji w składniku ASP.NET Web API | Dokumentacja firmy Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2012
ms.topic: article
ms.assetid: bcf2d223-cb7f-411e-be05-f43e96a14015
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-and-action-selection
msc.type: authoredcontent
ms.openlocfilehash: 02c2a01ef8ec2b5a49f2c303ee61f02702a3ba54
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="routing-and-action-selection-in-aspnet-web-api"></a>Wybieranie routingu i akcji w składniku ASP.NET Web API
====================
przez [Wasson Jan](https://github.com/MikeWasson)

W tym artykule opisano, jak składnika ASP.NET Web API kieruje żądania HTTP do określonej akcji w kontrolerze.

> [!NOTE]
> Szczegółowe omówienie routingu, zobacz [routingu na platformie ASP.NET Web API](routing-in-aspnet-web-api.md).


W tym artykule analizuje treść proces routingu. Jeśli okaże się, że niektóre żądania nie pobieraj kierowane w oczekiwany sposób tworzenia projektu interfejsu API sieci Web, miejmy nadzieję, że ten artykuł pomoże.

Routing ma trzy główne fazy:

1. Dopasowywania identyfikatora URI w szablonie trasy.
2. Wybiera kontroler.
3. Wybranie akcji.

Niektórych części procesu można zastąpić zachowania niestandardowych. W tym artykule opisano I zachowanie domyślne. Na koniec I uwaga miejsca, w którym można dostosować zachowanie.

## <a name="route-templates"></a>Szablony trasy

Szablon trasy wygląda podobnie do ścieżka identyfikatora URI, ale może mieć wartości symbolu zastępczego, wskazane w nawiasach klamrowych:

[!code-csharp[Main](routing-and-action-selection/samples/sample1.ps1)]

Gdy tworzona jest trasa, można podać wartości domyślnej niektóre lub wszystkie symbole zastępcze:

[!code-csharp[Main](routing-and-action-selection/samples/sample2.cs)]

Można też podać ograniczeń, które ograniczenia, jak segment identyfikatora URI może być zgodne z symbolem zastępczym:

[!code-csharp[Main](routing-and-action-selection/samples/sample3.js)]

Platformę próbuje odpowiada segmentów w ścieżce identyfikator URI do szablonu. Literały w szablonie muszą być zgodne. Symbol zastępczy dopasowuje dowolną wartością, chyba że zostanie ograniczenia. Platformę nie pasuje do innych części identyfikatora URI, takich jak nazwa hosta lub parametrów zapytania. Platformę wybiera pierwszy trasy w tabeli tras, który pasuje do identyfikatora URI.

Istnieją dwa specjalne symbole zastępcze: "{controller}" i "{action}".

- "{controller}" zawiera nazwę kontrolera.
- "{action}" zawiera nazwę akcji. W składniku Web API standardowej konwencji jest, aby pominąć "{action}".

### <a name="defaults"></a>Wartość domyślna

Jeśli podasz wartości domyślnych trasy będzie odpowiadała identyfikator URI, który nie ma tych segmentów. Na przykład:

[!code-csharp[Main](routing-and-action-selection/samples/sample4.cs)]

Identyfikator URI "`http://localhost/api/products`" tej trasie. Segment "{kategorii}" jest przypisany wartość domyślna "all".

### <a name="route-dictionary"></a>Słownika trasy

Jeśli w ramach znalezienia dopasowania dla identyfikatora URI, tworzy słownik, który zawiera wartość dla każdego symbolu zastępczego. Klucze są nazwy symbolu zastępczego, nie włączając nawiasów klamrowych. Wartości te są pobierane z ścieżka identyfikatora URI lub wartości domyślne. Słownik jest przechowywany w **IHttpRouteData** obiektu.

W tej fazie dopasowywanie trasy podobnie jak inne symbole zastępcze są traktowane specjalnych "{controller}" i "{action}" symbole zastępcze. Po prostu są przechowywane w słowniku z innych wartości.

Domyślnie może mieć wartości specjalne **RouteParameter.Optional**. Symbol zastępczy pobiera przypisany tej wartości, wartość nie została dodana do słownika trasy. Na przykład:

[!code-csharp[Main](routing-and-action-selection/samples/sample5.cs)]

Dla ścieżki identyfikatora URI "interfejsu api/produktów" będzie zawierać słownika trasy:

- Kontroler: "produktów"
- Kategoria: "wszystkie"

Dla "interfejsu api/produkty/toys/123" jednak słownika trasy będzie zawierać:

- Kontroler: "produktów"
- Kategoria: "toys"
- Identyfikator: "123"

Ustawienia domyślne mogą również obejmować wartość, która nie występować w dowolnym miejscu w szablonie trasy. Jeśli trasa odpowiada, ta wartość jest przechowywane w słowniku. Na przykład:

[!code-csharp[Main](routing-and-action-selection/samples/sample6.cs)]

Jeśli ścieżka identyfikatora URI to "interfejsu api głównego/8", słownik będzie zawierać dwóch wartości:

- Kontroler: "klientów"
- Identyfikator: "8"

## <a name="selecting-a-controller"></a>Wybiera kontroler

Wybór kontrolera jest obsługiwany przez **IHttpControllerSelector.SelectController** metody. Ta metoda przyjmuje **HttpRequestMessage** wystąpienia i zwraca **HttpControllerDescriptor**. Domyślna implementacja jest zapewniana przez **DefaultHttpControllerSelector** klasy. Ta klasa korzysta z prostego algorytmu:

1. Szukaj w słowniku trasy dla klucza "controller".
2. Pobrać wartość tego klucza i Dołącz ciąg "Controller", aby uzyskać nazwę typu kontrolera.
3. Poszukaj kontrolera interfejsu API sieci Web o tej nazwie typu.

Na przykład jeśli słownika trasy zawiera pary klucz wartość "controller" = "produktów", "ProductsController" jest typ kontrolera. Jeśli określono żadnego typu zgodnego lub wiele dopasowań, platformę zwraca błąd do klienta.

W kroku 3 **DefaultHttpControllerSelector** używa **IHttpControllerTypeResolver** interfejsu, aby uzyskać listę typów kontrolera interfejsu API sieci Web. Domyślna implementacja **IHttpControllerTypeResolver** zwraca wszystkich publicznych klas, które implementują () **IHttpController**, (b) jest abstrakcyjny i (c) ma nazwę, która kończy się na "Controller".

## <a name="action-selection"></a>Wybór działania

Po wybraniu kontrolera, platformę wybiera akcję wywołując **IHttpActionSelector.SelectAction** metody. Ta metoda przyjmuje **HttpControllerContext** i zwraca **HttpActionDescriptor**.

Domyślna implementacja jest zapewniana przez **ApiControllerActionSelector** klasy. Aby wybrać akcję, odbywa się na następujących czynności:

- Metoda HTTP żądania.
- Symbol zastępczy "{action}" w szablonie trasy, jeśli jest obecny.
- Parametry akcji w kontrolerze.

Przed patrzeć algorytm zaznaczenia, należy poznać kilka rzeczy, o akcji kontrolera.

**Które metody na kontrolerze są traktowane jako "Akcje"?** Po wybraniu akcji, platformę przegląda tylko metody wystąpienia publicznego na kontrolerze. Ponadto wyklucza ["specjalną nazwą"](https://msdn.microsoft.com/en-us/library/system.reflection.methodbase.isspecialname) metod (konstruktorów, zdarzenia przeciążenia operatora i tak dalej) i dziedziczone z metody **klasy ApiController** klasy.

**Metody HTTP.** Platformę wybiera tylko akcje, które odpowiada metoda HTTP żądania określane w następujący sposób:

1. Metoda HTTP można określić za pomocą atrybutu: **AcceptVerbs**, **HttpDelete**, **HttpGet**, **HttpHead**,  **Httpoptions miał**, **HttpPatch**, **HttpPost**, lub **HttpPut**.
2. W przeciwnym razie jeśli nazwa metody kontrolera rozpoczyna się od "Get", "Post", "Put", "Delete", "Head", "Opcje" lub "Poprawka", następnie według Konwencji obsługiwane przez akcję tej metody HTTP.
3. Jeśli żaden z powyższych metoda obsługuje POST.

**Powiązania parametrów.** Wiązanie parametru jest sposób interfejsu API sieci Web tworzy wartość dla parametru. Oto reguły domyślnej dla parametru wiązania:

- Proste typy są pobierane z identyfikatora URI.
- Typy złożone są pobierane z treści żądania.

Proste typy obejmują wszystkie [typów pierwotnych .NET Framework](https://msdn.microsoft.com/en-us/library/system.type.isprimitive), plus **DateTime**, **dziesiętną**, **identyfikatora Guid**, **ciągu** , i **TimeSpan**. Dla każdej akcji co najwyżej jeden parametr można odczytać treści żądania.

> [!NOTE]
> Istnieje możliwość zastąpienia domyślnych reguł powiązania. Zobacz [wiązanie parametru WebAPI kulisy](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx).


Tło Oto algorytm wybór akcji.

1. Utwórz listę wszystkich działań na kontrolerze, który odpowiada metoda żądania HTTP.
2. Jeśli słownika trasy ma wpis "Akcja", Usuń akcje, którego nazwa jest niezgodna z tej wartości.
3. Spróbuj odpowiadające parametry akcji do identyfikatora URI, w następujący sposób: 

    1. Dla każdej akcji pobrać listę parametrów, które są typu prostego, gdzie pobiera parametr wiązania z identyfikatora URI. Wyklucz następujące parametry opcjonalne.
    2. Z tej listy prób znalezienia dopasowania dla każdej nazwy parametru w słowniku trasy lub w ciągu zapytania identyfikatora URI. Dopasowań uwzględniana jest wielkość liter i nie zależą od kolejność parametrów.
    3. Wybierz akcję, gdzie każdy parametr na liście ma dopasowania w identyfikatorze URI.
    4. Jeśli bardziej tego jedną akcję spełnia te kryteria, wybierz jedną z większości dopasowań parametru.
4. Ignoruj akcji z **[NonAction]** atrybutu.

Krok #3 jest prawdopodobnie najbardziej mylące. Podstawową koncepcją jest parametrem można uzyskać jego wartość z identyfikatora URI, z treści żądania albo z niestandardowego powiązania. Dla parametrów, które pochodzą z identyfikatora URI chcemy upewnić się, że identyfikator URI zawiera wartości tego parametru, w polu Ścieżka (za pomocą słownika trasy) lub w ciągu zapytania.

Na przykład należy rozważyć następujące działania:

[!code-csharp[Main](routing-and-action-selection/samples/sample7.cs)]

*Identyfikator* powiązanie parametru identyfikatora URI. W związku z tym ta akcja może wyłącznie odpowiadać identyfikator URI, który zawiera wartość "id" w słowniku trasy lub w ciągu zapytania.

Parametry opcjonalne są wyjątek, ponieważ są opcjonalne. Parametr opcjonalny jest OK Jeśli powiązanie nie może pobrać wartości z identyfikatora URI.

Typy złożone są wyjątkiem z różnych przyczyn. Typ złożony można powiązać tylko z identyfikatora URI za pomocą niestandardowego powiązania. Jednak w takim przypadku platformę nie wiedzieć z wyprzedzeniem, czy parametr czy powiązania do określonego identyfikatora URI. Aby dowiedzieć się, czy trzeba wywołać powiązanie. Celem algorytm zaznaczenie ma wybierz akcję z opisu statycznych, zanim wywoła powiązań. W związku z tym typy złożone są wykluczone z algorytm dopasowania.

Po wybraniu akcji są wywoływane wszystkie powiązania parametrów.

Podsumowanie:

- Akcja musi być zgodna metoda HTTP żądania.
- Nazwa akcji musi być zgodna wpis "Akcja" w słowniku trasy, jeśli jest obecny.
- Dla każdego parametru akcji Jeśli parametr pochodzi z identyfikatora URI, następnie nazwa parametru musi zostać znaleziony w słowniku trasy lub w ciągu zapytania identyfikatora URI. (Opcjonalne parametry i parametry o typach złożonych, są wyłączone.)
- Podjąć próbę dopasowania największą liczbą parametrów. Najlepsze dopasowanie może być metodą bez parametrów.

## <a name="extended-example"></a>Przykład rozszerzone

Trasy:

[!code-csharp[Main](routing-and-action-selection/samples/sample8.cs)]

Kontroler:

[!code-csharp[Main](routing-and-action-selection/samples/sample9.cs)]

Żądania HTTP:

[!code-console[Main](routing-and-action-selection/samples/sample10.cmd)]

### <a name="route-matching"></a>Trasy dopasowania

Identyfikator URI trasie, o nazwie "DefaultApi". Słownika trasy zawiera następujące wpisy:

- Kontroler: "produktów"
- Identyfikator: "1"

Słownika trasy nie zawiera parametrów ciągu zapytania, "version" i "szczegóły", ale nadal będą one traktowane podczas wybór akcji.

### <a name="controller-selection"></a>Wybór kontrolera

Z wpisu "controller" w słowniku trasy, jest typ kontrolera `ProductsController`.

### <a name="action-selection"></a>Wybór działania

Żądanie HTTP jest żądaniem GET. Akcji kontrolera, które obsługują GET są `GetAll`, `GetById`, i `FindProductsByName`. Słownika trasy nie zawiera pozycji "Akcja", więc musimy nie jest zgodna z nazwą akcji.

Następnie próbujemy dopasować nazwy parametrów działań, patrzeć tylko akcje GET.

| Akcja | Parametry dopasowania |
| --- | --- |
| `GetAll` | brak |
| `GetById` | "id" |
| `FindProductsByName` | "Nazwa" |

Zwróć uwagę, że *wersji* parametr `GetById` jest uważana za, ponieważ jest parametrem opcjonalnym.

`GetAll` Metoda trivially odpowiada. `GetById` Metody zgodny, ponieważ słownika trasy zawiera "id". `FindProductsByName` — Metoda nie jest zgodna.

`GetById` Metody wins, ponieważ jest on zgodny jeden parametr, a żadne parametry dla `GetAll`. Metoda jest wywoływana z następujących wartości parametrów:

- *Identyfikator* = 1
- *Wersja* = 1.5

Należy zauważyć, że nawet jeśli *wersji* nie były używane w algorytmie zaznaczenia, wartość parametru pochodzi z ciągu zapytania identyfikatora URI.

## <a name="extension-points"></a>Punkty rozszerzenia

Interfejsu API sieci Web udostępniają punkty rozszerzeń dla niektórych części procesu routingu.

| Interface | Opis |
| --- | --- |
| **IHttpControllerSelector** | Wybiera kontrolera. |
| **IHttpControllerTypeResolver** | Pobiera listę typów kontrolerów. **DefaultHttpControllerSelector** wybiera typ kontrolera z tej listy. |
| **IAssembliesResolver** | Pobiera listę zestawów projektu. **IHttpControllerTypeResolver** interfejs używa tej listy można znaleźć typów kontrolerów. |
| **IHttpControllerActivator** | Tworzy nowe wystąpienia kontrolera. |
| **IHttpActionSelector** | Wybiera akcję. |
| **IHttpActionInvoker** | Wywołuje akcję. |

Aby podać własne wdrożenia dla każdego z tych interfejsów, należy użyć **usług** kolekcji na **HttpConfiguration** obiektu:

[!code-csharp[Main](routing-and-action-selection/samples/sample11.cs)]
