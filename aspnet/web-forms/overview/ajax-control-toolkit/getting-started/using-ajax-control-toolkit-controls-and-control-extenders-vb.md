---
uid: web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-vb
title: "Za pomocą technologii AJAX kontroli zestawu narzędzi kontrolek i rozszerzeń formantu (VB) | Dokumentacja firmy Microsoft"
author: microsoft
description: "Dowiedz się, jak dodać kontrolki AJAX kontroli narzędzi i rozszerzeń do stron ASP.NET."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 763650a9-ffde-46a9-b779-7a9145dd5d88
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-vb
msc.type: authoredcontent
ms.openlocfilehash: 7b248855a1b82f3e8f172b439ee36502f95a39ca
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="using-ajax-control-toolkit-controls-and-control-extenders-vb"></a>Za pomocą technologii AJAX kontroli zestawu narzędzi kontrolek i rozszerzeń formantu (VB)
====================
przez [firmy Microsoft](https://github.com/microsoft)

> Dowiedz się, jak dodać kontrolki AJAX kontroli narzędzi i rozszerzeń do stron ASP.NET.


Zestaw narzędzi kontroli AJAX zawiera zestaw kontrolek i rozszerzeń formantu. W tym samouczku krótkie możesz Dowiedz się, jak dodać kontrolki i rozszerzeń formantu strony ASP.NET.

> [!NOTE] 
> 
> Aby uzyskać instrukcje na temat instalowania narzędzi kontroli AJAX i dodawanie Toolkit kontroli AJAX do przybornika programu Visual Studio/Visual Web Developer zobacz samouczek [wprowadzenie do zestawu narzędzi kontroli AJAX](get-started-with-the-ajax-control-toolkit-vb.md).


## <a name="using-ajax-control-toolkit-controls"></a>Za pomocą technologii AJAX kontroli zestawu narzędzi formantów

Formant AJAX kontroli narzędzi działa tak, jak normalne formant ASP.NET. Można przeciągnij formant z przybornika do strony programu ASP.NET. Można dodać kontrolki na stronie w widoku projektu lub źródła.

Istnieje jedno wymaganie specjalne, gdy za pomocą formantów z zestawu narzędzi kontroli AJAX. Strona musi zawierać formantu ScriptManager. Formantu ScriptManager jest odpowiedzialny za tym wszystkie niezbędne języka JavaScript wymagane przez formanty Toolkit kontroli AJAX.

Na przykład karta AJAX kontroli Toolkit zawiera formant o nazwie kontrolce edytora. Ten formant Wyświetla Zaawansowany edytor HTML. Wykonaj następujące kroki, aby dodać kontrolki edytora do strony:

1. Utwórz nową stronę ASP.NET o nazwie ShowEditor.aspx
2. Wybierz formantu ScriptManager from beneath karta rozszerzenia AJAX w przyborniku i przeciągnij kontrolki na stronie.
3. Wybierz kontrolkę Edytor from beneath karcie Toolkit kontroli AJAX w przyborniku i przeciągnij kontrolki na stronie (zobacz rysunek 1). Projektant powinien wyglądać na rysunku 2.
4. Uruchom witrynę sieci web, wybierając opcję menu **debugowania i Rozpocznij debugowanie** lub naciśnięcie klawisza F5.
5. Powinna zostać wyświetlona strona na rysunku 3.


[![Wybranie kontrolki edytora HTML](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image1.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image1.png)

**Rysunek 01**: Wybieranie kontrolce edytora HTML ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image2.png))


[![Projektanta Visual Studio za pomocą formantu ScriptManager i edytowanie](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image2.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image3.png)

**Rysunek 02**: projektanta Visual Studio za pomocą formantu ScriptManager i Edytuj ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image4.png))


[![Na stronie DisplayEditor.aspx](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image3.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image5.png)

**Rysunek 03**: DisplayEditor.aspx strony ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image6.png))


## <a name="using-ajax-control-toolkit-control-extenders"></a>Przy użyciu rozszerzeń kontroli zestawu narzędzi kontroli AJAX

Zestaw narzędzi kontroli AJAX zawiera także rozszerzeń formantu. Zgodnie z sugestią, jego nazwa, rozszerzający kontroli rozszerza funkcjonalność formant. Na przykład rozszerzeń formantu ConfirmButton rozszerza standardowe formantu przycisku ASP.NET. Extender zmienia zachowanie s formantu przycisku Tak, aby przycisk wyświetla okno dialogowe potwierdzenia, gdy zostanie kliknięty.

Rozszerzeń formantu, podobnie jak kontroli zestawu narzędzi kontroli AJAX wymaga formantu ScriptManager. Należy dodać formantu ScriptManager do strony przed rozpoczęciem korzystania z rozszerzeń kontrolki na stronie.

Wykonaj następujące kroki, aby użyć rozszerzeń formantu ConfirmButton:

1. Utwórz nową stronę ASP.NET o nazwie ShowConfirmButton.aspx
2. Dodawanie formantu ScriptManager na stronie przeciągając kontrolki na stronie from beneath karta rozszerzenia AJAX.
3. Dodawanie formantu standardowego przycisku do strony przeciągając przycisk from beneath standardową kartę w przyborniku na powierzchnię projektanta.
4. Kliknij przycisk **Dodawanie rozszerzeń** zadań opcji (zobacz rysunek 4).
5. W oknie dialogowym Wybieranie rozszerzeń, wybierz ConfirmButtonExtender (patrz rysunek 5) i kliknij przycisk OK.
6. Wybierz kontrolkę przycisku w projektancie, a następnie rozwiń węzeł rozszerzenia, Button1\_ConfirmButtonExtender węzeł w oknie właściwości (patrz rysunek 6). Przypisuje wartość *naprawdę?* właściwości ConfirmText.
7. Uruchom strony przez wybranie opcji menu **debugowania i Rozpocznij debugowanie** lub naciśnij klawisz F5.


[![Opcja zadań Dodawanie rozszerzeń](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image4.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image7.png)

**Rysunek 04**: Dodawanie rozszerzeń zadań opcji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image8.png))


[![Wybieranie rozszerzeń formantu ConfirmButton](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image5.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image9.png)

**Rysunek 05**: Wybieranie rozszerzeń formantu ConfirmButton ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image10.png))


[![Ustawienie właściwości ConfirmButton](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image6.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image11.png)

**Rysunek 06**: ustawienie właściwości ConfirmButton ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image12.png))


Po otwarciu strony, powinien być widoczny przycisk. Po kliknięciu przycisku zostaną wyświetlone okno dialogowe potwierdzenia na rysunku 7.


[![Wyświetlanie okna dialogowego potwierdzenia](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image7.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image13.png)

**Rysunek 07**: wyświetlanie okna dialogowego potwierdzenia ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image14.png))


Zwróć uwagę, że zwykle nie przeciągania rozszerzeń kontrolki na stronie. Zamiast tego należy użyć **Dodawanie rozszerzeń** zadań opcję, aby dodać rozszerzeń do formantu, który już został dodany do strony. Zwróć uwagę, ponadto ustawienie sterowania właściwości rozszerzeń po otwarciu arkusza właściwości rozszerzanego formantu.

Jeden formant ASP.NET można rozszerzyć przez wiele rozszerzeń formantu. Arkusz właściwości formantu rozszerzaną spowoduje wyświetlenie listy wszystkich rozszerzeń formantu skojarzony z formantem.

>[!div class="step-by-step"]
[Poprzednie](get-started-with-the-ajax-control-toolkit-vb.md)
[dalej](creating-a-custom-ajax-control-toolkit-control-extender-vb.md)
