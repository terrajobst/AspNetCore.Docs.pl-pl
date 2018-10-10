---
uid: web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
title: Atrybut routingu we wzorcu ASP.NET Web API 2 | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: riande
ms.date: 01/20/2014
ms.assetid: 979d6c9f-0129-4e5b-ae56-4507b281b86d
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: d16dcc618bf6c60714179601db14f4dd2a9e41ce
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912155"
---
<a name="attribute-routing-in-aspnet-web-api-2"></a>Atrybut routingu we wzorcu ASP.NET Web API 2
====================
przez [Mike Wasson](https://github.com/MikeWasson)

*Routing* jest jak internetowy interfejs API dopasowuje się do identyfikatora URI do akcji. Składnik Web API 2 obsługuje nowy typ routingu, o nazwie *trasowanie atrybutów*. Jak wskazuje nazwa, routing atrybutu używa atrybutów do definiowania trasy. Trasowanie atrybutów zapewnia większą kontrolę nad identyfikatory URI w interfejsie API sieci web. Na przykład można łatwo utworzyć identyfikatory URI, które opisują hierarchie zasobów.

Wcześniej styl routingu o nazwie oparty na Konwencji routingu jest nadal w pełni obsługiwane. W rzeczywistości można łączyć z obu tych technik, w tym samym projekcie.

W tym temacie przedstawiono sposób włączania trasowanie atrybutów, a w tym artykule opisano różne opcje trasowanie atrybutów. Aby uzyskać samouczek end-to-end, który używa trasowanie atrybutów, zobacz [Tworzenie interfejsu API REST przy użyciu atrybutu routingu w sieci Web API 2](create-a-rest-api-with-attribute-routing.md).

## <a name="prerequisites"></a>Wymagania wstępne

[Program Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional lub Enterprise edition

Można również użyć Menedżera pakietów NuGet, aby zainstalować wymagane pakiety. Z **narzędzia** menu w programie Visual Studio, wybierz **Menedżera pakietów NuGet**, a następnie wybierz **Konsola Menedżera pakietów**. Wprowadź następujące polecenie w oknie Konsola Menedżera pakietów:

`Install-Package Microsoft.AspNet.WebApi.WebHost`

<a id="why"></a>
## <a name="why-attribute-routing"></a>Dlaczego atrybutu routingu?

Pierwsza wersja interfejsu API sieci Web używane *oparty na Konwencji* routingu. W tym typie routingu należy zdefiniować jedną lub więcej szablonów tras, które są po prostu sparametryzowane ciągów. Struktura odbiera żądanie, jest zgodna identyfikator URI dla szablonu trasy. (Aby uzyskać więcej informacji na temat routingu opartego na Konwencji zobacz [routingu ASP.NET Web API](routing-in-aspnet-web-api.md).

Jedną z zalet routing oparty na Konwencji jest szablonów są definiowane w jednym miejscu i reguły routingu są stosowane spójnie we wszystkich kontrolerów. Niestety routing oparty na Konwencji utrudnia do obsługi niektórych wzorców identyfikatora URI, które są wspólne w interfejsów API RESTful. Na przykład zasoby często zawierają zasoby podrzędne: klienci mają zamówienia, filmy mają aktorów, książki mają autorzy i tak dalej. Jest naturalnym do tworzenia identyfikatorów URI, które odzwierciedlają te relacje:

`/customers/1/orders`

Tego rodzaju identyfikatora URI jest trudne do utworzenia przy użyciu routingu opartego na Konwencji. Mimo że można to zrobić, wyniki nie skalują się dobrze, jeśli masz wiele kontrolerów lub typów zasobów.

Za pomocą atrybutu routingu jest prosta, aby zdefiniować trasę dla tego identyfikatora URI. Możesz po prostu dodaj atrybut do akcji kontrolera:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample1.cs)]

Poniżej przedstawiono niektóre wzorce, które atrybutu routingu powoduje, że łatwo.

**Przechowywanie wersji interfejsu API**

W tym przykładzie "/ api/v1/produktów" byłaby przekierowane do innego kontrolera niż "/ api/2/produktów".

`/api/v1/products`
`/api/v2/products`

**Przeciążona segmentów identyfikatora URI**

W tym przykładzie "1" jest numer zamówienia, ale "pending" mapuje do kolekcji.

`/orders/1`
`/orders/pending`

**Wieloma typami parametrów**

W tym przykładzie "1" jest numer zamówienia, ale "06/2013/16" Określa wartość typu date.

`/orders/1`
`/orders/2013/06/16`

<a id="enable"></a>
## <a name="enabling-attribute-routing"></a>Włączanie trasowanie atrybutów

Aby włączyć routing atrybutów, należy wywołać **MapHttpAttributeRoutes** podczas konfiguracji. Ta metoda rozszerzenia jest zdefiniowany w **System.Web.Http.HttpConfigurationExtensions** klasy.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample2.cs)]

