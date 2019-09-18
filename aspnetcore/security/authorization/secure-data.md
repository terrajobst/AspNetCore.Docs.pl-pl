---
title: Tworzenie aplikacji platformy ASP.NET Core przy użyciu danych użytkownika chronionych przez autoryzację
author: rick-anderson
description: Dowiedz się, jak utworzyć aplikację stron Razor przy użyciu danych użytkownika chronionych przez autoryzację. Obejmuje protokołu HTTPS, uwierzytelniania, zabezpieczeń i tożsamości platformy ASP.NET Core.
ms.author: riande
ms.date: 12/18/2018
ms.custom: mvc, seodec18
uid: security/authorization/secure-data
ms.openlocfilehash: d827f6f839c9e42e6d3d7b04fe8b24a1c9732aee
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082443"
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a><span data-ttu-id="236fb-104">Tworzenie aplikacji platformy ASP.NET Core przy użyciu danych użytkownika chronionych przez autoryzację</span><span class="sxs-lookup"><span data-stu-id="236fb-104">Create an ASP.NET Core app with user data protected by authorization</span></span>

<span data-ttu-id="236fb-105">Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Audette Jan](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="236fb-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="236fb-106">Zobacz [ta](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) dla wersji platformy ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="236fb-106">See [this PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC version.</span></span> <span data-ttu-id="236fb-107">Wersja platformy ASP.NET Core 1.1 po ukończeniu tego samouczka jest [to](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data) folderu.</span><span class="sxs-lookup"><span data-stu-id="236fb-107">The ASP.NET Core 1.1 version of this tutorial is in [this](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data) folder.</span></span> <span data-ttu-id="236fb-108">1\.1, na przykład platforma ASP.NET Core jest w [przykłady](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span><span class="sxs-lookup"><span data-stu-id="236fb-108">The 1.1 ASP.NET Core sample is in the [samples](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="236fb-109">Zobacz [ten plik pdf](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_July16_18.pdf)</span><span class="sxs-lookup"><span data-stu-id="236fb-109">See [this pdf](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_July16_18.pdf)</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="236fb-110">W tym samouczku pokazano, jak utworzyć aplikację sieci web platformy ASP.NET Core przy użyciu danych użytkownika chronionych przez autoryzację.</span><span class="sxs-lookup"><span data-stu-id="236fb-110">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="236fb-111">Wyświetla listę kontaktów, uwierzytelnionych użytkowników (zarejestrowane), które zostały utworzone.</span><span class="sxs-lookup"><span data-stu-id="236fb-111">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="236fb-112">Istnieją trzy grupy zabezpieczeń:</span><span class="sxs-lookup"><span data-stu-id="236fb-112">There are three security groups:</span></span>

* <span data-ttu-id="236fb-113">**Liczba zarejestrowanych użytkowników** może wyświetlać wszystkie zatwierdzone dane i może edytować/usuwać swoje dane.</span><span class="sxs-lookup"><span data-stu-id="236fb-113">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="236fb-114">**Menedżerowie** można zatwierdzić lub odrzucić dane kontaktowe.</span><span class="sxs-lookup"><span data-stu-id="236fb-114">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="236fb-115">Tylko zatwierdzone kontakty będą widoczne dla użytkowników.</span><span class="sxs-lookup"><span data-stu-id="236fb-115">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="236fb-116">**Administratorzy** można zatwierdzić/Odrzuć i edytowanie/usuwanie żadnych danych.</span><span class="sxs-lookup"><span data-stu-id="236fb-116">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="236fb-117">Obrazy w tym dokumencie nie są dokładnie zgodne z najnowszymi szablonami.</span><span class="sxs-lookup"><span data-stu-id="236fb-117">The images in this document don't exactly match the latest templates.</span></span>

<span data-ttu-id="236fb-118">Na poniższej ilustracji użytkownik Rick (`rick@example.com`) jest zalogowany.</span><span class="sxs-lookup"><span data-stu-id="236fb-118">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="236fb-119">Rick mogą jedynie wyświetlać kontakty zatwierdzone i **Edytuj**/**Usuń**/**Utwórz nowy** linki do swoich kontaktów.</span><span class="sxs-lookup"><span data-stu-id="236fb-119">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="236fb-120">Tylko ostatni rekord, utworzone przez Rick, wyświetla **Edytuj** i **Usuń** łącza.</span><span class="sxs-lookup"><span data-stu-id="236fb-120">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="236fb-121">Inni użytkownicy nie zobaczą ostatni rekord, aż Menedżer lub administrator zmienia stan na "Zatwierdzone".</span><span class="sxs-lookup"><span data-stu-id="236fb-121">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![Zrzut ekranu przedstawiający Rick zalogowany](secure-data/_static/rick.png)

<span data-ttu-id="236fb-123">Na poniższej ilustracji `manager@contoso.com` jest zalogowany i w roli menedżera:</span><span class="sxs-lookup"><span data-stu-id="236fb-123">In the following image, `manager@contoso.com` is signed in and in the manager's role:</span></span>

![Zrzut ekranu przedstawiający manager@contoso.com zalogowany](secure-data/_static/manager1.png)

<span data-ttu-id="236fb-125">Na poniższej ilustracji przedstawiono menedżerów widoku szczegółów kontaktu:</span><span class="sxs-lookup"><span data-stu-id="236fb-125">The following image shows the managers details view of a contact:</span></span>

![Widok menedżera kontaktu](secure-data/_static/manager.png)

<span data-ttu-id="236fb-127">**Zatwierdź** i **Odrzuć** przyciski są wyświetlane tylko dla menedżerów i administratorów.</span><span class="sxs-lookup"><span data-stu-id="236fb-127">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="236fb-128">Na poniższej ilustracji `admin@contoso.com` jest zalogowany i w roli administratora:</span><span class="sxs-lookup"><span data-stu-id="236fb-128">In the following image, `admin@contoso.com` is signed in and in the administrator's role:</span></span>

![Zrzut ekranu przedstawiający admin@contoso.com zalogowany](secure-data/_static/admin.png)

<span data-ttu-id="236fb-130">Administrator ma wszystkie uprawnienia.</span><span class="sxs-lookup"><span data-stu-id="236fb-130">The administrator has all privileges.</span></span> <span data-ttu-id="236fb-131">Ona można odczytu/edytowanie/usuwanie dowolnego skontaktuj się z pomocą i zmienić stan kontaktów.</span><span class="sxs-lookup"><span data-stu-id="236fb-131">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="236fb-132">Aplikacja została utworzona przez [tworzenia szkieletów](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) następujące `Contact` modelu:</span><span class="sxs-lookup"><span data-stu-id="236fb-132">The app was created by [scaffolding](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) the following `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

<span data-ttu-id="236fb-133">Przykład zawiera poniższe obsługi autoryzacji:</span><span class="sxs-lookup"><span data-stu-id="236fb-133">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="236fb-134">`ContactIsOwnerAuthorizationHandler`: Zapewnia, że użytkownik może edytować tylko swoje dane.</span><span class="sxs-lookup"><span data-stu-id="236fb-134">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="236fb-135">`ContactManagerAuthorizationHandler`: Umożliwia menedżerom zatwierdzanie lub odrzucanie kontaktów.</span><span class="sxs-lookup"><span data-stu-id="236fb-135">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="236fb-136">`ContactAdministratorsAuthorizationHandler`: Umożliwia administratorom zatwierdzanie lub odrzucanie kontaktów oraz Edytowanie/usuwanie kontaktów.</span><span class="sxs-lookup"><span data-stu-id="236fb-136">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="236fb-137">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="236fb-137">Prerequisites</span></span>

<span data-ttu-id="236fb-138">W tym samouczku jest zaawansowany.</span><span class="sxs-lookup"><span data-stu-id="236fb-138">This tutorial is advanced.</span></span> <span data-ttu-id="236fb-139">Należy zapoznać się z:</span><span class="sxs-lookup"><span data-stu-id="236fb-139">You should be familiar with:</span></span>

* [<span data-ttu-id="236fb-140">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="236fb-140">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="236fb-141">Uwierzytelnianie</span><span class="sxs-lookup"><span data-stu-id="236fb-141">Authentication</span></span>](xref:security/authentication/identity)
* [<span data-ttu-id="236fb-142">Potwierdzenie konta i odzyskiwanie hasła</span><span class="sxs-lookup"><span data-stu-id="236fb-142">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="236fb-143">Autoryzacja</span><span class="sxs-lookup"><span data-stu-id="236fb-143">Authorization</span></span>](xref:security/authorization/introduction)
* [<span data-ttu-id="236fb-144">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="236fb-144">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="236fb-145">Starter i ukończonej aplikacji</span><span class="sxs-lookup"><span data-stu-id="236fb-145">The starter and completed app</span></span>

<span data-ttu-id="236fb-146">[Pobierz](xref:index#how-to-download-a-sample) [ukończone](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) aplikacji.</span><span class="sxs-lookup"><span data-stu-id="236fb-146">[Download](xref:index#how-to-download-a-sample) the [completed](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) app.</span></span> <span data-ttu-id="236fb-147">[Test](#test-the-completed-app) ukończonej aplikacji, dzięki czemu można zapoznać się z jej funkcjami zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="236fb-147">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="236fb-148">Aplikację startową</span><span class="sxs-lookup"><span data-stu-id="236fb-148">The starter app</span></span>

<span data-ttu-id="236fb-149">[Pobierz](xref:index#how-to-download-a-sample) [starter](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) aplikacji.</span><span class="sxs-lookup"><span data-stu-id="236fb-149">[Download](xref:index#how-to-download-a-sample) the [starter](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) app.</span></span>

<span data-ttu-id="236fb-150">Uruchom aplikację, naciśnij przycisk **ContactManager** link i sprawdź, tworzenie, edytowanie i usuwanie kontaktu.</span><span class="sxs-lookup"><span data-stu-id="236fb-150">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="236fb-151">Zabezpieczanie danych użytkownika</span><span class="sxs-lookup"><span data-stu-id="236fb-151">Secure user data</span></span>

<span data-ttu-id="236fb-152">Poniższe sekcje mają główne kroki umożliwiające utworzenie aplikacji danych bezpiecznego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="236fb-152">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="236fb-153">Może okazać się przydatne do odwoływania się do projektu ukończona.</span><span class="sxs-lookup"><span data-stu-id="236fb-153">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="236fb-154">Powiąż dane kontaktowe dla użytkownika</span><span class="sxs-lookup"><span data-stu-id="236fb-154">Tie the contact data to the user</span></span>

<span data-ttu-id="236fb-155">Za pomocą programu ASP.NET [tożsamości](xref:security/authentication/identity) identyfikator użytkownika w celu zapewnienia, że użytkownicy mogą edytować swoje dane, ale nie dane innych użytkowników.</span><span class="sxs-lookup"><span data-stu-id="236fb-155">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="236fb-156">Dodaj `OwnerID` i `ContactStatus` do `Contact` modelu:</span><span class="sxs-lookup"><span data-stu-id="236fb-156">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/final3/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

<span data-ttu-id="236fb-157">`OwnerID` Identyfikator użytkownika z `AspNetUser` tabelę [tożsamości](xref:security/authentication/identity) bazy danych.</span><span class="sxs-lookup"><span data-stu-id="236fb-157">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="236fb-158">`Status` Pola określa, czy kontakt jest widoczny dla zwykłych użytkowników.</span><span class="sxs-lookup"><span data-stu-id="236fb-158">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="236fb-159">Tworzenie nowej migracji i aktualizowanie bazy danych:</span><span class="sxs-lookup"><span data-stu-id="236fb-159">Create a new migration and update the database:</span></span>

```dotnetcli
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-identity"></a><span data-ttu-id="236fb-160">Dodaj usługi ról do tożsamości</span><span class="sxs-lookup"><span data-stu-id="236fb-160">Add Role services to Identity</span></span>

<span data-ttu-id="236fb-161">Dołącz [opcji Dodawanie ról](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) można dodać usługi ról:</span><span class="sxs-lookup"><span data-stu-id="236fb-161">Append [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) to add Role services:</span></span>

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet2&highlight=9)]

