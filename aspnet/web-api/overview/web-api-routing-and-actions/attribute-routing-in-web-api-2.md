---
uid: web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
title: "Atrybut routingu w składniku ASP.NET Web API 2 | Dokumentacja firmy Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: 979d6c9f-0129-4e5b-ae56-4507b281b86d
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: 173add73a150d3e13ae243d6548463da912dadee
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="attribute-routing-in-aspnet-web-api-2"></a>Atrybut routingu w składniku ASP.NET Web API 2
====================
przez [Wasson Jan](https://github.com/MikeWasson)

*Routing* jest sposób interfejsu API sieci Web odpowiada identyfikatora URI do akcji. Składnik Web API 2 obsługuje nowy typ routingu, nazywany *trasami atrybutów*. Jak wskazuje nazwę, trasami atrybutów używa atrybutów do definiowania trasy. Atrybut routingu zapewnia większą kontrolę nad identyfikatory URI w interfejsie API sieci web. Na przykład można łatwo utworzyć identyfikatory URI, które opisują hierarchie zasobów.

Wcześniejszych styl routingu, o nazwie opartych na konwencjach routingu, jest nadal w pełni obsługiwane. W rzeczywistości można łączyć obie techniki, w tym samym projekcie.

W tym temacie pokazano, jak włączyć routing atrybutu i zawiera opis różnych opcji trasami atrybutów. Samouczek end-to-end, który używa atrybutu routingu, zobacz [utworzyć interfejs API REST z atrybutu routingu w sieci Web API 2](create-a-rest-api-with-attribute-routing.md).


## <a name="prerequisites"></a>Wymagania wstępne

[Visual Studio 2017](https://www.visualstudio.com/vs/) Community, Professional, or Enterprise Edition

Można również użyć Menedżera pakietów NuGet w celu zainstalowania wymaganych pakietów. Z **narzędzia** menu w programie Visual Studio, wybierz **Menedżer pakietów biblioteki**, a następnie wybierz pozycję **Konsola Menedżera pakietów**. Wprowadź następujące polecenie w oknie konsoli Menedżera pakietów:

`Install-Package Microsoft.AspNet.WebApi.WebHost`

<a id="why"></a>
## <a name="why-attribute-routing"></a>Dlaczego atrybutu routingu?

Pierwszą wersję interfejsu API sieci Web używany *opartych na konwencjach* routingu. W tym typie routingu należy zdefiniować lub więcej szablonów tras, które są po prostu sparametryzowana ciągów. Gdy w ramach odbiera żądanie, odpowiada identyfikator URI dla szablonu trasy. (Aby uzyskać więcej informacji na temat opartych na konwencjach routingu, zobacz [routingu na platformie ASP.NET Web API](routing-in-aspnet-web-api.md).

Jedną z zalet opartych na konwencjach routingu jest szablonów są definiowane w jednym miejscu, czy spójnego stosowania reguły routingu przez wszystkie kontrolery. Niestety opartych na konwencjach routingu utrudnia obsługę niektórych wzorców identyfikatora URI, które są często używane w interfejsy API RESTful. Na przykład zasoby często zawierają zasoby podrzędne: klienci mają zleceń, filmy ma uczestników, książek ma autorzy i tak dalej. Jest fizyczne do utworzenia identyfikatorów URI odzwierciedlenia tych relacji:

`/customers/1/orders`

Ten typ identyfikatora URI jest trudne do utworzenia przy użyciu opartych na konwencjach routingu. Mimo że mogą to robić, wyniki nie skalować dobrze, jeśli masz wiele kontrolerów lub typów zasobów.

W przypadku routingu atrybutu jest prosta do definiowania trasy dla tego identyfikatora URI. Atrybut jest po prostu Dodaj do akcji kontrolera:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample1.cs)]

Poniżej przedstawiono niektóre wzorców, które atrybutu routingu umożliwia łatwe.

**Przechowywanie wersji interfejsu API**

W tym przykładzie "/ api/v1/produktów" może być kierowany do kontrolera innego niż "/ api/v2/produktów".

`/api/v1/products`  
`/api/v2/products`

**Przeciążone segmentów identyfikatora URI**

W tym przykładzie "1" jest numer zamówienia, ale mapuje "oczekujące" do kolekcji.

`/orders/1`  
`/orders/pending`

**Wiele typów parametru**

W tym przykładzie "1" jest numer zamówienia, ale "2013 06/16" Określa datę.

`/orders/1`  
`/orders/2013/06/16`

<a id="enable"></a>
## <a name="enabling-attribute-routing"></a>Włączanie trasami atrybutów

Aby włączyć routing atrybutu, należy wywołać **MapHttpAttributeRoutes** podczas konfiguracji. Ta metoda rozszerzenia jest zdefiniowany w **System.Web.Http.HttpConfigurationExtensions** klasy.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample2.cs)]

