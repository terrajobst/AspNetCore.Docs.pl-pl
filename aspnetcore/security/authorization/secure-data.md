---
title: Tworzenie aplikacji platformy ASP.NET Core przy użyciu danych użytkownika chronionych przez autoryzację
author: rick-anderson
description: Dowiedz się, jak utworzyć aplikację stron Razor przy użyciu danych użytkownika chronionych przez autoryzację. Obejmuje protokołu HTTPS, uwierzytelniania, zabezpieczeń i tożsamości platformy ASP.NET Core.
ms.author: riande
ms.date: 7/24/2018
uid: security/authorization/secure-data
ms.openlocfilehash: 2fb13f0772a1f8aa4ed2ff3ece2a2c5d3b7360c9
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46010939"
---
::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="a18c0-104">Zobacz [ta](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) dla wersji platformy ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="a18c0-104">See [this PDF](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC version.</span></span> <span data-ttu-id="a18c0-105">Wersja platformy ASP.NET Core 1.1 po ukończeniu tego samouczka jest [to](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) folderu.</span><span class="sxs-lookup"><span data-stu-id="a18c0-105">The ASP.NET Core 1.1 version of this tutorial is in [this](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) folder.</span></span> <span data-ttu-id="a18c0-106">1.1, na przykład platforma ASP.NET Core jest w [przykłady](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span><span class="sxs-lookup"><span data-stu-id="a18c0-106">The 1.1 ASP.NET Core sample is in the [samples](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="a18c0-107">Zobacz [ten plik pdf](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_July16_18.pdf)</span><span class="sxs-lookup"><span data-stu-id="a18c0-107">See [this pdf](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_July16_18.pdf)</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a><span data-ttu-id="a18c0-108">Tworzenie aplikacji platformy ASP.NET Core przy użyciu danych użytkownika chronionych przez autoryzację</span><span class="sxs-lookup"><span data-stu-id="a18c0-108">Create an ASP.NET Core app with user data protected by authorization</span></span>

<span data-ttu-id="a18c0-109">Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Audette Jan](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="a18c0-109">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="a18c0-110">W tym samouczku pokazano, jak utworzyć aplikację sieci web platformy ASP.NET Core przy użyciu danych użytkownika chronionych przez autoryzację.</span><span class="sxs-lookup"><span data-stu-id="a18c0-110">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="a18c0-111">Wyświetla listę kontaktów, uwierzytelnionych użytkowników (zarejestrowane), które zostały utworzone.</span><span class="sxs-lookup"><span data-stu-id="a18c0-111">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="a18c0-112">Istnieją trzy grupy zabezpieczeń:</span><span class="sxs-lookup"><span data-stu-id="a18c0-112">There are three security groups:</span></span>

* <span data-ttu-id="a18c0-113">**Liczba zarejestrowanych użytkowników** może wyświetlać wszystkie zatwierdzone dane i może edytować/usuwać swoje dane.</span><span class="sxs-lookup"><span data-stu-id="a18c0-113">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="a18c0-114">**Menedżerowie** można zatwierdzić lub odrzucić dane kontaktowe.</span><span class="sxs-lookup"><span data-stu-id="a18c0-114">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="a18c0-115">Tylko zatwierdzone kontakty będą widoczne dla użytkowników.</span><span class="sxs-lookup"><span data-stu-id="a18c0-115">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="a18c0-116">**Administratorzy** można zatwierdzić/Odrzuć i edytowanie/usuwanie żadnych danych.</span><span class="sxs-lookup"><span data-stu-id="a18c0-116">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="a18c0-117">Na poniższej ilustracji użytkownik Rick (`rick@example.com`) jest zalogowany.</span><span class="sxs-lookup"><span data-stu-id="a18c0-117">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="a18c0-118">Rick mogą jedynie wyświetlać kontakty zatwierdzone i **Edytuj**/**Usuń**/**Utwórz nowy** linki do swoich kontaktów.</span><span class="sxs-lookup"><span data-stu-id="a18c0-118">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="a18c0-119">Tylko ostatni rekord, utworzone przez Rick, wyświetla **Edytuj** i **Usuń** łącza.</span><span class="sxs-lookup"><span data-stu-id="a18c0-119">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="a18c0-120">Inni użytkownicy nie zobaczą ostatni rekord, aż Menedżer lub administrator zmienia stan na "Zatwierdzone".</span><span class="sxs-lookup"><span data-stu-id="a18c0-120">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![Obraz opisanych w poprzednich](secure-data/_static/rick.png)

<span data-ttu-id="a18c0-122">Na poniższej ilustracji `manager@contoso.com` jest zarejestrowany i w roli menedżerów:</span><span class="sxs-lookup"><span data-stu-id="a18c0-122">In the following image, `manager@contoso.com` is signed in and in the managers role:</span></span>

![Obraz opisanych w poprzednich](secure-data/_static/manager1.png)

<span data-ttu-id="a18c0-124">Na poniższej ilustracji przedstawiono menedżerów widoku szczegółów kontaktu:</span><span class="sxs-lookup"><span data-stu-id="a18c0-124">The following image shows the managers details view of a contact:</span></span>

![Obraz opisanych w poprzednich](secure-data/_static/manager.png)

<span data-ttu-id="a18c0-126">**Zatwierdź** i **Odrzuć** przyciski są wyświetlane tylko dla menedżerów i administratorów.</span><span class="sxs-lookup"><span data-stu-id="a18c0-126">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="a18c0-127">Na poniższej ilustracji `admin@contoso.com` jest zarejestrowany i w roli administratora:</span><span class="sxs-lookup"><span data-stu-id="a18c0-127">In the following image, `admin@contoso.com` is signed in and in the administrators role:</span></span>

![Obraz opisanych w poprzednich](secure-data/_static/admin.png)

<span data-ttu-id="a18c0-129">Administrator ma wszystkie uprawnienia.</span><span class="sxs-lookup"><span data-stu-id="a18c0-129">The administrator has all privileges.</span></span> <span data-ttu-id="a18c0-130">Ona można odczytu/edytowanie/usuwanie dowolnego skontaktuj się z pomocą i zmienić stan kontaktów.</span><span class="sxs-lookup"><span data-stu-id="a18c0-130">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="a18c0-131">Aplikacja została utworzona przez [tworzenia szkieletów](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) następujące `Contact` modelu:</span><span class="sxs-lookup"><span data-stu-id="a18c0-131">The app was created by [scaffolding](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) the following `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet)]

<span data-ttu-id="a18c0-132">Przykład zawiera poniższe obsługi autoryzacji:</span><span class="sxs-lookup"><span data-stu-id="a18c0-132">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="a18c0-133">`ContactIsOwnerAuthorizationHandler`: Zapewnia, że użytkownika można edytować tylko swoje dane.</span><span class="sxs-lookup"><span data-stu-id="a18c0-133">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="a18c0-134">`ContactManagerAuthorizationHandler`: Umożliwia menedżerom zatwierdzić lub odrzucić kontaktów.</span><span class="sxs-lookup"><span data-stu-id="a18c0-134">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="a18c0-135">`ContactAdministratorsAuthorizationHandler`: Umożliwia administratorom, aby zatwierdzić lub odrzucić kontakty i edytowanie/usuwanie kontaktów.</span><span class="sxs-lookup"><span data-stu-id="a18c0-135">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a18c0-136">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="a18c0-136">Prerequisites</span></span>

<span data-ttu-id="a18c0-137">W tym samouczku jest zaawansowany.</span><span class="sxs-lookup"><span data-stu-id="a18c0-137">This tutorial is advanced.</span></span> <span data-ttu-id="a18c0-138">Należy zapoznać się z:</span><span class="sxs-lookup"><span data-stu-id="a18c0-138">You should be familiar with:</span></span>

* [<span data-ttu-id="a18c0-139">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a18c0-139">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="a18c0-140">Uwierzytelnianie</span><span class="sxs-lookup"><span data-stu-id="a18c0-140">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="a18c0-141">Potwierdzenie konta i odzyskiwanie hasła</span><span class="sxs-lookup"><span data-stu-id="a18c0-141">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="a18c0-142">Autoryzacja</span><span class="sxs-lookup"><span data-stu-id="a18c0-142">Authorization</span></span>](xref:security/authorization/index)
* [<span data-ttu-id="a18c0-143">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="a18c0-143">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="a18c0-144">W programie ASP.NET Core 2.1 `User.IsInRole` zakończy się niepowodzeniem w przypadku korzystania z `AddDefaultIdentity`.</span><span class="sxs-lookup"><span data-stu-id="a18c0-144">In ASP.NET Core 2.1, `User.IsInRole` fails when using `AddDefaultIdentity`.</span></span> <span data-ttu-id="a18c0-145">W tym samouczku `AddDefaultIdentity` i dlatego wymaga platformy ASP.NET Core 2.2 w wersji zapoznawczej 1 lub nowszym.</span><span class="sxs-lookup"><span data-stu-id="a18c0-145">This tutorial uses `AddDefaultIdentity` and therefore requires ASP.NET Core 2.2 preview 1 or later.</span></span> <span data-ttu-id="a18c0-146">Zobacz [problem w usłudze GitHub](https://github.com/aspnet/Identity/issues/1813#issuecomment-394543909) dla obejścia.</span><span class="sxs-lookup"><span data-stu-id="a18c0-146">See [this GitHub issue](https://github.com/aspnet/Identity/issues/1813#issuecomment-394543909) for a work-around.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="a18c0-147">Starter i ukończonej aplikacji</span><span class="sxs-lookup"><span data-stu-id="a18c0-147">The starter and completed app</span></span>

<span data-ttu-id="a18c0-148">[Pobierz](xref:tutorials/index#how-to-download-a-sample) [ukończone](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2) aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a18c0-148">[Download](xref:tutorials/index#how-to-download-a-sample) the [completed](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2) app.</span></span> <span data-ttu-id="a18c0-149">[Test](#test-the-completed-app) ukończonej aplikacji, dzięki czemu można zapoznać się z jej funkcjami zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="a18c0-149">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="a18c0-150">Aplikację startową</span><span class="sxs-lookup"><span data-stu-id="a18c0-150">The starter app</span></span>

<span data-ttu-id="a18c0-151">[Pobierz](xref:tutorials/index#how-to-download-a-sample) [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2) aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a18c0-151">[Download](xref:tutorials/index#how-to-download-a-sample) the [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2) app.</span></span>

<span data-ttu-id="a18c0-152">Uruchom aplikację, naciśnij przycisk **ContactManager** link i sprawdź, tworzenie, edytowanie i usuwanie kontaktu.</span><span class="sxs-lookup"><span data-stu-id="a18c0-152">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="a18c0-153">Zabezpieczanie danych użytkownika</span><span class="sxs-lookup"><span data-stu-id="a18c0-153">Secure user data</span></span>

<span data-ttu-id="a18c0-154">Poniższe sekcje mają główne kroki umożliwiające utworzenie aplikacji danych bezpiecznego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="a18c0-154">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="a18c0-155">Może okazać się przydatne do odwoływania się do projektu ukończona.</span><span class="sxs-lookup"><span data-stu-id="a18c0-155">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="a18c0-156">Powiąż dane kontaktowe dla użytkownika</span><span class="sxs-lookup"><span data-stu-id="a18c0-156">Tie the contact data to the user</span></span>

<span data-ttu-id="a18c0-157">Za pomocą programu ASP.NET [tożsamości](xref:security/authentication/identity) identyfikator użytkownika w celu zapewnienia, że użytkownicy mogą edytować swoje dane, ale nie dane innych użytkowników.</span><span class="sxs-lookup"><span data-stu-id="a18c0-157">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="a18c0-158">Dodaj `OwnerID` i `ContactStatus` do `Contact` modelu:</span><span class="sxs-lookup"><span data-stu-id="a18c0-158">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

<span data-ttu-id="a18c0-159">`OwnerID` Identyfikator użytkownika z `AspNetUser` tabelę [tożsamości](xref:security/authentication/identity) bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a18c0-159">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="a18c0-160">`Status` Pola określa, czy kontakt jest widoczny dla zwykłych użytkowników.</span><span class="sxs-lookup"><span data-stu-id="a18c0-160">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="a18c0-161">Tworzenie nowej migracji i aktualizowanie bazy danych:</span><span class="sxs-lookup"><span data-stu-id="a18c0-161">Create a new migration and update the database:</span></span>

```console
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-identity"></a><span data-ttu-id="a18c0-162">Dodaj usługi ról do tożsamości</span><span class="sxs-lookup"><span data-stu-id="a18c0-162">Add Role services to Identity</span></span>

<span data-ttu-id="a18c0-163">Dołącz [opcji Dodawanie ról](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) można dodać usługi ról:</span><span class="sxs-lookup"><span data-stu-id="a18c0-163">Append [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) to add Role services:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet2&highlight=12)]

### <a name="require-authenticated-users"></a><span data-ttu-id="a18c0-164">Wymaga uwierzytelnionych użytkowników</span><span class="sxs-lookup"><span data-stu-id="a18c0-164">Require authenticated users</span></span>

<span data-ttu-id="a18c0-165">Ustaw domyślne zasady uwierzytelniania, aby wymagać od użytkowników uwierzytelniania:</span><span class="sxs-lookup"><span data-stu-id="a18c0-165">Set the default authentication policy to require users to be authenticated:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet&highlight=17-99)] 

 <span data-ttu-id="a18c0-166">Użytkownik może zrezygnować z uwierzytelniania na poziomie metody strony Razor, kontrolera lub akcji z `[AllowAnonymous]` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="a18c0-166">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="a18c0-167">Ustawienie domyślne zasady uwierzytelniania, aby wymagać od użytkowników uwierzytelniania chroni nowo dodanych stronami Razor i kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="a18c0-167">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="a18c0-168">Posiadanie uwierzytelniania domyślnie wymagane jest bezpieczniejszy niż opierając się na nowych kontrolerów i stron Razor do uwzględnienia `[Authorize]` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="a18c0-168">Having authentication required by default is more secure than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span>

