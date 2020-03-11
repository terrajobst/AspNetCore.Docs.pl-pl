---
title: Rejestrowanie o wysokiej wydajności za pomocą LoggerMessage w ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak używać LoggerMessage do tworzenia delegatów z pamięcią podręczną, które wymagają mniejszej liczby alokacji obiektów do scenariuszy rejestrowania o wysokiej wydajności.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/26/2019
uid: fundamentals/logging/loggermessage
ms.openlocfilehash: 48ebba69b5c15a0f9a42f7f6b3d2c1fcb0a2211c
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78663220"
---
# <a name="high-performance-logging-with-loggermessage-in-aspnet-core"></a>Rejestrowanie o wysokiej wydajności za pomocą LoggerMessage w ASP.NET Core

::: moniker range=">= aspnetcore-3.0"

<xref:Microsoft.Extensions.Logging.LoggerMessage> funkcje tworzą delegatów z pamięcią podręczną, którzy wymagają mniejszej liczby alokacji obiektów i zredukowanego obciążenia obliczeniowego w porównaniu z [metodami rozszerzenia rejestratora](xref:Microsoft.Extensions.Logging.LoggerExtensions), takimi jak <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation*> i <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogDebug*>. W przypadku scenariuszy rejestrowania o wysokiej wydajności użyj wzorca <xref:Microsoft.Extensions.Logging.LoggerMessage>.

<xref:Microsoft.Extensions.Logging.LoggerMessage> zapewnia następujące korzyści z wydajności w porównaniu z metodami rozszerzenia rejestratora:

* Metody rozszerzenia rejestratora wymagają "pakowania" (do konwersji) typów wartości, takich jak `int`, na `object`. Wzorzec <xref:Microsoft.Extensions.Logging.LoggerMessage> pozwala uniknąć pakowania przy użyciu statycznych pól <xref:System.Action> i metod rozszerzających z parametrami o jednoznacznie określonym typie.
* Metody rozszerzenia rejestratora muszą analizować szablon wiadomości (nazwanego ciągu formatu) za każdym razem, gdy zostanie zapisany komunikat dziennika. <xref:Microsoft.Extensions.Logging.LoggerMessage> wymaga tylko przeanalizowania szablonu, gdy komunikat jest zdefiniowany.