Atrybut routingu można łączyć z [opartych na konwencjach](routing-in-aspnet-web-api.md) routingu. Aby zdefiniować na podstawie Konwencji tras, należy wywołać **MapHttpRoute** metody.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample3.cs)]

Aby uzyskać więcej informacji o konfigurowaniu interfejsu API sieci Web, zobacz [Konfigurowanie ASP.NET Web API 2](../advanced/configuring-aspnet-web-api.md).

<a id="config"></a>
### <a name="note-migrating-from-web-api-1"></a>Uwaga: Migracja z 1 interfejs API sieci Web

Przed 2 interfejsu API sieci Web Szablony projektu interfejsu API sieci Web wygenerowanego kodu następująco:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample4.cs)]

Jeśli atrybut routing jest włączony, ten kod spowoduje zgłoszenie wyjątku. Jeżeli uaktualnisz istniejący projekt interfejsu API sieci Web, aby korzystać z routingu atrybut, upewnij się, że zaktualizuj kod tej konfiguracji do następującego:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample5.cs?highlight=4)]

> [!NOTE]
> Aby uzyskać więcej informacji, zobacz [Konfigurowanie interfejsu API sieci Web z hostingu ASP.NET](../advanced/configuring-aspnet-web-api.md#webhost).


<a id="add-routes"></a>
## <a name="adding-route-attributes"></a>Dodawanie tras atrybutów

Oto przykład trasy zdefiniowane przy użyciu atrybutu:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample6.cs)]

Ciąg &quot;klienci / {customerId} / porządkuje&quot; jest szablon identyfikatora URI dla trasy. Interfejs API sieci Web próbuje pasuje do identyfikatora URI żądania do tego szablonu. W tym przykładzie "klienci" i "zamówienia" są segmenty literału i "{customerId}" jest parametrem zmiennej. Następujące identyfikatory URI spowoduje dopasowanie tego szablonu:

- `http://localhost/customers/1/orders`
- `http://localhost/customers/bob/orders`
- `http://localhost/customers/1234-5678/orders`

Można ograniczyć przy użyciu odpowiedniego [ograniczenia](#constraints), które zostały opisane w dalszej części tego tematu.

Zwróć uwagę, że &quot;{customerId}&quot; parametru w szablonie trasy odpowiada nazwie *customerId* parametru w metodzie. Gdy interfejs API sieci Web wywołuje akcji kontrolera, próbuje wiązania parametrów trasy. Na przykład, jeśli identyfikator URI jest `http://example.com/customers/1/orders`, interfejsu API sieci Web próbuje powiązać wartość "1" *customerId* parametru tej akcji.

Szablon identyfikatora URI może mieć kilka parametrów:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample7.cs)]

Wszystkie metody kontrolera, które nie mają atrybutu trasy korzystać z opartych na konwencjach routingu. W ten sposób można połączyć oba rodzaje routingu w tym samym projekcie.

## <a name="http-methods"></a>Metody HTTP

Interfejs API sieci Web wybiera także działania na podstawie metody HTTP żądania (GET, POST itp.). Domyślnie interfejsu API sieci Web wygląda bez uwzględniania wielkości liter dopasowanie z początku nazwy metody kontrolera. Na przykład metoda kontrolera o nazwie `PutCustomers` dopasowuje żądanie HTTP PUT.

Można zastąpić tę Konwencję dekoracji metoda ze wszystkimi następującymi atrybutami:

