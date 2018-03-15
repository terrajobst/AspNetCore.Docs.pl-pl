---
title: "Wiązania modelu w platformy ASP.NET Core"
author: rachelappel
description: "Dowiedz się, jak wiązanie modelu w programie ASP.NET MVC Core mapuje dane z żądania HTTP parametrami metody akcji."
manager: wpickett
ms.assetid: 0be164aa-1d72-4192-bd6b-192c9c301164
ms.author: rachelap
ms.date: 01/22/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/models/model-binding
ms.openlocfilehash: f416bda1d7bccdfa922ba598a411ef1d150e3111
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/15/2018
---
# <a name="model-binding-in-aspnet-core"></a>Wiązania modelu w platformy ASP.NET Core

Przez [Rachel Appel](https://github.com/rachelappel)

## <a name="introduction-to-model-binding"></a>Wprowadzenie do modelu powiązania

Wiązania modelu w programie ASP.NET MVC Core mapuje dane z żądania HTTP do parametrów metody akcji. Parametry mogą być typu prostego przykład ciągów, liczb całkowitych lub elementów przestawnych lub mogą być typy złożone. Jest funkcją dużą MVC, ponieważ mapowanie danych przychodzących z odpowiednikiem jest często powtarzane scenariusz, niezależnie od tego, czy rozmiar lub złożoność elementu danych. MVC rozwiązuje ten problem, optymalizacji abstrakcyjność powiązanie tak programiści nie muszą zachować ponowne zapisywanie nieco inna wersja tego samego kodu, w każdej aplikacji. Zapisywanie tekstu do typu konwertera kodu jest niewygodny i błąd podatnych na błędy.

## <a name="how-model-binding-works"></a>Jak działa wiązania modelu

Gdy MVC odbiera żądanie HTTP, kieruje go do metody określonej akcji kontrolera. Określa, która metoda akcji do uruchomienia w oparciu o nowościach w danych trasy, a następnie wiąże wartości z żądania HTTP do parametrów tej metody akcji. Rozważmy na przykład następujący adres URL:

`http://contoso.com/movies/edit/2`

Ponieważ szablon trasy wygląda tak, `{controller=Home}/{action=Index}/{id?}`, `movies/edit/2` kieruje do `Movies` kontrolera, a jego `Edit` metody akcji. Przyjmuje opcjonalny parametr o nazwie `id`. Kod dla metody akcji powinna wyglądać mniej więcej tak:

```csharp
public IActionResult Edit(int? id)
   ```

Uwaga: Ciągów w trasy adresu URL nie jest uwzględniana.

MVC spróbuje powiązać dane żądania z parametrami akcji według nazwy. MVC sprawdza wartości dla każdego parametru przy użyciu nazwy parametru i nazwy jej do ustawienia właściwości publiczne. W powyższym przykładzie parametr akcji tylko o nazwie `id`, który MVC wiąże wartość z tej samej nazwie w wartości trasy. Oprócz wartości trasy MVC zostanie wiązanie danych z różnych części żądania i kopiuje je w kolejności zestawu. Poniżej znajduje się lista źródeł danych w kolejności wiązania modelu przegląda je:

1. `Form values`: Są to wartości formularza, które go w programie żądań HTTP używających metody POST. (włącznie z żądaniami POST jQuery).

2. `Route values`: Zestaw wartości trasy udostępniane przez [routingu](xref:fundamentals/routing)

3. `Query strings`Części ciągu zapytania identyfikatora URI.

<!-- DocFX BUG
The link works but generates an error when building with DocFX
@fundamentals/routing
[Routing](xref:fundamentals/routing)
-->

Uwaga: Formularza, wartości, dane trasy i zapytania się, że parametry są przechowywane jako pary nazwa wartość.

Ponieważ wiązania modelu wyświetlony monit o podanie klucz o nazwie `id` i nie ma niczego o nazwie `id` w wartości formularza on przeniesiony wartości trasy wyszukiwania dla tego klucza. W naszym przykładzie jest dopasowanie. Powiązaniem, a wartość jest konwertowana na liczbę całkowitą 2. Tym samym żądanie za pomocą edycji (identyfikator ciągu) czy przekonwertować na ciąg "2".

W przykładzie użyto do tej pory typów prostych. W nazwie wzorca MVC proste typy są dowolnego typu pierwotnego .NET lub typu dla konwertera typu ciąg. Jeśli parametr metody akcji takich jak zostały klasy `Movie` typu, który zawiera zarówno proste i złożone typy właściwości, nadal będą powiązanie modelu MVC jego obsługa dobrze. Przechodzenie właściwości typów złożonych wyszukiwanie dopasowań używa odbicia i rekursji. Wiązanie modelu szuka wzorzec *parameter_name.property_name* powiązać wartości właściwości. Jeśli nie znajdzie pasujących wartości formularza, podejmie można powiązać, przy użyciu po prostu nazwę właściwości. Dla tych typów, takie jak `Collection` typów wiązania modelu szuka dopasowań, który ma *nazwa_parametru [Indeks]* lub po prostu *[Indeks]*. Traktuje powiązanie modelu `Dictionary` podobnie typy pytania o *nazwa_parametru [klucz]* lub po prostu *[klucz]*, tak długo, jak klucze są typów prostych. Klucze, które są obsługiwane zgodne, nazwy pola HTML i pomocników tagów wygenerowany dla tego samego typu modelu. Dzięki temu wartości dwustronną komunikację tak, aby pola formularza pozostają wypełniony przy użyciu danych wejściowych użytkownika udogodnienie, na przykład, gdy dane powiązane z Tworzenie lub edytowanie nie przeszedł pomyślnie weryfikacji.

Aby powiązanie nastąpić klasy musi mieć publicznego konstruktora domyślnego i element członkowski może być powiązane musi być zapisywalny właściwości publiczne. W przypadku wiązania modelu się, że klasa będzie można wdrożyć tylko, przy użyciu domyślnego publicznego konstruktora właściwości można ustawić.

Wiązania parametru wiązania modelu zatrzyma się wyszukiwanie wartości o takiej nazwie i powoduje przeniesienie powiązać parametr dalej. W przeciwnym razie domyślne zachowanie wiązania modelu ustawia parametry z wartościami domyślnymi, w zależności od ich typu:

* `T[]`: Z wyjątkiem produktów tablic typu `byte[]`, powiązanie ustawia parametry typu `T[]` do `Array.Empty<T>()`. Tablice typu `byte[]` są ustawione na `null`.

* Typy odwołań: Powiązanie tworzy wystąpienie klasy przy użyciu domyślnego konstruktora bez ustawienia właściwości. Jednak model ustawia powiązanie `string` parametry `null`.

* Typy dopuszczające wartości zerowe: Typy dopuszczające wartości null są ustawione na `null`. W powyższym przykładzie modelu powiązania zestawów `id` do `null` , ponieważ jest on typu `int?`.

* Typy wartości: Wartości null typów wartości typu `T` są ustawione na `default(T)`. Na przykład powiązanie modelu spowoduje ustawienie parametru `int id` na 0. Należy wziąć pod uwagę przy użyciu weryfikacji modelu lub typ zerowalny zamiast polegania na wartości domyślne.

Jeśli powiązanie kończy się niepowodzeniem, MVC nie Zgłoś błąd. Należy sprawdzić wszystkie akcje, które akceptuje dane wejściowe użytkownika `ModelState.IsValid` właściwości.

Uwaga: Każdy wpis na kontrolerze `ModelState` właściwość jest `ModelStateEntry` zawierający `Errors` właściwości. Rzadko należy samodzielnie zapytania tej kolekcji. Zamiast nich należy używać słów kluczowych `ModelState.IsValid`.

Istnieją ponadto niektóre typy specjalne danych, które MVC należy wziąć pod uwagę podczas tworzenia powiązania modelu:

* `IFormFile`, `IEnumerable<IFormFile>`: Jeden lub więcej plików przekazanego, które są częścią żądania HTTP.

* `CancellationToken`: Używany do anulowania działania w asynchronicznej kontrolerów.

Te typy może być powiązana do parametrów akcji lub właściwości w typie klasy.

Po zakończeniu wiązania modelu [weryfikacji](validation.md) występuje. Świetnie dla większość scenariuszy programowania sprawdza się w domyślnej wiązania modelu. Jest również rozszerzonego, więc mają unikatowe potrzeby można dostosować zachowanie wbudowanych.

## <a name="customize-model-binding-behavior-with-attributes"></a>Dostosowywać zachowanie wiązania modelu z atrybutami

MVC zawiera kilka atrybutów, które umożliwia bezpośrednie jego domyślne zachowanie wiązania modelu z innym źródłem. Na przykład można określić, czy powiązanie jest wymagany dla właściwości Jeśli go powinno nigdy się wydarzyć w ogóle przy użyciu `[BindRequired]` lub `[BindNever]` atrybutów. Alternatywnie można zastąpić domyślne źródło danych i określ źródło danych integratora modelu. Poniżej przedstawiono listę atrybutów powiązanie modelu:

* `[BindRequired]`: Ten atrybut dodaje błąd stanu modelu, jeśli nie można powiązać.

* `[BindNever]`: Określa, że nigdy nie powiązać ten parametr integratora modelu.

* `[FromHeader]`, `[FromQuery]`, `[FromRoute]`, `[FromForm]`: Ich używać do określ źródło powiązania dokładne chcesz zastosować.

* `[FromServices]`: Ten atrybut używa [iniekcji zależności](../../fundamentals/dependency-injection.md) by powiązać parametry z usług.

* `[FromBody]`: Użyj skonfigurowanych elementy formatujące wiązanie danych z treści żądania. Program formatujący jest zaznaczone, na podstawie typu zawartości żądania.

* `[ModelBinder]`: Służy do zastępowania domyślnego integratora modelu, źródle powiązania i nazwę.

Atrybuty są bardzo przydatne narzędzia, gdy chcesz zastąpić domyślne zachowanie wiązania modelu.

## <a name="bind-formatted-data-from-the-request-body"></a>Wiązanie danych sformatowany z treści żądania

Dane żądania są dostępne w różnych formatach, w tym JSON, XML i wiele innych. Użycie atrybutu [FromBody] Aby wskazać, że chcesz powiązać parametr z danymi w treści żądania, MVC używa skonfigurowany zestaw programów formatujących do obsługi danych żądania na podstawie jego typu zawartości. Domyślnie platforma MVC zawiera `JsonInputFormatter` klasy do obsługi danych JSON, ale można dodać dodatkowe elementy formatujące obsługi XML i inne niestandardowe formaty.

> [!NOTE]
> Może istnieć co najwyżej jeden parametr w każdej akcji ozdobione `[FromBody]`. Środowiska wykonawczego platformy ASP.NET Core MVC deleguje odpowiedzialność odczytywania strumienia żądania do elementu formatującego. Gdy dla parametru jest odczytać strumienia żądania, zwykle nie jest możliwe można odczytać strumienia żądania ponownie przez inne powiązanie `[FromBody]` parametrów.

> [!NOTE]
> `JsonInputFormatter` Jest domyślny element formatujący i jest oparty na [Json.NET](https://www.newtonsoft.com/json).

ASP.NET zaznacza programy formatujące wejściowych, na podstawie [Content-Type](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html) nagłówka i typ parametru, chyba że jest atrybutem stosowanym do go przez określenie, w przeciwnym razie wartość. Jeśli chcesz użyć XML lub innego formatu należy skonfigurować je w *Startup.cs* pliku, ale najpierw trzeba uzyskać odwołania do `Microsoft.AspNetCore.Mvc.Formatters.Xml` przy użyciu narzędzia NuGet. Kod uruchomienia powinien wyglądać mniej więcej tak:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddXmlSerializerFormatters();
   }
```

Kod w *Startup.cs* plik zawiera `ConfigureServices` metody z `services` argumentu można użyć do tworzenia usług dla aplikacji ASP.NET. W przykładzie dodajemy formatujący XML jako usługa, która określi MVC dla tej aplikacji. `options` Argument przekazany do `AddMvc` metoda pozwala na dodawanie i zarządzanie filtrów, elementy formatujące i inne opcje systemu z MVC podczas uruchamiania aplikacji. Następnie Zastosuj `Consumes` do klasy kontrolera lub metody akcji do pracy z formatem ma atrybut.

### <a name="custom-model-binding"></a>Wiązania niestandardowe modelu

Pisanie własnych integratorów modeli niestandardowych można rozszerzyć wiązania modelu. Dowiedz się więcej o [wiązania niestandardowego modelu](../advanced/custom-model-binding.md).
