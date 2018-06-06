---
title: Rejestrowanie wysokiej wydajności z LoggerMessage w ASP.NET Core
author: guardrex
description: Dowiedz się, jak używać LoggerMessage do tworzenia buforowalnej obiektów delegowanych, które wymagają mniej alokacji obiektu w scenariuszach logowania wysokiej wydajności.
manager: wpickett
ms.author: riande
ms.date: 11/03/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/logging/loggermessage
ms.openlocfilehash: 5b5bd03b6cb5da693f046653a09ba400ee6ff585
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34729197"
---
# <a name="high-performance-logging-with-loggermessage-in-aspnet-core"></a>Rejestrowanie wysokiej wydajności z LoggerMessage w ASP.NET Core

Przez [Luke Latham](https://github.com/guardrex)

[LoggerMessage](/dotnet/api/microsoft.extensions.logging.loggermessage) funkcje tworzenia buforowalnej obiektów delegowanych, które wymagają mniej obiekt alokacji i mniejsze koszty obliczeniowych w porównaniu do [metody rozszerzenia rejestratora](/dotnet/api/Microsoft.Extensions.Logging.LoggerExtensions), takich jak `LogInformation`, `LogDebug`, i `LogError`. W scenariuszach logowania wysokiej wydajności, należy użyć `LoggerMessage` wzorca.

`LoggerMessage` zapewnia następujące korzyści wydajności za pośrednictwem metody rozszerzenia rejestratora:

* Metody rozszerzenia rejestratora wymagane typy wartości "boxing" (konwertowanie), takich jak `int`, do `object`. `LoggerMessage` Wzorzec pozwala uniknąć opakowanie przy użyciu statycznego `Action` pól i metod rozszerzenia z silną kontrolą typów parametrów.
* Metody rozszerzenia rejestratora należy przeanalizować szablon wiadomości (ciąg formatu o nazwie) zawsze komunikatu dziennika są zapisywane. `LoggerMessage` wymaga tylko raz podczas analizowania szablonu, jeśli komunikat jest zdefiniowana.

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/loggermessage/sample/) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

Przedstawiono przykładową aplikację `LoggerMessage` funkcje podstawowe oferty, w systemie śledzenia. Aplikacja dodaje i usuwa cudzysłowy korzystania z bazy danych w pamięci. Ponieważ te operacje są wykonywane, komunikaty w dzienniku są generowane przy użyciu `LoggerMessage` wzorca.

## <a name="loggermessagedefine"></a>LoggerMessage.Define

[Zdefiniuj (LogLevel, identyfikator zdarzenia, ciąg)](/dotnet/api/microsoft.extensions.logging.loggermessage.define) tworzy `Action` delegować do rejestrowania wiadomości. `Define` przeciążenia zezwala na przekazywanie maksymalnie sześć parametrów typu do ciągu formatu o nazwie (szablonu).

Podany ciąg `Define` metoda jest szablon i nie ciągu interpolowanym. Symbole zastępcze są wypełnione w kolejności określono typów. Przez Szablony nazwy symbolu zastępczego w szablonie powinny być opisowe i spójne. Służą one jako nazwy właściwości w strukturze danych dziennika. Firma Microsoft zaleca [Pascal wielkości liter](/dotnet/standard/design-guidelines/capitalization-conventions) dla nazwy symbolu zastępczego. Na przykład `{Count}`, `{FirstName}`.

Każdy komunikat dziennika jest `Action` przechowywany w polu statycznym utworzone przez `LoggerMessage.Define`. Na przykład przykładowa aplikacja tworzy pole do opisu komunikatu dziennika dla żądania GET strony indeksu (*Internal/LoggerExtensions.cs*):

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet1)]

Aby uzyskać `Action`, określ:

* Poziom dziennika.
* Identyfikator unikatowy zdarzenia ([EventId](/dotnet/api/microsoft.extensions.logging.eventid)) z nazwą metody statycznej rozszerzenia.
* Szablon wiadomości (o nazwie ciąg formatu). 

Żądanie strony indeksu zestawów aplikacji przykładowej:

