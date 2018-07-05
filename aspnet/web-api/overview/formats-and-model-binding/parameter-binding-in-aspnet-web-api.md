---
uid: web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
title: Wiązanie parametrów interfejsie Web API platformy ASP.NET | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/11/2013
ms.topic: article
ms.assetid: e42c8388-04ed-4341-9fdb-41b1b4c06320
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 7519ad92334690817ae64b994762fd68e76ebc9e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377812"
---
<a name="parameter-binding-in-aspnet-web-api"></a>Wiązanie parametrów interfejsie Web API platformy ASP.NET
====================
przez [Mike Wasson](https://github.com/MikeWasson)

Gdy interfejs API sieci Web wywołuje metodę dla kontrolera, należy ustawić w niej wartości parametrów w procesie zwanym *powiązania*. W tym artykule opisano, jak interfejs API sieci Web tworzy powiązania parametrów i w jaki sposób dostosować proces wiązania.

Domyślnie by powiązać parametry interfejsu API sieci Web stosowane są następujące reguły:

- Jeśli parametr jest typu "prosta", interfejs API sieci Web próbuje pobrać wartość z identyfikatora URI. Proste typy obejmują .NET [typów pierwotnych](https://msdn.microsoft.com/library/system.type.isprimitive.aspx) (**int**, **bool**, **double**, i tak dalej), oraz **TimeSpan**, **Daty/godziny**, **Guid**, **dziesiętna**, i **ciąg**, *oraz* wszystkie Typ konwertera typów, który można przekonwertować ciągu. (Więcej informacji na temat konwerterów typów później.)
- Dla typów złożonych, interfejs API sieci Web próbuje odczytać wartości z treści wiadomości, za pomocą [program formatujący typ nośnika](media-formatters.md).

Na przykład poniżej przedstawiono typowe metody kontrolera interfejsu API sieci Web:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample1.cs)]

*Identyfikator* parametr jest &quot;proste&quot; typ, dzięki czemu próbuje pobrać wartość z identyfikatora URI żądania interfejsu API sieci Web. *Elementu* parametr jest typem złożonym, dzięki czemu można odczytać wartości z treści żądania interfejsu API sieci Web korzysta z elementu formatującego typu nośnika.

Aby uzyskać wartość z identyfikatora URI, internetowy interfejs API wygląda w danych trasy i ciągu zapytania identyfikatora URI. Dane trasy jest wypełniana w przypadku routingu system analizuje identyfikator URI i dopasowuje je do trasy. Aby uzyskać więcej informacji, zobacz [Routing i wybieranie akcji](../web-api-routing-and-actions/routing-and-action-selection.md).

W dalszej części tego artykułu I pokazano, w jaki sposób dostosować procesie powiązania modelu. W przypadku typów złożonych jednak należy rozważyć użycie programy formatujące typy nośnika, jeśli to możliwe. Klucza zasada HTTP jest, że zasoby są wysyłane w treści wiadomości, za pomocą negocjowania zawartości w celu określania reprezentację zasobu. Programy formatujące typy nośnika zostały zaprojektowane do dokładnie tego celu.

## <a name="using-fromuri"></a>Przy użyciu [FromUri]

Aby wymusić interfejsu API sieci Web można odczytać to typ złożony z identyfikatora URI, należy dodać **[FromUri]** atrybutu do parametru. W poniższym przykładzie zdefiniowano `GeoPoint` typu wraz z metody kontrolera, który pobiera `GeoPoint` z identyfikatora URI.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample2.cs)]

Klienta można umieścić wartości długości i szerokości geograficznej w ciągu zapytania i interfejsu API sieci Web będzie konstruowania za ich pomocą `GeoPoint`. Na przykład:

`http://localhost/api/values/?Latitude=47.678558&Longitude=-122.130989`

## <a name="using-frombody"></a>Przy użyciu [FromBody]

Aby wymusić odczytu typu prostego z treści żądania interfejsu API sieci Web, należy dodać **[FromBody]** atrybutu do parametru:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample3.cs)]

