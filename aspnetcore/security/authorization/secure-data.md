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
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a><span data-ttu-id="200e6-103">Tworzenie aplikacji platformy ASP.NET Core z danych użytkownika chronione przez autoryzacji</span><span class="sxs-lookup"><span data-stu-id="200e6-103">Create an ASP.NET Core app with user data protected by authorization</span></span>

<span data-ttu-id="200e6-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Audette Jan](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="200e6-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="200e6-105">W tym samouczku przedstawiono sposób tworzenia aplikacji sieci web z danymi użytkownika chronione przez autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="200e6-105">This tutorial shows how to create a web app with user data protected by authorization.</span></span> <span data-ttu-id="200e6-106">Wyświetla listę kontaktów, które uwierzytelnionych użytkowników (zarejestrowanym) został utworzony.</span><span class="sxs-lookup"><span data-stu-id="200e6-106">It  displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="200e6-107">Istnieją trzy grupy zabezpieczeń:</span><span class="sxs-lookup"><span data-stu-id="200e6-107">There are three security groups:</span></span>

* <span data-ttu-id="200e6-108">Zarejestrowani użytkownicy mogą wyświetlać wszystkie zatwierdzone dane kontaktu.</span><span class="sxs-lookup"><span data-stu-id="200e6-108">Registered users can view all the approved contact data.</span></span>
* <span data-ttu-id="200e6-109">Zarejestrowani użytkownicy mogą edytowanie/usuwanie własnych danych.</span><span class="sxs-lookup"><span data-stu-id="200e6-109">Registered users can edit/delete their own data.</span></span> 
* <span data-ttu-id="200e6-110">Menedżerowie można zatwierdzić lub odrzucić dane kontaktowe.</span><span class="sxs-lookup"><span data-stu-id="200e6-110">Managers can approve or reject contact data.</span></span> <span data-ttu-id="200e6-111">Tylko zatwierdzone kontakty są widoczne dla użytkowników.</span><span class="sxs-lookup"><span data-stu-id="200e6-111">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="200e6-112">Administratorzy mogą zatwierdzać Odrzuć i edytowanie/usuwanie żadnych danych.</span><span class="sxs-lookup"><span data-stu-id="200e6-112">Administrators can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="200e6-113">Na poniższej ilustracji użytkownik Rick (`rick@example.com`) jest zalogowany.</span><span class="sxs-lookup"><span data-stu-id="200e6-113">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="200e6-114">Użytkownik Rick może tylko widok zatwierdzone kontaktów i edytowanie/usuwanie jego kontaktów.</span><span class="sxs-lookup"><span data-stu-id="200e6-114">User Rick can only view approved contacts and edit/delete his contacts.</span></span> <span data-ttu-id="200e6-115">Tylko ostatni rekord powstały Rick, wyświetla edycji i usuwania łącza</span><span class="sxs-lookup"><span data-stu-id="200e6-115">Only the last record, created by Rick, displays edit and delete links</span></span>

![Powyższy obraz](secure-data/_static/rick.png)

<span data-ttu-id="200e6-117">Na poniższej ilustracji `manager@contoso.com` jest zarejestrowany i rolę menedżerów.</span><span class="sxs-lookup"><span data-stu-id="200e6-117">In the following image, `manager@contoso.com` is signed in and in the managers role.</span></span> 

![Powyższy obraz](secure-data/_static/manager1.png)

<span data-ttu-id="200e6-119">Na poniższej ilustracji przedstawiono menedżerów widoku Szczegóły kontaktu.</span><span class="sxs-lookup"><span data-stu-id="200e6-119">The following image shows the  managers details view of a contact.</span></span>

![Powyższy obraz](secure-data/_static/manager.png)

<span data-ttu-id="200e6-121">Tylko menedżerów i Administratorzy mają Zatwierdź i odrzucić przycisków.</span><span class="sxs-lookup"><span data-stu-id="200e6-121">Only managers and administrators have the approve and reject buttons.</span></span>

<span data-ttu-id="200e6-122">Na poniższej ilustracji `admin@contoso.com` jest zarejestrowany i w roli administratora.</span><span class="sxs-lookup"><span data-stu-id="200e6-122">In the following image, `admin@contoso.com` is signed in and in the administrator’s role.</span></span> 

![Powyższy obraz](secure-data/_static/admin.png)

<span data-ttu-id="200e6-124">Administrator ma wszystkie uprawnienia.</span><span class="sxs-lookup"><span data-stu-id="200e6-124">The administrator has all privileges.</span></span> <span data-ttu-id="200e6-125">Użytkownik może odczytu/edytowanie/usuwanie żadnych skontaktuj się z i Zmień stan kontaktów.</span><span class="sxs-lookup"><span data-stu-id="200e6-125">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="200e6-126">Aplikacja została utworzona przez [szkieletów](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) następujące `Contact` modelu:</span><span class="sxs-lookup"><span data-stu-id="200e6-126">The app was created by [scaffolding](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller)  the following `Contact` model:</span></span>

[!code-csharp[Main](secure-data/samples/starter/Models/Contact.cs?name=snippet1)]

<span data-ttu-id="200e6-127">A `ContactIsOwnerAuthorizationHandler` obsługi autoryzacji gwarantuje, że użytkownika można edytować tylko ich danych.</span><span class="sxs-lookup"><span data-stu-id="200e6-127">A `ContactIsOwnerAuthorizationHandler` authorization handler ensures that a user can only edit their data.</span></span> <span data-ttu-id="200e6-128">A `ContactManagerAuthorizationHandler` obsługi autoryzacji umożliwia menedżerów zatwierdzić lub odrzucić kontaktów.</span><span class="sxs-lookup"><span data-stu-id="200e6-128">A `ContactManagerAuthorizationHandler` authorization handler allows managers to approve or reject contacts.</span></span>  <span data-ttu-id="200e6-129">A `ContactAdministratorsAuthorizationHandler` obsługi autoryzacji pozwala administratorom, aby zatwierdzić lub odrzucić kontaktów i edytowanie/usuwanie kontaktów.</span><span class="sxs-lookup"><span data-stu-id="200e6-129">A `ContactAdministratorsAuthorizationHandler` authorization handler allows administrators to approve or reject contacts and to edit/delete contacts.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="200e6-130">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="200e6-130">Prerequisites</span></span>

