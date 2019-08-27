---
title: Testowanie interfejsów API sieci Web przy użyciu protokołu HTTP REPL
author: scottaddie
description: Dowiedz się, jak przeglądać i testować ASP.NET Core internetowy interfejs API przy użyciu globalnego narzędzia REPL .NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 07/25/2019
uid: web-api/http-repl
ms.openlocfilehash: d2c5f774595e7a2223e84cc76eecdb9baa04adfe
ms.sourcegitcommit: 776598f71da0d1e4c9e923b3b395d3c3b5825796
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/26/2019
ms.locfileid: "70024806"
---
# <a name="test-web-apis-with-the-http-repl"></a><span data-ttu-id="33401-103">Testowanie interfejsów API sieci Web przy użyciu protokołu HTTP REPL</span><span class="sxs-lookup"><span data-stu-id="33401-103">Test web APIs with the HTTP REPL</span></span>

<span data-ttu-id="33401-104">Przez [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="33401-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="33401-105">Pętla HTTP Read-eval-Print (REPL) to:</span><span class="sxs-lookup"><span data-stu-id="33401-105">The HTTP Read-Eval-Print Loop (REPL) is:</span></span>

* <span data-ttu-id="33401-106">Lekkie, międzyplatformowe narzędzie wiersza polecenia, które jest obsługiwane wszędzie tam, gdzie jest obsługiwane środowisko .NET Core.</span><span class="sxs-lookup"><span data-stu-id="33401-106">A lightweight, cross-platform command-line tool that's supported everywhere .NET Core is supported.</span></span>
* <span data-ttu-id="33401-107">Służy do wykonywania żądań HTTP do testowania ASP.NET Core interfejsów API sieci Web (oraz non-ASP.NET podstawowych interfejsów API sieci Web) i wyświetlania ich wyników.</span><span class="sxs-lookup"><span data-stu-id="33401-107">Used for making HTTP requests to test ASP.NET Core web APIs (and non-ASP.NET Core web APIs) and view their results.</span></span>
* <span data-ttu-id="33401-108">Możliwość testowania interfejsów API sieci Web hostowanych w dowolnym środowisku, w tym hosta lokalnego i Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="33401-108">Capable of testing web APIs hosted in any environment, including localhost and Azure App Service.</span></span>

<span data-ttu-id="33401-109">Obsługiwane są następujące [czasowniki http](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#74-supported-methods) :</span><span class="sxs-lookup"><span data-stu-id="33401-109">The following [HTTP verbs](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#74-supported-methods) are supported:</span></span>

* [<span data-ttu-id="33401-110">USUNIĘTY</span><span class="sxs-lookup"><span data-stu-id="33401-110">DELETE</span></span>](#test-http-delete-requests)
* [<span data-ttu-id="33401-111">GET</span><span class="sxs-lookup"><span data-stu-id="33401-111">GET</span></span>](#test-http-get-requests)
* [<span data-ttu-id="33401-112">MTP</span><span class="sxs-lookup"><span data-stu-id="33401-112">HEAD</span></span>](#test-http-head-requests)
* [<span data-ttu-id="33401-113">OPCJE</span><span class="sxs-lookup"><span data-stu-id="33401-113">OPTIONS</span></span>](#test-http-options-requests)
* [<span data-ttu-id="33401-114">WYSŁANA</span><span class="sxs-lookup"><span data-stu-id="33401-114">PATCH</span></span>](#test-http-patch-requests)
* [<span data-ttu-id="33401-115">POST</span><span class="sxs-lookup"><span data-stu-id="33401-115">POST</span></span>](#test-http-post-requests)
* [<span data-ttu-id="33401-116">UBRANI</span><span class="sxs-lookup"><span data-stu-id="33401-116">PUT</span></span>](#test-http-put-requests)

<span data-ttu-id="33401-117">Aby wykonać te czynności, [Wyświetl lub Pobierz przykładowy ASP.NET Core internetowy interfejs API](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/http-repl/samples) ([jak pobrać](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="33401-117">To follow along, [view or download the sample ASP.NET Core web API](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/http-repl/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="33401-118">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="33401-118">Prerequisites</span></span>

* [!INCLUDE [2.1-SDK](~/includes/2.1-SDK.md)]

## <a name="installation"></a><span data-ttu-id="33401-119">Instalacja</span><span class="sxs-lookup"><span data-stu-id="33401-119">Installation</span></span>

<span data-ttu-id="33401-120">Aby zainstalować REPL HTTP, uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="33401-120">To install the HTTP REPL, run the following command:</span></span>

```console
dotnet tool install -g Microsoft.dotnet-httprepl --version "3.0.0-*"
```

<span data-ttu-id="33401-121">[Narzędzie globalne platformy .NET Core](/dotnet/core/tools/global-tools#install-a-global-tool) jest instalowane z pakietu NuGet [Microsoft. dotnet-httprepl](https://www.nuget.org/packages/Microsoft.dotnet-httprepl) .</span><span class="sxs-lookup"><span data-stu-id="33401-121">A [.NET Core Global Tool](/dotnet/core/tools/global-tools#install-a-global-tool) is installed from the [Microsoft.dotnet-httprepl](https://www.nuget.org/packages/Microsoft.dotnet-httprepl) NuGet package.</span></span>

## <a name="usage"></a><span data-ttu-id="33401-122">Użycie</span><span class="sxs-lookup"><span data-stu-id="33401-122">Usage</span></span>

<span data-ttu-id="33401-123">Po pomyślnej instalacji narzędzia Uruchom następujące polecenie, aby uruchomić REPL HTTP:</span><span class="sxs-lookup"><span data-stu-id="33401-123">After successful installation of the tool, run the following command to start the HTTP REPL:</span></span>

```console
dotnet httprepl
```

<span data-ttu-id="33401-124">Aby wyświetlić dostępne polecenia HTTP REPL, Uruchom jedno z następujących poleceń:</span><span class="sxs-lookup"><span data-stu-id="33401-124">To view the available HTTP REPL commands, run one of the following commands:</span></span>

```console
dotnet httprepl -h
```

```console
dotnet httprepl --help
```

<span data-ttu-id="33401-125">Wyświetlane są następujące dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="33401-125">The following output is displayed:</span></span>

```console
Usage:
  dotnet httprepl [<BASE_ADDRESS>] [options]

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

<span data-ttu-id="33401-126">REPL HTTP oferuje polecenie uzupełniania.</span><span class="sxs-lookup"><span data-stu-id="33401-126">The HTTP REPL offers command completion.</span></span> <span data-ttu-id="33401-127">Naciśnięcie klawisza <kbd>Tab</kbd> wykonuje iterację na liście poleceń, które uzupełniają wpisane znaki lub punkt końcowy interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="33401-127">Pressing the <kbd>Tab</kbd> key iterates through the list of commands that complete the characters or API endpoint that you typed.</span></span> <span data-ttu-id="33401-128">W poniższych sekcjach znajduje się opis dostępnych poleceń interfejsu wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="33401-128">The following sections outline the available CLI commands.</span></span>

## <a name="connect-to-the-web-api"></a><span data-ttu-id="33401-129">Nawiązywanie połączenia z interfejsem API sieci Web</span><span class="sxs-lookup"><span data-stu-id="33401-129">Connect to the web API</span></span>

<span data-ttu-id="33401-130">Połącz się z interfejsem API sieci Web, uruchamiając następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="33401-130">Connect to a web API by running the following command:</span></span>

```console
dotnet httprepl <ROOT URI>
```

<span data-ttu-id="33401-131">`<ROOT URI>`jest podstawowym identyfikatorem URI dla internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="33401-131">`<ROOT URI>` is the base URI for the web API.</span></span> <span data-ttu-id="33401-132">Przykład:</span><span class="sxs-lookup"><span data-stu-id="33401-132">For example:</span></span>

```console
dotnet httprepl https://localhost:5001
```

<span data-ttu-id="33401-133">Alternatywnie Uruchom następujące polecenie w dowolnym momencie podczas działania REPL HTTP:</span><span class="sxs-lookup"><span data-stu-id="33401-133">Alternatively, run the following command at any time while the HTTP REPL is running:</span></span>

```console
connect <ROOT URI>
```

<span data-ttu-id="33401-134">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="33401-134">For example:</span></span>

```console
(Disconnected)~ connect https://localhost:5001
```

## <a name="manually-point-to-the-swagger-document-for-the-web-api"></a><span data-ttu-id="33401-135">Ręcznie wskaż dokument struktury Swagger dla internetowego interfejsu API</span><span class="sxs-lookup"><span data-stu-id="33401-135">Manually point to the Swagger document for the web API</span></span>

<span data-ttu-id="33401-136">Powyższe polecenie Connect podejmie próbę automatycznego znalezienia dokumentu struktury Swagger.</span><span class="sxs-lookup"><span data-stu-id="33401-136">The connect command above will attempt to find the Swagger document automatically.</span></span> <span data-ttu-id="33401-137">Jeśli z jakiegoś powodu nie można tego zrobić, możesz określić identyfikator URI dokumentu struktury Swagger dla internetowego interfejsu API przy użyciu `--swagger` opcji:</span><span class="sxs-lookup"><span data-stu-id="33401-137">If for some reason it is unable to do so, you can specify the URI of the Swagger document for the web API by using the `--swagger` option:</span></span>

```console
connect <ROOT URI> --swagger <SWAGGER URI>
```

<span data-ttu-id="33401-138">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="33401-138">For example:</span></span>

```console
(Disconnected)~ connect https://localhost:5001 --swagger /swagger/v1/swagger.json
```

## <a name="navigate-the-web-api"></a><span data-ttu-id="33401-139">Nawigowanie po interfejsie API sieci Web</span><span class="sxs-lookup"><span data-stu-id="33401-139">Navigate the web API</span></span>

### <a name="view-available-endpoints"></a><span data-ttu-id="33401-140">Wyświetl dostępne punkty końcowe</span><span class="sxs-lookup"><span data-stu-id="33401-140">View available endpoints</span></span>

<span data-ttu-id="33401-141">Aby wyświetlić listę różnych punktów końcowych (kontrolerów) znajdujących się w bieżącej ścieżce adresu internetowego interfejsu API `ls` , `dir` Uruchom polecenie lub:</span><span class="sxs-lookup"><span data-stu-id="33401-141">To list the different endpoints (controllers) at the current path of the web API address, run the `ls` or `dir` command:</span></span>

```console
https://localhot:5001/~ ls
```

<span data-ttu-id="33401-142">Wyświetlany jest następujący format danych wyjściowych:</span><span class="sxs-lookup"><span data-stu-id="33401-142">The following output format is displayed:</span></span>

```console
.        []
Fruits   [get|post]
People   [get|post]

https://localhost:5001/~
```

<span data-ttu-id="33401-143">Powyższe dane wyjściowe wskazują, że dostępne są dwa kontrolery: `Fruits` i `People`.</span><span class="sxs-lookup"><span data-stu-id="33401-143">The preceding output indicates that there are two controllers available: `Fruits` and `People`.</span></span> <span data-ttu-id="33401-144">Oba kontrolery obsługują bez parametrów operacje GET i POST HTTP.</span><span class="sxs-lookup"><span data-stu-id="33401-144">Both controllers support parameterless HTTP GET and POST operations.</span></span>

<span data-ttu-id="33401-145">Przechodzenie do określonego kontrolera ujawnia więcej szczegółów.</span><span class="sxs-lookup"><span data-stu-id="33401-145">Navigating into a specific controller reveals more detail.</span></span> <span data-ttu-id="33401-146">Na przykład następujące dane wyjściowe polecenia pokazują `Fruits` kontroler obsługuje również operacje Get, PUT i DELETE protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="33401-146">For example, the following command's output shows the `Fruits` controller also supports HTTP GET, PUT, and DELETE operations.</span></span> <span data-ttu-id="33401-147">Każda z tych operacji oczekuje `id` parametru w marszrucie:</span><span class="sxs-lookup"><span data-stu-id="33401-147">Each of these operations expects an `id` parameter in the route:</span></span>

```console
https://localhost:5001/fruits~ ls
.      [get|post]
..     []
{id}   [get|put|delete]

https://localhost:5001/fruits~
```

<span data-ttu-id="33401-148">Alternatywnie można uruchomić `ui` polecenie, aby otworzyć stronę interfejsu użytkownika programu Swagger interfejsu API sieci Web w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="33401-148">Alternatively, run the `ui` command to open the web API's Swagger UI page in a browser.</span></span> <span data-ttu-id="33401-149">Przykład:</span><span class="sxs-lookup"><span data-stu-id="33401-149">For example:</span></span>

```console
https://localhost:5001/~ ui
```

### <a name="navigate-to-an-endpoint"></a><span data-ttu-id="33401-150">Przejdź do punktu końcowego</span><span class="sxs-lookup"><span data-stu-id="33401-150">Navigate to an endpoint</span></span>

<span data-ttu-id="33401-151">Aby przejść do innego punktu końcowego w internetowym interfejsie API, `cd` Uruchom polecenie:</span><span class="sxs-lookup"><span data-stu-id="33401-151">To navigate to a different endpoint on the web API, run the `cd` command:</span></span>

```console
https://localhost:5001/~ cd people
```

<span data-ttu-id="33401-152">W ścieżce następującego `cd` polecenia jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="33401-152">The path following the `cd` command is case insensitive.</span></span> <span data-ttu-id="33401-153">Wyświetlany jest następujący format danych wyjściowych:</span><span class="sxs-lookup"><span data-stu-id="33401-153">The following output format is displayed:</span></span>

```console
/people    [get|post]

https://localhost:5001/people~
```

## <a name="customize-the-http-repl"></a><span data-ttu-id="33401-154">Dostosowywanie REPL HTTP</span><span class="sxs-lookup"><span data-stu-id="33401-154">Customize the HTTP REPL</span></span>

<span data-ttu-id="33401-155">Domyślne [kolory](#set-color-preferences) REPL http można dostosować.</span><span class="sxs-lookup"><span data-stu-id="33401-155">The HTTP REPL's default [colors](#set-color-preferences) can be customized.</span></span> <span data-ttu-id="33401-156">Ponadto można zdefiniować [domyślny edytor tekstu](#set-the-default-text-editor) .</span><span class="sxs-lookup"><span data-stu-id="33401-156">Additionally, a [default text editor](#set-the-default-text-editor) can be defined.</span></span> <span data-ttu-id="33401-157">Preferencje HTTP REPL są utrwalane w bieżącej sesji i są honorowane w przyszłych sesjach.</span><span class="sxs-lookup"><span data-stu-id="33401-157">The HTTP REPL preferences are persisted across the current session and are honored in future sessions.</span></span> <span data-ttu-id="33401-158">Po zmodyfikowaniu preferencje są przechowywane w następującym pliku:</span><span class="sxs-lookup"><span data-stu-id="33401-158">Once modified, the preferences are stored in the following file:</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="33401-159">Linux</span><span class="sxs-lookup"><span data-stu-id="33401-159">Linux</span></span>](#tab/linux)

<span data-ttu-id="33401-160">*% HOME%/.httpreplprefs*</span><span class="sxs-lookup"><span data-stu-id="33401-160">*%HOME%/.httpreplprefs*</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="33401-161">macOS</span><span class="sxs-lookup"><span data-stu-id="33401-161">macOS</span></span>](#tab/macos)

<span data-ttu-id="33401-162">*% HOME%/.httpreplprefs*</span><span class="sxs-lookup"><span data-stu-id="33401-162">*%HOME%/.httpreplprefs*</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="33401-163">Windows</span><span class="sxs-lookup"><span data-stu-id="33401-163">Windows</span></span>](#tab/windows)

<span data-ttu-id="33401-164">*% USERPROFILE%\\. httpreplprefs*</span><span class="sxs-lookup"><span data-stu-id="33401-164">*%USERPROFILE%\\.httpreplprefs*</span></span>

---

<span data-ttu-id="33401-165">Plik *. httpreplprefs* jest ładowany podczas uruchamiania i nie jest monitorowany pod kątem zmian w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="33401-165">The *.httpreplprefs* file is loaded on startup and not monitored for changes at runtime.</span></span> <span data-ttu-id="33401-166">Ręczne modyfikacje pliku zaczną obowiązywać dopiero po ponownym uruchomieniu narzędzia.</span><span class="sxs-lookup"><span data-stu-id="33401-166">Manual modifications to the file take effect only after restarting the tool.</span></span>

### <a name="view-the-settings"></a><span data-ttu-id="33401-167">Wyświetlanie ustawień</span><span class="sxs-lookup"><span data-stu-id="33401-167">View the settings</span></span>

<span data-ttu-id="33401-168">Aby wyświetlić dostępne ustawienia, uruchom `pref get` polecenie.</span><span class="sxs-lookup"><span data-stu-id="33401-168">To view the available settings, run the `pref get` command.</span></span> <span data-ttu-id="33401-169">Przykład:</span><span class="sxs-lookup"><span data-stu-id="33401-169">For example:</span></span>

```console
https://localhost:5001/~ pref get
```

<span data-ttu-id="33401-170">Poprzednie polecenie wyświetla dostępne pary klucz-wartość:</span><span class="sxs-lookup"><span data-stu-id="33401-170">The preceding command displays the available key-value pairs:</span></span>

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

### <a name="set-color-preferences"></a><span data-ttu-id="33401-171">Ustawianie preferencji koloru</span><span class="sxs-lookup"><span data-stu-id="33401-171">Set color preferences</span></span>

<span data-ttu-id="33401-172">Kolorowanie odpowiedzi jest obecnie obsługiwane tylko w przypadku formatu JSON.</span><span class="sxs-lookup"><span data-stu-id="33401-172">Response colorization is currently supported for JSON only.</span></span> <span data-ttu-id="33401-173">Aby dostosować domyślne kolorowanie narzędzi HTTP REPL, Znajdź klucz odpowiadający kolorowi do zmiany.</span><span class="sxs-lookup"><span data-stu-id="33401-173">To customize the default HTTP REPL tool coloring, locate the key corresponding to the color to be changed.</span></span> <span data-ttu-id="33401-174">Aby uzyskać instrukcje dotyczące znajdowania kluczy, zobacz sekcję [Wyświetlanie ustawień](#view-the-settings) .</span><span class="sxs-lookup"><span data-stu-id="33401-174">For instructions on how to find the keys, see the [View the settings](#view-the-settings) section.</span></span> <span data-ttu-id="33401-175">Na przykład zmień `colors.json` wartość klucza z `Green` na `White` w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="33401-175">For example, change the `colors.json` key value from `Green` to `White` as follows:</span></span>

```console
https://localhost:5001/people~ pref set colors.json White
```

<span data-ttu-id="33401-176">Można używać tylko [dozwolonych kolorów](https://github.com/aspnet/HttpRepl/blob/01d5c3c3373e98fe566ff5ef8a17c571de880293/src/Microsoft.Repl/ConsoleHandling/AllowedColors.cs) .</span><span class="sxs-lookup"><span data-stu-id="33401-176">Only the [allowed colors](https://github.com/aspnet/HttpRepl/blob/01d5c3c3373e98fe566ff5ef8a17c571de880293/src/Microsoft.Repl/ConsoleHandling/AllowedColors.cs) may be used.</span></span> <span data-ttu-id="33401-177">Kolejne żądania HTTP wyświetlają dane wyjściowe z nowym kolorem.</span><span class="sxs-lookup"><span data-stu-id="33401-177">Subsequent HTTP requests display output with the new coloring.</span></span>

<span data-ttu-id="33401-178">Jeśli określone klucze kolorów nie są ustawione, brane są więcej kluczy ogólnych.</span><span class="sxs-lookup"><span data-stu-id="33401-178">When specific color keys aren't set, more generic keys are considered.</span></span> <span data-ttu-id="33401-179">Aby zademonstrować to zachowanie rezerwowe, należy wziąć pod uwagę następujący przykład:</span><span class="sxs-lookup"><span data-stu-id="33401-179">To demonstrate this fallback behavior, consider the following example:</span></span>

* <span data-ttu-id="33401-180">Jeśli `colors.json.name` nie ma wartości, `colors.json.string` jest używana.</span><span class="sxs-lookup"><span data-stu-id="33401-180">If `colors.json.name` doesn't have a value, `colors.json.string` is used.</span></span>
* <span data-ttu-id="33401-181">Jeśli `colors.json.string` nie ma wartości, `colors.json.literal` jest używana.</span><span class="sxs-lookup"><span data-stu-id="33401-181">If `colors.json.string` doesn't have a value, `colors.json.literal` is used.</span></span>
* <span data-ttu-id="33401-182">Jeśli `colors.json.literal` nie ma wartości, `colors.json` jest używana.</span><span class="sxs-lookup"><span data-stu-id="33401-182">If `colors.json.literal` doesn't have a value, `colors.json` is used.</span></span> 
* <span data-ttu-id="33401-183">Jeśli `colors.json` nie ma wartości, używany jest domyślny kolor tekstu powłoki poleceń (`AllowedColors.None`).</span><span class="sxs-lookup"><span data-stu-id="33401-183">If `colors.json` doesn't have a value, the command shell's default text color (`AllowedColors.None`) is used.</span></span>

### <a name="set-indentation-size"></a><span data-ttu-id="33401-184">Ustaw rozmiar wcięcia</span><span class="sxs-lookup"><span data-stu-id="33401-184">Set indentation size</span></span>

<span data-ttu-id="33401-185">Dostosowanie rozmiaru wcięcia odpowiedzi jest obecnie obsługiwane tylko w przypadku formatu JSON.</span><span class="sxs-lookup"><span data-stu-id="33401-185">Response indentation size customization is currently supported for JSON only.</span></span> <span data-ttu-id="33401-186">Domyślny rozmiar to dwie spacje.</span><span class="sxs-lookup"><span data-stu-id="33401-186">The default size is two spaces.</span></span> <span data-ttu-id="33401-187">Przykład:</span><span class="sxs-lookup"><span data-stu-id="33401-187">For example:</span></span>

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

<span data-ttu-id="33401-188">Aby zmienić rozmiar domyślny, ustaw `formatting.json.indentSize` klucz.</span><span class="sxs-lookup"><span data-stu-id="33401-188">To change the default size, set the `formatting.json.indentSize` key.</span></span> <span data-ttu-id="33401-189">Na przykład, aby zawsze używać czterech spacji:</span><span class="sxs-lookup"><span data-stu-id="33401-189">For example, to always use four spaces:</span></span>

```console
pref set formatting.json.indentSize 4
```

<span data-ttu-id="33401-190">Kolejne odpowiedzi przestrzegają ustawień czterech spacji:</span><span class="sxs-lookup"><span data-stu-id="33401-190">Subsequent responses honor the setting of four spaces:</span></span>

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

### <a name="set-indentation-size"></a><span data-ttu-id="33401-191">Ustaw rozmiar wcięcia</span><span class="sxs-lookup"><span data-stu-id="33401-191">Set indentation size</span></span>

<span data-ttu-id="33401-192">Dostosowanie rozmiaru wcięcia odpowiedzi jest obecnie obsługiwane tylko w przypadku formatu JSON.</span><span class="sxs-lookup"><span data-stu-id="33401-192">Response indentation size customization is currently supported for JSON only.</span></span> <span data-ttu-id="33401-193">Domyślny rozmiar to dwie spacje.</span><span class="sxs-lookup"><span data-stu-id="33401-193">The default size is two spaces.</span></span> <span data-ttu-id="33401-194">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="33401-194">For example:</span></span>

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

<span data-ttu-id="33401-195">Aby zmienić rozmiar domyślny, ustaw `formatting.json.indentSize` klucz.</span><span class="sxs-lookup"><span data-stu-id="33401-195">To change the default size, set the `formatting.json.indentSize` key.</span></span> <span data-ttu-id="33401-196">Na przykład, aby zawsze używać czterech spacji:</span><span class="sxs-lookup"><span data-stu-id="33401-196">For example, to always use four spaces:</span></span>

```console
pref set formatting.json.indentSize 4
```

<span data-ttu-id="33401-197">Kolejne odpowiedzi przestrzegają ustawień czterech spacji:</span><span class="sxs-lookup"><span data-stu-id="33401-197">Subsequent responses honor the setting of four spaces:</span></span>

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

### <a name="set-the-default-text-editor"></a><span data-ttu-id="33401-198">Ustawianie domyślnego edytora tekstu</span><span class="sxs-lookup"><span data-stu-id="33401-198">Set the default text editor</span></span>

<span data-ttu-id="33401-199">Domyślnie REPL HTTP nie ma edytora tekstu skonfigurowanego do użycia.</span><span class="sxs-lookup"><span data-stu-id="33401-199">By default, the HTTP REPL has no text editor configured for use.</span></span> <span data-ttu-id="33401-200">Aby przetestować metody interfejsu API sieci Web wymagające treści żądania HTTP, należy ustawić domyślny edytor tekstu.</span><span class="sxs-lookup"><span data-stu-id="33401-200">To test web API methods requiring an HTTP request body, a default text editor must be set.</span></span> <span data-ttu-id="33401-201">Narzędzie HTTP REPL uruchamia skonfigurowany Edytor tekstów wyłącznie na potrzeby redagowania treści żądania.</span><span class="sxs-lookup"><span data-stu-id="33401-201">The HTTP REPL tool launches the configured text editor for the sole purpose of composing the request body.</span></span> <span data-ttu-id="33401-202">Uruchom następujące polecenie, aby ustawić preferowany Edytor tekstu jako domyślny:</span><span class="sxs-lookup"><span data-stu-id="33401-202">Run the following command to set your preferred text editor as the default:</span></span>

```console
pref set editor.command.default "<EXECUTABLE>"
```

<span data-ttu-id="33401-203">W poprzednim poleceniu `<EXECUTABLE>` jest pełną ścieżką do pliku wykonywalnego edytora tekstu.</span><span class="sxs-lookup"><span data-stu-id="33401-203">In the preceding command, `<EXECUTABLE>` is the full path to the text editor's executable file.</span></span> <span data-ttu-id="33401-204">Na przykład uruchom następujące polecenie, aby ustawić Visual Studio Code jako domyślny edytor tekstu:</span><span class="sxs-lookup"><span data-stu-id="33401-204">For example, run the following command to set Visual Studio Code as the default text editor:</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="33401-205">Linux</span><span class="sxs-lookup"><span data-stu-id="33401-205">Linux</span></span>](#tab/linux)

```console
pref set editor.command.default "/usr/bin/code"
```

# <a name="macostabmacos"></a>[<span data-ttu-id="33401-206">macOS</span><span class="sxs-lookup"><span data-stu-id="33401-206">macOS</span></span>](#tab/macos)

```console
pref set editor.command.default "/Applications/Visual Studio Code.app/Contents/Resources/app/bin/code"
```

# <a name="windowstabwindows"></a>[<span data-ttu-id="33401-207">Windows</span><span class="sxs-lookup"><span data-stu-id="33401-207">Windows</span></span>](#tab/windows)

```console
pref set editor.command.default "C:\Program Files\Microsoft VS Code\Code.exe"
```

---

<span data-ttu-id="33401-208">Aby uruchomić domyślny edytor tekstu z określonymi argumentami interfejsu wiersza polecenia, `editor.command.default.arguments` należy ustawić klucz.</span><span class="sxs-lookup"><span data-stu-id="33401-208">To launch the default text editor with specific CLI arguments, set the `editor.command.default.arguments` key.</span></span> <span data-ttu-id="33401-209">Załóżmy na przykład, że Visual Studio Code jest domyślnym edytorem tekstu i zawsze chcesz, aby REPL HTTP mógł otworzyć Visual Studio Code w nowej sesji z wyłączonymi rozszerzeniami.</span><span class="sxs-lookup"><span data-stu-id="33401-209">For example, assume Visual Studio Code is the default text editor and that you always want the HTTP REPL to open Visual Studio Code in a new session with extensions disabled.</span></span> <span data-ttu-id="33401-210">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="33401-210">Run the following command:</span></span>

```console
pref set editor.command.default.arguments "--disable-extensions --new-window"
```

### <a name="set-the-swagger-search-paths"></a><span data-ttu-id="33401-211">Ustawianie ścieżek wyszukiwania struktury Swagger</span><span class="sxs-lookup"><span data-stu-id="33401-211">Set the Swagger search paths</span></span>

<span data-ttu-id="33401-212">Domyślnie REPL http ma zestaw ścieżek względnych, których używa do znajdowania dokumentu struktury Swagger podczas wykonywania `connect` polecenia `--swagger` bez opcji.</span><span class="sxs-lookup"><span data-stu-id="33401-212">By default, the HTTP REPL has a set of relative paths that it uses to find the Swagger document when executing the `connect` command without the `--swagger` option.</span></span> <span data-ttu-id="33401-213">Ścieżki względne są łączone z ścieżkami głównymi i podstawowymi określonymi `connect` w poleceniu.</span><span class="sxs-lookup"><span data-stu-id="33401-213">These relative paths are combined with the root and base paths specified in the `connect` command.</span></span> <span data-ttu-id="33401-214">Domyślne ścieżki względne to:</span><span class="sxs-lookup"><span data-stu-id="33401-214">The default relative paths are:</span></span>

- <span data-ttu-id="33401-215">*plik Swagger. JSON*</span><span class="sxs-lookup"><span data-stu-id="33401-215">*swagger.json*</span></span>
- <span data-ttu-id="33401-216">*Swagger/V1/Swagger. JSON*</span><span class="sxs-lookup"><span data-stu-id="33401-216">*swagger/v1/swagger.json*</span></span>
- <span data-ttu-id="33401-217">*/swagger.json*</span><span class="sxs-lookup"><span data-stu-id="33401-217">*/swagger.json*</span></span>
- <span data-ttu-id="33401-218">*/swagger/v1/swagger.json*</span><span class="sxs-lookup"><span data-stu-id="33401-218">*/swagger/v1/swagger.json*</span></span>

<span data-ttu-id="33401-219">Aby użyć innego zestawu ścieżek wyszukiwania w środowisku, ustaw `swagger.searchPaths` preferencję.</span><span class="sxs-lookup"><span data-stu-id="33401-219">To use a different set of search paths in your environment, set the `swagger.searchPaths` preference.</span></span> <span data-ttu-id="33401-220">Wartość musi być rozdzielaną potokami listą ścieżek względnych.</span><span class="sxs-lookup"><span data-stu-id="33401-220">The value must be a pipe-delimited list of relative paths.</span></span> <span data-ttu-id="33401-221">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="33401-221">For example:</span></span>

```console
pref set swagger.searchPaths "swagger/v2/swagger.json|swagger/v3/swagger.json
```

## <a name="test-http-get-requests"></a><span data-ttu-id="33401-222">Testuj żądania HTTP GET</span><span class="sxs-lookup"><span data-stu-id="33401-222">Test HTTP GET requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="33401-223">Streszczenie</span><span class="sxs-lookup"><span data-stu-id="33401-223">Synopsis</span></span>

```console
get <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="33401-224">Argumenty</span><span class="sxs-lookup"><span data-stu-id="33401-224">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="33401-225">Parametr trasy, jeśli istnieje, oczekiwany przez skojarzoną metodę akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="33401-225">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="33401-226">Opcje</span><span class="sxs-lookup"><span data-stu-id="33401-226">Options</span></span>

<span data-ttu-id="33401-227">Następujące opcje są dostępne dla `get` polecenia:</span><span class="sxs-lookup"><span data-stu-id="33401-227">The following options are available for the `get` command:</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

### <a name="example"></a><span data-ttu-id="33401-228">Przykład</span><span class="sxs-lookup"><span data-stu-id="33401-228">Example</span></span>

<span data-ttu-id="33401-229">Aby wydać żądanie HTTP GET:</span><span class="sxs-lookup"><span data-stu-id="33401-229">To issue an HTTP GET request:</span></span>

1. <span data-ttu-id="33401-230">`get` Uruchom polecenie w punkcie końcowym, który go obsługuje:</span><span class="sxs-lookup"><span data-stu-id="33401-230">Run the `get` command on an endpoint that supports it:</span></span>

    ```console
    https://localhost:5001/people~ get
    ```

    <span data-ttu-id="33401-231">Poprzednie polecenie wyświetla następujący format danych wyjściowych:</span><span class="sxs-lookup"><span data-stu-id="33401-231">The preceding command displays the following output format:</span></span>

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

1. <span data-ttu-id="33401-232">Pobierz konkretny rekord, przekazując parametr do `get` polecenia:</span><span class="sxs-lookup"><span data-stu-id="33401-232">Retrieve a specific record by passing a parameter to the `get` command:</span></span>

    ```console
    https://localhost:5001/people~ get 2
    ```

    <span data-ttu-id="33401-233">Poprzednie polecenie wyświetla następujący format danych wyjściowych:</span><span class="sxs-lookup"><span data-stu-id="33401-233">The preceding command displays the following output format:</span></span>

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

## <a name="test-http-post-requests"></a><span data-ttu-id="33401-234">Testowanie żądań POST protokołu HTTP</span><span class="sxs-lookup"><span data-stu-id="33401-234">Test HTTP POST requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="33401-235">Streszczenie</span><span class="sxs-lookup"><span data-stu-id="33401-235">Synopsis</span></span>

```console
post <PARAMETER> [-c|--content] [-f|--file] [-h|--header] [--no-body] [-F|--no-formatting] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="33401-236">Argumenty</span><span class="sxs-lookup"><span data-stu-id="33401-236">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="33401-237">Parametr trasy, jeśli istnieje, oczekiwany przez skojarzoną metodę akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="33401-237">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="33401-238">Opcje</span><span class="sxs-lookup"><span data-stu-id="33401-238">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

[!INCLUDE [HTTP request body CLI options](~/includes/http-repl/requires-body-options.md)]

### <a name="example"></a><span data-ttu-id="33401-239">Przykład</span><span class="sxs-lookup"><span data-stu-id="33401-239">Example</span></span>

<span data-ttu-id="33401-240">Aby wydać żądanie HTTP POST:</span><span class="sxs-lookup"><span data-stu-id="33401-240">To issue an HTTP POST request:</span></span>

1. <span data-ttu-id="33401-241">`post` Uruchom polecenie w punkcie końcowym, który go obsługuje:</span><span class="sxs-lookup"><span data-stu-id="33401-241">Run the `post` command on an endpoint that supports it:</span></span>

    ```console
    https://localhost:5001/people~ post -h Content-Type=application/json
    ```

    <span data-ttu-id="33401-242">W poprzednim poleceniu nagłówek żądania `Content-Type` http jest ustawiany w taki sposób, aby wskazywał typ nośnika treści żądania JSON.</span><span class="sxs-lookup"><span data-stu-id="33401-242">In the preceding command, the `Content-Type` HTTP request header is set to indicate a request body media type of JSON.</span></span> <span data-ttu-id="33401-243">Domyślny edytor tekstu otwiera plik *. tmp* z szablonem JSON reprezentującym treść żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="33401-243">The default text editor opens a *.tmp* file with a JSON template representing the HTTP request body.</span></span> <span data-ttu-id="33401-244">Przykład:</span><span class="sxs-lookup"><span data-stu-id="33401-244">For example:</span></span>

    ```json
    {
      "id": 0,
      "name": ""
    }
    ```

    > [!TIP]
    > <span data-ttu-id="33401-245">Aby ustawić domyślny edytor tekstu, zobacz sekcję [Ustawianie domyślnego edytora tekstu](#set-the-default-text-editor) .</span><span class="sxs-lookup"><span data-stu-id="33401-245">To set the default text editor, see the [Set the default text editor](#set-the-default-text-editor) section.</span></span>

1. <span data-ttu-id="33401-246">Zmodyfikuj szablon JSON, aby spełniał wymagania dotyczące weryfikacji modelu:</span><span class="sxs-lookup"><span data-stu-id="33401-246">Modify the JSON template to satisfy model validation requirements:</span></span>

    ```json
    {
      "id": 0,
      "name": "Scott Addie"
    }
    ```

1. <span data-ttu-id="33401-247">Zapisz plik *. tmp* i Zamknij Edytor tekstu.</span><span class="sxs-lookup"><span data-stu-id="33401-247">Save the *.tmp* file, and close the text editor.</span></span> <span data-ttu-id="33401-248">Następujące dane wyjściowe są wyświetlane w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="33401-248">The following output appears in the command shell:</span></span>

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

## <a name="test-http-put-requests"></a><span data-ttu-id="33401-249">Testowanie żądań HTTP PUT</span><span class="sxs-lookup"><span data-stu-id="33401-249">Test HTTP PUT requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="33401-250">Streszczenie</span><span class="sxs-lookup"><span data-stu-id="33401-250">Synopsis</span></span>

```console
put <PARAMETER> [-c|--content] [-f|--file] [-h|--header] [--no-body] [-F|--no-formatting] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="33401-251">Argumenty</span><span class="sxs-lookup"><span data-stu-id="33401-251">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="33401-252">Parametr trasy, jeśli istnieje, oczekiwany przez skojarzoną metodę akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="33401-252">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="33401-253">Opcje</span><span class="sxs-lookup"><span data-stu-id="33401-253">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

[!INCLUDE [HTTP request body CLI options](~/includes/http-repl/requires-body-options.md)]

### <a name="example"></a><span data-ttu-id="33401-254">Przykład</span><span class="sxs-lookup"><span data-stu-id="33401-254">Example</span></span>

<span data-ttu-id="33401-255">Aby wydać żądanie HTTP PUT:</span><span class="sxs-lookup"><span data-stu-id="33401-255">To issue an HTTP PUT request:</span></span>

1. <span data-ttu-id="33401-256">*Opcjonalne*: `get` Uruchom polecenie, aby wyświetlić dane przed zmodyfikowaniem:</span><span class="sxs-lookup"><span data-stu-id="33401-256">*Optional*: Run the `get` command to view the data before modifying it:</span></span>

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

    <span data-ttu-id="33401-257">W poprzednim poleceniu nagłówek żądania `Content-Type` http jest ustawiany w taki sposób, aby wskazywał typ nośnika treści żądania JSON.</span><span class="sxs-lookup"><span data-stu-id="33401-257">In the preceding command, the `Content-Type` HTTP request header is set to indicate a request body media type of JSON.</span></span> <span data-ttu-id="33401-258">Domyślny edytor tekstu otwiera plik *. tmp* z szablonem JSON reprezentującym treść żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="33401-258">The default text editor opens a *.tmp* file with a JSON template representing the HTTP request body.</span></span> <span data-ttu-id="33401-259">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="33401-259">For example:</span></span>

    ```json
    {
      "id": 0,
      "name": ""
    }
    ```

    > [!TIP]
    > <span data-ttu-id="33401-260">Aby ustawić domyślny edytor tekstu, zobacz sekcję [Ustawianie domyślnego edytora tekstu](#set-the-default-text-editor) .</span><span class="sxs-lookup"><span data-stu-id="33401-260">To set the default text editor, see the [Set the default text editor](#set-the-default-text-editor) section.</span></span>

1. <span data-ttu-id="33401-261">Zmodyfikuj szablon JSON, aby spełniał wymagania dotyczące weryfikacji modelu:</span><span class="sxs-lookup"><span data-stu-id="33401-261">Modify the JSON template to satisfy model validation requirements:</span></span>

    ```json
    {
      "id": 2,
      "name": "Cherry"
    }
    ```

1. <span data-ttu-id="33401-262">Zapisz plik *. tmp* i Zamknij Edytor tekstu.</span><span class="sxs-lookup"><span data-stu-id="33401-262">Save the *.tmp* file, and close the text editor.</span></span> <span data-ttu-id="33401-263">Następujące dane wyjściowe są wyświetlane w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="33401-263">The following output appears in the command shell:</span></span>

    ```console
    [main 2019-06-28T17:27:01.805Z] update#setState idle
    HTTP/1.1 204 No Content
    Date: Fri, 28 Jun 2019 17:28:21 GMT
    Server: Kestrel
    ```

1. <span data-ttu-id="33401-264">*Opcjonalne*: `get` Wydaj polecenie, aby zobaczyć modyfikacje.</span><span class="sxs-lookup"><span data-stu-id="33401-264">*Optional*: Issue a `get` command to see the modifications.</span></span> <span data-ttu-id="33401-265">Na przykład, jeśli wpisano "wiśnię" w edytorze tekstu, `get` zwraca następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="33401-265">For example, if you typed "Cherry" in the text editor, a `get` returns the following:</span></span>

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

## <a name="test-http-delete-requests"></a><span data-ttu-id="33401-266">Testowanie żądań HTTP DELETE</span><span class="sxs-lookup"><span data-stu-id="33401-266">Test HTTP DELETE requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="33401-267">Streszczenie</span><span class="sxs-lookup"><span data-stu-id="33401-267">Synopsis</span></span>

```console
delete <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="33401-268">Argumenty</span><span class="sxs-lookup"><span data-stu-id="33401-268">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="33401-269">Parametr trasy, jeśli istnieje, oczekiwany przez skojarzoną metodę akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="33401-269">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="33401-270">Opcje</span><span class="sxs-lookup"><span data-stu-id="33401-270">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

### <a name="example"></a><span data-ttu-id="33401-271">Przykład</span><span class="sxs-lookup"><span data-stu-id="33401-271">Example</span></span>

<span data-ttu-id="33401-272">Aby wydać żądanie HTTP DELETE:</span><span class="sxs-lookup"><span data-stu-id="33401-272">To issue an HTTP DELETE request:</span></span>

1. <span data-ttu-id="33401-273">*Opcjonalne*: `get` Uruchom polecenie, aby wyświetlić dane przed zmodyfikowaniem:</span><span class="sxs-lookup"><span data-stu-id="33401-273">*Optional*: Run the `get` command to view the data before modifying it:</span></span>

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

    <span data-ttu-id="33401-274">Poprzednie polecenie wyświetla następujący format danych wyjściowych:</span><span class="sxs-lookup"><span data-stu-id="33401-274">The preceding command displays the following output format:</span></span>

    ```console
    HTTP/1.1 204 No Content
    Date: Fri, 28 Jun 2019 17:36:42 GMT
    Server: Kestrel
    ```

1. <span data-ttu-id="33401-275">*Opcjonalne*: `get` Wydaj polecenie, aby zobaczyć modyfikacje.</span><span class="sxs-lookup"><span data-stu-id="33401-275">*Optional*: Issue a `get` command to see the modifications.</span></span> <span data-ttu-id="33401-276">W tym przykładzie `get` zwraca następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="33401-276">In this example, a `get` returns the following:</span></span>

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

## <a name="test-http-patch-requests"></a><span data-ttu-id="33401-277">Testuj żądania poprawek HTTP</span><span class="sxs-lookup"><span data-stu-id="33401-277">Test HTTP PATCH requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="33401-278">Streszczenie</span><span class="sxs-lookup"><span data-stu-id="33401-278">Synopsis</span></span>

```console
patch <PARAMETER> [-c|--content] [-f|--file] [-h|--header] [--no-body] [-F|--no-formatting] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="33401-279">Argumenty</span><span class="sxs-lookup"><span data-stu-id="33401-279">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="33401-280">Parametr trasy, jeśli istnieje, oczekiwany przez skojarzoną metodę akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="33401-280">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="33401-281">Opcje</span><span class="sxs-lookup"><span data-stu-id="33401-281">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

[!INCLUDE [HTTP request body CLI options](~/includes/http-repl/requires-body-options.md)]

## <a name="test-http-head-requests"></a><span data-ttu-id="33401-282">Testowanie żądań głównych HTTP</span><span class="sxs-lookup"><span data-stu-id="33401-282">Test HTTP HEAD requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="33401-283">Streszczenie</span><span class="sxs-lookup"><span data-stu-id="33401-283">Synopsis</span></span>

```console
head <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="33401-284">Argumenty</span><span class="sxs-lookup"><span data-stu-id="33401-284">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="33401-285">Parametr trasy, jeśli istnieje, oczekiwany przez skojarzoną metodę akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="33401-285">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="33401-286">Opcje</span><span class="sxs-lookup"><span data-stu-id="33401-286">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

## <a name="test-http-options-requests"></a><span data-ttu-id="33401-287">Testowe żądania opcji HTTP</span><span class="sxs-lookup"><span data-stu-id="33401-287">Test HTTP OPTIONS requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="33401-288">Streszczenie</span><span class="sxs-lookup"><span data-stu-id="33401-288">Synopsis</span></span>

```console
options <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="33401-289">Argumenty</span><span class="sxs-lookup"><span data-stu-id="33401-289">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="33401-290">Parametr trasy, jeśli istnieje, oczekiwany przez skojarzoną metodę akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="33401-290">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="33401-291">Opcje</span><span class="sxs-lookup"><span data-stu-id="33401-291">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

## <a name="set-http-request-headers"></a><span data-ttu-id="33401-292">Ustawianie nagłówków żądań HTTP</span><span class="sxs-lookup"><span data-stu-id="33401-292">Set HTTP request headers</span></span>

<span data-ttu-id="33401-293">Aby ustawić nagłówek żądania HTTP, należy użyć jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="33401-293">To set an HTTP request header, use one of the following approaches:</span></span>

1. <span data-ttu-id="33401-294">Ustaw wartość inline z żądaniem HTTP.</span><span class="sxs-lookup"><span data-stu-id="33401-294">Set inline with the HTTP request.</span></span> <span data-ttu-id="33401-295">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="33401-295">For example:</span></span>

  ```console
  https://localhost:5001/people~ post -h Content-Type=application/json
  ```

  <span data-ttu-id="33401-296">W przypadku wcześniejszego podejścia każdy unikatowy nagłówek żądania HTTP wymaga własnej `-h` opcji.</span><span class="sxs-lookup"><span data-stu-id="33401-296">With the preceding approach, each distinct HTTP request header requires its own `-h` option.</span></span>

1. <span data-ttu-id="33401-297">Ustaw przed wysłaniem żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="33401-297">Set before sending the HTTP request.</span></span> <span data-ttu-id="33401-298">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="33401-298">For example:</span></span>

  ```console
  https://localhost:5001/people~ set header Content-Type application/json
  ```

  <span data-ttu-id="33401-299">Podczas ustawiania nagłówka przed wysłaniem żądania nagłówek pozostaje ustawiony na czas trwania sesji powłoki poleceń.</span><span class="sxs-lookup"><span data-stu-id="33401-299">When setting the header before sending a request, the header remains set for the duration of the command shell session.</span></span> <span data-ttu-id="33401-300">Aby wyczyścić nagłówek, podaj wartość pustą.</span><span class="sxs-lookup"><span data-stu-id="33401-300">To clear the header, provide an empty value.</span></span> <span data-ttu-id="33401-301">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="33401-301">For example:</span></span>

  ```console
  https://localhost:5001/people~ set header Content-Type
  ```

## <a name="toggle-http-request-display"></a><span data-ttu-id="33401-302">Przełącz wyświetlanie żądań HTTP</span><span class="sxs-lookup"><span data-stu-id="33401-302">Toggle HTTP request display</span></span>

<span data-ttu-id="33401-303">Domyślnie wyświetlanie wysyłanego żądania HTTP jest pomijane.</span><span class="sxs-lookup"><span data-stu-id="33401-303">By default, display of the HTTP request being sent is suppressed.</span></span> <span data-ttu-id="33401-304">Istnieje możliwość zmiany odpowiedniego ustawienia dla czasu trwania sesji powłoki poleceń.</span><span class="sxs-lookup"><span data-stu-id="33401-304">It's possible to change the corresponding setting for the duration of the command shell session.</span></span>

### <a name="enable-request-display"></a><span data-ttu-id="33401-305">Włącz wyświetlanie żądań</span><span class="sxs-lookup"><span data-stu-id="33401-305">Enable request display</span></span>

<span data-ttu-id="33401-306">Wyświetl wysyłane żądanie HTTP, uruchamiając `echo on` polecenie.</span><span class="sxs-lookup"><span data-stu-id="33401-306">View the HTTP request being sent by running the `echo on` command.</span></span> <span data-ttu-id="33401-307">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="33401-307">For example:</span></span>

```console
https://localhost:5001/people~ echo on
Request echoing is on
```

<span data-ttu-id="33401-308">Kolejne żądania HTTP w bieżącej sesji wyświetlają nagłówki żądań.</span><span class="sxs-lookup"><span data-stu-id="33401-308">Subsequent HTTP requests in the current session display the request headers.</span></span> <span data-ttu-id="33401-309">Przykład:</span><span class="sxs-lookup"><span data-stu-id="33401-309">For example:</span></span>

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

### <a name="disable-request-display"></a><span data-ttu-id="33401-310">Wyłącz wyświetlanie żądań</span><span class="sxs-lookup"><span data-stu-id="33401-310">Disable request display</span></span>

<span data-ttu-id="33401-311">Pomijaj wyświetlanie wysyłanego żądania HTTP przez uruchomienie `echo off` polecenia.</span><span class="sxs-lookup"><span data-stu-id="33401-311">Suppress display of the HTTP request being sent by running the `echo off` command.</span></span> <span data-ttu-id="33401-312">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="33401-312">For example:</span></span>

```console
https://localhost:5001/people~ echo off
Request echoing is off
```

## <a name="run-a-script"></a><span data-ttu-id="33401-313">Uruchamianie skryptu</span><span class="sxs-lookup"><span data-stu-id="33401-313">Run a script</span></span>

<span data-ttu-id="33401-314">Jeśli często wykonujesz ten sam zestaw poleceń HTTP REPL, Rozważ przechowywanie ich w pliku tekstowym.</span><span class="sxs-lookup"><span data-stu-id="33401-314">If you frequently execute the same set of HTTP REPL commands, consider storing them in a text file.</span></span> <span data-ttu-id="33401-315">Polecenia w pliku mają taki sam formularz jak te wykonywane ręcznie w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="33401-315">Commands in the file take the same form as those executed manually on the command line.</span></span> <span data-ttu-id="33401-316">Polecenia mogą być wykonywane w sposób wsadowy przy użyciu `run` polecenia.</span><span class="sxs-lookup"><span data-stu-id="33401-316">The commands can be executed in a batched fashion using the `run` command.</span></span> <span data-ttu-id="33401-317">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="33401-317">For example:</span></span>

1. <span data-ttu-id="33401-318">Utwórz plik tekstowy zawierający zestaw poleceń rozdzielanych znakami nowego wiersza.</span><span class="sxs-lookup"><span data-stu-id="33401-318">Create a text file containing a set of newline-delimited commands.</span></span> <span data-ttu-id="33401-319">Aby to zilustrować, należy rozważyć plik *People-Script. txt* zawierający następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="33401-319">To illustrate, consider a *people-script.txt* file containing the following commands:</span></span>

    ```text
    set base https://localhost:5001
    ls
    cd People
    ls
    get 1
    ```

1. <span data-ttu-id="33401-320">`run` Wykonaj polecenie, przekazując w ścieżce pliku tekstowego.</span><span class="sxs-lookup"><span data-stu-id="33401-320">Execute the `run` command, passing in the text file's path.</span></span> <span data-ttu-id="33401-321">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="33401-321">For example:</span></span>

    ```console
    https://localhost:5001/~ run C:\http-repl-scripts\people-script.txt
    ```

    <span data-ttu-id="33401-322">Wyświetlane są następujące dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="33401-322">The following output appears:</span></span>

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

## <a name="clear-the-output"></a><span data-ttu-id="33401-323">Wyczyść dane wyjściowe</span><span class="sxs-lookup"><span data-stu-id="33401-323">Clear the output</span></span>

<span data-ttu-id="33401-324">Aby usunąć wszystkie dane wyjściowe zapisywane do powłoki poleceń za pomocą narzędzia http REPL, uruchom `clear` polecenie lub. `cls`</span><span class="sxs-lookup"><span data-stu-id="33401-324">To remove all output written to the command shell by the HTTP REPL tool, run the `clear` or `cls` command.</span></span> <span data-ttu-id="33401-325">Do zilustrowania, Załóżmy, że powłoka poleceń zawiera następujące dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="33401-325">To illustrate, imagine the command shell contains the following output:</span></span>

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

<span data-ttu-id="33401-326">Uruchom następujące polecenie, aby wyczyścić dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="33401-326">Run the following command to clear the output:</span></span>

```console
https://localhost:5001/~ clear
```

<span data-ttu-id="33401-327">Po uruchomieniu poprzedniego polecenia powłoka poleceń zawiera tylko następujące dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="33401-327">After running the preceding command, the command shell contains only the following output:</span></span>

```console
https://localhost:5001/~
```

## <a name="additional-resources"></a><span data-ttu-id="33401-328">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="33401-328">Additional resources</span></span>

* [<span data-ttu-id="33401-329">Żądania interfejsu API REST</span><span class="sxs-lookup"><span data-stu-id="33401-329">REST API requests</span></span>](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#74-supported-methods)
* [<span data-ttu-id="33401-330">Repozytorium usługi GitHub HTTP REPL</span><span class="sxs-lookup"><span data-stu-id="33401-330">HTTP REPL GitHub repository</span></span>](https://github.com/aspnet/HttpRepl)
