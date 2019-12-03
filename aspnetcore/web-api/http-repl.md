---
title: Testowanie interfejsów API sieci Web przy użyciu protokołu HTTP REPL
author: scottaddie
description: Dowiedz się, jak przeglądać i testować ASP.NET Core internetowy interfejs API przy użyciu globalnego narzędzia REPL .NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 12/02/2019
uid: web-api/http-repl
ms.openlocfilehash: c6e3ab5685b5bd0b154d20585fb0d187f81da641
ms.sourcegitcommit: 3b6b0a54b20dc99b0c8c5978400c60adf431072f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/03/2019
ms.locfileid: "74717168"
---
# <a name="test-web-apis-with-the-http-repl"></a><span data-ttu-id="b9dba-103">Testowanie interfejsów API sieci Web przy użyciu protokołu HTTP REPL</span><span class="sxs-lookup"><span data-stu-id="b9dba-103">Test web APIs with the HTTP REPL</span></span>

<span data-ttu-id="b9dba-104">Przez [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="b9dba-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="b9dba-105">Pętla HTTP Read-eval-Print (REPL) to:</span><span class="sxs-lookup"><span data-stu-id="b9dba-105">The HTTP Read-Eval-Print Loop (REPL) is:</span></span>

* <span data-ttu-id="b9dba-106">Lekkie, międzyplatformowe narzędzie wiersza polecenia, które jest obsługiwane wszędzie tam, gdzie jest obsługiwane środowisko .NET Core.</span><span class="sxs-lookup"><span data-stu-id="b9dba-106">A lightweight, cross-platform command-line tool that's supported everywhere .NET Core is supported.</span></span>
* <span data-ttu-id="b9dba-107">Służy do wykonywania żądań HTTP do testowania ASP.NET Core interfejsów API sieci Web (oraz non-ASP.NET podstawowych interfejsów API sieci Web) i wyświetlania ich wyników.</span><span class="sxs-lookup"><span data-stu-id="b9dba-107">Used for making HTTP requests to test ASP.NET Core web APIs (and non-ASP.NET Core web APIs) and view their results.</span></span>
* <span data-ttu-id="b9dba-108">Możliwość testowania interfejsów API sieci Web hostowanych w dowolnym środowisku, w tym hosta lokalnego i Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="b9dba-108">Capable of testing web APIs hosted in any environment, including localhost and Azure App Service.</span></span>

<span data-ttu-id="b9dba-109">Obsługiwane są następujące [czasowniki http](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#74-supported-methods) :</span><span class="sxs-lookup"><span data-stu-id="b9dba-109">The following [HTTP verbs](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#74-supported-methods) are supported:</span></span>

* [<span data-ttu-id="b9dba-110">USUNIĘTY</span><span class="sxs-lookup"><span data-stu-id="b9dba-110">DELETE</span></span>](#test-http-delete-requests)
* [<span data-ttu-id="b9dba-111">Pobierz</span><span class="sxs-lookup"><span data-stu-id="b9dba-111">GET</span></span>](#test-http-get-requests)
* [<span data-ttu-id="b9dba-112">MTP</span><span class="sxs-lookup"><span data-stu-id="b9dba-112">HEAD</span></span>](#test-http-head-requests)
* [<span data-ttu-id="b9dba-113">Opcje</span><span class="sxs-lookup"><span data-stu-id="b9dba-113">OPTIONS</span></span>](#test-http-options-requests)
* [<span data-ttu-id="b9dba-114">WYSŁANA</span><span class="sxs-lookup"><span data-stu-id="b9dba-114">PATCH</span></span>](#test-http-patch-requests)
* [<span data-ttu-id="b9dba-115">POUBOJOWEGO</span><span class="sxs-lookup"><span data-stu-id="b9dba-115">POST</span></span>](#test-http-post-requests)
* [<span data-ttu-id="b9dba-116">Ubrani</span><span class="sxs-lookup"><span data-stu-id="b9dba-116">PUT</span></span>](#test-http-put-requests)

<span data-ttu-id="b9dba-117">Aby wykonać te czynności, [Wyświetl lub Pobierz przykładowy ASP.NET Core internetowy interfejs API](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/http-repl/samples) ([jak pobrać](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="b9dba-117">To follow along, [view or download the sample ASP.NET Core web API](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/http-repl/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b9dba-118">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="b9dba-118">Prerequisites</span></span>

* [!INCLUDE [2.1-SDK](~/includes/2.1-SDK.md)]

## <a name="installation"></a><span data-ttu-id="b9dba-119">Instalacja programu</span><span class="sxs-lookup"><span data-stu-id="b9dba-119">Installation</span></span>

<span data-ttu-id="b9dba-120">Aby zainstalować REPL HTTP, uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="b9dba-120">To install the HTTP REPL, run the following command:</span></span>

```dotnetcli
dotnet tool install -g Microsoft.dotnet-httprepl
```

<span data-ttu-id="b9dba-121">[Narzędzie globalne platformy .NET Core](/dotnet/core/tools/global-tools#install-a-global-tool) jest instalowane z pakietu NuGet [Microsoft. dotnet-httprepl](https://www.nuget.org/packages/Microsoft.dotnet-httprepl) .</span><span class="sxs-lookup"><span data-stu-id="b9dba-121">A [.NET Core Global Tool](/dotnet/core/tools/global-tools#install-a-global-tool) is installed from the [Microsoft.dotnet-httprepl](https://www.nuget.org/packages/Microsoft.dotnet-httprepl) NuGet package.</span></span>

## <a name="usage"></a><span data-ttu-id="b9dba-122">Pomiar</span><span class="sxs-lookup"><span data-stu-id="b9dba-122">Usage</span></span>

<span data-ttu-id="b9dba-123">Po pomyślnej instalacji narzędzia Uruchom następujące polecenie, aby uruchomić REPL HTTP:</span><span class="sxs-lookup"><span data-stu-id="b9dba-123">After successful installation of the tool, run the following command to start the HTTP REPL:</span></span>

```console
httprepl
```

<span data-ttu-id="b9dba-124">Aby wyświetlić dostępne polecenia HTTP REPL, Uruchom jedno z następujących poleceń:</span><span class="sxs-lookup"><span data-stu-id="b9dba-124">To view the available HTTP REPL commands, run one of the following commands:</span></span>

```console
httprepl -h
```

```console
httprepl --help
```

<span data-ttu-id="b9dba-125">Wyświetlane są następujące dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="b9dba-125">The following output is displayed:</span></span>

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

<span data-ttu-id="b9dba-126">REPL HTTP oferuje polecenie uzupełniania.</span><span class="sxs-lookup"><span data-stu-id="b9dba-126">The HTTP REPL offers command completion.</span></span> <span data-ttu-id="b9dba-127">Naciśnięcie klawisza <kbd>Tab</kbd> wykonuje iterację na liście poleceń, które uzupełniają wpisane znaki lub punkt końcowy interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="b9dba-127">Pressing the <kbd>Tab</kbd> key iterates through the list of commands that complete the characters or API endpoint that you typed.</span></span> <span data-ttu-id="b9dba-128">W poniższych sekcjach znajduje się opis dostępnych poleceń interfejsu wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="b9dba-128">The following sections outline the available CLI commands.</span></span>

## <a name="connect-to-the-web-api"></a><span data-ttu-id="b9dba-129">Nawiązywanie połączenia z interfejsem API sieci Web</span><span class="sxs-lookup"><span data-stu-id="b9dba-129">Connect to the web API</span></span>

<span data-ttu-id="b9dba-130">Połącz się z interfejsem API sieci Web, uruchamiając następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="b9dba-130">Connect to a web API by running the following command:</span></span>

```console
httprepl <ROOT URI>
```

<span data-ttu-id="b9dba-131">`<ROOT URI>` jest podstawowym identyfikatorem URI dla internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="b9dba-131">`<ROOT URI>` is the base URI for the web API.</span></span> <span data-ttu-id="b9dba-132">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="b9dba-132">For example:</span></span>

```console
httprepl https://localhost:5001
```

<span data-ttu-id="b9dba-133">Alternatywnie Uruchom następujące polecenie w dowolnym momencie podczas działania REPL HTTP:</span><span class="sxs-lookup"><span data-stu-id="b9dba-133">Alternatively, run the following command at any time while the HTTP REPL is running:</span></span>

```console
connect <ROOT URI>
```

<span data-ttu-id="b9dba-134">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="b9dba-134">For example:</span></span>

```console
(Disconnected)~ connect https://localhost:5001
```

## <a name="manually-point-to-the-swagger-document-for-the-web-api"></a><span data-ttu-id="b9dba-135">Ręcznie wskaż dokument struktury Swagger dla internetowego interfejsu API</span><span class="sxs-lookup"><span data-stu-id="b9dba-135">Manually point to the Swagger document for the web API</span></span>

<span data-ttu-id="b9dba-136">Powyższe polecenie Connect podejmie próbę automatycznego znalezienia dokumentu struktury Swagger.</span><span class="sxs-lookup"><span data-stu-id="b9dba-136">The connect command above will attempt to find the Swagger document automatically.</span></span> <span data-ttu-id="b9dba-137">Jeśli z jakiegoś powodu nie można tego zrobić, możesz określić identyfikator URI dokumentu struktury Swagger dla internetowego interfejsu API przy użyciu opcji `--swagger`:</span><span class="sxs-lookup"><span data-stu-id="b9dba-137">If for some reason it is unable to do so, you can specify the URI of the Swagger document for the web API by using the `--swagger` option:</span></span>

```console
connect <ROOT URI> --swagger <SWAGGER URI>
```

<span data-ttu-id="b9dba-138">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="b9dba-138">For example:</span></span>

```console
(Disconnected)~ connect https://localhost:5001 --swagger /swagger/v1/swagger.json
```

## <a name="navigate-the-web-api"></a><span data-ttu-id="b9dba-139">Nawigowanie po interfejsie API sieci Web</span><span class="sxs-lookup"><span data-stu-id="b9dba-139">Navigate the web API</span></span>

### <a name="view-available-endpoints"></a><span data-ttu-id="b9dba-140">Wyświetl dostępne punkty końcowe</span><span class="sxs-lookup"><span data-stu-id="b9dba-140">View available endpoints</span></span>

<span data-ttu-id="b9dba-141">Aby wyświetlić listę różnych punktów końcowych (kontrolerów) na bieżącej ścieżce adresu internetowego interfejsu API, uruchom polecenie `ls` lub `dir`:</span><span class="sxs-lookup"><span data-stu-id="b9dba-141">To list the different endpoints (controllers) at the current path of the web API address, run the `ls` or `dir` command:</span></span>

```console
https://localhot:5001/~ ls
```

<span data-ttu-id="b9dba-142">Wyświetlany jest następujący format danych wyjściowych:</span><span class="sxs-lookup"><span data-stu-id="b9dba-142">The following output format is displayed:</span></span>

```console
.        []
Fruits   [get|post]
People   [get|post]

https://localhost:5001/~
```

<span data-ttu-id="b9dba-143">Powyższe dane wyjściowe wskazują, że dostępne są dwa kontrolery: `Fruits` i `People`.</span><span class="sxs-lookup"><span data-stu-id="b9dba-143">The preceding output indicates that there are two controllers available: `Fruits` and `People`.</span></span> <span data-ttu-id="b9dba-144">Oba kontrolery obsługują bez parametrów operacje GET i POST HTTP.</span><span class="sxs-lookup"><span data-stu-id="b9dba-144">Both controllers support parameterless HTTP GET and POST operations.</span></span>

<span data-ttu-id="b9dba-145">Przechodzenie do określonego kontrolera ujawnia więcej szczegółów.</span><span class="sxs-lookup"><span data-stu-id="b9dba-145">Navigating into a specific controller reveals more detail.</span></span> <span data-ttu-id="b9dba-146">Na przykład następujące dane wyjściowe polecenia pokazują kontroler `Fruits` obsługują również operacje GET, PUT i DELETE protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="b9dba-146">For example, the following command's output shows the `Fruits` controller also supports HTTP GET, PUT, and DELETE operations.</span></span> <span data-ttu-id="b9dba-147">Każda z tych operacji oczekuje `id` parametru w marszrucie:</span><span class="sxs-lookup"><span data-stu-id="b9dba-147">Each of these operations expects an `id` parameter in the route:</span></span>

```console
https://localhost:5001/fruits~ ls
.      [get|post]
..     []
{id}   [get|put|delete]

https://localhost:5001/fruits~
```

<span data-ttu-id="b9dba-148">Alternatywnie Uruchom polecenie `ui`, aby otworzyć stronę interfejsu użytkownika programu Swagger interfejsu API sieci Web w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="b9dba-148">Alternatively, run the `ui` command to open the web API's Swagger UI page in a browser.</span></span> <span data-ttu-id="b9dba-149">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="b9dba-149">For example:</span></span>

```console
https://localhost:5001/~ ui
```

### <a name="navigate-to-an-endpoint"></a><span data-ttu-id="b9dba-150">Przejdź do punktu końcowego</span><span class="sxs-lookup"><span data-stu-id="b9dba-150">Navigate to an endpoint</span></span>

<span data-ttu-id="b9dba-151">Aby przejść do innego punktu końcowego w internetowym interfejsie API, uruchom polecenie `cd`:</span><span class="sxs-lookup"><span data-stu-id="b9dba-151">To navigate to a different endpoint on the web API, run the `cd` command:</span></span>

```console
https://localhost:5001/~ cd people
```

<span data-ttu-id="b9dba-152">W ścieżce następującego polecenia `cd` nie jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="b9dba-152">The path following the `cd` command is case insensitive.</span></span> <span data-ttu-id="b9dba-153">Wyświetlany jest następujący format danych wyjściowych:</span><span class="sxs-lookup"><span data-stu-id="b9dba-153">The following output format is displayed:</span></span>

```console
/people    [get|post]

https://localhost:5001/people~
```

## <a name="customize-the-http-repl"></a><span data-ttu-id="b9dba-154">Dostosowywanie REPL HTTP</span><span class="sxs-lookup"><span data-stu-id="b9dba-154">Customize the HTTP REPL</span></span>

<span data-ttu-id="b9dba-155">Domyślne [kolory](#set-color-preferences) REPL http można dostosować.</span><span class="sxs-lookup"><span data-stu-id="b9dba-155">The HTTP REPL's default [colors](#set-color-preferences) can be customized.</span></span> <span data-ttu-id="b9dba-156">Ponadto można zdefiniować [domyślny edytor tekstu](#set-the-default-text-editor) .</span><span class="sxs-lookup"><span data-stu-id="b9dba-156">Additionally, a [default text editor](#set-the-default-text-editor) can be defined.</span></span> <span data-ttu-id="b9dba-157">Preferencje HTTP REPL są utrwalane w bieżącej sesji i są honorowane w przyszłych sesjach.</span><span class="sxs-lookup"><span data-stu-id="b9dba-157">The HTTP REPL preferences are persisted across the current session and are honored in future sessions.</span></span> <span data-ttu-id="b9dba-158">Po zmodyfikowaniu preferencje są przechowywane w następującym pliku:</span><span class="sxs-lookup"><span data-stu-id="b9dba-158">Once modified, the preferences are stored in the following file:</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="b9dba-159">System</span><span class="sxs-lookup"><span data-stu-id="b9dba-159">Linux</span></span>](#tab/linux)

<span data-ttu-id="b9dba-160">*% HOME%/.httpreplprefs*</span><span class="sxs-lookup"><span data-stu-id="b9dba-160">*%HOME%/.httpreplprefs*</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="b9dba-161">macOS</span><span class="sxs-lookup"><span data-stu-id="b9dba-161">macOS</span></span>](#tab/macos)

<span data-ttu-id="b9dba-162">*% HOME%/.httpreplprefs*</span><span class="sxs-lookup"><span data-stu-id="b9dba-162">*%HOME%/.httpreplprefs*</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="b9dba-163">Windows</span><span class="sxs-lookup"><span data-stu-id="b9dba-163">Windows</span></span>](#tab/windows)

<span data-ttu-id="b9dba-164">*% USERPROFILE%\\. httpreplprefs*</span><span class="sxs-lookup"><span data-stu-id="b9dba-164">*%USERPROFILE%\\.httpreplprefs*</span></span>

---

<span data-ttu-id="b9dba-165">Plik *. httpreplprefs* jest ładowany podczas uruchamiania i nie jest monitorowany pod kątem zmian w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="b9dba-165">The *.httpreplprefs* file is loaded on startup and not monitored for changes at runtime.</span></span> <span data-ttu-id="b9dba-166">Ręczne modyfikacje pliku zaczną obowiązywać dopiero po ponownym uruchomieniu narzędzia.</span><span class="sxs-lookup"><span data-stu-id="b9dba-166">Manual modifications to the file take effect only after restarting the tool.</span></span>

### <a name="view-the-settings"></a><span data-ttu-id="b9dba-167">Wyświetlanie ustawień</span><span class="sxs-lookup"><span data-stu-id="b9dba-167">View the settings</span></span>

<span data-ttu-id="b9dba-168">Aby wyświetlić dostępne ustawienia, uruchom polecenie `pref get`.</span><span class="sxs-lookup"><span data-stu-id="b9dba-168">To view the available settings, run the `pref get` command.</span></span> <span data-ttu-id="b9dba-169">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="b9dba-169">For example:</span></span>

```console
https://localhost:5001/~ pref get
```

<span data-ttu-id="b9dba-170">Poprzednie polecenie wyświetla dostępne pary klucz-wartość:</span><span class="sxs-lookup"><span data-stu-id="b9dba-170">The preceding command displays the available key-value pairs:</span></span>

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

### <a name="set-color-preferences"></a><span data-ttu-id="b9dba-171">Ustawianie preferencji koloru</span><span class="sxs-lookup"><span data-stu-id="b9dba-171">Set color preferences</span></span>

<span data-ttu-id="b9dba-172">Kolorowanie odpowiedzi jest obecnie obsługiwane tylko w przypadku formatu JSON.</span><span class="sxs-lookup"><span data-stu-id="b9dba-172">Response colorization is currently supported for JSON only.</span></span> <span data-ttu-id="b9dba-173">Aby dostosować domyślne kolorowanie narzędzi HTTP REPL, Znajdź klucz odpowiadający kolorowi do zmiany.</span><span class="sxs-lookup"><span data-stu-id="b9dba-173">To customize the default HTTP REPL tool coloring, locate the key corresponding to the color to be changed.</span></span> <span data-ttu-id="b9dba-174">Aby uzyskać instrukcje dotyczące znajdowania kluczy, zobacz sekcję [Wyświetlanie ustawień](#view-the-settings) .</span><span class="sxs-lookup"><span data-stu-id="b9dba-174">For instructions on how to find the keys, see the [View the settings](#view-the-settings) section.</span></span> <span data-ttu-id="b9dba-175">Na przykład zmień wartość klucza `colors.json` z `Green` na `White` w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="b9dba-175">For example, change the `colors.json` key value from `Green` to `White` as follows:</span></span>

```console
https://localhost:5001/people~ pref set colors.json White
```

<span data-ttu-id="b9dba-176">Można używać tylko [dozwolonych kolorów](https://github.com/dotnet/HttpRepl/blob/01d5c3c3373e98fe566ff5ef8a17c571de880293/src/Microsoft.Repl/ConsoleHandling/AllowedColors.cs) .</span><span class="sxs-lookup"><span data-stu-id="b9dba-176">Only the [allowed colors](https://github.com/dotnet/HttpRepl/blob/01d5c3c3373e98fe566ff5ef8a17c571de880293/src/Microsoft.Repl/ConsoleHandling/AllowedColors.cs) may be used.</span></span> <span data-ttu-id="b9dba-177">Kolejne żądania HTTP wyświetlają dane wyjściowe z nowym kolorem.</span><span class="sxs-lookup"><span data-stu-id="b9dba-177">Subsequent HTTP requests display output with the new coloring.</span></span>

<span data-ttu-id="b9dba-178">Jeśli określone klucze kolorów nie są ustawione, brane są więcej kluczy ogólnych.</span><span class="sxs-lookup"><span data-stu-id="b9dba-178">When specific color keys aren't set, more generic keys are considered.</span></span> <span data-ttu-id="b9dba-179">Aby zademonstrować to zachowanie rezerwowe, należy wziąć pod uwagę następujący przykład:</span><span class="sxs-lookup"><span data-stu-id="b9dba-179">To demonstrate this fallback behavior, consider the following example:</span></span>

* <span data-ttu-id="b9dba-180">Jeśli `colors.json.name` nie ma wartości, używana jest `colors.json.string`.</span><span class="sxs-lookup"><span data-stu-id="b9dba-180">If `colors.json.name` doesn't have a value, `colors.json.string` is used.</span></span>
* <span data-ttu-id="b9dba-181">Jeśli `colors.json.string` nie ma wartości, używana jest `colors.json.literal`.</span><span class="sxs-lookup"><span data-stu-id="b9dba-181">If `colors.json.string` doesn't have a value, `colors.json.literal` is used.</span></span>
* <span data-ttu-id="b9dba-182">Jeśli `colors.json.literal` nie ma wartości, używana jest `colors.json`.</span><span class="sxs-lookup"><span data-stu-id="b9dba-182">If `colors.json.literal` doesn't have a value, `colors.json` is used.</span></span> 
* <span data-ttu-id="b9dba-183">Jeśli `colors.json` nie ma wartości, używany jest domyślny kolor tekstu powłoki poleceń (`AllowedColors.None`).</span><span class="sxs-lookup"><span data-stu-id="b9dba-183">If `colors.json` doesn't have a value, the command shell's default text color (`AllowedColors.None`) is used.</span></span>

### <a name="set-indentation-size"></a><span data-ttu-id="b9dba-184">Ustaw rozmiar wcięcia</span><span class="sxs-lookup"><span data-stu-id="b9dba-184">Set indentation size</span></span>

<span data-ttu-id="b9dba-185">Dostosowanie rozmiaru wcięcia odpowiedzi jest obecnie obsługiwane tylko w przypadku formatu JSON.</span><span class="sxs-lookup"><span data-stu-id="b9dba-185">Response indentation size customization is currently supported for JSON only.</span></span> <span data-ttu-id="b9dba-186">Domyślny rozmiar to dwie spacje.</span><span class="sxs-lookup"><span data-stu-id="b9dba-186">The default size is two spaces.</span></span> <span data-ttu-id="b9dba-187">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="b9dba-187">For example:</span></span>

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

<span data-ttu-id="b9dba-188">Aby zmienić rozmiar domyślny, Ustaw klucz `formatting.json.indentSize`.</span><span class="sxs-lookup"><span data-stu-id="b9dba-188">To change the default size, set the `formatting.json.indentSize` key.</span></span> <span data-ttu-id="b9dba-189">Na przykład, aby zawsze używać czterech spacji:</span><span class="sxs-lookup"><span data-stu-id="b9dba-189">For example, to always use four spaces:</span></span>

```console
pref set formatting.json.indentSize 4
```

<span data-ttu-id="b9dba-190">Kolejne odpowiedzi przestrzegają ustawień czterech spacji:</span><span class="sxs-lookup"><span data-stu-id="b9dba-190">Subsequent responses honor the setting of four spaces:</span></span>

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

### <a name="set-the-default-text-editor"></a><span data-ttu-id="b9dba-191">Ustawianie domyślnego edytora tekstu</span><span class="sxs-lookup"><span data-stu-id="b9dba-191">Set the default text editor</span></span>

<span data-ttu-id="b9dba-192">Domyślnie REPL HTTP nie ma edytora tekstu skonfigurowanego do użycia.</span><span class="sxs-lookup"><span data-stu-id="b9dba-192">By default, the HTTP REPL has no text editor configured for use.</span></span> <span data-ttu-id="b9dba-193">Aby przetestować metody interfejsu API sieci Web wymagające treści żądania HTTP, należy ustawić domyślny edytor tekstu.</span><span class="sxs-lookup"><span data-stu-id="b9dba-193">To test web API methods requiring an HTTP request body, a default text editor must be set.</span></span> <span data-ttu-id="b9dba-194">Narzędzie HTTP REPL uruchamia skonfigurowany Edytor tekstów wyłącznie na potrzeby redagowania treści żądania.</span><span class="sxs-lookup"><span data-stu-id="b9dba-194">The HTTP REPL tool launches the configured text editor for the sole purpose of composing the request body.</span></span> <span data-ttu-id="b9dba-195">Uruchom następujące polecenie, aby ustawić preferowany Edytor tekstu jako domyślny:</span><span class="sxs-lookup"><span data-stu-id="b9dba-195">Run the following command to set your preferred text editor as the default:</span></span>

```console
pref set editor.command.default "<EXECUTABLE>"
```

<span data-ttu-id="b9dba-196">W poprzednim poleceniu `<EXECUTABLE>` jest pełną ścieżką do pliku wykonywalnego edytora tekstu.</span><span class="sxs-lookup"><span data-stu-id="b9dba-196">In the preceding command, `<EXECUTABLE>` is the full path to the text editor's executable file.</span></span> <span data-ttu-id="b9dba-197">Na przykład uruchom następujące polecenie, aby ustawić Visual Studio Code jako domyślny edytor tekstu:</span><span class="sxs-lookup"><span data-stu-id="b9dba-197">For example, run the following command to set Visual Studio Code as the default text editor:</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="b9dba-198">System</span><span class="sxs-lookup"><span data-stu-id="b9dba-198">Linux</span></span>](#tab/linux)

```console
pref set editor.command.default "/usr/bin/code"
```

# <a name="macostabmacos"></a>[<span data-ttu-id="b9dba-199">macOS</span><span class="sxs-lookup"><span data-stu-id="b9dba-199">macOS</span></span>](#tab/macos)

```console
pref set editor.command.default "/Applications/Visual Studio Code.app/Contents/Resources/app/bin/code"
```

# <a name="windowstabwindows"></a>[<span data-ttu-id="b9dba-200">Windows</span><span class="sxs-lookup"><span data-stu-id="b9dba-200">Windows</span></span>](#tab/windows)

```console
pref set editor.command.default "C:\Program Files\Microsoft VS Code\Code.exe"
```

---

<span data-ttu-id="b9dba-201">Aby uruchomić domyślny edytor tekstu z określonymi argumentami interfejsu wiersza polecenia, należy ustawić klucz `editor.command.default.arguments`.</span><span class="sxs-lookup"><span data-stu-id="b9dba-201">To launch the default text editor with specific CLI arguments, set the `editor.command.default.arguments` key.</span></span> <span data-ttu-id="b9dba-202">Załóżmy na przykład, że Visual Studio Code jest domyślnym edytorem tekstu i zawsze chcesz, aby REPL HTTP mógł otworzyć Visual Studio Code w nowej sesji z wyłączonymi rozszerzeniami.</span><span class="sxs-lookup"><span data-stu-id="b9dba-202">For example, assume Visual Studio Code is the default text editor and that you always want the HTTP REPL to open Visual Studio Code in a new session with extensions disabled.</span></span> <span data-ttu-id="b9dba-203">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="b9dba-203">Run the following command:</span></span>

```console
pref set editor.command.default.arguments "--disable-extensions --new-window"
```

### <a name="set-the-swagger-search-paths"></a><span data-ttu-id="b9dba-204">Ustawianie ścieżek wyszukiwania struktury Swagger</span><span class="sxs-lookup"><span data-stu-id="b9dba-204">Set the Swagger search paths</span></span>

<span data-ttu-id="b9dba-205">Domyślnie REPL HTTP ma zestaw ścieżek względnych, których używa do znajdowania dokumentu struktury Swagger podczas wykonywania polecenia `connect` bez opcji `--swagger`.</span><span class="sxs-lookup"><span data-stu-id="b9dba-205">By default, the HTTP REPL has a set of relative paths that it uses to find the Swagger document when executing the `connect` command without the `--swagger` option.</span></span> <span data-ttu-id="b9dba-206">Ścieżki względne są łączone ze ścieżkami głównymi i podstawowymi określonymi w poleceniu `connect`.</span><span class="sxs-lookup"><span data-stu-id="b9dba-206">These relative paths are combined with the root and base paths specified in the `connect` command.</span></span> <span data-ttu-id="b9dba-207">Domyślne ścieżki względne to:</span><span class="sxs-lookup"><span data-stu-id="b9dba-207">The default relative paths are:</span></span>

- <span data-ttu-id="b9dba-208">*plik Swagger. JSON*</span><span class="sxs-lookup"><span data-stu-id="b9dba-208">*swagger.json*</span></span>
- <span data-ttu-id="b9dba-209">*Swagger/V1/Swagger. JSON*</span><span class="sxs-lookup"><span data-stu-id="b9dba-209">*swagger/v1/swagger.json*</span></span>
- <span data-ttu-id="b9dba-210">*/swagger.json*</span><span class="sxs-lookup"><span data-stu-id="b9dba-210">*/swagger.json*</span></span>
- <span data-ttu-id="b9dba-211">*/swagger/v1/swagger.json*</span><span class="sxs-lookup"><span data-stu-id="b9dba-211">*/swagger/v1/swagger.json*</span></span>

<span data-ttu-id="b9dba-212">Aby użyć innego zestawu ścieżek wyszukiwania w środowisku, ustaw preferencję `swagger.searchPaths`.</span><span class="sxs-lookup"><span data-stu-id="b9dba-212">To use a different set of search paths in your environment, set the `swagger.searchPaths` preference.</span></span> <span data-ttu-id="b9dba-213">Wartość musi być rozdzielaną potokami listą ścieżek względnych.</span><span class="sxs-lookup"><span data-stu-id="b9dba-213">The value must be a pipe-delimited list of relative paths.</span></span> <span data-ttu-id="b9dba-214">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="b9dba-214">For example:</span></span>

```console
pref set swagger.searchPaths "swagger/v2/swagger.json|swagger/v3/swagger.json"
```

## <a name="test-http-get-requests"></a><span data-ttu-id="b9dba-215">Testuj żądania HTTP GET</span><span class="sxs-lookup"><span data-stu-id="b9dba-215">Test HTTP GET requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="b9dba-216">Streszczenie</span><span class="sxs-lookup"><span data-stu-id="b9dba-216">Synopsis</span></span>

```console
get <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="b9dba-217">Argumenty</span><span class="sxs-lookup"><span data-stu-id="b9dba-217">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="b9dba-218">Parametr trasy, jeśli istnieje, oczekiwany przez skojarzoną metodę akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="b9dba-218">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="b9dba-219">Opcje</span><span class="sxs-lookup"><span data-stu-id="b9dba-219">Options</span></span>

<span data-ttu-id="b9dba-220">Następujące opcje są dostępne dla polecenia `get`:</span><span class="sxs-lookup"><span data-stu-id="b9dba-220">The following options are available for the `get` command:</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

### <a name="example"></a><span data-ttu-id="b9dba-221">Przykład</span><span class="sxs-lookup"><span data-stu-id="b9dba-221">Example</span></span>

<span data-ttu-id="b9dba-222">Aby wydać żądanie HTTP GET:</span><span class="sxs-lookup"><span data-stu-id="b9dba-222">To issue an HTTP GET request:</span></span>

1. <span data-ttu-id="b9dba-223">Uruchom `get` polecenie w punkcie końcowym, który go obsługuje:</span><span class="sxs-lookup"><span data-stu-id="b9dba-223">Run the `get` command on an endpoint that supports it:</span></span>

    ```console
    https://localhost:5001/people~ get
    ```

    <span data-ttu-id="b9dba-224">Poprzednie polecenie wyświetla następujący format danych wyjściowych:</span><span class="sxs-lookup"><span data-stu-id="b9dba-224">The preceding command displays the following output format:</span></span>

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

1. <span data-ttu-id="b9dba-225">Pobierz konkretny rekord, przekazując parametr do polecenia `get`:</span><span class="sxs-lookup"><span data-stu-id="b9dba-225">Retrieve a specific record by passing a parameter to the `get` command:</span></span>

    ```console
    https://localhost:5001/people~ get 2
    ```

    <span data-ttu-id="b9dba-226">Poprzednie polecenie wyświetla następujący format danych wyjściowych:</span><span class="sxs-lookup"><span data-stu-id="b9dba-226">The preceding command displays the following output format:</span></span>

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

## <a name="test-http-post-requests"></a><span data-ttu-id="b9dba-227">Testowanie żądań POST protokołu HTTP</span><span class="sxs-lookup"><span data-stu-id="b9dba-227">Test HTTP POST requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="b9dba-228">Streszczenie</span><span class="sxs-lookup"><span data-stu-id="b9dba-228">Synopsis</span></span>

```console
post <PARAMETER> [-c|--content] [-f|--file] [-h|--header] [--no-body] [-F|--no-formatting] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="b9dba-229">Argumenty</span><span class="sxs-lookup"><span data-stu-id="b9dba-229">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="b9dba-230">Parametr trasy, jeśli istnieje, oczekiwany przez skojarzoną metodę akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="b9dba-230">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="b9dba-231">Opcje</span><span class="sxs-lookup"><span data-stu-id="b9dba-231">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

[!INCLUDE [HTTP request body CLI options](~/includes/http-repl/requires-body-options.md)]

### <a name="example"></a><span data-ttu-id="b9dba-232">Przykład</span><span class="sxs-lookup"><span data-stu-id="b9dba-232">Example</span></span>

<span data-ttu-id="b9dba-233">Aby wydać żądanie HTTP POST:</span><span class="sxs-lookup"><span data-stu-id="b9dba-233">To issue an HTTP POST request:</span></span>

1. <span data-ttu-id="b9dba-234">Uruchom `post` polecenie w punkcie końcowym, który go obsługuje:</span><span class="sxs-lookup"><span data-stu-id="b9dba-234">Run the `post` command on an endpoint that supports it:</span></span>

    ```console
    https://localhost:5001/people~ post -h Content-Type=application/json
    ```

    <span data-ttu-id="b9dba-235">W poprzednim poleceniu w nagłówku żądania HTTP `Content-Type` jest ustawiona wartość wskazująca typ nośnika treści żądania JSON.</span><span class="sxs-lookup"><span data-stu-id="b9dba-235">In the preceding command, the `Content-Type` HTTP request header is set to indicate a request body media type of JSON.</span></span> <span data-ttu-id="b9dba-236">Domyślny edytor tekstu otwiera plik *. tmp* z szablonem JSON reprezentującym treść żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="b9dba-236">The default text editor opens a *.tmp* file with a JSON template representing the HTTP request body.</span></span> <span data-ttu-id="b9dba-237">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="b9dba-237">For example:</span></span>

    ```json
    {
      "id": 0,
      "name": ""
    }
    ```

    > [!TIP]
    > <span data-ttu-id="b9dba-238">Aby ustawić domyślny edytor tekstu, zobacz sekcję [Ustawianie domyślnego edytora tekstu](#set-the-default-text-editor) .</span><span class="sxs-lookup"><span data-stu-id="b9dba-238">To set the default text editor, see the [Set the default text editor](#set-the-default-text-editor) section.</span></span>

1. <span data-ttu-id="b9dba-239">Zmodyfikuj szablon JSON, aby spełniał wymagania dotyczące weryfikacji modelu:</span><span class="sxs-lookup"><span data-stu-id="b9dba-239">Modify the JSON template to satisfy model validation requirements:</span></span>

    ```json
    {
      "id": 0,
      "name": "Scott Addie"
    }
    ```

1. <span data-ttu-id="b9dba-240">Zapisz plik *. tmp* i Zamknij Edytor tekstu.</span><span class="sxs-lookup"><span data-stu-id="b9dba-240">Save the *.tmp* file, and close the text editor.</span></span> <span data-ttu-id="b9dba-241">Następujące dane wyjściowe są wyświetlane w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="b9dba-241">The following output appears in the command shell:</span></span>

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

## <a name="test-http-put-requests"></a><span data-ttu-id="b9dba-242">Testowanie żądań HTTP PUT</span><span class="sxs-lookup"><span data-stu-id="b9dba-242">Test HTTP PUT requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="b9dba-243">Streszczenie</span><span class="sxs-lookup"><span data-stu-id="b9dba-243">Synopsis</span></span>

```console
put <PARAMETER> [-c|--content] [-f|--file] [-h|--header] [--no-body] [-F|--no-formatting] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="b9dba-244">Argumenty</span><span class="sxs-lookup"><span data-stu-id="b9dba-244">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="b9dba-245">Parametr trasy, jeśli istnieje, oczekiwany przez skojarzoną metodę akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="b9dba-245">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="b9dba-246">Opcje</span><span class="sxs-lookup"><span data-stu-id="b9dba-246">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

[!INCLUDE [HTTP request body CLI options](~/includes/http-repl/requires-body-options.md)]

### <a name="example"></a><span data-ttu-id="b9dba-247">Przykład</span><span class="sxs-lookup"><span data-stu-id="b9dba-247">Example</span></span>

<span data-ttu-id="b9dba-248">Aby wydać żądanie HTTP PUT:</span><span class="sxs-lookup"><span data-stu-id="b9dba-248">To issue an HTTP PUT request:</span></span>

1. <span data-ttu-id="b9dba-249">*Opcjonalnie*: Uruchom `get` polecenie, aby wyświetlić dane przed zmodyfikowaniem:</span><span class="sxs-lookup"><span data-stu-id="b9dba-249">*Optional*: Run the `get` command to view the data before modifying it:</span></span>

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

    <span data-ttu-id="b9dba-250">W poprzednim poleceniu w nagłówku żądania HTTP `Content-Type` jest ustawiona wartość wskazująca typ nośnika treści żądania JSON.</span><span class="sxs-lookup"><span data-stu-id="b9dba-250">In the preceding command, the `Content-Type` HTTP request header is set to indicate a request body media type of JSON.</span></span> <span data-ttu-id="b9dba-251">Domyślny edytor tekstu otwiera plik *. tmp* z szablonem JSON reprezentującym treść żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="b9dba-251">The default text editor opens a *.tmp* file with a JSON template representing the HTTP request body.</span></span> <span data-ttu-id="b9dba-252">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="b9dba-252">For example:</span></span>

    ```json
    {
      "id": 0,
      "name": ""
    }
    ```

    > [!TIP]
    > <span data-ttu-id="b9dba-253">Aby ustawić domyślny edytor tekstu, zobacz sekcję [Ustawianie domyślnego edytora tekstu](#set-the-default-text-editor) .</span><span class="sxs-lookup"><span data-stu-id="b9dba-253">To set the default text editor, see the [Set the default text editor](#set-the-default-text-editor) section.</span></span>

1. <span data-ttu-id="b9dba-254">Zmodyfikuj szablon JSON, aby spełniał wymagania dotyczące weryfikacji modelu:</span><span class="sxs-lookup"><span data-stu-id="b9dba-254">Modify the JSON template to satisfy model validation requirements:</span></span>

    ```json
    {
      "id": 2,
      "name": "Cherry"
    }
    ```

1. <span data-ttu-id="b9dba-255">Zapisz plik *. tmp* i Zamknij Edytor tekstu.</span><span class="sxs-lookup"><span data-stu-id="b9dba-255">Save the *.tmp* file, and close the text editor.</span></span> <span data-ttu-id="b9dba-256">Następujące dane wyjściowe są wyświetlane w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="b9dba-256">The following output appears in the command shell:</span></span>

    ```console
    [main 2019-06-28T17:27:01.805Z] update#setState idle
    HTTP/1.1 204 No Content
    Date: Fri, 28 Jun 2019 17:28:21 GMT
    Server: Kestrel
    ```

1. <span data-ttu-id="b9dba-257">*Opcjonalnie*: wydaj polecenie `get`, aby zobaczyć modyfikacje.</span><span class="sxs-lookup"><span data-stu-id="b9dba-257">*Optional*: Issue a `get` command to see the modifications.</span></span> <span data-ttu-id="b9dba-258">Na przykład po wpisaniu "wiśni" w edytorze tekstów `get` zwraca następujące elementy:</span><span class="sxs-lookup"><span data-stu-id="b9dba-258">For example, if you typed "Cherry" in the text editor, a `get` returns the following:</span></span>

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

## <a name="test-http-delete-requests"></a><span data-ttu-id="b9dba-259">Testowanie żądań HTTP DELETE</span><span class="sxs-lookup"><span data-stu-id="b9dba-259">Test HTTP DELETE requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="b9dba-260">Streszczenie</span><span class="sxs-lookup"><span data-stu-id="b9dba-260">Synopsis</span></span>

```console
delete <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="b9dba-261">Argumenty</span><span class="sxs-lookup"><span data-stu-id="b9dba-261">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="b9dba-262">Parametr trasy, jeśli istnieje, oczekiwany przez skojarzoną metodę akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="b9dba-262">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="b9dba-263">Opcje</span><span class="sxs-lookup"><span data-stu-id="b9dba-263">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

### <a name="example"></a><span data-ttu-id="b9dba-264">Przykład</span><span class="sxs-lookup"><span data-stu-id="b9dba-264">Example</span></span>

<span data-ttu-id="b9dba-265">Aby wydać żądanie HTTP DELETE:</span><span class="sxs-lookup"><span data-stu-id="b9dba-265">To issue an HTTP DELETE request:</span></span>

1. <span data-ttu-id="b9dba-266">*Opcjonalnie*: Uruchom `get` polecenie, aby wyświetlić dane przed zmodyfikowaniem:</span><span class="sxs-lookup"><span data-stu-id="b9dba-266">*Optional*: Run the `get` command to view the data before modifying it:</span></span>

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

    <span data-ttu-id="b9dba-267">Poprzednie polecenie wyświetla następujący format danych wyjściowych:</span><span class="sxs-lookup"><span data-stu-id="b9dba-267">The preceding command displays the following output format:</span></span>

    ```console
    HTTP/1.1 204 No Content
    Date: Fri, 28 Jun 2019 17:36:42 GMT
    Server: Kestrel
    ```

1. <span data-ttu-id="b9dba-268">*Opcjonalnie*: wydaj polecenie `get`, aby zobaczyć modyfikacje.</span><span class="sxs-lookup"><span data-stu-id="b9dba-268">*Optional*: Issue a `get` command to see the modifications.</span></span> <span data-ttu-id="b9dba-269">W tym przykładzie `get` zwraca następujące elementy:</span><span class="sxs-lookup"><span data-stu-id="b9dba-269">In this example, a `get` returns the following:</span></span>

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

## <a name="test-http-patch-requests"></a><span data-ttu-id="b9dba-270">Testuj żądania poprawek HTTP</span><span class="sxs-lookup"><span data-stu-id="b9dba-270">Test HTTP PATCH requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="b9dba-271">Streszczenie</span><span class="sxs-lookup"><span data-stu-id="b9dba-271">Synopsis</span></span>

```console
patch <PARAMETER> [-c|--content] [-f|--file] [-h|--header] [--no-body] [-F|--no-formatting] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="b9dba-272">Argumenty</span><span class="sxs-lookup"><span data-stu-id="b9dba-272">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="b9dba-273">Parametr trasy, jeśli istnieje, oczekiwany przez skojarzoną metodę akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="b9dba-273">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="b9dba-274">Opcje</span><span class="sxs-lookup"><span data-stu-id="b9dba-274">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

[!INCLUDE [HTTP request body CLI options](~/includes/http-repl/requires-body-options.md)]

## <a name="test-http-head-requests"></a><span data-ttu-id="b9dba-275">Testowanie żądań głównych HTTP</span><span class="sxs-lookup"><span data-stu-id="b9dba-275">Test HTTP HEAD requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="b9dba-276">Streszczenie</span><span class="sxs-lookup"><span data-stu-id="b9dba-276">Synopsis</span></span>

```console
head <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="b9dba-277">Argumenty</span><span class="sxs-lookup"><span data-stu-id="b9dba-277">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="b9dba-278">Parametr trasy, jeśli istnieje, oczekiwany przez skojarzoną metodę akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="b9dba-278">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="b9dba-279">Opcje</span><span class="sxs-lookup"><span data-stu-id="b9dba-279">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

## <a name="test-http-options-requests"></a><span data-ttu-id="b9dba-280">Testowe żądania opcji HTTP</span><span class="sxs-lookup"><span data-stu-id="b9dba-280">Test HTTP OPTIONS requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="b9dba-281">Streszczenie</span><span class="sxs-lookup"><span data-stu-id="b9dba-281">Synopsis</span></span>

```console
options <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="b9dba-282">Argumenty</span><span class="sxs-lookup"><span data-stu-id="b9dba-282">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="b9dba-283">Parametr trasy, jeśli istnieje, oczekiwany przez skojarzoną metodę akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="b9dba-283">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="b9dba-284">Opcje</span><span class="sxs-lookup"><span data-stu-id="b9dba-284">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

## <a name="set-http-request-headers"></a><span data-ttu-id="b9dba-285">Ustawianie nagłówków żądań HTTP</span><span class="sxs-lookup"><span data-stu-id="b9dba-285">Set HTTP request headers</span></span>

<span data-ttu-id="b9dba-286">Aby ustawić nagłówek żądania HTTP, należy użyć jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="b9dba-286">To set an HTTP request header, use one of the following approaches:</span></span>

* <span data-ttu-id="b9dba-287">Ustaw wartość inline z żądaniem HTTP.</span><span class="sxs-lookup"><span data-stu-id="b9dba-287">Set inline with the HTTP request.</span></span> <span data-ttu-id="b9dba-288">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="b9dba-288">For example:</span></span>

    ```console
    https://localhost:5001/people~ post -h Content-Type=application/json
    ```
    
    <span data-ttu-id="b9dba-289">W przypadku wcześniejszego podejścia każdy unikatowy nagłówek żądania HTTP wymaga własnej opcji `-h`.</span><span class="sxs-lookup"><span data-stu-id="b9dba-289">With the preceding approach, each distinct HTTP request header requires its own `-h` option.</span></span>

* <span data-ttu-id="b9dba-290">Ustaw przed wysłaniem żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="b9dba-290">Set before sending the HTTP request.</span></span> <span data-ttu-id="b9dba-291">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="b9dba-291">For example:</span></span>

    ```console
    https://localhost:5001/people~ set header Content-Type application/json
    ```
    
    <span data-ttu-id="b9dba-292">Podczas ustawiania nagłówka przed wysłaniem żądania nagłówek pozostaje ustawiony na czas trwania sesji powłoki poleceń.</span><span class="sxs-lookup"><span data-stu-id="b9dba-292">When setting the header before sending a request, the header remains set for the duration of the command shell session.</span></span> <span data-ttu-id="b9dba-293">Aby wyczyścić nagłówek, podaj wartość pustą.</span><span class="sxs-lookup"><span data-stu-id="b9dba-293">To clear the header, provide an empty value.</span></span> <span data-ttu-id="b9dba-294">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="b9dba-294">For example:</span></span>
    
    ```console
    https://localhost:5001/people~ set header Content-Type
    ```

## <a name="test-secured-endpoints"></a><span data-ttu-id="b9dba-295">Testuj zabezpieczone punkty końcowe</span><span class="sxs-lookup"><span data-stu-id="b9dba-295">Test secured endpoints</span></span>

<span data-ttu-id="b9dba-296">REPL HTTP obsługuje testowanie zabezpieczonych punktów końcowych za pomocą nagłówków żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="b9dba-296">The HTTP REPL supports the testing of secured endpoints through the use of HTTP request headers.</span></span> <span data-ttu-id="b9dba-297">Przykłady obsługiwanych schematów uwierzytelniania i autoryzacji obejmują uwierzytelnianie podstawowe, tokeny okaziciela JWT i uwierzytelnianie szyfrowane.</span><span class="sxs-lookup"><span data-stu-id="b9dba-297">Examples of supported authentication and authorization schemes include basic authentication, JWT bearer tokens, and digest authentication.</span></span> <span data-ttu-id="b9dba-298">Na przykład można wysłać token okaziciela do punktu końcowego za pomocą następującego polecenia:</span><span class="sxs-lookup"><span data-stu-id="b9dba-298">For example, you can send a bearer token to an endpoint with the following command:</span></span>

```console
set header Authorization "bearer <TOKEN VALUE>"
```

<span data-ttu-id="b9dba-299">Aby uzyskać dostęp do punktu końcowego hostowanego na platformie Azure lub użyć [interfejsu API REST platformy Azure](/rest/api/azure/), potrzebujesz tokenu okaziciela.</span><span class="sxs-lookup"><span data-stu-id="b9dba-299">To access an Azure-hosted endpoint or to use the [Azure REST API](/rest/api/azure/), you need a bearer token.</span></span> <span data-ttu-id="b9dba-300">Wykonaj następujące kroki, aby uzyskać token okaziciela dla subskrypcji platformy Azure za pośrednictwem [interfejsu wiersza polecenia platformy Azure](/cli/azure/).</span><span class="sxs-lookup"><span data-stu-id="b9dba-300">Use the following steps to obtain a bearer token for your Azure subscription via the [Azure CLI](/cli/azure/).</span></span> <span data-ttu-id="b9dba-301">REPL HTTP ustawia token okaziciela w nagłówku żądania HTTP i pobiera listę Web Apps Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="b9dba-301">The HTTP REPL sets the bearer token in an HTTP request header and retrieves a list of Azure App Service Web Apps.</span></span>

1. <span data-ttu-id="b9dba-302">Zaloguj się do platformy Azure:</span><span class="sxs-lookup"><span data-stu-id="b9dba-302">Log in to Azure:</span></span>

    ```azcli
    az login
    ```

1. <span data-ttu-id="b9dba-303">Pobierz swój identyfikator subskrypcji za pomocą następującego polecenia:</span><span class="sxs-lookup"><span data-stu-id="b9dba-303">Get your subscription ID with the following command:</span></span>

    ```azcli
    az account show --query id
    ```

1. <span data-ttu-id="b9dba-304">Skopiuj identyfikator subskrypcji i uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="b9dba-304">Copy your subscription ID and run the following command:</span></span>

    ```azcli
    az account set --subscription "<SUBSCRIPTION ID>"
    ```

1. <span data-ttu-id="b9dba-305">Pobierz token okaziciela przy użyciu następującego polecenia:</span><span class="sxs-lookup"><span data-stu-id="b9dba-305">Get your bearer token with the following command:</span></span>

    ```azcli
    az account get-access-token --query accessToken
    ```

1. <span data-ttu-id="b9dba-306">Połącz się z interfejsem API REST platformy Azure za pośrednictwem REPL HTTP:</span><span class="sxs-lookup"><span data-stu-id="b9dba-306">Connect to the Azure REST API via the HTTP REPL:</span></span>

    ```console
    httprepl https://management.azure.com
    ```

1. <span data-ttu-id="b9dba-307">Ustaw `Authorization` nagłówek żądania HTTP:</span><span class="sxs-lookup"><span data-stu-id="b9dba-307">Set the `Authorization` HTTP request header:</span></span>

    ```console
    https://management.azure.com/> set header Authorization "bearer <ACCESS TOKEN>"
    ```

1. <span data-ttu-id="b9dba-308">Przejdź do subskrypcji:</span><span class="sxs-lookup"><span data-stu-id="b9dba-308">Navigate to the subscription:</span></span>

    ```console
    https://management.azure.com/> cd subscriptions/<SUBSCRIPTION ID>
    ```

1. <span data-ttu-id="b9dba-309">Pobierz listę Azure App Service subskrypcji Web Apps:</span><span class="sxs-lookup"><span data-stu-id="b9dba-309">Get a list of your subscription's Azure App Service Web Apps:</span></span>

    ```console
    https://management.azure.com/subscriptions/{SUBSCRIPTION ID}> get providers/Microsoft.Web/sites?api-version=2016-08-01
    ```

    <span data-ttu-id="b9dba-310">Zostanie wyświetlona następująca odpowiedź:</span><span class="sxs-lookup"><span data-stu-id="b9dba-310">The following response is displayed:</span></span>

    ```console
    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 35948
    Content-Type: application/json; charset=utf-8
    Date: Thu, 19 Sep 2019 23:04:03 GMT
    Expires: -1
    Pragma: no-cache
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    X-Content-Type-Options: nosniff
    x-ms-correlation-request-id: <em>xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx</em>
    x-ms-original-request-ids: <em>xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx;xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx</em>
    x-ms-ratelimit-remaining-subscription-reads: 11999
    x-ms-request-id: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    x-ms-routing-request-id: WESTUS:xxxxxxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxx
    {
      "value": [
        <AZURE RESOURCES LIST>
      ]
    }
    ```

## <a name="toggle-http-request-display"></a><span data-ttu-id="b9dba-311">Przełącz wyświetlanie żądań HTTP</span><span class="sxs-lookup"><span data-stu-id="b9dba-311">Toggle HTTP request display</span></span>

<span data-ttu-id="b9dba-312">Domyślnie wyświetlanie wysyłanego żądania HTTP jest pomijane.</span><span class="sxs-lookup"><span data-stu-id="b9dba-312">By default, display of the HTTP request being sent is suppressed.</span></span> <span data-ttu-id="b9dba-313">Istnieje możliwość zmiany odpowiedniego ustawienia dla czasu trwania sesji powłoki poleceń.</span><span class="sxs-lookup"><span data-stu-id="b9dba-313">It's possible to change the corresponding setting for the duration of the command shell session.</span></span>

### <a name="enable-request-display"></a><span data-ttu-id="b9dba-314">Włącz wyświetlanie żądań</span><span class="sxs-lookup"><span data-stu-id="b9dba-314">Enable request display</span></span>

<span data-ttu-id="b9dba-315">Wyświetl wysyłane żądanie HTTP, uruchamiając polecenie `echo on`.</span><span class="sxs-lookup"><span data-stu-id="b9dba-315">View the HTTP request being sent by running the `echo on` command.</span></span> <span data-ttu-id="b9dba-316">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="b9dba-316">For example:</span></span>

```console
https://localhost:5001/people~ echo on
Request echoing is on
```

<span data-ttu-id="b9dba-317">Kolejne żądania HTTP w bieżącej sesji wyświetlają nagłówki żądań.</span><span class="sxs-lookup"><span data-stu-id="b9dba-317">Subsequent HTTP requests in the current session display the request headers.</span></span> <span data-ttu-id="b9dba-318">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="b9dba-318">For example:</span></span>

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

### <a name="disable-request-display"></a><span data-ttu-id="b9dba-319">Wyłącz wyświetlanie żądań</span><span class="sxs-lookup"><span data-stu-id="b9dba-319">Disable request display</span></span>

<span data-ttu-id="b9dba-320">Pomijaj wyświetlanie wysyłanego żądania HTTP, uruchamiając polecenie `echo off`.</span><span class="sxs-lookup"><span data-stu-id="b9dba-320">Suppress display of the HTTP request being sent by running the `echo off` command.</span></span> <span data-ttu-id="b9dba-321">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="b9dba-321">For example:</span></span>

```console
https://localhost:5001/people~ echo off
Request echoing is off
```

## <a name="run-a-script"></a><span data-ttu-id="b9dba-322">Uruchom skrypt</span><span class="sxs-lookup"><span data-stu-id="b9dba-322">Run a script</span></span>

<span data-ttu-id="b9dba-323">Jeśli często wykonujesz ten sam zestaw poleceń HTTP REPL, Rozważ przechowywanie ich w pliku tekstowym.</span><span class="sxs-lookup"><span data-stu-id="b9dba-323">If you frequently execute the same set of HTTP REPL commands, consider storing them in a text file.</span></span> <span data-ttu-id="b9dba-324">Polecenia w pliku mają taki sam formularz jak te wykonywane ręcznie w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="b9dba-324">Commands in the file take the same form as those executed manually on the command line.</span></span> <span data-ttu-id="b9dba-325">Polecenia mogą być wykonywane w sposób wsadowy za pomocą polecenia `run`.</span><span class="sxs-lookup"><span data-stu-id="b9dba-325">The commands can be executed in a batched fashion using the `run` command.</span></span> <span data-ttu-id="b9dba-326">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="b9dba-326">For example:</span></span>

1. <span data-ttu-id="b9dba-327">Utwórz plik tekstowy zawierający zestaw poleceń rozdzielanych znakami nowego wiersza.</span><span class="sxs-lookup"><span data-stu-id="b9dba-327">Create a text file containing a set of newline-delimited commands.</span></span> <span data-ttu-id="b9dba-328">Aby to zilustrować, należy rozważyć plik *People-Script. txt* zawierający następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="b9dba-328">To illustrate, consider a *people-script.txt* file containing the following commands:</span></span>

    ```text
    set base https://localhost:5001
    ls
    cd People
    ls
    get 1
    ```

1. <span data-ttu-id="b9dba-329">Wykonaj `run` polecenie, przekazując w ścieżce pliku tekstowego.</span><span class="sxs-lookup"><span data-stu-id="b9dba-329">Execute the `run` command, passing in the text file's path.</span></span> <span data-ttu-id="b9dba-330">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="b9dba-330">For example:</span></span>

    ```console
    https://localhost:5001/~ run C:\http-repl-scripts\people-script.txt
    ```

    <span data-ttu-id="b9dba-331">Wyświetlane są następujące dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="b9dba-331">The following output appears:</span></span>

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

## <a name="clear-the-output"></a><span data-ttu-id="b9dba-332">Wyczyść dane wyjściowe</span><span class="sxs-lookup"><span data-stu-id="b9dba-332">Clear the output</span></span>

<span data-ttu-id="b9dba-333">Aby usunąć wszystkie dane wyjściowe zapisywane do powłoki poleceń za pomocą narzędzia HTTP REPL, uruchom polecenie `clear` lub `cls`.</span><span class="sxs-lookup"><span data-stu-id="b9dba-333">To remove all output written to the command shell by the HTTP REPL tool, run the `clear` or `cls` command.</span></span> <span data-ttu-id="b9dba-334">Do zilustrowania, Załóżmy, że powłoka poleceń zawiera następujące dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="b9dba-334">To illustrate, imagine the command shell contains the following output:</span></span>

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

<span data-ttu-id="b9dba-335">Uruchom następujące polecenie, aby wyczyścić dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="b9dba-335">Run the following command to clear the output:</span></span>

```console
https://localhost:5001/~ clear
```

<span data-ttu-id="b9dba-336">Po uruchomieniu poprzedniego polecenia powłoka poleceń zawiera tylko następujące dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="b9dba-336">After running the preceding command, the command shell contains only the following output:</span></span>

```console
https://localhost:5001/~
```

## <a name="additional-resources"></a><span data-ttu-id="b9dba-337">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="b9dba-337">Additional resources</span></span>

* [<span data-ttu-id="b9dba-338">Żądania interfejsu API REST</span><span class="sxs-lookup"><span data-stu-id="b9dba-338">REST API requests</span></span>](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#74-supported-methods)
* [<span data-ttu-id="b9dba-339">Repozytorium usługi GitHub HTTP REPL</span><span class="sxs-lookup"><span data-stu-id="b9dba-339">HTTP REPL GitHub repository</span></span>](https://github.com/dotnet/HttpRepl)
