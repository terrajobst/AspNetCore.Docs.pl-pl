---
uid: mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
title: Profilowanie i debugowanie aplikacji ASP.NET MVC za pomocą pakietów Glimpse | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: Możliwość wypróbowania to prężnie działającą rośnie z rodziny pakietów NuGet typu open source zapewniająca wydajność szczegółowe, debugowania i informacji diagnostycznych dla platformy ASP.NET...
ms.author: aspnetcontent
ms.date: 03/26/2015
ms.assetid: c205805f-efdd-4fa7-9616-f26eab180611
msc.legacyurl: /mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
msc.type: authoredcontent
ms.openlocfilehash: 4d044217d6c33594b39747a765165b8702338027
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808544"
---
<a name="profile-and-debug-your-aspnet-mvc-app-with-glimpse"></a>Profilowanie i debugowanie aplikacji ASP.NET MVC za pomocą pakietów Glimpse
====================
Przez [Rick Anderson](https://github.com/Rick-Anderson)

> Możliwość wypróbowania to prężnie działającą i rośnie z rodziny pakietów NuGet typu open source zapewniająca wydajność szczegółowe, debugowania i informacje diagnostyczne dla aplikacji ASP.NET. Jest proste do zainstalowania, lekki, niezwykle szybkie uzyskiwanie i przedstawia kluczowe metryki wydajności u dołu każdej strony. Umożliwia przechodzenie do aplikacji, gdy potrzebujesz dowiedzieć się, co się dzieje na serwerze. Możliwość wypróbowania zapewnia znacznie cenne informacje, zaleca się używania go w całym cyklu tworzenia oprogramowania, w tym środowisku testowym platformy Azure. Podczas [Fiddler](http://www.telerik.com/fiddler) i [narzędzia programistyczne F-12](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx) zapewniają po stronie klienta widok, możliwość wypróbowania zawiera szczegółowy widok z serwera. Ten samouczek koncentruje się na przy użyciu platformy ASP.NET MVC możliwość wypróbowania i EF pakietów, ale dostępnych jest wiele innych pakietów. Jeśli jest to możliwe I połączy się z odpowiednią [Poznaj docs](http://getglimpse.com/Docs/) co mogę pomóc zachować. Możliwość wypróbowania to projekt typu open source, zbyt można współtworzyć kod źródłowy i dokumentacja.


- [Instalowanie możliwość wypróbowania](#ig)
- [Włącz możliwość wypróbowania hosta lokalnego](#eg)
- [Na karcie osi czasu](#Time)
- [Wiązanie modelu](#mb)
- [Trasy](#route)
- [Używanie możliwość wypróbowania na platformie Azure](#da)
- [Dodatkowe zasoby](#addRes)

<a id="ig"></a>
## <a name="installing-glimpse"></a>Instalowanie możliwość wypróbowania

Możliwość wypróbowania można zainstalować z konsoli Menedżera pakietów NuGet lub **Zarządzaj pakietami NuGet** konsoli. Dla tej wersji demonstracyjnej dokonam instalacji pakietów Mvc5 i EF6:

![Zainstaluj możliwość wypróbowania z NuGet Dlg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image1.png)

Wyszukaj *Glimpse.EF*

![Glimpse.EF z NuGet install dlg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image2.png)

Wybierając **zainstalowanych pakietów**, zobaczysz możliwość wypróbowania modułów zależnych zainstalowane:

![Zainstalowane pakiety możliwość wypróbowania z DLg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image3.png)

Poniższe polecenia instalują MVC5 możliwość wypróbowania i EF6 modułów z konsoli Menedżera pakietów:

[!code-console[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample1.cmd)]

<a id="eg"></a>
## <a name="enable-glimpse-for-localhost"></a>Włącz możliwość wypróbowania hosta lokalnego

Przejdź do http://localhost:&lt; port #&gt;/glimpse.axd, a następnie kliknij przycisk <strong>Włącz możliwość wypróbowania</strong> przycisku.

![Możliwość wypróbowania axd strony](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image4.png)

W przypadku paska ulubionych wyświetlane możesz przeciągnij i upuść przyciski możliwość wypróbowania i dodać je jako bookmarklets:

![Programu Internet Explorer przy użyciu boookmarklets możliwość wypróbowania](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image5.png)

Możesz teraz przechodzić swojej aplikacji, a **głowic się wyświetlanie** (HUD) są widoczne u dołu strony.

![Strona Contact Manager z HUD](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image6.png)

[Strony HUD możliwość wypróbowania](http://getglimpse.com/Docs/Heads-up-Display) zawiera szczegółowe informacje o chronometrażu przedstawionych powyżej. Wyświetla dane HUD wydajności dyskretny kod może generować powiadomienia o problemie natychmiast — przed zagłębieniem się w cyklu testów. Kliknięcie &quot;g&quot; w prawym dolnym rogu powoduje wyświetlenie panelu możliwość wypróbowania:

![Możliwość wypróbowania panelu](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image7.png)

Na ilustracji powyżej [karcie wykonywanie](http://getglimpse.com/Docs/Execution-Tab) jest zaznaczone, które wyświetla szczegóły dotyczące działania i filtry w potoku. Możesz zobaczyć Moje [czasomierza filtr zatrzymać Obejrzyj](http://www.nuget.org/packages/StopWatch/) rozpoczynają się od 6 Etap potoku. Gdy mój czasomierza lekkie może zapewnić przydatne profilu/chronometrażu danych, jej chybień cały czas poświęcony na autoryzacji i renderowania widoku. Informacje o mojej zegara [profilowania i czas aplikacji ASP.NET MVC do usługi Azure](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx). [Karty](http://getglimpse.com/Docs/Tabs) strona zawiera linki do szczegółowych informacji na poszczególnych kartach.

<a id="Time"></a>
## <a name="the-timeline-tab"></a>Na karcie osi czasu

Po zmodyfikowaniu Tom Dykstra oczekujących [EF 6/MVC 5 samouczka](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) następującym kodem zmienić kontroler Instruktorzy:

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample2.cs?highlight=1,20-31)]

Powyższy kod pozwala mi przekazać w ciągu zapytania (`eager`) do formantu eager lub jawne ładowanie danych. Na poniższej ilustracji, używane jest jawne ładowanie i strona chronometrażu pokazuje każdej rejestracji załadowany w `Index` metody akcji:

![jawne ładowanie](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image8.png)

W poniższym kodzie określono eager i każdej rejestracji jest pobierana po `Index` nosi nazwę widoku:

![określono eager](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image9.png)

Możesz umieścić kursor segment czasu, aby uzyskać szczegółowe informacje o czasie:

![Po wskazaniu wskaźnikiem, aby wyświetlić szczegółowe informacje o czasach](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image10.png)

<a id="mb"></a>
## <a name="model-binding"></a>Wiązanie modelu

[Kartę powiązanie modelu](http://getglimpse.com/Docs/Model-Binding-Tab) zawiera mnóstwo informacji, które pomagają zrozumieć, jak zmiennych formularza są powiązane i dlaczego niektóre nie są powiązane zgodnie z oczekiwaniami. Obraz poniżej przedstawia **?** ikona, który można kliknąć, aby wyświetlić stronę pomocy możliwość wypróbowania tej funkcji.

![Poznaj widok wiązanie modelu](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image11.png)

<a id="route"></a>
## <a name="routes"></a>Trasy

 Na karcie możliwość wypróbowania trasy można pomoże Ci debugowania i poznanie routingu. Na poniższej ilustracji wybrano trasy produktu (i pokazuje, w kolorze zielonym, Konwencja możliwość wypróbowania). ![Wybrana nazwa produktu](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) tokeny ograniczenia, obszary i dane trasy są również wyświetlane. Zobacz [trasy możliwość wypróbowania](http://getglimpse.com/Docs/Routes-Tab) i [trasowanie atrybutów w programie ASP.NET MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) Aby uzyskać więcej informacji. 

<a id="da"></a>
## <a name="using-glimpse-on-azure"></a>Używanie możliwość wypróbowania na platformie Azure

Możliwość wypróbowania domyślne zasady zabezpieczeń zezwala tylko możliwość wypróbowania dane mają być wyświetlane z hosta lokalnego. Możesz zmienić te zasady zabezpieczeń, aby można było wyświetlić te dane na serwerze zdalnym (np. aplikacji sieci web na platformie Azure). Dla środowisk testowych na platformie Azure, Dodaj wyróżnione znacznika do końca *web.confg* plik, aby włączyć możliwość wypróbowania:

[!code-xml[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample3.xml?highlight=2-6)]

Dzięki tej zmianie samodzielnie każdemu użytkownikowi możliwość wyświetlenia danych możliwość wypróbowania w lokalizacji zdalnej. Rozważ dodanie znaczników powyżej profil publikowania, dzięki czemu jest ona wdrożona tylko stosowane podczas korzystania z tego profilu publikowania (na przykład Twoja Azure test proifle.) Aby ograniczyć możliwość wypróbowania dane, dodamy `canViewGlimpseData` roli oraz Zezwalanie tylko na użytkowników w tej roli, aby wyświetlić dane możliwość wypróbowania.

Usuń komentarze z *GlimpseSecurityPolicy.cs* pliku, a następnie zmień [IsInRole](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) wywołać z `Administrator` do `canViewGlimpseData` roli:

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample4.cs?highlight=6)]

> [!WARNING]
> Zabezpieczenia — zaawansowanych danych dostarczanych przez możliwość wypróbowania może narazić bezpieczeństwo aplikacji. Nie przeprowadzanych przez firmę Microsoft o możliwość wypróbowania inspekcji zabezpieczeń do użycia w aplikacji w produkcji.


Aby uzyskać informacje na temat dodawania ról, zobacz mój [wdrażanie aplikacji sieci web zabezpieczenia programu ASP.NET MVC 5 z członkostwa, uwierzytelnianiem OAuth i bazą danych SQL na platformie Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) samouczka.

<a id="addRes"></a>
## <a name="additional-resources"></a>Dodatkowe zasoby

- [Wdrażanie bezpiecznej aplikacji ASP.NET MVC 5 z członkostwa, uwierzytelnianiem OAuth i bazą danych SQL na platformie Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)
- [Poznaj konfiguracji](http://getglimpse.com/Docs/Configuration) -stronie dokumentu na temat konfigurowania kart, zasad wykonywania, rejestrowania i innych.