<span data-ttu-id="a18c0-169">Dodaj [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) do indeksu o i skontaktuj się z pomocą stron, użytkowników anonimowych można uzyskać informacji o lokacji, przed ich zarejestrowania.</span><span class="sxs-lookup"><span data-stu-id="a18c0-169">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index, About, and Contact pages so anonymous users can get information about the site before they register.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Index.cshtml.cs?highlight=1,6)]

### <a name="configure-the-test-account"></a><span data-ttu-id="a18c0-170">Skonfiguruj konto testu</span><span class="sxs-lookup"><span data-stu-id="a18c0-170">Configure the test account</span></span>

<span data-ttu-id="a18c0-171">`SeedData` Klasy tworzy dwa konta: administrator i Menedżer.</span><span class="sxs-lookup"><span data-stu-id="a18c0-171">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="a18c0-172">Użyj [narzędzie Menedżer klucz tajny](xref:security/app-secrets) ustawić hasło dla tych kont.</span><span class="sxs-lookup"><span data-stu-id="a18c0-172">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="a18c0-173">Ustaw hasło z katalogu projektu (katalogu zawierającego *Program.cs*):</span><span class="sxs-lookup"><span data-stu-id="a18c0-173">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```console
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="a18c0-174">Jeśli nie określono silne hasło, wyjątek jest generowany, gdy `SeedData.Initialize` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="a18c0-174">If a strong password is not specified, an exception is thrown when `SeedData.Initialize` is called.</span></span>

<span data-ttu-id="a18c0-175">Aktualizacja `Main` hasła testu:</span><span class="sxs-lookup"><span data-stu-id="a18c0-175">Update `Main` to use the test password:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="a18c0-176">Tworzenie konta testowe i aktualizowanie kontaktów</span><span class="sxs-lookup"><span data-stu-id="a18c0-176">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="a18c0-177">Aktualizacja `Initialize` method in Class metoda `SeedData` klasy w celu utworzenia konta testowe:</span><span class="sxs-lookup"><span data-stu-id="a18c0-177">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="a18c0-178">Dodaj identyfikator użytkownika administratora i `ContactStatus` do kontaktów.</span><span class="sxs-lookup"><span data-stu-id="a18c0-178">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="a18c0-179">Określ jeden z kontaktów "Przesłane" i jednym "odrzucone".</span><span class="sxs-lookup"><span data-stu-id="a18c0-179">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="a18c0-180">Dodawanie stanu i Identyfikatora użytkownika do wszystkich kontaktów.</span><span class="sxs-lookup"><span data-stu-id="a18c0-180">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="a18c0-181">Wyświetlane jest tylko jeden kontakt:</span><span class="sxs-lookup"><span data-stu-id="a18c0-181">Only one contact is shown:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="a18c0-182">Utwórz właściciela, Menedżer i obsługi autoryzacji administratora</span><span class="sxs-lookup"><span data-stu-id="a18c0-182">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="a18c0-183">Tworzenie `ContactIsOwnerAuthorizationHandler` klasy w *autoryzacji* folderu.</span><span class="sxs-lookup"><span data-stu-id="a18c0-183">Create a `ContactIsOwnerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="a18c0-184">`ContactIsOwnerAuthorizationHandler` Sprawdza, czy użytkownik, działając w zasobie jest właścicielem zasobu.</span><span class="sxs-lookup"><span data-stu-id="a18c0-184">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="a18c0-185">`ContactIsOwnerAuthorizationHandler` Wywołania [kontekstu. Powodzenie](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) aktualnego użytkownika uwierzytelnionego jest skontaktuj się z właścicielem.</span><span class="sxs-lookup"><span data-stu-id="a18c0-185">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="a18c0-186">Programy obsługi autoryzacji zazwyczaj:</span><span class="sxs-lookup"><span data-stu-id="a18c0-186">Authorization handlers generally:</span></span>

