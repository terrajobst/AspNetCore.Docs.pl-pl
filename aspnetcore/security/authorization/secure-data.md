---
title: "Tworzenie aplikacji platformy ASP.NET Core z danych użytkownika chronione przez autoryzacji"
author: rick-anderson
keywords: "Platformy ASP.NET Core, MVC, autoryzacji, role zabezpieczeń, administrator"
ms.author: riande
manager: wpickett
ms.date: 05/22/2017
ms.topic: article
ms.assetid: abeb2f8e-dfbf-4398-a04c-338a613a65bc
ms.technology: aspnet
ms.prod: aspnet-core
uid: security/authorization/secure-data
ms.openlocfilehash: 8eeb5d71575fd819239da6dd63dd31e323fb0556
ms.sourcegitcommit: 96af03c9f44f7c206e68ae3ef8596068e6b4e5fd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2017
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a>Tworzenie aplikacji platformy ASP.NET Core z danych użytkownika chronione przez autoryzacji

Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Audette Jan](https://twitter.com/joeaudette)

W tym samouczku przedstawiono sposób tworzenia aplikacji sieci web z danymi użytkownika chronione przez autoryzacji. Wyświetla listę kontaktów, które uwierzytelnionych użytkowników (zarejestrowanym) został utworzony. Istnieją trzy grupy zabezpieczeń:

* Zarejestrowani użytkownicy mogą wyświetlać wszystkie zatwierdzone dane kontaktu.
* Zarejestrowani użytkownicy mogą edytowanie/usuwanie własnych danych. 
* Menedżerowie można zatwierdzić lub odrzucić dane kontaktowe. Tylko zatwierdzone kontakty są widoczne dla użytkowników.
* Administratorzy mogą zatwierdzać Odrzuć i edytowanie/usuwanie żadnych danych.

Na poniższej ilustracji użytkownik Rick (`rick@example.com`) jest zalogowany. Użytkownik Rick może tylko widok zatwierdzone kontaktów i edytowanie/usuwanie jego kontaktów. Tylko ostatni rekord powstały Rick, wyświetla edycji i usuwania łącza

![Powyższy obraz](secure-data/_static/rick.png)

Na poniższej ilustracji `manager@contoso.com` jest zarejestrowany i rolę menedżerów. 

![Powyższy obraz](secure-data/_static/manager1.png)

Na poniższej ilustracji przedstawiono menedżerów widoku Szczegóły kontaktu.

![Powyższy obraz](secure-data/_static/manager.png)

Tylko menedżerów i Administratorzy mają Zatwierdź i odrzucić przycisków.

Na poniższej ilustracji `admin@contoso.com` jest zarejestrowany i w roli administratora. 

![Powyższy obraz](secure-data/_static/admin.png)

Administrator ma wszystkie uprawnienia. Użytkownik może odczytu/edytowanie/usuwanie żadnych skontaktuj się z i Zmień stan kontaktów.

Aplikacja została utworzona przez [szkieletów](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) następujące `Contact` modelu:

[!code-csharp[Main](secure-data/samples/starter/Models/Contact.cs?name=snippet1)]

A `ContactIsOwnerAuthorizationHandler` obsługi autoryzacji gwarantuje, że użytkownika można edytować tylko ich danych. A `ContactManagerAuthorizationHandler` obsługi autoryzacji umożliwia menedżerów zatwierdzić lub odrzucić kontaktów.  A `ContactAdministratorsAuthorizationHandler` obsługi autoryzacji pozwala administratorom, aby zatwierdzić lub odrzucić kontaktów i edytowanie/usuwanie kontaktów. 

## <a name="prerequisites"></a>Wymagania wstępne

Nie jest to początek samouczka. Należy zapoznać się z:

* [Podstawowe ASP.NET MVC](xref:tutorials/first-mvc-app/start-mvc)
* [Entity Framework Core](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a>Starter i ukończonej aplikacji

[Pobierz](xref:tutorials/index#how-to-download-a-sample) [ukończone](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final) aplikacji. [Test](#test-the-completed-app) ukończonej aplikacji, należy zapoznać się z jego funkcje zabezpieczeń. 

### <a name="the-starter-app"></a>Początkową aplikację

Warto porównać Twój kod z próbką ukończone.

[Pobierz](xref:tutorials/index#how-to-download-a-sample) [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter) aplikacji. 

Zobacz [utworzyć początkową aplikację](#create-the-starter-app) czy chcesz go utworzyć od podstaw.

Aktualizacja bazy danych:

```none
   dotnet ef database update
```

Uruchom aplikację, wybierz **ContactManager** link i sprawdź, tworzenie, edytowanie i usuwanie kontaktu.

Ten samouczek ma główne kroki umożliwiające utworzenie aplikacji danych bezpiecznego użytkownika. Może być przydatne do odwoływania się do projektu zakończone.

## <a name="modify-the-app-to-secure-user-data"></a>Zmodyfikować aplikację w celu zabezpieczenia danych użytkownika

Następujące sekcje zostały wszystkie główne kroki umożliwiające utworzenie aplikacji danych bezpiecznego użytkownika. Może być przydatne do odwoływania się do projektu zakończone.

### <a name="tie-the-contact-data-to-the-user"></a>Powiązanie dane kontaktowe dla użytkownika

Za pomocą programu ASP.NET [tożsamości](xref:security/authentication/identity) identyfikator użytkownika, aby zapewnić użytkownikom można edytować swoje dane, ale nie do innych danych użytkowników. Dodaj `OwnerID` do `Contact` modelu:

[!code-csharp[Main](secure-data/samples/final/Models/Contact.cs?name=snippet1&highlight=5-6,16-)]

`OwnerID`Identyfikator użytkownika z `AspNetUser` tabeli w [tożsamości](xref:security/authentication/identity) bazy danych. `Status` Pole określa, czy kontakt jest widoczny dla zwykłych użytkowników. 

Tworzenie szkieletu nowych migracji i aktualizacji bazy danych:

```console
dotnet ef migrations add userID_Status
dotnet ef database update
 ```

### <a name="require-ssl-and-authenticated-users"></a>Wymagaj protokołu SSL i uwierzytelnionych użytkowników

W `ConfigureServices` metody *Startup.cs* plików, dodawanie [RequireHttpsAttribute](/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) filtr autoryzacji:

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=snippet_SSL&highlight=1)]

Przekierowywanie żądań HTTP, HTTPS, zobacz [ponowne zapisywanie adresów URL w oprogramowaniu pośredniczącym](xref:fundamentals/url-rewriting). Jeśli przy użyciu programu Visual Studio Code lub testowania na platformie lokalnego, który nie zawiera certyfikatu testowego dla protokołu SSL:

- Ustaw `"LocalTest:skipSSL": true` w *appsettings.json* pliku.

### <a name="require-authenticated-users"></a>Wymaga uwierzytelnionych użytkowników

Ustaw domyślne zasady uwierzytelniania, aby wymagać od użytkowników uwierzytelniania. Można zrezygnować z uwierzytelniania metodą kontroler lub akcję z `[AllowAnonymous]` atrybutu. Z tej metody żadnych nowych kontrolerów dodaje automatycznie wymaga uwierzytelniania, który jest bezpieczniejszy niż polegania na nowych kontrolerów, aby uwzględnić `[Authorize]` atrybutu. Dodaj następujący kod do `ConfigureServices` metody *Startup.cs* pliku:

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=snippet_defaultPolicy)]

Dodaj `[AllowAnonymous]` kontrolerowi macierzystego, użytkowników anonimowych można uzyskać informacji o lokacji, przed ich zarejestrować.

[!code-csharp[Main](secure-data/samples/final/Controllers/HomeController.cs?name=snippet1&highlight=2,6)]

### <a name="configure-the-test-account"></a>Skonfiguruj konto testu

`SeedData` Klasy tworzy dwa konta administratora i menedżera. Użyj [narzędzie Menedżer klucz tajny](xref:security/app-secrets) ustawienie hasła dla tych kont. W tym z katalogu projektu (katalog zawierający *Program.cs*).

```console
dotnet user-secrets set SeedUserPW <PW>
```

Aktualizacja `Configure` hasła testu:

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=Configure&highlight=19-21)]

