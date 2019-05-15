---
title: Dostawca konfiguracji usługi Azure Key Vault w programie ASP.NET Core
author: guardrex
description: Dowiedz się, jak skonfigurować aplikację tak, za pomocą pary nazwa wartość ładowane w czasie wykonywania za pomocą dostawcy konfiguracji magazynu kluczy Azure.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/13/2019
uid: security/key-vault-configuration
ms.openlocfilehash: 78c63cf135ca92f0b5f6c6828b2ae34a44a7b36c
ms.sourcegitcommit: 3ee6ee0051c3d2c8d47a58cb17eef1a84a4c46a0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/14/2019
ms.locfileid: "65621016"
---
# <a name="azure-key-vault-configuration-provider-in-aspnet-core"></a>Dostawca konfiguracji usługi Azure Key Vault w programie ASP.NET Core

Przez [Luke Latham](https://github.com/guardrex) i [Andrew Stanton pielęgniarki](https://github.com/anurse)

W tym dokumencie wyjaśniono, jak używać [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) dostawcę konfiguracji, które można załadować wartości konfiguracji aplikacji z wpisy tajne w usłudze Azure Key Vault. Usługa Azure Key Vault to usługa oparta na chmurze, które pomaga w ochronę kluczy kryptograficznych i wpisów tajnych używanych przez aplikacje i usługi. Typowe scenariusze dotyczące korzystania z usługi Azure Key Vault z aplikacji platformy ASP.NET Core obejmują:

* Kontrolowanie dostępu do danych poufnych konfiguracji.
* Spełniające wymagania FIPS 140-2 Level 2 zweryfikować sprzętowych modułów zabezpieczeń (HSM), podczas zapisywania danych konfiguracji.

Ten scenariusz jest dostępna dla aplikacji przeznaczonych dla platformy ASP.NET Core 2.1 lub nowszej.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/key-vault-configuration/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))

## <a name="packages"></a>Pakiety

Aby używać dostawcy konfiguracji magazynu kluczy Azure, Dodaj odwołanie do pakietu [Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) pakietu.

Przyjęcie [zarządzanych tożsamości dla zasobów platformy Azure](/azure/active-directory/managed-identities-azure-resources/overview) scenariusza, Dodaj odwołanie do pakietu [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication/) pakietu.

