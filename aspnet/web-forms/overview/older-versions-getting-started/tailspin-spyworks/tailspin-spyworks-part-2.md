---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
title: "Część 2: Warstwa dostępu do danych | Dokumentacja firmy Microsoft"
author: JoeStagner
description: "Ta seria samouczek zawiera szczegóły dotyczące wszystkich kroków kompilacji Tailspin Spyworks przykładowej aplikacji. Część 2 obejmuje dodawanie Warstwa dostępu do danych."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 5a9d5429-d70b-411c-8474-f42cf7ef8a2b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
msc.type: authoredcontent
ms.openlocfilehash: 8b07b320640c1bb0074a4d3a04ca7c5b7e7bb6cd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="part-2-data-access-layer"></a>Część 2: Warstwa dostępu do danych
====================
przez [Stagner Jan](https://github.com/JoeStagner)

> Tailspin Spyworks pokazano, jak bardzo proste jest tworzenie zaawansowanych, skalowalnych aplikacji dla platformy .NET. Przedstawia on poza jak nowe, fantastyczne funkcje programu ASP.NET 4 do tworzenia sklepu online, łącznie z zakupów, wyewidencjonowania i administracji.
> 
> Ta seria samouczek zawiera szczegóły dotyczące wszystkich kroków kompilacji Tailspin Spyworks przykładowej aplikacji. Część 2 obejmuje dodawanie Warstwa dostępu do danych.


## <a id="_Toc260221668"></a>Dodawanie Warstwa dostępu do danych

Naszej aplikacji handlu elektronicznego będzie zależeć od dwóch baz danych.

Aby uzyskać informacje o kliencie użyjemy standardowe bazy danych członkostwa ASP.NET. Naszym zakupów katalogu koszyka i produktu firma Microsoft będzie zaimplementować bazy danych SQL Express w następujący sposób.

![](tailspin-spyworks-part-2/_static/image1.jpg)

Posiadanie utworzone bazy danych (Commerce.mdf) w aplikacji App\_folderu danych, można przejść do tworzenia naszych Warstwa dostępu do danych przy użyciu programu Entity Framework .NET.

Utworzymy folder o nazwie "danych\_dostępu" i ich kliknij folder prawym przyciskiem myszy i wybierz opcję "Dodaj nowy element".

W "Zainstalowane szablony" elementu, a następnie wybierz opcję "modelu danych jednostki ADO.NET" Wprowadź EDM\_Commerce.edmx jako nazwy i kliknij przycisk "Dodaj".

![](tailspin-spyworks-part-2/_static/image2.jpg)

Wybierz pozycję "Generuj z bazy danych".

![](tailspin-spyworks-part-2/_static/image1.png)

![](tailspin-spyworks-part-2/_static/image2.png)

![](tailspin-spyworks-part-2/_static/image3.png)

![](tailspin-spyworks-part-2/_static/image3.jpg)

Zapisz i kompilacji.

Teraz możemy dodać naszej pierwszej funkcji — menu kategorii produktów.

>[!div class="step-by-step"]
[Poprzednie](tailspin-spyworks-part-1.md)
[dalej](tailspin-spyworks-part-3.md)
