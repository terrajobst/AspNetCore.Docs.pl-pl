---
uid: web-forms/overview/deployment/visual-studio-web-deployment/setting-folder-permissions
title: 'Wdrażanie sieci Web ASP.NET przy użyciu programu Visual Studio: Ustawianie uprawnień folderu | Dokumentacja firmy Microsoft'
author: tdykstra
description: Ta seria samouczek pokazuje, jak wdrożyć platformy ASP.NET (publikowanie) aplikacji do aplikacji sieci Web usługi aplikacji Azure lub innego dostawcy hostingu sieci web przez używane...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 9715a121-fa55-4f1b-a5d2-fb3f6cd8be8f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/setting-folder-permissions
msc.type: authoredcontent
ms.openlocfilehash: 7efe267975835e889950983126088f1b637c28fb
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "30890401"
---
<a name="aspnet-web-deployment-using-visual-studio-setting-folder-permissions"></a>Wdrażanie sieci Web ASP.NET przy użyciu programu Visual Studio: ustawienie uprawnień do folderu
====================
Przez [Dykstra niestandardowy](https://github.com/tdykstra)

[Pobierz początkowego projektu](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Ta seria samouczek pokazuje, jak wdrożyć platformy ASP.NET (publikowanie) aplikacji do aplikacji sieci Web usługi aplikacji Azure lub innego dostawcy hostingu sieci web za pomocą programu Visual Studio 2012 lub Visual Studio 2010. Aby uzyskać informacje dotyczące serii, zobacz [pierwszy samouczek z tej serii](introduction.md).


## <a name="overview"></a>Omówienie

W tym samouczku, należy ustawić uprawnienia folderu do *Elmah* folder w sieci web, wdrożonych lokacji, dzięki czemu aplikacja może tworzyć pliki dziennika w tym folderze.

Podczas testowania aplikacji sieci web w programie Visual Studio przy użyciu serwera wdrożeniowego programu Visual Studio (Cassini) lub usługi IIS Express, aplikacja zostanie uruchomiona w ramach Twojej tożsamości. Prawdopodobnie są uprawnienia administratora na komputerze deweloperskim i mieć pełne uprawnienia do podejmować żadnych działań, plików w folderze. Ale po uruchomieniu aplikacji w środowisku usług IIS, działa z tożsamością zdefiniowane dla puli aplikacji, przypisane do lokacji. Jest to zazwyczaj konto zdefiniowane przez system, który ma ograniczone uprawnienia. Domyślnie przeczytał i uprawnienia do wykonywania dla aplikacji sieci web plików i folderów, ale nie ma dostępu do zapisu.

Staje się on problemu tworzy aplikacji lub aktualizacji plików, co jest typowe w aplikacji sieci web. W aplikacji Contoso University Elmah tworzy pliki XML z *Elmah* folderu w celu zapisania informacji o błędach. Nawet jeśli nie używasz przypominać Elmah, witryny zezwolić użytkownikom na przekazywanie plików lub innych zadań, które zapisu danych do folderu w witrynie sieci.

Przypomnienie: Jeśli coś nie działa podczas wykonywania kroków samouczka wyświetlony komunikat o błędzie, należy sprawdzić [Rozwiązywanie problemów z strony](troubleshooting.md).

## <a name="test-error-logging-and-reporting"></a>Rejestrowanie błędów testu i raportowanie

Aby zobaczyć, jak aplikacja nie działa prawidłowo w usługach IIS (chociaż jak testach w programie Visual Studio), może spowodować błąd, który zwykle rejestrowany przez Elmah, a następnie otwórz dziennik błędów Elmah, aby wyświetlić szczegóły. Jeśli Elmah nie może utworzyć pliku XML i zapisać szczegóły błędu, zostanie wyświetlony raport o błędach puste.

Otwórz przeglądarkę i przejdź do `http://localhost/ContosoUniversity`, a następnie żądają nieprawidłowy adres URL, takie jak *Studentsxxx.aspx*. Zostanie wyświetlona strona błędu generowanych przez system, zamiast *GenericErrorPage.aspx* strony, ponieważ `customErrors` ustawienia w pliku Web.config jest "RemoteOnly" i są uruchomione usługi IIS lokalnie:

![Strona błędu 404 protokołu HTTP](setting-folder-permissions/_static/image1.png)

Teraz uruchom *Elmah.axd* Aby wyświetlić raport o błędzie. Po zalogowaniu się przy użyciu poświadczeń konta administratora (&quot;admin&quot; i &quot;devpwd&quot;), zobacz stronę błędu pusty dziennika, ponieważ Elmah nie może utworzyć pliku XML w *Elmah*folderu:

![Błąd dziennika pusta](setting-folder-permissions/_static/image2.png)

## <a name="set-write-permission-on-the-elmah-folder"></a>Ustaw uprawnienia do zapisu w folderze Elmah

Możesz ręcznie ustawić uprawnienia do folderu lub można utworzyć automatycznego część procesu wdrażania. Dzięki automatycznej wymaga złożonego kodu MSBuild, a ponieważ masz w tym celu należy wdrożyć po raz pierwszy, następujące kroki jak to zrobić ręcznie. (Aby uzyskać informacje o sposobie tworzenia ta część procesu wdrażania, zobacz [ustawienie uprawnień folderu publikowania w sieci Web](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx) na blogu Sayed Hashimi.)

1. W **Eksploratora plików**, przejdź do *C:\inetpub\wwwroot\ContosoUniversity*. Kliknij prawym przyciskiem myszy *Elmah* folderu, wybierz opcję **właściwości**, a następnie wybierz **zabezpieczeń** kartę.
2. Kliknij przycisk **Edytuj**.
3. W **uprawnienia dla Elmah** okno dialogowe, wybierz opcję **domyślna pula aplikacji**, a następnie wybierz **zapisu** pole wyboru w **Zezwalaj** kolumny.

    ![Uprawnienia do folderu ELMAH](setting-folder-permissions/_static/image3.png)

    (Jeśli nie widzisz **DefaultAppPool** w **nazwy grup lub użytkowników** listy, prawdopodobnie użyto innej metody niż określona w tym samouczku można skonfigurować usługi IIS i platformy ASP.NET 4 na komputerze. W takim przypadku dowiedzieć się, jakie tożsamości jest używana przez pulę aplikacji przypisany do aplikacji Contoso University i przydziel uprawnienie zapisu do tej tożsamości. Zobacz linki dotyczące tożsamości puli aplikacji na końcu tego samouczka). Kliknij przycisk **OK** w obu polach okna dialogowego.

## <a name="retest-error-logging-and-reporting"></a>Sprawdź jeszcze raz rejestrowania błędów i raportowanie

Testowanie powodując błąd ponownie w taki sam sposób (żądanie nieprawidłowy adres URL) i uruchom **dziennik błędów** strony. Teraz ten błąd pojawia się na stronie.

![Strona dziennika błędów ELMAH](setting-folder-permissions/_static/image4.png)

## <a name="summary"></a>Podsumowanie

Ukończono wszystkich zadań, które są niezbędne, aby uzyskać Contoso University działa poprawnie w usługach IIS na komputerze lokalnym. W następnym samouczku udostępnienia lokacji publicznie przez wdrożenie jej na platformie Azure.

## <a name="more-information"></a>Więcej informacji

W tym przykładzie przyczyny, dlaczego nie można zapisać plików dziennika Elmah było dość oczywiste. Śledzenie usług IIS można użyć w przypadku gdy przyczyną tego problemu nie jest tak oczywiste; zobacz [Rozwiązywanie problemów z żądania za pomocą śledzenia nieudanych w usługach IIS 7](https://www.iis.net/learn/troubleshoot/using-failed-request-tracing/troubleshooting-failed-requests-using-tracing-in-iis) w witrynie IIS.net.

Aby uzyskać więcej informacji dotyczących sposobu udzielania uprawnień do tożsamości puli aplikacji, zobacz [tożsamości puli aplikacji](https://www.iis.net/learn/manage/configuring-security/application-pool-identities) i [Zabezpieczanie zawartości w usługach IIS za pomocą list ACL systemu plików](https://www.iis.net/learn/get-started/planning-for-security/secure-content-in-iis-through-file-system-acls) w witrynie IIS.net.

> [!div class="step-by-step"]
> [Poprzednie](deploying-to-iis.md)
> [dalej](deploying-to-production.md)
