---
title: "Rejestrowanie wysokiej wydajności z LoggerMessage w ASP.NET Core"
author: guardrex
description: "Dowiedz się, jak używać LoggerMessage do tworzenia buforowalnej obiektów delegowanych, które wymagają mniej alokacji obiektu w scenariuszach logowania wysokiej wydajności."
manager: wpickett
ms.author: riande
ms.date: 11/03/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/logging/loggermessage
ms.openlocfilehash: 24a75cfacfa61ca66e78deeb743baa75718dfb76
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/02/2018
---
# <a name="high-performance-logging-with-loggermessage-in-aspnet-core"></a><span data-ttu-id="af92f-103">Rejestrowanie wysokiej wydajności z LoggerMessage w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="af92f-103">High-performance logging with LoggerMessage in ASP.NET Core</span></span>

<span data-ttu-id="af92f-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="af92f-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="af92f-105">[LoggerMessage](/dotnet/api/microsoft.extensions.logging.loggermessage) funkcje tworzenia buforowalnej delegatów, które wymagają mniej obiekt alokacji i zmniejszyć obciążenie obliczeniowe niż [metody rozszerzenia rejestratora](/dotnet/api/Microsoft.Extensions.Logging.LoggerExtensions), takich jak `LogInformation`, `LogDebug`i `LogError`.</span><span class="sxs-lookup"><span data-stu-id="af92f-105">[LoggerMessage](/dotnet/api/microsoft.extensions.logging.loggermessage) features create cacheable delegates that require fewer object allocations and reduced computational overhead than [logger extension methods](/dotnet/api/Microsoft.Extensions.Logging.LoggerExtensions), such as `LogInformation`, `LogDebug`, and `LogError`.</span></span> <span data-ttu-id="af92f-106">W scenariuszach logowania wysokiej wydajności, należy użyć `LoggerMessage` wzorca.</span><span class="sxs-lookup"><span data-stu-id="af92f-106">For high-performance logging scenarios, use the `LoggerMessage` pattern.</span></span>

<span data-ttu-id="af92f-107">`LoggerMessage` zapewnia następujące korzyści wydajności za pośrednictwem metody rozszerzenia rejestratora:</span><span class="sxs-lookup"><span data-stu-id="af92f-107">`LoggerMessage` provides the following performance advantages over Logger extension methods:</span></span>

* <span data-ttu-id="af92f-108">Metody rozszerzenia rejestratora wymagane typy wartości "boxing" (konwertowanie), takich jak `int`, do `object`.</span><span class="sxs-lookup"><span data-stu-id="af92f-108">Logger extension methods require "boxing" (converting) value types, such as `int`, into `object`.</span></span> <span data-ttu-id="af92f-109">`LoggerMessage` Wzorzec pozwala uniknąć opakowanie przy użyciu statycznego `Action` pól i metod rozszerzenia z silną kontrolą typów parametrów.</span><span class="sxs-lookup"><span data-stu-id="af92f-109">The `LoggerMessage` pattern avoids boxing by using static `Action` fields and extension methods with strongly-typed parameters.</span></span>
* <span data-ttu-id="af92f-110">Metody rozszerzenia rejestratora należy przeanalizować szablon wiadomości (ciąg formatu o nazwie) zawsze komunikatu dziennika są zapisywane.</span><span class="sxs-lookup"><span data-stu-id="af92f-110">Logger extension methods must parse the message template (named format string) every time a log message is written.</span></span> <span data-ttu-id="af92f-111">`LoggerMessage` wymaga tylko raz podczas analizowania szablonu, jeśli komunikat jest zdefiniowana.</span><span class="sxs-lookup"><span data-stu-id="af92f-111">`LoggerMessage` only requires parsing a template once when the message is defined.</span></span>

