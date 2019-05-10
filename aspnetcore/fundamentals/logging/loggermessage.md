---
title: Rejestrowanie o wysokiej wydajności, za pomocą funkcji LoggerMessage w programie ASP.NET Core
author: guardrex
description: Dowiedz się, jak utworzyć podlega buforowaniu obiektów delegowanych, które wymagają mniejszej liczby alokacji obiektów w scenariuszach logowania o wysokiej wydajności przy za pomocą funkcji LoggerMessage.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/24/2019
uid: fundamentals/logging/loggermessage
ms.openlocfilehash: a82dfa36330e987f03f576d0f80198e3bad7b429
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65085599"
---
# <a name="high-performance-logging-with-loggermessage-in-aspnet-core"></a><span data-ttu-id="3780b-103">Rejestrowanie o wysokiej wydajności, za pomocą funkcji LoggerMessage w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3780b-103">High-performance logging with LoggerMessage in ASP.NET Core</span></span>

<span data-ttu-id="3780b-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="3780b-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="3780b-105"><xref:Microsoft.Extensions.Logging.LoggerMessage> funkcje tworzenia podlega buforowaniu obiektów delegowanych, które wymagają mniejszej liczby alokacji obiektów i mniejsze obciążenie obliczeniowe w porównaniu do [metody rozszerzenia rejestratora](xref:Microsoft.Extensions.Logging.LoggerExtensions), takich jak <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation*> i <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogDebug*>.</span><span class="sxs-lookup"><span data-stu-id="3780b-105"><xref:Microsoft.Extensions.Logging.LoggerMessage> features create cacheable delegates that require fewer object allocations and reduced computational overhead compared to [logger extension methods](xref:Microsoft.Extensions.Logging.LoggerExtensions), such as <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation*> and <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogDebug*>.</span></span> <span data-ttu-id="3780b-106">W scenariuszach logowania o wysokiej wydajności, należy użyć <xref:Microsoft.Extensions.Logging.LoggerMessage> wzorca.</span><span class="sxs-lookup"><span data-stu-id="3780b-106">For high-performance logging scenarios, use the <xref:Microsoft.Extensions.Logging.LoggerMessage> pattern.</span></span>

<span data-ttu-id="3780b-107"><xref:Microsoft.Extensions.Logging.LoggerMessage> zapewnia następujące korzyści wydajności za pośrednictwem metody rozszerzenia rejestratora:</span><span class="sxs-lookup"><span data-stu-id="3780b-107"><xref:Microsoft.Extensions.Logging.LoggerMessage> provides the following performance advantages over Logger extension methods:</span></span>

* <span data-ttu-id="3780b-108">Metody rozszerzenia rejestratora wymagają typów wartości "opakowanie" (konwertowanie), takich jak `int`, do `object`.</span><span class="sxs-lookup"><span data-stu-id="3780b-108">Logger extension methods require "boxing" (converting) value types, such as `int`, into `object`.</span></span> <span data-ttu-id="3780b-109"><xref:Microsoft.Extensions.Logging.LoggerMessage> Wzorzec pozwala uniknąć pakowania przy użyciu statycznej <xref:System.Action> pola i metody rozszerzające o silnie typizowane parametry.</span><span class="sxs-lookup"><span data-stu-id="3780b-109">The <xref:Microsoft.Extensions.Logging.LoggerMessage> pattern avoids boxing by using static <xref:System.Action> fields and extension methods with strongly-typed parameters.</span></span>
* <span data-ttu-id="3780b-110">Metody rozszerzenia rejestratora musi przeanalizować szablon wiadomości (ciąg formatu o nazwie), każdym razem, gdy zostanie napisany komunikat dziennika.</span><span class="sxs-lookup"><span data-stu-id="3780b-110">Logger extension methods must parse the message template (named format string) every time a log message is written.</span></span> <span data-ttu-id="3780b-111"><xref:Microsoft.Extensions.Logging.LoggerMessage> wymaga tylko raz podczas analizowania szablonu, gdy komunikat jest definiowany.</span><span class="sxs-lookup"><span data-stu-id="3780b-111"><xref:Microsoft.Extensions.Logging.LoggerMessage> only requires parsing a template once when the message is defined.</span></span>

