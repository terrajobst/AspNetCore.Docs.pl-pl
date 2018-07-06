---
uid: web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
title: Hostowanie wzorca ASP.NET Web API 2 w roli procesu roboczego platformy Azure | Dokumentacja firmy Microsoft
author: MikeWasson
description: Ten samouczek przedstawia hostowanie interfejsu API sieci Web platformy ASP.NET w roli procesu roboczego platformy Azure, za pomocą OWIN na potrzeby samodzielnego hostowania strukturę interfejsu API sieci Web. Otwórz interfejs sieci Web dla platformy .NET (OWIN) de...
ms.author: aspnetcontent
ms.date: 04/02/2014
ms.assetid: 6980ee2e-d6b0-4a08-8fb6-ab96362dd0e3
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: c53256b8a72a377f51b9fbac7944657cb6d4c6e4
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37803853"
---
<a name="host-aspnet-web-api-2-in-an-azure-worker-role"></a>Hostowanie wzorca ASP.NET Web API 2 w roli procesu roboczego platformy Azure
====================
przez [Mike Wasson](https://github.com/MikeWasson)

> Ten samouczek przedstawia hostowanie interfejsu API sieci Web platformy ASP.NET w roli procesu roboczego platformy Azure, za pomocą OWIN na potrzeby samodzielnego hostowania strukturę interfejsu API sieci Web.
> 
> [Otwórz interfejs sieci Web dla platformy .NET](http://owin.org/) (OWIN) definiuje abstrakcję między serwerami sieci web platformy .NET i aplikacji sieci web. OWIN oddziela aplikacji sieci web na serwerze, co sprawia, że OWIN jest idealnym rozwiązaniem dla hostingu samodzielnego aplikacji sieci web w swoim własnym procesie, poza programem IIS — na przykład w roli procesu roboczego platformy Azure.
> 
> W tym samouczku użyjesz pakietu Microsoft.Owin.Host.HttpListener udostępniającej serwer HTTP używane na potrzeby samodzielnego hostowania aplikacji OWIN.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używanego w tym samouczku
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - Składnik Web API 2
> - [Zestaw Azure SDK dla platformy .NET 2.3](https://azure.microsoft.com/downloads/)


## <a name="create-a-microsoft-azure-project"></a>Tworzenie projektu platformy Microsoft Azure

Uruchom program Visual Studio z uprawnieniami administratora. Aby debugować aplikację lokalnie, za pomocą emulatora obliczeń platformy Azure potrzebne są uprawnienia administratora.

Na **pliku** menu, kliknij przycisk **New**, następnie kliknij przycisk **projektu**. Z **zainstalowane szablony**, w obszarze Visual C#, kliknij przycisk **chmury** a następnie kliknij przycisk **usłudze Windows Azure Cloud Services**. Nadaj projektowi nazwę "AzureApp", a następnie kliknij przycisk **OK**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image2.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image1.png)

W **nową usługę w chmurze Azure Windows** okno dialogowe, kliknij dwukrotnie **roli procesu roboczego**. Pozostaw nazwę domyślną ("WorkerRole1"). Ten krok powoduje dodanie roli procesu roboczego do rozwiązania. Kliknij przycisk **OK**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image4.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image3.png)

Rozwiązanie programu Visual Studio, który jest tworzony zawiera dwa projekty:

- &quot;AzureApp&quot; definiuje ról i konfiguracji dla aplikacji platformy Azure.
- &quot;WorkerRole1&quot; zawiera kod dla roli procesu roboczego.

Ogólnie rzecz biorąc aplikacją platformy Azure może zawierać wiele ról, chociaż w tym samouczku korzysta z jedną rolę.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-web-api-and-owin-packages"></a>Dodawanie interfejsu API sieci Web i pakietów OWIN

Z **narzędzia** menu, kliknij przycisk **Menedżer pakietów biblioteki**, następnie kliknij przycisk **Konsola Menedżera pakietów**.

W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:

[!code-console[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a>Dodaj punkt końcowy HTTP

W Eksploratorze rozwiązań rozwiń projekt AzureApp. Rozwiń węzeł ról, kliknij prawym przyciskiem myszy WorkerRole1, a następnie wybierz **właściwości**.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image6.png)

Kliknij przycisk **punktów końcowych**, a następnie kliknij przycisk **Dodaj punkt końcowy**.

W **protokołu** listy rozwijanej wybierz opcję "http". W **Port publiczny** i **Port prywatny**, wpisz 80. Te numery portów mogą być różne. Port publiczny jest o tym, co klienci używają wysłanie żądania do roli.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image8.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image7.png)

## <a name="configure-web-api-for-self-host"></a>Konfigurowanie internetowego interfejsu API dla hosta samodzielnego

W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt WorkerRole1, a następnie wybierz **Dodaj** / **klasy** Aby dodać nową klasę. Nazwa klasy `Startup`.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image9.png)

Zastąp cały kod standardowy, w tym pliku następujących czynności:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample2.cs)]

