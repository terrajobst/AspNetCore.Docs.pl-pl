---
title: Wiązanie modelu w programie ASP.NET Core
author: tdykstra
description: Dowiedz się, jak działa powiązanie modelu w programie ASP.NET Core oraz dostosować jego zachowanie.
ms.assetid: 0be164aa-1d72-4192-bd6b-192c9c301164
ms.author: tdykstra
ms.date: 05/31/2019
uid: mvc/models/model-binding
ms.openlocfilehash: 7d62ccecdacbd34a38a1fd8c58979a9b09cf86e8
ms.sourcegitcommit: e7e04a45195d4e0527af6f7cf1807defb56dc3c3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/06/2019
ms.locfileid: "66750197"
---
# <a name="model-binding-in-aspnet-core"></a>Wiązanie modelu w programie ASP.NET Core

W tym artykule opisano, jakie wiązanie modelu przebiegło, jak działa i jak dostosować jego zachowanie.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/model-binding/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample)).

## <a name="what-is-model-binding"></a>Co to jest wiązania modelu

Kontrolery i stron Razor pracować z danymi, które pochodzą z żądania HTTP. Na przykład dane trasy może dostarczyć klucza rekordu, a pola przesłanego formularza może podać wartości dla właściwości modelu. Pisanie kodu w celu pobrania każdej z tych wartości i konwertować z ciągów na typy .NET będzie uciążliwe i podatne na błędy. Wiązanie modelu pozwala zautomatyzować ten proces. System powiązań modelu:

* Pobiera dane z różnych źródeł, takie jak przekierowywanie danych, pola formularza i ciągi zapytań.
* Udostępnia dane do kontrolerów i stronami Razor w parametrów metod i właściwości publiczne.
* Konwertuje ciąg danych do typów .NET.
* Aktualizuje właściwości typów złożonych.

## <a name="example"></a>Przykład

Załóżmy, że masz następujące metody akcji:

[!code-csharp[](model-binding/samples/2.x/Controllers/PetsController.cs?name=snippet_DogsOnly)]

I aplikacja odbiera żądanie z tym adresem URL:

```
http://contoso.com/api/pets/2?DogsOnly=true
```

Wiązanie modelu wykracza jednak poniższe kroki, po system routingu wybiera metody akcji:

* Wyszukuje pierwszy parametr `GetByID`, liczbą całkowitą o nazwie `id`.
* Przegląda dostępne źródła w żądaniu HTTP i wyszukuje `id` = "2" w danych trasy.
* Konwertuje ciąg "2" do liczby całkowitej 2.
* Wyszukuje następny parametr `GetByID`, wartość logiczna o nazwie `dogsOnly`.
* Przegląda źródeł i umożliwia znalezienie "DogsOnly = true" w ciągu zapytania. Dopasowywanie nazw nie jest rozróżniana wielkość liter.
* Konwertuje ciąg "true" na wartość logiczną `true`.

Struktura następnie wywołuje `GetById` jest metoda w wersji 2 dla `id` parametru i `true` dla `dogsOnly` parametru.

W powyższym przykładzie cele powiązanie modelu to parametry metody, które są typy proste. Obiekty docelowe mogą być również właściwości typu złożonego. Po każdej właściwości pomyślnie jest związany, [Walidacja modelu](xref:mvc/models/validation) występuje dla tej właściwości. Rekord danych jest powiązany z modelu, a wszelkie błędy powiązania lub sprawdzania poprawności są przechowywane w [ControllerBase.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState) lub [PageModel.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState). Aby dowiedzieć się, jeśli ten proces zakończył się pomyślnie, aplikacja sprawdza, czy [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) flagi.

## <a name="targets"></a>Obiekty docelowe

Wiązanie modelu próbuje znaleźć wartości dla następujących rodzajów elementów docelowych:

* Parametry metody akcji kontrolera, że żądanie jest kierowane do.
* Parametry metody obsługi stron Razor, który żądanie jest kierowane do. 
* Właściwości publiczne, kontrolera lub `PageModel` klasy, jeśli określony przez atrybuty.

