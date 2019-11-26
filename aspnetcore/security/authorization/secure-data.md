---
title: Tworzenie aplikacji platformy ASP.NET Core przy użyciu danych użytkownika chronionych przez autoryzację
author: rick-anderson
description: Dowiedz się, jak utworzyć aplikację stron Razor przy użyciu danych użytkownika chronionych przez autoryzację. Obejmuje protokołu HTTPS, uwierzytelniania, zabezpieczeń i tożsamości platformy ASP.NET Core.
ms.author: riande
ms.date: 12/18/2018
ms.custom: mvc, seodec18
uid: security/authorization/secure-data
ms.openlocfilehash: 65c72d4dd457f85451796c5713bedebafec7a7de
ms.sourcegitcommit: 8157e5a351f49aeef3769f7d38b787b4386aad5f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/20/2019
ms.locfileid: "74239831"
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a>Tworzenie aplikacji platformy ASP.NET Core przy użyciu danych użytkownika chronionych przez autoryzację

Autorzy [Rick Anderson](https://twitter.com/RickAndMSFT) i Jan [Audette](https://twitter.com/joeaudette)

::: moniker range="<= aspnetcore-1.1"

[Ten plik PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) jest wyświetlany w wersji ASP.NET Core MVC. Wersja ASP.NET Core 1,1 tego samouczka znajduje się w [tym](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data) folderze. Przykład 1,1 ASP.NET Core znajduje się w [próbkach](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Zobacz [ten plik PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_July16_18.pdf)

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

W tym samouczku pokazano, jak utworzyć aplikację sieci web platformy ASP.NET Core przy użyciu danych użytkownika chronionych przez autoryzację. Wyświetla listę kontaktów, uwierzytelnionych użytkowników (zarejestrowane), które zostały utworzone. Istnieją trzy grupy zabezpieczeń:

* **Zarejestrowani użytkownicy** mogą wyświetlać wszystkie zatwierdzone dane i edytować/usuwać własne dane.
* **Menedżerowie** mogą zatwierdzać lub odrzucać dane kontaktów. Tylko zatwierdzone kontakty będą widoczne dla użytkowników.
* **Administratorzy** mogą zatwierdzić/odrzucić i edytować/usunąć dowolne dane.

Obrazy w tym dokumencie nie są dokładnie zgodne z najnowszymi szablonami.

Na poniższej ilustracji użytkownik Rick (`rick@example.com`) jest zalogowany. Rick mogą wyświetlać tylko zatwierdzone kontakty i **edytować**/**usunąć**/**tworzyć nowe** linki dla swoich kontaktów. Tylko ostatni rekord utworzony przez Rick zawiera linki do **edycji** i **usuwania** . Inni użytkownicy nie zobaczą ostatni rekord, aż Menedżer lub administrator zmienia stan na "Zatwierdzone".

![Zrzut ekranu przedstawiający Rick zalogowany](secure-data/_static/rick.png)

Na poniższej ilustracji `manager@contoso.com` jest zalogowany i w roli menedżera:

![Zrzut ekranu przedstawiający manager@contoso.com zalogowany](secure-data/_static/manager1.png)

Na poniższej ilustracji przedstawiono menedżerów widoku szczegółów kontaktu:

![Widok menedżera kontaktu](secure-data/_static/manager.png)

Przyciski **Zatwierdź** i **Odrzuć** są wyświetlane tylko dla menedżerów i administratorów.

Na poniższej ilustracji `admin@contoso.com` jest zalogowany i w roli administratora:

![Zrzut ekranu przedstawiający admin@contoso.com zalogowany](secure-data/_static/admin.png)

Administrator ma wszystkie uprawnienia. Ona można odczytu/edytowanie/usuwanie dowolnego skontaktuj się z pomocą i zmienić stan kontaktów.

Aplikacja została utworzona przez utworzenie [szkieletu](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) następującego modelu `Contact`:

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

Przykład zawiera poniższe obsługi autoryzacji:

* `ContactIsOwnerAuthorizationHandler`: zapewnia, że użytkownik może edytować tylko swoje dane.
* `ContactManagerAuthorizationHandler`: umożliwia menedżerom zatwierdzanie lub odrzucanie kontaktów.
* `ContactAdministratorsAuthorizationHandler`: umożliwia administratorom zatwierdzanie lub odrzucanie kontaktów oraz Edytowanie/usuwanie kontaktów.

## <a name="prerequisites"></a>Wymagania wstępne

W tym samouczku jest zaawansowany. Należy zapoznać się z:

* [ASP.NET Core](xref:tutorials/first-mvc-app/start-mvc)
* [Uwierzytelnianie](xref:security/authentication/identity)
* [Potwierdzenie konta i odzyskiwanie hasła](xref:security/authentication/accconfirm)
* [Autoryzacja](xref:security/authorization/introduction)
* [Entity Framework Core](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a>Starter i ukończonej aplikacji

[Pobierz](xref:index#how-to-download-a-sample) [ukończoną](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) aplikację. [Przetestuj](#test-the-completed-app) ukończoną aplikację, aby zapoznać się z jej funkcjami zabezpieczeń.

### <a name="the-starter-app"></a>Aplikację startową

[Pobierz](xref:index#how-to-download-a-sample) aplikację [startową](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) .

Uruchom aplikację, naciśnij link **ContactManager** i sprawdź, czy można tworzyć, edytować i usuwać kontakty.

## <a name="secure-user-data"></a>Zabezpieczanie danych użytkownika

Poniższe sekcje mają główne kroki umożliwiające utworzenie aplikacji danych bezpiecznego użytkownika. Może okazać się przydatne do odwoływania się do projektu ukończona.

### <a name="tie-the-contact-data-to-the-user"></a>Powiąż dane kontaktowe dla użytkownika

Użyj identyfikatora użytkownika [tożsamości](xref:security/authentication/identity) ASP.NET, aby upewnić się, że użytkownicy będą mogli edytować swoje dane, ale nie inne dane użytkowników. Dodaj `OwnerID` i `ContactStatus` do modelu `Contact`:

[!code-csharp[](secure-data/samples/final3/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

`OwnerID` jest IDENTYFIKATORem użytkownika z tabeli `AspNetUser` w bazie danych [tożsamości](xref:security/authentication/identity) . Pole `Status` określa, czy kontakt jest widoczny dla użytkowników ogólnych.

Tworzenie nowej migracji i aktualizowanie bazy danych:

```dotnetcli
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-identity"></a>Dodaj usługi ról do tożsamości

Dołącz [Addroles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) , aby dodać usługi ról:

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet2&highlight=9)]

### <a name="require-authenticated-users"></a>Wymaga uwierzytelnionych użytkowników

Ustaw domyślne zasady uwierzytelniania, aby wymagać od użytkowników uwierzytelniania:

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet&highlight=15-99)] 

 Można zrezygnować z uwierzytelniania na stronie Razor, kontrolerze lub poziomie metody akcji z atrybutem `[AllowAnonymous]`. Ustawienie domyślne zasady uwierzytelniania, aby wymagać od użytkowników uwierzytelniania chroni nowo dodanych stronami Razor i kontrolerów. Uwierzytelnianie wymagane domyślnie jest bezpieczniejsze niż poleganie na nowych kontrolerach i Razor Pages uwzględnieniu atrybutu `[Authorize]`.

Dodaj [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) do stron indeksu i prywatności, aby użytkownicy anonimowi mogli uzyskać informacje o witrynie przed ich zarejestrowaniem.

[!code-csharp[](secure-data/samples/final3/Pages/Index.cshtml.cs?highlight=1,7)]

### <a name="configure-the-test-account"></a>Skonfiguruj konto testu

Klasa `SeedData` tworzy dwa konta: administrator i Menedżer. Użyj [Narzędzia Secret Manager](xref:security/app-secrets) , aby ustawić hasło dla tych kont. Ustaw hasło z katalogu projektu (katalog zawierający *program.cs*):

```dotnetcli
dotnet user-secrets set SeedUserPW <PW>
```

Jeśli nie określono silnego hasła, wyjątek jest zgłaszany, gdy zostanie wywołane `SeedData.Initialize`.

Zaktualizuj `Main`, aby użyć hasła testu:

[!code-csharp[](secure-data/samples/final3/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a>Tworzenie konta testowe i aktualizowanie kontaktów

Zaktualizuj metodę `Initialize` w klasie `SeedData`, aby utworzyć konta testowe:

[!code-csharp[](secure-data/samples/final3/Data/SeedData.cs?name=snippet_Initialize)]

Dodaj identyfikator użytkownika administratora i `ContactStatus` do kontaktów. Określ jeden z kontaktów "Przesłane" i jednym "odrzucone". Dodawanie stanu i Identyfikatora użytkownika do wszystkich kontaktów. Wyświetlane jest tylko jeden kontakt:

[!code-csharp[](secure-data/samples/final3/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a>Utwórz właściciela, Menedżer i obsługi autoryzacji administratora

Utwórz klasę `ContactIsOwnerAuthorizationHandler` w folderze *autoryzacji* . `ContactIsOwnerAuthorizationHandler` sprawdza, czy użytkownik, który działa na zasobów, jest właścicielem zasobu.

[!code-csharp[](secure-data/samples/final3/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

Kontekst wywołań `ContactIsOwnerAuthorizationHandler` [. Powodzenie](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) , jeśli bieżący użytkownik uwierzytelniony jest właścicielem osoby kontaktowej. Programy obsługi autoryzacji zazwyczaj:

* Zwróć `context.Succeed`, gdy wymagania są spełnione.
* Zwróć `Task.CompletedTask`, gdy wymagania nie są spełnione. `Task.CompletedTask` nie powiodło się lub wystąpił błąd&mdash;zezwala na uruchamianie innych programów obsługi autoryzacji.

Jeśli musisz jawnie niepowodzeniem, zwróć [kontekst. Nie powiodło się](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).

Aplikacja umożliwia skontaktuj się z pomocą właścicieli do edycji/delete/tworzenie własnych danych. `ContactIsOwnerAuthorizationHandler` nie musi sprawdzać operacji przesłanej w parametrze wymagania.

### <a name="create-a-manager-authorization-handler"></a>Utwórz procedurę obsługi Menedżer autoryzacji

Utwórz klasę `ContactManagerAuthorizationHandler` w folderze *autoryzacji* . `ContactManagerAuthorizationHandler` sprawdza, czy użytkownik działający na tym zasobie jest menedżerem. Tylko menedżerowie mogli zatwierdzać lub odrzucać zmiany zawartości (nowe lub zmienione).

[!code-csharp[](secure-data/samples/final3/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a>Utwórz procedurę obsługi autoryzacji administratora

Utwórz klasę `ContactAdministratorsAuthorizationHandler` w folderze *autoryzacji* . `ContactAdministratorsAuthorizationHandler` sprawdza, czy użytkownik działający na tym zasobie jest administratorem. Administrator może wykonać wszystkie operacje.

[!code-csharp[](secure-data/samples/final3/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a>Zarejestruj procedury obsługi autoryzacji

Usługi korzystające z Entity Framework Core muszą być zarejestrowane dla [iniekcji zależności](xref:fundamentals/dependency-injection) przy użyciu funkcji [addscoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions). `ContactIsOwnerAuthorizationHandler` używa ASP.NET Core [tożsamości](xref:security/authentication/identity), która jest oparta na Entity Framework Core. Zarejestruj procedury obsługi w kolekcji usług, aby były dostępne dla `ContactsController` za pomocą [iniekcji zależności](xref:fundamentals/dependency-injection). Dodaj następujący kod na końcu `ConfigureServices`:

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet_defaultPolicy&highlight=23-99)]

`ContactAdministratorsAuthorizationHandler` i `ContactManagerAuthorizationHandler` są dodawane jako pojedyncze. Są one pojedynczymi, ponieważ nie korzystają z EF, a wszystkie potrzebne informacje znajdują się w `Context` parametr metody `HandleRequirementAsync`.

## <a name="support-authorization"></a>Obsługuje autoryzację

W tej sekcji służy do aktualizacji stron Razor i Dodaj klasę wymagania dotyczące operacji.

### <a name="review-the-contact-operations-requirements-class"></a>Przegląd klasy wymagania dotyczące operacji kontaktu

Zapoznaj się z klasą `ContactOperations`. Ta klasa zawiera wymagania obsługiwanej przez aplikację:

[!code-csharp[](secure-data/samples/final3/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-razor-pages"></a>Utwórz klasę bazową dla stron Razor kontaktów

Utwórz klasę bazową, zawierający usług wykorzystanych w kontaktach stron Razor. Klasa bazowa umieszcza kod inicjowania w jednej lokalizacji:

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/DI_BasePageModel.cs)]

Powyższy kod:

* Dodaje usługę `IAuthorizationService`, aby uzyskać dostęp do programów obsługi autoryzacji.
* Dodaje usługę tożsamości `UserManager`.
* Dodaj `ApplicationDbContext`.

### <a name="update-the-createmodel"></a>Aktualizacja CreateModel

Zaktualizuj Konstruktor modelu tworzenia stron, aby używał `DI_BasePageModel` klasy bazowej:

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

Zaktualizuj metodę `CreateModel.OnPostAsync`, aby:

* Dodaj identyfikator użytkownika do modelu `Contact`.
* Wywołania obsługi autoryzacji, aby sprawdzić, czy użytkownik ma uprawnienia do tworzenia kontaktów.

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a>Aktualizacja IndexModel

Zaktualizuj metodę `OnGetAsync` tak, aby tylko zatwierdzone kontakty były widoczne dla użytkowników ogólnych:

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a>Aktualizacja EditModel

Dodaj program obsługi autoryzacji, aby sprawdzić, czy użytkownik jest właścicielem kontaktu. Ponieważ autoryzacja zasobów jest sprawdzana, atrybut `[Authorize]` nie jest wystarczający. Aplikacja nie ma dostępu do zasobu, gdy atrybuty są oceniane. Autoryzacja na podstawie zasobów musi być imperatywnego. Testy muszą być wykonywane, gdy aplikacja ma dostęp do zasobu przez załadowanie go w modelu strony lub przez załadowanie go w ramach programu obsługi, sam. Często dostęp do zasobu, przekazując klucz zasobu.

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a>Aktualizacja DeleteModel

Aktualizowanie modelu strony delete na potrzeby obsługi autoryzacji upewnij się, że użytkownik ma odpowiednie uprawnienia do usuwania dla kontaktu.

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a>Wstrzyknięcie usługi autoryzacji do widoków

Obecnie pokazuje interfejsu użytkownika edytowania i usuwania łącza kontaktów, do których użytkownik nie może modyfikować.

Wsuń usługę autoryzacji w pliku *Pages/_ViewImports. cshtml* , aby była dostępna dla wszystkich widoków:

[!code-cshtml[](secure-data/samples/final3/Pages/_ViewImports.cshtml?highlight=6-99)]

Powyższy znacznik dodaje kilka instrukcji `using`.

Zaktualizuj linki **Edytuj** i **Usuń** w obszarze *strony/Kontakty/index. cshtml* , aby były renderowane tylko dla użytkowników z odpowiednimi uprawnieniami:

[!code-cshtml[](secure-data/samples/final3/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> Ukrywanie łączy na podstawie użytkowników, którzy nie mają uprawnień do zmiany danych nie zabezpieczenia aplikacji. Ukrywanie łącza sprawia, że bardziej przyjazny dla użytkownika aplikacji, wyświetlając tylko poprawne linki. Użytkownicy mogą hack wygenerowanego adresy URL, aby wywołać Edytuj i Usuń operacje na danych, które nie są właścicielami. Strona Razor lub kontrolera musi wymuszają operacje sprawdzania dostępu do zabezpieczania danych.

### <a name="update-details"></a>Szczegóły aktualizacji

Zaktualizuj widok szczegółów, aby menedżerowie mogli zatwierdzać lub odrzucać kontaktów:

[!code-cshtml[](secure-data/samples/final3/Pages/Contacts/Details.cshtml?name=snippet)]

Aktualizowanie modelu strony szczegółów:

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="add-or-remove-a-user-to-a-role"></a>Dodawanie lub usuwanie użytkownika do roli

Zobacz [ten problem](https://github.com/aspnet/AspNetCore.Docs/issues/8502) , aby uzyskać informacje na temat:

* Usuwanie uprawnień z użytkownikiem. Na przykład wyciszenie użytkownika w aplikacji czatu.
* Dodawanie uprawnień dla użytkownika.

<a name="challenge"></a>

## <a name="differences-between-challenge-and-forbid"></a>Różnice między wyzwaniem i Zabroń

Ta aplikacja ustawia zasady domyślne, aby [wymagać uwierzytelnionych użytkowników](#require-authenticated-users). Poniższy kod umożliwia anonimowym użytkownikom. Użytkownicy anonimowi mogą wyświetlać różnice między wyzwaniem a zabranianiem.

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Details2.cshtml.cs?name=snippet)]

W powyższym kodzie:

* Gdy użytkownik **nie** jest uwierzytelniony, zwracany jest `ChallengeResult`. Gdy zostanie zwrócona `ChallengeResult`, użytkownik zostanie przekierowany do strony logowania.
* Gdy użytkownik jest uwierzytelniany, ale nie jest autoryzowany, zwracany jest `ForbidResult`. Gdy zostanie zwrócona `ForbidResult`, użytkownik zostanie przekierowany do strony odmowa dostępu.

## <a name="test-the-completed-app"></a>Testowanie aplikacji ukończone

Jeśli nie ustawiono jeszcze hasła dla kont użytkowników, użyj [Narzędzia Menedżera wpisów tajnych](xref:security/app-secrets#secret-manager) , aby ustawić hasło:

* Wybierz silne hasło: Użyj ośmiu lub więcej znaków i co najmniej jeden znak wielkie litery, numer i symboli. Na przykład `Passw0rd!` spełnia wymagania dotyczące silnych haseł.
* Wykonaj następujące polecenie z folderu projektu, gdzie `<PW>` jest hasłem:

  ```dotnetcli
  dotnet user-secrets set SeedUserPW <PW>
  ```

Jeśli aplikacja ma kontaktów:

* Usuń wszystkie rekordy z tabeli `Contact`.
* Ponowne uruchomienie aplikacji w celu umieszczenia bazy danych.

Łatwe testowanie ukończonej aplikacji jest do uruchomienia w trzech różnych przeglądarek (lub sesji incognito/InPrivate). W jednej przeglądarce Zarejestruj nowego użytkownika (na przykład `test@example.com`). Zaloguj się w każdej przeglądarce z innym użytkownikiem. Sprawdź następujące operacje:

* Zarejestrowani użytkownicy można wyświetlić wszystkie zatwierdzone dane kontaktowe.
* Zarejestrowani użytkownicy może edytować/usuwać swoje dane.
* Menedżerowie mogą zatwierdzać/Odrzuć dane kontaktowe. W widoku `Details` są wyświetlane przyciski **Zatwierdź** i **Odrzuć** .
* Administratorzy mogą zatwierdzać/Odrzuć i edytowanie/usuwanie wszystkich danych.

| Użytkownik                | Zasilany przez aplikację | Opcje                                  |
| ------------------- | :---------------: | ---------------------------------------- |
| test@example.com    | Nie                | Edytowanie/usuwanie własnych danych.                |
| manager@contoso.com | Tak               | Zatwierdź/Odrzuć i edytowanie/usuwanie własnych danych. |
| admin@contoso.com   | Tak               | Zatwierdź/Odrzuć i edytowanie/usuwanie wszystkich danych. |

Utwórz kontakt w przeglądarce administratora. Skopiuj adres URL do usunięcia, a następnie Edytuj z skontaktowanie się z administratorem. Wklej te linki do przeglądarki użytkownika testowego, aby sprawdzić, czy użytkownik testu nie może wykonywać te operacje.

## <a name="create-the-starter-app"></a>Utwórz aplikację startową

* Tworzenie stron Razor aplikacji o nazwie "ContactManager"
  * Utwórz aplikację przy użyciu **poszczególnych kont użytkowników**.
  * Nadaj mu nazwę "ContactManager", przestrzeń nazw używaną w próbce pasujących przestrzeni nazw.
  * `-uld` określa LocalDB zamiast oprogramowania SQLite

  ```dotnetcli
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* Dodaj *modele/Contact. cs*:

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* Tworzenie szkieletu modelu `Contact`.
* Tworzenie początkowej migracji i aktualizowanie bazy danych:

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

* Przetestuj aplikację, tworzenia, edytowania i usuwania kontaktu

### <a name="seed-the-database"></a>Inicjowanie bazy danych

Dodaj klasę [SeedData](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter3/Data/SeedData.cs) do folderu *danych* :

[!code-csharp[](secure-data/samples/starter3/Data/SeedData.cs)]

Wywołaj `SeedData.Initialize` z `Main`:

[!code-csharp[](secure-data/samples/starter3/Program.cs)]

Sprawdź, czy aplikacja zasilany bazy danych. W przypadku wszystkich wierszy w skontaktuj się z bazy danych, metoda inicjatora nie zostanie uruchomiony.

::: moniker-end

::: moniker range=">= aspnetcore-2.1 < aspnetcore-3.0"

W tym samouczku pokazano, jak utworzyć aplikację sieci web platformy ASP.NET Core przy użyciu danych użytkownika chronionych przez autoryzację. Wyświetla listę kontaktów, uwierzytelnionych użytkowników (zarejestrowane), które zostały utworzone. Istnieją trzy grupy zabezpieczeń:

* **Zarejestrowani użytkownicy** mogą wyświetlać wszystkie zatwierdzone dane i edytować/usuwać własne dane.
* **Menedżerowie** mogą zatwierdzać lub odrzucać dane kontaktów. Tylko zatwierdzone kontakty będą widoczne dla użytkowników.
* **Administratorzy** mogą zatwierdzić/odrzucić i edytować/usunąć dowolne dane.

Na poniższej ilustracji użytkownik Rick (`rick@example.com`) jest zalogowany. Rick mogą wyświetlać tylko zatwierdzone kontakty i **edytować**/**usunąć**/**tworzyć nowe** linki dla swoich kontaktów. Tylko ostatni rekord utworzony przez Rick zawiera linki do **edycji** i **usuwania** . Inni użytkownicy nie zobaczą ostatni rekord, aż Menedżer lub administrator zmienia stan na "Zatwierdzone".

![Zrzut ekranu przedstawiający Rick zalogowany](secure-data/_static/rick.png)

Na poniższej ilustracji `manager@contoso.com` jest zalogowany i w roli menedżera:

![Zrzut ekranu przedstawiający manager@contoso.com zalogowany](secure-data/_static/manager1.png)

Na poniższej ilustracji przedstawiono menedżerów widoku szczegółów kontaktu:

![Widok menedżera kontaktu](secure-data/_static/manager.png)

Przyciski **Zatwierdź** i **Odrzuć** są wyświetlane tylko dla menedżerów i administratorów.

Na poniższej ilustracji `admin@contoso.com` jest zalogowany i w roli administratora:

![Zrzut ekranu przedstawiający admin@contoso.com zalogowany](secure-data/_static/admin.png)

Administrator ma wszystkie uprawnienia. Ona można odczytu/edytowanie/usuwanie dowolnego skontaktuj się z pomocą i zmienić stan kontaktów.

Aplikacja została utworzona przez utworzenie [szkieletu](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) następującego modelu `Contact`:

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

Przykład zawiera poniższe obsługi autoryzacji:

* `ContactIsOwnerAuthorizationHandler`: zapewnia, że użytkownik może edytować tylko swoje dane.
* `ContactManagerAuthorizationHandler`: umożliwia menedżerom zatwierdzanie lub odrzucanie kontaktów.
* `ContactAdministratorsAuthorizationHandler`: umożliwia administratorom zatwierdzanie lub odrzucanie kontaktów oraz Edytowanie/usuwanie kontaktów.

## <a name="prerequisites"></a>Wymagania wstępne

W tym samouczku jest zaawansowany. Należy zapoznać się z:

* [ASP.NET Core](xref:tutorials/first-mvc-app/start-mvc)
* [Uwierzytelnianie](xref:security/authentication/identity)
* [Potwierdzenie konta i odzyskiwanie hasła](xref:security/authentication/accconfirm)
* [Autoryzacja](xref:security/authorization/introduction)
* [Entity Framework Core](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a>Starter i ukończonej aplikacji

[Pobierz](xref:index#how-to-download-a-sample) [ukończoną](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) aplikację. [Przetestuj](#test-the-completed-app) ukończoną aplikację, aby zapoznać się z jej funkcjami zabezpieczeń.

### <a name="the-starter-app"></a>Aplikację startową

[Pobierz](xref:index#how-to-download-a-sample) aplikację [startową](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) .

Uruchom aplikację, naciśnij link **ContactManager** i sprawdź, czy można tworzyć, edytować i usuwać kontakty.

## <a name="secure-user-data"></a>Zabezpieczanie danych użytkownika

Poniższe sekcje mają główne kroki umożliwiające utworzenie aplikacji danych bezpiecznego użytkownika. Może okazać się przydatne do odwoływania się do projektu ukończona.

### <a name="tie-the-contact-data-to-the-user"></a>Powiąż dane kontaktowe dla użytkownika

Użyj identyfikatora użytkownika [tożsamości](xref:security/authentication/identity) ASP.NET, aby upewnić się, że użytkownicy będą mogli edytować swoje dane, ale nie inne dane użytkowników. Dodaj `OwnerID` i `ContactStatus` do modelu `Contact`:

[!code-csharp[](secure-data/samples/final2.1/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

`OwnerID` jest IDENTYFIKATORem użytkownika z tabeli `AspNetUser` w bazie danych [tożsamości](xref:security/authentication/identity) . Pole `Status` określa, czy kontakt jest widoczny dla użytkowników ogólnych.

Tworzenie nowej migracji i aktualizowanie bazy danych:

```dotnetcli
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-identity"></a>Dodaj usługi ról do tożsamości

Dołącz [Addroles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) , aby dodać usługi ról:

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet2&highlight=12)]

### <a name="require-authenticated-users"></a>Wymaga uwierzytelnionych użytkowników

Ustaw domyślne zasady uwierzytelniania, aby wymagać od użytkowników uwierzytelniania:

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet&highlight=17-99)] 

 Można zrezygnować z uwierzytelniania na stronie Razor, kontrolerze lub poziomie metody akcji z atrybutem `[AllowAnonymous]`. Ustawienie domyślne zasady uwierzytelniania, aby wymagać od użytkowników uwierzytelniania chroni nowo dodanych stronami Razor i kontrolerów. Uwierzytelnianie wymagane domyślnie jest bezpieczniejsze niż poleganie na nowych kontrolerach i Razor Pages uwzględnieniu atrybutu `[Authorize]`.

Dodaj [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) do strony indeks, informacje i kontakty, dzięki czemu anonimowi użytkownicy mogą uzyskać informacje o witrynie przed ich zarejestrowaniem.

[!code-csharp[](secure-data/samples/final2.1/Pages/Index.cshtml.cs?highlight=1,6)]

### <a name="configure-the-test-account"></a>Skonfiguruj konto testu

Klasa `SeedData` tworzy dwa konta: administrator i Menedżer. Użyj [Narzędzia Secret Manager](xref:security/app-secrets) , aby ustawić hasło dla tych kont. Ustaw hasło z katalogu projektu (katalog zawierający *program.cs*):

```dotnetcli
dotnet user-secrets set SeedUserPW <PW>
```

Jeśli nie określono silnego hasła, wyjątek jest zgłaszany, gdy zostanie wywołane `SeedData.Initialize`.

Zaktualizuj `Main`, aby użyć hasła testu:

[!code-csharp[](secure-data/samples/final2.1/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a>Tworzenie konta testowe i aktualizowanie kontaktów

Zaktualizuj metodę `Initialize` w klasie `SeedData`, aby utworzyć konta testowe:

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet_Initialize)]

Dodaj identyfikator użytkownika administratora i `ContactStatus` do kontaktów. Określ jeden z kontaktów "Przesłane" i jednym "odrzucone". Dodawanie stanu i Identyfikatora użytkownika do wszystkich kontaktów. Wyświetlane jest tylko jeden kontakt:

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a>Utwórz właściciela, Menedżer i obsługi autoryzacji administratora

Utwórz folder *autoryzacji* i Utwórz w nim klasę `ContactIsOwnerAuthorizationHandler`. `ContactIsOwnerAuthorizationHandler` sprawdza, czy użytkownik, który działa na zasobów, jest właścicielem zasobu.

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

Kontekst wywołań `ContactIsOwnerAuthorizationHandler` [. Powodzenie](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) , jeśli bieżący użytkownik uwierzytelniony jest właścicielem osoby kontaktowej. Programy obsługi autoryzacji zazwyczaj:

* Zwróć `context.Succeed`, gdy wymagania są spełnione.
* Zwróć `Task.CompletedTask`, gdy wymagania nie są spełnione. `Task.CompletedTask` nie powiodło się lub wystąpił błąd&mdash;zezwala na uruchamianie innych programów obsługi autoryzacji.

Jeśli musisz jawnie niepowodzeniem, zwróć [kontekst. Nie powiodło się](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).

Aplikacja umożliwia skontaktuj się z pomocą właścicieli do edycji/delete/tworzenie własnych danych. `ContactIsOwnerAuthorizationHandler` nie musi sprawdzać operacji przesłanej w parametrze wymagania.

### <a name="create-a-manager-authorization-handler"></a>Utwórz procedurę obsługi Menedżer autoryzacji

Utwórz klasę `ContactManagerAuthorizationHandler` w folderze *autoryzacji* . `ContactManagerAuthorizationHandler` sprawdza, czy użytkownik działający na tym zasobie jest menedżerem. Tylko menedżerowie mogli zatwierdzać lub odrzucać zmiany zawartości (nowe lub zmienione).

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a>Utwórz procedurę obsługi autoryzacji administratora

Utwórz klasę `ContactAdministratorsAuthorizationHandler` w folderze *autoryzacji* . `ContactAdministratorsAuthorizationHandler` sprawdza, czy użytkownik działający na tym zasobie jest administratorem. Administrator może wykonać wszystkie operacje.

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a>Zarejestruj procedury obsługi autoryzacji

Usługi korzystające z Entity Framework Core muszą być zarejestrowane dla [iniekcji zależności](xref:fundamentals/dependency-injection) przy użyciu funkcji [addscoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions). `ContactIsOwnerAuthorizationHandler` używa ASP.NET Core [tożsamości](xref:security/authentication/identity), która jest oparta na Entity Framework Core. Zarejestruj procedury obsługi w kolekcji usług, aby były dostępne dla `ContactsController` za pomocą [iniekcji zależności](xref:fundamentals/dependency-injection). Dodaj następujący kod na końcu `ConfigureServices`:

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet_defaultPolicy&highlight=27-99)]

`ContactAdministratorsAuthorizationHandler` i `ContactManagerAuthorizationHandler` są dodawane jako pojedyncze. Są one pojedynczymi, ponieważ nie korzystają z EF, a wszystkie potrzebne informacje znajdują się w `Context` parametr metody `HandleRequirementAsync`.

## <a name="support-authorization"></a>Obsługuje autoryzację

W tej sekcji służy do aktualizacji stron Razor i Dodaj klasę wymagania dotyczące operacji.

### <a name="review-the-contact-operations-requirements-class"></a>Przegląd klasy wymagania dotyczące operacji kontaktu

Zapoznaj się z klasą `ContactOperations`. Ta klasa zawiera wymagania obsługiwanej przez aplikację:

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-razor-pages"></a>Utwórz klasę bazową dla stron Razor kontaktów

Utwórz klasę bazową, zawierający usług wykorzystanych w kontaktach stron Razor. Klasa bazowa umieszcza kod inicjowania w jednej lokalizacji:

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/DI_BasePageModel.cs)]

Powyższy kod:

* Dodaje usługę `IAuthorizationService`, aby uzyskać dostęp do programów obsługi autoryzacji.
* Dodaje usługę tożsamości `UserManager`.
* Dodaj `ApplicationDbContext`.

### <a name="update-the-createmodel"></a>Aktualizacja CreateModel

Zaktualizuj Konstruktor modelu tworzenia stron, aby używał `DI_BasePageModel` klasy bazowej:

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

Zaktualizuj metodę `CreateModel.OnPostAsync`, aby:

* Dodaj identyfikator użytkownika do modelu `Contact`.
* Wywołania obsługi autoryzacji, aby sprawdzić, czy użytkownik ma uprawnienia do tworzenia kontaktów.

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a>Aktualizacja IndexModel

Zaktualizuj metodę `OnGetAsync` tak, aby tylko zatwierdzone kontakty były widoczne dla użytkowników ogólnych:

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a>Aktualizacja EditModel

Dodaj program obsługi autoryzacji, aby sprawdzić, czy użytkownik jest właścicielem kontaktu. Ponieważ autoryzacja zasobów jest sprawdzana, atrybut `[Authorize]` nie jest wystarczający. Aplikacja nie ma dostępu do zasobu, gdy atrybuty są oceniane. Autoryzacja na podstawie zasobów musi być imperatywnego. Testy muszą być wykonywane, gdy aplikacja ma dostęp do zasobu przez załadowanie go w modelu strony lub przez załadowanie go w ramach programu obsługi, sam. Często dostęp do zasobu, przekazując klucz zasobu.

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a>Aktualizacja DeleteModel

Aktualizowanie modelu strony delete na potrzeby obsługi autoryzacji upewnij się, że użytkownik ma odpowiednie uprawnienia do usuwania dla kontaktu.

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a>Wstrzyknięcie usługi autoryzacji do widoków

Obecnie pokazuje interfejsu użytkownika edytowania i usuwania łącza kontaktów, do których użytkownik nie może modyfikować.

Wsuń usługę autoryzacji w pliku *views/_ViewImports. cshtml* , aby była dostępna dla wszystkich widoków:

[!code-cshtml[](secure-data/samples/final2.1/Pages/_ViewImports.cshtml?highlight=6-99)]

Powyższy znacznik dodaje kilka instrukcji `using`.

Zaktualizuj linki **Edytuj** i **Usuń** w obszarze *strony/Kontakty/index. cshtml* , aby były renderowane tylko dla użytkowników z odpowiednimi uprawnieniami:

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> Ukrywanie łączy na podstawie użytkowników, którzy nie mają uprawnień do zmiany danych nie zabezpieczenia aplikacji. Ukrywanie łącza sprawia, że bardziej przyjazny dla użytkownika aplikacji, wyświetlając tylko poprawne linki. Użytkownicy mogą hack wygenerowanego adresy URL, aby wywołać Edytuj i Usuń operacje na danych, które nie są właścicielami. Strona Razor lub kontrolera musi wymuszają operacje sprawdzania dostępu do zabezpieczania danych.

### <a name="update-details"></a>Szczegóły aktualizacji

Zaktualizuj widok szczegółów, aby menedżerowie mogli zatwierdzać lub odrzucać kontaktów:

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml?name=snippet)]

Aktualizowanie modelu strony szczegółów:

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="add-or-remove-a-user-to-a-role"></a>Dodawanie lub usuwanie użytkownika do roli

Zobacz [ten problem](https://github.com/aspnet/AspNetCore.Docs/issues/8502) , aby uzyskać informacje na temat:

* Usuwanie uprawnień z użytkownikiem. Na przykład wyciszenie użytkownika w aplikacji czatu.
* Dodawanie uprawnień dla użytkownika.

## <a name="test-the-completed-app"></a>Testowanie aplikacji ukończone

Jeśli nie ustawiono jeszcze hasła dla kont użytkowników, użyj [Narzędzia Menedżera wpisów tajnych](xref:security/app-secrets#secret-manager) , aby ustawić hasło:

* Wybierz silne hasło: Użyj ośmiu lub więcej znaków i co najmniej jeden znak wielkie litery, numer i symboli. Na przykład `Passw0rd!` spełnia wymagania dotyczące silnych haseł.
* Wykonaj następujące polecenie z folderu projektu, gdzie `<PW>` jest hasłem:

  ```dotnetcli
  dotnet user-secrets set SeedUserPW <PW>
  ```

* Porzuć i zaktualizuj bazę danych

  ```dotnetcli
  dotnet ef database drop -f
  dotnet ef database update  
  ```

* Ponowne uruchomienie aplikacji w celu umieszczenia bazy danych.

Łatwe testowanie ukończonej aplikacji jest do uruchomienia w trzech różnych przeglądarek (lub sesji incognito/InPrivate). W jednej przeglądarce Zarejestruj nowego użytkownika (na przykład `test@example.com`). Zaloguj się w każdej przeglądarce z innym użytkownikiem. Sprawdź następujące operacje:

* Zarejestrowani użytkownicy można wyświetlić wszystkie zatwierdzone dane kontaktowe.
* Zarejestrowani użytkownicy może edytować/usuwać swoje dane.
* Menedżerowie mogą zatwierdzać/Odrzuć dane kontaktowe. W widoku `Details` są wyświetlane przyciski **Zatwierdź** i **Odrzuć** .
* Administratorzy mogą zatwierdzać/Odrzuć i edytowanie/usuwanie wszystkich danych.

| Użytkownik                | Zasilany przez aplikację | Opcje                                  |
| ------------------- | :---------------: | ---------------------------------------- |
| test@example.com    | Nie                | Edytowanie/usuwanie własnych danych.                |
| manager@contoso.com | Tak               | Zatwierdź/Odrzuć i edytowanie/usuwanie własnych danych. |
| admin@contoso.com   | Tak               | Zatwierdź/Odrzuć i edytowanie/usuwanie wszystkich danych. |

Utwórz kontakt w przeglądarce administratora. Skopiuj adres URL do usunięcia, a następnie Edytuj z skontaktowanie się z administratorem. Wklej te linki do przeglądarki użytkownika testowego, aby sprawdzić, czy użytkownik testu nie może wykonywać te operacje.

## <a name="create-the-starter-app"></a>Utwórz aplikację startową

* Tworzenie stron Razor aplikacji o nazwie "ContactManager"
  * Utwórz aplikację przy użyciu **poszczególnych kont użytkowników**.
  * Nadaj mu nazwę "ContactManager", przestrzeń nazw używaną w próbce pasujących przestrzeni nazw.
  * `-uld` określa LocalDB zamiast oprogramowania SQLite

  ```dotnetcli
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* Dodaj *modele/Contact. cs*:

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* Tworzenie szkieletu modelu `Contact`.
* Tworzenie początkowej migracji i aktualizowanie bazy danych:

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
  dotnet ef database drop -f
  dotnet ef migrations add initial
  dotnet ef database update
  ```

* Zaktualizuj kotwicę **ContactManager** w pliku *pages/_Layout. cshtml* :

  ```cshtml
  <a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
  ```

* Przetestuj aplikację, tworzenia, edytowania i usuwania kontaktu

### <a name="seed-the-database"></a>Inicjowanie bazy danych

Dodaj klasę [SeedData](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs) do folderu *danych* .

Wywołaj `SeedData.Initialize` z `Main`:

[!code-csharp[](secure-data/samples/starter2.1/Program.cs?name=snippet)]

Sprawdź, czy aplikacja zasilany bazy danych. W przypadku wszystkich wierszy w skontaktuj się z bazy danych, metoda inicjatora nie zostanie uruchomiony.

::: moniker-end

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a>Dodatkowe zasoby

* [Tworzenie aplikacji internetowej platformy .NET Core i SQL Database w programie Azure App Service](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb)
* [ASP.NET Core laboratorium autoryzacji](https://github.com/blowdart/AspNetAuthorizationWorkshop). W tym laboratorium zawiera bardziej szczegółowe na temat funkcji zabezpieczeń wprowadzone w ramach tego samouczka.
* <xref:security/authorization/introduction>
* [Niestandardowa Autoryzacja oparta na zasadach](xref:security/authorization/policies)