Dodaj identyfikator użytkownika administratora i `Status = ContactStatus.Approved` do kontaktów. Skontaktuj się z tylko jedną jest wyświetlana, Dodaj identyfikator użytkownika do wszystkich kontaktów:

[!code-csharp[Main](secure-data/samples/final/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a>Utwórz właściciela, Menedżer i obsługi autoryzacji administratora

Utwórz `ContactIsOwnerAuthorizationHandler` klasy w *autoryzacji* folderu. `ContactIsOwnerAuthorizationHandler` Zweryfikuje użytkownika na zasób jest właścicielem zasobu.

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

`ContactIsOwnerAuthorizationHandler` Wywołania `context.Succeed` bieżący uwierzytelniony użytkownik jest właścicielem kontaktu. Programy obsługi autoryzacji zazwyczaj zwracać `context.Succeed` gdy są spełnione wymagania. Zwracają `Task.FromResult(0)` gdy nie są spełnione wymagania. `Task.FromResult(0)`jest ani powodzenie lub niepowodzenie, umożliwia innych obsługi autoryzacji do uruchomienia. Jeśli musisz jawnie zakończyć się niepowodzeniem, zwróć `context.Fail()`.

Zezwolimy kontaktu właścicieli edytowanie/usuwanie własnych danych, więc nie trzeba sprawdzić działanie przekazany parametr wymaganie.

### <a name="create-a-manager-authorization-handler"></a>Tworzenie obsługi programu Menedżer autoryzacji

Utwórz `ContactManagerAuthorizationHandler` klasy w *autoryzacji* folderu. `ContactManagerAuthorizationHandler` Potwierdzi użytkownika, działając w zasobie menedżera. Tylko menedżerowie można zatwierdzić lub odrzucić zmiany zawartości (nowe lub zmienione).

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a>Utwórz program obsługi autoryzacji administratora

Utwórz `ContactAdministratorsAuthorizationHandler` klasy w *autoryzacji* folderu. `ContactAdministratorsAuthorizationHandler` Zweryfikuje użytkownika na zasób ma uprawnienia administratora. Administrator może wykonać wszystkie operacje.

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a>Rejestrowanie procedur obsługi autoryzacji

Musi być zarejestrowany przy użyciu programu Entity Framework Core Services [iniekcji zależności](xref:fundamentals/dependency-injection) przy użyciu [AddScoped](/aspnet/core/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions). `ContactIsOwnerAuthorizationHandler` Korzysta z platformy ASP.NET Core [tożsamości](xref:security/authentication/identity), który jest wbudowany w program Entity Framework Core. Zarejestruj obsługi z kolekcją usługi, więc będą one dostępne do `ContactsController` za pośrednictwem [iniekcji zależności](xref:fundamentals/dependency-injection). Dodaj następujący kod na końcu `ConfigureServices`:

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=AuthorizationHandlers)]

