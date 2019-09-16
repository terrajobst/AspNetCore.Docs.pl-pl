---
title: Tworzenie aplikacji platformy ASP.NET Core przy użyciu danych użytkownika chronionych przez autoryzację
author: rick-anderson
description: Dowiedz się, jak utworzyć aplikację stron Razor przy użyciu danych użytkownika chronionych przez autoryzację. Obejmuje protokołu HTTPS, uwierzytelniania, zabezpieczeń i tożsamości platformy ASP.NET Core.
ms.author: riande
ms.date: 12/18/2018
ms.custom: mvc, seodec18
uid: security/authorization/secure-data
ms.openlocfilehash: d95f44394d6ecc3c3896b45c5bebc73fa2d92445
ms.sourcegitcommit: dc5b293e08336dc236de66ed1834f7ef78359531
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/16/2019
ms.locfileid: "71011189"
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a><span data-ttu-id="e177b-104">Tworzenie aplikacji platformy ASP.NET Core przy użyciu danych użytkownika chronionych przez autoryzację</span><span class="sxs-lookup"><span data-stu-id="e177b-104">Create an ASP.NET Core app with user data protected by authorization</span></span>

<span data-ttu-id="e177b-105">Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Audette Jan](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="e177b-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="e177b-106">Zobacz [ta](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) dla wersji platformy ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="e177b-106">See [this PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC version.</span></span> <span data-ttu-id="e177b-107">Wersja platformy ASP.NET Core 1.1 po ukończeniu tego samouczka jest [to](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data) folderu.</span><span class="sxs-lookup"><span data-stu-id="e177b-107">The ASP.NET Core 1.1 version of this tutorial is in [this](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data) folder.</span></span> <span data-ttu-id="e177b-108">1\.1, na przykład platforma ASP.NET Core jest w [przykłady](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span><span class="sxs-lookup"><span data-stu-id="e177b-108">The 1.1 ASP.NET Core sample is in the [samples](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="e177b-109">Zobacz [ten plik pdf](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_July16_18.pdf)</span><span class="sxs-lookup"><span data-stu-id="e177b-109">See [this pdf](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_July16_18.pdf)</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="e177b-110">W tym samouczku pokazano, jak utworzyć aplikację sieci web platformy ASP.NET Core przy użyciu danych użytkownika chronionych przez autoryzację.</span><span class="sxs-lookup"><span data-stu-id="e177b-110">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="e177b-111">Wyświetla listę kontaktów, uwierzytelnionych użytkowników (zarejestrowane), które zostały utworzone.</span><span class="sxs-lookup"><span data-stu-id="e177b-111">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="e177b-112">Istnieją trzy grupy zabezpieczeń:</span><span class="sxs-lookup"><span data-stu-id="e177b-112">There are three security groups:</span></span>

* <span data-ttu-id="e177b-113">**Liczba zarejestrowanych użytkowników** może wyświetlać wszystkie zatwierdzone dane i może edytować/usuwać swoje dane.</span><span class="sxs-lookup"><span data-stu-id="e177b-113">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="e177b-114">**Menedżerowie** można zatwierdzić lub odrzucić dane kontaktowe.</span><span class="sxs-lookup"><span data-stu-id="e177b-114">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="e177b-115">Tylko zatwierdzone kontakty będą widoczne dla użytkowników.</span><span class="sxs-lookup"><span data-stu-id="e177b-115">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="e177b-116">**Administratorzy** można zatwierdzić/Odrzuć i edytowanie/usuwanie żadnych danych.</span><span class="sxs-lookup"><span data-stu-id="e177b-116">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="e177b-117">Obrazy w tym dokumencie nie są dokładnie zgodne z najnowszymi szablonami.</span><span class="sxs-lookup"><span data-stu-id="e177b-117">The images in this document don't exactly match the latest templates.</span></span>

<span data-ttu-id="e177b-118">Na poniższej ilustracji użytkownik Rick (`rick@example.com`) jest zalogowany.</span><span class="sxs-lookup"><span data-stu-id="e177b-118">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="e177b-119">Rick mogą jedynie wyświetlać kontakty zatwierdzone i **Edytuj**/**Usuń**/**Utwórz nowy** linki do swoich kontaktów.</span><span class="sxs-lookup"><span data-stu-id="e177b-119">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="e177b-120">Tylko ostatni rekord, utworzone przez Rick, wyświetla **Edytuj** i **Usuń** łącza.</span><span class="sxs-lookup"><span data-stu-id="e177b-120">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="e177b-121">Inni użytkownicy nie zobaczą ostatni rekord, aż Menedżer lub administrator zmienia stan na "Zatwierdzone".</span><span class="sxs-lookup"><span data-stu-id="e177b-121">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![Zrzut ekranu przedstawiający Rick zalogowany](secure-data/_static/rick.png)

<span data-ttu-id="e177b-123">Na poniższej ilustracji `manager@contoso.com` jest zalogowany i w roli menedżera:</span><span class="sxs-lookup"><span data-stu-id="e177b-123">In the following image, `manager@contoso.com` is signed in and in the manager's role:</span></span>

![Zrzut ekranu przedstawiający manager@contoso.com zalogowany](secure-data/_static/manager1.png)

<span data-ttu-id="e177b-125">Na poniższej ilustracji przedstawiono menedżerów widoku szczegółów kontaktu:</span><span class="sxs-lookup"><span data-stu-id="e177b-125">The following image shows the managers details view of a contact:</span></span>

![Widok menedżera kontaktu](secure-data/_static/manager.png)

<span data-ttu-id="e177b-127">**Zatwierdź** i **Odrzuć** przyciski są wyświetlane tylko dla menedżerów i administratorów.</span><span class="sxs-lookup"><span data-stu-id="e177b-127">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="e177b-128">Na poniższej ilustracji `admin@contoso.com` jest zalogowany i w roli administratora:</span><span class="sxs-lookup"><span data-stu-id="e177b-128">In the following image, `admin@contoso.com` is signed in and in the administrator's role:</span></span>

![Zrzut ekranu przedstawiający admin@contoso.com zalogowany](secure-data/_static/admin.png)

<span data-ttu-id="e177b-130">Administrator ma wszystkie uprawnienia.</span><span class="sxs-lookup"><span data-stu-id="e177b-130">The administrator has all privileges.</span></span> <span data-ttu-id="e177b-131">Ona można odczytu/edytowanie/usuwanie dowolnego skontaktuj się z pomocą i zmienić stan kontaktów.</span><span class="sxs-lookup"><span data-stu-id="e177b-131">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="e177b-132">Aplikacja została utworzona przez [tworzenia szkieletów](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) następujące `Contact` modelu:</span><span class="sxs-lookup"><span data-stu-id="e177b-132">The app was created by [scaffolding](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) the following `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

<span data-ttu-id="e177b-133">Przykład zawiera poniższe obsługi autoryzacji:</span><span class="sxs-lookup"><span data-stu-id="e177b-133">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="e177b-134">`ContactIsOwnerAuthorizationHandler`: Zapewnia, że użytkownik może edytować tylko swoje dane.</span><span class="sxs-lookup"><span data-stu-id="e177b-134">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="e177b-135">`ContactManagerAuthorizationHandler`: Umożliwia menedżerom zatwierdzanie lub odrzucanie kontaktów.</span><span class="sxs-lookup"><span data-stu-id="e177b-135">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="e177b-136">`ContactAdministratorsAuthorizationHandler`: Umożliwia administratorom zatwierdzanie lub odrzucanie kontaktów oraz Edytowanie/usuwanie kontaktów.</span><span class="sxs-lookup"><span data-stu-id="e177b-136">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e177b-137">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="e177b-137">Prerequisites</span></span>

<span data-ttu-id="e177b-138">W tym samouczku jest zaawansowany.</span><span class="sxs-lookup"><span data-stu-id="e177b-138">This tutorial is advanced.</span></span> <span data-ttu-id="e177b-139">Należy zapoznać się z:</span><span class="sxs-lookup"><span data-stu-id="e177b-139">You should be familiar with:</span></span>

* [<span data-ttu-id="e177b-140">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e177b-140">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="e177b-141">Uwierzytelnianie</span><span class="sxs-lookup"><span data-stu-id="e177b-141">Authentication</span></span>](xref:security/authentication/identity)
* [<span data-ttu-id="e177b-142">Potwierdzenie konta i odzyskiwanie hasła</span><span class="sxs-lookup"><span data-stu-id="e177b-142">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="e177b-143">Autoryzacja</span><span class="sxs-lookup"><span data-stu-id="e177b-143">Authorization</span></span>](xref:security/authorization/introduction)
* [<span data-ttu-id="e177b-144">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="e177b-144">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="e177b-145">Starter i ukończonej aplikacji</span><span class="sxs-lookup"><span data-stu-id="e177b-145">The starter and completed app</span></span>

<span data-ttu-id="e177b-146">[Pobierz](xref:index#how-to-download-a-sample) [ukończone](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e177b-146">[Download](xref:index#how-to-download-a-sample) the [completed](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) app.</span></span> <span data-ttu-id="e177b-147">[Test](#test-the-completed-app) ukończonej aplikacji, dzięki czemu można zapoznać się z jej funkcjami zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="e177b-147">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="e177b-148">Aplikację startową</span><span class="sxs-lookup"><span data-stu-id="e177b-148">The starter app</span></span>

<span data-ttu-id="e177b-149">[Pobierz](xref:index#how-to-download-a-sample) [starter](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e177b-149">[Download](xref:index#how-to-download-a-sample) the [starter](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) app.</span></span>

<span data-ttu-id="e177b-150">Uruchom aplikację, naciśnij przycisk **ContactManager** link i sprawdź, tworzenie, edytowanie i usuwanie kontaktu.</span><span class="sxs-lookup"><span data-stu-id="e177b-150">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="e177b-151">Zabezpieczanie danych użytkownika</span><span class="sxs-lookup"><span data-stu-id="e177b-151">Secure user data</span></span>

<span data-ttu-id="e177b-152">Poniższe sekcje mają główne kroki umożliwiające utworzenie aplikacji danych bezpiecznego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="e177b-152">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="e177b-153">Może okazać się przydatne do odwoływania się do projektu ukończona.</span><span class="sxs-lookup"><span data-stu-id="e177b-153">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="e177b-154">Powiąż dane kontaktowe dla użytkownika</span><span class="sxs-lookup"><span data-stu-id="e177b-154">Tie the contact data to the user</span></span>

<span data-ttu-id="e177b-155">Za pomocą programu ASP.NET [tożsamości](xref:security/authentication/identity) identyfikator użytkownika w celu zapewnienia, że użytkownicy mogą edytować swoje dane, ale nie dane innych użytkowników.</span><span class="sxs-lookup"><span data-stu-id="e177b-155">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="e177b-156">Dodaj `OwnerID` i `ContactStatus` do `Contact` modelu:</span><span class="sxs-lookup"><span data-stu-id="e177b-156">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/final3/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

<span data-ttu-id="e177b-157">`OwnerID` Identyfikator użytkownika z `AspNetUser` tabelę [tożsamości](xref:security/authentication/identity) bazy danych.</span><span class="sxs-lookup"><span data-stu-id="e177b-157">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="e177b-158">`Status` Pola określa, czy kontakt jest widoczny dla zwykłych użytkowników.</span><span class="sxs-lookup"><span data-stu-id="e177b-158">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="e177b-159">Tworzenie nowej migracji i aktualizowanie bazy danych:</span><span class="sxs-lookup"><span data-stu-id="e177b-159">Create a new migration and update the database:</span></span>

```console
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-identity"></a><span data-ttu-id="e177b-160">Dodaj usługi ról do tożsamości</span><span class="sxs-lookup"><span data-stu-id="e177b-160">Add Role services to Identity</span></span>

<span data-ttu-id="e177b-161">Dołącz [opcji Dodawanie ról](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) można dodać usługi ról:</span><span class="sxs-lookup"><span data-stu-id="e177b-161">Append [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) to add Role services:</span></span>

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet2&highlight=9)]

