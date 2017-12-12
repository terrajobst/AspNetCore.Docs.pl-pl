---
uid: aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
title: Host OWIN w roli procesu roboczego platformy Azure | Dokumentacja firmy Microsoft
author: MikeWasson
description: "W tym samouczku przedstawiono sposób hosta samodzielnego OWIN roli procesu roboczego Microsoft Azure. Otwórz interfejs sieci Web dla platformy .NET (OWIN) definiuje abstrakcję między serwerem sieci web platformy .NET..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/11/2014
ms.topic: article
ms.assetid: 07aa855a-92ee-4d43-ba66-5bfd7de20ee6
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: 647514ae5a92b9d729179327fb97bd8005b0a4b2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="host-owin-in-an-azure-worker-role"></a>Host OWIN w roli procesu roboczego platformy Azure
====================
przez [Wasson Jan](https://github.com/MikeWasson)

> W tym samouczku przedstawiono sposób hosta samodzielnego OWIN roli procesu roboczego Microsoft Azure.
> 
> [Otwórz interfejs sieci Web dla platformy .NET](http://owin.org/) (OWIN) definiuje abstrakcję między serwerami sieci web .NET i aplikacji sieci web. OWIN oddziela aplikacji sieci web na serwerze, co sprawia, że OWIN idealne rozwiązanie w przypadku samodzielnej obsługi aplikacji sieci web w własnego procesu poza usług IIS — na przykład w roli procesu roboczego platformy Azure.
> 
> Z tego samouczka dowiesz się, jak własnym obsługi aplikacji OWIN wewnątrz roli procesu roboczego Microsoft Azure. Aby dowiedzieć się więcej na temat roli proces roboczy, zobacz [modeli wykonywania Azure](https://azure.microsoft.com/en-us/documentation/articles/fundamentals-application-models/#CloudServices).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Używane w samouczku wersje oprogramowania
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - [Zestaw Azure SDK dla platformy .NET w wersji 2.3](https://azure.microsoft.com/en-us/downloads/)
> - [Microsoft.Owin.Selfhost 2.1.0](http://www.nuget.org/packages/Microsoft.Owin.SelfHost/2.1.0)


## <a name="create-a-microsoft-azure-project"></a>Tworzenie projektu platformy Microsoft Azure

Uruchom program Visual Studio z uprawnieniami administratora. Aby debugować aplikację lokalnie, przy użyciu emulatora obliczeń platformy Azure, potrzebne są uprawnienia administratora.

Na **pliku** menu, kliknij przycisk **nowy**, następnie kliknij przycisk **projektu**. Z **zainstalowane szablony**, w obszarze Visual C#, kliknij przycisk **chmury** , a następnie kliknij przycisk **usługi w chmurze Windows Azure**. Nazwij projekt "AzureApp", a następnie kliknij przycisk **OK**.

[![](host-owin-in-an-azure-worker-role/_static/image2.png)](host-owin-in-an-azure-worker-role/_static/image1.png)

W **nowej usługi systemu Windows Azure Cloud** okna dialogowego, kliknij dwukrotnie **roli procesu roboczego**. Pozostaw nazwę domyślną ("WorkerRole1"). Ten krok powoduje dodanie roli procesu roboczego do rozwiązania. Kliknij przycisk **OK**.

[![](host-owin-in-an-azure-worker-role/_static/image4.png)](host-owin-in-an-azure-worker-role/_static/image3.png)

Rozwiązanie programu Visual Studio, która jest tworzona zawiera dwa projekty:

- &quot;AzureApp&quot; definiuje ról i konfiguracji aplikacji Azure.
- &quot;WorkerRole1&quot; zawiera kod roli proces roboczy.

Ogólnie rzecz biorąc aplikacja Azure może zawierać wiele ról, chociaż w tym samouczku korzysta z jedną rolę.

![](host-owin-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-owin-self-host-packages"></a>Dodawanie pakietów hosta samodzielnego OWIN

Z **narzędzia** menu, kliknij przycisk **Menedżer pakietów biblioteki**, następnie kliknij przycisk **Konsola Menedżera pakietów**.

W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:

[!code-console[Main](host-owin-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a>Dodawanie punktu końcowego HTTP

W Eksploratorze rozwiązań rozwiń projekt AzureApp. Rozwiń węzeł ról, kliknij prawym przyciskiem myszy WorkerRole1, a następnie wybierz **właściwości**.

![](host-owin-in-an-azure-worker-role/_static/image6.png)

Kliknij przycisk **punkty końcowe**, a następnie kliknij przycisk **Dodawanie punktu końcowego**.

W **protokołu** listy rozwijanej wybierz opcję "http". W **Port publiczny** i **Port prywatny**, wpisz 80. Numery portów mogą być różne. Port publiczny jest co używana przez klientów podczas wysyłania żądania do roli.

[![](host-owin-in-an-azure-worker-role/_static/image8.png)](host-owin-in-an-azure-worker-role/_static/image7.png)

## <a name="create-the-owin-startup-class"></a>Tworzenie klasy początkowej OWIN

W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt WorkerRole1 i wybierz **Dodaj** / **klasy** Aby dodać nową klasę. Nazwa klasy `Startup`.

Zamień wszystkie schematyczny kod poniżej:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample2.cs)]

`UseWelcomePage` — Metoda rozszerzenia dodaje prosty strony HTML do aplikacji, aby sprawdzić lokacji działa.

## <a name="start-the-owin-host"></a>Uruchom hosta OWIN

Otwórz plik WorkerRole.cs. Ta klasa definiuje kod, uruchamiany w momencie uruchamiania i zatrzymywania roli procesu roboczego.

Dodaj następującą instrukcję using:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample3.cs)]

Dodaj **IDisposable** członka `WorkerRole` klasy:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample4.cs)]

W `OnStart` metody, Dodaj następujący kod, aby uruchomić hosta:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample5.cs?highlight=5)]

