---
title: Create an ASP.NET Core app with user data protected by authorization
author: rick-anderson
description: Learn how to create a Razor Pages app with user data protected by authorization. Includes HTTPS, authentication, security, ASP.NET Core Identity.
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
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a><span data-ttu-id="6e408-104">Create an ASP.NET Core app with user data protected by authorization</span><span class="sxs-lookup"><span data-stu-id="6e408-104">Create an ASP.NET Core app with user data protected by authorization</span></span>

<span data-ttu-id="6e408-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="6e408-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="6e408-106">See [this PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC version.</span><span class="sxs-lookup"><span data-stu-id="6e408-106">See [this PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC version.</span></span> <span data-ttu-id="6e408-107">The ASP.NET Core 1.1 version of this tutorial is in [this](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data) folder.</span><span class="sxs-lookup"><span data-stu-id="6e408-107">The ASP.NET Core 1.1 version of this tutorial is in [this](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data) folder.</span></span> <span data-ttu-id="6e408-108">The 1.1 ASP.NET Core sample is in the [samples](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span><span class="sxs-lookup"><span data-stu-id="6e408-108">The 1.1 ASP.NET Core sample is in the [samples](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="6e408-109">See [this pdf](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_July16_18.pdf)</span><span class="sxs-lookup"><span data-stu-id="6e408-109">See [this pdf](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_July16_18.pdf)</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="6e408-110">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span><span class="sxs-lookup"><span data-stu-id="6e408-110">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="6e408-111">It displays a list of contacts that authenticated (registered) users have created.</span><span class="sxs-lookup"><span data-stu-id="6e408-111">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="6e408-112">There are three security groups:</span><span class="sxs-lookup"><span data-stu-id="6e408-112">There are three security groups:</span></span>

* <span data-ttu-id="6e408-113">**Registered users** can view all the approved data and can edit/delete their own data.</span><span class="sxs-lookup"><span data-stu-id="6e408-113">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="6e408-114">**Managers** can approve or reject contact data.</span><span class="sxs-lookup"><span data-stu-id="6e408-114">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="6e408-115">Only approved contacts are visible to users.</span><span class="sxs-lookup"><span data-stu-id="6e408-115">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="6e408-116">**Administrators** can approve/reject and edit/delete any data.</span><span class="sxs-lookup"><span data-stu-id="6e408-116">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="6e408-117">The images in this document don't exactly match the latest templates.</span><span class="sxs-lookup"><span data-stu-id="6e408-117">The images in this document don't exactly match the latest templates.</span></span>

<span data-ttu-id="6e408-118">In the following image, user Rick (`rick@example.com`) is signed in.</span><span class="sxs-lookup"><span data-stu-id="6e408-118">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="6e408-119">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span><span class="sxs-lookup"><span data-stu-id="6e408-119">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="6e408-120">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span><span class="sxs-lookup"><span data-stu-id="6e408-120">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="6e408-121">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span><span class="sxs-lookup"><span data-stu-id="6e408-121">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![Screenshot showing Rick signed in](secure-data/_static/rick.png)

<span data-ttu-id="6e408-123">In the following image, `manager@contoso.com` is signed in and in the manager's role:</span><span class="sxs-lookup"><span data-stu-id="6e408-123">In the following image, `manager@contoso.com` is signed in and in the manager's role:</span></span>

![Screenshot showing manager@contoso.com signed in](secure-data/_static/manager1.png)

<span data-ttu-id="6e408-125">The following image shows the managers details view of a contact:</span><span class="sxs-lookup"><span data-stu-id="6e408-125">The following image shows the managers details view of a contact:</span></span>

![Manager's view of a contact](secure-data/_static/manager.png)

<span data-ttu-id="6e408-127">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span><span class="sxs-lookup"><span data-stu-id="6e408-127">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="6e408-128">In the following image, `admin@contoso.com` is signed in and in the administrator's role:</span><span class="sxs-lookup"><span data-stu-id="6e408-128">In the following image, `admin@contoso.com` is signed in and in the administrator's role:</span></span>

![Screenshot showing admin@contoso.com signed in](secure-data/_static/admin.png)

<span data-ttu-id="6e408-130">The administrator has all privileges.</span><span class="sxs-lookup"><span data-stu-id="6e408-130">The administrator has all privileges.</span></span> <span data-ttu-id="6e408-131">She can read/edit/delete any contact and change the status of contacts.</span><span class="sxs-lookup"><span data-stu-id="6e408-131">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="6e408-132">The app was created by [scaffolding](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) the following `Contact` model:</span><span class="sxs-lookup"><span data-stu-id="6e408-132">The app was created by [scaffolding](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) the following `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

<span data-ttu-id="6e408-133">The sample contains the following authorization handlers:</span><span class="sxs-lookup"><span data-stu-id="6e408-133">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="6e408-134">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span><span class="sxs-lookup"><span data-stu-id="6e408-134">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="6e408-135">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span><span class="sxs-lookup"><span data-stu-id="6e408-135">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="6e408-136">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span><span class="sxs-lookup"><span data-stu-id="6e408-136">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6e408-137">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="6e408-137">Prerequisites</span></span>

<span data-ttu-id="6e408-138">This tutorial is advanced.</span><span class="sxs-lookup"><span data-stu-id="6e408-138">This tutorial is advanced.</span></span> <span data-ttu-id="6e408-139">You should be familiar with:</span><span class="sxs-lookup"><span data-stu-id="6e408-139">You should be familiar with:</span></span>

* [<span data-ttu-id="6e408-140">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6e408-140">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="6e408-141">Uwierzytelnianie</span><span class="sxs-lookup"><span data-stu-id="6e408-141">Authentication</span></span>](xref:security/authentication/identity)
* [<span data-ttu-id="6e408-142">Potwierdzenie konta i odzyskiwanie hasła</span><span class="sxs-lookup"><span data-stu-id="6e408-142">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="6e408-143">Autoryzacja</span><span class="sxs-lookup"><span data-stu-id="6e408-143">Authorization</span></span>](xref:security/authorization/introduction)
* [<span data-ttu-id="6e408-144">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="6e408-144">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="6e408-145">The starter and completed app</span><span class="sxs-lookup"><span data-stu-id="6e408-145">The starter and completed app</span></span>

<span data-ttu-id="6e408-146">[Download](xref:index#how-to-download-a-sample) the [completed](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) app.</span><span class="sxs-lookup"><span data-stu-id="6e408-146">[Download](xref:index#how-to-download-a-sample) the [completed](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) app.</span></span> <span data-ttu-id="6e408-147">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span><span class="sxs-lookup"><span data-stu-id="6e408-147">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="6e408-148">The starter app</span><span class="sxs-lookup"><span data-stu-id="6e408-148">The starter app</span></span>

<span data-ttu-id="6e408-149">[Download](xref:index#how-to-download-a-sample) the [starter](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) app.</span><span class="sxs-lookup"><span data-stu-id="6e408-149">[Download](xref:index#how-to-download-a-sample) the [starter](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) app.</span></span>

<span data-ttu-id="6e408-150">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span><span class="sxs-lookup"><span data-stu-id="6e408-150">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="6e408-151">Secure user data</span><span class="sxs-lookup"><span data-stu-id="6e408-151">Secure user data</span></span>

<span data-ttu-id="6e408-152">The following sections have all the major steps to create the secure user data app.</span><span class="sxs-lookup"><span data-stu-id="6e408-152">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="6e408-153">You may find it helpful to refer to the completed project.</span><span class="sxs-lookup"><span data-stu-id="6e408-153">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="6e408-154">Tie the contact data to the user</span><span class="sxs-lookup"><span data-stu-id="6e408-154">Tie the contact data to the user</span></span>

<span data-ttu-id="6e408-155">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span><span class="sxs-lookup"><span data-stu-id="6e408-155">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="6e408-156">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span><span class="sxs-lookup"><span data-stu-id="6e408-156">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/final3/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

<span data-ttu-id="6e408-157">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span><span class="sxs-lookup"><span data-stu-id="6e408-157">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="6e408-158">The `Status` field determines if a contact is viewable by general users.</span><span class="sxs-lookup"><span data-stu-id="6e408-158">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="6e408-159">Create a new migration and update the database:</span><span class="sxs-lookup"><span data-stu-id="6e408-159">Create a new migration and update the database:</span></span>

```dotnetcli
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-identity"></a><span data-ttu-id="6e408-160">Add Role services to Identity</span><span class="sxs-lookup"><span data-stu-id="6e408-160">Add Role services to Identity</span></span>

<span data-ttu-id="6e408-161">Append [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) to add Role services:</span><span class="sxs-lookup"><span data-stu-id="6e408-161">Append [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) to add Role services:</span></span>

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet2&highlight=9)]

### <a name="require-authenticated-users"></a><span data-ttu-id="6e408-162">Require authenticated users</span><span class="sxs-lookup"><span data-stu-id="6e408-162">Require authenticated users</span></span>

<span data-ttu-id="6e408-163">Set the default authentication policy to require users to be authenticated:</span><span class="sxs-lookup"><span data-stu-id="6e408-163">Set the default authentication policy to require users to be authenticated:</span></span>

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet&highlight=15-99)] 

 <span data-ttu-id="6e408-164">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span><span class="sxs-lookup"><span data-stu-id="6e408-164">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="6e408-165">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span><span class="sxs-lookup"><span data-stu-id="6e408-165">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="6e408-166">Having authentication required by default is more secure than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span><span class="sxs-lookup"><span data-stu-id="6e408-166">Having authentication required by default is more secure than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span>

<span data-ttu-id="6e408-167">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index and Privacy pages so anonymous users can get information about the site before they register.</span><span class="sxs-lookup"><span data-stu-id="6e408-167">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index and Privacy pages so anonymous users can get information about the site before they register.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Index.cshtml.cs?highlight=1,7)]

