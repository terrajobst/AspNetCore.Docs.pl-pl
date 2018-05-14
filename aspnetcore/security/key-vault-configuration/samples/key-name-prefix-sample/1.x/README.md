# <a name="prefix-key-vault-configuration-provider-sample-application-aspnet-core-1x"></a>Prefiks klucza magazynu konfiguracji dostawcy aplikacji przykładowej (platformy ASP.NET Core 1.x)

Ten przykład przedstawia użycie dostawcy konfiguracji magazynu kluczy Azure dla platformy ASP.NET Core 1.x przy użyciu nazwy klucza prefiksy. Dla przykładu 2.x platformy ASP.NET Core, zobacz [prefiksu dostawcy konfiguracji magazynu kluczy przykładowej aplikacji (platformy ASP.NET Core 2.x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/key-name-prefix-sample/2.x).

> [!NOTE]
> Dostawca konfiguracji nie jest dostępna dla platformy ASP.NET Core 1.0. Jeśli aplikacja to aplikacja platformy ASP.NET Core 1.0 Aby zaimplementować dostawcę konfiguracji, należy uaktualnić aplikację pierwszy 1.1 lub nowszej.

Aby uzyskać więcej informacji na działanie próbki, zobacz [dostawcę konfiguracji usługi Azure Key Vault](xref:security/key-vault-configuration) tematu.

## <a name="using-the-sample"></a>Przy użyciu próbki
1. Tworzenie magazynu kluczy i konfigurowanie usługi Azure Active Directory (Azure AD) dla aplikacji, postępując zgodnie ze wskazówkami w [wprowadzenie do usługi Azure Key Vault](https://azure.microsoft.com/documentation/articles/key-vault-get-started/).
   * Dodaj kluczy tajnych w magazynie kluczy, przy użyciu modułu programu Azure PowerShell, interfejsu API zarządzania platformy Azure lub w portalu Azure. Hasła są tworzone jako *ręcznego* lub *certyfikatu* kluczy tajnych. *Certyfikat* kluczy tajnych są certyfikaty na potrzeby używania przez aplikacje i usługi, ale nie są obsługiwane przez dostawcę konfiguracji. Należy używać *ręcznego* opcję, aby utworzyć klucze tajne pary nazwa wartość do użycia z dostawcą konfiguracji.
     * Użyj wartości hierarchicznych (sekcji konfiguracji) `--` (dwa łączniki) jako separatora.
     * Dla przykładowej aplikacji, należy utworzyć dwa *ręcznego* kluczy tajnych z następujących par nazwa wartość:
       * `5000-AppSecret`: `5.0.0.0_secret_value`
       * `5100-AppSecret`: `5.1.0.0_secret_value`
   * Zarejestruj przykładowej aplikacji w usłudze Azure Active Directory.
   * Zezwolić aplikacji na dostęp do magazynu kluczy. Jeśli używasz `Set-AzureRmKeyVaultAccessPolicy` polecenia cmdlet programu PowerShell, aby zezwolić aplikacji na dostęp do magazynu kluczy, podaj `List` i `Get` dostęp do kluczy tajnych z `-PermissionsToSecrets list,get`.

2. Aktualizowanie aplikacji *appsettings.json* pliku z wartościami `Vault`, `ClientId`, i `ClientSecret`.
3. Uruchamianie przykładowej aplikacji, która uzyskuje jego wartości konfiguracji z `IConfigurationRoot` z taką samą nazwę jak prefiksem nazwa klucza tajnego. W tym przykładzie prefiks jest wersja aplikacji, który dostarczony do `PrefixKeyVaultSecretManager` po dodaniu dostawcę konfiguracji usługi Azure Key Vault. Wartość `AppSecret` są uzyskiwane z `config["AppSecret"]`.
4. Zmień wersję zestawu aplikacji w pliku projektu z `5.0.0.0` do `5.1.0.0` i ponownie uruchom aplikację. Teraz, tajne wartość zwracana jest `5.1.0.0_secret_value`.
