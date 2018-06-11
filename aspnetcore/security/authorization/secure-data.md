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
ms.openlocfilehash: 1ffa44d1816284d563b80b2d9a02b7b816116ee1
ms.sourcegitcommit: 63fb07fb3f71b32daf2c9466e132f2e7cc617163
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/10/2018
ms.locfileid: "35252116"
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a><span data-ttu-id="ba348-104">Tworzenie aplikacji platformy ASP.NET Core z danych użytkownika chronione przez autoryzacji</span><span class="sxs-lookup"><span data-stu-id="ba348-104">Create an ASP.NET Core app with user data protected by authorization</span></span>

<span data-ttu-id="ba348-105">Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Audette Jan](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="ba348-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="ba348-106">W tym samouczku przedstawiono sposób tworzenia aplikacji sieci web platformy ASP.NET Core z danymi użytkownika chronione przez autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="ba348-106">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="ba348-107">Wyświetla listę kontaktów, które uwierzytelnionych użytkowników (zarejestrowanym) został utworzony.</span><span class="sxs-lookup"><span data-stu-id="ba348-107">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="ba348-108">Istnieją trzy grupy zabezpieczeń:</span><span class="sxs-lookup"><span data-stu-id="ba348-108">There are three security groups:</span></span>

* <span data-ttu-id="ba348-109">**Użytkownicy zarejestrowanych** może wyświetlać wszystkie zatwierdzone dane i może edytowanie/usuwanie własnych danych.</span><span class="sxs-lookup"><span data-stu-id="ba348-109">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="ba348-110">**Menedżerowie** można zatwierdzić lub odrzucić dane kontaktowe.</span><span class="sxs-lookup"><span data-stu-id="ba348-110">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="ba348-111">Tylko zatwierdzone kontakty są widoczne dla użytkowników.</span><span class="sxs-lookup"><span data-stu-id="ba348-111">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="ba348-112">**Administratorzy** można zatwierdzić Odrzuć i edytowanie/usuwanie żadnych danych.</span><span class="sxs-lookup"><span data-stu-id="ba348-112">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="ba348-113">Na poniższej ilustracji użytkownik Rick (`rick@example.com`) jest zalogowany.</span><span class="sxs-lookup"><span data-stu-id="ba348-113">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="ba348-114">Rick mogą jedynie wyświetlać zatwierdzonych kontaktów i **Edytuj**/**usunąć**/**Utwórz nowy** łącza do swoich kontaktów.</span><span class="sxs-lookup"><span data-stu-id="ba348-114">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="ba348-115">Tylko ostatni rekord powstały Rick, wyświetla **Edytuj** i **usunąć** łącza.</span><span class="sxs-lookup"><span data-stu-id="ba348-115">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="ba348-116">Inni użytkownicy zobaczą ostatniego rekordu do momentu menedżerowi lub administratorowi zmienia stan na "Zatwierdzone".</span><span class="sxs-lookup"><span data-stu-id="ba348-116">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![Obraz opisane poprzedzających](secure-data/_static/rick.png)

<span data-ttu-id="ba348-118">Na poniższej ilustracji `manager@contoso.com` jest zarejestrowany i rolę menedżerów:</span><span class="sxs-lookup"><span data-stu-id="ba348-118">In the following image, `manager@contoso.com` is signed in and in the managers role:</span></span>

![Obraz opisane poprzedzających](secure-data/_static/manager1.png)

<span data-ttu-id="ba348-120">Na poniższej ilustracji przedstawiono menedżerów widoku Szczegóły kontaktu:</span><span class="sxs-lookup"><span data-stu-id="ba348-120">The following image shows the managers details view of a contact:</span></span>

![Obraz opisane poprzedzających](secure-data/_static/manager.png)

<span data-ttu-id="ba348-122">**Zatwierdź** i **Odrzuć** przyciski są wyświetlane tylko dla menedżerów i administratorów.</span><span class="sxs-lookup"><span data-stu-id="ba348-122">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="ba348-123">Na poniższej ilustracji `admin@contoso.com` jest zarejestrowany i w roli administratora:</span><span class="sxs-lookup"><span data-stu-id="ba348-123">In the following image, `admin@contoso.com` is signed in and in the administrators role:</span></span>

![Obraz opisane poprzedzających](secure-data/_static/admin.png)

<span data-ttu-id="ba348-125">Administrator ma wszystkie uprawnienia.</span><span class="sxs-lookup"><span data-stu-id="ba348-125">The administrator has all privileges.</span></span> <span data-ttu-id="ba348-126">Użytkownik może odczytu/edytowanie/usuwanie żadnych skontaktuj się z i Zmień stan kontaktów.</span><span class="sxs-lookup"><span data-stu-id="ba348-126">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="ba348-127">Aplikacja została utworzona przez [szkieletów](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) następujące `Contact` modelu:</span><span class="sxs-lookup"><span data-stu-id="ba348-127">The app was created by [scaffolding](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) the following `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/starter2/Models/Contact.cs?name=snippet1)]

<span data-ttu-id="ba348-128">Próbka zawiera poniższe obsługi autoryzacji:</span><span class="sxs-lookup"><span data-stu-id="ba348-128">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="ba348-129">`ContactIsOwnerAuthorizationHandler`: Zapewnia, że użytkownika można edytować tylko ich danych.</span><span class="sxs-lookup"><span data-stu-id="ba348-129">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="ba348-130">`ContactManagerAuthorizationHandler`: Umożliwia menedżerów zatwierdzić lub odrzucić kontaktów.</span><span class="sxs-lookup"><span data-stu-id="ba348-130">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="ba348-131">`ContactAdministratorsAuthorizationHandler`: Pozwala administratorom, aby zatwierdzić lub odrzucić kontaktów i edytowanie/usuwanie kontaktów.</span><span class="sxs-lookup"><span data-stu-id="ba348-131">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ba348-132">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="ba348-132">Prerequisites</span></span>

<span data-ttu-id="ba348-133">W tym samouczku jest zaawansowane.</span><span class="sxs-lookup"><span data-stu-id="ba348-133">This tutorial is advanced.</span></span> <span data-ttu-id="ba348-134">Należy zapoznać się z:</span><span class="sxs-lookup"><span data-stu-id="ba348-134">You should be familiar with:</span></span>

* [<span data-ttu-id="ba348-135">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ba348-135">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="ba348-136">Uwierzytelnianie</span><span class="sxs-lookup"><span data-stu-id="ba348-136">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="ba348-137">Potwierdzenie konta i odzyskiwanie hasła</span><span class="sxs-lookup"><span data-stu-id="ba348-137">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="ba348-138">Autoryzacja</span><span class="sxs-lookup"><span data-stu-id="ba348-138">Authorization</span></span>](xref:security/authorization/index)
* [<span data-ttu-id="ba348-139">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="ba348-139">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<span data-ttu-id="ba348-140">Zobacz [plik PDF](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) dla wersji platformy ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="ba348-140">See [this PDF file](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC version.</span></span> <span data-ttu-id="ba348-141">Wersja platformy ASP.NET Core 1.1 tego samouczka jest [to](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) folderu.</span><span class="sxs-lookup"><span data-stu-id="ba348-141">The ASP.NET Core 1.1 version of this tutorial is in [this](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) folder.</span></span> <span data-ttu-id="ba348-142">1.1, na przykład ASP.NET Core jest w [przykłady](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span><span class="sxs-lookup"><span data-stu-id="ba348-142">The 1.1 ASP.NET Core sample is in the [samples](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span></span>

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="ba348-143">Starter i ukończonej aplikacji</span><span class="sxs-lookup"><span data-stu-id="ba348-143">The starter and completed app</span></span>

<span data-ttu-id="ba348-144">[Pobierz](xref:tutorials/index#how-to-download-a-sample) [ukończone](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2) aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ba348-144">[Download](xref:tutorials/index#how-to-download-a-sample) the [completed](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2) app.</span></span> <span data-ttu-id="ba348-145">[Test](#test-the-completed-app) ukończonej aplikacji, należy zapoznać się z jego funkcje zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="ba348-145">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="ba348-146">Początkową aplikację</span><span class="sxs-lookup"><span data-stu-id="ba348-146">The starter app</span></span>

<span data-ttu-id="ba348-147">[Pobierz](xref:tutorials/index#how-to-download-a-sample) [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2) aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ba348-147">[Download](xref:tutorials/index#how-to-download-a-sample) the [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2) app.</span></span>

<span data-ttu-id="ba348-148">Uruchom aplikację, wybierz **ContactManager** link i sprawdź, tworzenie, edytowanie i usuwanie kontaktu.</span><span class="sxs-lookup"><span data-stu-id="ba348-148">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="ba348-149">Zabezpieczanie danych użytkownika</span><span class="sxs-lookup"><span data-stu-id="ba348-149">Secure user data</span></span>

<span data-ttu-id="ba348-150">Następujące sekcje zostały wszystkie główne kroki umożliwiające utworzenie aplikacji danych bezpiecznego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="ba348-150">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="ba348-151">Może być przydatne do odwoływania się do projektu zakończone.</span><span class="sxs-lookup"><span data-stu-id="ba348-151">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="ba348-152">Powiązanie dane kontaktowe dla użytkownika</span><span class="sxs-lookup"><span data-stu-id="ba348-152">Tie the contact data to the user</span></span>

<span data-ttu-id="ba348-153">Za pomocą programu ASP.NET [tożsamości](xref:security/authentication/identity) identyfikator użytkownika, aby zapewnić użytkownikom można edytować swoje dane, ale nie do innych danych użytkowników.</span><span class="sxs-lookup"><span data-stu-id="ba348-153">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="ba348-154">Dodaj `OwnerID` i `ContactStatus` do `Contact` modelu:</span><span class="sxs-lookup"><span data-stu-id="ba348-154">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/final2/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

<span data-ttu-id="ba348-155">`OwnerID` Identyfikator użytkownika z `AspNetUser` tabeli w [tożsamości](xref:security/authentication/identity) bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ba348-155">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="ba348-156">`Status` Pole określa, czy kontakt jest widoczny dla zwykłych użytkowników.</span><span class="sxs-lookup"><span data-stu-id="ba348-156">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="ba348-157">Tworzenie nowych migracji i aktualizacji bazy danych:</span><span class="sxs-lookup"><span data-stu-id="ba348-157">Create a new migration and update the database:</span></span>

```console
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="require-https-and-authenticated-users"></a><span data-ttu-id="ba348-158">Wymagaj protokołu HTTPS i uwierzytelnionych użytkowników</span><span class="sxs-lookup"><span data-stu-id="ba348-158">Require HTTPS and authenticated users</span></span>

<span data-ttu-id="ba348-159">Dodaj [IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) do `Startup`:</span><span class="sxs-lookup"><span data-stu-id="ba348-159">Add [IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) to `Startup`:</span></span>

[!code-csharp[](secure-data/samples/final2/Startup.cs?name=snippet_env)]

<span data-ttu-id="ba348-160">W `ConfigureServices` metody *Startup.cs* plików, dodawanie [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) filtr autoryzacji:</span><span class="sxs-lookup"><span data-stu-id="ba348-160">In the `ConfigureServices` method of the *Startup.cs* file, add the [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) authorization filter:</span></span>

[!code-csharp[](secure-data/samples/final2/Startup.cs?name=snippet_SSL&highlight=10-999)]

<span data-ttu-id="ba348-161">Jeśli używasz programu Visual Studio, należy włączyć protokół HTTPS.</span><span class="sxs-lookup"><span data-stu-id="ba348-161">If you're using Visual Studio, enable HTTPS.</span></span>

<span data-ttu-id="ba348-162">Przekierowywanie żądań HTTP, HTTPS, zobacz [ponowne zapisywanie adresów URL w oprogramowaniu pośredniczącym](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="ba348-162">To redirect HTTP requests to HTTPS, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="ba348-163">Za pomocą programu Visual Studio Code lub testowania na platformie lokalnego, który nie zawiera certyfikatu testowego dla protokołu HTTPS:</span><span class="sxs-lookup"><span data-stu-id="ba348-163">If you're using Visual Studio Code or testing on a local platform that doesn't include a test certificate for HTTPS:</span></span>

  <span data-ttu-id="ba348-164">Ustaw `"LocalTest:skipSSL": true` w *appsettings. Developement.JSON* pliku.</span><span class="sxs-lookup"><span data-stu-id="ba348-164">Set `"LocalTest:skipSSL": true` in the *appsettings.Developement.json* file.</span></span>

### <a name="require-authenticated-users"></a><span data-ttu-id="ba348-165">Wymaga uwierzytelnionych użytkowników</span><span class="sxs-lookup"><span data-stu-id="ba348-165">Require authenticated users</span></span>

<span data-ttu-id="ba348-166">Ustaw domyślne zasady uwierzytelniania, aby wymagać od użytkowników uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="ba348-166">Set the default authentication policy to require users to be authenticated.</span></span> <span data-ttu-id="ba348-167">Można zrezygnować z uwierzytelniania na poziomie metody Razor strony, kontrolera lub akcji z `[AllowAnonymous]` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="ba348-167">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="ba348-168">Ustawienie domyślne zasady uwierzytelniania, aby wymagać od użytkowników uwierzytelniania chroni nowo dodanego stron Razor i kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="ba348-168">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="ba348-169">Posiadanie uwierzytelniania domyślnie wymagane jest bezpieczniejszy niż polegania na nowe kontrolery i stron Razor do uwzględnienia `[Authorize]` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="ba348-169">Having authentication required by default is safer than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span> 

<span data-ttu-id="ba348-170">Z wymaganiami wszyscy użytkownicy uwierzytelnieni [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder?view=aspnetcore-2.0#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) i [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage?view=aspnetcore-2.0) wywołania nie są wymagane.</span><span class="sxs-lookup"><span data-stu-id="ba348-170">With the requirement of all users authenticated, the [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder?view=aspnetcore-2.0#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) and [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage?view=aspnetcore-2.0) calls are not required.</span></span>

<span data-ttu-id="ba348-171">Aktualizacja `ConfigureServices` z następującymi zmianami:</span><span class="sxs-lookup"><span data-stu-id="ba348-171">Update `ConfigureServices` with the following changes:</span></span>

* <span data-ttu-id="ba348-172">Komentarz `AuthorizeFolder` i `AuthorizePage`.</span><span class="sxs-lookup"><span data-stu-id="ba348-172">Comment out `AuthorizeFolder` and `AuthorizePage`.</span></span>
* <span data-ttu-id="ba348-173">Ustaw domyślne zasady uwierzytelniania, aby wymagać od użytkowników uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="ba348-173">Set the default authentication policy to require users to be authenticated.</span></span>

[!code-csharp[](secure-data/samples/final2/Startup.cs?name=snippet_defaultPolicy&highlight=23-27,31-999)]

<span data-ttu-id="ba348-174">Dodaj [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) do indeksu strony o i skontaktuj się z pomocą tak anonimowych użytkowników można uzyskać informacji o lokacji, przed ich zarejestrować.</span><span class="sxs-lookup"><span data-stu-id="ba348-174">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index, About, and Contact pages so anonymous users can get information about the site before they register.</span></span> 

[!code-csharp[](secure-data/samples/final2/Pages/Index.cshtml.cs?name=snippet&highlight=2)]

<span data-ttu-id="ba348-175">Dodaj `[AllowAnonymous]` do [LoginModel i RegisterModel](https://github.com/aspnet/templating/issues/238).</span><span class="sxs-lookup"><span data-stu-id="ba348-175">Add `[AllowAnonymous]` to the [LoginModel and RegisterModel](https://github.com/aspnet/templating/issues/238).</span></span>

### <a name="configure-the-test-account"></a><span data-ttu-id="ba348-176">Skonfiguruj konto testu</span><span class="sxs-lookup"><span data-stu-id="ba348-176">Configure the test account</span></span>

<span data-ttu-id="ba348-177">`SeedData` Klasy tworzy dwa konta: administrator i Menedżer.</span><span class="sxs-lookup"><span data-stu-id="ba348-177">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="ba348-178">Użyj [narzędzie Menedżer klucz tajny](xref:security/app-secrets) ustawienie hasła dla tych kont.</span><span class="sxs-lookup"><span data-stu-id="ba348-178">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="ba348-179">Ustawianie hasła z katalogu projektu (katalog zawierający *Program.cs*):</span><span class="sxs-lookup"><span data-stu-id="ba348-179">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```console
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="ba348-180">Aktualizacja `Main` hasła testu:</span><span class="sxs-lookup"><span data-stu-id="ba348-180">Update `Main` to use the test password:</span></span>

[!code-csharp[](secure-data/samples/final2/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="ba348-181">Tworzenie konta testowe i aktualizowanie kontaktów</span><span class="sxs-lookup"><span data-stu-id="ba348-181">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="ba348-182">Aktualizacja `Initialize` metoda `SeedData` klasy w celu utworzenia konta testowego:</span><span class="sxs-lookup"><span data-stu-id="ba348-182">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[](secure-data/samples/final2/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="ba348-183">Dodaj identyfikator użytkownika administratora i `ContactStatus` do kontaktów.</span><span class="sxs-lookup"><span data-stu-id="ba348-183">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="ba348-184">Utworzyć kontaktów "Przesłane" i jednym "odrzucone".</span><span class="sxs-lookup"><span data-stu-id="ba348-184">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="ba348-185">Dodawanie stanu i Identyfikatora użytkownika do wszystkich kontaktów.</span><span class="sxs-lookup"><span data-stu-id="ba348-185">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="ba348-186">Wyświetlane jest tylko jeden kontakt:</span><span class="sxs-lookup"><span data-stu-id="ba348-186">Only one contact is shown:</span></span>

[!code-csharp[](secure-data/samples/final2/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="ba348-187">Utwórz właściciela, Menedżer i obsługi autoryzacji administratora</span><span class="sxs-lookup"><span data-stu-id="ba348-187">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="ba348-188">Utwórz `ContactIsOwnerAuthorizationHandler` klasy w *autoryzacji* folderu.</span><span class="sxs-lookup"><span data-stu-id="ba348-188">Create a `ContactIsOwnerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="ba348-189">`ContactIsOwnerAuthorizationHandler` Sprawdza, czy użytkownik, działając w zasobie jest właścicielem zasobu.</span><span class="sxs-lookup"><span data-stu-id="ba348-189">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[](secure-data/samples/final2/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="ba348-190">`ContactIsOwnerAuthorizationHandler` Wywołania [kontekstu. Pomyślnie](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) bieżący uwierzytelniony użytkownik jest właścicielem kontaktu.</span><span class="sxs-lookup"><span data-stu-id="ba348-190">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="ba348-191">Programy obsługi autoryzacji zazwyczaj:</span><span class="sxs-lookup"><span data-stu-id="ba348-191">Authorization handlers generally:</span></span>

* <span data-ttu-id="ba348-192">Zwraca `context.Succeed` gdy są spełnione wymagania.</span><span class="sxs-lookup"><span data-stu-id="ba348-192">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="ba348-193">Zwraca `Task.CompletedTask` gdy nie są spełnione wymagania.</span><span class="sxs-lookup"><span data-stu-id="ba348-193">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="ba348-194">`Task.CompletedTask` jest ani powodzenie lub niepowodzenie&mdash;umożliwia innych programów obsługi autoryzacji do uruchomienia.</span><span class="sxs-lookup"><span data-stu-id="ba348-194">`Task.CompletedTask` is neither success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="ba348-195">Jeśli musisz jawnie zakończyć się niepowodzeniem, zwróć [kontekstu. Niepowodzenie](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span><span class="sxs-lookup"><span data-stu-id="ba348-195">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="ba348-196">Aplikacja umożliwia kontaktu właścicieli edytowanie/usuwanie/utworzyć własne dane.</span><span class="sxs-lookup"><span data-stu-id="ba348-196">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="ba348-197">`ContactIsOwnerAuthorizationHandler` nie trzeba sprawdzić działanie przekazany parametr wymaganie.</span><span class="sxs-lookup"><span data-stu-id="ba348-197">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="ba348-198">Tworzenie obsługi programu Menedżer autoryzacji</span><span class="sxs-lookup"><span data-stu-id="ba348-198">Create a manager authorization handler</span></span>

<span data-ttu-id="ba348-199">Utwórz `ContactManagerAuthorizationHandler` klasy w *autoryzacji* folderu.</span><span class="sxs-lookup"><span data-stu-id="ba348-199">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="ba348-200">`ContactManagerAuthorizationHandler` Weryfikuje użytkownika, działając w zasobie menedżera.</span><span class="sxs-lookup"><span data-stu-id="ba348-200">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="ba348-201">Tylko menedżerowie można zatwierdzić lub odrzucić zmiany zawartości (nowe lub zmienione).</span><span class="sxs-lookup"><span data-stu-id="ba348-201">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[](secure-data/samples/final2/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="ba348-202">Utwórz program obsługi autoryzacji administratora</span><span class="sxs-lookup"><span data-stu-id="ba348-202">Create an administrator authorization handler</span></span>

<span data-ttu-id="ba348-203">Utwórz `ContactAdministratorsAuthorizationHandler` klasy w *autoryzacji* folderu.</span><span class="sxs-lookup"><span data-stu-id="ba348-203">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="ba348-204">`ContactAdministratorsAuthorizationHandler` Sprawdza, czy użytkownik na zasób jest administratorem.</span><span class="sxs-lookup"><span data-stu-id="ba348-204">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="ba348-205">Administrator może wykonać wszystkie operacje.</span><span class="sxs-lookup"><span data-stu-id="ba348-205">Administrator can do all operations.</span></span>

[!code-csharp[](secure-data/samples/final2/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="ba348-206">Rejestrowanie procedur obsługi autoryzacji</span><span class="sxs-lookup"><span data-stu-id="ba348-206">Register the authorization handlers</span></span>

<span data-ttu-id="ba348-207">Musi być zarejestrowany przy użyciu programu Entity Framework Core Services [iniekcji zależności](xref:fundamentals/dependency-injection) przy użyciu [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span><span class="sxs-lookup"><span data-stu-id="ba348-207">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="ba348-208">`ContactIsOwnerAuthorizationHandler` Korzysta z platformy ASP.NET Core [tożsamości](xref:security/authentication/identity), który jest wbudowany w program Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="ba348-208">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="ba348-209">Zarejestrować obsługi z kolekcją usługi, dzięki czemu są dostępne do `ContactsController` za pośrednictwem [iniekcji zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="ba348-209">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="ba348-210">Dodaj następujący kod na końcu `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="ba348-210">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[](secure-data/samples/final2/Startup.cs?name=ConfigureServices&highlight=41-999)]

<span data-ttu-id="ba348-211">`ContactAdministratorsAuthorizationHandler` i `ContactManagerAuthorizationHandler` są dodawane jako pojedynczych wystąpień.</span><span class="sxs-lookup"><span data-stu-id="ba348-211">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="ba348-212">Są one pojedynczych wystąpień, ponieważ nie używają EF i wszystkich informacji potrzebnych `Context` parametr `HandleRequirementAsync` metody.</span><span class="sxs-lookup"><span data-stu-id="ba348-212">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="ba348-213">Obsługuje autoryzacji</span><span class="sxs-lookup"><span data-stu-id="ba348-213">Support authorization</span></span>

<span data-ttu-id="ba348-214">W tej sekcji zaktualizuj stron Razor i Dodaj klasę wymagań operacji.</span><span class="sxs-lookup"><span data-stu-id="ba348-214">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="ba348-215">Przegląd klasy wymagań operacji kontaktów</span><span class="sxs-lookup"><span data-stu-id="ba348-215">Review the contact operations requirements class</span></span>

<span data-ttu-id="ba348-216">Przegląd `ContactOperations` klasy.</span><span class="sxs-lookup"><span data-stu-id="ba348-216">Review the `ContactOperations` class.</span></span> <span data-ttu-id="ba348-217">Ta klasa zawiera wymagania obsługuje aplikacja:</span><span class="sxs-lookup"><span data-stu-id="ba348-217">This class contains the requirements the app supports:</span></span>

[!code-csharp[](secure-data/samples/final2/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-razor-pages"></a><span data-ttu-id="ba348-218">Utwórz klasę podstawową dla stron Razor</span><span class="sxs-lookup"><span data-stu-id="ba348-218">Create a base class for the Razor Pages</span></span>

<span data-ttu-id="ba348-219">Tworzenie klasy podstawowej, który zawiera usługi używane w kontaktach stron Razor.</span><span class="sxs-lookup"><span data-stu-id="ba348-219">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="ba348-220">Klasa podstawowa umieszcza kod inicjowania w jednej lokalizacji:</span><span class="sxs-lookup"><span data-stu-id="ba348-220">The base class puts that initialization code in one location:</span></span>

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="ba348-221">Poprzedni kod:</span><span class="sxs-lookup"><span data-stu-id="ba348-221">The preceding code:</span></span>

* <span data-ttu-id="ba348-222">Dodaje `IAuthorizationService` usługi z dostępem do obsługi autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="ba348-222">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="ba348-223">Dodaje tożsamość `UserManager` usługi.</span><span class="sxs-lookup"><span data-stu-id="ba348-223">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="ba348-224">Dodaj `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="ba348-224">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="ba348-225">Aktualizacja CreateModel</span><span class="sxs-lookup"><span data-stu-id="ba348-225">Update the CreateModel</span></span>

<span data-ttu-id="ba348-226">Zaktualizuj Tworzenie strony modelu konstruktora do użycia `DI_BasePageModel` klasy podstawowej:</span><span class="sxs-lookup"><span data-stu-id="ba348-226">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="ba348-227">Aktualizacja `CreateModel.OnPostAsync` metodę:</span><span class="sxs-lookup"><span data-stu-id="ba348-227">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="ba348-228">Identyfikator użytkownika, aby dodać `Contact` modelu.</span><span class="sxs-lookup"><span data-stu-id="ba348-228">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="ba348-229">Wywołuje program obsługi autoryzacji, aby sprawdzić, czy użytkownik ma uprawnienia do tworzenia kontaktów.</span><span class="sxs-lookup"><span data-stu-id="ba348-229">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="ba348-230">Aktualizacja IndexModel</span><span class="sxs-lookup"><span data-stu-id="ba348-230">Update the IndexModel</span></span>

<span data-ttu-id="ba348-231">Aktualizacja `OnGetAsync` dlatego kontakty tylko zatwierdzone są wyświetlane użytkownikom ogólne:</span><span class="sxs-lookup"><span data-stu-id="ba348-231">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="ba348-232">Aktualizacja EditModel</span><span class="sxs-lookup"><span data-stu-id="ba348-232">Update the EditModel</span></span>

<span data-ttu-id="ba348-233">Dodaj program obsługi autoryzacji, aby sprawdzić, czy użytkownik jest właścicielem kontaktu.</span><span class="sxs-lookup"><span data-stu-id="ba348-233">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="ba348-234">Ponieważ jest sprawdzana autoryzacji zasobów `[Authorize]` atrybut nie jest wystarczająca.</span><span class="sxs-lookup"><span data-stu-id="ba348-234">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="ba348-235">Aplikacja nie ma dostępu do zasobu, gdy atrybuty są oceniane.</span><span class="sxs-lookup"><span data-stu-id="ba348-235">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="ba348-236">Musi być bezwzględnie autoryzacji na podstawie zasobów.</span><span class="sxs-lookup"><span data-stu-id="ba348-236">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="ba348-237">Kontroli należy wykonać, gdy aplikacja ma dostęp do zasobów przez załadowanie go w modelu strony lub przez załadowanie go w obrębie samego program obsługi.</span><span class="sxs-lookup"><span data-stu-id="ba348-237">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="ba348-238">Zasób jest często używany przez przekazywanie klucz zasobu.</span><span class="sxs-lookup"><span data-stu-id="ba348-238">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="ba348-239">Aktualizacja DeleteModel</span><span class="sxs-lookup"><span data-stu-id="ba348-239">Update the DeleteModel</span></span>

<span data-ttu-id="ba348-240">Aktualizacja modelu strony Usuń na potrzeby obsługi autoryzacji Sprawdź, czy użytkownik ma uprawnienia do usuwania na kontakt.</span><span class="sxs-lookup"><span data-stu-id="ba348-240">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="ba348-241">Wstaw usługi autoryzacji do widoków</span><span class="sxs-lookup"><span data-stu-id="ba348-241">Inject the authorization service into the views</span></span>

<span data-ttu-id="ba348-242">Obecnie pokazuje interfejsu użytkownika edytowania i usuwania łącza do danych użytkownika nie można zmodyfikować.</span><span class="sxs-lookup"><span data-stu-id="ba348-242">Currently, the UI shows edit and delete links for data the user can't modify.</span></span> <span data-ttu-id="ba348-243">Interfejs użytkownika jest rozwiązany przez zastosowanie obsługi autoryzacji do widoków.</span><span class="sxs-lookup"><span data-stu-id="ba348-243">The UI is fixed by applying the authorization handler to the views.</span></span>

<span data-ttu-id="ba348-244">Wstaw usługi autoryzacji w *Views/_ViewImports.cshtml* pliku, jest dostępny dla wszystkich widoków:</span><span class="sxs-lookup"><span data-stu-id="ba348-244">Inject the authorization service in the *Views/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[](secure-data/samples/final2/Pages/_ViewImports.cshtml?highlight=6-9)]

<span data-ttu-id="ba348-245">Poprzedni kod znaczników dodaje kilka `using` instrukcje.</span><span class="sxs-lookup"><span data-stu-id="ba348-245">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="ba348-246">Aktualizacja **Edytuj** i **usunąć** łączy w *Pages/Contacts/Index.cshtml* , są one tylko renderowania dla użytkowników z odpowiednimi uprawnieniami:</span><span class="sxs-lookup"><span data-stu-id="ba348-246">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[](secure-data/samples/final2/Pages/Contacts/Index.cshtml?highlight=34-36,64-999)]

> [!WARNING]
> <span data-ttu-id="ba348-247">Ukrywanie łącza od użytkowników, które nie mają uprawnień do zmiany danych nie zabezpieczenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ba348-247">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="ba348-248">Ukrywanie łączy powoduje, że bardziej przyjazny dla użytkownika aplikacji przez wyświetlanie łączy jedyne prawidłowe.</span><span class="sxs-lookup"><span data-stu-id="ba348-248">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="ba348-249">Użytkownicy mogą hack wygenerowanego adresy URL do wywołania edytowania i usuwania operacji na danych, które nie są właścicielami.</span><span class="sxs-lookup"><span data-stu-id="ba348-249">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="ba348-250">Razor strony lub kontrolera muszą wymuszać kontroli dostępu do zabezpieczania danych.</span><span class="sxs-lookup"><span data-stu-id="ba348-250">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="ba348-251">Szczegóły aktualizacji</span><span class="sxs-lookup"><span data-stu-id="ba348-251">Update Details</span></span>

<span data-ttu-id="ba348-252">Aktualizacja w widoku szczegółów, menedżerów można zatwierdzić lub odrzucić kontaktów:</span><span class="sxs-lookup"><span data-stu-id="ba348-252">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[](secure-data/samples/final2/Pages/Contacts/Details.cshtml?range=48-999)]

<span data-ttu-id="ba348-253">Aktualizacja modelu strony szczegółów:</span><span class="sxs-lookup"><span data-stu-id="ba348-253">Update the details page model:</span></span>

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="test-the-completed-app"></a><span data-ttu-id="ba348-254">Testowanie ukończonej aplikacji</span><span class="sxs-lookup"><span data-stu-id="ba348-254">Test the completed app</span></span>

<span data-ttu-id="ba348-255">Za pomocą programu Visual Studio Code lub testowania na platformie lokalnego, który nie zawiera certyfikatu testowego dla protokołu HTTPS:</span><span class="sxs-lookup"><span data-stu-id="ba348-255">If you're using Visual Studio Code or testing on a local platform that doesn't include a test certificate for HTTPS:</span></span>

* <span data-ttu-id="ba348-256">Ustaw `"LocalTest:skipSSL": true` w *appsettings. Developement.JSON* plik, aby pominąć wymaganie protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="ba348-256">Set `"LocalTest:skipSSL": true` in the *appsettings.Developement.json* file to skip the HTTPS requirement.</span></span> <span data-ttu-id="ba348-257">Pomiń HTTPS tylko na komputerze deweloperskim.</span><span class="sxs-lookup"><span data-stu-id="ba348-257">Skip HTTPS only on a development machine.</span></span>

<span data-ttu-id="ba348-258">Jeśli aplikacja ma kontaktów:</span><span class="sxs-lookup"><span data-stu-id="ba348-258">If the app has contacts:</span></span>

* <span data-ttu-id="ba348-259">Usuń wszystkie rekordy w `Contact` tabeli.</span><span class="sxs-lookup"><span data-stu-id="ba348-259">Delete all the records in the `Contact` table.</span></span>
* <span data-ttu-id="ba348-260">Ponowne uruchomienie aplikacji w celu umieszczenia bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ba348-260">Restart the app to seed the database.</span></span>

<span data-ttu-id="ba348-261">Zarejestruj użytkownika dla przeglądania kontaktów.</span><span class="sxs-lookup"><span data-stu-id="ba348-261">Register a user for browsing the contacts.</span></span>

<span data-ttu-id="ba348-262">Łatwe testowanie ukończonej aplikacji jest uruchamianie trzech różnych przeglądarkach (lub incognito/sesję InPrivate wersji).</span><span class="sxs-lookup"><span data-stu-id="ba348-262">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate versions).</span></span> <span data-ttu-id="ba348-263">W przeglądarce jednego zarejestrować nowego użytkownika (na przykład `test@example.com`).</span><span class="sxs-lookup"><span data-stu-id="ba348-263">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="ba348-264">Zaloguj się do każdej przeglądarki z innym użytkownikiem.</span><span class="sxs-lookup"><span data-stu-id="ba348-264">Sign in to each browser with a different user.</span></span> <span data-ttu-id="ba348-265">Sprawdź następujące operacje:</span><span class="sxs-lookup"><span data-stu-id="ba348-265">Verify the following operations:</span></span>

* <span data-ttu-id="ba348-266">Zarejestrowani użytkownicy mogą wyświetlać wszystkie zatwierdzone dane kontaktu.</span><span class="sxs-lookup"><span data-stu-id="ba348-266">Registered users can view all the approved contact data.</span></span>
* <span data-ttu-id="ba348-267">Zarejestrowani użytkownicy mogą edytowanie/usuwanie własnych danych.</span><span class="sxs-lookup"><span data-stu-id="ba348-267">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="ba348-268">Menedżerowie można zatwierdzić lub odrzucić dane kontaktowe.</span><span class="sxs-lookup"><span data-stu-id="ba348-268">Managers can approve or reject contact data.</span></span> <span data-ttu-id="ba348-269">`Details` Wyświetlić pokazuje **Zatwierdź** i **Odrzuć** przycisków.</span><span class="sxs-lookup"><span data-stu-id="ba348-269">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="ba348-270">Administratorzy mogą zatwierdzać Odrzuć i edytowanie/usuwanie żadnych danych.</span><span class="sxs-lookup"><span data-stu-id="ba348-270">Administrators can approve/reject and edit/delete any data.</span></span>

| <span data-ttu-id="ba348-271">Użytkownik</span><span class="sxs-lookup"><span data-stu-id="ba348-271">User</span></span>| <span data-ttu-id="ba348-272">Opcje</span><span class="sxs-lookup"><span data-stu-id="ba348-272">Options</span></span> |
| ------------ | ---------|
| test@example.com | <span data-ttu-id="ba348-273">Można edytowanie/usuwanie własnych danych</span><span class="sxs-lookup"><span data-stu-id="ba348-273">Can edit/delete own data</span></span> |
| manager@contoso.com | <span data-ttu-id="ba348-274">Można zatwierdzić Odrzuć i edytowanie i usuwanie własnych danych</span><span class="sxs-lookup"><span data-stu-id="ba348-274">Can approve/reject and edit/delete own data</span></span> |
| admin@contoso.com | <span data-ttu-id="ba348-275">Edytowanie i usuwanie można i Zatwierdź/Odrzuć wszystkie dane</span><span class="sxs-lookup"><span data-stu-id="ba348-275">Can edit/delete and approve/reject all data</span></span>|

<span data-ttu-id="ba348-276">Utworzenie kontaktu w przeglądarce administratora.</span><span class="sxs-lookup"><span data-stu-id="ba348-276">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="ba348-277">Skopiuj adres URL do usunięcia i Edytuj z skontaktowanie się z administratorem.</span><span class="sxs-lookup"><span data-stu-id="ba348-277">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="ba348-278">Wklej te linki do przeglądarki użytkownika testowego, aby sprawdzić, czy użytkownik testu nie może wykonywać te operacje.</span><span class="sxs-lookup"><span data-stu-id="ba348-278">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="ba348-279">Utworzyć początkową aplikację</span><span class="sxs-lookup"><span data-stu-id="ba348-279">Create the starter app</span></span>

* <span data-ttu-id="ba348-280">Tworzenie stron Razor aplikacji o nazwie "ContactManager"</span><span class="sxs-lookup"><span data-stu-id="ba348-280">Create a Razor Pages app named "ContactManager"</span></span>

  * <span data-ttu-id="ba348-281">Tworzenie aplikacji z **indywidualne konta użytkowników**.</span><span class="sxs-lookup"><span data-stu-id="ba348-281">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="ba348-282">Nadaj mu nazwę "ContactManager", przestrzeń nazw odpowiada przestrzeni nazw używany w próbce.</span><span class="sxs-lookup"><span data-stu-id="ba348-282">Name it "ContactManager" so your namespace matches the namespace used in the sample.</span></span>

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

  * <span data-ttu-id="ba348-284">`-uld` Określa LocalDB zamiast SQLite</span><span class="sxs-lookup"><span data-stu-id="ba348-284">`-uld` specifies LocalDB instead of SQLite</span></span>

* <span data-ttu-id="ba348-285">Dodaj następujące `Contact` modelu:</span><span class="sxs-lookup"><span data-stu-id="ba348-285">Add the following `Contact` model:</span></span>

  [!code-csharp[](secure-data/samples/starter2/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="ba348-286">Szkieletu `Contact` modelu:</span><span class="sxs-lookup"><span data-stu-id="ba348-286">Scaffold the `Contact` model:</span></span>

```console
dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
```

* <span data-ttu-id="ba348-287">Aktualizacja **ContactManager** zakotwiczenia w *Pages/_Layout.cshtml* pliku:</span><span class="sxs-lookup"><span data-stu-id="ba348-287">Update the **ContactManager** anchor in the *Pages/_Layout.cshtml* file:</span></span>

```cshtml
<a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
```

* <span data-ttu-id="ba348-288">Tworzenie szkieletu początkowej migracji i aktualizacji bazy danych:</span><span class="sxs-lookup"><span data-stu-id="ba348-288">Scaffold the initial migration and update the database:</span></span>

```console
dotnet ef migrations add initial
dotnet ef database update
```

* <span data-ttu-id="ba348-289">Testowanie aplikacji przez tworzenie, edytowanie i usuwanie kontaktu</span><span class="sxs-lookup"><span data-stu-id="ba348-289">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="ba348-290">Inicjatora bazy danych</span><span class="sxs-lookup"><span data-stu-id="ba348-290">Seed the database</span></span>

<span data-ttu-id="ba348-291">Dodaj `SeedData` klasy do *danych* folderu.</span><span class="sxs-lookup"><span data-stu-id="ba348-291">Add the `SeedData` class to the *Data* folder.</span></span> <span data-ttu-id="ba348-292">Jeśli pobrano próbki, możesz skopiować *SeedData.cs* pliku *danych* folderu projektu początkowego.</span><span class="sxs-lookup"><span data-stu-id="ba348-292">If you've downloaded the sample, you can copy the *SeedData.cs* file to the *Data* folder of the starter project.</span></span>

<span data-ttu-id="ba348-293">Wywołanie `SeedData.Initialize` z `Main`:</span><span class="sxs-lookup"><span data-stu-id="ba348-293">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[](secure-data/samples/starter2/Program.cs?name=snippet)]

<span data-ttu-id="ba348-294">Testowanie, czy aplikacja rozpoczęta bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ba348-294">Test that the app seeded the database.</span></span> <span data-ttu-id="ba348-295">Jeśli istnieją wszystkie wiersze w kontakcie DB, seed — metoda nie działa.</span><span class="sxs-lookup"><span data-stu-id="ba348-295">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a><span data-ttu-id="ba348-296">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="ba348-296">Additional resources</span></span>

* <span data-ttu-id="ba348-297">[Laboratorium autoryzacji platformy ASP.NET Core](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span><span class="sxs-lookup"><span data-stu-id="ba348-297">[ASP.NET Core Authorization Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span></span> <span data-ttu-id="ba348-298">W tym laboratorium przechodzi w stan więcej szczegółów na temat funkcji zabezpieczeń wprowadzone w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="ba348-298">This lab goes into more detail on the security features introduced in this tutorial.</span></span>
* [<span data-ttu-id="ba348-299">Autoryzacja w ASP.NET Core: prosty, rolę, opartej na oświadczeniach, a niestandardowe</span><span class="sxs-lookup"><span data-stu-id="ba348-299">Authorization in ASP.NET Core: Simple, role, claims-based, and custom</span></span>](xref:security/authorization/index)
* [<span data-ttu-id="ba348-300">Niestandardowe autoryzacji opartych na zasadach</span><span class="sxs-lookup"><span data-stu-id="ba348-300">Custom policy-based authorization</span></span>](xref:security/authorization/policies)