`ContactAdministratorsAuthorizationHandler`i `ContactManagerAuthorizationHandler` są dodawane jako pojedynczych wystąpień. Są one pojedynczych wystąpień, ponieważ nie używają EF i wszystkich informacji potrzebnych `Context` parametr `HandleRequirementAsync` metody.

Pełną `ConfigureServices`:

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=ConfigureServices)]

## <a name="update-the-code-to-support-authorization"></a>Zaktualizuj kod do obsługi autoryzacji

W tej sekcji kontrolera i widoki aktualizacji i Dodaj klasę wymagań operacji.

### <a name="update-the-contacts-controller"></a>Aktualizacja kontrolera kontaktów

Aktualizacja `ContactsController` konstruktora:

* Dodaj `IAuthorizationService` usługi z dostępem do obsługi autoryzacji. 
* Dodaj `Identity` `UserManager` usługi:

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_ContactsControllerCtor)]

### <a name="add-a-contact-operations-requirements-class"></a>Dodaj klasę wymagań operacji kontaktów

Dodaj `ContactOperations` klasy do *autoryzacji* folderu. Ta klasa zawiera wymagania naszego obsługuje aplikacja:

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactOperations.cs)]

### <a name="update-create"></a>Utwórz aktualizacji

Aktualizacja `HTTP POST Create` metodę:

* Identyfikator użytkownika, aby dodać `Contact` modelu.
* Wywołuje program obsługi autoryzacji, aby sprawdzić, czy użytkownik jest właścicielem kontaktu.

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Create)]

### <a name="update-edit"></a>Edytowanie aktualizacji

