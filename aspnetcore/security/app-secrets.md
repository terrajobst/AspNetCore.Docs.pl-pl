---
title: Bezpieczne przechowywanie kluczy tajnych aplikacji w rozwoju platformy ASP.NET Core
author: rick-anderson
description: Sposób przechowywania i pobierania informacji poufnych jako klucze tajne aplikacji podczas tworzenia aplikacji platformy ASP.NET Core.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 05/16/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/app-secrets
ms.openlocfilehash: 88b4ee9a963543f8cc97cb66271628a14fe657de
ms.sourcegitcommit: 3a893ae05f010656d99d6ddf55e82f1b5b6933bc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/18/2018
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a>Bezpieczne przechowywanie kluczy tajnych aplikacji w rozwoju platformy ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT), [Roth Danielowi](https://github.com/danroth27), i [Scott Addie](https://github.com/scottaddie)

::: moniker range="<= aspnetcore-1.1"
[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples/1.1) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples/2.1) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))
::: moniker-end

W tym dokumencie opisano techniki do przechowywania i pobierania danych poufnych podczas tworzenia aplikacji platformy ASP.NET Core. W kodzie źródłowym nigdy nie należy przechowywać haseł i innych poufnych danych, a nie powinien użyć kluczy tajnych produkcji w rozwoju lub testowania trybu. Można przechowywać i chronić Azure kluczy tajnych testowych i produkcyjnych z [dostawcę konfiguracji usługi Azure Key Vault](xref:security/key-vault-configuration).

## <a name="environment-variables"></a>Zmienne środowiskowe

Zmienne środowiskowe są używane do Rezygnacja z zapisywania haseł aplikacji, kodu lub lokalne pliki konfiguracji. Zmienne środowiskowe przesłaniają wartości konfiguracji dla wszystkich źródeł wcześniej określonej konfiguracji.

::: moniker range="<= aspnetcore-1.1"
Konfigurowanie przy odczycie wartości zmiennych środowiskowych, wywołując [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) w `Startup` konstruktora:

[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=10)]
::: moniker-end

Należy wziąć pod uwagę aplikacji sieci web platformy ASP.NET Core, w którym **indywidualnych kont użytkowników** zabezpieczeń jest włączone. Domyślny ciąg połączenia bazy danych jest dołączony do projektu *appsettings.json* pliku z kluczem `DefaultConnection`. Domyślny ciąg połączenia jest dla bazy danych LocalDB, która działa w trybie użytkownika i nie wymaga hasła. Podczas wdrażania aplikacji `DefaultConnection` wartość klucza może zostać zastąpiona przez wartość zmiennej środowiskowej. Zmienna środowiskowa może przechowywać parametry połączenia ukończone z poświadczeniami poufnych.

> [!WARNING]
> Zmienne środowiskowe są zazwyczaj przechowywane w zwykły tekst niezaszyfrowane. W przypadku naruszenia zabezpieczeń komputera lub proces, zmienne środowiskowe są dostępne przez niezaufane. Może być wymagane dodatkowe środki, aby zapobiec ujawnieniu hasła użytkownika.

## <a name="secret-manager"></a>Menedżer klucz tajny

Narzędzie Menedżer klucz tajny są przechowywane poufne dane podczas tworzenia projektu platformy ASP.NET Core. W tym kontekście element danych poufnych jest klucz tajny aplikacji. Klucze tajne aplikacji są przechowywane w innej lokalizacji niż drzewa projektu. Klucze tajne aplikacji są skojarzone z określonego projektu lub współużytkowane przez kilka projektów. Klucze tajne aplikacji nie jest wyewidencjonowany do kontroli źródła.

> [!WARNING]
> Narzędzie Menedżer klucz tajny nie Szyfruj przechowywane klucze tajne i nie powinny być traktowane jako zaufanego magazynu. Jest tylko do celów programistycznych. Klucze i wartości są przechowywane w pliku konfiguracji JSON w katalogu profilu użytkownika.

