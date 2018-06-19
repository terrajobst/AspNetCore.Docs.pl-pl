---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
title: Tworzenie rozdziale pliki do pobrania dla platformy MVC EF 5 samouczki 4 | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 4 przy użyciu Entity Framework 5 Code First i Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: d0a89089-eed8-4f61-a478-c5ffa30186f5
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
msc.type: authoredcontent
ms.openlocfilehash: bda7effabd715e4658d2472e1f0a66d7bba18cab
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30878519"
---
<a name="building-the-chapter-downloads-for-the-ef-5-mvc-4-tutorials"></a><span data-ttu-id="fe2fc-103">Tworzenie w rozdziale pliki do pobrania dla platformy MVC EF 5 samouczki 4</span><span class="sxs-lookup"><span data-stu-id="fe2fc-103">Building the Chapter Downloads for the EF 5 MVC 4 Tutorials</span></span>
====================
<span data-ttu-id="fe2fc-104">przez [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="fe2fc-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[<span data-ttu-id="fe2fc-105">Pobieranie ukończone projektu</span><span class="sxs-lookup"><span data-stu-id="fe2fc-105">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> <span data-ttu-id="fe2fc-106">Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 4 przy użyciu Entity Framework 5 Code First i programu Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="fe2fc-106">The Contoso University sample web application demonstrates how to create ASP.NET MVC 4 applications using the Entity Framework 5 Code First and Visual Studio 2012.</span></span> <span data-ttu-id="fe2fc-107">Informacje o samouczek serii, zobacz [pierwszy samouczek z tej serii](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="fe2fc-107">For information about the tutorial series, see [the first tutorial in the series](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>


## <a name="building-the-chapter-downloads"></a><span data-ttu-id="fe2fc-108">Tworzenie rozdział pliki do pobrania</span><span class="sxs-lookup"><span data-stu-id="fe2fc-108">Building the Chapter Downloads</span></span>

1. <span data-ttu-id="fe2fc-109">Pobierz i rozpakuj plik zip przykładowy projekt.</span><span class="sxs-lookup"><span data-stu-id="fe2fc-109">Download and unzip the  project sample zip file.</span></span> <span data-ttu-id="fe2fc-110">W pakiecie rozpakowane pobierania można znaleźć plików zip dodatkowe, po jednej dla każdego rozdziału zakończenia.</span><span class="sxs-lookup"><span data-stu-id="fe2fc-110">In the unzipped download package, you will find additional zip files, one for the completion of each chapter.</span></span>
2. <span data-ttu-id="fe2fc-111">Kliknij prawym przyciskiem myszy plik zip odpowiednią pozycję **właściwości**i kliknij przycisk **Odblokuj** przycisku.</span><span class="sxs-lookup"><span data-stu-id="fe2fc-111">Right click on the desired zip file, click **Properties**, and click the **Unblock** button.</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image1.png)
3. <span data-ttu-id="fe2fc-112">Rozpakuj plik.</span><span class="sxs-lookup"><span data-stu-id="fe2fc-112">Unzip the file.</span></span>
4. <span data-ttu-id="fe2fc-113">Kliknij dwukrotnie *CUx.sln* plik, aby uruchomić program Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="fe2fc-113">Double-click the *CUx.sln* file to launch Visual Studio.</span></span>
5. <span data-ttu-id="fe2fc-114">Z **narzędzia** menu, kliknij przycisk **Menedżer pakietów biblioteki**, następnie **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="fe2fc-114">From the **Tools** menu, click **Library Package Manager**, then **Package Manager Console**.</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image2.png)
6. <span data-ttu-id="fe2fc-115">W konsoli Menedżera pakietów (PMC), kliknij przycisk **przywrócić**.</span><span class="sxs-lookup"><span data-stu-id="fe2fc-115">In the Package Manager Console (PMC), click **Restore**.</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image3.png)
7. <span data-ttu-id="fe2fc-116">Zamknij program Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="fe2fc-116">Exit Visual Studio.</span></span>
8. <span data-ttu-id="fe2fc-117">Uruchom ponownie program Visual Studio, otwierając plik rozwiązania zamknięte w poprzednim kroku.</span><span class="sxs-lookup"><span data-stu-id="fe2fc-117">Restart Visual Studio, opening the solution file you closed in the step above.</span></span>
9. <span data-ttu-id="fe2fc-118">W konsoli Menedżera pakietów (PMC) wprowadź `Update-Database` polecenia:</span><span class="sxs-lookup"><span data-stu-id="fe2fc-118">In the Package Manager Console (PMC), enter the `Update-Database` command:</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image4.png)  

    > [!NOTE]
    > <span data-ttu-id="fe2fc-119">Jeśli zostanie wyświetlony następujący błąd:</span><span class="sxs-lookup"><span data-stu-id="fe2fc-119">If you get the following error:</span></span>  
    >   
    >  <span data-ttu-id="fe2fc-120">*Termin "Update-Database" nie został rozpoznany jako nazwa polecenia cmdlet, funkcji, pliku skryptu lub program wykonywalny. Sprawdź pisownię nazwy lub jeśli ścieżki został uwzględniony, sprawdź, czy ścieżka jest poprawna i spróbuj ponownie.*</span><span class="sxs-lookup"><span data-stu-id="fe2fc-120">*The term 'Update-Database' is not recognized as the name of a cmdlet, function, script file, or operable program. Check the spelling of the name, or if a path was included, verify that the path is correct and try again.*</span></span>  
    > <span data-ttu-id="fe2fc-121">Zamknij i uruchom ponownie program Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="fe2fc-121">Exit and restart Visual Studio.</span></span>

    <span data-ttu-id="fe2fc-122">Każdy migracji zostanie uruchomiony, a następnie uruchomi seed — metoda.</span><span class="sxs-lookup"><span data-stu-id="fe2fc-122">Each migration will run, then the seed method will run.</span></span> <span data-ttu-id="fe2fc-123">Teraz możesz uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="fe2fc-123">You can now run the app.</span></span>

    ![](building-the-ef5-mvc4-chapter-downloads/_static/image5.png)

> [!div class="step-by-step"]
> [<span data-ttu-id="fe2fc-124">Poprzednie</span><span class="sxs-lookup"><span data-stu-id="fe2fc-124">Previous</span></span>](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
