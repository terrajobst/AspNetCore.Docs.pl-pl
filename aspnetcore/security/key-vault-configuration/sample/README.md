# <a name="key-vault-configuration-provider-sample-app"></a>Przykładowa aplikacja do dostawcy konfiguracji usługi Key Vault

Ten przykład ilustruje sposób używania dostawcy usługi Azure Key Vault konfiguracji.

Przykład działa w jednym z dwóch trybów ustalany na podstawie `#define` instrukcji na górze *Program.cs* pliku. Aby uzyskać instrukcje, zobacz [dyrektywy preprocesora w przykładowym kodzie](https://docs.microsoft.com/aspnet/core#preprocessor-directives-in-sample-code):

* `Certificate` &ndash; Zademonstrowano użycie certyfikat X.509 i identyfikator klienta systemu Azure klucza magazynu do dostępu do kluczy tajnych przechowywanych w usłudze Azure Key Vault. Ta wersja przykładu można uruchomić z dowolnego miejsca i wdrażać w usłudze Azure App Service lub dowolnego hosta, które umożliwia obsługę aplikacji ASP.NET Core.
* `Managed` &ndash; Pokazuje sposób użycia platformy Azure [tożsamości usługi zarządzanej](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) do uwierzytelnienia aplikacji w usłudze Azure Key Vault przy użyciu uwierzytelniania usługi Azure AD bez poświadczeń w kodzie aplikacji lub konfiguracji. Klucz tajny i identyfikator klienta usługi AD Azure nie są wymagane dla aplikacji do uwierzytelniania za pomocą usługi Azure Key Vault. W tym przykładzie musi zostać wdrożony w usłudze Azure App Service, aby zapoznać się z scearnio tożsamości zarządzanej.

Aby uzyskać więcej informacji, zobacz [dostawcy konfiguracji magazynu kluczy Azure](https://docs.microsoft.com/aspnet/core/security/key-vault-configuration).
