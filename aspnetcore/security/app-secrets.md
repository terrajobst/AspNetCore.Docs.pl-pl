---
title: Bezpieczne przechowywanie kluczy tajnych aplikacji w trakcie opracowywania w programie ASP.NET Core
author: rick-anderson
description: Informacje o sposobie przechowywania i pobierania informacji poufnych jako wpisy tajne aplikacji podczas tworzenia aplikacji ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 09/24/2018
uid: security/app-secrets
ms.openlocfilehash: 1ecd9b87b9e4aa2c511349fe407c0cea9dc9f921
ms.sourcegitcommit: 4d5f8680d68b39c411b46c73f7014f8aa0f12026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47028274"
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a>Bezpieczne przechowywanie kluczy tajnych aplikacji w trakcie opracowywania w programie ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), i [Scott Addie](https://github.com/scottaddie)

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

W tym dokumencie opisano techniki do przechowywania i pobierania danych poufnych podczas tworzenia aplikacji ASP.NET Core. Nigdy nie przechowuj hasła lub innych danych poufnych w kodzie źródłowym. Wpisy tajne w środowisku produkcyjnym nie powinny być używane do projektowania lub testowania. Możesz przechowywać i chronić Azure testowych i produkcyjnych wpisów tajnych za pomocą [dostawcę konfiguracji usługi Azure Key Vault](xref:security/key-vault-configuration).

## <a name="environment-variables"></a>Zmienne środowiskowe

Zmienne środowiskowe są używane w celu uniknięcia przechowywanie kluczy tajnych aplikacji w kodzie lub w plikach konfiguracji lokalnej. Zmienne środowiskowe zastępować wartości konfiguracji dla wszystkich źródeł wcześniej określonej konfiguracji.

::: moniker range="<= aspnetcore-1.1"

Konfigurowanie odczytywania wartości zmiennych środowiskowych, wywołując [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) w `Startup` Konstruktor:

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=8)]

::: moniker-end

Należy wziąć pod uwagę aplikację internetową platformy ASP.NET Core, w którym **indywidualne konta użytkowników** zabezpieczenia są włączone. Domyślne parametry połączenia bazy danych znajduje się w projekcie *appsettings.json* pliku przy użyciu klucza `DefaultConnection`. Domyślny ciąg połączenia jest do LocalDB, który działa w trybie użytkownika i nie wymaga hasła. Podczas wdrażania aplikacji `DefaultConnection` wartość klucza może zostać zastąpiona przez wartość zmiennej środowiskowej. Zmienna środowiskowa mogą być przechowywane pełne parametry połączenia przy użyciu poświadczeń poufnych.

> [!WARNING]
> Zmienne środowiskowe są zazwyczaj przechowywane w zwykły tekst niezaszyfrowane. W przypadku naruszenia zabezpieczeń komputera lub proces, dostęp do zmiennych środowiskowych jest możliwy przez niezaufane. Dodatkowe środki, aby zapobiec ujawnieniu wpisami tajnymi użytkowników mogą być wymagane.

## <a name="secret-manager"></a>Menedżer wpisu tajnego

Narzędzie Menedżer klucz tajny są przechowywane poufne dane podczas tworzenia projektu ASP.NET Core. W tym kontekście część danych poufnych jest wpis tajny aplikacji. Wpisy tajne aplikacji są przechowywane w innej lokalizacji w drzewie projektu. Wpisy tajne aplikacji są skojarzone z określonym projektem lub udostępnione w kilku projektach. Wpisy tajne aplikacji nie są sprawdzane w systemie kontroli źródła.

> [!WARNING]
> Narzędzie Menedżer wpis tajny nie szyfruje przechowywane klucze tajne i nie powinien być traktowany jako zaufany magazyn. Jest tylko do celów programowania. Klucze i wartości są przechowywane w pliku konfiguracji JSON w katalogu profilu użytkownika.

## <a name="how-the-secret-manager-tool-works"></a>Jak działa narzędzie Menedżer wpisu tajnego

Narzędzie Menedżer klucz tajny ukrywa szczegóły implementacji, takich jak gdzie i w jaki sposób wartości są przechowywane. Narzędzie nie znając te szczegóły implementacji. Wartości są przechowywane w pliku konfiguracji JSON w folderze profilu użytkownika chronionych przez system na komputerze lokalnym:

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

