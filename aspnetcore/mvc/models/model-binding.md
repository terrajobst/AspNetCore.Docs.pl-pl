---
title: Wiązanie modelu w programie ASP.NET Core
author: tdykstra
description: Dowiedz się, jak wiązanie modelu programu ASP.NET Core MVC mapuje dane z żądań HTTP na parametry metody akcji.
ms.assetid: 0be164aa-1d72-4192-bd6b-192c9c301164
ms.author: tdykstra
ms.date: 08/14/2018
uid: mvc/models/model-binding
ms.openlocfilehash: 0ce20a8040c6b19da1f57e1c053a7ef81d8bcb23
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754191"
---
# <a name="model-binding-in-aspnet-core"></a>Wiązanie modelu w programie ASP.NET Core

Przez [Rachel Appel](https://github.com/rachelappel)

## <a name="introduction-to-model-binding"></a>Wprowadzenie do wiązanie modelu

Wiązanie modelu programu ASP.NET Core MVC mapuje dane z żądań HTTP na parametry metody akcji. Parametry mogą być proste typy, takie jak ciągi, liczby całkowite lub wartości zmiennoprzecinkowe lub mogą one być typów złożonych. Jest to atrakcyjną funkcją MVC, ponieważ mapowanie danych przychodzących z odpowiednikiem jest w przypadku często powtarzane, niezależnie od tego, rozmiarze i poziomie złożoności danych. MVC rozwiązuje ten problem przez troszczyć powiązania, dzięki którym deweloperzy nie muszą zachować poprawiania nieco inna wersja tego samego kodu w każdej aplikacji. Zapisywanie tekstu do typu konwertera kodu jest niewygodna i występowania błędów.

## <a name="how-model-binding-works"></a>Jak działa powiązanie modelu

Gdy MVC odbiera żądanie HTTP, kieruje je do metody określonej akcji kontrolera. Określa, która metoda akcji do uruchomienia w oparciu o na tym, co w danych trasy, a następnie powiąże wartości z żądania HTTP do parametrów tej metody akcji. Na przykład rozważmy następujący adres URL:

`http://contoso.com/movies/edit/2`

Ponieważ szablon trasy wyglądają następująco, `{controller=Home}/{action=Index}/{id?}`, `movies/edit/2` kieruje do `Movies` kontrolera, a jego `Edit` metody akcji. Akceptuje także opcjonalny parametr o nazwie `id`. Kod dla metody akcji powinna wyglądać mniej więcej tak:

```csharp
public IActionResult Edit(int? id)
   ```

Uwaga: Ciągi w trasy adresu URL nie jest uwzględniana.

MVC podejmie próbę żądania danych można powiązać parametry akcji według nazwy. MVC będzie szukać wartości dla każdego parametru przy użyciu nazwy parametrów i nazwy publicznej właściwości do ustawienia. W powyższym przykładzie nosi nazwę parametru jedyną akcją `id`, który MVC wiąże wartość z tej samej nazwie w wartości trasy. Oprócz wartości trasy MVC powiąże dane z różnych części żądania, a następnie robi to w ustalonej kolejności. Poniżej znajduje się lista źródeł danych, w kolejności wiązania modelu przegląda je:

1. `Form values`: Są to wartości formularza, które bardziej szczegółowo w żądaniu HTTP przy użyciu metody POST. (w tym żądania POST jQuery).

2. `Route values`: Zestaw wartości trasy udostępniane przez [routingu](xref:fundamentals/routing)

3. `Query strings`Części ciągu zapytania identyfikatora URI.

<!-- DocFX BUG
The link works but generates an error when building with DocFX
@fundamentals/routing
[Routing](xref:fundamentals/routing)
-->

Uwaga: Formularz wartości danych trasy i kwerend ciągów są przechowywane jako pary nazwa wartość.

Ponieważ wiązania modelu o podanie klucza o nazwie `id` i nie ma nic o nazwie `id` wartości formularza go następnie przeszedł do obozów szuka klucza, wartości trasy. W naszym przykładzie jest zgodny. Powiązaniem i wartością jest konwertowany na liczbę całkowitą 2. Tego samego żądania za pomocą edycji (identyfikator ciągu) czy przekonwertować na ciąg "2".

Do tej pory w przykładzie użyto typów prostych. W MVC proste typy są dowolnego typu pierwotnego .NET lub typu dla konwertera typu ciąg. Gdyby parametru metody akcji klasy takie jak `Movie` typ, który zawiera proste i złożone typy, właściwości nadal będą powiązanie modelu MVC go obsłużyć dobrze. Używa odbicia i rekursji przechodzenia właściwości typów złożonych szuka dopasowania. Wiązanie modelu wyszukuje wzorzec *parameter_name.property_name* do powiązania wartości właściwości. Jeśli nie znajdzie dopasowania wartości tego formularza podejmie można powiązać, przy użyciu tylko nazwy właściwości. Dla tych typów, takich jak `Collection` typów, wiązanie modelu szuka dopasowań, który ma *parameter_name [index]* lub po prostu *[index]*. Model traktuje powiązania `Dictionary` podobnie, typów, pytanie o *parameter_name [klucz]* lub po prostu *[klucz]*, tak długo, jak klucze są typy proste. Klucze, które są obsługiwane pasuje do nazwy pola HTML i pomocnicy tagów generowane dla tego samego typu modelu. Dzięki temu obustronne konwertowanie wartości tak, aby pola formularza pozostają wypełniony przy użyciu danych wejściowych użytkownika do ich zapewnienia wygody, na przykład, gdy powiązane dane z Utwórz lub zmodyfikuj nie przeszły pomyślnie sprawdzania poprawności.

Dla powiązania odbywały się klasy musi mieć publicznego konstruktora domyślnego i powiązać element członkowski musi być publicznymi właściwościami zapisywalny. W sytuacji, będzie tylko można utworzyć wystąpienia klasy przy użyciu publicznego konstruktora domyślnego wiązania modelu właściwości można ustawić.

Podczas wiązania parametru wiązania modelu zatrzymuje wyszukiwanie wartości o takiej nazwie i powoduje przeniesienie do powiązania następny parametr. W przeciwnym razie domyślne zachowanie wiązania modelu ustawia parametry domyślne wartości w zależności od ich typu:

* `T[]`: Z wyjątkiem produktów tablic typu `byte[]`, powiązanie ustawia parametry typu `T[]` do `Array.Empty<T>()`. Tablice typu `byte[]` są ustawione na `null`.

* Typy odwołań: Powiązanie tworzy instancję klasy przy użyciu domyślnego konstruktora bez konieczności ustawiania właściwości. Jednak model powiązania zestawów `string` parametry `null`.

* Typy dopuszczające wartości null: Typy dopuszczające wartości zerowe są ustawione na `null`. W powyższym przykładzie modelu powiązania zestawów `id` do `null` , ponieważ jest on typu `int?`.

* Typów wartości: Typy niedopuszczające wartości typu `T` są ustawione na `default(T)`. Na przykład powiązanie modelu spowoduje ustawienie parametru `int id` na 0. Należy wziąć pod uwagę przy użyciu sprawdzania poprawności modelu lub typy dopuszczające wartości null, a nie opierając się na wartości domyślne.

Jeśli powiązanie kończy się niepowodzeniem, MVC nie wyrzuca błąd. Należy sprawdzić wszystkie akcje, które akceptuje dane wejściowe użytkownika `ModelState.IsValid` właściwości.

Uwaga: Każdy wpis na kontrolerze `ModelState` właściwość `ModelStateEntry` zawierający `Errors` właściwości. Rzadko należy zbadać tę kolekcję, samodzielnie. Zamiast nich należy używać słów kluczowych `ModelState.IsValid`.

Ponadto istnieją pewne specjalne typy danych, które MVC należy wziąć pod uwagę podczas wiązania modelu:

* `IFormFile`, `IEnumerable<IFormFile>`: Jeden lub więcej plików przekazanych, będące częścią żądania HTTP.

* `CancellationToken`: Służy do anulować działanie w asynchronicznej kontrolerów.

Te typy można powiązać parametry akcji lub właściwości w typie klasy.

Po zakończeniu wiązania modelu [weryfikacji](validation.md) występuje. Wiązanie modelu domyślne działa doskonale nadaje się do większość scenariuszy projektowych. Jest również extensible, więc jeśli masz unikatowych potrzeb można dostosować wbudowane zachowanie.

## <a name="customize-model-binding-behavior-with-attributes"></a>Dostosowywanie zachowania wiązania modelu za pomocą atrybutów

MVC zawiera kilka atrybutów, które można użyć do kierowania jego domyślne zachowanie wiązania modelu do innego źródła. Na przykład można określić czy powiązania dla właściwości jest wymagana, czy jego powinno nigdy się wydarzyć na wszystkich przy użyciu `[BindRequired]` lub `[BindNever]` atrybutów. Alternatywnie można zastąpić domyślne źródło danych i określ źródło danych integratora modelu. Poniżej przedstawiono listę atrybutów powiązania modelu:

* `[BindRequired]`: Ten atrybut dodaje błąd stanu modelu, jeśli nie można powiązać.

* `[BindNever]`: Zawiera informacje dla integratora modelu, który nigdy nie można powiązać ten parametr.

* `[FromHeader]`, `[FromQuery]`, `[FromRoute]`, `[FromForm]`: Służą do określania źródło wiążące dokładnie, który chcesz zastosować.

* `[FromServices]`: Ten atrybut używa [wstrzykiwanie zależności](../../fundamentals/dependency-injection.md) by powiązać parametry z usług.

* `[FromBody]`: Służy elementy formatujące skonfigurowane wiązanie danych z treści żądania. Element formatujący jest zaznaczone, na podstawie typu zawartości żądania.

* `[ModelBinder]`: Służy do zastępowania domyślnego integratora modelu i źródło powiązania nazwy.

Atrybuty są bardzo przydatne narzędzia, gdy trzeba zastąpić domyślne zachowanie wiązania modelu.

## <a name="customize-model-binding-and-validation-globally"></a>Dostosowywanie wiązania modelu i sprawdzanie poprawności globalnie

Zachowanie modelu powiązania i sprawdzanie poprawności systemu jest wymuszany przez [ModelMetadata](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.modelmetadata) , który opisuje:

* Jak model ma zostać powiązany.
* Jak sprawdzanie poprawności jest wykonywane od typu i jego właściwości.

Aspekty zachowania systemu można skonfigurować globalnie przez dodanie dostawcy szczegóły [MvcOptions.ModelMetadataDetailsProviders](/dotnet/api/microsoft.aspnetcore.mvc.mvcoptions.modelmetadatadetailsproviders#Microsoft_AspNetCore_Mvc_MvcOptions_ModelMetadataDetailsProviders). MVC ma kilka dostawców wbudowanych szczegółowe informacje, które umożliwiają konfigurowanie zachowania, takie jak wyłączenie wiązania modelu lub sprawdzania poprawności w przypadku niektórych typów.

Aby wyłączyć wiązania modelu wszystkich modeli określonego typu, należy dodać [ExcludeBindingMetadataProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.metadata.excludebindingmetadataprovider) w `Startup.ConfigureServices`. Na przykład do wiązania modelu wyłączenie wszystkich modeli typu `System.Version`:

```csharp
services.AddMvc().AddMvcOptions(options =>
    options.ModelMetadataDetailsProviders.Add(
        new ExcludeBindingMetadataProvider(typeof(System.Version))));
```

Aby wyłączyć sprawdzanie poprawności właściwości określonego typu, należy dodać [SuppressChildValidationMetadataProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.suppresschildvalidationmetadataprovider) w `Startup.ConfigureServices`. Na przykład, aby wyłączyć sprawdzanie poprawności we właściwościach typu `System.Guid`:

```csharp
services.AddMvc().AddMvcOptions(options =>
    options.ModelMetadataDetailsProviders.Add(
        new SuppressChildValidationMetadataProvider(typeof(System.Guid))));
```

## <a name="bind-formatted-data-from-the-request-body"></a>Powiąż sformatowanych danych z treści żądania

Dane żądania mogą pochodzić w różnych formatach, takich jak JSON, XML i wiele innych. Użycie atrybutu [FromBody] Aby wskazać, że chcesz powiązać parametr z danymi w treści żądania, są używane skonfigurowany zestaw programów formatujących do obsługi danych żądania na podstawie jego typu zawartości. Domyślnie MVC zawiera `JsonInputFormatter` klasy do obsługi danych JSON, ale można dodać dodatkowe elementy formatujące do obsługi kodu XML i innych formatów niestandardowych.

> [!NOTE]
> Może być co najwyżej jeden parametr poszczególnych działań ozdobione `[FromBody]`. Wykonawczej programu ASP.NET Core MVC deleguje odpowiedzialność odczytywania strumienia żądania do elementu formatującego. Po strumienia żądania jest do odczytu dla parametru, zazwyczaj nie jest możliwe do odczytu strumienia żądania ponownie przez powiązanie inne `[FromBody]` parametrów.

> [!NOTE]
> `JsonInputFormatter` Jest domyślny element formatujący i opiera się na [Json.NET](https://www.newtonsoft.com/json).

ASP.NET Core wybiera elementy formatujące danych wejściowych, na podstawie [Content-Type](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html) nagłówka i typu parametru, chyba że jest atrybutem zastosowanym określającą inny sposób. Jeśli chcesz użyć kodu XML lub innego formatu należy skonfigurować je w *Startup.cs* plik, ale najpierw musisz uzyskać odwołanie do `Microsoft.AspNetCore.Mvc.Formatters.Xml` za pomocą narzędzia NuGet. Kod startowy powinien wyglądać mniej więcej tak:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddXmlSerializerFormatters();
   }
```

Możesz pisać kod w *Startup.cs* plik zawiera `ConfigureServices` metody z `services` argumentu można użyć do utworzenia usługi dla aplikacji platformy ASP.NET Core. W tym przykładzie dodamy element formatujący XML jako usługa, która zapewni MVC dla tej aplikacji. `options` Argument przekazany do `AddMvc` metoda umożliwia dodawanie oraz zarządzanie nimi filtry, elementy formatujące i inne opcje systemu z MVC podczas uruchamiania aplikacji. Następnie Zastosuj `Consumes` atrybutów do klasy kontrolera lub metody akcji do pracy z żądany format.

### <a name="custom-model-binding"></a>Niestandardowe wiązanie modelu

Wiązanie modelu można rozszerzyć przez napisanie własnego integratorów modelu niestandardowego. Dowiedz się więcej o [niestandardowe wiązanie modelu](../advanced/custom-model-binding.md).