* <span data-ttu-id="a18c0-187">Zwróć `context.Succeed` gdy spełniono wymagania.</span><span class="sxs-lookup"><span data-stu-id="a18c0-187">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="a18c0-188">Zwróć `Task.CompletedTask` gdy nie są spełnione wymagania.</span><span class="sxs-lookup"><span data-stu-id="a18c0-188">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="a18c0-189">`Task.CompletedTask` to nie sukces lub niepowodzenie&mdash;umożliwia innych programów obsługi autoryzacji uruchomić.</span><span class="sxs-lookup"><span data-stu-id="a18c0-189">`Task.CompletedTask` is neither success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="a18c0-190">Jeśli potrzebujesz jawnie nie powiedzie się, zwraca [kontekstu. Niepowodzenie](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span><span class="sxs-lookup"><span data-stu-id="a18c0-190">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="a18c0-191">Aplikacja umożliwia skontaktuj się z pomocą właścicieli do edycji/delete/tworzenie własnych danych.</span><span class="sxs-lookup"><span data-stu-id="a18c0-191">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="a18c0-192">`ContactIsOwnerAuthorizationHandler` nie muszą sprawdzać operacji przekazane w parametrze wymagań.</span><span class="sxs-lookup"><span data-stu-id="a18c0-192">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="a18c0-193">Utwórz procedurę obsługi Menedżer autoryzacji</span><span class="sxs-lookup"><span data-stu-id="a18c0-193">Create a manager authorization handler</span></span>

<span data-ttu-id="a18c0-194">Tworzenie `ContactManagerAuthorizationHandler` klasy w *autoryzacji* folderu.</span><span class="sxs-lookup"><span data-stu-id="a18c0-194">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="a18c0-195">`ContactManagerAuthorizationHandler` Weryfikuje użytkownika, działając w zasobie menedżera.</span><span class="sxs-lookup"><span data-stu-id="a18c0-195">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="a18c0-196">Tylko menedżerowie mogli zatwierdzać lub odrzucać zmiany zawartości (nowe lub zmienione).</span><span class="sxs-lookup"><span data-stu-id="a18c0-196">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="a18c0-197">Utwórz procedurę obsługi autoryzacji administratora</span><span class="sxs-lookup"><span data-stu-id="a18c0-197">Create an administrator authorization handler</span></span>

<span data-ttu-id="a18c0-198">Tworzenie `ContactAdministratorsAuthorizationHandler` klasy w *autoryzacji* folderu.</span><span class="sxs-lookup"><span data-stu-id="a18c0-198">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="a18c0-199">`ContactAdministratorsAuthorizationHandler` Weryfikuje użytkownika na zasób jest administratorem.</span><span class="sxs-lookup"><span data-stu-id="a18c0-199">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="a18c0-200">Administrator może wykonać wszystkie operacje.</span><span class="sxs-lookup"><span data-stu-id="a18c0-200">Administrator can do all operations.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="a18c0-201">Zarejestruj procedury obsługi autoryzacji</span><span class="sxs-lookup"><span data-stu-id="a18c0-201">Register the authorization handlers</span></span>

<span data-ttu-id="a18c0-202">Usługi za pomocą platformy Entity Framework Core musi być zarejestrowana do [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) przy użyciu [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span><span class="sxs-lookup"><span data-stu-id="a18c0-202">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="a18c0-203">`ContactIsOwnerAuthorizationHandler` Korzysta z platformy ASP.NET Core [tożsamości](xref:security/authentication/identity), która jest oparta na platformy Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="a18c0-203">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="a18c0-204">Zarejestruj procedury obsługi za pomocą kolekcji usługi, dzięki czemu są one dostępne do `ContactsController` za pośrednictwem [wstrzykiwanie zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="a18c0-204">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="a18c0-205">Dodaj następujący kod na końcu `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="a18c0-205">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet_defaultPolicy&highlight=27-99)]

<span data-ttu-id="a18c0-206">`ContactAdministratorsAuthorizationHandler` i `ContactManagerAuthorizationHandler` są dodawane jako pojedynczych wystąpień.</span><span class="sxs-lookup"><span data-stu-id="a18c0-206">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="a18c0-207">Są one pojedynczych wystąpień, ponieważ nie używają programu EF, a wszystkie informacje wymagane w `Context` parametru `HandleRequirementAsync` metody.</span><span class="sxs-lookup"><span data-stu-id="a18c0-207">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="a18c0-208">Obsługuje autoryzację</span><span class="sxs-lookup"><span data-stu-id="a18c0-208">Support authorization</span></span>

<span data-ttu-id="a18c0-209">W tej sekcji służy do aktualizacji stron Razor i Dodaj klasę wymagania dotyczące operacji.</span><span class="sxs-lookup"><span data-stu-id="a18c0-209">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="a18c0-210">Przegląd klasy wymagania dotyczące operacji kontaktu</span><span class="sxs-lookup"><span data-stu-id="a18c0-210">Review the contact operations requirements class</span></span>

<span data-ttu-id="a18c0-211">Przegląd `ContactOperations` klasy.</span><span class="sxs-lookup"><span data-stu-id="a18c0-211">Review the `ContactOperations` class.</span></span> <span data-ttu-id="a18c0-212">Ta klasa zawiera wymagania obsługiwanej przez aplikację:</span><span class="sxs-lookup"><span data-stu-id="a18c0-212">This class contains the requirements the app supports:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-razor-pages"></a><span data-ttu-id="a18c0-213">Utwórz klasę bazową dla stron Razor kontaktów</span><span class="sxs-lookup"><span data-stu-id="a18c0-213">Create a base class for the Contacts Razor Pages</span></span>

<span data-ttu-id="a18c0-214">Utwórz klasę bazową, zawierający usług wykorzystanych w kontaktach stron Razor.</span><span class="sxs-lookup"><span data-stu-id="a18c0-214">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="a18c0-215">Klasa bazowa umieszcza kod inicjowania w jednej lokalizacji:</span><span class="sxs-lookup"><span data-stu-id="a18c0-215">The base class puts the initialization code in one location:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="a18c0-216">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="a18c0-216">The preceding code:</span></span>

* <span data-ttu-id="a18c0-217">Dodaje `IAuthorizationService` usługi w celu uzyskania dostępu do obsługi autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="a18c0-217">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="a18c0-218">Dodaje tożsamości `UserManager` usługi.</span><span class="sxs-lookup"><span data-stu-id="a18c0-218">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="a18c0-219">Dodaj `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="a18c0-219">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="a18c0-220">Aktualizacja CreateModel</span><span class="sxs-lookup"><span data-stu-id="a18c0-220">Update the CreateModel</span></span>

<span data-ttu-id="a18c0-221">Aktualizuj Konstruktor Tworzenie strony modelu do użycia `DI_BasePageModel` klasa bazowa:</span><span class="sxs-lookup"><span data-stu-id="a18c0-221">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="a18c0-222">Aktualizacja `CreateModel.OnPostAsync` metody:</span><span class="sxs-lookup"><span data-stu-id="a18c0-222">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="a18c0-223">Dodaj identyfikator użytkownika, który `Contact` modelu.</span><span class="sxs-lookup"><span data-stu-id="a18c0-223">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="a18c0-224">Wywołania obsługi autoryzacji, aby sprawdzić, czy użytkownik ma uprawnienia do tworzenia kontaktów.</span><span class="sxs-lookup"><span data-stu-id="a18c0-224">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="a18c0-225">Aktualizacja IndexModel</span><span class="sxs-lookup"><span data-stu-id="a18c0-225">Update the IndexModel</span></span>

<span data-ttu-id="a18c0-226">Aktualizacja `OnGetAsync` metody, więc tylko zatwierdzonego kontakty są wyświetlane użytkownikom ogólne:</span><span class="sxs-lookup"><span data-stu-id="a18c0-226">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="a18c0-227">Aktualizacja EditModel</span><span class="sxs-lookup"><span data-stu-id="a18c0-227">Update the EditModel</span></span>

<span data-ttu-id="a18c0-228">Dodaj program obsługi autoryzacji, aby sprawdzić, czy użytkownik jest właścicielem kontaktu.</span><span class="sxs-lookup"><span data-stu-id="a18c0-228">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="a18c0-229">Ponieważ autoryzacji zasobów jest weryfikowany, `[Authorize]` atrybut nie jest wystarczająca.</span><span class="sxs-lookup"><span data-stu-id="a18c0-229">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="a18c0-230">Aplikacja nie ma dostępu do zasobu, gdy atrybuty są oceniane.</span><span class="sxs-lookup"><span data-stu-id="a18c0-230">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="a18c0-231">Autoryzacja na podstawie zasobów musi być imperatywnego.</span><span class="sxs-lookup"><span data-stu-id="a18c0-231">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="a18c0-232">Testy muszą być wykonywane, gdy aplikacja ma dostęp do zasobu przez załadowanie go w modelu strony lub przez załadowanie go w ramach programu obsługi, sam.</span><span class="sxs-lookup"><span data-stu-id="a18c0-232">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="a18c0-233">Często dostęp do zasobu, przekazując klucz zasobu.</span><span class="sxs-lookup"><span data-stu-id="a18c0-233">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="a18c0-234">Aktualizacja DeleteModel</span><span class="sxs-lookup"><span data-stu-id="a18c0-234">Update the DeleteModel</span></span>

<span data-ttu-id="a18c0-235">Aktualizowanie modelu strony delete na potrzeby obsługi autoryzacji upewnij się, że użytkownik ma odpowiednie uprawnienia do usuwania dla kontaktu.</span><span class="sxs-lookup"><span data-stu-id="a18c0-235">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="a18c0-236">Wstrzyknięcie usługi autoryzacji do widoków</span><span class="sxs-lookup"><span data-stu-id="a18c0-236">Inject the authorization service into the views</span></span>

<span data-ttu-id="a18c0-237">Obecnie pokazuje interfejsu użytkownika edytowania i usuwania łącza kontaktów, do których użytkownik nie może modyfikować.</span><span class="sxs-lookup"><span data-stu-id="a18c0-237">Currently, the UI shows edit and delete links for contacts the user can't modify.</span></span>

<span data-ttu-id="a18c0-238">Wstrzykiwanie usługi autoryzacji w *Views/_ViewImports.cshtml* pliku, dzięki czemu są one dostępne dla wszystkich widoków:</span><span class="sxs-lookup"><span data-stu-id="a18c0-238">Inject the authorization service in the *Views/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/_ViewImports.cshtml?highlight=6-99)]

<span data-ttu-id="a18c0-239">Poprzedni kod znaczników umożliwia dodanie kilku `using` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="a18c0-239">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="a18c0-240">Aktualizacja **Edytuj** i **Usuń** linki w *Pages/Contacts/Index.cshtml* więc tylko są one renderowane dla użytkowników z odpowiednimi uprawnieniami:</span><span class="sxs-lookup"><span data-stu-id="a18c0-240">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> <span data-ttu-id="a18c0-241">Ukrywanie łączy na podstawie użytkowników, którzy nie mają uprawnień do zmiany danych nie zabezpieczenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a18c0-241">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="a18c0-242">Ukrywanie łącza sprawia, że bardziej przyjazny dla użytkownika aplikacji, wyświetlając tylko poprawne linki.</span><span class="sxs-lookup"><span data-stu-id="a18c0-242">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="a18c0-243">Użytkownicy mogą hack wygenerowanego adresy URL, aby wywołać Edytuj i Usuń operacje na danych, które nie są właścicielami.</span><span class="sxs-lookup"><span data-stu-id="a18c0-243">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="a18c0-244">Strona Razor lub kontrolera musi wymuszają operacje sprawdzania dostępu do zabezpieczania danych.</span><span class="sxs-lookup"><span data-stu-id="a18c0-244">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="a18c0-245">Szczegóły aktualizacji</span><span class="sxs-lookup"><span data-stu-id="a18c0-245">Update Details</span></span>

<span data-ttu-id="a18c0-246">Zaktualizuj widok szczegółów, aby menedżerowie mogli zatwierdzać lub odrzucać kontaktów:</span><span class="sxs-lookup"><span data-stu-id="a18c0-246">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml?name=snippet)]

