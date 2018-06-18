---
title: Tworzenie aplikacji platformy ASP.NET Core z danych użytkownika chronione przez autoryzacji
author: rick-anderson
description: Dowiedz się, jak utworzyć aplikację Razor strony z danymi użytkownika chronione przez autoryzacji. Obejmuje HTTPS, uwierzytelnianie, zabezpieczeń, ASP.NET Core Identity.
manager: wpickett
ms.author: riande
ms.date: 01/24/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/secure-data
ms.openlocfilehash: 53cab4b72980eef47c899a22e49fa697e7497279
ms.sourcegitcommit: 9a35906446af7ffd4ccfc18daec38874b5abbef7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/18/2018
ms.locfileid: "35726007"
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a>Tworzenie aplikacji platformy ASP.NET Core z danych użytkownika chronione przez autoryzacji

Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Audette Jan](https://twitter.com/joeaudette)

W tym samouczku przedstawiono sposób tworzenia aplikacji sieci web platformy ASP.NET Core z danymi użytkownika chronione przez autoryzacji. Wyświetla listę kontaktów, które uwierzytelnionych użytkowników (zarejestrowanym) został utworzony. Istnieją trzy grupy zabezpieczeń:

* **Użytkownicy zarejestrowanych** może wyświetlać wszystkie zatwierdzone dane i może edytowanie/usuwanie własnych danych.
* **Menedżerowie** można zatwierdzić lub odrzucić dane kontaktowe. Tylko zatwierdzone kontakty są widoczne dla użytkowników.
* **Administratorzy** można zatwierdzić Odrzuć i edytowanie/usuwanie żadnych danych.

Na poniższej ilustracji użytkownik Rick (`rick@example.com`) jest zalogowany. Rick mogą jedynie wyświetlać zatwierdzonych kontaktów i **Edytuj**/**usunąć**/**Utwórz nowy** łącza do swoich kontaktów. Tylko ostatni rekord powstały Rick, wyświetla **Edytuj** i **usunąć** łącza. Inni użytkownicy zobaczą ostatniego rekordu do momentu menedżerowi lub administratorowi zmienia stan na "Zatwierdzone".

![Obraz opisane poprzedzających](secure-data/_static/rick.png)

Na poniższej ilustracji `manager@contoso.com` jest zarejestrowany i rolę menedżerów:

![Obraz opisane poprzedzających](secure-data/_static/manager1.png)

Na poniższej ilustracji przedstawiono menedżerów widoku Szczegóły kontaktu:

![Obraz opisane poprzedzających](secure-data/_static/manager.png)

**Zatwierdź** i **Odrzuć** przyciski są wyświetlane tylko dla menedżerów i administratorów.

Na poniższej ilustracji `admin@contoso.com` jest zarejestrowany i w roli administratora:

![Obraz opisane poprzedzających](secure-data/_static/admin.png)

Administrator ma wszystkie uprawnienia. Użytkownik może odczytu/edytowanie/usuwanie żadnych skontaktuj się z i Zmień stan kontaktów.

Aplikacja została utworzona przez [szkieletów](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) następujące `Contact` modelu:

[!code-csharp[](secure-data/samples/starter2/Models/Contact.cs?name=snippet1)]

Próbka zawiera poniższe obsługi autoryzacji:

* `ContactIsOwnerAuthorizationHandler`: Zapewnia, że użytkownika można edytować tylko ich danych.
* `ContactManagerAuthorizationHandler`: Umożliwia menedżerów zatwierdzić lub odrzucić kontaktów.
* `ContactAdministratorsAuthorizationHandler`: Pozwala administratorom, aby zatwierdzić lub odrzucić kontaktów i edytowanie/usuwanie kontaktów.

## <a name="prerequisites"></a>Wymagania wstępne

W tym samouczku jest zaawansowane. Należy zapoznać się z:

* [ASP.NET Core](xref:tutorials/first-mvc-app/start-mvc)
* [Uwierzytelnianie](xref:security/authentication/index)
* [Potwierdzenie konta i odzyskiwanie hasła](xref:security/authentication/accconfirm)
* [Autoryzacja](xref:security/authorization/index)
* [Entity Framework Core](xref:data/ef-mvc/intro)

Zobacz [plik PDF](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) dla wersji platformy ASP.NET Core MVC. Wersja platformy ASP.NET Core 1.1 tego samouczka jest [to](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) folderu. 1.1, na przykład ASP.NET Core jest w [przykłady](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).

## <a name="the-starter-and-completed-app"></a>Starter i ukończonej aplikacji

[Pobierz](xref:tutorials/index#how-to-download-a-sample) [ukończone](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2) aplikacji. [Test](#test-the-completed-app) ukończonej aplikacji, należy zapoznać się z jego funkcje zabezpieczeń.

### <a name="the-starter-app"></a>Początkową aplikację

[Pobierz](xref:tutorials/index#how-to-download-a-sample) [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2) aplikacji.

Uruchom aplikację, wybierz **ContactManager** link i sprawdź, tworzenie, edytowanie i usuwanie kontaktu.

## <a name="secure-user-data"></a>Zabezpieczanie danych użytkownika

Następujące sekcje zostały wszystkie główne kroki umożliwiające utworzenie aplikacji danych bezpiecznego użytkownika. Może być przydatne do odwoływania się do projektu zakończone.

### <a name="tie-the-contact-data-to-the-user"></a>Powiązanie dane kontaktowe dla użytkownika

Za pomocą programu ASP.NET [tożsamości](xref:security/authentication/identity) identyfikator użytkownika, aby zapewnić użytkownikom można edytować swoje dane, ale nie do innych danych użytkowników. Dodaj `OwnerID` i `ContactStatus` do `Contact` modelu:

[!code-csharp[](secure-data/samples/final2/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

`OwnerID` Identyfikator użytkownika z `AspNetUser` tabeli w [tożsamości](xref:security/authentication/identity) bazy danych. `Status` Pole określa, czy kontakt jest widoczny dla zwykłych użytkowników.

Tworzenie nowych migracji i aktualizacji bazy danych:

```console
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="require-https-and-authenticated-users"></a>Wymagaj protokołu HTTPS i uwierzytelnionych użytkowników

Dodaj [IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) do `Startup`:

[!code-csharp[](secure-data/samples/final2/Startup.cs?name=snippet_env)]

W `ConfigureServices` metody *Startup.cs* plików, dodawanie [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) filtr autoryzacji:

[!code-csharp[](secure-data/samples/final2/Startup.cs?name=snippet_SSL&highlight=10-999)]

Jeśli używasz programu Visual Studio, należy włączyć protokół HTTPS.

Przekierowywanie żądań HTTP, HTTPS, zobacz [ponowne zapisywanie adresów URL w oprogramowaniu pośredniczącym](xref:fundamentals/url-rewriting). Za pomocą programu Visual Studio Code lub testowania na platformie lokalnego, który nie zawiera certyfikatu testowego dla protokołu HTTPS:

  Ustaw `"LocalTest:skipHTTPS": true` w *appsettings. Developement.JSON* pliku.

### <a name="require-authenticated-users"></a>Wymaga uwierzytelnionych użytkowników

Ustaw domyślne zasady uwierzytelniania, aby wymagać od użytkowników uwierzytelniania. Można zrezygnować z uwierzytelniania na poziomie metody Razor strony, kontrolera lub akcji z `[AllowAnonymous]` atrybutu. Ustawienie domyślne zasady uwierzytelniania, aby wymagać od użytkowników uwierzytelniania chroni nowo dodanego stron Razor i kontrolerów. Posiadanie uwierzytelniania domyślnie wymagane jest bezpieczniejszy niż polegania na nowe kontrolery i stron Razor do uwzględnienia `[Authorize]` atrybutu. 

Z wymaganiami wszyscy użytkownicy uwierzytelnieni [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder?view=aspnetcore-2.0#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) i [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage?view=aspnetcore-2.0) wywołania nie są wymagane.

Aktualizacja `ConfigureServices` z następującymi zmianami:

* Komentarz `AuthorizeFolder` i `AuthorizePage`.
* Ustaw domyślne zasady uwierzytelniania, aby wymagać od użytkowników uwierzytelniania.

[!code-csharp[](secure-data/samples/final2/Startup.cs?name=snippet_defaultPolicy&highlight=23-27,31-999)]

Dodaj [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) do indeksu strony o i skontaktuj się z pomocą tak anonimowych użytkowników można uzyskać informacji o lokacji, przed ich zarejestrować. 

[!code-csharp[](secure-data/samples/final2/Pages/Index.cshtml.cs?name=snippet&highlight=2)]

Dodaj `[AllowAnonymous]` do [LoginModel i RegisterModel](https://github.com/aspnet/templating/issues/238).

### <a name="configure-the-test-account"></a>Skonfiguruj konto testu

`SeedData` Klasy tworzy dwa konta: administrator i Menedżer. Użyj [narzędzie Menedżer klucz tajny](xref:security/app-secrets) ustawienie hasła dla tych kont. Ustawianie hasła z katalogu projektu (katalog zawierający *Program.cs*):

```console
dotnet user-secrets set SeedUserPW <PW>
```

Jeśli użytkownik nie należy używać silnych haseł, jest zwracany wyjątek, kiedy `SeedData.Initialize` jest wywoływana.

Aktualizacja `Main` hasła testu:

[!code-csharp[](secure-data/samples/final2/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a>Tworzenie konta testowe i aktualizowanie kontaktów

Aktualizacja `Initialize` metoda `SeedData` klasy w celu utworzenia konta testowego:

[!code-csharp[](secure-data/samples/final2/Data/SeedData.cs?name=snippet_Initialize)]

Dodaj identyfikator użytkownika administratora i `ContactStatus` do kontaktów. Utworzyć kontaktów "Przesłane" i jednym "odrzucone". Dodawanie stanu i Identyfikatora użytkownika do wszystkich kontaktów. Wyświetlane jest tylko jeden kontakt:

[!code-csharp[](secure-data/samples/final2/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a>Utwórz właściciela, Menedżer i obsługi autoryzacji administratora

Utwórz `ContactIsOwnerAuthorizationHandler` klasy w *autoryzacji* folderu. `ContactIsOwnerAuthorizationHandler` Sprawdza, czy użytkownik, działając w zasobie jest właścicielem zasobu.

[!code-csharp[](secure-data/samples/final2/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

`ContactIsOwnerAuthorizationHandler` Wywołania [kontekstu. Pomyślnie](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) bieżący uwierzytelniony użytkownik jest właścicielem kontaktu. Programy obsługi autoryzacji zazwyczaj:

* Zwraca `context.Succeed` gdy są spełnione wymagania.
* Zwraca `Task.CompletedTask` gdy nie są spełnione wymagania. `Task.CompletedTask` jest ani powodzenie lub niepowodzenie&mdash;umożliwia innych programów obsługi autoryzacji do uruchomienia.

Jeśli musisz jawnie zakończyć się niepowodzeniem, zwróć [kontekstu. Niepowodzenie](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).

Aplikacja umożliwia kontaktu właścicieli edytowanie/usuwanie/utworzyć własne dane. `ContactIsOwnerAuthorizationHandler` nie trzeba sprawdzić działanie przekazany parametr wymaganie.

### <a name="create-a-manager-authorization-handler"></a>Tworzenie obsługi programu Menedżer autoryzacji

Utwórz `ContactManagerAuthorizationHandler` klasy w *autoryzacji* folderu. `ContactManagerAuthorizationHandler` Weryfikuje użytkownika, działając w zasobie menedżera. Tylko menedżerowie można zatwierdzić lub odrzucić zmiany zawartości (nowe lub zmienione).

[!code-csharp[](secure-data/samples/final2/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a>Utwórz program obsługi autoryzacji administratora

Utwórz `ContactAdministratorsAuthorizationHandler` klasy w *autoryzacji* folderu. `ContactAdministratorsAuthorizationHandler` Sprawdza, czy użytkownik na zasób jest administratorem. Administrator może wykonać wszystkie operacje.

[!code-csharp[](secure-data/samples/final2/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a>Rejestrowanie procedur obsługi autoryzacji

Musi być zarejestrowany przy użyciu programu Entity Framework Core Services [iniekcji zależności](xref:fundamentals/dependency-injection) przy użyciu [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions). `ContactIsOwnerAuthorizationHandler` Korzysta z platformy ASP.NET Core [tożsamości](xref:security/authentication/identity), który jest wbudowany w program Entity Framework Core. Zarejestrować obsługi z kolekcją usługi, dzięki czemu są dostępne do `ContactsController` za pośrednictwem [iniekcji zależności](xref:fundamentals/dependency-injection). Dodaj następujący kod na końcu `ConfigureServices`:

[!code-csharp[](secure-data/samples/final2/Startup.cs?name=ConfigureServices&highlight=41-999)]

`ContactAdministratorsAuthorizationHandler` i `ContactManagerAuthorizationHandler` są dodawane jako pojedynczych wystąpień. Są one pojedynczych wystąpień, ponieważ nie używają EF i wszystkich informacji potrzebnych `Context` parametr `HandleRequirementAsync` metody.

## <a name="support-authorization"></a>Obsługuje autoryzacji

W tej sekcji zaktualizuj stron Razor i Dodaj klasę wymagań operacji.

### <a name="review-the-contact-operations-requirements-class"></a>Przegląd klasy wymagań operacji kontaktów

Przegląd `ContactOperations` klasy. Ta klasa zawiera wymagania obsługuje aplikacja:

[!code-csharp[](secure-data/samples/final2/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-razor-pages"></a>Utwórz klasę podstawową dla stron Razor

Tworzenie klasy podstawowej, który zawiera usługi używane w kontaktach stron Razor. Klasa podstawowa umieszcza kod inicjowania w jednej lokalizacji:

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/DI_BasePageModel.cs)]

Poprzedni kod:

* Dodaje `IAuthorizationService` usługi z dostępem do obsługi autoryzacji.
* Dodaje tożsamość `UserManager` usługi.
* Dodaj `ApplicationDbContext`.

### <a name="update-the-createmodel"></a>Aktualizacja CreateModel

Zaktualizuj Tworzenie strony modelu konstruktora do użycia `DI_BasePageModel` klasy podstawowej:

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

Aktualizacja `CreateModel.OnPostAsync` metodę:

* Identyfikator użytkownika, aby dodać `Contact` modelu.
* Wywołuje program obsługi autoryzacji, aby sprawdzić, czy użytkownik ma uprawnienia do tworzenia kontaktów.

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a>Aktualizacja IndexModel

Aktualizacja `OnGetAsync` dlatego kontakty tylko zatwierdzone są wyświetlane użytkownikom ogólne:

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a>Aktualizacja EditModel

Dodaj program obsługi autoryzacji, aby sprawdzić, czy użytkownik jest właścicielem kontaktu. Ponieważ jest sprawdzana autoryzacji zasobów `[Authorize]` atrybut nie jest wystarczająca. Aplikacja nie ma dostępu do zasobu, gdy atrybuty są oceniane. Musi być bezwzględnie autoryzacji na podstawie zasobów. Kontroli należy wykonać, gdy aplikacja ma dostęp do zasobów przez załadowanie go w modelu strony lub przez załadowanie go w obrębie samego program obsługi. Zasób jest często używany przez przekazywanie klucz zasobu.

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a>Aktualizacja DeleteModel

Aktualizacja modelu strony Usuń na potrzeby obsługi autoryzacji Sprawdź, czy użytkownik ma uprawnienia do usuwania na kontakt.

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a>Wstaw usługi autoryzacji do widoków

Obecnie pokazuje interfejsu użytkownika edytowania i usuwania łącza do danych użytkownika nie można zmodyfikować. Interfejs użytkownika jest rozwiązany przez zastosowanie obsługi autoryzacji do widoków.

Wstaw usługi autoryzacji w *Views/_ViewImports.cshtml* pliku, jest dostępny dla wszystkich widoków:

[!code-cshtml[](secure-data/samples/final2/Pages/_ViewImports.cshtml?highlight=6-9)]

Poprzedni kod znaczników dodaje kilka `using` instrukcje.

Aktualizacja **Edytuj** i **usunąć** łączy w *Pages/Contacts/Index.cshtml* , są one tylko renderowania dla użytkowników z odpowiednimi uprawnieniami:

[!code-cshtml[](secure-data/samples/final2/Pages/Contacts/Index.cshtml?highlight=34-36,64-999)]

> [!WARNING]
> Ukrywanie łącza od użytkowników, które nie mają uprawnień do zmiany danych nie zabezpieczenia aplikacji. Ukrywanie łączy powoduje, że bardziej przyjazny dla użytkownika aplikacji przez wyświetlanie łączy jedyne prawidłowe. Użytkownicy mogą hack wygenerowanego adresy URL do wywołania edytowania i usuwania operacji na danych, które nie są właścicielami. Razor strony lub kontrolera muszą wymuszać kontroli dostępu do zabezpieczania danych.

### <a name="update-details"></a>Szczegóły aktualizacji

Aktualizacja w widoku szczegółów, menedżerów można zatwierdzić lub odrzucić kontaktów:

[!code-cshtml[](secure-data/samples/final2/Pages/Contacts/Details.cshtml?range=48-999)]

Aktualizacja modelu strony szczegółów:

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="test-the-completed-app"></a>Testowanie ukończonej aplikacji

Za pomocą programu Visual Studio Code lub testowania na platformie lokalnego, który nie zawiera certyfikatu testowego dla protokołu HTTPS:

* Ustaw `"LocalTest:skipHTTPS": true` w *appsettings. Developement.JSON* plik, aby pominąć wymaganie protokołu HTTPS. Pomiń HTTPS tylko na komputerze deweloperskim.

Jeśli aplikacja ma kontaktów:

* Usuń wszystkie rekordy w `Contact` tabeli.
* Ponowne uruchomienie aplikacji w celu umieszczenia bazy danych.

Zarejestruj użytkownika dla przeglądania kontaktów.

Łatwe testowanie ukończonej aplikacji jest uruchamianie trzech różnych przeglądarkach (lub incognito/sesję InPrivate wersji). W przeglądarce jednego zarejestrować nowego użytkownika (na przykład `test@example.com`). Zaloguj się do każdej przeglądarki z innym użytkownikiem. Sprawdź następujące operacje:

* Zarejestrowani użytkownicy mogą wyświetlać wszystkie zatwierdzone dane kontaktu.
* Zarejestrowani użytkownicy mogą edytowanie/usuwanie własnych danych.
* Menedżerowie można zatwierdzić lub odrzucić dane kontaktowe. `Details` Wyświetlić pokazuje **Zatwierdź** i **Odrzuć** przycisków.
* Administratorzy mogą zatwierdzać Odrzuć i edytowanie/usuwanie żadnych danych.

| Użytkownik| Opcje |
| ------------ | ---------|
| test@example.com | Można edytowanie/usuwanie własnych danych |
| manager@contoso.com | Można zatwierdzić Odrzuć i edytowanie i usuwanie własnych danych |
| admin@contoso.com | Edytowanie i usuwanie można i Zatwierdź/Odrzuć wszystkie dane|

Utworzenie kontaktu w przeglądarce administratora. Skopiuj adres URL do usunięcia i Edytuj z skontaktowanie się z administratorem. Wklej te linki do przeglądarki użytkownika testowego, aby sprawdzić, czy użytkownik testu nie może wykonywać te operacje.

## <a name="create-the-starter-app"></a>Utworzyć początkową aplikację

* Tworzenie stron Razor aplikacji o nazwie "ContactManager"

  * Tworzenie aplikacji z **indywidualne konta użytkowników**.
  * Nadaj mu nazwę "ContactManager", przestrzeń nazw odpowiada przestrzeni nazw używany w próbce.

::: moniker range=">= aspnetcore-2.1"

  ```console
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

  [!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

  ```console
  dotnet new razor -o ContactManager -au Individual -uld
  ```

::: moniker-end

  * `-uld` Określa LocalDB zamiast SQLite

* Dodaj następujące `Contact` modelu:

  [!code-csharp[](secure-data/samples/starter2/Models/Contact.cs?name=snippet1)]

* Szkieletu `Contact` modelu:

```console
dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
```

* Aktualizacja **ContactManager** zakotwiczenia w *Pages/_Layout.cshtml* pliku:

```cshtml
<a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
```

* Tworzenie szkieletu początkowej migracji i aktualizacji bazy danych:

```console
dotnet ef migrations add initial
dotnet ef database update
```

* Testowanie aplikacji przez tworzenie, edytowanie i usuwanie kontaktu

### <a name="seed-the-database"></a>Inicjatora bazy danych

Dodaj `SeedData` klasy do *danych* folderu. Jeśli pobrano próbki, możesz skopiować *SeedData.cs* pliku *danych* folderu projektu początkowego.

Wywołanie `SeedData.Initialize` z `Main`:

[!code-csharp[](secure-data/samples/starter2/Program.cs?name=snippet)]

Testowanie, czy aplikacja rozpoczęta bazy danych. Jeśli istnieją wszystkie wiersze w kontakcie DB, seed — metoda nie działa.

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a>Dodatkowe zasoby

* [Laboratorium autoryzacji platformy ASP.NET Core](https://github.com/blowdart/AspNetAuthorizationWorkshop). W tym laboratorium przechodzi w stan więcej szczegółów na temat funkcji zabezpieczeń wprowadzone w tym samouczku.
* [Autoryzacja w ASP.NET Core: prosty, rolę, opartej na oświadczeniach, a niestandardowe](xref:security/authorization/index)
* [Niestandardowe autoryzacji opartych na zasadach](xref:security/authorization/policies)
