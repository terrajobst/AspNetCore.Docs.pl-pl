---
title: Rejestrowanie o wysokiej wydajności, za pomocą funkcji LoggerMessage w programie ASP.NET Core
author: guardrex
description: Dowiedz się, jak utworzyć podlega buforowaniu obiektów delegowanych, które wymagają mniejszej liczby alokacji obiektów w scenariuszach logowania o wysokiej wydajności przy za pomocą funkcji LoggerMessage.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/31/2019
uid: fundamentals/logging/loggermessage
ms.openlocfilehash: 7a030b4bb754f65f8d93e51f203344c2dc02a634
ms.sourcegitcommit: 5995f44e9e13d7e7aa8d193e2825381c42184e47
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/02/2019
ms.locfileid: "58809266"
---
# <a name="high-performance-logging-with-loggermessage-in-aspnet-core"></a>Rejestrowanie o wysokiej wydajności, za pomocą funkcji LoggerMessage w programie ASP.NET Core

Przez [Luke Latham](https://github.com/guardrex)

<xref:Microsoft.Extensions.Logging.LoggerMessage> funkcje tworzenia podlega buforowaniu obiektów delegowanych, które wymagają mniejszej liczby alokacji obiektów i mniejsze obciążenie obliczeniowe w porównaniu do [metody rozszerzenia rejestratora](xref:Microsoft.Extensions.Logging.LoggerExtensions), takich jak <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation*> i <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogDebug*>. W scenariuszach logowania o wysokiej wydajności, należy użyć <xref:Microsoft.Extensions.Logging.LoggerMessage> wzorca.

<xref:Microsoft.Extensions.Logging.LoggerMessage> zapewnia następujące korzyści wydajności za pośrednictwem metody rozszerzenia rejestratora:

* Metody rozszerzenia rejestratora wymagają typów wartości "opakowanie" (konwertowanie), takich jak `int`, do `object`. <xref:Microsoft.Extensions.Logging.LoggerMessage> Wzorzec pozwala uniknąć pakowania przy użyciu statycznej <xref:System.Action> pola i metody rozszerzające o silnie typizowane parametry.
* Metody rozszerzenia rejestratora musi przeanalizować szablon wiadomości (ciąg formatu o nazwie), każdym razem, gdy zostanie napisany komunikat dziennika. <xref:Microsoft.Extensions.Logging.LoggerMessage> wymaga tylko raz podczas analizowania szablonu, gdy komunikat jest definiowany.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/loggermessage/samples/2.x/LoggerMessageSamples/) ([sposobu pobierania](xref:index#how-to-download-a-sample))

Przykładowa aplikacja pokazuje <xref:Microsoft.Extensions.Logging.LoggerMessage> funkcji podstawowych oferty systemu śledzenia. Aplikacja dodaje i usuwa przy użyciu bazy danych w pamięci. Ponieważ te operacje są wykonywane, komunikaty dziennika są generowane przy użyciu <xref:Microsoft.Extensions.Logging.LoggerMessage> wzorca.

## <a name="loggermessagedefine"></a>LoggerMessage.Define

[Zdefiniuj (LogLevel, identyfikator zdarzenia, String)](xref:Microsoft.Extensions.Logging.LoggerMessage.Define*) tworzy <xref:System.Action> delegowanie do rejestrowania wiadomości. <xref:Microsoft.Extensions.Logging.LoggerMessage.Define*> przeciążenia zezwalają na przekazywanie parametrów typu do sześciu ciągowi formatu o nazwie (szablon).

Ciąg do <xref:Microsoft.Extensions.Logging.LoggerMessage.Define*> metoda jest szablon i nie ciągu interpolowanym. Symbole zastępcze są wypełnione w kolejności, że typy są określone. Nazwy symbolu zastępczego w szablonie powinny być opisowe i spójny, różnych szablonów. Służą jako nazwy właściwości ze strukturą danych dziennika. Firma Microsoft zaleca [Pascal wielkość liter w wyrazie](/dotnet/standard/design-guidelines/capitalization-conventions) dla nazw zastępczych. Na przykład `{Count}`, `{FirstName}`.

Każdy komunikat dziennika jest <xref:System.Action> znajdujące się w polu statycznym, które zostały utworzone przez [LoggerMessage.Define](xref:Microsoft.Extensions.Logging.LoggerMessage.Define*). Na przykład przykładowa aplikacja tworzy pola do opisania komunikat dziennika dla żądania GET strony indeksu (*Internal/LoggerExtensions.cs*):

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet1)]

Aby uzyskać <xref:System.Action>, określ:

* Poziom dziennika.
* Identyfikatora unikatowego wydarzenia (<xref:Microsoft.Extensions.Logging.EventId>) o nazwie metody statyczne rozszerzenie.
* Szablon wiadomości (o nazwie ciąg formatu). 

Żądanie strony indeksu zestawów aplikacji przykładowej:

* Poziom do dziennika `Information`.
* Identyfikator zdarzenia do `1` o nazwie `IndexPageRequested` metody.
* Szablon wiadomości (o nazwie ciąg formatu) na ciąg.

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet5)]

Magazyny rejestrowaniem strukturalnym może używać nazwy zdarzenia, gdy są dostarczane z identyfikatorem zdarzenia w celu wzbogacenia rejestrowania. Na przykład [Serilog](https://github.com/serilog/serilog-extensions-logging) używa nazwy zdarzenia.

<xref:System.Action> Jest wywoływana za pomocą metody rozszerzenia silnie typizowane. `IndexPageRequested` Metoda rejestruje komunikat dla żądania GET strony indeksu w przykładowej aplikacji:

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet9)]

`IndexPageRequested` jest wywoływana w rejestratora w `OnGetAsync` method in Class metoda *Pages/Index.cshtml.cs*:

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3)]

Sprawdź dane wyjściowe konsoli z aplikacji:

```console
info: LoggerMessageSample.Pages.IndexModel[1]
      => RequestId:0HL90M6E7PHK4:00000001 RequestPath:/ => /Index
      GET request for Index page
```

Do przekazania parametrów do komunikatu dziennika, należy zdefiniować maksymalnie sześć typów, tworząc pole statyczne. Przykładowa aplikacja rejestruje ciągu podczas dodawania oferty, definiując `string` wpisz <xref:System.Action> pola:

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet2)]

Szablon wiadomości dziennika pełnomocnika odbiera jego wartości symboli zastępczych z typów dostarczonych. Przykładowa aplikacja definiuje pełnomocnika dodawania oferty, gdzie parametr oferty jest `string`:

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet6)]

Metody statyczne rozszerzenie dodawania oferty, `QuoteAdded`, otrzymuje wartość argumentu oferty i przekazuje go do <xref:System.Action> delegować:

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet10)]

W modelu strony strony indeksu (*Pages/Index.cshtml.cs*), `QuoteAdded` nazywa się komunikat:

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Pages/Index.cshtml.cs?name=snippet3&highlight=6)]

Sprawdź dane wyjściowe konsoli z aplikacji:

```console
info: LoggerMessageSample.Pages.IndexModel[2]
      => RequestId:0HL90M6E7PHK5:0000000A RequestPath:/ => /Index
      Quote added (Quote = 'You can avoid reality, but you cannot avoid the 
          consequences of avoiding reality. - Ayn Rand')
```

Implementuje aplikacji przykładowej [spróbuj&ndash;catch](/dotnet/csharp/language-reference/keywords/try-catch) wzorzec do usunięcia oferty. Komunikat informacyjny jest rejestrowane dla operacji usuwania pomyślnie. Komunikat o błędzie jest rejestrowane dla operacji usuwania, gdy zostanie zgłoszony wyjątek. Komunikat dziennika powiodła się operacja usuwania obejmuje ślad stosu wyjątku (*Internal/LoggerExtensions.cs*):

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet3)]

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet7)]

Należy zauważyć, jak wyjątku jest przekazywany do obiektu delegowanego w `QuoteDeleteFailed`:

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet11)]

W modelu strony dla strony indeksu, usunięcie oferty pomyślnego wywołania `QuoteDeleted` metody rejestratora. Gdy oferta nie zostanie odnaleziony do usunięcia, <xref:System.ArgumentNullException> zgłaszany. Wyjątkiem jest to spowodowane [spróbuj&ndash;catch](/dotnet/csharp/language-reference/keywords/try-catch) instrukcji i rejestrowane przez wywołanie metody `QuoteDeleteFailed` metody rejestratora w [catch](/dotnet/csharp/language-reference/keywords/try-catch) bloku (*Pages/Index.cshtml.cs* ):

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Pages/Index.cshtml.cs?name=snippet5&highlight=14,18)]

Po pomyślnym usunięciu oferty, sprawdź dane wyjściowe konsoli z aplikacji:

```console
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:00000016 RequestPath:/ => /Index
      Quote deleted (Quote = 'You can avoid reality, but you cannot avoid the 
          consequences of avoiding reality. - Ayn Rand' Id = 1)
```

Po usunięciu oferty nie powiedzie się, sprawdź dane wyjściowe konsoli z aplikacji. Należy zwrócić uwagę na to, czy wyjątek znajduje się w komunikatu dziennika:

```console
fail: LoggerMessageSample.Pages.IndexModel[5]
      => RequestId:0HL90M6E7PHK5:00000010 RequestPath:/ => /Index
      Quote delete failed (Id = 999)
System.ArgumentNullException: Value cannot be null.
Parameter name: entity
   at Microsoft.EntityFrameworkCore.Utilities.Check.NotNull[T]
       (T value, String parameterName)
   at Microsoft.EntityFrameworkCore.DbContext.Remove[TEntity](TEntity entity)
   at Microsoft.EntityFrameworkCore.Internal.InternalDbSet`1.Remove(TEntity entity)
   at LoggerMessageSample.Pages.IndexModel.<OnPostDeleteQuoteAsync>d__14.MoveNext() 
      in <PATH>\sample\Pages\Index.cshtml.cs:line 87
```

## <a name="loggermessagedefinescope"></a>LoggerMessage.DefineScope

[DefineScope(String)](xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*) tworzy <xref:System.Func%601> delegować do definiowania [dziennika zakres](xref:fundamentals/logging/index#log-scopes). <xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*> przeciążenia umożliwić przekazywanie maksymalnie trzy parametry typu ciągowi formatu o nazwie (szablonu).

Jak w przypadku <xref:Microsoft.Extensions.Logging.LoggerMessage.Define*> metody, parametry udostępnione firmie <xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*> metoda jest szablon i nie ciągu interpolowanym. Symbole zastępcze są wypełnione w kolejności, że typy są określone. Nazwy symbolu zastępczego w szablonie powinny być opisowe i spójny, różnych szablonów. Służą jako nazwy właściwości ze strukturą danych dziennika. Firma Microsoft zaleca [Pascal wielkość liter w wyrazie](/dotnet/standard/design-guidelines/capitalization-conventions) dla nazw zastępczych. Na przykład `{Count}`, `{FirstName}`.

Zdefiniuj [dziennika zakres](xref:fundamentals/logging/index#log-scopes) do zastosowania na serię komunikatów dziennika przy użyciu <xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*> metody.

Przykładowa aplikacja ma **Wyczyść wszystko** przycisk usuwania wszystkich ofert w bazie danych. Cudzysłowy zostaną usunięte, usuwając jeden na raz. Każdorazowo po usunięciu oferty `QuoteDeleted` metoda jest wywoływana w rejestratora. Zakres dziennika jest dodawany do tych komunikatów dziennika.

Włącz `IncludeScopes` w sekcji rejestratora konsoli *appsettings.json*:

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/appsettings.json?highlight=3-5)]

Aby utworzyć zakres dziennika, należy dodać pole do przechowywania <xref:System.Func%601> delegowanie dla zakresu. Przykładowa aplikacja tworzy pole o nazwie `_allQuotesDeletedScope` (*Internal/LoggerExtensions.cs*):

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet4)]

Użyj <xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*> do utworzenia delegata. Maksymalnie trzy typy można określić do użycia jako argumenty szablonu, gdy obiekt delegowany jest wywoływany. Przykładowa aplikacja korzysta z szablonu komunikatu, który zawiera liczba usuniętych ofert ( `int` typu):

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet8)]

Udostępnia metody statyczne rozszerzenie komunikatu dziennika. Uwzględnić inne parametry typu nazwane właściwości, które pojawiają się w szablonie wiadomości. Przykładowa aplikacja pobiera `count` apostrofu, aby usunąć i zwraca `_allQuotesDeletedScope`:

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet12)]

Zawija zakresu wywołuje rozszerzenia rejestrowania [przy użyciu](/dotnet/csharp/language-reference/keywords/using-statement) bloku:

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Pages/Index.cshtml.cs?name=snippet4&highlight=5-6,14)]

Sprawdź, czy komunikaty dziennika w aplikacji konsoli w danych wyjściowych. Następujące wyniki pokazuje trzy cudzysłowy usunięte z komunikatem o zakres dziennika, które zostały dołączone:

```console
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:0000002E RequestPath:/ => /Index => 
          All quotes deleted (Count = 3)
      Quote deleted (Quote = 'Quote 1' Id = 2)
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:0000002E RequestPath:/ => /Index => 
          All quotes deleted (Count = 3)
      Quote deleted (Quote = 'Quote 2' Id = 3)
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:0000002E RequestPath:/ => /Index => 
          All quotes deleted (Count = 3)
      Quote deleted (Quote = 'Quote 3' Id = 4)
```

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Rejestrowanie](xref:fundamentals/logging/index)