- **[HttpDelete]**
- **[HttpGet]**
- **[HttpHead]**
- **[Httpoptions miał]**
- **[HttpPatch]**
- **[HttpPost]**
- **[HttpPut]**

Poniższy przykład mapuje CreateBook metoda żądania HTTP POST.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample8.cs)]

W przypadku wszystkich innych metod HTTP, łącznie z metod niestandardowych, użycie **AcceptVerbs** atrybut, który przyjmuje listę metod HTTP.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample9.cs)]

<a id="prefixes"></a>
## <a name="route-prefixes"></a>Prefiksy trasy

Często trasy w kontrolerze wszystkie rozpoczyna się od tego samego prefiksu. Na przykład:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample10.cs)]

Należy określić Wspólny prefiks dla całego kontrolera przy użyciu **[RoutePrefix]** atrybutu:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample11.cs)]

Użyj tyldy (~) na atrybut method, aby przesłonić prefiks trasy:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample12.cs)]

Prefiks trasy może zawierać parametry:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample13.cs)]

<a id="constraints"></a>
## <a name="route-constraints"></a>Ograniczenia trasy

Ograniczenia trasy pozwalają ograniczyć, jak są dopasowywane parametry w szablonie trasy. Ogólna składnia jest &quot;{ograniczenia parametru}:&quot;. Na przykład:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample14.cs)]

W tym miejscu trasy pierwszy będzie można wybrać tylko jeśli &quot;identyfikator&quot; segmencie identyfikatora URI jest liczbą całkowitą. W przeciwnym razie zostanie wybrany drugi trasy.

W poniższej tabeli wymieniono ograniczenia, które są obsługiwane.

| Ograniczenia | Opis | Przykład |
| --- | --- | --- |
| alpha | Dopasowań wielkie lub małe litery alfabetu łacińskiego (a – z, A-Z) | {x:alpha} |
| bool | Dopasowuje wartość logiczną. | {x:bool} |
| datetime | Dopasowań **DateTime** wartość. | {x: datetime} |
| decimal | Odpowiada wartości dziesiętnej. | {x: decimal} |
| double | Odpowiada wartość 64-bitowych liczb zmiennoprzecinkowych. | {x:double} |
| float | Dopasowuje wartości zmiennoprzecinkowej 32-bitowych. | {x: float} |
| Identyfikator GUID | Dopasowuje wartości identyfikatora GUID. | {x:guid} |
| int | Zgodna z wartością 32-bitową liczbę całkowitą. | {x:int} |
| length | Dopasowuje ciąg znaków o określonej długości lub zakresu określonej długości. | {x: length(6)} {x: length(1,20)} |
| long | Odpowiada wartość 64-bitową liczbę całkowitą. | {x:long} |
| max | Dopasowuje typu integer o wartości maksymalnej. | {x:max(10)} |
| Element MaxLength | Dopasowuje ciąg o maksymalnej długości. | {x:maxlength(10)} |
| min | Dopasowuje jako liczba całkowita, wartość minimalna. | {x:min(10)} |
| minlength | Dopasowuje ciąg o minimalnej długości. | {x:minlength(10)} |
| range | Dopasowuje całkowitą w zakresie wartości. | {x:range(10,50)} |
| regex | Pasuje do wyrażenia regularnego. | {x:regex(^\d{3}-\d{3}-\d{4}$)} |

Powiadomienie niektórych ograniczeń, takich jak &quot;min&quot;, przyjmuje argumentów w nawiasach. Można stosować wiele ograniczeń do parametru, oddzielone dwukropkiem.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample15.cs)]

### <a name="custom-route-constraints"></a>Ograniczenia trasy niestandardowe

Można utworzyć ograniczenia trasy niestandardowe zaimplementowanie **IHttpRouteConstraint** interfejsu. Na przykład następujące ograniczenia ogranicza parametr na wartość niezerową liczbą całkowitą.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample16.cs)]

Poniższy kod przedstawia sposób rejestrowania ograniczenia:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample17.cs)]

Teraz można zastosować ograniczenia trasy:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample18.cs)]