<span data-ttu-id="af92f-112">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/loggermessage/sample/) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="af92f-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/loggermessage/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="af92f-113">Przedstawiono przykładową aplikację `LoggerMessage` funkcje podstawowe oferty, w systemie śledzenia.</span><span class="sxs-lookup"><span data-stu-id="af92f-113">The sample app demonstrates `LoggerMessage` features with a basic quote tracking system.</span></span> <span data-ttu-id="af92f-114">Aplikacja dodaje i usuwa cudzysłowy korzystania z bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="af92f-114">The app adds and deletes quotes using an in-memory database.</span></span> <span data-ttu-id="af92f-115">Ponieważ te operacje są wykonywane, komunikaty w dzienniku są generowane przy użyciu `LoggerMessage` wzorca.</span><span class="sxs-lookup"><span data-stu-id="af92f-115">As these operations occur, log messages are generated using the `LoggerMessage` pattern.</span></span>

## <a name="loggermessagedefine"></a><span data-ttu-id="af92f-116">LoggerMessage.Define</span><span class="sxs-lookup"><span data-stu-id="af92f-116">LoggerMessage.Define</span></span>

<span data-ttu-id="af92f-117">[Zdefiniuj (LogLevel, identyfikator zdarzenia, ciąg)](/dotnet/api/microsoft.extensions.logging.loggermessage.define) tworzy `Action` delegować do rejestrowania wiadomości.</span><span class="sxs-lookup"><span data-stu-id="af92f-117">[Define(LogLevel, EventId, String)](/dotnet/api/microsoft.extensions.logging.loggermessage.define) creates an `Action` delegate for logging a message.</span></span> <span data-ttu-id="af92f-118">`Define` przeciążenia zezwala na przekazywanie maksymalnie sześć parametrów typu do ciągu formatu o nazwie (szablonu).</span><span class="sxs-lookup"><span data-stu-id="af92f-118">`Define` overloads permit passing up to six type parameters to a named format string (template).</span></span>

<span data-ttu-id="af92f-119">Podany ciąg `Define` metoda jest szablon i nie ciągu interpolowanym.</span><span class="sxs-lookup"><span data-stu-id="af92f-119">The string provided to the `Define` method is a template and not an interpolated string.</span></span> <span data-ttu-id="af92f-120">Symbole zastępcze są wypełnione w kolejności określono typów.</span><span class="sxs-lookup"><span data-stu-id="af92f-120">Placeholders are filled in the order that the types are specified.</span></span> <span data-ttu-id="af92f-121">Przez Szablony nazwy symbolu zastępczego w szablonie powinny być opisowe i spójne.</span><span class="sxs-lookup"><span data-stu-id="af92f-121">Placeholder names in the template should be descriptive and consistent across templates.</span></span> <span data-ttu-id="af92f-122">Służą one jako nazwy właściwości w strukturze danych dziennika.</span><span class="sxs-lookup"><span data-stu-id="af92f-122">They serve as property names within structured log data.</span></span> <span data-ttu-id="af92f-123">Firma Microsoft zaleca [Pascal wielkości liter](/dotnet/standard/design-guidelines/capitalization-conventions) dla nazwy symbolu zastępczego.</span><span class="sxs-lookup"><span data-stu-id="af92f-123">We recommend [Pascal casing](/dotnet/standard/design-guidelines/capitalization-conventions) for placeholder names.</span></span> <span data-ttu-id="af92f-124">Na przykład `{Count}`, `{FirstName}`.</span><span class="sxs-lookup"><span data-stu-id="af92f-124">For example, `{Count}`, `{FirstName}`.</span></span>

<span data-ttu-id="af92f-125">Każdy komunikat dziennika jest `Action` przechowywany w polu statycznym utworzone przez `LoggerMessage.Define`.</span><span class="sxs-lookup"><span data-stu-id="af92f-125">Each log message is an `Action` held in a static field created by `LoggerMessage.Define`.</span></span> <span data-ttu-id="af92f-126">Na przykład przykładowa aplikacja tworzy pole do opisu komunikatu dziennika dla żądania GET strony indeksu (*Internal/LoggerExtensions.cs*):</span><span class="sxs-lookup"><span data-stu-id="af92f-126">For example, the sample app creates a field to describe a log message for a GET request for the Index page (*Internal/LoggerExtensions.cs*):</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet1)]