### <a name="require-authenticated-users"></a><span data-ttu-id="236fb-162">Wymaga uwierzytelnionych użytkowników</span><span class="sxs-lookup"><span data-stu-id="236fb-162">Require authenticated users</span></span>

<span data-ttu-id="236fb-163">Ustaw domyślne zasady uwierzytelniania, aby wymagać od użytkowników uwierzytelniania:</span><span class="sxs-lookup"><span data-stu-id="236fb-163">Set the default authentication policy to require users to be authenticated:</span></span>

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet&highlight=15-99)] 

 <span data-ttu-id="236fb-164">Użytkownik może zrezygnować z uwierzytelniania na poziomie metody strony Razor, kontrolera lub akcji z `[AllowAnonymous]` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="236fb-164">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="236fb-165">Ustawienie domyślne zasady uwierzytelniania, aby wymagać od użytkowników uwierzytelniania chroni nowo dodanych stronami Razor i kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="236fb-165">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="236fb-166">Posiadanie uwierzytelniania domyślnie wymagane jest bezpieczniejszy niż opierając się na nowych kontrolerów i stron Razor do uwzględnienia `[Authorize]` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="236fb-166">Having authentication required by default is more secure than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span>

<span data-ttu-id="236fb-167">Dodaj [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) do stron indeksu i prywatności, aby użytkownicy anonimowi mogli uzyskać informacje o witrynie przed ich zarejestrowaniem.</span><span class="sxs-lookup"><span data-stu-id="236fb-167">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index and Privacy pages so anonymous users can get information about the site before they register.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Index.cshtml.cs?highlight=1,7)]

### <a name="configure-the-test-account"></a><span data-ttu-id="236fb-168">Skonfiguruj konto testu</span><span class="sxs-lookup"><span data-stu-id="236fb-168">Configure the test account</span></span>

<span data-ttu-id="236fb-169">`SeedData` Klasy tworzy dwa konta: administrator i Menedżer.</span><span class="sxs-lookup"><span data-stu-id="236fb-169">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="236fb-170">Użyj [narzędzie Menedżer klucz tajny](xref:security/app-secrets) ustawić hasło dla tych kont.</span><span class="sxs-lookup"><span data-stu-id="236fb-170">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="236fb-171">Ustaw hasło z katalogu projektu (katalogu zawierającego *Program.cs*):</span><span class="sxs-lookup"><span data-stu-id="236fb-171">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```dotnetcli
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="236fb-172">Jeśli nie określono silne hasło, wyjątek jest generowany, gdy `SeedData.Initialize` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="236fb-172">If a strong password is not specified, an exception is thrown when `SeedData.Initialize` is called.</span></span>

<span data-ttu-id="236fb-173">Aktualizacja `Main` hasła testu:</span><span class="sxs-lookup"><span data-stu-id="236fb-173">Update `Main` to use the test password:</span></span>

[!code-csharp[](secure-data/samples/final3/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="236fb-174">Tworzenie konta testowe i aktualizowanie kontaktów</span><span class="sxs-lookup"><span data-stu-id="236fb-174">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="236fb-175">Aktualizacja `Initialize` method in Class metoda `SeedData` klasy w celu utworzenia konta testowe:</span><span class="sxs-lookup"><span data-stu-id="236fb-175">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[](secure-data/samples/final3/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="236fb-176">Dodaj identyfikator użytkownika administratora i `ContactStatus` do kontaktów.</span><span class="sxs-lookup"><span data-stu-id="236fb-176">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="236fb-177">Określ jeden z kontaktów "Przesłane" i jednym "odrzucone".</span><span class="sxs-lookup"><span data-stu-id="236fb-177">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="236fb-178">Dodawanie stanu i Identyfikatora użytkownika do wszystkich kontaktów.</span><span class="sxs-lookup"><span data-stu-id="236fb-178">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="236fb-179">Wyświetlane jest tylko jeden kontakt:</span><span class="sxs-lookup"><span data-stu-id="236fb-179">Only one contact is shown:</span></span>

[!code-csharp[](secure-data/samples/final3/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="236fb-180">Utwórz właściciela, Menedżer i obsługi autoryzacji administratora</span><span class="sxs-lookup"><span data-stu-id="236fb-180">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="236fb-181">Tworzenie `ContactIsOwnerAuthorizationHandler` klasy w *autoryzacji* folderu.</span><span class="sxs-lookup"><span data-stu-id="236fb-181">Create a `ContactIsOwnerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="236fb-182">`ContactIsOwnerAuthorizationHandler` Sprawdza, czy użytkownik, działając w zasobie jest właścicielem zasobu.</span><span class="sxs-lookup"><span data-stu-id="236fb-182">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="236fb-183">`ContactIsOwnerAuthorizationHandler` Wywołania [kontekstu. Powodzenie](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) aktualnego użytkownika uwierzytelnionego jest skontaktuj się z właścicielem.</span><span class="sxs-lookup"><span data-stu-id="236fb-183">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="236fb-184">Programy obsługi autoryzacji zazwyczaj:</span><span class="sxs-lookup"><span data-stu-id="236fb-184">Authorization handlers generally:</span></span>

