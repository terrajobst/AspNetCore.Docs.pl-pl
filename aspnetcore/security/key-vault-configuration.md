---
title: Dostawca konfiguracji usługi Azure Key Vault w ASP.NET Core
author: guardrex
description: Dowiedz się, jak skonfigurować aplikację przy użyciu pary nazwa wartość załadowane w czasie wykonywania przy użyciu dostawcy konfiguracji magazynu kluczy Azure.
manager: wpickett
ms.author: riande
ms.date: 08/09/2017
ms.prod: asp.net-core
ms.topic: article
uid: security/key-vault-configuration
ms.openlocfilehash: 78a00e04e260863af17d7888ca6bf77d3f915ce1
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/03/2018
---
# <a name="azure-key-vault-configuration-provider-in-aspnet-core"></a>Dostawca konfiguracji usługi Azure Key Vault w ASP.NET Core

Przez [Luke Latham](https://github.com/guardrex) i [Andrew Stanton pielęgniarki](https://github.com/anurse)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Wyświetl lub pobrać przykładowy kod 2.x:

* [Podstawowy przykład](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/basic-sample/2.x) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))-odczytuje wartości tajnych w aplikacji.
* [Przykład prefiks nazwy klucza](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/key-name-prefix-sample/2.x) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample)) — odczyty wartości tajny przy użyciu prefiks nazwy klucza, który reprezentuje wersji aplikacji, dzięki czemu można załadować zestaw tajny wartości dla każdej wersji aplikacji.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Wyświetl lub pobrać przykładowy kod 1.x:

* [Podstawowy przykład](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/basic-sample/1.x) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))-odczytuje wartości tajnych w aplikacji.
* [Przykład prefiks nazwy klucza](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/key-name-prefix-sample/1.x) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample)) — odczyty wartości tajny przy użyciu prefiks nazwy klucza, który reprezentuje wersji aplikacji, dzięki czemu można załadować zestaw tajny wartości dla każdej wersji aplikacji. 

---

W tym dokumencie opisano sposób użycia [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) dostawcę konfiguracji, aby załadować wartości konfiguracji aplikacji z usługi Azure Key Vault kluczy tajnych. Usługa Azure Key Vault jest oparta na chmurze usługi, która pomaga w zabezpieczaniu kluczy kryptograficznych i kluczy tajnych używanych przez aplikacje i usługi. Typowe scenariusze obejmują kontrolę dostępu do danych poufnych konfiguracji i weryfikacja spełniających wymaganie FIPS 140-2 poziom 2 sprzętowych modułów zabezpieczeń (HSM) podczas zapisywania danych konfiguracji. Ta funkcja jest dostępna dla aplikacji przeznaczonych dla platformy ASP.NET Core 1.1 lub nowszej.

## <a name="package"></a>Package
Aby użyć dostawcy, Dodaj odwołanie do [Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) pakietu.

## <a name="application-configuration"></a>Konfiguracja aplikacji
Można eksplorować dostawcy o [przykładowe aplikacje](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples). Po ustanowić magazyn kluczy i tworzenia kluczy tajnych w magazynie przykładowych aplikacji bezpiecznego załadować tajny wartości do ich konfiguracji i wyświetlać na stronach sieci Web.

Dostawca jest dodawany do `ConfigurationBuilder` z `AddAzureKeyVault` rozszerzenia. W przykładowych aplikacji rozszerzenie używa trzech wartości konfiguracji załadowane z *appsettings.json* pliku.

| Ustawienia aplikacji    | Opis                    | Przykład                                      |
| -------------- | ------------------------------ | -------------------------------------------- |
| `Vault`        | Nazwa magazynu kluczy Azure           | contosovault                                 |
| `ClientId`     | Identyfikator aplikacji usługi Azure Active Directory  | 627e911e-43cc-61d4-992e-12db9c81b413         |
| `ClientSecret` | Klucz aplikacji usługi Azure Active Directory | g58K3dtg59o1Pa+e59v2Tx829w6VxTB2yv9sv/101di= |

[!code-csharp[Program](key-vault-configuration/samples/basic-sample/2.x/Program.cs?name=snippet1&highlight=2,7-10)]

