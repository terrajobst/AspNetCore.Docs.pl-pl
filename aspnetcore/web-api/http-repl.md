---
title: Testowanie interfejsów API sieci Web przy użyciu protokołu HTTP REPL
author: scottaddie
description: Dowiedz się, jak przeglądać i testować ASP.NET Core internetowy interfejs API przy użyciu globalnego narzędzia REPL .NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 07/23/2019
uid: web-api/http-repl
ms.openlocfilehash: 29a29b98b31ac8e48545789928a8e83573ca9af5
ms.sourcegitcommit: b1662379b6aeca66ef6c696bd2300c81aa74a514
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/25/2019
ms.locfileid: "68485810"
---
# <a name="test-web-apis-with-the-http-repl"></a><span data-ttu-id="f242a-103">Testowanie interfejsów API sieci Web przy użyciu protokołu HTTP REPL</span><span class="sxs-lookup"><span data-stu-id="f242a-103">Test web APIs with the HTTP REPL</span></span>

<span data-ttu-id="f242a-104">Przez [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="f242a-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="f242a-105">Pętla HTTP Read-eval-Print (REPL) to:</span><span class="sxs-lookup"><span data-stu-id="f242a-105">The HTTP Read-Eval-Print Loop (REPL) is:</span></span>

* <span data-ttu-id="f242a-106">Lekkie, międzyplatformowe narzędzie wiersza polecenia, które jest obsługiwane wszędzie tam, gdzie jest obsługiwane środowisko .NET Core.</span><span class="sxs-lookup"><span data-stu-id="f242a-106">A lightweight, cross-platform command-line tool that's supported everywhere .NET Core is supported.</span></span>
* <span data-ttu-id="f242a-107">Służy do wykonywania żądań HTTP do testowania ASP.NET Core interfejsów API sieci Web i wyświetlania ich wyników.</span><span class="sxs-lookup"><span data-stu-id="f242a-107">Used for making HTTP requests to test ASP.NET Core web APIs and view their results.</span></span>

<span data-ttu-id="f242a-108">Obsługiwane są następujące [czasowniki http](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#74-supported-methods) :</span><span class="sxs-lookup"><span data-stu-id="f242a-108">The following [HTTP verbs](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#74-supported-methods) are supported:</span></span>

* [<span data-ttu-id="f242a-109">USUNIĘTY</span><span class="sxs-lookup"><span data-stu-id="f242a-109">DELETE</span></span>](#test-http-delete-requests)
* [<span data-ttu-id="f242a-110">GET</span><span class="sxs-lookup"><span data-stu-id="f242a-110">GET</span></span>](#test-http-get-requests)
* [<span data-ttu-id="f242a-111">MTP</span><span class="sxs-lookup"><span data-stu-id="f242a-111">HEAD</span></span>](#test-http-head-requests)
* [<span data-ttu-id="f242a-112">OPCJE</span><span class="sxs-lookup"><span data-stu-id="f242a-112">OPTIONS</span></span>](#test-http-options-requests)
* [<span data-ttu-id="f242a-113">WYSŁANA</span><span class="sxs-lookup"><span data-stu-id="f242a-113">PATCH</span></span>](#test-http-patch-requests)
* [<span data-ttu-id="f242a-114">POST</span><span class="sxs-lookup"><span data-stu-id="f242a-114">POST</span></span>](#test-http-post-requests)
* [<span data-ttu-id="f242a-115">UBRANI</span><span class="sxs-lookup"><span data-stu-id="f242a-115">PUT</span></span>](#test-http-put-requests)

<span data-ttu-id="f242a-116">Aby wykonać te czynności, [Wyświetl lub Pobierz przykładowy ASP.NET Core internetowy interfejs API](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/http-repl/samples) ([jak pobrać](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="f242a-116">To follow along, [view or download the sample ASP.NET Core web API](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/http-repl/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f242a-117">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="f242a-117">Prerequisites</span></span>

* [!INCLUDE [2.1-SDK](~/includes/2.1-SDK.md)]

## <a name="installation"></a><span data-ttu-id="f242a-118">Instalacja</span><span class="sxs-lookup"><span data-stu-id="f242a-118">Installation</span></span>

<span data-ttu-id="f242a-119">Aby zainstalować REPL HTTP, uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="f242a-119">To install the HTTP REPL, run the following command:</span></span>

```console
dotnet tool install -g Microsoft.dotnet-httprepl --version "3.0.0-*"
```

<span data-ttu-id="f242a-120">[Narzędzie globalne platformy .NET Core](/dotnet/core/tools/global-tools#install-a-global-tool) jest instalowane z pakietu NuGet [Microsoft. dotnet-httprepl](https://www.nuget.org/packages/Microsoft.dotnet-httprepl) .</span><span class="sxs-lookup"><span data-stu-id="f242a-120">A [.NET Core Global Tool](/dotnet/core/tools/global-tools#install-a-global-tool) is installed from the [Microsoft.dotnet-httprepl](https://www.nuget.org/packages/Microsoft.dotnet-httprepl) NuGet package.</span></span>

## <a name="usage"></a><span data-ttu-id="f242a-121">Użycie</span><span class="sxs-lookup"><span data-stu-id="f242a-121">Usage</span></span>

<span data-ttu-id="f242a-122">Po pomyślnej instalacji narzędzia Uruchom następujące polecenie, aby uruchomić REPL HTTP:</span><span class="sxs-lookup"><span data-stu-id="f242a-122">After successful installation of the tool, run the following command to start the HTTP REPL:</span></span>

```console
dotnet httprepl
```

<span data-ttu-id="f242a-123">Aby wyświetlić dostępne polecenia HTTP REPL, Uruchom jedno z następujących poleceń:</span><span class="sxs-lookup"><span data-stu-id="f242a-123">To view the available HTTP REPL commands, run one of the following commands:</span></span>

```console
dotnet httprepl -h
```

```console
dotnet httprepl --help
```

<span data-ttu-id="f242a-124">Wyświetlane są następujące dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="f242a-124">The following output is displayed:</span></span>

```console
Usage:
  dotnet httprepl [<BASE_ADDRESS>] [options]

Arguments:
  <BASE_ADDRESS> - The initial base address for the REPL.

Options:
  -h|--help - Show help information.

Once the REPL starts, these commands are valid:

HTTP Commands:
Use these commands to execute requests against your application.

GET            get - Issues a GET request
POST           post - Issues a POST request
PUT            put - Issues a PUT request
DELETE         delete - Issues a DELETE request
PATCH          patch - Issues a PATCH request
HEAD           head - Issues a HEAD request
OPTIONS        options - Issues a OPTIONS request

set header     Sets or clears a header for all requests. e.g. `set header content-type application/json`

Navigation Commands:
The REPL allows you to navigate your URL space and focus on specific APIs that you are working on.

set base       Set the base URI. e.g. `set base http://locahost:5000`
set swagger    Sets the swagger document to use for information about the current server
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

<span data-ttu-id="f242a-125">REPL HTTP oferuje polecenie uzupełniania.</span><span class="sxs-lookup"><span data-stu-id="f242a-125">The HTTP REPL offers command completion.</span></span> <span data-ttu-id="f242a-126">Naciśnięcie klawisza <kbd>Tab</kbd> wykonuje iterację na liście poleceń, które uzupełniają wpisane znaki lub punkt końcowy interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="f242a-126">Pressing the <kbd>Tab</kbd> key iterates through the list of commands that complete the characters or API endpoint that you typed.</span></span> <span data-ttu-id="f242a-127">W poniższych sekcjach znajduje się opis dostępnych poleceń interfejsu wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="f242a-127">The following sections outline the available CLI commands.</span></span>

## <a name="connect-to-the-web-api"></a><span data-ttu-id="f242a-128">Nawiązywanie połączenia z interfejsem API sieci Web</span><span class="sxs-lookup"><span data-stu-id="f242a-128">Connect to the web API</span></span>

<span data-ttu-id="f242a-129">Połącz się z interfejsem API sieci Web, uruchamiając następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="f242a-129">Connect to a web API by running the following command:</span></span>

```console
dotnet httprepl <BASE URI>
```

<span data-ttu-id="f242a-130">`<BASE URI>`jest podstawowym identyfikatorem URI dla internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="f242a-130">`<BASE URI>` is the base URI for the web API.</span></span> <span data-ttu-id="f242a-131">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="f242a-131">For example:</span></span>

```console
dotnet httprepl https://localhost:5001
```

<span data-ttu-id="f242a-132">Alternatywnie Uruchom następujące polecenie w dowolnym momencie podczas działania REPL HTTP:</span><span class="sxs-lookup"><span data-stu-id="f242a-132">Alternatively, run the following command at any time while the HTTP REPL is running:</span></span>

```console
set base <BASE URI>
```

<span data-ttu-id="f242a-133">Przykład:</span><span class="sxs-lookup"><span data-stu-id="f242a-133">For example:</span></span>

```console
(Disconnected)~ set base https://localhost:5001
```

## <a name="point-to-the-swagger-document-for-the-web-api"></a><span data-ttu-id="f242a-134">Wskaż dokument struktury Swagger dla internetowego interfejsu API</span><span class="sxs-lookup"><span data-stu-id="f242a-134">Point to the Swagger document for the web API</span></span>

<span data-ttu-id="f242a-135">Aby poprawnie sprawdzić internetowy interfejs API, ustaw względny identyfikator URI na dokument struktury Swagger dla internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="f242a-135">To properly inspect the web API, set the relative URI to the Swagger document for the web API.</span></span> <span data-ttu-id="f242a-136">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="f242a-136">Run the following command:</span></span>

```console
set swagger <RELATIVE URI>
```

<span data-ttu-id="f242a-137">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="f242a-137">For example:</span></span>

```console
https://localhost:5001/~ set swagger /swagger/v1/swagger.json
```

## <a name="navigate-the-web-api"></a><span data-ttu-id="f242a-138">Nawigowanie po interfejsie API sieci Web</span><span class="sxs-lookup"><span data-stu-id="f242a-138">Navigate the web API</span></span>

### <a name="view-available-endpoints"></a><span data-ttu-id="f242a-139">Wyświetl dostępne punkty końcowe</span><span class="sxs-lookup"><span data-stu-id="f242a-139">View available endpoints</span></span>

<span data-ttu-id="f242a-140">Aby wyświetlić listę różnych punktów końcowych (kontrolerów) znajdujących się w bieżącej ścieżce adresu internetowego interfejsu API `ls` , `dir` Uruchom polecenie lub:</span><span class="sxs-lookup"><span data-stu-id="f242a-140">To list the different endpoints (controllers) at the current path of the web API address, run the `ls` or `dir` command:</span></span>

```console
https://localhot:5001/~ ls
```

<span data-ttu-id="f242a-141">Wyświetlany jest następujący format danych wyjściowych:</span><span class="sxs-lookup"><span data-stu-id="f242a-141">The following output format is displayed:</span></span>

```console
.        []
Fruits   [get|post]
People   [get|post]

https://localhost:5001/~
```

<span data-ttu-id="f242a-142">Powyższe dane wyjściowe wskazują, że dostępne są dwa kontrolery: `Fruits` i `People`.</span><span class="sxs-lookup"><span data-stu-id="f242a-142">The preceding output indicates that there are two controllers available: `Fruits` and `People`.</span></span> <span data-ttu-id="f242a-143">Oba kontrolery obsługują bez parametrów operacje GET i POST HTTP.</span><span class="sxs-lookup"><span data-stu-id="f242a-143">Both controllers support parameterless HTTP GET and POST operations.</span></span>

<span data-ttu-id="f242a-144">Przechodzenie do określonego kontrolera ujawnia więcej szczegółów.</span><span class="sxs-lookup"><span data-stu-id="f242a-144">Navigating into a specific controller reveals more detail.</span></span> <span data-ttu-id="f242a-145">Na przykład następujące dane wyjściowe polecenia pokazują `Fruits` kontroler obsługuje również operacje Get, PUT i DELETE protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="f242a-145">For example, the following command's output shows the `Fruits` controller also supports HTTP GET, PUT, and DELETE operations.</span></span> <span data-ttu-id="f242a-146">Każda z tych operacji oczekuje `id` parametru w marszrucie:</span><span class="sxs-lookup"><span data-stu-id="f242a-146">Each of these operations expects an `id` parameter in the route:</span></span>

```console
https://localhost:5001/fruits~ ls
.      [get|post]
..     []
{id}   [get|put|delete]

https://localhost:5001/fruits~
```

<span data-ttu-id="f242a-147">Alternatywnie można uruchomić `ui` polecenie, aby otworzyć stronę interfejsu użytkownika programu Swagger interfejsu API sieci Web w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="f242a-147">Alternatively, run the `ui` command to open the web API's Swagger UI page in a browser.</span></span> <span data-ttu-id="f242a-148">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="f242a-148">For example:</span></span>

```console
https://localhost:5001/~ ui
```

### <a name="navigate-to-an-endpoint"></a><span data-ttu-id="f242a-149">Przejdź do punktu końcowego</span><span class="sxs-lookup"><span data-stu-id="f242a-149">Navigate to an endpoint</span></span>

<span data-ttu-id="f242a-150">Aby przejść do innego punktu końcowego w internetowym interfejsie API, `cd` Uruchom polecenie:</span><span class="sxs-lookup"><span data-stu-id="f242a-150">To navigate to a different endpoint on the web API, run the `cd` command:</span></span>

```console
https://localhost:5001/~ cd people
```

<span data-ttu-id="f242a-151">W ścieżce następującego `cd` polecenia jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="f242a-151">The path following the `cd` command is case insensitive.</span></span> <span data-ttu-id="f242a-152">Wyświetlany jest następujący format danych wyjściowych:</span><span class="sxs-lookup"><span data-stu-id="f242a-152">The following output format is displayed:</span></span>

```console
/people    [get|post]

https://localhost:5001/people~
```

## <a name="customize-the-http-repl"></a><span data-ttu-id="f242a-153">Dostosowywanie REPL HTTP</span><span class="sxs-lookup"><span data-stu-id="f242a-153">Customize the HTTP REPL</span></span>

<span data-ttu-id="f242a-154">Domyślne [kolory](#set-color-preferences) REPL http można dostosować.</span><span class="sxs-lookup"><span data-stu-id="f242a-154">The HTTP REPL's default [colors](#set-color-preferences) can be customized.</span></span> <span data-ttu-id="f242a-155">Ponadto można zdefiniować [domyślny edytor tekstu](#set-the-default-text-editor) .</span><span class="sxs-lookup"><span data-stu-id="f242a-155">Additionally, a [default text editor](#set-the-default-text-editor) can be defined.</span></span> <span data-ttu-id="f242a-156">Preferencje HTTP REPL są utrwalane w bieżącej sesji i są honorowane w przyszłych sesjach.</span><span class="sxs-lookup"><span data-stu-id="f242a-156">The HTTP REPL preferences are persisted across the current session and are honored in future sessions.</span></span> <span data-ttu-id="f242a-157">Po zmodyfikowaniu preferencje są przechowywane w następującym pliku:</span><span class="sxs-lookup"><span data-stu-id="f242a-157">Once modified, the preferences are stored in the following file:</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="f242a-158">Linux</span><span class="sxs-lookup"><span data-stu-id="f242a-158">Linux</span></span>](#tab/linux)

<span data-ttu-id="f242a-159">*% HOME%/.httpreplprefs*</span><span class="sxs-lookup"><span data-stu-id="f242a-159">*%HOME%/.httpreplprefs*</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="f242a-160">macOS</span><span class="sxs-lookup"><span data-stu-id="f242a-160">macOS</span></span>](#tab/macos)

<span data-ttu-id="f242a-161">*% HOME%/.httpreplprefs*</span><span class="sxs-lookup"><span data-stu-id="f242a-161">*%HOME%/.httpreplprefs*</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="f242a-162">Windows</span><span class="sxs-lookup"><span data-stu-id="f242a-162">Windows</span></span>](#tab/windows)

<span data-ttu-id="f242a-163">*% USERPROFILE%\\. httpreplprefs*</span><span class="sxs-lookup"><span data-stu-id="f242a-163">*%USERPROFILE%\\.httpreplprefs*</span></span>

---

<span data-ttu-id="f242a-164">Plik *. httpreplprefs* jest ładowany podczas uruchamiania i nie jest monitorowany pod kątem zmian w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="f242a-164">The *.httpreplprefs* file is loaded on startup and not monitored for changes at runtime.</span></span> <span data-ttu-id="f242a-165">Ręczne modyfikacje pliku zaczną obowiązywać dopiero po ponownym uruchomieniu narzędzia.</span><span class="sxs-lookup"><span data-stu-id="f242a-165">Manual modifications to the file take effect only after restarting the tool.</span></span>

### <a name="view-the-settings"></a><span data-ttu-id="f242a-166">Wyświetlanie ustawień</span><span class="sxs-lookup"><span data-stu-id="f242a-166">View the settings</span></span>

<span data-ttu-id="f242a-167">Aby wyświetlić dostępne ustawienia, uruchom `pref get` polecenie.</span><span class="sxs-lookup"><span data-stu-id="f242a-167">To view the available settings, run the `pref get` command.</span></span> <span data-ttu-id="f242a-168">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="f242a-168">For example:</span></span>

```console
https://localhost:5001/~ pref get
```

<span data-ttu-id="f242a-169">Poprzednie polecenie wyświetla dostępne pary klucz-wartość:</span><span class="sxs-lookup"><span data-stu-id="f242a-169">The preceding command displays the available key-value pairs:</span></span>

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

### <a name="set-color-preferences"></a><span data-ttu-id="f242a-170">Ustawianie preferencji koloru</span><span class="sxs-lookup"><span data-stu-id="f242a-170">Set color preferences</span></span>

<span data-ttu-id="f242a-171">Kolorowanie odpowiedzi jest obecnie obsługiwane tylko w przypadku formatu JSON.</span><span class="sxs-lookup"><span data-stu-id="f242a-171">Response colorization is currently supported for JSON only.</span></span> <span data-ttu-id="f242a-172">Aby dostosować domyślne kolorowanie narzędzi HTTP REPL, Znajdź klucz odpowiadający kolorowi do zmiany.</span><span class="sxs-lookup"><span data-stu-id="f242a-172">To customize the default HTTP REPL tool coloring, locate the key corresponding to the color to be changed.</span></span> <span data-ttu-id="f242a-173">Aby uzyskać instrukcje dotyczące znajdowania kluczy, zobacz sekcję [Wyświetlanie ustawień](#view-the-settings) .</span><span class="sxs-lookup"><span data-stu-id="f242a-173">For instructions on how to find the keys, see the [View the settings](#view-the-settings) section.</span></span> <span data-ttu-id="f242a-174">Na przykład zmień `colors.json` wartość klucza z `Green` na `White` w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="f242a-174">For example, change the `colors.json` key value from `Green` to `White` as follows:</span></span>

```console
https://localhost:5001/people~ pref set colors.json White
```

<span data-ttu-id="f242a-175">Można używać tylko [dozwolonych kolorów](https://github.com/aspnet/HttpRepl/blob/01d5c3c3373e98fe566ff5ef8a17c571de880293/src/Microsoft.Repl/ConsoleHandling/AllowedColors.cs) .</span><span class="sxs-lookup"><span data-stu-id="f242a-175">Only the [allowed colors](https://github.com/aspnet/HttpRepl/blob/01d5c3c3373e98fe566ff5ef8a17c571de880293/src/Microsoft.Repl/ConsoleHandling/AllowedColors.cs) may be used.</span></span> <span data-ttu-id="f242a-176">Kolejne żądania HTTP wyświetlają dane wyjściowe z nowym kolorem.</span><span class="sxs-lookup"><span data-stu-id="f242a-176">Subsequent HTTP requests display output with the new coloring.</span></span>

<span data-ttu-id="f242a-177">Jeśli określone klucze kolorów nie są ustawione, brane są więcej kluczy ogólnych.</span><span class="sxs-lookup"><span data-stu-id="f242a-177">When specific color keys aren't set, more generic keys are considered.</span></span> <span data-ttu-id="f242a-178">Aby zademonstrować to zachowanie rezerwowe, należy wziąć pod uwagę następujący przykład:</span><span class="sxs-lookup"><span data-stu-id="f242a-178">To demonstrate this fallback behavior, consider the following example:</span></span>

* <span data-ttu-id="f242a-179">Jeśli `colors.json.name` nie ma wartości, `colors.json.string` jest używana.</span><span class="sxs-lookup"><span data-stu-id="f242a-179">If `colors.json.name` doesn't have a value, `colors.json.string` is used.</span></span>
* <span data-ttu-id="f242a-180">Jeśli `colors.json.string` nie ma wartości, `colors.json.literal` jest używana.</span><span class="sxs-lookup"><span data-stu-id="f242a-180">If `colors.json.string` doesn't have a value, `colors.json.literal` is used.</span></span>
* <span data-ttu-id="f242a-181">Jeśli `colors.json.literal` nie ma wartości, `colors.json` jest używana.</span><span class="sxs-lookup"><span data-stu-id="f242a-181">If `colors.json.literal` doesn't have a value, `colors.json` is used.</span></span> 
* <span data-ttu-id="f242a-182">Jeśli `colors.json` nie ma wartości, używany jest domyślny kolor tekstu powłoki poleceń (`AllowedColors.None`).</span><span class="sxs-lookup"><span data-stu-id="f242a-182">If `colors.json` doesn't have a value, the command shell's default text color (`AllowedColors.None`) is used.</span></span>

### <a name="set-indentation-size"></a><span data-ttu-id="f242a-183">Ustaw rozmiar wcięcia</span><span class="sxs-lookup"><span data-stu-id="f242a-183">Set indentation size</span></span>

<span data-ttu-id="f242a-184">Dostosowanie rozmiaru wcięcia odpowiedzi jest obecnie obsługiwane tylko w przypadku formatu JSON.</span><span class="sxs-lookup"><span data-stu-id="f242a-184">Response indentation size customization is currently supported for JSON only.</span></span> <span data-ttu-id="f242a-185">Domyślny rozmiar to dwie spacje.</span><span class="sxs-lookup"><span data-stu-id="f242a-185">The default size is two spaces.</span></span> <span data-ttu-id="f242a-186">Przykład:</span><span class="sxs-lookup"><span data-stu-id="f242a-186">For example:</span></span>

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

<span data-ttu-id="f242a-187">Aby zmienić rozmiar domyślny, ustaw `formatting.json.indentSize` klucz.</span><span class="sxs-lookup"><span data-stu-id="f242a-187">To change the default size, set the `formatting.json.indentSize` key.</span></span> <span data-ttu-id="f242a-188">Na przykład, aby zawsze używać czterech spacji:</span><span class="sxs-lookup"><span data-stu-id="f242a-188">For example, to always use four spaces:</span></span>

```console
pref set formatting.json.indentSize 4
```

<span data-ttu-id="f242a-189">Kolejne odpowiedzi przestrzegają ustawień czterech spacji:</span><span class="sxs-lookup"><span data-stu-id="f242a-189">Subsequent responses honor the setting of four spaces:</span></span>

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

### <a name="set-indentation-size"></a><span data-ttu-id="f242a-190">Ustaw rozmiar wcięcia</span><span class="sxs-lookup"><span data-stu-id="f242a-190">Set indentation size</span></span>

<span data-ttu-id="f242a-191">Dostosowanie rozmiaru wcięcia odpowiedzi jest obecnie obsługiwane tylko w przypadku formatu JSON.</span><span class="sxs-lookup"><span data-stu-id="f242a-191">Response indentation size customization is currently supported for JSON only.</span></span> <span data-ttu-id="f242a-192">Domyślny rozmiar to dwie spacje.</span><span class="sxs-lookup"><span data-stu-id="f242a-192">The default size is two spaces.</span></span> <span data-ttu-id="f242a-193">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="f242a-193">For example:</span></span>

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

<span data-ttu-id="f242a-194">Aby zmienić rozmiar domyślny, ustaw `formatting.json.indentSize` klucz.</span><span class="sxs-lookup"><span data-stu-id="f242a-194">To change the default size, set the `formatting.json.indentSize` key.</span></span> <span data-ttu-id="f242a-195">Na przykład, aby zawsze używać czterech spacji:</span><span class="sxs-lookup"><span data-stu-id="f242a-195">For example, to always use four spaces:</span></span>

```console
pref set formatting.json.indentSize 4
```

<span data-ttu-id="f242a-196">Kolejne odpowiedzi przestrzegają ustawień czterech spacji:</span><span class="sxs-lookup"><span data-stu-id="f242a-196">Subsequent responses honor the setting of four spaces:</span></span>

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

### <a name="set-the-default-text-editor"></a><span data-ttu-id="f242a-197">Ustawianie domyślnego edytora tekstu</span><span class="sxs-lookup"><span data-stu-id="f242a-197">Set the default text editor</span></span>

<span data-ttu-id="f242a-198">Domyślnie REPL HTTP nie ma edytora tekstu skonfigurowanego do użycia.</span><span class="sxs-lookup"><span data-stu-id="f242a-198">By default, the HTTP REPL has no text editor configured for use.</span></span> <span data-ttu-id="f242a-199">Aby przetestować metody interfejsu API sieci Web wymagające treści żądania HTTP, należy ustawić domyślny edytor tekstu.</span><span class="sxs-lookup"><span data-stu-id="f242a-199">To test web API methods requiring an HTTP request body, a default text editor must be set.</span></span> <span data-ttu-id="f242a-200">Narzędzie HTTP REPL uruchamia skonfigurowany Edytor tekstów wyłącznie na potrzeby redagowania treści żądania.</span><span class="sxs-lookup"><span data-stu-id="f242a-200">The HTTP REPL tool launches the configured text editor for the sole purpose of composing the request body.</span></span> <span data-ttu-id="f242a-201">Uruchom następujące polecenie, aby ustawić preferowany Edytor tekstu jako domyślny:</span><span class="sxs-lookup"><span data-stu-id="f242a-201">Run the following command to set your preferred text editor as the default:</span></span>

```console
pref set editor.command.default "<EXECUTABLE>"
```

<span data-ttu-id="f242a-202">W poprzednim poleceniu `<EXECUTABLE>` jest pełną ścieżką do pliku wykonywalnego edytora tekstu.</span><span class="sxs-lookup"><span data-stu-id="f242a-202">In the preceding command, `<EXECUTABLE>` is the full path to the text editor's executable file.</span></span> <span data-ttu-id="f242a-203">Na przykład uruchom następujące polecenie, aby ustawić Visual Studio Code jako domyślny edytor tekstu:</span><span class="sxs-lookup"><span data-stu-id="f242a-203">For example, run the following command to set Visual Studio Code as the default text editor:</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="f242a-204">Linux</span><span class="sxs-lookup"><span data-stu-id="f242a-204">Linux</span></span>](#tab/linux)

```console
pref set editor.command.default "/usr/bin/code"
```

# <a name="macostabmacos"></a>[<span data-ttu-id="f242a-205">macOS</span><span class="sxs-lookup"><span data-stu-id="f242a-205">macOS</span></span>](#tab/macos)

```console
pref set editor.command.default "/Applications/Visual Studio Code.app/Contents/Resources/app/bin/code"
```

# <a name="windowstabwindows"></a>[<span data-ttu-id="f242a-206">Windows</span><span class="sxs-lookup"><span data-stu-id="f242a-206">Windows</span></span>](#tab/windows)

```console
pref set editor.command.default "C:\Program Files\Microsoft VS Code\Code.exe"
```

---

<span data-ttu-id="f242a-207">Aby uruchomić domyślny edytor tekstu z określonymi argumentami interfejsu wiersza polecenia, `editor.command.default.arguments` należy ustawić klucz.</span><span class="sxs-lookup"><span data-stu-id="f242a-207">To launch the default text editor with specific CLI arguments, set the `editor.command.default.arguments` key.</span></span> <span data-ttu-id="f242a-208">Załóżmy na przykład, że Visual Studio Code jest domyślnym edytorem tekstu i zawsze chcesz, aby REPL HTTP mógł otworzyć Visual Studio Code w nowej sesji z wyłączonymi rozszerzeniami.</span><span class="sxs-lookup"><span data-stu-id="f242a-208">For example, assume Visual Studio Code is the default text editor and that you always want the HTTP REPL to open Visual Studio Code in a new session with extensions disabled.</span></span> <span data-ttu-id="f242a-209">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="f242a-209">Run the following command:</span></span>

```console
pref set editor.command.default.arguments "--disable-extensions --new-window"
```

## <a name="test-http-get-requests"></a><span data-ttu-id="f242a-210">Testuj żądania HTTP GET</span><span class="sxs-lookup"><span data-stu-id="f242a-210">Test HTTP GET requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="f242a-211">Streszczenie</span><span class="sxs-lookup"><span data-stu-id="f242a-211">Synopsis</span></span>

```console
get <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="f242a-212">Argumenty</span><span class="sxs-lookup"><span data-stu-id="f242a-212">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="f242a-213">Parametr trasy, jeśli istnieje, oczekiwany przez skojarzoną metodę akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="f242a-213">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="f242a-214">Opcje</span><span class="sxs-lookup"><span data-stu-id="f242a-214">Options</span></span>

<span data-ttu-id="f242a-215">Następujące opcje są dostępne dla `get` polecenia:</span><span class="sxs-lookup"><span data-stu-id="f242a-215">The following options are available for the `get` command:</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

### <a name="example"></a><span data-ttu-id="f242a-216">Przykład</span><span class="sxs-lookup"><span data-stu-id="f242a-216">Example</span></span>

<span data-ttu-id="f242a-217">Aby wydać żądanie HTTP GET:</span><span class="sxs-lookup"><span data-stu-id="f242a-217">To issue an HTTP GET request:</span></span>

1. <span data-ttu-id="f242a-218">`get` Uruchom polecenie w punkcie końcowym, który go obsługuje:</span><span class="sxs-lookup"><span data-stu-id="f242a-218">Run the `get` command on an endpoint that supports it:</span></span>

    ```console
    https://localhost:5001/people~ get
    ```

    <span data-ttu-id="f242a-219">Poprzednie polecenie wyświetla następujący format danych wyjściowych:</span><span class="sxs-lookup"><span data-stu-id="f242a-219">The preceding command displays the following output format:</span></span>

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

1. <span data-ttu-id="f242a-220">Pobierz konkretny rekord, przekazując parametr do `get` polecenia:</span><span class="sxs-lookup"><span data-stu-id="f242a-220">Retrieve a specific record by passing a parameter to the `get` command:</span></span>

    ```console
    https://localhost:5001/people~ get 2
    ```

    <span data-ttu-id="f242a-221">Poprzednie polecenie wyświetla następujący format danych wyjściowych:</span><span class="sxs-lookup"><span data-stu-id="f242a-221">The preceding command displays the following output format:</span></span>

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

## <a name="test-http-post-requests"></a><span data-ttu-id="f242a-222">Testowanie żądań POST protokołu HTTP</span><span class="sxs-lookup"><span data-stu-id="f242a-222">Test HTTP POST requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="f242a-223">Streszczenie</span><span class="sxs-lookup"><span data-stu-id="f242a-223">Synopsis</span></span>

```console
post <PARAMETER> [-c|--content] [-f|--file] [-h|--header] [--no-body] [-F|--no-formatting] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="f242a-224">Argumenty</span><span class="sxs-lookup"><span data-stu-id="f242a-224">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="f242a-225">Parametr trasy, jeśli istnieje, oczekiwany przez skojarzoną metodę akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="f242a-225">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="f242a-226">Opcje</span><span class="sxs-lookup"><span data-stu-id="f242a-226">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

[!INCLUDE [HTTP request body CLI options](~/includes/http-repl/requires-body-options.md)]

### <a name="example"></a><span data-ttu-id="f242a-227">Przykład</span><span class="sxs-lookup"><span data-stu-id="f242a-227">Example</span></span>

<span data-ttu-id="f242a-228">Aby wydać żądanie HTTP POST:</span><span class="sxs-lookup"><span data-stu-id="f242a-228">To issue an HTTP POST request:</span></span>

1. <span data-ttu-id="f242a-229">`post` Uruchom polecenie w punkcie końcowym, który go obsługuje:</span><span class="sxs-lookup"><span data-stu-id="f242a-229">Run the `post` command on an endpoint that supports it:</span></span>

    ```console
    https://localhost:5001/people~ post -h Content-Type=application/json
    ```

    <span data-ttu-id="f242a-230">W poprzednim poleceniu nagłówek żądania `Content-Type` http jest ustawiany w taki sposób, aby wskazywał typ nośnika treści żądania JSON.</span><span class="sxs-lookup"><span data-stu-id="f242a-230">In the preceding command, the `Content-Type` HTTP request header is set to indicate a request body media type of JSON.</span></span> <span data-ttu-id="f242a-231">Domyślny edytor tekstu otwiera plik *. tmp* z szablonem JSON reprezentującym treść żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="f242a-231">The default text editor opens a *.tmp* file with a JSON template representing the HTTP request body.</span></span> <span data-ttu-id="f242a-232">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="f242a-232">For example:</span></span>

    ```json
    {
      "id": 0,
      "name": ""
    }
    ```

    > [!TIP]
    > <span data-ttu-id="f242a-233">Aby ustawić domyślny edytor tekstu, zobacz sekcję [Ustawianie domyślnego edytora tekstu](#set-the-default-text-editor) .</span><span class="sxs-lookup"><span data-stu-id="f242a-233">To set the default text editor, see the [Set the default text editor](#set-the-default-text-editor) section.</span></span>

1. <span data-ttu-id="f242a-234">Zmodyfikuj szablon JSON, aby spełniał wymagania dotyczące weryfikacji modelu:</span><span class="sxs-lookup"><span data-stu-id="f242a-234">Modify the JSON template to satisfy model validation requirements:</span></span>

  ```json
  {
    "id": 0,
    "name": "Scott Addie"
  }
  ```

1. <span data-ttu-id="f242a-235">Zapisz plik *. tmp* i Zamknij Edytor tekstu.</span><span class="sxs-lookup"><span data-stu-id="f242a-235">Save the *.tmp* file, and close the text editor.</span></span> <span data-ttu-id="f242a-236">Następujące dane wyjściowe są wyświetlane w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="f242a-236">The following output appears in the command shell:</span></span>

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

## <a name="test-http-put-requests"></a><span data-ttu-id="f242a-237">Testowanie żądań HTTP PUT</span><span class="sxs-lookup"><span data-stu-id="f242a-237">Test HTTP PUT requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="f242a-238">Streszczenie</span><span class="sxs-lookup"><span data-stu-id="f242a-238">Synopsis</span></span>

```console
put <PARAMETER> [-c|--content] [-f|--file] [-h|--header] [--no-body] [-F|--no-formatting] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="f242a-239">Argumenty</span><span class="sxs-lookup"><span data-stu-id="f242a-239">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="f242a-240">Parametr trasy, jeśli istnieje, oczekiwany przez skojarzoną metodę akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="f242a-240">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="f242a-241">Opcje</span><span class="sxs-lookup"><span data-stu-id="f242a-241">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

[!INCLUDE [HTTP request body CLI options](~/includes/http-repl/requires-body-options.md)]

### <a name="example"></a><span data-ttu-id="f242a-242">Przykład</span><span class="sxs-lookup"><span data-stu-id="f242a-242">Example</span></span>

<span data-ttu-id="f242a-243">Aby wydać żądanie HTTP PUT:</span><span class="sxs-lookup"><span data-stu-id="f242a-243">To issue an HTTP PUT request:</span></span>

1. <span data-ttu-id="f242a-244">*Opcjonalne*: `get` Uruchom polecenie, aby wyświetlić dane przed zmodyfikowaniem:</span><span class="sxs-lookup"><span data-stu-id="f242a-244">*Optional*: Run the `get` command to view the data before modifying it:</span></span>

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

    <span data-ttu-id="f242a-245">W poprzednim poleceniu nagłówek żądania `Content-Type` http jest ustawiany w taki sposób, aby wskazywał typ nośnika treści żądania JSON.</span><span class="sxs-lookup"><span data-stu-id="f242a-245">In the preceding command, the `Content-Type` HTTP request header is set to indicate a request body media type of JSON.</span></span> <span data-ttu-id="f242a-246">Domyślny edytor tekstu otwiera plik *. tmp* z szablonem JSON reprezentującym treść żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="f242a-246">The default text editor opens a *.tmp* file with a JSON template representing the HTTP request body.</span></span> <span data-ttu-id="f242a-247">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="f242a-247">For example:</span></span>

    ```json
    {
      "id": 0,
      "name": ""
    }
    ```

    > [!TIP]
    > <span data-ttu-id="f242a-248">Aby ustawić domyślny edytor tekstu, zobacz sekcję [Ustawianie domyślnego edytora tekstu](#set-the-default-text-editor) .</span><span class="sxs-lookup"><span data-stu-id="f242a-248">To set the default text editor, see the [Set the default text editor](#set-the-default-text-editor) section.</span></span>

1. <span data-ttu-id="f242a-249">Zmodyfikuj szablon JSON, aby spełniał wymagania dotyczące weryfikacji modelu:</span><span class="sxs-lookup"><span data-stu-id="f242a-249">Modify the JSON template to satisfy model validation requirements:</span></span>

    ```json
    {
      "id": 2,
      "name": "Cherry"
    }
    ```

1. <span data-ttu-id="f242a-250">Zapisz plik *. tmp* i Zamknij Edytor tekstu.</span><span class="sxs-lookup"><span data-stu-id="f242a-250">Save the *.tmp* file, and close the text editor.</span></span> <span data-ttu-id="f242a-251">Następujące dane wyjściowe są wyświetlane w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="f242a-251">The following output appears in the command shell:</span></span>

    ```console
    [main 2019-06-28T17:27:01.805Z] update#setState idle
    HTTP/1.1 204 No Content
    Date: Fri, 28 Jun 2019 17:28:21 GMT
    Server: Kestrel
    ```

1. <span data-ttu-id="f242a-252">*Opcjonalne*: `get` Wydaj polecenie, aby zobaczyć modyfikacje.</span><span class="sxs-lookup"><span data-stu-id="f242a-252">*Optional*: Issue a `get` command to see the modifications.</span></span> <span data-ttu-id="f242a-253">Na przykład, jeśli wpisano "wiśnię" w edytorze tekstu, `get` zwraca następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="f242a-253">For example, if you typed "Cherry" in the text editor, a `get` returns the following:</span></span>

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

## <a name="test-http-delete-requests"></a><span data-ttu-id="f242a-254">Testowanie żądań HTTP DELETE</span><span class="sxs-lookup"><span data-stu-id="f242a-254">Test HTTP DELETE requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="f242a-255">Streszczenie</span><span class="sxs-lookup"><span data-stu-id="f242a-255">Synopsis</span></span>

```console
delete <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="f242a-256">Argumenty</span><span class="sxs-lookup"><span data-stu-id="f242a-256">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="f242a-257">Parametr trasy, jeśli istnieje, oczekiwany przez skojarzoną metodę akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="f242a-257">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="f242a-258">Opcje</span><span class="sxs-lookup"><span data-stu-id="f242a-258">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

### <a name="example"></a><span data-ttu-id="f242a-259">Przykład</span><span class="sxs-lookup"><span data-stu-id="f242a-259">Example</span></span>

<span data-ttu-id="f242a-260">Aby wydać żądanie HTTP DELETE:</span><span class="sxs-lookup"><span data-stu-id="f242a-260">To issue an HTTP DELETE request:</span></span>

1. <span data-ttu-id="f242a-261">*Opcjonalne*: `get` Uruchom polecenie, aby wyświetlić dane przed zmodyfikowaniem:</span><span class="sxs-lookup"><span data-stu-id="f242a-261">*Optional*: Run the `get` command to view the data before modifying it:</span></span>

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

    <span data-ttu-id="f242a-262">Poprzednie polecenie wyświetla następujący format danych wyjściowych:</span><span class="sxs-lookup"><span data-stu-id="f242a-262">The preceding command displays the following output format:</span></span>

    ```console
    HTTP/1.1 204 No Content
    Date: Fri, 28 Jun 2019 17:36:42 GMT
    Server: Kestrel
    ```

1. <span data-ttu-id="f242a-263">*Opcjonalne*: `get` Wydaj polecenie, aby zobaczyć modyfikacje.</span><span class="sxs-lookup"><span data-stu-id="f242a-263">*Optional*: Issue a `get` command to see the modifications.</span></span> <span data-ttu-id="f242a-264">W tym przykładzie `get` zwraca następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="f242a-264">In this example, a `get` returns the following:</span></span>

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

## <a name="test-http-patch-requests"></a><span data-ttu-id="f242a-265">Testuj żądania poprawek HTTP</span><span class="sxs-lookup"><span data-stu-id="f242a-265">Test HTTP PATCH requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="f242a-266">Streszczenie</span><span class="sxs-lookup"><span data-stu-id="f242a-266">Synopsis</span></span>

```console
patch <PARAMETER> [-c|--content] [-f|--file] [-h|--header] [--no-body] [-F|--no-formatting] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="f242a-267">Argumenty</span><span class="sxs-lookup"><span data-stu-id="f242a-267">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="f242a-268">Parametr trasy, jeśli istnieje, oczekiwany przez skojarzoną metodę akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="f242a-268">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="f242a-269">Opcje</span><span class="sxs-lookup"><span data-stu-id="f242a-269">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

[!INCLUDE [HTTP request body CLI options](~/includes/http-repl/requires-body-options.md)]

## <a name="test-http-head-requests"></a><span data-ttu-id="f242a-270">Testowanie żądań głównych HTTP</span><span class="sxs-lookup"><span data-stu-id="f242a-270">Test HTTP HEAD requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="f242a-271">Streszczenie</span><span class="sxs-lookup"><span data-stu-id="f242a-271">Synopsis</span></span>

```console
head <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="f242a-272">Argumenty</span><span class="sxs-lookup"><span data-stu-id="f242a-272">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="f242a-273">Parametr trasy, jeśli istnieje, oczekiwany przez skojarzoną metodę akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="f242a-273">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="f242a-274">Opcje</span><span class="sxs-lookup"><span data-stu-id="f242a-274">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

## <a name="test-http-options-requests"></a><span data-ttu-id="f242a-275">Testowe żądania opcji HTTP</span><span class="sxs-lookup"><span data-stu-id="f242a-275">Test HTTP OPTIONS requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="f242a-276">Streszczenie</span><span class="sxs-lookup"><span data-stu-id="f242a-276">Synopsis</span></span>

```console
options <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="f242a-277">Argumenty</span><span class="sxs-lookup"><span data-stu-id="f242a-277">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="f242a-278">Parametr trasy, jeśli istnieje, oczekiwany przez skojarzoną metodę akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="f242a-278">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="f242a-279">Opcje</span><span class="sxs-lookup"><span data-stu-id="f242a-279">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

## <a name="set-http-request-headers"></a><span data-ttu-id="f242a-280">Ustawianie nagłówków żądań HTTP</span><span class="sxs-lookup"><span data-stu-id="f242a-280">Set HTTP request headers</span></span>

<span data-ttu-id="f242a-281">Aby ustawić nagłówek żądania HTTP, należy użyć jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="f242a-281">To set an HTTP request header, use one of the following approaches:</span></span>

1. <span data-ttu-id="f242a-282">Ustaw wartość inline z żądaniem HTTP.</span><span class="sxs-lookup"><span data-stu-id="f242a-282">Set inline with the HTTP request.</span></span> <span data-ttu-id="f242a-283">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="f242a-283">For example:</span></span>

  ```console
  https://localhost:5001/people~ post -h Content-Type=application/json
  ```

  <span data-ttu-id="f242a-284">W przypadku wcześniejszego podejścia każdy unikatowy nagłówek żądania HTTP wymaga własnej `-h` opcji.</span><span class="sxs-lookup"><span data-stu-id="f242a-284">With the preceding approach, each distinct HTTP request header requires its own `-h` option.</span></span>

1. <span data-ttu-id="f242a-285">Ustaw przed wysłaniem żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="f242a-285">Set before sending the HTTP request.</span></span> <span data-ttu-id="f242a-286">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="f242a-286">For example:</span></span>

  ```console
  https://localhost:5001/people~ set header Content-Type application/json
  ```

  <span data-ttu-id="f242a-287">Podczas ustawiania nagłówka przed wysłaniem żądania nagłówek pozostaje ustawiony na czas trwania sesji powłoki poleceń.</span><span class="sxs-lookup"><span data-stu-id="f242a-287">When setting the header before sending a request, the header remains set for the duration of the command shell session.</span></span> <span data-ttu-id="f242a-288">Aby wyczyścić nagłówek, podaj wartość pustą.</span><span class="sxs-lookup"><span data-stu-id="f242a-288">To clear the header, provide an empty value.</span></span> <span data-ttu-id="f242a-289">Przykład:</span><span class="sxs-lookup"><span data-stu-id="f242a-289">For example:</span></span>

  ```console
  https://localhost:5001/people~ set header Content-Type
  ```

## <a name="toggle-http-request-display"></a><span data-ttu-id="f242a-290">Przełącz wyświetlanie żądań HTTP</span><span class="sxs-lookup"><span data-stu-id="f242a-290">Toggle HTTP request display</span></span>

<span data-ttu-id="f242a-291">Domyślnie wyświetlanie wysyłanego żądania HTTP jest pomijane.</span><span class="sxs-lookup"><span data-stu-id="f242a-291">By default, display of the HTTP request being sent is suppressed.</span></span> <span data-ttu-id="f242a-292">Istnieje możliwość zmiany odpowiedniego ustawienia dla czasu trwania sesji powłoki poleceń.</span><span class="sxs-lookup"><span data-stu-id="f242a-292">It's possible to change the corresponding setting for the duration of the command shell session.</span></span>

### <a name="enable-request-display"></a><span data-ttu-id="f242a-293">Włącz wyświetlanie żądań</span><span class="sxs-lookup"><span data-stu-id="f242a-293">Enable request display</span></span>

<span data-ttu-id="f242a-294">Wyświetl wysyłane żądanie HTTP, uruchamiając `echo on` polecenie.</span><span class="sxs-lookup"><span data-stu-id="f242a-294">View the HTTP request being sent by running the `echo on` command.</span></span> <span data-ttu-id="f242a-295">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="f242a-295">For example:</span></span>

```console
https://localhost:5001/people~ echo on
Request echoing is on
```

<span data-ttu-id="f242a-296">Kolejne żądania HTTP w bieżącej sesji wyświetlają nagłówki żądań.</span><span class="sxs-lookup"><span data-stu-id="f242a-296">Subsequent HTTP requests in the current session display the request headers.</span></span> <span data-ttu-id="f242a-297">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="f242a-297">For example:</span></span>

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

### <a name="disable-request-display"></a><span data-ttu-id="f242a-298">Wyłącz wyświetlanie żądań</span><span class="sxs-lookup"><span data-stu-id="f242a-298">Disable request display</span></span>

<span data-ttu-id="f242a-299">Pomijaj wyświetlanie wysyłanego żądania HTTP przez uruchomienie `echo off` polecenia.</span><span class="sxs-lookup"><span data-stu-id="f242a-299">Suppress display of the HTTP request being sent by running the `echo off` command.</span></span> <span data-ttu-id="f242a-300">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="f242a-300">For example:</span></span>

```console
https://localhost:5001/people~ echo off
Request echoing is off
```

## <a name="run-a-script"></a><span data-ttu-id="f242a-301">Uruchamianie skryptu</span><span class="sxs-lookup"><span data-stu-id="f242a-301">Run a script</span></span>

<span data-ttu-id="f242a-302">Jeśli często wykonujesz ten sam zestaw poleceń HTTP REPL, Rozważ przechowywanie ich w pliku tekstowym.</span><span class="sxs-lookup"><span data-stu-id="f242a-302">If you frequently execute the same set of HTTP REPL commands, consider storing them in a text file.</span></span> <span data-ttu-id="f242a-303">Polecenia w pliku mają taki sam formularz jak te wykonywane ręcznie w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="f242a-303">Commands in the file take the same form as those executed manually on the command line.</span></span> <span data-ttu-id="f242a-304">Polecenia mogą być wykonywane w sposób wsadowy przy użyciu `run` polecenia.</span><span class="sxs-lookup"><span data-stu-id="f242a-304">The commands can be executed in a batched fashion using the `run` command.</span></span> <span data-ttu-id="f242a-305">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="f242a-305">For example:</span></span>

1. <span data-ttu-id="f242a-306">Utwórz plik tekstowy zawierający zestaw poleceń rozdzielanych znakami nowego wiersza.</span><span class="sxs-lookup"><span data-stu-id="f242a-306">Create a text file containing a set of newline-delimited commands.</span></span> <span data-ttu-id="f242a-307">Aby to zilustrować, należy rozważyć plik *People-Script. txt* zawierający następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="f242a-307">To illustrate, consider a *people-script.txt* file containing the following commands:</span></span>

    ```text
    set base https://localhost:5001
    ls
    cd People
    ls
    get 1
    ```

1. <span data-ttu-id="f242a-308">`run` Wykonaj polecenie, przekazując w ścieżce pliku tekstowego.</span><span class="sxs-lookup"><span data-stu-id="f242a-308">Execute the `run` command, passing in the text file's path.</span></span> <span data-ttu-id="f242a-309">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="f242a-309">For example:</span></span>

    ```console
    https://localhost:5001/~ run C:\http-repl-scripts\people-script.txt
    ```

    <span data-ttu-id="f242a-310">Wyświetlane są następujące dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="f242a-310">The following output appears:</span></span>

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

## <a name="clear-the-output"></a><span data-ttu-id="f242a-311">Wyczyść dane wyjściowe</span><span class="sxs-lookup"><span data-stu-id="f242a-311">Clear the output</span></span>

<span data-ttu-id="f242a-312">Aby usunąć wszystkie dane wyjściowe zapisywane do powłoki poleceń za pomocą narzędzia http REPL, uruchom `clear` polecenie lub. `cls`</span><span class="sxs-lookup"><span data-stu-id="f242a-312">To remove all output written to the command shell by the HTTP REPL tool, run the `clear` or `cls` command.</span></span> <span data-ttu-id="f242a-313">Do zilustrowania, Załóżmy, że powłoka poleceń zawiera następujące dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="f242a-313">To illustrate, imagine the command shell contains the following output:</span></span>

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

<span data-ttu-id="f242a-314">Uruchom następujące polecenie, aby wyczyścić dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="f242a-314">Run the following command to clear the output:</span></span>

```console
https://localhost:5001/~ clear
```

<span data-ttu-id="f242a-315">Po uruchomieniu poprzedniego polecenia powłoka poleceń zawiera tylko następujące dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="f242a-315">After running the preceding command, the command shell contains only the following output:</span></span>

```console
https://localhost:5001/~
```

## <a name="additional-resources"></a><span data-ttu-id="f242a-316">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="f242a-316">Additional resources</span></span>

* [<span data-ttu-id="f242a-317">Żądania interfejsu API REST</span><span class="sxs-lookup"><span data-stu-id="f242a-317">REST API requests</span></span>](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#74-supported-methods)
* [<span data-ttu-id="f242a-318">Repozytorium usługi GitHub HTTP REPL</span><span class="sxs-lookup"><span data-stu-id="f242a-318">HTTP REPL GitHub repository</span></span>](https://github.com/aspnet/HttpRepl)