<span data-ttu-id="af92f-127">Aby uzyskać `Action`, określ:</span><span class="sxs-lookup"><span data-stu-id="af92f-127">For the `Action`, specify:</span></span>

* <span data-ttu-id="af92f-128">Poziom dziennika.</span><span class="sxs-lookup"><span data-stu-id="af92f-128">The log level.</span></span>
* <span data-ttu-id="af92f-129">Identyfikator unikatowy zdarzenia ([EventId](/dotnet/api/microsoft.extensions.logging.eventid)) z nazwą metody statycznej rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="af92f-129">A unique event identifier ([EventId](/dotnet/api/microsoft.extensions.logging.eventid)) with the name of the static extension method.</span></span>
* <span data-ttu-id="af92f-130">Szablon wiadomości (o nazwie ciąg formatu).</span><span class="sxs-lookup"><span data-stu-id="af92f-130">The message template (named format string).</span></span> 

<span data-ttu-id="af92f-131">Żądanie strony indeksu zestawów aplikacji przykładowej:</span><span class="sxs-lookup"><span data-stu-id="af92f-131">A request for the Index page of the sample app sets the:</span></span>

* <span data-ttu-id="af92f-132">Poziom do dziennika `Information`.</span><span class="sxs-lookup"><span data-stu-id="af92f-132">Log level to `Information`.</span></span>
* <span data-ttu-id="af92f-133">Identyfikator zdarzenia do `1` o nazwie `IndexPageRequested` metody.</span><span class="sxs-lookup"><span data-stu-id="af92f-133">Event id to `1` with the name of the `IndexPageRequested` method.</span></span>
* <span data-ttu-id="af92f-134">Szablon wiadomości (ciąg formatu o nazwie) na ciąg.</span><span class="sxs-lookup"><span data-stu-id="af92f-134">Message template (named format string) to a string.</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet5)]

<span data-ttu-id="af92f-135">Rejestrowanie strukturalne magazyny może używać nazwy zdarzenia, gdy są dostarczane z identyfikatorem zdarzenia wzbogacić rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="af92f-135">Structured logging stores may use the event name when it's supplied with the event id to enrich logging.</span></span> <span data-ttu-id="af92f-136">Na przykład [Serilog](https://github.com/serilog/serilog-extensions-logging) , użyta zostanie nazwa zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="af92f-136">For example, [Serilog](https://github.com/serilog/serilog-extensions-logging) uses the event name.</span></span>

<span data-ttu-id="af92f-137">`Action` Jest wywoływana przez metodę rozszerzenie jednoznacznie.</span><span class="sxs-lookup"><span data-stu-id="af92f-137">The `Action` is invoked through a strongly-typed extension method.</span></span> <span data-ttu-id="af92f-138">`IndexPageRequested` — Metoda rejestruje komunikat dla żądania GET strony indeksu w przykładowej aplikacji:</span><span class="sxs-lookup"><span data-stu-id="af92f-138">The `IndexPageRequested` method logs a message for an Index page GET request in the sample app:</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet9)]

<span data-ttu-id="af92f-139">`IndexPageRequested` wywoływana jest rejestratora w `OnGetAsync` metody w *Pages/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="af92f-139">`IndexPageRequested` is called on the logger in the `OnGetAsync` method in *Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3)]

<span data-ttu-id="af92f-140">Sprawdź dane wyjściowe konsoli aplikacji:</span><span class="sxs-lookup"><span data-stu-id="af92f-140">Inspect the app's console output:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[1]
      => RequestId:0HL90M6E7PHK4:00000001 RequestPath:/ => /Index
      GET request for Index page
