---
title: Bezpieczne przechowywanie kluczy tajnych aplikacji w czasie opracowywania w ASP.NET Core
author: rick-anderson
description: "Pokazuje, jak bezpiecznie przechowywać klucze tajne podczas tworzenia"
manager: wpickett
ms.author: riande
ms.date: 09/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/app-secrets
ms.openlocfilehash: 489c53c066af87e02e43ab0b42b0712d80d5ee5a
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/02/2018
---
# <a name="safe-storage-of-app-secrets-during-development-in-aspnet-core"></a>Bezpieczne przechowywanie kluczy tajnych aplikacji w czasie opracowywania w ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT), [Roth Danielowi](https://github.com/danroth27), i [Scott Addie](https://scottaddie.com) 

Ten dokument zawiera, jak można użyć narzędzia menedżera klucza tajnego do rozwoju przechowywania kluczy tajnych poza swój kod. Najważniejsze punkt znajduje się w kodzie źródłowym nigdy nie należy przechowywać haseł i innych poufnych danych i kluczy tajnych w środowisku produkcyjnym nie można używać w trybie projektowania i testowania. Zamiast tego można użyć [konfiguracji](xref:fundamentals/configuration/index) systemu, aby odczytać te wartości zmiennych środowiskowych lub z wartości przechowywane przy użyciu Menedżera klucz tajny narzędzia. Narzędzie Menedżer klucz tajny zapobiega danych poufnych jest wyewidencjonowany do kontroli źródła. [Konfiguracji](xref:fundamentals/configuration/index) systemu można odczyt kluczy tajnych przechowywane za pomocą narzędzia menedżera klucz tajny opisane w tym artykule.

