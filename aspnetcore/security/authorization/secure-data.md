---
title: "Tworzenie aplikacji platformy ASP.NET Core z danych użytkownika chronione przez autoryzacji"
author: rick-anderson
description: "Dowiedz się, jak utworzyć aplikację Razor strony z danymi użytkownika chronione przez autoryzacji. Obejmuje SSL, uwierzytelniania, zabezpieczeń, ASP.NET Core Identity."
manager: wpickett
ms.author: riande
ms.date: 01/24/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/secure-data
ms.openlocfilehash: 944886a7d55af8966dc51424d16bec5ff58dbc05
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2018
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a><span data-ttu-id="464fe-104">Tworzenie aplikacji platformy ASP.NET Core z danych użytkownika chronione przez autoryzacji</span><span class="sxs-lookup"><span data-stu-id="464fe-104">Create an ASP.NET Core app with user data protected by authorization</span></span>

<span data-ttu-id="464fe-105">Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Audette Jan](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="464fe-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="464fe-106">W tym samouczku przedstawiono sposób tworzenia aplikacji sieci web platformy ASP.NET Core z danymi użytkownika chronione przez autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="464fe-106">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="464fe-107">Wyświetla listę kontaktów, które uwierzytelnionych użytkowników (zarejestrowanym) został utworzony.</span><span class="sxs-lookup"><span data-stu-id="464fe-107">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="464fe-108">Istnieją trzy grupy zabezpieczeń:</span><span class="sxs-lookup"><span data-stu-id="464fe-108">There are three security groups:</span></span>

* <span data-ttu-id="464fe-109">**Użytkownicy zarejestrowanych** może wyświetlać wszystkie zatwierdzone dane i może edytowanie/usuwanie własnych danych.</span><span class="sxs-lookup"><span data-stu-id="464fe-109">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="464fe-110">**Menedżerowie** można zatwierdzić lub odrzucić dane kontaktowe.</span><span class="sxs-lookup"><span data-stu-id="464fe-110">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="464fe-111">Tylko zatwierdzone kontakty są widoczne dla użytkowników.</span><span class="sxs-lookup"><span data-stu-id="464fe-111">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="464fe-112">**Administratorzy** można zatwierdzić Odrzuć i edytowanie/usuwanie żadnych danych.</span><span class="sxs-lookup"><span data-stu-id="464fe-112">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="464fe-113">Na poniższej ilustracji użytkownik Rick (`rick@example.com`) jest zalogowany.</span><span class="sxs-lookup"><span data-stu-id="464fe-113">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="464fe-114">Rick mogą jedynie wyświetlać zatwierdzonych kontaktów i **Edytuj**/**usunąć**/**Utwórz nowy** łącza do swoich kontaktów.</span><span class="sxs-lookup"><span data-stu-id="464fe-114">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="464fe-115">Tylko ostatni rekord powstały Rick, wyświetla **Edytuj** i **usunąć** łącza.</span><span class="sxs-lookup"><span data-stu-id="464fe-115">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="464fe-116">Inni użytkownicy zobaczą ostatniego rekordu do momentu menedżerowi lub administratorowi zmienia stan na "Zatwierdzone".</span><span class="sxs-lookup"><span data-stu-id="464fe-116">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![Obraz opisane poprzedzających](secure-data/_static/rick.png)

<span data-ttu-id="464fe-118">Na poniższej ilustracji `manager@contoso.com` jest zarejestrowany i rolę menedżerów:</span><span class="sxs-lookup"><span data-stu-id="464fe-118">In the following image, `manager@contoso.com` is signed in and in the managers role:</span></span>

![Obraz opisane poprzedzających](secure-data/_static/manager1.png)

<span data-ttu-id="464fe-120">Na poniższej ilustracji przedstawiono menedżerów widoku Szczegóły kontaktu:</span><span class="sxs-lookup"><span data-stu-id="464fe-120">The following image shows the managers details view of a contact:</span></span>

![Obraz opisane poprzedzających](secure-data/_static/manager.png)

<span data-ttu-id="464fe-122">**Zatwierdź** i **Odrzuć** przyciski są wyświetlane tylko dla menedżerów i administratorów.</span><span class="sxs-lookup"><span data-stu-id="464fe-122">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="464fe-123">Na poniższej ilustracji `admin@contoso.com` jest zarejestrowany i w roli administratora:</span><span class="sxs-lookup"><span data-stu-id="464fe-123">In the following image, `admin@contoso.com` is signed in and in the administrators role:</span></span>

![Obraz opisane poprzedzających](secure-data/_static/admin.png)

<span data-ttu-id="464fe-125">Administrator ma wszystkie uprawnienia.</span><span class="sxs-lookup"><span data-stu-id="464fe-125">The administrator has all privileges.</span></span> <span data-ttu-id="464fe-126">Użytkownik może odczytu/edytowanie/usuwanie żadnych skontaktuj się z i Zmień stan kontaktów.</span><span class="sxs-lookup"><span data-stu-id="464fe-126">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="464fe-127">Aplikacja została utworzona przez [szkieletów](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) następujące `Contact` modelu:</span><span class="sxs-lookup"><span data-stu-id="464fe-127">The app was created by [scaffolding](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) the following `Contact` model:</span></span>

[!code-csharp[Main](secure-data/samples/starter2/Models/Contact.cs?name=snippet1)]

<span data-ttu-id="464fe-128">Próbka zawiera poniższe obsługi autoryzacji:</span><span class="sxs-lookup"><span data-stu-id="464fe-128">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="464fe-129">`ContactIsOwnerAuthorizationHandler`: Zapewnia, że użytkownika można edytować tylko ich danych.</span><span class="sxs-lookup"><span data-stu-id="464fe-129">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="464fe-130">`ContactManagerAuthorizationHandler`: Umożliwia menedżerów zatwierdzić lub odrzucić kontaktów.</span><span class="sxs-lookup"><span data-stu-id="464fe-130">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="464fe-131">`ContactAdministratorsAuthorizationHandler`: Pozwala administratorom, aby zatwierdzić lub odrzucić kontaktów i edytowanie/usuwanie kontaktów.</span><span class="sxs-lookup"><span data-stu-id="464fe-131">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="464fe-132">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="464fe-132">Prerequisites</span></span>

<span data-ttu-id="464fe-133">W tym samouczku jest zaawansowane.</span><span class="sxs-lookup"><span data-stu-id="464fe-133">This tutorial is advanced.</span></span> <span data-ttu-id="464fe-134">Należy zapoznać się z:</span><span class="sxs-lookup"><span data-stu-id="464fe-134">You should be familiar with:</span></span>

* [<span data-ttu-id="464fe-135">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="464fe-135">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="464fe-136">Uwierzytelnianie</span><span class="sxs-lookup"><span data-stu-id="464fe-136">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="464fe-137">Potwierdzenie konta i odzyskiwanie hasła</span><span class="sxs-lookup"><span data-stu-id="464fe-137">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="464fe-138">Autoryzacja</span><span class="sxs-lookup"><span data-stu-id="464fe-138">Authorization</span></span>](xref:security/authorization/index)
* [<span data-ttu-id="464fe-139">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="464fe-139">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<span data-ttu-id="464fe-140">Wersja platformy ASP.NET Core 1.1 tego samouczka jest [to](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) folderu.</span><span class="sxs-lookup"><span data-stu-id="464fe-140">The ASP.NET Core 1.1 version of this tutorial is in [this](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) folder.</span></span> <span data-ttu-id="464fe-141">1.1, na przykład ASP.NET Core jest w [przykłady](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span><span class="sxs-lookup"><span data-stu-id="464fe-141">The 1.1 ASP.NET Core sample is in the [samples](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span></span>

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="464fe-142">Starter i ukończonej aplikacji</span><span class="sxs-lookup"><span data-stu-id="464fe-142">The starter and completed app</span></span>

<span data-ttu-id="464fe-143">[Pobierz](xref:tutorials/index#how-to-download-a-sample) [ukończone](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2) aplikacji.</span><span class="sxs-lookup"><span data-stu-id="464fe-143">[Download](xref:tutorials/index#how-to-download-a-sample) the [completed](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2) app.</span></span> <span data-ttu-id="464fe-144">[Test](#test-the-completed-app) ukończonej aplikacji, należy zapoznać się z jego funkcje zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="464fe-144">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="464fe-145">Początkową aplikację</span><span class="sxs-lookup"><span data-stu-id="464fe-145">The starter app</span></span>

<span data-ttu-id="464fe-146">[Pobierz](xref:tutorials/index#how-to-download-a-sample) [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2) aplikacji.</span><span class="sxs-lookup"><span data-stu-id="464fe-146">[Download](xref:tutorials/index#how-to-download-a-sample) the [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2) app.</span></span>

<span data-ttu-id="464fe-147">Uruchom aplikację, wybierz **ContactManager** link i sprawdź, tworzenie, edytowanie i usuwanie kontaktu.</span><span class="sxs-lookup"><span data-stu-id="464fe-147">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="464fe-148">Zabezpieczanie danych użytkownika</span><span class="sxs-lookup"><span data-stu-id="464fe-148">Secure user data</span></span>

<span data-ttu-id="464fe-149">Następujące sekcje zostały wszystkie główne kroki umożliwiające utworzenie aplikacji danych bezpiecznego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="464fe-149">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="464fe-150">Może być przydatne do odwoływania się do projektu zakończone.</span><span class="sxs-lookup"><span data-stu-id="464fe-150">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="464fe-151">Powiązanie dane kontaktowe dla użytkownika</span><span class="sxs-lookup"><span data-stu-id="464fe-151">Tie the contact data to the user</span></span>

<span data-ttu-id="464fe-152">Za pomocą programu ASP.NET [tożsamości](xref:security/authentication/identity) identyfikator użytkownika, aby zapewnić użytkownikom można edytować swoje dane, ale nie do innych danych użytkowników.</span><span class="sxs-lookup"><span data-stu-id="464fe-152">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="464fe-153">Dodaj `OwnerID` i `ContactStatus` do `Contact` modelu:</span><span class="sxs-lookup"><span data-stu-id="464fe-153">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Models/Contact.cs?name=snippet1&highlight=5-6,16-)]

<span data-ttu-id="464fe-154">`OwnerID`Identyfikator użytkownika z `AspNetUser` tabeli w [tożsamości](xref:security/authentication/identity) bazy danych.</span><span class="sxs-lookup"><span data-stu-id="464fe-154">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="464fe-155">`Status` Pole określa, czy kontakt jest widoczny dla zwykłych użytkowników.</span><span class="sxs-lookup"><span data-stu-id="464fe-155">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="464fe-156">Tworzenie nowych migracji i aktualizacji bazy danych:</span><span class="sxs-lookup"><span data-stu-id="464fe-156">Create a new migration and update the database:</span></span>

```console
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="require-ssl-and-authenticated-users"></a><span data-ttu-id="464fe-157">Wymagaj protokołu SSL i uwierzytelnionych użytkowników</span><span class="sxs-lookup"><span data-stu-id="464fe-157">Require SSL and authenticated users</span></span>

<span data-ttu-id="464fe-158">Dodaj [IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) do `Startup`:</span><span class="sxs-lookup"><span data-stu-id="464fe-158">Add [IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) to `Startup`:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Startup.cs?name=snippet_env)]

<span data-ttu-id="464fe-159">W `ConfigureServices` metody *Startup.cs* plików, dodawanie [RequireHttpsAttribute](/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) filtr autoryzacji:</span><span class="sxs-lookup"><span data-stu-id="464fe-159">In the `ConfigureServices` method of the *Startup.cs* file, add the [RequireHttpsAttribute](/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) authorization filter:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Startup.cs?name=snippet_SSL&highlight=19-)]

<span data-ttu-id="464fe-160">Jeśli używasz programu Visual Studio, Włącz protokół SSL.</span><span class="sxs-lookup"><span data-stu-id="464fe-160">If you're using Visual Studio, enable SSL.</span></span>

<span data-ttu-id="464fe-161">Przekierowywanie żądań HTTP, HTTPS, zobacz [ponowne zapisywanie adresów URL w oprogramowaniu pośredniczącym](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="464fe-161">To redirect HTTP requests to HTTPS, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="464fe-162">Za pomocą programu Visual Studio Code lub testowania na platformie lokalnego, który nie zawiera certyfikatu testowego dla protokołu SSL:</span><span class="sxs-lookup"><span data-stu-id="464fe-162">If you're using Visual Studio Code or testing on a local platform that doesn't include a test certificate for SSL:</span></span>

  <span data-ttu-id="464fe-163">Ustaw `"LocalTest:skipSSL": true` w *appsettings. Developement.JSON* pliku.</span><span class="sxs-lookup"><span data-stu-id="464fe-163">Set `"LocalTest:skipSSL": true` in the *appsettings.Developement.json* file.</span></span>

### <a name="require-authenticated-users"></a><span data-ttu-id="464fe-164">Wymaga uwierzytelnionych użytkowników</span><span class="sxs-lookup"><span data-stu-id="464fe-164">Require authenticated users</span></span>

<span data-ttu-id="464fe-165">Ustaw domyślne zasady uwierzytelniania, aby wymagać od użytkowników uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="464fe-165">Set the default authentication policy to require users to be authenticated.</span></span> <span data-ttu-id="464fe-166">Można zrezygnować z uwierzytelniania na poziomie metody Razor strony, kontrolera lub akcji z `[AllowAnonymous]` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="464fe-166">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="464fe-167">Ustawienie domyślne zasady uwierzytelniania, aby wymagać od użytkowników uwierzytelniania chroni nowo dodanego stron Razor i kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="464fe-167">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="464fe-168">Posiadanie uwierzytelniania domyślnie wymagane jest bezpieczniejszy niż polegania na nowe kontrolery i stron Razor do uwzględnienia `[Authorize]` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="464fe-168">Having authentication required by default is safer than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span> <span data-ttu-id="464fe-169">Dodaj następujący kod do `ConfigureServices` metody *Startup.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="464fe-169">Add the following to the `ConfigureServices` method of the *Startup.cs* file:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Startup.cs?name=snippet_defaultPolicy&highlight=31-)]

<span data-ttu-id="464fe-170">Dodaj [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) do indeksu strony o i skontaktuj się z pomocą tak anonimowych użytkowników można uzyskać informacji o lokacji, przed ich zarejestrować.</span><span class="sxs-lookup"><span data-stu-id="464fe-170">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index, About, and Contact pages so anonymous users can get information about the site before they register.</span></span> 

[!code-csharp[Main](secure-data/samples/final2/Pages/Index.cshtml.cs?name=snippet&highlight=2)]

<span data-ttu-id="464fe-171">Dodaj `[AllowAnonymous]` do [LoginModel i RegisterModel](https://github.com/aspnet/templating/issues/238).</span><span class="sxs-lookup"><span data-stu-id="464fe-171">Add `[AllowAnonymous]` to the [LoginModel and RegisterModel](https://github.com/aspnet/templating/issues/238).</span></span>

### <a name="configure-the-test-account"></a><span data-ttu-id="464fe-172">Skonfiguruj konto testu</span><span class="sxs-lookup"><span data-stu-id="464fe-172">Configure the test account</span></span>

<span data-ttu-id="464fe-173">`SeedData` Klasy tworzy dwa konta: administrator i Menedżer.</span><span class="sxs-lookup"><span data-stu-id="464fe-173">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="464fe-174">Użyj [narzędzie Menedżer klucz tajny](xref:security/app-secrets) ustawienie hasła dla tych kont.</span><span class="sxs-lookup"><span data-stu-id="464fe-174">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="464fe-175">Ustawianie hasła z katalogu projektu (katalog zawierający *Program.cs*):</span><span class="sxs-lookup"><span data-stu-id="464fe-175">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```console
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="464fe-176">Aktualizacja `Main` hasła testu:</span><span class="sxs-lookup"><span data-stu-id="464fe-176">Update `Main` to use the test password:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="464fe-177">Tworzenie konta testowe i aktualizowanie kontaktów</span><span class="sxs-lookup"><span data-stu-id="464fe-177">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="464fe-178">Aktualizacja `Initialize` metoda `SeedData` klasy w celu utworzenia konta testowego:</span><span class="sxs-lookup"><span data-stu-id="464fe-178">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="464fe-179">Dodaj identyfikator użytkownika administratora i `ContactStatus` do kontaktów.</span><span class="sxs-lookup"><span data-stu-id="464fe-179">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="464fe-180">Utworzyć kontaktów "Przesłane" i jednym "odrzucone".</span><span class="sxs-lookup"><span data-stu-id="464fe-180">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="464fe-181">Dodawanie stanu i Identyfikatora użytkownika do wszystkich kontaktów.</span><span class="sxs-lookup"><span data-stu-id="464fe-181">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="464fe-182">Wyświetlane jest tylko jeden kontakt:</span><span class="sxs-lookup"><span data-stu-id="464fe-182">Only one contact is shown:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="464fe-183">Utwórz właściciela, Menedżer i obsługi autoryzacji administratora</span><span class="sxs-lookup"><span data-stu-id="464fe-183">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="464fe-184">Utwórz `ContactIsOwnerAuthorizationHandler` klasy w *autoryzacji* folderu.</span><span class="sxs-lookup"><span data-stu-id="464fe-184">Create a `ContactIsOwnerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="464fe-185">`ContactIsOwnerAuthorizationHandler` Sprawdza, czy użytkownik, działając w zasobie jest właścicielem zasobu.</span><span class="sxs-lookup"><span data-stu-id="464fe-185">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[Main](secure-data/samples/final2/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="464fe-186">`ContactIsOwnerAuthorizationHandler` Wywołania [kontekstu. Pomyślnie](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) bieżący uwierzytelniony użytkownik jest właścicielem kontaktu.</span><span class="sxs-lookup"><span data-stu-id="464fe-186">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="464fe-187">Programy obsługi autoryzacji zazwyczaj:</span><span class="sxs-lookup"><span data-stu-id="464fe-187">Authorization handlers generally:</span></span>

* <span data-ttu-id="464fe-188">Zwraca `context.Succeed` gdy są spełnione wymagania.</span><span class="sxs-lookup"><span data-stu-id="464fe-188">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="464fe-189">Zwraca `Task.CompletedTask` gdy nie są spełnione wymagania.</span><span class="sxs-lookup"><span data-stu-id="464fe-189">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="464fe-190">`Task.CompletedTask`jest ani powodzenie lub niepowodzenie&mdash;umożliwia innych programów obsługi autoryzacji do uruchomienia.</span><span class="sxs-lookup"><span data-stu-id="464fe-190">`Task.CompletedTask` is neither success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="464fe-191">Jeśli musisz jawnie zakończyć się niepowodzeniem, zwróć [kontekstu. Niepowodzenie](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span><span class="sxs-lookup"><span data-stu-id="464fe-191">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="464fe-192">Aplikacja umożliwia kontaktu właścicieli edytowanie/usuwanie/utworzyć własne dane.</span><span class="sxs-lookup"><span data-stu-id="464fe-192">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="464fe-193">`ContactIsOwnerAuthorizationHandler`nie trzeba sprawdzić działanie przekazany parametr wymaganie.</span><span class="sxs-lookup"><span data-stu-id="464fe-193">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="464fe-194">Tworzenie obsługi programu Menedżer autoryzacji</span><span class="sxs-lookup"><span data-stu-id="464fe-194">Create a manager authorization handler</span></span>

<span data-ttu-id="464fe-195">Utwórz `ContactManagerAuthorizationHandler` klasy w *autoryzacji* folderu.</span><span class="sxs-lookup"><span data-stu-id="464fe-195">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="464fe-196">`ContactManagerAuthorizationHandler` Weryfikuje użytkownika, działając w zasobie menedżera.</span><span class="sxs-lookup"><span data-stu-id="464fe-196">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="464fe-197">Tylko menedżerowie można zatwierdzić lub odrzucić zmiany zawartości (nowe lub zmienione).</span><span class="sxs-lookup"><span data-stu-id="464fe-197">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[Main](secure-data/samples/final2/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="464fe-198">Utwórz program obsługi autoryzacji administratora</span><span class="sxs-lookup"><span data-stu-id="464fe-198">Create an administrator authorization handler</span></span>

<span data-ttu-id="464fe-199">Utwórz `ContactAdministratorsAuthorizationHandler` klasy w *autoryzacji* folderu.</span><span class="sxs-lookup"><span data-stu-id="464fe-199">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="464fe-200">`ContactAdministratorsAuthorizationHandler` Sprawdza, czy użytkownik na zasób jest administratorem.</span><span class="sxs-lookup"><span data-stu-id="464fe-200">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="464fe-201">Administrator może wykonać wszystkie operacje.</span><span class="sxs-lookup"><span data-stu-id="464fe-201">Administrator can do all operations.</span></span>

[!code-csharp[Main](secure-data/samples/final2/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="464fe-202">Rejestrowanie procedur obsługi autoryzacji</span><span class="sxs-lookup"><span data-stu-id="464fe-202">Register the authorization handlers</span></span>

<span data-ttu-id="464fe-203">Musi być zarejestrowany przy użyciu programu Entity Framework Core Services [iniekcji zależności](xref:fundamentals/dependency-injection) przy użyciu [AddScoped](/aspnet/core/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span><span class="sxs-lookup"><span data-stu-id="464fe-203">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/aspnet/core/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="464fe-204">`ContactIsOwnerAuthorizationHandler` Korzysta z platformy ASP.NET Core [tożsamości](xref:security/authentication/identity), który jest wbudowany w program Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="464fe-204">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="464fe-205">Zarejestrować obsługi z kolekcją usługi, dzięki czemu są dostępne do `ContactsController` za pośrednictwem [iniekcji zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="464fe-205">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="464fe-206">Dodaj następujący kod na końcu `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="464fe-206">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Startup.cs?name=ConfigureServices&highlight=41-)]

<span data-ttu-id="464fe-207">`ContactAdministratorsAuthorizationHandler`i `ContactManagerAuthorizationHandler` są dodawane jako pojedynczych wystąpień.</span><span class="sxs-lookup"><span data-stu-id="464fe-207">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="464fe-208">Są one pojedynczych wystąpień, ponieważ nie używają EF i wszystkich informacji potrzebnych `Context` parametr `HandleRequirementAsync` metody.</span><span class="sxs-lookup"><span data-stu-id="464fe-208">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="464fe-209">Obsługuje autoryzacji</span><span class="sxs-lookup"><span data-stu-id="464fe-209">Support authorization</span></span>

<span data-ttu-id="464fe-210">W tej sekcji zaktualizuj stron Razor i Dodaj klasę wymagań operacji.</span><span class="sxs-lookup"><span data-stu-id="464fe-210">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="464fe-211">Przegląd klasy wymagań operacji kontaktów</span><span class="sxs-lookup"><span data-stu-id="464fe-211">Review the contact operations requirements class</span></span>

<span data-ttu-id="464fe-212">Przegląd `ContactOperations` klasy.</span><span class="sxs-lookup"><span data-stu-id="464fe-212">Review the `ContactOperations` class.</span></span> <span data-ttu-id="464fe-213">Ta klasa zawiera wymagania obsługuje aplikacja:</span><span class="sxs-lookup"><span data-stu-id="464fe-213">This class contains the requirements the app supports:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-razor-pages"></a><span data-ttu-id="464fe-214">Utwórz klasę podstawową dla stron Razor</span><span class="sxs-lookup"><span data-stu-id="464fe-214">Create a base class for the Razor Pages</span></span>

<span data-ttu-id="464fe-215">Tworzenie klasy podstawowej, który zawiera usługi używane w kontaktach stron Razor.</span><span class="sxs-lookup"><span data-stu-id="464fe-215">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="464fe-216">Klasa podstawowa umieszcza kod inicjowania w jednej lokalizacji:</span><span class="sxs-lookup"><span data-stu-id="464fe-216">The base class puts that initialization code in one location:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="464fe-217">Poprzedni kod:</span><span class="sxs-lookup"><span data-stu-id="464fe-217">The preceding code:</span></span>

* <span data-ttu-id="464fe-218">Dodaje `IAuthorizationService` usługi z dostępem do obsługi autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="464fe-218">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="464fe-219">Dodaje tożsamość `UserManager` usługi.</span><span class="sxs-lookup"><span data-stu-id="464fe-219">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="464fe-220">Dodaj `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="464fe-220">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="464fe-221">Aktualizacja CreateModel</span><span class="sxs-lookup"><span data-stu-id="464fe-221">Update the CreateModel</span></span>

<span data-ttu-id="464fe-222">Zaktualizuj Tworzenie strony modelu konstruktora do użycia `DI_BasePageModel` klasy podstawowej:</span><span class="sxs-lookup"><span data-stu-id="464fe-222">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="464fe-223">Aktualizacja `CreateModel.OnPostAsync` metodę:</span><span class="sxs-lookup"><span data-stu-id="464fe-223">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="464fe-224">Identyfikator użytkownika, aby dodać `Contact` modelu.</span><span class="sxs-lookup"><span data-stu-id="464fe-224">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="464fe-225">Wywołuje program obsługi autoryzacji, aby sprawdzić, czy użytkownik ma uprawnienia do tworzenia kontaktów.</span><span class="sxs-lookup"><span data-stu-id="464fe-225">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="464fe-226">Aktualizacja IndexModel</span><span class="sxs-lookup"><span data-stu-id="464fe-226">Update the IndexModel</span></span>

<span data-ttu-id="464fe-227">Aktualizacja `OnGetAsync` dlatego kontakty tylko zatwierdzone są wyświetlane użytkownikom ogólne:</span><span class="sxs-lookup"><span data-stu-id="464fe-227">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="464fe-228">Aktualizacja EditModel</span><span class="sxs-lookup"><span data-stu-id="464fe-228">Update the EditModel</span></span>

<span data-ttu-id="464fe-229">Dodaj program obsługi autoryzacji, aby sprawdzić, czy użytkownik jest właścicielem kontaktu.</span><span class="sxs-lookup"><span data-stu-id="464fe-229">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="464fe-230">Ponieważ jest sprawdzana autoryzacji zasobów `[Authorize]` atrybut nie jest wystarczająca.</span><span class="sxs-lookup"><span data-stu-id="464fe-230">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="464fe-231">Aplikacja nie ma dostępu do zasobu, gdy atrybuty są oceniane.</span><span class="sxs-lookup"><span data-stu-id="464fe-231">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="464fe-232">Musi być bezwzględnie autoryzacji na podstawie zasobów.</span><span class="sxs-lookup"><span data-stu-id="464fe-232">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="464fe-233">Kontroli należy wykonać, gdy aplikacja ma dostęp do zasobów przez załadowanie go w modelu strony lub przez załadowanie go w obrębie samego program obsługi.</span><span class="sxs-lookup"><span data-stu-id="464fe-233">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="464fe-234">Zasób jest często używany przez przekazywanie klucz zasobu.</span><span class="sxs-lookup"><span data-stu-id="464fe-234">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="464fe-235">Aktualizacja DeleteModel</span><span class="sxs-lookup"><span data-stu-id="464fe-235">Update the DeleteModel</span></span>

<span data-ttu-id="464fe-236">Aktualizacja modelu strony Usuń na potrzeby obsługi autoryzacji Sprawdź, czy użytkownik ma uprawnienia do usuwania na kontakt.</span><span class="sxs-lookup"><span data-stu-id="464fe-236">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="464fe-237">Wstaw usługi autoryzacji do widoków</span><span class="sxs-lookup"><span data-stu-id="464fe-237">Inject the authorization service into the views</span></span>

<span data-ttu-id="464fe-238">Obecnie pokazuje interfejsu użytkownika edytowania i usuwania łącza do danych użytkownika nie można zmodyfikować.</span><span class="sxs-lookup"><span data-stu-id="464fe-238">Currently, the UI shows edit and delete links for data the user can't modify.</span></span> <span data-ttu-id="464fe-239">Interfejs użytkownika jest rozwiązany przez zastosowanie obsługi autoryzacji do widoków.</span><span class="sxs-lookup"><span data-stu-id="464fe-239">The UI is fixed by applying the authorization handler to the views.</span></span>

<span data-ttu-id="464fe-240">Wstaw usługi autoryzacji w *Views/_ViewImports.cshtml* pliku, jest dostępny dla wszystkich widoków:</span><span class="sxs-lookup"><span data-stu-id="464fe-240">Inject the authorization service in the *Views/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[Main](secure-data/samples/final2/Pages/_ViewImports.cshtml?highlight=6-9)]

<span data-ttu-id="464fe-241">Poprzedni kod znaczników dodaje kilka `using` instrukcje.</span><span class="sxs-lookup"><span data-stu-id="464fe-241">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="464fe-242">Aktualizacja **Edytuj** i **usunąć** łączy w *Pages/Contacts/Index.cshtml* , są one tylko renderowania dla użytkowników z odpowiednimi uprawnieniami:</span><span class="sxs-lookup"><span data-stu-id="464fe-242">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[Main](secure-data/samples/final2/Pages/Contacts/Index.cshtml?highlight=34-36,64-)]

> [!WARNING]
> <span data-ttu-id="464fe-243">Ukrywanie łącza od użytkowników, które nie mają uprawnień do zmiany danych nie zabezpieczenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="464fe-243">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="464fe-244">Ukrywanie łączy powoduje, że bardziej przyjazny dla użytkownika aplikacji przez wyświetlanie łączy jedyne prawidłowe.</span><span class="sxs-lookup"><span data-stu-id="464fe-244">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="464fe-245">Użytkownicy mogą hack wygenerowanego adresy URL do wywołania edytowania i usuwania operacji na danych, które nie są właścicielami.</span><span class="sxs-lookup"><span data-stu-id="464fe-245">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="464fe-246">Razor strony lub kontrolera muszą wymuszać kontroli dostępu do zabezpieczania danych.</span><span class="sxs-lookup"><span data-stu-id="464fe-246">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="464fe-247">Szczegóły aktualizacji</span><span class="sxs-lookup"><span data-stu-id="464fe-247">Update Details</span></span>

<span data-ttu-id="464fe-248">Aktualizacja w widoku szczegółów, menedżerów można zatwierdzić lub odrzucić kontaktów:</span><span class="sxs-lookup"><span data-stu-id="464fe-248">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[Main](secure-data/samples/final2/Pages/Contacts/Details.cshtml?range=48-)]

<span data-ttu-id="464fe-249">Aktualizacja modelu strony szczegółów:</span><span class="sxs-lookup"><span data-stu-id="464fe-249">Update the details page model:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="test-the-completed-app"></a><span data-ttu-id="464fe-250">Testowanie ukończonej aplikacji</span><span class="sxs-lookup"><span data-stu-id="464fe-250">Test the completed app</span></span>

<span data-ttu-id="464fe-251">Za pomocą programu Visual Studio Code lub testowania na platformie lokalnego, który nie zawiera certyfikatu testowego dla protokołu SSL:</span><span class="sxs-lookup"><span data-stu-id="464fe-251">If you're using Visual Studio Code or testing on a local platform that doesn't include a test certificate for SSL:</span></span>

* <span data-ttu-id="464fe-252">Ustaw `"LocalTest:skipSSL": true` w *appsettings. Developement.JSON* plik, aby pominąć wymaganie protokołu SSL.</span><span class="sxs-lookup"><span data-stu-id="464fe-252">Set `"LocalTest:skipSSL": true` in the *appsettings.Developement.json* file to skip the SSL requirement.</span></span> <span data-ttu-id="464fe-253">Pomiń SSL tylko na komputerze deweloperskim.</span><span class="sxs-lookup"><span data-stu-id="464fe-253">Skip SSL only on a development machine.</span></span>

<span data-ttu-id="464fe-254">Jeśli aplikacja ma kontaktów:</span><span class="sxs-lookup"><span data-stu-id="464fe-254">If the app has contacts:</span></span>

* <span data-ttu-id="464fe-255">Usuń wszystkie rekordy w `Contact` tabeli.</span><span class="sxs-lookup"><span data-stu-id="464fe-255">Delete all the records in the `Contact` table.</span></span>
* <span data-ttu-id="464fe-256">Ponowne uruchomienie aplikacji w celu umieszczenia bazy danych.</span><span class="sxs-lookup"><span data-stu-id="464fe-256">Restart the app to seed the database.</span></span>

<span data-ttu-id="464fe-257">Zarejestruj użytkownika dla przeglądania kontaktów.</span><span class="sxs-lookup"><span data-stu-id="464fe-257">Register a user for browsing the contacts.</span></span>

<span data-ttu-id="464fe-258">Łatwe testowanie ukończonej aplikacji jest uruchamianie trzech różnych przeglądarkach (lub incognito/sesję InPrivate wersji).</span><span class="sxs-lookup"><span data-stu-id="464fe-258">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate versions).</span></span> <span data-ttu-id="464fe-259">W przeglądarce jednego zarejestrować nowego użytkownika (na przykład `test@example.com`).</span><span class="sxs-lookup"><span data-stu-id="464fe-259">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="464fe-260">Zaloguj się do każdej przeglądarki z innym użytkownikiem.</span><span class="sxs-lookup"><span data-stu-id="464fe-260">Sign in to each browser with a different user.</span></span> <span data-ttu-id="464fe-261">Sprawdź następujące operacje:</span><span class="sxs-lookup"><span data-stu-id="464fe-261">Verify the following operations:</span></span>

* <span data-ttu-id="464fe-262">Zarejestrowani użytkownicy mogą wyświetlać wszystkie zatwierdzone dane kontaktu.</span><span class="sxs-lookup"><span data-stu-id="464fe-262">Registered users can view all the approved contact data.</span></span>
* <span data-ttu-id="464fe-263">Zarejestrowani użytkownicy mogą edytowanie/usuwanie własnych danych.</span><span class="sxs-lookup"><span data-stu-id="464fe-263">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="464fe-264">Menedżerowie można zatwierdzić lub odrzucić dane kontaktowe.</span><span class="sxs-lookup"><span data-stu-id="464fe-264">Managers can approve or reject contact data.</span></span> <span data-ttu-id="464fe-265">`Details` Wyświetlić pokazuje **Zatwierdź** i **Odrzuć** przycisków.</span><span class="sxs-lookup"><span data-stu-id="464fe-265">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="464fe-266">Administratorzy mogą zatwierdzać Odrzuć i edytowanie/usuwanie żadnych danych.</span><span class="sxs-lookup"><span data-stu-id="464fe-266">Administrators can approve/reject and edit/delete any data.</span></span>

| <span data-ttu-id="464fe-267">Użytkownik</span><span class="sxs-lookup"><span data-stu-id="464fe-267">User</span></span>| <span data-ttu-id="464fe-268">Opcje</span><span class="sxs-lookup"><span data-stu-id="464fe-268">Options</span></span> |
| ------------ | ---------|
| test@example.com | <span data-ttu-id="464fe-269">Można edytowanie/usuwanie własnych danych</span><span class="sxs-lookup"><span data-stu-id="464fe-269">Can edit/delete own data</span></span> |
| manager@contoso.com | <span data-ttu-id="464fe-270">Można zatwierdzić Odrzuć i edytowanie i usuwanie własnych danych</span><span class="sxs-lookup"><span data-stu-id="464fe-270">Can approve/reject and edit/delete own data</span></span> |
| admin@contoso.com | <span data-ttu-id="464fe-271">Edytowanie i usuwanie można i Zatwierdź/Odrzuć wszystkie dane</span><span class="sxs-lookup"><span data-stu-id="464fe-271">Can edit/delete and approve/reject all data</span></span>|

<span data-ttu-id="464fe-272">Utworzenie kontaktu w przeglądarce administratora.</span><span class="sxs-lookup"><span data-stu-id="464fe-272">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="464fe-273">Skopiuj adres URL do usunięcia i Edytuj z skontaktowanie się z administratorem.</span><span class="sxs-lookup"><span data-stu-id="464fe-273">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="464fe-274">Wklej te linki do przeglądarki użytkownika testowego, aby sprawdzić, czy użytkownik testu nie może wykonywać te operacje.</span><span class="sxs-lookup"><span data-stu-id="464fe-274">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="464fe-275">Utworzyć początkową aplikację</span><span class="sxs-lookup"><span data-stu-id="464fe-275">Create the starter app</span></span>

* <span data-ttu-id="464fe-276">Tworzenie stron Razor aplikacji o nazwie "ContactManager"</span><span class="sxs-lookup"><span data-stu-id="464fe-276">Create a Razor Pages app named "ContactManager"</span></span>

  * <span data-ttu-id="464fe-277">Tworzenie aplikacji z **indywidualne konta użytkowników**.</span><span class="sxs-lookup"><span data-stu-id="464fe-277">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="464fe-278">Nadaj mu nazwę "ContactManager", przestrzeń nazw odpowiada przestrzeni nazw używany w próbce.</span><span class="sxs-lookup"><span data-stu-id="464fe-278">Name it "ContactManager" so your namespace matches the namespace used in the sample.</span></span>

  ```console
  dotnet new razor -o ContactManager -au Individual -uld
  ```

  * <span data-ttu-id="464fe-279">`-uld`Określa LocalDB zamiast SQLite</span><span class="sxs-lookup"><span data-stu-id="464fe-279">`-uld` specifies LocalDB instead of SQLite</span></span>

* <span data-ttu-id="464fe-280">Dodaj następujące `Contact` modelu:</span><span class="sxs-lookup"><span data-stu-id="464fe-280">Add the following `Contact` model:</span></span>

  [!code-csharp[Main](secure-data/samples/starter2/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="464fe-281">Szkieletu `Contact` modelu:</span><span class="sxs-lookup"><span data-stu-id="464fe-281">Scaffold the `Contact` model:</span></span>

```console
dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
```

* <span data-ttu-id="464fe-282">Aktualizacja **ContactManager** zakotwiczenia w *Pages/_Layout.cshtml* pliku:</span><span class="sxs-lookup"><span data-stu-id="464fe-282">Update the **ContactManager** anchor in the *Pages/_Layout.cshtml* file:</span></span>

```cshtml
<a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
```

* <span data-ttu-id="464fe-283">Tworzenie szkieletu początkowej migracji i aktualizacji bazy danych:</span><span class="sxs-lookup"><span data-stu-id="464fe-283">Scaffold the initial migration and update the database:</span></span>

```console
dotnet ef migrations add initial
dotnet ef database update
```

* <span data-ttu-id="464fe-284">Testowanie aplikacji przez tworzenie, edytowanie i usuwanie kontaktu</span><span class="sxs-lookup"><span data-stu-id="464fe-284">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="464fe-285">Inicjatora bazy danych</span><span class="sxs-lookup"><span data-stu-id="464fe-285">Seed the database</span></span>

<span data-ttu-id="464fe-286">Dodaj `SeedData` klasy do *danych* folderu.</span><span class="sxs-lookup"><span data-stu-id="464fe-286">Add the `SeedData` class to the *Data* folder.</span></span> <span data-ttu-id="464fe-287">Jeśli pobrano próbki, możesz skopiować *SeedData.cs* pliku *danych* folderu projektu początkowego.</span><span class="sxs-lookup"><span data-stu-id="464fe-287">If you've downloaded the sample, you can copy the *SeedData.cs* file to the *Data* folder of the starter project.</span></span>

<span data-ttu-id="464fe-288">Wywołanie `SeedData.Initialize` z `Main`:</span><span class="sxs-lookup"><span data-stu-id="464fe-288">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[Main](secure-data/samples/starter2/Program.cs?name=snippet)]

<span data-ttu-id="464fe-289">Testowanie, czy aplikacja rozpoczęta bazy danych.</span><span class="sxs-lookup"><span data-stu-id="464fe-289">Test that the app seeded the database.</span></span> <span data-ttu-id="464fe-290">Jeśli istnieją wszystkie wiersze w kontakcie DB, seed — metoda nie działa.</span><span class="sxs-lookup"><span data-stu-id="464fe-290">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a><span data-ttu-id="464fe-291">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="464fe-291">Additional resources</span></span>

* <span data-ttu-id="464fe-292">[Laboratorium autoryzacji platformy ASP.NET Core](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span><span class="sxs-lookup"><span data-stu-id="464fe-292">[ASP.NET Core Authorization Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span></span> <span data-ttu-id="464fe-293">W tym laboratorium przechodzi w stan więcej szczegółów na temat funkcji zabezpieczeń wprowadzone w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="464fe-293">This lab goes into more detail on the security features introduced in this tutorial.</span></span>
* [<span data-ttu-id="464fe-294">Autoryzacja w ASP.NET Core: prosty, rolę, opartej na oświadczeniach, a niestandardowe</span><span class="sxs-lookup"><span data-stu-id="464fe-294">Authorization in ASP.NET Core: Simple, role, claims-based, and custom</span></span>](xref:security/authorization/index)
* [<span data-ttu-id="464fe-295">Niestandardowe autoryzacji opartych na zasadach</span><span class="sxs-lookup"><span data-stu-id="464fe-295">Custom policy-based authorization</span></span>](xref:security/authorization/policies)