```

<span data-ttu-id="af92f-141">Do przekazania parametrów do komunikatu dziennika, należy zdefiniować typy sześciu podczas tworzenia pola statycznego.</span><span class="sxs-lookup"><span data-stu-id="af92f-141">To pass parameters to a log message, define up to six types when creating the static field.</span></span> <span data-ttu-id="af92f-142">Przykładowa aplikacja rejestruje ciąg podczas dodawania oferty, definiując `string` wpisz `Action` pola:</span><span class="sxs-lookup"><span data-stu-id="af92f-142">The sample app logs a string when adding a quote by defining a `string` type for the `Action` field:</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet2)]

<span data-ttu-id="af92f-143">Szablon wiadomości dziennika delegata odbiera jego symbole zastępcze z udostępnione typy.</span><span class="sxs-lookup"><span data-stu-id="af92f-143">The delegate's log message template receives its placeholder values from the types provided.</span></span> <span data-ttu-id="af92f-144">Przykładowa aplikacja definiuje delegata dodawania oferty, w którym parametr oferty jest `string`:</span><span class="sxs-lookup"><span data-stu-id="af92f-144">The sample app defines a delegate for adding a quote where the quote parameter is a `string`:</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet6)]

<span data-ttu-id="af92f-145">Metody rozszerzenia statycznych dodawania oferty, `QuoteAdded`, otrzymuje wartość argumentu oferty i przekazuje je do `Action` delegować:</span><span class="sxs-lookup"><span data-stu-id="af92f-145">The static extension method for adding a quote, `QuoteAdded`, receives the quote argument value and passes it to the `Action` delegate:</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet10)]

<span data-ttu-id="af92f-146">W modelu strony strony indeksu (*Pages/Index.cshtml.cs*), `QuoteAdded` nosi nazwę logowania komunikat:</span><span class="sxs-lookup"><span data-stu-id="af92f-146">In the Index page's page model (*Pages/Index.cshtml.cs*), `QuoteAdded` is called to log the message:</span></span>

[!code-csharp[](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet3&highlight=6)]

<span data-ttu-id="af92f-147">Sprawdź dane wyjściowe konsoli aplikacji:</span><span class="sxs-lookup"><span data-stu-id="af92f-147">Inspect the app's console output:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[2]
      => RequestId:0HL90M6E7PHK5:0000000A RequestPath:/ => /Index
      Quote added (Quote = 'You can avoid reality, but you cannot avoid the consequences of avoiding reality. - Ayn Rand')
```

<span data-ttu-id="af92f-148">Implementuje aplikacji przykładowej `try` &ndash; `catch` wzorzec do usunięcia oferty.</span><span class="sxs-lookup"><span data-stu-id="af92f-148">The sample app implements a `try`&ndash;`catch` pattern for quote deletion.</span></span> <span data-ttu-id="af92f-149">Komunikat informacyjny jest rejestrowany przez operację usuwania powiodło się.</span><span class="sxs-lookup"><span data-stu-id="af92f-149">An informational message is logged for a successful delete operation.</span></span> <span data-ttu-id="af92f-150">Komunikat o błędzie jest rejestrowane dla operacji delete, gdy jest zgłaszany wyjątek.</span><span class="sxs-lookup"><span data-stu-id="af92f-150">An error message is logged for a delete operation when an exception is thrown.</span></span> <span data-ttu-id="af92f-151">Komunikat dziennika powiodła się operacja usuwania obejmuje ślad stosu wyjątku (*Internal/LoggerExtensions.cs*):</span><span class="sxs-lookup"><span data-stu-id="af92f-151">The log message for the unsuccessful delete operation includes the exception stack trace (*Internal/LoggerExtensions.cs*):</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet3)]

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet7)]