### <a name="bindproperty-attribute"></a>Atrybut [BindProperty]

Można zastosować do właściwości publicznej kontrolera lub `PageModel` klasy, aby spowodować, że wiązanie modelu pod kątem tej właściwości:

[!code-csharp[](model-binding/samples/2.x/Pages/Instructors/Edit.cshtml.cs?name=snippet_BindProperty&highlight=7-8)]

### <a name="bindpropertiesattribute"></a>Atrybut [BindProperties]

Dostępne w programie ASP.NET Core 2.1 lub nowszej.  Mogą być stosowane do kontrolera lub `PageModel` klasy, aby poinformować wiązania modelu pod kątem wszystkie publiczne właściwości klasy:

[!code-csharp[](model-binding/samples/2.x/Pages/Instructors/Create.cshtml.cs?name=snippet_BindProperties&highlight=1-2)]

### <a name="model-binding-for-http-get-requests"></a>Wiązanie dla żądania HTTP GET modelu

Domyślnie właściwości nie są związane na żądania HTTP GET. Zazwyczaj jest wszystko, czego potrzebujesz do żądania GET rekordów parametru Identyfikatora. Identyfikator rekordu jest używany do wyszukiwania elementów w bazie danych. W związku z tym nie ma potrzeby do powiązania właściwość, która posiada wystąpienie modelu. W scenariuszach, którego właściwości powiązane z danymi z żądania GET, należy ustawić `SupportsGet` właściwości `true`:

[!code-csharp[](model-binding/samples/2.x/Pages/Instructors/Index.cshtml.cs?name=snippet_SupportsGet)]

## <a name="sources"></a>Źródła

Domyślnie powiązanie modelu pobiera dane w postaci par klucz wartość z następujących źródeł w żądaniu HTTP:

1. Pola formularza 
1. Treść żądania (dla [kontrolery, które mają atrybut [klasy ApiController]](xref:web-api/index#binding-source-parameter-inference).)
1. Dane trasy
1. Parametry ciągu zapytania
1. Przekazane pliki 

Dla każdego parametru target lub właściwość źródła są skanowane w kolejności wskazanej na tej liście. Istnieje kilka wyjątków:

* Trasy, danych i zapytań, wartościami ciągów są używane tylko dla typów prostych.
* Przekazane pliki są powiązane tylko do typów docelowych, które implementują `IFormFile` lub `IEnumerable<IFormFile>`.

Jeśli domyślne zachowanie nie zapewnia odpowiednich wyników, można użyć jednej z następujących atrybutów, aby określić źródło do użycia w dowolnym danym obiektem docelowym. 

* [[FromQuery] ](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute) — Pobiera wartości z ciągu zapytania. 
* [[FromRoute] ](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute) — Pobiera wartości z danych trasy.
* [[FromForm] ](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) — Pobiera wartości z pól przesłanego formularza.
* [[FromBody] ](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute) — Pobiera wartości z treści żądania.
* [[FromHeader] ](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute) — Pobiera wartości z nagłówków HTTP.

Te atrybuty:

* Zostaną dodane do właściwości modelu indywidualnie (nie do klasy modelu), jak w poniższym przykładzie:

  [!code-csharp[](model-binding/samples/2.x/Models/Instructor.cs?name=snippet_FromQuery&highlight=5-6)]

* Opcjonalnie akceptuje wartości Nazwa modelu w konstruktorze. Ta opcja znajduje się w przypadku, gdy nazwa właściwości nie jest zgodna wartość w żądaniu. Na przykład wartość w żądaniu może być nagłówka z łącznikiem w nazwie, jak w poniższym przykładzie:

  [!code-csharp[](model-binding/samples/2.x/Pages/Instructors/Index.cshtml.cs?name=snippet_FromHeader)]

### <a name="frombody-attribute"></a>Atrybut [FromBody]

Dane treści żądania jest analizowany przy użyciu danych wejściowych elementy formatujące, które są specyficzne dla typu zawartości żądania. Opisano elementy formatujące danych wejściowych [w dalszej części tego artykułu](#input-formatters).

Nie stosuj `[FromBody]` do więcej niż jeden parametr, dla każdej metody akcji. Środowisko uruchomieniowe programu ASP.NET Core deleguje odpowiedzialność odczytywania strumienia żądania do wejściowego elementu formatującego. Gdy strumienia żądania jest do odczytu, nie jest już dostępny do odczytu ponownie dla powiązania innych `[FromBody]` parametrów.

### <a name="additional-sources"></a>Dodatkowe źródła

Źródło danych znajduje się w systemie wiązania modelu przez *wartość dostawców*. Można napisać i zarejestrować dostawców wartości niestandardowych, które pobierają dane do wiązania modelu z innych źródeł. Na przykład możesz zechcieć danych z plików cookie lub stanu sesji. Aby uzyskać dane z nowego źródła:

* Utwórz klasę, która implementuje `IValueProvider`.
* Utwórz klasę, która implementuje `IValueProviderFactory`.
* Rejestrowanie klasy fabryki w `Startup.ConfigureServices`.

Przykładowa aplikacja zawiera [dostawcy wartości](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/CookieValueProvider.cs) i [fabryki](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/CookieValueProviderFactory.cs) przykładu, który pobiera wartości z plików cookie. Oto kod rejestracyjny w `Startup.ConfigureServices`:

[!code-csharp[](model-binding/samples/2.x/Startup.cs?name=snippet_ValueProvider&highlight=3)]

Kod przedstawiony umieszcza dostawcy wartości niestandardowej po wszystkich dostawców wartości wbudowanej.  Aby stał się pierwszy na liście wywołań `Insert(0, new CookieValueProviderFactory())` zamiast `Add`.

## <a name="no-source-for-a-model-property"></a>Nie źródła dla właściwości modelu

Domyślnie błąd stanu modelu nie jest tworzone, jeśli wartość nie zostanie znaleziony dla właściwości modelu. Dla właściwości ustawiono na wartość null lub wartość domyślna:

* Typy dopuszczające wartości zerowe proste są ustawione na `null`.
* Typy nieprzyjmujące wartości są ustawione na `default(T)`. Na przykład parametr `int id` jest równa 0.
* W przypadku typów złożonych wiązania modelu tworzy wystąpienie przy użyciu domyślnego konstruktora bez konieczności ustawiania właściwości.
* Tablice są ustawione na `Array.Empty<T>()`, chyba że `byte[]` tablice są ustawione na `null`.

Jeśli stan modelu należy unieważniony, jeśli nic nie zostanie znaleziony w pola formularza dla właściwości modelu, należy użyć [atrybutu [BindRequired]](#bindrequired-attribute).

Uwaga że `[BindRequired]` zachowanie odnosi się do wiązania modelu z danych przesłanego formularza, a nie dane JSON lub XML w treści żądania. Dane treści żądania jest obsługiwany przez [wejściowych elementy formatujące](#input-formatters).

## <a name="type-conversion-errors"></a>Błędy konwersji typu

Jeśli źródło zostanie znaleziony, ale nie można przekonwertować na typ docelowy, stan modelu został oznaczony jako nieprawidłowy. Jako parametru target lub właściwości jest równa null lub wartość domyślną, jak wspomniano w poprzedniej sekcji.

W kontrolerze interfejsu API, który ma `[ApiController]` atrybutu nieprawidłowy model stanu powoduje automatyczne odpowiedzi HTTP 400.

Na stronie Razor ponowne wyświetlenie strony zawierającej komunikat o błędzie:

[!code-csharp[](model-binding/samples/2.x/Pages/Instructors/Create.cshtml.cs?name=snippet_HandleMBError&highlight=3-6)]

Weryfikacja po stronie klienta przechwytuje większość złe dane, które w przeciwnym razie będzie można przesłać formularza stron Razor. Tej weryfikacji sprawia, że trudno wyzwolić poprzedniego wyróżniony kod. Przykładowa aplikacja zawiera **Prześlij z nieprawidłową datę** przycisku, który umieszcza złe dane w **Data zatrudnienia** pola, a następnie przesyła formularz. Ten przycisk, przedstawiono sposób wyświetlania strony w kodzie po wystąpieniu błędów konwersji danych.

Po stronie zostanie wyświetlony ponownie, poprzedni kod, nieprawidłowe dane wejściowe nie jest wyświetlany w polu formularza. Jest to spowodowane właściwość modelu została ustawiona na wartość null lub wartość domyślną. Nieprawidłowe dane wejściowe są wyświetlane w komunikacie o błędzie. Ale jeśli chcesz ponownie wyświetlić złe dane w polu formularza, należy wziąć pod uwagę co właściwość modelu ciągu oraz wykonywania konwersji danych ręcznie.

Tej samej strategii jest zalecane, jeśli nie chcesz, aby błędy konwersji typu, aby spowodować błędy stanu modelu. Ciąg w takiej sytuacji należy właściwości modelu.

## <a name="simple-types"></a>Typy proste

Proste typy, które integratora modelu można przekonwertować ciągi źródłowe do są następujące:

* [Boolean](xref:System.ComponentModel.BooleanConverter)
* [Bajt](xref:System.ComponentModel.ByteConverter), [SByte](xref:System.ComponentModel.SByteConverter)
* [Char](xref:System.ComponentModel.CharConverter)
* [DateTime](xref:System.ComponentModel.DateTimeConverter)
* [DateTimeOffset](xref:System.ComponentModel.DateTimeOffsetConverter)
* [Decimal](xref:System.ComponentModel.DecimalConverter)
* [Double](xref:System.ComponentModel.DoubleConverter)
* [Enum](xref:System.ComponentModel.EnumConverter)
* [Identyfikator GUID](xref:System.ComponentModel.GuidConverter)
* [Int16](xref:System.ComponentModel.Int16Converter), [Int32](xref:System.ComponentModel.Int32Converter), [Int64](xref:System.ComponentModel.Int64Converter)
* [Single](xref:System.ComponentModel.SingleConverter)
* [TimeSpan](xref:System.ComponentModel.TimeSpanConverter)
* [UInt16](xref:System.ComponentModel.UInt16Converter), [UInt32](xref:System.ComponentModel.UInt32Converter), [UInt64](xref:System.ComponentModel.UInt64Converter)
* [Identyfikator URI](xref:System.UriTypeConverter)
* [Wersja](xref:System.ComponentModel.VersionConverter)

## <a name="complex-types"></a>Typy złożone

Typ złożony musi mieć publicznego konstruktora domyślnego i publicznego modyfikowalne właściwości do powiązania. W przypadku tworzenia powiązania modelu tworzenia wystąpienia klasy przy użyciu publicznego konstruktora domyślnego. 

Dla każdej właściwości typu złożonego wiązania modelu przegląda źródeł dla wzorca nazwy *prefix.property_name*. Jeśli nic nie zostanie znaleziony, szuka po prostu *property_name* bez prefiksu.

Powiązanie parametru, prefiks jest nazwą parametru. Powiązanie `PageModel` właściwość publiczną, prefiks jest nazwą właściwości publicznej. Niektóre atrybuty mają `Prefix` właściwość, która umożliwia zastąpienie użycie domyślnego parametru lub nazwę właściwości.

Załóżmy, że typ złożony jest następująca `Instructor` klasy:

  ```csharp
  public class Instructor
  {
      public int ID { get; set; }
      public string LastName { get; set; }
      public string FirstName { get; set; }
  }
  ```

### <a name="prefix--parameter-name"></a>Prefiks = Nazwa parametru

Czy model, który ma zostać powiązany parametr o nazwie `instructorToUpdate`:

```csharp
public IActionResult OnPost(int? id, Instructor instructorToUpdate)
```

Model rozpoczyna się wiązanie przeglądając źródeł dla klucza `instructorToUpdate.ID`. Jeśli, nie zostanie znaleziona, szuka `ID` bez prefiksu.

### <a name="prefix--property-name"></a>Prefiks = nazwa właściwości

Jeśli model, który ma zostać powiązany jest właściwość o nazwie `Instructor` kontrolera lub `PageModel` klasy:

```csharp
[BindProperty]
public Instructor Instructor { get; set; }
```

Model rozpoczyna się wiązanie przeglądając źródeł dla klucza `Instructor.ID`. Jeśli, nie zostanie znaleziona, szuka `ID` bez prefiksu.

### <a name="custom-prefix"></a>Prefiks niestandardowy

Czy model, który ma zostać powiązany parametr o nazwie `instructorToUpdate` i `Bind` Określa atrybut `Instructor` jako prefiksu:

```csharp
public IActionResult OnPost(
    int? id, [Bind(Prefix = "Instructor")] Instructor instructorToUpdate)
```

Model rozpoczyna się wiązanie przeglądając źródeł dla klucza `Instructor.ID`. Jeśli, nie zostanie znaleziona, szuka `ID` bez prefiksu.

### <a name="attributes-for-complex-type-targets"></a>Atrybuty dla celów typu złożonego

Kilka wbudowanych atrybutów są dostępne do kontrolowania wiązania modelu złożonych typów:

* `[BindRequired]`
* `[BindNever]`
* `[Bind]`

> [!NOTE]
> Te atrybuty mają wpływ na model powiązań gdy opublikowane dane formularza źródła wartości. Nie wpływają na elementy formatujące wejściowego, który proces opublikowane treść żądań JSON i XML. Opisano elementy formatujące danych wejściowych [w dalszej części tego artykułu](#input-formatters).
>
> Zawiera również omówienie `[Required]` atrybutu w [Walidacja modelu](xref:mvc/models/validation#required-attribute).

### <a name="bindrequired-attribute"></a>Atrybut [BindRequired]

Można zastosować tylko do właściwości modelu, aby nie parametry metody. Powoduje, że model można dodać błąd stanu modelu, w przypadku powiązania nie można powiązać właściwości modelu. Oto przykład:

[!code-csharp[](model-binding/samples/2.x/Models/InstructorWithCollection.cs?name=snippet_BindRequired&highlight=8-9)]

### <a name="bindnever-attribute"></a>Atrybut [BindNever]

Można zastosować tylko do właściwości modelu, aby nie parametry metody. Zapobiega wiązanie modelu z ustawienie dla właściwości modelu. Oto przykład:

[!code-csharp[](model-binding/samples/2.x/Models/InstructorWithDictionary.cs?name=snippet_BindNever&highlight=3-4)]

### <a name="bind-attribute"></a>Atrybut [powiązania]

Mogą być stosowane do klasy lub parametru metody. Określa właściwości modelu, które powinny zostać uwzględnione w wiązania modelu.

W poniższym przykładzie, tylko określonej właściwości elementu `Instructor` modelu są powiązane, po wywołaniu dowolnej procedury obsługi lub metody akcji:

```csharp
[Bind("LastName,FirstMidName,HireDate")]
public class Instructor
```

W poniższym przykładzie, tylko określonej właściwości elementu `Instructor` modelu są powiązane po `OnPost` metoda jest wywoływana:

```csharp
[HttpPost]
public IActionResult OnPost([Bind("LastName,FirstMidName,HireDate")] Instructor instructor)
```

`[Bind]` Atrybut może służyć do ochrony przed overposting w *tworzenie* scenariuszy. Nie działa dobrze w scenariuszach edycji wykluczone właściwości są ustawione na wartość null lub wartość domyślną, zamiast pozostał niezmieniony. Do obrony przed overposting, zaleca się wyświetlanie modeli zamiast `[Bind]` atrybutu. Aby uzyskać więcej informacji, zobacz [Uwaga dotycząca zabezpieczeń o overposting](xref:data/ef-mvc/crud#security-note-about-overposting).

## <a name="collections"></a>Kolekcje

Dla celów, które są kolekcjami typów prostych, wiązanie modelu szuka dopasowań, który ma *parameter_name* lub *property_name*. Jeśli nie zostanie znalezione dopasowanie, szuka w jednym z obsługiwanych formatów, bez prefiksu. Na przykład:

* Załóżmy, że można powiązać parametr jest tablicą o nazwie `selectedCourses`:

  ```csharp
  public IActionResult OnPost(int? id, int[] selectedCourses)
  ```

* Dane ciągu formularza lub zapytanie może być w jednym z następujących formatów:
   
  ```
  selectedCourses=1050&selectedCourses=2000 
  ```

  ```
  selectedCourses[0]=1050&selectedCourses[1]=2000
  ```

  ```
  [0]=1050&[1]=2000
  ```

  ```
  selectedCourses[a]=1050&selectedCourses[b]=2000&selectedCourses.index=a&selectedCourses.index=b
  ```

  ```
  [a]=1050&[b]=2000&index=a&index=b
  ```

* Następujący format jest obsługiwany tylko w formie danych:

  ```
  selectedCourses[]=1050&selectedCourses[]=2000
  ```

* Dla wszystkich poprzedni przykład formatuje wiązania modelu przekazuje tablicę dwa elementy, które `selectedCourses` parametru:

  * selectedCourses[0]=1050
  * selectedCourses [1] = 2000

  Dane formatuje tego użycie indeksu dolnego liczby (...) [0]... [1]...) należy się upewnić, że ich są numerowane kolejno od zera. Jeśli ma żadnych przerw w numeracji indeksu dolnego, wszystkie elementy po przerwa zostaną zignorowane. Na przykład jeśli indeksy dolne 0 i 2 zamiast 0 i 1, drugi element jest ignorowany.

## <a name="dictionaries"></a>słowniki

Aby uzyskać `Dictionary` lokalizacjach docelowych oraz wiązania modelu szuka dopasowań, który ma *parameter_name* lub *property_name*. Jeśli nie zostanie znalezione dopasowanie, szuka w jednym z obsługiwanych formatów, bez prefiksu. Na przykład:

* Załóżmy, że parametr docelowy jest `Dictionary<string, string>` o nazwie `selectedCourses`:

  ```csharp
  public IActionResult OnPost(int? id, Dictionary<int, string> selectedCourses)
  ```

* Przesłane dane ciągu formularza lub zapytania może wyglądać jak jeden z poniższych przykładów:

  ```
  selectedCourses[1050]=Chemistry&selectedCourses[2000]=Economics
  ```

  ```
  [1050]=Chemistry&selectedCourses[2000]=Economics
  ```

  ```
  selectedCourses[0].Key=1050&selectedCourses[0].Value=Chemistry&
  selectedCourses[1].Key=2000&selectedCourses[1].Value=Economics
  ```

  ```
  [0].Key=1050&[0].Value=Chemistry&[1].Key=2000&[1].Value=Economics
  ```

* Dla wszystkich poprzedni przykład formatuje wiązania modelu przekazuje słownika zawierającego dwa elementy, które `selectedCourses` parametru:

  * selectedCourses["1050"]="Chemistry"
  * selectedCourses["2000"]="Economics"

## <a name="special-data-types"></a>Specjalne typy danych

Istnieją pewne specjalne typy danych, które może obsłużyć wiązania modelu.

### <a name="iformfile-and-iformfilecollection"></a>IFormFile i IFormFileCollection

Przekazany plik uwzględnione w żądaniu HTTP.  Jest również obsługiwane `IEnumerable<IFormFile>` dla wielu plików.

### <a name="cancellationtoken"></a>Token anulowania

Używane, aby anulować działanie w asynchronicznej kontrolerów.

### <a name="formcollection"></a>FormCollection

Używany do pobierania wszystkich wartości z formularza przesłanych danych.

## <a name="input-formatters"></a>Programy formatujące danych wejściowych

Dane w treści żądania mogą być w formacie JSON, XML lub innym formacie. Aby analizować te dane, model wiązania używa *wejściowego elementu formatującego* skonfigurowanego do obsługi określonego typu zawartości. Domyślnie platformy ASP.NET Core obejmuje elementy formatujące wejściowych opartych na notacji JSON do obsługi danych JSON. Możesz dodać inne elementy formatujące dla innych typów zawartości.

Platforma ASP.NET Core wybiera elementy formatujące danych wejściowych, na podstawie [Consumes](xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute) atrybutu. Jeśli atrybut nie jest obecny, używa [nagłówek Content-Type](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html).

Aby użyć wbudowanych formatujących danych wejściowych XML:

* Zainstaluj `Microsoft.AspNetCore.Mvc.Formatters.Xml` pakietu NuGet.

* W `Startup.ConfigureServices`, wywołaj <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlSerializerFormatters*> lub <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlDataContractSerializerFormatters*>.

  [!code-csharp[](model-binding/samples/2.x/Startup.cs?name=snippet_ValueProvider&highlight=9)]

* Zastosuj `Consumes` atrybutów do klasy kontrolera lub metody akcji, które należy się spodziewać XML w treści żądania.

  ```csharp
  [HttpPost]
  [Consumes("application/xml")]
  public ActionResult<Pet> Create(Pet pet)
  ```

  Aby uzyskać więcej informacji, zobacz [wprowadzenie do serializacji XML](https://docs.microsoft.com/en-us/dotnet/standard/serialization/introducing-xml-serialization).

## <a name="exclude-specified-types-from-model-binding"></a>Wyklucz określonych typów z wiązania modelu

Model powiązania i sprawdzanie poprawności systems zachowanie jest wymuszany przez [ModelMetadata](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.modelmetadata). Można dostosować `ModelMetadata` przez dodanie dostawcy szczegóły [MvcOptions.ModelMetadataDetailsProviders](xref:Microsoft.AspNetCore.Mvc.MvcOptions.ModelMetadataDetailsProviders). Szczegóły wbudowane są dostępni dostawcy dla wyłączenie wiązania modelu lub sprawdzania poprawności dla określonych typów.

Aby wyłączyć wiązania modelu wszystkich modeli określonego typu, należy dodać <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.ExcludeBindingMetadataProvider> w `Startup.ConfigureServices`. Na przykład do wiązania modelu wyłączenie wszystkich modeli typu `System.Version`:

[!code-csharp[](model-binding/samples/2.x/Startup.cs?name=snippet_ValueProvider&highlight=4-5)]

Aby wyłączyć sprawdzanie poprawności właściwości określonego typu, należy dodać <xref:Microsoft.AspNetCore.Mvc.ModelBinding.SuppressChildValidationMetadataProvider> w `Startup.ConfigureServices`. Na przykład, aby wyłączyć sprawdzanie poprawności we właściwościach typu `System.Guid`:

[!code-csharp[](model-binding/samples/2.x/Startup.cs?name=snippet_ValueProvider&highlight=6-7)]

## <a name="custom-model-binders"></a>Integratorów modelu niestandardowego

Możesz rozszerzyć wiązania modelu przez napisanie niestandardowego integratora i przy użyciu modelu `[ModelBinder]` atrybutu, aby ją wybrać dla danego obiektu docelowego. Dowiedz się więcej o [niestandardowe wiązanie modelu](xref:mvc/advanced/custom-model-binding).

## <a name="manual-model-binding"></a>Wiązanie modelu ręczne

Wiązanie modelu można uruchomić ręcznie za pomocą <xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*> metody. Metoda jest określona zarówno `ControllerBase` i `PageModel` klasy. Przeciążenia metody umożliwiają określenie dostawca prefiksu i wartości do użycia. Metoda ta zwraca `false` Jeśli wiązanie modelu nie powiedzie się. Oto przykład:

[!code-csharp[](model-binding/samples/2.x/Pages/InstructorsWithCollection/Create.cshtml.cs?name=snippet_TryUpdate&highlight=1-4)]

## <a name="fromservices-attribute"></a>Atrybut [FromServices]

Nazwa tego atrybutu jest zgodna z wzorcem atrybutów powiązanie modelu, które określić źródło danych. Ale nie chodzi o powiązanie danych z dostawcy wartości. Pobiera wystąpienie typu z [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) kontenera. Jej celem jest zapewnienie alternatywa iniekcji konstruktora dla, gdy niezbędna jest usługa tylko wtedy, gdy wywoływana jest metoda określonego.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:mvc/models/validation>
* <xref:mvc/advanced/custom-model-binding>