<span data-ttu-id="a18c0-247">Aktualizowanie modelu strony szczegółów:</span><span class="sxs-lookup"><span data-stu-id="a18c0-247">Update the details page model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="add-or-remove-a-user-to-a-role"></a><span data-ttu-id="a18c0-248">Dodawanie lub usuwanie użytkownika do roli</span><span class="sxs-lookup"><span data-stu-id="a18c0-248">Add or remove a user to a role</span></span>

<span data-ttu-id="a18c0-249">Zobacz [ten problem](https://github.com/aspnet/Docs/issues/8502) uzyskać informacji na temat:</span><span class="sxs-lookup"><span data-stu-id="a18c0-249">See [this issue](https://github.com/aspnet/Docs/issues/8502) for information on:</span></span>

* <span data-ttu-id="a18c0-250">Usuwanie uprawnień z użytkownikiem.</span><span class="sxs-lookup"><span data-stu-id="a18c0-250">Removing privileges from a user.</span></span> <span data-ttu-id="a18c0-251">Na przykład wyciszanie użytkownika w aplikacji do rozmów.</span><span class="sxs-lookup"><span data-stu-id="a18c0-251">For example muting a user in a chat app.</span></span>
* <span data-ttu-id="a18c0-252">Dodawanie uprawnień dla użytkownika.</span><span class="sxs-lookup"><span data-stu-id="a18c0-252">Adding privileges to a user.</span></span>

## <a name="test-the-completed-app"></a><span data-ttu-id="a18c0-253">Testowanie aplikacji ukończone</span><span class="sxs-lookup"><span data-stu-id="a18c0-253">Test the completed app</span></span>

<span data-ttu-id="a18c0-254">Jeśli aplikacja ma kontaktów:</span><span class="sxs-lookup"><span data-stu-id="a18c0-254">If the app has contacts:</span></span>

* <span data-ttu-id="a18c0-255">Usuń wszystkie rekordy w `Contact` tabeli.</span><span class="sxs-lookup"><span data-stu-id="a18c0-255">Delete all the records in the `Contact` table.</span></span>
* <span data-ttu-id="a18c0-256">Ponowne uruchomienie aplikacji w celu umieszczenia bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a18c0-256">Restart the app to seed the database.</span></span>

<span data-ttu-id="a18c0-257">Zarejestruj użytkownika do przeglądania kontaktów.</span><span class="sxs-lookup"><span data-stu-id="a18c0-257">Register a user for browsing the contacts.</span></span>

<span data-ttu-id="a18c0-258">Łatwe testowanie ukończonej aplikacji jest do uruchomienia w trzech różnych przeglądarek (lub incognito/InPrivate wersje).</span><span class="sxs-lookup"><span data-stu-id="a18c0-258">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate versions).</span></span> <span data-ttu-id="a18c0-259">W jednej przeglądarki zarejestrować nowego użytkownika (na przykład `test@example.com`).</span><span class="sxs-lookup"><span data-stu-id="a18c0-259">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="a18c0-260">Zaloguj się w każdej przeglądarce z innym użytkownikiem.</span><span class="sxs-lookup"><span data-stu-id="a18c0-260">Sign in to each browser with a different user.</span></span> <span data-ttu-id="a18c0-261">Sprawdź następujące operacje:</span><span class="sxs-lookup"><span data-stu-id="a18c0-261">Verify the following operations:</span></span>