<span data-ttu-id="3780b-112">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/logging/loggermessage/samples/2.x/LoggerMessageSample) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3780b-112">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/logging/loggermessage/samples/2.x/LoggerMessageSample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="3780b-113">Przykładowa aplikacja pokazuje <xref:Microsoft.Extensions.Logging.LoggerMessage> funkcji podstawowych oferty systemu śledzenia.</span><span class="sxs-lookup"><span data-stu-id="3780b-113">The sample app demonstrates <xref:Microsoft.Extensions.Logging.LoggerMessage> features with a basic quote tracking system.</span></span> <span data-ttu-id="3780b-114">Aplikacja dodaje i usuwa przy użyciu bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="3780b-114">The app adds and deletes quotes using an in-memory database.</span></span> <span data-ttu-id="3780b-115">Ponieważ te operacje są wykonywane, komunikaty dziennika są generowane przy użyciu <xref:Microsoft.Extensions.Logging.LoggerMessage> wzorca.</span><span class="sxs-lookup"><span data-stu-id="3780b-115">As these operations occur, log messages are generated using the <xref:Microsoft.Extensions.Logging.LoggerMessage> pattern.</span></span>

## <a name="loggermessagedefine"></a><span data-ttu-id="3780b-116">LoggerMessage.Define</span><span class="sxs-lookup"><span data-stu-id="3780b-116">LoggerMessage.Define</span></span>

<span data-ttu-id="3780b-117">[Zdefiniuj (LogLevel, identyfikator zdarzenia, String)](xref:Microsoft.Extensions.Logging.LoggerMessage.Define*) tworzy <xref:System.Action> delegowanie do rejestrowania wiadomości.</span><span class="sxs-lookup"><span data-stu-id="3780b-117">[Define(LogLevel, EventId, String)](xref:Microsoft.Extensions.Logging.LoggerMessage.Define*) creates an <xref:System.Action> delegate for logging a message.</span></span> <span data-ttu-id="3780b-118"><xref:Microsoft.Extensions.Logging.LoggerMessage.Define*> przeciążenia zezwalają na przekazywanie parametrów typu do sześciu ciągowi formatu o nazwie (szablon).</span><span class="sxs-lookup"><span data-stu-id="3780b-118"><xref:Microsoft.Extensions.Logging.LoggerMessage.Define*> overloads permit passing up to six type parameters to a named format string (template).</span></span>

<span data-ttu-id="3780b-119">Ciąg do <xref:Microsoft.Extensions.Logging.LoggerMessage.Define*> metoda jest szablon i nie ciągu interpolowanym.</span><span class="sxs-lookup"><span data-stu-id="3780b-119">The string provided to the <xref:Microsoft.Extensions.Logging.LoggerMessage.Define*> method is a template and not an interpolated string.</span></span> <span data-ttu-id="3780b-120">Symbole zastępcze są wypełnione w kolejności, że typy są określone.</span><span class="sxs-lookup"><span data-stu-id="3780b-120">Placeholders are filled in the order that the types are specified.</span></span> <span data-ttu-id="3780b-121">Nazwy symbolu zastępczego w szablonie powinny być opisowe i spójny, różnych szablonów.</span><span class="sxs-lookup"><span data-stu-id="3780b-121">Placeholder names in the template should be descriptive and consistent across templates.</span></span> <span data-ttu-id="3780b-122">Służą jako nazwy właściwości ze strukturą danych dziennika.</span><span class="sxs-lookup"><span data-stu-id="3780b-122">They serve as property names within structured log data.</span></span> <span data-ttu-id="3780b-123">Firma Microsoft zaleca [Pascal wielkość liter w wyrazie](/dotnet/standard/design-guidelines/capitalization-conventions) dla nazw zastępczych.</span><span class="sxs-lookup"><span data-stu-id="3780b-123">We recommend [Pascal casing](/dotnet/standard/design-guidelines/capitalization-conventions) for placeholder names.</span></span> <span data-ttu-id="3780b-124">Na przykład `{Count}`, `{FirstName}`.</span><span class="sxs-lookup"><span data-stu-id="3780b-124">For example, `{Count}`, `{FirstName}`.</span></span>

