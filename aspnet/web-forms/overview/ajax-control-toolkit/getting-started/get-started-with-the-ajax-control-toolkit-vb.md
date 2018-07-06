---
uid: web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-vb
title: Rozpoczynanie pracy z usługą AJAX Control Toolkit (VB) | Dokumentacja firmy Microsoft
author: microsoft
description: Dowiedz się, wszystko, czego potrzebujesz, aby rozpocząć korzystanie z zestawu narzędzi AJAX Control Toolkit.
ms.author: aspnetcontent
ms.date: 05/12/2009
ms.assetid: 9f8fa166-49a2-402c-b236-20caef0c658f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-vb
msc.type: authoredcontent
ms.openlocfilehash: 65743c9eb8eb1b02c6cf87da53222af1312f4445
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37822082"
---
<a name="get-started-with-the-ajax-control-toolkit-vb"></a>Rozpoczynanie pracy z usługą AJAX Control Toolkit (VB)
====================
przez [firmy Microsoft](https://github.com/microsoft)

> Dowiedz się, wszystko, czego potrzebujesz, aby rozpocząć korzystanie z zestawu narzędzi AJAX Control Toolkit.


Zestawu narzędzi AJAX Control Toolkit zawiera ponad 30 bezpłatnych formantów, które można używać w aplikacjach ASP.NET. W tym samouczku dowiesz się, jak pobrać zestawu narzędzi AJAX Control Toolkit i dodawanie kontrolek zestawu narzędzi do usługi Visual Studio/Visual Web Developer Express przybornika.

## <a name="downloading-the-ajax-control-toolkit"></a>Pobieranie zestawu narzędzi AJAX Control Toolkit

[Zestawu narzędzi AJAX Control Toolkit](http://devexpress.com/act) jest to projekt open source opracowany przez członków społeczności platformy ASP.NET i zespół programu ASP.NET.


[![Pobieranie zestawu narzędzi AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-vb/_static/image1.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image1.png)

**Rysunek 01**: pobranie zestawu narzędzi AJAX Control Toolkit ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](get-started-with-the-ajax-control-toolkit-vb/_static/image2.png))


Po pobraniu pliku, musisz odblokować plik. Kliknij prawym przyciskiem myszy plik, wybierz polecenie Właściwości, a następnie kliknij przycisk **odblokowanie** przycisku (patrz rysunek 2).


[![Odblokowanie pliku AJAX kontrolki zestawu narzędzi ZIP](get-started-with-the-ajax-control-toolkit-vb/_static/image2.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image3.png)

**Rysunek 02**: odblokowanie pliku AJAX kontrolki zestawu narzędzi ZIP ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](get-started-with-the-ajax-control-toolkit-vb/_static/image4.png))


Po odblokowaniu plik można Rozpakuj plik: kliknij prawym przyciskiem myszy plik i wybierz **Wyodrębnij wszystkie** opcji menu. Teraz jesteśmy gotowi dodać zestawu narzędzi do przybornika Visual Studio/Visual Web Developer.

## <a name="adding-the-ajax-control-toolkit-to-the-toolbox"></a>Dodawanie zestawu narzędzi AJAX Control Toolkit do przybornika

Aby użyć zestawu narzędzi AJAX Control Toolkit najłatwiej można dodać zestawu narzędzi do zestawu swoich Visual Studio/Visual Web Developer (zobacz rysunek 3). W ten sposób można po prostu przeciągnąć kontrolki zestawu narzędzi na stronie go użyć.


[![Zestawu narzędzi AJAX Control Toolkit zostanie wyświetlone w przyborniku](get-started-with-the-ajax-control-toolkit-vb/_static/image3.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image5.png)

**Rysunek 03**: zestawu narzędzi AJAX Control Toolkit zostanie wyświetlone w przyborniku ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](get-started-with-the-ajax-control-toolkit-vb/_static/image6.png))


Najpierw należy dodać kartę zestawu narzędzi AJAX Control Toolkit do przybornika. Wykonaj następujące kroki.

1. Utwórz nową witrynę sieci Web platformy ASP.NET, wybierając opcję menu Plik, Nowa witryna. Kliknij dwukrotnie Default.aspx w oknie Eksploratora rozwiązań, aby otworzyć go w edytorze.
2. Kliknij prawym przyciskiem myszy przybornika poniżej na karcie Ogólne, a następnie wybierz opcję menu **Dodaj zakładkę** (zobacz rysunek 4).
3. Wprowadź nową kartę o nazwie zestawu narzędzi AJAX Control Toolkit.


[![Dodawanie nowej karty](get-started-with-the-ajax-control-toolkit-vb/_static/image4.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image7.png)

**Rysunek 04**: Dodawanie nowej karty ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](get-started-with-the-ajax-control-toolkit-vb/_static/image8.png))


Następnie należy dodać kontrolki zestawu narzędzi AJAX Control Toolkit na nowej karcie. Wykonaj następujące kroki:

- Kliknij prawym przyciskiem myszy, poniżej na karcie zestawu narzędzi AJAX Control Toolkit, a następnie wybierz opcję menu **wybierz elementy (zobacz rysunek 5)**.
- Przejdź do lokalizacji, w którym rozpakowano zestawu narzędzi AJAX Control Toolkit i wybierz zestaw AjaxControlToolkit.dll.


[![Wybierz elementy do dodania do przybornika](get-started-with-the-ajax-control-toolkit-vb/_static/image5.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image9.png)

**Rysunek 05**: Wybierz elementy do dodania do przybornika ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](get-started-with-the-ajax-control-toolkit-vb/_static/image10.png))


Po wykonaniu tych kroków, zostaną wyświetlone wszystkie kontrolki zestawu narzędzi z przybornika.

## <a name="upgrading-to-a-new-version-of-the-toolkit"></a>Uaktualnianie do nowej wersji zestawu narzędzi

Jeśli była używana starsza wersja zestawu narzędzi, a teraz konieczne przeniesienie do nowszej wersji są zalecane kroki:

- Pliki binarne — usuń starą wersję zestawu AjaxControlToolkit.dll z folderu Bin witryny sieci Web.
- Elementy przybornika — Usuń kartę zestawu narzędzi AJAX Control Toolkit i postępuj zgodnie z instrukcjami powyżej, aby ponownie utworzyć kartę w nowej wersji zestawu AjaxControlToolkit.dll.

> [!div class="step-by-step"]
> [Poprzednie](creating-a-custom-ajax-control-toolkit-control-extender-cs.md)
> [dalej](using-ajax-control-toolkit-controls-and-control-extenders-vb.md)