### <a name="configure-the-test-account"></a><span data-ttu-id="6e408-168">Configure the test account</span><span class="sxs-lookup"><span data-stu-id="6e408-168">Configure the test account</span></span>

<span data-ttu-id="6e408-169">The `SeedData` class creates two accounts: administrator and manager.</span><span class="sxs-lookup"><span data-stu-id="6e408-169">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="6e408-170">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span><span class="sxs-lookup"><span data-stu-id="6e408-170">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="6e408-171">Set the password from the project directory (the directory containing *Program.cs*):</span><span class="sxs-lookup"><span data-stu-id="6e408-171">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```dotnetcli
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="6e408-172">If a strong password is not specified, an exception is thrown when `SeedData.Initialize` is called.</span><span class="sxs-lookup"><span data-stu-id="6e408-172">If a strong password is not specified, an exception is thrown when `SeedData.Initialize` is called.</span></span>

<span data-ttu-id="6e408-173">Update `Main` to use the test password:</span><span class="sxs-lookup"><span data-stu-id="6e408-173">Update `Main` to use the test password:</span></span>

[!code-csharp[](secure-data/samples/final3/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="6e408-174">Create the test accounts and update the contacts</span><span class="sxs-lookup"><span data-stu-id="6e408-174">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="6e408-175">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span><span class="sxs-lookup"><span data-stu-id="6e408-175">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[](secure-data/samples/final3/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="6e408-176">Add the administrator user ID and `ContactStatus` to the contacts.</span><span class="sxs-lookup"><span data-stu-id="6e408-176">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="6e408-177">Make one of the contacts "Submitted" and one "Rejected".</span><span class="sxs-lookup"><span data-stu-id="6e408-177">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="6e408-178">Add the user ID and status to all the contacts.</span><span class="sxs-lookup"><span data-stu-id="6e408-178">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="6e408-179">Only one contact is shown:</span><span class="sxs-lookup"><span data-stu-id="6e408-179">Only one contact is shown:</span></span>

[!code-csharp[](secure-data/samples/final3/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="6e408-180">Create owner, manager, and administrator authorization handlers</span><span class="sxs-lookup"><span data-stu-id="6e408-180">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="6e408-181">Create a `ContactIsOwnerAuthorizationHandler` class in the *Authorization* folder.</span><span class="sxs-lookup"><span data-stu-id="6e408-181">Create a `ContactIsOwnerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="6e408-182">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span><span class="sxs-lookup"><span data-stu-id="6e408-182">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="6e408-183">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span><span class="sxs-lookup"><span data-stu-id="6e408-183">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="6e408-184">Authorization handlers generally:</span><span class="sxs-lookup"><span data-stu-id="6e408-184">Authorization handlers generally:</span></span>

* <span data-ttu-id="6e408-185">Return `context.Succeed` when the requirements are met.</span><span class="sxs-lookup"><span data-stu-id="6e408-185">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="6e408-186">Return `Task.CompletedTask` when requirements aren't met.</span><span class="sxs-lookup"><span data-stu-id="6e408-186">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="6e408-187">`Task.CompletedTask` is not success or failure&mdash;it allows other authorization handlers to run.</span><span class="sxs-lookup"><span data-stu-id="6e408-187">`Task.CompletedTask` is not success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="6e408-188">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span><span class="sxs-lookup"><span data-stu-id="6e408-188">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="6e408-189">The app allows contact owners to edit/delete/create their own data.</span><span class="sxs-lookup"><span data-stu-id="6e408-189">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="6e408-190">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span><span class="sxs-lookup"><span data-stu-id="6e408-190">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="6e408-191">Create a manager authorization handler</span><span class="sxs-lookup"><span data-stu-id="6e408-191">Create a manager authorization handler</span></span>

<span data-ttu-id="6e408-192">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span><span class="sxs-lookup"><span data-stu-id="6e408-192">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="6e408-193">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span><span class="sxs-lookup"><span data-stu-id="6e408-193">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="6e408-194">Only managers can approve or reject content changes (new or changed).</span><span class="sxs-lookup"><span data-stu-id="6e408-194">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="6e408-195">Create an administrator authorization handler</span><span class="sxs-lookup"><span data-stu-id="6e408-195">Create an administrator authorization handler</span></span>

<span data-ttu-id="6e408-196">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span><span class="sxs-lookup"><span data-stu-id="6e408-196">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="6e408-197">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span><span class="sxs-lookup"><span data-stu-id="6e408-197">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="6e408-198">Administrator can do all operations.</span><span class="sxs-lookup"><span data-stu-id="6e408-198">Administrator can do all operations.</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="6e408-199">Register the authorization handlers</span><span class="sxs-lookup"><span data-stu-id="6e408-199">Register the authorization handlers</span></span>

<span data-ttu-id="6e408-200">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span><span class="sxs-lookup"><span data-stu-id="6e408-200">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="6e408-201">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="6e408-201">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="6e408-202">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="6e408-202">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="6e408-203">Add the following code to the end of `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="6e408-203">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet_defaultPolicy&highlight=23-99)]

<span data-ttu-id="6e408-204">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span><span class="sxs-lookup"><span data-stu-id="6e408-204">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="6e408-205">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span><span class="sxs-lookup"><span data-stu-id="6e408-205">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="6e408-206">Support authorization</span><span class="sxs-lookup"><span data-stu-id="6e408-206">Support authorization</span></span>

<span data-ttu-id="6e408-207">In this section, you update the Razor Pages and add an operations requirements class.</span><span class="sxs-lookup"><span data-stu-id="6e408-207">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="6e408-208">Review the contact operations requirements class</span><span class="sxs-lookup"><span data-stu-id="6e408-208">Review the contact operations requirements class</span></span>

<span data-ttu-id="6e408-209">Review the `ContactOperations` class.</span><span class="sxs-lookup"><span data-stu-id="6e408-209">Review the `ContactOperations` class.</span></span> <span data-ttu-id="6e408-210">This class contains the requirements the app supports:</span><span class="sxs-lookup"><span data-stu-id="6e408-210">This class contains the requirements the app supports:</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-razor-pages"></a><span data-ttu-id="6e408-211">Create a base class for the Contacts Razor Pages</span><span class="sxs-lookup"><span data-stu-id="6e408-211">Create a base class for the Contacts Razor Pages</span></span>

<span data-ttu-id="6e408-212">Create a base class that contains the services used in the contacts Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="6e408-212">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="6e408-213">The base class puts the initialization code in one location:</span><span class="sxs-lookup"><span data-stu-id="6e408-213">The base class puts the initialization code in one location:</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="6e408-214">The preceding code:</span><span class="sxs-lookup"><span data-stu-id="6e408-214">The preceding code:</span></span>

* <span data-ttu-id="6e408-215">Adds the `IAuthorizationService` service to access to the authorization handlers.</span><span class="sxs-lookup"><span data-stu-id="6e408-215">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="6e408-216">Adds the Identity `UserManager` service.</span><span class="sxs-lookup"><span data-stu-id="6e408-216">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="6e408-217">Add the `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="6e408-217">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="6e408-218">Update the CreateModel</span><span class="sxs-lookup"><span data-stu-id="6e408-218">Update the CreateModel</span></span>