<span data-ttu-id="3780b-125">Każdy komunikat dziennika jest <xref:System.Action> znajdujące się w polu statycznym, które zostały utworzone przez [LoggerMessage.Define](xref:Microsoft.Extensions.Logging.LoggerMessage.Define*).</span><span class="sxs-lookup"><span data-stu-id="3780b-125">Each log message is an <xref:System.Action> held in a static field created by [LoggerMessage.Define](xref:Microsoft.Extensions.Logging.LoggerMessage.Define*).</span></span> <span data-ttu-id="3780b-126">Na przykład przykładowa aplikacja tworzy pola do opisania komunikat dziennika dla żądania GET strony indeksu (*Internal/LoggerExtensions.cs*):</span><span class="sxs-lookup"><span data-stu-id="3780b-126">For example, the sample app creates a field to describe a log message for a GET request for the Index page (*Internal/LoggerExtensions.cs*):</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet1)]

<span data-ttu-id="3780b-127">Aby uzyskać <xref:System.Action>, określ:</span><span class="sxs-lookup"><span data-stu-id="3780b-127">For the <xref:System.Action>, specify:</span></span>

* <span data-ttu-id="3780b-128">Poziom dziennika.</span><span class="sxs-lookup"><span data-stu-id="3780b-128">The log level.</span></span>
* <span data-ttu-id="3780b-129">Identyfikatora unikatowego wydarzenia (<xref:Microsoft.Extensions.Logging.EventId>) o nazwie metody statyczne rozszerzenie.</span><span class="sxs-lookup"><span data-stu-id="3780b-129">A unique event identifier (<xref:Microsoft.Extensions.Logging.EventId>) with the name of the static extension method.</span></span>
* <span data-ttu-id="3780b-130">Szablon wiadomości (o nazwie ciąg formatu).</span><span class="sxs-lookup"><span data-stu-id="3780b-130">The message template (named format string).</span></span> 

<span data-ttu-id="3780b-131">Żądanie strony indeksu zestawów aplikacji przykładowej:</span><span class="sxs-lookup"><span data-stu-id="3780b-131">A request for the Index page of the sample app sets the:</span></span>

* <span data-ttu-id="3780b-132">Poziom do dziennika `Information`.</span><span class="sxs-lookup"><span data-stu-id="3780b-132">Log level to `Information`.</span></span>
* <span data-ttu-id="3780b-133">Identyfikator zdarzenia do `1` o nazwie `IndexPageRequested` metody.</span><span class="sxs-lookup"><span data-stu-id="3780b-133">Event id to `1` with the name of the `IndexPageRequested` method.</span></span>
* <span data-ttu-id="3780b-134">Szablon wiadomości (o nazwie ciąg formatu) na ciąg.</span><span class="sxs-lookup"><span data-stu-id="3780b-134">Message template (named format string) to a string.</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet5)]

<span data-ttu-id="3780b-135">Magazyny rejestrowaniem strukturalnym może używać nazwy zdarzenia, gdy są dostarczane z identyfikatorem zdarzenia w celu wzbogacenia rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="3780b-135">Structured logging stores may use the event name when it's supplied with the event id to enrich logging.</span></span> <span data-ttu-id="3780b-136">Na przykład [Serilog](https://github.com/serilog/serilog-extensions-logging) używa nazwy zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="3780b-136">For example, [Serilog](https://github.com/serilog/serilog-extensions-logging) uses the event name.</span></span>

