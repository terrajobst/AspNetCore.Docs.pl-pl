---
title: "Rejestrowanie wysokiej wydajności z LoggerMessage w ASP.NET Core"
author: guardrex
description: "Dowiedz się, jak utworzyć buforowalnej obiektów delegowanych, które wymagają mniej alokacji obiektu od metod rozszerzenia rejestratora w scenariuszach wysokiej wydajności logowania przy użyciu funkcji LoggerMessage."
manager: wpickett
ms.author: riande
ms.date: 11/03/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/logging/loggermessage
ms.openlocfilehash: b155826b5047e88a79d9e339d7bca8885a79006d
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2018
---
# <a name="high-performance-logging-with-loggermessage-in-aspnet-core"></a><span data-ttu-id="2b638-103">Rejestrowanie wysokiej wydajności z LoggerMessage w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2b638-103">High-performance logging with LoggerMessage in ASP.NET Core</span></span>

<span data-ttu-id="2b638-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="2b638-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="2b638-105">[LoggerMessage](/dotnet/api/microsoft.extensions.logging.loggermessage) funkcje tworzenia buforowalnej delegatów, które wymagają mniej obiekt alokacji i zmniejszyć obciążenie obliczeniowe niż [metody rozszerzenia rejestratora](/dotnet/api/Microsoft.Extensions.Logging.LoggerExtensions), takich jak `LogInformation`, `LogDebug`i `LogError`.</span><span class="sxs-lookup"><span data-stu-id="2b638-105">[LoggerMessage](/dotnet/api/microsoft.extensions.logging.loggermessage) features create cacheable delegates that require fewer object allocations and reduced computational overhead than [logger extension methods](/dotnet/api/Microsoft.Extensions.Logging.LoggerExtensions), such as `LogInformation`, `LogDebug`, and `LogError`.</span></span> <span data-ttu-id="2b638-106">W scenariuszach logowania wysokiej wydajności, należy użyć `LoggerMessage` wzorca.</span><span class="sxs-lookup"><span data-stu-id="2b638-106">For high-performance logging scenarios, use the `LoggerMessage` pattern.</span></span>

<span data-ttu-id="2b638-107">`LoggerMessage`zapewnia następujące korzyści wydajności za pośrednictwem metody rozszerzenia rejestratora:</span><span class="sxs-lookup"><span data-stu-id="2b638-107">`LoggerMessage` provides the following performance advantages over Logger extension methods:</span></span>

* <span data-ttu-id="2b638-108">Metody rozszerzenia rejestratora wymagane typy wartości "boxing" (konwertowanie), takich jak `int`, do `object`.</span><span class="sxs-lookup"><span data-stu-id="2b638-108">Logger extension methods require "boxing" (converting) value types, such as `int`, into `object`.</span></span> <span data-ttu-id="2b638-109">`LoggerMessage` Wzorzec pozwala uniknąć opakowanie przy użyciu statycznego `Action` pól i metod rozszerzenia z silną kontrolą typów parametrów.</span><span class="sxs-lookup"><span data-stu-id="2b638-109">The `LoggerMessage` pattern avoids boxing by using static `Action` fields and extension methods with strongly-typed parameters.</span></span>
* <span data-ttu-id="2b638-110">Metody rozszerzenia rejestratora należy przeanalizować szablon wiadomości (ciąg formatu o nazwie) zawsze komunikatu dziennika są zapisywane.</span><span class="sxs-lookup"><span data-stu-id="2b638-110">Logger extension methods must parse the message template (named format string) every time a log message is written.</span></span> <span data-ttu-id="2b638-111">`LoggerMessage`wymaga tylko raz podczas analizowania szablonu, jeśli komunikat jest zdefiniowana.</span><span class="sxs-lookup"><span data-stu-id="2b638-111">`LoggerMessage` only requires parsing a template once when the message is defined.</span></span>

<span data-ttu-id="2b638-112">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/loggermessage/sample/) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2b638-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/loggermessage/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="2b638-113">Przedstawiono przykładową aplikację `LoggerMessage` funkcje podstawowe oferty, w systemie śledzenia.</span><span class="sxs-lookup"><span data-stu-id="2b638-113">The sample app demonstrates `LoggerMessage` features with a basic quote tracking system.</span></span> <span data-ttu-id="2b638-114">Aplikacja dodaje i usuwa cudzysłowy korzystania z bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="2b638-114">The app adds and deletes quotes using an in-memory database.</span></span> <span data-ttu-id="2b638-115">Ponieważ te operacje są wykonywane, komunikaty w dzienniku są generowane przy użyciu `LoggerMessage` wzorca.</span><span class="sxs-lookup"><span data-stu-id="2b638-115">As these operations occur, log messages are generated using the `LoggerMessage` pattern.</span></span>