Ścieżka systemu plików:

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="macostabmacos"></a>[macOS](#tab/macos)

Ścieżka systemu plików:

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

Ścieżka systemu plików:

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

W poprzednim ścieżki plików, Zastąp `<user_secrets_id>` z `UserSecretsId` wartości określonej w *.csproj* pliku.

Nie pisz kod, który zależy od lokalizacji lub formatu danych zapisanych przy użyciu narzędzia menedżera klucz tajny. Szczegóły tych implementacji mogą ulec zmianie. Na przykład klucza tajnego wartości nie są szyfrowane, ale może być w przyszłości.

::: moniker range="<= aspnetcore-2.0"

## <a name="install-the-secret-manager-tool"></a>Zainstaluj narzędzie Menedżer wpisu tajnego

Narzędzie Menedżer klucz tajny jest powiązane z platformy .NET Core interfejsu wiersza polecenia w zestawie SDK programu .NET Core 2.1.300 lub nowszej. Wersje programu .NET Core SDK przed 2.1.300 niezbędne jest narzędzie instalacji.

> [!TIP]
> Uruchom `dotnet --version` z powłoki poleceń, aby wyświetlić numer wersji zainstalowanego zestawu .NET Core SDK.

Zostanie wyświetlone ostrzeżenie, jeśli używany zestaw .NET Core SDK zawiera narzędzia:

```console
The tool 'Microsoft.Extensions.SecretManager.Tools' is now included in the .NET Core SDK. Information on resolving this warning is available at (https://aka.ms/dotnetclitools-in-box).
```

Zainstaluj [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) pakietu NuGet w projekcie platformy ASP.NET Core. Na przykład:

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=15-16)]

Wykonaj następujące polecenie w powłoce poleceń, aby sprawdzić poprawność instalacji narzędzia:

```console
dotnet user-secrets -h
```

Narzędzie Menedżer klucz tajny Wyświetla przykładowe zastosowanie i opcje pomocy dla poleceń:

```console
Usage: dotnet user-secrets [options] [command]

Options:
  -?|-h|--help                        Show help information
  --version                           Show version information
  -v|--verbose                        Show verbose output
  -p|--project <PROJECT>              Path to project. Defaults to searching the current directory.
  -c|--configuration <CONFIGURATION>  The project configuration to use. Defaults to 'Debug'.
  --id                                The user secret ID to use.

Commands:
  clear   Deletes all the application secrets
  list    Lists all the application secrets
  remove  Removes the specified user secret
  set     Sets the user secret to the specified value

Use "dotnet user-secrets [command] --help" for more information about a command.
```

> [!NOTE]
> Musi być w tym samym katalogu co *.csproj* uruchamianie narzędzi zdefiniowane w pliku *.csproj* pliku `DotNetCliToolReference` elementów.

::: moniker-end

## <a name="set-a-secret"></a>Ustaw klucz tajny

Narzędzie Menedżer klucz tajny działa na ustawień konfiguracji specyficznych dla projektu, przechowywane w profilu użytkownika. Aby korzystać z wpisami tajnymi użytkowników, należy zdefiniować `UserSecretsId` elemencie `PropertyGroup` z *.csproj* pliku. Wartość `UserSecretsId` jest określana, ale jest unikatowy dla projektu. Deweloperzy zazwyczaj Wygeneruj identyfikator GUID dla `UserSecretsId`.

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

> [!TIP]
> W programie Visual Studio, kliknij prawym przyciskiem myszy projekt w Eksploratorze rozwiązań i wybierz **zarządzania wpisami tajnymi użytkowników** z menu kontekstowego. Dodaje ten gest `UserSecretsId` elementu wypełniane przy użyciu identyfikatora GUID do *.csproj* pliku. Zostanie otwarty program Visual Studio *secrets.json* plik w edytorze tekstów. Zastąp zawartość *secrets.json* za pomocą pary klucz wartość, która ma być przechowywany. Na przykład:
> ```json
> {
>   "Movies": {
>     "ConnectionString": "Server=(localdb)\\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true",
>     "ServiceApiKey": "12345"
>   }
> }
> ```
> Struktura JSON jest spłaszczany po modyfikacji za pośrednictwem `dotnet user-secrets remove` lub `dotnet user-secrets set`. Aby na przykład uruchomić `dotnet user-secrets remove "Movies:ConnectionString"` zwija `Movies` literału obiektu. Zmodyfikowany plik jest podobny do następującego:
> ```json
> {
>   "Movies:ServiceApiKey": "12345"
> }
> ```

Zdefiniuj klucz tajny aplikacji składających się z klucza i wartości. Klucz tajny jest skojarzony z projektem `UserSecretsId` wartość. Na przykład, uruchom następujące polecenie z katalogu, w którym *.csproj* istnieje plik:

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

W poprzednim przykładzie, dwukropek, oznacza, że `Movies` jest obiektem literału z `ServiceApiKey` właściwości.

Narzędzie Menedżer klucz tajny może służyć za z innych katalogów. Użyj `--project` opcję, aby podać ścieżka systemu plików, w którym *.csproj* plik istnieje. Na przykład:

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

## <a name="set-multiple-secrets"></a>Ustawianie wielu wpisów tajnych

Seria wpisów tajnych można ustawić przez przekazanie w potoku JSON do `set` polecenia. W poniższym przykładzie *input.json* zawartość tego pliku są potokiem do `set` polecenia.

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

Otwórz powłokę wiersza polecenia, a następnie wykonaj następujące polecenie:

  ```console
  type .\input.json | dotnet user-secrets set
  ```

# <a name="macostabmacos"></a>[macOS](#tab/macos)