<span data-ttu-id="3780b-137"><xref:System.Action> Jest wywoływana za pomocą metody rozszerzenia silnie typizowane.</span><span class="sxs-lookup"><span data-stu-id="3780b-137">The <xref:System.Action> is invoked through a strongly-typed extension method.</span></span> <span data-ttu-id="3780b-138">`IndexPageRequested` Metoda rejestruje komunikat dla żądania GET strony indeksu w przykładowej aplikacji:</span><span class="sxs-lookup"><span data-stu-id="3780b-138">The `IndexPageRequested` method logs a message for an Index page GET request in the sample app:</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet9)]

<span data-ttu-id="3780b-139">`IndexPageRequested` jest wywoływana w rejestratora w `OnGetAsync` method in Class metoda *Pages/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="3780b-139">`IndexPageRequested` is called on the logger in the `OnGetAsync` method in *Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3)]

<span data-ttu-id="3780b-140">Sprawdź dane wyjściowe konsoli z aplikacji:</span><span class="sxs-lookup"><span data-stu-id="3780b-140">Inspect the app's console output:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[1]
      => RequestId:0HL90M6E7PHK4:00000001 RequestPath:/ => /Index
      GET request for Index page
```

<span data-ttu-id="3780b-141">Do przekazania parametrów do komunikatu dziennika, należy zdefiniować maksymalnie sześć typów, tworząc pole statyczne.</span><span class="sxs-lookup"><span data-stu-id="3780b-141">To pass parameters to a log message, define up to six types when creating the static field.</span></span> <span data-ttu-id="3780b-142">Przykładowa aplikacja rejestruje ciągu podczas dodawania oferty, definiując `string` wpisz <xref:System.Action> pola:</span><span class="sxs-lookup"><span data-stu-id="3780b-142">The sample app logs a string when adding a quote by defining a `string` type for the <xref:System.Action> field:</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet2)]

<span data-ttu-id="3780b-143">Szablon wiadomości dziennika pełnomocnika odbiera jego wartości symboli zastępczych z typów dostarczonych.</span><span class="sxs-lookup"><span data-stu-id="3780b-143">The delegate's log message template receives its placeholder values from the types provided.</span></span> <span data-ttu-id="3780b-144">Przykładowa aplikacja definiuje pełnomocnika dodawania oferty, gdzie parametr oferty jest `string`:</span><span class="sxs-lookup"><span data-stu-id="3780b-144">The sample app defines a delegate for adding a quote where the quote parameter is a `string`:</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet6)]

<span data-ttu-id="3780b-145">Metody statyczne rozszerzenie dodawania oferty, `QuoteAdded`, otrzymuje wartość argumentu oferty i przekazuje go do <xref:System.Action> delegować:</span><span class="sxs-lookup"><span data-stu-id="3780b-145">The static extension method for adding a quote, `QuoteAdded`, receives the quote argument value and passes it to the <xref:System.Action> delegate:</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet10)]

<span data-ttu-id="3780b-146">W modelu strony strony indeksu (*Pages/Index.cshtml.cs*), `QuoteAdded` nazywa się komunikat:</span><span class="sxs-lookup"><span data-stu-id="3780b-146">In the Index page's page model (*Pages/Index.cshtml.cs*), `QuoteAdded` is called to log the message:</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Pages/Index.cshtml.cs?name=snippet3&highlight=6)]

<span data-ttu-id="3780b-147">Sprawdź dane wyjściowe konsoli z aplikacji:</span><span class="sxs-lookup"><span data-stu-id="3780b-147">Inspect the app's console output:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[2]
      => RequestId:0HL90M6E7PHK5:0000000A RequestPath:/ => /Index
      Quote added (Quote = 'You can avoid reality, but you cannot avoid the 
          consequences of avoiding reality. - Ayn Rand')
```

