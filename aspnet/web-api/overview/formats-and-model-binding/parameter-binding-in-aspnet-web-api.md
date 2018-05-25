---
uid: web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
title: Parametr wiązania w składniku ASP.NET Web API | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/11/2013
ms.topic: article
ms.assetid: e42c8388-04ed-4341-9fdb-41b1b4c06320
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 5aa532137436922519c86246ebfa834910ac0d86
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="parameter-binding-in-aspnet-web-api"></a>Parametr wiązania w składniku ASP.NET Web API
====================
przez [Wasson Jan](https://github.com/MikeWasson)

Gdy interfejs API sieci Web wywołuje metodę dla kontrolera, należy ustawić wartości parametrów w procesie nazywanym *powiązania*. W tym artykule opisano sposób interfejsu API sieci Web wiąże parametry, i jak można dostosować proces wiązania.

Domyślnie interfejsu API sieci Web używa następujące reguły można powiązać parametry:

- Jeśli parametr jest typu "prosty", interfejsu API sieci Web próbuje pobrać wartość z identyfikatora URI. Proste typy .NET [typów pierwotnych](https://msdn.microsoft.com/library/system.type.isprimitive.aspx) (**int**, **bool**, **podwójne**itd), oraz **TimeSpan**, **DateTime**, **Guid**, **dziesiętną**, i **ciąg**, *plus* żadnych Typ konwertera typów, który można przekonwertować ciągu. (Więcej informacji na temat typów konwerterów później.)
- Dla typów złożonych, interfejs API sieci Web próbuje odczytać wartości z treści wiadomości, przy użyciu [program formatujący typ nośnika](media-formatters.md).

Na przykład poniżej przedstawiono typowe metody kontrolera interfejsu API sieci Web:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample1.cs)]

*Identyfikator* parametr jest &quot;proste&quot; typ, więc próbuje pobrać wartość z identyfikatora URI żądania interfejsu API sieci Web. *Elementu* parametr jest typu złożonego, więc można odczytać wartości z treści żądania interfejsu API sieci Web korzysta z elementu formatującego typu nośnika.

Aby uzyskać wartość z identyfikatora URI, interfejsu API sieci Web jest wyszukiwany w danych trasy i ciągu zapytania identyfikatora URI. Dane trasy jest wypełniana podczas routingu systemu analizuje identyfikator URI i dopasowuje je do trasy. Aby uzyskać więcej informacji, zobacz [routingu i wybór akcji](../web-api-routing-and-actions/routing-and-action-selection.md).

W dalszej części tego artykułu I opisano, jak można dostosować procesie powiązania modelu. Dla typów złożonych jednak należy rozważyć użycie programy formatujące typy nośnika, jeśli to możliwe. Klucza zasady HTTP jest, że zasoby są wysyłane w treści wiadomości, za pomocą negocjowania zawartości, aby określić reprezentacja zasobu. Programy formatujące typy nośnika zostały zaprojektowane do dokładnie tego celu.

## <a name="using-fromuri"></a>Przy użyciu [FromUri]

Aby wymusić interfejsu API sieci Web do odczytu typu złożonego z identyfikatora URI, Dodaj **[FromUri]** atrybutu parametru. W poniższym przykładzie zdefiniowano `GeoPoint` typu wraz z metody kontrolera, który pobiera `GeoPoint` z identyfikatora URI.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample2.cs)]

Klienta można ustawić wartości szerokości i długości geograficznej w ciągu zapytania i interfejsu API sieci Web będą z nich korzystać do utworzenia `GeoPoint`. Na przykład:

`http://localhost/api/values/?Latitude=47.678558&Longitude=-122.130989`

## <a name="using-frombody"></a>Przy użyciu [FromBody]

Aby wymusić interfejsu API sieci Web do odczytu treści żądania typu prostego, Dodaj **[FromBody]** atrybutu parametru:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample3.cs)]