<span data-ttu-id="6e408-219">Update the create page model constructor to use the `DI_BasePageModel` base class:</span><span class="sxs-lookup"><span data-stu-id="6e408-219">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="6e408-220">Update the `CreateModel.OnPostAsync` method to:</span><span class="sxs-lookup"><span data-stu-id="6e408-220">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="6e408-221">Add the user ID to the `Contact` model.</span><span class="sxs-lookup"><span data-stu-id="6e408-221">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="6e408-222">Call the authorization handler to verify the user has permission to create contacts.</span><span class="sxs-lookup"><span data-stu-id="6e408-222">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="6e408-223">Update the IndexModel</span><span class="sxs-lookup"><span data-stu-id="6e408-223">Update the IndexModel</span></span>

<span data-ttu-id="6e408-224">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span><span class="sxs-lookup"><span data-stu-id="6e408-224">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="6e408-225">Update the EditModel</span><span class="sxs-lookup"><span data-stu-id="6e408-225">Update the EditModel</span></span>

<span data-ttu-id="6e408-226">Add an authorization handler to verify the user owns the contact.</span><span class="sxs-lookup"><span data-stu-id="6e408-226">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="6e408-227">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span><span class="sxs-lookup"><span data-stu-id="6e408-227">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="6e408-228">The app doesn't have access to the resource when attributes are evaluated.</span><span class="sxs-lookup"><span data-stu-id="6e408-228">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="6e408-229">Resource-based authorization must be imperative.</span><span class="sxs-lookup"><span data-stu-id="6e408-229">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="6e408-230">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span><span class="sxs-lookup"><span data-stu-id="6e408-230">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="6e408-231">You frequently access the resource by passing in the resource key.</span><span class="sxs-lookup"><span data-stu-id="6e408-231">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="6e408-232">Update the DeleteModel</span><span class="sxs-lookup"><span data-stu-id="6e408-232">Update the DeleteModel</span></span>

<span data-ttu-id="6e408-233">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span><span class="sxs-lookup"><span data-stu-id="6e408-233">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="6e408-234">Inject the authorization service into the views</span><span class="sxs-lookup"><span data-stu-id="6e408-234">Inject the authorization service into the views</span></span>

<span data-ttu-id="6e408-235">Currently, the UI shows edit and delete links for contacts the user can't modify.</span><span class="sxs-lookup"><span data-stu-id="6e408-235">Currently, the UI shows edit and delete links for contacts the user can't modify.</span></span>

<span data-ttu-id="6e408-236">Inject the authorization service in the *Pages/_ViewImports.cshtml* file so it's available to all views:</span><span class="sxs-lookup"><span data-stu-id="6e408-236">Inject the authorization service in the *Pages/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[](secure-data/samples/final3/Pages/_ViewImports.cshtml?highlight=6-99)]

<span data-ttu-id="6e408-237">The preceding markup adds several `using` statements.</span><span class="sxs-lookup"><span data-stu-id="6e408-237">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="6e408-238">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span><span class="sxs-lookup"><span data-stu-id="6e408-238">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[](secure-data/samples/final3/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> <span data-ttu-id="6e408-239">Hiding links from users that don't have permission to change data doesn't secure the app.</span><span class="sxs-lookup"><span data-stu-id="6e408-239">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="6e408-240">Hiding links makes the app more user-friendly by displaying only valid links.</span><span class="sxs-lookup"><span data-stu-id="6e408-240">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="6e408-241">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span><span class="sxs-lookup"><span data-stu-id="6e408-241">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="6e408-242">The Razor Page or controller must enforce access checks to secure the data.</span><span class="sxs-lookup"><span data-stu-id="6e408-242">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="6e408-243">Update Details</span><span class="sxs-lookup"><span data-stu-id="6e408-243">Update Details</span></span>

<span data-ttu-id="6e408-244">Update the details view so managers can approve or reject contacts:</span><span class="sxs-lookup"><span data-stu-id="6e408-244">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[](secure-data/samples/final3/Pages/Contacts/Details.cshtml?name=snippet)]

<span data-ttu-id="6e408-245">Update the details page model:</span><span class="sxs-lookup"><span data-stu-id="6e408-245">Update the details page model:</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="add-or-remove-a-user-to-a-role"></a><span data-ttu-id="6e408-246">Add or remove a user to a role</span><span class="sxs-lookup"><span data-stu-id="6e408-246">Add or remove a user to a role</span></span>