[Wyświetl lub pobierz przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/logging/loggermessage/samples/) ([jak pobrać](xref:index#how-to-download-a-sample))

Przykładowa aplikacja pokazuje <xref:Microsoft.Extensions.Logging.LoggerMessage> funkcje z systemem podstawowego śledzenia ofert. Aplikacja dodaje i usuwa oferty przy użyciu bazy danych w pamięci. W przypadku wystąpienia tych operacji komunikaty dziennika są generowane przy użyciu wzorca <xref:Microsoft.Extensions.Logging.LoggerMessage>.

## <a name="loggermessagedefine"></a>LoggerMessage.Define

[Zdefiniuj (LogLevel, EventId, String)](xref:Microsoft.Extensions.Logging.LoggerMessage.Define*) tworzy delegata <xref:System.Action> na potrzeby rejestrowania wiadomości. <xref:Microsoft.Extensions.Logging.LoggerMessage.Define*> overloads zezwalają na przekazywanie do sześciu parametrów typu do nazwanego ciągu formatu (szablonu).

Ciąg dostarczony do metody <xref:Microsoft.Extensions.Logging.LoggerMessage.Define*> jest szablonem, a nie ciągiem interpolowanym. Symbole zastępcze są wypełniane w kolejności, w jakiej są określone typy. Nazwy symboli zastępczych w szablonie powinny być opisowe i spójne w szablonach. Służą one jako nazwy właściwości w danych dziennika strukturalnego. Zalecamy używanie [wielkości liter](/dotnet/standard/design-guidelines/capitalization-conventions) w języku Pascal dla nazw zastępczych. Na przykład `{Count}`, `{FirstName}`.

Każdy komunikat dziennika jest <xref:System.Action> przechowywany w polu statycznym utworzonym przez [LoggerMessage. define](xref:Microsoft.Extensions.Logging.LoggerMessage.Define*). Przykładowo Przykładowa aplikacja tworzy pole opisujące komunikat dziennika dla żądania GET dla strony indeksu (*Internal/LoggerExtensions. cs*):

[!code-csharp[](loggermessage/samples/3.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet1)]

Dla <xref:System.Action>Określ:

* Poziom dziennika.
* Unikatowy identyfikator zdarzenia (<xref:Microsoft.Extensions.Logging.EventId>) z nazwą statycznej metody rozszerzenia.
* Szablon wiadomości (nazwany ciąg formatu). 

Żądanie dotyczące strony indeksu przykładowej aplikacji ustawia:

* Poziom rejestrowania do `Information`.
* Identyfikator zdarzenia do `1` z nazwą metody `IndexPageRequested`.
* Szablon wiadomości (nazwany ciąg formatu) do ciągu.

[!code-csharp[](loggermessage/samples/3.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet5)]

Magazyny rejestrowania strukturalnego mogą używać nazwy zdarzenia, gdy jest ona dostarczana z identyfikatorem zdarzenia w celu wzbogacania rejestrowania. Na przykład [Serilog](https://github.com/serilog/serilog-extensions-logging) używa nazwy zdarzenia.

<xref:System.Action> jest wywoływany za pomocą metody rozszerzającej o jednoznacznie określonym typie. Metoda `IndexPageRequested` rejestruje komunikat dotyczący żądania pobrania strony indeksu w przykładowej aplikacji:

[!code-csharp[](loggermessage/samples/3.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet9)]

`IndexPageRequested` jest wywoływana dla rejestratora w metodzie `OnGetAsync` w obszarze *Pages/index. cshtml. cs*:

[!code-csharp[](loggermessage/samples/3.x/LoggerMessageSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3)]

Sprawdź dane wyjściowe konsoli aplikacji:

```console
info: LoggerMessageSample.Pages.IndexModel[1]
      => RequestId:0HL90M6E7PHK4:00000001 RequestPath:/ => /Index
      GET request for Index page
```

Aby przekazać parametry do komunikatu dziennika, zdefiniuj maksymalnie sześć typów podczas tworzenia pola statycznego. Przykładowa aplikacja rejestruje ciąg podczas dodawania oferty przez zdefiniowanie typu `string` dla pola <xref:System.Action>:

[!code-csharp[](loggermessage/samples/3.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet2)]

Szablon komunikatu dziennika delegata otrzymuje wartości zastępcze z dostarczonych typów. Przykładowa aplikacja definiuje delegata do dodawania oferty, w której parametr Quote jest `string`:

[!code-csharp[](loggermessage/samples/3.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet6)]

Metoda rozszerzenia statycznego służąca do dodawania oferty, `QuoteAdded`, odbiera wartość argumentu cudzysłowu i przekazuje ją do delegata <xref:System.Action>:

[!code-csharp[](loggermessage/samples/3.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet10)]

W modelu strony strony indeksu (*Pages/index. cshtml. cs*) `QuoteAdded` jest wywoływana w celu zarejestrowania komunikatu:

[!code-csharp[](loggermessage/samples/3.x/LoggerMessageSample/Pages/Index.cshtml.cs?name=snippet3&highlight=6)]

Sprawdź dane wyjściowe konsoli aplikacji:

```console
info: LoggerMessageSample.Pages.IndexModel[2]
      => RequestId:0HL90M6E7PHK5:0000000A RequestPath:/ => /Index
      Quote added (Quote = 'You can avoid reality, but you cannot avoid the 
          consequences of avoiding reality. - Ayn Rand')
```

Przykładowa aplikacja implementuje wzór [catch try&ndash;](/dotnet/csharp/language-reference/keywords/try-catch) dla usunięcia oferty. Komunikat informacyjny jest rejestrowany w przypadku pomyślnego usunięcia operacji usuwania. Komunikat o błędzie jest rejestrowany dla operacji usuwania, gdy zostanie zgłoszony wyjątek. Komunikat dziennika dla nieprawidłowej operacji usuwania obejmuje ślad stosu wyjątku (*wewnętrzny/LoggerExtensions. cs*):

[!code-csharp[](loggermessage/samples/3.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet3)]

[!code-csharp[](loggermessage/samples/3.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet7)]

Zwróć uwagę, jak wyjątek jest przesyłany do delegata w `QuoteDeleteFailed`:

[!code-csharp[](loggermessage/samples/3.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet11)]

W modelu strony dla strony indeksu pomyślne usunięcie cudzysłowu wywołuje metodę `QuoteDeleted` w rejestratorze. Gdy cytat nie zostanie znaleziony do usunięcia, zostanie zgłoszony <xref:System.ArgumentNullException>. Wyjątek jest zalewkowany przez instrukcję [try&ndash;catch](/dotnet/csharp/language-reference/keywords/try-catch) i rejestrowany przez wywołanie metody `QuoteDeleteFailed` w rejestratorze w bloku [catch](/dotnet/csharp/language-reference/keywords/try-catch) (*Pages/index. cshtml. cs*):

[!code-csharp[](loggermessage/samples/3.x/LoggerMessageSample/Pages/Index.cshtml.cs?name=snippet5&highlight=9,13)]

Po pomyślnym usunięciu oferty Sprawdź dane wyjściowe konsoli aplikacji:

```console
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:00000016 RequestPath:/ => /Index
      Quote deleted (Quote = 'You can avoid reality, but you cannot avoid the 
          consequences of avoiding reality. - Ayn Rand' Id = 1)
```

Po niepowodzeniu usuwania oferty Sprawdź dane wyjściowe konsoli aplikacji. Należy zauważyć, że wyjątek jest zawarty w komunikacie dziennika:

```console
LoggerMessageSample.Pages.IndexModel: Error: Quote delete failed (Id = 999)

System.NullReferenceException: Object reference not set to an instance of an object.
   at lambda_method(Closure , ValueBuffer )
   at System.Linq.Enumerable.SelectEnumerableIterator`2.MoveNext()
   at Microsoft.EntityFrameworkCore.InMemory.Query.Internal.InMemoryShapedQueryCompilingExpressionVisitor.AsyncQueryingEnumerable`1.AsyncEnumerator.MoveNextAsync()
   at Microsoft.EntityFrameworkCore.Query.ShapedQueryCompilingExpressionVisitor.SingleOrDefaultAsync[TSource](IAsyncEnumerable`1 asyncEnumerable, CancellationToken cancellationToken)
   at Microsoft.EntityFrameworkCore.Query.ShapedQueryCompilingExpressionVisitor.SingleOrDefaultAsync[TSource](IAsyncEnumerable`1 asyncEnumerable, CancellationToken cancellationToken)
   at LoggerMessageSample.Pages.IndexModel.OnPostDeleteQuoteAsync(Int32 id) in c:\Users\guard\Documents\GitHub\Docs\aspnetcore\fundamentals\logging\loggermessage\samples\3.x\LoggerMessageSample\Pages\Index.cshtml.cs:line 77
```

## <a name="loggermessagedefinescope"></a>LoggerMessage.DefineScope

[DefineScope — (String)](xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*) tworzy delegata <xref:System.Func%601> do definiowania [zakresu dziennika](xref:fundamentals/logging/index#log-scopes). przeciążanie <xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*> zezwala na przekazywanie maksymalnie trzech parametrów typu do nazwanego ciągu formatu (szablonu).

Podobnie jak w przypadku metody <xref:Microsoft.Extensions.Logging.LoggerMessage.Define*>, ciąg dostarczony do metody <xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*> jest szablonem, a nie ciągiem interpolowanym. Symbole zastępcze są wypełniane w kolejności, w jakiej są określone typy. Nazwy symboli zastępczych w szablonie powinny być opisowe i spójne w szablonach. Służą one jako nazwy właściwości w danych dziennika strukturalnego. Zalecamy używanie [wielkości liter](/dotnet/standard/design-guidelines/capitalization-conventions) w języku Pascal dla nazw zastępczych. Na przykład `{Count}`, `{FirstName}`.

Zdefiniuj [zakres dziennika](xref:fundamentals/logging/index#log-scopes) , który ma zostać zastosowany do serii komunikatów dziennika przy użyciu metody <xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*>.

Przykładowa aplikacja ma przycisk **Wyczyść wszystko** , aby usunąć wszystkie cudzysłowy w bazie danych. Cudzysłowy są usuwane, usuwając je pojedynczo. Za każdym razem, gdy cytat jest usuwany, Metoda `QuoteDeleted` jest wywoływana w rejestratorze. Do tych komunikatów dziennika jest dodawany zakres dziennika.

Włącz `IncludeScopes` w sekcji rejestratora konsoli pliku *appSettings. JSON*:

[!code-csharp[](loggermessage/samples/3.x/LoggerMessageSample/appsettings.json?highlight=3-5)]

Aby utworzyć zakres dziennika, należy dodać pole, aby pomieścić <xref:System.Func%601> delegata dla zakresu. Przykładowa aplikacja tworzy pole o nazwie `_allQuotesDeletedScope` (*wewnętrzne/LoggerExtensions. cs*):

[!code-csharp[](loggermessage/samples/3.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet4)]

Użyj <xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*>, aby utworzyć delegata. Można określić maksymalnie trzy typy do użycia jako argumenty szablonu, gdy obiekt delegowany jest wywoływany. Przykładowa aplikacja używa szablonu komunikatu zawierającego liczbę usuniętych cudzysłowów (typ `int`):

[!code-csharp[](loggermessage/samples/3.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet8)]

Podaj statyczną metodę rozszerzenia dla komunikatu dziennika. Dołącz wszystkie parametry typu dla nazwanych właściwości, które są wyświetlane w szablonie wiadomości. Przykładowa aplikacja wykonuje `count` cudzysłowy, aby usunąć i zwracać `_allQuotesDeletedScope`:

[!code-csharp[](loggermessage/samples/3.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet12)]

Zakres zawija wywołania rozszerzenia rejestrowania w bloku [using](/dotnet/csharp/language-reference/keywords/using-statement) :

[!code-csharp[](loggermessage/samples/3.x/LoggerMessageSample/Pages/Index.cshtml.cs?name=snippet4&highlight=5-6,14)]

Sprawdź komunikaty dziennika w danych wyjściowych konsoli aplikacji. Poniższy wynik pokazuje trzy cudzysłowy usunięte z uwzględnieniem komunikatu o zakresie dziennika:

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

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<xref:Microsoft.Extensions.Logging.LoggerMessage> funkcje tworzą delegatów z pamięcią podręczną, którzy wymagają mniejszej liczby alokacji obiektów i zredukowanego obciążenia obliczeniowego w porównaniu z [metodami rozszerzenia rejestratora](xref:Microsoft.Extensions.Logging.LoggerExtensions), takimi jak <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation*> i <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogDebug*>. W przypadku scenariuszy rejestrowania o wysokiej wydajności użyj wzorca <xref:Microsoft.Extensions.Logging.LoggerMessage>.

<xref:Microsoft.Extensions.Logging.LoggerMessage> zapewnia następujące korzyści z wydajności w porównaniu z metodami rozszerzenia rejestratora:

* Metody rozszerzenia rejestratora wymagają "pakowania" (do konwersji) typów wartości, takich jak `int`, na `object`. Wzorzec <xref:Microsoft.Extensions.Logging.LoggerMessage> pozwala uniknąć pakowania przy użyciu statycznych pól <xref:System.Action> i metod rozszerzających z parametrami o jednoznacznie określonym typie.
* Metody rozszerzenia rejestratora muszą analizować szablon wiadomości (nazwanego ciągu formatu) za każdym razem, gdy zostanie zapisany komunikat dziennika. <xref:Microsoft.Extensions.Logging.LoggerMessage> wymaga tylko przeanalizowania szablonu, gdy komunikat jest zdefiniowany.

[Wyświetl lub pobierz przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/logging/loggermessage/samples/) ([jak pobrać](xref:index#how-to-download-a-sample))

Przykładowa aplikacja pokazuje <xref:Microsoft.Extensions.Logging.LoggerMessage> funkcje z systemem podstawowego śledzenia ofert. Aplikacja dodaje i usuwa oferty przy użyciu bazy danych w pamięci. W przypadku wystąpienia tych operacji komunikaty dziennika są generowane przy użyciu wzorca <xref:Microsoft.Extensions.Logging.LoggerMessage>.

## <a name="loggermessagedefine"></a>LoggerMessage.Define

[Zdefiniuj (LogLevel, EventId, String)](xref:Microsoft.Extensions.Logging.LoggerMessage.Define*) tworzy delegata <xref:System.Action> na potrzeby rejestrowania wiadomości. <xref:Microsoft.Extensions.Logging.LoggerMessage.Define*> overloads zezwalają na przekazywanie do sześciu parametrów typu do nazwanego ciągu formatu (szablonu).

Ciąg dostarczony do metody <xref:Microsoft.Extensions.Logging.LoggerMessage.Define*> jest szablonem, a nie ciągiem interpolowanym. Symbole zastępcze są wypełniane w kolejności, w jakiej są określone typy. Nazwy symboli zastępczych w szablonie powinny być opisowe i spójne w szablonach. Służą one jako nazwy właściwości w danych dziennika strukturalnego. Zalecamy używanie [wielkości liter](/dotnet/standard/design-guidelines/capitalization-conventions) w języku Pascal dla nazw zastępczych. Na przykład `{Count}`, `{FirstName}`.

Każdy komunikat dziennika jest <xref:System.Action> przechowywany w polu statycznym utworzonym przez [LoggerMessage. define](xref:Microsoft.Extensions.Logging.LoggerMessage.Define*). Przykładowo Przykładowa aplikacja tworzy pole opisujące komunikat dziennika dla żądania GET dla strony indeksu (*Internal/LoggerExtensions. cs*):

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet1)]

Dla <xref:System.Action>Określ:

* Poziom dziennika.
* Unikatowy identyfikator zdarzenia (<xref:Microsoft.Extensions.Logging.EventId>) z nazwą statycznej metody rozszerzenia.
* Szablon wiadomości (nazwany ciąg formatu). 

Żądanie dotyczące strony indeksu przykładowej aplikacji ustawia:

* Poziom rejestrowania do `Information`.
* Identyfikator zdarzenia do `1` z nazwą metody `IndexPageRequested`.
* Szablon wiadomości (nazwany ciąg formatu) do ciągu.

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet5)]

Magazyny rejestrowania strukturalnego mogą używać nazwy zdarzenia, gdy jest ona dostarczana z identyfikatorem zdarzenia w celu wzbogacania rejestrowania. Na przykład [Serilog](https://github.com/serilog/serilog-extensions-logging) używa nazwy zdarzenia.

<xref:System.Action> jest wywoływany za pomocą metody rozszerzającej o jednoznacznie określonym typie. Metoda `IndexPageRequested` rejestruje komunikat dotyczący żądania pobrania strony indeksu w przykładowej aplikacji:

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet9)]

`IndexPageRequested` jest wywoływana dla rejestratora w metodzie `OnGetAsync` w obszarze *Pages/index. cshtml. cs*:

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3)]