## <a name="creating-key-vault-secrets-and-loading-configuration-values-basic-sample"></a>Tworzenie kluczy tajnych w magazynie kluczy i ładowanie wartości konfiguracji (basic przykład)
1. Tworzenie magazynu kluczy i konfigurowanie usługi Azure Active Directory (Azure AD) dla aplikacji, postępując zgodnie ze wskazówkami w [wprowadzenie do usługi Azure Key Vault](https://azure.microsoft.com/documentation/articles/key-vault-get-started/).
   * Dodawanie kluczy tajnych przy użyciu magazynu kluczy [modułu PowerShell magazynu klucz AzureRM](/powershell/module/azurerm.keyvault) dostępnej w sklepie [galerii programu PowerShell](https://www.powershellgallery.com/packages/AzureRM.KeyVault), [interfejsu API REST magazynu kluczy Azure](/rest/api/keyvault/), lub [Portalu azure](https://portal.azure.com/). Hasła są tworzone jako *ręcznego* lub *certyfikatu* kluczy tajnych. *Certyfikat* kluczy tajnych są certyfikaty na potrzeby używania przez aplikacje i usługi, ale nie są obsługiwane przez dostawcę konfiguracji. Należy używać *ręcznego* opcję, aby utworzyć klucze tajne pary nazwa wartość do użycia z dostawcą konfiguracji.
     * Proste kluczy tajnych są tworzone jako pary nazwa wartość. Usługa Azure Key Vault nazw kluczy tajnych są ograniczone do znaki alfanumeryczne i łączniki.
     * Użyj wartości hierarchicznych (sekcji konfiguracji) `--` (dwa łączniki) jako separator w próbce. Dwukropki, które zwykle są używane do rozdzielenia sekcji z podklucz [konfiguracji platformy ASP.NET Core](xref:fundamentals/configuration/index), nie są dozwolone w nazwach tajny. W związku z tym są używane dwa łączniki i podmienić dla dwukropek, gdy kluczy tajnych są ładowane do konfiguracji aplikacji.
     * Utwórz dwa *ręcznego* kluczy tajnych z następujących par nazwa wartość. Pierwszy klucz tajny jest proste nazwy i wartości, a drugi klucz tajny tworzy wartość tajna z sekcji i podklucz nazwa klucza tajnego:
       * `SecretName`: `secret_value_1`
       * `Section--SecretName`: `secret_value_2`
   * Zarejestruj przykładowej aplikacji w usłudze Azure Active Directory.
   * Zezwolić aplikacji na dostęp do magazynu kluczy. Jeśli używasz `Set-AzureRmKeyVaultAccessPolicy` polecenia cmdlet programu PowerShell, aby zezwolić aplikacji na dostęp do magazynu kluczy, podaj `List` i `Get` dostęp do kluczy tajnych z `-PermissionsToSecrets list,get`.

2. Aktualizowanie aplikacji *appsettings.json* pliku z wartościami `Vault`, `ClientId`, i `ClientSecret`.
3. Uruchamianie przykładowej aplikacji, która uzyskuje jego wartości konfiguracji z `IConfigurationRoot` o takiej samej nazwie jak nazwa klucza tajnego.
   * Inne niż hierarchicznych wartości: wartość `SecretName` są uzyskiwane z `config["SecretName"]`.
   * Hierarchiczna wartości (sekcje): Użyj `:` notacji (dwukropek) lub `GetSection` — metoda rozszerzenia. Użyj jednej z tych metod można uzyskać wartości konfiguracji:
     * `config["Section:SecretName"]`
     * `config.GetSection("Section")["SecretName"]`

Po uruchomieniu aplikacji załadowanych tajny wartości wyświetlane strony sieci Web:

![Okno przeglądarki zawierające poufne wartości załadowanego za pomocą dostawcy konfiguracji magazynu kluczy Azure](key-vault-configuration/_static/sample1.png)

## <a name="creating-prefixed-key-vault-secrets-and-loading-configuration-values-key-name-prefix-sample"></a>Tworzenie kluczy tajnych prefiksem magazynu kluczy i ładowanie wartości konfiguracji (klucz name prefiks sample)
`AddAzureKeyVault` dostępne są także przeciążenia, które akceptuje implementacja `IKeyVaultSecretManager`, co umożliwia kontrolowanie sposobu klucza magazynu kluczy tajnych są konwertowane na klucze konfiguracji. Na przykład można zaimplementować interfejsu załadować tajny wartości na podstawie wartości prefiksu podane podczas uruchamiania aplikacji. Dzięki temu można na przykład można załadować kluczy tajnych na podstawie wersji aplikacji.

> [!WARNING]
> Nie używaj prefiksy na kluczy tajnych w magazynie kluczy, umieść klucze tajne dla wielu aplikacji w tym samym magazynie kluczy lub umieść środowiska klucze tajne (na przykład *programowanie* i *produkcji* kluczy tajnych) w taki sam Magazyn. Zalecane jest różnych aplikacji i środowisk produkcyjnych/programowanie użycie oddzielnych magazynów kluczy do izolowania aplikacji środowisk najwyższego poziomu zabezpieczeń.

Za pomocą drugiego przykładowej aplikacji, Utwórz klucz tajny w magazynie kluczy dla `5000-AppSecret` (kropki nie są dozwolone w nazwach tajny magazynu kluczy) reprezentujący klucz tajny aplikacji dla wersji 5.0.0.0 aplikacji. Inna wersja 5.1.0.0, można utworzyć klucz tajny dla `5100-AppSecret`. Każda wersja aplikacji ładuje poufną wartość do jego konfigurację jako `AppSecret`, oddzielającego poza wersji, ładuje klucz tajny. Poniżej przedstawiono przykładowe zastosowanie:

[!code-csharp[Configuration builder](key-vault-configuration/samples/key-name-prefix-sample/2.x/Program.cs?name=snippet1&highlight=12)]

[!code-csharp[PrefixKeyVaultSecretManager](key-vault-configuration/samples/key-name-prefix-sample/2.x/Startup.cs?name=snippet1)]

`Load` Metoda jest wywoływana przez algorytm dostawcy, który iteruje po kluczy tajnych magazynu, aby znaleźć te, które mają prefiks wersji. Gdy znaleziono prefiksu wersji o `Load`, algorytm używa `GetKey` metodę, aby zwrócić nazwę konfiguracji nazwa klucza tajnego. Usuwa poza prefiks wersji od nazwy klucza tajnego, a zwraca reszty nazwa klucza tajnego dla obciążenia w konfiguracji aplikacji pary nazwa wartość.

Po zaimplementowaniu tej metody:

1. Załadowano kluczy tajnych magazynu kluczy.
2. Klucz tajny ciąg dla `5000-AppSecret` dopasowaniu.
3. Wersja `5000` (na pauzy), jest usuwany poza nazwę klucza, pozostawiając `AppSecret` załadować z wartością tajnego do konfiguracji aplikacji.

> [!NOTE]
> Można też podać własne `KeyVaultClient` wykonania `AddAzureKeyVault`. Dostarczanie niestandardowego klienta umożliwia udostępnianie jedno wystąpienie klienta między dostawcy konfiguracji i innymi częściami pakietu aplikacji.

1. Tworzenie magazynu kluczy i konfigurowanie usługi Azure Active Directory (Azure AD) dla aplikacji, postępując zgodnie ze wskazówkami w [wprowadzenie do usługi Azure Key Vault](https://azure.microsoft.com/documentation/articles/key-vault-get-started/).
   * Dodawanie kluczy tajnych przy użyciu magazynu kluczy [modułu PowerShell magazynu klucz AzureRM](/powershell/module/azurerm.keyvault) dostępnej w sklepie [galerii programu PowerShell](https://www.powershellgallery.com/packages/AzureRM.KeyVault), [interfejsu API REST magazynu kluczy Azure](/rest/api/keyvault/), lub [Portalu azure](https://portal.azure.com/). Hasła są tworzone jako *ręcznego* lub *certyfikatu* kluczy tajnych. *Certyfikat* kluczy tajnych są certyfikaty na potrzeby używania przez aplikacje i usługi, ale nie są obsługiwane przez dostawcę konfiguracji. Należy używać *ręcznego* opcję, aby utworzyć klucze tajne pary nazwa wartość do użycia z dostawcą konfiguracji.
     * Użyj wartości hierarchicznych (sekcji konfiguracji) `--` (dwa łączniki) jako separatora.
     * Utwórz dwa *ręcznego* kluczy tajnych z następujących par nazwa wartość:
       * `5000-AppSecret`: `5.0.0.0_secret_value`
       * `5100-AppSecret`: `5.1.0.0_secret_value`
   * Zarejestruj przykładowej aplikacji w usłudze Azure Active Directory.
   * Zezwolić aplikacji na dostęp do magazynu kluczy. Jeśli używasz `Set-AzureRmKeyVaultAccessPolicy` polecenia cmdlet programu PowerShell, aby zezwolić aplikacji na dostęp do magazynu kluczy, podaj `List` i `Get` dostęp do kluczy tajnych z `-PermissionsToSecrets list,get`.

2. Aktualizowanie aplikacji *appsettings.json* pliku z wartościami `Vault`, `ClientId`, i `ClientSecret`.
3. Uruchamianie przykładowej aplikacji, która uzyskuje jego wartości konfiguracji z `IConfigurationRoot` z taką samą nazwę jak prefiksem nazwa klucza tajnego. W tym przykładzie prefiks jest wersja aplikacji, który dostarczony do `PrefixKeyVaultSecretManager` po dodaniu dostawcę konfiguracji usługi Azure Key Vault. Wartość `AppSecret` są uzyskiwane z `config["AppSecret"]`. Strony sieci Web wygenerowany przez aplikację przedstawia załadowana wartość:

   ![Okno przeglądarki, przedstawiające wartość tajna załadowanego za pomocą dostawcy konfiguracji magazynu kluczy Azure, gdy 5.0.0.0 jest wersja aplikacji](key-vault-configuration/_static/sample2-1.png)

4. Zmień wersję zestawu aplikacji w pliku projektu z `5.0.0.0` do `5.1.0.0` i ponownie uruchom aplikację. Teraz, tajne wartość zwracana jest `5.1.0.0_secret_value`. Strony sieci Web wygenerowany przez aplikację przedstawia załadowana wartość:

   ![Okno przeglądarki, przedstawiające wartość tajna załadowanego za pomocą dostawcy konfiguracji magazynu kluczy Azure, gdy 5.1.0.0 jest wersja aplikacji](key-vault-configuration/_static/sample2-2.png)

## <a name="controlling-access-to-the-clientsecret"></a>Kontrolowanie dostępu do ClientSecret
Użyj [narzędzie Menedżer klucz tajny](xref:security/app-secrets) do obsługi `ClientSecret` poza drzewa źródła z projektu. Za pomocą Menedżera klucz tajny skojarzyć klucze tajne aplikacji z określonego projektu i udostępniać je w wielu projektach.

Podczas opracowywania aplikacji .NET Framework, w środowisku, które obsługuje certyfikaty, można uwierzytelniać do usługi Azure Key Vault za pomocą certyfikatu X.509. Klucz prywatny certyfikatu X.509 jest zarządzana przez system operacyjny. Aby uzyskać więcej informacji, zobacz [uwierzytelniania przy użyciu certyfikatu zamiast klucz tajny klienta](https://docs.microsoft.com/azure/key-vault/key-vault-use-from-web-application#authenticate-with-a-certificate-instead-of-a-client-secret). Użyj `AddAzureKeyVault` przeciążenia, które akceptuje `X509Certificate2`.

```csharp
var store = new X509Store(StoreLocation.CurrentUser);
store.Open(OpenFlags.ReadOnly);
var cert = store.Certificates.Find(X509FindType.FindByThumbprint, config["CertificateThumbprint"], false);

builder.AddAzureKeyVault(
    config["Vault"],
    config["ClientId"],
    cert.OfType<X509Certificate2>().Single(),
    new EnvironmentSecretManager(env.ApplicationName));
store.Close();

Configuration = builder.Build();
```

## <a name="reloading-secrets"></a>Ponowne ładowanie kluczy tajnych
Hasła są buforowane do momentu `IConfigurationRoot.Reload()` jest wywoływana. Ważność, wyłączona, i zaktualizowane kluczy tajnych w magazynie kluczy nie są przestrzegane przez aplikację do `Reload` jest wykonywana.

```csharp
Configuration.Reload();
```

## <a name="disabled-and-expired-secrets"></a>Wyłączone i wygasłego hasła
Wyłączone i wygasłe kluczy tajnych throw `KeyVaultClientException`. Aby zapobiec zgłaszanie aplikacji, aplikację zamienić lub zaktualizować klucza tajnego wyłączone/wygasły.

## <a name="troubleshooting"></a>Rozwiązywanie problemów
Gdy aplikacja nie może załadować konfiguracji przy użyciu dostawcy, komunikat o błędzie zostanie zapisany [rejestrowanie platformy ASP.NET infrastruktury](xref:fundamentals/logging/index). Uniemożliwia konfiguracji ładowanie następujące warunki:
* Aplikacja nie jest poprawnie skonfigurowany w usłudze Azure Active Directory.
* Magazyn kluczy nie istnieje w usłudze Azure Key Vault.
* Aplikacja nie ma uprawnień dostępu magazynu kluczy.
* Zasady dostępu nie zawiera `Get` i `List` uprawnienia.
* W magazynie kluczy dane konfiguracji (pary nazwa wartość) jest niepoprawnie o nazwie, brak wyłączone lub wygasł.
* Aplikacja ma nazwę niewłaściwego magazynu kluczy (`Vault`), identyfikator aplikacji usługi Azure AD (`ClientId`), lub klucza usługi Azure AD (`ClientSecret`).
* Klucza usługi Azure AD (`ClientSecret`) wygasł.
* Klucz konfiguracji (nazwa) jest niepoprawny w aplikacji dla wartości, które próbujesz załadować.

## <a name="additional-resources"></a>Dodatkowe zasoby
* [Konfiguracja](xref:fundamentals/configuration/index)
* [Platformy Microsoft Azure: Magazyn kluczy](https://azure.microsoft.com/services/key-vault/)
* [Platformy Microsoft Azure: Dokumentacja magazynu kluczy](https://docs.microsoft.com/azure/key-vault/)
* [Jak Generowanie i przenoszenie chronionego przez moduł HSM kluczy dla usługi Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-hsm-protected-keys)
* [Klasa KeyVaultClient](/dotnet/api/microsoft.azure.keyvault.keyvaultclient)
