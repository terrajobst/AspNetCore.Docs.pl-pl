---
title: Wprowadzenie do składników Razor
author: guardrex
description: Dowiedz się, jak rozpocząć pracę z architekturą składniki Razor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: razor-components/get-started
ms.openlocfilehash: c83af10fd84bc8238f5fe20c66b91ba17de80ae3
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/02/2019
ms.locfileid: "55668126"
---
# <a name="get-started-with-razor-components"></a><span data-ttu-id="f54f4-103">Wprowadzenie do składników Razor</span><span class="sxs-lookup"><span data-stu-id="f54f4-103">Get started with Razor Components</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

## <a name="setup"></a><span data-ttu-id="f54f4-104">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="f54f4-104">Setup</span></span>

<span data-ttu-id="f54f4-105">Zainstaluj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="f54f4-105">Install the following:</span></span>

1. <span data-ttu-id="f54f4-106">[Zestaw SDK programu .NET core 2.1](https://go.microsoft.com/fwlink/?linkid=873092) (2.1.500 lub nowszej).</span><span class="sxs-lookup"><span data-stu-id="f54f4-106">[.NET Core 2.1 SDK](https://go.microsoft.com/fwlink/?linkid=873092) (2.1.500 or later).</span></span>
1. <span data-ttu-id="f54f4-107">[Program Visual Studio 2017](https://go.microsoft.com/fwlink/?linkid=873093) (15.9 lub nowszej) z *ASP.NET i tworzenie aplikacji internetowych* wybranym pakietem roboczym.</span><span class="sxs-lookup"><span data-stu-id="f54f4-107">[Visual Studio 2017](https://go.microsoft.com/fwlink/?linkid=873093) (15.9 or later) with the *ASP.NET and web development* workload selected.</span></span>
1. <span data-ttu-id="f54f4-108">Najnowsze [rozszerzenia usług językowych Blazor](https://go.microsoft.com/fwlink/?linkid=870389) z witryny Marketplace programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f54f4-108">The latest [Blazor Language Services extension](https://go.microsoft.com/fwlink/?linkid=870389) from the Visual Studio Marketplace.</span></span>
1. <span data-ttu-id="f54f4-109">Szablony Blazor w wierszu polecenia:</span><span class="sxs-lookup"><span data-stu-id="f54f4-109">The Blazor templates on the command-line:</span></span>

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates
   ```

<span data-ttu-id="f54f4-110">Aby utworzyć swój pierwszy projekt w programie Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="f54f4-110">To create your first project from Visual Studio:</span></span>

1. <span data-ttu-id="f54f4-111">Wybierz **pliku** > **nowy projekt** > **Web** > **aplikacji sieci Web platformy ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="f54f4-111">Select **File** > **New Project** > **Web** > **ASP.NET Core Web Application**.</span></span>
1. <span data-ttu-id="f54f4-112">Upewnij się, że **platformy .NET Core** i **platformy ASP.NET Core 2.1** wybrano u góry.</span><span class="sxs-lookup"><span data-stu-id="f54f4-112">Make sure **.NET Core** and **ASP.NET Core 2.1** are selected at the top.</span></span>
1. <span data-ttu-id="f54f4-113">Wybierz szablon Blazor **OK**.</span><span class="sxs-lookup"><span data-stu-id="f54f4-113">Choose the Blazor template and select **OK**.</span></span>

   ![Nowe okno dialogowe aplikacji](https://msdnshared.blob.core.windows.net/media/2018/07/new-blazor-app-dialog-0.5.0.png)

1. <span data-ttu-id="f54f4-115">Naciśnij klawisz **Ctrl-F5** do uruchomienia aplikacji *bez debugera*.</span><span class="sxs-lookup"><span data-stu-id="f54f4-115">Press **Ctrl-F5** to run the app *without the debugger*.</span></span> <span data-ttu-id="f54f4-116">Uruchamianie przy użyciu debugera (**F5**) nie jest obsługiwana w tej chwili.</span><span class="sxs-lookup"><span data-stu-id="f54f4-116">Running with the debugger (**F5**) isn't supported at this time.</span></span>

<span data-ttu-id="f54f4-117">Aby utworzyć nową aplikację Blazor z wiersza polecenia:</span><span class="sxs-lookup"><span data-stu-id="f54f4-117">To create a new Blazor app from the command-line:</span></span>

```console
dotnet new blazor -o BlazorApp1
cd BlazorApp1
dotnet run
```

<span data-ttu-id="f54f4-118">Gratulacje!</span><span class="sxs-lookup"><span data-stu-id="f54f4-118">Congrats!</span></span> <span data-ttu-id="f54f4-119">Po prostu uruchomiono swoją pierwszą aplikację Blazor!</span><span class="sxs-lookup"><span data-stu-id="f54f4-119">You just ran your first Blazor app!</span></span>

![Strona główna aplikacji Blazor](https://msdnshared.blob.core.windows.net/media/2018/04/blazor-bootstrap-4.png)

## <a name="help--feedback"></a><span data-ttu-id="f54f4-121">Pomoc i opinie</span><span class="sxs-lookup"><span data-stu-id="f54f4-121">Help & feedback</span></span>

<span data-ttu-id="f54f4-122">Twoja opinia jest dla nas szczególnie ważna w tej fazie eksperymentalne dla Blazor.</span><span class="sxs-lookup"><span data-stu-id="f54f4-122">Your feedback is especially important to us during this experimental phase for Blazor.</span></span> <span data-ttu-id="f54f4-123">W przypadku napotkania problemów lub masz pytania, podczas próby się Blazor, Daj nam znać!</span><span class="sxs-lookup"><span data-stu-id="f54f4-123">If you run into issues or have questions while trying out Blazor, please let us know!</span></span>

* <span data-ttu-id="f54f4-124">[Plik problemów w usłudze GitHub](https://github.com/aspnet/AspNetCore/issues) przypadku napotkania problemów lub sugestie dotyczące ulepszenia.</span><span class="sxs-lookup"><span data-stu-id="f54f4-124">[File issues on GitHub](https://github.com/aspnet/AspNetCore/issues) for any problems you run into or to make suggestions for improvements.</span></span>
* <span data-ttu-id="f54f4-125">Porozmawiaj z nami i społeczności Blazor na [dotyczącym oprogramowania Gitter](https://gitter.im/aspnet/blazor) czy uzyskać została zablokowana na udostępnianie Blazor pracy za Ciebie.</span><span class="sxs-lookup"><span data-stu-id="f54f4-125">Chat with us and the Blazor community on [Gitter](https://gitter.im/aspnet/blazor) if you get stuck or to share how Blazor is working for you.</span></span>

<span data-ttu-id="f54f4-126">Po wypróbowaniu Blazor Podaj nam co myślisz, wykorzystując naszą ankietę w ramach produktu.</span><span class="sxs-lookup"><span data-stu-id="f54f4-126">After you've tried out Blazor, please let us know what you think by taking our in-product survey.</span></span> <span data-ttu-id="f54f4-127">Kliknij łącze ankiety, wyświetlany na stronie głównej aplikacji, gdy uruchomiony jeden z szablonów projektów Blazor:</span><span class="sxs-lookup"><span data-stu-id="f54f4-127">Just click the survey link shown on the app home page when running one of the Blazor project templates:</span></span>

![Udział w ankiecie Blazor](https://msdnshared.blob.core.windows.net/media/2018/05/blazor-survey-new.png)

## <a name="next-steps"></a><span data-ttu-id="f54f4-129">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="f54f4-129">Next steps</span></span>

<xref:tutorials/first-blazor-app>