<span data-ttu-id="6e408-247">See [this issue](https://github.com/aspnet/AspNetCore.Docs/issues/8502) for information on:</span><span class="sxs-lookup"><span data-stu-id="6e408-247">See [this issue](https://github.com/aspnet/AspNetCore.Docs/issues/8502) for information on:</span></span>

* <span data-ttu-id="6e408-248">Removing privileges from a user.</span><span class="sxs-lookup"><span data-stu-id="6e408-248">Removing privileges from a user.</span></span> <span data-ttu-id="6e408-249">For example, muting a user in a chat app.</span><span class="sxs-lookup"><span data-stu-id="6e408-249">For example, muting a user in a chat app.</span></span>
* <span data-ttu-id="6e408-250">Adding privileges to a user.</span><span class="sxs-lookup"><span data-stu-id="6e408-250">Adding privileges to a user.</span></span>

<a name="challenge"></a>

## <a name="differences-between-challenge-and-forbid"></a><span data-ttu-id="6e408-251">Differences between Challenge and Forbid</span><span class="sxs-lookup"><span data-stu-id="6e408-251">Differences between Challenge and Forbid</span></span>

<span data-ttu-id="6e408-252">This app sets the default policy to [require authenticated users](#require-authenticated-users).</span><span class="sxs-lookup"><span data-stu-id="6e408-252">This app sets the default policy to [require authenticated users](#require-authenticated-users).</span></span> <span data-ttu-id="6e408-253">The following code allows anonymous users.</span><span class="sxs-lookup"><span data-stu-id="6e408-253">The following code allows anonymous users.</span></span> <span data-ttu-id="6e408-254">Anonymous users are allowed to show the differences between Challenge vs Forbid.</span><span class="sxs-lookup"><span data-stu-id="6e408-254">Anonymous users are allowed to show the differences between Challenge vs Forbid.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Details2.cshtml.cs?name=snippet)]

<span data-ttu-id="6e408-255">In the preceding code:</span><span class="sxs-lookup"><span data-stu-id="6e408-255">In the preceding code:</span></span>

* <span data-ttu-id="6e408-256">When the user is **not** authenticated, a `ChallengeResult` is returned.</span><span class="sxs-lookup"><span data-stu-id="6e408-256">When the user is **not** authenticated, a `ChallengeResult` is returned.</span></span> <span data-ttu-id="6e408-257">When a `ChallengeResult` is returned, the user is redirected to the sign-in page.</span><span class="sxs-lookup"><span data-stu-id="6e408-257">When a `ChallengeResult` is returned, the user is redirected to the sign-in page.</span></span>
* <span data-ttu-id="6e408-258">When the user is authenticated, but not authorized, a `ForbidResult` is returned.</span><span class="sxs-lookup"><span data-stu-id="6e408-258">When the user is authenticated, but not authorized, a `ForbidResult` is returned.</span></span> <span data-ttu-id="6e408-259">When a `ForbidResult` is returned, the user is redirected to the access denied page.</span><span class="sxs-lookup"><span data-stu-id="6e408-259">When a `ForbidResult` is returned, the user is redirected to the access denied page.</span></span>

## <a name="test-the-completed-app"></a><span data-ttu-id="6e408-260">Test the completed app</span><span class="sxs-lookup"><span data-stu-id="6e408-260">Test the completed app</span></span>

<span data-ttu-id="6e408-261">If you haven't already set a password for seeded user accounts, use the [Secret Manager tool](xref:security/app-secrets#secret-manager) to set a password:</span><span class="sxs-lookup"><span data-stu-id="6e408-261">If you haven't already set a password for seeded user accounts, use the [Secret Manager tool](xref:security/app-secrets#secret-manager) to set a password:</span></span>

* <span data-ttu-id="6e408-262">Choose a strong password: Use eight or more characters and at least one upper-case character, number, and symbol.</span><span class="sxs-lookup"><span data-stu-id="6e408-262">Choose a strong password: Use eight or more characters and at least one upper-case character, number, and symbol.</span></span> <span data-ttu-id="6e408-263">For example, `Passw0rd!` meets the strong password requirements.</span><span class="sxs-lookup"><span data-stu-id="6e408-263">For example, `Passw0rd!` meets the strong password requirements.</span></span>
* <span data-ttu-id="6e408-264">Execute the following command from the project's folder, where `<PW>` is the password:</span><span class="sxs-lookup"><span data-stu-id="6e408-264">Execute the following command from the project's folder, where `<PW>` is the password:</span></span>

  ```dotnetcli
  dotnet user-secrets set SeedUserPW <PW>
  ```

<span data-ttu-id="6e408-265">If the app has contacts:</span><span class="sxs-lookup"><span data-stu-id="6e408-265">If the app has contacts:</span></span>

* <span data-ttu-id="6e408-266">Delete all of the records in the `Contact` table.</span><span class="sxs-lookup"><span data-stu-id="6e408-266">Delete all of the records in the `Contact` table.</span></span>
* <span data-ttu-id="6e408-267">Restart the app to seed the database.</span><span class="sxs-lookup"><span data-stu-id="6e408-267">Restart the app to seed the database.</span></span>

<span data-ttu-id="6e408-268">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate sessions).</span><span class="sxs-lookup"><span data-stu-id="6e408-268">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate sessions).</span></span> <span data-ttu-id="6e408-269">In one browser, register a new user (for example, `test@example.com`).</span><span class="sxs-lookup"><span data-stu-id="6e408-269">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="6e408-270">Sign in to each browser with a different user.</span><span class="sxs-lookup"><span data-stu-id="6e408-270">Sign in to each browser with a different user.</span></span> <span data-ttu-id="6e408-271">Verify the following operations:</span><span class="sxs-lookup"><span data-stu-id="6e408-271">Verify the following operations:</span></span>

* <span data-ttu-id="6e408-272">Registered users can view all of the approved contact data.</span><span class="sxs-lookup"><span data-stu-id="6e408-272">Registered users can view all of the approved contact data.</span></span>
* <span data-ttu-id="6e408-273">Registered users can edit/delete their own data.</span><span class="sxs-lookup"><span data-stu-id="6e408-273">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="6e408-274">Managers can approve/reject contact data.</span><span class="sxs-lookup"><span data-stu-id="6e408-274">Managers can approve/reject contact data.</span></span> <span data-ttu-id="6e408-275">The `Details` view shows **Approve** and **Reject** buttons.</span><span class="sxs-lookup"><span data-stu-id="6e408-275">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="6e408-276">Administrators can approve/reject and edit/delete all data.</span><span class="sxs-lookup"><span data-stu-id="6e408-276">Administrators can approve/reject and edit/delete all data.</span></span>

| <span data-ttu-id="6e408-277">Użytkownik</span><span class="sxs-lookup"><span data-stu-id="6e408-277">User</span></span>                | <span data-ttu-id="6e408-278">Seeded by the app</span><span class="sxs-lookup"><span data-stu-id="6e408-278">Seeded by the app</span></span> | <span data-ttu-id="6e408-279">Opcje</span><span class="sxs-lookup"><span data-stu-id="6e408-279">Options</span></span>                                  |
| ------------------- | :---------------: | ---------------------------------------- |
| test@example.com    | <span data-ttu-id="6e408-280">Nie</span><span class="sxs-lookup"><span data-stu-id="6e408-280">No</span></span>                | <span data-ttu-id="6e408-281">Edit/delete the own data.</span><span class="sxs-lookup"><span data-stu-id="6e408-281">Edit/delete the own data.</span></span>                |
| manager@contoso.com | <span data-ttu-id="6e408-282">Tak</span><span class="sxs-lookup"><span data-stu-id="6e408-282">Yes</span></span>               | <span data-ttu-id="6e408-283">Approve/reject and edit/delete own data.</span><span class="sxs-lookup"><span data-stu-id="6e408-283">Approve/reject and edit/delete own data.</span></span> |
| admin@contoso.com   | <span data-ttu-id="6e408-284">Tak</span><span class="sxs-lookup"><span data-stu-id="6e408-284">Yes</span></span>               | <span data-ttu-id="6e408-285">Approve/reject and edit/delete all data.</span><span class="sxs-lookup"><span data-stu-id="6e408-285">Approve/reject and edit/delete all data.</span></span> |

<span data-ttu-id="6e408-286">Create a contact in the administrator's browser.</span><span class="sxs-lookup"><span data-stu-id="6e408-286">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="6e408-287">Copy the URL for delete and edit from the administrator contact.</span><span class="sxs-lookup"><span data-stu-id="6e408-287">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="6e408-288">Paste these links into the test user's browser to verify the test user can't perform these operations.</span><span class="sxs-lookup"><span data-stu-id="6e408-288">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="6e408-289">Create the starter app</span><span class="sxs-lookup"><span data-stu-id="6e408-289">Create the starter app</span></span>

* <span data-ttu-id="6e408-290">Create a Razor Pages app named "ContactManager"</span><span class="sxs-lookup"><span data-stu-id="6e408-290">Create a Razor Pages app named "ContactManager"</span></span>
  * <span data-ttu-id="6e408-291">Create the app with **Individual User Accounts**.</span><span class="sxs-lookup"><span data-stu-id="6e408-291">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="6e408-292">Name it "ContactManager" so the namespace matches the namespace used in the sample.</span><span class="sxs-lookup"><span data-stu-id="6e408-292">Name it "ContactManager" so the namespace matches the namespace used in the sample.</span></span>
  * <span data-ttu-id="6e408-293">`-uld` specifies LocalDB instead of SQLite</span><span class="sxs-lookup"><span data-stu-id="6e408-293">`-uld` specifies LocalDB instead of SQLite</span></span>

  ```dotnetcli
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* <span data-ttu-id="6e408-294">Add *Models/Contact.cs*:</span><span class="sxs-lookup"><span data-stu-id="6e408-294">Add *Models/Contact.cs*:</span></span>

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="6e408-295">Scaffold the `Contact` model.</span><span class="sxs-lookup"><span data-stu-id="6e408-295">Scaffold the `Contact` model.</span></span>
* <span data-ttu-id="6e408-296">Create initial migration and update the database:</span><span class="sxs-lookup"><span data-stu-id="6e408-296">Create initial migration and update the database:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet tool install -g dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
dotnet ef database drop -f
dotnet ef migrations add initial
dotnet ef database update
```

<span data-ttu-id="6e408-297">If you experience a bug with the `dotnet aspnet-codegenerator razorpage` command, see [this GitHub issue](https://github.com/aspnet/Scaffolding/issues/984).</span><span class="sxs-lookup"><span data-stu-id="6e408-297">If you experience a bug with the `dotnet aspnet-codegenerator razorpage` command, see [this GitHub issue](https://github.com/aspnet/Scaffolding/issues/984).</span></span>

* <span data-ttu-id="6e408-298">Update the **ContactManager** anchor in the *Pages/Shared/_Layout.cshtml* file:</span><span class="sxs-lookup"><span data-stu-id="6e408-298">Update the **ContactManager** anchor in the *Pages/Shared/_Layout.cshtml* file:</span></span>

 ```cshtml
<a class="navbar-brand" asp-area="" asp-page="/Contacts/Index">ContactManager</a>
  ```

* <span data-ttu-id="6e408-299">Test the app by creating, editing, and deleting a contact</span><span class="sxs-lookup"><span data-stu-id="6e408-299">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="6e408-300">Seed the database</span><span class="sxs-lookup"><span data-stu-id="6e408-300">Seed the database</span></span>

<span data-ttu-id="6e408-301">Add the [SeedData](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter3/Data/SeedData.cs) class to the *Data* folder:</span><span class="sxs-lookup"><span data-stu-id="6e408-301">Add the [SeedData](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter3/Data/SeedData.cs) class to the *Data* folder:</span></span>

[!code-csharp[](secure-data/samples/starter3/Data/SeedData.cs)]

<span data-ttu-id="6e408-302">Call `SeedData.Initialize` from `Main`:</span><span class="sxs-lookup"><span data-stu-id="6e408-302">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[](secure-data/samples/starter3/Program.cs)]

<span data-ttu-id="6e408-303">Test that the app seeded the database.</span><span class="sxs-lookup"><span data-stu-id="6e408-303">Test that the app seeded the database.</span></span> <span data-ttu-id="6e408-304">If there are any rows in the contact DB, the seed method doesn't run.</span><span class="sxs-lookup"><span data-stu-id="6e408-304">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1 < aspnetcore-3.0"

<span data-ttu-id="6e408-305">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span><span class="sxs-lookup"><span data-stu-id="6e408-305">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="6e408-306">It displays a list of contacts that authenticated (registered) users have created.</span><span class="sxs-lookup"><span data-stu-id="6e408-306">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="6e408-307">There are three security groups:</span><span class="sxs-lookup"><span data-stu-id="6e408-307">There are three security groups:</span></span>

* <span data-ttu-id="6e408-308">**Registered users** can view all the approved data and can edit/delete their own data.</span><span class="sxs-lookup"><span data-stu-id="6e408-308">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="6e408-309">**Managers** can approve or reject contact data.</span><span class="sxs-lookup"><span data-stu-id="6e408-309">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="6e408-310">Only approved contacts are visible to users.</span><span class="sxs-lookup"><span data-stu-id="6e408-310">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="6e408-311">**Administrators** can approve/reject and edit/delete any data.</span><span class="sxs-lookup"><span data-stu-id="6e408-311">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="6e408-312">In the following image, user Rick (`rick@example.com`) is signed in.</span><span class="sxs-lookup"><span data-stu-id="6e408-312">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="6e408-313">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span><span class="sxs-lookup"><span data-stu-id="6e408-313">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="6e408-314">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span><span class="sxs-lookup"><span data-stu-id="6e408-314">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="6e408-315">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span><span class="sxs-lookup"><span data-stu-id="6e408-315">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![Screenshot showing Rick signed in](secure-data/_static/rick.png)

<span data-ttu-id="6e408-317">In the following image, `manager@contoso.com` is signed in and in the manager's role:</span><span class="sxs-lookup"><span data-stu-id="6e408-317">In the following image, `manager@contoso.com` is signed in and in the manager's role:</span></span>

![Screenshot showing manager@contoso.com signed in](secure-data/_static/manager1.png)

<span data-ttu-id="6e408-319">The following image shows the managers details view of a contact:</span><span class="sxs-lookup"><span data-stu-id="6e408-319">The following image shows the managers details view of a contact:</span></span>

![Manager's view of a contact](secure-data/_static/manager.png)

<span data-ttu-id="6e408-321">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span><span class="sxs-lookup"><span data-stu-id="6e408-321">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="6e408-322">In the following image, `admin@contoso.com` is signed in and in the administrator's role:</span><span class="sxs-lookup"><span data-stu-id="6e408-322">In the following image, `admin@contoso.com` is signed in and in the administrator's role:</span></span>

![Screenshot showing admin@contoso.com signed in](secure-data/_static/admin.png)

<span data-ttu-id="6e408-324">The administrator has all privileges.</span><span class="sxs-lookup"><span data-stu-id="6e408-324">The administrator has all privileges.</span></span> <span data-ttu-id="6e408-325">She can read/edit/delete any contact and change the status of contacts.</span><span class="sxs-lookup"><span data-stu-id="6e408-325">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="6e408-326">The app was created by [scaffolding](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) the following `Contact` model:</span><span class="sxs-lookup"><span data-stu-id="6e408-326">The app was created by [scaffolding](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) the following `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

<span data-ttu-id="6e408-327">The sample contains the following authorization handlers:</span><span class="sxs-lookup"><span data-stu-id="6e408-327">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="6e408-328">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span><span class="sxs-lookup"><span data-stu-id="6e408-328">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="6e408-329">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span><span class="sxs-lookup"><span data-stu-id="6e408-329">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="6e408-330">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span><span class="sxs-lookup"><span data-stu-id="6e408-330">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6e408-331">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="6e408-331">Prerequisites</span></span>

<span data-ttu-id="6e408-332">This tutorial is advanced.</span><span class="sxs-lookup"><span data-stu-id="6e408-332">This tutorial is advanced.</span></span> <span data-ttu-id="6e408-333">You should be familiar with:</span><span class="sxs-lookup"><span data-stu-id="6e408-333">You should be familiar with:</span></span>

* [<span data-ttu-id="6e408-334">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6e408-334">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="6e408-335">Uwierzytelnianie</span><span class="sxs-lookup"><span data-stu-id="6e408-335">Authentication</span></span>](xref:security/authentication/identity)
* [<span data-ttu-id="6e408-336">Potwierdzenie konta i odzyskiwanie hasła</span><span class="sxs-lookup"><span data-stu-id="6e408-336">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="6e408-337">Autoryzacja</span><span class="sxs-lookup"><span data-stu-id="6e408-337">Authorization</span></span>](xref:security/authorization/introduction)
* [<span data-ttu-id="6e408-338">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="6e408-338">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="6e408-339">The starter and completed app</span><span class="sxs-lookup"><span data-stu-id="6e408-339">The starter and completed app</span></span>

<span data-ttu-id="6e408-340">[Download](xref:index#how-to-download-a-sample) the [completed](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) app.</span><span class="sxs-lookup"><span data-stu-id="6e408-340">[Download](xref:index#how-to-download-a-sample) the [completed](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) app.</span></span> <span data-ttu-id="6e408-341">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span><span class="sxs-lookup"><span data-stu-id="6e408-341">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="6e408-342">The starter app</span><span class="sxs-lookup"><span data-stu-id="6e408-342">The starter app</span></span>

<span data-ttu-id="6e408-343">[Download](xref:index#how-to-download-a-sample) the [starter](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) app.</span><span class="sxs-lookup"><span data-stu-id="6e408-343">[Download](xref:index#how-to-download-a-sample) the [starter](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) app.</span></span>

<span data-ttu-id="6e408-344">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span><span class="sxs-lookup"><span data-stu-id="6e408-344">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="6e408-345">Secure user data</span><span class="sxs-lookup"><span data-stu-id="6e408-345">Secure user data</span></span>

<span data-ttu-id="6e408-346">The following sections have all the major steps to create the secure user data app.</span><span class="sxs-lookup"><span data-stu-id="6e408-346">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="6e408-347">You may find it helpful to refer to the completed project.</span><span class="sxs-lookup"><span data-stu-id="6e408-347">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="6e408-348">Tie the contact data to the user</span><span class="sxs-lookup"><span data-stu-id="6e408-348">Tie the contact data to the user</span></span>

<span data-ttu-id="6e408-349">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span><span class="sxs-lookup"><span data-stu-id="6e408-349">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="6e408-350">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span><span class="sxs-lookup"><span data-stu-id="6e408-350">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

<span data-ttu-id="6e408-351">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span><span class="sxs-lookup"><span data-stu-id="6e408-351">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="6e408-352">The `Status` field determines if a contact is viewable by general users.</span><span class="sxs-lookup"><span data-stu-id="6e408-352">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="6e408-353">Create a new migration and update the database:</span><span class="sxs-lookup"><span data-stu-id="6e408-353">Create a new migration and update the database:</span></span>

```dotnetcli
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-identity"></a><span data-ttu-id="6e408-354">Add Role services to Identity</span><span class="sxs-lookup"><span data-stu-id="6e408-354">Add Role services to Identity</span></span>

<span data-ttu-id="6e408-355">Append [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) to add Role services:</span><span class="sxs-lookup"><span data-stu-id="6e408-355">Append [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) to add Role services:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet2&highlight=12)]

