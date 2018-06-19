---
uid: mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
title: Opis procesu wykonywania platformy ASP.NET MVC | Dokumentacja firmy Microsoft
author: microsoft
description: Dowiedz się, jak platforma ASP.NET MVC przetwarza żądanie przeglądarki krok po kroku.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: d1608db3-660d-4079-8c15-f452ff01f1db
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
msc.type: authoredcontent
ms.openlocfilehash: 5837c6e49709d6b86ee52cd88ffd4759c1850544
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
ms.locfileid: "26564524"
---
<a name="understanding-the-aspnet-mvc-execution-process"></a>Opis procesu wykonywania platformy ASP.NET MVC
====================
przez [firmy Microsoft](https://github.com/microsoft)

> Dowiedz się, jak platforma ASP.NET MVC przetwarza żądanie przeglądarki krok po kroku.


Najpierw przekazywania żądań do aplikacji sieci Web opartych na platformie ASP.NET MVC **UrlRoutingModule** obiektu, który jest moduł protokołu HTTP. Ten moduł analizuje żądania i wykonuje wybór trasy. **UrlRoutingModule** obiektu wybiera pierwszy obiekt trasy odpowiadający bieżącego żądania. (Obiekt trasy jest klasa implementująca **RouteBase**, i jest zwykle wystąpienia **trasy** klasy.) Jeśli trasy nie są zgodne, **UrlRoutingModule** obiekt nie robi nic i umożliwia przełączyć się na regularne żądania ASP.NET i IIS przetwarzania żądania.

Z wybranego **trasy** obiektu **UrlRoutingModule** uzyskuje obiekt **IRouteHandler** obiektu, z którym skojarzony jest **trasy**obiektu. Zazwyczaj w aplikacji MVC, będzie to wystąpienie **MvcRouteHandler**. **IRouteHandler** tworzy wystąpienie **IHttpHandler** obiektu i przekazuje je **IHttpContext** obiektu. Domyślnie **IHttpHandler** wystąpienie jest MVC **MvcHandler** obiektu. **MvcHandler** obiektu następnie wybiera kontrolera, który ostatecznie obsłuży żądanie.

> [!NOTE]
> Po uruchomieniu aplikacji sieci Web programu ASP.NET MVC w usługach IIS 7.0, bez rozszerzenia nazwy pliku jest wymagana dla projektów MVC. W usługach IIS 6.0, program obsługi wymaga jednak mapowania rozszerzenia nazwy pliku MVC ASP.NET ISAPI DLL.


Moduł i obsługi są punkty wejścia do platformę ASP.NET MVC. Wykonują następujące czynności:

- Wybierz odpowiedni kontroler w aplikacji sieci Web MVC.
- Uzyskaj wystąpienia określonego kontrolera.
- Wywołanie kontrolera **Execute** metody.

Poniższa lista zawiera etapy wykonywania dla projektu sieci Web MVC:

- Odbieranie pierwsze żądanie dla aplikacji 

    - W pliku Global.asax **trasy** obiekty są dodawane do **stan** obiektu.
- Wykonaj routingu 

    - **UrlRoutingModule** modułu używa pierwszego dopasowywania **trasy** obiektu w **stan** kolekcji, aby utworzyć **RouteData** obiekt, który następnie używane w celu utworzenia **RequestContext** (**IHttpContext**) obiektu.
- Utwórz program obsługi żądania MVC 

    - **MvcRouteHandler** obiektu tworzy wystąpienie **MvcHandler** klasy i przekazuje je **RequestContext** wystąpienia.
- Tworzenie kontrolera 

    - **MvcHandler** obiekt używa **RequestContext** wystąpienia, aby zidentyfikować **IControllerFactory** obiektu (zazwyczaj wystąpienia  **DefaultControllerFactory** klasy) można utworzyć wystąpienia kontrolera z.
- Kontroler EXECUTE - **MvcHandler** wystąpienia wywołuje kontroler s **Execute** — metoda. |
- Wywołaj akcję 

    - Większość kontrolerów dziedziczyć **kontrolera** klasy podstawowej. Kontrolerów z tym **ControllerActionInvoker** obiekt, który jest skojarzony z kontrolerem określa metodę akcji klasy kontrolera do wywołania, a następnie wywołuje tę metodę.
- Wynik wykonania 

    - Metoda typowych akcji może odbierać dane wejściowe użytkownika, przygotowanie danych właściwą odpowiedź, a następnie wykonującą wynik zwróciła typ wyniku. Typy wbudowane wyników, które mogą być wykonywane są następujące: **ViewResult** (który renderuje widok i jest typu wyniku większość — często używane), **RedirectToRouteResult**,  **RedirectResult**, **ContentResult**, **JsonResult**, i **EmptyResult**.
