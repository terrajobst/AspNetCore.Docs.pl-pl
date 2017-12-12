---
uid: mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
title: "Cykl życia aplikacji platformy ASP.NET MVC 5 | Dokumentacja firmy Microsoft"
author: cephalin
description: "Pobierz dokument PDF, który wykresy cyklem życia aplikacji ASP.NET MVC 5. Ten cykl życia dokument przedstawia ogólny widok życia MVC..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/28/2014
ms.topic: article
ms.assetid: 9c1e3a75-b644-4480-8326-11300b1ec4b3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
msc.type: authoredcontent
ms.openlocfilehash: 5692c43168eb261c91f40e2046897a1e5d31a028
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="lifecycle-of-an-aspnet-mvc-5-application"></a>Cykl życia aplikacji platformy ASP.NET MVC 5
====================
przez [Cephas łącz](https://github.com/cephalin)

[Pobierz dokument PDF](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf)

W tym miejscu możesz pobrać dokument PDF, który wykresy cyklem życia każda aplikacja ASP.NET MVC 5 z otrzymywania HTTP żądania do wysyłania odpowiedzi HTTP z powrotem do klienta. Zaprojektowano go jako edukacyjnym narzędzia dla osób, które są nowe w programie ASP.NET MVC, a także jako punkt odniesienia dla tych, którzy muszą przejść do szczegółów w określonych aspektów aplikacji. Dokument PDF ma następujące funkcje:

- Odpowiednie [obiekcie HttpApplication](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.aspx) etapy pomagające zrozumieć, w którym integruje MVC [cyklem życia aplikacji ASP.NET](https://msdn.microsoft.com/en-us/library/bb470252.aspx).
- Widok wysokiego poziomu cyklem życia aplikacji MVC, gdy znasz etapów głównych, które każda aplikacja MVC przekazuje w potoku przetwarzania żądań.  
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image1.jpg)
- Widok szczegółów, w którym przedstawiono ćwiczenia w dół do szczegółów potoku przetwarzania żądań. Możesz porównać ogólny widok i widoku szczegółów, aby zobaczyć, jak szczegóły cykle są zbierane w poszczególnych etapów. [Pobierz plik PDF](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf) wyświetlić większym widoku.
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image2.jpg)
- Położenie i cel wszystkie metody możliwym do zastąpienia w [kontrolera](https://msdn.microsoft.com/en-us/library/system.web.mvc.controller.aspx) obiektu w potoku przetwarzania żądań. Może lub nie ma potrzeby zastąpienia żadnych jedną metodę, ale ważne jest, aby zrozumieć ich roli w cyklu życia aplikacji, dzięki czemu można napisać kod na etapie cyklu życia odpowiednie dla efektu, które mają.
- Diagramy skierowany w górę przedstawiający sposób wywoływania każdego typu filtru (uwierzytelniania, autoryzacji, działania i wyników).
- Połączyć artykuł lub blogu z każdego punktu odsetek w widoku szczegółów.


## <a name="next-steps"></a>Następne kroki

Ten dokument spełnia Twoje potrzeby? Prosimy o wyrażenie opinii. Jeśli masz pytania na cyklu życia ASP.NET MVC w aplikacji, [Stackoverflow](http://stackoverflow.com/help) i [ASP.NET MVC forum](https://forums.asp.net/1146.aspx) są doskonałe miejsca, aby zadać. Postępuj zgodnie z [mnie](https://twitter.com/Cephas_MSFT) w serwisie twitter, aby aktualizacje na Moje najnowsze samouczki.
