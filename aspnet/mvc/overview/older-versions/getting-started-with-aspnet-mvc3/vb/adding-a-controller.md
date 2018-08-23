---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-controller
title: Dodawanie kontrolera (VB) | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: Ta seria samouczków obejmuje podstawy tworzenia aplikacji sieci Web platformy ASP.NET MVC przy użyciu programu Microsoft Visual Web Developer 2010 Express Service Pack 1, czyli...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 741259e1-54ac-4f71-b4e8-2bd5560bb950
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: fc44a6825c08f2c04dbc1a1332d46771da70b149
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756916"
---
<a name="adding-a-controller-vb"></a>Dodawanie kontrolera (VB)
====================
Przez [Rick Anderson](https://github.com/Rick-Anderson)

> Ta seria samouczków obejmuje podstawy tworzenia aplikacji sieci Web platformy ASP.NET MVC przy użyciu Microsoft Visual Web Developer 2010 Express Service Pack 1, która jest bezpłatna wersja programu Microsoft Visual Studio. Przed rozpoczęciem upewnij się, że po zainstalowaniu wymagań wstępnych wymienionych poniżej. Możesz zainstalować wszystkie z nich, klikając poniższe łącze: [Instalatora platformy sieci Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternatywnie można indywidualnie zainstalować wymagania wstępne, korzystając z następujących linków:
> 
> - [Visual Studio Web Developer Express z dodatkiem SP1 wymagań wstępnych.](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Program ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(Obsługa środowiska uruchomieniowego i narzędzi)
> 
> Jeśli używasz programu Visual Studio 2010 zamiast Visual Web Developer 2010, zainstaluj wymagania wstępne, klikając poniższe łącze: [wymagania wstępne programu Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Projekt Visual Web Developer z kodem źródłowym VB.NET jest dostępna powiązany z tym tematem. [Pobierz wersję VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Jeśli wolisz C#, przełącz się do [wersji języka C#](../cs/adding-a-controller.md) po ukończeniu tego samouczka.


MVC to *model-view-controller*. MVC to wzorzec do tworzenia aplikacji w taki sposób, że każda część ma oddzielne odpowiedzialność za:

- Model: Dane dla aplikacji.
- Widoki: Pliki szablonów aplikacji użyje do dynamicznego generowania odpowiedzi HTML.
- Kontrolery: Klas, które obsługują przychodzące żądania adres URL do aplikacji, pobrać modelu danych, a następnie określ Przeglądanie szablonów, które renderowania odpowiedzi do klienta.

Firma Microsoft będzie obejmujące wszystkie te pojęcia, w tym samouczku a pokazują, jak ich używać do tworzenia aplikacji.

Utwórz nowy kontroler, klikając prawym przyciskiem myszy *kontrolerów* folderu w **Eksploratora rozwiązań** , a następnie wybierając **Dodaj kontroler**.

[![AddController](adding-a-controller/_static/image2.png "AddController")](adding-a-controller/_static/image1.png)

Nadaj nazwę nowemu kontrolerowi &quot;HelloWorldController&quot; i kliknij przycisk **Dodaj**.

[![2AddEmptyController](adding-a-controller/_static/image4.png "2AddEmptyController")](adding-a-controller/_static/image3.png)

Należy zauważyć w **Eksploratora rozwiązań** po prawej stronie tworzony dla Ciebie nowy plik o nazwie *HelloWorldController.cs* i że plik jest otwarty w środowisku IDE.

W nowym `public class HelloWorldController` zablokować, utworzyć dwie nowe metody, które wyglądają podobnie do poniższego kodu. Na przykład zostanie zwrócona ciąg HTML bezpośrednio z kontrolerem.

[!code-vb[Main](adding-a-controller/samples/sample1.vb)]

Nosi nazwę usługi kontrolera `HelloWorldController` i nową metodę o nazwie `Index`. Uruchom aplikację (naciśnij klawisz F5 lub Ctrl + F5). Po uruchomieniu przeglądarki Dołącz &quot;HelloWorld&quot; do ścieżki, w pasku adresu. (Na tym komputerze, ma ona `http://localhost:43246/HelloWorld`) przeglądarki będzie wyglądać jak poniższy zrzut ekranu. W powyższej metody kod zwrócony ciąg bezpośrednio. Powiedzieliśmy systemu, aby po prostu zwraca kod HTML i tak!

![](adding-a-controller/_static/image5.png)

ASP.NET MVC wywołuje innego kontrolera klasy (i różnych metod akcji w nich), w zależności od przychodzącego adresu URL. Domyślne mapowanie logikę używaną przez platformę ASP.NET MVC używa formatu to do kontrolowania, jaki kod jest wywoływana:

`/[Controller]/[ActionName]/[Parameters]`

Pierwszą część adresu URL określa klasę kontrolera do wykonania. Dlatego */HelloWorld* mapuje `HelloWorldController` klasy. Druga część adresu URL określa metody akcji w klasie, do wykonania. Dlatego */HelloWorld/indeks* spowodowałoby `Index` metody `HelloWorldController` klasy do wykonania. Należy zauważyć, że musimy odwiedź */HelloWorld* powyżej i `Index` metoda została użyta domyślna. Jest to spowodowane metodę o nazwie `Index` jest domyślną metodą, która zostanie wywołana na kontrolerze, jeśli nie jest jawnie określona.

Przejdź do `http://localhost:xxxx/HelloWorld/Welcome`. `Welcome` Metoda uruchamia i zwraca ciąg &quot;jest to metoda powitalnej akcji... &quot;. Domyślne mapowanie MVC to `/[Controller]/[ActionName]/[Parameters]`. Dla tego adresu URL jest kontroler `HelloWorld` i `Welcome` jest metodą. Firma Microsoft nie były używane `[Parameters]` wchodzi w skład jeszcze adresu URL.

![](adding-a-controller/_static/image6.png)

Zmodyfikujmy przykład nieco tak, że możemy przekazać niektóre informacje o parametrach w z adresu URL do kontrolera (na przykład */HelloWorld/Witaj? nazwa = Scott&amp;numtimes = 4*). Zmień swoje `Welcome` metodę, aby uwzględnić dwa parametry, jak pokazano poniżej. Należy pamiętać, aby wskazać, że używaliśmy funkcji opcjonalny parametr VB `numTimes` parametr powinien domyślnie na 1, jeśli nie przekazano żadnej wartości tego parametru.

[!code-vb[Main](adding-a-controller/samples/sample2.vb)]

Uruchom aplikację, a następnie przejdź do `http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4` **.** Możesz spróbować różne wartości `name` i `numtimes`. System automatycznie mapuje nazwane parametry ciągu zapytania w pasku adresu do parametrów w metodzie.

![](adding-a-controller/_static/image7.png)

W obu tych przykładach kontroler ma zostały wykonując VC część MVC — to praca widok i kontroler. Kontroler zwraca HTML bezpośrednio. Zwykle nie chcemy kontrolerów bezpośrednio, zwracając HTML, ponieważ staje się bardzo trudne kodu. Zamiast tego zazwyczaj użyjemy pliku szablonu osobny widok ułatwiający Generowanie odpowiedzi HTML. Przyjrzyjmy się jak możemy to zrobić.

> [!div class="step-by-step"]
> [Poprzednie](intro-to-aspnet-mvc-3.md)
> [dalej](adding-a-view.md)