<span data-ttu-id="200e6-131">Nie jest to początek samouczka.</span><span class="sxs-lookup"><span data-stu-id="200e6-131">This is not a beginning tutorial.</span></span> <span data-ttu-id="200e6-132">Należy zapoznać się z:</span><span class="sxs-lookup"><span data-stu-id="200e6-132">You should be familiar with:</span></span>

* [<span data-ttu-id="200e6-133">Podstawowe ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="200e6-133">ASP.NET Core MVC</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="200e6-134">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="200e6-134">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="200e6-135">Starter i ukończonej aplikacji</span><span class="sxs-lookup"><span data-stu-id="200e6-135">The starter and completed app</span></span>

<span data-ttu-id="200e6-136">[Pobierz](xref:tutorials/index#how-to-download-a-sample) [ukończone](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final) aplikacji.</span><span class="sxs-lookup"><span data-stu-id="200e6-136">[Download](xref:tutorials/index#how-to-download-a-sample) the [completed](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final) app.</span></span> <span data-ttu-id="200e6-137">[Test](#test-the-completed-app) ukończonej aplikacji, należy zapoznać się z jego funkcje zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="200e6-137">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span> 

### <a name="the-starter-app"></a><span data-ttu-id="200e6-138">Początkową aplikację</span><span class="sxs-lookup"><span data-stu-id="200e6-138">The starter app</span></span>

<span data-ttu-id="200e6-139">Warto porównać Twój kod z próbką ukończone.</span><span class="sxs-lookup"><span data-stu-id="200e6-139">It's helpful to compare your code with the completed sample.</span></span>

<span data-ttu-id="200e6-140">[Pobierz](xref:tutorials/index#how-to-download-a-sample) [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter) aplikacji.</span><span class="sxs-lookup"><span data-stu-id="200e6-140">[Download](xref:tutorials/index#how-to-download-a-sample) the [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter) app.</span></span> 

<span data-ttu-id="200e6-141">Zobacz [utworzyć początkową aplikację](#create-the-starter-app) czy chcesz go utworzyć od podstaw.</span><span class="sxs-lookup"><span data-stu-id="200e6-141">See [Create the starter app](#create-the-starter-app) if you'd like to create it from scratch.</span></span>

<span data-ttu-id="200e6-142">Aktualizacja bazy danych:</span><span class="sxs-lookup"><span data-stu-id="200e6-142">Update the database:</span></span>

```none
   dotnet ef database update
```

<span data-ttu-id="200e6-143">Uruchom aplikację, wybierz **ContactManager** link i sprawdź, tworzenie, edytowanie i usuwanie kontaktu.</span><span class="sxs-lookup"><span data-stu-id="200e6-143">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

<span data-ttu-id="200e6-144">Ten samouczek ma główne kroki umożliwiające utworzenie aplikacji danych bezpiecznego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="200e6-144">This tutorial has all the major steps to create the secure user data app.</span></span> <span data-ttu-id="200e6-145">Może być przydatne do odwoływania się do projektu zakończone.</span><span class="sxs-lookup"><span data-stu-id="200e6-145">You may find it helpful to refer to the completed project.</span></span>

## <a name="modify-the-app-to-secure-user-data"></a><span data-ttu-id="200e6-146">Zmodyfikować aplikację w celu zabezpieczenia danych użytkownika</span><span class="sxs-lookup"><span data-stu-id="200e6-146">Modify the app to secure user data</span></span>

<span data-ttu-id="200e6-147">Następujące sekcje zostały wszystkie główne kroki umożliwiające utworzenie aplikacji danych bezpiecznego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="200e6-147">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="200e6-148">Może być przydatne do odwoływania się do projektu zakończone.</span><span class="sxs-lookup"><span data-stu-id="200e6-148">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="200e6-149">Powiązanie dane kontaktowe dla użytkownika</span><span class="sxs-lookup"><span data-stu-id="200e6-149">Tie the contact data to the user</span></span>

<span data-ttu-id="200e6-150">Za pomocą programu ASP.NET [tożsamości](xref:security/authentication/identity) identyfikator użytkownika, aby zapewnić użytkownikom można edytować swoje dane, ale nie do innych danych użytkowników.</span><span class="sxs-lookup"><span data-stu-id="200e6-150">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="200e6-151">Dodaj `OwnerID` do `Contact` modelu:</span><span class="sxs-lookup"><span data-stu-id="200e6-151">Add `OwnerID` to the `Contact` model:</span></span>

[!code-csharp[Main](secure-data/samples/final/Models/Contact.cs?name=snippet1&highlight=5-6,16-)]

<span data-ttu-id="200e6-152">`OwnerID`Identyfikator użytkownika z `AspNetUser` tabeli w [tożsamości](xref:security/authentication/identity) bazy danych.</span><span class="sxs-lookup"><span data-stu-id="200e6-152">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="200e6-153">`Status` Pole określa, czy kontakt jest widoczny dla zwykłych użytkowników.</span><span class="sxs-lookup"><span data-stu-id="200e6-153">The `Status` field determines if a contact is viewable by general users.</span></span> 

<span data-ttu-id="200e6-154">Tworzenie szkieletu nowych migracji i aktualizacji bazy danych:</span><span class="sxs-lookup"><span data-stu-id="200e6-154">Scaffold a new migration and update the database:</span></span>

```console
dotnet ef migrations add userID_Status
dotnet ef database update
 ```

### <a name="require-ssl-and-authenticated-users"></a><span data-ttu-id="200e6-155">Wymagaj protokołu SSL i uwierzytelnionych użytkowników</span><span class="sxs-lookup"><span data-stu-id="200e6-155">Require SSL and authenticated users</span></span>

<span data-ttu-id="200e6-156">W `ConfigureServices` metody *Startup.cs* plików, dodawanie [RequireHttpsAttribute](/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) filtr autoryzacji:</span><span class="sxs-lookup"><span data-stu-id="200e6-156">In the `ConfigureServices` method of the *Startup.cs* file, add the [RequireHttpsAttribute](/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) authorization filter:</span></span>

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=snippet_SSL&highlight=1)]

<span data-ttu-id="200e6-157">Przekierowywanie żądań HTTP, HTTPS, zobacz [ponowne zapisywanie adresów URL w oprogramowaniu pośredniczącym](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="200e6-157">To redirect HTTP requests to HTTPS, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="200e6-158">Jeśli przy użyciu programu Visual Studio Code lub testowania na platformie lokalnego, który nie zawiera certyfikatu testowego dla protokołu SSL:</span><span class="sxs-lookup"><span data-stu-id="200e6-158">If you are using Visual Studio Code or testing on local platform that doesn't include a test certificate for SSL:</span></span>

- <span data-ttu-id="200e6-159">Ustaw `"LocalTest:skipSSL": true` w *appsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="200e6-159">Set `"LocalTest:skipSSL": true` in the *appsettings.json* file.</span></span>

### <a name="require-authenticated-users"></a><span data-ttu-id="200e6-160">Wymaga uwierzytelnionych użytkowników</span><span class="sxs-lookup"><span data-stu-id="200e6-160">Require authenticated users</span></span>

<span data-ttu-id="200e6-161">Ustaw domyślne zasady uwierzytelniania, aby wymagać od użytkowników uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="200e6-161">Set the default authentication policy to require users to be authenticated.</span></span> <span data-ttu-id="200e6-162">Można zrezygnować z uwierzytelniania metodą kontroler lub akcję z `[AllowAnonymous]` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="200e6-162">You can opt out of authentication at the controller or action method with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="200e6-163">Z tej metody żadnych nowych kontrolerów dodaje automatycznie wymaga uwierzytelniania, który jest bezpieczniejszy niż polegania na nowych kontrolerów, aby uwzględnić `[Authorize]` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="200e6-163">With this approach, any new controllers added will automatically require authentication, which is safer than relying on new controllers to include the `[Authorize]` attribute.</span></span> <span data-ttu-id="200e6-164">Dodaj następujący kod do `ConfigureServices` metody *Startup.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="200e6-164">Add the following to  the `ConfigureServices` method of the *Startup.cs* file:</span></span>

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=snippet_defaultPolicy)]

<span data-ttu-id="200e6-165">Dodaj `[AllowAnonymous]` kontrolerowi macierzystego, użytkowników anonimowych można uzyskać informacji o lokacji, przed ich zarejestrować.</span><span class="sxs-lookup"><span data-stu-id="200e6-165">Add `[AllowAnonymous]` to the home controller so anonymous users can get information about the site before they register.</span></span>

[!code-csharp[Main](secure-data/samples/final/Controllers/HomeController.cs?name=snippet1&highlight=2,6)]

### <a name="configure-the-test-account"></a><span data-ttu-id="200e6-166">Skonfiguruj konto testu</span><span class="sxs-lookup"><span data-stu-id="200e6-166">Configure the test account</span></span>

<span data-ttu-id="200e6-167">`SeedData` Klasy tworzy dwa konta administratora i menedżera.</span><span class="sxs-lookup"><span data-stu-id="200e6-167">The `SeedData` class creates two accounts,  administrator and manager.</span></span> <span data-ttu-id="200e6-168">Użyj [narzędzie Menedżer klucz tajny](xref:security/app-secrets) ustawienie hasła dla tych kont.</span><span class="sxs-lookup"><span data-stu-id="200e6-168">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="200e6-169">W tym z katalogu projektu (katalog zawierający *Program.cs*).</span><span class="sxs-lookup"><span data-stu-id="200e6-169">Do this from the project directory (the directory containing *Program.cs*).</span></span>

```console
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="200e6-170">Aktualizacja `Configure` hasła testu:</span><span class="sxs-lookup"><span data-stu-id="200e6-170">Update `Configure` to use the test password:</span></span>

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=Configure&highlight=19-21)]

<span data-ttu-id="200e6-171">Dodaj identyfikator użytkownika administratora i `Status = ContactStatus.Approved` do kontaktów.</span><span class="sxs-lookup"><span data-stu-id="200e6-171">Add the administrator user ID and `Status = ContactStatus.Approved` to the contacts.</span></span> <span data-ttu-id="200e6-172">Skontaktuj się z tylko jedną jest wyświetlana, Dodaj identyfikator użytkownika do wszystkich kontaktów:</span><span class="sxs-lookup"><span data-stu-id="200e6-172">Only one contact is shown, add the user ID to all contacts:</span></span>

[!code-csharp[Main](secure-data/samples/final/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="200e6-173">Utwórz właściciela, Menedżer i obsługi autoryzacji administratora</span><span class="sxs-lookup"><span data-stu-id="200e6-173">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="200e6-174">Utwórz `ContactIsOwnerAuthorizationHandler` klasy w *autoryzacji* folderu.</span><span class="sxs-lookup"><span data-stu-id="200e6-174">Create a `ContactIsOwnerAuthorizationHandler` class in the  *Authorization* folder.</span></span> <span data-ttu-id="200e6-175">`ContactIsOwnerAuthorizationHandler` Zweryfikuje użytkownika na zasób jest właścicielem zasobu.</span><span class="sxs-lookup"><span data-stu-id="200e6-175">The `ContactIsOwnerAuthorizationHandler` will verify the user acting on the resource owns the resource.</span></span>

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="200e6-176">`ContactIsOwnerAuthorizationHandler` Wywołania `context.Succeed` bieżący uwierzytelniony użytkownik jest właścicielem kontaktu.</span><span class="sxs-lookup"><span data-stu-id="200e6-176">The `ContactIsOwnerAuthorizationHandler` calls `context.Succeed` if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="200e6-177">Programy obsługi autoryzacji zazwyczaj zwracać `context.Succeed` gdy są spełnione wymagania.</span><span class="sxs-lookup"><span data-stu-id="200e6-177">Authorization handlers generally return `context.Succeed` when the requirements are met.</span></span> <span data-ttu-id="200e6-178">Zwracają `Task.FromResult(0)` gdy nie są spełnione wymagania.</span><span class="sxs-lookup"><span data-stu-id="200e6-178">They return `Task.FromResult(0)` when requirements are not met.</span></span> <span data-ttu-id="200e6-179">`Task.FromResult(0)`jest ani powodzenie lub niepowodzenie, umożliwia innych obsługi autoryzacji do uruchomienia.</span><span class="sxs-lookup"><span data-stu-id="200e6-179">`Task.FromResult(0)` is neither success or failure, it allows other authorization handler to run.</span></span> <span data-ttu-id="200e6-180">Jeśli musisz jawnie zakończyć się niepowodzeniem, zwróć `context.Fail()`.</span><span class="sxs-lookup"><span data-stu-id="200e6-180">If you need to explicitly fail, return `context.Fail()`.</span></span>

<span data-ttu-id="200e6-181">Zezwolimy kontaktu właścicieli edytowanie/usuwanie własnych danych, więc nie trzeba sprawdzić działanie przekazany parametr wymaganie.</span><span class="sxs-lookup"><span data-stu-id="200e6-181">We allow contact owners to edit/delete their own data, so we don't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="200e6-182">Tworzenie obsługi programu Menedżer autoryzacji</span><span class="sxs-lookup"><span data-stu-id="200e6-182">Create a manager authorization handler</span></span>

<span data-ttu-id="200e6-183">Utwórz `ContactManagerAuthorizationHandler` klasy w *autoryzacji* folderu.</span><span class="sxs-lookup"><span data-stu-id="200e6-183">Create a `ContactManagerAuthorizationHandler` class in the  *Authorization* folder.</span></span> <span data-ttu-id="200e6-184">`ContactManagerAuthorizationHandler` Potwierdzi użytkownika, działając w zasobie menedżera.</span><span class="sxs-lookup"><span data-stu-id="200e6-184">The `ContactManagerAuthorizationHandler` will verify the user acting on the resource is a manager.</span></span> <span data-ttu-id="200e6-185">Tylko menedżerowie można zatwierdzić lub odrzucić zmiany zawartości (nowe lub zmienione).</span><span class="sxs-lookup"><span data-stu-id="200e6-185">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="200e6-186">Utwórz program obsługi autoryzacji administratora</span><span class="sxs-lookup"><span data-stu-id="200e6-186">Create an administrator authorization handler</span></span>

<span data-ttu-id="200e6-187">Utwórz `ContactAdministratorsAuthorizationHandler` klasy w *autoryzacji* folderu.</span><span class="sxs-lookup"><span data-stu-id="200e6-187">Create a `ContactAdministratorsAuthorizationHandler` class in the  *Authorization* folder.</span></span> <span data-ttu-id="200e6-188">`ContactAdministratorsAuthorizationHandler` Zweryfikuje użytkownika na zasób ma uprawnienia administratora.</span><span class="sxs-lookup"><span data-stu-id="200e6-188">The `ContactAdministratorsAuthorizationHandler` will verify the user acting on the resource is a administrator.</span></span> <span data-ttu-id="200e6-189">Administrator może wykonać wszystkie operacje.</span><span class="sxs-lookup"><span data-stu-id="200e6-189">Administrator can do all operations.</span></span>

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="200e6-190">Rejestrowanie procedur obsługi autoryzacji</span><span class="sxs-lookup"><span data-stu-id="200e6-190">Register the authorization handlers</span></span>

<span data-ttu-id="200e6-191">Musi być zarejestrowany przy użyciu programu Entity Framework Core Services [iniekcji zależności](xref:fundamentals/dependency-injection) przy użyciu [AddScoped](/aspnet/core/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span><span class="sxs-lookup"><span data-stu-id="200e6-191">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/aspnet/core/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="200e6-192">`ContactIsOwnerAuthorizationHandler` Korzysta z platformy ASP.NET Core [tożsamości](xref:security/authentication/identity), który jest wbudowany w program Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="200e6-192">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="200e6-193">Zarejestruj obsługi z kolekcją usługi, więc będą one dostępne do `ContactsController` za pośrednictwem [iniekcji zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="200e6-193">Register the handlers with the service collection so they will be available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="200e6-194">Dodaj następujący kod na końcu `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="200e6-194">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=AuthorizationHandlers)]

