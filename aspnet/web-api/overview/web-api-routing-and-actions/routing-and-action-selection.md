---
uid: web-api/overview/web-api-routing-and-actions/routing-and-action-selection
title: Routing i wybieranie akcji w interfejsie Web API platformy ASP.NET | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 07/27/2012
ms.assetid: bcf2d223-cb7f-411e-be05-f43e96a14015
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-and-action-selection
msc.type: authoredcontent
ms.openlocfilehash: 2421d3dd134e9188faf6933b076fb9d22a119617
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37832833"
---
<a name="routing-and-action-selection-in-aspnet-web-api"></a>Routing i wybieranie akcji we wzorcu ASP.NET Web API
====================
przez [Mike Wasson](https://github.com/MikeWasson)

W tym artykule opisano, jak ASP.NET Web API kieruje żądania HTTP do określonej akcji w kontrolerze.

> [!NOTE]
> Aby uzyskać ogólne omówienie routingu, zobacz [routingu ASP.NET Web API](routing-in-aspnet-web-api.md).


W tym artykule przedstawiono szczegółowych informacji dotyczących procesu routingu. Jeśli podczas tworzenia projektu składnika Web API okaże się, że niektóre żądania nie uzyskasz kierowane sposób, w których oczekujesz, miejmy nadzieję ten artykuł pomoże.

Routing obejmuje trzy główne etapy:

1. Dopasowywania identyfikatora URI do szablonu trasy.
2. Wybiera kontroler.
3. Wybieranie akcji.

Możesz zastąpić niektóre części procesu własne niestandardowe zachowania. W tym artykule I opisano domyślne zachowanie. Na koniec czy należy pamiętać, miejsca, w którym można dostosować zachowanie.

## <a name="route-templates"></a>Szablonów tras

Szablon trasy wygląda podobnie do ścieżki identyfikatora URI, ale może mieć wartości symboli zastępczych, wskazane w nawiasach klamrowych:

[!code-csharp[Main](routing-and-action-selection/samples/sample1.ps1)]

Podczas tworzenia trasy, możesz podać wartości domyślne, niektóre lub wszystkie symbole zastępcze:

[!code-csharp[Main](routing-and-action-selection/samples/sample2.cs)]

Możesz też podać ograniczenia, które ograniczają sposób segmentem identyfikatora URI może odnosić się do symbolu zastępczego:

[!code-csharp[Main](routing-and-action-selection/samples/sample3.js)]

Struktura próbuje dopasować segmenty ścieżki identyfikatora URI do szablonu. Literały w szablonie musi dokładnie pasować. Symbol zastępczy pasuje do dowolnej wartości, chyba że określono ograniczenia. Struktura jest niezgodna z innych części identyfikatora URI, takich jak nazwa hosta lub parametrów zapytań. Struktura wybiera pierwszy trasy w tabeli tras, która pasuje do identyfikatora URI.

Istnieją dwa specjalne symbole zastępcze: "{controller}" i "{action}".

- "{controller}" zawiera nazwę kontrolera.
- "{action}" zawiera nazwę akcji. W interfejsie API sieci Web standardowej konwencji jest pominięcie "{action}".

### <a name="defaults"></a>Wartość domyślna

Jeśli podano wartości domyślne trasy będą zgodne identyfikator URI, który nie ma te segmenty. Na przykład:

[!code-csharp[Main](routing-and-action-selection/samples/sample4.cs)]

Identyfikator URI "`http://localhost/api/products`" pasuje do tej trasy. Segment "{category}" jest przypisana wartość domyślna "all".

### <a name="route-dictionary"></a>Słownika trasy

Jeśli struktura znajdzie dopasowanie dla identyfikatora URI, tworzy słownik, który zawiera wartość dla każdego symbolu zastępczego. Klucze są nazw zastępczych, nie wliczając nawiasów klamrowych. Wartości te są pobierane z składnik path identyfikatora URI lub wartości domyślne. Słownik jest przechowywany w **IHttpRouteData** obiektu.

W tej fazie dopasowania trasy specjalnego "{controller}" i symbole zastępcze "{action}" są traktowane tak samo jak inne symbole zastępcze. Po prostu są one przechowywane w słowniku inne wartości.

Domyślny może mieć specjalna wartość **RouteParameter.Optional**. Jeśli symbol zastępczy, pobiera przypisaną tę wartość, wartość nie zostanie dodany do słownika trasy. Na przykład:

[!code-csharp[Main](routing-and-action-selection/samples/sample5.cs)]

Dla ścieżki identyfikatora URI "interfejsu api/produkty" będzie zawierać słownika trasy:

- Kontroler: "produkty"
- Kategoria: "all"

Dla "interfejsu api/produkty/zabawki/123" jednak słownika trasy będzie zawierać:

- Kontroler: "produkty"
- Kategoria: "zabawki"
- Identyfikator: "123"

Wartości domyślne mogą również zawierać wartość, która nie występować w dowolnym miejscu w szablonie trasy. Jeśli trasa odpowiada, ta wartość jest przechowywany w słowniku. Na przykład:

[!code-csharp[Main](routing-and-action-selection/samples/sample6.cs)]

Jeśli ścieżka identyfikatora URI to "interfejsu api/główny/8", słownik będzie zawierać dwie wartości:

- Kontroler: "klientów"
- Identyfikator: "8"

## <a name="selecting-a-controller"></a>Wybiera kontroler

Wybieranie kontrolera jest obsługiwany przez **IHttpControllerSelector.SelectController** metody. Ta metoda przyjmuje **HttpRequestMessage** wystąpienie i zwraca **HttpControllerDescriptor**. Domyślna implementacja jest dostarczany przez **DefaultHttpControllerSelector** klasy. Ta klasa używa algorytmu proste:

1. Znajdź słownika trasy dla klucza "controller".
2. Pobrać wartość dla tego klucza i Dołącz ciąg "Controller" w celu otrzymania nazwy typu kontrolera.
3. Zwróć uwagę na kontrolerze interfejsu API sieci Web o tej nazwie typu.

Na przykład jeśli słownika trasy zawiera pary klucz wartość "controller" = "produkty", a następnie typ kontrolera jest "ProductsController". Jeśli określono żadnego typu zgodnego lub wiele dopasowań, struktura zwraca błąd do klienta.

W kroku 3 **DefaultHttpControllerSelector** używa **IHttpControllerTypeResolver** interfejsu, aby uzyskać listę typów kontrolera interfejsu API sieci Web. Domyślna implementacja klasy **IHttpControllerTypeResolver** zwraca wszystkie klasy publiczne, które implementują () **IHttpController**, (b) jest abstrakcyjna i (c) mają nazwę, która kończy się na "Controller".

## <a name="action-selection"></a>Wybór akcji

Po wybraniu kontrolera, struktura wybiera akcję, wywołując **IHttpActionSelector.SelectAction** metody. Ta metoda przyjmuje **HttpControllerContext** i zwraca **HttpActionDescriptor**.

Domyślna implementacja jest dostarczany przez **ApiControllerActionSelector** klasy. Aby wybrać akcję, odbywa się na następujących czynności:

- Metoda HTTP żądania.
- Symbol zastępczy "{action}" w szablonie trasy, jeśli jest obecny.
- Parametry akcji w kontrolerze.

Przed obejrzeniem algorytm wybór, należy zrozumieć kilka rzeczy, o akcji kontrolera.

**Które metody na kontrolerze są traktowane jako "Akcje"?** Wybierając akcję, struktura przegląda tylko metody publiczne wystąpienia kontrolera. Ponadto nie obejmuje ["specjalne name"](https://msdn.microsoft.com/library/system.reflection.methodbase.isspecialname) metody (konstruktory, zdarzenia, przeciążenia operatorów i tak dalej) i metod odziedziczone **klasy ApiController** klasy.

**Metody HTTP.** Struktura wybiera tylko akcje, które odpowiada metoda HTTP żądania, określany w następujący sposób:

1. Metoda HTTP można określić za pomocą atrybutu: **AcceptVerbs**, **HttpDelete**, **HttpGet**, **HttpHead**,  **Httpoptions miał**, **HttpPatch**, **HttpPost**, lub **HttpPut**.
2. W przeciwnym razie jeśli nazwa metody kontrolera rozpoczyna się od "Get", "Post", "Put", "Delete", "Head", "Opcje" lub "Poprawka", następnie zgodnie z Konwencją obsługiwane przez akcję tę metodę HTTP.
3. Jeśli żadne z powyższych metoda obsługuje POST.

**Powiązania parametrów.** Wiązanie parametru jest o tym, jak interfejs API sieci Web tworzy wartość dla parametru. Poniżej przedstawiono domyślną regułę dla wiązania parametru:

- Proste typy są pobierane z identyfikatora URI.
- Typy złożone są pobierane z treści żądania.

Proste typy obejmują wszystkie [pierwotnych typów programu .NET Framework](https://msdn.microsoft.com/library/system.type.isprimitive), oraz **daty/godziny**, **dziesiętna**, **Guid**, **ciągu** , i **TimeSpan**. Dla każdej akcji co najwyżej jeden parametr można odczytać treści żądania.

> [!NOTE]
> Istnieje możliwość zastąpić domyślne reguły powiązania. Zobacz [wiązanie parametru WebAPI kulisy](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx).


W tle Oto algorytm wybór akcji.

1. Utwórz listę wszystkich działań na kontrolerze, który odpowiada metoda żądania HTTP.
2. Jeśli słownika trasy ma wpis "action", Usuń akcje, których nazwa jest niezgodna z tej wartości.
3. Podjąć próbę dopasowania parametry akcji do identyfikatora URI, w następujący sposób: 

    1. Dla każdej akcji zostanie wyświetlona lista parametrów, które są typu prostego, w którym pobiera parametr wiązania z identyfikatora URI. Wyklucz następujące parametry opcjonalne.
    2. Z tej listy spróbuj znaleźć dopasowania dla każdej nazwy parametru słownika trasy albo w ciągu zapytania identyfikatora URI. Dopasowania jest rozróżniana wielkość liter i nie są zależne od kolejność parametrów.
    3. Wybierz akcję, której każdy parametr na liście jest zgodny w identyfikatorze URI.
    4. Jeśli bardziej tego jedną akcję spełnia te kryteria, wybierz jedną z większości dopasowaniami parametru.
4. Ignoruj akcji przy użyciu **[NonAction]** atrybutu.

Krok #3 jest prawdopodobnie najbardziej mylące. Podstawowa koncepcja jest parametrem można jego wartość z identyfikatora URI, z treści żądania albo z niestandardowego powiązania. Dla parametrów, które pochodzą z identyfikatora URI chcemy się upewnić, że identyfikator URI faktycznie zawiera wartości tego parametru, w polu Ścieżka (za pomocą słownika trasy) lub w ciągu zapytania.

Na przykład rozważmy następującą akcję:

[!code-csharp[Main](routing-and-action-selection/samples/sample7.cs)]

*Identyfikator* powiązanie parametru identyfikatora URI. W związku z tym ta akcja może wyłącznie odpowiadać identyfikator URI, który zawiera wartość "id" słownika trasy albo w ciągu zapytania.

Następujące parametry opcjonalne są wyjątku, ponieważ są opcjonalne. Parametr opcjonalny jest OK Jeśli wiązanie nie może pobrać wartość z identyfikatora URI.

Typy złożone są wyjątkiem od innego powodu. Typ złożony można powiązać tylko z identyfikatora URI za pomocą niestandardowego powiązania. Jednak w takim przypadku ramach nie wiedzieć z wyprzedzeniem czy parametr będzie powiązać z określonego identyfikatora URI. Aby dowiedzieć się, należałoby do wywołania wiązania. Celem algorytm wybór jest wybierz akcję z opisu statyczne, przed wywołaniem wszystkie powiązania. W związku z tym typy złożone są wykluczone z algorytm dopasowania.

Po wybraniu akcji są wywoływane wszystkie powiązania parametrów.

Podsumowanie:

- Akcja musi być zgodna metoda HTTP żądania.
- Nazwa akcji musi odpowiadać wpis "action" w słownika trasy, jeśli jest obecny.
- Dla każdego parametru akcji Jeśli parametr pochodzi z identyfikatora URI, następnie nazwa parametru musi można znaleźć słownika trasy albo w ciągu zapytania identyfikatora URI. (Opcjonalne parametry i parametry za pomocą typy złożone są wyłączone.)
- Podjąć próbę dopasowania z największą liczbą parametrów. Najlepsze dopasowanie może być metoda bez parametrów.

## <a name="extended-example"></a>Przykład rozszerzone

Trasy:

[!code-csharp[Main](routing-and-action-selection/samples/sample8.cs)]

Kontroler:

[!code-csharp[Main](routing-and-action-selection/samples/sample9.cs)]

Żądania HTTP:

[!code-console[Main](routing-and-action-selection/samples/sample10.cmd)]

### <a name="route-matching"></a>Kierowanie dopasowania

Trasy o nazwie "DefaultApi" pasuje do identyfikatora URI. Słownik trasa zawiera następujące wpisy:

- Kontroler: "produkty"
- Identyfikator: "1"

Słownika trasy nie zawiera parametrów ciągu zapytania, "wersja" i "szczegóły", ale nadal będą one traktowane podczas wybór akcji.

### <a name="controller-selection"></a>Wybieranie kontrolera

Z pozycji "controller" w słowniku trasy, jest typ kontrolera `ProductsController`.

### <a name="action-selection"></a>Wybór akcji

Żądanie HTTP jest żądaniem GET. Akcji kontrolera, które obsługują GET są `GetAll`, `GetById`, i `FindProductsByName`. Słownika trasy nie zawiera wpis dla "action", więc nie trzeba być zgodna z nazwą akcji.

Następnie próbujemy dopasować nazw parametrów dla akcji, patrząc tylko operacje GET.

| Akcja | Parametry dopasowania |
| --- | --- |
| `GetAll` | brak |
| `GetById` | "id" |
| `FindProductsByName` | "name" |

Należy zauważyć, że *wersji* parametru `GetById` nie uznaje się, ponieważ jest on opcjonalny parametr.

`GetAll` Metoda odpowiada przypadku. `GetById` Metoda również jest zgodny, ponieważ słownika trasy zawiera "id". `FindProductsByName` Metoda nie jest zgodny.

`GetById` Metoda serwery wins, ponieważ jest on zgodny jeden parametr, a żadnych parametrów `GetAll`. Metoda jest wywoływana z następującymi wartościami parametrów:

- *id* = 1
- *Wersja* = 1.5

Należy zauważyć, że nawet jeśli *wersji* nie był używany w algorytm wybór wartości parametru pochodzi z ciągu zapytania identyfikatora URI.

## <a name="extension-points"></a>Punkty rozszerzenia

Internetowy interfejs API udostępnia punkty rozszerzenia dla niektórych części procesu routingu.

| Interface | Opis |
| --- | --- |
| **IHttpControllerSelector** | Wybiera kontroler. |
| **IHttpControllerTypeResolver** | Pobiera listę typów kontrolerów. **DefaultHttpControllerSelector** wybiera typ kontrolera z tej listy. |
| **IAssembliesResolver** | Pobiera listę zestawów do projektu. **IHttpControllerTypeResolver** interfejsu używa tej listy, aby znaleźć typy kontrolerów. |
| **IHttpControllerActivator** | Tworzy nowe wystąpienia kontrolera. |
| **IHttpActionSelector** | Wybiera akcję. |
| **IHttpActionInvoker** | Wywołuje akcję. |

Aby dostarczyć własnej implementacji dla każdego z tych interfejsów, użyj **usług** kolekcji na **HttpConfiguration** obiektu:

[!code-csharp[Main](routing-and-action-selection/samples/sample11.cs)]