W tym przykładzie interfejsu API sieci Web użyje elementu formatującego typu nośnika do odczytania wartości *nazwa* z treści żądania. Oto przykład żądania klienta.

[!code-console[Main](parameter-binding-in-aspnet-web-api/samples/sample4.cmd)]

Jeśli parametr ma [FromBody], interfejsu API sieci Web używa nagłówka Content-Type, aby wybrać element formatujący. W tym przykładzie typ zawartości jest &quot;application/json&quot; i treść żądania jest nieprzetworzonego ciągu JSON (nie obiekt JSON).

Co najwyżej jeden parametr może odczytywać treść komunikatu. Dlatego to nie będzie działać:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample5.cs)]

Przyczyną dla tej reguły jest, że treść żądania mogą być przechowywane w niebuforowanym strumienia, który może zostać odczytany tylko raz.

## <a name="type-converters"></a>Konwertery typu

Możesz wprowadzić traktowanie klasy jako typu prostego (dzięki czemu interfejsu API sieci Web próbuje powiązać go z identyfikatora URI) interfejsu API sieci Web, tworząc **TypeConverter** i Konwersja ciągu.

Poniższy kod przedstawia `GeoPoint` Klasa reprezentująca punkt geograficznych oraz **TypeConverter** konwertująca z ciągów do `GeoPoint` wystąpień. `GeoPoint` Zostanie nadany klasy **[TypeConverter]** Podaj konwerter typów dla atrybutu. (W tym przykładzie został inspirowana przez wstrzymania Jan wpis w blogu [temat wiązania niestandardowe obiekty w podpisach akcji w MVC/WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx).)

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample6.cs)]

Teraz interfejsu API sieci Web będzie traktować `GeoPoint` jako typu prostego, czyli spróbuje powiązać `GeoPoint` parametry z identyfikatora URI. Nie trzeba uwzględnić **[FromUri]** w parametrze.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample7.cs)]

Klienta można wywołać metody o identyfikatorze URI w następujący sposób:

`http://localhost/api/values/?location=47.678558,-122.130989`

## <a name="model-binders"></a>Integratorów modeli

Opcja bardziej elastyczne niż konwertera typów jest utworzenie niestandardowego integratora modelu. Z integratora modelu Ty masz dostęp do elementów, jak żądania HTTP, opis akcji i wartości w wierszach z danych trasy.

Aby utworzyć obiekt wiążący modelu, należy zaimplementować **interfejs IModelBinder** interfejsu. Ten interfejs definiuje jedną metodę **BindModel**:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample8.cs)]

Oto integratora modelu dla `GeoPoint` obiektów.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample9.cs)]

Integrator modelu pobiera nieprzetworzone wartości wejściowych z *dostawcy wartości*. W tym projekcie oddziela dwa różne funkcje:

- Dostawca wartości przyjmuje żądania HTTP i wypełnia słownik par klucz wartość.
- Integrator modelu użyje tego słownika, aby wypełnić modelu.

Domyślny dostawca wartości w interfejsie API sieci Web pobiera wartości z danych trasy i ciągu zapytania. Na przykład, jeśli identyfikator URI jest `http://localhost/api/values/1?location=48,-122`, dostawca wartości tworzy następujące pary klucz wartość:

- id = &quot;1&quot;
- Lokalizacja = &quot;48,122&quot;

(I używam zakładając, że szablon trasy domyślne, czyli &quot;interfejsu api / {controller} / {id}&quot;.)

Nazwa parametru do powiązania jest przechowywana w **ModelBindingContext.ModelName** właściwości. Wyszukuje integratora modelu dla klucza z tej wartości w słowniku. Jeśli wartość istnieje i mogą być konwertowane na `GeoPoint`, integratora modelu przypisuje wartości powiązanej do **ModelBindingContext.Model** właściwości.

Zwróć uwagę, że integratora modelu nie jest ograniczona do konwersji typu prostego. W tym przykładzie integratora modelu najpierw wyszukiwana w tabeli znanej lokalizacji, a w przypadku niepowodzenia używa konwersji typu.