## <a name="loggermessagedefine"></a><span data-ttu-id="2b638-116">LoggerMessage.Define</span><span class="sxs-lookup"><span data-stu-id="2b638-116">LoggerMessage.Define</span></span>

<span data-ttu-id="2b638-117">[Zdefiniuj (LogLevel, identyfikator zdarzenia, ciąg)](/dotnet/api/microsoft.extensions.logging.loggermessage.define) tworzy `Action` delegować do rejestrowania wiadomości.</span><span class="sxs-lookup"><span data-stu-id="2b638-117">[Define(LogLevel, EventId, String)](/dotnet/api/microsoft.extensions.logging.loggermessage.define) creates an `Action` delegate for logging a message.</span></span> <span data-ttu-id="2b638-118">`Define`przeciążenia zezwala na przekazywanie maksymalnie sześć parametrów typu do ciągu formatu o nazwie (szablonu).</span><span class="sxs-lookup"><span data-stu-id="2b638-118">`Define` overloads permit passing up to six type parameters to a named format string (template).</span></span>

## <a name="loggermessagedefinescope"></a><span data-ttu-id="2b638-119">LoggerMessage.DefineScope</span><span class="sxs-lookup"><span data-stu-id="2b638-119">LoggerMessage.DefineScope</span></span>

<span data-ttu-id="2b638-120">[DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) tworzy `Func` delegować do definiowania [dziennika zakresu](xref:fundamentals/logging/index#log-scopes).</span><span class="sxs-lookup"><span data-stu-id="2b638-120">[DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) creates a `Func` delegate for defining a [log scope](xref:fundamentals/logging/index#log-scopes).</span></span> <span data-ttu-id="2b638-121">`DefineScope`przeciążenia zezwala na przekazywanie maksymalnie trzech parametrów typu do ciągu formatu o nazwie (szablonu).</span><span class="sxs-lookup"><span data-stu-id="2b638-121">`DefineScope` overloads permit passing up to three type parameters to a named format string (template).</span></span>

## <a name="message-template-named-format-string"></a><span data-ttu-id="2b638-122">Szablon wiadomości (ciąg formatu o nazwie)</span><span class="sxs-lookup"><span data-stu-id="2b638-122">Message template (named format string)</span></span>

<span data-ttu-id="2b638-123">Podany ciąg `Define` i `DefineScope` metod to szablon, a nie ciągu interpolowanym.</span><span class="sxs-lookup"><span data-stu-id="2b638-123">The string provided to the `Define` and `DefineScope` methods is a template and not an interpolated string.</span></span> <span data-ttu-id="2b638-124">Symbole zastępcze są wypełnione w kolejności określono typów.</span><span class="sxs-lookup"><span data-stu-id="2b638-124">Placeholders are filled in the order that the types are specified.</span></span> <span data-ttu-id="2b638-125">Przez Szablony nazwy symbolu zastępczego w szablonie powinny być opisowe i spójne.</span><span class="sxs-lookup"><span data-stu-id="2b638-125">Placeholder names in the template should be descriptive and consistent across templates.</span></span> <span data-ttu-id="2b638-126">Służą one jako nazwy właściwości w strukturze danych dziennika.</span><span class="sxs-lookup"><span data-stu-id="2b638-126">They serve as property names within structured log data.</span></span> <span data-ttu-id="2b638-127">Firma Microsoft zaleca [Pascal wielkości liter](/dotnet/standard/design-guidelines/capitalization-conventions) dla nazwy symbolu zastępczego.</span><span class="sxs-lookup"><span data-stu-id="2b638-127">We recommend [Pascal casing](/dotnet/standard/design-guidelines/capitalization-conventions) for placeholder names.</span></span> <span data-ttu-id="2b638-128">Na przykład `{Count}`, `{FirstName}`.</span><span class="sxs-lookup"><span data-stu-id="2b638-128">For example, `{Count}`, `{FirstName}`.</span></span>

## <a name="implementing-loggermessagedefine"></a><span data-ttu-id="2b638-129">Implementowanie LoggerMessage.Define</span><span class="sxs-lookup"><span data-stu-id="2b638-129">Implementing LoggerMessage.Define</span></span>

<span data-ttu-id="2b638-130">Każdy komunikat dziennika jest `Action` przechowywany w polu statycznym utworzone przez `LoggerMessage.Define`.</span><span class="sxs-lookup"><span data-stu-id="2b638-130">Each log message is an `Action` held in a static field created by `LoggerMessage.Define`.</span></span> <span data-ttu-id="2b638-131">Na przykład przykładowa aplikacja tworzy pole do opisu komunikatu dziennika dla żądania GET strony indeksu (*Internal/LoggerExtensions.cs*):</span><span class="sxs-lookup"><span data-stu-id="2b638-131">For example, the sample app creates a field to describe a log message for a GET request for the Index page (*Internal/LoggerExtensions.cs*):</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet1)]