<span data-ttu-id="200e6-195">`ContactAdministratorsAuthorizationHandler`i `ContactManagerAuthorizationHandler` są dodawane jako pojedynczych wystąpień.</span><span class="sxs-lookup"><span data-stu-id="200e6-195">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="200e6-196">Są one pojedynczych wystąpień, ponieważ nie używają EF i wszystkich informacji potrzebnych `Context` parametr `HandleRequirementAsync` metody.</span><span class="sxs-lookup"><span data-stu-id="200e6-196">They are singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

<span data-ttu-id="200e6-197">Pełną `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="200e6-197">The complete `ConfigureServices`:</span></span>

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=ConfigureServices)]

## <a name="update-the-code-to-support-authorization"></a><span data-ttu-id="200e6-198">Zaktualizuj kod do obsługi autoryzacji</span><span class="sxs-lookup"><span data-stu-id="200e6-198">Update the code to support authorization</span></span>

<span data-ttu-id="200e6-199">W tej sekcji kontrolera i widoki aktualizacji i Dodaj klasę wymagań operacji.</span><span class="sxs-lookup"><span data-stu-id="200e6-199">In this section, you update the controller and views and add an operations requirements class.</span></span>

### <a name="update-the-contacts-controller"></a><span data-ttu-id="200e6-200">Aktualizacja kontrolera kontaktów</span><span class="sxs-lookup"><span data-stu-id="200e6-200">Update the Contacts controller</span></span>

<span data-ttu-id="200e6-201">Aktualizacja `ContactsController` konstruktora:</span><span class="sxs-lookup"><span data-stu-id="200e6-201">Update the `ContactsController` constructor:</span></span>

* <span data-ttu-id="200e6-202">Dodaj `IAuthorizationService` usługi z dostępem do obsługi autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="200e6-202">Add the `IAuthorizationService` service to  access to the authorization handlers.</span></span> 
* <span data-ttu-id="200e6-203">Dodaj `Identity` `UserManager` usługi:</span><span class="sxs-lookup"><span data-stu-id="200e6-203">Add the `Identity` `UserManager` service:</span></span>

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_ContactsControllerCtor)]

### <a name="add-a-contact-operations-requirements-class"></a><span data-ttu-id="200e6-204">Dodaj klasę wymagań operacji kontaktów</span><span class="sxs-lookup"><span data-stu-id="200e6-204">Add a contact operations requirements class</span></span>

<span data-ttu-id="200e6-205">Dodaj `ContactOperations` klasy do *autoryzacji* folderu.</span><span class="sxs-lookup"><span data-stu-id="200e6-205">Add the `ContactOperations` class to the *Authorization* folder.</span></span> <span data-ttu-id="200e6-206">Ta klasa zawiera wymagania naszego obsługuje aplikacja:</span><span class="sxs-lookup"><span data-stu-id="200e6-206">This class  contain the requirements our app supports:</span></span>

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactOperations.cs)]

### <a name="update-create"></a><span data-ttu-id="200e6-207">Utwórz aktualizacji</span><span class="sxs-lookup"><span data-stu-id="200e6-207">Update Create</span></span>

<span data-ttu-id="200e6-208">Aktualizacja `HTTP POST Create` metodę:</span><span class="sxs-lookup"><span data-stu-id="200e6-208">Update the `HTTP POST Create` method to:</span></span>

* <span data-ttu-id="200e6-209">Identyfikator użytkownika, aby dodać `Contact` modelu.</span><span class="sxs-lookup"><span data-stu-id="200e6-209">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="200e6-210">Wywołuje program obsługi autoryzacji, aby sprawdzić, czy użytkownik jest właścicielem kontaktu.</span><span class="sxs-lookup"><span data-stu-id="200e6-210">Call the authorization handler to verify the user owns the contact.</span></span>

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Create)]

### <a name="update-edit"></a><span data-ttu-id="200e6-211">Edytowanie aktualizacji</span><span class="sxs-lookup"><span data-stu-id="200e6-211">Update Edit</span></span>

<span data-ttu-id="200e6-212">Zaktualizuj zarówno `Edit` metody na potrzeby obsługi autoryzacji weryfikacji użytkownika należy kontakt.</span><span class="sxs-lookup"><span data-stu-id="200e6-212">Update both `Edit` methods to use the authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="200e6-213">Ponieważ firma Microsoft działają autoryzacji zasobów nie można użyć `[Authorize]` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="200e6-213">Because we are performing resource authorization we cannot use the `[Authorize]` attribute.</span></span> <span data-ttu-id="200e6-214">Gdy atrybuty są oceniane nie mamy dostęp do zasobu.</span><span class="sxs-lookup"><span data-stu-id="200e6-214">We don't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="200e6-215">Autoryzacji zasobów na podstawie musi być konieczne.</span><span class="sxs-lookup"><span data-stu-id="200e6-215">Resource based authorization must be imperative.</span></span> <span data-ttu-id="200e6-216">Kontroli należy wykonać, gdy będziemy mieć dostęp do zasobu, przez załadowanie go w kontrolera lub przez załadowanie go w obrębie samego program obsługi.</span><span class="sxs-lookup"><span data-stu-id="200e6-216">Checks must be performed once we have access to the resource, either by loading it in our controller, or by loading it within the handler itself.</span></span> <span data-ttu-id="200e6-217">Często będzie dostęp do zasobu, przekazując w kluczu zasobów.</span><span class="sxs-lookup"><span data-stu-id="200e6-217">Frequently you will access the resource by passing in the resource key.</span></span>

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Edit)]

### <a name="update-the-delete-method"></a><span data-ttu-id="200e6-218">Metoda usuwania aktualizacji</span><span class="sxs-lookup"><span data-stu-id="200e6-218">Update the Delete method</span></span>

<span data-ttu-id="200e6-219">Zaktualizuj zarówno `Delete` metody na potrzeby obsługi autoryzacji weryfikacji użytkownika należy kontakt.</span><span class="sxs-lookup"><span data-stu-id="200e6-219">Update both `Delete` methods to use the authorization handler to verify the user owns the contact.</span></span>

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Delete)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="200e6-220">Wstaw usługi autoryzacji do widoków</span><span class="sxs-lookup"><span data-stu-id="200e6-220">Inject the authorization service into the views</span></span>

<span data-ttu-id="200e6-221">Obecnie pokazuje interfejsu użytkownika edytowania i usuwania łącza do danych użytkownika nie można zmodyfikować.</span><span class="sxs-lookup"><span data-stu-id="200e6-221">Currently the UI shows edit and delete links for data the user cannot modify.</span></span> <span data-ttu-id="200e6-222">Firma Microsoft będzie rozwiązać ten problem, stosując obsługi autoryzacji do widoków.</span><span class="sxs-lookup"><span data-stu-id="200e6-222">We'll fix that by applying the authorization handler to the views.</span></span>

<span data-ttu-id="200e6-223">Wstaw usługi autoryzacji w *Views/_ViewImports.cshtml* plik, będzie on dostępny do wszystkich widoków:</span><span class="sxs-lookup"><span data-stu-id="200e6-223">Inject the authorization service in the *Views/_ViewImports.cshtml* file so it will be available to all views:</span></span>

[!code-html[Main](secure-data/samples/final/Views/_ViewImports.cshtml)]

<span data-ttu-id="200e6-224">Aktualizacja *Views/Contacts/Index.cshtml* widoku Razor tylko wyświetlanie, edytowanie i usuwanie łączy dla użytkowników, którzy mogą edytowanie/usuwanie kontaktu.</span><span class="sxs-lookup"><span data-stu-id="200e6-224">Update the *Views/Contacts/Index.cshtml* Razor view to only display the edit and delete links for users who can edit/delete the contact.</span></span>

<span data-ttu-id="200e6-225">Dodaj`@using ContactManager.Authorization;`</span><span class="sxs-lookup"><span data-stu-id="200e6-225">Add `@using ContactManager.Authorization;`</span></span>

<span data-ttu-id="200e6-226">Aktualizacja `Edit` i `Delete` łączy, aby były wyświetlane tylko dla użytkowników z uprawnieniem do edytowania i usuwania kontaktu.</span><span class="sxs-lookup"><span data-stu-id="200e6-226">Update the `Edit` and `Delete` links so they are only rendered for users with permission to edit and delete the contact.</span></span>

[!code-html[Main](secure-data/samples/final/Views/Contacts/Index.cshtml?range=63-84)]

<span data-ttu-id="200e6-227">Ostrzeżenie: Ukrywanie łącza z użytkowników, którzy nie mają uprawnień do edytowania ani usuwania danych nie zabezpieczyć aplikację.</span><span class="sxs-lookup"><span data-stu-id="200e6-227">Warning: Hiding links from users that do not have permission to edit or delete data does not secure the app.</span></span> <span data-ttu-id="200e6-228">Ukrywanie łączy powoduje, że aplikacja więcej użytkowników przyjazną przez wyświetlanie łączy jedyne prawidłowe.</span><span class="sxs-lookup"><span data-stu-id="200e6-228">Hiding links makes the app more user friendly by displaying only valid links.</span></span> <span data-ttu-id="200e6-229">Użytkownicy mogą hack wygenerowanego adresy URL do wywołania edytowania i usuwania operacji na danych, które nie są właścicielami.</span><span class="sxs-lookup"><span data-stu-id="200e6-229">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span>  <span data-ttu-id="200e6-230">Kontroler należy powtórzyć kontroli dostępu do zabezpieczenia.</span><span class="sxs-lookup"><span data-stu-id="200e6-230">The controller must repeat the access checks to be secure.</span></span>

### <a name="update-the-details-view"></a><span data-ttu-id="200e6-231">Aktualizacja w widoku szczegółów</span><span class="sxs-lookup"><span data-stu-id="200e6-231">Update the Details view</span></span>

<span data-ttu-id="200e6-232">Aktualizacja w widoku szczegółów, menedżerów można zatwierdzić lub odrzucić kontaktów:</span><span class="sxs-lookup"><span data-stu-id="200e6-232">Update the details view so managers can approve or reject contacts:</span></span>

[!code-html[Main](secure-data/samples/final/Views/Contacts/Details.cshtml?range=53-)]

## <a name="test-the-completed-app"></a><span data-ttu-id="200e6-233">Testowanie ukończonej aplikacji</span><span class="sxs-lookup"><span data-stu-id="200e6-233">Test the completed app</span></span>

<span data-ttu-id="200e6-234">Jeśli przy użyciu programu Visual Studio Code lub testowania na platformie lokalnego, który nie zawiera certyfikatu testowego dla protokołu SSL:</span><span class="sxs-lookup"><span data-stu-id="200e6-234">If you are using Visual Studio Code or testing on local platform that doesn't include a test certificate for SSL:</span></span>

- <span data-ttu-id="200e6-235">Ustaw `"LocalTest:skipSSL": true` w *appsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="200e6-235">Set `"LocalTest:skipSSL": true` in the *appsettings.json* file.</span></span>

<span data-ttu-id="200e6-236">Uruchomiono aplikację i mieć kontaktów należy usunąć wszystkie rekordy w `Contact` tabeli i ponownie uruchom aplikację w celu umieszczenia bazy danych.</span><span class="sxs-lookup"><span data-stu-id="200e6-236">If you have run the app and have contacts, delete all the records in the `Contact` table and restart the app to seed the database.</span></span> <span data-ttu-id="200e6-237">Jeśli używasz programu Visual Studio, musisz zamknąć i ponownie uruchomić usługi IIS Express do inicjatora bazy danych.</span><span class="sxs-lookup"><span data-stu-id="200e6-237">If you are using Visual Studio, you need to exit and restart IIS Express to seed the database.</span></span>

<span data-ttu-id="200e6-238">Zarejestruj użytkownikowi przeglądanie kontaktów.</span><span class="sxs-lookup"><span data-stu-id="200e6-238">Register a user to browse the contacts.</span></span>

<span data-ttu-id="200e6-239">Łatwe testowanie ukończonej aplikacji jest uruchamianie trzech różnych przeglądarkach (lub incognito/sesję InPrivate wersji).</span><span class="sxs-lookup"><span data-stu-id="200e6-239">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate versions).</span></span> <span data-ttu-id="200e6-240">W przeglądarce jednego zarejestrować nowego użytkownika, na przykład `test@example.com`.</span><span class="sxs-lookup"><span data-stu-id="200e6-240">In one browser, register a new user, for example, `test@example.com`.</span></span> <span data-ttu-id="200e6-241">Zaloguj się do każdej przeglądarki z innym użytkownikiem.</span><span class="sxs-lookup"><span data-stu-id="200e6-241">Sign in to each browser with a different user.</span></span> <span data-ttu-id="200e6-242">Sprawdź następujące informacje:</span><span class="sxs-lookup"><span data-stu-id="200e6-242">Verify the following:</span></span>

* <span data-ttu-id="200e6-243">Zarejestrowani użytkownicy mogą wyświetlać wszystkie zatwierdzone dane kontaktu.</span><span class="sxs-lookup"><span data-stu-id="200e6-243">Registered users can view all the approved contact data.</span></span>
* <span data-ttu-id="200e6-244">Zarejestrowani użytkownicy mogą edytowanie/usuwanie własnych danych.</span><span class="sxs-lookup"><span data-stu-id="200e6-244">Registered users can edit/delete their own data.</span></span> 
* <span data-ttu-id="200e6-245">Menedżerowie można zatwierdzić lub odrzucić dane kontaktowe.</span><span class="sxs-lookup"><span data-stu-id="200e6-245">Managers can approve or reject contact data.</span></span> <span data-ttu-id="200e6-246">`Details` Wyświetlić pokazuje **Zatwierdź** i **Odrzuć** przycisków.</span><span class="sxs-lookup"><span data-stu-id="200e6-246">The `Details` view shows **Approve** and **Reject** buttons.</span></span> 
* <span data-ttu-id="200e6-247">Administratorzy mogą zatwierdzać Odrzuć i edytowanie/usuwanie żadnych danych.</span><span class="sxs-lookup"><span data-stu-id="200e6-247">Administrators can approve/reject and edit/delete any data.</span></span>

| <span data-ttu-id="200e6-248">Użytkownik</span><span class="sxs-lookup"><span data-stu-id="200e6-248">User</span></span>| <span data-ttu-id="200e6-249">Opcje</span><span class="sxs-lookup"><span data-stu-id="200e6-249">Options</span></span> |
| ------------ | ---------|
| test@example.com | <span data-ttu-id="200e6-250">Można edytowanie/usuwanie własnych danych</span><span class="sxs-lookup"><span data-stu-id="200e6-250">Can edit/delete own data</span></span> |
| manager@contoso.com | <span data-ttu-id="200e6-251">Można zatwierdzić Odrzuć i edytowanie i usuwanie własnych danych</span><span class="sxs-lookup"><span data-stu-id="200e6-251">Can approve/reject and edit/delete own data</span></span>  |
| admin@contoso.com | <span data-ttu-id="200e6-252">Edytowanie i usuwanie można i Zatwierdź/Odrzuć wszystkie dane</span><span class="sxs-lookup"><span data-stu-id="200e6-252">Can edit/delete and approve/reject all data</span></span>|

<span data-ttu-id="200e6-253">Utworzenie kontaktu w przeglądarce Administratorzy.</span><span class="sxs-lookup"><span data-stu-id="200e6-253">Create a contact in the administrators browser.</span></span> <span data-ttu-id="200e6-254">Skopiuj adres URL do usunięcia i Edytuj z skontaktowanie się z administratorem.</span><span class="sxs-lookup"><span data-stu-id="200e6-254">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="200e6-255">Wklej te linki do przeglądarki użytkownika testowego, aby sprawdzić, czy użytkownik testu nie może wykonywać te operacje.</span><span class="sxs-lookup"><span data-stu-id="200e6-255">Paste these links into the test user's browser to verify the test user cannot perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="200e6-256">Utworzyć początkową aplikację</span><span class="sxs-lookup"><span data-stu-id="200e6-256">Create the starter app</span></span>

<span data-ttu-id="200e6-257">Wykonaj te instrukcje, aby utworzyć początkową aplikację.</span><span class="sxs-lookup"><span data-stu-id="200e6-257">Follow these instructions to create the starter app.</span></span>

* <span data-ttu-id="200e6-258">Utwórz **aplikacji sieci Web platformy ASP.NET Core** przy użyciu [programu Visual Studio 2017](https://www.visualstudio.com/) o nazwie "ContactManager"</span><span class="sxs-lookup"><span data-stu-id="200e6-258">Create an **ASP.NET Core Web Application** using [Visual Studio 2017](https://www.visualstudio.com/) named "ContactManager"</span></span>

  * <span data-ttu-id="200e6-259">Tworzenie aplikacji z **indywidualne konta użytkowników**.</span><span class="sxs-lookup"><span data-stu-id="200e6-259">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="200e6-260">Nadaj mu nazwę "ContactManager", przestrzeń nazw będzie odpowiadała Użyj przestrzeni nazw w próbce.</span><span class="sxs-lookup"><span data-stu-id="200e6-260">Name it "ContactManager" so your namespace will match the namespace use in the sample.</span></span>

* <span data-ttu-id="200e6-261">Dodaj następujące `Contact` modelu:</span><span class="sxs-lookup"><span data-stu-id="200e6-261">Add the following `Contact` model:</span></span>

  [!code-csharp[Main](secure-data/samples/starter/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="200e6-262">Szkieletu `Contact` modelu przy użyciu programu Entity Framework Core i `ApplicationDbContext` kontekstu danych.</span><span class="sxs-lookup"><span data-stu-id="200e6-262">Scaffold the `Contact` model using Entity Framework Core and the `ApplicationDbContext` data context.</span></span> <span data-ttu-id="200e6-263">Zaakceptuj wszystkie ustawienia domyślne szkieletów.</span><span class="sxs-lookup"><span data-stu-id="200e6-263">Accept all the scaffolding defaults.</span></span> <span data-ttu-id="200e6-264">Przy użyciu `ApplicationDbContext` dla kontekstu danych klasy umieszcza tabeli kontaktu [tożsamości](xref:security/authentication/identity) bazy danych.</span><span class="sxs-lookup"><span data-stu-id="200e6-264">Using `ApplicationDbContext` for the data context class  puts the contact table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="200e6-265">Zobacz [Dodawanie modelu](xref:tutorials/first-mvc-app/adding-model) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="200e6-265">See [Adding a model](xref:tutorials/first-mvc-app/adding-model) for more information.</span></span>

* <span data-ttu-id="200e6-266">Aktualizacja **ContactManager** zakotwiczenia w *Views/Shared/_Layout.cshtml* plik z `asp-controller="Home"` do `asp-controller="Contacts"` tak naciśnięcie **ContactManager** łącza wywoła kontrolera kontaktów.</span><span class="sxs-lookup"><span data-stu-id="200e6-266">Update the **ContactManager** anchor in the *Views/Shared/_Layout.cshtml* file from `asp-controller="Home"` to `asp-controller="Contacts"` so tapping the **ContactManager** link will invoke the Contacts controller.</span></span> <span data-ttu-id="200e6-267">Oryginalny kod znaczników:</span><span class="sxs-lookup"><span data-stu-id="200e6-267">The original markup:</span></span>

```html
   <a asp-area="" asp-controller="Home" asp-action="Index" class="navbar-brand">ContactManager</a>
   ```

<span data-ttu-id="200e6-268">Zaktualizowany kod znaczników:</span><span class="sxs-lookup"><span data-stu-id="200e6-268">The updated markup:</span></span>

```html
   <a asp-area="" asp-controller="Contacts" asp-action="Index" class="navbar-brand">ContactManager</a>
   ```

* <span data-ttu-id="200e6-269">Tworzenie szkieletu początkowej migracji i aktualizacji bazy danych</span><span class="sxs-lookup"><span data-stu-id="200e6-269">Scaffold the initial migration and update the database</span></span>

```none
   dotnet ef migrations add initial
   dotnet ef database update
   ```

* <span data-ttu-id="200e6-270">Testowanie aplikacji przez tworzenie, edytowanie i usuwanie kontaktu</span><span class="sxs-lookup"><span data-stu-id="200e6-270">Test the app by creating, editing and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="200e6-271">Inicjatora bazy danych</span><span class="sxs-lookup"><span data-stu-id="200e6-271">Seed the database</span></span>

<span data-ttu-id="200e6-272">Dodaj `SeedData` klasy do *danych* folderu.</span><span class="sxs-lookup"><span data-stu-id="200e6-272">Add the `SeedData` class to the *Data* folder.</span></span> <span data-ttu-id="200e6-273">Jeśli pobrano próbki, możesz skopiować *SeedData.cs* pliku *danych* folderu projektu początkowego.</span><span class="sxs-lookup"><span data-stu-id="200e6-273">If you've downloaded the sample, you can copy the *SeedData.cs* file to the *Data* folder of the starter project.</span></span>

[!code-csharp[Main](secure-data/samples/starter/Data/SeedData.cs)]

<span data-ttu-id="200e6-274">Dodaj wyróżniony kod na końcu `Configure` metody w *Startup.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="200e6-274">Add the highlighted code to the end of the `Configure` method in the *Startup.cs* file:</span></span>

[!code-csharp[Main](secure-data/samples/starter/Startup.cs?name=Configure&highlight=28-)]

<span data-ttu-id="200e6-275">Testowanie, czy aplikacja rozpoczęta bazy danych.</span><span class="sxs-lookup"><span data-stu-id="200e6-275">Test that the app seeded the database.</span></span> <span data-ttu-id="200e6-276">Seed — metoda nie działa, jeśli istnieją wszystkie wiersze w kontakcie bazy danych.</span><span class="sxs-lookup"><span data-stu-id="200e6-276">The seed method does not run if there are any rows in the contact DB.</span></span>

### <a name="create-a-class-used-in-the-tutorial"></a><span data-ttu-id="200e6-277">Tworzenie klasy używane w samouczku</span><span class="sxs-lookup"><span data-stu-id="200e6-277">Create a class used in the tutorial</span></span>

* <span data-ttu-id="200e6-278">Utwórz folder o nazwie *autoryzacji*.</span><span class="sxs-lookup"><span data-stu-id="200e6-278">Create a folder named *Authorization*.</span></span>
* <span data-ttu-id="200e6-279">Kopiuj *Authorization\ContactOperations.cs* plik Pobieranie ukończone projektu lub skopiuj następujący kod:</span><span class="sxs-lookup"><span data-stu-id="200e6-279">Copy the *Authorization\ContactOperations.cs* file from the completed project download, or copy the following code:</span></span>

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactOperations.cs)]

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a><span data-ttu-id="200e6-280">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="200e6-280">Additional resources</span></span>

* <span data-ttu-id="200e6-281">[Laboratorium autoryzacji platformy ASP.NET Core](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span><span class="sxs-lookup"><span data-stu-id="200e6-281">[ASP.NET Core Authorization Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span></span> <span data-ttu-id="200e6-282">W tym laboratorium przechodzi w stan więcej szczegółów na temat funkcji zabezpieczeń wprowadzone w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="200e6-282">This lab goes into more detail on the security features introduced in this tutorial.</span></span>
* [<span data-ttu-id="200e6-283">Autoryzacja w ASP.NET Core: prosty, roli, opartej na oświadczeniach i niestandardowych</span><span class="sxs-lookup"><span data-stu-id="200e6-283">Authorization in ASP.NET Core : Simple, role, claims-based and custom</span></span>](index.md)
* [<span data-ttu-id="200e6-284">Niestandardowe autoryzacji opartych na zasadach</span><span class="sxs-lookup"><span data-stu-id="200e6-284">Custom Policy-Based Authorization</span></span>](policies.md)
