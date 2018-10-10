---
uid: aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
title: Hostowanie OWIN w roli procesu roboczego platformy Azure | Dokumentacja firmy Microsoft
author: MikeWasson
description: W tym samouczku pokazano, jak na potrzeby samodzielnego hostowania OWIN w roli procesu roboczego Microsoft Azure. Open Web Interface for .NET (OWIN) definiuje abstrakcję między serwerem sieci web platformy .NET...
ms.author: riande
ms.date: 04/11/2014
ms.assetid: 07aa855a-92ee-4d43-ba66-5bfd7de20ee6
msc.legacyurl: /aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: dbf0964695dd2592d063b05c0778923edffe8e2e
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/10/2018
ms.locfileid: "48910779"
---
<a name="host-owin-in-an-azure-worker-role"></a>Hostowanie OWIN w roli procesu roboczego platformy Azure
====================
przez [Mike Wasson](https://github.com/MikeWasson)

> W tym samouczku pokazano, jak na potrzeby samodzielnego hostowania OWIN w roli procesu roboczego Microsoft Azure.
>
> [Otwórz interfejs sieci Web dla platformy .NET](http://owin.org/) (OWIN) definiuje abstrakcję między serwerami sieci web platformy .NET i aplikacji sieci web. OWIN oddziela aplikacji sieci web na serwerze, co sprawia, że OWIN jest idealnym rozwiązaniem dla hostingu samodzielnego aplikacji sieci web w swoim własnym procesie, poza programem IIS — na przykład w roli procesu roboczego platformy Azure.
>
> W tym samouczku dowiesz się, jak na potrzeby samodzielnego hostowania aplikacji OWIN w roli procesu roboczego Microsoft Azure. Aby dowiedzieć się więcej o rolach procesów roboczych, zobacz [modele wykonywania Azure](https://azure.microsoft.com/documentation/articles/fundamentals-application-models/#CloudServices).
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używanego w tym samouczku
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - [Zestaw Azure SDK dla platformy .NET 2.3](https://azure.microsoft.com/downloads/)
> - [Microsoft.Owin.Selfhost 2.1.0](http://www.nuget.org/packages/Microsoft.Owin.SelfHost/2.1.0)


## <a name="create-a-microsoft-azure-project"></a>Tworzenie projektu platformy Microsoft Azure

Uruchom program Visual Studio z uprawnieniami administratora. Aby debugować aplikację lokalnie, za pomocą emulatora obliczeń platformy Azure potrzebne są uprawnienia administratora.

Na **pliku** menu, kliknij przycisk **New**, następnie kliknij przycisk **projektu**. Z **zainstalowane szablony**, w obszarze Visual C#, kliknij przycisk **chmury** a następnie kliknij przycisk **usłudze Windows Azure Cloud Services**. Nadaj projektowi nazwę "AzureApp", a następnie kliknij przycisk **OK**.

[![](host-owin-in-an-azure-worker-role/_static/image2.png)](host-owin-in-an-azure-worker-role/_static/image1.png)

W **nową usługę w chmurze Azure Windows** okno dialogowe, kliknij dwukrotnie **roli procesu roboczego**. Pozostaw nazwę domyślną ("WorkerRole1"). Ten krok powoduje dodanie roli procesu roboczego do rozwiązania. Kliknij przycisk **OK**.

[![](host-owin-in-an-azure-worker-role/_static/image4.png)](host-owin-in-an-azure-worker-role/_static/image3.png)

Rozwiązanie programu Visual Studio, który jest tworzony zawiera dwa projekty:

- &quot;AzureApp&quot; definiuje ról i konfiguracji dla aplikacji platformy Azure.
- &quot;WorkerRole1&quot; zawiera kod dla roli procesu roboczego.

Ogólnie rzecz biorąc aplikacją platformy Azure może zawierać wiele ról, chociaż w tym samouczku korzysta z jedną rolę.

![](host-owin-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-owin-self-host-packages"></a>Dodaj pakiety hosta samodzielnego OWIN

Z **narzędzia** menu, kliknij przycisk **Menedżera pakietów NuGet**, następnie kliknij przycisk **Konsola Menedżera pakietów**.

W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:

[!code-console[Main](host-owin-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a>Dodaj punkt końcowy HTTP

W Eksploratorze rozwiązań rozwiń projekt AzureApp. Rozwiń węzeł ról, kliknij prawym przyciskiem myszy WorkerRole1, a następnie wybierz **właściwości**.

![](host-owin-in-an-azure-worker-role/_static/image6.png)

Kliknij przycisk **punktów końcowych**, a następnie kliknij przycisk **Dodaj punkt końcowy**.

W **protokołu** listy rozwijanej wybierz opcję "http". W **Port publiczny** i **Port prywatny**, wpisz 80. Te numery portów mogą być różne. Port publiczny jest o tym, co klienci używają wysłanie żądania do roli.

[![](host-owin-in-an-azure-worker-role/_static/image8.png)](host-owin-in-an-azure-worker-role/_static/image7.png)

## <a name="create-the-owin-startup-class"></a>Tworzenie klasy początkowej OWIN

W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt WorkerRole1, a następnie wybierz **Dodaj** / **klasy** Aby dodać nową klasę. Nazwa klasy `Startup`.

Zastąp cały kod standardowy następujących czynności:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample2.cs)]

`UseWelcomePage` — Metoda rozszerzenia dodaje prostą stronę HTML do aplikacji, aby sprawdzić lokacji działa.

## <a name="start-the-owin-host"></a>Uruchom hosta OWIN

Otwórz plik WorkerRole.cs. Ta klasa definiuje kod, który jest uruchamiany podczas uruchamiania i zatrzymywania roli procesu roboczego.

Dodaj następującą instrukcję using:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample3.cs)]

Dodaj **interfejsu IDisposable** członka do `WorkerRole` klasy:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample4.cs)]

W `OnStart` metody, Dodaj następujący kod, aby uruchomić hosta:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample5.cs?highlight=5)]

