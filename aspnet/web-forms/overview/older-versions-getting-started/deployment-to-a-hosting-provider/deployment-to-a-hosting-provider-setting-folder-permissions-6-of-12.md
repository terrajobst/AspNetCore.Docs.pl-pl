---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12
title: 'Wdrażanie aplikacji sieci Web ASP.NET za pomocą programu SQL Server Compact przy użyciu programu Visual Studio lub Visual Web Developer: Ustawianie uprawnień folderów - 6, 12 | Dokumentacja firmy Microsoft'
author: tdykstra
description: W tej serii samouczków dowiesz się, jak wdrożyć (opublikować) programu ASP.NET projektu aplikacji sieci web, który zawiera bazę danych programu SQL Server Compact przy użyciu Visual Stu...
ms.author: aspnetcontent
ms.date: 11/17/2011
ms.assetid: cd03a188-e947-4f55-9bda-b8bce201d8c6
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12
msc.type: authoredcontent
ms.openlocfilehash: 50b67c20ee374f1c7ab846446f836411132ddf78
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37803684"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-setting-folder-permissions---6-of-12"></a>Wdrażanie aplikacji sieci Web ASP.NET za pomocą programu SQL Server Compact przy użyciu programu Visual Studio lub Visual Web Developer: Ustawianie uprawnień folderów - 6, 12
====================
przez [Tom Dykstra](https://github.com/tdykstra)

[Pobieranie projektu startowego](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> W tej serii samouczków dowiesz się, jak wdrożyć (opublikować) ASP.NET projektu aplikacji sieci web, która zawiera bazę danych programu SQL Server Compact przy użyciu programu Visual Studio 2012 RC lub Visual Studio Express 2012 RC for Web. Umożliwia także programu Visual Studio 2010 po zainstalowaniu aktualizacji publikowania w sieci Web. Aby zapoznać się z wprowadzeniem do serii, zobacz [pierwszym samouczku tej serii](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Aby uzyskać samouczek, który zawiera funkcje wdrażania wprowadzone po wersji RC programu Visual Studio 2012, pokazuje, jak wdrażać wersje programu SQL Server, innym niż SQL Server Compact i pokazuje, jak wdrożyć w usłudze Azure App Service Web Apps, zobacz [wdrażanie aplikacji internetowych ASP.NET za pomocą programu Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Omówienie

W tym samouczku Ustaw uprawnienia do folderu dla *Elmah* folderu w sieci web wdrożonej lokacji tak, aby aplikacja mogła tworzyć pliki dziennika, w tym folderze.

Podczas testowania aplikacji sieci web w programie Visual Studio przy użyciu serwera wdrożeniowego usługi Visual Studio (Cassini), aplikacja będzie działać w ramach Twojej tożsamości. Prawdopodobnie jesteś administratorem na komputerze deweloperskim i mają pełne uprawnienia do wykonywania wszystkich plików w dowolnym folderze. Jednak po uruchomieniu aplikacji w środowisku usług IIS, jest uruchamiana przy użyciu tożsamości zdefiniowane dla puli aplikacji, przypisany do lokacji. Jest to zazwyczaj konta zdefiniowaną przez system, które ma ograniczone uprawnienia. Domyślnie jej ma uprawnienia odczytu i wykonania do plików i folderów aplikacji sieci web, ale nie ma dostępu do zapisu.

Ten staje się problem, jeśli aplikacja tworzy albo pliki aktualizacji, które pojawia się często w aplikacjach sieci web. W aplikacji Contoso University Elmah tworzy pliki XML w *Elmah* folder, aby zapisać szczegółowe informacje o błędach. Nawet wtedy, gdy nie używasz podobny Elmah, witryny sieci mogą zezwolić użytkownikom na przekazywanie plików lub wykonywać inne zadania, które zapisu danych do folderu, w witrynie.

Przypomnienie: Jeśli otrzymujesz komunikat o błędzie lub coś nie działa podczas wykonywania kroków samouczka, upewnij się sprawdzić [strona rozwiązywania problemów](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="testing-error-logging-and-reporting"></a>Testowanie rejestrowania i raportowania błędów

Aby zobaczyć, jak aplikacja nie działa prawidłowo w usługach IIS (chociaż rejestrowaniu przetestowano ją w programie Visual Studio), może spowodować błąd, który normalnie zostanie zarejestrowany przez Elmah, a następnie otwórz dziennik błędów Elmah, aby wyświetlić szczegóły. Jeśli Elmah nie mógł utworzyć pliku XML i zapisać szczegóły błędu, zostanie wyświetlony raport o błędach puste.

Otwórz przeglądarkę i przejdź do `http://localhost/ContosoUniversity`, a następnie żądają nieprawidłowy adres URL, takich jak *Studentsxxx.aspx*. Zostanie wyświetlona strona błędu generowanych przez system, zamiast *GenericErrorPage.aspx* strony, ponieważ `customErrors` "RemoteOnly" jest ustawione w pliku Web.config i są uruchomione usługi IIS lokalnie:

[![Error_page_Test](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image2.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image1.png)

Teraz uruchom *Elmah.axd* Aby wyświetlić raport o błędach. Zostanie wyświetlona strona dziennika błędów pusty, ponieważ Elmah nie może utworzyć pliku XML w *Elmah* folderu:

[![Error_log_page_empty](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image4.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image3.png)

## <a name="setting-write-permission-on-the-elmah-folder"></a>Ustawienie uprawnień do zapisu w folderze Elmah

Możesz ręcznie ustawić uprawnienia do folderu, lub możesz przekształcić ją w automatyczne część procesu wdrażania. Dzięki czemu automatycznego wymaga złożonego kodu programu MSBuild, a ponieważ trzeba to zrobić podczas pierwszego wdrażania, w tym samouczku tylko pokazuje, jak to zrobić ręcznie. (Aby dowiedzieć się, jak ta część procesu wdrażania, zobacz [Ustawianie uprawnień folderu publikowania w sieci Web](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx) na blogu Sayed Hashimi.)

W **Eksplorator Windows**, przejdź do *C:\inetpub\wwwroot\ContosoUniversity*. Kliknij prawym przyciskiem myszy *Elmah* folderu, wybierz **właściwości**, a następnie wybierz pozycję **zabezpieczeń** kartę.

[![Elmah_folder_Properties_Security_tab](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image6.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image5.png)

(Jeśli nie widzisz **DefaultAppPool** w **nazwy grupy lub użytkownika** listy, prawdopodobnie używasz innej metody niż określona w tym samouczku do skonfigurowania usług IIS i platformy ASP.NET 4 na komputerze. W takim przypadku Dowiedz się, jakie tożsamość jest używana przez pulę aplikacji przypisane do aplikacji Contoso University i udzielanie uprawnień do zapisu do tej tożsamości. Zobacz linki dotyczące tożsamości puli aplikacji na końcu tego samouczka).

Kliknij przycisk **Edytuj**. W **uprawnienia dla biblioteki Elmah** okno dialogowe, wybierz opcję **domyślna pula aplikacji**, a następnie wybierz pozycję **zapisu** pole wyboru w **Zezwalaj** kolumny.

[![Permissions_for_Elmah_dialog_box](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image8.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image7.png)

Kliknij przycisk **OK** w obu polach okna dialogowego.

## <a name="retesting-error-logging-and-reporting"></a>Ponowne przetestowanie rejestrowania i raportowania błędów

Przetestuj, co powoduje wystąpienie błędu, ponownie tak samo jak (żądanie nieprawidłowy adres URL) i uruchom **dziennik błędów** strony. Tym razem ten błąd pojawia się na stronie.

[![Elmah_Error_Log_page_Test](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image10.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image9.png)

Musisz też mieć uprawnienia zapisu w *aplikacji\_danych* folderu, ponieważ masz pliki bazy danych programu SQL Server Compact, w tym folderze i chcesz można było zaktualizować dane w tych bazach danych. W takim przypadku możesz nie mają jednak zrobić niczego, ponieważ proces wdrażania automatycznie ustawia uprawnienia do zapisu dla *aplikacji\_danych* folderu.

Ukończono wszystkie zadania, które są niezbędne do doprowadzenia Contoso University działa prawidłowo w usługach IIS na komputerze lokalnym. W następnym samouczku będzie witryna staje się publicznie dostępny przez wdrożenie jej dostawcy usług hostingowych.

## <a name="more-information"></a>Więcej informacji

W tym przykładzie powód, dlaczego Elmah nie może zapisywać pliki dziennika było dość oczywiste. Śledzenie usług IIS można użyć w przypadku gdy przyczyną problemu jest nie tak oczywiste; zobacz [rozwiązywania problemów nie powiodło się żądania przy użyciu śledzenia w usługach IIS 7](https://www.iis.net/learn/troubleshoot/using-failed-request-tracing/troubleshooting-failed-requests-using-tracing-in-iis) IIS.net w witrynie.

Aby uzyskać więcej informacji na temat sposobu przyznawania uprawnień do tożsamości puli aplikacji, zobacz [tożsamości puli aplikacji](https://www.iis.net/learn/manage/configuring-security/application-pool-identities) i [Zabezpieczanie zawartości w usługach IIS za pomocą list ACL systemu plików](https://www.iis.net/learn/get-started/planning-for-security/secure-content-in-iis-through-file-system-acls) IIS.net w witrynie.

> [!div class="step-by-step"]
> [Poprzednie](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)
> [dalej](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)