### <a name="require-authenticated-users"></a><span data-ttu-id="6e408-356">Require authenticated users</span><span class="sxs-lookup"><span data-stu-id="6e408-356">Require authenticated users</span></span>

<span data-ttu-id="6e408-357">Set the default authentication policy to require users to be authenticated:</span><span class="sxs-lookup"><span data-stu-id="6e408-357">Set the default authentication policy to require users to be authenticated:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet&highlight=17-99)] 

 <span data-ttu-id="6e408-358">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span><span class="sxs-lookup"><span data-stu-id="6e408-358">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="6e408-359">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span><span class="sxs-lookup"><span data-stu-id="6e408-359">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="6e408-360">Having authentication required by default is more secure than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span><span class="sxs-lookup"><span data-stu-id="6e408-360">Having authentication required by default is more secure than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span>

<span data-ttu-id="6e408-361">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index, About, and Contact pages so anonymous users can get information about the site before they register.</span><span class="sxs-lookup"><span data-stu-id="6e408-361">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index, About, and Contact pages so anonymous users can get information about the site before they register.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Index.cshtml.cs?highlight=1,6)]

### <a name="configure-the-test-account"></a><span data-ttu-id="6e408-362">Configure the test account</span><span class="sxs-lookup"><span data-stu-id="6e408-362">Configure the test account</span></span>

