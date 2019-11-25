---
title: Azure Key Vault dostawcę konfiguracji w programie ASP.NET Core
author: guardrex
description: Informacje dotyczące konfigurowania aplikacji przy użyciu par nazwa-wartość ładowanych w czasie wykonywania przy użyciu dostawcy konfiguracji Azure Key Vault.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/14/2019
uid: security/key-vault-configuration
ms.openlocfilehash: e0e55d40734e0cb6e3e1afe1c708ec47c6f43054
ms.sourcegitcommit: f91d322f790123d41ec3271fa084ae20ed9f89a6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/18/2019
ms.locfileid: "74155170"
---
# <a name="azure-key-vault-configuration-provider-in-aspnet-core"></a>Azure Key Vault dostawcę konfiguracji w programie ASP.NET Core

Autorzy [Luke Latham](https://github.com/guardrex) i [Andrew Stanton-pielęgniarki](https://github.com/anurse)

W tym dokumencie wyjaśniono, jak za pomocą dostawcy konfiguracji [Key Vault Microsoft Azure](https://azure.microsoft.com/services/key-vault/) załadować wartości konfiguracji aplikacji z Azure Key Vault wpisów tajnych. Azure Key Vault to usługa oparta na chmurze, która pomaga chronić klucze kryptograficzne i wpisy tajne używane przez aplikacje i usługi. Typowe scenariusze używania Azure Key Vault z aplikacjami ASP.NET Core obejmują:

* Kontrolowanie dostępu do poufnych danych konfiguracyjnych.
* Spełnienie wymagania dotyczącego sprawdzania poprawności sprzętowych modułów zabezpieczeń FIPS 140-2 Level 2 (HSM) podczas przechowywania danych konfiguracyjnych.

[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/key-vault-configuration/samples) ([jak pobrać](xref:index#how-to-download-a-sample))

## <a name="packages"></a>Pakiety

Dodaj odwołanie do pakietu do pakietu [Microsoft. Extensions. Configuration. AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) .

## <a name="sample-app"></a>Przykładowa aplikacja

Przykładowa aplikacja działa w dwóch trybów określonych przez instrukcję `#define` w górnej części pliku *program.cs* :

* `Certificate` &ndash; ilustruje użycie identyfikatora klienta Azure Key Vault i certyfikatu X. 509 w celu uzyskania dostępu do wpisów tajnych przechowywanych w Azure Key Vault. Tę wersję przykładu można uruchomić z dowolnej lokalizacji wdrożonej do Azure App Service lub dowolnego hosta, który może obsługiwać aplikację ASP.NET Core.
* `Managed` &ndash; ilustruje sposób używania [tożsamości zarządzanych dla zasobów platformy Azure](/azure/active-directory/managed-identities-azure-resources/overview) do uwierzytelniania aplikacji w celu Azure Key Vault z uwierzytelnianiem za pomocą usługi Azure AD bez poświadczeń przechowywanych w kodzie lub konfiguracji aplikacji. Podczas uwierzytelniania przy użyciu tożsamości zarządzanych nie są wymagane identyfikatory aplikacji i hasła usługi Azure AD (klucz tajny klienta). Wersja przykładu `Managed` musi być wdrożona na platformie Azure. Postępuj zgodnie ze wskazówkami zawartymi w sekcji [Korzystanie z zarządzanych tożsamości dla zasobów platformy Azure](#use-managed-identities-for-azure-resources) .

Aby uzyskać więcej informacji na temat konfigurowania przykładowej aplikacji przy użyciu dyrektyw preprocesora (`#define`), zobacz <xref:index#preprocessor-directives-in-sample-code>.

## <a name="secret-storage-in-the-development-environment"></a>Magazyn tajny w środowisku programistycznym

Ustaw wpisy tajne lokalnie przy użyciu [Narzędzia do zarządzania kluczami tajnymi](xref:security/app-secrets). Gdy aplikacja Przykładowa zostanie uruchomiona na komputerze lokalnym w środowisku deweloperskim, wpisy tajne są ładowane z lokalnego magazynu lokalnych wpisów tajnych.

Narzędzie Secret Manager wymaga właściwości `<UserSecretsId>` w pliku projektu aplikacji. Ustaw wartość właściwości (`{GUID}`) na dowolny unikatowy identyfikator GUID:

```xml
<PropertyGroup>
  <UserSecretsId>{GUID}</UserSecretsId>
</PropertyGroup>
```

Wpisy tajne są tworzone jako pary nazwa-wartość. Wartości hierarchiczne (sekcje konfiguracji) używają `:` (dwukropek) jako separatora w nazwach kluczy [konfiguracji ASP.NET Core](xref:fundamentals/configuration/index) .

Menedżer wpisów tajnych jest używany z poziomu powłoki poleceń otwartej w [katalogu głównym zawartości](xref:fundamentals/index#content-root)projektu, gdzie `{SECRET NAME}` jest nazwą, a `{SECRET VALUE}` jest wartością:

```dotnetcli
dotnet user-secrets set "{SECRET NAME}" "{SECRET VALUE}"
```

Wykonaj następujące polecenia w powłoce poleceń z poziomu [głównego zawartości](xref:fundamentals/index#content-root) projektu, aby ustawić wpisy tajne dla przykładowej aplikacji:

```dotnetcli
dotnet user-secrets set "SecretName" "secret_value_1_dev"
dotnet user-secrets set "Section:SecretName" "secret_value_2_dev"
```

Jeśli te wpisy tajne są przechowywane w Azure Key Vault w [magazynie kluczy tajnych w środowisku produkcyjnym z](#secret-storage-in-the-production-environment-with-azure-key-vault) sekcją Azure Key Vault, sufiks `_dev` zostanie zmieniony na `_prod`. Sufiks udostępnia wizualizację wizualną w danych wyjściowych aplikacji wskazującej źródło wartości konfiguracyjnych.

## <a name="secret-storage-in-the-production-environment-with-azure-key-vault"></a>Magazyn tajny w środowisku produkcyjnym z Azure Key Vault

Instrukcje dostępne w [przewodniku szybki start: Ustawianie i pobieranie wpisu tajnego z Azure Key Vault za pomocą interfejsu wiersza polecenia platformy Azure](/azure/key-vault/quick-create-cli) są zestawione w tym miejscu na potrzeby tworzenia Azure Key Vault i przechowywania wpisów tajnych używanych przez przykładową aplikację. Więcej informacji można znaleźć w temacie.

1. Otwórz usługę Azure Cloud Shell przy użyciu jednej z następujących metod w [Azure Portal](https://portal.azure.com/):

   * Wybierz pozycję **Wypróbuj** w prawym górnym rogu bloku kodu. Użyj ciągu wyszukiwania "interfejs wiersza polecenia platformy Azure" w polu tekstowym.
   * Otwórz Cloud Shell w przeglądarce za pomocą przycisku **uruchom Cloud Shell** .
   * Wybierz przycisk **Cloud Shell** w menu w prawym górnym rogu Azure Portal.

   Aby uzyskać więcej informacji, zobacz [interfejs wiersza polecenia platformy Azure (CLI)](/cli/azure/) i [Omówienie Azure Cloud Shell](/azure/cloud-shell/overview).

1. Jeśli nie masz jeszcze uwierzytelnienia, zaloguj się za pomocą polecenia `az login`.

1. Utwórz grupę zasobów za pomocą następującego polecenia, gdzie `{RESOURCE GROUP NAME}` jest nazwą grupy zasobów dla nowej grupy zasobów, a `{LOCATION}` jest regionem platformy Azure (centrum danych):

   ```azure-cli
   az group create --name "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. Utwórz magazyn kluczy w grupie zasobów przy użyciu następującego polecenia, gdzie `{KEY VAULT NAME}` jest nazwą nowego magazynu kluczy, a `{LOCATION}` jest regionem platformy Azure (centrum danych):

   ```azure-cli
   az keyvault create --name "{KEY VAULT NAME}" --resource-group "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. Utwórz klucze tajne w magazynie kluczy jako pary nazwa-wartość.

   Nazwy tajne Azure Key Vault są ograniczone do znaków alfanumerycznych i kresek. Wartości hierarchiczne (sekcje konfiguracji) używają `--` (dwóch kresek) jako separatora. Dwukropek, które zwykle są używane do ograniczania sekcji z podklucza w [konfiguracji ASP.NET Core](xref:fundamentals/configuration/index), nie są dozwolone w nazwach tajnych magazynu kluczy. W związku z tym dwie kreski są używane i zamieniane na dwukropek, gdy wpisy tajne są ładowane do konfiguracji aplikacji.

   Następujące wpisy tajne są przeznaczone do użycia z przykładową aplikacją. Wartości zawierają sufiks `_prod`, aby odróżnić je od wartości sufiksów `_dev` ładowanych w środowisku programistycznym ze swoich kluczy tajnych użytkownika. Zastąp `{KEY VAULT NAME}` nazwą magazynu kluczy utworzonego w poprzednim kroku:

   ```azure-cli
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "SecretName" --value "secret_value_1_prod"
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "Section--SecretName" --value "secret_value_2_prod"
   ```

## <a name="use-application-id-and-x509-certificate-for-non-azure-hosted-apps"></a>Używanie identyfikatora aplikacji i certyfikatu X. 509 dla aplikacji nieobsługiwanych przez platformę Azure

Skonfiguruj usługę Azure AD, Azure Key Vault i aplikację, aby używać identyfikatora aplikacji Azure Active Directory i certyfikatu X. 509 do uwierzytelniania w magazynie kluczy **, gdy aplikacja jest hostowana poza platformą Azure**. Aby uzyskać więcej informacji, zobacz [Informacje o kluczach, wpisach tajnych i certyfikatach](/azure/key-vault/about-keys-secrets-and-certificates).

> [!NOTE]
> Chociaż użycie identyfikatora aplikacji i certyfikatu X. 509 jest obsługiwane w przypadku aplikacji hostowanych na platformie Azure, zalecamy używanie [zarządzanych tożsamości dla zasobów platformy Azure](#use-managed-identities-for-azure-resources) podczas hostowania aplikacji na platformie Azure. Tożsamości zarządzane nie wymagają przechowywania certyfikatu w aplikacji ani w środowisku deweloperskim.

Przykładowa aplikacja używa identyfikatora aplikacji i certyfikatu X. 509, gdy instrukcja `#define` w górnej części pliku *program.cs* jest ustawiona na `Certificate`.

1. Utwórz certyfikat archiwum PKCS # 12 (*PFX*). Opcje tworzenia certyfikatów obejmują [MakeCert w systemach Windows](/windows/desktop/seccrypto/makecert) i [OpenSSL](https://www.openssl.org/).
1. Zainstaluj certyfikat w osobistym magazynie certyfikatów bieżącego użytkownika. Oznaczenie klucza jako możliwego do eksportu jest opcjonalne. Zanotuj odcisk palca certyfikatu, który jest używany w dalszej części tego procesu.
1. Wyeksportuj certyfikat archiwum PKCS # 12 (*PFX*) jako certyfikat szyfrowany algorytmem DER (*CER*).
1. Zarejestruj aplikację w usłudze Azure AD (**rejestracje aplikacji**).
1. Przekaż certyfikat szyfrowany algorytmem DER (*CER*) do usługi Azure AD:
   1. Wybierz aplikację w usłudze Azure AD.
   1. Przejdź do **przystawki certyfikaty & wpisy tajne**.
   1. Wybierz pozycję **Przekaż certyfikat** , aby przekazać certyfikat zawierający klucz publiczny. Akceptowany jest certyfikat *CER*, *PEM*lub *CRT* .
1. Zapisz nazwę magazynu kluczy, identyfikator aplikacji i odcisk palca certyfikatu w pliku *appSettings. JSON* aplikacji.
1. Przejdź do **magazynu kluczy** w Azure Portal.
1. Wybierz magazyn kluczy utworzony w [magazynie wpisów tajnych w środowisku produkcyjnym z](#secret-storage-in-the-production-environment-with-azure-key-vault) sekcją Azure Key Vault.
1. Wybierz pozycję **zasady dostępu**.
1. Wybierz pozycję **Dodaj zasady dostępu**.
1. Otwórz **uprawnienia do wpisów tajnych** i Udostępnij aplikację z uprawnieniami **pobierania** i **wyświetlania listy** .
1. Wybierz pozycję **Wybierz podmiot zabezpieczeń** i wybierz zarejestrowaną aplikację według nazwy. Wybierz przycisk **Wybierz** .
1. Wybierz **przycisk OK**.
1. Wybierz pozycję **Zapisz**.
1. Wdróż aplikację.

`Certificate` Przykładowa aplikacja uzyskuje swoje wartości konfiguracji z `IConfigurationRoot` o takiej samej nazwie jak nazwa klucza tajnego:

* Wartości niehierarchiczne: wartość `SecretName` jest uzyskiwana z `config["SecretName"]`.
* Wartości hierarchiczne (sekcje): Użyj `:` (dwukropek) lub metody rozszerzenia `GetSection`. Użyj jednego z tych metod, aby uzyskać wartość konfiguracji:
  * `config["Section:SecretName"]`
  * `config.GetSection("Section")["SecretName"]`

Certyfikat X. 509 jest zarządzany przez system operacyjny. Aplikacja wywołuje <xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> przy użyciu wartości dostarczonych przez plik *appSettings. JSON* :

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](key-vault-configuration/samples/3.x/SampleApp/Program.cs?name=snippet1&highlight=20-23)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](key-vault-configuration/samples/2.x/SampleApp/Program.cs?name=snippet1&highlight=20-23)]

::: moniker-end

Przykładowe wartości:

* Nazwa magazynu kluczy: `contosovault`
* Identyfikator aplikacji: `627e911e-43cc-61d4-992e-12db9c81b413`
* Odcisk palca certyfikatu: `fe14593dd66b2406c5269d742d04b6e1ab03adb1`

*appSettings. JSON*:

::: moniker range=">= aspnetcore-3.0"

[!code-json[](key-vault-configuration/samples/3.x/SampleApp/appsettings.json?highlight=10-12)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](key-vault-configuration/samples/2.x/SampleApp/appsettings.json?highlight=10-12)]

::: moniker-end

Po uruchomieniu aplikacji na stronie sieci Web są wyświetlane załadowane wartości klucza tajnego. W środowisku programistycznym wartości klucza tajnego są ładowane z sufiksem `_dev`. W środowisku produkcyjnym wartości są ładowane z sufiksem `_prod`.

## <a name="use-managed-identities-for-azure-resources"></a>Korzystanie z tożsamości zarządzanych dla zasobów platformy Azure

**Aplikacja wdrożona na platformie Azure** może korzystać z [zarządzanych tożsamości dla zasobów platformy Azure](/azure/active-directory/managed-identities-azure-resources/overview), co pozwala na uwierzytelnianie aplikacji przy użyciu Azure Key Vault uwierzytelniania usługi Azure AD bez poświadczeń (Identyfikator aplikacji i hasło/klucz tajny klienta) przechowywane w aplikacji.

Przykładowa aplikacja używa zarządzanych tożsamości dla zasobów platformy Azure, gdy instrukcja `#define` w górnej części pliku *program.cs* jest ustawiona na `Managed`.

Wprowadź nazwę magazynu w pliku *appSettings. JSON* aplikacji. Przykładowa aplikacja nie wymaga identyfikatora aplikacji i hasła (klucz tajny klienta), gdy jest ustawiony na wersję `Managed`, więc można zignorować te wpisy konfiguracji. Aplikacja została wdrożona na platformie Azure, a platforma Azure uwierzytelnia aplikację w celu uzyskiwania dostępu do Azure Key Vault tylko przy użyciu nazwy magazynu przechowywanej w pliku *appSettings. JSON* .

Wdróż przykładową aplikację w Azure App Service.

Aplikacja wdrożona do Azure App Service jest automatycznie zarejestrowana w usłudze Azure AD podczas tworzenia usługi. Uzyskaj identyfikator obiektu z wdrożenia do użycia w poniższym poleceniu. Identyfikator obiektu jest pokazywany w Azure Portal na panelu **Identity** App Service.

Za pomocą interfejsu wiersza polecenia platformy Azure i identyfikatora obiektu aplikacji Udostępnij aplikację z uprawnieniami `list` i `get`, aby uzyskać dostęp do magazynu kluczy:

```azure-cli
az keyvault set-policy --name '{KEY VAULT NAME}' --object-id {OBJECT ID} --secret-permissions get list
```

**Uruchom ponownie aplikację** przy użyciu interfejsu wiersza polecenia platformy Azure, programu PowerShell lub Azure Portal.

Przykładowa aplikacja:

* Tworzy wystąpienie klasy `AzureServiceTokenProvider` bez parametrów połączenia. Jeśli nie podano parametrów połączenia, Dostawca próbuje uzyskać token dostępu z zarządzanych tożsamości dla zasobów platformy Azure.
* Tworzony jest nowy <xref:Microsoft.Azure.KeyVault.KeyVaultClient> przy użyciu wywołania zwrotnego tokenu wystąpienia `AzureServiceTokenProvider`.
* Wystąpienie <xref:Microsoft.Azure.KeyVault.KeyVaultClient> jest używane z domyślną implementacją <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager>, która ładuje wszystkie wartości tajne i zastępuje podwójne myślniki (`--`) średnikami (`:`) w nazwach kluczy.

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](key-vault-configuration/samples/3.x/SampleApp/Program.cs?name=snippet2&highlight=13-21)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](key-vault-configuration/samples/2.x/SampleApp/Program.cs?name=snippet2&highlight=13-21)]

::: moniker-end

Przykładowa wartość nazwy magazynu kluczy: `contosovault`
    
*appSettings. JSON*:

```json
{
  "KeyVaultName": "Key Vault Name"
}
```

Po uruchomieniu aplikacji na stronie sieci Web są wyświetlane załadowane wartości klucza tajnego. W środowisku programistycznym wartości klucza tajnego mają sufiks `_dev`, ponieważ są one dostarczane przez wpisy tajne użytkownika. W środowisku produkcyjnym wartości są ładowane z sufiksem `_prod`, ponieważ są one dostarczane przez Azure Key Vault.

Jeśli wystąpi błąd `Access denied`, potwierdź, że aplikacja jest zarejestrowana w usłudze Azure AD i zapewniony dostęp do magazynu kluczy. Upewnij się, że usługa została uruchomiona ponownie na platformie Azure.

Aby uzyskać informacje na temat używania dostawcy z zarządzaną tożsamością i potoku usługi Azure DevOps, zobacz [Tworzenie połączenia usługi Azure Resource Manager z maszyną wirtualną przy użyciu tożsamości usługi zarządzanej](/azure/devops/pipelines/library/connect-to-azure#create-an-azure-resource-manager-service-connection-to-a-vm-with-a-managed-service-identity).

::: moniker range=">= aspnetcore-3.0"

## <a name="configuration-options"></a>Opcje konfiguracji

<xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> można zaakceptować <xref:Microsoft.Extensions.Configuration.AzureKeyVault.AzureKeyVaultConfigurationOptions>:

```csharp
config.AddAzureKeyVault(
    new AzureKeyVaultConfigurationOptions()
    {
        ...
    });
```

| Właściwość         | Opis |
| ---------------- | ----------- |
| `Client`         | <xref:Microsoft.Azure.KeyVault.KeyVaultClient> do użycia podczas pobierania wartości. |
| `Manager`        | <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager> wystąpienie używane do kontrolowania ładowania klucza tajnego. |
| `ReloadInterval` | `Timespan`, aby poczekać między kolejnymi próbami sondowania magazynu kluczy pod kątem zmian. Wartość domyślna to `null` (konfiguracja nie jest załadowana ponownie). |
| `Vault`          | Identyfikator URI magazynu kluczy. |

::: moniker-end

## <a name="use-a-key-name-prefix"></a>Użyj prefiksu nazwy klucza

<xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> zapewnia Przeciążenie, które akceptuje implementację <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager>, co umożliwia kontrolowanie sposobu, w jaki wpisy tajne magazynu kluczy są konwertowane na klucze konfiguracji. Na przykład można zaimplementować interfejs w celu załadowania wartości tajnych na podstawie wartości prefiksu podanej podczas uruchamiania aplikacji. Dzięki temu można na przykład ładować wpisy tajne na podstawie wersji aplikacji.

> [!WARNING]
> Nie należy używać prefiksów w kluczach tajnych magazynu kluczy w celu umieszczenia wpisów tajnych dla wielu aplikacji w tym samym magazynie kluczy lub umieszczenia tajemnicy środowiskowej (na przykład *tworzenia* i *produkcji* wpisów tajnych) w tym samym magazynie. Firma Microsoft zaleca, aby różne aplikacje i środowiska deweloperskie i produkcyjne używały oddzielnych magazynów kluczy do izolowania środowisk aplikacji w celu uzyskania najwyższego poziomu zabezpieczeń.

W poniższym przykładzie wpis tajny jest ustanowiony w magazynie kluczy (oraz za pomocą narzędzia tajnego Menedżera dla środowiska programistycznego) dla `5000-AppSecret` (okresy nie są dozwolone w nazwach wpisów tajnych magazynu kluczy). Ten klucz tajny reprezentuje wpis tajny aplikacji dla 5.0.0.0 wersji aplikacji. W przypadku innej wersji aplikacji 5.1.0.0 wpis tajny jest dodawany do magazynu kluczy (i przy użyciu narzędzia tajnego Menedżera) dla `5100-AppSecret`. Każda wersja aplikacji ładuje swoją wersję tajną swojej wersji do konfiguracji jako `AppSecret`, oddzielając ją od wersji podczas ładowania klucza tajnego.

<xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> jest wywoływana z niestandardowym <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager>:

[!code-csharp[](key-vault-configuration/samples_snapshot/Program.cs)]

Implementacja <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager> reaguje na prefiksy wersji wpisów tajnych w celu załadowania odpowiedniego wpisu tajnego do konfiguracji:

* `Load` ładuje wpis tajny, gdy jego nazwa zaczyna się od prefiksu. Inne wpisy tajne nie są ładowane.
* `GetKey`:
  * Usuwa prefiks z nazwy wpisu tajnego.
  * Zastępuje dwie kreski w każdej nazwie `KeyDelimiter`, która jest ogranicznikiem używanym w konfiguracji (zazwyczaj dwukropek). Azure Key Vault nie zezwala na dwukropek w nazwach kluczy tajnych.

[!code-csharp[](key-vault-configuration/samples_snapshot/Startup.cs)]

Metoda `Load` jest wywoływana przez algorytm dostawcy, który iteruje przez wpisy tajne magazynu, aby znaleźć te, które mają prefiks wersji. Po znalezieniu prefiksu wersji z `Load`algorytm używa metody `GetKey` do zwrócenia nazwy konfiguracji wpisu tajnego. Rozdziela prefiks wersji z nazwy wpisu tajnego i zwraca resztę nazwy wpisu tajnego do załadowania do par nazwa-wartość konfiguracji aplikacji.

Gdy takie podejście jest zaimplementowane:

1. Wersja aplikacji określona w pliku projektu aplikacji. W poniższym przykładzie wersja aplikacji jest ustawiona na `5.0.0.0`:

   ```xml
   <PropertyGroup>
     <Version>5.0.0.0</Version>
   </PropertyGroup>
   ```

1. Upewnij się, że właściwość `<UserSecretsId>` jest obecna w pliku projektu aplikacji, gdzie `{GUID}` jest identyfikatorem GUID dostarczonym przez użytkownika:

   ```xml
   <PropertyGroup>
     <UserSecretsId>{GUID}</UserSecretsId>
   </PropertyGroup>
   ```

   Zapisz następujące wpisy tajne lokalnie za pomocą [Narzędzia tajnego Menedżera](xref:security/app-secrets):

   ```dotnetcli
   dotnet user-secrets set "5000-AppSecret" "5.0.0.0_secret_value_dev"
   dotnet user-secrets set "5100-AppSecret" "5.1.0.0_secret_value_dev"
   ```

1. Wpisy tajne są zapisywane w Azure Key Vault przy użyciu następujących poleceń interfejsu wiersza polecenia platformy Azure:

   ```azure-cli
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "5000-AppSecret" --value "5.0.0.0_secret_value_prod"
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "5100-AppSecret" --value "5.1.0.0_secret_value_prod"
   ```

1. Po uruchomieniu aplikacji są ładowane wpisy tajne magazynu kluczy. Wpis tajny ciągu dla `5000-AppSecret` jest zgodny z wersją aplikacji określoną w pliku projektu aplikacji (`5.0.0.0`).

1. Wersja, `5000` (z kreską), jest usuwana z nazwy klucza. W całej aplikacji odczytywanie konfiguracji za pomocą klucza `AppSecret` ładuje wartość klucza tajnego.

1. Jeśli wersja aplikacji została zmieniona w pliku projektu na `5.1.0.0`, a aplikacja zostanie uruchomiona ponownie, zwracana wartość wpisu tajnego jest `5.1.0.0_secret_value_dev` w środowisku deweloperskim i `5.1.0.0_secret_value_prod` w produkcji.

> [!NOTE]
> Możesz również podać własną implementację <xref:Microsoft.Azure.KeyVault.KeyVaultClient>, aby <xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*>. Niestandardowy klient umożliwia udostępnianie pojedynczego wystąpienia klienta w aplikacji.

## <a name="bind-an-array-to-a-class"></a>Powiąż tablicę z klasą

Dostawca może odczytać wartości konfiguracyjne w tablicy w celu powiązania z tablicą POCO.

Podczas odczytywania ze źródła konfiguracji, które pozwala na używanie kluczy zawierających dwukropek (`:`) separatory, segment klucza numerycznego służy do odróżniania kluczy tworzących tablicę (`:0:`, `:1:`,... `:{n}:`). Aby uzyskać więcej informacji, zobacz [Konfiguracja: Powiąż tablicę z klasą](xref:fundamentals/configuration/index#bind-an-array-to-a-class).

Klucze Azure Key Vault nie mogą używać dwukropka jako separatora. Metoda opisana w tym temacie używa podwójnych kresek (`--`) jako separatora wartości hierarchicznych (sekcji). Klucze tablic są przechowywane w Azure Key Vault przy użyciu podwójnych kresek i segmentów kluczy numerycznych (`--0--`, `--1--`, &hellip; `--{n}--`).

Zapoznaj się z następującą konfiguracją dostawcy rejestrowania [Serilog](https://serilog.net/) dostarczoną przez plik JSON. W tablicy `WriteTo` znajdują się dwa literały obiektów, które odzwierciedlają dwa *ujścia*Serilog, które opisują miejsca docelowe na potrzeby rejestrowania danych wyjściowych:

```json
"Serilog": {
  "WriteTo": [
    {
      "Name": "AzureTableStorage",
      "Args": {
        "storageTableName": "logs",
        "connectionString": "DefaultEnd...ountKey=Eby8...GMGw=="
      }
    },
    {
      "Name": "AzureDocumentDB",
      "Args": {
        "endpointUrl": "https://contoso.documents.azure.com:443",
        "authorizationKey": "Eby8...GMGw=="
      }
    }
  ]
}
```

Konfiguracja pokazana w poprzednim pliku JSON jest przechowywana w Azure Key Vault przy użyciu notacji podwójnej kreski (`--`) i segmentów liczbowych:

| Key | Wartość |
| --- | ----- |
| `Serilog--WriteTo--0--Name` | `AzureTableStorage` |
| `Serilog--WriteTo--0--Args--storageTableName` | `logs` |
| `Serilog--WriteTo--0--Args--connectionString` | `DefaultEnd...ountKey=Eby8...GMGw==` |
| `Serilog--WriteTo--1--Name` | `AzureDocumentDB` |
| `Serilog--WriteTo--1--Args--endpointUrl` | `https://contoso.documents.azure.com:443` |
| `Serilog--WriteTo--1--Args--authorizationKey` | `Eby8...GMGw==` |

## <a name="reload-secrets"></a>Załaduj ponownie klucze tajne

Wpisy tajne są buforowane do momentu wywołania `IConfigurationRoot.Reload()`. Wygasłe, wyłączone i zaktualizowane klucze tajne w magazynie kluczy nie są przestrzegane przez aplikację do momentu wykonania `Reload`.

```csharp
Configuration.Reload();
```

## <a name="disabled-and-expired-secrets"></a>Wyłączone i wygasłe wpisy tajne

Wyłączone i wygasłe wpisy tajne zwracają <xref:Microsoft.Azure.KeyVault.Models.KeyVaultErrorException>. Aby zapobiec zgłaszaniu aplikacji, podaj konfigurację przy użyciu innego dostawcy konfiguracji lub zaktualizuj klucz tajny wyłączony lub wygasły.

## <a name="troubleshoot"></a>Rozwiązywanie problemów

Gdy aplikacja nie może załadować konfiguracji przy użyciu dostawcy, komunikat o błędzie jest zapisywana do [infrastruktury rejestrowania ASP.NET Core](xref:fundamentals/logging/index). Następujące warunki uniemożliwią ładowanie konfiguracji:

* Aplikacja lub certyfikat nie jest poprawnie skonfigurowany w Azure Active Directory.
* Magazyn kluczy nie istnieje w Azure Key Vault.
* Aplikacja nie ma uprawnień dostępu do magazynu kluczy.
* Zasady dostępu nie obejmują uprawnień `Get` i `List`.
* W magazynie kluczy dane konfiguracji (para nazwa-wartość) są nieprawidłowo nazwane, brakujące, wyłączone lub wygasłe.
* Aplikacja ma nieprawidłową nazwę magazynu kluczy (`KeyVaultName`), identyfikator aplikacji usługi Azure AD (`AzureADApplicationId`) lub odcisk palca certyfikatu usługi Azure AD (`AzureADCertThumbprint`).
* Klucz konfiguracji (nazwa) jest niepoprawny w aplikacji dla wartości, którą próbujesz załadować.
* Podczas dodawania zasad dostępu dla aplikacji do magazynu kluczy zasady zostały utworzone, ale nie wybrano przycisku **Zapisz** w interfejsie użytkownika **zasad dostępu** .

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:fundamentals/configuration/index>
* [Microsoft Azure: Key Vault](https://azure.microsoft.com/services/key-vault/)
* [Microsoft Azure: dokumentacja Key Vault](/azure/key-vault/)
* [Jak generować i przesyłać klucze chronione przez moduł HSM dla Azure Key Vault](/azure/key-vault/key-vault-hsm-protected-keys)
* [Klasa KeyVaultClient](/dotnet/api/microsoft.azure.keyvault.keyvaultclient)
* [Szybki Start: Ustawianie i pobieranie klucza tajnego z Azure Key Vault przy użyciu aplikacji sieci Web platformy .NET](/azure/key-vault/quick-create-net)
* [Samouczek: jak używać Azure Key Vault z maszyną wirtualną platformy Azure z systemem Windows w środowisku .NET](/azure/key-vault/tutorial-net-windows-virtual-machine)