W tym przykładzie interfejs API sieci Web użyje elementu formatującego typu nośnika można odczytać wartości z *nazwa* z treści żądania. Oto przykładowe żądanie klienta.

[!code-console[Main](parameter-binding-in-aspnet-web-api/samples/sample4.cmd)]

Jeśli parametr ma [FromBody], interfejs API sieci Web używa nagłówka Content-Type, aby wybrać element formatujący. W tym przykładzie typem zawartości jest &quot;application/json&quot; i treść żądania jest nieprzetworzonego ciągu JSON (a nie obiekt JSON).

Co najwyżej jeden parametr może odczytywać treść komunikatu. Dlatego ta nie będzie działać:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample5.cs)]

Przyczyna, dla tej reguły jest, że treść żądania mogą być przechowywane w niebuforowanym strumienia, który może zostać odczytany tylko raz.

## <a name="type-converters"></a>Konwertery typu

Ułatwia traktować klasę jako typu prostego (dzięki czemu interfejs API sieci Web spróbuje powiązać go z identyfikatora URI) interfejsu API sieci Web, tworząc **TypeConverter** i zapewniając konwersji ciągu.

Poniższy kod przedstawia `GeoPoint` klasa, która reprezentuje punkt geograficzne oraz **TypeConverter** konwersji z ciągów do `GeoPoint` wystąpień. `GeoPoint` Klasy zostanie nadany **[TypeConverter]** atrybutu, aby określić konwertera typów. (W tym przykładzie został ZAINSPIROWANE wpis w blogu wstrzymania Mike [sposobu tworzenia powiązania do obiektów niestandardowych w podpisach akcji w MVC/WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx).)

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample6.cs)]

Teraz interfejs API sieci Web będą traktować `GeoPoint` jako typ prosty, czyli podejmie próbę powiązania `GeoPoint` parametry z identyfikatora URI. Nie musisz uwzględniać **[FromUri]** w parametrze.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample7.cs)]

Klient może wywołać metody za pomocą identyfikatora URI w następujący sposób:

`http://localhost/api/values/?location=47.678558,-122.130989`

## <a name="model-binders"></a>Integratorów modelu

Opcja bardziej elastyczne niż konwertera typów polega na utworzeniu niestandardowego integratora modelu. Za pomocą integratora modelu masz dostęp do elementów, takich jak żądania HTTP, opis akcji i wartości w wierszach z danych trasy.

Aby utworzyć obiekt wiążący modelu, należy zaimplementować **interfejs IModelBinder** interfejsu. Ten interfejs definiuje jedną metodę **BindModel**:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample8.cs)]

Oto integratora modelu dla `GeoPoint` obiektów.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample9.cs)]

Integrator modelu pobiera nieprzetworzone wartości wejściowe z *dostawcy wartości*. W tym projekcie oddziela dwie odrębne funkcje:

- Dostawca wartości przyjmuje żądania HTTP i wypełnia słownik par klucz wartość.
- Integrator modelu użyje tego słownika, aby wypełnić modelu.

Dostawca wartości domyślne w interfejsie API sieci Web pobiera wartości z danych trasy i ciągu zapytania. Na przykład, jeśli identyfikator URI jest `http://localhost/api/values/1?location=48,-122`, dostawca wartości tworzy następujące pary klucz wartość:

- id = &quot;1&quot;
- Lokalizacja = &quot;48,122&quot;

(Czy jestem zakładając, że szablon trasy domyślne, czyli &quot;interfejsu api / {controller} / {id}&quot;.)

Nazwa parametru do powiązania są przechowywane w **ModelBindingContext.ModelName** właściwości. Integrator modelu szuka klucza o tej wartości w słowniku. Jeśli wartość istnieje i mogą być konwertowane na `GeoPoint`, integratora modelu przypisuje wartość powiązanych **ModelBindingContext.Model** właściwości.