<span data-ttu-id="af92f-152">Należy zwrócić uwagę, jak wyjątek jest przekazywana do delegata `QuoteDeleteFailed`:</span><span class="sxs-lookup"><span data-stu-id="af92f-152">Note how the exception is passed to the delegate in `QuoteDeleteFailed`:</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet11)]

<span data-ttu-id="af92f-153">W modelu strony dla strony indeksu, usuwanie oferty pomyślne wywołanie `QuoteDeleted` metoda rejestratora.</span><span class="sxs-lookup"><span data-stu-id="af92f-153">In the page model for the Index page, a successful quote deletion calls the `QuoteDeleted` method on the logger.</span></span> <span data-ttu-id="af92f-154">Gdy oferta nie zostanie odnaleziony do usunięcia, `ArgumentNullException` jest generowany.</span><span class="sxs-lookup"><span data-stu-id="af92f-154">When a quote isn't found for deletion, an `ArgumentNullException` is thrown.</span></span> <span data-ttu-id="af92f-155">Wyjątkiem jest to spowodowane `try` &ndash; `catch` instrukcji i rejestrowane przez wywołanie metody `QuoteDeleteFailed` metoda rejestratora w `catch` bloku (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="af92f-155">The exception is trapped by the `try`&ndash;`catch` statement and logged by calling the `QuoteDeleteFailed` method on the logger in the `catch` block (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet5&highlight=14,18)]

<span data-ttu-id="af92f-156">Po pomyślnym usunięciu oferty Sprawdź dane wyjściowe konsoli aplikacji:</span><span class="sxs-lookup"><span data-stu-id="af92f-156">When a quote is successfully deleted, inspect the app's console output:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:00000016 RequestPath:/ => /Index
      Quote deleted (Quote = 'You can avoid reality, but you cannot avoid the consequences of avoiding reality. - Ayn Rand' Id = 1)