<span data-ttu-id="6e408-363">The `SeedData` class creates two accounts: administrator and manager.</span><span class="sxs-lookup"><span data-stu-id="6e408-363">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="6e408-364">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span><span class="sxs-lookup"><span data-stu-id="6e408-364">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="6e408-365">Set the password from the project directory (the directory containing *Program.cs*):</span><span class="sxs-lookup"><span data-stu-id="6e408-365">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```dotnetcli
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="6e408-366">If a strong password is not specified, an exception is thrown when `SeedData.Initialize` is called.</span><span class="sxs-lookup"><span data-stu-id="6e408-366">If a strong password is not specified, an exception is thrown when `SeedData.Initialize` is called.</span></span>

<span data-ttu-id="6e408-367">Update `Main` to use the test password:</span><span class="sxs-lookup"><span data-stu-id="6e408-367">Update `Main` to use the test password:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="6e408-368">Create the test accounts and update the contacts</span><span class="sxs-lookup"><span data-stu-id="6e408-368">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="6e408-369">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span><span class="sxs-lookup"><span data-stu-id="6e408-369">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="6e408-370">Add the administrator user ID and `ContactStatus` to the contacts.</span><span class="sxs-lookup"><span data-stu-id="6e408-370">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="6e408-371">Make one of the contacts "Submitted" and one "Rejected".</span><span class="sxs-lookup"><span data-stu-id="6e408-371">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="6e408-372">Add the user ID and status to all the contacts.</span><span class="sxs-lookup"><span data-stu-id="6e408-372">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="6e408-373">Only one contact is shown:</span><span class="sxs-lookup"><span data-stu-id="6e408-373">Only one contact is shown:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="6e408-374">Create owner, manager, and administrator authorization handlers</span><span class="sxs-lookup"><span data-stu-id="6e408-374">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="6e408-375">Create an *Authorization* folder and create a `ContactIsOwnerAuthorizationHandler` class in it.</span><span class="sxs-lookup"><span data-stu-id="6e408-375">Create an *Authorization* folder and create a `ContactIsOwnerAuthorizationHandler` class in it.</span></span> <span data-ttu-id="6e408-376">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span><span class="sxs-lookup"><span data-stu-id="6e408-376">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="6e408-377">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span><span class="sxs-lookup"><span data-stu-id="6e408-377">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="6e408-378">Authorization handlers generally:</span><span class="sxs-lookup"><span data-stu-id="6e408-378">Authorization handlers generally:</span></span>

* <span data-ttu-id="6e408-379">Return `context.Succeed` when the requirements are met.</span><span class="sxs-lookup"><span data-stu-id="6e408-379">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="6e408-380">Return `Task.CompletedTask` when requirements aren't met.</span><span class="sxs-lookup"><span data-stu-id="6e408-380">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="6e408-381">`Task.CompletedTask` is not success or failure&mdash;it allows other authorization handlers to run.</span><span class="sxs-lookup"><span data-stu-id="6e408-381">`Task.CompletedTask` is not success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="6e408-382">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span><span class="sxs-lookup"><span data-stu-id="6e408-382">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="6e408-383">The app allows contact owners to edit/delete/create their own data.</span><span class="sxs-lookup"><span data-stu-id="6e408-383">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="6e408-384">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span><span class="sxs-lookup"><span data-stu-id="6e408-384">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="6e408-385">Create a manager authorization handler</span><span class="sxs-lookup"><span data-stu-id="6e408-385">Create a manager authorization handler</span></span>

<span data-ttu-id="6e408-386">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span><span class="sxs-lookup"><span data-stu-id="6e408-386">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="6e408-387">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span><span class="sxs-lookup"><span data-stu-id="6e408-387">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="6e408-388">Only managers can approve or reject content changes (new or changed).</span><span class="sxs-lookup"><span data-stu-id="6e408-388">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="6e408-389">Create an administrator authorization handler</span><span class="sxs-lookup"><span data-stu-id="6e408-389">Create an administrator authorization handler</span></span>

<span data-ttu-id="6e408-390">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span><span class="sxs-lookup"><span data-stu-id="6e408-390">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="6e408-391">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span><span class="sxs-lookup"><span data-stu-id="6e408-391">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="6e408-392">Administrator can do all operations.</span><span class="sxs-lookup"><span data-stu-id="6e408-392">Administrator can do all operations.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="6e408-393">Register the authorization handlers</span><span class="sxs-lookup"><span data-stu-id="6e408-393">Register the authorization handlers</span></span>

<span data-ttu-id="6e408-394">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span><span class="sxs-lookup"><span data-stu-id="6e408-394">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="6e408-395">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="6e408-395">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="6e408-396">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="6e408-396">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="6e408-397">Add the following code to the end of `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="6e408-397">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet_defaultPolicy&highlight=27-99)]

<span data-ttu-id="6e408-398">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span><span class="sxs-lookup"><span data-stu-id="6e408-398">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="6e408-399">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span><span class="sxs-lookup"><span data-stu-id="6e408-399">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="6e408-400">Support authorization</span><span class="sxs-lookup"><span data-stu-id="6e408-400">Support authorization</span></span>

<span data-ttu-id="6e408-401">In this section, you update the Razor Pages and add an operations requirements class.</span><span class="sxs-lookup"><span data-stu-id="6e408-401">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="6e408-402">Review the contact operations requirements class</span><span class="sxs-lookup"><span data-stu-id="6e408-402">Review the contact operations requirements class</span></span>

