---
title: Tworzenie aplikacji platformy ASP.NET Core przy użyciu danych użytkownika chronionych przez autoryzację
author: rick-anderson
description: Dowiedz się, jak utworzyć aplikację stron Razor przy użyciu danych użytkownika chronionych przez autoryzację. Obejmuje protokołu HTTPS, uwierzytelniania, zabezpieczeń i tożsamości platformy ASP.NET Core.
ms.author: riande
ms.date: 7/24/2018
uid: security/authorization/secure-data
ms.openlocfilehash: ba59e8d6243965188397c4ba7a130eec42acfb91
ms.sourcegitcommit: 847cc1de5526ff42a7303491e6336c2dbdb45de4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43055883"
---
::: moniker range="<= aspnetcore-1.1"

Zobacz [ta](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) dla wersji platformy ASP.NET Core MVC. Wersja platformy ASP.NET Core 1.1 po ukończeniu tego samouczka jest [to](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) folderu. 1.1, na przykład platforma ASP.NET Core jest w [przykłady](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).
::: moniker-end

::: moniker range="= aspnetcore-2.0"

Zobacz temat [tego pdf] ()https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_July16_18.pdf)

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a>Tworzenie aplikacji platformy ASP.NET Core przy użyciu danych użytkownika chronionych przez autoryzację

Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Audette Jan](https://twitter.com/joeaudette)

W tym samouczku pokazano, jak utworzyć aplikację sieci web platformy ASP.NET Core przy użyciu danych użytkownika chronionych przez autoryzację. Wyświetla listę kontaktów, uwierzytelnionych użytkowników (zarejestrowane), które zostały utworzone. Istnieją trzy grupy zabezpieczeń:

* **Liczba zarejestrowanych użytkowników** może wyświetlać wszystkie zatwierdzone dane i może edytować/usuwać swoje dane.
* **Menedżerowie** można zatwierdzić lub odrzucić dane kontaktowe. Tylko zatwierdzone kontakty będą widoczne dla użytkowników.
* **Administratorzy** można zatwierdzić/Odrzuć i edytowanie/usuwanie żadnych danych.

Na poniższej ilustracji użytkownik Rick (`rick@example.com`) jest zalogowany. Rick mogą jedynie wyświetlać kontakty zatwierdzone i **Edytuj**/**Usuń**/**Utwórz nowy** linki do swoich kontaktów. Tylko ostatni rekord, utworzone przez Rick, wyświetla **Edytuj** i **Usuń** łącza. Inni użytkownicy nie zobaczą ostatni rekord, aż Menedżer lub administrator zmienia stan na "Zatwierdzone".

![Obraz opisanych w poprzednich](secure-data/_static/rick.png)

Na poniższej ilustracji `manager@contoso.com` jest zarejestrowany i w roli menedżerów:

![Obraz opisanych w poprzednich](secure-data/_static/manager1.png)

Na poniższej ilustracji przedstawiono menedżerów widoku szczegółów kontaktu:

![Obraz opisanych w poprzednich](secure-data/_static/manager.png)

**Zatwierdź** i **Odrzuć** przyciski są wyświetlane tylko dla menedżerów i administratorów.

Na poniższej ilustracji `admin@contoso.com` jest zarejestrowany i w roli administratora:

![Obraz opisanych w poprzednich](secure-data/_static/admin.png)

Administrator ma wszystkie uprawnienia. Ona można odczytu/edytowanie/usuwanie dowolnego skontaktuj się z pomocą i zmienić stan kontaktów.

Aplikacja została utworzona przez [tworzenia szkieletów](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) następujące `Contact` modelu:

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet)]

Przykład zawiera poniższe obsługi autoryzacji:

* `ContactIsOwnerAuthorizationHandler`: Zapewnia, że użytkownika można edytować tylko swoje dane.
* `ContactManagerAuthorizationHandler`: Umożliwia menedżerom zatwierdzić lub odrzucić kontaktów.
* `ContactAdministratorsAuthorizationHandler`: Umożliwia administratorom, aby zatwierdzić lub odrzucić kontakty i edytowanie/usuwanie kontaktów.

