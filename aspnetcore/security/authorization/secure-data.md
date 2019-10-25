---
title: Tworzenie aplikacji ASP.NET Core przy użyciu danych użytkownika chronionych przez autoryzację
author: rick-anderson
description: Dowiedz się, jak utworzyć aplikację Razor Pages przy użyciu danych użytkownika chronionych przez autoryzację. Obejmuje HTTPS, uwierzytelnianie, zabezpieczenia ASP.NET Core tożsamość.
ms.author: riande
ms.date: 12/18/2018
ms.custom: mvc, seodec18
uid: security/authorization/secure-data
ms.openlocfilehash: 6e2f785a6dc014884f105766686f284cb2685530
ms.sourcegitcommit: 383017d7060a6d58f6a79cf4d7335d5b4b6c5659
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/23/2019
ms.locfileid: "72816153"
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a>Tworzenie aplikacji ASP.NET Core przy użyciu danych użytkownika chronionych przez autoryzację

Autorzy [Rick Anderson](https://twitter.com/RickAndMSFT) i Jan [Audette](https://twitter.com/joeaudette)

::: moniker range="<= aspnetcore-1.1"

[Ten plik PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) jest wyświetlany w wersji ASP.NET Core MVC. Wersja ASP.NET Core 1,1 tego samouczka znajduje się w [tym](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data) folderze. Przykład 1,1 ASP.NET Core znajduje się w [próbkach](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Zobacz [ten plik PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_July16_18.pdf)

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

W tym samouczku pokazano, jak utworzyć aplikację sieci Web ASP.NET Core przy użyciu danych użytkownika chronionych przez autoryzację. Zostanie wyświetlona lista kontaktów, które zostały utworzone przez uwierzytelnionych (zarejestrowanych) użytkowników. Istnieją trzy grupy zabezpieczeń:

* **Zarejestrowani użytkownicy** mogą wyświetlać wszystkie zatwierdzone dane i edytować/usuwać własne dane.
* **Menedżerowie** mogą zatwierdzać lub odrzucać dane kontaktów. Tylko zatwierdzone kontakty są widoczne dla użytkowników.
* **Administratorzy** mogą zatwierdzić/odrzucić i edytować/usunąć dowolne dane.

Obrazy w tym dokumencie nie są dokładnie zgodne z najnowszymi szablonami.

Na poniższej ilustracji użytkownik Rick (`rick@example.com`) jest zalogowany. Rick mogą wyświetlać tylko zatwierdzone kontakty i **edytować**/**usunąć**/**tworzyć nowe** linki dla swoich kontaktów. Tylko ostatni rekord utworzony przez Rick zawiera linki do **edycji** i **usuwania** . Inni użytkownicy nie będą widzieć ostatniego rekordu do momentu zmiany stanu na "zatwierdzone" przez Menedżera lub administratora.

![Zrzut ekranu przedstawiający Rick zalogowany](secure-data/_static/rick.png)

Na poniższej ilustracji `manager@contoso.com` jest zalogowany i w roli menedżera:

![Zrzut ekranu przedstawiający manager@contoso.com zalogowany](secure-data/_static/manager1.png)

Na poniższej ilustracji przedstawiono widok szczegółów osoby kontaktowej:

![Widok kontaktu w Menedżerze](secure-data/_static/manager.png)

Przyciski **Zatwierdź** i **Odrzuć** są wyświetlane tylko dla menedżerów i administratorów.

Na poniższej ilustracji `admin@contoso.com` jest zalogowany i w roli administratora:

![Zrzut ekranu przedstawiający admin@contoso.com zalogowany](secure-data/_static/admin.png)

Administrator ma wszystkie uprawnienia. Może odczytywać/edytować/usuwać dowolne kontakty i zmieniać stan kontaktów.

Aplikacja została utworzona przez utworzenie [szkieletu](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) następującego modelu `Contact`:

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

Przykład zawiera następujące programy obsługi autoryzacji:

* `ContactIsOwnerAuthorizationHandler`: zapewnia, że użytkownik może edytować tylko swoje dane.
* `ContactManagerAuthorizationHandler`: umożliwia menedżerom zatwierdzanie lub odrzucanie kontaktów.
* `ContactAdministratorsAuthorizationHandler`: umożliwia administratorom zatwierdzanie lub odrzucanie kontaktów oraz Edytowanie/usuwanie kontaktów.

## <a name="prerequisites"></a>Wymagania wstępne

Ten samouczek jest zaawansowany. Należy zapoznać się z:

* [ASP.NET Core](xref:tutorials/first-mvc-app/start-mvc)
* [Uwierzytelnianie](xref:security/authentication/identity)
* [Potwierdzenie konta i odzyskiwanie hasła](xref:security/authentication/accconfirm)
* [Autoryzacja](xref:security/authorization/introduction)
* [Entity Framework Core](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a>Aplikacja Starter i ukończona

[Pobierz](xref:index#how-to-download-a-sample) [ukończoną](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) aplikację. [Przetestuj](#test-the-completed-app) ukończoną aplikację, aby zapoznać się z jej funkcjami zabezpieczeń.

### <a name="the-starter-app"></a>Aplikacja początkowa

[Pobierz](xref:index#how-to-download-a-sample) aplikację [startową](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) .

Uruchom aplikację, naciśnij link **ContactManager** i sprawdź, czy można tworzyć, edytować i usuwać kontakty.

## <a name="secure-user-data"></a>Zabezpieczanie danych użytkownika

Poniższe sekcje zawierają wszystkie najważniejsze kroki umożliwiające utworzenie aplikacji zabezpieczonych danych użytkownika. Pomocne może być odwołanie do ukończonego projektu.

### <a name="tie-the-contact-data-to-the-user"></a>Powiązanie danych kontaktowych z użytkownikiem

Użyj identyfikatora użytkownika [tożsamości](xref:security/authentication/identity) ASP.NET, aby upewnić się, że użytkownicy będą mogli edytować swoje dane, ale nie inne dane użytkowników. Dodaj `OwnerID` i `ContactStatus` do modelu `Contact`:

[!code-csharp[](secure-data/samples/final3/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

`OwnerID` jest IDENTYFIKATORem użytkownika z tabeli `AspNetUser` w bazie danych [tożsamości](xref:security/authentication/identity) . Pole `Status` określa, czy kontakt jest widoczny dla użytkowników ogólnych.

Utwórz nową migrację i zaktualizuj bazę danych:

```dotnetcli
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-identity"></a>Dodawanie usług ról do tożsamości

Dołącz [Addroles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) , aby dodać usługi ról:

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet2&highlight=9)]

### <a name="require-authenticated-users"></a>Wymagaj uwierzytelnionych użytkowników

Ustaw domyślne zasady uwierzytelniania, aby wymagać uwierzytelnienia użytkowników:

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet&highlight=15-99)] 

 Można zrezygnować z uwierzytelniania na stronie Razor, kontrolerze lub poziomie metody akcji z atrybutem `[AllowAnonymous]`. Ustawienie domyślnych zasad uwierzytelniania wymagające uwierzytelnienia użytkowników chroniących nowo dodane Razor Pages i kontrolery. Uwierzytelnianie wymagane domyślnie jest bezpieczniejsze niż poleganie na nowych kontrolerach i Razor Pages uwzględnieniu atrybutu `[Authorize]`.

Dodaj [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) do stron indeksu i prywatności, aby użytkownicy anonimowi mogli uzyskać informacje o witrynie przed ich zarejestrowaniem.

[!code-csharp[](secure-data/samples/final3/Pages/Index.cshtml.cs?highlight=1,7)]

### <a name="configure-the-test-account"></a>Konfigurowanie konta testowego

Klasa `SeedData` tworzy dwa konta: administrator i Menedżer. Użyj [Narzędzia Secret Manager](xref:security/app-secrets) , aby ustawić hasło dla tych kont. Ustaw hasło z katalogu projektu (katalog zawierający *program.cs*):

```dotnetcli
dotnet user-secrets set SeedUserPW <PW>
```

Jeśli nie określono silnego hasła, wyjątek jest zgłaszany, gdy zostanie wywołane `SeedData.Initialize`.

Zaktualizuj `Main`, aby użyć hasła testu:

[!code-csharp[](secure-data/samples/final3/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a>Tworzenie kont testowych i aktualizowanie kontaktów

Zaktualizuj metodę `Initialize` w klasie `SeedData`, aby utworzyć konta testowe:

[!code-csharp[](secure-data/samples/final3/Data/SeedData.cs?name=snippet_Initialize)]

Dodaj identyfikator użytkownika administratora i `ContactStatus` do kontaktów. Utwórz jeden z kontaktów "przesłane" i jeden "odrzucony". Dodaj identyfikator i stan użytkownika do wszystkich kontaktów. Pokazywany jest tylko jeden kontakt:

[!code-csharp[](secure-data/samples/final3/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a>Tworzenie programów do obsługi autoryzacji właściciela, Menedżera i administratora

Utwórz klasę `ContactIsOwnerAuthorizationHandler` w folderze *autoryzacji* . `ContactIsOwnerAuthorizationHandler` sprawdza, czy użytkownik, który działa na zasobów, jest właścicielem zasobu.

[!code-csharp[](secure-data/samples/final3/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

Kontekst wywołań `ContactIsOwnerAuthorizationHandler` [. Powodzenie](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) , jeśli bieżący użytkownik uwierzytelniony jest właścicielem osoby kontaktowej. Obsługa autoryzacji zazwyczaj:

* Zwróć `context.Succeed`, gdy wymagania są spełnione.
* Zwróć `Task.CompletedTask`, gdy wymagania nie są spełnione. `Task.CompletedTask` nie powiodło się lub wystąpił błąd&mdash;zezwala na uruchamianie innych programów obsługi autoryzacji.

Jeśli musisz jawnie niepowodzeniem, zwróć [kontekst. Nie powiodło się](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).

Aplikacja umożliwia właścicielom kontaktu Edytowanie/usuwanie/tworzenie własnych danych. `ContactIsOwnerAuthorizationHandler` nie musi sprawdzać operacji przesłanej w parametrze wymagania.

### <a name="create-a-manager-authorization-handler"></a>Tworzenie procedury obsługi autoryzacji Menedżera

Utwórz klasę `ContactManagerAuthorizationHandler` w folderze *autoryzacji* . `ContactManagerAuthorizationHandler` sprawdza, czy użytkownik działający na tym zasobie jest menedżerem. Tylko menedżerowie mogą zatwierdzać lub odrzucać zmiany zawartości (nowe lub zmienione).

[!code-csharp[](secure-data/samples/final3/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a>Tworzenie procedury obsługi autoryzacji administratora

Utwórz klasę `ContactAdministratorsAuthorizationHandler` w folderze *autoryzacji* . `ContactAdministratorsAuthorizationHandler` sprawdza, czy użytkownik działający na tym zasobie jest administratorem. Administrator może wykonać wszystkie operacje.

[!code-csharp[](secure-data/samples/final3/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a>Rejestrowanie programów obsługi autoryzacji

Usługi korzystające z Entity Framework Core muszą być zarejestrowane dla [iniekcji zależności](xref:fundamentals/dependency-injection) przy użyciu funkcji [addscoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions). `ContactIsOwnerAuthorizationHandler` używa ASP.NET Core [tożsamości](xref:security/authentication/identity), która jest oparta na Entity Framework Core. Zarejestruj procedury obsługi w kolekcji usług, aby były dostępne dla `ContactsController` za pomocą [iniekcji zależności](xref:fundamentals/dependency-injection). Dodaj następujący kod na końcu `ConfigureServices`:

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet_defaultPolicy&highlight=23-99)]

`ContactAdministratorsAuthorizationHandler` i `ContactManagerAuthorizationHandler` są dodawane jako pojedyncze. Są one pojedynczymi, ponieważ nie korzystają z EF, a wszystkie potrzebne informacje znajdują się w `Context` parametr metody `HandleRequirementAsync`.

## <a name="support-authorization"></a>Obsługa autoryzacji

W tej sekcji należy zaktualizować Razor Pages i dodać klasę wymagania operacji.

### <a name="review-the-contact-operations-requirements-class"></a>Przeglądanie klasy wymagań operacji kontaktu

Zapoznaj się z klasą `ContactOperations`. Ta klasa zawiera wymagania obsługiwane przez aplikację:

[!code-csharp[](secure-data/samples/final3/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-razor-pages"></a>Utwórz klasę bazową dla kontaktów Razor Pages

Utwórz klasę bazową zawierającą usługi używane w Razor Pages kontaktów. Klasa bazowa umieszcza kod inicjujący w jednej lokalizacji:

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/DI_BasePageModel.cs)]

Poprzedni kod:

* Dodaje usługę `IAuthorizationService`, aby uzyskać dostęp do programów obsługi autoryzacji.
* Dodaje usługę tożsamości `UserManager`.
* Dodaj `ApplicationDbContext`.

### <a name="update-the-createmodel"></a>Aktualizowanie modelu

Zaktualizuj Konstruktor modelu tworzenia stron, aby używał `DI_BasePageModel` klasy bazowej:

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

Zaktualizuj metodę `CreateModel.OnPostAsync`, aby:

* Dodaj identyfikator użytkownika do modelu `Contact`.
* Wywołaj procedurę obsługi autoryzacji, aby upewnić się, że użytkownik ma uprawnienia do tworzenia kontaktów.

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a>Aktualizowanie IndexModel

Zaktualizuj metodę `OnGetAsync` tak, aby tylko zatwierdzone kontakty były widoczne dla użytkowników ogólnych:

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a>Aktualizowanie EditModel

Dodaj procedurę obsługi autoryzacji, aby sprawdzić, czy użytkownik jest właścicielem osoby kontaktowej. Ponieważ autoryzacja zasobów jest sprawdzana, atrybut `[Authorize]` nie jest wystarczający. Aplikacja nie ma dostępu do zasobu, gdy są oceniane atrybuty. Autoryzacja na podstawie zasobów musi być bezwzględna. Testy muszą zostać wykonane, gdy aplikacja ma dostęp do zasobu, przez załadowanie go w modelu strony lub przez załadowanie go w ramach procedury obsługi. Często uzyskujesz dostęp do zasobu przez przekazanie klucza zasobu.

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a>Aktualizowanie DeleteModel

Zaktualizuj model usuwania stron, aby użyć procedury obsługi autoryzacji do sprawdzenia, czy użytkownik ma uprawnienie do usuwania w kontakcie.

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a>Wsuń usługę autoryzacji do widoków

Obecnie interfejs użytkownika zawiera linki do edycji i usuwania dla kontaktów, których użytkownik nie może modyfikować.

Wsuń usługę autoryzacji w pliku *Pages/_ViewImports. cshtml* , aby była dostępna dla wszystkich widoków:

[!code-cshtml[](secure-data/samples/final3/Pages/_ViewImports.cshtml?highlight=6-99)]

Powyższy znacznik dodaje kilka instrukcji `using`.

Zaktualizuj linki **Edytuj** i **Usuń** w obszarze *strony/Kontakty/index. cshtml* , aby były renderowane tylko dla użytkowników z odpowiednimi uprawnieniami:

[!code-cshtml[](secure-data/samples/final3/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> Ukrycie linków użytkowników, którzy nie mają uprawnień do zmiany danych, nie powoduje zabezpieczenia aplikacji. Ukrycie linków sprawia, że aplikacja jest bardziej przyjazny dla użytkownika, wyświetlając tylko prawidłowe linki. Użytkownicy mogą zahakerować wygenerowane adresy URL, aby wywoływać operacje edycji i usuwania na danych, które nie są właścicielami. Strona Razor lub kontroler musi wymusić sprawdzanie dostępu w celu zabezpieczenia danych.

### <a name="update-details"></a>Szczegóły aktualizacji

Zaktualizuj widok szczegółów, aby menedżerowie mogli zatwierdzać lub odrzucać kontakty:

[!code-cshtml[](secure-data/samples/final3/Pages/Contacts/Details.cshtml?name=snippet)]

Zaktualizuj model strony szczegółów:

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="add-or-remove-a-user-to-a-role"></a>Dodawanie użytkownika do roli lub usuwanie go

Zobacz [ten problem](https://github.com/aspnet/AspNetCore.Docs/issues/8502) , aby uzyskać informacje na temat:

* Usuwanie uprawnień użytkownika. Na przykład wyciszenie użytkownika w aplikacji czatu.
* Dodawanie uprawnień do użytkownika.

## <a name="differences-between-challenge-vs-forbid"></a>Różnice między wyzwaniem a zabranianiem

Ta aplikacja ustawia zasady domyślne, aby [wymagać uwierzytelnionych użytkowników](#require-authenticated-users). Poniższy kod umożliwia anonimowym użytkownikom. Użytkownicy anonimowi mogą wyświetlać różnice między wyzwaniem a zabranianiem.

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Details2.cshtml.cs?name=snippet)]

W powyższym kodzie:

* Gdy użytkownik **nie** jest uwierzytelniony, zwracany jest `ChallengeResult`. Gdy zostanie zwrócona `ChallengeResult`, użytkownik zostanie przekierowany do strony logowania.
* Gdy użytkownik jest uwierzytelniany, ale nie jest autoryzowany, zwracany jest `ForbidResult`. Gdy zostanie zwrócona `ForbidResult`, użytkownik zostanie przekierowany do strony odmowa dostępu.

## <a name="test-the-completed-app"></a>Testowanie ukończonej aplikacji

Jeśli nie ustawiono jeszcze hasła dla kont użytkowników, użyj [Narzędzia Menedżera wpisów tajnych](xref:security/app-secrets#secret-manager) , aby ustawić hasło:

* Wybierz silne hasło: Użyj ośmiu lub więcej znaków oraz co najmniej jednego znaku wielkie litery, cyfry i symbolu. Na przykład `Passw0rd!` spełnia wymagania dotyczące silnych haseł.
* Wykonaj następujące polecenie z folderu projektu, gdzie `<PW>` jest hasłem:

  ```dotnetcli
  dotnet user-secrets set SeedUserPW <PW>
  ```

Jeśli aplikacja ma kontakty:

* Usuń wszystkie rekordy z tabeli `Contact`.
* Uruchom ponownie aplikację, aby wypełniać bazę danych.

Prostym sposobem przetestowania ukończonej aplikacji jest uruchomienie trzech różnych przeglądarek (lub sesji incognito/InPrivate). W jednej przeglądarce Zarejestruj nowego użytkownika (na przykład `test@example.com`). Zaloguj się do każdej przeglądarki za pomocą innego użytkownika. Sprawdź następujące operacje:

* Zarejestrowani użytkownicy mogą wyświetlać wszystkie zatwierdzone dane kontaktowe.
* Zarejestrowani użytkownicy mogą edytować/usuwać własne dane.
* Menedżerowie mogą zatwierdzać i odrzucać dane kontaktów. W widoku `Details` są wyświetlane przyciski **Zatwierdź** i **Odrzuć** .
* Administratorzy mogą zatwierdzić/odrzucić i edytować/usunąć wszystkie dane.

| Użytkownik                | Wypełnianie przez aplikację | Opcje                                  |
| ------------------- | :---------------: | ---------------------------------------- |
| test@example.com    | Nie                | Edytuj/Usuń własne dane.                |
| manager@contoso.com | Tak               | Zatwierdź/Odrzuć i edytuj/usuń własne dane. |
| admin@contoso.com   | Tak               | Zatwierdź/Odrzuć i edytuj/usuń wszystkie dane. |

Utwórz kontakt w przeglądarce administratora. Skopiuj adres URL służący do usuwania i edytowania z osoby kontaktowej administratora. Wklej te linki do przeglądarki użytkownika testowego, aby sprawdzić, czy użytkownik testowy nie może wykonać tych operacji.

## <a name="create-the-starter-app"></a>Tworzenie aplikacji Starter

* Tworzenie aplikacji Razor Pages o nazwie "Contacter"
  * Utwórz aplikację przy użyciu **poszczególnych kont użytkowników**.
  * Nadaj mu nazwę "ContactName", aby przestrzeń nazw była zgodna z przestrzenią nazw używaną w przykładzie.
  * `-uld` określa LocalDB zamiast oprogramowania SQLite

  ```dotnetcli
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* Dodaj *modele/Contact. cs*:

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* Tworzenie szkieletu modelu `Contact`.
* Utwórz migrację początkową i zaktualizuj bazę danych:

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet tool install -g dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
dotnet ef database drop -f
dotnet ef migrations add initial
dotnet ef database update
```

Jeśli wystąpi błąd przy użyciu polecenia `dotnet aspnet-codegenerator razorpage`, zobacz [ten problem](https://github.com/aspnet/Scaffolding/issues/984)w usłudze GitHub.

* Zaktualizuj kotwicę **ContactManager** w pliku *Pages/Shared/_Layout. cshtml* :

 ```cshtml
<a class="navbar-brand" asp-area="" asp-page="/Contacts/Index">ContactManager</a>
  ```

* Testowanie aplikacji przez tworzenie, edytowanie i usuwanie kontaktu

### <a name="seed-the-database"></a>Wypełnianie bazy danych

Dodaj klasę [SeedData](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter3/Data/SeedData.cs) do folderu *danych* :

[!code-csharp[](secure-data/samples/starter3/Data/SeedData.cs)]

Wywołaj `SeedData.Initialize` z `Main`:

[!code-csharp[](secure-data/samples/starter3/Program.cs)]

Sprawdź, czy aplikacja wykorzystana z bazy danych. Jeśli w bazie danych kontaktów znajdują się jakieś wiersze, Metoda inicjatora nie zostanie uruchomiona.

::: moniker-end

::: moniker range=">= aspnetcore-2.1 < aspnetcore-3.0"

W tym samouczku pokazano, jak utworzyć aplikację sieci Web ASP.NET Core przy użyciu danych użytkownika chronionych przez autoryzację. Zostanie wyświetlona lista kontaktów, które zostały utworzone przez uwierzytelnionych (zarejestrowanych) użytkowników. Istnieją trzy grupy zabezpieczeń:

* **Zarejestrowani użytkownicy** mogą wyświetlać wszystkie zatwierdzone dane i edytować/usuwać własne dane.
* **Menedżerowie** mogą zatwierdzać lub odrzucać dane kontaktów. Tylko zatwierdzone kontakty są widoczne dla użytkowników.
* **Administratorzy** mogą zatwierdzić/odrzucić i edytować/usunąć dowolne dane.

Na poniższej ilustracji użytkownik Rick (`rick@example.com`) jest zalogowany. Rick mogą wyświetlać tylko zatwierdzone kontakty i **edytować**/**usunąć**/**tworzyć nowe** linki dla swoich kontaktów. Tylko ostatni rekord utworzony przez Rick zawiera linki do **edycji** i **usuwania** . Inni użytkownicy nie będą widzieć ostatniego rekordu do momentu zmiany stanu na "zatwierdzone" przez Menedżera lub administratora.

![Zrzut ekranu przedstawiający Rick zalogowany](secure-data/_static/rick.png)

Na poniższej ilustracji `manager@contoso.com` jest zalogowany i w roli menedżera:

![Zrzut ekranu przedstawiający manager@contoso.com zalogowany](secure-data/_static/manager1.png)

Na poniższej ilustracji przedstawiono widok szczegółów osoby kontaktowej:

![Widok kontaktu w Menedżerze](secure-data/_static/manager.png)

Przyciski **Zatwierdź** i **Odrzuć** są wyświetlane tylko dla menedżerów i administratorów.

Na poniższej ilustracji `admin@contoso.com` jest zalogowany i w roli administratora:

![Zrzut ekranu przedstawiający admin@contoso.com zalogowany](secure-data/_static/admin.png)

Administrator ma wszystkie uprawnienia. Może odczytywać/edytować/usuwać dowolne kontakty i zmieniać stan kontaktów.

Aplikacja została utworzona przez utworzenie [szkieletu](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) następującego modelu `Contact`:

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

Przykład zawiera następujące programy obsługi autoryzacji:

* `ContactIsOwnerAuthorizationHandler`: zapewnia, że użytkownik może edytować tylko swoje dane.
* `ContactManagerAuthorizationHandler`: umożliwia menedżerom zatwierdzanie lub odrzucanie kontaktów.
* `ContactAdministratorsAuthorizationHandler`: umożliwia administratorom zatwierdzanie lub odrzucanie kontaktów oraz Edytowanie/usuwanie kontaktów.

## <a name="prerequisites"></a>Wymagania wstępne

Ten samouczek jest zaawansowany. Należy zapoznać się z:

* [ASP.NET Core](xref:tutorials/first-mvc-app/start-mvc)
* [Uwierzytelnianie](xref:security/authentication/identity)
* [Potwierdzenie konta i odzyskiwanie hasła](xref:security/authentication/accconfirm)
* [Autoryzacja](xref:security/authorization/introduction)
* [Entity Framework Core](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a>Aplikacja Starter i ukończona

[Pobierz](xref:index#how-to-download-a-sample) [ukończoną](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) aplikację. [Przetestuj](#test-the-completed-app) ukończoną aplikację, aby zapoznać się z jej funkcjami zabezpieczeń.

### <a name="the-starter-app"></a>Aplikacja początkowa

[Pobierz](xref:index#how-to-download-a-sample) aplikację [startową](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) .

Uruchom aplikację, naciśnij link **ContactManager** i sprawdź, czy można tworzyć, edytować i usuwać kontakty.

## <a name="secure-user-data"></a>Zabezpieczanie danych użytkownika

Poniższe sekcje zawierają wszystkie najważniejsze kroki umożliwiające utworzenie aplikacji zabezpieczonych danych użytkownika. Pomocne może być odwołanie do ukończonego projektu.

### <a name="tie-the-contact-data-to-the-user"></a>Powiązanie danych kontaktowych z użytkownikiem

Użyj identyfikatora użytkownika [tożsamości](xref:security/authentication/identity) ASP.NET, aby upewnić się, że użytkownicy będą mogli edytować swoje dane, ale nie inne dane użytkowników. Dodaj `OwnerID` i `ContactStatus` do modelu `Contact`:

[!code-csharp[](secure-data/samples/final2.1/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

`OwnerID` jest IDENTYFIKATORem użytkownika z tabeli `AspNetUser` w bazie danych [tożsamości](xref:security/authentication/identity) . Pole `Status` określa, czy kontakt jest widoczny dla użytkowników ogólnych.

Utwórz nową migrację i zaktualizuj bazę danych:

```dotnetcli
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-identity"></a>Dodawanie usług ról do tożsamości

Dołącz [Addroles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) , aby dodać usługi ról:

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet2&highlight=12)]

### <a name="require-authenticated-users"></a>Wymagaj uwierzytelnionych użytkowników

Ustaw domyślne zasady uwierzytelniania, aby wymagać uwierzytelnienia użytkowników:

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet&highlight=17-99)] 

 Można zrezygnować z uwierzytelniania na stronie Razor, kontrolerze lub poziomie metody akcji z atrybutem `[AllowAnonymous]`. Ustawienie domyślnych zasad uwierzytelniania wymagające uwierzytelnienia użytkowników chroniących nowo dodane Razor Pages i kontrolery. Uwierzytelnianie wymagane domyślnie jest bezpieczniejsze niż poleganie na nowych kontrolerach i Razor Pages uwzględnieniu atrybutu `[Authorize]`.

Dodaj [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) do strony indeks, informacje i kontakty, dzięki czemu anonimowi użytkownicy mogą uzyskać informacje o witrynie przed ich zarejestrowaniem.

[!code-csharp[](secure-data/samples/final2.1/Pages/Index.cshtml.cs?highlight=1,6)]

### <a name="configure-the-test-account"></a>Konfigurowanie konta testowego

Klasa `SeedData` tworzy dwa konta: administrator i Menedżer. Użyj [Narzędzia Secret Manager](xref:security/app-secrets) , aby ustawić hasło dla tych kont. Ustaw hasło z katalogu projektu (katalog zawierający *program.cs*):

```dotnetcli
dotnet user-secrets set SeedUserPW <PW>
```

Jeśli nie określono silnego hasła, wyjątek jest zgłaszany, gdy zostanie wywołane `SeedData.Initialize`.

Zaktualizuj `Main`, aby użyć hasła testu:

[!code-csharp[](secure-data/samples/final2.1/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a>Tworzenie kont testowych i aktualizowanie kontaktów

Zaktualizuj metodę `Initialize` w klasie `SeedData`, aby utworzyć konta testowe:

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet_Initialize)]

Dodaj identyfikator użytkownika administratora i `ContactStatus` do kontaktów. Utwórz jeden z kontaktów "przesłane" i jeden "odrzucony". Dodaj identyfikator i stan użytkownika do wszystkich kontaktów. Pokazywany jest tylko jeden kontakt:

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a>Tworzenie programów do obsługi autoryzacji właściciela, Menedżera i administratora

Utwórz folder *autoryzacji* i Utwórz w nim klasę `ContactIsOwnerAuthorizationHandler`. `ContactIsOwnerAuthorizationHandler` sprawdza, czy użytkownik, który działa na zasobów, jest właścicielem zasobu.

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

Kontekst wywołań `ContactIsOwnerAuthorizationHandler` [. Powodzenie](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) , jeśli bieżący użytkownik uwierzytelniony jest właścicielem osoby kontaktowej. Obsługa autoryzacji zazwyczaj:

* Zwróć `context.Succeed`, gdy wymagania są spełnione.
* Zwróć `Task.CompletedTask`, gdy wymagania nie są spełnione. `Task.CompletedTask` nie powiodło się lub wystąpił błąd&mdash;zezwala na uruchamianie innych programów obsługi autoryzacji.

Jeśli musisz jawnie niepowodzeniem, zwróć [kontekst. Nie powiodło się](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).

Aplikacja umożliwia właścicielom kontaktu Edytowanie/usuwanie/tworzenie własnych danych. `ContactIsOwnerAuthorizationHandler` nie musi sprawdzać operacji przesłanej w parametrze wymagania.

### <a name="create-a-manager-authorization-handler"></a>Tworzenie procedury obsługi autoryzacji Menedżera

Utwórz klasę `ContactManagerAuthorizationHandler` w folderze *autoryzacji* . `ContactManagerAuthorizationHandler` sprawdza, czy użytkownik działający na tym zasobie jest menedżerem. Tylko menedżerowie mogą zatwierdzać lub odrzucać zmiany zawartości (nowe lub zmienione).

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a>Tworzenie procedury obsługi autoryzacji administratora

Utwórz klasę `ContactAdministratorsAuthorizationHandler` w folderze *autoryzacji* . `ContactAdministratorsAuthorizationHandler` sprawdza, czy użytkownik działający na tym zasobie jest administratorem. Administrator może wykonać wszystkie operacje.

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a>Rejestrowanie programów obsługi autoryzacji

Usługi korzystające z Entity Framework Core muszą być zarejestrowane dla [iniekcji zależności](xref:fundamentals/dependency-injection) przy użyciu funkcji [addscoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions). `ContactIsOwnerAuthorizationHandler` używa ASP.NET Core [tożsamości](xref:security/authentication/identity), która jest oparta na Entity Framework Core. Zarejestruj procedury obsługi w kolekcji usług, aby były dostępne dla `ContactsController` za pomocą [iniekcji zależności](xref:fundamentals/dependency-injection). Dodaj następujący kod na końcu `ConfigureServices`:

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet_defaultPolicy&highlight=27-99)]

`ContactAdministratorsAuthorizationHandler` i `ContactManagerAuthorizationHandler` są dodawane jako pojedyncze. Są one pojedynczymi, ponieważ nie korzystają z EF, a wszystkie potrzebne informacje znajdują się w `Context` parametr metody `HandleRequirementAsync`.

## <a name="support-authorization"></a>Obsługa autoryzacji

W tej sekcji należy zaktualizować Razor Pages i dodać klasę wymagania operacji.

### <a name="review-the-contact-operations-requirements-class"></a>Przeglądanie klasy wymagań operacji kontaktu

Zapoznaj się z klasą `ContactOperations`. Ta klasa zawiera wymagania obsługiwane przez aplikację:

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-razor-pages"></a>Utwórz klasę bazową dla kontaktów Razor Pages

Utwórz klasę bazową zawierającą usługi używane w Razor Pages kontaktów. Klasa bazowa umieszcza kod inicjujący w jednej lokalizacji:

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/DI_BasePageModel.cs)]