<span data-ttu-id="6e408-403">Review the `ContactOperations` class.</span><span class="sxs-lookup"><span data-stu-id="6e408-403">Review the `ContactOperations` class.</span></span> <span data-ttu-id="6e408-404">This class contains the requirements the app supports:</span><span class="sxs-lookup"><span data-stu-id="6e408-404">This class contains the requirements the app supports:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-razor-pages"></a><span data-ttu-id="6e408-405">Create a base class for the Contacts Razor Pages</span><span class="sxs-lookup"><span data-stu-id="6e408-405">Create a base class for the Contacts Razor Pages</span></span>

<span data-ttu-id="6e408-406">Create a base class that contains the services used in the contacts Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="6e408-406">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="6e408-407">The base class puts the initialization code in one location:</span><span class="sxs-lookup"><span data-stu-id="6e408-407">The base class puts the initialization code in one location:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="6e408-408">The preceding code:</span><span class="sxs-lookup"><span data-stu-id="6e408-408">The preceding code:</span></span>

* <span data-ttu-id="6e408-409">Adds the `IAuthorizationService` service to access to the authorization handlers.</span><span class="sxs-lookup"><span data-stu-id="6e408-409">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="6e408-410">Adds the Identity `UserManager` service.</span><span class="sxs-lookup"><span data-stu-id="6e408-410">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="6e408-411">Add the `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="6e408-411">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="6e408-412">Update the CreateModel</span><span class="sxs-lookup"><span data-stu-id="6e408-412">Update the CreateModel</span></span>

<span data-ttu-id="6e408-413">Update the create page model constructor to use the `DI_BasePageModel` base class:</span><span class="sxs-lookup"><span data-stu-id="6e408-413">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="6e408-414">Update the `CreateModel.OnPostAsync` method to:</span><span class="sxs-lookup"><span data-stu-id="6e408-414">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="6e408-415">Add the user ID to the `Contact` model.</span><span class="sxs-lookup"><span data-stu-id="6e408-415">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="6e408-416">Call the authorization handler to verify the user has permission to create contacts.</span><span class="sxs-lookup"><span data-stu-id="6e408-416">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="6e408-417">Update the IndexModel</span><span class="sxs-lookup"><span data-stu-id="6e408-417">Update the IndexModel</span></span>

<span data-ttu-id="6e408-418">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span><span class="sxs-lookup"><span data-stu-id="6e408-418">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="6e408-419">Update the EditModel</span><span class="sxs-lookup"><span data-stu-id="6e408-419">Update the EditModel</span></span>

<span data-ttu-id="6e408-420">Add an authorization handler to verify the user owns the contact.</span><span class="sxs-lookup"><span data-stu-id="6e408-420">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="6e408-421">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span><span class="sxs-lookup"><span data-stu-id="6e408-421">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="6e408-422">The app doesn't have access to the resource when attributes are evaluated.</span><span class="sxs-lookup"><span data-stu-id="6e408-422">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="6e408-423">Resource-based authorization must be imperative.</span><span class="sxs-lookup"><span data-stu-id="6e408-423">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="6e408-424">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span><span class="sxs-lookup"><span data-stu-id="6e408-424">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="6e408-425">You frequently access the resource by passing in the resource key.</span><span class="sxs-lookup"><span data-stu-id="6e408-425">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="6e408-426">Update the DeleteModel</span><span class="sxs-lookup"><span data-stu-id="6e408-426">Update the DeleteModel</span></span>

<span data-ttu-id="6e408-427">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span><span class="sxs-lookup"><span data-stu-id="6e408-427">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="6e408-428">Inject the authorization service into the views</span><span class="sxs-lookup"><span data-stu-id="6e408-428">Inject the authorization service into the views</span></span>

<span data-ttu-id="6e408-429">Currently, the UI shows edit and delete links for contacts the user can't modify.</span><span class="sxs-lookup"><span data-stu-id="6e408-429">Currently, the UI shows edit and delete links for contacts the user can't modify.</span></span>

<span data-ttu-id="6e408-430">Inject the authorization service in the *Views/_ViewImports.cshtml* file so it's available to all views:</span><span class="sxs-lookup"><span data-stu-id="6e408-430">Inject the authorization service in the *Views/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/_ViewImports.cshtml?highlight=6-99)]

<span data-ttu-id="6e408-431">The preceding markup adds several `using` statements.</span><span class="sxs-lookup"><span data-stu-id="6e408-431">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="6e408-432">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span><span class="sxs-lookup"><span data-stu-id="6e408-432">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> <span data-ttu-id="6e408-433">Hiding links from users that don't have permission to change data doesn't secure the app.</span><span class="sxs-lookup"><span data-stu-id="6e408-433">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="6e408-434">Hiding links makes the app more user-friendly by displaying only valid links.</span><span class="sxs-lookup"><span data-stu-id="6e408-434">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="6e408-435">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span><span class="sxs-lookup"><span data-stu-id="6e408-435">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="6e408-436">The Razor Page or controller must enforce access checks to secure the data.</span><span class="sxs-lookup"><span data-stu-id="6e408-436">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="6e408-437">Update Details</span><span class="sxs-lookup"><span data-stu-id="6e408-437">Update Details</span></span>

<span data-ttu-id="6e408-438">Update the details view so managers can approve or reject contacts:</span><span class="sxs-lookup"><span data-stu-id="6e408-438">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml?name=snippet)]

<span data-ttu-id="6e408-439">Update the details page model:</span><span class="sxs-lookup"><span data-stu-id="6e408-439">Update the details page model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="add-or-remove-a-user-to-a-role"></a><span data-ttu-id="6e408-440">Add or remove a user to a role</span><span class="sxs-lookup"><span data-stu-id="6e408-440">Add or remove a user to a role</span></span>

<span data-ttu-id="6e408-441">See [this issue](https://github.com/aspnet/AspNetCore.Docs/issues/8502) for information on:</span><span class="sxs-lookup"><span data-stu-id="6e408-441">See [this issue](https://github.com/aspnet/AspNetCore.Docs/issues/8502) for information on:</span></span>

* <span data-ttu-id="6e408-442">Removing privileges from a user.</span><span class="sxs-lookup"><span data-stu-id="6e408-442">Removing privileges from a user.</span></span> <span data-ttu-id="6e408-443">For example, muting a user in a chat app.</span><span class="sxs-lookup"><span data-stu-id="6e408-443">For example, muting a user in a chat app.</span></span>
* <span data-ttu-id="6e408-444">Adding privileges to a user.</span><span class="sxs-lookup"><span data-stu-id="6e408-444">Adding privileges to a user.</span></span>

## <a name="test-the-completed-app"></a><span data-ttu-id="6e408-445">Test the completed app</span><span class="sxs-lookup"><span data-stu-id="6e408-445">Test the completed app</span></span>

<span data-ttu-id="6e408-446">If you haven't already set a password for seeded user accounts, use the [Secret Manager tool](xref:security/app-secrets#secret-manager) to set a password:</span><span class="sxs-lookup"><span data-stu-id="6e408-446">If you haven't already set a password for seeded user accounts, use the [Secret Manager tool](xref:security/app-secrets#secret-manager) to set a password:</span></span>

* <span data-ttu-id="6e408-447">Choose a strong password: Use eight or more characters and at least one upper-case character, number, and symbol.</span><span class="sxs-lookup"><span data-stu-id="6e408-447">Choose a strong password: Use eight or more characters and at least one upper-case character, number, and symbol.</span></span> <span data-ttu-id="6e408-448">For example, `Passw0rd!` meets the strong password requirements.</span><span class="sxs-lookup"><span data-stu-id="6e408-448">For example, `Passw0rd!` meets the strong password requirements.</span></span>
* <span data-ttu-id="6e408-449">Execute the following command from the project's folder, where `<PW>` is the password:</span><span class="sxs-lookup"><span data-stu-id="6e408-449">Execute the following command from the project's folder, where `<PW>` is the password:</span></span>

  ```dotnetcli
  dotnet user-secrets set SeedUserPW <PW>
  ```

* <span data-ttu-id="6e408-450">Drop and update the Database</span><span class="sxs-lookup"><span data-stu-id="6e408-450">Drop and update the Database</span></span>

  ```dotnetcli
  dotnet ef database drop -f
  dotnet ef database update  
  ```

* <span data-ttu-id="6e408-451">Restart the app to seed the database.</span><span class="sxs-lookup"><span data-stu-id="6e408-451">Restart the app to seed the database.</span></span>

<span data-ttu-id="6e408-452">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate sessions).</span><span class="sxs-lookup"><span data-stu-id="6e408-452">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate sessions).</span></span> <span data-ttu-id="6e408-453">In one browser, register a new user (for example, `test@example.com`).</span><span class="sxs-lookup"><span data-stu-id="6e408-453">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="6e408-454">Sign in to each browser with a different user.</span><span class="sxs-lookup"><span data-stu-id="6e408-454">Sign in to each browser with a different user.</span></span> <span data-ttu-id="6e408-455">Verify the following operations:</span><span class="sxs-lookup"><span data-stu-id="6e408-455">Verify the following operations:</span></span>

* <span data-ttu-id="6e408-456">Registered users can view all of the approved contact data.</span><span class="sxs-lookup"><span data-stu-id="6e408-456">Registered users can view all of the approved contact data.</span></span>
* <span data-ttu-id="6e408-457">Registered users can edit/delete their own data.</span><span class="sxs-lookup"><span data-stu-id="6e408-457">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="6e408-458">Managers can approve/reject contact data.</span><span class="sxs-lookup"><span data-stu-id="6e408-458">Managers can approve/reject contact data.</span></span> <span data-ttu-id="6e408-459">The `Details` view shows **Approve** and **Reject** buttons.</span><span class="sxs-lookup"><span data-stu-id="6e408-459">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="6e408-460">Administrators can approve/reject and edit/delete all data.</span><span class="sxs-lookup"><span data-stu-id="6e408-460">Administrators can approve/reject and edit/delete all data.</span></span>

| <span data-ttu-id="6e408-461">Użytkownik</span><span class="sxs-lookup"><span data-stu-id="6e408-461">User</span></span>                | <span data-ttu-id="6e408-462">Seeded by the app</span><span class="sxs-lookup"><span data-stu-id="6e408-462">Seeded by the app</span></span> | <span data-ttu-id="6e408-463">Opcje</span><span class="sxs-lookup"><span data-stu-id="6e408-463">Options</span></span>                                  |
| ------------------- | :---------------: | ---------------------------------------- |
| test@example.com    | <span data-ttu-id="6e408-464">Nie</span><span class="sxs-lookup"><span data-stu-id="6e408-464">No</span></span>                | <span data-ttu-id="6e408-465">Edit/delete the own data.</span><span class="sxs-lookup"><span data-stu-id="6e408-465">Edit/delete the own data.</span></span>                |
| manager@contoso.com | <span data-ttu-id="6e408-466">Tak</span><span class="sxs-lookup"><span data-stu-id="6e408-466">Yes</span></span>               | <span data-ttu-id="6e408-467">Approve/reject and edit/delete own data.</span><span class="sxs-lookup"><span data-stu-id="6e408-467">Approve/reject and edit/delete own data.</span></span> |
| admin@contoso.com   | <span data-ttu-id="6e408-468">Tak</span><span class="sxs-lookup"><span data-stu-id="6e408-468">Yes</span></span>               | <span data-ttu-id="6e408-469">Approve/reject and edit/delete all data.</span><span class="sxs-lookup"><span data-stu-id="6e408-469">Approve/reject and edit/delete all data.</span></span> |

<span data-ttu-id="6e408-470">Create a contact in the administrator's browser.</span><span class="sxs-lookup"><span data-stu-id="6e408-470">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="6e408-471">Copy the URL for delete and edit from the administrator contact.</span><span class="sxs-lookup"><span data-stu-id="6e408-471">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="6e408-472">Paste these links into the test user's browser to verify the test user can't perform these operations.</span><span class="sxs-lookup"><span data-stu-id="6e408-472">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="6e408-473">Create the starter app</span><span class="sxs-lookup"><span data-stu-id="6e408-473">Create the starter app</span></span>

* <span data-ttu-id="6e408-474">Create a Razor Pages app named "ContactManager"</span><span class="sxs-lookup"><span data-stu-id="6e408-474">Create a Razor Pages app named "ContactManager"</span></span>
  * <span data-ttu-id="6e408-475">Create the app with **Individual User Accounts**.</span><span class="sxs-lookup"><span data-stu-id="6e408-475">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="6e408-476">Name it "ContactManager" so the namespace matches the namespace used in the sample.</span><span class="sxs-lookup"><span data-stu-id="6e408-476">Name it "ContactManager" so the namespace matches the namespace used in the sample.</span></span>
  * <span data-ttu-id="6e408-477">`-uld` specifies LocalDB instead of SQLite</span><span class="sxs-lookup"><span data-stu-id="6e408-477">`-uld` specifies LocalDB instead of SQLite</span></span>

  ```dotnetcli
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* <span data-ttu-id="6e408-478">Add *Models/Contact.cs*:</span><span class="sxs-lookup"><span data-stu-id="6e408-478">Add *Models/Contact.cs*:</span></span>

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="6e408-479">Scaffold the `Contact` model.</span><span class="sxs-lookup"><span data-stu-id="6e408-479">Scaffold the `Contact` model.</span></span>
* <span data-ttu-id="6e408-480">Create initial migration and update the database:</span><span class="sxs-lookup"><span data-stu-id="6e408-480">Create initial migration and update the database:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
  dotnet ef database drop -f
  dotnet ef migrations add initial
  dotnet ef database update
  ```

* <span data-ttu-id="6e408-481">Update the **ContactManager** anchor in the *Pages/_Layout.cshtml* file:</span><span class="sxs-lookup"><span data-stu-id="6e408-481">Update the **ContactManager** anchor in the *Pages/_Layout.cshtml* file:</span></span>

  ```cshtml
  <a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
  ```

* <span data-ttu-id="6e408-482">Test the app by creating, editing, and deleting a contact</span><span class="sxs-lookup"><span data-stu-id="6e408-482">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="6e408-483">Seed the database</span><span class="sxs-lookup"><span data-stu-id="6e408-483">Seed the database</span></span>

<span data-ttu-id="6e408-484">Add the [SeedData](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs) class to the *Data* folder.</span><span class="sxs-lookup"><span data-stu-id="6e408-484">Add the [SeedData](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs) class to the *Data* folder.</span></span>

<span data-ttu-id="6e408-485">Call `SeedData.Initialize` from `Main`:</span><span class="sxs-lookup"><span data-stu-id="6e408-485">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Program.cs?name=snippet)]

<span data-ttu-id="6e408-486">Test that the app seeded the database.</span><span class="sxs-lookup"><span data-stu-id="6e408-486">Test that the app seeded the database.</span></span> <span data-ttu-id="6e408-487">If there are any rows in the contact DB, the seed method doesn't run.</span><span class="sxs-lookup"><span data-stu-id="6e408-487">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

::: moniker-end

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a><span data-ttu-id="6e408-488">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="6e408-488">Additional resources</span></span>

* [<span data-ttu-id="6e408-489">Build a .NET Core and SQL Database web app in Azure App Service</span><span class="sxs-lookup"><span data-stu-id="6e408-489">Build a .NET Core and SQL Database web app in Azure App Service</span></span>](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb)
* <span data-ttu-id="6e408-490">[ASP.NET Core Authorization Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span><span class="sxs-lookup"><span data-stu-id="6e408-490">[ASP.NET Core Authorization Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span></span> <span data-ttu-id="6e408-491">This lab goes into more detail on the security features introduced in this tutorial.</span><span class="sxs-lookup"><span data-stu-id="6e408-491">This lab goes into more detail on the security features introduced in this tutorial.</span></span>
* <xref:security/authorization/introduction>
* [<span data-ttu-id="6e408-492">Custom policy-based authorization</span><span class="sxs-lookup"><span data-stu-id="6e408-492">Custom policy-based authorization</span></span>](xref:security/authorization/policies)