### <a name="require-authenticated-users"></a><span data-ttu-id="e177b-162">Wymaga uwierzytelnionych użytkowników</span><span class="sxs-lookup"><span data-stu-id="e177b-162">Require authenticated users</span></span>

<span data-ttu-id="e177b-163">Ustaw domyślne zasady uwierzytelniania, aby wymagać od użytkowników uwierzytelniania:</span><span class="sxs-lookup"><span data-stu-id="e177b-163">Set the default authentication policy to require users to be authenticated:</span></span>

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet&highlight=15-99)] 

 <span data-ttu-id="e177b-164">Użytkownik może zrezygnować z uwierzytelniania na poziomie metody strony Razor, kontrolera lub akcji z `[AllowAnonymous]` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="e177b-164">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="e177b-165">Ustawienie domyślne zasady uwierzytelniania, aby wymagać od użytkowników uwierzytelniania chroni nowo dodanych stronami Razor i kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="e177b-165">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="e177b-166">Posiadanie uwierzytelniania domyślnie wymagane jest bezpieczniejszy niż opierając się na nowych kontrolerów i stron Razor do uwzględnienia `[Authorize]` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="e177b-166">Having authentication required by default is more secure than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span>

<span data-ttu-id="e177b-167">Dodaj [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) do stron indeksu i prywatności, aby użytkownicy anonimowi mogli uzyskać informacje o witrynie przed ich zarejestrowaniem.</span><span class="sxs-lookup"><span data-stu-id="e177b-167">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index and Privacy pages so anonymous users can get information about the site before they register.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Index.cshtml.cs?highlight=1,7)]

### <a name="configure-the-test-account"></a><span data-ttu-id="e177b-168">Skonfiguruj konto testu</span><span class="sxs-lookup"><span data-stu-id="e177b-168">Configure the test account</span></span>

<span data-ttu-id="e177b-169">`SeedData` Klasy tworzy dwa konta: administrator i Menedżer.</span><span class="sxs-lookup"><span data-stu-id="e177b-169">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="e177b-170">Użyj [narzędzie Menedżer klucz tajny](xref:security/app-secrets) ustawić hasło dla tych kont.</span><span class="sxs-lookup"><span data-stu-id="e177b-170">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="e177b-171">Ustaw hasło z katalogu projektu (katalogu zawierającego *Program.cs*):</span><span class="sxs-lookup"><span data-stu-id="e177b-171">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```console
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="e177b-172">Jeśli nie określono silne hasło, wyjątek jest generowany, gdy `SeedData.Initialize` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="e177b-172">If a strong password is not specified, an exception is thrown when `SeedData.Initialize` is called.</span></span>

<span data-ttu-id="e177b-173">Aktualizacja `Main` hasła testu:</span><span class="sxs-lookup"><span data-stu-id="e177b-173">Update `Main` to use the test password:</span></span>

[!code-csharp[](secure-data/samples/final3/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="e177b-174">Tworzenie konta testowe i aktualizowanie kontaktów</span><span class="sxs-lookup"><span data-stu-id="e177b-174">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="e177b-175">Aktualizacja `Initialize` method in Class metoda `SeedData` klasy w celu utworzenia konta testowe:</span><span class="sxs-lookup"><span data-stu-id="e177b-175">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[](secure-data/samples/final3/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="e177b-176">Dodaj identyfikator użytkownika administratora i `ContactStatus` do kontaktów.</span><span class="sxs-lookup"><span data-stu-id="e177b-176">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="e177b-177">Określ jeden z kontaktów "Przesłane" i jednym "odrzucone".</span><span class="sxs-lookup"><span data-stu-id="e177b-177">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="e177b-178">Dodawanie stanu i Identyfikatora użytkownika do wszystkich kontaktów.</span><span class="sxs-lookup"><span data-stu-id="e177b-178">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="e177b-179">Wyświetlane jest tylko jeden kontakt:</span><span class="sxs-lookup"><span data-stu-id="e177b-179">Only one contact is shown:</span></span>

[!code-csharp[](secure-data/samples/final3/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="e177b-180">Utwórz właściciela, Menedżer i obsługi autoryzacji administratora</span><span class="sxs-lookup"><span data-stu-id="e177b-180">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="e177b-181">Tworzenie `ContactIsOwnerAuthorizationHandler` klasy w *autoryzacji* folderu.</span><span class="sxs-lookup"><span data-stu-id="e177b-181">Create a `ContactIsOwnerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="e177b-182">`ContactIsOwnerAuthorizationHandler` Sprawdza, czy użytkownik, działając w zasobie jest właścicielem zasobu.</span><span class="sxs-lookup"><span data-stu-id="e177b-182">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="e177b-183">`ContactIsOwnerAuthorizationHandler` Wywołania [kontekstu. Powodzenie](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) aktualnego użytkownika uwierzytelnionego jest skontaktuj się z właścicielem.</span><span class="sxs-lookup"><span data-stu-id="e177b-183">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="e177b-184">Programy obsługi autoryzacji zazwyczaj:</span><span class="sxs-lookup"><span data-stu-id="e177b-184">Authorization handlers generally:</span></span>