Poprzedni kod:

* Dodaje usługę `IAuthorizationService`, aby uzyskać dostęp do programów obsługi autoryzacji.
* Dodaje usługę tożsamości `UserManager`.
* Dodaj `ApplicationDbContext`.

### <a name="update-the-createmodel"></a>Aktualizowanie modelu

Zaktualizuj Konstruktor modelu tworzenia stron, aby używał `DI_BasePageModel` klasy bazowej:

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

Zaktualizuj metodę `CreateModel.OnPostAsync`, aby:

* Dodaj identyfikator użytkownika do modelu `Contact`.
* Wywołaj procedurę obsługi autoryzacji, aby upewnić się, że użytkownik ma uprawnienia do tworzenia kontaktów.

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a>Aktualizowanie IndexModel

Zaktualizuj metodę `OnGetAsync` tak, aby tylko zatwierdzone kontakty były widoczne dla użytkowników ogólnych:

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a>Aktualizowanie EditModel

Dodaj procedurę obsługi autoryzacji, aby sprawdzić, czy użytkownik jest właścicielem osoby kontaktowej. Ponieważ autoryzacja zasobów jest sprawdzana, atrybut `[Authorize]` nie jest wystarczający. Aplikacja nie ma dostępu do zasobu, gdy są oceniane atrybuty. Autoryzacja na podstawie zasobów musi być bezwzględna. Testy muszą zostać wykonane, gdy aplikacja ma dostęp do zasobu, przez załadowanie go w modelu strony lub przez załadowanie go w ramach procedury obsługi. Często uzyskujesz dostęp do zasobu przez przekazanie klucza zasobu.

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a>Aktualizowanie DeleteModel

