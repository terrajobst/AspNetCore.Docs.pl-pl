---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-cs
title: Tworzenie akcji (C#) | Dokumentacja firmy Microsoft
author: microsoft
description: Dowiedz się, jak dodać nową akcję do Kontroler składnika ASP.NET MVC. Dowiedz się więcej o wymaganiach dotyczących metodę jako akcję.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: cb33b28c-3025-4bd1-a1fa-eaa3af7bb56f
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-cs
msc.type: authoredcontent
ms.openlocfilehash: d6f981201b93e3650b18f112dd9330f1470aa573
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37372501"
---
<a name="creating-an-action-c"></a>Tworzenie akcji (C#)
====================
przez [firmy Microsoft](https://github.com/microsoft)

> Dowiedz się, jak dodać nową akcję do Kontroler składnika ASP.NET MVC. Dowiedz się więcej o wymaganiach dotyczących metodę jako akcję.


Celem tego samouczka jest wyjaśniają, jak można utworzyć nowej akcji kontrolera. Dowiesz się o wymagania dotyczące metody akcji. Poznasz również sposób zapobiec metody przed przypadkowym jako akcję.

## <a name="adding-an-action-to-a-controller"></a>Dodawanie akcji do kontrolera

Możesz dodać nową akcję do kontrolera, dodając nową metodę do kontrolera. Na przykład kontroler w ofercie 1 zawiera akcję o nazwie indeks() i akcji, o nazwie SayHello(). Obie metody są widoczne jako akcje.

**Wyświetlanie listy 1 - Controllers\HomeController.cs**

[!code-csharp[Main](creating-an-action-cs/samples/sample1.cs)]

Aby można ujawnić modelowi wszechświat jako akcję, metody muszą spełniać określone wymagania:

- Metoda muszą być publiczne.
- Metoda nie może być metodą statyczną.
- Metoda nie może być metodą rozszerzenia.
- Metoda nie może być Konstruktor, metody pobierającej lub ustawiającej.
- Metoda nie może mieć otwartych typów ogólnych.
- Metoda nie jest metodą klasy bazowej kontrolera.
- Metoda nie może zawierać **ref** lub **się** parametrów.

Należy zauważyć, że nie ma żadnych ograniczeń w zwracanym typie akcji kontrolera. Akcja kontrolera może zwracać ciąg, DateTime, wystąpienie klasy Random lub void. Platforma ASP.NET MVC spowoduje Konwertuj zwracany typ, który nie jest wynik akcji na ciąg i renderowania ciąg do przeglądarki.

Po dodaniu dowolnej metody, które naruszają tych wymagań z kontrolerem, metoda jest ujawniona jako akcji kontrolera. Należy zachować ostrożność, w tym miejscu. Akcja kontrolera może być wywoływany przez każdy komputer połączony z Internetem. Nie, na przykład utworzyć DeleteMyWebsite() akcji kontrolera.

## <a name="preventing-a-public-method-from-being-invoked"></a>Uniemożliwienie wywoływana przez publiczną metodę

Jeśli musisz utworzyć publiczną metodę w klasie kontrolera, a nie chcesz ujawniać metody akcji kontrolera może uniemożliwić metody wywoływanie za pomocą atrybutu [NonAction]. Na przykład kontroler w ofercie 2 zawiera publiczną metodę o nazwie CompanySecrets(), który zostanie nadany atrybut [NonAction].

**Wyświetlanie listy 2 - Controllers\WorkController.cs**

[!code-csharp[Main](creating-an-action-cs/samples/sample2.cs)]

Jeśli użytkownik podejmie próbę wywołania akcji kontrolera CompanySecrets(), wpisując /Work/CompanySecrets na pasku adresu przeglądarki następnie otrzymasz komunikat o błędzie na rysunku 1.


[![Wywoływanie metody NonAction](creating-an-action-cs/_static/image1.jpg)](creating-an-action-cs/_static/image1.png)

**Rysunek 01**: wywoływanie metody NonAction ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-an-action-cs/_static/image2.png))

> [!div class="step-by-step"]
> [Poprzednie](creating-a-controller-cs.md)
> [dalej](asp-net-mvc-routing-overview-vb.md)
