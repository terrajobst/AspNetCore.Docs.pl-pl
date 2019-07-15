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
# <a name="test-web-apis-with-the-http-repl"></a>Test sieci web, interfejsów API za pomocą HTTP REPL

Przez [Scott Addie](https://twitter.com/Scott_Addie)

Jest pętlą odczytu HTTP — Eval-Print (REPL):

* Lekkie, Międzyplatformowe narzędzie wiersza polecenia, które ma obsługiwane wszędzie, gdzie platformy .NET Core jest obsługiwane.
* Używany do wysyłania żądań HTTP do testowania interfejsów API sieci web platformy ASP.NET Core i wyświetlania ich wyników.

Następujące [zleceń HTTP](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#74-supported-methods) są obsługiwane:

* [USUŃ](#test-http-delete-requests)
* [GET](#test-http-get-requests)
* [GŁÓWNY](#test-http-head-requests)
* [OPTIONS](#test-http-options-requests)
* [POPRAWKI](#test-http-patch-requests)
* [POST](#test-http-post-requests)
* [PUT](#test-http-put-requests)

Aby kontynuować, [wyświetlanie lub pobieranie przykładowego interfejsu API sieci web platformy ASP.NET Core](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/http-repl/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample)).

## <a name="prerequisites"></a>Wymagania wstępne

* [!INCLUDE [2.1-SDK](~/includes/2.1-SDK.md)]

## <a name="installation"></a>Instalacja

Aby zainstalować HTTP REPL, uruchom następujące polecenie:

```console
dotnet tool install -g dotnet-httprepl
    --version 2.2.0-*
    --add-source https://dotnet.myget.org/F/dotnet-core/api/v3/index.json
```

A [narzędzia programu .NET Core globalnego](/dotnet/core/tools/global-tools#install-a-global-tool) jest instalowany z [dotnet httprepl](https://dotnet.myget.org/feed/dotnet-core/package/nuget/dotnet-httprepl
) pakietu NuGet hostowanymi na platformie MyGet.

## <a name="usage"></a>Użycie

Po pomyślnej instalacji tego narzędzia uruchom następujące polecenie, aby uruchomić HTTP REPL:

```console
dotnet httprepl
```

Aby wyświetlić dostępne polecenia HTTP REPL, uruchom jedną z następujących poleceń:

```console
dotnet httprepl -h
```

```console
dotnet httprepl --help
```

Są wyświetlane następujące dane wyjściowe:

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

HTTP REPL oferuje ukończenie polecenia. Naciśnięcie klawisza <kbd>kartę</kbd> klucza, który wykonuje iterację przez listę poleceń, kończące znaki lub punkt końcowy interfejsu API, która została wpisana. W poniższych sekcjach opisano dostępne polecenia interfejsu wiersza polecenia.

## <a name="connect-to-the-web-api"></a>Połącz się z internetowym interfejsem API

Podłącz do internetowego interfejsu API, uruchamiając następujące polecenie:

```console
dotnet httprepl <BASE URI>
```

`<BASE URI>` jest podstawowy identyfikator URI dla internetowego interfejsu API. Przykład:

```console
dotnet httprepl https://localhost:5001
```

Alternatywnie uruchom następujące polecenie w dowolnym momencie po uruchomieniu HTTP REPL:

```console
set base <BASE URI>
```

Na przykład:

```console
(Disconnected)~ set base https://localhost:5001
```

## <a name="point-to-the-swagger-document-for-the-web-api"></a>Wskaż dokument struktury Swagger dla interfejsu API sieci web

Aby prawidłowo sprawdzić interfejsu API sieci web, należy ustawić względny identyfikator URI do dokumentu Swagger dla interfejsu API sieci web. Uruchom następujące polecenie:

```console
set swagger <RELATIVE URI>
```

Na przykład:

```console
https://localhost:5001/~ set swagger /swagger/v1/swagger.json
```

## <a name="navigate-the-web-api"></a>Przejdź do interfejsu API sieci web

### <a name="view-available-endpoints"></a>Wyświetl dostępne punkty końcowe

Aby wyświetlić listę różnych punktów końcowych (kontrolery) w bieżącej ścieżce adresu interfejsu API sieci web, należy uruchomić `ls` lub `dir` polecenia:

```console
https://localhot:5001/~ ls
```

Zostanie wyświetlony następujący format danych wyjściowych:

```console
.        []
Fruits   [get|post]
People   [get|post]

https://localhost:5001/~
```

Dane wyjściowe poprzedniego oznacza, że są dwa kontrolery: `Fruits` i `People`. Oba kontrolery obsługuje bez parametrów operacji HTTP GET i POST.

Przejdź do określonego kontrolera, co spowoduje wyświetlenie bardziej szczegółowo. Na przykład następujące polecenie na dane wyjściowe `Fruits` kontroler obsługuje również operacje HTTP GET, PUT i DELETE. Każda z tych operacji oczekuje `id` parametr trasy:

```console
https://localhost:5001/fruits~ ls
.      [get|post]
..     []
{id}   [get|put|delete]

https://localhost:5001/fruits~
```

Możesz też uruchomić `ui` polecenie, aby otworzyć stronę interfejs użytkownika struktury Swagger interfejsu API sieci web w przeglądarce. Przykład:

```console
https://localhost:5001/~ ui
```

### <a name="navigate-to-an-endpoint"></a>Przejdź do punktu końcowego

Aby przejść do innego punktu końcowego na interfejs API sieci web, należy uruchomić `cd` polecenia:

```console
https://localhost:5001/~ cd people
```

Następujące ścieżki `cd` polecenia jest uwzględniana wielkość liter. Zostanie wyświetlony następujący format danych wyjściowych:

```console
/people    [get|post]

https://localhost:5001/people~
```

## <a name="customize-the-http-repl"></a>Dostosowywanie HTTP REPL

Domyślne HTTP REPL [kolory](#set-color-preferences) można dostosować. Ponadto [domyślny edytor tekstu](#set-the-default-text-editor) można zdefiniować. Preferencje HTTP REPL są utrwalane w bieżącej sesji i są uwzględniane w przyszłych sesjach. Po zmodyfikowaniu preferencje są przechowywane w następującym pliku:

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

*%HOME%/.httpreplprefs*

# <a name="macostabmacos"></a>[macOS](#tab/macos)

*%HOME%/.httpreplprefs*

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

*%USERPROFILE%\\.httpreplprefs*

---

*.Httpreplprefs* plik jest ładowany podczas uruchamiania i nie są monitorowane pod kątem zmian w czasie wykonywania. Ręcznej modyfikacje pliku obowiązywać dopiero po ponownym uruchomieniu narzędzia.

### <a name="view-the-settings"></a>Wyświetl ustawienia

Aby wyświetlić dostępnych ustawień, uruchom `pref get` polecenia. Na przykład:

```console
https://localhost:5001/~ pref get
```

Poprzednie polecenie wyświetli dostępne pary klucz wartość:

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

### <a name="set-color-preferences"></a>Ustawianie preferencji koloru

Kolorowanie odpowiedzi obecnie obsługiwany tylko dla formatu JSON. Aby dostosować domyślne narzędzie HTTP REPL, kolorowanie, zlokalizuj klucz odpowiadający kolorów, które mają być zmienione. Aby uzyskać instrukcje na temat znajdowania kluczy, zobacz [wyświetlić ustawienia](#view-the-settings) sekcji. Na przykład zmienić `colors.json` wartość z klucza `Green` do `White` w następujący sposób:

```console
https://localhost:5001/people~ pref set colors.json White
```

Tylko [dozwolone kolory](https://github.com/aspnet/HttpRepl/blob/01d5c3c3373e98fe566ff5ef8a17c571de880293/src/Microsoft.Repl/ConsoleHandling/AllowedColors.cs) mogą być używane. Kolejne HTTP żądania wyświetlania danych wyjściowych za pomocą nowego kolorowanie.

Gdy określony kolor klucze nie są ustawione, są uważane za klucze bardziej ogólnymi. Aby zademonstrować to działanie rezerwowe, rozważmy następujący przykład:

* Jeśli `colors.json.name` nie ma wartości `colors.json.string` jest używany.
* Jeśli `colors.json.string` nie ma wartości `colors.json.literal` jest używany.
* Jeśli `colors.json.literal` nie ma wartości `colors.json` jest używany. 
* Jeśli `colors.json` nie ma wartości, powłoki poleceń domyślny kolor tekstu (`AllowedColors.None`) jest używany.

### <a name="set-indentation-size"></a>Ustaw rozmiar wcięć

Dostosowywanie rozmiaru wcięcia odpowiedzi obecnie obsługiwany tylko dla formatu JSON. Domyślny rozmiar to dwa miejsca do magazynowania. Na przykład:

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

Aby zmienić domyślny rozmiar, należy ustawić `formatting.json.indentSize` klucza. Na przykład aby zawsze użycie czterech spacji:

```console
pref set formatting.json.indentSize 4
```

Kolejne odpowiedzi uwzględnić ustawienie czterech miejsc do magazynowania:

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

### <a name="set-indentation-size"></a>Ustaw rozmiar wcięć

Dostosowywanie rozmiaru wcięcia odpowiedzi obecnie obsługiwany tylko dla formatu JSON. Domyślny rozmiar to dwa miejsca do magazynowania. Przykład:

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

Aby zmienić domyślny rozmiar, należy ustawić `formatting.json.indentSize` klucza. Na przykład aby zawsze użycie czterech spacji:

```console
pref set formatting.json.indentSize 4
```

Kolejne odpowiedzi uwzględnić ustawienie czterech miejsc do magazynowania:

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

### <a name="set-the-default-text-editor"></a>Ustaw domyślny edytor tekstu

Domyślnie HTTP REPL ma nie Edytor tekstu, skonfigurowany do użycia. Aby przetestować metody interfejsu API sieci web wymaga treści żądania HTTP, można ustawić domyślnego edytora tekstu. HTTP REPL zostanie uruchomione narzędzie do edytora tekstów skonfigurowane wyłącznie w celu tworzenia treści żądania. Uruchom następujące polecenie, aby ustawić preferowanym edytorze tekstu jako domyślny:

```console
pref set editor.command.default "<EXECUTABLE>"
```

W poprzednim poleceniu `<EXECUTABLE>` jest pełną ścieżką do pliku wykonywalnego w edytorze tekstów. Na przykład uruchom następujące polecenie, aby ustawić Visual Studio Code jako domyślny edytor tekstu:

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

```console
pref set editor.command.default "/usr/bin/code"
```

# <a name="macostabmacos"></a>[macOS](#tab/macos)

```console
pref set editor.command.default "/Applications/Visual Studio Code.app/Contents/Resources/app/bin/code"
```

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

```console
pref set editor.command.default "C:\Program Files\Microsoft VS Code\Code.exe"
```

---

Aby uruchomić domyślny edytor tekstu przy użyciu określonych argumentów wiersza polecenia platformy, należy ustawić `editor.command.default.arguments` klucza. Załóżmy na przykład, Visual Studio Code to domyślny edytor tekstu i zawsze ma REPL HTTP, które można otworzyć programu Visual Studio Code w nowej sesji przy użyciu rozszerzeń wyłączone. Uruchom następujące polecenie:

```console
pref set editor.command.default.arguments "--disable-extensions --new-window"
```

## <a name="test-http-get-requests"></a>Testowanie żądania HTTP GET

### <a name="synopsis"></a>Streszczenie

```console
get <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a>Argumenty

`PARAMETER`

Parametr trasy, oczekiwany przez metodę akcji kontrolera skojarzone.

### <a name="options"></a>Opcje

Poniższe opcje są dostępne dla `get` polecenia:

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

### <a name="example"></a>Przykład

Wysłanie żądania HTTP GET:

1. Uruchom `get` polecenia w punkcie końcowym, który ją obsługuje:

    ```console
    https://localhost:5001/people~ get
    ```

    Poprzednie polecenie wyświetli następujący format danych wyjściowych:

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

1. Pobieranie określonego rekordu przez przekazanie parametru, aby `get` polecenia:

    ```console
    https://localhost:5001/people~ get 2
    ```

    Poprzednie polecenie wyświetli następujący format danych wyjściowych:

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

## <a name="test-http-post-requests"></a>Testowanie żądania HTTP POST

### <a name="synopsis"></a>Streszczenie

```console
post <PARAMETER> [-c|--content] [-f|--file] [-h|--header] [--no-body] [-F|--no-formatting] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a>Argumenty

`PARAMETER`

Parametr trasy, oczekiwany przez metodę akcji kontrolera skojarzone.

### <a name="options"></a>Opcje

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

[!INCLUDE [HTTP request body CLI options](~/includes/http-repl/requires-body-options.md)]

### <a name="example"></a>Przykład

Wysłanie żądania POST protokołu HTTP:

1. Uruchom `post` polecenia w punkcie końcowym, który ją obsługuje:

    ```console
    https://localhost:5001/people~ post -h Content-Type=application/json
    ```

    W poprzednim poleceniu `Content-Type` nagłówek żądania HTTP jest ustawiona, aby wskazać typ nośnika treści żądania JSON. Zostanie otwarty Edytor tekstu domyślne *.tmp* pliku przy użyciu szablonu JSON reprezentująca treść żądania HTTP. Przykład:

    ```json
    {
      "id": 0,
      "name": ""
    }
    ```

    > [!TIP]
    > Aby ustawić domyślny edytor tekstu, zobacz [ustawiony domyślny edytor tekstu](#set-the-default-text-editor) sekcji.

1. Zmodyfikuj szablon JSON w celu spełnienia wymagań weryfikacji modelu:

  ```json
  {
    "id": 0,
    "name": "Scott Addie"
  }
  ```

1. Zapisz *.tmp* plik i zamknij Edytor tekstu. Następujące dane wyjściowe pojawia się w powłoce poleceń:

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

## <a name="test-http-put-requests"></a>Testowanie żądania HTTP PUT

### <a name="synopsis"></a>Streszczenie

```console
put <PARAMETER> [-c|--content] [-f|--file] [-h|--header] [--no-body] [-F|--no-formatting] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a>Argumenty

`PARAMETER`

Parametr trasy, oczekiwany przez metodę akcji kontrolera skojarzone.

### <a name="options"></a>Opcje

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

[!INCLUDE [HTTP request body CLI options](~/includes/http-repl/requires-body-options.md)]

### <a name="example"></a>Przykład

Wysłanie żądania HTTP PUT:

1. *Opcjonalnie*: Uruchom `get` polecenie, aby wyświetlić dane przed zmodyfikowaniem:

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

    W poprzednim poleceniu `Content-Type` nagłówek żądania HTTP jest ustawiona, aby wskazać typ nośnika treści żądania JSON. Zostanie otwarty Edytor tekstu domyślne *.tmp* pliku przy użyciu szablonu JSON reprezentująca treść żądania HTTP. Przykład:

    ```json
    {
      "id": 0,
      "name": ""
    }
    ```

    > [!TIP]
    > Aby ustawić domyślny edytor tekstu, zobacz [ustawiony domyślny edytor tekstu](#set-the-default-text-editor) sekcji.

1. Zmodyfikuj szablon JSON w celu spełnienia wymagań weryfikacji modelu:

    ```json
    {
      "id": 2,
      "name": "Cherry"
    }
    ```

1. Zapisz *.tmp* plik i zamknij Edytor tekstu. Następujące dane wyjściowe pojawia się w powłoce poleceń:

    ```console
    [main 2019-06-28T17:27:01.805Z] update#setState idle
    HTTP/1.1 204 No Content
    Date: Fri, 28 Jun 2019 17:28:21 GMT
    Server: Kestrel
    ```

1. *Opcjonalnie*: Problem `get` polecenie, aby wyświetlić modyfikacje. Na przykład, jeśli wpiszesz "Wykonywanie operacji Cherry" w edytorze tekstów `get` zwraca następujące:

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

## <a name="test-http-delete-requests"></a>Testowanie żądania HTTP DELETE

### <a name="synopsis"></a>Streszczenie

```console
delete <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a>Argumenty

`PARAMETER`

Parametr trasy, oczekiwany przez metodę akcji kontrolera skojarzone.

### <a name="options"></a>Opcje

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

### <a name="example"></a>Przykład

Wysłanie żądania HTTP DELETE:

1. *Opcjonalnie*: Uruchom `get` polecenie, aby wyświetlić dane przed zmodyfikowaniem:

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

    Poprzednie polecenie wyświetli następujący format danych wyjściowych:

    ```console
    HTTP/1.1 204 No Content
    Date: Fri, 28 Jun 2019 17:36:42 GMT
    Server: Kestrel
    ```

1. *Opcjonalnie*: Problem `get` polecenie, aby wyświetlić modyfikacje. W tym przykładzie `get` zwraca następujące:

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

## <a name="test-http-patch-requests"></a>Testowanie żądania HTTP PATCH

### <a name="synopsis"></a>Streszczenie

```console
patch <PARAMETER> [-c|--content] [-f|--file] [-h|--header] [--no-body] [-F|--no-formatting] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a>Argumenty

`PARAMETER`

Parametr trasy, oczekiwany przez metodę akcji kontrolera skojarzone.

### <a name="options"></a>Opcje

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

[!INCLUDE [HTTP request body CLI options](~/includes/http-repl/requires-body-options.md)]

## <a name="test-http-head-requests"></a>Testowanie żądania HTTP HEAD

### <a name="synopsis"></a>Streszczenie

```console
head <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a>Argumenty

`PARAMETER`

Parametr trasy, oczekiwany przez metodę akcji kontrolera skojarzone.

### <a name="options"></a>Opcje

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

## <a name="test-http-options-requests"></a>Testowanie żądania OPTIONS protokołu HTTP

### <a name="synopsis"></a>Streszczenie

```console
options <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a>Argumenty

`PARAMETER`

Parametr trasy, oczekiwany przez metodę akcji kontrolera skojarzone.

### <a name="options"></a>Opcje

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

## <a name="set-http-request-headers"></a>Ustawia nagłówki żądania HTTP

Aby ustawić nagłówek żądania HTTP, użyj jednej z następujących metod:

1. Wbudowany zestaw z żądania HTTP. Przykład:

  ```console
  https://localhost:5001/people~ post -h Content-Type=application/json
  ```

  Z poprzednim przypadku każdego distinct nagłówek żądania HTTP wymaga własnej `-h` opcji.

1. Ustaw przed wysłaniem żądania HTTP. Na przykład:

  ```console
  https://localhost:5001/people~ set header Content-Type application/json
  ```

  Podczas ustawiania nagłówka przed wysłaniem żądania, nagłówek pozostanie ustawiony na czas trwania sesji powłoki poleceń. Aby usunąć nagłówek, podaj wartość pustą. Na przykład:

  ```console
  https://localhost:5001/people~ set header Content-Type
  ```

## <a name="toggle-http-request-display"></a>Przełącz wyświetlanie żądania HTTP

Domyślnie pomijane jest wyświetlana wysłanie żądania HTTP. Istnieje możliwość Zmień odpowiednie ustawienia na czas trwania sesji powłoki poleceń.

### <a name="enable-request-display"></a>Włącz wyświetlanie żądania

Wyświetl żądania HTTP są wysyłane przez uruchomienie `echo on` polecenia. Na przykład:

```console
https://localhost:5001/people~ echo on
Request echoing is on
```

Kolejne żądania HTTP w bieżącej sesji, wyświetlanie nagłówków żądania. Na przykład:

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

### <a name="disable-request-display"></a>Wyłącz wyświetlanie żądania

Pomija wyświetlanie żądania HTTP są wysyłane przez uruchomienie `echo off` polecenia. Przykład:

```console
https://localhost:5001/people~ echo off
Request echoing is off
```

## <a name="run-a-script"></a>Uruchamianie skryptu

Jeśli ten sam zestaw poleceń HTTP REPL jest często wykonywane, należy wziąć pod uwagę przechowywania ich w pliku tekstowym. Polecenia w pliku formę ten sam jak wykonywane ręcznie, w wierszu polecenia. Polecenia mogą być wykonywane w sposób wsadowej za pomocą `run` polecenia. Przykład:

1. Utwórz plik tekstowy zawierający zestaw poleceń rozdzielonych znakami nowego wiersza. Aby zilustrować, należy wziąć pod uwagę *script.txt osób* plik zawierający następujące polecenia:

    ```text
    set base https://localhost:5001
    ls
    cd People
    ls
    get 1
    ```

1. Wykonaj `run` polecenia, przekazując ścieżkę do pliku tekstowego. Na przykład:

    ```console
    https://localhost:5001/~ run C:\http-repl-scripts\people-script.txt
    ```

    Zostanie wyświetlone następujące dane wyjściowe:

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

## <a name="clear-the-output"></a>Wyczyść dane wyjściowe

Aby usunąć wszystkich danych wyjściowych zapisywanych powłoki poleceń narzędzia HTTP REPL, uruchom `clear` lub `cls` polecenia. Aby zilustrować, załóżmy, że powłoka poleceń zawiera następujące dane wyjściowe:

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

Uruchom następujące polecenie, aby wyczyścić dane wyjściowe:

```console
https://localhost:5001/~ clear
```

Po uruchomieniu poprzedniego polecenia, powłokę poleceń zawiera tylko następujące dane wyjściowe:

```console
https://localhost:5001/~
```

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Żądań interfejsu API REST](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#74-supported-methods)
* [Repozytorium HTTP REPL GitHub](https://github.com/aspnet/HttpRepl)