Zaktualizuj model usuwania stron, aby użyć procedury obsługi autoryzacji do sprawdzenia, czy użytkownik ma uprawnienie do usuwania w kontakcie.

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a>Wsuń usługę autoryzacji do widoków

Obecnie interfejs użytkownika zawiera linki do edycji i usuwania dla kontaktów, których użytkownik nie może modyfikować.

Wsuń usługę autoryzacji w pliku *views/_ViewImports. cshtml* , aby była dostępna dla wszystkich widoków:

[!code-cshtml[](secure-data/samples/final2.1/Pages/_ViewImports.cshtml?highlight=6-99)]

Powyższy znacznik dodaje kilka instrukcji `using`.

Zaktualizuj linki **Edytuj** i **Usuń** w obszarze *strony/Kontakty/index. cshtml* , aby były renderowane tylko dla użytkowników z odpowiednimi uprawnieniami:

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> Ukrycie linków użytkowników, którzy nie mają uprawnień do zmiany danych, nie powoduje zabezpieczenia aplikacji. Ukrycie linków sprawia, że aplikacja jest bardziej przyjazny dla użytkownika, wyświetlając tylko prawidłowe linki. Użytkownicy mogą zahakerować wygenerowane adresy URL, aby wywoływać operacje edycji i usuwania na danych, które nie są właścicielami. Strona Razor lub kontroler musi wymusić sprawdzanie dostępu w celu zabezpieczenia danych.