* <span data-ttu-id="a18c0-262">Zarejestrowani użytkownicy można wyświetlić wszystkie zatwierdzone dane kontaktowe.</span><span class="sxs-lookup"><span data-stu-id="a18c0-262">Registered users can view all the approved contact data.</span></span>
* <span data-ttu-id="a18c0-263">Zarejestrowani użytkownicy może edytować/usuwać swoje dane.</span><span class="sxs-lookup"><span data-stu-id="a18c0-263">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="a18c0-264">Menedżerowie mogli zatwierdzać lub odrzucać dane kontaktowe.</span><span class="sxs-lookup"><span data-stu-id="a18c0-264">Managers can approve or reject contact data.</span></span> <span data-ttu-id="a18c0-265">`Details` Wyświetlić pokazuje **Zatwierdź** i **Odrzuć** przycisków.</span><span class="sxs-lookup"><span data-stu-id="a18c0-265">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="a18c0-266">Administratorzy mogą zatwierdzać/Odrzuć i edytowanie/usuwanie żadnych danych.</span><span class="sxs-lookup"><span data-stu-id="a18c0-266">Administrators can approve/reject and edit/delete any data.</span></span>

| <span data-ttu-id="a18c0-267">Użytkownik</span><span class="sxs-lookup"><span data-stu-id="a18c0-267">User</span></span>| <span data-ttu-id="a18c0-268">Opcje</span><span class="sxs-lookup"><span data-stu-id="a18c0-268">Options</span></span> |
| ------------ | ---------|
| test@example.com | <span data-ttu-id="a18c0-269">Może edytować/usuwać własnych danych</span><span class="sxs-lookup"><span data-stu-id="a18c0-269">Can edit/delete own data</span></span> |
| manager@contoso.com | <span data-ttu-id="a18c0-270">Można zatwierdzić/Odrzuć i edytowanie/usuwanie własnych danych</span><span class="sxs-lookup"><span data-stu-id="a18c0-270">Can approve/reject and edit/delete own data</span></span> |
| admin@contoso.com | <span data-ttu-id="a18c0-271">Edytowanie/usuwanie można i Zatwierdź/Odrzuć wszystkie dane</span><span class="sxs-lookup"><span data-stu-id="a18c0-271">Can edit/delete and approve/reject all data</span></span>|

