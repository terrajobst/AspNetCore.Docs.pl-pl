---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12
title: "Wdrażanie aplikacji sieci Web ASP.NET przy użyciu programu Visual Studio lub Visual Web Developer programu SQL Server Compact: Ustawianie uprawnień folderu - 6, 12 | Dokumentacja firmy Microsoft"
author: tdykstra
description: "Tej serii samouczków opisano sposób wdrażania platformy ASP.NET (publikowanie) projektu aplikacji sieci web, który zawiera bazę danych programu SQL Server Compact przy użyciu Visual Stu..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: cd03a188-e947-4f55-9bda-b8bce201d8c6
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12
msc.type: authoredcontent
ms.openlocfilehash: 42085fff5f1aed1440f49e1e2ceee0cf0e751e2c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-setting-folder-permissions---6-of-12"></a>Wdrażanie aplikacji sieci Web ASP.NET przy użyciu programu Visual Studio lub Visual Web Developer programu SQL Server Compact: Ustawianie uprawnień folderu - 6, 12
====================
przez [Dykstra niestandardowy](https://github.com/tdykstra)

[Pobierz początkowego projektu](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Tej serii samouczków opisano sposób wdrażania platformy ASP.NET (publikowanie) projektu aplikacji sieci web, który zawiera bazę danych programu SQL Server Compact przy użyciu programu Visual Studio 2012 RC i Visual Studio Express 2012 RC for Web. W razie musisz zainstalować aktualizację publikowania w sieci Web, można również używać programu Visual Studio 2010. Aby obejrzeć wprowadzenie do serii, zobacz [pierwszy samouczek z tej serii](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Samouczek zawiera funkcje wdrażania dodane po wydaniu programu Visual Studio 2012 RC, pokazuje, jak wdrożyć wersjach programu SQL Server innych niż SQL Server Compact, która przedstawia sposób wdrażania aplikacji sieci Web usługi aplikacji Azure, zobacz [wdrożenia sieci Web ASP.NET za pomocą programu Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Omówienie

W tym samouczku, należy ustawić uprawnienia folderu do *Elmah* folder w sieci web, wdrożonych lokacji, dzięki czemu aplikacja może tworzyć pliki dziennika w tym folderze.

Podczas testowania aplikacji sieci web w programie Visual Studio przy użyciu programu Visual Studio serwera projektowego (Cassini), aplikacja zostanie uruchomiona w ramach Twojej tożsamości. Prawdopodobnie są uprawnienia administratora na komputerze deweloperskim i mieć pełne uprawnienia do podejmować żadnych działań, plików w folderze. Ale po uruchomieniu aplikacji w środowisku usług IIS, działa z tożsamością zdefiniowane dla puli aplikacji, przypisane do lokacji. Jest to zazwyczaj konto zdefiniowane przez system, który ma ograniczone uprawnienia. Domyślnie przeczytał i uprawnienia do wykonywania dla aplikacji sieci web plików i folderów, ale nie ma dostępu do zapisu.

Staje się on problemu tworzy aplikacji lub aktualizacji plików, co jest typowe w aplikacji sieci web. W aplikacji Contoso University Elmah tworzy pliki XML z *Elmah* folderu w celu zapisania informacji o błędach. Nawet jeśli nie używasz przypominać Elmah, witryny zezwolić użytkownikom na przekazywanie plików lub innych zadań, które zapisu danych do folderu w witrynie sieci.

Przypomnienie: Jeśli coś nie działa podczas wykonywania kroków samouczka wyświetlony komunikat o błędzie, należy sprawdzić [Rozwiązywanie problemów z strony](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="testing-error-logging-and-reporting"></a>Testowanie Błąd rejestrowania i raportowania

Aby zobaczyć, jak aplikacja nie działa prawidłowo w usługach IIS (chociaż jak testach w programie Visual Studio), może spowodować błąd, który zwykle rejestrowany przez Elmah, a następnie otwórz dziennik błędów Elmah, aby wyświetlić szczegóły. Jeśli Elmah nie może utworzyć pliku XML i zapisać szczegóły błędu, zostanie wyświetlony raport o błędach puste.

Otwórz przeglądarkę i przejdź do `http://localhost/ContosoUniversity`, a następnie żądają nieprawidłowy adres URL, takie jak *Studentsxxx.aspx*. Zostanie wyświetlona strona błędu generowanych przez system, zamiast *GenericErrorPage.aspx* strony, ponieważ `customErrors` ustawienia w pliku Web.config jest "RemoteOnly" i są uruchomione usługi IIS lokalnie:

[![Error_page_Test](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image2.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image1.png)

Teraz uruchom *Elmah.axd* Aby wyświetlić raport o błędzie. Zobacz stronę błędu pusty dziennika, ponieważ Elmah nie może utworzyć pliku XML w *Elmah* folderu:

[![Error_log_page_empty](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image4.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image3.png)

## <a name="setting-write-permission-on-the-elmah-folder"></a>Ustawienie uprawnienie zapisu do folderu Elmah

Możesz ręcznie ustawić uprawnienia do folderu lub można utworzyć automatycznego część procesu wdrażania. Dzięki automatycznej wymaga złożonego kodu MSBuild, a ponieważ trzeba to zrobić podczas pierwszego wdrażania, w tym samouczku tylko pokazano, jak to zrobić ręcznie. (Aby uzyskać informacje o sposobie tworzenia ta część procesu wdrażania, zobacz [ustawienie uprawnień folderu publikowania w sieci Web](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx) na blogu Sayed Hashimi.)

W **Eksploratora Windows**, przejdź do *C:\inetpub\wwwroot\ContosoUniversity*. Kliknij prawym przyciskiem myszy *Elmah* folderu, wybierz opcję **właściwości**, a następnie wybierz **zabezpieczeń** kartę.

[![Elmah_folder_Properties_Security_tab](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image6.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image5.png)

(Jeśli nie widzisz **DefaultAppPool** w **nazwy grup lub użytkowników** listy, prawdopodobnie użyto innej metody niż określona w tym samouczku można skonfigurować usługi IIS i platformy ASP.NET 4 na komputerze. W takim przypadku dowiedzieć się, jakie tożsamości jest używana przez pulę aplikacji przypisany do aplikacji Contoso University i przydziel uprawnienie zapisu do tej tożsamości. Zobacz linki dotyczące tożsamości puli aplikacji na końcu tego samouczka).

Kliknij przycisk **Edytuj**. W **uprawnienia dla Elmah** okno dialogowe, wybierz opcję **domyślna pula aplikacji**, a następnie wybierz **zapisu** pole wyboru w **Zezwalaj** kolumny.

[![Permissions_for_Elmah_dialog_box](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image8.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image7.png)

Kliknij przycisk **OK** w obu polach okna dialogowego.

## <a name="retesting-error-logging-and-reporting"></a>Ponowne Błąd rejestrowania i raportowania

Testowanie powodując błąd ponownie w taki sam sposób (żądanie nieprawidłowy adres URL) i uruchom **dziennik błędów** strony. Teraz ten błąd pojawia się na stronie.

[![Elmah_Error_Log_page_Test](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image10.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image9.png)

Musisz też mieć uprawnienia zapisu na *aplikacji\_danych* folderu ponieważ masz pliki bazy danych programu SQL Server Compact w tym folderze, i chcesz można było zaktualizować dane w tych baz danych. W takim przypadku, nie trzeba wykonywać żadnych dodatkowych czynności, ponieważ proces wdrażania automatycznie ustawia uprawnienia zapisu w *aplikacji\_danych* folderu.

Ukończono wszystkich zadań, które są niezbędne, aby uzyskać Contoso University działa poprawnie w usługach IIS na komputerze lokalnym. W następnym samouczku udostępnienia lokacji publicznie przez wdrożenie jej do dostawcy hostingu.

## <a name="more-information"></a>Więcej informacji

W tym przykładzie przyczyny, dlaczego nie można zapisać plików dziennika Elmah było dość oczywiste. Śledzenie usług IIS można użyć w przypadku gdy przyczyną tego problemu nie jest tak oczywiste; zobacz [Rozwiązywanie problemów z żądania za pomocą śledzenia nieudanych w usługach IIS 7](https://www.iis.net/learn/troubleshoot/using-failed-request-tracing/troubleshooting-failed-requests-using-tracing-in-iis) w witrynie IIS.net.

Aby uzyskać więcej informacji dotyczących sposobu udzielania uprawnień do tożsamości puli aplikacji, zobacz [tożsamości puli aplikacji](https://www.iis.net/learn/manage/configuring-security/application-pool-identities) i [Zabezpieczanie zawartości w usługach IIS za pomocą list ACL systemu plików](https://www.iis.net/learn/get-started/planning-for-security/secure-content-in-iis-through-file-system-acls) w witrynie IIS.net.

>[!div class="step-by-step"]
[Poprzednie](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)
[dalej](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)