**WebApp.Start** metoda uruchamia hosta OWIN. Nazwa `Startup` klasy jest parametrem typu metody. Według Konwencji wywoła hosta `Configure` metody tej klasy.

Zastąpienie `OnStop` zlikwidować  *\_aplikacji* wystąpienie:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample6.cs)]

W tym miejscu jest kompletny kod dla WorkerRole.cs:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample7.cs)]

Skompiluj rozwiązanie, a następnie naciśnij klawisz F5, aby uruchomić aplikację lokalnie w emulatorze obliczeniowe Azure. W zależności od ustawienia zapory konieczne może być Zezwalaj emulatora przez zaporę.

Emulator obliczeń przypisuje lokalny adres IP punktu końcowego. Adres IP można znaleźć, wyświetlając interfejs użytkownika emulatora obliczeń. Kliknij prawym przyciskiem myszy ikonę emulatora w pasku obszaru powiadomień zadań, a następnie wybierz **Pokaż interfejs użytkownika emulatora obliczeń**.

[![](host-owin-in-an-azure-worker-role/_static/image10.png)](host-owin-in-an-azure-worker-role/_static/image9.png)

Znajdowanie adresu IP w ramach wdrożenia usługi, wdrażania [id], szczegóły usługi. Otwórz przeglądarkę sieci web i przejdź do http://*adres*, gdzie *adres* to adres IP przypisany przez emulator obliczeń; na przykład `http://127.0.0.1:80`. Powinna zostać wyświetlona strona powitalna OWIN:

![](host-owin-in-an-azure-worker-role/_static/image11.png)

## <a name="deploy-to-azure"></a>Wdrażanie na platformie Azure

W tym kroku musi mieć konto platformy Azure. Jeśli nie masz jeszcze jeden, możesz utworzyć bezpłatne konto próbne w zaledwie kilka minut. Aby uzyskać więcej informacji, zobacz [bezpłatna wersja próbna programu Microsoft Azure](https://azure.microsoft.com/en-us/pricing/free-trial/?WT.mc_id=A261C142F).

W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt AzureApp. Wybierz **publikowania**.

![](host-owin-in-an-azure-worker-role/_static/image12.png)

Jeśli użytkownik nie jest zalogowany do konta platformy Azure, kliknij przycisk **logowania**.

[![](host-owin-in-an-azure-worker-role/_static/image14.png)](host-owin-in-an-azure-worker-role/_static/image13.png)

Po zarejestrowaniu w Wybierz subskrypcję i kliknij przycisk **dalej**.

[![](host-owin-in-an-azure-worker-role/_static/image16.png)](host-owin-in-an-azure-worker-role/_static/image15.png)

Wprowadź nazwę usługi w chmurze i wybierz region. Kliknij przycisk **Utwórz**.

![](host-owin-in-an-azure-worker-role/_static/image17.png)

Kliknij przycisk **publikowania**.

[![](host-owin-in-an-azure-worker-role/_static/image19.png)](host-owin-in-an-azure-worker-role/_static/image18.png)

Okno Dziennik aktywności platformy Azure będzie wyświetlany postęp wdrażania. Gdy aplikacja jest wdrożona, przejdź do `http://appname.cloudapp.net/`, gdzie *appname* to nazwa usługi w chmurze.

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Przegląd projektu Katana](an-overview-of-project-katana.md)
- [Projekt Katana w witrynie GitHub](https://github.com/aspnet/AspNetKatana/)