**WebApp.Start** metoda uruchamia hosta OWIN. Nazwa `Startup` klasy jest parametrem typu w metodzie. Zgodnie z Konwencją, host będzie wywoływać `Configure` metody tej klasy.

Zastąp `OnStop` do rozporządzania  *\_aplikacji* wystąpienie:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample6.cs)]

Oto kompletny kod dla WorkerRole.cs:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample7.cs)]

Skompiluj rozwiązanie, a następnie naciśnij klawisz F5, aby uruchomić aplikację lokalnie w emulatorze obliczeń systemu Azure. W zależności od ustawień zapory konieczne może być Zezwalaj na emulator przez zaporę.

Emulator obliczeń przypisuje lokalny adres IP punktu końcowego. Adres IP można znaleźć, wyświetlając interfejs użytkownika emulatora obliczeń. Kliknij prawym przyciskiem myszy ikonę emulatora w zadaniu paska w obszarze powiadomień, a następnie wybierz pozycję **Pokaż interfejs użytkownika emulatora obliczeń**.

[![](host-owin-in-an-azure-worker-role/_static/image10.png)](host-owin-in-an-azure-worker-role/_static/image9.png)

Znajdowanie adresu IP, w ramach wdrożenia usług, wdrożenia [identyfikator], szczegóły usługi. Otwórz przeglądarkę internetową i przejdź do http://<em>adres</em>, gdzie <em>adres</em> jest adres IP przypisany przez emulator obliczeń; na przykład `http://127.0.0.1:80`. Powinny pojawić się Strona powitalna OWIN:

![](host-owin-in-an-azure-worker-role/_static/image11.png)

## <a name="deploy-to-azure"></a>Wdrażanie na platformie Azure

W tym kroku musisz mieć konto platformy Azure. Jeśli nie masz jeszcze jeden, możesz utworzyć bezpłatne konto próbne w zaledwie kilka minut. Aby uzyskać więcej informacji, zobacz [bezpłatnej wersji próbnej Microsoft Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).

W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt AzureApp. Wybierz **publikowania**.

![](host-owin-in-an-azure-worker-role/_static/image12.png)

Jeśli nie jest zalogowany do konta platformy Azure, kliknij przycisk **Sign In**.

[![](host-owin-in-an-azure-worker-role/_static/image14.png)](host-owin-in-an-azure-worker-role/_static/image13.png)

Po użytkownik jest zalogowany, wybierz subskrypcję i kliknij przycisk **dalej**.

[![](host-owin-in-an-azure-worker-role/_static/image16.png)](host-owin-in-an-azure-worker-role/_static/image15.png)

Wprowadź nazwę usługi w chmurze, a następnie wybierz region. Kliknij przycisk **Utwórz**.

![](host-owin-in-an-azure-worker-role/_static/image17.png)

Kliknij przycisk **publikowania**.

[![](host-owin-in-an-azure-worker-role/_static/image19.png)](host-owin-in-an-azure-worker-role/_static/image18.png)

Okno Dziennik aktywności platformy Azure Pokazuje postęp wdrażania. Gdy aplikacja jest wdrożona, przejdź do `http://appname.cloudapp.net/`, gdzie *appname* to nazwa usługi w chmurze.

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Omówienie projektu Katana](an-overview-of-project-katana.md)
- [Projektu Katana w usłudze GitHub](https://github.com/aspnet/AspNetKatana/)