* <span data-ttu-id="e177b-185">Zwróć `context.Succeed` gdy spełniono wymagania.</span><span class="sxs-lookup"><span data-stu-id="e177b-185">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="e177b-186">Zwróć `Task.CompletedTask` gdy nie są spełnione wymagania.</span><span class="sxs-lookup"><span data-stu-id="e177b-186">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="e177b-187">`Task.CompletedTask`nie powiodło się lub&mdash;niepowodzenie — zezwala na uruchamianie innych programów obsługi autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="e177b-187">`Task.CompletedTask` is not success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="e177b-188">Jeśli potrzebujesz jawnie nie powiedzie się, zwraca [kontekstu. Niepowodzenie](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span><span class="sxs-lookup"><span data-stu-id="e177b-188">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="e177b-189">Aplikacja umożliwia skontaktuj się z pomocą właścicieli do edycji/delete/tworzenie własnych danych.</span><span class="sxs-lookup"><span data-stu-id="e177b-189">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="e177b-190">`ContactIsOwnerAuthorizationHandler` nie muszą sprawdzać operacji przekazane w parametrze wymagań.</span><span class="sxs-lookup"><span data-stu-id="e177b-190">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="e177b-191">Utwórz procedurę obsługi Menedżer autoryzacji</span><span class="sxs-lookup"><span data-stu-id="e177b-191">Create a manager authorization handler</span></span>

<span data-ttu-id="e177b-192">Tworzenie `ContactManagerAuthorizationHandler` klasy w *autoryzacji* folderu.</span><span class="sxs-lookup"><span data-stu-id="e177b-192">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="e177b-193">`ContactManagerAuthorizationHandler` Weryfikuje użytkownika, działając w zasobie menedżera.</span><span class="sxs-lookup"><span data-stu-id="e177b-193">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="e177b-194">Tylko menedżerowie mogli zatwierdzać lub odrzucać zmiany zawartości (nowe lub zmienione).</span><span class="sxs-lookup"><span data-stu-id="e177b-194">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="e177b-195">Utwórz procedurę obsługi autoryzacji administratora</span><span class="sxs-lookup"><span data-stu-id="e177b-195">Create an administrator authorization handler</span></span>

<span data-ttu-id="e177b-196">Tworzenie `ContactAdministratorsAuthorizationHandler` klasy w *autoryzacji* folderu.</span><span class="sxs-lookup"><span data-stu-id="e177b-196">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="e177b-197">`ContactAdministratorsAuthorizationHandler` Weryfikuje użytkownika na zasób jest administratorem.</span><span class="sxs-lookup"><span data-stu-id="e177b-197">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="e177b-198">Administrator może wykonać wszystkie operacje.</span><span class="sxs-lookup"><span data-stu-id="e177b-198">Administrator can do all operations.</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="e177b-199">Zarejestruj procedury obsługi autoryzacji</span><span class="sxs-lookup"><span data-stu-id="e177b-199">Register the authorization handlers</span></span>

<span data-ttu-id="e177b-200">Usługi za pomocą platformy Entity Framework Core musi być zarejestrowana do [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) przy użyciu [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span><span class="sxs-lookup"><span data-stu-id="e177b-200">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="e177b-201">`ContactIsOwnerAuthorizationHandler` Korzysta z platformy ASP.NET Core [tożsamości](xref:security/authentication/identity), która jest oparta na platformy Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="e177b-201">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="e177b-202">Zarejestruj procedury obsługi za pomocą kolekcji usługi, dzięki czemu są one dostępne do `ContactsController` za pośrednictwem [wstrzykiwanie zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="e177b-202">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="e177b-203">Dodaj następujący kod na końcu `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="e177b-203">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet_defaultPolicy&highlight=23-99)]

<span data-ttu-id="e177b-204">`ContactAdministratorsAuthorizationHandler` i `ContactManagerAuthorizationHandler` są dodawane jako pojedynczych wystąpień.</span><span class="sxs-lookup"><span data-stu-id="e177b-204">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="e177b-205">Są one pojedynczych wystąpień, ponieważ nie używają programu EF, a wszystkie informacje wymagane w `Context` parametru `HandleRequirementAsync` metody.</span><span class="sxs-lookup"><span data-stu-id="e177b-205">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="e177b-206">Obsługuje autoryzację</span><span class="sxs-lookup"><span data-stu-id="e177b-206">Support authorization</span></span>

<span data-ttu-id="e177b-207">W tej sekcji służy do aktualizacji stron Razor i Dodaj klasę wymagania dotyczące operacji.</span><span class="sxs-lookup"><span data-stu-id="e177b-207">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="e177b-208">Przegląd klasy wymagania dotyczące operacji kontaktu</span><span class="sxs-lookup"><span data-stu-id="e177b-208">Review the contact operations requirements class</span></span>

<span data-ttu-id="e177b-209">Przegląd `ContactOperations` klasy.</span><span class="sxs-lookup"><span data-stu-id="e177b-209">Review the `ContactOperations` class.</span></span> <span data-ttu-id="e177b-210">Ta klasa zawiera wymagania obsługiwanej przez aplikację:</span><span class="sxs-lookup"><span data-stu-id="e177b-210">This class contains the requirements the app supports:</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-razor-pages"></a><span data-ttu-id="e177b-211">Utwórz klasę bazową dla stron Razor kontaktów</span><span class="sxs-lookup"><span data-stu-id="e177b-211">Create a base class for the Contacts Razor Pages</span></span>

<span data-ttu-id="e177b-212">Utwórz klasę bazową, zawierający usług wykorzystanych w kontaktach stron Razor.</span><span class="sxs-lookup"><span data-stu-id="e177b-212">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="e177b-213">Klasa bazowa umieszcza kod inicjowania w jednej lokalizacji:</span><span class="sxs-lookup"><span data-stu-id="e177b-213">The base class puts the initialization code in one location:</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="e177b-214">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="e177b-214">The preceding code:</span></span>

* <span data-ttu-id="e177b-215">Dodaje `IAuthorizationService` usługi w celu uzyskania dostępu do obsługi autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="e177b-215">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="e177b-216">Dodaje tożsamości `UserManager` usługi.</span><span class="sxs-lookup"><span data-stu-id="e177b-216">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="e177b-217">Dodaj `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="e177b-217">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="e177b-218">Aktualizacja CreateModel</span><span class="sxs-lookup"><span data-stu-id="e177b-218">Update the CreateModel</span></span>

<span data-ttu-id="e177b-219">Aktualizuj Konstruktor Tworzenie strony modelu do użycia `DI_BasePageModel` klasa bazowa:</span><span class="sxs-lookup"><span data-stu-id="e177b-219">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="e177b-220">Aktualizacja `CreateModel.OnPostAsync` metody:</span><span class="sxs-lookup"><span data-stu-id="e177b-220">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="e177b-221">Dodaj identyfikator użytkownika, który `Contact` modelu.</span><span class="sxs-lookup"><span data-stu-id="e177b-221">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="e177b-222">Wywołania obsługi autoryzacji, aby sprawdzić, czy użytkownik ma uprawnienia do tworzenia kontaktów.</span><span class="sxs-lookup"><span data-stu-id="e177b-222">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="e177b-223">Aktualizacja IndexModel</span><span class="sxs-lookup"><span data-stu-id="e177b-223">Update the IndexModel</span></span>

<span data-ttu-id="e177b-224">Aktualizacja `OnGetAsync` metody, więc tylko zatwierdzonego kontakty są wyświetlane użytkownikom ogólne:</span><span class="sxs-lookup"><span data-stu-id="e177b-224">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="e177b-225">Aktualizacja EditModel</span><span class="sxs-lookup"><span data-stu-id="e177b-225">Update the EditModel</span></span>

<span data-ttu-id="e177b-226">Dodaj program obsługi autoryzacji, aby sprawdzić, czy użytkownik jest właścicielem kontaktu.</span><span class="sxs-lookup"><span data-stu-id="e177b-226">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="e177b-227">Ponieważ autoryzacji zasobów jest weryfikowany, `[Authorize]` atrybut nie jest wystarczająca.</span><span class="sxs-lookup"><span data-stu-id="e177b-227">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="e177b-228">Aplikacja nie ma dostępu do zasobu, gdy atrybuty są oceniane.</span><span class="sxs-lookup"><span data-stu-id="e177b-228">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="e177b-229">Autoryzacja na podstawie zasobów musi być imperatywnego.</span><span class="sxs-lookup"><span data-stu-id="e177b-229">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="e177b-230">Testy muszą być wykonywane, gdy aplikacja ma dostęp do zasobu przez załadowanie go w modelu strony lub przez załadowanie go w ramach programu obsługi, sam.</span><span class="sxs-lookup"><span data-stu-id="e177b-230">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="e177b-231">Często dostęp do zasobu, przekazując klucz zasobu.</span><span class="sxs-lookup"><span data-stu-id="e177b-231">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="e177b-232">Aktualizacja DeleteModel</span><span class="sxs-lookup"><span data-stu-id="e177b-232">Update the DeleteModel</span></span>

<span data-ttu-id="e177b-233">Aktualizowanie modelu strony delete na potrzeby obsługi autoryzacji upewnij się, że użytkownik ma odpowiednie uprawnienia do usuwania dla kontaktu.</span><span class="sxs-lookup"><span data-stu-id="e177b-233">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="e177b-234">Wstrzyknięcie usługi autoryzacji do widoków</span><span class="sxs-lookup"><span data-stu-id="e177b-234">Inject the authorization service into the views</span></span>

<span data-ttu-id="e177b-235">Obecnie pokazuje interfejsu użytkownika edytowania i usuwania łącza kontaktów, do których użytkownik nie może modyfikować.</span><span class="sxs-lookup"><span data-stu-id="e177b-235">Currently, the UI shows edit and delete links for contacts the user can't modify.</span></span>

<span data-ttu-id="e177b-236">Wsuń usługę autoryzacji w pliku *Pages/_ViewImports. cshtml* , aby była dostępna dla wszystkich widoków:</span><span class="sxs-lookup"><span data-stu-id="e177b-236">Inject the authorization service in the *Pages/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[](secure-data/samples/final3/Pages/_ViewImports.cshtml?highlight=6-99)]

<span data-ttu-id="e177b-237">Poprzedni kod znaczników umożliwia dodanie kilku `using` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="e177b-237">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="e177b-238">Aktualizacja **Edytuj** i **Usuń** linki w *Pages/Contacts/Index.cshtml* więc tylko są one renderowane dla użytkowników z odpowiednimi uprawnieniami:</span><span class="sxs-lookup"><span data-stu-id="e177b-238">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[](secure-data/samples/final3/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> <span data-ttu-id="e177b-239">Ukrywanie łączy na podstawie użytkowników, którzy nie mają uprawnień do zmiany danych nie zabezpieczenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e177b-239">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="e177b-240">Ukrywanie łącza sprawia, że bardziej przyjazny dla użytkownika aplikacji, wyświetlając tylko poprawne linki.</span><span class="sxs-lookup"><span data-stu-id="e177b-240">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="e177b-241">Użytkownicy mogą hack wygenerowanego adresy URL, aby wywołać Edytuj i Usuń operacje na danych, które nie są właścicielami.</span><span class="sxs-lookup"><span data-stu-id="e177b-241">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="e177b-242">Strona Razor lub kontrolera musi wymuszają operacje sprawdzania dostępu do zabezpieczania danych.</span><span class="sxs-lookup"><span data-stu-id="e177b-242">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="e177b-243">Szczegóły aktualizacji</span><span class="sxs-lookup"><span data-stu-id="e177b-243">Update Details</span></span>

<span data-ttu-id="e177b-244">Zaktualizuj widok szczegółów, aby menedżerowie mogli zatwierdzać lub odrzucać kontaktów:</span><span class="sxs-lookup"><span data-stu-id="e177b-244">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml?name=snippet)]

<span data-ttu-id="e177b-245">Aktualizowanie modelu strony szczegółów:</span><span class="sxs-lookup"><span data-stu-id="e177b-245">Update the details page model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="add-or-remove-a-user-to-a-role"></a><span data-ttu-id="e177b-246">Dodawanie lub usuwanie użytkownika do roli</span><span class="sxs-lookup"><span data-stu-id="e177b-246">Add or remove a user to a role</span></span>

<span data-ttu-id="e177b-247">Zobacz [ten problem](https://github.com/aspnet/AspNetCore.Docs/issues/8502) uzyskać informacji na temat:</span><span class="sxs-lookup"><span data-stu-id="e177b-247">See [this issue](https://github.com/aspnet/AspNetCore.Docs/issues/8502) for information on:</span></span>

* <span data-ttu-id="e177b-248">Usuwanie uprawnień z użytkownikiem.</span><span class="sxs-lookup"><span data-stu-id="e177b-248">Removing privileges from a user.</span></span> <span data-ttu-id="e177b-249">Na przykład wyciszenie użytkownika w aplikacji czatu.</span><span class="sxs-lookup"><span data-stu-id="e177b-249">For example, muting a user in a chat app.</span></span>
* <span data-ttu-id="e177b-250">Dodawanie uprawnień dla użytkownika.</span><span class="sxs-lookup"><span data-stu-id="e177b-250">Adding privileges to a user.</span></span>

## <a name="test-the-completed-app"></a><span data-ttu-id="e177b-251">Testowanie aplikacji ukończone</span><span class="sxs-lookup"><span data-stu-id="e177b-251">Test the completed app</span></span>

<span data-ttu-id="e177b-252">Jeśli nie został jeszcze ustawiony hasła dla kont użytkowników wypełnionych, użyj [narzędzie Menedżer klucz tajny](xref:security/app-secrets#secret-manager) ustawić hasło:</span><span class="sxs-lookup"><span data-stu-id="e177b-252">If you haven't already set a password for seeded user accounts, use the [Secret Manager tool](xref:security/app-secrets#secret-manager) to set a password:</span></span>

* <span data-ttu-id="e177b-253">Wybierz silne hasło: Użyj ośmiu lub więcej znaków i co najmniej jednego znaku wielkie litery, cyfry i symbolu.</span><span class="sxs-lookup"><span data-stu-id="e177b-253">Choose a strong password: Use eight or more characters and at least one upper-case character, number, and symbol.</span></span> <span data-ttu-id="e177b-254">Na przykład `Passw0rd!` spełnia wymagania silne hasło.</span><span class="sxs-lookup"><span data-stu-id="e177b-254">For example, `Passw0rd!` meets the strong password requirements.</span></span>
* <span data-ttu-id="e177b-255">Wykonaj następujące polecenie z folderu projektu, gdzie `<PW>` jest hasłem:</span><span class="sxs-lookup"><span data-stu-id="e177b-255">Execute the following command from the project's folder, where `<PW>` is the password:</span></span>

  ```console
  dotnet user-secrets set SeedUserPW <PW>
  ```

<span data-ttu-id="e177b-256">Jeśli aplikacja ma kontaktów:</span><span class="sxs-lookup"><span data-stu-id="e177b-256">If the app has contacts:</span></span>

* <span data-ttu-id="e177b-257">Usuń wszystkie rekordy w `Contact` tabeli.</span><span class="sxs-lookup"><span data-stu-id="e177b-257">Delete all of the records in the `Contact` table.</span></span>
* <span data-ttu-id="e177b-258">Ponowne uruchomienie aplikacji w celu umieszczenia bazy danych.</span><span class="sxs-lookup"><span data-stu-id="e177b-258">Restart the app to seed the database.</span></span>

<span data-ttu-id="e177b-259">Łatwe testowanie ukończonej aplikacji jest do uruchomienia w trzech różnych przeglądarek (lub sesji incognito/InPrivate).</span><span class="sxs-lookup"><span data-stu-id="e177b-259">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate sessions).</span></span> <span data-ttu-id="e177b-260">W jednej przeglądarki zarejestrować nowego użytkownika (na przykład `test@example.com`).</span><span class="sxs-lookup"><span data-stu-id="e177b-260">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="e177b-261">Zaloguj się w każdej przeglądarce z innym użytkownikiem.</span><span class="sxs-lookup"><span data-stu-id="e177b-261">Sign in to each browser with a different user.</span></span> <span data-ttu-id="e177b-262">Sprawdź następujące operacje:</span><span class="sxs-lookup"><span data-stu-id="e177b-262">Verify the following operations:</span></span>

* <span data-ttu-id="e177b-263">Zarejestrowani użytkownicy można wyświetlić wszystkie zatwierdzone dane kontaktowe.</span><span class="sxs-lookup"><span data-stu-id="e177b-263">Registered users can view all of the approved contact data.</span></span>
* <span data-ttu-id="e177b-264">Zarejestrowani użytkownicy może edytować/usuwać swoje dane.</span><span class="sxs-lookup"><span data-stu-id="e177b-264">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="e177b-265">Menedżerowie mogą zatwierdzać/Odrzuć dane kontaktowe.</span><span class="sxs-lookup"><span data-stu-id="e177b-265">Managers can approve/reject contact data.</span></span> <span data-ttu-id="e177b-266">`Details` Wyświetlić pokazuje **Zatwierdź** i **Odrzuć** przycisków.</span><span class="sxs-lookup"><span data-stu-id="e177b-266">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="e177b-267">Administratorzy mogą zatwierdzać/Odrzuć i edytowanie/usuwanie wszystkich danych.</span><span class="sxs-lookup"><span data-stu-id="e177b-267">Administrators can approve/reject and edit/delete all data.</span></span>

| <span data-ttu-id="e177b-268">Użytkownik</span><span class="sxs-lookup"><span data-stu-id="e177b-268">User</span></span>                | <span data-ttu-id="e177b-269">Zasilany przez aplikację</span><span class="sxs-lookup"><span data-stu-id="e177b-269">Seeded by the app</span></span> | <span data-ttu-id="e177b-270">Opcje</span><span class="sxs-lookup"><span data-stu-id="e177b-270">Options</span></span>                                  |
| ------------------- | :---------------: | ---------------------------------------- |
| test@example.com    | <span data-ttu-id="e177b-271">Nie</span><span class="sxs-lookup"><span data-stu-id="e177b-271">No</span></span>                | <span data-ttu-id="e177b-272">Edytowanie/usuwanie własnych danych.</span><span class="sxs-lookup"><span data-stu-id="e177b-272">Edit/delete the own data.</span></span>                |
| manager@contoso.com | <span data-ttu-id="e177b-273">Tak</span><span class="sxs-lookup"><span data-stu-id="e177b-273">Yes</span></span>               | <span data-ttu-id="e177b-274">Zatwierdź/Odrzuć i edytowanie/usuwanie własnych danych.</span><span class="sxs-lookup"><span data-stu-id="e177b-274">Approve/reject and edit/delete own data.</span></span> |
| admin@contoso.com   | <span data-ttu-id="e177b-275">Tak</span><span class="sxs-lookup"><span data-stu-id="e177b-275">Yes</span></span>               | <span data-ttu-id="e177b-276">Zatwierdź/Odrzuć i edytowanie/usuwanie wszystkich danych.</span><span class="sxs-lookup"><span data-stu-id="e177b-276">Approve/reject and edit/delete all data.</span></span> |

<span data-ttu-id="e177b-277">Utwórz kontakt w przeglądarce administratora.</span><span class="sxs-lookup"><span data-stu-id="e177b-277">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="e177b-278">Skopiuj adres URL do usunięcia, a następnie Edytuj z skontaktowanie się z administratorem.</span><span class="sxs-lookup"><span data-stu-id="e177b-278">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="e177b-279">Wklej te linki do przeglądarki użytkownika testowego, aby sprawdzić, czy użytkownik testu nie może wykonywać te operacje.</span><span class="sxs-lookup"><span data-stu-id="e177b-279">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="e177b-280">Utwórz aplikację startową</span><span class="sxs-lookup"><span data-stu-id="e177b-280">Create the starter app</span></span>

* <span data-ttu-id="e177b-281">Tworzenie stron Razor aplikacji o nazwie "ContactManager"</span><span class="sxs-lookup"><span data-stu-id="e177b-281">Create a Razor Pages app named "ContactManager"</span></span>
  * <span data-ttu-id="e177b-282">Tworzenie aplikacji za pomocą **indywidualne konta użytkowników**.</span><span class="sxs-lookup"><span data-stu-id="e177b-282">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="e177b-283">Nadaj mu nazwę "ContactManager", przestrzeń nazw używaną w próbce pasujących przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="e177b-283">Name it "ContactManager" so the namespace matches the namespace used in the sample.</span></span>
  * <span data-ttu-id="e177b-284">`-uld` Określa LocalDB zamiast bazy danych SQLite</span><span class="sxs-lookup"><span data-stu-id="e177b-284">`-uld` specifies LocalDB instead of SQLite</span></span>

  ```console
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* <span data-ttu-id="e177b-285">Dodaj *modele/Contact. cs*:</span><span class="sxs-lookup"><span data-stu-id="e177b-285">Add *Models/Contact.cs*:</span></span>

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="e177b-286">Tworzenie szkieletu `Contact` modelu.</span><span class="sxs-lookup"><span data-stu-id="e177b-286">Scaffold the `Contact` model.</span></span>
* <span data-ttu-id="e177b-287">Tworzenie początkowej migracji i aktualizowanie bazy danych:</span><span class="sxs-lookup"><span data-stu-id="e177b-287">Create initial migration and update the database:</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet tool install -g dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
dotnet ef database drop -f
dotnet ef migrations add initial
dotnet ef database update
  ```

<span data-ttu-id="e177b-288">Jeśli wystąpi usterka z `dotnet aspnet-codegenerator razorpage` poleceniem, zobacz [ten problem](https://github.com/aspnet/Scaffolding/issues/984)w usłudze GitHub.</span><span class="sxs-lookup"><span data-stu-id="e177b-288">If you experience a bug with the `dotnet aspnet-codegenerator razorpage` command, see [this GitHub issue](https://github.com/aspnet/Scaffolding/issues/984).</span></span>

* <span data-ttu-id="e177b-289">Zaktualizuj kotwicę **ContactManager** w pliku *Pages/Shared/_Layout. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="e177b-289">Update the **ContactManager** anchor in the *Pages/Shared/_Layout.cshtml* file:</span></span>

 ```cshtml
<a class="navbar-brand" asp-area="" asp-page="/Contacts/Index">ContactManager</a>
  ```

* <span data-ttu-id="e177b-290">Przetestuj aplikację, tworzenia, edytowania i usuwania kontaktu</span><span class="sxs-lookup"><span data-stu-id="e177b-290">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="e177b-291">Inicjowanie bazy danych</span><span class="sxs-lookup"><span data-stu-id="e177b-291">Seed the database</span></span>

<span data-ttu-id="e177b-292">Dodaj klasę [SeedData](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter3/Data/SeedData.cs) do folderu *danych* :</span><span class="sxs-lookup"><span data-stu-id="e177b-292">Add the [SeedData](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter3/Data/SeedData.cs) class to the *Data* folder:</span></span>

[!code-csharp[](secure-data/samples/starter3/Data/SeedData.cs)]

<span data-ttu-id="e177b-293">Wywołaj `SeedData.Initialize` z `Main`:</span><span class="sxs-lookup"><span data-stu-id="e177b-293">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[](secure-data/samples/starter3/Program.cs)]

<span data-ttu-id="e177b-294">Sprawdź, czy aplikacja zasilany bazy danych.</span><span class="sxs-lookup"><span data-stu-id="e177b-294">Test that the app seeded the database.</span></span> <span data-ttu-id="e177b-295">W przypadku wszystkich wierszy w skontaktuj się z bazy danych, metoda inicjatora nie zostanie uruchomiony.</span><span class="sxs-lookup"><span data-stu-id="e177b-295">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1 < aspnetcore-3.0"

<span data-ttu-id="e177b-296">W tym samouczku pokazano, jak utworzyć aplikację sieci web platformy ASP.NET Core przy użyciu danych użytkownika chronionych przez autoryzację.</span><span class="sxs-lookup"><span data-stu-id="e177b-296">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="e177b-297">Wyświetla listę kontaktów, uwierzytelnionych użytkowników (zarejestrowane), które zostały utworzone.</span><span class="sxs-lookup"><span data-stu-id="e177b-297">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="e177b-298">Istnieją trzy grupy zabezpieczeń:</span><span class="sxs-lookup"><span data-stu-id="e177b-298">There are three security groups:</span></span>

* <span data-ttu-id="e177b-299">**Liczba zarejestrowanych użytkowników** może wyświetlać wszystkie zatwierdzone dane i może edytować/usuwać swoje dane.</span><span class="sxs-lookup"><span data-stu-id="e177b-299">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="e177b-300">**Menedżerowie** można zatwierdzić lub odrzucić dane kontaktowe.</span><span class="sxs-lookup"><span data-stu-id="e177b-300">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="e177b-301">Tylko zatwierdzone kontakty będą widoczne dla użytkowników.</span><span class="sxs-lookup"><span data-stu-id="e177b-301">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="e177b-302">**Administratorzy** można zatwierdzić/Odrzuć i edytowanie/usuwanie żadnych danych.</span><span class="sxs-lookup"><span data-stu-id="e177b-302">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="e177b-303">Na poniższej ilustracji użytkownik Rick (`rick@example.com`) jest zalogowany.</span><span class="sxs-lookup"><span data-stu-id="e177b-303">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="e177b-304">Rick mogą jedynie wyświetlać kontakty zatwierdzone i **Edytuj**/**Usuń**/**Utwórz nowy** linki do swoich kontaktów.</span><span class="sxs-lookup"><span data-stu-id="e177b-304">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="e177b-305">Tylko ostatni rekord, utworzone przez Rick, wyświetla **Edytuj** i **Usuń** łącza.</span><span class="sxs-lookup"><span data-stu-id="e177b-305">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="e177b-306">Inni użytkownicy nie zobaczą ostatni rekord, aż Menedżer lub administrator zmienia stan na "Zatwierdzone".</span><span class="sxs-lookup"><span data-stu-id="e177b-306">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![Zrzut ekranu przedstawiający Rick zalogowany](secure-data/_static/rick.png)

<span data-ttu-id="e177b-308">Na poniższej ilustracji `manager@contoso.com` jest zalogowany i w roli menedżera:</span><span class="sxs-lookup"><span data-stu-id="e177b-308">In the following image, `manager@contoso.com` is signed in and in the manager's role:</span></span>

![Zrzut ekranu przedstawiający manager@contoso.com zalogowany](secure-data/_static/manager1.png)

<span data-ttu-id="e177b-310">Na poniższej ilustracji przedstawiono menedżerów widoku szczegółów kontaktu:</span><span class="sxs-lookup"><span data-stu-id="e177b-310">The following image shows the managers details view of a contact:</span></span>

![Widok menedżera kontaktu](secure-data/_static/manager.png)

<span data-ttu-id="e177b-312">**Zatwierdź** i **Odrzuć** przyciski są wyświetlane tylko dla menedżerów i administratorów.</span><span class="sxs-lookup"><span data-stu-id="e177b-312">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="e177b-313">Na poniższej ilustracji `admin@contoso.com` jest zalogowany i w roli administratora:</span><span class="sxs-lookup"><span data-stu-id="e177b-313">In the following image, `admin@contoso.com` is signed in and in the administrator's role:</span></span>

![Zrzut ekranu przedstawiający admin@contoso.com zalogowany](secure-data/_static/admin.png)

<span data-ttu-id="e177b-315">Administrator ma wszystkie uprawnienia.</span><span class="sxs-lookup"><span data-stu-id="e177b-315">The administrator has all privileges.</span></span> <span data-ttu-id="e177b-316">Ona można odczytu/edytowanie/usuwanie dowolnego skontaktuj się z pomocą i zmienić stan kontaktów.</span><span class="sxs-lookup"><span data-stu-id="e177b-316">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="e177b-317">Aplikacja została utworzona przez [tworzenia szkieletów](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) następujące `Contact` modelu:</span><span class="sxs-lookup"><span data-stu-id="e177b-317">The app was created by [scaffolding](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) the following `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

<span data-ttu-id="e177b-318">Przykład zawiera poniższe obsługi autoryzacji:</span><span class="sxs-lookup"><span data-stu-id="e177b-318">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="e177b-319">`ContactIsOwnerAuthorizationHandler`: Zapewnia, że użytkownik może edytować tylko swoje dane.</span><span class="sxs-lookup"><span data-stu-id="e177b-319">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="e177b-320">`ContactManagerAuthorizationHandler`: Umożliwia menedżerom zatwierdzanie lub odrzucanie kontaktów.</span><span class="sxs-lookup"><span data-stu-id="e177b-320">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="e177b-321">`ContactAdministratorsAuthorizationHandler`: Umożliwia administratorom zatwierdzanie lub odrzucanie kontaktów oraz Edytowanie/usuwanie kontaktów.</span><span class="sxs-lookup"><span data-stu-id="e177b-321">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e177b-322">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="e177b-322">Prerequisites</span></span>

<span data-ttu-id="e177b-323">W tym samouczku jest zaawansowany.</span><span class="sxs-lookup"><span data-stu-id="e177b-323">This tutorial is advanced.</span></span> <span data-ttu-id="e177b-324">Należy zapoznać się z:</span><span class="sxs-lookup"><span data-stu-id="e177b-324">You should be familiar with:</span></span>

* [<span data-ttu-id="e177b-325">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e177b-325">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="e177b-326">Uwierzytelnianie</span><span class="sxs-lookup"><span data-stu-id="e177b-326">Authentication</span></span>](xref:security/authentication/identity)
* [<span data-ttu-id="e177b-327">Potwierdzenie konta i odzyskiwanie hasła</span><span class="sxs-lookup"><span data-stu-id="e177b-327">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="e177b-328">Autoryzacja</span><span class="sxs-lookup"><span data-stu-id="e177b-328">Authorization</span></span>](xref:security/authorization/introduction)
* [<span data-ttu-id="e177b-329">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="e177b-329">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="e177b-330">Starter i ukończonej aplikacji</span><span class="sxs-lookup"><span data-stu-id="e177b-330">The starter and completed app</span></span>

<span data-ttu-id="e177b-331">[Pobierz](xref:index#how-to-download-a-sample) [ukończone](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e177b-331">[Download](xref:index#how-to-download-a-sample) the [completed](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) app.</span></span> <span data-ttu-id="e177b-332">[Test](#test-the-completed-app) ukończonej aplikacji, dzięki czemu można zapoznać się z jej funkcjami zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="e177b-332">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="e177b-333">Aplikację startową</span><span class="sxs-lookup"><span data-stu-id="e177b-333">The starter app</span></span>

<span data-ttu-id="e177b-334">[Pobierz](xref:index#how-to-download-a-sample) [starter](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e177b-334">[Download](xref:index#how-to-download-a-sample) the [starter](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) app.</span></span>

<span data-ttu-id="e177b-335">Uruchom aplikację, naciśnij przycisk **ContactManager** link i sprawdź, tworzenie, edytowanie i usuwanie kontaktu.</span><span class="sxs-lookup"><span data-stu-id="e177b-335">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="e177b-336">Zabezpieczanie danych użytkownika</span><span class="sxs-lookup"><span data-stu-id="e177b-336">Secure user data</span></span>

<span data-ttu-id="e177b-337">Poniższe sekcje mają główne kroki umożliwiające utworzenie aplikacji danych bezpiecznego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="e177b-337">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="e177b-338">Może okazać się przydatne do odwoływania się do projektu ukończona.</span><span class="sxs-lookup"><span data-stu-id="e177b-338">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="e177b-339">Powiąż dane kontaktowe dla użytkownika</span><span class="sxs-lookup"><span data-stu-id="e177b-339">Tie the contact data to the user</span></span>

<span data-ttu-id="e177b-340">Za pomocą programu ASP.NET [tożsamości](xref:security/authentication/identity) identyfikator użytkownika w celu zapewnienia, że użytkownicy mogą edytować swoje dane, ale nie dane innych użytkowników.</span><span class="sxs-lookup"><span data-stu-id="e177b-340">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="e177b-341">Dodaj `OwnerID` i `ContactStatus` do `Contact` modelu:</span><span class="sxs-lookup"><span data-stu-id="e177b-341">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

<span data-ttu-id="e177b-342">`OwnerID` Identyfikator użytkownika z `AspNetUser` tabelę [tożsamości](xref:security/authentication/identity) bazy danych.</span><span class="sxs-lookup"><span data-stu-id="e177b-342">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="e177b-343">`Status` Pola określa, czy kontakt jest widoczny dla zwykłych użytkowników.</span><span class="sxs-lookup"><span data-stu-id="e177b-343">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="e177b-344">Tworzenie nowej migracji i aktualizowanie bazy danych:</span><span class="sxs-lookup"><span data-stu-id="e177b-344">Create a new migration and update the database:</span></span>

```console
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-identity"></a><span data-ttu-id="e177b-345">Dodaj usługi ról do tożsamości</span><span class="sxs-lookup"><span data-stu-id="e177b-345">Add Role services to Identity</span></span>

<span data-ttu-id="e177b-346">Dołącz [opcji Dodawanie ról](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) można dodać usługi ról:</span><span class="sxs-lookup"><span data-stu-id="e177b-346">Append [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) to add Role services:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet2&highlight=12)]

