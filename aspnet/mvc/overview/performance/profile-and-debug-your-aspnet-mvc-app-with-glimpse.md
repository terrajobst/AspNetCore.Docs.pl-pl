---
uid: mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
title: Profile i debugowania aplikacji ASP.NET MVC za pomocą Glimpse | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: Glimpse jest thriving i powiększania rodziny pakietów NuGet typu open source, który zapewnia szczegółowe wydajność, debugowanie i informacji diagnostycznych dla platformy ASP.NET...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/26/2015
ms.topic: article
ms.assetid: c205805f-efdd-4fa7-9616-f26eab180611
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
msc.type: authoredcontent
ms.openlocfilehash: 6ac23256c57116de81c7bf690d5ce743301c75ce
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872978"
---
<a name="profile-and-debug-your-aspnet-mvc-app-with-glimpse"></a>Profile i debugowania aplikacji ASP.NET MVC za pomocą Glimpse
====================
przez [Rick Anderson](https://github.com/Rick-Anderson)

> Glimpse jest thriving i powiększania rodziny pakietów NuGet typu open source, który zapewnia szczegółowe wydajność, debugowanie i informacje diagnostyczne dotyczące aplikacji ASP.NET. Jest prosta do zainstalowania, lekkie, niezwykle szybka i przedstawia kluczowe metryki wydajności u dołu każdej strony. Umożliwia przechodzenie w swojej aplikacji, gdy chcesz dowiedzieć się, co dzieje się na serwerze. Glimpse zapewnia dużo cenne informacje, które firma Microsoft zaleca się, że używasz przez cały cykl programowania, w tym środowiska testowego Azure. Podczas [Fiddler](http://www.telerik.com/fiddler) i [narzędzi programistycznych F-12](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx) Podaj po stronie klienta widoku Glimpse zawiera widok szczegółowy z serwera. Ten samouczek koncentruje się na użyciu Glimpse ASP.NET MVC i EF pakietów, ale dostępnych jest wiele innych pakietów. W miarę możliwości I połączy się z odpowiednim [Glimpse docs](http://getglimpse.com/Docs/) którego pomoc I Obsługa. Glimpse jest projekt typu open source, zbyt można współtworzyć kod źródłowy i dokumentacja.


- [Instalowanie Glimpse](#ig)
- [Włącz Glimpse hosta lokalnego](#eg)
- [Na karcie osi czasu](#Time)
- [Wiązanie modelu](#mb)
- [Trasy](#route)
- [Przy użyciu Glimpse na platformie Azure](#da)
- [Dodatkowe zasoby](#addRes)

<a id="ig"></a>
## <a name="installing-glimpse"></a>Instalowanie Glimpse

Glimpse można zainstalować z konsoli Menedżera pakietów NuGet lub **Zarządzaj pakietami NuGet** konsoli. Dla tego pokazu I Zainstaluj pakiety Mvc5 i EF6:

![Zainstaluj Glimpse z okno NuGet](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image1.png)

Wyszukaj *Glimpse.EF*

![Glimpse.EF z NuGet install dlg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image2.png)

Wybierając **zainstalowane pakiety**, widzą modułów zależnych Glimpse zainstalowane:

![Zainstalowane pakiety Glimpse z okno dialogowe](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image3.png)

Następujące polecenia zainstalować moduły Glimpse MVC5 i EF6 z konsoli Menedżera pakietów:

[!code-console[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample1.cmd)]

<a id="eg"></a>
## <a name="enable-glimpse-for-localhost"></a>Włącz Glimpse hosta lokalnego

Przejdź do http://localhost: &lt;portu #&gt;/glimpse.axd, a następnie kliknij przycisk <strong>Włącz Glimpse</strong> przycisku.

![Strona axd glimpse](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image4.png)

Masz paska ulubionych wyświetlane można przeciągnij i upuść przycisków Glimpse i dodać je jako bookmarklets:

![IE z Glimpse boookmarklets](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image5.png)

Teraz można przechodzić aplikacji oraz **głowic zapasowej wyświetlania** (HUD) jest wyświetlany w dolnej części strony.

![Strona Menedżer kontakt z HUD](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image6.png)

[Glimpse HUD strony](http://getglimpse.com/Docs/Heads-up-Display) szczegóły informacji przedstawionych powyżej. Wyświetla dane HUD wydajności dyskretny kod może powiadomić o problemie natychmiast — przed uzyskanie cyklu testu. Kliknięcie &quot;g&quot; w prawym dolnym rogu wyświetlenie panelu Glimpse:

![Glimpse panel](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image7.png)

Na ilustracji powyżej [karcie wykonywanie](http://getglimpse.com/Docs/Execution-Tab) jest zaznaczone, który zawiera szczegóły dotyczące działań i filtry w potoku. Widać Mój [czujki zatrzymać czasomierza filtru](http://www.nuget.org/packages/StopWatch/) start na etapie 6 potoku. Gdy mój czasomierza lekkie zapewniają przydatne profilu/chronometrażu danych, jej chybień cały czas spędzony w autoryzacji i renderowania widoku. Informacje o Moje czasomierza po [profilu i czas aplikacji ASP.NET MVC do Azure](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx). [Karty](http://getglimpse.com/Docs/Tabs) strona zawiera linki do szczegółowych informacji na poszczególnych kartach.

<a id="Time"></a>
## <a name="the-timeline-tab"></a>Na karcie osi czasu

Po zmodyfikowaniu Tomasz Dykstra oczekujących [EF 6/MVC 5 samouczka](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) następującym kodem zmienić kontroler instruktorów:

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample2.cs?highlight=1,20-31)]

Powyższy kod umożliwia mi do przekazania w ciągu zapytania (`eager`) do formantu wczesny lub jawnego ładowania danych. Na poniższej ilustracji, jest używany podczas jawnego ładowania i strona chronometrażu przedstawia każdego rejestrowania załadowany w `Index` metody akcji:

![Ładowanie jawne](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image8.png)

W poniższym kodzie określono eager i każdej rejestracji jest pobierana po `Index` nosi nazwę widoku:

![określono eager](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image9.png)

Można umieść kursor nad segment czasu można uzyskać informacji o chronometrażu:

![aktywowany, aby zobaczyć chronometrażu](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image10.png)

<a id="mb"></a>
## <a name="model-binding"></a>Wiązanie modelu

[Kartę powiązanie modelu](http://getglimpse.com/Docs/Model-Binding-Tab) zawiera szereg informacji, aby lepiej zrozumieć, jak są powiązane zmiennych formularza i dlaczego niektóre nie są powiązane zgodnie z oczekiwaniami. Obraz poniżej przedstawia **?** ikona, które można kliknąć można wyświetlić strony pomocy glimpse tej funkcji.

![glimpse widoku powiązanie modelu](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image11.png)

<a id="route"></a>
## <a name="routes"></a>Trasy

 Na karcie Glimpse trasy można pomoże Ci debugowania i zrozumieć ich routingu. Na poniższej ilustracji wybór trasy produktu (i będzie wyświetlana na zielono, Konwencji Glimpse). ![Wybrana nazwa produktu](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) wyświetlana jest również tokeny ograniczenia, obszarów i dane trasy. Zobacz [tras Glimpse](http://getglimpse.com/Docs/Routes-Tab) i [atrybutu routingu na platformie ASP.NET MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) Aby uzyskać więcej informacji. 

<a id="da"></a>
## <a name="using-glimpse-on-azure"></a>Przy użyciu Glimpse na platformie Azure

Domyślnych zasad zabezpieczeń Glimpse umożliwia tylko Glimpse danych mają być wyświetlane z hosta lokalnego. Ta zasada zabezpieczeń można zmienić, aby wyświetlić te dane na serwerze zdalnym (np. aplikacji sieci web na platformie Azure). Dla środowisk testowych na platformie Azure, Dodaj wyróżnione znacznika do końca *web.confg* plik w celu włączenia Glimpse:

[!code-xml[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample3.xml?highlight=2-6)]

Z tej samej zmiany dowolnego użytkownika można wyświetlić danych Glimpse lokacji zdalnej. Rozważ dodanie znaczników powyżej profil publikowania, więc ona wdrożona tylko stosowane podczas korzystania z tego profilu publikowania (na przykład sieci Azure testu proifle.) Aby ograniczyć dane Glimpse, dodamy `canViewGlimpseData` roli i umożliwić tylko przez użytkowników w tej roli, aby wyświetlić dane Glimpse.

Usuń komentarze z *GlimpseSecurityPolicy.cs* plików i zmień [IsInRole](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) wywoływać z `Administrator` do `canViewGlimpseData` roli:

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample4.cs?highlight=6)]

> [!WARNING]
> Zabezpieczenia — obszerne dane przez Glimpse może spowodować narażenie zabezpieczeń aplikacji. Firma Microsoft nie przeprowadziła inspekcji zabezpieczeń programu Glimpse do użycia w produkcji aplikacji.


Aby uzyskać informacje na temat dodawania ról, zobacz Moje [wdrażanie aplikacji sieci web zabezpieczenia programu ASP.NET MVC 5 z członkostwa, OAuth i bazy danych SQL Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) samouczka.

<a id="addRes"></a>
## <a name="additional-resources"></a>Dodatkowe zasoby

- [Wdrażanie aplikacji bezpiecznego platformy ASP.NET MVC 5 z członkostwa, OAuth i bazy danych SQL Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)
- [Glimpse konfiguracji](http://getglimpse.com/Docs/Configuration) -Doc strony na temat konfigurowania kart, zasad wykonywania, rejestrowania i inne.
