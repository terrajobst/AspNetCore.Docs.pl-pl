---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
title: Trwa konfigurowanie rozwiązania kontaktów Menedżerze | Dokumentacja firmy Microsoft
author: jrjlee
description: W tym temacie opisano, jak pobrać i skonfigurować Menedżera skontaktuj się z rozwiązania do uruchomienia lokalnie na stacji roboczej developer.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 200b973c-776b-4a9b-9e82-39fda6120a52
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
msc.type: authoredcontent
ms.openlocfilehash: e8fb24f5b2d96d864d1aa6bc0f78644773de00ab
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30881815"
---
<a name="setting-up-the-contact-manager-solution"></a>Trwa konfigurowanie rozwiązania z menedżerem kontaktu
====================
przez [Lewandowski Jason](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie opisano, jak pobrać i skonfigurować Menedżera skontaktuj się z rozwiązania do uruchomienia lokalnie na stacji roboczej developer.


## <a name="system-requirements"></a>Wymagania systemowe

Aby uruchomić rozwiązanie kontaktów Menedżerze lokalnie i wykonywać inne zadania opisane w tym samouczku, musisz zainstalować tego oprogramowania na stacji roboczej developer:

- Visual Studio 2010 Service Pack 1, Premium lub Ultimate
- Internetowe usługi informacyjne (IIS) 7.5 Express
- SQL Server Express 2008 R2
- Usługi IIS narzędzie Web Deployment (Web Deploy) 2.1 lub nowszej
- ASP.NET 4.0
- ASP.NET MVC 3
- Program .NET Framework 4
- .NET Framework 3.5 SP1

Z wyjątkiem programu Visual Studio 2010, należy pobrać i zainstalować najnowsze wersje wszystkich produktów i składników za pomocą [Instalatora platformy sieci Web](https://go.microsoft.com/?linkid=9805118).

## <a name="download-and-extract-the-solution"></a>Pobierać i wyodrębniać rozwiązania

Możesz pobrać menedżera skontaktuj się z przykładową aplikację z galerii kodu MSDN [tutaj](https://code.msdn.microsoft.com/Deploying-Web-Applications-9d9093c0).

## <a name="configure-and-run-the-solution"></a>Konfigurowanie i uruchamianie rozwiązania

Aby skonfigurować i uruchamianie rozwiązania Contact Manager na komputerze lokalnym, należy wykonać następujące ogólne kroki:

1. Jeśli nie masz już, Utwórz lokalną bazę danych usług aplikacji platformy ASP.NET z funkcji zarządzania członkostwa i ról włączona.
2. Edytuj parametry połączenia w *web.config* pliki, aby wskazywał lokalne wystąpienie programu SQL Server Express.
3. Uruchom rozwiązania z programu Visual Studio 2010.

W pozostałej części tej sekcji znajdują się wskazówki więcej na temat sposobu ukończenia każdego z tych zadań.

**Aby utworzyć bazę danych usług aplikacji**

1. Otwórz wiersz polecenia programu Visual Studio 2010. Aby to zrobić, na **Start** menu wskaż **wszystkie programy**, kliknij przycisk **programu Microsoft Visual Studio 2010**, kliknij przycisk **programu Visual Studio Tools**, a następnie Kliknij przycisk **wiersz polecenia programu Visual Studio (2010)**.
2. W wierszu polecenia wpisz następujące polecenie, a następnie naciśnij klawisz Enter:

    [!code-console[Main](setting-up-the-contact-manager-solution/samples/sample1.cmd)]

    1. Użyj **– C** przełącznik, aby określić parametry połączenia dla serwera bazy danych.
    2. Użyj **–A** przełącznik, aby określić funkcje, które mają zostać dodane do bazy danych usług aplikacji. W takim przypadku **m** wskazuje, że chcesz dodać obsługę dostawcy członkostwa i **r** wskazuje, że chcesz dodać obsługę menedżera ról.
    3. Użyj **– d** przełącznik, aby określić nazwę bazy danych usług aplikacji. Jeśli ta opcja zostanie pominięta, narzędzie utworzy bazę danych o nazwie domyślnej **aspnetdb**.
3. Gdy baza danych została pomyślnie utworzona, wiersz polecenia zostanie wyświetlone potwierdzenie.

    ![](setting-up-the-contact-manager-solution/_static/image1.png)

> [!NOTE]
> Aby uzyskać więcej informacji na temat aspnet\_regsql narzędzie, zobacz [ASP.NET SQL Server Registration Tool (Aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx).


Następnym krokiem jest, aby upewnić się, że parametry połączenia w rozwiązaniu kontaktów Menedżerze odwołują się do lokalnego wystąpienia programu SQL Server Express.

**Aby zaktualizować ciągi połączenia**

1. Otwórz rozwiązanie Contact Manager w programie Visual Studio 2010.
2. W **Eksploratora rozwiązań** okna, rozwiń węzeł **ContactManager.Mvc** projektu, a następnie kliknij dwukrotnie **Web.config** węzła.

    > [!NOTE]
    > Projekt ContactManager.Mvc zawiera dwa *web.config* plików. Musisz zmodyfikować plik poziom projektu.

    ![](setting-up-the-contact-manager-solution/_static/image2.png)
3. W **connectionStrings** elementu, sprawdź, czy parametry połączenia o nazwie **ApplicationServices** wskazuje lokalnej bazy danych usług aplikacji ASP.NET.

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample2.xml)]
4. W **Eksploratora rozwiązań** okna, rozwiń węzeł **ContactManager.Service** projektu, a następnie kliknij dwukrotnie **Web.config** węzła.

    ![](setting-up-the-contact-manager-solution/_static/image3.png)
5. W **connectionStrings** elementu w parametrach połączenia o nazwie **ContactManagerContext**, upewnij się, że **źródła danych** właściwość jest ustawiona na lokalnego wystąpienia programu SQL Server Server Express. Nie trzeba zmieniać niczego więcej w parametrach połączenia.

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample3.xml)]
6. Zapisz wszystkie otwarte pliki.