Sprawdź dane wyjściowe konsoli aplikacji:

```console
info: LoggerMessageSample.Pages.IndexModel[1]
      => RequestId:0HL90M6E7PHK4:00000001 RequestPath:/ => /Index
      GET request for Index page
```

Aby przekazać parametry do komunikatu dziennika, zdefiniuj maksymalnie sześć typów podczas tworzenia pola statycznego. Przykładowa aplikacja rejestruje ciąg podczas dodawania oferty przez zdefiniowanie typu `string` dla pola <xref:System.Action>:

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet2)]

Szablon komunikatu dziennika delegata otrzymuje wartości zastępcze z dostarczonych typów. Przykładowa aplikacja definiuje delegata do dodawania oferty, w której parametr Quote jest `string`:

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet6)]

Metoda rozszerzenia statycznego służąca do dodawania oferty, `QuoteAdded`, odbiera wartość argumentu cudzysłowu i przekazuje ją do delegata <xref:System.Action>:

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet10)]

W modelu strony strony indeksu (*Pages/index. cshtml. cs*) `QuoteAdded` jest wywoływana w celu zarejestrowania komunikatu:

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Pages/Index.cshtml.cs?name=snippet3&highlight=6)]

Sprawdź dane wyjściowe konsoli aplikacji:

```console
info: LoggerMessageSample.Pages.IndexModel[2]
      => RequestId:0HL90M6E7PHK5:0000000A RequestPath:/ => /Index
      Quote added (Quote = 'You can avoid reality, but you cannot avoid the 
          consequences of avoiding reality. - Ayn Rand')
```

