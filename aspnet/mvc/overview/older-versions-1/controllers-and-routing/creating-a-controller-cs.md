---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-cs
title: Tworzenie kontrolera (C#) | Dokumentacja firmy Microsoft
author: StephenWalther
description: "W tym samouczku Stephen Walther pokazano, jak dodać kontrolera do aplikacji platformy ASP.NET MVC."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 719d50d4-2305-454c-98b4-bae64937c48f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-cs
msc.type: authoredcontent
ms.openlocfilehash: 9faaff1e00998ef9a77c4928a9eb36fc93ab97f4
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-controller-c"></a>Tworzenie kontrolera (C#)
====================
przez [Stephen Walther](https://github.com/StephenWalther)

> W tym samouczku Stephen Walther pokazano, jak dodać kontrolera do aplikacji platformy ASP.NET MVC.


Celem tego samouczka jest opisano sposób tworzenia nowego ASP.NET MVC kontrolerów. Jak utworzyć kontrolerów zarówno przy użyciu opcji menu programu Visual Studio Dodaj kontroler, jak i ręcznie tworząc plik klasy.

### <a name="using-the-add-controller-menu-option"></a>Przy użyciu dodać kontroler opcji Menu

Najprostszym sposobem tworzenia nowego kontrolera jest kliknij prawym przyciskiem myszy folder kontrolery, w oknie Eksploratora rozwiązań w usłudze Visual Studio i wybierz **Dodaj, kontrolera** menu opcji (zobacz rysunek 1). Wybranie tej opcji menu otwiera **Dodaj kontroler** okna dialogowego (patrz rysunek 2).


[![Okno dialogowe nowego projektu](creating-a-controller-cs/_static/image1.jpg)](creating-a-controller-cs/_static/image1.png)

**Rysunek 01**: dodawania nowego kontrolera ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-controller-cs/_static/image2.png))


[![Okno dialogowe nowego projektu](creating-a-controller-cs/_static/image2.jpg)](creating-a-controller-cs/_static/image3.png)

**Rysunek 02**: okno dialogowe Dodaj kontroler ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-controller-cs/_static/image4.png))


Należy zauważyć, że pierwsza część nazwy kontrolera zostanie wyróżniona w **Dodaj kontroler** okna dialogowego. Każda nazwa kontrolera musi kończyć się sufiksem *kontrolera*. Na przykład można utworzyć kontroler o nazwie *ProductController* , ale nie kontrolerze o nazwie *produktu*.


Jeśli utworzysz kontroler brakuje *kontrolera* sufiks, a następnie nie można wywołać z kontrolerem. Nie wykonuj tej — I po niewykorzystana niezliczonych godzin życiu po wprowadzeniu tego błędu.


**Wyświetlanie listy 1 - Controllers\ProductController.cs**

[!code-csharp[Main](creating-a-controller-cs/samples/sample1.cs)]

Należy zawsze tworzyć kontrolerów w folderze kontrolerów. W przeciwnym razie będzie naruszania konwencje platformy ASP.NET MVC i innymi deweloperami będzie mieć trudniejsze czas opis aplikacji.

### <a name="scaffolding-action-methods"></a>Metody akcji szkieletów

Podczas tworzenia kontrolera, użytkownik może automatycznie wygenerować metod akcji tworzenia, aktualizacji i szczegółów (patrz rysunek 3). Jeśli zostanie wybrana ta opcja jest generowany klasy kontrolera wyświetlania 2.


[![Automatyczne tworzenie metod akcji](creating-a-controller-cs/_static/image3.jpg)](creating-a-controller-cs/_static/image5.png)

**Rysunek 03**: automatycznego tworzenia metod akcji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-controller-cs/_static/image6.png))


**Wyświetlanie listy 2 - Controllers\CustomerController.cs**

[!code-csharp[Main](creating-a-controller-cs/samples/sample2.cs)]

Te metody generowane są szkieletu metody. Należy dodać logikę rzeczywiste tworzenie, aktualizowanie i zawierającego szczegóły klienta samodzielnie. Jednak metody klasy zastępczej Podaj dobry punkt wyjścia.

### <a name="creating-a-controller-class"></a>Tworzenie klasy kontrolera

Kontroler ASP.NET MVC jest tylko klasę. Jeśli wolisz, można zignorować wygodny szkieletów kontrolera Visual Studio i ręcznie utworzyć klasę kontrolera. Wykonaj następujące kroki:

1. Kliknij prawym przyciskiem myszy folder kontrolery, a następnie wybierz opcję menu **Dodaj, nowy element** i wybierz **klasy** szablonu (patrz rysunek 4).
2. Nazwa nowej klasy PersonController.cs, a następnie kliknij przycisk **Dodaj** przycisku.
3. Zmodyfikuj plik wynikowy klasy, tak, aby klasa dziedziczy z klasy podstawowej System.Web.Mvc.Controller (patrz lista 3).


[![Tworzenie nowej klasy](creating-a-controller-cs/_static/image4.jpg)](creating-a-controller-cs/_static/image7.png)

**Rysunek 04**: Tworzenie nowej klasy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-controller-cs/_static/image8.png))


**Wyświetlanie listy 3 - Controllers\PersonController.cs**

[!code-csharp[Main](creating-a-controller-cs/samples/sample3.cs)]

Kontroler w 3 lista przedstawia jedną akcję o nazwie indeks(), która zwraca ciąg "Hello World!". Ta akcja kontrolera można wywołać przez uruchomienie aplikacji i żądanie adresu URL podobne do poniższych:

`http://localhost:40071/Person`

> [!NOTE] 
> 
> ASP.NET Development Server używa numeru portu losowych (na przykład 40071). Wprowadzenie adresu URL do wywołania kontrolera, należy podać numer prawy port. Należy określić numer portu, ustawiając kursor myszy na ikonie serwera projektowego ASP.NET, w obszarze powiadomień systemu Windows (prawym dolnym rogu ekranu).

>[!div class="step-by-step"]
[Poprzednie](adding-dynamic-content-to-a-cached-page-cs.md)
[dalej](creating-an-action-cs.md)