* Poziom do dziennika `Information`.
* Identyfikator zdarzenia do `1` o nazwie `IndexPageRequested` metody.
* Szablon wiadomości (ciąg formatu o nazwie) na ciąg.

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet5)]

Rejestrowanie strukturalne magazyny może używać nazwy zdarzenia, gdy są dostarczane z identyfikatorem zdarzenia wzbogacić rejestrowania. Na przykład [Serilog](https://github.com/serilog/serilog-extensions-logging) , użyta zostanie nazwa zdarzenia.

`Action` Jest wywoływana przez metodę rozszerzenie jednoznacznie. `IndexPageRequested` — Metoda rejestruje komunikat dla żądania GET strony indeksu w przykładowej aplikacji:

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet9)]

`IndexPageRequested` wywoływana jest rejestratora w `OnGetAsync` metody w *Pages/Index.cshtml.cs*:

[!code-csharp[](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3)]

Sprawdź dane wyjściowe konsoli aplikacji:

```console
info: LoggerMessageSample.Pages.IndexModel[1]
      => RequestId:0HL90M6E7PHK4:00000001 RequestPath:/ => /Index
      GET request for Index page
```

Do przekazania parametrów do komunikatu dziennika, należy zdefiniować typy sześciu podczas tworzenia pola statycznego. Przykładowa aplikacja rejestruje ciąg podczas dodawania oferty, definiując `string` wpisz `Action` pola:

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet2)]

Szablon wiadomości dziennika delegata odbiera jego symbole zastępcze z udostępnione typy. Przykładowa aplikacja definiuje delegata dodawania oferty, w którym parametr oferty jest `string`:

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet6)]

Metody rozszerzenia statycznych dodawania oferty, `QuoteAdded`, otrzymuje wartość argumentu oferty i przekazuje je do `Action` delegować:

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet10)]

W modelu strony strony indeksu (*Pages/Index.cshtml.cs*), `QuoteAdded` nosi nazwę logowania komunikat:

[!code-csharp[](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet3&highlight=6)]

Sprawdź dane wyjściowe konsoli aplikacji:

```console
info: LoggerMessageSample.Pages.IndexModel[2]
      => RequestId:0HL90M6E7PHK5:0000000A RequestPath:/ => /Index
      Quote added (Quote = 'You can avoid reality, but you cannot avoid the consequences of avoiding reality. - Ayn Rand')
```

Implementuje aplikacji przykładowej `try` &ndash; `catch` wzorzec do usunięcia oferty. Komunikat informacyjny jest rejestrowany przez operację usuwania powiodło się. Komunikat o błędzie jest rejestrowane dla operacji delete, gdy jest zgłaszany wyjątek. Komunikat dziennika powiodła się operacja usuwania obejmuje ślad stosu wyjątku (*Internal/LoggerExtensions.cs*):

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet3)]

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet7)]

Należy zwrócić uwagę, jak wyjątek jest przekazywana do delegata `QuoteDeleteFailed`:

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet11)]

W modelu strony dla strony indeksu, usuwanie oferty pomyślne wywołanie `QuoteDeleted` metoda rejestratora. Gdy oferta nie zostanie odnaleziony do usunięcia, `ArgumentNullException` jest generowany. Wyjątkiem jest to spowodowane `try` &ndash; `catch` instrukcji i rejestrowane przez wywołanie metody `QuoteDeleteFailed` metoda rejestratora w `catch` bloku (*Pages/Index.cshtml.cs*):

[!code-csharp[](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet5&highlight=14,18)]

Po pomyślnym usunięciu oferty Sprawdź dane wyjściowe konsoli aplikacji:

```console
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:00000016 RequestPath:/ => /Index
      Quote deleted (Quote = 'You can avoid reality, but you cannot avoid the consequences of avoiding reality. - Ayn Rand' Id = 1)
```

Podczas usuwania oferty zakończy się niepowodzeniem, sprawdź, czy dane wyjściowe aplikacji konsoli. Należy pamiętać, że wyjątek jest dołączany do wiadomości dziennika:

