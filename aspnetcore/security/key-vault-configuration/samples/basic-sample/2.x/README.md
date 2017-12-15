# <a name="key-vault-configuration-provider-sample-application-aspnet-core-2x"></a>Dostawca konfiguracji magazynu kluczy przykładowej aplikacji (platformy ASP.NET Core 2.x)

Ten przykład przedstawia użycie dostawcy konfiguracji magazynu kluczy Azure dla platformy ASP.NET Core 2.x. Dla przykładu 1.x platformy ASP.NET Core, zobacz [dostawcy konfiguracji magazynu kluczy przykładowej aplikacji (platformy ASP.NET Core 1.x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/basic-sample/1.x).

Aby uzyskać więcej informacji na działanie próbki, zobacz [dostawcę konfiguracji usługi Azure Key Vault](xref:security/key-vault-configuration) tematu.

## <a name="using-the-sample"></a>Przy użyciu próbki
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
