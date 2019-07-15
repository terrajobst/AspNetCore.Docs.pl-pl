---
title: Test sieci web, interfejsów API za pomocą HTTP REPL
author: scottaddie
description: Dowiedz się, jak przeglądanie i testu internetowego interfejsu API platformy ASP.NET Core za pomocą narzędzia HTTP REPL środowiska .NET Core globalnego.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 07/12/2019
uid: web-api/http-repl
ms.openlocfilehash: 920561fc86ed90eef64af49fa5936a080a096de2
ms.sourcegitcommit: 040aedca220ed24ee1726e6886daf6906f95a028
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/15/2019
ms.locfileid: "67919262"
---
# <a name="test-web-apis-with-the-http-repl"></a><span data-ttu-id="83cea-103">Test sieci web, interfejsów API za pomocą HTTP REPL</span><span class="sxs-lookup"><span data-stu-id="83cea-103">Test web APIs with the HTTP REPL</span></span>

<span data-ttu-id="83cea-104">Przez [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="83cea-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="83cea-105">Jest pętlą odczytu HTTP — Eval-Print (REPL):</span><span class="sxs-lookup"><span data-stu-id="83cea-105">The HTTP Read-Eval-Print Loop (REPL) is:</span></span>

* <span data-ttu-id="83cea-106">Lekkie, Międzyplatformowe narzędzie wiersza polecenia, które ma obsługiwane wszędzie, gdzie platformy .NET Core jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="83cea-106">A lightweight, cross-platform command-line tool that's supported everywhere .NET Core is supported.</span></span>
* <span data-ttu-id="83cea-107">Używany do wysyłania żądań HTTP do testowania interfejsów API sieci web platformy ASP.NET Core i wyświetlania ich wyników.</span><span class="sxs-lookup"><span data-stu-id="83cea-107">Used for making HTTP requests to test ASP.NET Core web APIs and view their results.</span></span>

<span data-ttu-id="83cea-108">Następujące [zleceń HTTP](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#74-supported-methods) są obsługiwane:</span><span class="sxs-lookup"><span data-stu-id="83cea-108">The following [HTTP verbs](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#74-supported-methods) are supported:</span></span>

* [<span data-ttu-id="83cea-109">USUŃ</span><span class="sxs-lookup"><span data-stu-id="83cea-109">DELETE</span></span>](#test-http-delete-requests)
* [<span data-ttu-id="83cea-110">GET</span><span class="sxs-lookup"><span data-stu-id="83cea-110">GET</span></span>](#test-http-get-requests)
* [<span data-ttu-id="83cea-111">GŁÓWNY</span><span class="sxs-lookup"><span data-stu-id="83cea-111">HEAD</span></span>](#test-http-head-requests)
* [<span data-ttu-id="83cea-112">OPTIONS</span><span class="sxs-lookup"><span data-stu-id="83cea-112">OPTIONS</span></span>](#test-http-options-requests)
* [<span data-ttu-id="83cea-113">POPRAWKI</span><span class="sxs-lookup"><span data-stu-id="83cea-113">PATCH</span></span>](#test-http-patch-requests)
* [<span data-ttu-id="83cea-114">POST</span><span class="sxs-lookup"><span data-stu-id="83cea-114">POST</span></span>](#test-http-post-requests)
* [<span data-ttu-id="83cea-115">PUT</span><span class="sxs-lookup"><span data-stu-id="83cea-115">PUT</span></span>](#test-http-put-requests)

<span data-ttu-id="83cea-116">Aby kontynuować, [wyświetlanie lub pobieranie przykładowego interfejsu API sieci web platformy ASP.NET Core](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/http-repl/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="83cea-116">To follow along, [view or download the sample ASP.NET Core web API](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/http-repl/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="83cea-117">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="83cea-117">Prerequisites</span></span>

* [!INCLUDE [2.1-SDK](~/includes/2.1-SDK.md)]

## <a name="installation"></a><span data-ttu-id="83cea-118">Instalacja</span><span class="sxs-lookup"><span data-stu-id="83cea-118">Installation</span></span>

<span data-ttu-id="83cea-119">Aby zainstalować HTTP REPL, uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="83cea-119">To install the HTTP REPL, run the following command:</span></span>

```console
dotnet tool install -g dotnet-httprepl
    --version 2.2.0-*
    --add-source https://dotnet.myget.org/F/dotnet-core/api/v3/index.json
```

<span data-ttu-id="83cea-120">A [narzędzia programu .NET Core globalnego](/dotnet/core/tools/global-tools#install-a-global-tool) jest instalowany z [dotnet httprepl](https://dotnet.myget.org/feed/dotnet-core/package/nuget/dotnet-httprepl
) pakietu NuGet hostowanymi na platformie MyGet.</span><span class="sxs-lookup"><span data-stu-id="83cea-120">A [.NET Core Global Tool](/dotnet/core/tools/global-tools#install-a-global-tool) is installed from the [dotnet-httprepl](https://dotnet.myget.org/feed/dotnet-core/package/nuget/dotnet-httprepl
) NuGet package hosted on MyGet.</span></span>

## <a name="usage"></a><span data-ttu-id="83cea-121">Użycie</span><span class="sxs-lookup"><span data-stu-id="83cea-121">Usage</span></span>

<span data-ttu-id="83cea-122">Po pomyślnej instalacji tego narzędzia uruchom następujące polecenie, aby uruchomić HTTP REPL:</span><span class="sxs-lookup"><span data-stu-id="83cea-122">After successful installation of the tool, run the following command to start the HTTP REPL:</span></span>

```console
dotnet httprepl
```

<span data-ttu-id="83cea-123">Aby wyświetlić dostępne polecenia HTTP REPL, uruchom jedną z następujących poleceń:</span><span class="sxs-lookup"><span data-stu-id="83cea-123">To view the available HTTP REPL commands, run one of the following commands:</span></span>

```console
dotnet httprepl -h
```

```console
dotnet httprepl --help
```

<span data-ttu-id="83cea-124">Są wyświetlane następujące dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="83cea-124">The following output is displayed:</span></span>

```console
Usage: dotnet httprepl [<BASE_ADDRESS>] [options]

Arguments:
  <BASE_ADDRESS> - The initial base address for the REPL.

Options:
  --help - Show help information.

REPL Commands:

HTTP Commands:
Use these commands to execute requests against your application.

GET            Issues a GET request.
POST           Issues a POST request.
PUT            Issues a PUT request.
DELETE         Issues a DELETE request.
PATCH          Issues a PATCH request.
HEAD           Issues a HEAD request.
OPTIONS        Issues an OPTIONS request.

set header     Sets or clears a header for all requests. e.g. `set header content-type application/json`


Navigation Commands:
The REPL allows you to navigate your URL space and focus on specific APIs that you are working on.

set base       Set the base URI. e.g. `set base http://locahost:5000`
set swagger    Set the URI, relative to your base if set, of the Swagger document for this API. e.g. `set swagger /swagger/v1/swagger.json`
ls             Show all endpoints for the current path.
cd             Append the given directory to the currently selected path, or move up a path when using `cd ..`.

Shell Commands:
Use these commands to interact with the REPL shell.

clear          Removes all text from the shell.
echo [on/off]  Turns request echoing on or off, show the request that was made when using request commands.
exit           Exit the shell.

REPL Customization Commands:
Use these commands to customize the REPL behavior.

pref [get/set] Allows viewing or changing preferences, e.g. 'pref set editor.command.default 'C:\Program Files\Microsoft VS Code\Code.exe'`
run            Runs the script at the given path. A script is a set of commands that can be typed with one command per line.
ui             Displays the Swagger UI page, if available, in the default browser.

Use help <COMMAND> to learn more details about individual commands. e.g. `help get`
```

<span data-ttu-id="83cea-125">HTTP REPL oferuje ukończenie polecenia.</span><span class="sxs-lookup"><span data-stu-id="83cea-125">The HTTP REPL offers command completion.</span></span> <span data-ttu-id="83cea-126">Naciśnięcie klawisza <kbd>kartę</kbd> klucza, który wykonuje iterację przez listę poleceń, kończące znaki lub punkt końcowy interfejsu API, która została wpisana.</span><span class="sxs-lookup"><span data-stu-id="83cea-126">Pressing the <kbd>Tab</kbd> key iterates through the list of commands that complete the characters or API endpoint that you typed.</span></span> <span data-ttu-id="83cea-127">W poniższych sekcjach opisano dostępne polecenia interfejsu wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="83cea-127">The following sections outline the available CLI commands.</span></span>

## <a name="connect-to-the-web-api"></a><span data-ttu-id="83cea-128">Połącz się z internetowym interfejsem API</span><span class="sxs-lookup"><span data-stu-id="83cea-128">Connect to the web API</span></span>

<span data-ttu-id="83cea-129">Podłącz do internetowego interfejsu API, uruchamiając następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="83cea-129">Connect to a web API by running the following command:</span></span>

```console
dotnet httprepl <BASE URI>
```

<span data-ttu-id="83cea-130">`<BASE URI>` jest podstawowy identyfikator URI dla internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="83cea-130">`<BASE URI>` is the base URI for the web API.</span></span> <span data-ttu-id="83cea-131">Przykład:</span><span class="sxs-lookup"><span data-stu-id="83cea-131">For example:</span></span>

```console
dotnet httprepl https://localhost:5001
```

<span data-ttu-id="83cea-132">Alternatywnie uruchom następujące polecenie w dowolnym momencie po uruchomieniu HTTP REPL:</span><span class="sxs-lookup"><span data-stu-id="83cea-132">Alternatively, run the following command at any time while the HTTP REPL is running:</span></span>

```console
set base <BASE URI>
```

<span data-ttu-id="83cea-133">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="83cea-133">For example:</span></span>

```console
(Disconnected)~ set base https://localhost:5001
```

## <a name="point-to-the-swagger-document-for-the-web-api"></a><span data-ttu-id="83cea-134">Wskaż dokument struktury Swagger dla interfejsu API sieci web</span><span class="sxs-lookup"><span data-stu-id="83cea-134">Point to the Swagger document for the web API</span></span>

<span data-ttu-id="83cea-135">Aby prawidłowo sprawdzić interfejsu API sieci web, należy ustawić względny identyfikator URI do dokumentu Swagger dla interfejsu API sieci web.</span><span class="sxs-lookup"><span data-stu-id="83cea-135">To properly inspect the web API, set the relative URI to the Swagger document for the web API.</span></span> <span data-ttu-id="83cea-136">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="83cea-136">Run the following command:</span></span>

```console
set swagger <RELATIVE URI>
```

<span data-ttu-id="83cea-137">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="83cea-137">For example:</span></span>

```console
https://localhost:5001/~ set swagger /swagger/v1/swagger.json
```

## <a name="navigate-the-web-api"></a><span data-ttu-id="83cea-138">Przejdź do interfejsu API sieci web</span><span class="sxs-lookup"><span data-stu-id="83cea-138">Navigate the web API</span></span>

### <a name="view-available-endpoints"></a><span data-ttu-id="83cea-139">Wyświetl dostępne punkty końcowe</span><span class="sxs-lookup"><span data-stu-id="83cea-139">View available endpoints</span></span>

<span data-ttu-id="83cea-140">Aby wyświetlić listę różnych punktów końcowych (kontrolery) w bieżącej ścieżce adresu interfejsu API sieci web, należy uruchomić `ls` lub `dir` polecenia:</span><span class="sxs-lookup"><span data-stu-id="83cea-140">To list the different endpoints (controllers) at the current path of the web API address, run the `ls` or `dir` command:</span></span>

```console
https://localhot:5001/~ ls
```

<span data-ttu-id="83cea-141">Zostanie wyświetlony następujący format danych wyjściowych:</span><span class="sxs-lookup"><span data-stu-id="83cea-141">The following output format is displayed:</span></span>

```console
.        []
Fruits   [get|post]
People   [get|post]

https://localhost:5001/~
```

<span data-ttu-id="83cea-142">Dane wyjściowe poprzedniego oznacza, że są dwa kontrolery: `Fruits` i `People`.</span><span class="sxs-lookup"><span data-stu-id="83cea-142">The preceding output indicates that there are two controllers available: `Fruits` and `People`.</span></span> <span data-ttu-id="83cea-143">Oba kontrolery obsługuje bez parametrów operacji HTTP GET i POST.</span><span class="sxs-lookup"><span data-stu-id="83cea-143">Both controllers support parameterless HTTP GET and POST operations.</span></span>

<span data-ttu-id="83cea-144">Przejdź do określonego kontrolera, co spowoduje wyświetlenie bardziej szczegółowo.</span><span class="sxs-lookup"><span data-stu-id="83cea-144">Navigating into a specific controller reveals more detail.</span></span> <span data-ttu-id="83cea-145">Na przykład następujące polecenie na dane wyjściowe `Fruits` kontroler obsługuje również operacje HTTP GET, PUT i DELETE.</span><span class="sxs-lookup"><span data-stu-id="83cea-145">For example, the following command's output shows the `Fruits` controller also supports HTTP GET, PUT, and DELETE operations.</span></span> <span data-ttu-id="83cea-146">Każda z tych operacji oczekuje `id` parametr trasy:</span><span class="sxs-lookup"><span data-stu-id="83cea-146">Each of these operations expects an `id` parameter in the route:</span></span>

```console
https://localhost:5001/fruits~ ls
.      [get|post]
..     []
{id}   [get|put|delete]

https://localhost:5001/fruits~
```

<span data-ttu-id="83cea-147">Możesz też uruchomić `ui` polecenie, aby otworzyć stronę interfejs użytkownika struktury Swagger interfejsu API sieci web w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="83cea-147">Alternatively, run the `ui` command to open the web API's Swagger UI page in a browser.</span></span> <span data-ttu-id="83cea-148">Przykład:</span><span class="sxs-lookup"><span data-stu-id="83cea-148">For example:</span></span>

```console
https://localhost:5001/~ ui
```

### <a name="navigate-to-an-endpoint"></a><span data-ttu-id="83cea-149">Przejdź do punktu końcowego</span><span class="sxs-lookup"><span data-stu-id="83cea-149">Navigate to an endpoint</span></span>

<span data-ttu-id="83cea-150">Aby przejść do innego punktu końcowego na interfejs API sieci web, należy uruchomić `cd` polecenia:</span><span class="sxs-lookup"><span data-stu-id="83cea-150">To navigate to a different endpoint on the web API, run the `cd` command:</span></span>

```console
https://localhost:5001/~ cd people
```

<span data-ttu-id="83cea-151">Następujące ścieżki `cd` polecenia jest uwzględniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="83cea-151">The path following the `cd` command is case insensitive.</span></span> <span data-ttu-id="83cea-152">Zostanie wyświetlony następujący format danych wyjściowych:</span><span class="sxs-lookup"><span data-stu-id="83cea-152">The following output format is displayed:</span></span>

```console
/people    [get|post]

https://localhost:5001/people~
```

## <a name="customize-the-http-repl"></a><span data-ttu-id="83cea-153">Dostosowywanie HTTP REPL</span><span class="sxs-lookup"><span data-stu-id="83cea-153">Customize the HTTP REPL</span></span>

<span data-ttu-id="83cea-154">Domyślne HTTP REPL [kolory](#set-color-preferences) można dostosować.</span><span class="sxs-lookup"><span data-stu-id="83cea-154">The HTTP REPL's default [colors](#set-color-preferences) can be customized.</span></span> <span data-ttu-id="83cea-155">Ponadto [domyślny edytor tekstu](#set-the-default-text-editor) można zdefiniować.</span><span class="sxs-lookup"><span data-stu-id="83cea-155">Additionally, a [default text editor](#set-the-default-text-editor) can be defined.</span></span> <span data-ttu-id="83cea-156">Preferencje HTTP REPL są utrwalane w bieżącej sesji i są uwzględniane w przyszłych sesjach.</span><span class="sxs-lookup"><span data-stu-id="83cea-156">The HTTP REPL preferences are persisted across the current session and are honored in future sessions.</span></span> <span data-ttu-id="83cea-157">Po zmodyfikowaniu preferencje są przechowywane w następującym pliku:</span><span class="sxs-lookup"><span data-stu-id="83cea-157">Once modified, the preferences are stored in the following file:</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="83cea-158">Linux</span><span class="sxs-lookup"><span data-stu-id="83cea-158">Linux</span></span>](#tab/linux)

<span data-ttu-id="83cea-159">*%HOME%/.httpreplprefs*</span><span class="sxs-lookup"><span data-stu-id="83cea-159">*%HOME%/.httpreplprefs*</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="83cea-160">macOS</span><span class="sxs-lookup"><span data-stu-id="83cea-160">macOS</span></span>](#tab/macos)

<span data-ttu-id="83cea-161">*%HOME%/.httpreplprefs*</span><span class="sxs-lookup"><span data-stu-id="83cea-161">*%HOME%/.httpreplprefs*</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="83cea-162">Windows</span><span class="sxs-lookup"><span data-stu-id="83cea-162">Windows</span></span>](#tab/windows)

<span data-ttu-id="83cea-163">*%USERPROFILE%\\.httpreplprefs*</span><span class="sxs-lookup"><span data-stu-id="83cea-163">*%USERPROFILE%\\.httpreplprefs*</span></span>

---

<span data-ttu-id="83cea-164">*.Httpreplprefs* plik jest ładowany podczas uruchamiania i nie są monitorowane pod kątem zmian w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="83cea-164">The *.httpreplprefs* file is loaded on startup and not monitored for changes at runtime.</span></span> <span data-ttu-id="83cea-165">Ręcznej modyfikacje pliku obowiązywać dopiero po ponownym uruchomieniu narzędzia.</span><span class="sxs-lookup"><span data-stu-id="83cea-165">Manual modifications to the file take effect only after restarting the tool.</span></span>

### <a name="view-the-settings"></a><span data-ttu-id="83cea-166">Wyświetl ustawienia</span><span class="sxs-lookup"><span data-stu-id="83cea-166">View the settings</span></span>

<span data-ttu-id="83cea-167">Aby wyświetlić dostępnych ustawień, uruchom `pref get` polecenia.</span><span class="sxs-lookup"><span data-stu-id="83cea-167">To view the available settings, run the `pref get` command.</span></span> <span data-ttu-id="83cea-168">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="83cea-168">For example:</span></span>

```console
https://localhost:5001/~ pref get
```

<span data-ttu-id="83cea-169">Poprzednie polecenie wyświetli dostępne pary klucz wartość:</span><span class="sxs-lookup"><span data-stu-id="83cea-169">The preceding command displays the available key-value pairs:</span></span>

```console
colors.json=Green
colors.json.arrayBrace=BoldCyan
colors.json.comma=BoldYellow
colors.json.name=BoldMagenta
colors.json.nameSeparator=BoldWhite
colors.json.objectBrace=Cyan
colors.protocol=BoldGreen
colors.status=BoldYellow
```

### <a name="set-color-preferences"></a><span data-ttu-id="83cea-170">Ustawianie preferencji koloru</span><span class="sxs-lookup"><span data-stu-id="83cea-170">Set color preferences</span></span>

<span data-ttu-id="83cea-171">Kolorowanie odpowiedzi obecnie obsługiwany tylko dla formatu JSON.</span><span class="sxs-lookup"><span data-stu-id="83cea-171">Response colorization is currently supported for JSON only.</span></span> <span data-ttu-id="83cea-172">Aby dostosować domyślne narzędzie HTTP REPL, kolorowanie, zlokalizuj klucz odpowiadający kolorów, które mają być zmienione.</span><span class="sxs-lookup"><span data-stu-id="83cea-172">To customize the default HTTP REPL tool coloring, locate the key corresponding to the color to be changed.</span></span> <span data-ttu-id="83cea-173">Aby uzyskać instrukcje na temat znajdowania kluczy, zobacz [wyświetlić ustawienia](#view-the-settings) sekcji.</span><span class="sxs-lookup"><span data-stu-id="83cea-173">For instructions on how to find the keys, see the [View the settings](#view-the-settings) section.</span></span> <span data-ttu-id="83cea-174">Na przykład zmienić `colors.json` wartość z klucza `Green` do `White` w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="83cea-174">For example, change the `colors.json` key value from `Green` to `White` as follows:</span></span>

```console
https://localhost:5001/people~ pref set colors.json White
```

<span data-ttu-id="83cea-175">Tylko [dozwolone kolory](https://github.com/aspnet/HttpRepl/blob/01d5c3c3373e98fe566ff5ef8a17c571de880293/src/Microsoft.Repl/ConsoleHandling/AllowedColors.cs) mogą być używane.</span><span class="sxs-lookup"><span data-stu-id="83cea-175">Only the [allowed colors](https://github.com/aspnet/HttpRepl/blob/01d5c3c3373e98fe566ff5ef8a17c571de880293/src/Microsoft.Repl/ConsoleHandling/AllowedColors.cs) may be used.</span></span> <span data-ttu-id="83cea-176">Kolejne HTTP żądania wyświetlania danych wyjściowych za pomocą nowego kolorowanie.</span><span class="sxs-lookup"><span data-stu-id="83cea-176">Subsequent HTTP requests display output with the new coloring.</span></span>

<span data-ttu-id="83cea-177">Gdy określony kolor klucze nie są ustawione, są uważane za klucze bardziej ogólnymi.</span><span class="sxs-lookup"><span data-stu-id="83cea-177">When specific color keys aren't set, more generic keys are considered.</span></span> <span data-ttu-id="83cea-178">Aby zademonstrować to działanie rezerwowe, rozważmy następujący przykład:</span><span class="sxs-lookup"><span data-stu-id="83cea-178">To demonstrate this fallback behavior, consider the following example:</span></span>

* <span data-ttu-id="83cea-179">Jeśli `colors.json.name` nie ma wartości `colors.json.string` jest używany.</span><span class="sxs-lookup"><span data-stu-id="83cea-179">If `colors.json.name` doesn't have a value, `colors.json.string` is used.</span></span>
* <span data-ttu-id="83cea-180">Jeśli `colors.json.string` nie ma wartości `colors.json.literal` jest używany.</span><span class="sxs-lookup"><span data-stu-id="83cea-180">If `colors.json.string` doesn't have a value, `colors.json.literal` is used.</span></span>
* <span data-ttu-id="83cea-181">Jeśli `colors.json.literal` nie ma wartości `colors.json` jest używany.</span><span class="sxs-lookup"><span data-stu-id="83cea-181">If `colors.json.literal` doesn't have a value, `colors.json` is used.</span></span> 
* <span data-ttu-id="83cea-182">Jeśli `colors.json` nie ma wartości, powłoki poleceń domyślny kolor tekstu (`AllowedColors.None`) jest używany.</span><span class="sxs-lookup"><span data-stu-id="83cea-182">If `colors.json` doesn't have a value, the command shell's default text color (`AllowedColors.None`) is used.</span></span>

### <a name="set-indentation-size"></a><span data-ttu-id="83cea-183">Ustaw rozmiar wcięć</span><span class="sxs-lookup"><span data-stu-id="83cea-183">Set indentation size</span></span>

<span data-ttu-id="83cea-184">Dostosowywanie rozmiaru wcięcia odpowiedzi obecnie obsługiwany tylko dla formatu JSON.</span><span class="sxs-lookup"><span data-stu-id="83cea-184">Response indentation size customization is currently supported for JSON only.</span></span> <span data-ttu-id="83cea-185">Domyślny rozmiar to dwa miejsca do magazynowania.</span><span class="sxs-lookup"><span data-stu-id="83cea-185">The default size is two spaces.</span></span> <span data-ttu-id="83cea-186">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="83cea-186">For example:</span></span>

```json
[
  {
    "id": 1,
    "name": "Apple"
  },
  {
    "id": 2,
    "name": "Orange"
  },
  {
    "id": 3,
    "name": "Strawberry"
  }
]
```

<span data-ttu-id="83cea-187">Aby zmienić domyślny rozmiar, należy ustawić `formatting.json.indentSize` klucza.</span><span class="sxs-lookup"><span data-stu-id="83cea-187">To change the default size, set the `formatting.json.indentSize` key.</span></span> <span data-ttu-id="83cea-188">Na przykład aby zawsze użycie czterech spacji:</span><span class="sxs-lookup"><span data-stu-id="83cea-188">For example, to always use four spaces:</span></span>

```console
pref set formatting.json.indentSize 4
```

<span data-ttu-id="83cea-189">Kolejne odpowiedzi uwzględnić ustawienie czterech miejsc do magazynowania:</span><span class="sxs-lookup"><span data-stu-id="83cea-189">Subsequent responses honor the setting of four spaces:</span></span>

```json
[
    {
        "id": 1,
        "name": "Apple"
    },
    {
        "id": 2,
        "name": "Orange"
    },
    {
        "id": 3,
        "name": "Strawberry"
    }
]
```

### <a name="set-indentation-size"></a><span data-ttu-id="83cea-190">Ustaw rozmiar wcięć</span><span class="sxs-lookup"><span data-stu-id="83cea-190">Set indentation size</span></span>

<span data-ttu-id="83cea-191">Dostosowywanie rozmiaru wcięcia odpowiedzi obecnie obsługiwany tylko dla formatu JSON.</span><span class="sxs-lookup"><span data-stu-id="83cea-191">Response indentation size customization is currently supported for JSON only.</span></span> <span data-ttu-id="83cea-192">Domyślny rozmiar to dwa miejsca do magazynowania.</span><span class="sxs-lookup"><span data-stu-id="83cea-192">The default size is two spaces.</span></span> <span data-ttu-id="83cea-193">Przykład:</span><span class="sxs-lookup"><span data-stu-id="83cea-193">For example:</span></span>

```json
[
  {
    "id": 1,
    "name": "Apple"
  },
  {
    "id": 2,
    "name": "Orange"
  },
  {
    "id": 3,
    "name": "Strawberry"
  }
]
```

<span data-ttu-id="83cea-194">Aby zmienić domyślny rozmiar, należy ustawić `formatting.json.indentSize` klucza.</span><span class="sxs-lookup"><span data-stu-id="83cea-194">To change the default size, set the `formatting.json.indentSize` key.</span></span> <span data-ttu-id="83cea-195">Na przykład aby zawsze użycie czterech spacji:</span><span class="sxs-lookup"><span data-stu-id="83cea-195">For example, to always use four spaces:</span></span>

```console
pref set formatting.json.indentSize 4
```

<span data-ttu-id="83cea-196">Kolejne odpowiedzi uwzględnić ustawienie czterech miejsc do magazynowania:</span><span class="sxs-lookup"><span data-stu-id="83cea-196">Subsequent responses honor the setting of four spaces:</span></span>

```json
[
    {
        "id": 1,
        "name": "Apple"
    },
    {
        "id": 2,
        "name": "Orange"
    },
    {
        "id": 3,
        "name": "Strawberry"
    }
]
```

### <a name="set-the-default-text-editor"></a><span data-ttu-id="83cea-197">Ustaw domyślny edytor tekstu</span><span class="sxs-lookup"><span data-stu-id="83cea-197">Set the default text editor</span></span>

<span data-ttu-id="83cea-198">Domyślnie HTTP REPL ma nie Edytor tekstu, skonfigurowany do użycia.</span><span class="sxs-lookup"><span data-stu-id="83cea-198">By default, the HTTP REPL has no text editor configured for use.</span></span> <span data-ttu-id="83cea-199">Aby przetestować metody interfejsu API sieci web wymaga treści żądania HTTP, można ustawić domyślnego edytora tekstu.</span><span class="sxs-lookup"><span data-stu-id="83cea-199">To test web API methods requiring an HTTP request body, a default text editor must be set.</span></span> <span data-ttu-id="83cea-200">HTTP REPL zostanie uruchomione narzędzie do edytora tekstów skonfigurowane wyłącznie w celu tworzenia treści żądania.</span><span class="sxs-lookup"><span data-stu-id="83cea-200">The HTTP REPL tool launches the configured text editor for the sole purpose of composing the request body.</span></span> <span data-ttu-id="83cea-201">Uruchom następujące polecenie, aby ustawić preferowanym edytorze tekstu jako domyślny:</span><span class="sxs-lookup"><span data-stu-id="83cea-201">Run the following command to set your preferred text editor as the default:</span></span>

```console
pref set editor.command.default "<EXECUTABLE>"
```

<span data-ttu-id="83cea-202">W poprzednim poleceniu `<EXECUTABLE>` jest pełną ścieżką do pliku wykonywalnego w edytorze tekstów.</span><span class="sxs-lookup"><span data-stu-id="83cea-202">In the preceding command, `<EXECUTABLE>` is the full path to the text editor's executable file.</span></span> <span data-ttu-id="83cea-203">Na przykład uruchom następujące polecenie, aby ustawić Visual Studio Code jako domyślny edytor tekstu:</span><span class="sxs-lookup"><span data-stu-id="83cea-203">For example, run the following command to set Visual Studio Code as the default text editor:</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="83cea-204">Linux</span><span class="sxs-lookup"><span data-stu-id="83cea-204">Linux</span></span>](#tab/linux)

```console
pref set editor.command.default "/usr/bin/code"
```

# <a name="macostabmacos"></a>[<span data-ttu-id="83cea-205">macOS</span><span class="sxs-lookup"><span data-stu-id="83cea-205">macOS</span></span>](#tab/macos)

```console
pref set editor.command.default "/Applications/Visual Studio Code.app/Contents/Resources/app/bin/code"
```

# <a name="windowstabwindows"></a>[<span data-ttu-id="83cea-206">Windows</span><span class="sxs-lookup"><span data-stu-id="83cea-206">Windows</span></span>](#tab/windows)

```console
pref set editor.command.default "C:\Program Files\Microsoft VS Code\Code.exe"
```

---

<span data-ttu-id="83cea-207">Aby uruchomić domyślny edytor tekstu przy użyciu określonych argumentów wiersza polecenia platformy, należy ustawić `editor.command.default.arguments` klucza.</span><span class="sxs-lookup"><span data-stu-id="83cea-207">To launch the default text editor with specific CLI arguments, set the `editor.command.default.arguments` key.</span></span> <span data-ttu-id="83cea-208">Załóżmy na przykład, Visual Studio Code to domyślny edytor tekstu i zawsze ma REPL HTTP, które można otworzyć programu Visual Studio Code w nowej sesji przy użyciu rozszerzeń wyłączone.</span><span class="sxs-lookup"><span data-stu-id="83cea-208">For example, assume Visual Studio Code is the default text editor and that you always want the HTTP REPL to open Visual Studio Code in a new session with extensions disabled.</span></span> <span data-ttu-id="83cea-209">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="83cea-209">Run the following command:</span></span>

```console
pref set editor.command.default.arguments "--disable-extensions --new-window"
```

## <a name="test-http-get-requests"></a><span data-ttu-id="83cea-210">Testowanie żądania HTTP GET</span><span class="sxs-lookup"><span data-stu-id="83cea-210">Test HTTP GET requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="83cea-211">Streszczenie</span><span class="sxs-lookup"><span data-stu-id="83cea-211">Synopsis</span></span>

```console
get <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="83cea-212">Argumenty</span><span class="sxs-lookup"><span data-stu-id="83cea-212">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="83cea-213">Parametr trasy, oczekiwany przez metodę akcji kontrolera skojarzone.</span><span class="sxs-lookup"><span data-stu-id="83cea-213">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="83cea-214">Opcje</span><span class="sxs-lookup"><span data-stu-id="83cea-214">Options</span></span>

<span data-ttu-id="83cea-215">Poniższe opcje są dostępne dla `get` polecenia:</span><span class="sxs-lookup"><span data-stu-id="83cea-215">The following options are available for the `get` command:</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

### <a name="example"></a><span data-ttu-id="83cea-216">Przykład</span><span class="sxs-lookup"><span data-stu-id="83cea-216">Example</span></span>

<span data-ttu-id="83cea-217">Wysłanie żądania HTTP GET:</span><span class="sxs-lookup"><span data-stu-id="83cea-217">To issue an HTTP GET request:</span></span>

1. <span data-ttu-id="83cea-218">Uruchom `get` polecenia w punkcie końcowym, który ją obsługuje:</span><span class="sxs-lookup"><span data-stu-id="83cea-218">Run the `get` command on an endpoint that supports it:</span></span>

    ```console
    https://localhost:5001/people~ get
    ```

    <span data-ttu-id="83cea-219">Poprzednie polecenie wyświetli następujący format danych wyjściowych:</span><span class="sxs-lookup"><span data-stu-id="83cea-219">The preceding command displays the following output format:</span></span>

    ```console
    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8
    Date: Fri, 21 Jun 2019 03:38:45 GMT
    Server: Kestrel
    Transfer-Encoding: chunked

    [
      {
        "id": 1,
        "name": "Scott Hunter"
      },
      {
        "id": 2,
        "name": "Scott Hanselman"
      },
      {
        "id": 3,
        "name": "Scott Guthrie"
      }
    ]


    https://localhost:5001/people~
    ```

1. <span data-ttu-id="83cea-220">Pobieranie określonego rekordu przez przekazanie parametru, aby `get` polecenia:</span><span class="sxs-lookup"><span data-stu-id="83cea-220">Retrieve a specific record by passing a parameter to the `get` command:</span></span>

    ```console
    https://localhost:5001/people~ get 2
    ```

    <span data-ttu-id="83cea-221">Poprzednie polecenie wyświetli następujący format danych wyjściowych:</span><span class="sxs-lookup"><span data-stu-id="83cea-221">The preceding command displays the following output format:</span></span>

    ```console
    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8
    Date: Fri, 21 Jun 2019 06:17:57 GMT
    Server: Kestrel
    Transfer-Encoding: chunked

    [
      {
        "id": 2,
        "name": "Scott Hanselman"
      }
    ]


    https://localhost:5001/people~
    ```

## <a name="test-http-post-requests"></a><span data-ttu-id="83cea-222">Testowanie żądania HTTP POST</span><span class="sxs-lookup"><span data-stu-id="83cea-222">Test HTTP POST requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="83cea-223">Streszczenie</span><span class="sxs-lookup"><span data-stu-id="83cea-223">Synopsis</span></span>

```console
post <PARAMETER> [-c|--content] [-f|--file] [-h|--header] [--no-body] [-F|--no-formatting] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="83cea-224">Argumenty</span><span class="sxs-lookup"><span data-stu-id="83cea-224">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="83cea-225">Parametr trasy, oczekiwany przez metodę akcji kontrolera skojarzone.</span><span class="sxs-lookup"><span data-stu-id="83cea-225">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="83cea-226">Opcje</span><span class="sxs-lookup"><span data-stu-id="83cea-226">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

[!INCLUDE [HTTP request body CLI options](~/includes/http-repl/requires-body-options.md)]

### <a name="example"></a><span data-ttu-id="83cea-227">Przykład</span><span class="sxs-lookup"><span data-stu-id="83cea-227">Example</span></span>

<span data-ttu-id="83cea-228">Wysłanie żądania POST protokołu HTTP:</span><span class="sxs-lookup"><span data-stu-id="83cea-228">To issue an HTTP POST request:</span></span>

1. <span data-ttu-id="83cea-229">Uruchom `post` polecenia w punkcie końcowym, który ją obsługuje:</span><span class="sxs-lookup"><span data-stu-id="83cea-229">Run the `post` command on an endpoint that supports it:</span></span>

    ```console
    https://localhost:5001/people~ post -h Content-Type=application/json
    ```

    <span data-ttu-id="83cea-230">W poprzednim poleceniu `Content-Type` nagłówek żądania HTTP jest ustawiona, aby wskazać typ nośnika treści żądania JSON.</span><span class="sxs-lookup"><span data-stu-id="83cea-230">In the preceding command, the `Content-Type` HTTP request header is set to indicate a request body media type of JSON.</span></span> <span data-ttu-id="83cea-231">Zostanie otwarty Edytor tekstu domyślne *.tmp* pliku przy użyciu szablonu JSON reprezentująca treść żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="83cea-231">The default text editor opens a *.tmp* file with a JSON template representing the HTTP request body.</span></span> <span data-ttu-id="83cea-232">Przykład:</span><span class="sxs-lookup"><span data-stu-id="83cea-232">For example:</span></span>

    ```json
    {
      "id": 0,
      "name": ""
    }
    ```

    > [!TIP]
    > <span data-ttu-id="83cea-233">Aby ustawić domyślny edytor tekstu, zobacz [ustawiony domyślny edytor tekstu](#set-the-default-text-editor) sekcji.</span><span class="sxs-lookup"><span data-stu-id="83cea-233">To set the default text editor, see the [Set the default text editor](#set-the-default-text-editor) section.</span></span>

1. <span data-ttu-id="83cea-234">Zmodyfikuj szablon JSON w celu spełnienia wymagań weryfikacji modelu:</span><span class="sxs-lookup"><span data-stu-id="83cea-234">Modify the JSON template to satisfy model validation requirements:</span></span>

  ```json
  {
    "id": 0,
    "name": "Scott Addie"
  }
  ```

1. <span data-ttu-id="83cea-235">Zapisz *.tmp* plik i zamknij Edytor tekstu.</span><span class="sxs-lookup"><span data-stu-id="83cea-235">Save the *.tmp* file, and close the text editor.</span></span> <span data-ttu-id="83cea-236">Następujące dane wyjściowe pojawia się w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="83cea-236">The following output appears in the command shell:</span></span>

    ```console
    HTTP/1.1 201 Created
    Content-Type: application/json; charset=utf-8
    Date: Thu, 27 Jun 2019 21:24:18 GMT
    Location: https://localhost:5001/people/4
    Server: Kestrel
    Transfer-Encoding: chunked

    {
      "id": 4,
      "name": "Scott Addie"
    }


    https://localhost:5001/people~
    ```

## <a name="test-http-put-requests"></a><span data-ttu-id="83cea-237">Testowanie żądania HTTP PUT</span><span class="sxs-lookup"><span data-stu-id="83cea-237">Test HTTP PUT requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="83cea-238">Streszczenie</span><span class="sxs-lookup"><span data-stu-id="83cea-238">Synopsis</span></span>

```console
put <PARAMETER> [-c|--content] [-f|--file] [-h|--header] [--no-body] [-F|--no-formatting] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="83cea-239">Argumenty</span><span class="sxs-lookup"><span data-stu-id="83cea-239">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="83cea-240">Parametr trasy, oczekiwany przez metodę akcji kontrolera skojarzone.</span><span class="sxs-lookup"><span data-stu-id="83cea-240">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="83cea-241">Opcje</span><span class="sxs-lookup"><span data-stu-id="83cea-241">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

[!INCLUDE [HTTP request body CLI options](~/includes/http-repl/requires-body-options.md)]

### <a name="example"></a><span data-ttu-id="83cea-242">Przykład</span><span class="sxs-lookup"><span data-stu-id="83cea-242">Example</span></span>

<span data-ttu-id="83cea-243">Wysłanie żądania HTTP PUT:</span><span class="sxs-lookup"><span data-stu-id="83cea-243">To issue an HTTP PUT request:</span></span>

1. <span data-ttu-id="83cea-244">*Opcjonalnie*: Uruchom `get` polecenie, aby wyświetlić dane przed zmodyfikowaniem:</span><span class="sxs-lookup"><span data-stu-id="83cea-244">*Optional*: Run the `get` command to view the data before modifying it:</span></span>

    ```console
    https://localhost:5001/fruits~ get
    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8
    Date: Sat, 22 Jun 2019 00:07:32 GMT
    Server: Kestrel
    Transfer-Encoding: chunked

    [
      {
        "id": 1,
        "data": "Apple"
      },
      {
        "id": 2,
        "data": "Orange"
      },
      {
        "id": 3,
        "data": "Strawberry"
      }
    ]

1. Run the `put` command on an endpoint that supports it:

    ```console
    https://localhost:5001/fruits~ put 2 -h Content-Type=application/json
    ```

    <span data-ttu-id="83cea-245">W poprzednim poleceniu `Content-Type` nagłówek żądania HTTP jest ustawiona, aby wskazać typ nośnika treści żądania JSON.</span><span class="sxs-lookup"><span data-stu-id="83cea-245">In the preceding command, the `Content-Type` HTTP request header is set to indicate a request body media type of JSON.</span></span> <span data-ttu-id="83cea-246">Zostanie otwarty Edytor tekstu domyślne *.tmp* pliku przy użyciu szablonu JSON reprezentująca treść żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="83cea-246">The default text editor opens a *.tmp* file with a JSON template representing the HTTP request body.</span></span> <span data-ttu-id="83cea-247">Przykład:</span><span class="sxs-lookup"><span data-stu-id="83cea-247">For example:</span></span>

    ```json
    {
      "id": 0,
      "name": ""
    }
    ```

    > [!TIP]
    > <span data-ttu-id="83cea-248">Aby ustawić domyślny edytor tekstu, zobacz [ustawiony domyślny edytor tekstu](#set-the-default-text-editor) sekcji.</span><span class="sxs-lookup"><span data-stu-id="83cea-248">To set the default text editor, see the [Set the default text editor](#set-the-default-text-editor) section.</span></span>

1. <span data-ttu-id="83cea-249">Zmodyfikuj szablon JSON w celu spełnienia wymagań weryfikacji modelu:</span><span class="sxs-lookup"><span data-stu-id="83cea-249">Modify the JSON template to satisfy model validation requirements:</span></span>

    ```json
    {
      "id": 2,
      "name": "Cherry"
    }
    ```

1. <span data-ttu-id="83cea-250">Zapisz *.tmp* plik i zamknij Edytor tekstu.</span><span class="sxs-lookup"><span data-stu-id="83cea-250">Save the *.tmp* file, and close the text editor.</span></span> <span data-ttu-id="83cea-251">Następujące dane wyjściowe pojawia się w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="83cea-251">The following output appears in the command shell:</span></span>

    ```console
    [main 2019-06-28T17:27:01.805Z] update#setState idle
    HTTP/1.1 204 No Content
    Date: Fri, 28 Jun 2019 17:28:21 GMT
    Server: Kestrel
    ```

1. <span data-ttu-id="83cea-252">*Opcjonalnie*: Problem `get` polecenie, aby wyświetlić modyfikacje.</span><span class="sxs-lookup"><span data-stu-id="83cea-252">*Optional*: Issue a `get` command to see the modifications.</span></span> <span data-ttu-id="83cea-253">Na przykład, jeśli wpiszesz "Wykonywanie operacji Cherry" w edytorze tekstów `get` zwraca następujące:</span><span class="sxs-lookup"><span data-stu-id="83cea-253">For example, if you typed "Cherry" in the text editor, a `get` returns the following:</span></span>

    ```console
    https://localhost:5001/fruits~ get
    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8
    Date: Sat, 22 Jun 2019 00:08:20 GMT
    Server: Kestrel
    Transfer-Encoding: chunked

    [
      {
        "id": 1,
        "data": "Apple"
      },
      {
        "id": 2,
        "data": "Cherry"
      },
      {
        "id": 3,
        "data": "Strawberry"
      }
    ]


    https://localhost:5001/fruits~
    ```

## <a name="test-http-delete-requests"></a><span data-ttu-id="83cea-254">Testowanie żądania HTTP DELETE</span><span class="sxs-lookup"><span data-stu-id="83cea-254">Test HTTP DELETE requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="83cea-255">Streszczenie</span><span class="sxs-lookup"><span data-stu-id="83cea-255">Synopsis</span></span>

```console
delete <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="83cea-256">Argumenty</span><span class="sxs-lookup"><span data-stu-id="83cea-256">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="83cea-257">Parametr trasy, oczekiwany przez metodę akcji kontrolera skojarzone.</span><span class="sxs-lookup"><span data-stu-id="83cea-257">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="83cea-258">Opcje</span><span class="sxs-lookup"><span data-stu-id="83cea-258">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

### <a name="example"></a><span data-ttu-id="83cea-259">Przykład</span><span class="sxs-lookup"><span data-stu-id="83cea-259">Example</span></span>

<span data-ttu-id="83cea-260">Wysłanie żądania HTTP DELETE:</span><span class="sxs-lookup"><span data-stu-id="83cea-260">To issue an HTTP DELETE request:</span></span>

1. <span data-ttu-id="83cea-261">*Opcjonalnie*: Uruchom `get` polecenie, aby wyświetlić dane przed zmodyfikowaniem:</span><span class="sxs-lookup"><span data-stu-id="83cea-261">*Optional*: Run the `get` command to view the data before modifying it:</span></span>

    ```console
    https://localhost:5001/fruits~ get
    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8
    Date: Sat, 22 Jun 2019 00:07:32 GMT
    Server: Kestrel
    Transfer-Encoding: chunked

    [
      {
        "id": 1,
        "data": "Apple"
      },
      {
        "id": 2,
        "data": "Orange"
      },
      {
        "id": 3,
        "data": "Strawberry"
      }
    ]

1. Run the `delete` command on an endpoint that supports it:

    ```console
    https://localhost:5001/fruits~ delete 2
    ```

    <span data-ttu-id="83cea-262">Poprzednie polecenie wyświetli następujący format danych wyjściowych:</span><span class="sxs-lookup"><span data-stu-id="83cea-262">The preceding command displays the following output format:</span></span>

    ```console
    HTTP/1.1 204 No Content
    Date: Fri, 28 Jun 2019 17:36:42 GMT
    Server: Kestrel
    ```

1. <span data-ttu-id="83cea-263">*Opcjonalnie*: Problem `get` polecenie, aby wyświetlić modyfikacje.</span><span class="sxs-lookup"><span data-stu-id="83cea-263">*Optional*: Issue a `get` command to see the modifications.</span></span> <span data-ttu-id="83cea-264">W tym przykładzie `get` zwraca następujące:</span><span class="sxs-lookup"><span data-stu-id="83cea-264">In this example, a `get` returns the following:</span></span>

    ```console
    https://localhost:5001/fruits~ get
    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8
    Date: Sat, 22 Jun 2019 00:16:30 GMT
    Server: Kestrel
    Transfer-Encoding: chunked

    [
      {
        "id": 1,
        "data": "Apple"
      },
      {
        "id": 3,
        "data": "Strawberry"
      }
    ]


    https://localhost:5001/fruits~
    ```

## <a name="test-http-patch-requests"></a><span data-ttu-id="83cea-265">Testowanie żądania HTTP PATCH</span><span class="sxs-lookup"><span data-stu-id="83cea-265">Test HTTP PATCH requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="83cea-266">Streszczenie</span><span class="sxs-lookup"><span data-stu-id="83cea-266">Synopsis</span></span>

```console
patch <PARAMETER> [-c|--content] [-f|--file] [-h|--header] [--no-body] [-F|--no-formatting] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="83cea-267">Argumenty</span><span class="sxs-lookup"><span data-stu-id="83cea-267">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="83cea-268">Parametr trasy, oczekiwany przez metodę akcji kontrolera skojarzone.</span><span class="sxs-lookup"><span data-stu-id="83cea-268">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="83cea-269">Opcje</span><span class="sxs-lookup"><span data-stu-id="83cea-269">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

[!INCLUDE [HTTP request body CLI options](~/includes/http-repl/requires-body-options.md)]

## <a name="test-http-head-requests"></a><span data-ttu-id="83cea-270">Testowanie żądania HTTP HEAD</span><span class="sxs-lookup"><span data-stu-id="83cea-270">Test HTTP HEAD requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="83cea-271">Streszczenie</span><span class="sxs-lookup"><span data-stu-id="83cea-271">Synopsis</span></span>

```console
head <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="83cea-272">Argumenty</span><span class="sxs-lookup"><span data-stu-id="83cea-272">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="83cea-273">Parametr trasy, oczekiwany przez metodę akcji kontrolera skojarzone.</span><span class="sxs-lookup"><span data-stu-id="83cea-273">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="83cea-274">Opcje</span><span class="sxs-lookup"><span data-stu-id="83cea-274">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

## <a name="test-http-options-requests"></a><span data-ttu-id="83cea-275">Testowanie żądania OPTIONS protokołu HTTP</span><span class="sxs-lookup"><span data-stu-id="83cea-275">Test HTTP OPTIONS requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="83cea-276">Streszczenie</span><span class="sxs-lookup"><span data-stu-id="83cea-276">Synopsis</span></span>

```console
options <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="83cea-277">Argumenty</span><span class="sxs-lookup"><span data-stu-id="83cea-277">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="83cea-278">Parametr trasy, oczekiwany przez metodę akcji kontrolera skojarzone.</span><span class="sxs-lookup"><span data-stu-id="83cea-278">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="83cea-279">Opcje</span><span class="sxs-lookup"><span data-stu-id="83cea-279">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

## <a name="set-http-request-headers"></a><span data-ttu-id="83cea-280">Ustawia nagłówki żądania HTTP</span><span class="sxs-lookup"><span data-stu-id="83cea-280">Set HTTP request headers</span></span>

<span data-ttu-id="83cea-281">Aby ustawić nagłówek żądania HTTP, użyj jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="83cea-281">To set an HTTP request header, use one of the following approaches:</span></span>

1. <span data-ttu-id="83cea-282">Wbudowany zestaw z żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="83cea-282">Set inline with the HTTP request.</span></span> <span data-ttu-id="83cea-283">Przykład:</span><span class="sxs-lookup"><span data-stu-id="83cea-283">For example:</span></span>

  ```console
  https://localhost:5001/people~ post -h Content-Type=application/json
  ```

  <span data-ttu-id="83cea-284">Z poprzednim przypadku każdego distinct nagłówek żądania HTTP wymaga własnej `-h` opcji.</span><span class="sxs-lookup"><span data-stu-id="83cea-284">With the preceding approach, each distinct HTTP request header requires its own `-h` option.</span></span>

1. <span data-ttu-id="83cea-285">Ustaw przed wysłaniem żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="83cea-285">Set before sending the HTTP request.</span></span> <span data-ttu-id="83cea-286">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="83cea-286">For example:</span></span>

  ```console
  https://localhost:5001/people~ set header Content-Type application/json
  ```

  <span data-ttu-id="83cea-287">Podczas ustawiania nagłówka przed wysłaniem żądania, nagłówek pozostanie ustawiony na czas trwania sesji powłoki poleceń.</span><span class="sxs-lookup"><span data-stu-id="83cea-287">When setting the header before sending a request, the header remains set for the duration of the command shell session.</span></span> <span data-ttu-id="83cea-288">Aby usunąć nagłówek, podaj wartość pustą.</span><span class="sxs-lookup"><span data-stu-id="83cea-288">To clear the header, provide an empty value.</span></span> <span data-ttu-id="83cea-289">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="83cea-289">For example:</span></span>

  ```console
  https://localhost:5001/people~ set header Content-Type
  ```

## <a name="toggle-http-request-display"></a><span data-ttu-id="83cea-290">Przełącz wyświetlanie żądania HTTP</span><span class="sxs-lookup"><span data-stu-id="83cea-290">Toggle HTTP request display</span></span>

<span data-ttu-id="83cea-291">Domyślnie pomijane jest wyświetlana wysłanie żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="83cea-291">By default, display of the HTTP request being sent is suppressed.</span></span> <span data-ttu-id="83cea-292">Istnieje możliwość Zmień odpowiednie ustawienia na czas trwania sesji powłoki poleceń.</span><span class="sxs-lookup"><span data-stu-id="83cea-292">It's possible to change the corresponding setting for the duration of the command shell session.</span></span>

### <a name="enable-request-display"></a><span data-ttu-id="83cea-293">Włącz wyświetlanie żądania</span><span class="sxs-lookup"><span data-stu-id="83cea-293">Enable request display</span></span>

<span data-ttu-id="83cea-294">Wyświetl żądania HTTP są wysyłane przez uruchomienie `echo on` polecenia.</span><span class="sxs-lookup"><span data-stu-id="83cea-294">View the HTTP request being sent by running the `echo on` command.</span></span> <span data-ttu-id="83cea-295">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="83cea-295">For example:</span></span>

```console
https://localhost:5001/people~ echo on
Request echoing is on
```

<span data-ttu-id="83cea-296">Kolejne żądania HTTP w bieżącej sesji, wyświetlanie nagłówków żądania.</span><span class="sxs-lookup"><span data-stu-id="83cea-296">Subsequent HTTP requests in the current session display the request headers.</span></span> <span data-ttu-id="83cea-297">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="83cea-297">For example:</span></span>

```console
https://localhost:5001/people~ post

[main 2019-06-28T18:50:11.930Z] update#setState idle
Request to https://localhost:5001...

POST /people HTTP/1.1
Content-Length: 41
Content-Type: application/json
User-Agent: HTTP-REPL

{
  "id": 0,
  "name": "Scott Addie"
}

Response from https://localhost:5001...

HTTP/1.1 201 Created
Content-Type: application/json; charset=utf-8
Date: Fri, 28 Jun 2019 18:50:21 GMT
Location: https://localhost:5001/people/4
Server: Kestrel
Transfer-Encoding: chunked

{
  "id": 4,
  "name": "Scott Addie"
}


https://localhost:5001/people~
```

### <a name="disable-request-display"></a><span data-ttu-id="83cea-298">Wyłącz wyświetlanie żądania</span><span class="sxs-lookup"><span data-stu-id="83cea-298">Disable request display</span></span>

<span data-ttu-id="83cea-299">Pomija wyświetlanie żądania HTTP są wysyłane przez uruchomienie `echo off` polecenia.</span><span class="sxs-lookup"><span data-stu-id="83cea-299">Suppress display of the HTTP request being sent by running the `echo off` command.</span></span> <span data-ttu-id="83cea-300">Przykład:</span><span class="sxs-lookup"><span data-stu-id="83cea-300">For example:</span></span>

```console
https://localhost:5001/people~ echo off
Request echoing is off
```

## <a name="run-a-script"></a><span data-ttu-id="83cea-301">Uruchamianie skryptu</span><span class="sxs-lookup"><span data-stu-id="83cea-301">Run a script</span></span>

<span data-ttu-id="83cea-302">Jeśli ten sam zestaw poleceń HTTP REPL jest często wykonywane, należy wziąć pod uwagę przechowywania ich w pliku tekstowym.</span><span class="sxs-lookup"><span data-stu-id="83cea-302">If you frequently execute the same set of HTTP REPL commands, consider storing them in a text file.</span></span> <span data-ttu-id="83cea-303">Polecenia w pliku formę ten sam jak wykonywane ręcznie, w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="83cea-303">Commands in the file take the same form as those executed manually on the command line.</span></span> <span data-ttu-id="83cea-304">Polecenia mogą być wykonywane w sposób wsadowej za pomocą `run` polecenia.</span><span class="sxs-lookup"><span data-stu-id="83cea-304">The commands can be executed in a batched fashion using the `run` command.</span></span> <span data-ttu-id="83cea-305">Przykład:</span><span class="sxs-lookup"><span data-stu-id="83cea-305">For example:</span></span>

1. <span data-ttu-id="83cea-306">Utwórz plik tekstowy zawierający zestaw poleceń rozdzielonych znakami nowego wiersza.</span><span class="sxs-lookup"><span data-stu-id="83cea-306">Create a text file containing a set of newline-delimited commands.</span></span> <span data-ttu-id="83cea-307">Aby zilustrować, należy wziąć pod uwagę *script.txt osób* plik zawierający następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="83cea-307">To illustrate, consider a *people-script.txt* file containing the following commands:</span></span>

    ```text
    set base https://localhost:5001
    ls
    cd People
    ls
    get 1
    ```

1. <span data-ttu-id="83cea-308">Wykonaj `run` polecenia, przekazując ścieżkę do pliku tekstowego.</span><span class="sxs-lookup"><span data-stu-id="83cea-308">Execute the `run` command, passing in the text file's path.</span></span> <span data-ttu-id="83cea-309">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="83cea-309">For example:</span></span>

    ```console
    https://localhost:5001/~ run C:\http-repl-scripts\people-script.txt
    ```

    <span data-ttu-id="83cea-310">Zostanie wyświetlone następujące dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="83cea-310">The following output appears:</span></span>

    ```console
    https://localhost:5001/~ set base https://localhost:5001
    Using swagger metadata from https://localhost:5001/swagger/v1/swagger.json
    
    https://localhost:5001/~ ls
    .        []
    Fruits   [get|post]
    People   [get|post]
    
    https://localhost:5001/~ cd People
    /People    [get|post]
    
    https://localhost:5001/People~ ls
    .      [get|post]
    ..     []
    {id}   [get|put|delete]
    
    https://localhost:5001/People~ get 1
    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8
    Date: Fri, 12 Jul 2019 19:20:10 GMT
    Server: Kestrel
    Transfer-Encoding: chunked
    
    {
      "id": 1,
      "name": "Scott Hunter"
    }
    
    
    https://localhost:5001/People~
    ```

## <a name="clear-the-output"></a><span data-ttu-id="83cea-311">Wyczyść dane wyjściowe</span><span class="sxs-lookup"><span data-stu-id="83cea-311">Clear the output</span></span>

<span data-ttu-id="83cea-312">Aby usunąć wszystkich danych wyjściowych zapisywanych powłoki poleceń narzędzia HTTP REPL, uruchom `clear` lub `cls` polecenia.</span><span class="sxs-lookup"><span data-stu-id="83cea-312">To remove all output written to the command shell by the HTTP REPL tool, run the `clear` or `cls` command.</span></span> <span data-ttu-id="83cea-313">Aby zilustrować, załóżmy, że powłoka poleceń zawiera następujące dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="83cea-313">To illustrate, imagine the command shell contains the following output:</span></span>

```console
dotnet httprepl https://localhost:5001
(Disconnected)~ set base "https://localhost:5001"
Using swagger metadata from https://localhost:5001/swagger/v1/swagger.json

https://localhost:5001/~ ls
.        []
Fruits   [get|post]
People   [get|post]

https://localhost:5001/~
```

<span data-ttu-id="83cea-314">Uruchom następujące polecenie, aby wyczyścić dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="83cea-314">Run the following command to clear the output:</span></span>

```console
https://localhost:5001/~ clear
```

<span data-ttu-id="83cea-315">Po uruchomieniu poprzedniego polecenia, powłokę poleceń zawiera tylko następujące dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="83cea-315">After running the preceding command, the command shell contains only the following output:</span></span>

```console
https://localhost:5001/~
```

## <a name="additional-resources"></a><span data-ttu-id="83cea-316">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="83cea-316">Additional resources</span></span>

* [<span data-ttu-id="83cea-317">Żądań interfejsu API REST</span><span class="sxs-lookup"><span data-stu-id="83cea-317">REST API requests</span></span>](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#74-supported-methods)
* [<span data-ttu-id="83cea-318">Repozytorium HTTP REPL GitHub</span><span class="sxs-lookup"><span data-stu-id="83cea-318">HTTP REPL GitHub repository</span></span>](https://github.com/aspnet/HttpRepl)
