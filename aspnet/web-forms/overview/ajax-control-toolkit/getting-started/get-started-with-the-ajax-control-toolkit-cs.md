---
uid: web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-cs
title: Wprowadzenie do zestawu narzędzi kontroli AJAX (C#) | Dokumentacja firmy Microsoft
author: microsoft
description: Dowiedz się, musisz wiedzieć, aby rozpocząć korzystanie z zestawu narzędzi kontroli AJAX.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 16dc5c11-65be-4eae-a818-9fad7f8259c6
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-cs
msc.type: authoredcontent
ms.openlocfilehash: e6a7a8d45f32a33eaacf3c42b52a02d2ada1aab6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="get-started-with-the-ajax-control-toolkit-c"></a>Wprowadzenie do zestawu narzędzi kontroli AJAX (C#)
====================
przez [firmy Microsoft](https://github.com/microsoft)

> Dowiedz się, musisz wiedzieć, aby rozpocząć korzystanie z zestawu narzędzi kontroli AJAX.


Zestaw narzędzi kontroli AJAX zawiera ponad 30 wolnego kontrolki, których można używać w aplikacji ASP.NET. W tym samouczku Dowiedz się jak pobrać Toolkit kontroli AJAX i Dodaj formanty zestawu narzędzi do przybornika programu Visual Studio/Visual Web Developer Express.

## <a name="downloading-the-ajax-control-toolkit"></a>Pobieranie zestawu narzędzi kontroli AJAX

[Toolkit kontroli AJAX](http://devexpress.com/act) jest projekt typu open source opracowany przez członków społeczności ASP.NET i zespołu programu ASP.NET. 


[![Pobieranie zestawu narzędzi kontroli AJAX](get-started-with-the-ajax-control-toolkit-cs/_static/image1.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image1.png)

**Rysunek 01**: pobranie zestawu narzędzi kontroli AJAX ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](get-started-with-the-ajax-control-toolkit-cs/_static/image2.png))


Po pobraniu pliku należy odblokować plik. Kliknij plik prawym przyciskiem myszy, wybierz polecenie Właściwości, a następnie kliknij przycisk **Odblokuj** przycisk (patrz rysunek 2).


[![Odblokowanie pliku ZIP zestawu narzędzi kontroli AJAX](get-started-with-the-ajax-control-toolkit-cs/_static/image2.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image3.png)

**Rysunek 02**: odblokowywania pliku ZIP zestawu narzędzi kontroli AJAX ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](get-started-with-the-ajax-control-toolkit-cs/_static/image4.png))


Po odblokowaniu plik można rozpakować plik: kliknij prawym przyciskiem myszy plik i wybierz **Wyodrębnij wszystkie** opcji menu. Teraz możemy dodać zestaw narzędzi do przybornika programu Visual Studio/Visual Web Developer.

## <a name="adding-the-ajax-control-toolkit-to-the-toolbox"></a>Dodawanie do przybornika Toolkit kontroli AJAX

Najprostszym sposobem użyj narzędzi kontroli AJAX jest dodanie zestawu narzędzi do przybornika programu Visual Studio/Visual Web Developer (patrz rysunek 3). W ten sposób można po prostu przeciągnąć kontroli zestawu narzędzi na stronie można z niego korzystać.


[![Zestaw narzędzi kontroli AJAX pojawia się w przyborniku](get-started-with-the-ajax-control-toolkit-cs/_static/image3.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image5.png)

**Rysunek 03**: AJAX kontroli Toolkit zostanie wyświetlony w przyborniku ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](get-started-with-the-ajax-control-toolkit-cs/_static/image6.png))


Najpierw należy dodać kartę Toolkit kontroli AJAX do przybornika. Wykonaj następujące kroki.

1. Utwórz nową witrynę sieci Web ASP.NET przez wybranie opcji menu Plik, nowej witryny sieci Web. Kliknij dwukrotnie plik Default.aspx w oknie Eksploratora rozwiązań, aby otworzyć plik w edytorze.
2. Kliknij prawym przyciskiem myszy przybornika poniżej karta Ogólne i wybierz opcję menu **Dodaj kartę** (patrz rysunek 4).
3. Wprowadź nową kartę o nazwie Toolkit kontroli AJAX.


[![Dodawanie nowej karty](get-started-with-the-ajax-control-toolkit-cs/_static/image4.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image7.png)

**Rysunek 04**: Dodawanie nowej karty ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](get-started-with-the-ajax-control-toolkit-cs/_static/image8.png))


Następnie należy Dodaj formanty Toolkit kontroli AJAX na nowej karcie. Wykonaj następujące kroki:

- Kliknij prawym przyciskiem myszy poniżej karcie AJAX kontroli narzędzi i wybierz opcję menu **wybierz elementy (patrz rysunek 5)**.
- Przejdź do lokalizacji, gdzie unzipped Toolkit kontroli AJAX i wybierz zestaw AjaxControlToolkit.dll.


[![Wybierz elementy do dodania do przybornika](get-started-with-the-ajax-control-toolkit-cs/_static/image5.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image9.png)

**Rysunek 05**: Wybierz elementy do dodania do przybornika ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](get-started-with-the-ajax-control-toolkit-cs/_static/image10.png))


Po wykonaniu tych kroków, zostaną wyświetlone wszystkie formanty zestaw narzędzi z przybornika.

## <a name="upgrading-to-a-new-version-of-the-toolkit"></a>Uaktualnienie do nowej wersji zestawu narzędzi

Jeśli zostały przy użyciu starszej wersji zestawu narzędzi, a teraz trzeba przenosić do nowszej wersji są zalecane kroki:

- Pliki binarne - usuń starą wersję zestawu AjaxControlToolkit.dll z folderu Bin witryny sieci Web.
- Elementy przybornika — Usuń kartę Toolkit kontroli AJAX i wykonaj kroki opisane powyżej, aby ponownie utworzyć kartę w nowej wersji zestawu AjaxControlToolkit.dll.

> [!div class="step-by-step"]
> [Next](using-ajax-control-toolkit-controls-and-control-extenders-cs.md)
