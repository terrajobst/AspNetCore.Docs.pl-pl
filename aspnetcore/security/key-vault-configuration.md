---
title: Dostawca konfiguracji usługi Azure Key Vault w programie ASP.NET Core
author: guardrex
description: Dowiedz się, jak skonfigurować aplikację tak, za pomocą pary nazwa wartość ładowane w czasie wykonywania za pomocą dostawcy konfiguracji magazynu kluczy Azure.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/01/2018
uid: security/key-vault-configuration
ms.openlocfilehash: 933f4fb1f2c1c412d318af5974cc9653805242ca
ms.sourcegitcommit: 25150f4398de83132965a89f12d3a030f6cce48d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/25/2018
ms.locfileid: "42927990"
---
# <a name="azure-key-vault-configuration-provider-in-aspnet-core"></a>Dostawca konfiguracji usługi Azure Key Vault w programie ASP.NET Core

Przez [Luke Latham](https://github.com/guardrex) i [Andrew Stanton pielęgniarki](https://github.com/anurse)

W tym dokumencie wyjaśniono, jak używać [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) dostawcę konfiguracji, które można załadować wartości konfiguracji aplikacji z wpisy tajne w usłudze Azure Key Vault. Usługa Azure Key Vault to usługa oparta na chmurze, która pomaga chronić klucze kryptograficzne i wpisy tajne używane przez aplikacje i usługi. Typowe scenariusze obejmują kontrolę dostępu do danych poufnych konfiguracji i weryfikacja spełniające wymagania FIPS 140-2 poziom 2 sprzętowych modułów zabezpieczeń (HSM) w przypadku przechowywania danych konfiguracji. Ta funkcja jest dostępna dla aplikacji przeznaczonych dla platformy ASP.NET Core 1.1 lub nowszej.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

## <a name="package"></a>Package

Aby użyć dostawcy, Dodaj odwołanie do [Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) pakietu.

## <a name="app-configuration"></a>Konfiguracja aplikacji

Możesz zapoznać się z dostawcą z [przykładowe aplikacje](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples). Gdy magazynu kluczy i tworzenie wpisów tajnych w magazynie, przykładowych aplikacji bezpiecznego załadować wartościami wpisów tajnych do ich konfiguracji i wyświetlaj je w stron sieci Web.

Dostawca zostanie dodany do konfiguracji aplikacji z `AddAzureKeyVault` rozszerzenia. W przykładowych aplikacji, rozszerzenia używa załadowane z trzech wartości konfiguracji *appsettings.json* pliku.

| Ustawienia aplikacji    | Opis                    | Przykład                                      |
| -------------- | ------------------------------ | -------------------------------------------- |
| `Vault`        | Nazwa usługi Azure Key Vault           | contosovault                                 |
| `ClientId`     | Identyfikator aplikacji usługi Azure Active Directory  | 627e911e-43cc-61d4-992e-12db9c81b413         |
| `ClientSecret` | Klucz aplikacji usługi Azure Active Directory | g58K3dtg59o1Pa+e59v2Tx829w6VxTB2yv9sv/101di= |

[!code-csharp[Program](key-vault-configuration/samples/basic-sample/2.x/Program.cs?name=snippet1)]

## <a name="create-key-vault-secrets-and-load-configuration-values-basic-sample"></a>Tworzenie magazynu kluczy, wpisów tajnych i załadować wartości konfiguracji (podstawowy przykład)

1. Tworzenie magazynu kluczy i konfigurowanie usługi Azure Active Directory (Azure AD) dla aplikacji, postępując zgodnie ze wskazówkami w [Rozpoczynanie pracy z usługą Azure Key Vault](https://azure.microsoft.com/documentation/articles/key-vault-get-started/).
   * Dodawanie kluczy tajnych do magazynu kluczy przy użyciu [modułu programu PowerShell magazynu klucz AzureRM](/powershell/module/azurerm.keyvault) dostępnym [galerii programu PowerShell](https://www.powershellgallery.com/packages/AzureRM.KeyVault), [API REST usługi Azure Key Vault](/rest/api/keyvault/), lub [Witryny azure Portal](https://portal.azure.com/). Klucze tajne są tworzone jako *ręczne* lub *certyfikatu* wpisów tajnych. *Certyfikat* klucze tajne certyfikatów do użytku przez aplikacje i usługi, ale nie są obsługiwane przez dostawcę konfiguracji. Należy używać *ręczne* opcję, aby utworzyć klucze tajne pary nazwa wartość, do użytku z dostawcą konfiguracji.
     * Proste hasła są tworzone jako pary nazwa wartość. Usługa Azure Key Vault nazw kluczy tajnych są ograniczone do znaki alfanumeryczne i łączniki.
     * Użyj wartości hierarchicznych (sekcji konfiguracji) `--` (dwa łączniki) jako separatora w próbce. Dwukropki, które są zwykle używane do ograniczania sekcji w podkluczu w [konfiguracji platformy ASP.NET Core](xref:fundamentals/configuration/index), nie są dozwolone w nazwach wpisu tajnego. W związku z tym dwa łączniki są używane i zamienione dla dwukropek, gdy wpisy tajne są ładowane do konfiguracji aplikacji.
     * Utworzyć dwa *ręczne* wpisów tajnych za pomocą poniższej pary nazwa wartość. Pierwszy wpis tajny jest prostą nazwę i wartość, a drugi wpis tajny tworzy wartość wpisu tajnego z sekcji i podklucz nazwa klucza tajnego:
       * `SecretName`: `secret_value_1`
       * `Section--SecretName`: `secret_value_2`
   * Rejestrowanie przykładowej aplikacji z usługą Azure Active Directory.
   * Zezwolić aplikacji na dostęp do magazynu kluczy. Kiedy używasz `Set-AzureRmKeyVaultAccessPolicy` polecenia cmdlet programu PowerShell, aby autoryzować aplikację do dostępu do magazynu kluczy, podaj `List` i `Get` dostęp do wpisów tajnych za pomocą `-PermissionsToSecrets list,get`.

2. Aktualizowanie aplikacji *appsettings.json* pliku z wartościami `Vault`, `ClientId`, i `ClientSecret`.
3. Uruchom przykładową aplikację, która uzyskuje jego wartości konfiguracji z `IConfigurationRoot` o takiej samej nazwie jak nazwa wpisu tajnego.
   * Wartości inne niż hierarchiczne: wartość `SecretName` są uzyskiwane z `config["SecretName"]`.
   * Hierarchiczna wartości (sekcje): Użyj `:` notacji (dwukropek) lub `GetSection` — metoda rozszerzenia. Użyj jednej z tych metod można uzyskać wartości konfiguracji:
     * `config["Section:SecretName"]`
     * `config.GetSection("Section")["SecretName"]`

Po uruchomieniu aplikacji, strony sieci Web pokazuje załadowany wartości klucza tajnego:

![Okno przeglądarki zawierające wartości klucza tajnego załadowany za pośrednictwem dostawcy konfiguracji magazynu kluczy Azure](key-vault-configuration/_static/sample1.png)

## <a name="create-prefixed-key-vault-secrets-and-load-configuration-values-key-name-prefix-sample"></a>Tworzenie prefiksem magazynu kluczy, wpisów tajnych i załadować wartości konfiguracji (klucz name prefiks sample)

`AddAzureKeyVault` udostępnia również przeciążenie, które akceptuje implementację `IKeyVaultSecretManager`, co pozwala na kontrolowanie sposobu klucza magazynu kluczy tajnych są konwertowane na klucze konfiguracji. Na przykład można zaimplementować interfejs służący do ładowania wartościami wpisów tajnych na podstawie wartości prefiks, który podajesz podczas uruchamiania aplikacji. Dzięki temu można na przykład, aby ładować wpisy tajne w oparciu o wersję aplikacji.

> [!WARNING]
> Nie używaj prefiksy na wpisy tajne usługi key vault, umieść wpisów tajnych dla wielu aplikacji w tym samym magazynie kluczy lub umieść środowiska wpisy tajne (na przykład *rozwoju* a *produkcji* wpisy tajne) w tej samej Magazyn. Zalecane jest różnych aplikacji i środowisk tworzenia/produkcyjnych użycie oddzielnych magazynów kluczy do izolowania aplikacji środowiska na najwyższy poziom zabezpieczeń.

Przy użyciu drugiej aplikacji przykładowej, Utwórz klucz tajny w usłudze key vault dla `5000-AppSecret` (kropki nie są dozwolone w nazwach wpisu tajnego usługi key vault) reprezentujący klucz tajny aplikacji dla 5.0.0.0 wersję aplikacji. Dla innej wersji, 5.1.0.0, Utwórz klucz tajny dla `5100-AppSecret`. Każda wersja aplikacji ładuje własną wartość wpisu tajnego do jego konfigurację jako `AppSecret`, oddzielającego off wersji podczas ładowania klucza tajnego. Poniżej przedstawiono przykładowe zastosowanie:

[!code-csharp[Configuration builder](key-vault-configuration/samples/key-name-prefix-sample/2.x/Program.cs?name=snippet1&highlight=18)]

[!code-csharp[PrefixKeyVaultSecretManager](key-vault-configuration/samples/key-name-prefix-sample/2.x/Startup.cs?name=snippet1)]

`Load` Metoda jest wywoływana przez algorytm dostawcy, który iteruje po magazyn kluczy tajnych wyszukać te, które mają prefiks wersji. Gdy prefiks wersji znajduje się za pomocą `Load`, algorytm używa `GetKey` metodę, aby zwrócić nazwę konfiguracji nazwa wpisu tajnego. On paski Wyłącz prefiks wersji od nazwy klucza tajnego i zwraca pozostałe Nazwa wpisu tajnego do załadowania do konfiguracji aplikacji pary nazwa wartość.

Podczas implementowania tego podejścia:

1. Wpisy tajne usługi key vault są ładowane.
2. Klucz tajny ciągu dla `5000-AppSecret` dopasowaniu.
3. Wersja, `5000` (przy użyciu dash), jest usuwany poza nazwę klucza, pozostawiając `AppSecret` załadować dane przy użyciu wartość wpisu tajnego do konfiguracji aplikacji.

> [!NOTE]
> Możesz również podać własne `KeyVaultClient` implementacji `AddAzureKeyVault`. Dostarczanie niestandardowego klienta umożliwia udostępnianie jedno wystąpienie klienta między dostawcę konfiguracji i innych części aplikacji.

1. Tworzenie magazynu kluczy i konfigurowanie usługi Azure Active Directory (Azure AD) dla aplikacji, postępując zgodnie ze wskazówkami w [Rozpoczynanie pracy z usługą Azure Key Vault](https://azure.microsoft.com/documentation/articles/key-vault-get-started/).
   * Dodawanie kluczy tajnych do magazynu kluczy przy użyciu [modułu programu PowerShell magazynu klucz AzureRM](/powershell/module/azurerm.keyvault) dostępnym [galerii programu PowerShell](https://www.powershellgallery.com/packages/AzureRM.KeyVault), [API REST usługi Azure Key Vault](/rest/api/keyvault/), lub [Witryny azure Portal](https://portal.azure.com/). Klucze tajne są tworzone jako *ręczne* lub *certyfikatu* wpisów tajnych. *Certyfikat* klucze tajne certyfikatów do użytku przez aplikacje i usługi, ale nie są obsługiwane przez dostawcę konfiguracji. Należy używać *ręczne* opcję, aby utworzyć klucze tajne pary nazwa wartość, do użytku z dostawcą konfiguracji.
     * Użyj wartości hierarchicznych (sekcji konfiguracji) `--` (dwa łączniki) jako separatora.
     * Utworzyć dwa *ręczne* wpisów tajnych za pomocą poniższej pary nazwa wartość:
       * `5000-AppSecret`: `5.0.0.0_secret_value`
       * `5100-AppSecret`: `5.1.0.0_secret_value`
   * Rejestrowanie przykładowej aplikacji z usługą Azure Active Directory.
   * Zezwolić aplikacji na dostęp do magazynu kluczy. Kiedy używasz `Set-AzureRmKeyVaultAccessPolicy` polecenia cmdlet programu PowerShell, aby autoryzować aplikację do dostępu do magazynu kluczy, podaj `List` i `Get` dostęp do wpisów tajnych za pomocą `-PermissionsToSecrets list,get`.

2. Aktualizowanie aplikacji *appsettings.json* pliku z wartościami `Vault`, `ClientId`, i `ClientSecret`.
3. Uruchom przykładową aplikację, która uzyskuje jego wartości konfiguracji z `IConfigurationRoot` z taką samą nazwę jak prefiksem Nazwa wpisu tajnego. W tym przykładzie prefiks jest wersji aplikacji, która podano `PrefixKeyVaultSecretManager` podczas dodawania dostawcę konfiguracji usługi Azure Key Vault. Wartość `AppSecret` są uzyskiwane z `config["AppSecret"]`. Strony sieci Web, generowany przez aplikację pokazuje załadowany wartość:

   ![Okno przeglądarki, pokazujący wartość tajna załadowany za pośrednictwem dostawcy konfiguracji magazynu kluczy Azure, gdy wersja aplikacji jest 5.0.0.0](key-vault-configuration/_static/sample2-1.png)

4. Zmień wersję zestawu aplikacji w pliku projektu z `5.0.0.0` do `5.1.0.0` i ponownie uruchom aplikację. Tym razem, jest zwracana wartość wpisu tajnego `5.1.0.0_secret_value`. Strony sieci Web, generowany przez aplikację pokazuje załadowany wartość:

   ![Okno przeglądarki, pokazujący wartość tajna załadowany za pośrednictwem dostawcy konfiguracji magazynu kluczy Azure, gdy wersja aplikacji jest 5.1.0.0](key-vault-configuration/_static/sample2-2.png)

## <a name="control-access-to-the-clientsecret"></a>Kontrola dostępu do ClientSecret

Użyj [narzędzie Menedżer klucz tajny](xref:security/app-secrets) do utrzymania `ClientSecret` spoza projektu drzewie źródłowym. Za pomocą Menedżera klucz tajny kojarzenie kluczy tajnych aplikacji z określonego projektu i udostępniać je w wielu projektach.

Podczas tworzenia aplikacji programu .NET Framework w środowisku, które obsługuje certyfikaty, można wybrać metodę uwierzytelniania usługi Azure Key Vault za pomocą certyfikatu X.509. Klucz prywatny certyfikatu X.509 jest zarządzane przez system operacyjny. Aby uzyskać więcej informacji, zobacz [uwierzytelnianie przy użyciu certyfikatu zamiast wpisu tajnego klienta](https://docs.microsoft.com/azure/key-vault/key-vault-use-from-web-application#authenticate-with-a-certificate-instead-of-a-client-secret). Użyj `AddAzureKeyVault` przeciążenie, które akceptuje `X509Certificate2` (`_env` w następującym przykładzie:

```csharp
var builtConfig = config.Build();

var store = new X509Store(StoreLocation.CurrentUser);
store.Open(OpenFlags.ReadOnly);
var cert = store.Certificates
    .Find(X509FindType.FindByThumbprint, 
        config["CertificateThumbprint"], false);

config.AddAzureKeyVault(
    builtConfig["Vault"],
    builtConfig["ClientId"],
    cert.OfType<X509Certificate2>().Single(),
    new EnvironmentSecretManager(context.HostingEnvironment.ApplicationName));

store.Close();
```

## <a name="reload-secrets"></a>Załaduj ponownie wpisy tajne

Klucze tajne są buforowane do momentu `IConfigurationRoot.Reload()` jest wywoływana. Wygasłe, wyłączona, i zaktualizowanych wpisów tajnych w magazynie kluczy nie są przestrzegane przez aplikację do momentu `Reload` jest wykonywany.

```csharp
Configuration.Reload();
```

## <a name="disabled-and-expired-secrets"></a>Wyłączone lub wygasłe klucze tajne

Wyłączone lub wygasłe klucze tajne throw `KeyVaultClientException`. Aby zapobiec sytuacji, w której aplikacja zostanie zgłoszony, zastąpić aplikację, lub zaktualizuj wyłączone/wygasły klucz tajny.

## <a name="troubleshoot"></a>Rozwiązywanie problemów

Gdy aplikacja zakończy się niepowodzeniem, można załadować konfiguracji przy użyciu dostawcy, komunikat o błędzie zostanie zapisany [infrastruktury platformy ASP.NET Core rejestrowania](xref:fundamentals/logging/index). Konfiguracja ładowanie uniemożliwia następujące warunki:

* Aplikacja nie jest poprawnie skonfigurowane w usłudze Azure Active Directory.
* Magazyn kluczy nie istnieje w usłudze Azure Key Vault.
* Aplikacji nie jest upoważniony do dostępu do magazynu kluczy.
* Zasady dostępu nie zawiera `Get` i `List` uprawnienia.
* W usłudze key vault dane konfiguracji (pary nazwa wartość) niepoprawnie nosi nazwę, Brak, wyłączone lub wygasła.
* Aplikacja ma nazwę niewłaściwego magazynu kluczy (`Vault`), identyfikator aplikacji usługi Azure AD (`ClientId`), lub klucza usługi AD Azure (`ClientSecret`).
* Klucz usługi Azure AD (`ClientSecret`) wygasł.
* Klucz konfiguracji (nazwa) jest niepoprawny w aplikacji dla wartości, które próbujesz załadować.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:fundamentals/configuration/index>
* [Platformy Microsoft Azure: Magazyn kluczy](https://azure.microsoft.com/services/key-vault/)
* [Platformy Microsoft Azure: Dokumentacja usługi Key Vault](/azure/key-vault/)
* [Jak Generowanie i przenoszenie chronionego przez moduł HSM kluczy dla usługi Azure Key Vault](/azure/key-vault/key-vault-hsm-protected-keys)
* [Klasa KeyVaultClient](/dotnet/api/microsoft.azure.keyvault.keyvaultclient)
