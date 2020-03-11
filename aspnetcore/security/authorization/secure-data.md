---
title: Tworzenie aplikacji platformy ASP.NET Core przy użyciu danych użytkownika chronionych przez autoryzację
author: rick-anderson
description: Dowiedz się, jak utworzyć aplikację stron Razor przy użyciu danych użytkownika chronionych przez autoryzację. Obejmuje protokołu HTTPS, uwierzytelniania, zabezpieczeń i tożsamości platformy ASP.NET Core.
ms.author: riande
ms.date: 12/18/2018
ms.custom: mvc, seodec18
uid: security/authorization/secure-data
ms.openlocfilehash: 7710a8965771db02e601dafb7da752906bcd43e5
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78659580"
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a><span data-ttu-id="c4b7c-104">Tworzenie aplikacji platformy ASP.NET Core przy użyciu danych użytkownika chronionych przez autoryzację</span><span class="sxs-lookup"><span data-stu-id="c4b7c-104">Create an ASP.NET Core app with user data protected by authorization</span></span>

<span data-ttu-id="c4b7c-105">Autorzy [Rick Anderson](https://twitter.com/RickAndMSFT) i Jan [Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="c4b7c-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="c4b7c-106">[Ten plik PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) jest wyświetlany w wersji ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-106">See [this PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC version.</span></span> <span data-ttu-id="c4b7c-107">Wersja ASP.NET Core 1,1 tego samouczka znajduje się w [tym](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data) folderze.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-107">The ASP.NET Core 1.1 version of this tutorial is in [this](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data) folder.</span></span> <span data-ttu-id="c4b7c-108">Przykład 1,1 ASP.NET Core znajduje się w [próbkach](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span><span class="sxs-lookup"><span data-stu-id="c4b7c-108">The 1.1 ASP.NET Core sample is in the [samples](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="c4b7c-109">Zobacz [ten plik PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_July16_18.pdf)</span><span class="sxs-lookup"><span data-stu-id="c4b7c-109">See [this pdf](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_July16_18.pdf)</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="c4b7c-110">W tym samouczku pokazano, jak utworzyć aplikację sieci web platformy ASP.NET Core przy użyciu danych użytkownika chronionych przez autoryzację.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-110">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="c4b7c-111">Wyświetla listę kontaktów, uwierzytelnionych użytkowników (zarejestrowane), które zostały utworzone.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-111">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="c4b7c-112">Istnieją trzy grupy zabezpieczeń:</span><span class="sxs-lookup"><span data-stu-id="c4b7c-112">There are three security groups:</span></span>

* <span data-ttu-id="c4b7c-113">**Zarejestrowani użytkownicy** mogą wyświetlać wszystkie zatwierdzone dane i edytować/usuwać własne dane.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-113">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="c4b7c-114">**Menedżerowie** mogą zatwierdzać lub odrzucać dane kontaktów.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-114">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="c4b7c-115">Tylko zatwierdzone kontakty będą widoczne dla użytkowników.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-115">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="c4b7c-116">**Administratorzy** mogą zatwierdzić/odrzucić i edytować/usunąć dowolne dane.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-116">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="c4b7c-117">Obrazy w tym dokumencie nie są dokładnie zgodne z najnowszymi szablonami.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-117">The images in this document don't exactly match the latest templates.</span></span>

<span data-ttu-id="c4b7c-118">Na poniższej ilustracji użytkownik Rick (`rick@example.com`) jest zalogowany.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-118">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="c4b7c-119">Rick mogą wyświetlać tylko zatwierdzone kontakty i **edytować**/**usunąć**/**tworzyć nowe** linki dla swoich kontaktów.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-119">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="c4b7c-120">Tylko ostatni rekord utworzony przez Rick zawiera linki do **edycji** i **usuwania** .</span><span class="sxs-lookup"><span data-stu-id="c4b7c-120">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="c4b7c-121">Inni użytkownicy nie zobaczą ostatni rekord, aż Menedżer lub administrator zmienia stan na "Zatwierdzone".</span><span class="sxs-lookup"><span data-stu-id="c4b7c-121">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![Zrzut ekranu przedstawiający Rick zalogowany](secure-data/_static/rick.png)

<span data-ttu-id="c4b7c-123">Na poniższej ilustracji `manager@contoso.com` jest zalogowany i w roli menedżera:</span><span class="sxs-lookup"><span data-stu-id="c4b7c-123">In the following image, `manager@contoso.com` is signed in and in the manager's role:</span></span>

![Zrzut ekranu przedstawiający manager@contoso.com zalogowany](secure-data/_static/manager1.png)

<span data-ttu-id="c4b7c-125">Na poniższej ilustracji przedstawiono menedżerów widoku szczegółów kontaktu:</span><span class="sxs-lookup"><span data-stu-id="c4b7c-125">The following image shows the managers details view of a contact:</span></span>

![Widok menedżera kontaktu](secure-data/_static/manager.png)

<span data-ttu-id="c4b7c-127">Przyciski **Zatwierdź** i **Odrzuć** są wyświetlane tylko dla menedżerów i administratorów.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-127">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="c4b7c-128">Na poniższej ilustracji `admin@contoso.com` jest zalogowany i w roli administratora:</span><span class="sxs-lookup"><span data-stu-id="c4b7c-128">In the following image, `admin@contoso.com` is signed in and in the administrator's role:</span></span>

![Zrzut ekranu przedstawiający admin@contoso.com zalogowany](secure-data/_static/admin.png)

<span data-ttu-id="c4b7c-130">Administrator ma wszystkie uprawnienia.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-130">The administrator has all privileges.</span></span> <span data-ttu-id="c4b7c-131">Ona można odczytu/edytowanie/usuwanie dowolnego skontaktuj się z pomocą i zmienić stan kontaktów.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-131">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="c4b7c-132">Aplikacja została utworzona przez utworzenie [szkieletu](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) następującego modelu `Contact`:</span><span class="sxs-lookup"><span data-stu-id="c4b7c-132">The app was created by [scaffolding](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) the following `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

<span data-ttu-id="c4b7c-133">Przykład zawiera poniższe obsługi autoryzacji:</span><span class="sxs-lookup"><span data-stu-id="c4b7c-133">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="c4b7c-134">`ContactIsOwnerAuthorizationHandler`: zapewnia, że użytkownik może edytować tylko swoje dane.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-134">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="c4b7c-135">`ContactManagerAuthorizationHandler`: umożliwia menedżerom zatwierdzanie lub odrzucanie kontaktów.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-135">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="c4b7c-136">`ContactAdministratorsAuthorizationHandler`: umożliwia administratorom zatwierdzanie lub odrzucanie kontaktów oraz Edytowanie/usuwanie kontaktów.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-136">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c4b7c-137">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="c4b7c-137">Prerequisites</span></span>

<span data-ttu-id="c4b7c-138">W tym samouczku jest zaawansowany.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-138">This tutorial is advanced.</span></span> <span data-ttu-id="c4b7c-139">Należy zapoznać się z:</span><span class="sxs-lookup"><span data-stu-id="c4b7c-139">You should be familiar with:</span></span>

* [<span data-ttu-id="c4b7c-140">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c4b7c-140">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="c4b7c-141">Uwierzytelnianie</span><span class="sxs-lookup"><span data-stu-id="c4b7c-141">Authentication</span></span>](xref:security/authentication/identity)
* [<span data-ttu-id="c4b7c-142">Potwierdzenie konta i odzyskiwanie hasła</span><span class="sxs-lookup"><span data-stu-id="c4b7c-142">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="c4b7c-143">Autoryzacja</span><span class="sxs-lookup"><span data-stu-id="c4b7c-143">Authorization</span></span>](xref:security/authorization/introduction)
* [<span data-ttu-id="c4b7c-144">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="c4b7c-144">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="c4b7c-145">Starter i ukończonej aplikacji</span><span class="sxs-lookup"><span data-stu-id="c4b7c-145">The starter and completed app</span></span>

<span data-ttu-id="c4b7c-146">[Pobierz](xref:index#how-to-download-a-sample) [ukończoną](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) aplikację.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-146">[Download](xref:index#how-to-download-a-sample) the [completed](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) app.</span></span> <span data-ttu-id="c4b7c-147">[Przetestuj](#test-the-completed-app) ukończoną aplikację, aby zapoznać się z jej funkcjami zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-147">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="c4b7c-148">Aplikację startową</span><span class="sxs-lookup"><span data-stu-id="c4b7c-148">The starter app</span></span>

<span data-ttu-id="c4b7c-149">[Pobierz](xref:index#how-to-download-a-sample) aplikację [startową](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) .</span><span class="sxs-lookup"><span data-stu-id="c4b7c-149">[Download](xref:index#how-to-download-a-sample) the [starter](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) app.</span></span>

<span data-ttu-id="c4b7c-150">Uruchom aplikację, naciśnij link **ContactManager** i sprawdź, czy można tworzyć, edytować i usuwać kontakty.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-150">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="c4b7c-151">Zabezpieczanie danych użytkownika</span><span class="sxs-lookup"><span data-stu-id="c4b7c-151">Secure user data</span></span>

<span data-ttu-id="c4b7c-152">Poniższe sekcje mają główne kroki umożliwiające utworzenie aplikacji danych bezpiecznego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-152">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="c4b7c-153">Może okazać się przydatne do odwoływania się do projektu ukończona.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-153">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="c4b7c-154">Powiąż dane kontaktowe dla użytkownika</span><span class="sxs-lookup"><span data-stu-id="c4b7c-154">Tie the contact data to the user</span></span>

<span data-ttu-id="c4b7c-155">Użyj identyfikatora użytkownika [tożsamości](xref:security/authentication/identity) ASP.NET, aby upewnić się, że użytkownicy będą mogli edytować swoje dane, ale nie inne dane użytkowników.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-155">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="c4b7c-156">Dodaj `OwnerID` i `ContactStatus` do modelu `Contact`:</span><span class="sxs-lookup"><span data-stu-id="c4b7c-156">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/final3/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

<span data-ttu-id="c4b7c-157">`OwnerID` jest IDENTYFIKATORem użytkownika z tabeli `AspNetUser` w bazie danych [tożsamości](xref:security/authentication/identity) .</span><span class="sxs-lookup"><span data-stu-id="c4b7c-157">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="c4b7c-158">Pole `Status` określa, czy kontakt jest widoczny dla użytkowników ogólnych.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-158">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="c4b7c-159">Tworzenie nowej migracji i aktualizowanie bazy danych:</span><span class="sxs-lookup"><span data-stu-id="c4b7c-159">Create a new migration and update the database:</span></span>

```dotnetcli
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-identity"></a><span data-ttu-id="c4b7c-160">Dodaj usługi ról do tożsamości</span><span class="sxs-lookup"><span data-stu-id="c4b7c-160">Add Role services to Identity</span></span>

<span data-ttu-id="c4b7c-161">Dołącz [Addroles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) , aby dodać usługi ról:</span><span class="sxs-lookup"><span data-stu-id="c4b7c-161">Append [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) to add Role services:</span></span>

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet2&highlight=9)]