<span data-ttu-id="2b638-132">Aby uzyskać `Action`, określ:</span><span class="sxs-lookup"><span data-stu-id="2b638-132">For the `Action`, specify:</span></span>

* <span data-ttu-id="2b638-133">Poziom dziennika.</span><span class="sxs-lookup"><span data-stu-id="2b638-133">The log level.</span></span>
* <span data-ttu-id="2b638-134">Identyfikator unikatowy zdarzenia ([EventId](/dotnet/api/microsoft.extensions.logging.eventid)) z nazwą metody statycznej rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="2b638-134">A unique event identifier ([EventId](/dotnet/api/microsoft.extensions.logging.eventid)) with the name of the static extension method.</span></span>
* <span data-ttu-id="2b638-135">Szablon wiadomości (o nazwie ciąg formatu).</span><span class="sxs-lookup"><span data-stu-id="2b638-135">The message template (named format string).</span></span> 

<span data-ttu-id="2b638-136">Żądanie strony indeksu zestawów aplikacji przykładowej:</span><span class="sxs-lookup"><span data-stu-id="2b638-136">A request for the Index page of the sample app sets the:</span></span>

* <span data-ttu-id="2b638-137">Poziom do dziennika `Information`.</span><span class="sxs-lookup"><span data-stu-id="2b638-137">Log level to `Information`.</span></span>
* <span data-ttu-id="2b638-138">Identyfikator zdarzenia do `1` o nazwie `IndexPageRequested` metody.</span><span class="sxs-lookup"><span data-stu-id="2b638-138">Event id to `1` with the name of the `IndexPageRequested` method.</span></span>
* <span data-ttu-id="2b638-139">Szablon wiadomości (ciąg formatu o nazwie) na ciąg.</span><span class="sxs-lookup"><span data-stu-id="2b638-139">Message template (named format string) to a string.</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet5)]

<span data-ttu-id="2b638-140">Rejestrowanie strukturalne magazyny może używać nazwy zdarzenia, gdy są dostarczane z identyfikatorem zdarzenia wzbogacić rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="2b638-140">Structured logging stores may use the event name when it's supplied with the event id to enrich logging.</span></span> <span data-ttu-id="2b638-141">Na przykład [Serilog](https://github.com/serilog/serilog-extensions-logging) , użyta zostanie nazwa zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="2b638-141">For example, [Serilog](https://github.com/serilog/serilog-extensions-logging) uses the event name.</span></span>

<span data-ttu-id="2b638-142">`Action` Jest wywoływana przez metodę rozszerzenie jednoznacznie.</span><span class="sxs-lookup"><span data-stu-id="2b638-142">The `Action` is invoked through a strongly-typed extension method.</span></span> <span data-ttu-id="2b638-143">`IndexPageRequested` — Metoda rejestruje komunikat dla żądania GET strony indeksu w przykładowej aplikacji:</span><span class="sxs-lookup"><span data-stu-id="2b638-143">The `IndexPageRequested` method logs a message for an Index page GET request in the sample app:</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet9)]

<span data-ttu-id="2b638-144">`IndexPageRequested`wywoływana jest rejestratora w `OnGetAsync` metody w *Pages/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="2b638-144">`IndexPageRequested` is called on the logger in the `OnGetAsync` method in *Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[Main](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3)]

<span data-ttu-id="2b638-145">Sprawdź dane wyjściowe konsoli aplikacji:</span><span class="sxs-lookup"><span data-stu-id="2b638-145">Inspect the app's console output:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[1]
      => RequestId:0HL90M6E7PHK4:00000001 RequestPath:/ => /Index
      GET request for Index page