Narzędzie Menedżer klucz tajny jest używana tylko programowanie. Można chronić Azure kluczy tajnych testowych i produkcyjnych z [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) dostawcę konfiguracji. Zobacz [dostawcę konfiguracji usługi Azure Key Vault](https://docs.microsoft.com/aspnet/core/security/key-vault-configuration) Aby uzyskać więcej informacji.

## <a name="environment-variables"></a>Zmienne środowiskowe

Aby uniknąć przechowywania klucze tajne aplikacji, kodu lub lokalne pliki konfiguracji, kluczy tajnych są przechowywane w zmiennych środowiskowych. Możesz skonfigurować [konfiguracji](xref:fundamentals/configuration/index) framework odczytywać wartości zmiennych środowiskowych przez wywołanie metody `AddEnvironmentVariables`. Następnie można używać zmiennych środowiskowych, aby zastąpić wartości konfiguracji dla wszystkich źródeł wcześniej określonej konfiguracji.

Na przykład w przypadku utworzenia nowej aplikacji sieci web platformy ASP.NET Core z indywidualne konta użytkowników, spowoduje to dodanie domyślnego ciągu połączenia do *appsettings.json* plik w projekcie z kluczem `DefaultConnection`. Domyślny ciąg połączenia jest skonfigurowana do używania bazy danych LocalDB, która działa w trybie użytkownika i nie wymaga hasła. Podczas wdrażania aplikacji na serwerze testowym lub produkcyjnym można zastąpić `DefaultConnection` wartość klucza z ustawienia zmiennej środowiskowej, zawierający parametry połączenia (potencjalnie o poufnych poświadczeń) w bazie danych testowym lub produkcyjnym serwer.

>[!WARNING]
> Zmienne środowiskowe są zwykle przechowywane w postaci zwykłego tekstu i nie są szyfrowane. W przypadku naruszenia zabezpieczeń komputera lub proces, następnie zmienne środowiskowe są dostępne przez niezaufane. Dodatkowe środki, aby zapobiec ujawnieniu kluczy tajnych użytkownik nadal może być wymagane.

## <a name="secret-manager"></a>Menedżer klucz tajny

Narzędzie Menedżer klucz tajny są przechowywane poufne dane w projektach poza drzewa Twojego projektu. Narzędzie Menedżer klucz tajny jest narzędzie projektu, który może służyć do przechowywania kluczy tajnych dla [.NET Core](https://www.microsoft.com/net/core) projektu w czasie projektowania. Za pomocą narzędzia menedżera klucz tajny można skojarzyć klucze tajne aplikacji z określonego projektu i udostępniać je w wielu projektach.

>[!WARNING]
> Narzędzie Menedżer klucz tajny nie Szyfruj przechowywane klucze tajne i nie powinny być traktowane jako zaufanego magazynu. Jest tylko do celów programistycznych. Klucze i wartości są przechowywane w pliku konfiguracji JSON w katalogu profilu użytkownika.

## <a name="installing-the-secret-manager-tool"></a>Instalowanie narzędzia menedżera klucz tajny

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Kliknij prawym przyciskiem myszy projekt w Eksploratorze rozwiązań i wybierz **Edytuj \<project_name\>.csproj** z menu kontekstowego. Dodaj wybrany element do *.csproj* pliku, a następnie zapisz do przywrócenia skojarzonego pakietu NuGet:

[!code-xml[](app-secrets/sample/UserSecrets/UserSecrets-before.csproj?highlight=10)]

Ponownie kliknij prawym przyciskiem myszy projekt w Eksploratorze rozwiązań i wybierz **Zarządzanie kluczy tajnych użytkownika** z menu kontekstowego. Ten gest dodaje nowy `UserSecretsId` węzła w ramach `PropertyGroup` z *.csproj* plików, jak w poniższym przykładzie wyróżniono:

[!code-xml[](app-secrets/sample/UserSecrets/UserSecrets-after.csproj?highlight=4)]

Zapisywanie zmodyfikowanych *.csproj* również plik zostanie otwarta `secrets.json` plik w edytorze tekstu. Zastąp zawartość `secrets.json` pliku następującym kodem:

```json
{
    "MySecret": "ValueOfMySecret"
}
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Dodaj `Microsoft.Extensions.SecretManager.Tools` do *.csproj* pliku i uruchom [przywracania dotnet](/dotnet/core/tools/dotnet-restore). Te same kroki można użyć do zainstalowania narzędzia menedżera klucz tajny przy użyciu wiersza polecenia.

[!code-xml[](app-secrets/sample/UserSecrets/UserSecrets-before.csproj?highlight=10)]

Przetestuj narzędzie Menedżer klucz tajny, uruchamiając następujące polecenie:

```console
dotnet user-secrets -h
```

Narzędzie Menedżer klucz tajny wyświetli użycia, opcje i pomoc dla polecenia.

> [!NOTE]
> Musi być w tym samym katalogu co *.csproj* uruchamianie narzędzia zdefiniowane w pliku *.csproj* pliku `DotNetCliToolReference` węzłów.

Narzędzie Menedżer klucz tajny działa na ustawienia konfiguracji określonego projektu, które są przechowywane w profilu użytkownika. Aby użyć hasła użytkownika, należy określić projekt `UserSecretsId` wartości w jego *.csproj* pliku. Wartość `UserSecretsId` jest określana, ale jest zazwyczaj unikatowa do projektu. Deweloperzy zazwyczaj generowania identyfikatora GUID dla `UserSecretsId`.

Dodaj `UserSecretsId` projektu w *.csproj* pliku:

[!code-xml[](app-secrets/sample/UserSecrets/UserSecrets-after.csproj?highlight=4)]

Ustaw klucz tajny narzędzie Menedżer klucz tajny. Na przykład w oknie polecenia w katalogu projektu, wprowadź następujące polecenie:

```console
dotnet user-secrets set MySecret ValueOfMySecret
```

Można uruchomić narzędzie Menedżer klucz tajny z innych katalogów, ale należy użyć `--project` opcję, aby przekazać w ścieżce do *.csproj* pliku:
 
```console
dotnet user-secrets set MySecret ValueOfMySecret --project c:\work\WebApp1\src\webapp1
```

Narzędzie Menedżer klucz tajny służy również do, Usuń, a następnie wyczyść klucze tajne aplikacji.

-----

## <a name="accessing-user-secrets-via-configuration"></a>Uzyskiwanie dostępu do kluczy tajnych użytkownika za pomocą konfiguracji

Klucz tajny Menedżera kluczy tajnych dostępu za pomocą systemu konfiguracji. Dodaj `Microsoft.Extensions.Configuration.UserSecrets` pakiet, a następnie uruchom [przywracania dotnet](/dotnet/core/tools/dotnet-restore).

Dodaj użytkownika kluczy tajnych konfiguracji źródła `Startup` metody:

[!code-csharp[](app-secrets/sample/UserSecrets/Startup.cs?highlight=16-19)]

Aby dostęp do kluczy tajnych użytkownika przez interfejs API konfiguracji:

[!code-csharp[](app-secrets/sample/UserSecrets/Startup.cs?highlight=26-29)]

## <a name="how-the-secret-manager-tool-works"></a>Jak działa narzędzie Menedżer klucz tajny

Narzędzie Menedżer klucz tajny optymalizacji abstracts szczegóły implementacji, takich jak jak i gdzie są przechowywane wartości. Można użyć narzędzia bez uprzedniego uzyskania informacji o tych szczegóły implementacji. W bieżącej wersji wartości są przechowywane w [JSON](http://json.org/) pliku konfiguracji w katalogu profilu użytkownika:

* System Windows: `%APPDATA%\microsoft\UserSecrets\<userSecretsId>\secrets.json`

* Linux: `~/.microsoft/usersecrets/<userSecretsId>/secrets.json`

* Mac: `~/.microsoft/usersecrets/<userSecretsId>/secrets.json`

Wartość `userSecretsId` pochodzi z wartością określoną w *.csproj* pliku.

Nie należy napisać kod, który zależy od lokalizacji lub format dane zapisane za pomocą narzędzia menedżera klucz tajny, jak te szczegóły implementacji mogą ulec zmianie. Na przykład tajny wartości są obecnie *nie* dzisiaj zaszyfrowany, ale może być w przyszłości.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Konfiguracja](xref:fundamentals/configuration/index)