<span data-ttu-id="3780b-148">Implementuje aplikacji przykładowej [spróbuj&ndash;catch](/dotnet/csharp/language-reference/keywords/try-catch) wzorzec do usunięcia oferty.</span><span class="sxs-lookup"><span data-stu-id="3780b-148">The sample app implements a [try&ndash;catch](/dotnet/csharp/language-reference/keywords/try-catch) pattern for quote deletion.</span></span> <span data-ttu-id="3780b-149">Komunikat informacyjny jest rejestrowane dla operacji usuwania pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="3780b-149">An informational message is logged for a successful delete operation.</span></span> <span data-ttu-id="3780b-150">Komunikat o błędzie jest rejestrowane dla operacji usuwania, gdy zostanie zgłoszony wyjątek.</span><span class="sxs-lookup"><span data-stu-id="3780b-150">An error message is logged for a delete operation when an exception is thrown.</span></span> <span data-ttu-id="3780b-151">Komunikat dziennika powiodła się operacja usuwania obejmuje ślad stosu wyjątku (*Internal/LoggerExtensions.cs*):</span><span class="sxs-lookup"><span data-stu-id="3780b-151">The log message for the unsuccessful delete operation includes the exception stack trace (*Internal/LoggerExtensions.cs*):</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet3)]

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet7)]

<span data-ttu-id="3780b-152">Należy zauważyć, jak wyjątku jest przekazywany do obiektu delegowanego w `QuoteDeleteFailed`:</span><span class="sxs-lookup"><span data-stu-id="3780b-152">Note how the exception is passed to the delegate in `QuoteDeleteFailed`:</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet11)]

<span data-ttu-id="3780b-153">W modelu strony dla strony indeksu, usunięcie oferty pomyślnego wywołania `QuoteDeleted` metody rejestratora.</span><span class="sxs-lookup"><span data-stu-id="3780b-153">In the page model for the Index page, a successful quote deletion calls the `QuoteDeleted` method on the logger.</span></span> <span data-ttu-id="3780b-154">Gdy oferta nie zostanie odnaleziony do usunięcia, <xref:System.ArgumentNullException> zgłaszany.</span><span class="sxs-lookup"><span data-stu-id="3780b-154">When a quote isn't found for deletion, an <xref:System.ArgumentNullException> is thrown.</span></span> <span data-ttu-id="3780b-155">Wyjątkiem jest to spowodowane [spróbuj&ndash;catch](/dotnet/csharp/language-reference/keywords/try-catch) instrukcji i rejestrowane przez wywołanie metody `QuoteDeleteFailed` metody rejestratora w [catch](/dotnet/csharp/language-reference/keywords/try-catch) bloku (*Pages/Index.cshtml.cs* ):</span><span class="sxs-lookup"><span data-stu-id="3780b-155">The exception is trapped by the [try&ndash;catch](/dotnet/csharp/language-reference/keywords/try-catch) statement and logged by calling the `QuoteDeleteFailed` method on the logger in the [catch](/dotnet/csharp/language-reference/keywords/try-catch) block (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Pages/Index.cshtml.cs?name=snippet5&highlight=14,18)]