Routing atrybut może być łączone z [oparty na Konwencji](routing-in-aspnet-web-api.md) routingu. Aby zdefiniować oparty na Konwencji tras, należy wywołać **MapHttpRoute** metody.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample3.cs)]

Aby uzyskać więcej informacji na temat konfigurowania interfejsu API sieci Web, zobacz [Konfigurowanie wzorca ASP.NET Web API 2](../advanced/configuring-aspnet-web-api.md).

<a id="config"></a>
### <a name="note-migrating-from-web-api-1"></a>Uwaga: Migrowanie z interfejsu Web API 1

Przed Web API 2 szablony projektu interfejsu API sieci Web wygenerowany kod w następujący sposób:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample4.cs)]

Jeśli atrybut routing jest włączony, ten kod spowoduje zgłoszenie wyjątku. Jeśli uaktualnisz istniejący projekt interfejsu API sieci Web do użycia trasowanie atrybutów, upewnij się zaktualizować ten kod konfiguracji do następujących:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample5.cs?highlight=4)]

> [!NOTE]
> Aby uzyskać więcej informacji, zobacz [Konfigurowanie internetowego interfejsu API z hostingu platformy ASP.NET](../advanced/configuring-aspnet-web-api.md#webhost).


<a id="add-routes"></a>
## <a name="adding-route-attributes"></a>Dodawanie atrybutów trasy

Oto przykład trasy definiowane przy użyciu atrybutu:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample6.cs)]

Ciąg &quot;klientów / {customerId} / orders&quot; jest szablon identyfikatora URI dla danej trasy. Interfejs API sieci Web próbuje dopasować identyfikatora URI żądania do szablonu. W tym przykładzie "klientów" i "orders" to segmentów literału, a "{customerId}" jest parametrem zmiennej. Następujące identyfikatory URI będzie odpowiadać tego szablonu:

- `http://localhost/customers/1/orders`
- `http://localhost/customers/bob/orders`
- `http://localhost/customers/1234-5678/orders`

Można ograniczyć za pomocą dopasowywania [ograniczenia](#constraints), które zostały opisane w dalszej części tego tematu.

Należy zauważyć, że &quot;{customerId}&quot; parametrów w szablonie trasy jest zgodna z nazwą *customerId* parametru w metodzie. Gdy internetowy interfejs API wywołuje akcji kontrolera, próbuje powiązania parametrów trasy. Na przykład, jeśli identyfikator URI jest `http://example.com/customers/1/orders`, interfejs API sieci Web próbuje można powiązać wartości "1" *customerId* parametru w akcji.

Szablon identyfikatora URI może mieć kilka parametrów:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample7.cs)]

Wszystkimi metodami kontrolera, które nie mają atrybutu trasy Użyj, routing oparty na Konwencji. W ten sposób można połączyć oba rodzaje routingu w tym samym projekcie.

## <a name="http-methods"></a>Metody HTTP

Interfejs API sieci Web również wybiera akcje na podstawie metody HTTP żądania (GET, POST itp.). Domyślnie internetowy interfejs API wygląda dopasowanie bez uwzględniania wielkości liter z początku nazwy metody kontrolera. Na przykład metody kontroler o nazwie `PutCustomers` dopasowuje żądanie HTTP PUT.

Możesz zastąpić tę Konwencję przez urządzanie metody z dowolnym następujące atrybuty:

- **[HttpDelete]**
- **[HttpGet]**
- **[HttpHead]**
- **[Httpoptions miał]**
- **[HttpPatch]**
- **[HttpPost]**
- **[HttpPut]**

Poniższy przykład mapuje metoda CreateBook żądania HTTP POST.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample8.cs)]

Wszystkie inne metody HTTP, łącznie z metod niestandardowych do używania **AcceptVerbs** atrybut, który przyjmuje listę metod HTTP.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample9.cs)]

<a id="prefixes"></a>
## <a name="route-prefixes"></a>Prefiksy trasy

Często trasy w kontrolerze wszystkie identyfikatory rozpoczynają się przy użyciu tego samego prefiksu. Na przykład:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample10.cs)]

Możesz ustawić Wspólny prefiks dla całego kontrolera, używając **[RoutePrefix]** atrybutu:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample11.cs)]

Użyj tyldy (~) dla atrybutu metody, aby przesłonić prefiks trasy:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample12.cs)]

Prefiks trasy może zawierać parametry:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample13.cs)]

<a id="constraints"></a>
## <a name="route-constraints"></a>Ograniczenia trasy

Ograniczenia trasy pozwalają na określenie, jak są dopasowywane parametry w szablonie trasy. Ogólna składnia jest &quot;{parametr: ograniczenie}&quot;. Na przykład:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample14.cs)]

W tym miejscu pierwsza trasa będzie można wybrać tylko jeśli &quot;identyfikator&quot; segmencie identyfikatora URI jest liczbą całkowitą. W przeciwnym razie zostanie wybrany druga trasa.

W poniższej tabeli wymieniono ograniczenia, które są obsługiwane.

| Ograniczenia | Opis | Przykład |
| --- | --- | --- |
| alfa | Dopasowuje wielkie lub małe litery alfabetu łacińskiego (a – z, A – Z) | {x: alfa} |
| bool | Dopasowuje wartość logiczną. | {x:bool} |
| datetime | Dopasowuje **daty/godziny** wartość. | {x: Data i godzina} |
| decimal | Dopasowuje wartość dziesiętną. | {x: dziesiętne} |
| double | Dopasowuje wartość zmiennoprzecinkową 64-bitowych. | {x:double} |
| float | Dopasowuje wartość zmiennoprzecinkowa 32-bitowych. | {x: float} |
| Identyfikator GUID | Dopasowuje wartość identyfikatora GUID. | {x:guid} |
| int | Odpowiada wartości 32-bitową liczbę całkowitą. | {x:int} |
| length | Dopasowuje ciąg znaków o określonej długości lub w określonym zakresie długości. | {x: length(6)} {x: length(1,20)} |
| long | Dopasowuje wartość 64-bitową liczbę całkowitą. | {x:long} |
| max | Reprezentuje liczbą całkowitą, przy czym wartość maksymalna. | {x:max(10)} |
| Element MaxLength | Dopasowuje ciąg o maksymalnej długości. | {x:maxlength(10)} |
| min | Reprezentuje liczbą całkowitą o określonej wartości minimalnej. | {x:min(10)} |
| Element MinLength | Dopasowuje ciąg o długości minimalnej. | {x:minlength(10)} |
| range | Pasuje do liczby całkowitej mającej zakresu wartości. | {x: range(10,50)} |
| wyrażenie regularne | Pasuje do wyrażenia regularnego. | {x: regex(^\d{3}-\d{3}-\d{4}$)} |

Zwróć uwagę niektórych ograniczeń, takich jak &quot;min&quot;, przyjmują argumentów w nawiasach. Można zastosować wiele ograniczeń za pomocą parametru oddzieloną dwukropkiem.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample15.cs)]

### <a name="custom-route-constraints"></a>Ograniczenia trasy niestandardowe

Można utworzyć ograniczenia trasy niestandardowe, implementując **IHttpRouteConstraint** interfejsu. Na przykład następujące ograniczenia ogranicza parametr na wartość całkowitą od zera.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample16.cs)]

Poniższy kod przedstawia sposób rejestrowania ograniczenia:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample17.cs)]

Teraz można zastosować ograniczenia trasy:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample18.cs)]