<span data-ttu-id="a18c0-272">Utwórz kontakt w przeglądarce administratora.</span><span class="sxs-lookup"><span data-stu-id="a18c0-272">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="a18c0-273">Skopiuj adres URL do usunięcia, a następnie Edytuj z skontaktowanie się z administratorem.</span><span class="sxs-lookup"><span data-stu-id="a18c0-273">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="a18c0-274">Wklej te linki do przeglądarki użytkownika testowego, aby sprawdzić, czy użytkownik testu nie może wykonywać te operacje.</span><span class="sxs-lookup"><span data-stu-id="a18c0-274">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="a18c0-275">Utwórz aplikację startową</span><span class="sxs-lookup"><span data-stu-id="a18c0-275">Create the starter app</span></span>

* <span data-ttu-id="a18c0-276">Tworzenie stron Razor aplikacji o nazwie "ContactManager"</span><span class="sxs-lookup"><span data-stu-id="a18c0-276">Create a Razor Pages app named "ContactManager"</span></span>
   * <span data-ttu-id="a18c0-277">Tworzenie aplikacji za pomocą **indywidualne konta użytkowników**.</span><span class="sxs-lookup"><span data-stu-id="a18c0-277">Create the app with **Individual User Accounts**.</span></span>
   * <span data-ttu-id="a18c0-278">Nadaj mu nazwę "ContactManager", przestrzeń nazw używaną w próbce pasujących przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="a18c0-278">Name it "ContactManager" so the namespace matches the namespace used in the sample.</span></span>
   * <span data-ttu-id="a18c0-279">`-uld` Określa LocalDB zamiast bazy danych SQLite</span><span class="sxs-lookup"><span data-stu-id="a18c0-279">`-uld` specifies LocalDB instead of SQLite</span></span>

  ```console
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* <span data-ttu-id="a18c0-280">Dodaj *Models\Contact.cs*:</span><span class="sxs-lookup"><span data-stu-id="a18c0-280">Add *Models\Contact.cs*:</span></span>

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="a18c0-281">Tworzenie szkieletu `Contact` modelu.</span><span class="sxs-lookup"><span data-stu-id="a18c0-281">Scaffold the `Contact` model.</span></span>
* <span data-ttu-id="a18c0-282">Tworzenie początkowej migracji i aktualizowanie bazy danych:</span><span class="sxs-lookup"><span data-stu-id="a18c0-282">Create initial migration and update the database:</span></span>

```console
dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
dotnet ef database drop -f
dotnet ef migrations add initial
dotnet ef database update
```

* <span data-ttu-id="a18c0-283">Aktualizacja **ContactManager** zakotwiczenia w *Pages/_Layout.cshtml* pliku:</span><span class="sxs-lookup"><span data-stu-id="a18c0-283">Update the **ContactManager** anchor in the *Pages/_Layout.cshtml* file:</span></span>

```cshtml
<a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
```

* <span data-ttu-id="a18c0-284">Przetestuj aplikację, tworzenia, edytowania i usuwania kontaktu</span><span class="sxs-lookup"><span data-stu-id="a18c0-284">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="a18c0-285">Inicjowanie bazy danych</span><span class="sxs-lookup"><span data-stu-id="a18c0-285">Seed the database</span></span>

<span data-ttu-id="a18c0-286">Dodaj [SeedData](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs) klasy *danych* folderu.</span><span class="sxs-lookup"><span data-stu-id="a18c0-286">Add the [SeedData](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs) class to the *Data* folder.</span></span>

<span data-ttu-id="a18c0-287">Wywołaj `SeedData.Initialize` z `Main`:</span><span class="sxs-lookup"><span data-stu-id="a18c0-287">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Program.cs?name=snippet)]

<span data-ttu-id="a18c0-288">Sprawdź, czy aplikacja zasilany bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a18c0-288">Test that the app seeded the database.</span></span> <span data-ttu-id="a18c0-289">W przypadku wszystkich wierszy w skontaktuj się z bazy danych, metoda inicjatora nie zostanie uruchomiony.</span><span class="sxs-lookup"><span data-stu-id="a18c0-289">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a><span data-ttu-id="a18c0-290">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="a18c0-290">Additional resources</span></span>

* <span data-ttu-id="a18c0-291">[Laboratorium autoryzacji platformy ASP.NET Core](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span><span class="sxs-lookup"><span data-stu-id="a18c0-291">[ASP.NET Core Authorization Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span></span> <span data-ttu-id="a18c0-292">W tym laboratorium zawiera bardziej szczegółowe na temat funkcji zabezpieczeń wprowadzone w ramach tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="a18c0-292">This lab goes into more detail on the security features introduced in this tutorial.</span></span>
* [<span data-ttu-id="a18c0-293">Autoryzacja w programie ASP.NET Core: prosty, rola, opartej na oświadczeniach i niestandardowe</span><span class="sxs-lookup"><span data-stu-id="a18c0-293">Authorization in ASP.NET Core: Simple, role, claims-based, and custom</span></span>](xref:security/authorization/index)
* [<span data-ttu-id="a18c0-294">Autoryzacja niestandardowa oparta na zasadach</span><span class="sxs-lookup"><span data-stu-id="a18c0-294">Custom policy-based authorization</span></span>](xref:security/authorization/policies)

::: moniker-end