* <span data-ttu-id="236fb-185">Zwróć `context.Succeed` gdy spełniono wymagania.</span><span class="sxs-lookup"><span data-stu-id="236fb-185">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="236fb-186">Zwróć `Task.CompletedTask` gdy nie są spełnione wymagania.</span><span class="sxs-lookup"><span data-stu-id="236fb-186">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="236fb-187">`Task.CompletedTask`nie powiodło się lub&mdash;niepowodzenie — zezwala na uruchamianie innych programów obsługi autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="236fb-187">`Task.CompletedTask` is not success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="236fb-188">Jeśli potrzebujesz jawnie nie powiedzie się, zwraca [kontekstu. Niepowodzenie](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span><span class="sxs-lookup"><span data-stu-id="236fb-188">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="236fb-189">Aplikacja umożliwia skontaktuj się z pomocą właścicieli do edycji/delete/tworzenie własnych danych.</span><span class="sxs-lookup"><span data-stu-id="236fb-189">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="236fb-190">`ContactIsOwnerAuthorizationHandler` nie muszą sprawdzać operacji przekazane w parametrze wymagań.</span><span class="sxs-lookup"><span data-stu-id="236fb-190">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="236fb-191">Utwórz procedurę obsługi Menedżer autoryzacji</span><span class="sxs-lookup"><span data-stu-id="236fb-191">Create a manager authorization handler</span></span>

<span data-ttu-id="236fb-192">Tworzenie `ContactManagerAuthorizationHandler` klasy w *autoryzacji* folderu.</span><span class="sxs-lookup"><span data-stu-id="236fb-192">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="236fb-193">`ContactManagerAuthorizationHandler` Weryfikuje użytkownika, działając w zasobie menedżera.</span><span class="sxs-lookup"><span data-stu-id="236fb-193">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="236fb-194">Tylko menedżerowie mogli zatwierdzać lub odrzucać zmiany zawartości (nowe lub zmienione).</span><span class="sxs-lookup"><span data-stu-id="236fb-194">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="236fb-195">Utwórz procedurę obsługi autoryzacji administratora</span><span class="sxs-lookup"><span data-stu-id="236fb-195">Create an administrator authorization handler</span></span>

<span data-ttu-id="236fb-196">Tworzenie `ContactAdministratorsAuthorizationHandler` klasy w *autoryzacji* folderu.</span><span class="sxs-lookup"><span data-stu-id="236fb-196">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="236fb-197">`ContactAdministratorsAuthorizationHandler` Weryfikuje użytkownika na zasób jest administratorem.</span><span class="sxs-lookup"><span data-stu-id="236fb-197">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="236fb-198">Administrator może wykonać wszystkie operacje.</span><span class="sxs-lookup"><span data-stu-id="236fb-198">Administrator can do all operations.</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="236fb-199">Zarejestruj procedury obsługi autoryzacji</span><span class="sxs-lookup"><span data-stu-id="236fb-199">Register the authorization handlers</span></span>

<span data-ttu-id="236fb-200">Usługi za pomocą platformy Entity Framework Core musi być zarejestrowana do [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) przy użyciu [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span><span class="sxs-lookup"><span data-stu-id="236fb-200">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="236fb-201">`ContactIsOwnerAuthorizationHandler` Korzysta z platformy ASP.NET Core [tożsamości](xref:security/authentication/identity), która jest oparta na platformy Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="236fb-201">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="236fb-202">Zarejestruj procedury obsługi za pomocą kolekcji usługi, dzięki czemu są one dostępne do `ContactsController` za pośrednictwem [wstrzykiwanie zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="236fb-202">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="236fb-203">Dodaj następujący kod na końcu `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="236fb-203">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet_defaultPolicy&highlight=23-99)]

<span data-ttu-id="236fb-204">`ContactAdministratorsAuthorizationHandler` i `ContactManagerAuthorizationHandler` są dodawane jako pojedynczych wystąpień.</span><span class="sxs-lookup"><span data-stu-id="236fb-204">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="236fb-205">Są one pojedynczych wystąpień, ponieważ nie używają programu EF, a wszystkie informacje wymagane w `Context` parametru `HandleRequirementAsync` metody.</span><span class="sxs-lookup"><span data-stu-id="236fb-205">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="236fb-206">Obsługuje autoryzację</span><span class="sxs-lookup"><span data-stu-id="236fb-206">Support authorization</span></span>

<span data-ttu-id="236fb-207">W tej sekcji służy do aktualizacji stron Razor i Dodaj klasę wymagania dotyczące operacji.</span><span class="sxs-lookup"><span data-stu-id="236fb-207">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="236fb-208">Przegląd klasy wymagania dotyczące operacji kontaktu</span><span class="sxs-lookup"><span data-stu-id="236fb-208">Review the contact operations requirements class</span></span>

<span data-ttu-id="236fb-209">Przegląd `ContactOperations` klasy.</span><span class="sxs-lookup"><span data-stu-id="236fb-209">Review the `ContactOperations` class.</span></span> <span data-ttu-id="236fb-210">Ta klasa zawiera wymagania obsługiwanej przez aplikację:</span><span class="sxs-lookup"><span data-stu-id="236fb-210">This class contains the requirements the app supports:</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-razor-pages"></a><span data-ttu-id="236fb-211">Utwórz klasę bazową dla stron Razor kontaktów</span><span class="sxs-lookup"><span data-stu-id="236fb-211">Create a base class for the Contacts Razor Pages</span></span>

<span data-ttu-id="236fb-212">Utwórz klasę bazową, zawierający usług wykorzystanych w kontaktach stron Razor.</span><span class="sxs-lookup"><span data-stu-id="236fb-212">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="236fb-213">Klasa bazowa umieszcza kod inicjowania w jednej lokalizacji:</span><span class="sxs-lookup"><span data-stu-id="236fb-213">The base class puts the initialization code in one location:</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="236fb-214">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="236fb-214">The preceding code:</span></span>

* <span data-ttu-id="236fb-215">Dodaje `IAuthorizationService` usługi w celu uzyskania dostępu do obsługi autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="236fb-215">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="236fb-216">Dodaje tożsamości `UserManager` usługi.</span><span class="sxs-lookup"><span data-stu-id="236fb-216">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="236fb-217">Dodaj `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="236fb-217">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="236fb-218">Aktualizacja CreateModel</span><span class="sxs-lookup"><span data-stu-id="236fb-218">Update the CreateModel</span></span>

<span data-ttu-id="236fb-219">Aktualizuj Konstruktor Tworzenie strony modelu do użycia `DI_BasePageModel` klasa bazowa:</span><span class="sxs-lookup"><span data-stu-id="236fb-219">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="236fb-220">Aktualizacja `CreateModel.OnPostAsync` metody:</span><span class="sxs-lookup"><span data-stu-id="236fb-220">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="236fb-221">Dodaj identyfikator użytkownika, który `Contact` modelu.</span><span class="sxs-lookup"><span data-stu-id="236fb-221">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="236fb-222">Wywołania obsługi autoryzacji, aby sprawdzić, czy użytkownik ma uprawnienia do tworzenia kontaktów.</span><span class="sxs-lookup"><span data-stu-id="236fb-222">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="236fb-223">Aktualizacja IndexModel</span><span class="sxs-lookup"><span data-stu-id="236fb-223">Update the IndexModel</span></span>

<span data-ttu-id="236fb-224">Aktualizacja `OnGetAsync` metody, więc tylko zatwierdzonego kontakty są wyświetlane użytkownikom ogólne:</span><span class="sxs-lookup"><span data-stu-id="236fb-224">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="236fb-225">Aktualizacja EditModel</span><span class="sxs-lookup"><span data-stu-id="236fb-225">Update the EditModel</span></span>

<span data-ttu-id="236fb-226">Dodaj program obsługi autoryzacji, aby sprawdzić, czy użytkownik jest właścicielem kontaktu.</span><span class="sxs-lookup"><span data-stu-id="236fb-226">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="236fb-227">Ponieważ autoryzacji zasobów jest weryfikowany, `[Authorize]` atrybut nie jest wystarczająca.</span><span class="sxs-lookup"><span data-stu-id="236fb-227">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="236fb-228">Aplikacja nie ma dostępu do zasobu, gdy atrybuty są oceniane.</span><span class="sxs-lookup"><span data-stu-id="236fb-228">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="236fb-229">Autoryzacja na podstawie zasobów musi być imperatywnego.</span><span class="sxs-lookup"><span data-stu-id="236fb-229">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="236fb-230">Testy muszą być wykonywane, gdy aplikacja ma dostęp do zasobu przez załadowanie go w modelu strony lub przez załadowanie go w ramach programu obsługi, sam.</span><span class="sxs-lookup"><span data-stu-id="236fb-230">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="236fb-231">Często dostęp do zasobu, przekazując klucz zasobu.</span><span class="sxs-lookup"><span data-stu-id="236fb-231">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="236fb-232">Aktualizacja DeleteModel</span><span class="sxs-lookup"><span data-stu-id="236fb-232">Update the DeleteModel</span></span>

<span data-ttu-id="236fb-233">Aktualizowanie modelu strony delete na potrzeby obsługi autoryzacji upewnij się, że użytkownik ma odpowiednie uprawnienia do usuwania dla kontaktu.</span><span class="sxs-lookup"><span data-stu-id="236fb-233">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="236fb-234">Wstrzyknięcie usługi autoryzacji do widoków</span><span class="sxs-lookup"><span data-stu-id="236fb-234">Inject the authorization service into the views</span></span>

<span data-ttu-id="236fb-235">Obecnie pokazuje interfejsu użytkownika edytowania i usuwania łącza kontaktów, do których użytkownik nie może modyfikować.</span><span class="sxs-lookup"><span data-stu-id="236fb-235">Currently, the UI shows edit and delete links for contacts the user can't modify.</span></span>

<span data-ttu-id="236fb-236">Wsuń usługę autoryzacji w pliku *Pages/_ViewImports. cshtml* , aby była dostępna dla wszystkich widoków:</span><span class="sxs-lookup"><span data-stu-id="236fb-236">Inject the authorization service in the *Pages/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[](secure-data/samples/final3/Pages/_ViewImports.cshtml?highlight=6-99)]

<span data-ttu-id="236fb-237">Poprzedni kod znaczników umożliwia dodanie kilku `using` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="236fb-237">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="236fb-238">Aktualizacja **Edytuj** i **Usuń** linki w *Pages/Contacts/Index.cshtml* więc tylko są one renderowane dla użytkowników z odpowiednimi uprawnieniami:</span><span class="sxs-lookup"><span data-stu-id="236fb-238">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[](secure-data/samples/final3/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> <span data-ttu-id="236fb-239">Ukrywanie łączy na podstawie użytkowników, którzy nie mają uprawnień do zmiany danych nie zabezpieczenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="236fb-239">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="236fb-240">Ukrywanie łącza sprawia, że bardziej przyjazny dla użytkownika aplikacji, wyświetlając tylko poprawne linki.</span><span class="sxs-lookup"><span data-stu-id="236fb-240">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="236fb-241">Użytkownicy mogą hack wygenerowanego adresy URL, aby wywołać Edytuj i Usuń operacje na danych, które nie są właścicielami.</span><span class="sxs-lookup"><span data-stu-id="236fb-241">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="236fb-242">Strona Razor lub kontrolera musi wymuszają operacje sprawdzania dostępu do zabezpieczania danych.</span><span class="sxs-lookup"><span data-stu-id="236fb-242">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="236fb-243">Szczegóły aktualizacji</span><span class="sxs-lookup"><span data-stu-id="236fb-243">Update Details</span></span>

<span data-ttu-id="236fb-244">Zaktualizuj widok szczegółów, aby menedżerowie mogli zatwierdzać lub odrzucać kontaktów:</span><span class="sxs-lookup"><span data-stu-id="236fb-244">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml?name=snippet)]

<span data-ttu-id="236fb-245">Aktualizowanie modelu strony szczegółów:</span><span class="sxs-lookup"><span data-stu-id="236fb-245">Update the details page model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="add-or-remove-a-user-to-a-role"></a><span data-ttu-id="236fb-246">Dodawanie lub usuwanie użytkownika do roli</span><span class="sxs-lookup"><span data-stu-id="236fb-246">Add or remove a user to a role</span></span>

<span data-ttu-id="236fb-247">Zobacz [ten problem](https://github.com/aspnet/AspNetCore.Docs/issues/8502) uzyskać informacji na temat:</span><span class="sxs-lookup"><span data-stu-id="236fb-247">See [this issue](https://github.com/aspnet/AspNetCore.Docs/issues/8502) for information on:</span></span>

* <span data-ttu-id="236fb-248">Usuwanie uprawnień z użytkownikiem.</span><span class="sxs-lookup"><span data-stu-id="236fb-248">Removing privileges from a user.</span></span> <span data-ttu-id="236fb-249">Na przykład wyciszenie użytkownika w aplikacji czatu.</span><span class="sxs-lookup"><span data-stu-id="236fb-249">For example, muting a user in a chat app.</span></span>
* <span data-ttu-id="236fb-250">Dodawanie uprawnień dla użytkownika.</span><span class="sxs-lookup"><span data-stu-id="236fb-250">Adding privileges to a user.</span></span>

## <a name="test-the-completed-app"></a><span data-ttu-id="236fb-251">Testowanie aplikacji ukończone</span><span class="sxs-lookup"><span data-stu-id="236fb-251">Test the completed app</span></span>

<span data-ttu-id="236fb-252">Jeśli nie został jeszcze ustawiony hasła dla kont użytkowników wypełnionych, użyj [narzędzie Menedżer klucz tajny](xref:security/app-secrets#secret-manager) ustawić hasło:</span><span class="sxs-lookup"><span data-stu-id="236fb-252">If you haven't already set a password for seeded user accounts, use the [Secret Manager tool](xref:security/app-secrets#secret-manager) to set a password:</span></span>

* <span data-ttu-id="236fb-253">Wybierz silne hasło: Użyj ośmiu lub więcej znaków i co najmniej jednego znaku wielkie litery, cyfry i symbolu.</span><span class="sxs-lookup"><span data-stu-id="236fb-253">Choose a strong password: Use eight or more characters and at least one upper-case character, number, and symbol.</span></span> <span data-ttu-id="236fb-254">Na przykład `Passw0rd!` spełnia wymagania silne hasło.</span><span class="sxs-lookup"><span data-stu-id="236fb-254">For example, `Passw0rd!` meets the strong password requirements.</span></span>
* <span data-ttu-id="236fb-255">Wykonaj następujące polecenie z folderu projektu, gdzie `<PW>` jest hasłem:</span><span class="sxs-lookup"><span data-stu-id="236fb-255">Execute the following command from the project's folder, where `<PW>` is the password:</span></span>

  ```dotnetcli
  dotnet user-secrets set SeedUserPW <PW>
  ```

<span data-ttu-id="236fb-256">Jeśli aplikacja ma kontaktów:</span><span class="sxs-lookup"><span data-stu-id="236fb-256">If the app has contacts:</span></span>

* <span data-ttu-id="236fb-257">Usuń wszystkie rekordy w `Contact` tabeli.</span><span class="sxs-lookup"><span data-stu-id="236fb-257">Delete all of the records in the `Contact` table.</span></span>
* <span data-ttu-id="236fb-258">Ponowne uruchomienie aplikacji w celu umieszczenia bazy danych.</span><span class="sxs-lookup"><span data-stu-id="236fb-258">Restart the app to seed the database.</span></span>

<span data-ttu-id="236fb-259">Łatwe testowanie ukończonej aplikacji jest do uruchomienia w trzech różnych przeglądarek (lub sesji incognito/InPrivate).</span><span class="sxs-lookup"><span data-stu-id="236fb-259">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate sessions).</span></span> <span data-ttu-id="236fb-260">W jednej przeglądarki zarejestrować nowego użytkownika (na przykład `test@example.com`).</span><span class="sxs-lookup"><span data-stu-id="236fb-260">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="236fb-261">Zaloguj się w każdej przeglądarce z innym użytkownikiem.</span><span class="sxs-lookup"><span data-stu-id="236fb-261">Sign in to each browser with a different user.</span></span> <span data-ttu-id="236fb-262">Sprawdź następujące operacje:</span><span class="sxs-lookup"><span data-stu-id="236fb-262">Verify the following operations:</span></span>

* <span data-ttu-id="236fb-263">Zarejestrowani użytkownicy można wyświetlić wszystkie zatwierdzone dane kontaktowe.</span><span class="sxs-lookup"><span data-stu-id="236fb-263">Registered users can view all of the approved contact data.</span></span>
* <span data-ttu-id="236fb-264">Zarejestrowani użytkownicy może edytować/usuwać swoje dane.</span><span class="sxs-lookup"><span data-stu-id="236fb-264">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="236fb-265">Menedżerowie mogą zatwierdzać/Odrzuć dane kontaktowe.</span><span class="sxs-lookup"><span data-stu-id="236fb-265">Managers can approve/reject contact data.</span></span> <span data-ttu-id="236fb-266">`Details` Wyświetlić pokazuje **Zatwierdź** i **Odrzuć** przycisków.</span><span class="sxs-lookup"><span data-stu-id="236fb-266">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="236fb-267">Administratorzy mogą zatwierdzać/Odrzuć i edytowanie/usuwanie wszystkich danych.</span><span class="sxs-lookup"><span data-stu-id="236fb-267">Administrators can approve/reject and edit/delete all data.</span></span>

| <span data-ttu-id="236fb-268">Użytkownik</span><span class="sxs-lookup"><span data-stu-id="236fb-268">User</span></span>                | <span data-ttu-id="236fb-269">Zasilany przez aplikację</span><span class="sxs-lookup"><span data-stu-id="236fb-269">Seeded by the app</span></span> | <span data-ttu-id="236fb-270">Opcje</span><span class="sxs-lookup"><span data-stu-id="236fb-270">Options</span></span>                                  |
| ------------------- | :---------------: | ---------------------------------------- |
| test@example.com    | <span data-ttu-id="236fb-271">Nie</span><span class="sxs-lookup"><span data-stu-id="236fb-271">No</span></span>                | <span data-ttu-id="236fb-272">Edytowanie/usuwanie własnych danych.</span><span class="sxs-lookup"><span data-stu-id="236fb-272">Edit/delete the own data.</span></span>                |
| manager@contoso.com | <span data-ttu-id="236fb-273">Tak</span><span class="sxs-lookup"><span data-stu-id="236fb-273">Yes</span></span>               | <span data-ttu-id="236fb-274">Zatwierdź/Odrzuć i edytowanie/usuwanie własnych danych.</span><span class="sxs-lookup"><span data-stu-id="236fb-274">Approve/reject and edit/delete own data.</span></span> |
| admin@contoso.com   | <span data-ttu-id="236fb-275">Tak</span><span class="sxs-lookup"><span data-stu-id="236fb-275">Yes</span></span>               | <span data-ttu-id="236fb-276">Zatwierdź/Odrzuć i edytowanie/usuwanie wszystkich danych.</span><span class="sxs-lookup"><span data-stu-id="236fb-276">Approve/reject and edit/delete all data.</span></span> |

<span data-ttu-id="236fb-277">Utwórz kontakt w przeglądarce administratora.</span><span class="sxs-lookup"><span data-stu-id="236fb-277">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="236fb-278">Skopiuj adres URL do usunięcia, a następnie Edytuj z skontaktowanie się z administratorem.</span><span class="sxs-lookup"><span data-stu-id="236fb-278">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="236fb-279">Wklej te linki do przeglądarki użytkownika testowego, aby sprawdzić, czy użytkownik testu nie może wykonywać te operacje.</span><span class="sxs-lookup"><span data-stu-id="236fb-279">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="236fb-280">Utwórz aplikację startową</span><span class="sxs-lookup"><span data-stu-id="236fb-280">Create the starter app</span></span>

* <span data-ttu-id="236fb-281">Tworzenie stron Razor aplikacji o nazwie "ContactManager"</span><span class="sxs-lookup"><span data-stu-id="236fb-281">Create a Razor Pages app named "ContactManager"</span></span>
  * <span data-ttu-id="236fb-282">Tworzenie aplikacji za pomocą **indywidualne konta użytkowników**.</span><span class="sxs-lookup"><span data-stu-id="236fb-282">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="236fb-283">Nadaj mu nazwę "ContactManager", przestrzeń nazw używaną w próbce pasujących przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="236fb-283">Name it "ContactManager" so the namespace matches the namespace used in the sample.</span></span>
  * <span data-ttu-id="236fb-284">`-uld` Określa LocalDB zamiast bazy danych SQLite</span><span class="sxs-lookup"><span data-stu-id="236fb-284">`-uld` specifies LocalDB instead of SQLite</span></span>

  ```dotnetcli
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* <span data-ttu-id="236fb-285">Dodaj *modele/Contact. cs*:</span><span class="sxs-lookup"><span data-stu-id="236fb-285">Add *Models/Contact.cs*:</span></span>

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="236fb-286">Tworzenie szkieletu `Contact` modelu.</span><span class="sxs-lookup"><span data-stu-id="236fb-286">Scaffold the `Contact` model.</span></span>
* <span data-ttu-id="236fb-287">Tworzenie początkowej migracji i aktualizowanie bazy danych:</span><span class="sxs-lookup"><span data-stu-id="236fb-287">Create initial migration and update the database:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet tool install -g dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
dotnet ef database drop -f
dotnet ef migrations add initial
dotnet ef database update
```

<span data-ttu-id="236fb-288">Jeśli wystąpi usterka z `dotnet aspnet-codegenerator razorpage` poleceniem, zobacz [ten problem](https://github.com/aspnet/Scaffolding/issues/984)w usłudze GitHub.</span><span class="sxs-lookup"><span data-stu-id="236fb-288">If you experience a bug with the `dotnet aspnet-codegenerator razorpage` command, see [this GitHub issue](https://github.com/aspnet/Scaffolding/issues/984).</span></span>

* <span data-ttu-id="236fb-289">Zaktualizuj kotwicę **ContactManager** w pliku *Pages/Shared/_Layout. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="236fb-289">Update the **ContactManager** anchor in the *Pages/Shared/_Layout.cshtml* file:</span></span>

 ```cshtml
<a class="navbar-brand" asp-area="" asp-page="/Contacts/Index">ContactManager</a>
  ```

* <span data-ttu-id="236fb-290">Przetestuj aplikację, tworzenia, edytowania i usuwania kontaktu</span><span class="sxs-lookup"><span data-stu-id="236fb-290">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="236fb-291">Inicjowanie bazy danych</span><span class="sxs-lookup"><span data-stu-id="236fb-291">Seed the database</span></span>

<span data-ttu-id="236fb-292">Dodaj klasę [SeedData](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter3/Data/SeedData.cs) do folderu *danych* :</span><span class="sxs-lookup"><span data-stu-id="236fb-292">Add the [SeedData](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter3/Data/SeedData.cs) class to the *Data* folder:</span></span>

[!code-csharp[](secure-data/samples/starter3/Data/SeedData.cs)]

<span data-ttu-id="236fb-293">Wywołaj `SeedData.Initialize` z `Main`:</span><span class="sxs-lookup"><span data-stu-id="236fb-293">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[](secure-data/samples/starter3/Program.cs)]

<span data-ttu-id="236fb-294">Sprawdź, czy aplikacja zasilany bazy danych.</span><span class="sxs-lookup"><span data-stu-id="236fb-294">Test that the app seeded the database.</span></span> <span data-ttu-id="236fb-295">W przypadku wszystkich wierszy w skontaktuj się z bazy danych, metoda inicjatora nie zostanie uruchomiony.</span><span class="sxs-lookup"><span data-stu-id="236fb-295">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1 < aspnetcore-3.0"

<span data-ttu-id="236fb-296">W tym samouczku pokazano, jak utworzyć aplikację sieci web platformy ASP.NET Core przy użyciu danych użytkownika chronionych przez autoryzację.</span><span class="sxs-lookup"><span data-stu-id="236fb-296">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="236fb-297">Wyświetla listę kontaktów, uwierzytelnionych użytkowników (zarejestrowane), które zostały utworzone.</span><span class="sxs-lookup"><span data-stu-id="236fb-297">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="236fb-298">Istnieją trzy grupy zabezpieczeń:</span><span class="sxs-lookup"><span data-stu-id="236fb-298">There are three security groups:</span></span>

* <span data-ttu-id="236fb-299">**Liczba zarejestrowanych użytkowników** może wyświetlać wszystkie zatwierdzone dane i może edytować/usuwać swoje dane.</span><span class="sxs-lookup"><span data-stu-id="236fb-299">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="236fb-300">**Menedżerowie** można zatwierdzić lub odrzucić dane kontaktowe.</span><span class="sxs-lookup"><span data-stu-id="236fb-300">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="236fb-301">Tylko zatwierdzone kontakty będą widoczne dla użytkowników.</span><span class="sxs-lookup"><span data-stu-id="236fb-301">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="236fb-302">**Administratorzy** można zatwierdzić/Odrzuć i edytowanie/usuwanie żadnych danych.</span><span class="sxs-lookup"><span data-stu-id="236fb-302">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="236fb-303">Na poniższej ilustracji użytkownik Rick (`rick@example.com`) jest zalogowany.</span><span class="sxs-lookup"><span data-stu-id="236fb-303">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="236fb-304">Rick mogą jedynie wyświetlać kontakty zatwierdzone i **Edytuj**/**Usuń**/**Utwórz nowy** linki do swoich kontaktów.</span><span class="sxs-lookup"><span data-stu-id="236fb-304">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="236fb-305">Tylko ostatni rekord, utworzone przez Rick, wyświetla **Edytuj** i **Usuń** łącza.</span><span class="sxs-lookup"><span data-stu-id="236fb-305">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="236fb-306">Inni użytkownicy nie zobaczą ostatni rekord, aż Menedżer lub administrator zmienia stan na "Zatwierdzone".</span><span class="sxs-lookup"><span data-stu-id="236fb-306">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![Zrzut ekranu przedstawiający Rick zalogowany](secure-data/_static/rick.png)

<span data-ttu-id="236fb-308">Na poniższej ilustracji `manager@contoso.com` jest zalogowany i w roli menedżera:</span><span class="sxs-lookup"><span data-stu-id="236fb-308">In the following image, `manager@contoso.com` is signed in and in the manager's role:</span></span>

![Zrzut ekranu przedstawiający manager@contoso.com zalogowany](secure-data/_static/manager1.png)

<span data-ttu-id="236fb-310">Na poniższej ilustracji przedstawiono menedżerów widoku szczegółów kontaktu:</span><span class="sxs-lookup"><span data-stu-id="236fb-310">The following image shows the managers details view of a contact:</span></span>

![Widok menedżera kontaktu](secure-data/_static/manager.png)

<span data-ttu-id="236fb-312">**Zatwierdź** i **Odrzuć** przyciski są wyświetlane tylko dla menedżerów i administratorów.</span><span class="sxs-lookup"><span data-stu-id="236fb-312">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="236fb-313">Na poniższej ilustracji `admin@contoso.com` jest zalogowany i w roli administratora:</span><span class="sxs-lookup"><span data-stu-id="236fb-313">In the following image, `admin@contoso.com` is signed in and in the administrator's role:</span></span>

![Zrzut ekranu przedstawiający admin@contoso.com zalogowany](secure-data/_static/admin.png)

<span data-ttu-id="236fb-315">Administrator ma wszystkie uprawnienia.</span><span class="sxs-lookup"><span data-stu-id="236fb-315">The administrator has all privileges.</span></span> <span data-ttu-id="236fb-316">Ona można odczytu/edytowanie/usuwanie dowolnego skontaktuj się z pomocą i zmienić stan kontaktów.</span><span class="sxs-lookup"><span data-stu-id="236fb-316">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="236fb-317">Aplikacja została utworzona przez [tworzenia szkieletów](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) następujące `Contact` modelu:</span><span class="sxs-lookup"><span data-stu-id="236fb-317">The app was created by [scaffolding](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) the following `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

<span data-ttu-id="236fb-318">Przykład zawiera poniższe obsługi autoryzacji:</span><span class="sxs-lookup"><span data-stu-id="236fb-318">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="236fb-319">`ContactIsOwnerAuthorizationHandler`: Zapewnia, że użytkownik może edytować tylko swoje dane.</span><span class="sxs-lookup"><span data-stu-id="236fb-319">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="236fb-320">`ContactManagerAuthorizationHandler`: Umożliwia menedżerom zatwierdzanie lub odrzucanie kontaktów.</span><span class="sxs-lookup"><span data-stu-id="236fb-320">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="236fb-321">`ContactAdministratorsAuthorizationHandler`: Umożliwia administratorom zatwierdzanie lub odrzucanie kontaktów oraz Edytowanie/usuwanie kontaktów.</span><span class="sxs-lookup"><span data-stu-id="236fb-321">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="236fb-322">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="236fb-322">Prerequisites</span></span>

<span data-ttu-id="236fb-323">W tym samouczku jest zaawansowany.</span><span class="sxs-lookup"><span data-stu-id="236fb-323">This tutorial is advanced.</span></span> <span data-ttu-id="236fb-324">Należy zapoznać się z:</span><span class="sxs-lookup"><span data-stu-id="236fb-324">You should be familiar with:</span></span>

* [<span data-ttu-id="236fb-325">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="236fb-325">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="236fb-326">Uwierzytelnianie</span><span class="sxs-lookup"><span data-stu-id="236fb-326">Authentication</span></span>](xref:security/authentication/identity)
* [<span data-ttu-id="236fb-327">Potwierdzenie konta i odzyskiwanie hasła</span><span class="sxs-lookup"><span data-stu-id="236fb-327">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="236fb-328">Autoryzacja</span><span class="sxs-lookup"><span data-stu-id="236fb-328">Authorization</span></span>](xref:security/authorization/introduction)
* [<span data-ttu-id="236fb-329">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="236fb-329">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="236fb-330">Starter i ukończonej aplikacji</span><span class="sxs-lookup"><span data-stu-id="236fb-330">The starter and completed app</span></span>

<span data-ttu-id="236fb-331">[Pobierz](xref:index#how-to-download-a-sample) [ukończone](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) aplikacji.</span><span class="sxs-lookup"><span data-stu-id="236fb-331">[Download](xref:index#how-to-download-a-sample) the [completed](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) app.</span></span> <span data-ttu-id="236fb-332">[Test](#test-the-completed-app) ukończonej aplikacji, dzięki czemu można zapoznać się z jej funkcjami zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="236fb-332">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="236fb-333">Aplikację startową</span><span class="sxs-lookup"><span data-stu-id="236fb-333">The starter app</span></span>

<span data-ttu-id="236fb-334">[Pobierz](xref:index#how-to-download-a-sample) [starter](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) aplikacji.</span><span class="sxs-lookup"><span data-stu-id="236fb-334">[Download](xref:index#how-to-download-a-sample) the [starter](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) app.</span></span>

<span data-ttu-id="236fb-335">Uruchom aplikację, naciśnij przycisk **ContactManager** link i sprawdź, tworzenie, edytowanie i usuwanie kontaktu.</span><span class="sxs-lookup"><span data-stu-id="236fb-335">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="236fb-336">Zabezpieczanie danych użytkownika</span><span class="sxs-lookup"><span data-stu-id="236fb-336">Secure user data</span></span>

<span data-ttu-id="236fb-337">Poniższe sekcje mają główne kroki umożliwiające utworzenie aplikacji danych bezpiecznego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="236fb-337">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="236fb-338">Może okazać się przydatne do odwoływania się do projektu ukończona.</span><span class="sxs-lookup"><span data-stu-id="236fb-338">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="236fb-339">Powiąż dane kontaktowe dla użytkownika</span><span class="sxs-lookup"><span data-stu-id="236fb-339">Tie the contact data to the user</span></span>

<span data-ttu-id="236fb-340">Za pomocą programu ASP.NET [tożsamości](xref:security/authentication/identity) identyfikator użytkownika w celu zapewnienia, że użytkownicy mogą edytować swoje dane, ale nie dane innych użytkowników.</span><span class="sxs-lookup"><span data-stu-id="236fb-340">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="236fb-341">Dodaj `OwnerID` i `ContactStatus` do `Contact` modelu:</span><span class="sxs-lookup"><span data-stu-id="236fb-341">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

<span data-ttu-id="236fb-342">`OwnerID` Identyfikator użytkownika z `AspNetUser` tabelę [tożsamości](xref:security/authentication/identity) bazy danych.</span><span class="sxs-lookup"><span data-stu-id="236fb-342">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="236fb-343">`Status` Pola określa, czy kontakt jest widoczny dla zwykłych użytkowników.</span><span class="sxs-lookup"><span data-stu-id="236fb-343">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="236fb-344">Tworzenie nowej migracji i aktualizowanie bazy danych:</span><span class="sxs-lookup"><span data-stu-id="236fb-344">Create a new migration and update the database:</span></span>

```dotnetcli
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-identity"></a><span data-ttu-id="236fb-345">Dodaj usługi ról do tożsamości</span><span class="sxs-lookup"><span data-stu-id="236fb-345">Add Role services to Identity</span></span>

<span data-ttu-id="236fb-346">Dołącz [opcji Dodawanie ról](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) można dodać usługi ról:</span><span class="sxs-lookup"><span data-stu-id="236fb-346">Append [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) to add Role services:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet2&highlight=12)]