Przykładowa aplikacja implementuje wzór [catch try&ndash;](/dotnet/csharp/language-reference/keywords/try-catch) dla usunięcia oferty. Komunikat informacyjny jest rejestrowany w przypadku pomyślnego usunięcia operacji usuwania. Komunikat o błędzie jest rejestrowany dla operacji usuwania, gdy zostanie zgłoszony wyjątek. Komunikat dziennika dla nieprawidłowej operacji usuwania obejmuje ślad stosu wyjątku (*wewnętrzny/LoggerExtensions. cs*):

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet3)]

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet7)]

Zwróć uwagę, jak wyjątek jest przesyłany do delegata w `QuoteDeleteFailed`:

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet11)]

W modelu strony dla strony indeksu pomyślne usunięcie cudzysłowu wywołuje metodę `QuoteDeleted` w rejestratorze. Gdy cytat nie zostanie znaleziony do usunięcia, zostanie zgłoszony <xref:System.ArgumentNullException>. Wyjątek jest zalewkowany przez instrukcję [try&ndash;catch](/dotnet/csharp/language-reference/keywords/try-catch) i rejestrowany przez wywołanie metody `QuoteDeleteFailed` w rejestratorze w bloku [catch](/dotnet/csharp/language-reference/keywords/try-catch) (*Pages/index. cshtml. cs*):

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Pages/Index.cshtml.cs?name=snippet5&highlight=14,18)]