```console
fail: LoggerMessageSample.Pages.IndexModel[5]
      => RequestId:0HL90M6E7PHK5:00000010 RequestPath:/ => /Index
      Quote delete failed (Id = 999)
System.ArgumentNullException: Value cannot be null.
Parameter name: entity
   at Microsoft.EntityFrameworkCore.Utilities.Check.NotNull[T](T value, String parameterName)
   at Microsoft.EntityFrameworkCore.DbContext.Remove[TEntity](TEntity entity)
   at Microsoft.EntityFrameworkCore.Internal.InternalDbSet`1.Remove(TEntity entity)
   at LoggerMessageSample.Pages.IndexModel.<OnPostDeleteQuoteAsync>d__14.MoveNext() in 
      <PATH>\sample\Pages\Index.cshtml.cs:line 87
```

## <a name="loggermessagedefinescope"></a>LoggerMessage.DefineScope

[DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) tworzy `Func` delegować do definiowania [dziennika zakresu](xref:fundamentals/logging/index#log-scopes). `DefineScope` przeciążenia zezwala na przekazywanie maksymalnie trzech parametrów typu do ciągu formatu o nazwie (szablonu).

Jak w przypadku `Define` metoda, do podanego ciągu `DefineScope` metoda jest szablon i nie ciągu interpolowanym. Symbole zastępcze są wypełnione w kolejności określono typów. Przez Szablony nazwy symbolu zastępczego w szablonie powinny być opisowe i spójne. Służą one jako nazwy właściwości w strukturze danych dziennika. Firma Microsoft zaleca [Pascal wielkości liter](/dotnet/standard/design-guidelines/capitalization-conventions) dla nazwy symbolu zastępczego. Na przykład `{Count}`, `{FirstName}`.

Zdefiniuj [dziennika zakres](xref:fundamentals/logging/index#log-scopes) do zastosowania do serii komunikaty dziennika przy użyciu [DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) metody.

Przykładowa aplikacja ma **Wyczyść wszystko** przycisk usuwania wszystkich znaków cudzysłowu w bazie danych. Cudzysłowy zostaną usunięte przez usunięcie jednego naraz. Zawsze oferty zostanie usunięty, `QuoteDeleted` wywoływana jest metoda rejestratora. Zakres dziennika zostanie dodany do tych wiadomości dziennika.

Włącz `IncludeScopes` w sekcji rejestratora konsoli *appsettings.json*:

[!code-csharp[](loggermessage/sample/appsettings.json?highlight=3-5)]

Aby utworzyć zakres dziennika, Dodaj pole do przechowywania `Func` delegowanie dla zakresu. Przykładowa aplikacja tworzy pole o nazwie `_allQuotesDeletedScope` (*Internal/LoggerExtensions.cs*):

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet4)]

Użyj `DefineScope` można utworzyć obiektu delegowanego. Maksymalnie trzy typy można określić do użycia jako argumenty szablonu po wywołaniu obiektu delegowanego. Przykładowa aplikacja korzysta z szablonu wiadomości, który zawiera liczby cudzysłowów usunięto ( `int` typu):

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet8)]

Udostępnia metody statyczne rozszerzenie wiadomości dziennika. Uwzględnij wszystkie parametry typu dla nazwanych właściwości, które pojawiają się w szablon wiadomości. Przykładowa aplikacja przyjmuje `count` cudzysłowy do usunięcia i zwraca `_allQuotesDeletedScope`:

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet12)]

Zawija zakres odwołuje się do rozszerzenia rejestrowania `using` bloku:

[!code-csharp[](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet4&highlight=5-6,14)]

Sprawdź, czy w komunikatach w dzienniku aplikacji konsoli w danych wyjściowych. Następujące wyniki przedstawiono trzy cudzysłowy usunięte z komunikatem zakresu dziennika uwzględnione:

```console
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:0000002E RequestPath:/ => /Index => All quotes deleted (Count = 3)
      Quote deleted (Quote = 'Quote 1' Id = 2)
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:0000002E RequestPath:/ => /Index => All quotes deleted (Count = 3)
      Quote deleted (Quote = 'Quote 2' Id = 3)
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:0000002E RequestPath:/ => /Index => All quotes deleted (Count = 3)
      Quote deleted (Quote = 'Quote 3' Id = 4)
```

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Rejestrowanie](xref:fundamentals/logging/index)
