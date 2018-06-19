---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-vb
title: Tworzenie akcji (VB) | Dokumentacja firmy Microsoft
author: microsoft
description: Dowiedz się, jak dodać nową akcję do kontrolera ASP.NET MVC. Dowiedz się więcej o wymaganiach dotyczących metodę, aby akcji.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: c8d93e11-ef78-4a30-afbc-f30419000a60
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-vb
msc.type: authoredcontent
ms.openlocfilehash: c77e4738444c61d60bdd78a50b36f98be41fc271
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30867895"
---
<a name="creating-an-action-vb"></a>Tworzenie akcji (VB)
====================
przez [firmy Microsoft](https://github.com/microsoft)

> Dowiedz się, jak dodać nową akcję do kontrolera ASP.NET MVC. Dowiedz się więcej o wymaganiach dotyczących metodę, aby akcji.


Celem tego samouczka jest opisano sposób tworzenia nowej akcji kontrolera. Poznaj wymagania dotyczące metody akcji. Możesz również sposób zapobiec przypadkowym jako akcja metody.

## <a name="adding-an-action-to-a-controller"></a>Dodawanie akcji do kontrolera

Możesz dodać nową akcję do kontrolera, dodając nową metodę do kontrolera. Na przykład kontrolera 1 Lista zawiera akcji o nazwie indeks() i o nazwie SayHello(). Obie metody są widoczne jako akcje.

**Wyświetlanie listy 1 - Controllers\HomeController.vb**

[!code-vb[Main](creating-an-action-vb/samples/sample1.vb)]

Aby udostępnić universe jako akcja, metody muszą spełniać określone wymagania:

- Metoda musi być publiczny.
- Metoda nie może być metodą statyczną.
- Metoda nie może być metodą rozszerzenia.
- Metoda nie może być Konstruktor, metody pobierającej lub ustawiającej.
- Metoda nie może mieć Otwórz typów ogólnych.
- Metoda nie jest metodą klasy podstawowej kontrolera.
- Metoda nie może zawierać **ref** lub **limit** parametrów.

Zwróć uwagę, że nie ma żadnych ograniczeń na typ zwrotny akcji kontrolera. Akcja kontrolera może zwrócić typu string, DateTime, wystąpienie klasy Random lub void. Platforma ASP.NET MVC zostanie przekonwertować zwracany typ, który nie jest wynik akcji na ciąg i renderowania ciąg do przeglądarki.

Po dodaniu dowolnej metody, która narusza wymagania do kontrolera metody jest ujawniona jako akcji kontrolera. Należy zachować ostrożność, w tym miejscu. Akcja kontrolera może być wywoływany przez każdy komputer połączony z Internetem. Nie, na przykład utworzyć DeleteMyWebsite() akcji kontrolera.

## <a name="preventing-a-public-method-from-being-invoked"></a>Uniemożliwia publicznego — metoda

Jeśli musisz utworzyć publiczną metodę w klasie kontrolera i nie chcesz ujawnia metody akcji kontrolera, można zapobiec metodę wywoływaną przy użyciu &lt;NonAction&gt; atrybutu. Na przykład kontroler wyświetlania 2 zawiera metody publicznej o nazwie CompanySecrets(), który ma &lt;NonAction&gt; atrybutu.

**Wyświetlanie listy 2 - Controllers\WorkController.vb**

[!code-vb[Main](creating-an-action-vb/samples/sample2.vb)]

Jeśli będziesz próbować wywołać CompanySecrets() akcji kontrolera, wpisując /Work/CompanySecrets na pasku adresu przeglądarki następnie zostanie wyświetlony komunikat o błędzie na rysunku 1.


[![Wywoływanie metody NonAction](creating-an-action-vb/_static/image1.jpg)](creating-an-action-vb/_static/image1.png)

**Rysunek 01**: wywoływanie metody NonAction ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-an-action-vb/_static/image2.png))

> [!div class="step-by-step"]
> [Poprzednie](creating-a-controller-vb.md)
> [dalej](aspnet-mvc-controllers-overview-cs.md)