## <a name="how-the-secret-manager-tool-works"></a>Jak działa narzędzie Menedżer klucz tajny

Narzędzie Menedżer klucz tajny optymalizacji abstracts szczegóły implementacji, takich jak jak i gdzie są przechowywane wartości. Można użyć narzędzia bez uprzedniego uzyskania informacji o tych szczegóły implementacji. Wartości są przechowywane w [JSON](https://json.org/) pliku konfiguracji w folderze profilu użytkownika chronionych przez system na komputerze lokalnym:

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

W poprzednim ścieżki pliku, Zastąp `<user_secrets_id>` z `UserSecretsId` wartości określonej w *.csproj* pliku.

Nie pisania kodu, który jest zależny od lokalizacji lub format dane zapisane za pomocą narzędzia menedżera klucz tajny. Te szczegóły implementacji mogą ulec zmianie. Na przykład tajny wartości nie są zaszyfrowane, ale może być w przyszłości.

::: moniker range="<= aspnetcore-2.0"
## <a name="install-the-secret-manager-tool"></a>Zainstaluj narzędzie Menedżer klucz tajny

Narzędzie Menedżer klucz tajny jest dołączany .NET Core interfejsu wiersza polecenia w .NET Core SDK 2.1. Dla platformy .NET Core SDK 2.0 i starszych wersji niezbędne jest narzędzie instalacji.

Zainstaluj [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) pakietu NuGet w projekcie platformy ASP.NET Core:

[!code-xml[](app-secrets/samples/1.1/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=13-14)]

Uruchom następujące polecenie w powłoce poleceń, aby sprawdzić poprawność instalacji narzędzia:

```console
dotnet user-secrets -h
```

Narzędzie Menedżer klucz tajny zawiera przykładowe zastosowanie, opcje i pomoc dla polecenia:

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
> Musi być w tym samym katalogu co *.csproj* uruchamianie narzędzia zdefiniowane w pliku *.csproj* pliku `DotNetCliToolReference` elementów.
::: moniker-end

## <a name="set-a-secret"></a>Ustaw klucz tajny

Narzędzie Menedżer klucz tajny działa na ustawienia konfiguracji określonego projektu przechowywane w profilu użytkownika. Aby użyć hasła użytkownika, należy zdefiniować `UserSecretsId` w elemencie `PropertyGroup` z *.csproj* pliku. Wartość `UserSecretsId` jest określana, ale jest unikatowy dla projektu. Deweloperzy zazwyczaj generowania identyfikatora GUID dla `UserSecretsId`.

::: moniker range="<= aspnetcore-1.1"
[!code-xml[](app-secrets/samples/1.1/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
[!code-xml[](app-secrets/samples/2.1/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]
::: moniker-end

> [!TIP]
> W programie Visual Studio, kliknij prawym przyciskiem myszy projekt w Eksploratorze rozwiązań i wybierz **Zarządzanie kluczy tajnych użytkownika** z menu kontekstowego. Dodaje ten gest `UserSecretsId` element wypełniać za pomocą identyfikatora GUID do *.csproj* pliku. Visual Studio otworzy *secrets.json* plik w edytorze tekstu. Zastąp zawartość *secrets.json* z pary klucz wartość do zapisania. Na przykład: [!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file.md)]

Zdefiniuj klucz tajny aplikacji składający się z klucza i wartości. Klucz tajny jest skojarzony z projektem `UserSecretsId` wartość. Na przykład, uruchom następujące polecenie z katalogu, w którym *.csproj* plik istnieje:

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

W poprzednim przykładzie, dwukropek, oznacza, że `Movies` jest obiektem literału z `ServiceApiKey` właściwości.

Narzędzie Menedżer klucz tajny może służyć za z innych katalogów. Użyj `--project` opcję, aby podać ścieżka systemu plików, w którym *.csproj* plik istnieje. Na przykład:

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

## <a name="set-multiple-secrets"></a>Ustawianie wielu kluczy tajnych

Można ustawić partii kluczy tajnych przez przekazanie w potoku JSON do `set` polecenia. W poniższym przykładzie *input.json* jego zawartość jest przetwarzana potokowo do `set` polecenia.

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

Otwórz powłokę poleceń, a następnie uruchom następujące polecenie:

  ```console
  type .\input.json | dotnet user-secrets set
  ```

# <a name="macostabmacos"></a>[macOS](#tab/macos)

Otwórz powłokę poleceń, a następnie uruchom następujące polecenie:

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

Otwórz powłokę poleceń, a następnie uruchom następujące polecenie:

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a>Dostęp do klucza tajnego

[Interfejsu API platformy ASP.NET Core](xref:fundamentals/configuration/index) zapewnia dostęp do kluczy tajnych Menedżera klucz tajny. Jeśli przeznaczonych dla platformy .NET Core 1.x lub .NET Framework, zainstaluj [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) pakietu NuGet.

::: moniker range="<= aspnetcore-1.1"
Dodaj użytkownika kluczy tajnych konfiguracji źródła `Startup` konstruktora:

[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]
::: moniker-end

Klucze tajne użytkownika mogą zostać pobrane za pośrednictwem `Configuration` interfejsu API:

::: moniker range="<= aspnetcore-1.1"
[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=23)]
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
[!code-csharp[](app-secrets/samples/2.1/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]
::: moniker-end

## <a name="string-replacement-with-secrets"></a>Ciąg zastępczy z kluczy tajnych

Przechowywanie haseł w postaci zwykłego tekstu jest ryzykowne. Na przykład ciąg połączenia bazy danych są przechowywane w *appsettings.json* może zawierać hasła dla określonego użytkownika:

[!code-json[](app-secrets/samples/2.1/UserSecrets/appsettings-unsecure.json?highlight=3)]

Bardziej bezpieczną metodą jest, aby zapisać hasło jako klucz tajny. Na przykład:

```console
dotnet user-secrets set "DbPassword" "pass123"
```

Zamień hasło w *appsettings.json* symbol zastępczy. W poniższym przykładzie `{0}` jest używany jako symbol zastępczy do formularza [złożonego ciąg formatu](/dotnet/standard/base-types/composite-formatting#composite-format-string).

[!code-json[](app-secrets/samples/2.1/UserSecrets/appsettings.json?highlight=3)]

Wartość klucza tajnego mogą zostać dodane do symbolu zastępczego, aby ukończyć parametry połączenia:

::: moniker range="<= aspnetcore-1.1"
[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=23-25)]
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
[!code-csharp[](app-secrets/samples/2.1/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-16)]
::: moniker-end

## <a name="list-the-secrets"></a>Lista kluczy tajnych

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

Uruchom następujące polecenie z katalogu, w którym *.csproj* plik istnieje:

```console
dotnet user-secrets list
```

Zostaną wyświetlone następujące dane wyjściowe:

```console
Movies:ServiceApiKey = 12345
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
```

W poprzednim przykładzie, dwukropek w nazwach kluczy oznacza hierarchii obiektów w ramach *secrets.json*.

## <a name="remove-a-single-secret"></a>Usuń jeden klucz tajny

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

Uruchom następujące polecenie z katalogu, w którym *.csproj* plik istnieje:

```console
dotnet user-secrets remove "Movies:ConnectionString"
```

Aplikacji *secrets.json* plik został zmodyfikowany, aby usunąć parę klucz wartość skojarzonych z `MoviesConnectionString` klucza:

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

Uruchomiona `dotnet user-secrets list` wyświetli następujący komunikat:

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a>Usunięcie wszystkich kluczy tajnych

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

Uruchom następujące polecenie z katalogu, w którym *.csproj* plik istnieje:

```console
dotnet user-secrets clear
```

Usunięto wszystkie hasła użytkownika dla aplikacji z *secrets.json* pliku:

```json
{}
```

Uruchomiona `dotnet user-secrets list` wyświetli następujący komunikat:

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>