---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
title: 'Część 3: Układ i Menu kategorii | Dokumentacja firmy Microsoft'
author: JoeStagner
description: Ta seria samouczek zawiera szczegóły dotyczące wszystkich kroków kompilacji Tailspin Spyworks przykładowej aplikacji. Część 3 obejmuje dodawanie układ i menu kategorii.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 94ea1a70-a9bc-4241-8f36-08366d64bab9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
msc.type: authoredcontent
ms.openlocfilehash: 27a493173b03f813ee3dcbbfafd8bc52fb0b9771
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="part-3-layout-and-category-menu"></a>Część 3: Układ i Menu kategorii
====================
przez [Stagner Jan](https://github.com/JoeStagner)

> Tailspin Spyworks pokazano, jak bardzo proste jest tworzenie zaawansowanych, skalowalnych aplikacji dla platformy .NET. Przedstawia on poza jak nowe, fantastyczne funkcje programu ASP.NET 4 do tworzenia sklepu online, łącznie z zakupów, wyewidencjonowania i administracji.
> 
> Ta seria samouczek zawiera szczegóły dotyczące wszystkich kroków kompilacji Tailspin Spyworks przykładowej aplikacji. Część 3 obejmuje dodawanie układ i menu kategorii.


## <a id="_Toc260221669"></a>  Dodanie niektórych układ i Menu kategorii

W naszym strony wzorcowej witryny dodamy div kolumny po lewej stronie, która będzie zawierać naszych menu Kategoria produktu.

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample1.aspx)]

Należy pamiętać, że inne formatowanie i odpowiednie dopasowanie będą udostępniane przez klasy CSS, która dodane do naszych pliku Style.css.

[!code-css[Main](tailspin-spyworks-part-3/samples/sample2.css)]

Menu Kategoria produktu zostanie dynamicznie utworzony w czasie wykonywania, badając Commerce bazy danych dla istniejącej kategorii produktów i tworzenie elementów menu i odpowiadającego łączy.

W tym celu użyjemy dwa ASP. Formanty zaawansowanych danych w sieci. Formantem "Entity Data Source" i "W elemencie ListView".

![](tailspin-spyworks-part-3/_static/image1.jpg)

Załóżmy Przełącz się do "Projekt" i pomocników umożliwiają konfigurowanie naszych kontrolki.

![](tailspin-spyworks-part-3/_static/image2.jpg)

Załóżmy ustawioną właściwość Identyfikatora obiektu EntityDataSource EDS\_kategorii\_Menu i kliknij pozycję "Konfigurowanie źródła danych".

![](tailspin-spyworks-part-3/_static/image3.jpg)

Wybierz połączenie CommerceEntities utworzonym dla nas podczas utworzyliśmy źródłowego modelu danych jednostki dla naszych Commerce bazy danych i kliknij przycisk "Dalej".

![](tailspin-spyworks-part-3/_static/image4.jpg)

Wybierz nazwę zestawu jednostek "Kategorie" i pozostaw pozostałe opcje jako domyślny. Kliknij przycisk "Zakończ".

Teraz załóżmy ustawić właściwość Identyfikator wystąpienia formantu ListView, który możemy umieścić na naszą stronę do elementu ListView\_ProductsMenu i Aktywuj jego pomocy.

![](tailspin-spyworks-part-3/_static/image5.jpg)

Mimo że firma Microsoft można użyć opcji kontroli do formatowania wyświetlania elementu danych i formatowania, wszystkich naszych tworzenie menu będzie wymagać tylko proste znaczników, firma Microsoft będzie wprowadzenie kodu w widoku źródła.

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample3.aspx)]

Należy pamiętać, instrukcji "Eval": &lt;% # % Eval("CategoryName")&gt;

Składnia ASP.NET &lt;% # %&gt; Konwencji skrótowa, która sprawia, że środowisko uruchomieniowe można wykonać, jest zawarty w i zapisuje wyniki "w wierszu".

Instrukcja Eval("CategoryName") nakazuje, dla bieżącego wpisu w powiązanej kolekcji elementów danych, Pobierz wartość nazwy elementów modelu jednostki "CatagoryName". To krótkie składnię bardzo zaawansowaną funkcją.

Umożliwia uruchamianie aplikacji teraz.

![](tailspin-spyworks-part-3/_static/image6.jpg)

Nasze menu Kategoria produktu jest teraz wyświetlany i po możemy umieść kursor nad jednym z elementów menu kategorii widzimy punktów menu element link do strony mamy jeszcze do zaimplementowania o nazwie ProductsList.aspx i że nawiązaliśmy argument ciągu zapytania dynamicznego który zawiera  Identyfikator kategorii.

> [!div class="step-by-step"]
> [Poprzednie](tailspin-spyworks-part-2.md)
> [dalej](tailspin-spyworks-part-4.md)