Otwórz powłokę wiersza polecenia, a następnie wykonaj następujące polecenie:

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

Otwórz powłokę wiersza polecenia, a następnie wykonaj następujące polecenie:

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a>Dostęp do klucza tajnego

::: moniker range=">= aspnetcore-2.0"

[Interfejsu API platformy ASP.NET Core](xref:fundamentals/configuration/index) zapewnia dostęp do kluczy tajnych Menedżera klucz tajny. Jeśli projekt jest przeznaczony dla .NET Framework, należy zainstalować [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) pakietu NuGet.

W programie ASP.NET Core 2.0 lub nowszej, źródło konfiguracji kluczy tajnych użytkownika zostanie automatycznie dodana w trybie projektowania, przy projektu wywołuje [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) zainicjować nowe wystąpienie hosta przy użyciu wstępnie skonfigurowanych ustawień domyślnych. `CreateDefaultBuilder` wywołania [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) podczas [EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname) jest [rozwoju](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development):

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Program.cs?name=snippet_CreateWebHostBuilder&highlight=2)]

Gdy `CreateDefaultBuilder` nie jest wywoływana podczas konstruowania hosta, Dodaj źródło konfiguracji wpisami tajnymi użytkowników przy użyciu wywołania do [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) w `Startup` Konstruktor:

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[Interfejsu API platformy ASP.NET Core](xref:fundamentals/configuration/index) zapewnia dostęp do kluczy tajnych Menedżera klucz tajny. Zainstaluj [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) pakietu NuGet.

Dodawanie źródła konfiguracji kluczy tajnych użytkownika z wywołaniem [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) w `Startup` Konstruktor:

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

Wpisami tajnymi użytkowników mogą być pobierane za pośrednictwem `Configuration` interfejsu API:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=26)]

::: moniker-end

## <a name="map-secrets-to-a-poco"></a>Klucze tajne mapy POCO

Mapowanie całego literału obiektu do POCO (.NET klasą prostą z właściwościami) jest przydatne w przypadku agregowania powiązane właściwości.

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

Aby mapować POCO poprzednich wpisów tajnych, użyj `Configuration` interfejsów API [obiektu powiązania wykres](xref:fundamentals/configuration/index#bind-to-an-object-graph) funkcji. Poniższy kod tworzy powiązanie z niestandardowego `MovieSettings` POCO i uzyskuje dostęp do `ServiceApiKey` wartość właściwości:

::: moniker range=">= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

::: moniker range="= aspnetcore-1.0"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

`Movies:ConnectionString` i `Movies:ServiceApiKey` wpisów tajnych są mapowane do odpowiednich właściwości w `MovieSettings`:

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Models/MovieSettings.cs?name=snippet_MovieSettingsClass)]

## <a name="string-replacement-with-secrets"></a>Zastąpienie ciągu przy użyciu kluczy tajnych

Przechowywanie haseł w postaci zwykłego tekstu jest niezabezpieczona. Na przykład parametry połączenia bazy danych są przechowywane w *appsettings.json* może zawierać hasła dla określonego użytkownika:

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings-unsecure.json?highlight=3)]

Bardziej bezpieczną metodą jest w celu zapisania hasła jako klucz tajny. Na przykład:

```console
dotnet user-secrets set "DbPassword" "pass123"
```

Usuń `Password` pary klucz wartość z parametrów połączenia w *appsettings.json*. Na przykład:

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings.json?highlight=3)]

Klucz tajny, wartość można ustawić na [SqlConnectionStringBuilder](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder) obiektu [hasło](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder.password) właściwości, aby ukończyć parametry połączenia:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-17)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=26-29)]

::: moniker-end

## <a name="list-the-secrets"></a>Wyświetl klucze tajne

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

Uruchom następujące polecenie z katalogu, w którym *.csproj* istnieje plik:

```console
dotnet user-secrets list
```

Zostanie wyświetlone następujące dane wyjściowe:

```console
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
Movies:ServiceApiKey = 12345
```

W poprzednim przykładzie, dwukropek w nazwach kluczy oznacza hierarchię obiektów w ramach *secrets.json*.

## <a name="remove-a-single-secret"></a>Usunąć pojedynczy wpis tajny

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

Uruchom następujące polecenie z katalogu, w którym *.csproj* istnieje plik:

```console
dotnet user-secrets remove "Movies:ConnectionString"
```

Aplikacja *secrets.json* plik został zmodyfikowany, aby usunąć pary klucz wartość skojarzonych z `MoviesConnectionString` klucza:

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

Uruchamianie `dotnet user-secrets list` wyświetla następujący komunikat:

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a>Usuwanie wszystkich wpisów tajnych

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

Uruchom następujące polecenie z katalogu, w którym *.csproj* istnieje plik:

```console
dotnet user-secrets clear
```

Wszystkie wpisami tajnymi użytkowników dla aplikacji został usunięty z *secrets.json* pliku:

```json
{}
```

Uruchamianie `dotnet user-secrets list` wyświetla następujący komunikat:

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>