> [!NOTE]
> W czasie pisania najnowszą stabilną wersję `Microsoft.Azure.Services.AppAuthentication`, wersja `1.0.3`, zapewnia obsługę [przypisany systemowo zarządzanych tożsamości](/azure/active-directory/managed-identities-azure-resources/overview#how-does-the-managed-identities-for-azure-resources-worka-namehow-does-it-worka). Obsługa *przypisanych do użytkowników zarządzanych tożsamości* jest dostępna w `1.2.0-preview2` pakietu. W tym temacie przedstawiono użycie tożsamości zarządzanych przez system, a podana Przykładowa aplikacja korzysta z wersji `1.0.3` z `Microsoft.Azure.Services.AppAuthentication` pakietu.

## <a name="sample-app"></a>Przykładowa aplikacja

Przykładowa aplikacja jest uruchamiana w jednym z dwóch trybów ustalany na podstawie `#define` instrukcji na górze *Program.cs* pliku:

* `Certificate` &ndash; Zademonstrowano użycie certyfikat X.509 i identyfikator klienta systemu Azure klucza magazynu do dostępu do kluczy tajnych przechowywanych w usłudze Azure Key Vault. Ta wersja przykładu można uruchomić z dowolnego miejsca i wdrażać w usłudze Azure App Service lub dowolnego hosta, które umożliwia obsługę aplikacji ASP.NET Core.
* `Managed` &ndash; Pokazuje sposób użycia [zarządzanych tożsamości dla zasobów platformy Azure](/azure/active-directory/managed-identities-azure-resources/overview) można uwierzytelnić aplikację usługi Azure Key Vault przy użyciu uwierzytelniania usługi Azure AD bez poświadczeń przechowywanych w kodzie aplikacji lub konfiguracji. Korzystając z zarządzanych tożsamości do uwierzytelniania, identyfikator aplikacji w usłudze Azure AD i hasło (klucz tajny klienta) nie są wymagane. `Managed` Wersję przykładu, należy wdrożyć na platformie Azure. Postępuj zgodnie ze wskazówkami w [używania tożsamości zarządzanych zasobów platformy Azure](#use-managed-identities-for-azure-resources) sekcji.

Aby uzyskać więcej informacji na temat konfigurowania przykładowej aplikacji za pomocą dyrektywy preprocesora (`#define`), zobacz <xref:index#preprocessor-directives-in-sample-code>.

## <a name="secret-storage-in-the-development-environment"></a>Wpisu tajnego magazynu w środowisku programistycznym

Ustaw lokalnie przy użyciu kluczy tajnych [narzędzie Menedżer klucz tajny](xref:security/app-secrets). Jeśli przykładowa aplikacja jest uruchamiana na komputerze lokalnym w środowisku programistycznym, wpisów tajnych są ładowane z magazynu lokalnego Menedżera klucz tajny.

Narzędzie Menedżer klucz tajny wymaga `<UserSecretsId>` właściwość w pliku projektu aplikacji. Ustaw wartość właściwości (`{GUID}`) do dowolnej Unikatowy identyfikator GUID:

```xml
<PropertyGroup>
  <UserSecretsId>{GUID}</UserSecretsId>
</PropertyGroup>
```

Klucze tajne są tworzone jako pary nazwa wartość. Użyj wartości hierarchicznych (sekcji konfiguracji) `:` (dwukropek) jako separator w [konfiguracji platformy ASP.NET Core](xref:fundamentals/configuration/index) nazw kluczy.

Menedżer klucz tajny jest używany z powłoki poleceń, otwarte, aby zawartość katalogu głównego projektu, gdzie `{SECRET NAME}` nazywa się i `{SECRET VALUE}` jest wartością:

```console
dotnet user-secrets set "{SECRET NAME}" "{SECRET VALUE}"
```

W powłoce poleceń z katalogu głównego zawartości projektu, aby ustawić wpisów tajnych dla przykładowej aplikacji, wykonaj następujące polecenia:

```console
dotnet user-secrets set "SecretName" "secret_value_1_dev"
dotnet user-secrets set "Section:SecretName" "secret_value_2_dev"
```

Kiedy te klucze tajne są przechowywane w usłudze Azure Key Vault w [wpisu tajnego magazynu w środowisku produkcyjnym za pomocą usługi Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) sekcji `_dev` sufiks jest zmieniana na `_prod`. Sufiks oferuje wizualną w danych wyjściowych aplikacji wskazujący źródła wartości konfiguracji.

## <a name="secret-storage-in-the-production-environment-with-azure-key-vault"></a>Wpisu tajnego magazynu w środowisku produkcyjnym za pomocą usługi Azure Key Vault

Z instrukcjami wyświetlanymi przez [Szybki Start: Ustawianie i pobieranie wpisu tajnego z usługi Azure Key Vault przy użyciu wiersza polecenia platformy Azure](/azure/key-vault/quick-create-cli) tematu są podsumowywane w tym miejscu służą do tworzenia usługi Azure Key Vault i przechowywania wpisów tajnych używanych przez aplikację przykładową. Zapoznaj się z tematem, aby uzyskać więcej informacji.

1. Otwórz usługa Azure Cloud shell przy użyciu jednej z następujących metod w [witryny Azure portal](https://portal.azure.com/):

   * Wybierz **wypróbuj** w prawym górnym rogu bloku kodu. W polu tekstowym, należy użyć ciągu "Wiersza polecenia platformy Azure".
   * Otwórz usługę Cloud Shell w przeglądarce za pośrednictwem **Launch Cloud Shell** przycisku.
   * Wybierz **Cloud Shell** przycisk menu w prawym górnym rogu witryny Azure portal.

   Aby uzyskać więcej informacji, zobacz [interfejsu wiersza polecenia platformy Azure (CLI)](/cli/azure/) i [Omówienie usługi Azure Cloud Shell](/azure/cloud-shell/overview).

1. Jeśli nie są już uwierzytelniony, zaloguj się przy użyciu `az login` polecenia.

1. Utwórz grupę zasobów za pomocą następującego polecenia, gdzie `{RESOURCE GROUP NAME}` to nazwa grupy zasobów dla nowej grupy zasobów i `{LOCATION}` to region platformy Azure (datacenter):

   ```console
   az group create --name "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. Tworzenie magazynu kluczy w grupie zasobów za pomocą następującego polecenia, gdzie `{KEY VAULT NAME}` to nazwa dla nowego magazynu kluczy i `{LOCATION}` to region platformy Azure (datacenter):

   ```console
   az keyvault create --name "{KEY VAULT NAME}" --resource-group "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. Tworzenie wpisów tajnych w magazynie kluczy jako pary nazwa wartość.

   Usługa Azure Key Vault nazw kluczy tajnych są ograniczone do znaki alfanumeryczne i łączniki. Użyj wartości hierarchicznych (sekcji konfiguracji) `--` (dwa łączniki) jako separatora. Dwukropki, które są zwykle używane do ograniczania sekcji w podkluczu w [konfiguracji platformy ASP.NET Core](xref:fundamentals/configuration/index), nie są dozwolone w nazwach wpisu tajnego usługi key vault. W związku z tym dwa łączniki są używane i zamienione dla dwukropek, gdy wpisy tajne są ładowane do konfiguracji aplikacji.

   Następujące klucze tajne są do użytku z przykładowej aplikacji. Wartości obejmują `_prod` sufiks, aby odróżnić je od `_dev` sufiks wartości załadowane w środowisku programistycznym z wpisami tajnymi użytkowników. Zastąp `{KEY VAULT NAME}` nazwą magazynu kluczy, który został utworzony w poprzednim kroku:

   ```console
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "SecretName" --value "secret_value_1_prod"
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "Section--SecretName" --value "secret_value_2_prod"
   ```

## <a name="use-application-id-and-x509-certificate-for-non-azure-hosted-apps"></a>Użyj Identyfikatora aplikacji i X.509 certyfikatu dla aplikacji hostowanej platformy Azure

Konfigurowanie usługi Azure AD i usługi Azure Key Vault oraz aplikacji, które można użyć Identyfikatora aplikacji Azure Active Directory i X.509 certyfikatu do uwierzytelniania do magazynu kluczy **gdy aplikacja jest hostowana poza platformą Azure**. Aby uzyskać więcej informacji, zobacz [o kluczy, wpisów tajnych i certyfikatów](/azure/key-vault/about-keys-secrets-and-certificates).

> [!NOTE]
> Przy użyciu Identyfikatora aplikacji i X.509 certyfikatu jest obsługiwana dla aplikacji hostowanych na platformie Azure, zalecamy używanie [zarządzanych tożsamości dla zasobów platformy Azure](#use-managed-identities-for-azure-resources) odnośnie do hostowania aplikacji na platformie Azure. Zarządzanych tożsamości nie wymagają przechowywania certyfikatu w aplikacji lub w środowisku programistycznym.

Ta aplikacja używa przykładowych Identyfikatora aplikacji oraz X.509 certyfikatu, kiedy `#define` instrukcji na górze *Program.cs* pliku jest ustawiona na `Certificate`.

1. Utwórz archiwum PKCS #12 (*PFX*) certyfikatu. Opcje tworzenia certyfikatów obejmują [MakeCert w Windows](/windows/desktop/seccrypto/makecert) i [OpenSSL](https://www.openssl.org/).
1. Zainstaluj certyfikat w magazynie certyfikatów osobistych bieżącego użytkownika. Oznaczanie klucz jako eksportowalny jest opcjonalne. Należy pamiętać, odcisk palca certyfikatu, który jest używany w dalszej części tego procesu.
1. Eksportuj archiwum PKCS #12 (*PFX*) certyfikat jako certyfikatu z algorytmem DER (*cer*).
1. Rejestrowanie aplikacji w usłudze Azure AD (**rejestracje aplikacji**).
1. Przekaż certyfikat algorytmem DER (*cer*) do usługi Azure AD:
   1. Wybierz aplikację w usłudze Azure AD.
   1. Przejdź do **certyfikaty i klucze tajne**.
   1. Wybierz **Przekaż certyfikat** można przekazać certyfikatu, który zawiera klucz publiczny. A *cer*, *PEM*, lub *.crt* certyfikatu jest dopuszczalna.
1. Store nazwa magazynu kluczy, identyfikator aplikacji i odcisk palca certyfikatu w aplikacji *appsettings.json* pliku.
1. Przejdź do **klucza magazynów** w witrynie Azure portal.
1. Wybierz magazyn kluczy, który został utworzony w [wpisu tajnego magazynu w środowisku produkcyjnym za pomocą usługi Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) sekcji.
1. Wybierz **zasady dostępu**.
1. Wybierz **Dodaj nowe**.
1. Wybierz **Wybierz podmiot zabezpieczeń** i wybierz zarejestrowany aplikację według nazwy. Wybierz **wybierz** przycisku.
1. Otwórz **uprawnienia klucza tajnego** i zapewniają aplikacji za pomocą **uzyskać** i **listy** uprawnienia.
1. Kliknij przycisk **OK**.
1. Wybierz pozycję **Zapisz**.
1. Wdróż aplikację.

`Certificate` Przykładowa aplikacja uzyskuje jego wartości konfiguracji z `IConfigurationRoot` o takiej samej nazwie jak nazwa wpisu tajnego:

* Wartości inne niż hierarchiczne: Wartość `SecretName` są uzyskiwane z `config["SecretName"]`.
* Hierarchiczna wartości (sekcje): Użyj `:` notacji (dwukropek) lub `GetSection` — metoda rozszerzenia. Użyj jednej z tych metod można uzyskać wartości konfiguracji:
  * `config["Section:SecretName"]`
  * `config.GetSection("Section")["SecretName"]`

Certyfikat X.509 jest zarządzany przez system operacyjny. Wywołania aplikacji `AddAzureKeyVault` przy użyciu wartości dostarczone przez *appsettings.json* pliku:

[!code-csharp[](key-vault-configuration/sample/Program.cs?name=snippet1&highlight=20-23)]

Przykładowe wartości:

* Nazwa magazynu kluczy: `contosovault`
* Identyfikator aplikacji: `627e911e-43cc-61d4-992e-12db9c81b413`
* Odcisk palca certyfikatu: `fe14593dd66b2406c5269d742d04b6e1ab03adb1`

*appsettings.json*:

[!code-json[](key-vault-configuration/sample/appsettings.json)]

Po uruchomieniu aplikacji, strony sieci Web wyświetlane są załadowane wartości klucza tajnego. W środowisku programistycznym wartościami wpisów tajnych załadować dane przy użyciu `_dev` sufiks. W środowisku produkcyjnym wartości załadować dane przy użyciu `_prod` sufiks.

## <a name="use-managed-identities-for-azure-resources"></a>Użyj tożsamości zarządzanych zasobów platformy Azure

**Aplikacja wdrożona na platformie Azure** korzystać z zalet [zarządzanych tożsamości dla zasobów platformy Azure](/azure/active-directory/managed-identities-azure-resources/overview), co pozwala aplikacji na uwierzytelnianie za pomocą usługi Azure Key Vault przy użyciu uwierzytelniania usługi Azure AD bez poświadczeń (identyfikator aplikacji i Klucz tajny Password/Client) przechowywanych w aplikacji.

Przykładowa aplikacja korzysta z tożsamości zarządzanego dla zasobów platformy Azure po `#define` instrukcji na górze *Program.cs* pliku jest ustawiona na `Managed`.

Wprowadź nazwę magazynu z aplikacją *appsettings.json* pliku. Przykładowa aplikacja nie wymaga Identyfikatora aplikacji i hasło (klucz tajny klienta), po ustawieniu `Managed` wersji, dzięki czemu można zignorować te pozycje konfiguracji. Aplikacja jest wdrożona na platformie Azure, a platforma Azure uwierzytelnia aplikacji dostępu do usługi Azure Key Vault tylko przy użyciu nazwy magazynu przechowywanych w *appsettings.json* pliku.

Wdróż przykładową aplikację w usłudze Azure App Service.

Wdrożenie aplikacji w usłudze Azure App Service jest automatycznie rejestrowane w usłudze Azure AD podczas tworzenia usługi. Uzyskaj identyfikator obiektu z wdrożenia do użycia w następującym poleceniu. Identyfikator obiektu są wyświetlane w witrynie Azure portal na **tożsamości** panelu usługi App Service.

Przy użyciu wiersza polecenia platformy Azure i identyfikator obiektu aplikacji, zapewnić aplikacji za pomocą `list` i `get` uprawnienia do dostępu do magazynu kluczy:

```console
az keyvault set-policy --name '{KEY VAULT NAME}' --object-id {OBJECT ID} --secret-permissions get list
```

**Uruchom ponownie aplikację** przy użyciu wiersza polecenia platformy Azure, programu PowerShell lub witryny Azure portal.

Przykładowa aplikacja:

* Tworzy wystąpienie `AzureServiceTokenProvider` klasy bez parametrów połączenia. Gdy parametry połączenia nie jest podany, dostawca próbuje uzyskać token dostępu z tożsamości zarządzanego dla zasobów platformy Azure.
* Nowy `KeyVaultClient` jest tworzona przy użyciu `AzureServiceTokenProvider` wystąpienie tokenu wywołania zwrotnego.
* `KeyVaultClient` Wystąpienie jest używane z domyślną implementację elementu `IKeyVaultSecretManager` , ładuje wszystkie wartości klucza tajnego i zastępuje kresek podwójnej precyzji (`--`) z dwukropkiem (`:`) nazwy kluczy.

[!code-csharp[](key-vault-configuration/sample/Program.cs?name=snippet2&highlight=13-21)]

Po uruchomieniu aplikacji, strony sieci Web wyświetlane są załadowane wartości klucza tajnego. W środowisku programistycznym wartościami wpisów tajnych mają `_dev` sufiksu domeny, ponieważ są one udostępniane przez wpisami tajnymi użytkowników. W środowisku produkcyjnym wartości załadować dane przy użyciu `_prod` sufiksu domeny, ponieważ są one udostępniane przez usługę Azure Key Vault.

Jeśli zostanie wyświetlony `Access denied` błąd, upewnij się, że aplikacja jest zarejestrowane w usłudze Azure AD i dostępu do magazynu kluczy. Upewnij się, że został uruchomiony ponownie usługi na platformie Azure.

## <a name="use-a-key-name-prefix"></a>Użyj prefiksu nazwy klucza

`AddAzureKeyVault` udostępnia przeciążenie, które akceptuje implementację `IKeyVaultSecretManager`, co pozwala na kontrolowanie sposobu klucza magazynu kluczy tajnych są konwertowane na klucze konfiguracji. Na przykład można zaimplementować interfejs służący do ładowania wartościami wpisów tajnych na podstawie wartości prefiks, który podajesz podczas uruchamiania aplikacji. Dzięki temu można na przykład, aby ładować wpisy tajne w oparciu o wersję aplikacji.

> [!WARNING]
> Nie używaj prefiksy na wpisy tajne usługi key vault, umieść wpisów tajnych dla wielu aplikacji w tym samym magazynie kluczy lub umieść środowiska wpisy tajne (na przykład *rozwoju* a *produkcji* wpisy tajne) w tej samej Magazyn. Zalecane jest różnych aplikacji i środowisk tworzenia/produkcyjnych użycie oddzielnych magazynów kluczy do izolowania aplikacji środowiska na najwyższy poziom zabezpieczeń.

W poniższym przykładzie wpisu tajnego jest określana w klucza magazynu (i przy użyciu narzędzia menedżera klucz tajny dla środowiska programistycznego) dla `5000-AppSecret` (kropki nie są dozwolone w nazwach wpisu tajnego usługi key vault). Ten wpis tajny reprezentuje klucz tajny aplikacji dla 5.0.0.0 wersję aplikacji. Dla innej wersji aplikacji, 5.1.0.0, klucz tajny jest dodawany do klucza magazynu (i przy użyciu narzędzia menedżera klucz tajny) dla `5100-AppSecret`. Każda wersja aplikacji ładuje jego wersjonowany wartość wpisu tajnego do jego konfigurację jako `AppSecret`, oddzielającego off wersji podczas ładowania klucza tajnego.

`AddAzureKeyVault` jest wywoływana z niestandardowego `IKeyVaultSecretManager`:

[!code-csharp[](key-vault-configuration/sample_snapshot/Program.cs?highlight=30-34)]

`IKeyVaultSecretManager` Implementacji reaguje na prefiksy wersji wpisów tajnych można załadować odpowiedniego wpisu tajnego do konfiguracji:

[!code-csharp[](key-vault-configuration/sample_snapshot/Startup.cs?name=snippet1)]

`Load` Metoda jest wywoływana przez algorytm dostawcy, który iteruje po magazyn kluczy tajnych wyszukać te, które mają prefiks wersji. Gdy prefiks wersji znajduje się za pomocą `Load`, algorytm używa `GetKey` metodę, aby zwrócić nazwę konfiguracji nazwa wpisu tajnego. On paski Wyłącz prefiks wersji od nazwy klucza tajnego i zwraca pozostałe Nazwa wpisu tajnego do załadowania do konfiguracji aplikacji pary nazwa wartość.

Gdy ta metoda jest zaimplementowana:

1. Wersja aplikacji określony w pliku projektu aplikacji. W poniższym przykładzie ustawiono wersję aplikacji `5.0.0.0`:

   ```xml
   <PropertyGroup>
     <Version>5.0.0.0</Version>
   </PropertyGroup>
   ```

1. Upewnij się, że `<UserSecretsId>` właściwość znajduje się w pliku projektu aplikacji, gdzie `{GUID}` jest identyfikatorem GUID, dostarczone przez użytkownika:

   ```xml
   <PropertyGroup>
     <UserSecretsId>{GUID}</UserSecretsId>
   </PropertyGroup>
   ```

   Zapisz następujące wpisy tajne lokalnie za pomocą [narzędzie Menedżer klucz tajny](xref:security/app-secrets):

   ```console
   dotnet user-secrets set "5000-AppSecret" "5.0.0.0_secret_value_dev"
   dotnet user-secrets set "5100-AppSecret" "5.1.0.0_secret_value_dev"
   ```

1. Klucze tajne są zapisywane w usłudze Azure Key Vault przy użyciu następujących poleceń interfejsu wiersza polecenia platformy Azure:

   ```console
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "5000-AppSecret" --value "5.0.0.0_secret_value_prod"
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "5100-AppSecret" --value "5.1.0.0_secret_value_prod"
   ```

1. Gdy aplikacja jest uruchamiana, wpisy tajne usługi key vault są ładowane. Klucz tajny ciągu dla `5000-AppSecret` pasuje do wersji aplikacji określonej w pliku projektu aplikacji (`5.0.0.0`).

1. Wersja, `5000` (przy użyciu dash), jest usuwany z nazwą klucza. W całej aplikacji odczytywanie konfiguracji przy użyciu klucza `AppSecret` ładuje wartość wpisu tajnego.

1. Jeśli wersja aplikacji została zmieniona w pliku projektu do `5.1.0.0` i aplikacja jest uruchamiana ponownie, jest zwracana wartość wpisu tajnego `5.1.0.0_secret_value_dev` w środowisku deweloperskim i `5.1.0.0_secret_value_prod` w środowisku produkcyjnym.

> [!NOTE]
> Możesz również podać własne `KeyVaultClient` implementacji `AddAzureKeyVault`. Klient niestandardowy umożliwia udostępnianie jedno wystąpienie klienta w aplikacji.

## <a name="bind-an-array-to-a-class"></a>Powiąż tablicę do klasy

Dostawca jest zdolny do odczytywania wartości konfiguracji do tablicy, do powiązania z macierzą POCO.

Podczas odczytywania ze źródła konfiguracji, która umożliwia klucze, aby zawierać dwukropek (`:`) separatory, liczbowych segment klucza jest używane do odróżniania klucze, które tworzą tablicę (`:0:`, `:1:`,... `:{n}:`). Aby uzyskać więcej informacji, zobacz [konfiguracji: Powiąż tablicę do klasy](xref:fundamentals/configuration/index#bind-an-array-to-a-class).

Usługa Azure Key Vault klucze nie można użyć dwukropek jako separatora. Podejście opisane w tym temacie używa podwójnego kreski (`--`) jako separatora hierarchiczne wartości (sekcji). Tablicy klucze są przechowywane w usłudze Azure Key Vault, przy użyciu podwójnych kresek i numeryczne kluczowe segmenty (`--0--`, `--1--`, &hellip; `--{n}--`).

Sprawdź następujące [Serilog](https://serilog.net/) rejestrowania konfigurację dostawcy udostępniane przez plik w formacie JSON. Istnieją dwa obiektu literały zdefiniowane w `WriteTo` tablica, która odzwierciedla dwóch Serilog *wychwytywanie*, opisywać miejsca docelowe danych wyjściowych rejestrowania:

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

Konfiguracji przedstawionej w poprzednim pliku JSON jest przechowywany w usłudze Azure Key Vault przy użyciu Podwójna kreska (`--`) notacją i numeryczne segmenty:

| Key | Wartość |
| --- | ----- |
| `Serilog--WriteTo--0--Name` | `AzureTableStorage` |
| `Serilog--WriteTo--0--Args--storageTableName` | `logs` |
| `Serilog--WriteTo--0--Args--connectionString` | `DefaultEnd...ountKey=Eby8...GMGw==` |
| `Serilog--WriteTo--1--Name` | `AzureDocumentDB` |
| `Serilog--WriteTo--1--Args--endpointUrl` | `https://contoso.documents.azure.com:443` |
| `Serilog--WriteTo--1--Args--authorizationKey` | `Eby8...GMGw==` |

## <a name="reload-secrets"></a>Załaduj ponownie wpisy tajne

Klucze tajne są buforowane do momentu `IConfigurationRoot.Reload()` jest wywoływana. Wygasłe, wyłączona, i zaktualizowanych wpisów tajnych w magazynie kluczy nie są przestrzegane przez aplikację do momentu `Reload` jest wykonywany.

```csharp
Configuration.Reload();
```

## <a name="disabled-and-expired-secrets"></a>Wyłączone lub wygasłe klucze tajne

Wyłączone lub wygasłe klucze tajne throw `KeyVaultClientException`. Aby zapobiec sytuacji, w której aplikacja zostanie zgłoszony, zastąpić aplikację, lub zaktualizuj wyłączone/wygasły klucz tajny.

## <a name="troubleshoot"></a>Rozwiązywanie problemów

Gdy aplikacja zakończy się niepowodzeniem, można załadować konfiguracji przy użyciu dostawcy, komunikat o błędzie zostanie zapisany [infrastruktury platformy ASP.NET Core rejestrowania](xref:fundamentals/logging/index). Konfiguracja ładowanie uniemożliwia następujące warunki:

* Aplikacja lub certyfikat nie jest poprawnie skonfigurowany w usłudze Azure Active Directory.
* Magazyn kluczy nie istnieje w usłudze Azure Key Vault.
* Aplikacji nie jest upoważniony do dostępu do magazynu kluczy.
* Zasady dostępu nie zawiera `Get` i `List` uprawnienia.
* W usłudze key vault dane konfiguracji (pary nazwa wartość) niepoprawnie nosi nazwę, Brak, wyłączone lub wygasła.
* Aplikacja ma nazwę niewłaściwego magazynu kluczy (`KeyVaultName`), identyfikator aplikacji w usłudze Azure AD (`AzureADApplicationId`), lub odcisk palca certyfikatu usługi Azure AD (`AzureADCertThumbprint`).
* Klucz konfiguracji (nazwa) jest niepoprawny w aplikacji dla wartości, które próbujesz załadować.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:fundamentals/configuration/index>
* [Microsoft Azure: Magazyn kluczy](https://azure.microsoft.com/services/key-vault/)
* [Microsoft Azure: Dokumentacja usługi Key Vault](/azure/key-vault/)
* [Jak Generowanie i przenoszenie chronionego przez moduł HSM kluczy dla usługi Azure Key Vault](/azure/key-vault/key-vault-hsm-protected-keys)
* [Klasa KeyVaultClient](/dotnet/api/microsoft.azure.keyvault.keyvaultclient)
* [Szybki start: Ustawianie i pobieranie wpisu tajnego z usługi Azure Key Vault za pomocą aplikacji internetowej platformy .NET](/azure/key-vault/quick-create-net)
* [Samouczek: Jak używać usługi Azure Key Vault za pomocą maszyny wirtualnej Windows Azure na platformie .NET](/azure/key-vault/tutorial-net-windows-virtual-machine)