### <a name="require-authenticated-users"></a><span data-ttu-id="c4b7c-162">Wymaga uwierzytelnionych użytkowników</span><span class="sxs-lookup"><span data-stu-id="c4b7c-162">Require authenticated users</span></span>

<span data-ttu-id="c4b7c-163">Ustaw domyślne zasady uwierzytelniania, aby wymagać od użytkowników uwierzytelniania:</span><span class="sxs-lookup"><span data-stu-id="c4b7c-163">Set the default authentication policy to require users to be authenticated:</span></span>

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet&highlight=15-99)] 

 <span data-ttu-id="c4b7c-164">Można zrezygnować z uwierzytelniania na stronie Razor, kontrolerze lub poziomie metody akcji z atrybutem `[AllowAnonymous]`.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-164">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="c4b7c-165">Ustawienie domyślne zasady uwierzytelniania, aby wymagać od użytkowników uwierzytelniania chroni nowo dodanych stronami Razor i kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-165">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="c4b7c-166">Uwierzytelnianie wymagane domyślnie jest bezpieczniejsze niż poleganie na nowych kontrolerach i Razor Pages uwzględnieniu atrybutu `[Authorize]`.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-166">Having authentication required by default is more secure than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span>

<span data-ttu-id="c4b7c-167">Dodaj [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) do stron indeksu i prywatności, aby użytkownicy anonimowi mogli uzyskać informacje o witrynie przed ich zarejestrowaniem.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-167">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index and Privacy pages so anonymous users can get information about the site before they register.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Index.cshtml.cs?highlight=1,7)]

### <a name="configure-the-test-account"></a><span data-ttu-id="c4b7c-168">Skonfiguruj konto testu</span><span class="sxs-lookup"><span data-stu-id="c4b7c-168">Configure the test account</span></span>

<span data-ttu-id="c4b7c-169">Klasa `SeedData` tworzy dwa konta: administrator i Menedżer.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-169">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="c4b7c-170">Użyj [Narzędzia Secret Manager](xref:security/app-secrets) , aby ustawić hasło dla tych kont.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-170">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="c4b7c-171">Ustaw hasło z katalogu projektu (katalog zawierający *program.cs*):</span><span class="sxs-lookup"><span data-stu-id="c4b7c-171">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```dotnetcli
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="c4b7c-172">Jeśli nie określono silnego hasła, wyjątek jest zgłaszany, gdy zostanie wywołane `SeedData.Initialize`.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-172">If a strong password is not specified, an exception is thrown when `SeedData.Initialize` is called.</span></span>

<span data-ttu-id="c4b7c-173">Zaktualizuj `Main`, aby użyć hasła testu:</span><span class="sxs-lookup"><span data-stu-id="c4b7c-173">Update `Main` to use the test password:</span></span>

[!code-csharp[](secure-data/samples/final3/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="c4b7c-174">Tworzenie konta testowe i aktualizowanie kontaktów</span><span class="sxs-lookup"><span data-stu-id="c4b7c-174">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="c4b7c-175">Zaktualizuj metodę `Initialize` w klasie `SeedData`, aby utworzyć konta testowe:</span><span class="sxs-lookup"><span data-stu-id="c4b7c-175">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[](secure-data/samples/final3/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="c4b7c-176">Dodaj identyfikator użytkownika administratora i `ContactStatus` do kontaktów.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-176">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="c4b7c-177">Określ jeden z kontaktów "Przesłane" i jednym "odrzucone".</span><span class="sxs-lookup"><span data-stu-id="c4b7c-177">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="c4b7c-178">Dodawanie stanu i Identyfikatora użytkownika do wszystkich kontaktów.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-178">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="c4b7c-179">Wyświetlane jest tylko jeden kontakt:</span><span class="sxs-lookup"><span data-stu-id="c4b7c-179">Only one contact is shown:</span></span>

[!code-csharp[](secure-data/samples/final3/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="c4b7c-180">Utwórz właściciela, Menedżer i obsługi autoryzacji administratora</span><span class="sxs-lookup"><span data-stu-id="c4b7c-180">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="c4b7c-181">Utwórz klasę `ContactIsOwnerAuthorizationHandler` w folderze *autoryzacji* .</span><span class="sxs-lookup"><span data-stu-id="c4b7c-181">Create a `ContactIsOwnerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="c4b7c-182">`ContactIsOwnerAuthorizationHandler` sprawdza, czy użytkownik, który działa na zasobów, jest właścicielem zasobu.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-182">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="c4b7c-183">Kontekst wywołań `ContactIsOwnerAuthorizationHandler` [. Powodzenie](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) , jeśli bieżący użytkownik uwierzytelniony jest właścicielem osoby kontaktowej.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-183">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="c4b7c-184">Programy obsługi autoryzacji zazwyczaj:</span><span class="sxs-lookup"><span data-stu-id="c4b7c-184">Authorization handlers generally:</span></span>

* <span data-ttu-id="c4b7c-185">Zwróć `context.Succeed`, gdy wymagania są spełnione.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-185">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="c4b7c-186">Zwróć `Task.CompletedTask`, gdy wymagania nie są spełnione.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-186">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="c4b7c-187">`Task.CompletedTask` nie powiodło się lub wystąpił błąd&mdash;zezwala na uruchamianie innych programów obsługi autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-187">`Task.CompletedTask` is not success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="c4b7c-188">Jeśli musisz jawnie niepowodzeniem, zwróć [kontekst. Nie powiodło się](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span><span class="sxs-lookup"><span data-stu-id="c4b7c-188">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="c4b7c-189">Aplikacja umożliwia skontaktuj się z pomocą właścicieli do edycji/delete/tworzenie własnych danych.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-189">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="c4b7c-190">`ContactIsOwnerAuthorizationHandler` nie musi sprawdzać operacji przesłanej w parametrze wymagania.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-190">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="c4b7c-191">Utwórz procedurę obsługi Menedżer autoryzacji</span><span class="sxs-lookup"><span data-stu-id="c4b7c-191">Create a manager authorization handler</span></span>

<span data-ttu-id="c4b7c-192">Utwórz klasę `ContactManagerAuthorizationHandler` w folderze *autoryzacji* .</span><span class="sxs-lookup"><span data-stu-id="c4b7c-192">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="c4b7c-193">`ContactManagerAuthorizationHandler` sprawdza, czy użytkownik działający na tym zasobie jest menedżerem.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-193">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="c4b7c-194">Tylko menedżerowie mogli zatwierdzać lub odrzucać zmiany zawartości (nowe lub zmienione).</span><span class="sxs-lookup"><span data-stu-id="c4b7c-194">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="c4b7c-195">Utwórz procedurę obsługi autoryzacji administratora</span><span class="sxs-lookup"><span data-stu-id="c4b7c-195">Create an administrator authorization handler</span></span>

<span data-ttu-id="c4b7c-196">Utwórz klasę `ContactAdministratorsAuthorizationHandler` w folderze *autoryzacji* .</span><span class="sxs-lookup"><span data-stu-id="c4b7c-196">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="c4b7c-197">`ContactAdministratorsAuthorizationHandler` sprawdza, czy użytkownik działający na tym zasobie jest administratorem.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-197">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="c4b7c-198">Administrator może wykonać wszystkie operacje.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-198">Administrator can do all operations.</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="c4b7c-199">Zarejestruj procedury obsługi autoryzacji</span><span class="sxs-lookup"><span data-stu-id="c4b7c-199">Register the authorization handlers</span></span>

