---
uid: mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
title: Cykl życia aplikacji ASP.NET MVC 5 | Dokumentacja firmy Microsoft
author: cephalin
description: Pobierz dokument PDF, który wykresy cykl życia aplikacji ASP.NET MVC 5. Ten dokument cyklu życia zawiera widok ogólny cykl życia MVC...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/28/2014
ms.topic: article
ms.assetid: 9c1e3a75-b644-4480-8326-11300b1ec4b3
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
msc.type: authoredcontent
ms.openlocfilehash: 98735f2e04bdd0f2fec19524e59f6272dbc4ca57
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393918"
---
<a name="lifecycle-of-an-aspnet-mvc-5-application"></a>Cykl życia aplikacji ASP.NET MVC 5
====================
przez [łą Cephas](https://github.com/cephalin)

[Pobierz dokument PDF](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf)

W tym miejscu możesz pobrać dokument PDF, który wykresy cyklu życia każda aplikacja ASP.NET MVC 5 z otrzymywania HTTP żądania do wysłania odpowiedzi HTTP z powrotem do klienta. Jest on zaprojektowany jako narzędzie edukacyjnych dla osób, które są nowe do wzorca ASP.NET MVC, a także jako odwołanie dla tych, którzy muszą przejść do określonych aspektów aplikacji. Dokument PDF ma następujące cechy:

- Odpowiednie [aplikacji HttpApplication](https://msdn.microsoft.com/library/system.web.httpapplication.aspx) etapów, które pomagają zrozumieć, w którym MVC integruje się z [cyklem życia aplikacji ASP.NET](https://msdn.microsoft.com/library/bb470252.aspx).
- Widok ogólny cykl życia aplikacji MVC, gdzie może zrozumieć główne etapy, które każda aplikacja MVC przechodzi w potoku przetwarzania żądań.  
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image1.jpg)
- Widok szczegółów, w którym przedstawiono awarii w dół w szczegóły potoku przetwarzania żądań. Możesz porównać Widok wysokiego poziomu oraz szczegółowy widok, aby zobaczyć, jak cykle życia szczegółowe informacje są zbierane w poszczególnych etapów. [Pobierz plik PDF](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf) się większym widoku.
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image2.jpg)
- Umieszczanie i cel wszystkie metody możliwym do zastąpienia w [kontrolera](https://msdn.microsoft.com/library/system.web.mvc.controller.aspx) obiekt w potoku przetwarzania żądań. Może lub nie ma konieczności zastąpienie wszelkich jednej metody, ale ważne jest, aby Ci zrozumieć ich roli w cyklu życia aplikacji, tak aby można napisać kod na etapie cyklu życia odpowiednie dla efektu, które mają.
- Diagramy skierowany w górę, przedstawiający sposób wywoływania z każdego typu filtru (uwierzytelniania, autoryzacji, akcji i wyniku).
- Połączyć artykuł lub w blogu z każdego punktu orientacyjnego w widoku szczegółów.


## <a name="next-steps"></a>Następne kroki

W tym dokumencie spełnia Twoje potrzeby? Będziemy wdzięczni za Twoją opinię. Jeśli masz pytania na temat cyklu platformy ASP.NET MVC w aplikacji [Stackoverflow](http://stackoverflow.com/help) i [fora platformy ASP.NET MVC](https://forums.asp.net/1146.aspx) są doskonałym miejscem do zadania. Postępuj zgodnie z [mnie](https://twitter.com/Cephas_MSFT) w serwisie twitter, dzięki czemu można uzyskać aktualizacje na Moje najnowsze samouczki.