Po pomyślnym usunięciu oferty Sprawdź dane wyjściowe konsoli aplikacji:

```console
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:00000016 RequestPath:/ => /Index
      Quote deleted (Quote = 'You can avoid reality, but you cannot avoid the 
          consequences of avoiding reality. - Ayn Rand' Id = 1)
```

Po niepowodzeniu usuwania oferty Sprawdź dane wyjściowe konsoli aplikacji. Należy zauważyć, że wyjątek jest zawarty w komunikacie dziennika:

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

[DefineScope — (String)](xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*) tworzy delegata <xref:System.Func%601> do definiowania [zakresu dziennika](xref:fundamentals/logging/index#log-scopes). przeciążanie <xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*> zezwala na przekazywanie maksymalnie trzech parametrów typu do nazwanego ciągu formatu (szablonu).

Podobnie jak w przypadku metody <xref:Microsoft.Extensions.Logging.LoggerMessage.Define*>, ciąg dostarczony do metody <xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*> jest szablonem, a nie ciągiem interpolowanym. Symbole zastępcze są wypełniane w kolejności, w jakiej są określone typy. Nazwy symboli zastępczych w szablonie powinny być opisowe i spójne w szablonach. Służą one jako nazwy właściwości w danych dziennika strukturalnego. Zalecamy używanie [wielkości liter](/dotnet/standard/design-guidelines/capitalization-conventions) w języku Pascal dla nazw zastępczych. Na przykład `{Count}`, `{FirstName}`.

Zdefiniuj [zakres dziennika](xref:fundamentals/logging/index#log-scopes) , który ma zostać zastosowany do serii komunikatów dziennika przy użyciu metody <xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*>.

Przykładowa aplikacja ma przycisk **Wyczyść wszystko** , aby usunąć wszystkie cudzysłowy w bazie danych. Cudzysłowy są usuwane, usuwając je pojedynczo. Za każdym razem, gdy cytat jest usuwany, Metoda `QuoteDeleted` jest wywoływana w rejestratorze. Do tych komunikatów dziennika jest dodawany zakres dziennika.

Włącz `IncludeScopes` w sekcji rejestratora konsoli pliku *appSettings. JSON*:

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/appsettings.json?highlight=3-5)]