### <a name="require-authenticated-users"></a><span data-ttu-id="236fb-347">Wymaga uwierzytelnionych użytkowników</span><span class="sxs-lookup"><span data-stu-id="236fb-347">Require authenticated users</span></span>

<span data-ttu-id="236fb-348">Ustaw domyślne zasady uwierzytelniania, aby wymagać od użytkowników uwierzytelniania:</span><span class="sxs-lookup"><span data-stu-id="236fb-348">Set the default authentication policy to require users to be authenticated:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet&highlight=17-99)] 

 <span data-ttu-id="236fb-349">Użytkownik może zrezygnować z uwierzytelniania na poziomie metody strony Razor, kontrolera lub akcji z `[AllowAnonymous]` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="236fb-349">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="236fb-350">Ustawienie domyślne zasady uwierzytelniania, aby wymagać od użytkowników uwierzytelniania chroni nowo dodanych stronami Razor i kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="236fb-350">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="236fb-351">Posiadanie uwierzytelniania domyślnie wymagane jest bezpieczniejszy niż opierając się na nowych kontrolerów i stron Razor do uwzględnienia `[Authorize]` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="236fb-351">Having authentication required by default is more secure than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span>

<span data-ttu-id="236fb-352">Dodaj [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) do indeksu o i skontaktuj się z pomocą stron, użytkowników anonimowych można uzyskać informacji o lokacji, przed ich zarejestrowania.</span><span class="sxs-lookup"><span data-stu-id="236fb-352">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index, About, and Contact pages so anonymous users can get information about the site before they register.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Index.cshtml.cs?highlight=1,6)]