### <a name="require-authenticated-users"></a><span data-ttu-id="e177b-347">Wymaga uwierzytelnionych użytkowników</span><span class="sxs-lookup"><span data-stu-id="e177b-347">Require authenticated users</span></span>

<span data-ttu-id="e177b-348">Ustaw domyślne zasady uwierzytelniania, aby wymagać od użytkowników uwierzytelniania:</span><span class="sxs-lookup"><span data-stu-id="e177b-348">Set the default authentication policy to require users to be authenticated:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet&highlight=17-99)] 

 <span data-ttu-id="e177b-349">Użytkownik może zrezygnować z uwierzytelniania na poziomie metody strony Razor, kontrolera lub akcji z `[AllowAnonymous]` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="e177b-349">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="e177b-350">Ustawienie domyślne zasady uwierzytelniania, aby wymagać od użytkowników uwierzytelniania chroni nowo dodanych stronami Razor i kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="e177b-350">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="e177b-351">Posiadanie uwierzytelniania domyślnie wymagane jest bezpieczniejszy niż opierając się na nowych kontrolerów i stron Razor do uwzględnienia `[Authorize]` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="e177b-351">Having authentication required by default is more secure than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span>

<span data-ttu-id="e177b-352">Dodaj [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) do indeksu o i skontaktuj się z pomocą stron, użytkowników anonimowych można uzyskać informacji o lokacji, przed ich zarejestrowania.</span><span class="sxs-lookup"><span data-stu-id="e177b-352">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index, About, and Contact pages so anonymous users can get information about the site before they register.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Index.cshtml.cs?highlight=1,6)]