Należy zauważyć, że integratora modelu nie jest ograniczona do konwersji typu prostego. W tym przykładzie integratora modelu najpierw przeszukuje tabelę znanej lokalizacji, a jeśli ono zawiedzie, używa konwersji typu.

**Ustawienie integratora modelu**

Istnieje kilka sposobów, aby ustawić integratora modelu. Po pierwsze, możesz dodać **[ModelBinder]** atrybutu do parametru.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample10.cs)]

Można również dodać **[ModelBinder]** atrybutu typu. Interfejs API sieci Web użyje integratora modelu określonego dla wszystkich parametrów tego typu.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample11.cs)]

Na koniec można dodać dostawcę integratora modelu do **HttpConfiguration**. Dostawca integratora modelu to po prostu klasę fabryki, która tworzy integratora modelu. Możesz utworzyć dostawcę, wynikające z [ModelBinderProvider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx) klasy. Jednak jeśli Twoja integratora modelu obsługuje jeden typ, łatwiej jest użyć wbudowanego **SimpleModelBinderProvider**, który jest przeznaczony do tego celu. Poniższy kod pokazuje, jak to zrobić.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample12.cs)]

Za pomocą dostawcy powiązanie modelu, nadal musisz dodać **[ModelBinder]** atrybutu do parametru mówić interfejsu API sieci Web, że należy używać w integratora modelu i nie element formatujący typu nośnika. Ale teraz nie musisz określić typ integratora modelu w atrybucie:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample13.cs)]

## <a name="value-providers"></a>Dostawców wartości

Wspomniałem, że obiekt wiążący modelu pobiera wartości z dostawcy wartości. Aby zapisać dostawcę wartości niestandardowych, należy zaimplementować **IValueProvider** interfejsu. Oto przykład, która pobiera wartości z plików cookie żądania:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample14.cs)]

Należy również utworzyć fabryki dostawców wartości, wynikające z **ValueProviderFactory** klasy.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample15.cs)]

Fabryki dostawców wartości, aby dodać **HttpConfiguration** w następujący sposób.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample16.cs)]

Interfejs API sieci Web Redaguj wszystkich dostawców wartości, więc kiedy wywołuje obiekt wiążący modelu **ValueProvider.GetValue**, integratora modelu otrzymuje wartość z pierwszym dostawcy wartości, które może wygenerować go.

Alternatywnie, można ustawić fabryki dostawców wartości na poziomie parametru przy użyciu **ValueProvider** atrybutu w następujący sposób:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample17.cs)]

Oznacza to, internetowy interfejs API wiązania modelu za pomocą fabryki dostawców określoną wartość, a nie używać żadnych innych dostawców zarejestrowanych wartości.

## <a name="httpparameterbinding"></a>HttpParameterBinding

Integratorów modelu są konkretne wystąpienie mechanizmu bardziej ogólnych. Jeśli przyjrzymy się **[ModelBinder]** atrybutu, zobaczysz, że pochodzi od abstrakcyjnej **ParameterBindingAttribute** klasy. Ta klasa definiuje jedną metodę **GetBinding**, co powoduje zwrócenie **HttpParameterBinding** obiektu:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample18.cs)]

**HttpParameterBinding** jest odpowiedzialny za powiązanie parametru z wartością. W przypadku właściwości **[ModelBinder]**, zwraca ten atrybut **HttpParameterBinding** implementację, która używa **interfejs IModelBinder** przeprowadzić faktycznego wiązania. Można też wdrożyć własną **HttpParameterBinding**.

Załóżmy, że chcesz pobrać elementów etag z `if-match` i `if-none-match` nagłówki w żądaniu. Rozpoczniemy od zdefiniowania klasy do reprezentowania elementów ETag.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample19.cs)]

Firma Microsoft będzie również zdefiniować wyliczenie, aby wskazać, czy można pobrać elementu ETag z `if-match` nagłówek lub `if-none-match` nagłówka.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample20.cs)]

Oto **HttpParameterBinding** , pobiera element ETag z żądanego nagłówka i wiąże ją do parametru typu tagu ETag:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample21.cs)]