Zaktualizuj zarówno `Edit` metody na potrzeby obsługi autoryzacji weryfikacji użytkownika należy kontakt. Ponieważ firma Microsoft działają autoryzacji zasobów nie można użyć `[Authorize]` atrybutu. Gdy atrybuty są oceniane nie mamy dostęp do zasobu. Autoryzacji zasobów na podstawie musi być konieczne. Kontroli należy wykonać, gdy będziemy mieć dostęp do zasobu, przez załadowanie go w kontrolera lub przez załadowanie go w obrębie samego program obsługi. Często będzie dostęp do zasobu, przekazując w kluczu zasobów.

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Edit)]

### <a name="update-the-delete-method"></a>Metoda usuwania aktualizacji

Zaktualizuj zarówno `Delete` metody na potrzeby obsługi autoryzacji weryfikacji użytkownika należy kontakt.

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Delete)]

## <a name="inject-the-authorization-service-into-the-views"></a>Wstaw usługi autoryzacji do widoków

Obecnie pokazuje interfejsu użytkownika edytowania i usuwania łącza do danych użytkownika nie można zmodyfikować. Firma Microsoft będzie rozwiązać ten problem, stosując obsługi autoryzacji do widoków.

Wstaw usługi autoryzacji w *Views/_ViewImports.cshtml* plik, będzie on dostępny do wszystkich widoków:

[!code-html[Main](secure-data/samples/final/Views/_ViewImports.cshtml)]

Aktualizacja *Views/Contacts/Index.cshtml* widoku Razor tylko wyświetlanie, edytowanie i usuwanie łączy dla użytkowników, którzy mogą edytowanie/usuwanie kontaktu.

Dodaj`@using ContactManager.Authorization;`

Aktualizacja `Edit` i `Delete` łączy, aby były wyświetlane tylko dla użytkowników z uprawnieniem do edytowania i usuwania kontaktu.

[!code-html[Main](secure-data/samples/final/Views/Contacts/Index.cshtml?range=63-84)]

Ostrzeżenie: Ukrywanie łącza z użytkowników, którzy nie mają uprawnień do edytowania ani usuwania danych nie zabezpieczyć aplikację. Ukrywanie łączy powoduje, że aplikacja więcej użytkowników przyjazną przez wyświetlanie łączy jedyne prawidłowe. Użytkownicy mogą hack wygenerowanego adresy URL do wywołania edytowania i usuwania operacji na danych, które nie są właścicielami.  Kontroler należy powtórzyć kontroli dostępu do zabezpieczenia.

### <a name="update-the-details-view"></a>Aktualizacja w widoku szczegółów

Aktualizacja w widoku szczegółów, menedżerów można zatwierdzić lub odrzucić kontaktów:

[!code-html[Main](secure-data/samples/final/Views/Contacts/Details.cshtml?range=53-)]

## <a name="test-the-completed-app"></a>Testowanie ukończonej aplikacji

Jeśli przy użyciu programu Visual Studio Code lub testowania na platformie lokalnego, który nie zawiera certyfikatu testowego dla protokołu SSL:

- Ustaw `"LocalTest:skipSSL": true` w *appsettings.json* pliku.

Uruchomiono aplikację i mieć kontaktów należy usunąć wszystkie rekordy w `Contact` tabeli i ponownie uruchom aplikację w celu umieszczenia bazy danych. Jeśli używasz programu Visual Studio, musisz zamknąć i ponownie uruchomić usługi IIS Express do inicjatora bazy danych.

Zarejestruj użytkownikowi przeglądanie kontaktów.

Łatwe testowanie ukończonej aplikacji jest uruchamianie trzech różnych przeglądarkach (lub incognito/sesję InPrivate wersji). W przeglądarce jednego zarejestrować nowego użytkownika, na przykład `test@example.com`. Zaloguj się do każdej przeglądarki z innym użytkownikiem. Sprawdź następujące informacje:

* Zarejestrowani użytkownicy mogą wyświetlać wszystkie zatwierdzone dane kontaktu.
* Zarejestrowani użytkownicy mogą edytowanie/usuwanie własnych danych. 
* Menedżerowie można zatwierdzić lub odrzucić dane kontaktowe. `Details` Wyświetlić pokazuje **Zatwierdź** i **Odrzuć** przycisków. 
* Administratorzy mogą zatwierdzać Odrzuć i edytowanie/usuwanie żadnych danych.

