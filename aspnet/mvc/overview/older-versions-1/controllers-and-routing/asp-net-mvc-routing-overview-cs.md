---
uid: mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs
title: Omówienie routingu platformy ASP.NET MVC (C#) | Dokumentacja firmy Microsoft
author: StephenWalther
description: 'W tym samouczku Walther Autor: Stephen pokazuje, jak platforma ASP.NET MVC mapuje żądania przeglądarki do akcji kontrolera.'
ms.author: aspnetcontent
ms.date: 08/19/2008
ms.assetid: 5b39d2d5-4bf9-4d04-94c7-81b84dfeeb31
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs
msc.type: authoredcontent
ms.openlocfilehash: 2b43c3dca1587d79a60c5a89a451dda989403d56
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37819150"
---
<a name="aspnet-mvc-routing-overview-c"></a>Omówienie routingu platformy ASP.NET MVC (C#)
====================
przez [Walther Autor: Stephen](https://github.com/StephenWalther)

> W tym samouczku Walther Autor: Stephen pokazuje, jak platforma ASP.NET MVC mapuje żądania przeglądarki do akcji kontrolera.


W tym samouczku zostały wprowadzone do ważną funkcją każda aplikacja platformy ASP.NET MVC wywołuje *routingu platformy ASP.NET*. Moduł routingu platformy ASP.NET jest odpowiedzialny za mapowanie żądań przychodzących przeglądarki do określonej akcji kontrolera MVC. Do końca tego samouczka wiesz jak tabela tras standardowa mapuje żądania do akcji kontrolera.

## <a name="using-the-default-route-table"></a>Za pomocą tabeli tras domyślne

Podczas tworzenia nowej aplikacji platformy ASP.NET MVC, aplikacja jest już skonfigurowana do użycia routingu platformy ASP.NET. Routingu platformy ASP.NET jest skonfigurowana w dwóch miejscach.

Po pierwsze routingu platformy ASP.NET jest włączone w pliku konfiguracji sieci Web aplikacji (plik Web.config). Istnieją cztery sekcje w pliku konfiguracji, które są istotne dla routingu: sekcja system.web.httpModules, sekcja system.web.httpHandlers, system.webserver.modules i system.webserver.handlers sekcji. Uważaj, aby nie usunąć te sekcje, ponieważ bez tych sekcji routingu nie będą już działać.

Po drugie, i co ważniejsze tabelę tras jest tworzony w pliku Global.asax aplikacji. Plik Global.asax to plik specjalny, który zawiera programy obsługi zdarzeń dla zdarzenia cyklu życia aplikacji ASP.NET. Tabela tras jest tworzony podczas zdarzenia rozpoczęcia aplikacji.

Plik w ofercie 1 zawiera domyślny plik Global.asax dla aplikacji ASP.NET MVC.

**Wyświetlanie listy 1 — Global.asax.cs**

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample1.cs)]

Gdy aplikacja MVC pierwszym uruchomieniu, aplikacja\_wywołanie metody Start(). Ta metoda z kolei wywołuje metodę RegisterRoutes(). Metoda RegisterRoutes() tworzy tabelę tras.

Tabela routingu domyślnego zawiera jedną trasę (o nazwie domyślnej). Trasa domyślna mapuje pierwszy segment adresu URL do nazwy kontrolera, drugi segment adresu URL do akcji kontrolera i segmentu trzeciego parametru o nazwie **identyfikator**.

Wyobraź sobie, wprowadź następujący adres URL na pasku adresu przeglądarki sieci web:

/ Home/Index/3

Trasa domyślna mapuje ten adres URL do następujących parametrów:

- Kontroler = strona główna

- Akcja = indeks

- id = 3

W przypadku żądania /Home adresu URL/indeksu/3, poniższy kod jest wykonywany:

HomeController.Index(3)

Trasa domyślna obejmuje ustawienia domyślne dla wszystkich trzech parametrów. Jeśli nie podasz kontrolera, następnie kontrolera jest domyślnie wartość **Home**. Jeśli nie podasz akcję, parametr akcji wartość domyślna to wartość **indeksu**. Na koniec Jeśli nie podasz identyfikatora parametru identyfikatora wartość domyślna to ciąg pusty.

Spójrzmy na kilka przykładów jak trasa domyślna mapuje adresy URL do akcji kontrolera. Wyobraź sobie, wprowadź następujący adres URL w pasku adresu przeglądarki:

Domowych

Ze względu na wartości domyślne parametrów trasy domyślne wprowadzając ten adres URL spowoduje, że metoda indeks() klasy HomeController w ofercie 2 do wywołania.

**Wyświetlanie listy 2 - HomeController.cs**

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample2.cs)]

W ofercie 2 Klasa HomeController zawiera metodę o nazwie indeks(), który akceptuje pojedynczy parametr o nazwie identyfikatora. /Home adres URL spowoduje, że metoda indeks() nelze volat pusty ciąg jako wartość parametru identyfikatora.

Ze względu na sposób, że struktura MVC wywołuje akcji kontrolera /Home adresu URL dopasowuje metodę indeks() klasy HomeController w ofercie 3.

**Wyświetlanie listy 3 - HomeController.cs (indeks akcji w przypadku braku parametrów)**

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample3.cs)]

Metoda indeks() w ofercie 3 nie przyjmuje żadnych parametrów. /Home adres URL spowoduje, że ta metoda indeks() do wywołania. /Home adresu URL/indeksu/3 również wywołuje tę metodę (identyfikator jest ignorowany).

/Home adres URL zgodny metoda indeks() klasy HomeController w ofercie 4.

**Wyświetlanie listy 4 - HomeController.cs (Akcja indeksu za pomocą parametru dopuszczającego wartość null)**

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample4.cs)]

W ofercie 4 metoda indeks() ma jeden parametr liczby całkowitej. Ponieważ wartość parametru jest parametru dopuszczającego wartość null (może mieć wartości Null), indeks() mogą być wywoływane bez zgłaszania błędu.

Na koniec wywołania metody indeks() w ofercie 5 /Home adres URL powoduje wyjątek od parametru identyfikatora *nie* parametru dopuszczającego wartość null. Jeśli użytkownik podejmie próbę wywołania metody indeks() otrzymasz błąd wyświetlane na rysunku 1.

**Wyświetlanie listy 5 - HomeController.cs (Akcja indeksu za pomocą parametru Id)**

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample5.cs)]


[![Wywołanie akcji kontrolera, który oczekuje, że wartość parametru](asp-net-mvc-routing-overview-cs/_static/image1.jpg)](asp-net-mvc-routing-overview-cs/_static/image1.png)

**Rysunek 01**: wywołanie akcji kontrolera, który oczekuje, że wartość parametru ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](asp-net-mvc-routing-overview-cs/_static/image2.png))


Adres URL /Home/indeksu/3 z drugiej strony, nadaje tylko przy użyciu akcji kontrolera indeksu w ofercie 5. /Home/Index/3 żądanie spowoduje, że metoda indeks() nelze volat parametru Id, który ma wartość 3.

## <a name="summary"></a>Podsumowanie

Celem tego samouczka było udostępnić krótkie wprowadzenie do routingu platformy ASP.NET. Zbadaliśmy tabela routingu domyślnego, której można korzystać z nowej aplikacji platformy ASP.NET MVC. Pokazaliśmy ci, jak trasy domyślnej mapuje adresy URL do akcji kontrolera.

> [!div class="step-by-step"]
> [Next](understanding-action-filters-cs.md)