**ExecuteBindingAsync** metoda wykonuje wiązanie. W ramach tej metody, Dodaj wartość powiązano parametr **ActionArgument** słownika **HttpActionContext**.

> [!NOTE]
> Jeśli Twoje **ExecuteBindingAsync** metoda odczytuje treści komunikatu żądania, Zastąp **WillReadBody** właściwości, aby zwracała wartość true. Treść żądania może być Niebuforowane strumienia, które mogą być odczytane tylko raz, więc interfejsu API sieci Web wymusza reguły co najwyżej jednego powiązanie można odczytać treści komunikatu.


Aby zastosować niestandardowy **HttpParameterBinding**, można zdefiniować atrybut, który pochodzi od klasy **ParameterBindingAttribute**. Aby uzyskać `ETagParameterBinding`, zdefiniujemy dwa atrybuty, jeden dla `if-match` nagłówków i jeden dla `if-none-match` nagłówków. Abstrakcyjna klasa bazowa zarówno dziedziczyć.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample22.cs)]

Poniżej przedstawiono metody kontrolera, który używa `[IfNoneMatch]` atrybutu.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample23.cs)]

Oprócz **ParameterBindingAttribute**, jest innym haku dodawania niestandardowego **HttpParameterBinding**. Na **HttpConfiguration** obiektu **ParameterBindingRules** właściwości to zbiór funkcji anomymous typu (**HttpParameterDescriptor**  - &gt; **HttpParameterBinding**). Na przykład można dodać regułę, która korzysta z żadnych parametrów element ETag dla metody GET `ETagParameterBinding` z `if-none-match`:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample24.cs)]

Funkcja powinna zwrócić `null` parametrów, których powiązanie nie ma zastosowania.

## <a name="iactionvaluebinder"></a>IActionValueBinder

Cały proces wiązanie parametru jest kontrolowany przez podłączane usługę **IActionValueBinder**. Domyślna implementacja klasy **IActionValueBinder** wykonuje następujące czynności:

1. Wyszukaj **ParameterBindingAttribute** w parametrze. Obejmuje to **[FromBody]**, **[FromUri]**, i **[ModelBinder]**, lub atrybutów niestandardowych.
2. W przeciwnym razie Szukaj w **HttpConfiguration.ParameterBindingRules** dla funkcji, która zwraca wartość inną niż null **HttpParameterBinding**.
3. W przeciwnym razie Użyj domyślnych reguł, które mogę opisane wcześniej. 

    - Jeśli typ parametru jest "prosta" lub ma konwertera typów, należy powiązać z identyfikatora URI. Jest to równoważne umieszczenie **[FromUri]** atrybutu w parametrze.
    - W przeciwnym razie spróbuj odczytywać parametr treści komunikatu. Jest to równoważne umieszczenie **[FromBody]** w parametrze.

Jeśli chcesz, możesz zamienić całą **IActionValueBinder** usługi przy użyciu niestandardowych implementacji.

## <a name="additional-resources"></a>Dodatkowe zasoby

[Przykład powiązania parametru niestandardowego](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/CustomParameterBinding/ReadMe.txt)

Mike wstrzymania napisał dobre serię wpisów w blogu o wiązanie parametru interfejsu API sieci Web:

- [Jak działa interfejs API sieci Web, wiązanie parametrów](https://blogs.msdn.com/b/jmstall/archive/2012/04/16/how-webapi-does-parameter-binding.aspx)
- [Styl MVC wiązanie parametru dla interfejsu API sieci Web](https://blogs.msdn.com/b/jmstall/archive/2012/04/18/mvc-style-parameter-binding-for-webapi.aspx)
- [Jak powiązać do obiektów niestandardowych w podpisach akcji w interfejsie API sieci Web i MVC](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx)
- [Jak utworzyć dostawcę wartości niestandardowe w interfejsie API sieci Web](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx)
- [Wiązanie parametru internetowy interfejs API kulisy](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx)