Teraz powinno być gotowy do uruchomienia rozwiązania Contact Manager na komputerze lokalnym.

> [!NOTE]
> Jeśli wykonujesz te kroki bez tworzenia bazy danych usług aplikacji ASP.NET utworzy bazę danych po raz pierwszy próbę utworzenia użytkownika. Jednak ręcznego tworzenia bazy danych zapewnia znacznie większą kontrolę nad zestawu funkcji usług aplikacji, które mają być obsługiwane.


**Aby uruchomić rozwiązanie Contact Manager**

1. In Visual Studio 2010, press F5.
2. Program Internet Explorer jest uruchamiany i żąda adres URL aplikacji, skontaktuj się z Menedżera ASP.NET MVC 3. Domyślnie są wyświetlane w aplikacji **wszystkie kontakty** strony.

    ![](setting-up-the-contact-manager-solution/_static/image4.png)
3. Dodaj kilka kontaktów, a następnie sprawdź, czy aplikacja działa zgodnie z oczekiwaniami.

    ![](setting-up-the-contact-manager-solution/_static/image5.png)
4. Przejdź do `http://localhost:50114/Account/Register` (Dostosuj adres URL, jeśli przechowujesz aplikacji na innym porcie). Dodaj nazwę użytkownika, adres e-mail i hasło i sprawdź, czy możesz pomyślnie zarejestrować konto.

    ![](setting-up-the-contact-manager-solution/_static/image6.png)
5. Przejdź do `http://localhost:50114/Account/LogOn` (Dostosuj adres URL, jeśli przechowujesz aplikacji na innym porcie). Sprawdź, czy będziesz mieć możliwość logowania za pomocą utworzonego konta.

    ![](setting-up-the-contact-manager-solution/_static/image7.png)
6. Zamknij program Internet Explorer, aby zatrzymać debugowanie.

## <a name="conclusion"></a>Wniosek

W tym momencie rozwiązanie kontaktów Menedżerze powinna być w pełni skonfigurowane do uruchomienia na komputerze lokalnym. Rozwiązanie służy jako odwołanie podczas pracy za pośrednictwem innych tematach w tej instrukcji.

Następnym temacie [opis pliku projektu](understanding-the-project-file.md), wyjaśniono, jak niestandardowe pliki projektu Microsoft kompilacji Engine (MSBuild) w ramach rozwiązania Contact Manager umożliwia kontrolować proces wdrażania.

> [!div class="step-by-step"]
> [Poprzednie](the-contact-manager-solution.md)
> [dalej](understanding-the-project-file.md)
