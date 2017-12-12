---
uid: mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs
title: "Omówienie routingu platformy ASP.NET MVC (C#) | Dokumentacja firmy Microsoft"
author: StephenWalther
description: "W tym samouczku Stephen Walther pokazuje sposób platforma ASP.NET MVC mapowania żądania przeglądarki akcji kontrolera."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: 5b39d2d5-4bf9-4d04-94c7-81b84dfeeb31
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs
msc.type: authoredcontent
ms.openlocfilehash: 714fd1939ffeba11b84a82e80193ecbbe4b12e09
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-routing-overview-c"></a>Omówienie routingu platformy ASP.NET MVC (C#)
====================
przez [Stephen Walther](https://github.com/StephenWalther)

> W tym samouczku Stephen Walther pokazuje sposób platforma ASP.NET MVC mapowania żądania przeglądarki akcji kontrolera.


W tym samouczku są wprowadzane do ważna cecha każda aplikacja platformy ASP.NET MVC wywołuje *routingu platformy ASP.NET*. Moduł routingu platformy ASP.NET jest odpowiedzialny za mapowanie przychodzących żądań przeglądarki do określonej akcji kontrolera MVC. Na koniec tego samouczka będzie zrozumiałe, jak tabela tras standardowe mapuje żądania do akcji kontrolera.

## <a name="using-the-default-route-table"></a>Korzystając z tabeli trasy domyślnej

Podczas tworzenia nowej aplikacji ASP.NET MVC aplikacji jest już skonfigurowana do używania routingu platformy ASP.NET. ASP.NET Routing jest skonfigurowana w dwóch miejscach.

Po pierwsze proces routingu platformy ASP.NET jest włączone w pliku konfiguracji aplikacji sieci Web (plik Web.config). Istnieją cztery sekcje w pliku konfiguracji, które mają zastosowanie do routingu: sekcja system.web.httpModules, sekcji system.web.httpHandlers sekcji system.webserver.modules i system.webserver.handlers sekcji. Uważaj, aby nie usunąć tych sekcji, ponieważ bez tych sekcji routingu przestanie działać.

Po drugie, a ważniejsze tabelę tras jest tworzony w pliku Global.asax aplikacji. Plik Global.asax to specjalny plik, który zawiera programy obsługi zdarzeń dla zdarzenia cyklu życia aplikacji ASP.NET. Tabela tras jest tworzona podczas zdarzenia uruchomić aplikacji.

Plik w 1 Lista zawiera domyślne pliku Global.asax aplikacji platformy ASP.NET MVC.

**Wyświetlanie listy 1 — Global.asax.cs**

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample1.cs)]

Gdy aplikacji MVC uruchomieniu aplikacji\_Start() metoda jest wywoływana. Ta metoda z kolei wywołuje metodę RegisterRoutes(). Metoda RegisterRoutes() tworzy tabelę tras.

Tabela tras domyślnych zawiera jedną trasę (o nazwie domyślnej). Trasa domyślna mapuje pierwszy segment adresu URL do nazwy kontrolera, drugi segment adresu URL do akcji kontrolera i segmentu trzeciego parametru o nazwie **identyfikator**.

Wyobraź sobie, wprowadź następujący adres URL na pasku adresu przeglądarki sieci web:

/ Głównej/indeks/3

Trasa domyślna mapuje ten adres URL do następujących parametrów:

- Kontroler = Home

- Akcja = indeks

- Identyfikator = 3

Gdy użytkownik żąda adresu URL /Home/indeks/3, jest wykonywany następujący kod:

HomeController.Index(3)

Trasa domyślna obejmuje ustawienia domyślne dla wszystkich trzech parametrów. Jeśli nie zostanie podane kontrolera, następnie parametr kontrolera domyślnie przyjmowana jest wartość **Home**. Jeśli akcja nie zostanie podane, parametr akcji domyślnie przyjmowana jest wartość **indeksu**. Ponadto jeśli identyfikator nie zostanie podane, parametru identyfikatora domyślnie pusty ciąg.

Oto kilka przykładów sposobu trasy domyślnej mapowania adresów URL do akcji kontrolera. Wyobraź sobie, wprowadź następujący adres URL na pasku adresu przeglądarki:

Domowych

Ze względu na wartości domyślne parametrów trasy domyślne wprowadzić ten adres URL spowoduje, że metoda indeks() klasy HomeController wyświetlania 2 do wywołania.

**Wyświetlanie listy 2 - HomeController.cs**

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample2.cs)]

Wyświetlanie 2 Klasa HomeController zawiera metodę o nazwie indeks(), który przyjmuje jeden parametr o nazwie identyfikatora. Adres URL /Home powoduje, że indeks() metodę można wywołać za pomocą ciągu pustego jako wartość parametru identyfikatora.

Ze względu na sposób, że struktura MVC wywołuje akcji kontrolera /Home adres URL zgodna metoda indeks() klasy HomeController w 3 wyświetlania.

**Wyświetlanie listy 3 - HomeController.cs (Akcja indeks parametru nie)**

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample3.cs)]

Metoda indeks() w 3 wyświetlania nie akceptuje żadnych parametrów. /Home adres URL spowoduje, że ta metoda indeks() do wywołania. Adres URL /Home/indeks/3 również wywołuje tę metodę (identyfikator zostanie zignorowana).

/Home adres URL zgodny metody indeks() klasy HomeController w listę 4.

**Wyświetlanie listy 4 - HomeController.cs (akcji indeksu z parametru dopuszczającego wartość null)**

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample4.cs)]

W przypadku wyświetlania 4 metoda indeks() ma jeden parametr. Ponieważ parametr ma wartość null parametru (może mieć wartości Null), indeks() można wywołać bez zgłaszania błędu.

Na koniec wywołania metody indeks() listę 5 z /Home adres URL powoduje zgłoszenie wyjątku od parametru identyfikatora *nie jest* parametru dopuszczającego wartość null. Jeśli próba wywołania metody indeks() otrzymasz błąd wyświetlane na rysunku 1.

**Wyświetlanie listy 5 - HomeController.cs (akcji indeksu z parametrem Id)**

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample5.cs)]


[![Wywoływanie akcji kontrolera, która oczekuje wartości parametru](asp-net-mvc-routing-overview-cs/_static/image1.jpg)](asp-net-mvc-routing-overview-cs/_static/image1.png)

**Rysunek 01**: wywoływania akcji kontrolera, która oczekuje wartości parametru ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](asp-net-mvc-routing-overview-cs/_static/image2.png))


Adres URL /Home/indeks/3 z drugiej strony, działa bez problemu z akcji kontrolera indeksu w listę 5. /Home/Index/3 żądania powoduje, że metoda indeks() ma być wywoływana z parametru identyfikatora, który ma wartość 3.

## <a name="summary"></a>Podsumowanie

Celem tego samouczka został dostarczają krótkie wprowadzenie do routingu platformy ASP.NET. Firma Microsoft zbadać tabeli trasy domyślnej, której można korzystać z nowej aplikacji ASP.NET MVC. Przedstawiono sposób trasy domyślnej mapowania adresów URL akcji kontrolera.

>[!div class="step-by-step"]
[Dalej](understanding-action-filters-cs.md)