### <a name="configure-the-test-account"></a><span data-ttu-id="236fb-353">Skonfiguruj konto testu</span><span class="sxs-lookup"><span data-stu-id="236fb-353">Configure the test account</span></span>

<span data-ttu-id="236fb-354">`SeedData` Klasy tworzy dwa konta: administrator i Menedżer.</span><span class="sxs-lookup"><span data-stu-id="236fb-354">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="236fb-355">Użyj [narzędzie Menedżer klucz tajny](xref:security/app-secrets) ustawić hasło dla tych kont.</span><span class="sxs-lookup"><span data-stu-id="236fb-355">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="236fb-356">Ustaw hasło z katalogu projektu (katalogu zawierającego *Program.cs*):</span><span class="sxs-lookup"><span data-stu-id="236fb-356">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```dotnetcli
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="236fb-357">Jeśli nie określono silne hasło, wyjątek jest generowany, gdy `SeedData.Initialize` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="236fb-357">If a strong password is not specified, an exception is thrown when `SeedData.Initialize` is called.</span></span>

<span data-ttu-id="236fb-358">Aktualizacja `Main` hasła testu:</span><span class="sxs-lookup"><span data-stu-id="236fb-358">Update `Main` to use the test password:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="236fb-359">Tworzenie konta testowe i aktualizowanie kontaktów</span><span class="sxs-lookup"><span data-stu-id="236fb-359">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="236fb-360">Aktualizacja `Initialize` method in Class metoda `SeedData` klasy w celu utworzenia konta testowe:</span><span class="sxs-lookup"><span data-stu-id="236fb-360">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="236fb-361">Dodaj identyfikator użytkownika administratora i `ContactStatus` do kontaktów.</span><span class="sxs-lookup"><span data-stu-id="236fb-361">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="236fb-362">Określ jeden z kontaktów "Przesłane" i jednym "odrzucone".</span><span class="sxs-lookup"><span data-stu-id="236fb-362">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="236fb-363">Dodawanie stanu i Identyfikatora użytkownika do wszystkich kontaktów.</span><span class="sxs-lookup"><span data-stu-id="236fb-363">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="236fb-364">Wyświetlane jest tylko jeden kontakt:</span><span class="sxs-lookup"><span data-stu-id="236fb-364">Only one contact is shown:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="236fb-365">Utwórz właściciela, Menedżer i obsługi autoryzacji administratora</span><span class="sxs-lookup"><span data-stu-id="236fb-365">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="236fb-366">Utwórz folder *autoryzacji* i Utwórz `ContactIsOwnerAuthorizationHandler` w nim klasę.</span><span class="sxs-lookup"><span data-stu-id="236fb-366">Create an *Authorization* folder and create a `ContactIsOwnerAuthorizationHandler` class in it.</span></span> <span data-ttu-id="236fb-367">`ContactIsOwnerAuthorizationHandler` Sprawdza, czy użytkownik, działając w zasobie jest właścicielem zasobu.</span><span class="sxs-lookup"><span data-stu-id="236fb-367">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="236fb-368">`ContactIsOwnerAuthorizationHandler` Wywołania [kontekstu. Powodzenie](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) aktualnego użytkownika uwierzytelnionego jest skontaktuj się z właścicielem.</span><span class="sxs-lookup"><span data-stu-id="236fb-368">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="236fb-369">Programy obsługi autoryzacji zazwyczaj:</span><span class="sxs-lookup"><span data-stu-id="236fb-369">Authorization handlers generally:</span></span>

* <span data-ttu-id="236fb-370">Zwróć `context.Succeed` gdy spełniono wymagania.</span><span class="sxs-lookup"><span data-stu-id="236fb-370">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="236fb-371">Zwróć `Task.CompletedTask` gdy nie są spełnione wymagania.</span><span class="sxs-lookup"><span data-stu-id="236fb-371">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="236fb-372">`Task.CompletedTask`nie powiodło się lub&mdash;niepowodzenie — zezwala na uruchamianie innych programów obsługi autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="236fb-372">`Task.CompletedTask` is not success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="236fb-373">Jeśli potrzebujesz jawnie nie powiedzie się, zwraca [kontekstu. Niepowodzenie](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span><span class="sxs-lookup"><span data-stu-id="236fb-373">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="236fb-374">Aplikacja umożliwia skontaktuj się z pomocą właścicieli do edycji/delete/tworzenie własnych danych.</span><span class="sxs-lookup"><span data-stu-id="236fb-374">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="236fb-375">`ContactIsOwnerAuthorizationHandler` nie muszą sprawdzać operacji przekazane w parametrze wymagań.</span><span class="sxs-lookup"><span data-stu-id="236fb-375">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="236fb-376">Utwórz procedurę obsługi Menedżer autoryzacji</span><span class="sxs-lookup"><span data-stu-id="236fb-376">Create a manager authorization handler</span></span>

<span data-ttu-id="236fb-377">Tworzenie `ContactManagerAuthorizationHandler` klasy w *autoryzacji* folderu.</span><span class="sxs-lookup"><span data-stu-id="236fb-377">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="236fb-378">`ContactManagerAuthorizationHandler` Weryfikuje użytkownika, działając w zasobie menedżera.</span><span class="sxs-lookup"><span data-stu-id="236fb-378">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="236fb-379">Tylko menedżerowie mogli zatwierdzać lub odrzucać zmiany zawartości (nowe lub zmienione).</span><span class="sxs-lookup"><span data-stu-id="236fb-379">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="236fb-380">Utwórz procedurę obsługi autoryzacji administratora</span><span class="sxs-lookup"><span data-stu-id="236fb-380">Create an administrator authorization handler</span></span>

<span data-ttu-id="236fb-381">Tworzenie `ContactAdministratorsAuthorizationHandler` klasy w *autoryzacji* folderu.</span><span class="sxs-lookup"><span data-stu-id="236fb-381">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="236fb-382">`ContactAdministratorsAuthorizationHandler` Weryfikuje użytkownika na zasób jest administratorem.</span><span class="sxs-lookup"><span data-stu-id="236fb-382">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="236fb-383">Administrator może wykonać wszystkie operacje.</span><span class="sxs-lookup"><span data-stu-id="236fb-383">Administrator can do all operations.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="236fb-384">Zarejestruj procedury obsługi autoryzacji</span><span class="sxs-lookup"><span data-stu-id="236fb-384">Register the authorization handlers</span></span>

<span data-ttu-id="236fb-385">Usługi za pomocą platformy Entity Framework Core musi być zarejestrowana do [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) przy użyciu [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span><span class="sxs-lookup"><span data-stu-id="236fb-385">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="236fb-386">`ContactIsOwnerAuthorizationHandler` Korzysta z platformy ASP.NET Core [tożsamości](xref:security/authentication/identity), która jest oparta na platformy Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="236fb-386">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="236fb-387">Zarejestruj procedury obsługi za pomocą kolekcji usługi, dzięki czemu są one dostępne do `ContactsController` za pośrednictwem [wstrzykiwanie zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="236fb-387">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="236fb-388">Dodaj następujący kod na końcu `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="236fb-388">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet_defaultPolicy&highlight=27-99)]

