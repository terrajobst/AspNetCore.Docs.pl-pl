---
title: Niestandardowe elementy formatujące w ASP.NET Core Web API
author: rick-anderson
description: Dowiedz się, jak tworzyć i używać niestandardowych elementów formatujących dla interfejsów API sieci Web w programie ASP.NET Core.
ms.author: riande
ms.date: 02/08/2017
uid: web-api/advanced/custom-formatters
ms.openlocfilehash: dd25cda460ba758cd07de094eaadd1f2d8c28657
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78667672"
---
# <a name="custom-formatters-in-aspnet-core-web-api"></a>Niestandardowe elementy formatujące w ASP.NET Core Web API

Autor [Dykstra](https://github.com/tdykstra)

ASP.NET Core MVC obsługuje wymianę danych w interfejsach API sieci Web przy użyciu danych wejściowych i wyjściowych. Wejściowe elementy formatującego są używane przez [powiązanie modelu](xref:mvc/models/model-binding). Wyjściowe elementy formatujące są używane do [formatowania odpowiedzi](xref:web-api/advanced/formatting).

Struktura zawiera wbudowane, wejściowe i wyjściowe elementy formatujące dla JSON i XML. Udostępnia wbudowany program formatujący dane wyjściowe dla zwykłego tekstu, ale nie udostępnia wejściowego programu formatującego dla zwykłego tekstu.

W tym artykule pokazano, jak dodać obsługę dodatkowych formatów, tworząc niestandardowe elementy formatujące. Aby zapoznać się z przykładem niestandardowego wejściowego programu formatującego dla zwykłego tekstu, zobacz [TextPlainInputFormatter](https://github.com/aspnet/Entropy/blob/master/samples/Mvc.Formatters/TextPlainInputFormatter.cs) w witrynie GitHub.

[Wyświetl lub pobierz przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample) ([jak pobrać](xref:index#how-to-download-a-sample))

## <a name="when-to-use-custom-formatters"></a>Kiedy używać niestandardowych elementów formatujących

Użyj niestandardowego programu formatującego, jeśli chcesz, aby proces [negocjacji zawartości](xref:web-api/advanced/formatting#content-negotiation) obsługiwał typ zawartości, który nie jest obsługiwany przez wbudowane elementy formatujące.

Na przykład jeśli niektórzy klienci dla internetowego interfejsu API mogą obsłużyć format [protobuf](https://github.com/google/protobuf) , możesz chcieć użyć protobuf z tymi klientami, ponieważ jest to bardziej wydajne. Możesz też chcieć, aby internetowy interfejs API wysyłał nazwy i adresy kontaktów w formacie [vCard](https://wikipedia.org/wiki/VCard) , powszechnie używany format do wymiany danych kontaktowych. Przykładowa aplikacja udostępniona w tym artykule implementuje prosty program formatujący vCard.

## <a name="overview-of-how-to-use-a-custom-formatter"></a>Omówienie korzystania z niestandardowego programu formatującego

Poniżej przedstawiono procedurę tworzenia i używania niestandardowego programu formatującego:

* Utwórz wyjściową klasę programu formatującego, jeśli chcesz serializować dane do wysłania do klienta.
* Utwórz wejściową klasę programu formatującego, jeśli chcesz zdeserializować dane odebrane od klienta.
* Dodaj wystąpienia programu formatującego do `InputFormatters` i `OutputFormatters` kolekcji w [MvcOptions](/dotnet/api/microsoft.aspnetcore.mvc.mvcoptions).

W poniższych sekcjach przedstawiono wskazówki i przykłady kodu dla każdego z tych kroków.

## <a name="how-to-create-a-custom-formatter-class"></a>Jak utworzyć niestandardową klasę programu formatującego

Aby utworzyć program formatujący:

* Utwórz klasę z odpowiedniej klasy bazowej.
* Określ prawidłowe typy nośników i kodowania w konstruktorze.
* Zastąp `CanReadType`/`CanWriteType` metodami
* Zastąp `ReadRequestBodyAsync`/`WriteResponseBodyAsync` metodami
  
### <a name="derive-from-the-appropriate-base-class"></a>Pochodny od odpowiedniej klasy bazowej

W przypadku typów multimediów tekstowych (na przykład vCard) pochodzi z klasy bazowej [TextInputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.textinputformatter) lub [TextOutputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.textoutputformatter) .

[!code-csharp[](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=classdef)]

Przykład danych wejściowych programu formatującego można znaleźć w [aplikacji](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample)przykładowej.

W przypadku typów binarnych pochodzi z klasy bazowej [InputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.inputformatter) lub [OutputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.outputformatter) .

### <a name="specify-valid-media-types-and-encodings"></a>Określ prawidłowe typy multimediów i kodowania

W konstruktorze Określ prawidłowe typy nośników i kodowania, dodając do kolekcji `SupportedMediaTypes` i `SupportedEncodings`.

[!code-csharp[](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=ctor&highlight=3,5-6)]

Przykład danych wejściowych programu formatującego można znaleźć w [aplikacji](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample)przykładowej.

> [!NOTE]
> Nie można wykonać iniekcji zależności konstruktora w klasie programu formatującego. Na przykład nie można uzyskać rejestratora, dodając parametr rejestratora do konstruktora. Aby uzyskać dostęp do usług, należy użyć obiektu kontekstu, który jest przesyłany do Twoich metod. W [poniższym](#read-write) przykładzie kodu pokazano, jak to zrobić.

### <a name="override-canreadtypecanwritetype"></a>Zastąp element overridetype/unwritetype

Określ typ, do którego można wykonać deserializacja lub serializować z, zastępując metody `CanReadType` lub `CanWriteType`. Na przykład można utworzyć tylko tekst w formacie vCard z typu `Contact` i na odwrót.

[!code-csharp[](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=canwritetype)]

Przykład danych wejściowych programu formatującego można znaleźć w [aplikacji](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample)przykładowej.

#### <a name="the-canwriteresult-method"></a>Metoda CanWriteResult

W niektórych scenariuszach trzeba przesłonić `CanWriteResult`, a nie `CanWriteType`. Użyj `CanWriteResult`, jeśli są spełnione następujące warunki:

* Metoda działania zwraca klasę modelu.
* Istnieją klasy pochodne, które mogą być zwracane w czasie wykonywania.
* Musisz wiedzieć w czasie wykonywania, który Klasa pochodna została zwrócona przez akcję.

Na przykład załóżmy, że podpis metody akcji zwraca typ `Person`, ale może zwrócić `Student` lub `Instructor` typ, który pochodzi z `Person`. Jeśli chcesz, aby program formatujący obsługiwał tylko `Student` obiektów, Sprawdź typ [obiektu](/dotnet/api/microsoft.aspnetcore.mvc.formatters.outputformattercanwritecontext.object#Microsoft_AspNetCore_Mvc_Formatters_OutputFormatterCanWriteContext_Object) w obiekcie kontekstu dostarczonym do metody `CanWriteResult`. Należy zauważyć, że nie jest konieczne używanie `CanWriteResult`, gdy metoda akcji zwróci `IActionResult`; w takim przypadku Metoda `CanWriteType` otrzymuje typ środowiska uruchomieniowego.

<a id="read-write"></a>

### <a name="override-readrequestbodyasyncwriteresponsebodyasync"></a>Override ReadRequestBodyAsync/WriteResponseBodyAsync

Wykonywanie rzeczywistej pracy deserializacji lub serializacji w `ReadRequestBodyAsync` lub `WriteResponseBodyAsync`. W wyróżnionych wierszach w poniższym przykładzie pokazano, jak uzyskać usługi z kontenera iniekcji zależności (nie można pobrać ich z parametrów konstruktora).

[!code-csharp[](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=writeresponse&highlight=3-4)]

Przykład danych wejściowych programu formatującego można znaleźć w [aplikacji](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample)przykładowej.

## <a name="how-to-configure-mvc-to-use-a-custom-formatter"></a>Jak skonfigurować MVC do używania niestandardowego programu formatującego

Aby użyć niestandardowego programu formatującego, Dodaj wystąpienie klasy programu formatującego do kolekcji `InputFormatters` lub `OutputFormatters`.

[!code-csharp[](custom-formatters/sample/Startup.cs?name=mvcoptions&highlight=3-4)]

Elementy formatujące są oceniane w kolejności, w jakiej je wstawiasz. Pierwszeństwo ma pierwszy.

## <a name="next-steps"></a>Następne kroki

* [Przykładowa aplikacja dla tego dokumentu](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample), która implementuje proste elementy formatujące dane wejściowe i wyjściowe w formacie vCard. Aplikacje odczytuje i zapisuje wizytówki vCard, które wyglądają jak w poniższym przykładzie:

```
BEGIN:VCARD
VERSION:2.1
N:Davolio;Nancy
FN:Nancy Davolio
UID:20293482-9240-4d68-b475-325df4a83728
END:VCARD
```

Aby wyświetlić dane wyjściowe w formacie vCard, uruchom aplikację i Wyślij żądanie Get z nagłówkiem Accept "text/vCard" do `http://localhost:63313/api/contacts/` (w przypadku uruchamiania z programu Visual Studio) lub `http://localhost:5000/api/contacts/` (w przypadku uruchamiania z wiersza polecenia).

Aby dodać wizytówkę vCard do kolekcji kontaktów w pamięci, Wyślij żądanie post na ten sam adres URL, z nagłówkiem content-type "text/vCard" i z tekstem w formacie vCard w treści, tak jak w powyższym przykładzie.