### <a name="configure-the-test-account"></a><span data-ttu-id="e177b-353">Skonfiguruj konto testu</span><span class="sxs-lookup"><span data-stu-id="e177b-353">Configure the test account</span></span>

<span data-ttu-id="e177b-354">`SeedData` Klasy tworzy dwa konta: administrator i Menedżer.</span><span class="sxs-lookup"><span data-stu-id="e177b-354">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="e177b-355">Użyj [narzędzie Menedżer klucz tajny](xref:security/app-secrets) ustawić hasło dla tych kont.</span><span class="sxs-lookup"><span data-stu-id="e177b-355">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="e177b-356">Ustaw hasło z katalogu projektu (katalogu zawierającego *Program.cs*):</span><span class="sxs-lookup"><span data-stu-id="e177b-356">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```console
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="e177b-357">Jeśli nie określono silne hasło, wyjątek jest generowany, gdy `SeedData.Initialize` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="e177b-357">If a strong password is not specified, an exception is thrown when `SeedData.Initialize` is called.</span></span>

<span data-ttu-id="e177b-358">Aktualizacja `Main` hasła testu:</span><span class="sxs-lookup"><span data-stu-id="e177b-358">Update `Main` to use the test password:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="e177b-359">Tworzenie konta testowe i aktualizowanie kontaktów</span><span class="sxs-lookup"><span data-stu-id="e177b-359">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="e177b-360">Aktualizacja `Initialize` method in Class metoda `SeedData` klasy w celu utworzenia konta testowe:</span><span class="sxs-lookup"><span data-stu-id="e177b-360">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="e177b-361">Dodaj identyfikator użytkownika administratora i `ContactStatus` do kontaktów.</span><span class="sxs-lookup"><span data-stu-id="e177b-361">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="e177b-362">Określ jeden z kontaktów "Przesłane" i jednym "odrzucone".</span><span class="sxs-lookup"><span data-stu-id="e177b-362">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="e177b-363">Dodawanie stanu i Identyfikatora użytkownika do wszystkich kontaktów.</span><span class="sxs-lookup"><span data-stu-id="e177b-363">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="e177b-364">Wyświetlane jest tylko jeden kontakt:</span><span class="sxs-lookup"><span data-stu-id="e177b-364">Only one contact is shown:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="e177b-365">Utwórz właściciela, Menedżer i obsługi autoryzacji administratora</span><span class="sxs-lookup"><span data-stu-id="e177b-365">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="e177b-366">Utwórz folder *autoryzacji* i Utwórz `ContactIsOwnerAuthorizationHandler` w nim klasę.</span><span class="sxs-lookup"><span data-stu-id="e177b-366">Create an *Authorization* folder and create a `ContactIsOwnerAuthorizationHandler` class in it.</span></span> <span data-ttu-id="e177b-367">`ContactIsOwnerAuthorizationHandler` Sprawdza, czy użytkownik, działając w zasobie jest właścicielem zasobu.</span><span class="sxs-lookup"><span data-stu-id="e177b-367">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="e177b-368">`ContactIsOwnerAuthorizationHandler` Wywołania [kontekstu. Powodzenie](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) aktualnego użytkownika uwierzytelnionego jest skontaktuj się z właścicielem.</span><span class="sxs-lookup"><span data-stu-id="e177b-368">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="e177b-369">Programy obsługi autoryzacji zazwyczaj:</span><span class="sxs-lookup"><span data-stu-id="e177b-369">Authorization handlers generally:</span></span>

* <span data-ttu-id="e177b-370">Zwróć `context.Succeed` gdy spełniono wymagania.</span><span class="sxs-lookup"><span data-stu-id="e177b-370">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="e177b-371">Zwróć `Task.CompletedTask` gdy nie są spełnione wymagania.</span><span class="sxs-lookup"><span data-stu-id="e177b-371">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="e177b-372">`Task.CompletedTask`nie powiodło się lub&mdash;niepowodzenie — zezwala na uruchamianie innych programów obsługi autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="e177b-372">`Task.CompletedTask` is not success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="e177b-373">Jeśli potrzebujesz jawnie nie powiedzie się, zwraca [kontekstu. Niepowodzenie](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span><span class="sxs-lookup"><span data-stu-id="e177b-373">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="e177b-374">Aplikacja umożliwia skontaktuj się z pomocą właścicieli do edycji/delete/tworzenie własnych danych.</span><span class="sxs-lookup"><span data-stu-id="e177b-374">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="e177b-375">`ContactIsOwnerAuthorizationHandler` nie muszą sprawdzać operacji przekazane w parametrze wymagań.</span><span class="sxs-lookup"><span data-stu-id="e177b-375">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="e177b-376">Utwórz procedurę obsługi Menedżer autoryzacji</span><span class="sxs-lookup"><span data-stu-id="e177b-376">Create a manager authorization handler</span></span>

<span data-ttu-id="e177b-377">Tworzenie `ContactManagerAuthorizationHandler` klasy w *autoryzacji* folderu.</span><span class="sxs-lookup"><span data-stu-id="e177b-377">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="e177b-378">`ContactManagerAuthorizationHandler` Weryfikuje użytkownika, działając w zasobie menedżera.</span><span class="sxs-lookup"><span data-stu-id="e177b-378">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="e177b-379">Tylko menedżerowie mogli zatwierdzać lub odrzucać zmiany zawartości (nowe lub zmienione).</span><span class="sxs-lookup"><span data-stu-id="e177b-379">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="e177b-380">Utwórz procedurę obsługi autoryzacji administratora</span><span class="sxs-lookup"><span data-stu-id="e177b-380">Create an administrator authorization handler</span></span>

<span data-ttu-id="e177b-381">Tworzenie `ContactAdministratorsAuthorizationHandler` klasy w *autoryzacji* folderu.</span><span class="sxs-lookup"><span data-stu-id="e177b-381">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="e177b-382">`ContactAdministratorsAuthorizationHandler` Weryfikuje użytkownika na zasób jest administratorem.</span><span class="sxs-lookup"><span data-stu-id="e177b-382">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="e177b-383">Administrator może wykonać wszystkie operacje.</span><span class="sxs-lookup"><span data-stu-id="e177b-383">Administrator can do all operations.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="e177b-384">Zarejestruj procedury obsługi autoryzacji</span><span class="sxs-lookup"><span data-stu-id="e177b-384">Register the authorization handlers</span></span>

<span data-ttu-id="e177b-385">Usługi za pomocą platformy Entity Framework Core musi być zarejestrowana do [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) przy użyciu [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span><span class="sxs-lookup"><span data-stu-id="e177b-385">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="e177b-386">`ContactIsOwnerAuthorizationHandler` Korzysta z platformy ASP.NET Core [tożsamości](xref:security/authentication/identity), która jest oparta na platformy Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="e177b-386">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="e177b-387">Zarejestruj procedury obsługi za pomocą kolekcji usługi, dzięki czemu są one dostępne do `ContactsController` za pośrednictwem [wstrzykiwanie zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="e177b-387">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="e177b-388">Dodaj następujący kod na końcu `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="e177b-388">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet_defaultPolicy&highlight=27-99)]

