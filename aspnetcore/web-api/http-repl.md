---
title: Testowanie interfejsów API sieci Web przy użyciu protokołu HTTP REPL
author: scottaddie
description: Dowiedz się, jak przeglądać i testować ASP.NET Core internetowy interfejs API przy użyciu globalnego narzędzia REPL .NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 08/29/2019
uid: web-api/http-repl
ms.openlocfilehash: b93641c96ad093b40aaa887f19409d4c2e1490df
ms.sourcegitcommit: efb9c16321b34f8ede802822b26eb983de16479b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/09/2019
ms.locfileid: "70842267"
---
# <a name="test-web-apis-with-the-http-repl"></a><span data-ttu-id="fb2c4-103">Testowanie interfejsów API sieci Web przy użyciu protokołu HTTP REPL</span><span class="sxs-lookup"><span data-stu-id="fb2c4-103">Test web APIs with the HTTP REPL</span></span>

<span data-ttu-id="fb2c4-104">Przez [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="fb2c4-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="fb2c4-105">Pętla HTTP Read-eval-Print (REPL) to:</span><span class="sxs-lookup"><span data-stu-id="fb2c4-105">The HTTP Read-Eval-Print Loop (REPL) is:</span></span>

* <span data-ttu-id="fb2c4-106">Lekkie, międzyplatformowe narzędzie wiersza polecenia, które jest obsługiwane wszędzie tam, gdzie jest obsługiwane środowisko .NET Core.</span><span class="sxs-lookup"><span data-stu-id="fb2c4-106">A lightweight, cross-platform command-line tool that's supported everywhere .NET Core is supported.</span></span>
* <span data-ttu-id="fb2c4-107">Służy do wykonywania żądań HTTP do testowania ASP.NET Core interfejsów API sieci Web (oraz non-ASP.NET podstawowych interfejsów API sieci Web) i wyświetlania ich wyników.</span><span class="sxs-lookup"><span data-stu-id="fb2c4-107">Used for making HTTP requests to test ASP.NET Core web APIs (and non-ASP.NET Core web APIs) and view their results.</span></span>
* <span data-ttu-id="fb2c4-108">Możliwość testowania interfejsów API sieci Web hostowanych w dowolnym środowisku, w tym hosta lokalnego i Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="fb2c4-108">Capable of testing web APIs hosted in any environment, including localhost and Azure App Service.</span></span>

<span data-ttu-id="fb2c4-109">Obsługiwane są następujące [czasowniki http](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#74-supported-methods) :</span><span class="sxs-lookup"><span data-stu-id="fb2c4-109">The following [HTTP verbs](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#74-supported-methods) are supported:</span></span>

* [<span data-ttu-id="fb2c4-110">USUNIĘTY</span><span class="sxs-lookup"><span data-stu-id="fb2c4-110">DELETE</span></span>](#test-http-delete-requests)
* [<span data-ttu-id="fb2c4-111">GET</span><span class="sxs-lookup"><span data-stu-id="fb2c4-111">GET</span></span>](#test-http-get-requests)
* [<span data-ttu-id="fb2c4-112">MTP</span><span class="sxs-lookup"><span data-stu-id="fb2c4-112">HEAD</span></span>](#test-http-head-requests)
* [<span data-ttu-id="fb2c4-113">OPCJE</span><span class="sxs-lookup"><span data-stu-id="fb2c4-113">OPTIONS</span></span>](#test-http-options-requests)
* [<span data-ttu-id="fb2c4-114">WYSŁANA</span><span class="sxs-lookup"><span data-stu-id="fb2c4-114">PATCH</span></span>](#test-http-patch-requests)
* [<span data-ttu-id="fb2c4-115">POST</span><span class="sxs-lookup"><span data-stu-id="fb2c4-115">POST</span></span>](#test-http-post-requests)
* [<span data-ttu-id="fb2c4-116">UBRANI</span><span class="sxs-lookup"><span data-stu-id="fb2c4-116">PUT</span></span>](#test-http-put-requests)

<span data-ttu-id="fb2c4-117">Aby wykonać te czynności, [Wyświetl lub Pobierz przykładowy ASP.NET Core internetowy interfejs API](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/http-repl/samples) ([jak pobrać](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="fb2c4-117">To follow along, [view or download the sample ASP.NET Core web API](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/http-repl/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fb2c4-118">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="fb2c4-118">Prerequisites</span></span>

* [!INCLUDE [2.1-SDK](~/includes/2.1-SDK.md)]

## <a name="installation"></a><span data-ttu-id="fb2c4-119">Instalacja</span><span class="sxs-lookup"><span data-stu-id="fb2c4-119">Installation</span></span>

<span data-ttu-id="fb2c4-120">Aby zainstalować REPL HTTP, uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="fb2c4-120">To install the HTTP REPL, run the following command:</span></span>

```console
dotnet tool install -g Microsoft.dotnet-httprepl --version "3.0.0-*"
```

<span data-ttu-id="fb2c4-121">[Narzędzie globalne platformy .NET Core](/dotnet/core/tools/global-tools#install-a-global-tool) jest instalowane z pakietu NuGet [Microsoft. dotnet-httprepl](https://www.nuget.org/packages/Microsoft.dotnet-httprepl) .</span><span class="sxs-lookup"><span data-stu-id="fb2c4-121">A [.NET Core Global Tool](/dotnet/core/tools/global-tools#install-a-global-tool) is installed from the [Microsoft.dotnet-httprepl](https://www.nuget.org/packages/Microsoft.dotnet-httprepl) NuGet package.</span></span>

## <a name="usage"></a><span data-ttu-id="fb2c4-122">Użycie</span><span class="sxs-lookup"><span data-stu-id="fb2c4-122">Usage</span></span>

<span data-ttu-id="fb2c4-123">Po pomyślnej instalacji narzędzia Uruchom następujące polecenie, aby uruchomić REPL HTTP:</span><span class="sxs-lookup"><span data-stu-id="fb2c4-123">After successful installation of the tool, run the following command to start the HTTP REPL:</span></span>

```console
httprepl
```

<span data-ttu-id="fb2c4-124">Aby wyświetlić dostępne polecenia HTTP REPL, Uruchom jedno z następujących poleceń:</span><span class="sxs-lookup"><span data-stu-id="fb2c4-124">To view the available HTTP REPL commands, run one of the following commands:</span></span>

```console
httprepl -h
```

```console
httprepl --help
```

<span data-ttu-id="fb2c4-125">Wyświetlane są następujące dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="fb2c4-125">The following output is displayed:</span></span>

```console
Usage:
  httprepl [<BASE_ADDRESS>] [options]

Arguments:
  <BASE_ADDRESS> - The initial base address for the REPL.

Options:
  -h|--help - Show help information.

Once the REPL starts, these commands are valid:

Setup Commands:
Use these commands to configure the tool for your API server

connect        Configures the directory structure and base address of the api server
set header     Sets or clears a header for all requests. e.g. `set header content-type application/json`

HTTP Commands:
Use these commands to execute requests against your application.

GET            get - Issues a GET request
POST           post - Issues a POST request
PUT            put - Issues a PUT request
DELETE         delete - Issues a DELETE request
PATCH          patch - Issues a PATCH request
HEAD           head - Issues a HEAD request
OPTIONS        options - Issues a OPTIONS request

Navigation Commands:
The REPL allows you to navigate your URL space and focus on specific APIs that you are working on.

set base       Set the base URI. e.g. `set base http://locahost:5000`
ls             Show all endpoints for the current path
cd             Append the given directory to the currently selected path, or move up a path when using `cd ..`

Shell Commands:
Use these commands to interact with the REPL shell.

clear          Removes all text from the shell
echo [on/off]  Turns request echoing on or off, show the request that was made when using request commands
exit           Exit the shell

REPL Customization Commands:
Use these commands to customize the REPL behavior.

pref [get/set] Allows viewing or changing preferences, e.g. 'pref set editor.command.default 'C:\\Program Files\\Microsoft VS Code\\Code.exe'`
run            Runs the script at the given path. A script is a set of commands that can be typed with one command per line
ui             Displays the Swagger UI page, if available, in the default browser

Use `help <COMMAND>` for more detail on an individual command. e.g. `help get`.
For detailed tool info, see https://aka.ms/http-repl-doc.
```

<span data-ttu-id="fb2c4-126">REPL HTTP oferuje polecenie uzupełniania.</span><span class="sxs-lookup"><span data-stu-id="fb2c4-126">The HTTP REPL offers command completion.</span></span> <span data-ttu-id="fb2c4-127">Naciśnięcie klawisza <kbd>Tab</kbd> wykonuje iterację na liście poleceń, które uzupełniają wpisane znaki lub punkt końcowy interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="fb2c4-127">Pressing the <kbd>Tab</kbd> key iterates through the list of commands that complete the characters or API endpoint that you typed.</span></span> <span data-ttu-id="fb2c4-128">W poniższych sekcjach znajduje się opis dostępnych poleceń interfejsu wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="fb2c4-128">The following sections outline the available CLI commands.</span></span>

## <a name="connect-to-the-web-api"></a><span data-ttu-id="fb2c4-129">Nawiązywanie połączenia z interfejsem API sieci Web</span><span class="sxs-lookup"><span data-stu-id="fb2c4-129">Connect to the web API</span></span>

<span data-ttu-id="fb2c4-130">Połącz się z interfejsem API sieci Web, uruchamiając następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="fb2c4-130">Connect to a web API by running the following command:</span></span>

```console
httprepl <ROOT URI>
```

<span data-ttu-id="fb2c4-131">`<ROOT URI>`jest podstawowym identyfikatorem URI dla internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="fb2c4-131">`<ROOT URI>` is the base URI for the web API.</span></span> <span data-ttu-id="fb2c4-132">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="fb2c4-132">For example:</span></span>

```console
httprepl https://localhost:5001
```

<span data-ttu-id="fb2c4-133">Alternatywnie Uruchom następujące polecenie w dowolnym momencie podczas działania REPL HTTP:</span><span class="sxs-lookup"><span data-stu-id="fb2c4-133">Alternatively, run the following command at any time while the HTTP REPL is running:</span></span>

```console
connect <ROOT URI>
```

<span data-ttu-id="fb2c4-134">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="fb2c4-134">For example:</span></span>

```console
(Disconnected)~ connect https://localhost:5001
```

## <a name="manually-point-to-the-swagger-document-for-the-web-api"></a><span data-ttu-id="fb2c4-135">Ręcznie wskaż dokument struktury Swagger dla internetowego interfejsu API</span><span class="sxs-lookup"><span data-stu-id="fb2c4-135">Manually point to the Swagger document for the web API</span></span>

<span data-ttu-id="fb2c4-136">Powyższe polecenie Connect podejmie próbę automatycznego znalezienia dokumentu struktury Swagger.</span><span class="sxs-lookup"><span data-stu-id="fb2c4-136">The connect command above will attempt to find the Swagger document automatically.</span></span> <span data-ttu-id="fb2c4-137">Jeśli z jakiegoś powodu nie można tego zrobić, możesz określić identyfikator URI dokumentu struktury Swagger dla internetowego interfejsu API przy użyciu `--swagger` opcji:</span><span class="sxs-lookup"><span data-stu-id="fb2c4-137">If for some reason it is unable to do so, you can specify the URI of the Swagger document for the web API by using the `--swagger` option:</span></span>

```console
connect <ROOT URI> --swagger <SWAGGER URI>
```

<span data-ttu-id="fb2c4-138">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="fb2c4-138">For example:</span></span>

```console
(Disconnected)~ connect https://localhost:5001 --swagger /swagger/v1/swagger.json
```

## <a name="navigate-the-web-api"></a><span data-ttu-id="fb2c4-139">Nawigowanie po interfejsie API sieci Web</span><span class="sxs-lookup"><span data-stu-id="fb2c4-139">Navigate the web API</span></span>

### <a name="view-available-endpoints"></a><span data-ttu-id="fb2c4-140">Wyświetl dostępne punkty końcowe</span><span class="sxs-lookup"><span data-stu-id="fb2c4-140">View available endpoints</span></span>

<span data-ttu-id="fb2c4-141">Aby wyświetlić listę różnych punktów końcowych (kontrolerów) znajdujących się w bieżącej ścieżce adresu internetowego interfejsu API `ls` , `dir` Uruchom polecenie lub:</span><span class="sxs-lookup"><span data-stu-id="fb2c4-141">To list the different endpoints (controllers) at the current path of the web API address, run the `ls` or `dir` command:</span></span>

```console
https://localhot:5001/~ ls
```

<span data-ttu-id="fb2c4-142">Wyświetlany jest następujący format danych wyjściowych:</span><span class="sxs-lookup"><span data-stu-id="fb2c4-142">The following output format is displayed:</span></span>

```console
.        []
Fruits   [get|post]
People   [get|post]

https://localhost:5001/~
```

<span data-ttu-id="fb2c4-143">Powyższe dane wyjściowe wskazują, że dostępne są dwa kontrolery: `Fruits` i `People`.</span><span class="sxs-lookup"><span data-stu-id="fb2c4-143">The preceding output indicates that there are two controllers available: `Fruits` and `People`.</span></span> <span data-ttu-id="fb2c4-144">Oba kontrolery obsługują bez parametrów operacje GET i POST HTTP.</span><span class="sxs-lookup"><span data-stu-id="fb2c4-144">Both controllers support parameterless HTTP GET and POST operations.</span></span>

<span data-ttu-id="fb2c4-145">Przechodzenie do określonego kontrolera ujawnia więcej szczegółów.</span><span class="sxs-lookup"><span data-stu-id="fb2c4-145">Navigating into a specific controller reveals more detail.</span></span> <span data-ttu-id="fb2c4-146">Na przykład następujące dane wyjściowe polecenia pokazują `Fruits` kontroler obsługuje również operacje Get, PUT i DELETE protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="fb2c4-146">For example, the following command's output shows the `Fruits` controller also supports HTTP GET, PUT, and DELETE operations.</span></span> <span data-ttu-id="fb2c4-147">Każda z tych operacji oczekuje `id` parametru w marszrucie:</span><span class="sxs-lookup"><span data-stu-id="fb2c4-147">Each of these operations expects an `id` parameter in the route:</span></span>

```console
https://localhost:5001/fruits~ ls
.      [get|post]
..     []
{id}   [get|put|delete]

https://localhost:5001/fruits~
```

<span data-ttu-id="fb2c4-148">Alternatywnie można uruchomić `ui` polecenie, aby otworzyć stronę interfejsu użytkownika programu Swagger interfejsu API sieci Web w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="fb2c4-148">Alternatively, run the `ui` command to open the web API's Swagger UI page in a browser.</span></span> <span data-ttu-id="fb2c4-149">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="fb2c4-149">For example:</span></span>

```console
https://localhost:5001/~ ui
```

### <a name="navigate-to-an-endpoint"></a><span data-ttu-id="fb2c4-150">Przejdź do punktu końcowego</span><span class="sxs-lookup"><span data-stu-id="fb2c4-150">Navigate to an endpoint</span></span>

<span data-ttu-id="fb2c4-151">Aby przejść do innego punktu końcowego w internetowym interfejsie API, `cd` Uruchom polecenie:</span><span class="sxs-lookup"><span data-stu-id="fb2c4-151">To navigate to a different endpoint on the web API, run the `cd` command:</span></span>

```console
https://localhost:5001/~ cd people
```

<span data-ttu-id="fb2c4-152">W ścieżce następującego `cd` polecenia jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="fb2c4-152">The path following the `cd` command is case insensitive.</span></span> <span data-ttu-id="fb2c4-153">Wyświetlany jest następujący format danych wyjściowych:</span><span class="sxs-lookup"><span data-stu-id="fb2c4-153">The following output format is displayed:</span></span>

```console
/people    [get|post]

https://localhost:5001/people~
```

## <a name="customize-the-http-repl"></a><span data-ttu-id="fb2c4-154">Dostosowywanie REPL HTTP</span><span class="sxs-lookup"><span data-stu-id="fb2c4-154">Customize the HTTP REPL</span></span>

<span data-ttu-id="fb2c4-155">Domyślne [kolory](#set-color-preferences) REPL http można dostosować.</span><span class="sxs-lookup"><span data-stu-id="fb2c4-155">The HTTP REPL's default [colors](#set-color-preferences) can be customized.</span></span> <span data-ttu-id="fb2c4-156">Ponadto można zdefiniować [domyślny edytor tekstu](#set-the-default-text-editor) .</span><span class="sxs-lookup"><span data-stu-id="fb2c4-156">Additionally, a [default text editor](#set-the-default-text-editor) can be defined.</span></span> <span data-ttu-id="fb2c4-157">Preferencje HTTP REPL są utrwalane w bieżącej sesji i są honorowane w przyszłych sesjach.</span><span class="sxs-lookup"><span data-stu-id="fb2c4-157">The HTTP REPL preferences are persisted across the current session and are honored in future sessions.</span></span> <span data-ttu-id="fb2c4-158">Po zmodyfikowaniu preferencje są przechowywane w następującym pliku:</span><span class="sxs-lookup"><span data-stu-id="fb2c4-158">Once modified, the preferences are stored in the following file:</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="fb2c4-159">Linux</span><span class="sxs-lookup"><span data-stu-id="fb2c4-159">Linux</span></span>](#tab/linux)

<span data-ttu-id="fb2c4-160">*% HOME%/.httpreplprefs*</span><span class="sxs-lookup"><span data-stu-id="fb2c4-160">*%HOME%/.httpreplprefs*</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="fb2c4-161">macOS</span><span class="sxs-lookup"><span data-stu-id="fb2c4-161">macOS</span></span>](#tab/macos)

<span data-ttu-id="fb2c4-162">*% HOME%/.httpreplprefs*</span><span class="sxs-lookup"><span data-stu-id="fb2c4-162">*%HOME%/.httpreplprefs*</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="fb2c4-163">Windows</span><span class="sxs-lookup"><span data-stu-id="fb2c4-163">Windows</span></span>](#tab/windows)

<span data-ttu-id="fb2c4-164">*% USERPROFILE%\\. httpreplprefs*</span><span class="sxs-lookup"><span data-stu-id="fb2c4-164">*%USERPROFILE%\\.httpreplprefs*</span></span>

---

<span data-ttu-id="fb2c4-165">Plik *. httpreplprefs* jest ładowany podczas uruchamiania i nie jest monitorowany pod kątem zmian w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="fb2c4-165">The *.httpreplprefs* file is loaded on startup and not monitored for changes at runtime.</span></span> <span data-ttu-id="fb2c4-166">Ręczne modyfikacje pliku zaczną obowiązywać dopiero po ponownym uruchomieniu narzędzia.</span><span class="sxs-lookup"><span data-stu-id="fb2c4-166">Manual modifications to the file take effect only after restarting the tool.</span></span>

### <a name="view-the-settings"></a><span data-ttu-id="fb2c4-167">Wyświetlanie ustawień</span><span class="sxs-lookup"><span data-stu-id="fb2c4-167">View the settings</span></span>

<span data-ttu-id="fb2c4-168">Aby wyświetlić dostępne ustawienia, uruchom `pref get` polecenie.</span><span class="sxs-lookup"><span data-stu-id="fb2c4-168">To view the available settings, run the `pref get` command.</span></span> <span data-ttu-id="fb2c4-169">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="fb2c4-169">For example:</span></span>

```console
https://localhost:5001/~ pref get
```

<span data-ttu-id="fb2c4-170">Poprzednie polecenie wyświetla dostępne pary klucz-wartość:</span><span class="sxs-lookup"><span data-stu-id="fb2c4-170">The preceding command displays the available key-value pairs:</span></span>

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

### <a name="set-color-preferences"></a><span data-ttu-id="fb2c4-171">Ustawianie preferencji koloru</span><span class="sxs-lookup"><span data-stu-id="fb2c4-171">Set color preferences</span></span>

<span data-ttu-id="fb2c4-172">Kolorowanie odpowiedzi jest obecnie obsługiwane tylko w przypadku formatu JSON.</span><span class="sxs-lookup"><span data-stu-id="fb2c4-172">Response colorization is currently supported for JSON only.</span></span> <span data-ttu-id="fb2c4-173">Aby dostosować domyślne kolorowanie narzędzi HTTP REPL, Znajdź klucz odpowiadający kolorowi do zmiany.</span><span class="sxs-lookup"><span data-stu-id="fb2c4-173">To customize the default HTTP REPL tool coloring, locate the key corresponding to the color to be changed.</span></span> <span data-ttu-id="fb2c4-174">Aby uzyskać instrukcje dotyczące znajdowania kluczy, zobacz sekcję [Wyświetlanie ustawień](#view-the-settings) .</span><span class="sxs-lookup"><span data-stu-id="fb2c4-174">For instructions on how to find the keys, see the [View the settings](#view-the-settings) section.</span></span> <span data-ttu-id="fb2c4-175">Na przykład zmień `colors.json` wartość klucza z `Green` na `White` w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="fb2c4-175">For example, change the `colors.json` key value from `Green` to `White` as follows:</span></span>

```console
https://localhost:5001/people~ pref set colors.json White
```

<span data-ttu-id="fb2c4-176">Można używać tylko [dozwolonych kolorów](https://github.com/aspnet/HttpRepl/blob/01d5c3c3373e98fe566ff5ef8a17c571de880293/src/Microsoft.Repl/ConsoleHandling/AllowedColors.cs) .</span><span class="sxs-lookup"><span data-stu-id="fb2c4-176">Only the [allowed colors](https://github.com/aspnet/HttpRepl/blob/01d5c3c3373e98fe566ff5ef8a17c571de880293/src/Microsoft.Repl/ConsoleHandling/AllowedColors.cs) may be used.</span></span> <span data-ttu-id="fb2c4-177">Kolejne żądania HTTP wyświetlają dane wyjściowe z nowym kolorem.</span><span class="sxs-lookup"><span data-stu-id="fb2c4-177">Subsequent HTTP requests display output with the new coloring.</span></span>

<span data-ttu-id="fb2c4-178">Jeśli określone klucze kolorów nie są ustawione, brane są więcej kluczy ogólnych.</span><span class="sxs-lookup"><span data-stu-id="fb2c4-178">When specific color keys aren't set, more generic keys are considered.</span></span> <span data-ttu-id="fb2c4-179">Aby zademonstrować to zachowanie rezerwowe, należy wziąć pod uwagę następujący przykład:</span><span class="sxs-lookup"><span data-stu-id="fb2c4-179">To demonstrate this fallback behavior, consider the following example:</span></span>

* <span data-ttu-id="fb2c4-180">Jeśli `colors.json.name` nie ma wartości, `colors.json.string` jest używana.</span><span class="sxs-lookup"><span data-stu-id="fb2c4-180">If `colors.json.name` doesn't have a value, `colors.json.string` is used.</span></span>
* <span data-ttu-id="fb2c4-181">Jeśli `colors.json.string` nie ma wartości, `colors.json.literal` jest używana.</span><span class="sxs-lookup"><span data-stu-id="fb2c4-181">If `colors.json.string` doesn't have a value, `colors.json.literal` is used.</span></span>
* <span data-ttu-id="fb2c4-182">Jeśli `colors.json.literal` nie ma wartości, `colors.json` jest używana.</span><span class="sxs-lookup"><span data-stu-id="fb2c4-182">If `colors.json.literal` doesn't have a value, `colors.json` is used.</span></span> 
* <span data-ttu-id="fb2c4-183">Jeśli `colors.json` nie ma wartości, używany jest domyślny kolor tekstu powłoki poleceń (`AllowedColors.None`).</span><span class="sxs-lookup"><span data-stu-id="fb2c4-183">If `colors.json` doesn't have a value, the command shell's default text color (`AllowedColors.None`) is used.</span></span>

### <a name="set-indentation-size"></a><span data-ttu-id="fb2c4-184">Ustaw rozmiar wcięcia</span><span class="sxs-lookup"><span data-stu-id="fb2c4-184">Set indentation size</span></span>

<span data-ttu-id="fb2c4-185">Dostosowanie rozmiaru wcięcia odpowiedzi jest obecnie obsługiwane tylko w przypadku formatu JSON.</span><span class="sxs-lookup"><span data-stu-id="fb2c4-185">Response indentation size customization is currently supported for JSON only.</span></span> <span data-ttu-id="fb2c4-186">Domyślny rozmiar to dwie spacje.</span><span class="sxs-lookup"><span data-stu-id="fb2c4-186">The default size is two spaces.</span></span> <span data-ttu-id="fb2c4-187">Przykład:</span><span class="sxs-lookup"><span data-stu-id="fb2c4-187">For example:</span></span>

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

<span data-ttu-id="fb2c4-188">Aby zmienić rozmiar domyślny, ustaw `formatting.json.indentSize` klucz.</span><span class="sxs-lookup"><span data-stu-id="fb2c4-188">To change the default size, set the `formatting.json.indentSize` key.</span></span> <span data-ttu-id="fb2c4-189">Na przykład, aby zawsze używać czterech spacji:</span><span class="sxs-lookup"><span data-stu-id="fb2c4-189">For example, to always use four spaces:</span></span>

```console
pref set formatting.json.indentSize 4
```

<span data-ttu-id="fb2c4-190">Kolejne odpowiedzi przestrzegają ustawień czterech spacji:</span><span class="sxs-lookup"><span data-stu-id="fb2c4-190">Subsequent responses honor the setting of four spaces:</span></span>

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

### <a name="set-the-default-text-editor"></a><span data-ttu-id="fb2c4-191">Ustawianie domyślnego edytora tekstu</span><span class="sxs-lookup"><span data-stu-id="fb2c4-191">Set the default text editor</span></span>

<span data-ttu-id="fb2c4-192">Domyślnie REPL HTTP nie ma edytora tekstu skonfigurowanego do użycia.</span><span class="sxs-lookup"><span data-stu-id="fb2c4-192">By default, the HTTP REPL has no text editor configured for use.</span></span> <span data-ttu-id="fb2c4-193">Aby przetestować metody interfejsu API sieci Web wymagające treści żądania HTTP, należy ustawić domyślny edytor tekstu.</span><span class="sxs-lookup"><span data-stu-id="fb2c4-193">To test web API methods requiring an HTTP request body, a default text editor must be set.</span></span> <span data-ttu-id="fb2c4-194">Narzędzie HTTP REPL uruchamia skonfigurowany Edytor tekstów wyłącznie na potrzeby redagowania treści żądania.</span><span class="sxs-lookup"><span data-stu-id="fb2c4-194">The HTTP REPL tool launches the configured text editor for the sole purpose of composing the request body.</span></span> <span data-ttu-id="fb2c4-195">Uruchom następujące polecenie, aby ustawić preferowany Edytor tekstu jako domyślny:</span><span class="sxs-lookup"><span data-stu-id="fb2c4-195">Run the following command to set your preferred text editor as the default:</span></span>

```console
pref set editor.command.default "<EXECUTABLE>"
```

<span data-ttu-id="fb2c4-196">W poprzednim poleceniu `<EXECUTABLE>` jest pełną ścieżką do pliku wykonywalnego edytora tekstu.</span><span class="sxs-lookup"><span data-stu-id="fb2c4-196">In the preceding command, `<EXECUTABLE>` is the full path to the text editor's executable file.</span></span> <span data-ttu-id="fb2c4-197">Na przykład uruchom następujące polecenie, aby ustawić Visual Studio Code jako domyślny edytor tekstu:</span><span class="sxs-lookup"><span data-stu-id="fb2c4-197">For example, run the following command to set Visual Studio Code as the default text editor:</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="fb2c4-198">Linux</span><span class="sxs-lookup"><span data-stu-id="fb2c4-198">Linux</span></span>](#tab/linux)

```console
pref set editor.command.default "/usr/bin/code"
```

# <a name="macostabmacos"></a>[<span data-ttu-id="fb2c4-199">macOS</span><span class="sxs-lookup"><span data-stu-id="fb2c4-199">macOS</span></span>](#tab/macos)

```console
pref set editor.command.default "/Applications/Visual Studio Code.app/Contents/Resources/app/bin/code"
```

# <a name="windowstabwindows"></a>[<span data-ttu-id="fb2c4-200">Windows</span><span class="sxs-lookup"><span data-stu-id="fb2c4-200">Windows</span></span>](#tab/windows)

```console
pref set editor.command.default "C:\Program Files\Microsoft VS Code\Code.exe"
```

---

<span data-ttu-id="fb2c4-201">Aby uruchomić domyślny edytor tekstu z określonymi argumentami interfejsu wiersza polecenia, `editor.command.default.arguments` należy ustawić klucz.</span><span class="sxs-lookup"><span data-stu-id="fb2c4-201">To launch the default text editor with specific CLI arguments, set the `editor.command.default.arguments` key.</span></span> <span data-ttu-id="fb2c4-202">Załóżmy na przykład, że Visual Studio Code jest domyślnym edytorem tekstu i zawsze chcesz, aby REPL HTTP mógł otworzyć Visual Studio Code w nowej sesji z wyłączonymi rozszerzeniami.</span><span class="sxs-lookup"><span data-stu-id="fb2c4-202">For example, assume Visual Studio Code is the default text editor and that you always want the HTTP REPL to open Visual Studio Code in a new session with extensions disabled.</span></span> <span data-ttu-id="fb2c4-203">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="fb2c4-203">Run the following command:</span></span>

```console
pref set editor.command.default.arguments "--disable-extensions --new-window"
```

### <a name="set-the-swagger-search-paths"></a><span data-ttu-id="fb2c4-204">Ustawianie ścieżek wyszukiwania struktury Swagger</span><span class="sxs-lookup"><span data-stu-id="fb2c4-204">Set the Swagger search paths</span></span>

<span data-ttu-id="fb2c4-205">Domyślnie REPL http ma zestaw ścieżek względnych, których używa do znajdowania dokumentu struktury Swagger podczas wykonywania `connect` polecenia `--swagger` bez opcji.</span><span class="sxs-lookup"><span data-stu-id="fb2c4-205">By default, the HTTP REPL has a set of relative paths that it uses to find the Swagger document when executing the `connect` command without the `--swagger` option.</span></span> <span data-ttu-id="fb2c4-206">Ścieżki względne są łączone z ścieżkami głównymi i podstawowymi określonymi `connect` w poleceniu.</span><span class="sxs-lookup"><span data-stu-id="fb2c4-206">These relative paths are combined with the root and base paths specified in the `connect` command.</span></span> <span data-ttu-id="fb2c4-207">Domyślne ścieżki względne to:</span><span class="sxs-lookup"><span data-stu-id="fb2c4-207">The default relative paths are:</span></span>

- <span data-ttu-id="fb2c4-208">*plik Swagger. JSON*</span><span class="sxs-lookup"><span data-stu-id="fb2c4-208">*swagger.json*</span></span>
- <span data-ttu-id="fb2c4-209">*Swagger/V1/Swagger. JSON*</span><span class="sxs-lookup"><span data-stu-id="fb2c4-209">*swagger/v1/swagger.json*</span></span>
- <span data-ttu-id="fb2c4-210">*/swagger.json*</span><span class="sxs-lookup"><span data-stu-id="fb2c4-210">*/swagger.json*</span></span>
- <span data-ttu-id="fb2c4-211">*/swagger/v1/swagger.json*</span><span class="sxs-lookup"><span data-stu-id="fb2c4-211">*/swagger/v1/swagger.json*</span></span>

<span data-ttu-id="fb2c4-212">Aby użyć innego zestawu ścieżek wyszukiwania w środowisku, ustaw `swagger.searchPaths` preferencję.</span><span class="sxs-lookup"><span data-stu-id="fb2c4-212">To use a different set of search paths in your environment, set the `swagger.searchPaths` preference.</span></span> <span data-ttu-id="fb2c4-213">Wartość musi być rozdzielaną potokami listą ścieżek względnych.</span><span class="sxs-lookup"><span data-stu-id="fb2c4-213">The value must be a pipe-delimited list of relative paths.</span></span> <span data-ttu-id="fb2c4-214">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="fb2c4-214">For example:</span></span>

```console
pref set swagger.searchPaths "swagger/v2/swagger.json|swagger/v3/swagger.json"
```

## <a name="test-http-get-requests"></a><span data-ttu-id="fb2c4-215">Testuj żądania HTTP GET</span><span class="sxs-lookup"><span data-stu-id="fb2c4-215">Test HTTP GET requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="fb2c4-216">Streszczenie</span><span class="sxs-lookup"><span data-stu-id="fb2c4-216">Synopsis</span></span>

```console
get <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="fb2c4-217">Argumenty</span><span class="sxs-lookup"><span data-stu-id="fb2c4-217">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="fb2c4-218">Parametr trasy, jeśli istnieje, oczekiwany przez skojarzoną metodę akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="fb2c4-218">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="fb2c4-219">Opcje</span><span class="sxs-lookup"><span data-stu-id="fb2c4-219">Options</span></span>

<span data-ttu-id="fb2c4-220">Następujące opcje są dostępne dla `get` polecenia:</span><span class="sxs-lookup"><span data-stu-id="fb2c4-220">The following options are available for the `get` command:</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

### <a name="example"></a><span data-ttu-id="fb2c4-221">Przykład</span><span class="sxs-lookup"><span data-stu-id="fb2c4-221">Example</span></span>

<span data-ttu-id="fb2c4-222">Aby wydać żądanie HTTP GET:</span><span class="sxs-lookup"><span data-stu-id="fb2c4-222">To issue an HTTP GET request:</span></span>

1. <span data-ttu-id="fb2c4-223">`get` Uruchom polecenie w punkcie końcowym, który go obsługuje:</span><span class="sxs-lookup"><span data-stu-id="fb2c4-223">Run the `get` command on an endpoint that supports it:</span></span>

    ```console
    https://localhost:5001/people~ get
    ```

    <span data-ttu-id="fb2c4-224">Poprzednie polecenie wyświetla następujący format danych wyjściowych:</span><span class="sxs-lookup"><span data-stu-id="fb2c4-224">The preceding command displays the following output format:</span></span>

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

1. <span data-ttu-id="fb2c4-225">Pobierz konkretny rekord, przekazując parametr do `get` polecenia:</span><span class="sxs-lookup"><span data-stu-id="fb2c4-225">Retrieve a specific record by passing a parameter to the `get` command:</span></span>

    ```console
    https://localhost:5001/people~ get 2
    ```

    <span data-ttu-id="fb2c4-226">Poprzednie polecenie wyświetla następujący format danych wyjściowych:</span><span class="sxs-lookup"><span data-stu-id="fb2c4-226">The preceding command displays the following output format:</span></span>

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

## <a name="test-http-post-requests"></a><span data-ttu-id="fb2c4-227">Testowanie żądań POST protokołu HTTP</span><span class="sxs-lookup"><span data-stu-id="fb2c4-227">Test HTTP POST requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="fb2c4-228">Streszczenie</span><span class="sxs-lookup"><span data-stu-id="fb2c4-228">Synopsis</span></span>

```console
post <PARAMETER> [-c|--content] [-f|--file] [-h|--header] [--no-body] [-F|--no-formatting] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="fb2c4-229">Argumenty</span><span class="sxs-lookup"><span data-stu-id="fb2c4-229">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="fb2c4-230">Parametr trasy, jeśli istnieje, oczekiwany przez skojarzoną metodę akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="fb2c4-230">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="fb2c4-231">Opcje</span><span class="sxs-lookup"><span data-stu-id="fb2c4-231">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

[!INCLUDE [HTTP request body CLI options](~/includes/http-repl/requires-body-options.md)]

### <a name="example"></a><span data-ttu-id="fb2c4-232">Przykład</span><span class="sxs-lookup"><span data-stu-id="fb2c4-232">Example</span></span>

<span data-ttu-id="fb2c4-233">Aby wydać żądanie HTTP POST:</span><span class="sxs-lookup"><span data-stu-id="fb2c4-233">To issue an HTTP POST request:</span></span>

1. <span data-ttu-id="fb2c4-234">`post` Uruchom polecenie w punkcie końcowym, który go obsługuje:</span><span class="sxs-lookup"><span data-stu-id="fb2c4-234">Run the `post` command on an endpoint that supports it:</span></span>

    ```console
    https://localhost:5001/people~ post -h Content-Type=application/json
    ```

    <span data-ttu-id="fb2c4-235">W poprzednim poleceniu nagłówek żądania `Content-Type` http jest ustawiany w taki sposób, aby wskazywał typ nośnika treści żądania JSON.</span><span class="sxs-lookup"><span data-stu-id="fb2c4-235">In the preceding command, the `Content-Type` HTTP request header is set to indicate a request body media type of JSON.</span></span> <span data-ttu-id="fb2c4-236">Domyślny edytor tekstu otwiera plik *. tmp* z szablonem JSON reprezentującym treść żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="fb2c4-236">The default text editor opens a *.tmp* file with a JSON template representing the HTTP request body.</span></span> <span data-ttu-id="fb2c4-237">Przykład:</span><span class="sxs-lookup"><span data-stu-id="fb2c4-237">For example:</span></span>

    ```json
    {
      "id": 0,
      "name": ""
    }
    ```

    > [!TIP]
    > <span data-ttu-id="fb2c4-238">Aby ustawić domyślny edytor tekstu, zobacz sekcję [Ustawianie domyślnego edytora tekstu](#set-the-default-text-editor) .</span><span class="sxs-lookup"><span data-stu-id="fb2c4-238">To set the default text editor, see the [Set the default text editor](#set-the-default-text-editor) section.</span></span>

1. <span data-ttu-id="fb2c4-239">Zmodyfikuj szablon JSON, aby spełniał wymagania dotyczące weryfikacji modelu:</span><span class="sxs-lookup"><span data-stu-id="fb2c4-239">Modify the JSON template to satisfy model validation requirements:</span></span>

    ```json
    {
      "id": 0,
      "name": "Scott Addie"
    }
    ```

1. <span data-ttu-id="fb2c4-240">Zapisz plik *. tmp* i Zamknij Edytor tekstu.</span><span class="sxs-lookup"><span data-stu-id="fb2c4-240">Save the *.tmp* file, and close the text editor.</span></span> <span data-ttu-id="fb2c4-241">Następujące dane wyjściowe są wyświetlane w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="fb2c4-241">The following output appears in the command shell:</span></span>

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

## <a name="test-http-put-requests"></a><span data-ttu-id="fb2c4-242">Testowanie żądań HTTP PUT</span><span class="sxs-lookup"><span data-stu-id="fb2c4-242">Test HTTP PUT requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="fb2c4-243">Streszczenie</span><span class="sxs-lookup"><span data-stu-id="fb2c4-243">Synopsis</span></span>

```console
put <PARAMETER> [-c|--content] [-f|--file] [-h|--header] [--no-body] [-F|--no-formatting] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="fb2c4-244">Argumenty</span><span class="sxs-lookup"><span data-stu-id="fb2c4-244">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="fb2c4-245">Parametr trasy, jeśli istnieje, oczekiwany przez skojarzoną metodę akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="fb2c4-245">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="fb2c4-246">Opcje</span><span class="sxs-lookup"><span data-stu-id="fb2c4-246">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

[!INCLUDE [HTTP request body CLI options](~/includes/http-repl/requires-body-options.md)]

### <a name="example"></a><span data-ttu-id="fb2c4-247">Przykład</span><span class="sxs-lookup"><span data-stu-id="fb2c4-247">Example</span></span>

<span data-ttu-id="fb2c4-248">Aby wydać żądanie HTTP PUT:</span><span class="sxs-lookup"><span data-stu-id="fb2c4-248">To issue an HTTP PUT request:</span></span>

1. <span data-ttu-id="fb2c4-249">*Opcjonalne*: `get` Uruchom polecenie, aby wyświetlić dane przed zmodyfikowaniem:</span><span class="sxs-lookup"><span data-stu-id="fb2c4-249">*Optional*: Run the `get` command to view the data before modifying it:</span></span>

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

    <span data-ttu-id="fb2c4-250">W poprzednim poleceniu nagłówek żądania `Content-Type` http jest ustawiany w taki sposób, aby wskazywał typ nośnika treści żądania JSON.</span><span class="sxs-lookup"><span data-stu-id="fb2c4-250">In the preceding command, the `Content-Type` HTTP request header is set to indicate a request body media type of JSON.</span></span> <span data-ttu-id="fb2c4-251">Domyślny edytor tekstu otwiera plik *. tmp* z szablonem JSON reprezentującym treść żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="fb2c4-251">The default text editor opens a *.tmp* file with a JSON template representing the HTTP request body.</span></span> <span data-ttu-id="fb2c4-252">Przykład:</span><span class="sxs-lookup"><span data-stu-id="fb2c4-252">For example:</span></span>

    ```json
    {
      "id": 0,
      "name": ""
    }
    ```

    > [!TIP]
    > <span data-ttu-id="fb2c4-253">Aby ustawić domyślny edytor tekstu, zobacz sekcję [Ustawianie domyślnego edytora tekstu](#set-the-default-text-editor) .</span><span class="sxs-lookup"><span data-stu-id="fb2c4-253">To set the default text editor, see the [Set the default text editor](#set-the-default-text-editor) section.</span></span>

1. <span data-ttu-id="fb2c4-254">Zmodyfikuj szablon JSON, aby spełniał wymagania dotyczące weryfikacji modelu:</span><span class="sxs-lookup"><span data-stu-id="fb2c4-254">Modify the JSON template to satisfy model validation requirements:</span></span>

    ```json
    {
      "id": 2,
      "name": "Cherry"
    }
    ```

1. <span data-ttu-id="fb2c4-255">Zapisz plik *. tmp* i Zamknij Edytor tekstu.</span><span class="sxs-lookup"><span data-stu-id="fb2c4-255">Save the *.tmp* file, and close the text editor.</span></span> <span data-ttu-id="fb2c4-256">Następujące dane wyjściowe są wyświetlane w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="fb2c4-256">The following output appears in the command shell:</span></span>

    ```console
    [main 2019-06-28T17:27:01.805Z] update#setState idle
    HTTP/1.1 204 No Content
    Date: Fri, 28 Jun 2019 17:28:21 GMT
    Server: Kestrel
    ```

1. <span data-ttu-id="fb2c4-257">*Opcjonalne*: `get` Wydaj polecenie, aby zobaczyć modyfikacje.</span><span class="sxs-lookup"><span data-stu-id="fb2c4-257">*Optional*: Issue a `get` command to see the modifications.</span></span> <span data-ttu-id="fb2c4-258">Na przykład, jeśli wpisano "wiśnię" w edytorze tekstu, `get` zwraca następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="fb2c4-258">For example, if you typed "Cherry" in the text editor, a `get` returns the following:</span></span>

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

## <a name="test-http-delete-requests"></a><span data-ttu-id="fb2c4-259">Testowanie żądań HTTP DELETE</span><span class="sxs-lookup"><span data-stu-id="fb2c4-259">Test HTTP DELETE requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="fb2c4-260">Streszczenie</span><span class="sxs-lookup"><span data-stu-id="fb2c4-260">Synopsis</span></span>

```console
delete <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="fb2c4-261">Argumenty</span><span class="sxs-lookup"><span data-stu-id="fb2c4-261">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="fb2c4-262">Parametr trasy, jeśli istnieje, oczekiwany przez skojarzoną metodę akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="fb2c4-262">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="fb2c4-263">Opcje</span><span class="sxs-lookup"><span data-stu-id="fb2c4-263">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

### <a name="example"></a><span data-ttu-id="fb2c4-264">Przykład</span><span class="sxs-lookup"><span data-stu-id="fb2c4-264">Example</span></span>

<span data-ttu-id="fb2c4-265">Aby wydać żądanie HTTP DELETE:</span><span class="sxs-lookup"><span data-stu-id="fb2c4-265">To issue an HTTP DELETE request:</span></span>

1. <span data-ttu-id="fb2c4-266">*Opcjonalne*: `get` Uruchom polecenie, aby wyświetlić dane przed zmodyfikowaniem:</span><span class="sxs-lookup"><span data-stu-id="fb2c4-266">*Optional*: Run the `get` command to view the data before modifying it:</span></span>

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

    <span data-ttu-id="fb2c4-267">Poprzednie polecenie wyświetla następujący format danych wyjściowych:</span><span class="sxs-lookup"><span data-stu-id="fb2c4-267">The preceding command displays the following output format:</span></span>

    ```console
    HTTP/1.1 204 No Content
    Date: Fri, 28 Jun 2019 17:36:42 GMT
    Server: Kestrel
    ```

1. <span data-ttu-id="fb2c4-268">*Opcjonalne*: `get` Wydaj polecenie, aby zobaczyć modyfikacje.</span><span class="sxs-lookup"><span data-stu-id="fb2c4-268">*Optional*: Issue a `get` command to see the modifications.</span></span> <span data-ttu-id="fb2c4-269">W tym przykładzie `get` zwraca następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="fb2c4-269">In this example, a `get` returns the following:</span></span>

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

## <a name="test-http-patch-requests"></a><span data-ttu-id="fb2c4-270">Testuj żądania poprawek HTTP</span><span class="sxs-lookup"><span data-stu-id="fb2c4-270">Test HTTP PATCH requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="fb2c4-271">Streszczenie</span><span class="sxs-lookup"><span data-stu-id="fb2c4-271">Synopsis</span></span>

```console
patch <PARAMETER> [-c|--content] [-f|--file] [-h|--header] [--no-body] [-F|--no-formatting] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="fb2c4-272">Argumenty</span><span class="sxs-lookup"><span data-stu-id="fb2c4-272">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="fb2c4-273">Parametr trasy, jeśli istnieje, oczekiwany przez skojarzoną metodę akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="fb2c4-273">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="fb2c4-274">Opcje</span><span class="sxs-lookup"><span data-stu-id="fb2c4-274">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

[!INCLUDE [HTTP request body CLI options](~/includes/http-repl/requires-body-options.md)]

## <a name="test-http-head-requests"></a><span data-ttu-id="fb2c4-275">Testowanie żądań głównych HTTP</span><span class="sxs-lookup"><span data-stu-id="fb2c4-275">Test HTTP HEAD requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="fb2c4-276">Streszczenie</span><span class="sxs-lookup"><span data-stu-id="fb2c4-276">Synopsis</span></span>

```console
head <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="fb2c4-277">Argumenty</span><span class="sxs-lookup"><span data-stu-id="fb2c4-277">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="fb2c4-278">Parametr trasy, jeśli istnieje, oczekiwany przez skojarzoną metodę akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="fb2c4-278">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="fb2c4-279">Opcje</span><span class="sxs-lookup"><span data-stu-id="fb2c4-279">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

## <a name="test-http-options-requests"></a><span data-ttu-id="fb2c4-280">Testowe żądania opcji HTTP</span><span class="sxs-lookup"><span data-stu-id="fb2c4-280">Test HTTP OPTIONS requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="fb2c4-281">Streszczenie</span><span class="sxs-lookup"><span data-stu-id="fb2c4-281">Synopsis</span></span>

```console
options <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="fb2c4-282">Argumenty</span><span class="sxs-lookup"><span data-stu-id="fb2c4-282">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="fb2c4-283">Parametr trasy, jeśli istnieje, oczekiwany przez skojarzoną metodę akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="fb2c4-283">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="fb2c4-284">Opcje</span><span class="sxs-lookup"><span data-stu-id="fb2c4-284">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

## <a name="set-http-request-headers"></a><span data-ttu-id="fb2c4-285">Ustawianie nagłówków żądań HTTP</span><span class="sxs-lookup"><span data-stu-id="fb2c4-285">Set HTTP request headers</span></span>

<span data-ttu-id="fb2c4-286">Aby ustawić nagłówek żądania HTTP, należy użyć jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="fb2c4-286">To set an HTTP request header, use one of the following approaches:</span></span>

1. <span data-ttu-id="fb2c4-287">Ustaw wartość inline z żądaniem HTTP.</span><span class="sxs-lookup"><span data-stu-id="fb2c4-287">Set inline with the HTTP request.</span></span> <span data-ttu-id="fb2c4-288">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="fb2c4-288">For example:</span></span>

  ```console
  https://localhost:5001/people~ post -h Content-Type=application/json
  ```

  <span data-ttu-id="fb2c4-289">W przypadku wcześniejszego podejścia każdy unikatowy nagłówek żądania HTTP wymaga własnej `-h` opcji.</span><span class="sxs-lookup"><span data-stu-id="fb2c4-289">With the preceding approach, each distinct HTTP request header requires its own `-h` option.</span></span>

1. <span data-ttu-id="fb2c4-290">Ustaw przed wysłaniem żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="fb2c4-290">Set before sending the HTTP request.</span></span> <span data-ttu-id="fb2c4-291">Przykład:</span><span class="sxs-lookup"><span data-stu-id="fb2c4-291">For example:</span></span>

  ```console
  https://localhost:5001/people~ set header Content-Type application/json
  ```

  <span data-ttu-id="fb2c4-292">Podczas ustawiania nagłówka przed wysłaniem żądania nagłówek pozostaje ustawiony na czas trwania sesji powłoki poleceń.</span><span class="sxs-lookup"><span data-stu-id="fb2c4-292">When setting the header before sending a request, the header remains set for the duration of the command shell session.</span></span> <span data-ttu-id="fb2c4-293">Aby wyczyścić nagłówek, podaj wartość pustą.</span><span class="sxs-lookup"><span data-stu-id="fb2c4-293">To clear the header, provide an empty value.</span></span> <span data-ttu-id="fb2c4-294">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="fb2c4-294">For example:</span></span>

  ```console
  https://localhost:5001/people~ set header Content-Type
  ```

## <a name="toggle-http-request-display"></a><span data-ttu-id="fb2c4-295">Przełącz wyświetlanie żądań HTTP</span><span class="sxs-lookup"><span data-stu-id="fb2c4-295">Toggle HTTP request display</span></span>

<span data-ttu-id="fb2c4-296">Domyślnie wyświetlanie wysyłanego żądania HTTP jest pomijane.</span><span class="sxs-lookup"><span data-stu-id="fb2c4-296">By default, display of the HTTP request being sent is suppressed.</span></span> <span data-ttu-id="fb2c4-297">Istnieje możliwość zmiany odpowiedniego ustawienia dla czasu trwania sesji powłoki poleceń.</span><span class="sxs-lookup"><span data-stu-id="fb2c4-297">It's possible to change the corresponding setting for the duration of the command shell session.</span></span>

### <a name="enable-request-display"></a><span data-ttu-id="fb2c4-298">Włącz wyświetlanie żądań</span><span class="sxs-lookup"><span data-stu-id="fb2c4-298">Enable request display</span></span>

<span data-ttu-id="fb2c4-299">Wyświetl wysyłane żądanie HTTP, uruchamiając `echo on` polecenie.</span><span class="sxs-lookup"><span data-stu-id="fb2c4-299">View the HTTP request being sent by running the `echo on` command.</span></span> <span data-ttu-id="fb2c4-300">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="fb2c4-300">For example:</span></span>

```console
https://localhost:5001/people~ echo on
Request echoing is on
```

<span data-ttu-id="fb2c4-301">Kolejne żądania HTTP w bieżącej sesji wyświetlają nagłówki żądań.</span><span class="sxs-lookup"><span data-stu-id="fb2c4-301">Subsequent HTTP requests in the current session display the request headers.</span></span> <span data-ttu-id="fb2c4-302">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="fb2c4-302">For example:</span></span>

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

### <a name="disable-request-display"></a><span data-ttu-id="fb2c4-303">Wyłącz wyświetlanie żądań</span><span class="sxs-lookup"><span data-stu-id="fb2c4-303">Disable request display</span></span>

<span data-ttu-id="fb2c4-304">Pomijaj wyświetlanie wysyłanego żądania HTTP przez uruchomienie `echo off` polecenia.</span><span class="sxs-lookup"><span data-stu-id="fb2c4-304">Suppress display of the HTTP request being sent by running the `echo off` command.</span></span> <span data-ttu-id="fb2c4-305">Przykład:</span><span class="sxs-lookup"><span data-stu-id="fb2c4-305">For example:</span></span>

```console
https://localhost:5001/people~ echo off
Request echoing is off
```

## <a name="run-a-script"></a><span data-ttu-id="fb2c4-306">Uruchamianie skryptu</span><span class="sxs-lookup"><span data-stu-id="fb2c4-306">Run a script</span></span>

<span data-ttu-id="fb2c4-307">Jeśli często wykonujesz ten sam zestaw poleceń HTTP REPL, Rozważ przechowywanie ich w pliku tekstowym.</span><span class="sxs-lookup"><span data-stu-id="fb2c4-307">If you frequently execute the same set of HTTP REPL commands, consider storing them in a text file.</span></span> <span data-ttu-id="fb2c4-308">Polecenia w pliku mają taki sam formularz jak te wykonywane ręcznie w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="fb2c4-308">Commands in the file take the same form as those executed manually on the command line.</span></span> <span data-ttu-id="fb2c4-309">Polecenia mogą być wykonywane w sposób wsadowy przy użyciu `run` polecenia.</span><span class="sxs-lookup"><span data-stu-id="fb2c4-309">The commands can be executed in a batched fashion using the `run` command.</span></span> <span data-ttu-id="fb2c4-310">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="fb2c4-310">For example:</span></span>

1. <span data-ttu-id="fb2c4-311">Utwórz plik tekstowy zawierający zestaw poleceń rozdzielanych znakami nowego wiersza.</span><span class="sxs-lookup"><span data-stu-id="fb2c4-311">Create a text file containing a set of newline-delimited commands.</span></span> <span data-ttu-id="fb2c4-312">Aby to zilustrować, należy rozważyć plik *People-Script. txt* zawierający następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="fb2c4-312">To illustrate, consider a *people-script.txt* file containing the following commands:</span></span>

    ```text
    set base https://localhost:5001
    ls
    cd People
    ls
    get 1
    ```

1. <span data-ttu-id="fb2c4-313">`run` Wykonaj polecenie, przekazując w ścieżce pliku tekstowego.</span><span class="sxs-lookup"><span data-stu-id="fb2c4-313">Execute the `run` command, passing in the text file's path.</span></span> <span data-ttu-id="fb2c4-314">Przykład:</span><span class="sxs-lookup"><span data-stu-id="fb2c4-314">For example:</span></span>

    ```console
    https://localhost:5001/~ run C:\http-repl-scripts\people-script.txt
    ```

    <span data-ttu-id="fb2c4-315">Wyświetlane są następujące dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="fb2c4-315">The following output appears:</span></span>

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

## <a name="clear-the-output"></a><span data-ttu-id="fb2c4-316">Wyczyść dane wyjściowe</span><span class="sxs-lookup"><span data-stu-id="fb2c4-316">Clear the output</span></span>

<span data-ttu-id="fb2c4-317">Aby usunąć wszystkie dane wyjściowe zapisywane do powłoki poleceń za pomocą narzędzia http REPL, uruchom `clear` polecenie lub. `cls`</span><span class="sxs-lookup"><span data-stu-id="fb2c4-317">To remove all output written to the command shell by the HTTP REPL tool, run the `clear` or `cls` command.</span></span> <span data-ttu-id="fb2c4-318">Do zilustrowania, Załóżmy, że powłoka poleceń zawiera następujące dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="fb2c4-318">To illustrate, imagine the command shell contains the following output:</span></span>

```console
httprepl https://localhost:5001
(Disconnected)~ set base "https://localhost:5001"
Using swagger metadata from https://localhost:5001/swagger/v1/swagger.json

https://localhost:5001/~ ls
.        []
Fruits   [get|post]
People   [get|post]

https://localhost:5001/~
```

<span data-ttu-id="fb2c4-319">Uruchom następujące polecenie, aby wyczyścić dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="fb2c4-319">Run the following command to clear the output:</span></span>

```console
https://localhost:5001/~ clear
```

<span data-ttu-id="fb2c4-320">Po uruchomieniu poprzedniego polecenia powłoka poleceń zawiera tylko następujące dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="fb2c4-320">After running the preceding command, the command shell contains only the following output:</span></span>

```console
https://localhost:5001/~
```

## <a name="additional-resources"></a><span data-ttu-id="fb2c4-321">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="fb2c4-321">Additional resources</span></span>

* [<span data-ttu-id="fb2c4-322">Żądania interfejsu API REST</span><span class="sxs-lookup"><span data-stu-id="fb2c4-322">REST API requests</span></span>](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#74-supported-methods)
* [<span data-ttu-id="fb2c4-323">Repozytorium usługi GitHub HTTP REPL</span><span class="sxs-lookup"><span data-stu-id="fb2c4-323">HTTP REPL GitHub repository</span></span>](https://github.com/aspnet/HttpRepl)