## <a name="add-a-web-api-controller"></a>Dodaj Kontroler interfejsu API sieci Web

Następnie Dodaj klasę kontrolera interfejsu API sieci Web. Kliknij prawym przyciskiem myszy projekt WorkerRole1, a następnie wybierz pozycję **Dodaj** / **klasy**. Nazwa klasy kontrolera testów. Zastąp cały kod standardowy, w tym pliku następujących czynności:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample3.cs)]

Dla uproszczenia tego kontrolera po prostu definiuje dwie metody GET, które zwracają zwykły tekst.

## <a name="start-the-owin-host"></a>Uruchom hosta OWIN

Otwórz plik WorkerRole.cs. Ta klasa definiuje kod, który jest uruchamiany podczas uruchamiania i zatrzymywania roli procesu roboczego.

Dodaj następującą instrukcję using:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample4.cs)]

Dodaj **interfejsu IDisposable** członka do `WorkerRole` klasy:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample5.cs)]

W `OnStart` metody, Dodaj następujący kod, aby uruchomić hosta:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample6.cs?highlight=5)]

**WebApp.Start** metoda uruchamia hosta OWIN. Nazwa `Startup` klasy jest parametrem typu w metodzie. Zgodnie z Konwencją, host będzie wywoływać `Configure` metody tej klasy.

Zastąp `OnStop` do rozporządzania  *\_aplikacji* wystąpienie:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample7.cs)]

Oto kompletny kod dla WorkerRole.cs:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample8.cs)]

Skompiluj rozwiązanie, a następnie naciśnij klawisz F5, aby uruchomić aplikację lokalnie w emulatorze obliczeń systemu Azure. W zależności od ustawień zapory konieczne może być Zezwalaj na emulator przez zaporę.

> [!NOTE]
> Jeśli wystąpi wyjątek, podobnie do poniższego, zobacz [ten wpis w blogu](https://blogs.msdn.com/b/praburaj/archive/2013/11/20/fileloadexception-on-microsoft-owin-when-running-on-worker-role.aspx) obejście tego problemu. "Nie można załadować pliku lub zestawu ' Microsoft.Owin, wersja = 2.0.2.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35" lub jednej z jego zależności. Definicja manifestu zestawu znajduje się niezgodna odwołanie do zestawu. (Wyjątek od HRESULT: 0x80131040) "


Emulator obliczeń przypisuje lokalny adres IP punktu końcowego. Adres IP można znaleźć, wyświetlając interfejs użytkownika emulatora obliczeń. Kliknij prawym przyciskiem myszy ikonę emulatora w zadaniu paska w obszarze powiadomień, a następnie wybierz pozycję **Pokaż interfejs użytkownika emulatora obliczeń**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image11.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image10.png)

Znajdowanie adresu IP, w ramach wdrożenia usług, wdrożenia [identyfikator], szczegóły usługi. Otwórz przeglądarkę internetową i przejdź do http://<em>adres</em>/test/1, gdzie <em>adres</em> jest adres IP przypisany przez emulator obliczeń; na przykład `http://127.0.0.1:80/test/1`. Powinny pojawić się odpowiedzi z kontrolera interfejsu API sieci Web:

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image12.png)

## <a name="deploy-to-azure"></a>Wdrażanie na platformie Azure

W tym kroku musisz mieć konto platformy Azure. Jeśli nie masz jeszcze jeden, możesz utworzyć bezpłatne konto próbne w zaledwie kilka minut. Aby uzyskać więcej informacji, zobacz [bezpłatnej wersji próbnej Microsoft Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).

W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt AzureApp. Wybierz **publikowania**.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image13.png)

Jeśli nie jest zalogowany do konta platformy Azure, kliknij przycisk **Sign In**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image15.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image14.png)

Po użytkownik jest zalogowany, wybierz subskrypcję i kliknij przycisk **dalej**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image17.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image16.png)

Wprowadź nazwę usługi w chmurze, a następnie wybierz region. Kliknij przycisk **Utwórz**.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image18.png)

Kliknij przycisk **publikowania**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image20.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image19.png)

Okno Dziennik aktywności platformy Azure Pokazuje postęp wdrażania. Gdy aplikacja jest wdrożona, przejdź do http://appname.cloudapp.net/test/1.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image21.png)

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Omówienie projektu Katana](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)
- [Projektu Katana w usłudze GitHub](https://github.com/aspnet/AspNetKatana)