<span data-ttu-id="e177b-389">`ContactAdministratorsAuthorizationHandler` i `ContactManagerAuthorizationHandler` są dodawane jako pojedynczych wystąpień.</span><span class="sxs-lookup"><span data-stu-id="e177b-389">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="e177b-390">Są one pojedynczych wystąpień, ponieważ nie używają programu EF, a wszystkie informacje wymagane w `Context` parametru `HandleRequirementAsync` metody.</span><span class="sxs-lookup"><span data-stu-id="e177b-390">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="e177b-391">Obsługuje autoryzację</span><span class="sxs-lookup"><span data-stu-id="e177b-391">Support authorization</span></span>

<span data-ttu-id="e177b-392">W tej sekcji służy do aktualizacji stron Razor i Dodaj klasę wymagania dotyczące operacji.</span><span class="sxs-lookup"><span data-stu-id="e177b-392">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="e177b-393">Przegląd klasy wymagania dotyczące operacji kontaktu</span><span class="sxs-lookup"><span data-stu-id="e177b-393">Review the contact operations requirements class</span></span>

<span data-ttu-id="e177b-394">Przegląd `ContactOperations` klasy.</span><span class="sxs-lookup"><span data-stu-id="e177b-394">Review the `ContactOperations` class.</span></span> <span data-ttu-id="e177b-395">Ta klasa zawiera wymagania obsługiwanej przez aplikację:</span><span class="sxs-lookup"><span data-stu-id="e177b-395">This class contains the requirements the app supports:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-razor-pages"></a><span data-ttu-id="e177b-396">Utwórz klasę bazową dla stron Razor kontaktów</span><span class="sxs-lookup"><span data-stu-id="e177b-396">Create a base class for the Contacts Razor Pages</span></span>