**Ustawienie integratora modelu**

Istnieje kilka sposobów, aby ustawić integratora modelu. Po pierwsze, można dodać **[ModelBinder]** atrybutu parametru.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample10.cs)]

Można również dodać **[ModelBinder]** atrybutu typu. Interfejs API sieci Web użyje integratora modelu określonego dla wszystkich parametrów typu.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample11.cs)]

Na koniec można dodać dostawcę integratora modelu do **HttpConfiguration**. Dostawca integratora modelu to po prostu klasę fabryki, która tworzy integratora modelu. Można utworzyć dostawcę pochodny [ModelBinderProvider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx) klasy. Jednak jeśli Twoje integratora modelu obsługuje jednego typu, łatwiej jest użyć wbudowanych **SimpleModelBinderProvider**, które jest przeznaczone do tego celu. Poniższy kod przedstawia, jak to zrobić.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample12.cs)]

Z dostawcy wiązania modelu, nadal konieczne jest dodanie **[ModelBinder]** atrybutu do parametru powiedzieć interfejsu API sieci Web, aby go używać integratora modelu, a nie element formatujący typu nośnika. Jednak obecnie nie trzeba określić typ integratora modelu w atrybucie:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample13.cs)]

## <a name="value-providers"></a>Dostawców wartości

Wymienione I że integratora modelu pobiera wartości z dostawcy wartości. Aby napisać dostawcę wartości niestandardowych, zaimplementuj **IValueProvider** interfejsu. Oto przykład pobierający wartości z plików cookie w żądaniu:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample14.cs)]

Należy także utworzyć fabryki dostawców wartości przez wynikających z **ValueProviderFactory** klasy.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample15.cs)]

Fabryki dostawców wartości, aby dodać **HttpConfiguration** w następujący sposób.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample16.cs)]

Interfejs API sieci Web Redaguj wszystkich dostawców wartości, więc jeśli wywołuje integratora modelu **ValueProvider.GetValue**, integratora modelu otrzymuje wartość z pierwszego dostawcy wartości, który może go wygenerować.

Alternatywnie można ustawić fabryki dostawców wartości na poziomie parametr przy użyciu **ValueProvider** atrybutu w następujący sposób:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample17.cs)]

Ta wartość informuje interfejsu API sieci Web do używania wiązania modelu z fabryki dostawców wartości określonej i nie można użyć dowolnego dostawców zarejestrowanych wartości.

## <a name="httpparameterbinding"></a>HttpParameterBinding

Integratorów modeli są określone wystąpienie mechanizmu bardziej ogólne. Jeśli przyjrzymy się **[ModelBinder]** atrybutu, zobaczysz, że pochodzi od klasy abstrakcyjnej **ParameterBindingAttribute** klasy. Ta klasa definiuje jedną metodę **GetBinding**, która zwraca **HttpParameterBinding** obiektu:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample18.cs)]

**HttpParameterBinding** jest odpowiedzialny za wiązanie parametru na wartość. W przypadku programu **[ModelBinder]**, zwraca ten atrybut **HttpParameterBinding** implementację, która używa **interfejs IModelBinder** przeprowadzić rzeczywistego powiązania. Można też wdrożyć własne **HttpParameterBinding**.

Na przykład, załóżmy, że chcesz pobrać elementy etag z `if-match` i `if-none-match` nagłówków w żądaniu. Zaczniemy, definiując klasę do reprezentowania elementy ETag.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample19.cs)]

Zdefiniujemy również wyliczenia wskazująca, czy można uzyskać ETag z `if-match` nagłówka lub `if-none-match` nagłówka.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample20.cs)]

Oto **HttpParameterBinding** czy pobiera tagu ETag z żądaną nagłówka i wiąże go z parametrem typu ETag:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample21.cs)]