```

<span data-ttu-id="2b638-146">Do przekazania parametrów do komunikatu dziennika, należy zdefiniować typy sześciu podczas tworzenia pola statycznego.</span><span class="sxs-lookup"><span data-stu-id="2b638-146">To pass parameters to a log message, define up to six types when creating the static field.</span></span> <span data-ttu-id="2b638-147">Przykładowa aplikacja rejestruje ciąg podczas dodawania oferty, definiując `string` wpisz `Action` pola:</span><span class="sxs-lookup"><span data-stu-id="2b638-147">The sample app logs a string when adding a quote by defining a `string` type for the `Action` field:</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet2)]

<span data-ttu-id="2b638-148">Szablon wiadomości dziennika delegata odbiera jego symbole zastępcze z udostępnione typy.</span><span class="sxs-lookup"><span data-stu-id="2b638-148">The delegate's log message template receives its placeholder values from the types provided.</span></span> <span data-ttu-id="2b638-149">Przykładowa aplikacja definiuje delegata dodawania oferty, w którym parametr oferty jest `string`:</span><span class="sxs-lookup"><span data-stu-id="2b638-149">The sample app defines a delegate for adding a quote where the quote parameter is a `string`:</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet6)]

<span data-ttu-id="2b638-150">Metody rozszerzenia statycznych dodawania oferty, `QuoteAdded`, otrzymuje wartość argumentu oferty i przekazuje je do `Action` delegować:</span><span class="sxs-lookup"><span data-stu-id="2b638-150">The static extension method for adding a quote, `QuoteAdded`, receives the quote argument value and passes it to the `Action` delegate:</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet10)]

<span data-ttu-id="2b638-151">W modelu strony strony indeksu (*Pages/Index.cshtml.cs*), `QuoteAdded` nosi nazwę logowania komunikat:</span><span class="sxs-lookup"><span data-stu-id="2b638-151">In the Index page's page model (*Pages/Index.cshtml.cs*), `QuoteAdded` is called to log the message:</span></span>

[!code-csharp[Main](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet3&highlight=6)]

<span data-ttu-id="2b638-152">Sprawdź dane wyjściowe konsoli aplikacji:</span><span class="sxs-lookup"><span data-stu-id="2b638-152">Inspect the app's console output:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[2]
      => RequestId:0HL90M6E7PHK5:0000000A RequestPath:/ => /Index
      Quote added (Quote = 'You can avoid reality, but you cannot avoid the consequences of avoiding reality. - Ayn Rand')
```

<span data-ttu-id="2b638-153">Implementuje aplikacji przykładowej `try` &ndash; `catch` wzorzec do usunięcia oferty.</span><span class="sxs-lookup"><span data-stu-id="2b638-153">The sample app implements a `try`&ndash;`catch` pattern for quote deletion.</span></span> <span data-ttu-id="2b638-154">Komunikat informacyjny jest rejestrowany przez operację usuwania powiodło się.</span><span class="sxs-lookup"><span data-stu-id="2b638-154">An informational message is logged for a successful delete operation.</span></span> <span data-ttu-id="2b638-155">Komunikat o błędzie jest rejestrowane dla operacji delete, gdy jest zgłaszany wyjątek.</span><span class="sxs-lookup"><span data-stu-id="2b638-155">An error message is logged for a delete operation when an exception is thrown.</span></span> <span data-ttu-id="2b638-156">Komunikat dziennika powiodła się operacja usuwania obejmuje ślad stosu wyjątku (*Internal/LoggerExtensions.cs*):</span><span class="sxs-lookup"><span data-stu-id="2b638-156">The log message for the unsuccessful delete operation includes the exception stack trace (*Internal/LoggerExtensions.cs*):</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet3)]

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet7)]

<span data-ttu-id="2b638-157">Należy zwrócić uwagę, jak wyjątek jest przekazywana do delegata `QuoteDeleteFailed`:</span><span class="sxs-lookup"><span data-stu-id="2b638-157">Note how the exception is passed to the delegate in `QuoteDeleteFailed`:</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet11)]