<span data-ttu-id="e177b-397">Utwórz klasę bazową, zawierający usług wykorzystanych w kontaktach stron Razor.</span><span class="sxs-lookup"><span data-stu-id="e177b-397">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="e177b-398">Klasa bazowa umieszcza kod inicjowania w jednej lokalizacji:</span><span class="sxs-lookup"><span data-stu-id="e177b-398">The base class puts the initialization code in one location:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="e177b-399">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="e177b-399">The preceding code:</span></span>

* <span data-ttu-id="e177b-400">Dodaje `IAuthorizationService` usługi w celu uzyskania dostępu do obsługi autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="e177b-400">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="e177b-401">Dodaje tożsamości `UserManager` usługi.</span><span class="sxs-lookup"><span data-stu-id="e177b-401">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="e177b-402">Dodaj `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="e177b-402">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="e177b-403">Aktualizacja CreateModel</span><span class="sxs-lookup"><span data-stu-id="e177b-403">Update the CreateModel</span></span>

<span data-ttu-id="e177b-404">Aktualizuj Konstruktor Tworzenie strony modelu do użycia `DI_BasePageModel` klasa bazowa:</span><span class="sxs-lookup"><span data-stu-id="e177b-404">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="e177b-405">Aktualizacja `CreateModel.OnPostAsync` metody:</span><span class="sxs-lookup"><span data-stu-id="e177b-405">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="e177b-406">Dodaj identyfikator użytkownika, który `Contact` modelu.</span><span class="sxs-lookup"><span data-stu-id="e177b-406">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="e177b-407">Wywołania obsługi autoryzacji, aby sprawdzić, czy użytkownik ma uprawnienia do tworzenia kontaktów.</span><span class="sxs-lookup"><span data-stu-id="e177b-407">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="e177b-408">Aktualizacja IndexModel</span><span class="sxs-lookup"><span data-stu-id="e177b-408">Update the IndexModel</span></span>

<span data-ttu-id="e177b-409">Aktualizacja `OnGetAsync` metody, więc tylko zatwierdzonego kontakty są wyświetlane użytkownikom ogólne:</span><span class="sxs-lookup"><span data-stu-id="e177b-409">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="e177b-410">Aktualizacja EditModel</span><span class="sxs-lookup"><span data-stu-id="e177b-410">Update the EditModel</span></span>

<span data-ttu-id="e177b-411">Dodaj program obsługi autoryzacji, aby sprawdzić, czy użytkownik jest właścicielem kontaktu.</span><span class="sxs-lookup"><span data-stu-id="e177b-411">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="e177b-412">Ponieważ autoryzacji zasobów jest weryfikowany, `[Authorize]` atrybut nie jest wystarczająca.</span><span class="sxs-lookup"><span data-stu-id="e177b-412">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="e177b-413">Aplikacja nie ma dostępu do zasobu, gdy atrybuty są oceniane.</span><span class="sxs-lookup"><span data-stu-id="e177b-413">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="e177b-414">Autoryzacja na podstawie zasobów musi być imperatywnego.</span><span class="sxs-lookup"><span data-stu-id="e177b-414">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="e177b-415">Testy muszą być wykonywane, gdy aplikacja ma dostęp do zasobu przez załadowanie go w modelu strony lub przez załadowanie go w ramach programu obsługi, sam.</span><span class="sxs-lookup"><span data-stu-id="e177b-415">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="e177b-416">Często dostęp do zasobu, przekazując klucz zasobu.</span><span class="sxs-lookup"><span data-stu-id="e177b-416">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="e177b-417">Aktualizacja DeleteModel</span><span class="sxs-lookup"><span data-stu-id="e177b-417">Update the DeleteModel</span></span>

<span data-ttu-id="e177b-418">Aktualizowanie modelu strony delete na potrzeby obsługi autoryzacji upewnij się, że użytkownik ma odpowiednie uprawnienia do usuwania dla kontaktu.</span><span class="sxs-lookup"><span data-stu-id="e177b-418">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="e177b-419">Wstrzyknięcie usługi autoryzacji do widoków</span><span class="sxs-lookup"><span data-stu-id="e177b-419">Inject the authorization service into the views</span></span>

<span data-ttu-id="e177b-420">Obecnie pokazuje interfejsu użytkownika edytowania i usuwania łącza kontaktów, do których użytkownik nie może modyfikować.</span><span class="sxs-lookup"><span data-stu-id="e177b-420">Currently, the UI shows edit and delete links for contacts the user can't modify.</span></span>

<span data-ttu-id="e177b-421">Wstrzykiwanie usługi autoryzacji w *Views/_ViewImports.cshtml* pliku, dzięki czemu są one dostępne dla wszystkich widoków:</span><span class="sxs-lookup"><span data-stu-id="e177b-421">Inject the authorization service in the *Views/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/_ViewImports.cshtml?highlight=6-99)]

<span data-ttu-id="e177b-422">Poprzedni kod znaczników umożliwia dodanie kilku `using` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="e177b-422">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="e177b-423">Aktualizacja **Edytuj** i **Usuń** linki w *Pages/Contacts/Index.cshtml* więc tylko są one renderowane dla użytkowników z odpowiednimi uprawnieniami:</span><span class="sxs-lookup"><span data-stu-id="e177b-423">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> <span data-ttu-id="e177b-424">Ukrywanie łączy na podstawie użytkowników, którzy nie mają uprawnień do zmiany danych nie zabezpieczenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e177b-424">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="e177b-425">Ukrywanie łącza sprawia, że bardziej przyjazny dla użytkownika aplikacji, wyświetlając tylko poprawne linki.</span><span class="sxs-lookup"><span data-stu-id="e177b-425">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="e177b-426">Użytkownicy mogą hack wygenerowanego adresy URL, aby wywołać Edytuj i Usuń operacje na danych, które nie są właścicielami.</span><span class="sxs-lookup"><span data-stu-id="e177b-426">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="e177b-427">Strona Razor lub kontrolera musi wymuszają operacje sprawdzania dostępu do zabezpieczania danych.</span><span class="sxs-lookup"><span data-stu-id="e177b-427">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="e177b-428">Szczegóły aktualizacji</span><span class="sxs-lookup"><span data-stu-id="e177b-428">Update Details</span></span>

<span data-ttu-id="e177b-429">Zaktualizuj widok szczegółów, aby menedżerowie mogli zatwierdzać lub odrzucać kontaktów:</span><span class="sxs-lookup"><span data-stu-id="e177b-429">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml?name=snippet)]

<span data-ttu-id="e177b-430">Aktualizowanie modelu strony szczegółów:</span><span class="sxs-lookup"><span data-stu-id="e177b-430">Update the details page model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="add-or-remove-a-user-to-a-role"></a><span data-ttu-id="e177b-431">Dodawanie lub usuwanie użytkownika do roli</span><span class="sxs-lookup"><span data-stu-id="e177b-431">Add or remove a user to a role</span></span>

<span data-ttu-id="e177b-432">Zobacz [ten problem](https://github.com/aspnet/AspNetCore.Docs/issues/8502) uzyskać informacji na temat:</span><span class="sxs-lookup"><span data-stu-id="e177b-432">See [this issue](https://github.com/aspnet/AspNetCore.Docs/issues/8502) for information on:</span></span>

* <span data-ttu-id="e177b-433">Usuwanie uprawnień z użytkownikiem.</span><span class="sxs-lookup"><span data-stu-id="e177b-433">Removing privileges from a user.</span></span> <span data-ttu-id="e177b-434">Na przykład wyciszenie użytkownika w aplikacji czatu.</span><span class="sxs-lookup"><span data-stu-id="e177b-434">For example, muting a user in a chat app.</span></span>
* <span data-ttu-id="e177b-435">Dodawanie uprawnień dla użytkownika.</span><span class="sxs-lookup"><span data-stu-id="e177b-435">Adding privileges to a user.</span></span>

## <a name="test-the-completed-app"></a><span data-ttu-id="e177b-436">Testowanie aplikacji ukończone</span><span class="sxs-lookup"><span data-stu-id="e177b-436">Test the completed app</span></span>

<span data-ttu-id="e177b-437">Jeśli nie został jeszcze ustawiony hasła dla kont użytkowników wypełnionych, użyj [narzędzie Menedżer klucz tajny](xref:security/app-secrets#secret-manager) ustawić hasło:</span><span class="sxs-lookup"><span data-stu-id="e177b-437">If you haven't already set a password for seeded user accounts, use the [Secret Manager tool](xref:security/app-secrets#secret-manager) to set a password:</span></span>

* <span data-ttu-id="e177b-438">Wybierz silne hasło: Użyj ośmiu lub więcej znaków i co najmniej jednego znaku wielkie litery, cyfry i symbolu.</span><span class="sxs-lookup"><span data-stu-id="e177b-438">Choose a strong password: Use eight or more characters and at least one upper-case character, number, and symbol.</span></span> <span data-ttu-id="e177b-439">Na przykład `Passw0rd!` spełnia wymagania silne hasło.</span><span class="sxs-lookup"><span data-stu-id="e177b-439">For example, `Passw0rd!` meets the strong password requirements.</span></span>
* <span data-ttu-id="e177b-440">Wykonaj następujące polecenie z folderu projektu, gdzie `<PW>` jest hasłem:</span><span class="sxs-lookup"><span data-stu-id="e177b-440">Execute the following command from the project's folder, where `<PW>` is the password:</span></span>

  ```console
  dotnet user-secrets set SeedUserPW <PW>
  ```

* <span data-ttu-id="e177b-441">Porzuć i zaktualizuj bazę danych</span><span class="sxs-lookup"><span data-stu-id="e177b-441">Drop and update the Database</span></span>

    ```console
     dotnet ef database drop -f
     dotnet ef database update  
     ```

* <span data-ttu-id="e177b-442">Ponowne uruchomienie aplikacji w celu umieszczenia bazy danych.</span><span class="sxs-lookup"><span data-stu-id="e177b-442">Restart the app to seed the database.</span></span>

<span data-ttu-id="e177b-443">Łatwe testowanie ukończonej aplikacji jest do uruchomienia w trzech różnych przeglądarek (lub sesji incognito/InPrivate).</span><span class="sxs-lookup"><span data-stu-id="e177b-443">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate sessions).</span></span> <span data-ttu-id="e177b-444">W jednej przeglądarki zarejestrować nowego użytkownika (na przykład `test@example.com`).</span><span class="sxs-lookup"><span data-stu-id="e177b-444">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="e177b-445">Zaloguj się w każdej przeglądarce z innym użytkownikiem.</span><span class="sxs-lookup"><span data-stu-id="e177b-445">Sign in to each browser with a different user.</span></span> <span data-ttu-id="e177b-446">Sprawdź następujące operacje:</span><span class="sxs-lookup"><span data-stu-id="e177b-446">Verify the following operations:</span></span>

* <span data-ttu-id="e177b-447">Zarejestrowani użytkownicy można wyświetlić wszystkie zatwierdzone dane kontaktowe.</span><span class="sxs-lookup"><span data-stu-id="e177b-447">Registered users can view all of the approved contact data.</span></span>
* <span data-ttu-id="e177b-448">Zarejestrowani użytkownicy może edytować/usuwać swoje dane.</span><span class="sxs-lookup"><span data-stu-id="e177b-448">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="e177b-449">Menedżerowie mogą zatwierdzać/Odrzuć dane kontaktowe.</span><span class="sxs-lookup"><span data-stu-id="e177b-449">Managers can approve/reject contact data.</span></span> <span data-ttu-id="e177b-450">`Details` Wyświetlić pokazuje **Zatwierdź** i **Odrzuć** przycisków.</span><span class="sxs-lookup"><span data-stu-id="e177b-450">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="e177b-451">Administratorzy mogą zatwierdzać/Odrzuć i edytowanie/usuwanie wszystkich danych.</span><span class="sxs-lookup"><span data-stu-id="e177b-451">Administrators can approve/reject and edit/delete all data.</span></span>

| <span data-ttu-id="e177b-452">Użytkownik</span><span class="sxs-lookup"><span data-stu-id="e177b-452">User</span></span>                | <span data-ttu-id="e177b-453">Zasilany przez aplikację</span><span class="sxs-lookup"><span data-stu-id="e177b-453">Seeded by the app</span></span> | <span data-ttu-id="e177b-454">Opcje</span><span class="sxs-lookup"><span data-stu-id="e177b-454">Options</span></span>                                  |
| ------------------- | :---------------: | ---------------------------------------- |
| test@example.com    | <span data-ttu-id="e177b-455">Nie</span><span class="sxs-lookup"><span data-stu-id="e177b-455">No</span></span>                | <span data-ttu-id="e177b-456">Edytowanie/usuwanie własnych danych.</span><span class="sxs-lookup"><span data-stu-id="e177b-456">Edit/delete the own data.</span></span>                |
| manager@contoso.com | <span data-ttu-id="e177b-457">Tak</span><span class="sxs-lookup"><span data-stu-id="e177b-457">Yes</span></span>               | <span data-ttu-id="e177b-458">Zatwierdź/Odrzuć i edytowanie/usuwanie własnych danych.</span><span class="sxs-lookup"><span data-stu-id="e177b-458">Approve/reject and edit/delete own data.</span></span> |
| admin@contoso.com   | <span data-ttu-id="e177b-459">Tak</span><span class="sxs-lookup"><span data-stu-id="e177b-459">Yes</span></span>               | <span data-ttu-id="e177b-460">Zatwierdź/Odrzuć i edytowanie/usuwanie wszystkich danych.</span><span class="sxs-lookup"><span data-stu-id="e177b-460">Approve/reject and edit/delete all data.</span></span> |

<span data-ttu-id="e177b-461">Utwórz kontakt w przeglądarce administratora.</span><span class="sxs-lookup"><span data-stu-id="e177b-461">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="e177b-462">Skopiuj adres URL do usunięcia, a następnie Edytuj z skontaktowanie się z administratorem.</span><span class="sxs-lookup"><span data-stu-id="e177b-462">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="e177b-463">Wklej te linki do przeglądarki użytkownika testowego, aby sprawdzić, czy użytkownik testu nie może wykonywać te operacje.</span><span class="sxs-lookup"><span data-stu-id="e177b-463">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="e177b-464">Utwórz aplikację startową</span><span class="sxs-lookup"><span data-stu-id="e177b-464">Create the starter app</span></span>

* <span data-ttu-id="e177b-465">Tworzenie stron Razor aplikacji o nazwie "ContactManager"</span><span class="sxs-lookup"><span data-stu-id="e177b-465">Create a Razor Pages app named "ContactManager"</span></span>
  * <span data-ttu-id="e177b-466">Tworzenie aplikacji za pomocą **indywidualne konta użytkowników**.</span><span class="sxs-lookup"><span data-stu-id="e177b-466">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="e177b-467">Nadaj mu nazwę "ContactManager", przestrzeń nazw używaną w próbce pasujących przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="e177b-467">Name it "ContactManager" so the namespace matches the namespace used in the sample.</span></span>
  * <span data-ttu-id="e177b-468">`-uld` Określa LocalDB zamiast bazy danych SQLite</span><span class="sxs-lookup"><span data-stu-id="e177b-468">`-uld` specifies LocalDB instead of SQLite</span></span>

  ```console
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* <span data-ttu-id="e177b-469">Dodaj *modele/Contact. cs*:</span><span class="sxs-lookup"><span data-stu-id="e177b-469">Add *Models/Contact.cs*:</span></span>

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="e177b-470">Tworzenie szkieletu `Contact` modelu.</span><span class="sxs-lookup"><span data-stu-id="e177b-470">Scaffold the `Contact` model.</span></span>
* <span data-ttu-id="e177b-471">Tworzenie początkowej migracji i aktualizowanie bazy danych:</span><span class="sxs-lookup"><span data-stu-id="e177b-471">Create initial migration and update the database:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
  dotnet ef database drop -f
  dotnet ef migrations add initial
  dotnet ef database update
  ```

* <span data-ttu-id="e177b-472">Aktualizacja **ContactManager** zakotwiczenia w *Pages/_Layout.cshtml* pliku:</span><span class="sxs-lookup"><span data-stu-id="e177b-472">Update the **ContactManager** anchor in the *Pages/_Layout.cshtml* file:</span></span>

  ```cshtml
  <a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
  ```

* <span data-ttu-id="e177b-473">Przetestuj aplikację, tworzenia, edytowania i usuwania kontaktu</span><span class="sxs-lookup"><span data-stu-id="e177b-473">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="e177b-474">Inicjowanie bazy danych</span><span class="sxs-lookup"><span data-stu-id="e177b-474">Seed the database</span></span>

<span data-ttu-id="e177b-475">Dodaj [SeedData](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs) klasy *danych* folderu.</span><span class="sxs-lookup"><span data-stu-id="e177b-475">Add the [SeedData](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs) class to the *Data* folder.</span></span>

<span data-ttu-id="e177b-476">Wywołaj `SeedData.Initialize` z `Main`:</span><span class="sxs-lookup"><span data-stu-id="e177b-476">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Program.cs?name=snippet)]

<span data-ttu-id="e177b-477">Sprawdź, czy aplikacja zasilany bazy danych.</span><span class="sxs-lookup"><span data-stu-id="e177b-477">Test that the app seeded the database.</span></span> <span data-ttu-id="e177b-478">W przypadku wszystkich wierszy w skontaktuj się z bazy danych, metoda inicjatora nie zostanie uruchomiony.</span><span class="sxs-lookup"><span data-stu-id="e177b-478">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

::: moniker-end

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a><span data-ttu-id="e177b-479">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="e177b-479">Additional resources</span></span>

* [<span data-ttu-id="e177b-480">Tworzenie aplikacji internetowej platformy .NET Core i SQL Database w usłudze Azure App Service</span><span class="sxs-lookup"><span data-stu-id="e177b-480">Build a .NET Core and SQL Database web app in Azure App Service</span></span>](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb)
* <span data-ttu-id="e177b-481">[Laboratorium autoryzacji platformy ASP.NET Core](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span><span class="sxs-lookup"><span data-stu-id="e177b-481">[ASP.NET Core Authorization Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span></span> <span data-ttu-id="e177b-482">W tym laboratorium zawiera bardziej szczegółowe na temat funkcji zabezpieczeń wprowadzone w ramach tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="e177b-482">This lab goes into more detail on the security features introduced in this tutorial.</span></span>
* <xref:security/authorization/introduction>
* [<span data-ttu-id="e177b-483">Autoryzacja niestandardowa oparta na zasadach</span><span class="sxs-lookup"><span data-stu-id="e177b-483">Custom policy-based authorization</span></span>](xref:security/authorization/policies)