<span data-ttu-id="3780b-156">Po pomyślnym usunięciu oferty, sprawdź dane wyjściowe konsoli z aplikacji:</span><span class="sxs-lookup"><span data-stu-id="3780b-156">When a quote is successfully deleted, inspect the app's console output:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:00000016 RequestPath:/ => /Index
      Quote deleted (Quote = 'You can avoid reality, but you cannot avoid the 
          consequences of avoiding reality. - Ayn Rand' Id = 1)
```

<span data-ttu-id="3780b-157">Po usunięciu oferty nie powiedzie się, sprawdź dane wyjściowe konsoli z aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3780b-157">When quote deletion fails, inspect the app's console output.</span></span> <span data-ttu-id="3780b-158">Należy zwrócić uwagę na to, czy wyjątek znajduje się w komunikatu dziennika:</span><span class="sxs-lookup"><span data-stu-id="3780b-158">Note that the exception is included in the log message:</span></span>

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

## <a name="loggermessagedefinescope"></a><span data-ttu-id="3780b-159">LoggerMessage.DefineScope</span><span class="sxs-lookup"><span data-stu-id="3780b-159">LoggerMessage.DefineScope</span></span>

<span data-ttu-id="3780b-160">[DefineScope(String)](xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*) tworzy <xref:System.Func%601> delegować do definiowania [dziennika zakres](xref:fundamentals/logging/index#log-scopes).</span><span class="sxs-lookup"><span data-stu-id="3780b-160">[DefineScope(String)](xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*) creates a <xref:System.Func%601> delegate for defining a [log scope](xref:fundamentals/logging/index#log-scopes).</span></span> <span data-ttu-id="3780b-161"><xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*> przeciążenia umożliwić przekazywanie maksymalnie trzy parametry typu ciągowi formatu o nazwie (szablonu).</span><span class="sxs-lookup"><span data-stu-id="3780b-161"><xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*> overloads permit passing up to three type parameters to a named format string (template).</span></span>

<span data-ttu-id="3780b-162">Jak w przypadku <xref:Microsoft.Extensions.Logging.LoggerMessage.Define*> metody, parametry udostępnione firmie <xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*> metoda jest szablon i nie ciągu interpolowanym.</span><span class="sxs-lookup"><span data-stu-id="3780b-162">As is the case with the <xref:Microsoft.Extensions.Logging.LoggerMessage.Define*> method, the string provided to the <xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*> method is a template and not an interpolated string.</span></span> <span data-ttu-id="3780b-163">Symbole zastępcze są wypełnione w kolejności, że typy są określone.</span><span class="sxs-lookup"><span data-stu-id="3780b-163">Placeholders are filled in the order that the types are specified.</span></span> <span data-ttu-id="3780b-164">Nazwy symbolu zastępczego w szablonie powinny być opisowe i spójny, różnych szablonów.</span><span class="sxs-lookup"><span data-stu-id="3780b-164">Placeholder names in the template should be descriptive and consistent across templates.</span></span> <span data-ttu-id="3780b-165">Służą jako nazwy właściwości ze strukturą danych dziennika.</span><span class="sxs-lookup"><span data-stu-id="3780b-165">They serve as property names within structured log data.</span></span> <span data-ttu-id="3780b-166">Firma Microsoft zaleca [Pascal wielkość liter w wyrazie](/dotnet/standard/design-guidelines/capitalization-conventions) dla nazw zastępczych.</span><span class="sxs-lookup"><span data-stu-id="3780b-166">We recommend [Pascal casing](/dotnet/standard/design-guidelines/capitalization-conventions) for placeholder names.</span></span> <span data-ttu-id="3780b-167">Na przykład `{Count}`, `{FirstName}`.</span><span class="sxs-lookup"><span data-stu-id="3780b-167">For example, `{Count}`, `{FirstName}`.</span></span>

<span data-ttu-id="3780b-168">Zdefiniuj [dziennika zakres](xref:fundamentals/logging/index#log-scopes) do zastosowania na serię komunikatów dziennika przy użyciu <xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*> metody.</span><span class="sxs-lookup"><span data-stu-id="3780b-168">Define a [log scope](xref:fundamentals/logging/index#log-scopes) to apply to a series of log messages using the <xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*> method.</span></span>

<span data-ttu-id="3780b-169">Przykładowa aplikacja ma **Wyczyść wszystko** przycisk usuwania wszystkich ofert w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="3780b-169">The sample app has a **Clear All** button for deleting all of the quotes in the database.</span></span> <span data-ttu-id="3780b-170">Cudzysłowy zostaną usunięte, usuwając jeden na raz.</span><span class="sxs-lookup"><span data-stu-id="3780b-170">The quotes are deleted by removing them one at a time.</span></span> <span data-ttu-id="3780b-171">Każdorazowo po usunięciu oferty `QuoteDeleted` metoda jest wywoływana w rejestratora.</span><span class="sxs-lookup"><span data-stu-id="3780b-171">Each time a quote is deleted, the `QuoteDeleted` method is called on the logger.</span></span> <span data-ttu-id="3780b-172">Zakres dziennika jest dodawany do tych komunikatów dziennika.</span><span class="sxs-lookup"><span data-stu-id="3780b-172">A log scope is added to these log messages.</span></span>

<span data-ttu-id="3780b-173">Włącz `IncludeScopes` w sekcji rejestratora konsoli *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="3780b-173">Enable `IncludeScopes` in the console logger section of *appsettings.json*:</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/appsettings.json?highlight=3-5)]

<span data-ttu-id="3780b-174">Aby utworzyć zakres dziennika, należy dodać pole do przechowywania <xref:System.Func%601> delegowanie dla zakresu.</span><span class="sxs-lookup"><span data-stu-id="3780b-174">To create a log scope, add a field to hold a <xref:System.Func%601> delegate for the scope.</span></span> <span data-ttu-id="3780b-175">Przykładowa aplikacja tworzy pole o nazwie `_allQuotesDeletedScope` (*Internal/LoggerExtensions.cs*):</span><span class="sxs-lookup"><span data-stu-id="3780b-175">The sample app creates a field called `_allQuotesDeletedScope` (*Internal/LoggerExtensions.cs*):</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet4)]

<span data-ttu-id="3780b-176">Użyj <xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*> do utworzenia delegata.</span><span class="sxs-lookup"><span data-stu-id="3780b-176">Use <xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*> to create the delegate.</span></span> <span data-ttu-id="3780b-177">Maksymalnie trzy typy można określić do użycia jako argumenty szablonu, gdy obiekt delegowany jest wywoływany.</span><span class="sxs-lookup"><span data-stu-id="3780b-177">Up to three types can be specified for use as template arguments when the delegate is invoked.</span></span> <span data-ttu-id="3780b-178">Przykładowa aplikacja korzysta z szablonu komunikatu, który zawiera liczba usuniętych ofert ( `int` typu):</span><span class="sxs-lookup"><span data-stu-id="3780b-178">The sample app uses a message template that includes the number of deleted quotes (an `int` type):</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet8)]

<span data-ttu-id="3780b-179">Udostępnia metody statyczne rozszerzenie komunikatu dziennika.</span><span class="sxs-lookup"><span data-stu-id="3780b-179">Provide a static extension method for the log message.</span></span> <span data-ttu-id="3780b-180">Uwzględnić inne parametry typu nazwane właściwości, które pojawiają się w szablonie wiadomości.</span><span class="sxs-lookup"><span data-stu-id="3780b-180">Include any type parameters for named properties that appear in the message template.</span></span> <span data-ttu-id="3780b-181">Przykładowa aplikacja pobiera `count` apostrofu, aby usunąć i zwraca `_allQuotesDeletedScope`:</span><span class="sxs-lookup"><span data-stu-id="3780b-181">The sample app takes in a `count` of quotes to delete and returns `_allQuotesDeletedScope`:</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet12)]

<span data-ttu-id="3780b-182">Zawija zakresu wywołuje rozszerzenia rejestrowania [przy użyciu](/dotnet/csharp/language-reference/keywords/using-statement) bloku:</span><span class="sxs-lookup"><span data-stu-id="3780b-182">The scope wraps the logging extension calls in a [using](/dotnet/csharp/language-reference/keywords/using-statement) block:</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Pages/Index.cshtml.cs?name=snippet4&highlight=5-6,14)]

<span data-ttu-id="3780b-183">Sprawdź, czy komunikaty dziennika w aplikacji konsoli w danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="3780b-183">Inspect the log messages in the app's console output.</span></span> <span data-ttu-id="3780b-184">Następujące wyniki pokazuje trzy cudzysłowy usunięte z komunikatem o zakres dziennika, które zostały dołączone:</span><span class="sxs-lookup"><span data-stu-id="3780b-184">The following result shows three quotes deleted with the log scope message included:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="3780b-185">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="3780b-185">Additional resources</span></span>

* [<span data-ttu-id="3780b-186">Rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="3780b-186">Logging</span></span>](xref:fundamentals/logging/index)
