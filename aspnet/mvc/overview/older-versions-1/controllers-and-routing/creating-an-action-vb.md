---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-vb
title: Tworzenie akcji (VB) | Dokumentacja firmy Microsoft
author: microsoft
description: Dowiedz się, jak dodać nową akcję do Kontroler składnika ASP.NET MVC. Dowiedz się więcej o wymaganiach dotyczących metodę jako akcję.
ms.author: riande
ms.date: 03/02/2009
ms.assetid: c8d93e11-ef78-4a30-afbc-f30419000a60
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-vb
msc.type: authoredcontent
ms.openlocfilehash: 070dd4c9d68327eec52fe385000b9ca3907eaa9f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752769"
---
<a name="creating-an-action-vb"></a>Tworzenie akcji (VB)
====================
przez [firmy Microsoft](https://github.com/microsoft)

> Dowiedz się, jak dodać nową akcję do Kontroler składnika ASP.NET MVC. Dowiedz się więcej o wymaganiach dotyczących metodę jako akcję.


Celem tego samouczka jest wyjaśniają, jak można utworzyć nowej akcji kontrolera. Dowiesz się o wymagania dotyczące metody akcji. Poznasz również sposób zapobiec metody przed przypadkowym jako akcję.

## <a name="adding-an-action-to-a-controller"></a>Dodawanie akcji do kontrolera

Możesz dodać nową akcję do kontrolera, dodając nową metodę do kontrolera. Na przykład kontroler w ofercie 1 zawiera akcję o nazwie indeks() i akcji, o nazwie SayHello(). Obie metody są widoczne jako akcje.

**Wyświetlanie listy 1 - Controllers\HomeController.vb**

[!code-vb[Main](creating-an-action-vb/samples/sample1.vb)]

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

Jeśli musisz utworzyć publiczną metodę w klasie kontrolera i nie chcesz ujawniać metody akcji kontrolera, a następnie można zapobiec metodę wywoływaną za pomocą &lt;NonAction&gt; atrybutu. Na przykład kontroler w ofercie 2 zawiera publiczną metodę o nazwie CompanySecrets(), który zostanie nadany &lt;NonAction&gt; atrybutu.

**Wyświetlanie listy 2 - Controllers\WorkController.vb**

[!code-vb[Main](creating-an-action-vb/samples/sample2.vb)]

Jeśli użytkownik podejmie próbę wywołania akcji kontrolera CompanySecrets(), wpisując /Work/CompanySecrets na pasku adresu przeglądarki następnie otrzymasz komunikat o błędzie na rysunku 1.


[![Wywoływanie metody NonAction](creating-an-action-vb/_static/image1.jpg)](creating-an-action-vb/_static/image1.png)

**Rysunek 01**: wywoływanie metody NonAction ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-an-action-vb/_static/image2.png))

> [!div class="step-by-step"]
> [Poprzednie](creating-a-controller-vb.md)
> [dalej](aspnet-mvc-controllers-overview-cs.md)