Możesz również zastąpić całą **DefaultInlineConstraintResolver** klasy przez zaimplementowanie **IInlineConstraintResolver** interfejsu. To spowoduje zastąpienie wszystkie wbudowane ograniczenia, chyba że implementacja **IInlineConstraintResolver** specjalnie dodaje je.

<a id="optional"></a>
## <a name="optional-uri-parameters-and-default-values"></a>Parametry opcjonalne identyfikatora URI i wartości domyślne

Istnieje możliwość parametru identyfikatora URI opcjonalne, dodając znak zapytania do parametru trasy. Jeśli parametr trasy jest opcjonalny, zdefiniuj wartość domyślną dla parametru metody.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample19.cs)]

W tym przykładzie `/api/books/locale/1033` i `/api/books/locale` zwracać ten sam zasób.

Alternatywnie można określić wartość domyślną wewnątrz szablonu trasy w następujący sposób:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample20.cs)]

Jest prawie taki sam, jak w poprzednim przykładzie, ale istnieje niewielka różnica zachowanie po zastosowaniu wartość domyślną.

- W pierwszym przykładzie ("{lcid?}") wartość domyślna 1033, to przypisać bezpośrednio do parametru metody, tak aby było to dokładna wartość parametru.
- W drugim przykładzie ("{lcid = 1033}"), wartość domyślna "1033" przechodzi przez proces wiązania modelu. Integrator modelu domyślne przekonwertuje "1033" na wartość liczbową 1033. Można jednak dodatku niestandardowego integratora modelu, który może zrobić coś inaczej.

(W większości przypadków, chyba że masz niestandardowe integratorów modeli w potoku, dwa formularze będą równoważne).

<a id="route-names"></a>
## <a name="route-names"></a>Nazwy tras

W interfejsie API sieci Web co trasy o nazwie. Nazwy tras przydają się podczas generowania łączy, tak że można uwzględnić łącze w odpowiedzi HTTP.

Aby określić nazwę trasy, ustaw **nazwa** właściwości w atrybucie. Poniższy przykład pokazuje, jak ustawić nazwę trasy, a także jak używać nazwy trasy podczas generowania łącza.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample21.cs)]

<a id="order"></a>
## <a name="route-order"></a>Kolejność trasy

Kiedy struktura próbuje dopasować identyfikatora URI z trasą, ocenia trasy w określonej kolejności. Aby określić kolejność, ustaw **RouteOrder** właściwość atrybut trasy. Niższe wartości są obliczane jako pierwsze. Wartość domyślna kolejności wynosi zero.

Poniżej przedstawiono sposób ustalania kolejności, łączna liczba:

1. Porównaj **RouteOrder** właściwość atrybut trasy.
2. Spójrz na każdym segmencie identyfikatora URI w szablonie trasy. Dla każdego segmentu kolejność w następujący sposób:

    1. Literał segmenty.
    2. Parametry trasy z ograniczeniami.
    3. Parametry trasy bez ograniczeń.
    4. Symbol wieloznaczny parametr segmentów z ograniczeniami.
    5. Symbol wieloznaczny segmenty parametru bez ograniczeń.
3. W przypadku tie trasy są uporządkowane według porównania bez uwzględniania wielkości liter ciągu porządkowe ([OrdinalIgnoreCase](https://msdn.microsoft.com/library/system.stringcomparer.ordinalignorecase.aspx)) szablonu trasy.

Oto przykład. Załóżmy, że należy zdefiniować następujące kontrolera:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample22.cs)]

Te trasy są uporządkowane w następujący sposób.

1. Szczegóły/zamówień
2. zamówienia / {id}
3. zamówienia / {customerName}
4. zamówienia / {\*date}
5. zamówienia / oczekujące

Zwróć uwagę, że "szczegóły" jest literał segmentu i pojawia się przed "{id}", ale "oczekujące" pojawia się ostatnio ponieważ **RouteOrder** właściwość ma wartość 1. (W tym przykładzie przyjęto założenie, są nie klientów o nazwie "szczegóły" lub "pending". Ogólnie rzecz biorąc Staraj się unikać niejednoznaczne trasy. W tym przykładzie lepsze szablon trasy dla `GetByCustomer` jest "klientów / {customerName}")
