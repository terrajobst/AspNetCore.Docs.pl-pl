---
uid: web-forms/overview/deployment/visual-studio-web-deployment/setting-folder-permissions
title: 'Wdrażanie aplikacji internetowych ASP.NET przy użyciu programu Visual Studio: Ustawianie uprawnień folderów | Dokumentacja firmy Microsoft'
author: tdykstra
description: W tej serii samouczków dowiesz się, jak wdrożyć (opublikować) platformy ASP.NET sieci web aplikacji do usługi Azure App Service Web Apps lub dostawcy hostingu w innych firm, używane...
ms.author: aspnetcontent
ms.date: 02/15/2013
ms.assetid: 9715a121-fa55-4f1b-a5d2-fb3f6cd8be8f
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/setting-folder-permissions
msc.type: authoredcontent
ms.openlocfilehash: 0660a464063783406a69caf663036811c8ac818e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37802036"
---
<a name="aspnet-web-deployment-using-visual-studio-setting-folder-permissions"></a>Wdrażanie aplikacji internetowych ASP.NET przy użyciu programu Visual Studio: Ustawianie uprawnień folderów
====================
przez [Tom Dykstra](https://github.com/tdykstra)

[Pobieranie projektu startowego](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> W tej serii samouczków dowiesz się, jak wdrożyć (opublikować) platformy ASP.NET sieci web aplikacji do usługi Azure App Service Web Apps lub innych firm dostawcy hostingu za pomocą programu Visual Studio 2012 lub Visual Studio 2010. Aby uzyskać informacje na temat serii, zobacz [pierwszym samouczku tej serii](introduction.md).


## <a name="overview"></a>Omówienie

W tym samouczku Ustaw uprawnienia do folderu dla *Elmah* folderu w sieci web wdrożonej lokacji tak, aby aplikacja mogła tworzyć pliki dziennika, w tym folderze.

Podczas testowania aplikacji sieci web w programie Visual Studio przy użyciu serwera wdrożeniowego programu Visual Studio (Cassini) lub usług IIS Express, aplikacja będzie działać w ramach Twojej tożsamości. Prawdopodobnie jesteś administratorem na komputerze deweloperskim i mają pełne uprawnienia do wykonywania wszystkich plików w dowolnym folderze. Jednak po uruchomieniu aplikacji w środowisku usług IIS, jest uruchamiana przy użyciu tożsamości zdefiniowane dla puli aplikacji, przypisany do lokacji. Jest to zazwyczaj konta zdefiniowaną przez system, które ma ograniczone uprawnienia. Domyślnie jej ma uprawnienia odczytu i wykonania do plików i folderów aplikacji sieci web, ale nie ma dostępu do zapisu.

Ten staje się problem, jeśli aplikacja tworzy albo pliki aktualizacji, które pojawia się często w aplikacjach sieci web. W aplikacji Contoso University Elmah tworzy pliki XML w *Elmah* folder, aby zapisać szczegółowe informacje o błędach. Nawet wtedy, gdy nie używasz podobny Elmah, witryny sieci mogą zezwolić użytkownikom na przekazywanie plików lub wykonywać inne zadania, które zapisu danych do folderu, w witrynie.

Przypomnienie: Jeśli otrzymujesz komunikat o błędzie lub coś nie działa podczas wykonywania kroków samouczka, upewnij się sprawdzić [strona rozwiązywania problemów](troubleshooting.md).

## <a name="test-error-logging-and-reporting"></a>Test rejestrowania błędów i raportowanie

Aby zobaczyć, jak aplikacja nie działa prawidłowo w usługach IIS (chociaż rejestrowaniu przetestowano ją w programie Visual Studio), może spowodować błąd, który normalnie zostanie zarejestrowany przez Elmah, a następnie otwórz dziennik błędów Elmah, aby wyświetlić szczegóły. Jeśli Elmah nie mógł utworzyć pliku XML i zapisać szczegóły błędu, zostanie wyświetlony raport o błędach puste.

Otwórz przeglądarkę i przejdź do `http://localhost/ContosoUniversity`, a następnie żądają nieprawidłowy adres URL, takich jak *Studentsxxx.aspx*. Zostanie wyświetlona strona błędu generowanych przez system, zamiast *GenericErrorPage.aspx* strony, ponieważ `customErrors` "RemoteOnly" jest ustawione w pliku Web.config i są uruchomione usługi IIS lokalnie:

![Strona błędu 404 protokołu HTTP](setting-folder-permissions/_static/image1.png)

Teraz uruchom *Elmah.axd* Aby wyświetlić raport o błędach. Po zalogowaniu się przy użyciu poświadczeń konta administratora (&quot;administratora&quot; i &quot;devpwd&quot;), zostanie wyświetlona strona dziennika błędów pusty, ponieważ Elmah nie może utworzyć pliku XML w *Elmah*folderu:

![Pusta dziennik błędów](setting-folder-permissions/_static/image2.png)

## <a name="set-write-permission-on-the-elmah-folder"></a>Ustaw uprawnienia do zapisu do folderu biblioteki Elmah

Możesz ręcznie ustawić uprawnienia do folderu, lub możesz przekształcić ją w automatyczne część procesu wdrażania. Dzięki czemu automatycznego wymaga złożonego kodu programu MSBuild, a ponieważ trzeba to zrobić podczas pierwszego wdrażania, następujące kroki jak to zrobić ręcznie. (Aby dowiedzieć się, jak ta część procesu wdrażania, zobacz [Ustawianie uprawnień folderu publikowania w sieci Web](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx) na blogu Sayed Hashimi.)

1. W **Eksploratora plików**, przejdź do *C:\inetpub\wwwroot\ContosoUniversity*. Kliknij prawym przyciskiem myszy *Elmah* folderu, wybierz **właściwości**, a następnie wybierz pozycję **zabezpieczeń** kartę.
2. Kliknij przycisk **Edytuj**.
3. W **uprawnienia dla biblioteki Elmah** okno dialogowe, wybierz opcję **domyślna pula aplikacji**, a następnie wybierz pozycję **zapisu** pole wyboru w **Zezwalaj** kolumny.

    ![Uprawnienia do folderu biblioteki ELMAH](setting-folder-permissions/_static/image3.png)

    (Jeśli nie widzisz **DefaultAppPool** w **nazwy grupy lub użytkownika** listy, prawdopodobnie używasz innej metody niż określona w tym samouczku do skonfigurowania usług IIS i platformy ASP.NET 4 na komputerze. W takim przypadku Dowiedz się, jakie tożsamość jest używana przez pulę aplikacji przypisane do aplikacji Contoso University i udzielanie uprawnień do zapisu do tej tożsamości. Zobacz linki dotyczące tożsamości puli aplikacji na końcu tego samouczka). Kliknij przycisk **OK** w obu polach okna dialogowego.

## <a name="retest-error-logging-and-reporting"></a>Przetestowanie rejestrowania błędów i raportowanie

Przetestuj, co powoduje wystąpienie błędu, ponownie tak samo jak (żądanie nieprawidłowy adres URL) i uruchom **dziennik błędów** strony. Tym razem ten błąd pojawia się na stronie.

![Strona dziennika błędów ELMAH](setting-folder-permissions/_static/image4.png)

## <a name="summary"></a>Podsumowanie

Ukończono wszystkie zadania, które są niezbędne do doprowadzenia Contoso University działa prawidłowo w usługach IIS na komputerze lokalnym. W następnym samouczku będzie witryna staje się publicznie dostępny przez wdrożenie jej na platformie Azure.

## <a name="more-information"></a>Więcej informacji

W tym przykładzie powód, dlaczego Elmah nie może zapisywać pliki dziennika było dość oczywiste. Śledzenie usług IIS można użyć w przypadku gdy przyczyną problemu jest nie tak oczywiste; zobacz [rozwiązywania problemów nie powiodło się żądania przy użyciu śledzenia w usługach IIS 7](https://www.iis.net/learn/troubleshoot/using-failed-request-tracing/troubleshooting-failed-requests-using-tracing-in-iis) IIS.net w witrynie.

Aby uzyskać więcej informacji na temat sposobu przyznawania uprawnień do tożsamości puli aplikacji, zobacz [tożsamości puli aplikacji](https://www.iis.net/learn/manage/configuring-security/application-pool-identities) i [Zabezpieczanie zawartości w usługach IIS za pomocą list ACL systemu plików](https://www.iis.net/learn/get-started/planning-for-security/secure-content-in-iis-through-file-system-acls) IIS.net w witrynie.

> [!div class="step-by-step"]
> [Poprzednie](deploying-to-iis.md)
> [dalej](deploying-to-production.md)