Można również zastąpić całą **DefaultInlineConstraintResolver** klasy zaimplementowanie **IInlineConstraintResolver** interfejsu. W ten sposób spowoduje zastąpienie wszystkich wbudowane ograniczenia, chyba że implementacji **IInlineConstraintResolver** specjalnie dodaje je.

<a id="optional"></a>
## <a name="optional-uri-parameters-and-default-values"></a>Parametry opcjonalne identyfikatora URI i wartości domyślnych

Możesz wprowadzić parametr URI opcjonalne, dodając znak zapytania do parametru trasy. Jeśli parametr trasy jest opcjonalny, należy zdefiniować wartości domyślnej dla parametru metody.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample19.cs)]

W tym przykładzie `/api/books/locale/1033` i `/api/books/locale` zwrócić tego samego zasobu.

Alternatywnie można określić wartość domyślną w szablonie trasy w następujący sposób:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample20.cs)]

To jest prawie takie same, co w poprzednim przykładzie, ale istnieje niewielka różnica zachowania, gdy wartością domyślną jest stosowany.

- W pierwszym przykładzie ("{lcid?}") wartość domyślna 1033 jest przypisywane bezpośrednio do parametru metody, tak aby było to dokładną wartość parametru.
- W drugim przykładzie ("{lcid = 1033}"), wartość domyślna "1033" przechodzi przez proces tworzenia powiązania modelu. Wartość liczbowa 1033 przekonwertuje "1033" domyślnego-integratora modelu. Można jednak dodatku niestandardowego integratora modelu, co może zrobić coś innego.

(W większości przypadków, chyba że masz integratorów modeli niestandardowych w potoku sieci dwa formularze będą równoważne).

<a id="route-names"></a>
## <a name="route-names"></a>Nazwy tras

W składniku Web API każdy ma nazwę. Nazwy trasy są przydatne podczas generowania łączy, tak, aby można uwzględnić łącze w odpowiedzi HTTP.

Aby określić nazwę trasy, ustaw **nazwa** właściwości atrybutu. Poniższy przykład przedstawia sposób ustawiania nazwy trasy i sposobu użycia nazwy trasy podczas generowania łącza.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample21.cs)]

<a id="order"></a>
## <a name="route-order"></a>Kolejność trasy

Próba URI trasa pasuje przez platformę oblicza trasy w określonej kolejności. Aby określić kolejność, ustaw **RouteOrder** właściwość atrybut trasy. Niższe wartości są sprawdzane jako pierwsze. Wartość domyślna kolejności wynosi zero.

Oto, jak całkowita kolejność jest określana:

1. Porównaj **RouteOrder** właściwości atrybutu trasy.
2. Szukaj w każdym segmencie identyfikatora URI w szablonie trasy. Dla każdego segmentu kolejność w następujący sposób: 

    1. Literał segmentów.
    2. Parametry trasy z ograniczeniami.
    3. Parametry trasy bez ograniczeń.
    4. Symboli wieloznacznych parametrem segmentów z ograniczeniami.
    5. Symbol wieloznaczny parametrów segmenty bez ograniczeń.
3. W przypadku zostanie rozwiązany, trasy są uporządkowane według porównania ciągów porządkowych bez uwzględniania wielkości liter ([OrdinalIgnoreCase](https://msdn.microsoft.com/library/system.stringcomparer.ordinalignorecase.aspx)) szablonu trasy.

Oto przykład. Załóżmy, że zdefiniujesz następujący kontroler:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample22.cs)]

Te trasy są sortowane w następujący sposób.

1. Szczegóły/zamówień
2. zamówienia / {id}
3. orders/{customerName}
4. zamówienia / {\*Data}
5. zamówienia / oczekujące

Powiadomienie, że "szczegóły" jest segmentem literału i pojawia się przed "{id}", ale "oczekujące" pojawia się ostatnio ponieważ **RouteOrder** właściwość jest 1. (W tym przykładzie przyjęto założenie, są Brak odbiorców o nazwie "szczegóły" lub "oczekujące". Ogólnie rzecz biorąc należy unikać niejednoznaczne trasy. W tym przykładzie lepsze szablon trasy dla `GetByCustomer` jest "klienci / {customerName}")