<span data-ttu-id="236fb-389">`ContactAdministratorsAuthorizationHandler` i `ContactManagerAuthorizationHandler` są dodawane jako pojedynczych wystąpień.</span><span class="sxs-lookup"><span data-stu-id="236fb-389">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="236fb-390">Są one pojedynczych wystąpień, ponieważ nie używają programu EF, a wszystkie informacje wymagane w `Context` parametru `HandleRequirementAsync` metody.</span><span class="sxs-lookup"><span data-stu-id="236fb-390">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="236fb-391">Obsługuje autoryzację</span><span class="sxs-lookup"><span data-stu-id="236fb-391">Support authorization</span></span>

<span data-ttu-id="236fb-392">W tej sekcji służy do aktualizacji stron Razor i Dodaj klasę wymagania dotyczące operacji.</span><span class="sxs-lookup"><span data-stu-id="236fb-392">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="236fb-393">Przegląd klasy wymagania dotyczące operacji kontaktu</span><span class="sxs-lookup"><span data-stu-id="236fb-393">Review the contact operations requirements class</span></span>

<span data-ttu-id="236fb-394">Przegląd `ContactOperations` klasy.</span><span class="sxs-lookup"><span data-stu-id="236fb-394">Review the `ContactOperations` class.</span></span> <span data-ttu-id="236fb-395">Ta klasa zawiera wymagania obsługiwanej przez aplikację:</span><span class="sxs-lookup"><span data-stu-id="236fb-395">This class contains the requirements the app supports:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-razor-pages"></a><span data-ttu-id="236fb-396">Utwórz klasę bazową dla stron Razor kontaktów</span><span class="sxs-lookup"><span data-stu-id="236fb-396">Create a base class for the Contacts Razor Pages</span></span>

