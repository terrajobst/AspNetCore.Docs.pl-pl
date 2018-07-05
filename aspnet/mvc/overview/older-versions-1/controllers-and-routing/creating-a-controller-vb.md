---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-vb
title: Tworzenie kontrolera (VB) | Dokumentacja firmy Microsoft
author: StephenWalther
description: 'W tym samouczku Walther Autor: Stephen pokazuje, jak dodać kontrolera do aplikacji ASP.NET MVC.'
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 204b7e86-f560-4611-8adb-785b33e777b9
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-vb
msc.type: authoredcontent
ms.openlocfilehash: 619fff7a78526cae6c3db0f3c414215bf7db9bb5
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393892"
---
<a name="creating-a-controller-vb"></a>Tworzenie kontrolera (VB)
====================
przez [Walther Autor: Stephen](https://github.com/StephenWalther)

> W tym samouczku Walther Autor: Stephen pokazuje, jak dodać kontrolera do aplikacji ASP.NET MVC.


Celem tego samouczka jest wyjaśniają, jak utworzyć nowe platformy ASP.NET MVC kontrolerów. Dowiesz się, jak utworzyć kontrolerów, zarówno za pomocą opcji menu Visual Studio Dodaj kontroler, jak i przez utworzenie pliku klasy ręcznie.

### <a name="using-the-add-controller-menu-option"></a>Za pomocą Dodaj opcję Menu kontrolera

Najprostszym sposobem utworzenia nowego kontrolera jest kliknij prawym przyciskiem myszy folder kontrolerów, w oknie Eksploratora rozwiązań w usłudze Visual Studio i wybierz **Dodaj, kontroler** opcji menu (patrz rysunek 1). Wybranie tej opcji menu otwiera **Dodaj kontroler** okna dialogowego (patrz rysunek 2).


[![Okno dialogowe Nowy projekt](creating-a-controller-vb/_static/image1.jpg)](creating-a-controller-vb/_static/image1.png)

**Rysunek 01**: dodanie nowego kontrolera ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-controller-vb/_static/image2.png))


[![Okno dialogowe Nowy projekt](creating-a-controller-vb/_static/image2.jpg)](creating-a-controller-vb/_static/image3.png)

**Rysunek 02**: okno dialogowe Dodawanie kontrolera ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-controller-vb/_static/image4.png))


Należy zauważyć, że pierwsza część nazwy kontrolera jest wyróżniony na **Dodaj kontroler** okna dialogowego. Każda nazwa kontrolera musi kończyć się sufiksem *kontrolera*. Na przykład można utworzyć kontroler o nazwie *ProductController* , ale nie kontroler o nazwie *produktu*.


Jeśli utworzysz kontroler, na których brakuje *kontrolera* sufiks, a następnie nie będzie można wywołać z kontrolerem. Nie ma tych — mam już zmarnowany niezliczone godziny swojego życia po wprowadzeniu tego błędu.


**Wyświetlanie listy 1 - Controllers\ProductController.vb**

[!code-vb[Main](creating-a-controller-vb/samples/sample1.vb)]

Należy zawsze tworzyć kontrolerów w folderze kontrolerów. W przeciwnym razie będzie naruszenie Konwencji platformy ASP.NET MVC i inni deweloperzy mają coraz trudniejszy czasu, informacje o aplikacji.

### <a name="scaffolding-action-methods"></a>Metody akcji tworzenia szkieletów

Podczas tworzenia kontrolera, masz możliwość automatycznego wygenerowania metod akcji tworzenia, aktualizowania lub szczegóły (zobacz rysunek 3). Jeśli wybierzesz tę opcję, generowany jest klasy kontrolera w ofercie 2.


[![Automatyczne tworzenie metod akcji](creating-a-controller-vb/_static/image3.jpg)](creating-a-controller-vb/_static/image5.png)

**Rysunek 03**: automatyczne tworzenie metod akcji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-controller-vb/_static/image6.png))


**Wyświetlanie listy 2 - Controllers\CustomerController.vb**

[!code-vb[Main](creating-a-controller-vb/samples/sample2.vb)]

Te wygenerowane metody są metodami klasy zastępczej. Należy dodać rzeczywiste Logic Apps do tworzenia, aktualizowania i wyświetlania szczegółów dla klienta, samodzielnie. Jednak metod klasy zastępczej zapewnia dobre rozwiązanie punkt początkowy.

### <a name="creating-a-controller-class"></a>Tworzenie klasy kontrolera

Kontroler ASP.NET MVC jest po prostu klasą. Jeśli wolisz, możesz zignorować wygodne szkieletu kontrolera programu Visual Studio i ręcznie utworzyć klasę kontrolera. Wykonaj następujące kroki:

1. Kliknij prawym przyciskiem myszy folder kontrolery i wybierz opcję menu **Dodaj, nowy element** i wybierz **klasy** szablonu (zobacz rysunek 4).
2. Nadaj nowej klasie PersonController.vb, a następnie kliknij przycisk **Dodaj** przycisku.
3. Zmodyfikuj plik wynikowy klasy, tak, aby klasa dziedziczy z klasy bazowej System.Web.Mvc.Controller (patrz lista 3).


[![Tworzenie nowej klasy](creating-a-controller-vb/_static/image4.jpg)](creating-a-controller-vb/_static/image7.png)

**Rysunek 04**: Tworzenie nowej klasy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-controller-vb/_static/image8.png))


**Wyświetlanie listy 3 - Controllers\PersonController.vb**

[!code-vb[Main](creating-a-controller-vb/samples/sample3.vb)]

Kontroler w ofercie 3 udostępnia jedną akcję o nazwie indeks(), która zwraca ciąg "Hello World!". Można wywołać tej akcji kontrolera, uruchamiając aplikację i żąda adresu URL, jak pokazano poniżej:

`http://localhost:40071/Person`

> [!NOTE]
> 
> ASP.NET Development Server używa numeru portu losowego (na przykład 40071). Wprowadzenie adresu URL do wywołania z kontrolerem, należy podać numer portu w prawo. Należy określić numer portu, ustawiając kursor myszy na ikonie dla ASP.NET Development Server, w obszarze powiadomień (prawym dolnym rogu ekranu), Windows.
> 
> [!div class="step-by-step"]
> [Poprzednie](adding-dynamic-content-to-a-cached-page-vb.md)
> [dalej](creating-an-action-vb.md)