<span data-ttu-id="c4b7c-200">Usługi korzystające z Entity Framework Core muszą być zarejestrowane dla [iniekcji zależności](xref:fundamentals/dependency-injection) przy użyciu funkcji [addscoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span><span class="sxs-lookup"><span data-stu-id="c4b7c-200">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="c4b7c-201">`ContactIsOwnerAuthorizationHandler` używa ASP.NET Core [tożsamości](xref:security/authentication/identity), która jest oparta na Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-201">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="c4b7c-202">Zarejestruj procedury obsługi w kolekcji usług, aby były dostępne dla `ContactsController` za pomocą [iniekcji zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="c4b7c-202">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="c4b7c-203">Dodaj następujący kod na końcu `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="c4b7c-203">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet_defaultPolicy&highlight=23-99)]

<span data-ttu-id="c4b7c-204">`ContactAdministratorsAuthorizationHandler` i `ContactManagerAuthorizationHandler` są dodawane jako pojedyncze.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-204">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="c4b7c-205">Są one pojedynczymi, ponieważ nie korzystają z EF, a wszystkie potrzebne informacje znajdują się w `Context` parametr metody `HandleRequirementAsync`.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-205">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="c4b7c-206">Obsługuje autoryzację</span><span class="sxs-lookup"><span data-stu-id="c4b7c-206">Support authorization</span></span>

<span data-ttu-id="c4b7c-207">W tej sekcji służy do aktualizacji stron Razor i Dodaj klasę wymagania dotyczące operacji.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-207">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="c4b7c-208">Przegląd klasy wymagania dotyczące operacji kontaktu</span><span class="sxs-lookup"><span data-stu-id="c4b7c-208">Review the contact operations requirements class</span></span>

<span data-ttu-id="c4b7c-209">Zapoznaj się z klasą `ContactOperations`.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-209">Review the `ContactOperations` class.</span></span> <span data-ttu-id="c4b7c-210">Ta klasa zawiera wymagania obsługiwanej przez aplikację:</span><span class="sxs-lookup"><span data-stu-id="c4b7c-210">This class contains the requirements the app supports:</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-razor-pages"></a><span data-ttu-id="c4b7c-211">Utwórz klasę bazową dla stron Razor kontaktów</span><span class="sxs-lookup"><span data-stu-id="c4b7c-211">Create a base class for the Contacts Razor Pages</span></span>

<span data-ttu-id="c4b7c-212">Utwórz klasę bazową, zawierający usług wykorzystanych w kontaktach stron Razor.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-212">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="c4b7c-213">Klasa bazowa umieszcza kod inicjowania w jednej lokalizacji:</span><span class="sxs-lookup"><span data-stu-id="c4b7c-213">The base class puts the initialization code in one location:</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="c4b7c-214">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="c4b7c-214">The preceding code:</span></span>

* <span data-ttu-id="c4b7c-215">Dodaje usługę `IAuthorizationService`, aby uzyskać dostęp do programów obsługi autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-215">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="c4b7c-216">Dodaje usługę tożsamości `UserManager`.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-216">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="c4b7c-217">Dodaj `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-217">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="c4b7c-218">Aktualizacja CreateModel</span><span class="sxs-lookup"><span data-stu-id="c4b7c-218">Update the CreateModel</span></span>

<span data-ttu-id="c4b7c-219">Zaktualizuj Konstruktor modelu tworzenia stron, aby używał `DI_BasePageModel` klasy bazowej:</span><span class="sxs-lookup"><span data-stu-id="c4b7c-219">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="c4b7c-220">Zaktualizuj metodę `CreateModel.OnPostAsync`, aby:</span><span class="sxs-lookup"><span data-stu-id="c4b7c-220">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="c4b7c-221">Dodaj identyfikator użytkownika do modelu `Contact`.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-221">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="c4b7c-222">Wywołania obsługi autoryzacji, aby sprawdzić, czy użytkownik ma uprawnienia do tworzenia kontaktów.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-222">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="c4b7c-223">Aktualizacja IndexModel</span><span class="sxs-lookup"><span data-stu-id="c4b7c-223">Update the IndexModel</span></span>

<span data-ttu-id="c4b7c-224">Zaktualizuj metodę `OnGetAsync` tak, aby tylko zatwierdzone kontakty były widoczne dla użytkowników ogólnych:</span><span class="sxs-lookup"><span data-stu-id="c4b7c-224">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="c4b7c-225">Aktualizacja EditModel</span><span class="sxs-lookup"><span data-stu-id="c4b7c-225">Update the EditModel</span></span>

<span data-ttu-id="c4b7c-226">Dodaj program obsługi autoryzacji, aby sprawdzić, czy użytkownik jest właścicielem kontaktu.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-226">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="c4b7c-227">Ponieważ autoryzacja zasobów jest sprawdzana, atrybut `[Authorize]` nie jest wystarczający.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-227">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="c4b7c-228">Aplikacja nie ma dostępu do zasobu, gdy atrybuty są oceniane.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-228">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="c4b7c-229">Autoryzacja na podstawie zasobów musi być imperatywnego.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-229">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="c4b7c-230">Testy muszą być wykonywane, gdy aplikacja ma dostęp do zasobu przez załadowanie go w modelu strony lub przez załadowanie go w ramach programu obsługi, sam.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-230">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="c4b7c-231">Często dostęp do zasobu, przekazując klucz zasobu.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-231">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="c4b7c-232">Aktualizacja DeleteModel</span><span class="sxs-lookup"><span data-stu-id="c4b7c-232">Update the DeleteModel</span></span>

<span data-ttu-id="c4b7c-233">Aktualizowanie modelu strony delete na potrzeby obsługi autoryzacji upewnij się, że użytkownik ma odpowiednie uprawnienia do usuwania dla kontaktu.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-233">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="c4b7c-234">Wstrzyknięcie usługi autoryzacji do widoków</span><span class="sxs-lookup"><span data-stu-id="c4b7c-234">Inject the authorization service into the views</span></span>

<span data-ttu-id="c4b7c-235">Obecnie pokazuje interfejsu użytkownika edytowania i usuwania łącza kontaktów, do których użytkownik nie może modyfikować.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-235">Currently, the UI shows edit and delete links for contacts the user can't modify.</span></span>

<span data-ttu-id="c4b7c-236">Wsuń usługę autoryzacji w pliku *Pages/_ViewImports. cshtml* , aby była dostępna dla wszystkich widoków:</span><span class="sxs-lookup"><span data-stu-id="c4b7c-236">Inject the authorization service in the *Pages/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[](secure-data/samples/final3/Pages/_ViewImports.cshtml?highlight=6-99)]

<span data-ttu-id="c4b7c-237">Powyższy znacznik dodaje kilka instrukcji `using`.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-237">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="c4b7c-238">Zaktualizuj linki **Edytuj** i **Usuń** w obszarze *strony/Kontakty/index. cshtml* , aby były renderowane tylko dla użytkowników z odpowiednimi uprawnieniami:</span><span class="sxs-lookup"><span data-stu-id="c4b7c-238">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[](secure-data/samples/final3/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> <span data-ttu-id="c4b7c-239">Ukrywanie łączy na podstawie użytkowników, którzy nie mają uprawnień do zmiany danych nie zabezpieczenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-239">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="c4b7c-240">Ukrywanie łącza sprawia, że bardziej przyjazny dla użytkownika aplikacji, wyświetlając tylko poprawne linki.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-240">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="c4b7c-241">Użytkownicy mogą hack wygenerowanego adresy URL, aby wywołać Edytuj i Usuń operacje na danych, które nie są właścicielami.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-241">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="c4b7c-242">Strona Razor lub kontrolera musi wymuszają operacje sprawdzania dostępu do zabezpieczania danych.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-242">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="c4b7c-243">Szczegóły aktualizacji</span><span class="sxs-lookup"><span data-stu-id="c4b7c-243">Update Details</span></span>

<span data-ttu-id="c4b7c-244">Zaktualizuj widok szczegółów, aby menedżerowie mogli zatwierdzać lub odrzucać kontaktów:</span><span class="sxs-lookup"><span data-stu-id="c4b7c-244">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[](secure-data/samples/final3/Pages/Contacts/Details.cshtml?name=snippet)]

<span data-ttu-id="c4b7c-245">Aktualizowanie modelu strony szczegółów:</span><span class="sxs-lookup"><span data-stu-id="c4b7c-245">Update the details page model:</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="add-or-remove-a-user-to-a-role"></a><span data-ttu-id="c4b7c-246">Dodawanie lub usuwanie użytkownika do roli</span><span class="sxs-lookup"><span data-stu-id="c4b7c-246">Add or remove a user to a role</span></span>

<span data-ttu-id="c4b7c-247">Zobacz [ten problem](https://github.com/dotnet/AspNetCore.Docs/issues/8502) , aby uzyskać informacje na temat:</span><span class="sxs-lookup"><span data-stu-id="c4b7c-247">See [this issue](https://github.com/dotnet/AspNetCore.Docs/issues/8502) for information on:</span></span>

* <span data-ttu-id="c4b7c-248">Usuwanie uprawnień z użytkownikiem.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-248">Removing privileges from a user.</span></span> <span data-ttu-id="c4b7c-249">Na przykład wyciszenie użytkownika w aplikacji czatu.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-249">For example, muting a user in a chat app.</span></span>
* <span data-ttu-id="c4b7c-250">Dodawanie uprawnień dla użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-250">Adding privileges to a user.</span></span>

<a name="challenge"></a>

## <a name="differences-between-challenge-and-forbid"></a><span data-ttu-id="c4b7c-251">Różnice między wyzwaniem i Zabroń</span><span class="sxs-lookup"><span data-stu-id="c4b7c-251">Differences between Challenge and Forbid</span></span>

<span data-ttu-id="c4b7c-252">Ta aplikacja ustawia zasady domyślne, aby [wymagać uwierzytelnionych użytkowników](#require-authenticated-users).</span><span class="sxs-lookup"><span data-stu-id="c4b7c-252">This app sets the default policy to [require authenticated users](#require-authenticated-users).</span></span> <span data-ttu-id="c4b7c-253">Poniższy kod umożliwia anonimowym użytkownikom.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-253">The following code allows anonymous users.</span></span> <span data-ttu-id="c4b7c-254">Użytkownicy anonimowi mogą wyświetlać różnice między wyzwaniem a zabranianiem.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-254">Anonymous users are allowed to show the differences between Challenge vs Forbid.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Details2.cshtml.cs?name=snippet)]

<span data-ttu-id="c4b7c-255">W powyższym kodzie:</span><span class="sxs-lookup"><span data-stu-id="c4b7c-255">In the preceding code:</span></span>

* <span data-ttu-id="c4b7c-256">Gdy użytkownik **nie** jest uwierzytelniony, zwracany jest `ChallengeResult`.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-256">When the user is **not** authenticated, a `ChallengeResult` is returned.</span></span> <span data-ttu-id="c4b7c-257">Gdy zostanie zwrócona `ChallengeResult`, użytkownik zostanie przekierowany do strony logowania.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-257">When a `ChallengeResult` is returned, the user is redirected to the sign-in page.</span></span>
* <span data-ttu-id="c4b7c-258">Gdy użytkownik jest uwierzytelniany, ale nie jest autoryzowany, zwracany jest `ForbidResult`.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-258">When the user is authenticated, but not authorized, a `ForbidResult` is returned.</span></span> <span data-ttu-id="c4b7c-259">Gdy zostanie zwrócona `ForbidResult`, użytkownik zostanie przekierowany do strony odmowa dostępu.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-259">When a `ForbidResult` is returned, the user is redirected to the access denied page.</span></span>

## <a name="test-the-completed-app"></a><span data-ttu-id="c4b7c-260">Testowanie aplikacji ukończone</span><span class="sxs-lookup"><span data-stu-id="c4b7c-260">Test the completed app</span></span>

<span data-ttu-id="c4b7c-261">Jeśli nie ustawiono jeszcze hasła dla kont użytkowników, użyj [Narzędzia Menedżera wpisów tajnych](xref:security/app-secrets#secret-manager) , aby ustawić hasło:</span><span class="sxs-lookup"><span data-stu-id="c4b7c-261">If you haven't already set a password for seeded user accounts, use the [Secret Manager tool](xref:security/app-secrets#secret-manager) to set a password:</span></span>

* <span data-ttu-id="c4b7c-262">Wybierz silne hasło: Użyj ośmiu lub więcej znaków i co najmniej jeden znak wielkie litery, numer i symboli.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-262">Choose a strong password: Use eight or more characters and at least one upper-case character, number, and symbol.</span></span> <span data-ttu-id="c4b7c-263">Na przykład `Passw0rd!` spełnia wymagania dotyczące silnych haseł.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-263">For example, `Passw0rd!` meets the strong password requirements.</span></span>
* <span data-ttu-id="c4b7c-264">Wykonaj następujące polecenie z folderu projektu, gdzie `<PW>` jest hasłem:</span><span class="sxs-lookup"><span data-stu-id="c4b7c-264">Execute the following command from the project's folder, where `<PW>` is the password:</span></span>

  ```dotnetcli
  dotnet user-secrets set SeedUserPW <PW>
  ```

<span data-ttu-id="c4b7c-265">Jeśli aplikacja ma kontaktów:</span><span class="sxs-lookup"><span data-stu-id="c4b7c-265">If the app has contacts:</span></span>

* <span data-ttu-id="c4b7c-266">Usuń wszystkie rekordy z tabeli `Contact`.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-266">Delete all of the records in the `Contact` table.</span></span>
* <span data-ttu-id="c4b7c-267">Ponowne uruchomienie aplikacji w celu umieszczenia bazy danych.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-267">Restart the app to seed the database.</span></span>

<span data-ttu-id="c4b7c-268">Łatwe testowanie ukończonej aplikacji jest do uruchomienia w trzech różnych przeglądarek (lub sesji incognito/InPrivate).</span><span class="sxs-lookup"><span data-stu-id="c4b7c-268">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate sessions).</span></span> <span data-ttu-id="c4b7c-269">W jednej przeglądarce Zarejestruj nowego użytkownika (na przykład `test@example.com`).</span><span class="sxs-lookup"><span data-stu-id="c4b7c-269">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="c4b7c-270">Zaloguj się w każdej przeglądarce z innym użytkownikiem.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-270">Sign in to each browser with a different user.</span></span> <span data-ttu-id="c4b7c-271">Sprawdź następujące operacje:</span><span class="sxs-lookup"><span data-stu-id="c4b7c-271">Verify the following operations:</span></span>

* <span data-ttu-id="c4b7c-272">Zarejestrowani użytkownicy można wyświetlić wszystkie zatwierdzone dane kontaktowe.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-272">Registered users can view all of the approved contact data.</span></span>
* <span data-ttu-id="c4b7c-273">Zarejestrowani użytkownicy może edytować/usuwać swoje dane.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-273">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="c4b7c-274">Menedżerowie mogą zatwierdzać/Odrzuć dane kontaktowe.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-274">Managers can approve/reject contact data.</span></span> <span data-ttu-id="c4b7c-275">W widoku `Details` są wyświetlane przyciski **Zatwierdź** i **Odrzuć** .</span><span class="sxs-lookup"><span data-stu-id="c4b7c-275">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="c4b7c-276">Administratorzy mogą zatwierdzać/Odrzuć i edytowanie/usuwanie wszystkich danych.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-276">Administrators can approve/reject and edit/delete all data.</span></span>

| <span data-ttu-id="c4b7c-277">Użytkownik</span><span class="sxs-lookup"><span data-stu-id="c4b7c-277">User</span></span>                | <span data-ttu-id="c4b7c-278">Zasilany przez aplikację</span><span class="sxs-lookup"><span data-stu-id="c4b7c-278">Seeded by the app</span></span> | <span data-ttu-id="c4b7c-279">Opcje</span><span class="sxs-lookup"><span data-stu-id="c4b7c-279">Options</span></span>                                  |
| ------------------- | :---------------: | ---------------------------------------- |
| test@example.com    | <span data-ttu-id="c4b7c-280">Nie</span><span class="sxs-lookup"><span data-stu-id="c4b7c-280">No</span></span>                | <span data-ttu-id="c4b7c-281">Edytowanie/usuwanie własnych danych.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-281">Edit/delete the own data.</span></span>                |
| manager@contoso.com | <span data-ttu-id="c4b7c-282">Yes</span><span class="sxs-lookup"><span data-stu-id="c4b7c-282">Yes</span></span>               | <span data-ttu-id="c4b7c-283">Zatwierdź/Odrzuć i edytowanie/usuwanie własnych danych.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-283">Approve/reject and edit/delete own data.</span></span> |
| admin@contoso.com   | <span data-ttu-id="c4b7c-284">Yes</span><span class="sxs-lookup"><span data-stu-id="c4b7c-284">Yes</span></span>               | <span data-ttu-id="c4b7c-285">Zatwierdź/Odrzuć i edytowanie/usuwanie wszystkich danych.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-285">Approve/reject and edit/delete all data.</span></span> |

<span data-ttu-id="c4b7c-286">Utwórz kontakt w przeglądarce administratora.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-286">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="c4b7c-287">Skopiuj adres URL do usunięcia, a następnie Edytuj z skontaktowanie się z administratorem.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-287">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="c4b7c-288">Wklej te linki do przeglądarki użytkownika testowego, aby sprawdzić, czy użytkownik testu nie może wykonywać te operacje.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-288">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="c4b7c-289">Utwórz aplikację startową</span><span class="sxs-lookup"><span data-stu-id="c4b7c-289">Create the starter app</span></span>

* <span data-ttu-id="c4b7c-290">Tworzenie stron Razor aplikacji o nazwie "ContactManager"</span><span class="sxs-lookup"><span data-stu-id="c4b7c-290">Create a Razor Pages app named "ContactManager"</span></span>
  * <span data-ttu-id="c4b7c-291">Utwórz aplikację przy użyciu **poszczególnych kont użytkowników**.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-291">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="c4b7c-292">Nadaj mu nazwę "ContactManager", przestrzeń nazw używaną w próbce pasujących przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-292">Name it "ContactManager" so the namespace matches the namespace used in the sample.</span></span>
  * <span data-ttu-id="c4b7c-293">`-uld` określa LocalDB zamiast oprogramowania SQLite</span><span class="sxs-lookup"><span data-stu-id="c4b7c-293">`-uld` specifies LocalDB instead of SQLite</span></span>

  ```dotnetcli
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* <span data-ttu-id="c4b7c-294">Dodaj *modele/Contact. cs*:</span><span class="sxs-lookup"><span data-stu-id="c4b7c-294">Add *Models/Contact.cs*:</span></span>

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="c4b7c-295">Tworzenie szkieletu modelu `Contact`.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-295">Scaffold the `Contact` model.</span></span>
* <span data-ttu-id="c4b7c-296">Tworzenie początkowej migracji i aktualizowanie bazy danych:</span><span class="sxs-lookup"><span data-stu-id="c4b7c-296">Create initial migration and update the database:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet tool install -g dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
dotnet ef database drop -f
dotnet ef migrations add initial
dotnet ef database update
```

<span data-ttu-id="c4b7c-297">Jeśli wystąpi błąd przy użyciu polecenia `dotnet aspnet-codegenerator razorpage`, zobacz [ten problem](https://github.com/aspnet/Scaffolding/issues/984)w usłudze GitHub.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-297">If you experience a bug with the `dotnet aspnet-codegenerator razorpage` command, see [this GitHub issue](https://github.com/aspnet/Scaffolding/issues/984).</span></span>

* <span data-ttu-id="c4b7c-298">Zaktualizuj kotwicę **ContactManager** w pliku *Pages/Shared/_Layout. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="c4b7c-298">Update the **ContactManager** anchor in the *Pages/Shared/_Layout.cshtml* file:</span></span>

 ```cshtml
<a class="navbar-brand" asp-area="" asp-page="/Contacts/Index">ContactManager</a>
  ```

* <span data-ttu-id="c4b7c-299">Przetestuj aplikację, tworzenia, edytowania i usuwania kontaktu</span><span class="sxs-lookup"><span data-stu-id="c4b7c-299">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="c4b7c-300">Inicjowanie bazy danych</span><span class="sxs-lookup"><span data-stu-id="c4b7c-300">Seed the database</span></span>

<span data-ttu-id="c4b7c-301">Dodaj klasę [SeedData](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter3/Data/SeedData.cs) do folderu *danych* :</span><span class="sxs-lookup"><span data-stu-id="c4b7c-301">Add the [SeedData](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter3/Data/SeedData.cs) class to the *Data* folder:</span></span>

[!code-csharp[](secure-data/samples/starter3/Data/SeedData.cs)]

<span data-ttu-id="c4b7c-302">Wywołaj `SeedData.Initialize` z `Main`:</span><span class="sxs-lookup"><span data-stu-id="c4b7c-302">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[](secure-data/samples/starter3/Program.cs)]

<span data-ttu-id="c4b7c-303">Sprawdź, czy aplikacja zasilany bazy danych.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-303">Test that the app seeded the database.</span></span> <span data-ttu-id="c4b7c-304">W przypadku wszystkich wierszy w skontaktuj się z bazy danych, metoda inicjatora nie zostanie uruchomiony.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-304">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1 < aspnetcore-3.0"

<span data-ttu-id="c4b7c-305">W tym samouczku pokazano, jak utworzyć aplikację sieci web platformy ASP.NET Core przy użyciu danych użytkownika chronionych przez autoryzację.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-305">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="c4b7c-306">Wyświetla listę kontaktów, uwierzytelnionych użytkowników (zarejestrowane), które zostały utworzone.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-306">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="c4b7c-307">Istnieją trzy grupy zabezpieczeń:</span><span class="sxs-lookup"><span data-stu-id="c4b7c-307">There are three security groups:</span></span>

* <span data-ttu-id="c4b7c-308">**Zarejestrowani użytkownicy** mogą wyświetlać wszystkie zatwierdzone dane i edytować/usuwać własne dane.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-308">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="c4b7c-309">**Menedżerowie** mogą zatwierdzać lub odrzucać dane kontaktów.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-309">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="c4b7c-310">Tylko zatwierdzone kontakty będą widoczne dla użytkowników.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-310">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="c4b7c-311">**Administratorzy** mogą zatwierdzić/odrzucić i edytować/usunąć dowolne dane.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-311">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="c4b7c-312">Na poniższej ilustracji użytkownik Rick (`rick@example.com`) jest zalogowany.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-312">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="c4b7c-313">Rick mogą wyświetlać tylko zatwierdzone kontakty i **edytować**/**usunąć**/**tworzyć nowe** linki dla swoich kontaktów.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-313">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="c4b7c-314">Tylko ostatni rekord utworzony przez Rick zawiera linki do **edycji** i **usuwania** .</span><span class="sxs-lookup"><span data-stu-id="c4b7c-314">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="c4b7c-315">Inni użytkownicy nie zobaczą ostatni rekord, aż Menedżer lub administrator zmienia stan na "Zatwierdzone".</span><span class="sxs-lookup"><span data-stu-id="c4b7c-315">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![Zrzut ekranu przedstawiający Rick zalogowany](secure-data/_static/rick.png)

<span data-ttu-id="c4b7c-317">Na poniższej ilustracji `manager@contoso.com` jest zalogowany i w roli menedżera:</span><span class="sxs-lookup"><span data-stu-id="c4b7c-317">In the following image, `manager@contoso.com` is signed in and in the manager's role:</span></span>

![Zrzut ekranu przedstawiający manager@contoso.com zalogowany](secure-data/_static/manager1.png)

<span data-ttu-id="c4b7c-319">Na poniższej ilustracji przedstawiono menedżerów widoku szczegółów kontaktu:</span><span class="sxs-lookup"><span data-stu-id="c4b7c-319">The following image shows the managers details view of a contact:</span></span>

![Widok menedżera kontaktu](secure-data/_static/manager.png)

<span data-ttu-id="c4b7c-321">Przyciski **Zatwierdź** i **Odrzuć** są wyświetlane tylko dla menedżerów i administratorów.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-321">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="c4b7c-322">Na poniższej ilustracji `admin@contoso.com` jest zalogowany i w roli administratora:</span><span class="sxs-lookup"><span data-stu-id="c4b7c-322">In the following image, `admin@contoso.com` is signed in and in the administrator's role:</span></span>

![Zrzut ekranu przedstawiający admin@contoso.com zalogowany](secure-data/_static/admin.png)

<span data-ttu-id="c4b7c-324">Administrator ma wszystkie uprawnienia.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-324">The administrator has all privileges.</span></span> <span data-ttu-id="c4b7c-325">Ona można odczytu/edytowanie/usuwanie dowolnego skontaktuj się z pomocą i zmienić stan kontaktów.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-325">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="c4b7c-326">Aplikacja została utworzona przez utworzenie [szkieletu](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) następującego modelu `Contact`:</span><span class="sxs-lookup"><span data-stu-id="c4b7c-326">The app was created by [scaffolding](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) the following `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

<span data-ttu-id="c4b7c-327">Przykład zawiera poniższe obsługi autoryzacji:</span><span class="sxs-lookup"><span data-stu-id="c4b7c-327">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="c4b7c-328">`ContactIsOwnerAuthorizationHandler`: zapewnia, że użytkownik może edytować tylko swoje dane.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-328">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="c4b7c-329">`ContactManagerAuthorizationHandler`: umożliwia menedżerom zatwierdzanie lub odrzucanie kontaktów.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-329">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="c4b7c-330">`ContactAdministratorsAuthorizationHandler`: umożliwia administratorom zatwierdzanie lub odrzucanie kontaktów oraz Edytowanie/usuwanie kontaktów.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-330">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c4b7c-331">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="c4b7c-331">Prerequisites</span></span>

<span data-ttu-id="c4b7c-332">W tym samouczku jest zaawansowany.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-332">This tutorial is advanced.</span></span> <span data-ttu-id="c4b7c-333">Należy zapoznać się z:</span><span class="sxs-lookup"><span data-stu-id="c4b7c-333">You should be familiar with:</span></span>

* [<span data-ttu-id="c4b7c-334">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c4b7c-334">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="c4b7c-335">Uwierzytelnianie</span><span class="sxs-lookup"><span data-stu-id="c4b7c-335">Authentication</span></span>](xref:security/authentication/identity)
* [<span data-ttu-id="c4b7c-336">Potwierdzenie konta i odzyskiwanie hasła</span><span class="sxs-lookup"><span data-stu-id="c4b7c-336">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="c4b7c-337">Autoryzacja</span><span class="sxs-lookup"><span data-stu-id="c4b7c-337">Authorization</span></span>](xref:security/authorization/introduction)
* [<span data-ttu-id="c4b7c-338">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="c4b7c-338">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="c4b7c-339">Starter i ukończonej aplikacji</span><span class="sxs-lookup"><span data-stu-id="c4b7c-339">The starter and completed app</span></span>

<span data-ttu-id="c4b7c-340">[Pobierz](xref:index#how-to-download-a-sample) [ukończoną](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) aplikację.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-340">[Download](xref:index#how-to-download-a-sample) the [completed](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) app.</span></span> <span data-ttu-id="c4b7c-341">[Przetestuj](#test-the-completed-app) ukończoną aplikację, aby zapoznać się z jej funkcjami zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-341">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="c4b7c-342">Aplikację startową</span><span class="sxs-lookup"><span data-stu-id="c4b7c-342">The starter app</span></span>

<span data-ttu-id="c4b7c-343">[Pobierz](xref:index#how-to-download-a-sample) aplikację [startową](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) .</span><span class="sxs-lookup"><span data-stu-id="c4b7c-343">[Download](xref:index#how-to-download-a-sample) the [starter](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) app.</span></span>

<span data-ttu-id="c4b7c-344">Uruchom aplikację, naciśnij link **ContactManager** i sprawdź, czy można tworzyć, edytować i usuwać kontakty.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-344">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="c4b7c-345">Zabezpieczanie danych użytkownika</span><span class="sxs-lookup"><span data-stu-id="c4b7c-345">Secure user data</span></span>

<span data-ttu-id="c4b7c-346">Poniższe sekcje mają główne kroki umożliwiające utworzenie aplikacji danych bezpiecznego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-346">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="c4b7c-347">Może okazać się przydatne do odwoływania się do projektu ukończona.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-347">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="c4b7c-348">Powiąż dane kontaktowe dla użytkownika</span><span class="sxs-lookup"><span data-stu-id="c4b7c-348">Tie the contact data to the user</span></span>

<span data-ttu-id="c4b7c-349">Użyj identyfikatora użytkownika [tożsamości](xref:security/authentication/identity) ASP.NET, aby upewnić się, że użytkownicy będą mogli edytować swoje dane, ale nie inne dane użytkowników.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-349">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="c4b7c-350">Dodaj `OwnerID` i `ContactStatus` do modelu `Contact`:</span><span class="sxs-lookup"><span data-stu-id="c4b7c-350">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

<span data-ttu-id="c4b7c-351">`OwnerID` jest IDENTYFIKATORem użytkownika z tabeli `AspNetUser` w bazie danych [tożsamości](xref:security/authentication/identity) .</span><span class="sxs-lookup"><span data-stu-id="c4b7c-351">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="c4b7c-352">Pole `Status` określa, czy kontakt jest widoczny dla użytkowników ogólnych.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-352">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="c4b7c-353">Tworzenie nowej migracji i aktualizowanie bazy danych:</span><span class="sxs-lookup"><span data-stu-id="c4b7c-353">Create a new migration and update the database:</span></span>

```dotnetcli
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-identity"></a><span data-ttu-id="c4b7c-354">Dodaj usługi ról do tożsamości</span><span class="sxs-lookup"><span data-stu-id="c4b7c-354">Add Role services to Identity</span></span>

<span data-ttu-id="c4b7c-355">Dołącz [Addroles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) , aby dodać usługi ról:</span><span class="sxs-lookup"><span data-stu-id="c4b7c-355">Append [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) to add Role services:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet2&highlight=12)]

### <a name="require-authenticated-users"></a><span data-ttu-id="c4b7c-356">Wymaga uwierzytelnionych użytkowników</span><span class="sxs-lookup"><span data-stu-id="c4b7c-356">Require authenticated users</span></span>

<span data-ttu-id="c4b7c-357">Ustaw domyślne zasady uwierzytelniania, aby wymagać od użytkowników uwierzytelniania:</span><span class="sxs-lookup"><span data-stu-id="c4b7c-357">Set the default authentication policy to require users to be authenticated:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet&highlight=17-99)] 

 <span data-ttu-id="c4b7c-358">Można zrezygnować z uwierzytelniania na stronie Razor, kontrolerze lub poziomie metody akcji z atrybutem `[AllowAnonymous]`.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-358">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="c4b7c-359">Ustawienie domyślne zasady uwierzytelniania, aby wymagać od użytkowników uwierzytelniania chroni nowo dodanych stronami Razor i kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-359">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="c4b7c-360">Uwierzytelnianie wymagane domyślnie jest bezpieczniejsze niż poleganie na nowych kontrolerach i Razor Pages uwzględnieniu atrybutu `[Authorize]`.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-360">Having authentication required by default is more secure than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span>

<span data-ttu-id="c4b7c-361">Dodaj [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) do strony indeks, informacje i kontakty, dzięki czemu anonimowi użytkownicy mogą uzyskać informacje o witrynie przed ich zarejestrowaniem.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-361">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index, About, and Contact pages so anonymous users can get information about the site before they register.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Index.cshtml.cs?highlight=1,6)]

### <a name="configure-the-test-account"></a><span data-ttu-id="c4b7c-362">Skonfiguruj konto testu</span><span class="sxs-lookup"><span data-stu-id="c4b7c-362">Configure the test account</span></span>

<span data-ttu-id="c4b7c-363">Klasa `SeedData` tworzy dwa konta: administrator i Menedżer.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-363">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="c4b7c-364">Użyj [Narzędzia Secret Manager](xref:security/app-secrets) , aby ustawić hasło dla tych kont.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-364">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="c4b7c-365">Ustaw hasło z katalogu projektu (katalog zawierający *program.cs*):</span><span class="sxs-lookup"><span data-stu-id="c4b7c-365">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```dotnetcli
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="c4b7c-366">Jeśli nie określono silnego hasła, wyjątek jest zgłaszany, gdy zostanie wywołane `SeedData.Initialize`.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-366">If a strong password is not specified, an exception is thrown when `SeedData.Initialize` is called.</span></span>

<span data-ttu-id="c4b7c-367">Zaktualizuj `Main`, aby użyć hasła testu:</span><span class="sxs-lookup"><span data-stu-id="c4b7c-367">Update `Main` to use the test password:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="c4b7c-368">Tworzenie konta testowe i aktualizowanie kontaktów</span><span class="sxs-lookup"><span data-stu-id="c4b7c-368">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="c4b7c-369">Zaktualizuj metodę `Initialize` w klasie `SeedData`, aby utworzyć konta testowe:</span><span class="sxs-lookup"><span data-stu-id="c4b7c-369">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="c4b7c-370">Dodaj identyfikator użytkownika administratora i `ContactStatus` do kontaktów.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-370">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="c4b7c-371">Określ jeden z kontaktów "Przesłane" i jednym "odrzucone".</span><span class="sxs-lookup"><span data-stu-id="c4b7c-371">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="c4b7c-372">Dodawanie stanu i Identyfikatora użytkownika do wszystkich kontaktów.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-372">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="c4b7c-373">Wyświetlane jest tylko jeden kontakt:</span><span class="sxs-lookup"><span data-stu-id="c4b7c-373">Only one contact is shown:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="c4b7c-374">Utwórz właściciela, Menedżer i obsługi autoryzacji administratora</span><span class="sxs-lookup"><span data-stu-id="c4b7c-374">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="c4b7c-375">Utwórz folder *autoryzacji* i Utwórz w nim klasę `ContactIsOwnerAuthorizationHandler`.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-375">Create an *Authorization* folder and create a `ContactIsOwnerAuthorizationHandler` class in it.</span></span> <span data-ttu-id="c4b7c-376">`ContactIsOwnerAuthorizationHandler` sprawdza, czy użytkownik, który działa na zasobów, jest właścicielem zasobu.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-376">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="c4b7c-377">Kontekst wywołań `ContactIsOwnerAuthorizationHandler` [. Powodzenie](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) , jeśli bieżący użytkownik uwierzytelniony jest właścicielem osoby kontaktowej.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-377">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="c4b7c-378">Programy obsługi autoryzacji zazwyczaj:</span><span class="sxs-lookup"><span data-stu-id="c4b7c-378">Authorization handlers generally:</span></span>

* <span data-ttu-id="c4b7c-379">Zwróć `context.Succeed`, gdy wymagania są spełnione.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-379">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="c4b7c-380">Zwróć `Task.CompletedTask`, gdy wymagania nie są spełnione.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-380">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="c4b7c-381">`Task.CompletedTask` nie powiodło się lub wystąpił błąd&mdash;zezwala na uruchamianie innych programów obsługi autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-381">`Task.CompletedTask` is not success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="c4b7c-382">Jeśli musisz jawnie niepowodzeniem, zwróć [kontekst. Nie powiodło się](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span><span class="sxs-lookup"><span data-stu-id="c4b7c-382">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="c4b7c-383">Aplikacja umożliwia skontaktuj się z pomocą właścicieli do edycji/delete/tworzenie własnych danych.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-383">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="c4b7c-384">`ContactIsOwnerAuthorizationHandler` nie musi sprawdzać operacji przesłanej w parametrze wymagania.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-384">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="c4b7c-385">Utwórz procedurę obsługi Menedżer autoryzacji</span><span class="sxs-lookup"><span data-stu-id="c4b7c-385">Create a manager authorization handler</span></span>

<span data-ttu-id="c4b7c-386">Utwórz klasę `ContactManagerAuthorizationHandler` w folderze *autoryzacji* .</span><span class="sxs-lookup"><span data-stu-id="c4b7c-386">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="c4b7c-387">`ContactManagerAuthorizationHandler` sprawdza, czy użytkownik działający na tym zasobie jest menedżerem.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-387">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="c4b7c-388">Tylko menedżerowie mogli zatwierdzać lub odrzucać zmiany zawartości (nowe lub zmienione).</span><span class="sxs-lookup"><span data-stu-id="c4b7c-388">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="c4b7c-389">Utwórz procedurę obsługi autoryzacji administratora</span><span class="sxs-lookup"><span data-stu-id="c4b7c-389">Create an administrator authorization handler</span></span>

<span data-ttu-id="c4b7c-390">Utwórz klasę `ContactAdministratorsAuthorizationHandler` w folderze *autoryzacji* .</span><span class="sxs-lookup"><span data-stu-id="c4b7c-390">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="c4b7c-391">`ContactAdministratorsAuthorizationHandler` sprawdza, czy użytkownik działający na tym zasobie jest administratorem.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-391">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="c4b7c-392">Administrator może wykonać wszystkie operacje.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-392">Administrator can do all operations.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="c4b7c-393">Zarejestruj procedury obsługi autoryzacji</span><span class="sxs-lookup"><span data-stu-id="c4b7c-393">Register the authorization handlers</span></span>

<span data-ttu-id="c4b7c-394">Usługi korzystające z Entity Framework Core muszą być zarejestrowane dla [iniekcji zależności](xref:fundamentals/dependency-injection) przy użyciu funkcji [addscoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span><span class="sxs-lookup"><span data-stu-id="c4b7c-394">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="c4b7c-395">`ContactIsOwnerAuthorizationHandler` używa ASP.NET Core [tożsamości](xref:security/authentication/identity), która jest oparta na Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-395">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="c4b7c-396">Zarejestruj procedury obsługi w kolekcji usług, aby były dostępne dla `ContactsController` za pomocą [iniekcji zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="c4b7c-396">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="c4b7c-397">Dodaj następujący kod na końcu `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="c4b7c-397">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet_defaultPolicy&highlight=27-99)]

<span data-ttu-id="c4b7c-398">`ContactAdministratorsAuthorizationHandler` i `ContactManagerAuthorizationHandler` są dodawane jako pojedyncze.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-398">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="c4b7c-399">Są one pojedynczymi, ponieważ nie korzystają z EF, a wszystkie potrzebne informacje znajdują się w `Context` parametr metody `HandleRequirementAsync`.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-399">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="c4b7c-400">Obsługuje autoryzację</span><span class="sxs-lookup"><span data-stu-id="c4b7c-400">Support authorization</span></span>

<span data-ttu-id="c4b7c-401">W tej sekcji służy do aktualizacji stron Razor i Dodaj klasę wymagania dotyczące operacji.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-401">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="c4b7c-402">Przegląd klasy wymagania dotyczące operacji kontaktu</span><span class="sxs-lookup"><span data-stu-id="c4b7c-402">Review the contact operations requirements class</span></span>

<span data-ttu-id="c4b7c-403">Zapoznaj się z klasą `ContactOperations`.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-403">Review the `ContactOperations` class.</span></span> <span data-ttu-id="c4b7c-404">Ta klasa zawiera wymagania obsługiwanej przez aplikację:</span><span class="sxs-lookup"><span data-stu-id="c4b7c-404">This class contains the requirements the app supports:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-razor-pages"></a><span data-ttu-id="c4b7c-405">Utwórz klasę bazową dla stron Razor kontaktów</span><span class="sxs-lookup"><span data-stu-id="c4b7c-405">Create a base class for the Contacts Razor Pages</span></span>

<span data-ttu-id="c4b7c-406">Utwórz klasę bazową, zawierający usług wykorzystanych w kontaktach stron Razor.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-406">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="c4b7c-407">Klasa bazowa umieszcza kod inicjowania w jednej lokalizacji:</span><span class="sxs-lookup"><span data-stu-id="c4b7c-407">The base class puts the initialization code in one location:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="c4b7c-408">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="c4b7c-408">The preceding code:</span></span>

* <span data-ttu-id="c4b7c-409">Dodaje usługę `IAuthorizationService`, aby uzyskać dostęp do programów obsługi autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-409">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="c4b7c-410">Dodaje usługę tożsamości `UserManager`.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-410">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="c4b7c-411">Dodaj `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-411">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="c4b7c-412">Aktualizacja CreateModel</span><span class="sxs-lookup"><span data-stu-id="c4b7c-412">Update the CreateModel</span></span>

<span data-ttu-id="c4b7c-413">Zaktualizuj Konstruktor modelu tworzenia stron, aby używał `DI_BasePageModel` klasy bazowej:</span><span class="sxs-lookup"><span data-stu-id="c4b7c-413">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="c4b7c-414">Zaktualizuj metodę `CreateModel.OnPostAsync`, aby:</span><span class="sxs-lookup"><span data-stu-id="c4b7c-414">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="c4b7c-415">Dodaj identyfikator użytkownika do modelu `Contact`.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-415">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="c4b7c-416">Wywołania obsługi autoryzacji, aby sprawdzić, czy użytkownik ma uprawnienia do tworzenia kontaktów.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-416">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="c4b7c-417">Aktualizacja IndexModel</span><span class="sxs-lookup"><span data-stu-id="c4b7c-417">Update the IndexModel</span></span>

<span data-ttu-id="c4b7c-418">Zaktualizuj metodę `OnGetAsync` tak, aby tylko zatwierdzone kontakty były widoczne dla użytkowników ogólnych:</span><span class="sxs-lookup"><span data-stu-id="c4b7c-418">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="c4b7c-419">Aktualizacja EditModel</span><span class="sxs-lookup"><span data-stu-id="c4b7c-419">Update the EditModel</span></span>

<span data-ttu-id="c4b7c-420">Dodaj program obsługi autoryzacji, aby sprawdzić, czy użytkownik jest właścicielem kontaktu.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-420">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="c4b7c-421">Ponieważ autoryzacja zasobów jest sprawdzana, atrybut `[Authorize]` nie jest wystarczający.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-421">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="c4b7c-422">Aplikacja nie ma dostępu do zasobu, gdy atrybuty są oceniane.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-422">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="c4b7c-423">Autoryzacja na podstawie zasobów musi być imperatywnego.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-423">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="c4b7c-424">Testy muszą być wykonywane, gdy aplikacja ma dostęp do zasobu przez załadowanie go w modelu strony lub przez załadowanie go w ramach programu obsługi, sam.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-424">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="c4b7c-425">Często dostęp do zasobu, przekazując klucz zasobu.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-425">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="c4b7c-426">Aktualizacja DeleteModel</span><span class="sxs-lookup"><span data-stu-id="c4b7c-426">Update the DeleteModel</span></span>

<span data-ttu-id="c4b7c-427">Aktualizowanie modelu strony delete na potrzeby obsługi autoryzacji upewnij się, że użytkownik ma odpowiednie uprawnienia do usuwania dla kontaktu.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-427">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="c4b7c-428">Wstrzyknięcie usługi autoryzacji do widoków</span><span class="sxs-lookup"><span data-stu-id="c4b7c-428">Inject the authorization service into the views</span></span>

<span data-ttu-id="c4b7c-429">Obecnie pokazuje interfejsu użytkownika edytowania i usuwania łącza kontaktów, do których użytkownik nie może modyfikować.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-429">Currently, the UI shows edit and delete links for contacts the user can't modify.</span></span>

<span data-ttu-id="c4b7c-430">Wsuń usługę autoryzacji w pliku *views/_ViewImports. cshtml* , aby była dostępna dla wszystkich widoków:</span><span class="sxs-lookup"><span data-stu-id="c4b7c-430">Inject the authorization service in the *Views/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/_ViewImports.cshtml?highlight=6-99)]

<span data-ttu-id="c4b7c-431">Powyższy znacznik dodaje kilka instrukcji `using`.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-431">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="c4b7c-432">Zaktualizuj linki **Edytuj** i **Usuń** w obszarze *strony/Kontakty/index. cshtml* , aby były renderowane tylko dla użytkowników z odpowiednimi uprawnieniami:</span><span class="sxs-lookup"><span data-stu-id="c4b7c-432">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> <span data-ttu-id="c4b7c-433">Ukrywanie łączy na podstawie użytkowników, którzy nie mają uprawnień do zmiany danych nie zabezpieczenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-433">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="c4b7c-434">Ukrywanie łącza sprawia, że bardziej przyjazny dla użytkownika aplikacji, wyświetlając tylko poprawne linki.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-434">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="c4b7c-435">Użytkownicy mogą hack wygenerowanego adresy URL, aby wywołać Edytuj i Usuń operacje na danych, które nie są właścicielami.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-435">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="c4b7c-436">Strona Razor lub kontrolera musi wymuszają operacje sprawdzania dostępu do zabezpieczania danych.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-436">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="c4b7c-437">Szczegóły aktualizacji</span><span class="sxs-lookup"><span data-stu-id="c4b7c-437">Update Details</span></span>

<span data-ttu-id="c4b7c-438">Zaktualizuj widok szczegółów, aby menedżerowie mogli zatwierdzać lub odrzucać kontaktów:</span><span class="sxs-lookup"><span data-stu-id="c4b7c-438">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml?name=snippet)]

<span data-ttu-id="c4b7c-439">Aktualizowanie modelu strony szczegółów:</span><span class="sxs-lookup"><span data-stu-id="c4b7c-439">Update the details page model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="add-or-remove-a-user-to-a-role"></a><span data-ttu-id="c4b7c-440">Dodawanie lub usuwanie użytkownika do roli</span><span class="sxs-lookup"><span data-stu-id="c4b7c-440">Add or remove a user to a role</span></span>

<span data-ttu-id="c4b7c-441">Zobacz [ten problem](https://github.com/dotnet/AspNetCore.Docs/issues/8502) , aby uzyskać informacje na temat:</span><span class="sxs-lookup"><span data-stu-id="c4b7c-441">See [this issue](https://github.com/dotnet/AspNetCore.Docs/issues/8502) for information on:</span></span>

* <span data-ttu-id="c4b7c-442">Usuwanie uprawnień z użytkownikiem.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-442">Removing privileges from a user.</span></span> <span data-ttu-id="c4b7c-443">Na przykład wyciszenie użytkownika w aplikacji czatu.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-443">For example, muting a user in a chat app.</span></span>
* <span data-ttu-id="c4b7c-444">Dodawanie uprawnień dla użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-444">Adding privileges to a user.</span></span>

## <a name="test-the-completed-app"></a><span data-ttu-id="c4b7c-445">Testowanie aplikacji ukończone</span><span class="sxs-lookup"><span data-stu-id="c4b7c-445">Test the completed app</span></span>

<span data-ttu-id="c4b7c-446">Jeśli nie ustawiono jeszcze hasła dla kont użytkowników, użyj [Narzędzia Menedżera wpisów tajnych](xref:security/app-secrets#secret-manager) , aby ustawić hasło:</span><span class="sxs-lookup"><span data-stu-id="c4b7c-446">If you haven't already set a password for seeded user accounts, use the [Secret Manager tool](xref:security/app-secrets#secret-manager) to set a password:</span></span>

* <span data-ttu-id="c4b7c-447">Wybierz silne hasło: Użyj ośmiu lub więcej znaków i co najmniej jeden znak wielkie litery, numer i symboli.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-447">Choose a strong password: Use eight or more characters and at least one upper-case character, number, and symbol.</span></span> <span data-ttu-id="c4b7c-448">Na przykład `Passw0rd!` spełnia wymagania dotyczące silnych haseł.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-448">For example, `Passw0rd!` meets the strong password requirements.</span></span>
* <span data-ttu-id="c4b7c-449">Wykonaj następujące polecenie z folderu projektu, gdzie `<PW>` jest hasłem:</span><span class="sxs-lookup"><span data-stu-id="c4b7c-449">Execute the following command from the project's folder, where `<PW>` is the password:</span></span>

  ```dotnetcli
  dotnet user-secrets set SeedUserPW <PW>
  ```

* <span data-ttu-id="c4b7c-450">Porzuć i zaktualizuj bazę danych</span><span class="sxs-lookup"><span data-stu-id="c4b7c-450">Drop and update the Database</span></span>

  ```dotnetcli
  dotnet ef database drop -f
  dotnet ef database update  
  ```

* <span data-ttu-id="c4b7c-451">Ponowne uruchomienie aplikacji w celu umieszczenia bazy danych.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-451">Restart the app to seed the database.</span></span>

<span data-ttu-id="c4b7c-452">Łatwe testowanie ukończonej aplikacji jest do uruchomienia w trzech różnych przeglądarek (lub sesji incognito/InPrivate).</span><span class="sxs-lookup"><span data-stu-id="c4b7c-452">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate sessions).</span></span> <span data-ttu-id="c4b7c-453">W jednej przeglądarce Zarejestruj nowego użytkownika (na przykład `test@example.com`).</span><span class="sxs-lookup"><span data-stu-id="c4b7c-453">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="c4b7c-454">Zaloguj się w każdej przeglądarce z innym użytkownikiem.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-454">Sign in to each browser with a different user.</span></span> <span data-ttu-id="c4b7c-455">Sprawdź następujące operacje:</span><span class="sxs-lookup"><span data-stu-id="c4b7c-455">Verify the following operations:</span></span>

* <span data-ttu-id="c4b7c-456">Zarejestrowani użytkownicy można wyświetlić wszystkie zatwierdzone dane kontaktowe.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-456">Registered users can view all of the approved contact data.</span></span>
* <span data-ttu-id="c4b7c-457">Zarejestrowani użytkownicy może edytować/usuwać swoje dane.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-457">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="c4b7c-458">Menedżerowie mogą zatwierdzać/Odrzuć dane kontaktowe.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-458">Managers can approve/reject contact data.</span></span> <span data-ttu-id="c4b7c-459">W widoku `Details` są wyświetlane przyciski **Zatwierdź** i **Odrzuć** .</span><span class="sxs-lookup"><span data-stu-id="c4b7c-459">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="c4b7c-460">Administratorzy mogą zatwierdzać/Odrzuć i edytowanie/usuwanie wszystkich danych.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-460">Administrators can approve/reject and edit/delete all data.</span></span>

| <span data-ttu-id="c4b7c-461">Użytkownik</span><span class="sxs-lookup"><span data-stu-id="c4b7c-461">User</span></span>                | <span data-ttu-id="c4b7c-462">Zasilany przez aplikację</span><span class="sxs-lookup"><span data-stu-id="c4b7c-462">Seeded by the app</span></span> | <span data-ttu-id="c4b7c-463">Opcje</span><span class="sxs-lookup"><span data-stu-id="c4b7c-463">Options</span></span>                                  |
| ------------------- | :---------------: | ---------------------------------------- |
| test@example.com    | <span data-ttu-id="c4b7c-464">Nie</span><span class="sxs-lookup"><span data-stu-id="c4b7c-464">No</span></span>                | <span data-ttu-id="c4b7c-465">Edytowanie/usuwanie własnych danych.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-465">Edit/delete the own data.</span></span>                |
| manager@contoso.com | <span data-ttu-id="c4b7c-466">Yes</span><span class="sxs-lookup"><span data-stu-id="c4b7c-466">Yes</span></span>               | <span data-ttu-id="c4b7c-467">Zatwierdź/Odrzuć i edytowanie/usuwanie własnych danych.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-467">Approve/reject and edit/delete own data.</span></span> |
| admin@contoso.com   | <span data-ttu-id="c4b7c-468">Yes</span><span class="sxs-lookup"><span data-stu-id="c4b7c-468">Yes</span></span>               | <span data-ttu-id="c4b7c-469">Zatwierdź/Odrzuć i edytowanie/usuwanie wszystkich danych.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-469">Approve/reject and edit/delete all data.</span></span> |

<span data-ttu-id="c4b7c-470">Utwórz kontakt w przeglądarce administratora.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-470">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="c4b7c-471">Skopiuj adres URL do usunięcia, a następnie Edytuj z skontaktowanie się z administratorem.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-471">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="c4b7c-472">Wklej te linki do przeglądarki użytkownika testowego, aby sprawdzić, czy użytkownik testu nie może wykonywać te operacje.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-472">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="c4b7c-473">Utwórz aplikację startową</span><span class="sxs-lookup"><span data-stu-id="c4b7c-473">Create the starter app</span></span>

* <span data-ttu-id="c4b7c-474">Tworzenie stron Razor aplikacji o nazwie "ContactManager"</span><span class="sxs-lookup"><span data-stu-id="c4b7c-474">Create a Razor Pages app named "ContactManager"</span></span>
  * <span data-ttu-id="c4b7c-475">Utwórz aplikację przy użyciu **poszczególnych kont użytkowników**.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-475">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="c4b7c-476">Nadaj mu nazwę "ContactManager", przestrzeń nazw używaną w próbce pasujących przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-476">Name it "ContactManager" so the namespace matches the namespace used in the sample.</span></span>
  * <span data-ttu-id="c4b7c-477">`-uld` określa LocalDB zamiast oprogramowania SQLite</span><span class="sxs-lookup"><span data-stu-id="c4b7c-477">`-uld` specifies LocalDB instead of SQLite</span></span>

  ```dotnetcli
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* <span data-ttu-id="c4b7c-478">Dodaj *modele/Contact. cs*:</span><span class="sxs-lookup"><span data-stu-id="c4b7c-478">Add *Models/Contact.cs*:</span></span>

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="c4b7c-479">Tworzenie szkieletu modelu `Contact`.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-479">Scaffold the `Contact` model.</span></span>
* <span data-ttu-id="c4b7c-480">Tworzenie początkowej migracji i aktualizowanie bazy danych:</span><span class="sxs-lookup"><span data-stu-id="c4b7c-480">Create initial migration and update the database:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
  dotnet ef database drop -f
  dotnet ef migrations add initial
  dotnet ef database update
  ```

* <span data-ttu-id="c4b7c-481">Zaktualizuj kotwicę **ContactManager** w pliku *pages/_Layout. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="c4b7c-481">Update the **ContactManager** anchor in the *Pages/_Layout.cshtml* file:</span></span>

  ```cshtml
  <a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
  ```

* <span data-ttu-id="c4b7c-482">Przetestuj aplikację, tworzenia, edytowania i usuwania kontaktu</span><span class="sxs-lookup"><span data-stu-id="c4b7c-482">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="c4b7c-483">Inicjowanie bazy danych</span><span class="sxs-lookup"><span data-stu-id="c4b7c-483">Seed the database</span></span>

<span data-ttu-id="c4b7c-484">Dodaj klasę [SeedData](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs) do folderu *danych* .</span><span class="sxs-lookup"><span data-stu-id="c4b7c-484">Add the [SeedData](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs) class to the *Data* folder.</span></span>

<span data-ttu-id="c4b7c-485">Wywołaj `SeedData.Initialize` z `Main`:</span><span class="sxs-lookup"><span data-stu-id="c4b7c-485">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Program.cs?name=snippet)]

<span data-ttu-id="c4b7c-486">Sprawdź, czy aplikacja zasilany bazy danych.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-486">Test that the app seeded the database.</span></span> <span data-ttu-id="c4b7c-487">W przypadku wszystkich wierszy w skontaktuj się z bazy danych, metoda inicjatora nie zostanie uruchomiony.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-487">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

::: moniker-end

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a><span data-ttu-id="c4b7c-488">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="c4b7c-488">Additional resources</span></span>

* [<span data-ttu-id="c4b7c-489">Tworzenie aplikacji internetowej platformy .NET Core i SQL Database w programie Azure App Service</span><span class="sxs-lookup"><span data-stu-id="c4b7c-489">Build a .NET Core and SQL Database web app in Azure App Service</span></span>](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb)
* <span data-ttu-id="c4b7c-490">[ASP.NET Core laboratorium autoryzacji](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span><span class="sxs-lookup"><span data-stu-id="c4b7c-490">[ASP.NET Core Authorization Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span></span> <span data-ttu-id="c4b7c-491">W tym laboratorium zawiera bardziej szczegółowe na temat funkcji zabezpieczeń wprowadzone w ramach tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="c4b7c-491">This lab goes into more detail on the security features introduced in this tutorial.</span></span>
* <xref:security/authorization/introduction>
* [<span data-ttu-id="c4b7c-492">Niestandardowa Autoryzacja oparta na zasadach</span><span class="sxs-lookup"><span data-stu-id="c4b7c-492">Custom policy-based authorization</span></span>](xref:security/authorization/policies)
