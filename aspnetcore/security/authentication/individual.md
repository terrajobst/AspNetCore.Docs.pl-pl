---
title: Artykuły oparte na ASP.NET Core projektach utworzonych przy użyciu poszczególnych kont użytkowników
author: rick-anderson
description: Odkryj artykuły na podstawie ASP.NET Core projektów utworzonych przy użyciu poszczególnych kont użytkowników.
ms.author: riande
ms.date: 11/30/2017
uid: security/authentication/individual
ms.openlocfilehash: cf548417268a8587787471b9ed91c0ed109fbee9
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/18/2019
ms.locfileid: "71080704"
---
# <a name="articles-based-on-aspnet-core-projects-created-with-individual-user-accounts"></a><span data-ttu-id="92bcf-103">Artykuły oparte na ASP.NET Core projektach utworzonych przy użyciu poszczególnych kont użytkowników</span><span class="sxs-lookup"><span data-stu-id="92bcf-103">Articles based on ASP.NET Core projects created with individual user accounts</span></span>

<span data-ttu-id="92bcf-104">ASP.NET Core tożsamość jest dołączona do szablonów projektu w programie Visual Studio z opcją "indywidualne konta użytkowników".</span><span class="sxs-lookup"><span data-stu-id="92bcf-104">ASP.NET Core Identity is included in project templates in Visual Studio with the "Individual User Accounts" option.</span></span>

<span data-ttu-id="92bcf-105">Szablony uwierzytelniania są dostępne w interfejs wiersza polecenia platformy .NET Core z `-au Individual`:</span><span class="sxs-lookup"><span data-stu-id="92bcf-105">The authentication templates are available in .NET Core CLI with `-au Individual`:</span></span>

::: moniker range=">= aspnetcore-2.1"

```dotnetcli
dotnet new mvc -au Individual
dotnet new webapp -au Individual
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```dotnetcli
dotnet new mvc -au Individual
dotnet new razor -au Individual
```

::: moniker-end

<span data-ttu-id="92bcf-106">Zobacz [ten problem](https://github.com/aspnet/AspNetCore/issues/5833) w usłudze GitHub, aby uzyskać uwierzytelnianie interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="92bcf-106">See [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/5833) for web API authentication.</span></span>

<a name="no"></a>

## <a name="no-authentication"></a><span data-ttu-id="92bcf-107">Bez uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="92bcf-107">No Authentication</span></span>

<span data-ttu-id="92bcf-108">Uwierzytelnianie jest określone w interfejs wiersza polecenia platformy .NET Core z `-au` opcją.</span><span class="sxs-lookup"><span data-stu-id="92bcf-108">Authentication is specified in the .NET Core CLI with the `-au` option.</span></span> <span data-ttu-id="92bcf-109">W programie Visual Studio okno dialogowe **Zmienianie uwierzytelniania** jest dostępne dla nowych aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="92bcf-109">In Visual Studio, the **Change Authentication** dialog is available for new web applications.</span></span> <span data-ttu-id="92bcf-110">Wartość domyślna dla nowych aplikacji sieci Web w programie Visual Studio **nie jest uwierzytelnianiem**.</span><span class="sxs-lookup"><span data-stu-id="92bcf-110">The default for new web apps in Visual Studio is **No Authentication**.</span></span>

<span data-ttu-id="92bcf-111">Projekty utworzone bez uwierzytelniania:</span><span class="sxs-lookup"><span data-stu-id="92bcf-111">Projects created with no authentication:</span></span>

* <span data-ttu-id="92bcf-112">Nie zawierają stron sieci Web i interfejsu użytkownika w celu zalogowania się i wylogowania.</span><span class="sxs-lookup"><span data-stu-id="92bcf-112">Don't contain web pages and UI to sign in and sign out.</span></span>
* <span data-ttu-id="92bcf-113">Nie zawierają kodu uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="92bcf-113">Don't contain authentication code.</span></span>

<a name="win"></a>

## <a name="windows-authentication"></a><span data-ttu-id="92bcf-114">Uwierzytelnianie systemu Windows</span><span class="sxs-lookup"><span data-stu-id="92bcf-114">Windows Authentication</span></span>

<span data-ttu-id="92bcf-115">Uwierzytelnianie systemu Windows jest określone dla nowych aplikacji sieci Web w interfejs wiersza polecenia platformy .NET Core z `-au Windows` opcją.</span><span class="sxs-lookup"><span data-stu-id="92bcf-115">Windows Authentication is specified for new web apps in the .NET Core CLI with the `-au Windows` option.</span></span> <span data-ttu-id="92bcf-116">W programie Visual Studio w oknie dialogowym **Zmienianie uwierzytelniania** dostępne są opcje **uwierzytelniania systemu Windows** .</span><span class="sxs-lookup"><span data-stu-id="92bcf-116">In Visual Studio, the **Change Authentication** dialog provides the **Windows Authentication** options.</span></span>

<span data-ttu-id="92bcf-117">Jeśli wybrano opcję uwierzytelnianie systemu Windows, aplikacja jest skonfigurowana do korzystania z [modułu usług IIS uwierzytelniania systemu Windows](xref:host-and-deploy/iis/modules).</span><span class="sxs-lookup"><span data-stu-id="92bcf-117">If Windows Authentication is selected, the app is configured to use the [Windows Authentication IIS module](xref:host-and-deploy/iis/modules).</span></span> <span data-ttu-id="92bcf-118">Uwierzytelnianie systemu Windows jest przeznaczone dla intranetowych witryn sieci Web.</span><span class="sxs-lookup"><span data-stu-id="92bcf-118">Windows Authentication is intended for Intranet web sites.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="92bcf-119">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="92bcf-119">Additional resources</span></span>

<span data-ttu-id="92bcf-120">W poniższych artykułach pokazano, jak używać kodu wygenerowanego w szablonach ASP.NET Core, które używają poszczególnych kont użytkowników:</span><span class="sxs-lookup"><span data-stu-id="92bcf-120">The following articles show how to use the code generated in ASP.NET Core templates that use individual user accounts:</span></span>

* [<span data-ttu-id="92bcf-121">Uwierzytelnianie dwuskładnikowe za pomocą wiadomości SMS</span><span class="sxs-lookup"><span data-stu-id="92bcf-121">Two-factor authentication with SMS</span></span>](xref:security/authentication/2fa)
* [<span data-ttu-id="92bcf-122">Potwierdzenie konta i odzyskiwanie hasła w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="92bcf-122">Account confirmation and password recovery in ASP.NET Core</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="92bcf-123">Tworzenie aplikacji ASP.NET Core przy użyciu danych użytkownika chronionych przez autoryzację</span><span class="sxs-lookup"><span data-stu-id="92bcf-123">Create an ASP.NET Core app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)
