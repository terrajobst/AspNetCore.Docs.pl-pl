---
title: Niestandardowe elementy formatujące w interfejsu API platformy ASP.NET Core sieci Web
author: rick-anderson
description: Informacje o sposobie tworzenia i używania niestandardowych elementy formatujące do interfejsów API w ASP.NET Core sieci web.
ms.author: tdykstra
ms.date: 02/08/2017
uid: web-api/advanced/custom-formatters
ms.openlocfilehash: a21fcea68d957d0344309c9bbd3286b71c092f60
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36273861"
---
# <a name="custom-formatters-in-aspnet-core-web-api"></a>Niestandardowe elementy formatujące w interfejsu API platformy ASP.NET Core sieci Web

Przez [Dykstra niestandardowy](https://github.com/tdykstra)

Podstawowe ASP.NET MVC ma wbudowaną obsługę wymiany danych w interfejsów API sieci web za pomocą formatu JSON, XML lub zwykły tekst. W tym artykule pokazano, jak dodać obsługę dodatkowych formatach tworząc niestandardowe elementy formatujące.

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

## <a name="when-to-use-custom-formatters"></a>Kiedy używać niestandardowej elementy formatujące

Niestandardowy element formatujący należy użyć [zawartości negocjacji](xref:web-api/advanced/formatting#content-negotiation) procesu do obsługi typu zawartości, która nie jest obsługiwana przez wbudowane elementy formatujące (JSON, XML i zwykły tekst).

Na przykład, jeśli niektórzy klienci dla interfejsu API sieci web mogą obsługiwać [Protobuf](https://github.com/google/protobuf) format, warto użyć Protobuf z tymi klientami, ponieważ jest bardziej wydajne. Można też interfejs API sieci web do wysyłania nazwy i adresy [vCard](https://wikipedia.org/wiki/VCard) formatu, często używane format wymiany danych kontaktowych. Przykładowa aplikacja podaną w tym artykule implementuje element formatujący vCard proste.

## <a name="overview-of-how-to-use-a-custom-formatter"></a>Przegląd sposobu użycia niestandardowego elementu formatującego

Poniżej przedstawiono procedurę tworzenia i używania niestandardowego elementu formatującego:

* Utwórz klasę program formatujący danych wyjściowych, aby serializować dane do wysłania do klienta.
* Utwórz klasę wejściowy element formatujący, aby deserializować danych otrzymanych od klienta.
* Dodawanie wystąpień użytkownika elementy formatujące do `InputFormatters` i `OutputFormatters` kolekcji w [MvcOptions](/dotnet/api/microsoft.aspnetcore.mvc.mvcoptions).

Poniższe sekcje zawierają wskazówki i przykłady kodu dla każdego z tych kroków.

## <a name="how-to-create-a-custom-formatter-class"></a>Tworzenie klasy niestandardowej programu formatującego

Aby utworzyć element formatujący:

* Klasa wyprowadzona z odpowiedniej klasy podstawowej.
* Podaj prawidłowy nośnik typy i kodowania w konstruktorze.
* Zastąpienie `CanReadType` / `CanWriteType` metody
* Zastąpienie `ReadRequestBodyAsync` / `WriteResponseBodyAsync` metody
  
### <a name="derive-from-the-appropriate-base-class"></a>Pochodzi od odpowiedniej klasy podstawowej

Dla typów nośników tekstu (na przykład vCard), pochodzi z [TextInputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.textinputformatter) lub [TextOutputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.textoutputformatter) klasy podstawowej.

[!code-csharp[](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=classdef)]

Dla typu binary, pochodzi z [InputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.inputformatter) lub [OutputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.outputformatter) klasy podstawowej.

### <a name="specify-valid-media-types-and-encodings"></a>Określ prawidłowy nośnik typy i kodowania

W konstruktorze, Określ prawidłowy nośnik typy i kodowania przez dodanie do `SupportedMediaTypes` i `SupportedEncodings` kolekcji.

[!code-csharp[](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=ctor&highlight=3,5-6)]

> [!NOTE]
> Nie można wykonać iniekcji zależności konstruktora klasy elementu formatującego. Na przykład nie można pobrać Rejestrator przez dodanie parametru rejestratora konstruktora. Aby uzyskać dostęp do usługi, należy użyć obiektu context, który zostanie przekazany do metody. Przykładowy kod [poniżej](#read-write) pokazano, jak to zrobić.

### <a name="override-canreadtypecanwritetype"></a>Zastąpienie CanReadType/CanWriteType

Określ typ może deserializować do lub serializacji z przez zastąpienie `CanReadType` lub `CanWriteType` metody. Na przykład tylko można utworzyć pliku vCard tekst z `Contact` typu i na odwrót.

[!code-csharp[](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=canwritetype)]

#### <a name="the-canwriteresult-method"></a>Metoda CanWriteResult

W niektórych scenariuszach należy zastąpić `CanWriteResult` zamiast `CanWriteType`. Użyj `CanWriteResult` jeśli są spełnione następujące warunki:

* Stosowana metoda akcji zwraca klasę modelu.
* Brak klasy pochodne, które może być zwracany w czasie wykonywania.
* Należy znać w czasie wykonywania, z którego pochodzi klasy został zwrócony przez akcję.

Na przykład załóżmy, że podpis metody akcji zwraca `Person` typu, ale może zwrócić `Student` lub `Instructor` typu pochodzącego od `Person`. Jeśli chcesz, aby Twoje elementu formatującego do obsługi tylko `Student` obiektów, sprawdź typ [obiektu](/dotnet/api/microsoft.aspnetcore.mvc.formatters.outputformattercanwritecontext#Microsoft_AspNetCore_Mvc_Formatters_OutputFormatterCanWriteContext_Object) w obiekt kontekstu do `CanWriteResult` — metoda. Należy pamiętać, że nie jest konieczne użycie `CanWriteResult` gdy metoda akcji zwraca `IActionResult`; w takim przypadku `CanWriteType` metody odbiera typ środowiska uruchomieniowego.

<a id="read-write"></a>
### <a name="override-readrequestbodyasyncwriteresponsebodyasync"></a>Zastąpienie ReadRequestBodyAsync/WriteResponseBodyAsync

Wykonują rzeczywistą pracę podczas deserializacji lub serializacji w `ReadRequestBodyAsync` lub `WriteResponseBodyAsync`. Wyróżnione wiersze w następującym przykładzie pokazano, jak można pobrać usługi z kontenera iniekcji zależności (nie można pobrać je z parametrami konstruktora).

[!code-csharp[](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=writeresponse&highlight=3-4)]

## <a name="how-to-configure-mvc-to-use-a-custom-formatter"></a>Jak skonfigurować MVC, aby użyć niestandardowego elementu formatującego

Aby użyć niestandardowego elementu formatującego, dodać wystąpienia klasy elementu formatującego do `InputFormatters` lub `OutputFormatters` kolekcji.

[!code-csharp[](custom-formatters/sample/Startup.cs?name=mvcoptions&highlight=3-4)]

Elementy formatujące są oceniane w kolejności, w jakiej wstawić je. Pierwsza z nich ma pierwszeństwo.

## <a name="next-steps"></a>Następne kroki

Zobacz [Przykładowa aplikacja](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample), który implementuje proste vCard dane wejściowe i elementy formatujące danych wyjściowych. Aplikacja czyta i zapisuje vCard, który wygląda jak w poniższym przykładzie:

```
BEGIN:VCARD
VERSION:2.1
N:Davolio;Nancy
FN:Nancy Davolio
UID:20293482-9240-4d68-b475-325df4a83728
END:VCARD
```

Aby wyświetlić vCard dane wyjściowe, uruchom aplikację i Wyślij żądanie Get z Akceptuj nagłówka "tekstu/vcard" do `http://localhost:63313/api/contacts/` (jeśli jest uruchomiony w programie Visual Studio) lub `http://localhost:5000/api/contacts/` (jeśli jest uruchomione z wiersza polecenia).

Aby dodać vCard do kolekcji w pamięci, kontaktów, Wyślij żądanie Post do tego samego adresu URL, z nagłówka Content-Type "tekstu/vcard" i vCard z tekstem, sformatowany tak jak w powyższym przykładzie.