## <a name="prerequisites"></a>Wymagania wstępne

W tym samouczku jest zaawansowany. Należy zapoznać się z:

* [ASP.NET Core](xref:tutorials/first-mvc-app/start-mvc)
* [Uwierzytelnianie](xref:security/authentication/index)
* [Potwierdzenie konta i odzyskiwanie hasła](xref:security/authentication/accconfirm)
* [Autoryzacja](xref:security/authorization/index)
* [Entity Framework Core](xref:data/ef-mvc/intro)

Kod pobierania, jak w przypadku tego samouczka wymaga platformy ASP.NET Core 2.2 w wersji zapoznawczej 1 lub nowszym. Zobacz [problem w usłudze GitHub](https://github.com/aspnet/Identity/issues/1813#issuecomment-394543909) dla obejścia.

## <a name="the-starter-and-completed-app"></a>Starter i ukończonej aplikacji

[Pobierz](xref:tutorials/index#how-to-download-a-sample) [ukończone](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2) aplikacji. [Test](#test-the-completed-app) ukończonej aplikacji, dzięki czemu można zapoznać się z jej funkcjami zabezpieczeń.

### <a name="the-starter-app"></a>Aplikację startową

[Pobierz](xref:tutorials/index#how-to-download-a-sample) [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2) aplikacji.

Uruchom aplikację, naciśnij przycisk **ContactManager** link i sprawdź, tworzenie, edytowanie i usuwanie kontaktu.

## <a name="secure-user-data"></a>Zabezpieczanie danych użytkownika

Poniższe sekcje mają główne kroki umożliwiające utworzenie aplikacji danych bezpiecznego użytkownika. Może okazać się przydatne do odwoływania się do projektu ukończona.

### <a name="tie-the-contact-data-to-the-user"></a>Powiąż dane kontaktowe dla użytkownika

Za pomocą programu ASP.NET [tożsamości](xref:security/authentication/identity) identyfikator użytkownika w celu zapewnienia, że użytkownicy mogą edytować swoje dane, ale nie dane innych użytkowników. Dodaj `OwnerID` i `ContactStatus` do `Contact` modelu:

[!code-csharp[](secure-data/samples/final2.1/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

`OwnerID` Identyfikator użytkownika z `AspNetUser` tabelę [tożsamości](xref:security/authentication/identity) bazy danych. `Status` Pola określa, czy kontakt jest widoczny dla zwykłych użytkowników.

Tworzenie nowej migracji i aktualizowanie bazy danych:

```console
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-identity"></a>Dodaj usługi ról do tożsamości

Dołącz [opcji Dodawanie ról](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) można dodać usługi ról:

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet2&highlight=12)]

### <a name="require-authenticated-users"></a>Wymaga uwierzytelnionych użytkowników

Ustaw domyślne zasady uwierzytelniania, aby wymagać od użytkowników uwierzytelniania:

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet&highlight=17-99)] 

 Użytkownik może zrezygnować z uwierzytelniania na poziomie metody strony Razor, kontrolera lub akcji z `[AllowAnonymous]` atrybutu. Ustawienie domyślne zasady uwierzytelniania, aby wymagać od użytkowników uwierzytelniania chroni nowo dodanych stronami Razor i kontrolerów. Posiadanie uwierzytelniania domyślnie wymagane jest bezpieczniejszy niż opierając się na nowych kontrolerów i stron Razor do uwzględnienia `[Authorize]` atrybutu.

Dodaj [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) do indeksu o i skontaktuj się z pomocą stron, użytkowników anonimowych można uzyskać informacji o lokacji, przed ich zarejestrowania.

[!code-csharp[](secure-data/samples/final2.1/Pages/Index.cshtml.cs?highlight=1,6)]

### <a name="configure-the-test-account"></a>Skonfiguruj konto testu

`SeedData` Klasy tworzy dwa konta: administrator i Menedżer. Użyj [narzędzie Menedżer klucz tajny](xref:security/app-secrets) ustawić hasło dla tych kont. Ustaw hasło z katalogu projektu (katalogu zawierającego *Program.cs*):

```console
dotnet user-secrets set SeedUserPW <PW>
```

Jeśli nie określono silne hasło, wyjątek jest generowany, gdy `SeedData.Initialize` jest wywoływana.

Aktualizacja `Main` hasła testu:

[!code-csharp[](secure-data/samples/final2.1/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a>Tworzenie konta testowe i aktualizowanie kontaktów

Aktualizacja `Initialize` method in Class metoda `SeedData` klasy w celu utworzenia konta testowe:

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet_Initialize)]

Dodaj identyfikator użytkownika administratora i `ContactStatus` do kontaktów. Określ jeden z kontaktów "Przesłane" i jednym "odrzucone". Dodawanie stanu i Identyfikatora użytkownika do wszystkich kontaktów. Wyświetlane jest tylko jeden kontakt:

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a>Utwórz właściciela, Menedżer i obsługi autoryzacji administratora

Tworzenie `ContactIsOwnerAuthorizationHandler` klasy w *autoryzacji* folderu. `ContactIsOwnerAuthorizationHandler` Sprawdza, czy użytkownik, działając w zasobie jest właścicielem zasobu.

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

`ContactIsOwnerAuthorizationHandler` Wywołania [kontekstu. Powodzenie](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) aktualnego użytkownika uwierzytelnionego jest skontaktuj się z właścicielem. Programy obsługi autoryzacji zazwyczaj:

* Zwróć `context.Succeed` gdy spełniono wymagania.
* Zwróć `Task.CompletedTask` gdy nie są spełnione wymagania. `Task.CompletedTask` to nie sukces lub niepowodzenie&mdash;umożliwia innych programów obsługi autoryzacji uruchomić.

Jeśli potrzebujesz jawnie nie powiedzie się, zwraca [kontekstu. Niepowodzenie](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).

Aplikacja umożliwia skontaktuj się z pomocą właścicieli do edycji/delete/tworzenie własnych danych. `ContactIsOwnerAuthorizationHandler` nie muszą sprawdzać operacji przekazane w parametrze wymagań.

### <a name="create-a-manager-authorization-handler"></a>Utwórz procedurę obsługi Menedżer autoryzacji

Tworzenie `ContactManagerAuthorizationHandler` klasy w *autoryzacji* folderu. `ContactManagerAuthorizationHandler` Weryfikuje użytkownika, działając w zasobie menedżera. Tylko menedżerowie mogli zatwierdzać lub odrzucać zmiany zawartości (nowe lub zmienione).

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a>Utwórz procedurę obsługi autoryzacji administratora

Tworzenie `ContactAdministratorsAuthorizationHandler` klasy w *autoryzacji* folderu. `ContactAdministratorsAuthorizationHandler` Weryfikuje użytkownika na zasób jest administratorem. Administrator może wykonać wszystkie operacje.

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a>Zarejestruj procedury obsługi autoryzacji

Usługi za pomocą platformy Entity Framework Core musi być zarejestrowana do [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) przy użyciu [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions). `ContactIsOwnerAuthorizationHandler` Korzysta z platformy ASP.NET Core [tożsamości](xref:security/authentication/identity), która jest oparta na platformy Entity Framework Core. Zarejestruj procedury obsługi za pomocą kolekcji usługi, dzięki czemu są one dostępne do `ContactsController` za pośrednictwem [wstrzykiwanie zależności](xref:fundamentals/dependency-injection). Dodaj następujący kod na końcu `ConfigureServices`:

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet_defaultPolicy&highlight=27-99)]

`ContactAdministratorsAuthorizationHandler` i `ContactManagerAuthorizationHandler` są dodawane jako pojedynczych wystąpień. Są one pojedynczych wystąpień, ponieważ nie używają programu EF, a wszystkie informacje wymagane w `Context` parametru `HandleRequirementAsync` metody.

## <a name="support-authorization"></a>Obsługuje autoryzację

W tej sekcji służy do aktualizacji stron Razor i Dodaj klasę wymagania dotyczące operacji.

### <a name="review-the-contact-operations-requirements-class"></a>Przegląd klasy wymagania dotyczące operacji kontaktu

Przegląd `ContactOperations` klasy. Ta klasa zawiera wymagania obsługiwanej przez aplikację:

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-razor-pages"></a>Utwórz klasę bazową dla stron Razor kontaktów

Utwórz klasę bazową, zawierający usług wykorzystanych w kontaktach stron Razor. Klasa bazowa umieszcza kod inicjowania w jednej lokalizacji:

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/DI_BasePageModel.cs)]

Powyższy kod:

* Dodaje `IAuthorizationService` usługi w celu uzyskania dostępu do obsługi autoryzacji.
* Dodaje tożsamości `UserManager` usługi.
* Dodaj `ApplicationDbContext`.

### <a name="update-the-createmodel"></a>Aktualizacja CreateModel

Aktualizuj Konstruktor Tworzenie strony modelu do użycia `DI_BasePageModel` klasa bazowa:

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

Aktualizacja `CreateModel.OnPostAsync` metody:

* Dodaj identyfikator użytkownika, który `Contact` modelu.
* Wywołania obsługi autoryzacji, aby sprawdzić, czy użytkownik ma uprawnienia do tworzenia kontaktów.

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a>Aktualizacja IndexModel

Aktualizacja `OnGetAsync` metody, więc tylko zatwierdzonego kontakty są wyświetlane użytkownikom ogólne:

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a>Aktualizacja EditModel

Dodaj program obsługi autoryzacji, aby sprawdzić, czy użytkownik jest właścicielem kontaktu. Ponieważ autoryzacji zasobów jest weryfikowany, `[Authorize]` atrybut nie jest wystarczająca. Aplikacja nie ma dostępu do zasobu, gdy atrybuty są oceniane. Autoryzacja na podstawie zasobów musi być imperatywnego. Testy muszą być wykonywane, gdy aplikacja ma dostęp do zasobu przez załadowanie go w modelu strony lub przez załadowanie go w ramach programu obsługi, sam. Często dostęp do zasobu, przekazując klucz zasobu.

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a>Aktualizacja DeleteModel

Aktualizowanie modelu strony delete na potrzeby obsługi autoryzacji upewnij się, że użytkownik ma odpowiednie uprawnienia do usuwania dla kontaktu.

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a>Wstrzyknięcie usługi autoryzacji do widoków

Obecnie pokazuje interfejsu użytkownika edytowania i usuwania łącza kontaktów, do których użytkownik nie może modyfikować.

Wstrzykiwanie usługi autoryzacji w *Views/_ViewImports.cshtml* pliku, dzięki czemu są one dostępne dla wszystkich widoków:

[!code-cshtml[](secure-data/samples/final2.1/Pages/_ViewImports.cshtml?highlight=6-99)]

Poprzedni kod znaczników umożliwia dodanie kilku `using` instrukcji.

Aktualizacja **Edytuj** i **Usuń** linki w *Pages/Contacts/Index.cshtml* więc tylko są one renderowane dla użytkowników z odpowiednimi uprawnieniami:

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> Ukrywanie łączy na podstawie użytkowników, którzy nie mają uprawnień do zmiany danych nie zabezpieczenia aplikacji. Ukrywanie łącza sprawia, że bardziej przyjazny dla użytkownika aplikacji, wyświetlając tylko poprawne linki. Użytkownicy mogą hack wygenerowanego adresy URL, aby wywołać Edytuj i Usuń operacje na danych, które nie są właścicielami. Strona Razor lub kontrolera musi wymuszają operacje sprawdzania dostępu do zabezpieczania danych.

### <a name="update-details"></a>Szczegóły aktualizacji

Zaktualizuj widok szczegółów, aby menedżerowie mogli zatwierdzać lub odrzucać kontaktów:

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml?name=snippet)]

Aktualizowanie modelu strony szczegółów:

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="test-the-completed-app"></a>Testowanie aplikacji ukończone

Jeśli aplikacja ma kontaktów:

* Usuń wszystkie rekordy w `Contact` tabeli.
* Ponowne uruchomienie aplikacji w celu umieszczenia bazy danych.

Zarejestruj użytkownika do przeglądania kontaktów.

Łatwe testowanie ukończonej aplikacji jest do uruchomienia w trzech różnych przeglądarek (lub incognito/InPrivate wersje). W jednej przeglądarki zarejestrować nowego użytkownika (na przykład `test@example.com`). Zaloguj się w każdej przeglądarce z innym użytkownikiem. Sprawdź następujące operacje:

* Zarejestrowani użytkownicy można wyświetlić wszystkie zatwierdzone dane kontaktowe.
* Zarejestrowani użytkownicy może edytować/usuwać swoje dane.
* Menedżerowie mogli zatwierdzać lub odrzucać dane kontaktowe. `Details` Wyświetlić pokazuje **Zatwierdź** i **Odrzuć** przycisków.
* Administratorzy mogą zatwierdzać/Odrzuć i edytowanie/usuwanie żadnych danych.

| Użytkownik| Opcje |
| ------------ | ---------|
| test@example.com | Może edytować/usuwać własnych danych |
| manager@contoso.com | Można zatwierdzić/Odrzuć i edytowanie/usuwanie własnych danych |
| admin@contoso.com | Edytowanie/usuwanie można i Zatwierdź/Odrzuć wszystkie dane|

Utwórz kontakt w przeglądarce administratora. Skopiuj adres URL do usunięcia, a następnie Edytuj z skontaktowanie się z administratorem. Wklej te linki do przeglądarki użytkownika testowego, aby sprawdzić, czy użytkownik testu nie może wykonywać te operacje.

## <a name="create-the-starter-app"></a>Utwórz aplikację startową

* Tworzenie stron Razor aplikacji o nazwie "ContactManager"
   * Tworzenie aplikacji za pomocą **indywidualne konta użytkowników**.
   * Nadaj mu nazwę "ContactManager", przestrzeń nazw używaną w próbce pasujących przestrzeni nazw.
   * `-uld` Określa LocalDB zamiast bazy danych SQLite

  ```console
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* Dodaj *Models\Contact.cs*:

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* Tworzenie szkieletu `Contact` modelu.
* Tworzenie początkowej migracji i aktualizowanie bazy danych:

```console
dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
dotnet ef database drop -f
dotnet ef migrations add initial
dotnet ef database update
```

* Aktualizacja **ContactManager** zakotwiczenia w *Pages/_Layout.cshtml* pliku:

```cshtml
<a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
```

* Przetestuj aplikację, tworzenia, edytowania i usuwania kontaktu

### <a name="seed-the-database"></a>Inicjowanie bazy danych

Dodaj [SeedData](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs) klasy *danych* folderu.

Wywołaj `SeedData.Initialize` z `Main`:

[!code-csharp[](secure-data/samples/starter2.1/Program.cs?name=snippet)]

Sprawdź, czy aplikacja zasilany bazy danych. W przypadku wszystkich wierszy w skontaktuj się z bazy danych, metoda inicjatora nie zostanie uruchomiony.

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a>Dodatkowe zasoby

* [Laboratorium autoryzacji platformy ASP.NET Core](https://github.com/blowdart/AspNetAuthorizationWorkshop). W tym laboratorium zawiera bardziej szczegółowe na temat funkcji zabezpieczeń wprowadzone w ramach tego samouczka.
* [Autoryzacja w programie ASP.NET Core: prosty, rola, opartej na oświadczeniach i niestandardowe](xref:security/authorization/index)
* [Autoryzacja niestandardowa oparta na zasadach](xref:security/authorization/policies)

::: moniker-end