Aby utworzyć zakres dziennika, należy dodać pole, aby pomieścić <xref:System.Func%601> delegata dla zakresu. Przykładowa aplikacja tworzy pole o nazwie `_allQuotesDeletedScope` (*wewnętrzne/LoggerExtensions. cs*):

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet4)]

Użyj <xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*>, aby utworzyć delegata. Można określić maksymalnie trzy typy do użycia jako argumenty szablonu, gdy obiekt delegowany jest wywoływany. Przykładowa aplikacja używa szablonu komunikatu zawierającego liczbę usuniętych cudzysłowów (typ `int`):

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet8)]

Podaj statyczną metodę rozszerzenia dla komunikatu dziennika. Dołącz wszystkie parametry typu dla nazwanych właściwości, które są wyświetlane w szablonie wiadomości. Przykładowa aplikacja wykonuje `count` cudzysłowy, aby usunąć i zwracać `_allQuotesDeletedScope`:

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet12)]

Zakres zawija wywołania rozszerzenia rejestrowania w bloku [using](/dotnet/csharp/language-reference/keywords/using-statement) :

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Pages/Index.cshtml.cs?name=snippet4&highlight=5-6,14)]

Sprawdź komunikaty dziennika w danych wyjściowych konsoli aplikacji. Poniższy wynik pokazuje trzy cudzysłowy usunięte z uwzględnieniem komunikatu o zakresie dziennika:

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

::: moniker-end

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Rejestrowanie](xref:fundamentals/logging/index)
