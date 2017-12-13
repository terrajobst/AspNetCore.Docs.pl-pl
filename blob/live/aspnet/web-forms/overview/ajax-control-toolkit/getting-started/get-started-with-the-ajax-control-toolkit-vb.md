---
uid: web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-vb
title: "Wprowadzenie do zestawu narzędzi kontroli AJAX (VB) | Dokumentacja firmy Microsoft"
author: microsoft
description: "Dowiedz się, musisz wiedzieć, aby rozpocząć korzystanie z zestawu narzędzi kontroli AJAX."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 9f8fa166-49a2-402c-b236-20caef0c658f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-vb
msc.type: authoredcontent
ms.openlocfilehash: 0bbf6dc0be8a96ecd47b8620a6ba3220b50f10d4
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="get-started-with-the-ajax-control-toolkit-vb"></a><span data-ttu-id="1d0d0-103">Wprowadzenie do zestawu narzędzi kontroli AJAX (VB)</span><span class="sxs-lookup"><span data-stu-id="1d0d0-103">Get Started with the AJAX Control Toolkit (VB)</span></span>
====================
<span data-ttu-id="1d0d0-104">przez [firmy Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="1d0d0-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="1d0d0-105">Dowiedz się, musisz wiedzieć, aby rozpocząć korzystanie z zestawu narzędzi kontroli AJAX.</span><span class="sxs-lookup"><span data-stu-id="1d0d0-105">Learn all you need to know to get started using the AJAX Control Toolkit.</span></span>


<span data-ttu-id="1d0d0-106">Zestaw narzędzi kontroli AJAX zawiera ponad 30 wolnego kontrolki, których można używać w aplikacji ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="1d0d0-106">The AJAX Control Toolkit contains more than 30 free controls that you can use in your ASP.NET applications.</span></span> <span data-ttu-id="1d0d0-107">W tym samouczku Dowiedz się jak pobrać Toolkit kontroli AJAX i Dodaj formanty zestawu narzędzi do przybornika programu Visual Studio/Visual Web Developer Express.</span><span class="sxs-lookup"><span data-stu-id="1d0d0-107">In this tutorial, you learn how to download the AJAX Control Toolkit and add the toolkit controls to your Visual Studio/Visual Web Developer Express toolbox.</span></span>

## <a name="downloading-the-ajax-control-toolkit"></a><span data-ttu-id="1d0d0-108">Pobieranie zestawu narzędzi kontroli AJAX</span><span class="sxs-lookup"><span data-stu-id="1d0d0-108">Downloading the AJAX Control Toolkit</span></span>

<span data-ttu-id="1d0d0-109">[Toolkit kontroli AJAX](http://devexpress.com/act) jest projekt typu open source opracowany przez członków społeczności ASP.NET i zespołu programu ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="1d0d0-109">The [AJAX Control Toolkit](http://devexpress.com/act) is an open source project developed by the members of the ASP.NET community and the ASP.NET team.</span></span>


<span data-ttu-id="1d0d0-110">[![Pobieranie zestawu narzędzi kontroli AJAX](get-started-with-the-ajax-control-toolkit-vb/_static/image1.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="1d0d0-110">[![Downloading the AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-vb/_static/image1.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image1.png)</span></span>

<span data-ttu-id="1d0d0-111">**Rysunek 01**: pobranie zestawu narzędzi kontroli AJAX ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](get-started-with-the-ajax-control-toolkit-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="1d0d0-111">**Figure 01**: Downloading the AJAX Control Toolkit([Click to view full-size image](get-started-with-the-ajax-control-toolkit-vb/_static/image2.png))</span></span>


<span data-ttu-id="1d0d0-112">Po pobraniu pliku należy odblokować plik.</span><span class="sxs-lookup"><span data-stu-id="1d0d0-112">After you download the file, you need to unblock the file.</span></span> <span data-ttu-id="1d0d0-113">Kliknij plik prawym przyciskiem myszy, wybierz polecenie Właściwości, a następnie kliknij przycisk **Odblokuj** przycisk (patrz rysunek 2).</span><span class="sxs-lookup"><span data-stu-id="1d0d0-113">Right-click the file, select Properties, and click the **Unblock** button (see Figure 2).</span></span>


<span data-ttu-id="1d0d0-114">[![Odblokowanie pliku ZIP zestawu narzędzi kontroli AJAX](get-started-with-the-ajax-control-toolkit-vb/_static/image2.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="1d0d0-114">[![Unblocking the AJAX Control Toolkit ZIP file](get-started-with-the-ajax-control-toolkit-vb/_static/image2.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image3.png)</span></span>

<span data-ttu-id="1d0d0-115">**Rysunek 02**: odblokowywania pliku ZIP zestawu narzędzi kontroli AJAX ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](get-started-with-the-ajax-control-toolkit-vb/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="1d0d0-115">**Figure 02**: Unblocking the AJAX Control Toolkit ZIP file([Click to view full-size image](get-started-with-the-ajax-control-toolkit-vb/_static/image4.png))</span></span>


<span data-ttu-id="1d0d0-116">Po odblokowaniu plik można rozpakować plik: kliknij prawym przyciskiem myszy plik i wybierz **Wyodrębnij wszystkie** opcji menu.</span><span class="sxs-lookup"><span data-stu-id="1d0d0-116">After you unblock the file, you can unzip the file: Right-click the file and select the **Extract All** menu option.</span></span> <span data-ttu-id="1d0d0-117">Teraz możemy dodać zestaw narzędzi do przybornika programu Visual Studio/Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="1d0d0-117">Now, we are ready to add the toolkit to the Visual Studio/Visual Web Developer toolbox.</span></span>

## <a name="adding-the-ajax-control-toolkit-to-the-toolbox"></a><span data-ttu-id="1d0d0-118">Dodawanie do przybornika Toolkit kontroli AJAX</span><span class="sxs-lookup"><span data-stu-id="1d0d0-118">Adding the AJAX Control Toolkit to the Toolbox</span></span>

<span data-ttu-id="1d0d0-119">Najprostszym sposobem użyj narzędzi kontroli AJAX jest dodanie zestawu narzędzi do przybornika programu Visual Studio/Visual Web Developer (patrz rysunek 3).</span><span class="sxs-lookup"><span data-stu-id="1d0d0-119">The easiest way to use the AJAX Control Toolkit is to add the toolkit to your Visual Studio/Visual Web Developer toolbox (see Figure 3).</span></span> <span data-ttu-id="1d0d0-120">W ten sposób można po prostu przeciągnąć kontroli zestawu narzędzi na stronie można z niego korzystać.</span><span class="sxs-lookup"><span data-stu-id="1d0d0-120">That way, you can simply drag a toolkit control onto a page when you want to use it.</span></span>


<span data-ttu-id="1d0d0-121">[![Zestaw narzędzi kontroli AJAX pojawia się w przyborniku](get-started-with-the-ajax-control-toolkit-vb/_static/image3.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="1d0d0-121">[![AJAX Control Toolkit appears in toolbox](get-started-with-the-ajax-control-toolkit-vb/_static/image3.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image5.png)</span></span>

<span data-ttu-id="1d0d0-122">**Rysunek 03**: AJAX kontroli Toolkit zostanie wyświetlony w przyborniku ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](get-started-with-the-ajax-control-toolkit-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="1d0d0-122">**Figure 03**: AJAX Control Toolkit appears in toolbox([Click to view full-size image](get-started-with-the-ajax-control-toolkit-vb/_static/image6.png))</span></span>


<span data-ttu-id="1d0d0-123">Najpierw należy dodać kartę Toolkit kontroli AJAX do przybornika.</span><span class="sxs-lookup"><span data-stu-id="1d0d0-123">First, you need to add an AJAX Control Toolkit tab to the toolbox.</span></span> <span data-ttu-id="1d0d0-124">Wykonaj następujące kroki.</span><span class="sxs-lookup"><span data-stu-id="1d0d0-124">Follow these steps.</span></span>

1. <span data-ttu-id="1d0d0-125">Utwórz nową witrynę sieci Web ASP.NET przez wybranie opcji menu Plik, nowej witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="1d0d0-125">Create a new ASP.NET Website by selecting the menu option File, New Website.</span></span> <span data-ttu-id="1d0d0-126">Kliknij dwukrotnie plik Default.aspx w oknie Eksploratora rozwiązań, aby otworzyć plik w edytorze.</span><span class="sxs-lookup"><span data-stu-id="1d0d0-126">Double-click the Default.aspx in the Solution Explorer window to open the file in the editor.</span></span>
2. <span data-ttu-id="1d0d0-127">Kliknij prawym przyciskiem myszy przybornika poniżej karta Ogólne i wybierz opcję menu **Dodaj kartę** (patrz rysunek 4).</span><span class="sxs-lookup"><span data-stu-id="1d0d0-127">Right-click the Toolbox beneath the General Tab and select the menu option **Add Tab** (see Figure 4).</span></span>
3. <span data-ttu-id="1d0d0-128">Wprowadź nową kartę o nazwie Toolkit kontroli AJAX.</span><span class="sxs-lookup"><span data-stu-id="1d0d0-128">Enter a new tab named AJAX Control Toolkit.</span></span>


<span data-ttu-id="1d0d0-129">[![Dodawanie nowej karty](get-started-with-the-ajax-control-toolkit-vb/_static/image4.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="1d0d0-129">[![Adding a new tab](get-started-with-the-ajax-control-toolkit-vb/_static/image4.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image7.png)</span></span>

<span data-ttu-id="1d0d0-130">**Rysunek 04**: Dodawanie nowej karty ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](get-started-with-the-ajax-control-toolkit-vb/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="1d0d0-130">**Figure 04**: Adding a new tab([Click to view full-size image](get-started-with-the-ajax-control-toolkit-vb/_static/image8.png))</span></span>


<span data-ttu-id="1d0d0-131">Następnie należy Dodaj formanty Toolkit kontroli AJAX na nowej karcie. Wykonaj następujące kroki:</span><span class="sxs-lookup"><span data-stu-id="1d0d0-131">Next, you need to add the AJAX Control Toolkit controls to the new tab. Follow these steps:</span></span>

- <span data-ttu-id="1d0d0-132">Kliknij prawym przyciskiem myszy poniżej karcie AJAX kontroli narzędzi i wybierz opcję menu **wybierz elementy (patrz rysunek 5)**.</span><span class="sxs-lookup"><span data-stu-id="1d0d0-132">Right-click beneath the AJAX Control Toolkit tab and select the menu option **Choose Items (see Figure 5)**.</span></span>
- <span data-ttu-id="1d0d0-133">Przejdź do lokalizacji, gdzie unzipped Toolkit kontroli AJAX i wybierz zestaw AjaxControlToolkit.dll.</span><span class="sxs-lookup"><span data-stu-id="1d0d0-133">Browse to the location where you unzipped the AJAX Control Toolkit and select the AjaxControlToolkit.dll assembly.</span></span>


<span data-ttu-id="1d0d0-134">[![Wybierz elementy do dodania do przybornika](get-started-with-the-ajax-control-toolkit-vb/_static/image5.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="1d0d0-134">[![Choose items to add to the toolbox](get-started-with-the-ajax-control-toolkit-vb/_static/image5.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image9.png)</span></span>

<span data-ttu-id="1d0d0-135">**Rysunek 05**: Wybierz elementy do dodania do przybornika ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](get-started-with-the-ajax-control-toolkit-vb/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="1d0d0-135">**Figure 05**: Choose items to add to the toolbox([Click to view full-size image](get-started-with-the-ajax-control-toolkit-vb/_static/image10.png))</span></span>


<span data-ttu-id="1d0d0-136">Po wykonaniu tych kroków, zostaną wyświetlone wszystkie formanty zestaw narzędzi z przybornika.</span><span class="sxs-lookup"><span data-stu-id="1d0d0-136">After you complete these steps, all of the toolkit controls will appear in your toolbox.</span></span>

## <a name="upgrading-to-a-new-version-of-the-toolkit"></a><span data-ttu-id="1d0d0-137">Uaktualnienie do nowej wersji zestawu narzędzi</span><span class="sxs-lookup"><span data-stu-id="1d0d0-137">Upgrading to a New Version of the Toolkit</span></span>

<span data-ttu-id="1d0d0-138">Jeśli zostały przy użyciu starszej wersji zestawu narzędzi, a teraz trzeba przenosić do nowszej wersji są zalecane kroki:</span><span class="sxs-lookup"><span data-stu-id="1d0d0-138">If you were using an older release of the Toolkit and now need to move to a later version here are the recommended steps:</span></span>

- <span data-ttu-id="1d0d0-139">Pliki binarne - usuń starą wersję zestawu AjaxControlToolkit.dll z folderu Bin witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="1d0d0-139">Binaries - Delete the old version of the AjaxControlToolkit.dll assembly from your website Bin folder.</span></span>
- <span data-ttu-id="1d0d0-140">Elementy przybornika — Usuń kartę Toolkit kontroli AJAX i wykonaj kroki opisane powyżej, aby ponownie utworzyć kartę w nowej wersji zestawu AjaxControlToolkit.dll.</span><span class="sxs-lookup"><span data-stu-id="1d0d0-140">Toolbox Items - Delete the AJAX Control Toolkit tab and follow the steps above to re-create the tab with the new version of the AjaxControlToolkit.dll assembly.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="1d0d0-141">[Poprzednie](creating-a-custom-ajax-control-toolkit-control-extender-cs.md)
[dalej](using-ajax-control-toolkit-controls-and-control-extenders-vb.md)</span><span class="sxs-lookup"><span data-stu-id="1d0d0-141">[Previous](creating-a-custom-ajax-control-toolkit-control-extender-cs.md)
[Next](using-ajax-control-toolkit-controls-and-control-extenders-vb.md)</span></span>