### <a name="update-details"></a>Szczegóły aktualizacji

Zaktualizuj widok szczegółów, aby menedżerowie mogli zatwierdzać lub odrzucać kontakty:

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml?name=snippet)]

Zaktualizuj model strony szczegółów:

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="add-or-remove-a-user-to-a-role"></a>Dodawanie użytkownika do roli lub usuwanie go

Zobacz [ten problem](https://github.com/aspnet/AspNetCore.Docs/issues/8502) , aby uzyskać informacje na temat:

* Usuwanie uprawnień użytkownika. Na przykład wyciszenie użytkownika w aplikacji czatu.
* Dodawanie uprawnień do użytkownika.

## <a name="test-the-completed-app"></a>Testowanie ukończonej aplikacji

Jeśli nie ustawiono jeszcze hasła dla kont użytkowników, użyj [Narzędzia Menedżera wpisów tajnych](xref:security/app-secrets#secret-manager) , aby ustawić hasło:

* Wybierz silne hasło: Użyj ośmiu lub więcej znaków oraz co najmniej jednego znaku wielkie litery, cyfry i symbolu. Na przykład `Passw0rd!` spełnia wymagania dotyczące silnych haseł.
* Wykonaj następujące polecenie z folderu projektu, gdzie `<PW>` jest hasłem:

  ```dotnetcli
  dotnet user-secrets set SeedUserPW <PW>
  ```

* Porzuć i zaktualizuj bazę danych

  ```dotnetcli
  dotnet ef database drop -f
  dotnet ef database update  
  ```

* Uruchom ponownie aplikację, aby wypełniać bazę danych.

Prostym sposobem przetestowania ukończonej aplikacji jest uruchomienie trzech różnych przeglądarek (lub sesji incognito/InPrivate). W jednej przeglądarce Zarejestruj nowego użytkownika (na przykład `test@example.com`). Zaloguj się do każdej przeglądarki za pomocą innego użytkownika. Sprawdź następujące operacje:

* Zarejestrowani użytkownicy mogą wyświetlać wszystkie zatwierdzone dane kontaktowe.
* Zarejestrowani użytkownicy mogą edytować/usuwać własne dane.
* Menedżerowie mogą zatwierdzać i odrzucać dane kontaktów. W widoku `Details` są wyświetlane przyciski **Zatwierdź** i **Odrzuć** .
* Administratorzy mogą zatwierdzić/odrzucić i edytować/usunąć wszystkie dane.

| Użytkownik                | Wypełnianie przez aplikację | Opcje                                  |
| ------------------- | :---------------: | ---------------------------------------- |
| test@example.com    | Nie                | Edytuj/Usuń własne dane.                |
| manager@contoso.com | Tak               | Zatwierdź/Odrzuć i edytuj/usuń własne dane. |
| admin@contoso.com   | Tak               | Zatwierdź/Odrzuć i edytuj/usuń wszystkie dane. |

Utwórz kontakt w przeglądarce administratora. Skopiuj adres URL służący do usuwania i edytowania z osoby kontaktowej administratora. Wklej te linki do przeglądarki użytkownika testowego, aby sprawdzić, czy użytkownik testowy nie może wykonać tych operacji.

## <a name="create-the-starter-app"></a>Tworzenie aplikacji Starter

* Tworzenie aplikacji Razor Pages o nazwie "Contacter"
  * Utwórz aplikację przy użyciu **poszczególnych kont użytkowników**.
  * Nadaj mu nazwę "ContactName", aby przestrzeń nazw była zgodna z przestrzenią nazw używaną w przykładzie.
  * `-uld` określa LocalDB zamiast oprogramowania SQLite

  ```dotnetcli
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* Dodaj *modele/Contact. cs*:

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* Tworzenie szkieletu modelu `Contact`.
* Utwórz migrację początkową i zaktualizuj bazę danych:

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
  dotnet ef database drop -f
  dotnet ef migrations add initial
  dotnet ef database update
  ```

* Zaktualizuj kotwicę **ContactManager** w pliku *Pages/_Layout. cshtml* :

  ```cshtml
  <a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
  ```

* Testowanie aplikacji przez tworzenie, edytowanie i usuwanie kontaktu

### <a name="seed-the-database"></a>Wypełnianie bazy danych

Dodaj klasę [SeedData](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs) do folderu *danych* .

Wywołaj `SeedData.Initialize` z `Main`:

[!code-csharp[](secure-data/samples/starter2.1/Program.cs?name=snippet)]

Sprawdź, czy aplikacja wykorzystana z bazy danych. Jeśli w bazie danych kontaktów znajdują się jakieś wiersze, Metoda inicjatora nie zostanie uruchomiona.

::: moniker-end

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a>Dodatkowe zasoby

* [Tworzenie aplikacji internetowej platformy .NET Core i SQL Database w programie Azure App Service](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb)
* [ASP.NET Core laboratorium autoryzacji](https://github.com/blowdart/AspNetAuthorizationWorkshop). To laboratorium prowadzi do bardziej szczegółowych informacji na temat funkcji zabezpieczeń wprowadzonych w tym samouczku.
* <xref:security/authorization/introduction>
* [Niestandardowa Autoryzacja oparta na zasadach](xref:security/authorization/policies)