<span data-ttu-id="2b638-158">W modelu strony dla strony indeksu, usuwanie oferty pomyślne wywołanie `QuoteDeleted` metoda rejestratora.</span><span class="sxs-lookup"><span data-stu-id="2b638-158">In the page model for the Index page, a successful quote deletion calls the `QuoteDeleted` method on the logger.</span></span> <span data-ttu-id="2b638-159">Gdy oferta nie zostanie odnaleziony do usunięcia, `ArgumentNullException` jest generowany.</span><span class="sxs-lookup"><span data-stu-id="2b638-159">When a quote isn't found for deletion, an `ArgumentNullException` is thrown.</span></span> <span data-ttu-id="2b638-160">Wyjątkiem jest to spowodowane `try` &ndash; `catch` instrukcji i rejestrowane przez wywołanie metody `QuoteDeleteFailed` metoda rejestratora w `catch` bloku (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="2b638-160">The exception is trapped by the `try`&ndash;`catch` statement and logged by calling the `QuoteDeleteFailed` method on the logger in the `catch` block (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[Main](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet5&highlight=14,18)]

<span data-ttu-id="2b638-161">Po pomyślnym usunięciu oferty Sprawdź dane wyjściowe konsoli aplikacji:</span><span class="sxs-lookup"><span data-stu-id="2b638-161">When a quote is successfully deleted, inspect the app's console output:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:00000016 RequestPath:/ => /Index
      Quote deleted (Quote = 'You can avoid reality, but you cannot avoid the consequences of avoiding reality. - Ayn Rand' Id = 1)