| Użytkownik| Opcje |
| ------------ | ---------|
| test@example.com | Można edytowanie/usuwanie własnych danych |
| manager@contoso.com | Można zatwierdzić Odrzuć i edytowanie i usuwanie własnych danych  |
| admin@contoso.com | Edytowanie i usuwanie można i Zatwierdź/Odrzuć wszystkie dane|

Utworzenie kontaktu w przeglądarce Administratorzy. Skopiuj adres URL do usunięcia i Edytuj z skontaktowanie się z administratorem. Wklej te linki do przeglądarki użytkownika testowego, aby sprawdzić, czy użytkownik testu nie może wykonywać te operacje.

## <a name="create-the-starter-app"></a>Utworzyć początkową aplikację

Wykonaj te instrukcje, aby utworzyć początkową aplikację.

* Utwórz **aplikacji sieci Web platformy ASP.NET Core** przy użyciu [programu Visual Studio 2017](https://www.visualstudio.com/) o nazwie "ContactManager"

  * Tworzenie aplikacji z **indywidualne konta użytkowników**.
  * Nadaj mu nazwę "ContactManager", przestrzeń nazw będzie odpowiadała Użyj przestrzeni nazw w próbce.

* Dodaj następujące `Contact` modelu:

  [!code-csharp[Main](secure-data/samples/starter/Models/Contact.cs?name=snippet1)]

* Szkieletu `Contact` modelu przy użyciu programu Entity Framework Core i `ApplicationDbContext` kontekstu danych. Zaakceptuj wszystkie ustawienia domyślne szkieletów. Przy użyciu `ApplicationDbContext` dla kontekstu danych klasy umieszcza tabeli kontaktu [tożsamości](xref:security/authentication/identity) bazy danych. Zobacz [Dodawanie modelu](xref:tutorials/first-mvc-app/adding-model) Aby uzyskać więcej informacji.

* Aktualizacja **ContactManager** zakotwiczenia w *Views/Shared/_Layout.cshtml* plik z `asp-controller="Home"` do `asp-controller="Contacts"` tak naciśnięcie **ContactManager** łącza wywoła kontrolera kontaktów. Oryginalny kod znaczników:

```html
   <a asp-area="" asp-controller="Home" asp-action="Index" class="navbar-brand">ContactManager</a>
   ```

Zaktualizowany kod znaczników:

```html
   <a asp-area="" asp-controller="Contacts" asp-action="Index" class="navbar-brand">ContactManager</a>
   ```

* Tworzenie szkieletu początkowej migracji i aktualizacji bazy danych

```none
   dotnet ef migrations add initial
   dotnet ef database update
   ```

* Testowanie aplikacji przez tworzenie, edytowanie i usuwanie kontaktu

### <a name="seed-the-database"></a>Inicjatora bazy danych

Dodaj `SeedData` klasy do *danych* folderu. Jeśli pobrano próbki, możesz skopiować *SeedData.cs* pliku *danych* folderu projektu początkowego.

[!code-csharp[Main](secure-data/samples/starter/Data/SeedData.cs)]

Dodaj wyróżniony kod na końcu `Configure` metody w *Startup.cs* pliku:

[!code-csharp[Main](secure-data/samples/starter/Startup.cs?name=Configure&highlight=28-)]

Testowanie, czy aplikacja rozpoczęta bazy danych. Seed — metoda nie działa, jeśli istnieją wszystkie wiersze w kontakcie bazy danych.

### <a name="create-a-class-used-in-the-tutorial"></a>Tworzenie klasy używane w samouczku

* Utwórz folder o nazwie *autoryzacji*.
* Kopiuj *Authorization\ContactOperations.cs* plik Pobieranie ukończone projektu lub skopiuj następujący kod:

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactOperations.cs)]

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a>Dodatkowe zasoby

* [Laboratorium autoryzacji platformy ASP.NET Core](https://github.com/blowdart/AspNetAuthorizationWorkshop). W tym laboratorium przechodzi w stan więcej szczegółów na temat funkcji zabezpieczeń wprowadzone w tym samouczku.
* [Autoryzacja w ASP.NET Core: prosty, roli, opartej na oświadczeniach i niestandardowych](index.md)
* [Niestandardowe autoryzacji opartych na zasadach](policies.md)
