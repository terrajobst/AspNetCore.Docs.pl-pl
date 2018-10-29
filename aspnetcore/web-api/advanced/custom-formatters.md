---
title: Niestandardowe elementy formatujące w interfejsie API sieci Web platformy ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak utworzyć i używać niestandardowe elementy formatujące internetowych interfejsów API w programie ASP.NET Core.
ms.author: tdykstra
ms.date: 02/08/2017
uid: web-api/advanced/custom-formatters
ms.openlocfilehash: a038cd9c05950333fce9e72f67d6721198fae4d3
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/29/2018
ms.locfileid: "50206318"
---
# <a name="custom-formatters-in-aspnet-core-web-api"></a>Niestandardowe elementy formatujące w interfejsie API sieci Web platformy ASP.NET Core

przez [Tom Dykstra](https://github.com/tdykstra)

Platforma ASP.NET Core MVC ma wbudowaną obsługę wymiany danych w interfejsie web API przy użyciu formatów JSON, XML lub zwykły tekst. W tym artykule przedstawiono sposób dodawania Obsługa dodatkowych formatów, tworząc niestandardowe elementy formatujące.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))

## <a name="when-to-use-custom-formatters"></a>Kiedy używać niestandardowe elementy formatujące

Należy używać niestandardowego elementu formatującego [negocjacji zawartości](xref:web-api/advanced/formatting#content-negotiation) procesu do wspierania typu zawartości, która nie jest obsługiwana przez wbudowane elementy formatujące (JSON, XML i zwykły tekst).

Na przykład, jeśli niektórzy klienci dla interfejsu API sieci web mogą obsługiwać [Protobuf](https://github.com/google/protobuf) formatu, można użyć formatu Protobuf z tymi klientami, ponieważ jest bardziej wydajne. Można też interfejs API sieci web do wysłania, skontaktuj się z nazwy i adresy w [vCard](https://wikipedia.org/wiki/VCard) format, powszechnie używany format wymiany danych kontaktowych. Przykładowa aplikacja wyposażone w tym artykule implementuje prosty vCard elementu formatującego.

## <a name="overview-of-how-to-use-a-custom-formatter"></a>Omówienie sposobu użycia niestandardowego elementu formatującego

Poniżej przedstawiono kroki tworzenia i używania niestandardowego elementu formatującego:

* Aby serializacja danych do wysłania do klienta, należy utworzyć klasę program formatujący dane wyjściowe.
* Jeśli chcesz wykonać deserializacji dane odebrane od klienta, należy utworzyć klasę wejściowego elementu formatującego.
* Dodawanie wystąpień usługi elementy formatujące do `InputFormatters` i `OutputFormatters` kolekcje w programie [MvcOptions](/dotnet/api/microsoft.aspnetcore.mvc.mvcoptions).

Poniższe sekcje zawierają wskazówki i przykłady kodu dla każdego z tych kroków.

## <a name="how-to-create-a-custom-formatter-class"></a>Jak utworzyć klasy niestandardowego elementu formatującego.

Aby utworzyć element formatujący:

* Pochodną klasy z odpowiedniej klasy bazowej.
* Podaj prawidłowe pliki multimedialne i kodowania w konstruktorze.
* Zastąp `CanReadType` / `CanWriteType` metody
* Zastąp `ReadRequestBodyAsync` / `WriteResponseBodyAsync` metody
  
### <a name="derive-from-the-appropriate-base-class"></a>Pochodzi z odpowiedniej klasy bazowej

Dla typów nośników tekstu (na przykład vCard) dziedziczyć [TextInputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.textinputformatter) lub [TextOutputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.textoutputformatter) klasy bazowej.

[!code-csharp[](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=classdef)]

Dla typów binarnych dziedziczyć [InputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.inputformatter) lub [OutputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.outputformatter) klasy bazowej.

### <a name="specify-valid-media-types-and-encodings"></a>Określ prawidłowe pliki multimedialne i kodowania

W konstruktorze, należy określić, dodając do typów nośników prawidłowe i kodowania `SupportedMediaTypes` i `SupportedEncodings` kolekcji.

[!code-csharp[](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=ctor&highlight=3,5-6)]

> [!NOTE]
> Nie można wykonać wstrzykiwanie zależności Konstruktor w klasie elementu formatującego. Na przykład nie można pobrać rejestrator, dodając parametr rejestratora do konstruktora. Dostęp do usług, należy użyć obiektu context, który zostanie przekazany do metody. Przykładowy kod [poniżej](#read-write) pokazuje, jak to zrobić.

### <a name="override-canreadtypecanwritetype"></a>Zastąp CanReadType/CanWriteType

Określ typ, można wykonać deserializacji do lub serializacji z przez zastąpienie `CanReadType` lub `CanWriteType` metody. Na przykład tylko można utworzyć pliku vCard tekst z `Contact` typu i na odwrót.

[!code-csharp[](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=canwritetype)]

#### <a name="the-canwriteresult-method"></a>Metoda CanWriteResult

W niektórych przypadkach trzeba zastąpić `CanWriteResult` zamiast `CanWriteType`. Użyj `CanWriteResult` jeśli są spełnione następujące warunki:

* Metoda akcji zwraca klasę modelu.
* Brak klasy pochodne, które mogą być zwracane w czasie wykonywania.
* Należy znać w czasie wykonywania, który pochodne klasy został zwrócony przez akcję.

Załóżmy, że podpis metody akcji, zwraca `Person` typu, ale może zwrócić `Student` lub `Instructor` typ, który pochodzi od klasy `Person`. Jeśli chcesz, aby Twoje elementu formatującego do obsługi tylko `Student` obiektów, sprawdź typ [obiektu](/dotnet/api/microsoft.aspnetcore.mvc.formatters.outputformattercanwritecontext#Microsoft_AspNetCore_Mvc_Formatters_OutputFormatterCanWriteContext_Object) w obiekcie kontekstu udostępniane `CanWriteResult` metody. Należy pamiętać, że nie jest konieczne użycie `CanWriteResult` gdy metoda akcji zwraca `IActionResult`; w takim przypadku `CanWriteType` metoda otrzymuje typ środowiska uruchomieniowego.

<a id="read-write"></a>
### <a name="override-readrequestbodyasyncwriteresponsebodyasync"></a>Zastąp ReadRequestBodyAsync/WriteResponseBodyAsync

Wykonują rzeczywistą pracę podczas deserializacji lub serializacji w `ReadRequestBodyAsync` lub `WriteResponseBodyAsync`. Wyróżnione wiersze w poniższym przykładzie pokazano, jak można pobrać usługi z kontenera iniekcji zależności (nie można dostać się je z parametrów konstruktora).

[!code-csharp[](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=writeresponse&highlight=3-4)]

## <a name="how-to-configure-mvc-to-use-a-custom-formatter"></a>Jak skonfigurować MVC do użycia niestandardowego elementu formatującego.

Aby użyć niestandardowego elementu formatującego, dodaje wystąpienie klasy program formatujący `InputFormatters` lub `OutputFormatters` kolekcji.

[!code-csharp[](custom-formatters/sample/Startup.cs?name=mvcoptions&highlight=3-4)]

Programy formatujące są obliczane w kolejności, umieść je. Pierwsza z nich ma pierwszeństwo.

## <a name="next-steps"></a>Następne kroki

Zobacz [Przykładowa aplikacja](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample), który implementuje prosty vCard dane wejściowe i dane wyjściowe elementy formatujące. Aplikacja odczytuje i zapisuje vCard, które wyglądają jak w poniższym przykładzie:

```
BEGIN:VCARD
VERSION:2.1
N:Davolio;Nancy
FN:Nancy Davolio
UID:20293482-9240-4d68-b475-325df4a83728
END:VCARD
```

Aby wyświetlić vCard dane wyjściowe, uruchom aplikację i Wyślij żądanie Pobierz z Akceptuj nagłówek "text/vcard" `http://localhost:63313/api/contacts/` (jeśli jest uruchomiony w programie Visual Studio) lub `http://localhost:5000/api/contacts/` (uruchamianego z wiersza polecenia).

Aby dodać vCard do kolekcji w pamięci, kontaktów, Wyślij żądanie Wyślij do tego samego adresu URL, z nagłówka Content-Type "text/vcard" i vCard z tekstem, sformatowane tak jak w powyższym przykładzie.