```

<span data-ttu-id="2b638-162">Podczas usuwania oferty zakończy się niepowodzeniem, sprawdź, czy dane wyjściowe aplikacji konsoli.</span><span class="sxs-lookup"><span data-stu-id="2b638-162">When quote deletion fails, inspect the app's console output.</span></span> <span data-ttu-id="2b638-163">Należy pamiętać, że wyjątek jest dołączany do wiadomości dziennika:</span><span class="sxs-lookup"><span data-stu-id="2b638-163">Note that the exception is included in the log message:</span></span>

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

## <a name="implementing-loggermessagedefinescope"></a><span data-ttu-id="2b638-164">Implementing LoggerMessage.DefineScope</span><span class="sxs-lookup"><span data-stu-id="2b638-164">Implementing LoggerMessage.DefineScope</span></span>

<span data-ttu-id="2b638-165">Zdefiniuj [dziennika zakres](xref:fundamentals/logging/index#log-scopes) do zastosowania do serii komunikaty dziennika przy użyciu [DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) metody.</span><span class="sxs-lookup"><span data-stu-id="2b638-165">Define a [log scope](xref:fundamentals/logging/index#log-scopes) to apply to a series of log messages using the [DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) method.</span></span>

<span data-ttu-id="2b638-166">Przykładowa aplikacja ma **Wyczyść wszystko** przycisk usuwania wszystkich znaków cudzysłowu w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="2b638-166">The sample app has a **Clear All** button for deleting all of the quotes in the database.</span></span> <span data-ttu-id="2b638-167">Cudzysłowy zostaną usunięte przez usunięcie jednego naraz.</span><span class="sxs-lookup"><span data-stu-id="2b638-167">The quotes are deleted by removing them one at a time.</span></span> <span data-ttu-id="2b638-168">Zawsze oferty zostanie usunięty, `QuoteDeleted` wywoływana jest metoda rejestratora.</span><span class="sxs-lookup"><span data-stu-id="2b638-168">Each time a quote is deleted, the `QuoteDeleted` method is called on the logger.</span></span> <span data-ttu-id="2b638-169">Zakres dziennika zostanie dodany do tych wiadomości dziennika.</span><span class="sxs-lookup"><span data-stu-id="2b638-169">A log scope is added to these log messages.</span></span>

<span data-ttu-id="2b638-170">Włącz `IncludeScopes` w opcjach rejestratora konsoli:</span><span class="sxs-lookup"><span data-stu-id="2b638-170">Enable `IncludeScopes` in the console logger options:</span></span>

[!code-csharp[Main](loggermessage/sample/Program.cs?name=snippet1&highlight=22)]

<span data-ttu-id="2b638-171">Ustawienie `IncludeScopes` wymaganego do włączenia dziennika zakresów w aplikacjach ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="2b638-171">Setting `IncludeScopes` is required in ASP.NET Core 2.0 apps to enable log scopes.</span></span> <span data-ttu-id="2b638-172">Ustawienie `IncludeScopes` za pośrednictwem *appsettings* plików konfiguracyjnych jest funkcją, która ma zaplanowane dla wersji platformy ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="2b638-172">Setting `IncludeScopes` via *appsettings* configuration files is a feature that's planned for the ASP.NET Core 2.1 release.</span></span>

<span data-ttu-id="2b638-173">Przykładowa aplikacja czyści innych dostawców i dodaje filtry, aby zmniejszyć dane wyjściowe rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="2b638-173">The sample app clears other providers and adds filters to reduce the logging output.</span></span> <span data-ttu-id="2b638-174">Ułatwia zobaczyć przykładowy komunikatów dziennika, które pokazują `LoggerMessage` funkcji.</span><span class="sxs-lookup"><span data-stu-id="2b638-174">This makes it easier to see the sample's log messages that demonstrate `LoggerMessage` features.</span></span>

<span data-ttu-id="2b638-175">Aby utworzyć zakres dziennika, Dodaj pole do przechowywania `Func` delegowanie dla zakresu.</span><span class="sxs-lookup"><span data-stu-id="2b638-175">To create a log scope, add a field to hold a `Func` delegate for the scope.</span></span> <span data-ttu-id="2b638-176">Przykładowa aplikacja tworzy pole o nazwie `_allQuotesDeletedScope` (*Internal/LoggerExtensions.cs*):</span><span class="sxs-lookup"><span data-stu-id="2b638-176">The sample app creates a field called `_allQuotesDeletedScope` (*Internal/LoggerExtensions.cs*):</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet4)]

<span data-ttu-id="2b638-177">Użyj `DefineScope` można utworzyć obiektu delegowanego.</span><span class="sxs-lookup"><span data-stu-id="2b638-177">Use `DefineScope` to create the delegate.</span></span> <span data-ttu-id="2b638-178">Maksymalnie trzy typy można określić do użycia jako argumenty szablonu po wywołaniu obiektu delegowanego.</span><span class="sxs-lookup"><span data-stu-id="2b638-178">Up to three types can be specified for use as template arguments when the delegate is invoked.</span></span> <span data-ttu-id="2b638-179">Przykładowa aplikacja korzysta z szablonu wiadomości, który zawiera liczby cudzysłowów usunięto ( `int` typu):</span><span class="sxs-lookup"><span data-stu-id="2b638-179">The sample app uses a message template that includes the number of deleted quotes (an `int` type):</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet8)]

<span data-ttu-id="2b638-180">Udostępnia metody statyczne rozszerzenie wiadomości dziennika.</span><span class="sxs-lookup"><span data-stu-id="2b638-180">Provide a static extension method for the log message.</span></span> <span data-ttu-id="2b638-181">Uwzględnij wszystkie parametry typu dla nazwanych właściwości, które pojawiają się w szablon wiadomości.</span><span class="sxs-lookup"><span data-stu-id="2b638-181">Include any type parameters for named properties that appear in the message template.</span></span> <span data-ttu-id="2b638-182">Przykładowa aplikacja przyjmuje `count` cudzysłowy do usunięcia i zwraca `_allQuotesDeletedScope`:</span><span class="sxs-lookup"><span data-stu-id="2b638-182">The sample app takes in a `count` of quotes to delete and returns `_allQuotesDeletedScope`:</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet12)]

<span data-ttu-id="2b638-183">Zawija zakres odwołuje się do rozszerzenia rejestrowania `using` bloku:</span><span class="sxs-lookup"><span data-stu-id="2b638-183">The scope wraps the logging extension calls in a `using` block:</span></span>

[!code-csharp[Main](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet4&highlight=5-6,14)]

<span data-ttu-id="2b638-184">Sprawdź, czy w komunikatach w dzienniku aplikacji konsoli w danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="2b638-184">Inspect the log messages in the app's console output.</span></span> <span data-ttu-id="2b638-185">Następujące wyniki przedstawiono trzy cudzysłowy usunięte z komunikatem zakresu dziennika uwzględnione:</span><span class="sxs-lookup"><span data-stu-id="2b638-185">The following result shows three quotes deleted with the log scope message included:</span></span>

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

## <a name="see-also"></a><span data-ttu-id="2b638-186">Zobacz także</span><span class="sxs-lookup"><span data-stu-id="2b638-186">See also</span></span>

* [<span data-ttu-id="2b638-187">Rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="2b638-187">Logging</span></span>](xref:fundamentals/logging/index)