<span data-ttu-id="236fb-397">Utwórz klasę bazową, zawierający usług wykorzystanych w kontaktach stron Razor.</span><span class="sxs-lookup"><span data-stu-id="236fb-397">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="236fb-398">Klasa bazowa umieszcza kod inicjowania w jednej lokalizacji:</span><span class="sxs-lookup"><span data-stu-id="236fb-398">The base class puts the initialization code in one location:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="236fb-399">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="236fb-399">The preceding code:</span></span>

* <span data-ttu-id="236fb-400">Dodaje `IAuthorizationService` usługi w celu uzyskania dostępu do obsługi autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="236fb-400">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="236fb-401">Dodaje tożsamości `UserManager` usługi.</span><span class="sxs-lookup"><span data-stu-id="236fb-401">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="236fb-402">Dodaj `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="236fb-402">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="236fb-403">Aktualizacja CreateModel</span><span class="sxs-lookup"><span data-stu-id="236fb-403">Update the CreateModel</span></span>

<span data-ttu-id="236fb-404">Aktualizuj Konstruktor Tworzenie strony modelu do użycia `DI_BasePageModel` klasa bazowa:</span><span class="sxs-lookup"><span data-stu-id="236fb-404">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="236fb-405">Aktualizacja `CreateModel.OnPostAsync` metody:</span><span class="sxs-lookup"><span data-stu-id="236fb-405">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="236fb-406">Dodaj identyfikator użytkownika, który `Contact` modelu.</span><span class="sxs-lookup"><span data-stu-id="236fb-406">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="236fb-407">Wywołania obsługi autoryzacji, aby sprawdzić, czy użytkownik ma uprawnienia do tworzenia kontaktów.</span><span class="sxs-lookup"><span data-stu-id="236fb-407">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="236fb-408">Aktualizacja IndexModel</span><span class="sxs-lookup"><span data-stu-id="236fb-408">Update the IndexModel</span></span>

<span data-ttu-id="236fb-409">Aktualizacja `OnGetAsync` metody, więc tylko zatwierdzonego kontakty są wyświetlane użytkownikom ogólne:</span><span class="sxs-lookup"><span data-stu-id="236fb-409">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="236fb-410">Aktualizacja EditModel</span><span class="sxs-lookup"><span data-stu-id="236fb-410">Update the EditModel</span></span>

<span data-ttu-id="236fb-411">Dodaj program obsługi autoryzacji, aby sprawdzić, czy użytkownik jest właścicielem kontaktu.</span><span class="sxs-lookup"><span data-stu-id="236fb-411">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="236fb-412">Ponieważ autoryzacji zasobów jest weryfikowany, `[Authorize]` atrybut nie jest wystarczająca.</span><span class="sxs-lookup"><span data-stu-id="236fb-412">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="236fb-413">Aplikacja nie ma dostępu do zasobu, gdy atrybuty są oceniane.</span><span class="sxs-lookup"><span data-stu-id="236fb-413">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="236fb-414">Autoryzacja na podstawie zasobów musi być imperatywnego.</span><span class="sxs-lookup"><span data-stu-id="236fb-414">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="236fb-415">Testy muszą być wykonywane, gdy aplikacja ma dostęp do zasobu przez załadowanie go w modelu strony lub przez załadowanie go w ramach programu obsługi, sam.</span><span class="sxs-lookup"><span data-stu-id="236fb-415">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="236fb-416">Często dostęp do zasobu, przekazując klucz zasobu.</span><span class="sxs-lookup"><span data-stu-id="236fb-416">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="236fb-417">Aktualizacja DeleteModel</span><span class="sxs-lookup"><span data-stu-id="236fb-417">Update the DeleteModel</span></span>

<span data-ttu-id="236fb-418">Aktualizowanie modelu strony delete na potrzeby obsługi autoryzacji upewnij się, że użytkownik ma odpowiednie uprawnienia do usuwania dla kontaktu.</span><span class="sxs-lookup"><span data-stu-id="236fb-418">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="236fb-419">Wstrzyknięcie usługi autoryzacji do widoków</span><span class="sxs-lookup"><span data-stu-id="236fb-419">Inject the authorization service into the views</span></span>

<span data-ttu-id="236fb-420">Obecnie pokazuje interfejsu użytkownika edytowania i usuwania łącza kontaktów, do których użytkownik nie może modyfikować.</span><span class="sxs-lookup"><span data-stu-id="236fb-420">Currently, the UI shows edit and delete links for contacts the user can't modify.</span></span>

<span data-ttu-id="236fb-421">Wstrzykiwanie usługi autoryzacji w *Views/_ViewImports.cshtml* pliku, dzięki czemu są one dostępne dla wszystkich widoków:</span><span class="sxs-lookup"><span data-stu-id="236fb-421">Inject the authorization service in the *Views/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/_ViewImports.cshtml?highlight=6-99)]

<span data-ttu-id="236fb-422">Poprzedni kod znaczników umożliwia dodanie kilku `using` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="236fb-422">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="236fb-423">Aktualizacja **Edytuj** i **Usuń** linki w *Pages/Contacts/Index.cshtml* więc tylko są one renderowane dla użytkowników z odpowiednimi uprawnieniami:</span><span class="sxs-lookup"><span data-stu-id="236fb-423">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> <span data-ttu-id="236fb-424">Ukrywanie łączy na podstawie użytkowników, którzy nie mają uprawnień do zmiany danych nie zabezpieczenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="236fb-424">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="236fb-425">Ukrywanie łącza sprawia, że bardziej przyjazny dla użytkownika aplikacji, wyświetlając tylko poprawne linki.</span><span class="sxs-lookup"><span data-stu-id="236fb-425">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="236fb-426">Użytkownicy mogą hack wygenerowanego adresy URL, aby wywołać Edytuj i Usuń operacje na danych, które nie są właścicielami.</span><span class="sxs-lookup"><span data-stu-id="236fb-426">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="236fb-427">Strona Razor lub kontrolera musi wymuszają operacje sprawdzania dostępu do zabezpieczania danych.</span><span class="sxs-lookup"><span data-stu-id="236fb-427">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="236fb-428">Szczegóły aktualizacji</span><span class="sxs-lookup"><span data-stu-id="236fb-428">Update Details</span></span>

<span data-ttu-id="236fb-429">Zaktualizuj widok szczegółów, aby menedżerowie mogli zatwierdzać lub odrzucać kontaktów:</span><span class="sxs-lookup"><span data-stu-id="236fb-429">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml?name=snippet)]

<span data-ttu-id="236fb-430">Aktualizowanie modelu strony szczegółów:</span><span class="sxs-lookup"><span data-stu-id="236fb-430">Update the details page model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="add-or-remove-a-user-to-a-role"></a><span data-ttu-id="236fb-431">Dodawanie lub usuwanie użytkownika do roli</span><span class="sxs-lookup"><span data-stu-id="236fb-431">Add or remove a user to a role</span></span>

<span data-ttu-id="236fb-432">Zobacz [ten problem](https://github.com/aspnet/AspNetCore.Docs/issues/8502) uzyskać informacji na temat:</span><span class="sxs-lookup"><span data-stu-id="236fb-432">See [this issue](https://github.com/aspnet/AspNetCore.Docs/issues/8502) for information on:</span></span>

* <span data-ttu-id="236fb-433">Usuwanie uprawnień z użytkownikiem.</span><span class="sxs-lookup"><span data-stu-id="236fb-433">Removing privileges from a user.</span></span> <span data-ttu-id="236fb-434">Na przykład wyciszenie użytkownika w aplikacji czatu.</span><span class="sxs-lookup"><span data-stu-id="236fb-434">For example, muting a user in a chat app.</span></span>
* <span data-ttu-id="236fb-435">Dodawanie uprawnień dla użytkownika.</span><span class="sxs-lookup"><span data-stu-id="236fb-435">Adding privileges to a user.</span></span>

## <a name="test-the-completed-app"></a><span data-ttu-id="236fb-436">Testowanie aplikacji ukończone</span><span class="sxs-lookup"><span data-stu-id="236fb-436">Test the completed app</span></span>

<span data-ttu-id="236fb-437">Jeśli nie został jeszcze ustawiony hasła dla kont użytkowników wypełnionych, użyj [narzędzie Menedżer klucz tajny](xref:security/app-secrets#secret-manager) ustawić hasło:</span><span class="sxs-lookup"><span data-stu-id="236fb-437">If you haven't already set a password for seeded user accounts, use the [Secret Manager tool](xref:security/app-secrets#secret-manager) to set a password:</span></span>

* <span data-ttu-id="236fb-438">Wybierz silne hasło: Użyj ośmiu lub więcej znaków i co najmniej jednego znaku wielkie litery, cyfry i symbolu.</span><span class="sxs-lookup"><span data-stu-id="236fb-438">Choose a strong password: Use eight or more characters and at least one upper-case character, number, and symbol.</span></span> <span data-ttu-id="236fb-439">Na przykład `Passw0rd!` spełnia wymagania silne hasło.</span><span class="sxs-lookup"><span data-stu-id="236fb-439">For example, `Passw0rd!` meets the strong password requirements.</span></span>
* <span data-ttu-id="236fb-440">Wykonaj następujące polecenie z folderu projektu, gdzie `<PW>` jest hasłem:</span><span class="sxs-lookup"><span data-stu-id="236fb-440">Execute the following command from the project's folder, where `<PW>` is the password:</span></span>

  ```dotnetcli
  dotnet user-secrets set SeedUserPW <PW>
  ```

* <span data-ttu-id="236fb-441">Porzuć i zaktualizuj bazę danych</span><span class="sxs-lookup"><span data-stu-id="236fb-441">Drop and update the Database</span></span>

  ```dotnetcli
  dotnet ef database drop -f
  dotnet ef database update  
  ```

* <span data-ttu-id="236fb-442">Ponowne uruchomienie aplikacji w celu umieszczenia bazy danych.</span><span class="sxs-lookup"><span data-stu-id="236fb-442">Restart the app to seed the database.</span></span>

<span data-ttu-id="236fb-443">Łatwe testowanie ukończonej aplikacji jest do uruchomienia w trzech różnych przeglądarek (lub sesji incognito/InPrivate).</span><span class="sxs-lookup"><span data-stu-id="236fb-443">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate sessions).</span></span> <span data-ttu-id="236fb-444">W jednej przeglądarki zarejestrować nowego użytkownika (na przykład `test@example.com`).</span><span class="sxs-lookup"><span data-stu-id="236fb-444">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="236fb-445">Zaloguj się w każdej przeglądarce z innym użytkownikiem.</span><span class="sxs-lookup"><span data-stu-id="236fb-445">Sign in to each browser with a different user.</span></span> <span data-ttu-id="236fb-446">Sprawdź następujące operacje:</span><span class="sxs-lookup"><span data-stu-id="236fb-446">Verify the following operations:</span></span>

* <span data-ttu-id="236fb-447">Zarejestrowani użytkownicy można wyświetlić wszystkie zatwierdzone dane kontaktowe.</span><span class="sxs-lookup"><span data-stu-id="236fb-447">Registered users can view all of the approved contact data.</span></span>
* <span data-ttu-id="236fb-448">Zarejestrowani użytkownicy może edytować/usuwać swoje dane.</span><span class="sxs-lookup"><span data-stu-id="236fb-448">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="236fb-449">Menedżerowie mogą zatwierdzać/Odrzuć dane kontaktowe.</span><span class="sxs-lookup"><span data-stu-id="236fb-449">Managers can approve/reject contact data.</span></span> <span data-ttu-id="236fb-450">`Details` Wyświetlić pokazuje **Zatwierdź** i **Odrzuć** przycisków.</span><span class="sxs-lookup"><span data-stu-id="236fb-450">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="236fb-451">Administratorzy mogą zatwierdzać/Odrzuć i edytowanie/usuwanie wszystkich danych.</span><span class="sxs-lookup"><span data-stu-id="236fb-451">Administrators can approve/reject and edit/delete all data.</span></span>

| <span data-ttu-id="236fb-452">Użytkownik</span><span class="sxs-lookup"><span data-stu-id="236fb-452">User</span></span>                | <span data-ttu-id="236fb-453">Zasilany przez aplikację</span><span class="sxs-lookup"><span data-stu-id="236fb-453">Seeded by the app</span></span> | <span data-ttu-id="236fb-454">Opcje</span><span class="sxs-lookup"><span data-stu-id="236fb-454">Options</span></span>                                  |
| ------------------- | :---------------: | ---------------------------------------- |
| test@example.com    | <span data-ttu-id="236fb-455">Nie</span><span class="sxs-lookup"><span data-stu-id="236fb-455">No</span></span>                | <span data-ttu-id="236fb-456">Edytowanie/usuwanie własnych danych.</span><span class="sxs-lookup"><span data-stu-id="236fb-456">Edit/delete the own data.</span></span>                |
| manager@contoso.com | <span data-ttu-id="236fb-457">Tak</span><span class="sxs-lookup"><span data-stu-id="236fb-457">Yes</span></span>               | <span data-ttu-id="236fb-458">Zatwierdź/Odrzuć i edytowanie/usuwanie własnych danych.</span><span class="sxs-lookup"><span data-stu-id="236fb-458">Approve/reject and edit/delete own data.</span></span> |
| admin@contoso.com   | <span data-ttu-id="236fb-459">Tak</span><span class="sxs-lookup"><span data-stu-id="236fb-459">Yes</span></span>               | <span data-ttu-id="236fb-460">Zatwierdź/Odrzuć i edytowanie/usuwanie wszystkich danych.</span><span class="sxs-lookup"><span data-stu-id="236fb-460">Approve/reject and edit/delete all data.</span></span> |

<span data-ttu-id="236fb-461">Utwórz kontakt w przeglądarce administratora.</span><span class="sxs-lookup"><span data-stu-id="236fb-461">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="236fb-462">Skopiuj adres URL do usunięcia, a następnie Edytuj z skontaktowanie się z administratorem.</span><span class="sxs-lookup"><span data-stu-id="236fb-462">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="236fb-463">Wklej te linki do przeglądarki użytkownika testowego, aby sprawdzić, czy użytkownik testu nie może wykonywać te operacje.</span><span class="sxs-lookup"><span data-stu-id="236fb-463">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="236fb-464">Utwórz aplikację startową</span><span class="sxs-lookup"><span data-stu-id="236fb-464">Create the starter app</span></span>

* <span data-ttu-id="236fb-465">Tworzenie stron Razor aplikacji o nazwie "ContactManager"</span><span class="sxs-lookup"><span data-stu-id="236fb-465">Create a Razor Pages app named "ContactManager"</span></span>
  * <span data-ttu-id="236fb-466">Tworzenie aplikacji za pomocą **indywidualne konta użytkowników**.</span><span class="sxs-lookup"><span data-stu-id="236fb-466">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="236fb-467">Nadaj mu nazwę "ContactManager", przestrzeń nazw używaną w próbce pasujących przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="236fb-467">Name it "ContactManager" so the namespace matches the namespace used in the sample.</span></span>
  * <span data-ttu-id="236fb-468">`-uld` Określa LocalDB zamiast bazy danych SQLite</span><span class="sxs-lookup"><span data-stu-id="236fb-468">`-uld` specifies LocalDB instead of SQLite</span></span>

  ```dotnetcli
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* <span data-ttu-id="236fb-469">Dodaj *modele/Contact. cs*:</span><span class="sxs-lookup"><span data-stu-id="236fb-469">Add *Models/Contact.cs*:</span></span>

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="236fb-470">Tworzenie szkieletu `Contact` modelu.</span><span class="sxs-lookup"><span data-stu-id="236fb-470">Scaffold the `Contact` model.</span></span>
* <span data-ttu-id="236fb-471">Tworzenie początkowej migracji i aktualizowanie bazy danych:</span><span class="sxs-lookup"><span data-stu-id="236fb-471">Create initial migration and update the database:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
  dotnet ef database drop -f
  dotnet ef migrations add initial
  dotnet ef database update
  ```

* <span data-ttu-id="236fb-472">Aktualizacja **ContactManager** zakotwiczenia w *Pages/_Layout.cshtml* pliku:</span><span class="sxs-lookup"><span data-stu-id="236fb-472">Update the **ContactManager** anchor in the *Pages/_Layout.cshtml* file:</span></span>

  ```cshtml
  <a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
  ```

* <span data-ttu-id="236fb-473">Przetestuj aplikację, tworzenia, edytowania i usuwania kontaktu</span><span class="sxs-lookup"><span data-stu-id="236fb-473">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="236fb-474">Inicjowanie bazy danych</span><span class="sxs-lookup"><span data-stu-id="236fb-474">Seed the database</span></span>

<span data-ttu-id="236fb-475">Dodaj [SeedData](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs) klasy *danych* folderu.</span><span class="sxs-lookup"><span data-stu-id="236fb-475">Add the [SeedData](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs) class to the *Data* folder.</span></span>

<span data-ttu-id="236fb-476">Wywołaj `SeedData.Initialize` z `Main`:</span><span class="sxs-lookup"><span data-stu-id="236fb-476">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Program.cs?name=snippet)]

<span data-ttu-id="236fb-477">Sprawdź, czy aplikacja zasilany bazy danych.</span><span class="sxs-lookup"><span data-stu-id="236fb-477">Test that the app seeded the database.</span></span> <span data-ttu-id="236fb-478">W przypadku wszystkich wierszy w skontaktuj się z bazy danych, metoda inicjatora nie zostanie uruchomiony.</span><span class="sxs-lookup"><span data-stu-id="236fb-478">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

::: moniker-end

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a><span data-ttu-id="236fb-479">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="236fb-479">Additional resources</span></span>

* [<span data-ttu-id="236fb-480">Tworzenie aplikacji internetowej platformy .NET Core i SQL Database w usłudze Azure App Service</span><span class="sxs-lookup"><span data-stu-id="236fb-480">Build a .NET Core and SQL Database web app in Azure App Service</span></span>](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb)
* <span data-ttu-id="236fb-481">[Laboratorium autoryzacji platformy ASP.NET Core](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span><span class="sxs-lookup"><span data-stu-id="236fb-481">[ASP.NET Core Authorization Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span></span> <span data-ttu-id="236fb-482">W tym laboratorium zawiera bardziej szczegółowe na temat funkcji zabezpieczeń wprowadzone w ramach tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="236fb-482">This lab goes into more detail on the security features introduced in this tutorial.</span></span>
* <xref:security/authorization/introduction>
* [<span data-ttu-id="236fb-483">Autoryzacja niestandardowa oparta na zasadach</span><span class="sxs-lookup"><span data-stu-id="236fb-483">Custom policy-based authorization</span></span>](xref:security/authorization/policies)