```

<span data-ttu-id="af92f-157">Podczas usuwania oferty zakończy się niepowodzeniem, sprawdź, czy dane wyjściowe aplikacji konsoli.</span><span class="sxs-lookup"><span data-stu-id="af92f-157">When quote deletion fails, inspect the app's console output.</span></span> <span data-ttu-id="af92f-158">Należy pamiętać, że wyjątek jest dołączany do wiadomości dziennika:</span><span class="sxs-lookup"><span data-stu-id="af92f-158">Note that the exception is included in the log message:</span></span>

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

## <a name="loggermessagedefinescope"></a><span data-ttu-id="af92f-159">LoggerMessage.DefineScope</span><span class="sxs-lookup"><span data-stu-id="af92f-159">LoggerMessage.DefineScope</span></span>

<span data-ttu-id="af92f-160">[DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) tworzy `Func` delegować do definiowania [dziennika zakresu](xref:fundamentals/logging/index#log-scopes).</span><span class="sxs-lookup"><span data-stu-id="af92f-160">[DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) creates a `Func` delegate for defining a [log scope](xref:fundamentals/logging/index#log-scopes).</span></span> <span data-ttu-id="af92f-161">`DefineScope` przeciążenia zezwala na przekazywanie maksymalnie trzech parametrów typu do ciągu formatu o nazwie (szablonu).</span><span class="sxs-lookup"><span data-stu-id="af92f-161">`DefineScope` overloads permit passing up to three type parameters to a named format string (template).</span></span>

<span data-ttu-id="af92f-162">Jak w przypadku `Define` metoda, do podanego ciągu `DefineScope` metoda jest szablon i nie ciągu interpolowanym.</span><span class="sxs-lookup"><span data-stu-id="af92f-162">As is the case with the `Define` method, the string provided to the `DefineScope` method is a template and not an interpolated string.</span></span> <span data-ttu-id="af92f-163">Symbole zastępcze są wypełnione w kolejności określono typów.</span><span class="sxs-lookup"><span data-stu-id="af92f-163">Placeholders are filled in the order that the types are specified.</span></span> <span data-ttu-id="af92f-164">Przez Szablony nazwy symbolu zastępczego w szablonie powinny być opisowe i spójne.</span><span class="sxs-lookup"><span data-stu-id="af92f-164">Placeholder names in the template should be descriptive and consistent across templates.</span></span> <span data-ttu-id="af92f-165">Służą one jako nazwy właściwości w strukturze danych dziennika.</span><span class="sxs-lookup"><span data-stu-id="af92f-165">They serve as property names within structured log data.</span></span> <span data-ttu-id="af92f-166">Firma Microsoft zaleca [Pascal wielkości liter](/dotnet/standard/design-guidelines/capitalization-conventions) dla nazwy symbolu zastępczego.</span><span class="sxs-lookup"><span data-stu-id="af92f-166">We recommend [Pascal casing](/dotnet/standard/design-guidelines/capitalization-conventions) for placeholder names.</span></span> <span data-ttu-id="af92f-167">Na przykład `{Count}`, `{FirstName}`.</span><span class="sxs-lookup"><span data-stu-id="af92f-167">For example, `{Count}`, `{FirstName}`.</span></span>

<span data-ttu-id="af92f-168">Zdefiniuj [dziennika zakres](xref:fundamentals/logging/index#log-scopes) do zastosowania do serii komunikaty dziennika przy użyciu [DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) metody.</span><span class="sxs-lookup"><span data-stu-id="af92f-168">Define a [log scope](xref:fundamentals/logging/index#log-scopes) to apply to a series of log messages using the [DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) method.</span></span>

<span data-ttu-id="af92f-169">Przykładowa aplikacja ma **Wyczyść wszystko** przycisk usuwania wszystkich znaków cudzysłowu w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="af92f-169">The sample app has a **Clear All** button for deleting all of the quotes in the database.</span></span> <span data-ttu-id="af92f-170">Cudzysłowy zostaną usunięte przez usunięcie jednego naraz.</span><span class="sxs-lookup"><span data-stu-id="af92f-170">The quotes are deleted by removing them one at a time.</span></span> <span data-ttu-id="af92f-171">Zawsze oferty zostanie usunięty, `QuoteDeleted` wywoływana jest metoda rejestratora.</span><span class="sxs-lookup"><span data-stu-id="af92f-171">Each time a quote is deleted, the `QuoteDeleted` method is called on the logger.</span></span> <span data-ttu-id="af92f-172">Zakres dziennika zostanie dodany do tych wiadomości dziennika.</span><span class="sxs-lookup"><span data-stu-id="af92f-172">A log scope is added to these log messages.</span></span>

<span data-ttu-id="af92f-173">Włącz `IncludeScopes` w opcjach rejestratora konsoli:</span><span class="sxs-lookup"><span data-stu-id="af92f-173">Enable `IncludeScopes` in the console logger options:</span></span>

[!code-csharp[](loggermessage/sample/Program.cs?name=snippet1&highlight=10)]

<span data-ttu-id="af92f-174">Ustawienie `IncludeScopes` wymaganego do włączenia dziennika zakresów w aplikacjach ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="af92f-174">Setting `IncludeScopes` is required in ASP.NET Core 2.0 apps to enable log scopes.</span></span> <span data-ttu-id="af92f-175">Ustawienie `IncludeScopes` za pośrednictwem *appsettings* plików konfiguracyjnych jest funkcją, która ma zaplanowane dla wersji platformy ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="af92f-175">Setting `IncludeScopes` via *appsettings* configuration files is a feature that's planned for the ASP.NET Core 2.1 release.</span></span>

<span data-ttu-id="af92f-176">Przykładowa aplikacja czyści innych dostawców i dodaje filtry, aby zmniejszyć dane wyjściowe rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="af92f-176">The sample app clears other providers and adds filters to reduce the logging output.</span></span> <span data-ttu-id="af92f-177">Ułatwia zobaczyć przykładowy komunikatów dziennika, które pokazują `LoggerMessage` funkcji.</span><span class="sxs-lookup"><span data-stu-id="af92f-177">This makes it easier to see the sample's log messages that demonstrate `LoggerMessage` features.</span></span>

<span data-ttu-id="af92f-178">Aby utworzyć zakres dziennika, Dodaj pole do przechowywania `Func` delegowanie dla zakresu.</span><span class="sxs-lookup"><span data-stu-id="af92f-178">To create a log scope, add a field to hold a `Func` delegate for the scope.</span></span> <span data-ttu-id="af92f-179">Przykładowa aplikacja tworzy pole o nazwie `_allQuotesDeletedScope` (*Internal/LoggerExtensions.cs*):</span><span class="sxs-lookup"><span data-stu-id="af92f-179">The sample app creates a field called `_allQuotesDeletedScope` (*Internal/LoggerExtensions.cs*):</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet4)]

<span data-ttu-id="af92f-180">Użyj `DefineScope` można utworzyć obiektu delegowanego.</span><span class="sxs-lookup"><span data-stu-id="af92f-180">Use `DefineScope` to create the delegate.</span></span> <span data-ttu-id="af92f-181">Maksymalnie trzy typy można określić do użycia jako argumenty szablonu po wywołaniu obiektu delegowanego.</span><span class="sxs-lookup"><span data-stu-id="af92f-181">Up to three types can be specified for use as template arguments when the delegate is invoked.</span></span> <span data-ttu-id="af92f-182">Przykładowa aplikacja korzysta z szablonu wiadomości, który zawiera liczby cudzysłowów usunięto ( `int` typu):</span><span class="sxs-lookup"><span data-stu-id="af92f-182">The sample app uses a message template that includes the number of deleted quotes (an `int` type):</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet8)]

<span data-ttu-id="af92f-183">Udostępnia metody statyczne rozszerzenie wiadomości dziennika.</span><span class="sxs-lookup"><span data-stu-id="af92f-183">Provide a static extension method for the log message.</span></span> <span data-ttu-id="af92f-184">Uwzględnij wszystkie parametry typu dla nazwanych właściwości, które pojawiają się w szablon wiadomości.</span><span class="sxs-lookup"><span data-stu-id="af92f-184">Include any type parameters for named properties that appear in the message template.</span></span> <span data-ttu-id="af92f-185">Przykładowa aplikacja przyjmuje `count` cudzysłowy do usunięcia i zwraca `_allQuotesDeletedScope`:</span><span class="sxs-lookup"><span data-stu-id="af92f-185">The sample app takes in a `count` of quotes to delete and returns `_allQuotesDeletedScope`:</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet12)]

<span data-ttu-id="af92f-186">Zawija zakres odwołuje się do rozszerzenia rejestrowania `using` bloku:</span><span class="sxs-lookup"><span data-stu-id="af92f-186">The scope wraps the logging extension calls in a `using` block:</span></span>

[!code-csharp[](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet4&highlight=5-6,14)]

<span data-ttu-id="af92f-187">Sprawdź, czy w komunikatach w dzienniku aplikacji konsoli w danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="af92f-187">Inspect the log messages in the app's console output.</span></span> <span data-ttu-id="af92f-188">Następujące wyniki przedstawiono trzy cudzysłowy usunięte z komunikatem zakresu dziennika uwzględnione:</span><span class="sxs-lookup"><span data-stu-id="af92f-188">The following result shows three quotes deleted with the log scope message included:</span></span>

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

## <a name="see-also"></a><span data-ttu-id="af92f-189">Zobacz także</span><span class="sxs-lookup"><span data-stu-id="af92f-189">See also</span></span>

* [<span data-ttu-id="af92f-190">Rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="af92f-190">Logging</span></span>](xref:fundamentals/logging/index)