**ExecuteBindingAsync** metoda wykonuje wiązanie. W ramach tej metody, Dodaj wartość parametru powiązania **ActionArgument** słownika **element HttpActionContext**.

> [!NOTE]
> Jeśli Twoje **ExecuteBindingAsync** metoda odczytuje treść komunikatu żądania, Zastąp **WillReadBody** właściwości, aby zwracała wartość true. Treść żądania może być Niebuforowane strumienia, które mogą być odczytywane tylko raz, więc interfejsu API sieci Web wymusza reguły tego co najwyżej jeden powiązanie można odczytać treści wiadomości.


Aby zastosować niestandardowy **HttpParameterBinding**, można zdefiniować atrybut, który jest pochodną **ParameterBindingAttribute**. Aby uzyskać `ETagParameterBinding`, zdefiniujemy dwa atrybuty, jeden dla `if-match` nagłówki i jeden dla `if-none-match` nagłówków. Abstrakcyjna klasa podstawowa zarówno pochodną.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample22.cs)]

Oto metoda kontrolera, który używa `[IfNoneMatch]` atrybutu.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample23.cs)]

Oprócz **ParameterBindingAttribute**, istnieje inny haku dodawania niestandardowego **HttpParameterBinding**. Na **HttpConfiguration** obiektu **ParameterBindingRules** właściwości to zbiór funkcji anomymous typu (**HttpParameterDescriptor**  - &gt; **HttpParameterBinding**). Na przykład można dodać regułę, która korzysta z żadnych parametrów ETag dla metody GET `ETagParameterBinding` z `if-none-match`:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample24.cs)]

Funkcja powinna zwrócić `null` dla parametrów, których powiązanie nie ma zastosowania.

## <a name="iactionvaluebinder"></a>IActionValueBinder

Cały proces wiązanie parametru jest kontrolowany przez usługę podłączania, **IActionValueBinder**. Domyślna implementacja **IActionValueBinder** wykonuje następujące czynności:

1. Wyszukaj **ParameterBindingAttribute** w parametrze. Obejmuje to **[FromBody]**, **[FromUri]**, i **[ModelBinder]**, lub atrybutów niestandardowych.
2. W przeciwnym razie Szukaj w **HttpConfiguration.ParameterBindingRules** dla funkcji, która zwraca wartość inną niż null **HttpParameterBinding**.
3. W przeciwnym razie Użyj domyślnych reguł, które I opisane wcześniej. 

    - Jeśli typ parametru jest "prosta" lub ma konwertera typów, należy powiązać z identyfikatora URI. Jest to równoważne umieszczanie **[FromUri]** atrybutu w parametrze.
    - W przeciwnym razie spróbuj odczytywać parametr treści wiadomości. Jest to równoważne umieszczanie **[FromBody]** w parametrze.

Jeśli potrzebujesz, można zastąpić całą **IActionValueBinder** usługi z implementacją niestandardowych.

## <a name="additional-resources"></a>Dodatkowe zasoby

[Przykładowe powiązanie parametru niestandardowego](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/CustomParameterBinding/ReadMe.txt)

Jan wstrzymania zapisano szereg dobrej wpisy na blogu dotyczące wiązanie parametru interfejsu API sieci Web:

- [Jak wiązanie parametru przez interfejs API sieci Web](https://blogs.msdn.com/b/jmstall/archive/2012/04/16/how-webapi-does-parameter-binding.aspx)
- [Styl MVC wiązanie parametru dla interfejsu API sieci Web](https://blogs.msdn.com/b/jmstall/archive/2012/04/18/mvc-style-parameter-binding-for-webapi.aspx)
- [Jak chcesz powiązać niestandardowych obiektów w podpisach akcji w interfejsie API MVC/sieci Web](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx)
- [Jak utworzyć dostawcę wartości niestandardowe w interfejsie API sieci Web](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx)
- [Wiązanie parametru interfejsu API sieci Web kulisy](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx)
