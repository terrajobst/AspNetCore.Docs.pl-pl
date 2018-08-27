---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
title: Konfigurowanie rozwiązania Contact Manager | Dokumentacja firmy Microsoft
author: jrjlee
description: W tym temacie opisano, jak pobrać i skonfigurować rozwiązanie Contact Manager, aby uruchomić lokalnie na stacji roboczej dewelopera.
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 200b973c-776b-4a9b-9e82-39fda6120a52
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
msc.type: authoredcontent
ms.openlocfilehash: d834e6fbd2b46dca8bd7eeadc1eb68b33e62bb38
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756220"
---
<a name="setting-up-the-contact-manager-solution"></a>Konfigurowanie rozwiązania Contact Manager
====================
przez [Jason Lee](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie opisano, jak pobrać i skonfigurować rozwiązanie Contact Manager, aby uruchomić lokalnie na stacji roboczej dewelopera.


## <a name="system-requirements"></a>Wymagania systemowe

Aby uruchomić rozwiązanie Contact Manager lokalnie i wykonywać inne zadania, które opisano w tym samouczku będą potrzebne do zainstalowania tego oprogramowania na stacji roboczej dewelopera:

- Wersji Ultimate, Premium lub Visual Studio 2010 Service Pack 1
- Internetowe usługi informacyjne (IIS) 7.5 Express
- SQL Server Express 2008 R2
- Usługi IIS narzędzie Web Deployment (Web Deploy) 2.1 lub nowszej
- ASP.NET 4.0
- ASP.NET MVC 3
- Program .NET Framework 4
- .NET Framework 3.5 SP1

Z wyjątkiem programu Visual Studio 2010, możesz pobrać i zainstalować najnowsze wersje wszystkich tych produktów i składników za pomocą [Instalatora platformy sieci Web](https://go.microsoft.com/?linkid=9805118).

## <a name="download-and-extract-the-solution"></a>Pobierz i Wyodrębnij rozwiązania

Przykładowa aplikacja Contact Manager można pobrać z galerii kodu MSDN [tutaj](https://code.msdn.microsoft.com/Deploying-Web-Applications-9d9093c0).

## <a name="configure-and-run-the-solution"></a>Konfigurowanie i uruchamianie rozwiązania

Aby skonfigurować i uruchomić rozwiązanie Contact Manager na komputerze lokalnym, należy wykonać następującą ogólną procedurą:

1. Jeśli nie masz jeszcze, należy utworzyć lokalnej bazy danych usług aplikacji ASP.NET przy użyciu funkcji zarządzania członkostwa i ról włączona.
2. Edytuj parametry połączenia w *web.config* pliki, aby wskazywały do lokalnego wystąpienia programu SQL Server Express.
3. Uruchom rozwiązanie programu Visual Studio 2010.

Dalszej części tej sekcji zawiera więcej wskazówek na temat sposobu ukończenia każdego z tych zadań.

**Aby utworzyć bazę danych usług aplikacji**

1. Otwórz wiersz polecenia programu Visual Studio 2010. Aby to zrobić, na **Start** menu wskaż **wszystkie programy**, kliknij przycisk **Microsoft Visual Studio 2010**, kliknij przycisk **Visual Studio Tools**, a następnie Kliknij przycisk **wiersza polecenia programu Visual Studio (2010)**.
2. W wierszu polecenia wpisz następujące polecenie, a następnie naciśnij klawisz Enter:

    [!code-console[Main](setting-up-the-contact-manager-solution/samples/sample1.cmd)]

    1. Użyj **– C** przełącznik, aby określić parametry połączenia dla serwera bazy danych.
    2. Użyj **–A** przełącznika, usługi aplikacji funkcji, które chcesz dodać do bazy danych. W tym przypadku **m** wskazuje, że dodanie obsługi dostawcy członkostwa i **r** wskazuje, że chcesz dodać obsługę menedżera ról.
    3. Użyj **– d** przełącznika, aby określić nazwę bazy danych usług aplikacji. Jeśli ta opcja zostanie pominięta, narzędzie utworzy bazę danych z domyślną nazwą **aspnetdb**.
3. Gdy baza danych została utworzona pomyślnie, wiersz polecenia zostanie wyświetlone potwierdzenie.

    ![](setting-up-the-contact-manager-solution/_static/image1.png)

> [!NOTE]
> Aby uzyskać więcej informacji na temat aspnet\_regsql narzędzia, zobacz [narzędzia rejestracji serwera SQL platformy ASP.NET (Aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx).


Następnym krokiem jest, aby upewnić się, że parametry połączenia dla rozwiązania Contact Manager odwołują się do lokalnego wystąpienia programu SQL Server Express.

**Aby zaktualizować ciągi połączenia**

1. Otwórz rozwiązanie Contact Manager w programie Visual Studio 2010.
2. W **Eksploratora rozwiązań** okna, rozwiń węzeł **ContactManager.Mvc** projektu, a następnie kliknij dwukrotnie **Web.config** węzła.

    > [!NOTE]
    > Projekt ContactManager.Mvc zawiera dwie *web.config* plików. Należy edytować plik na poziomie projektu.

    ![](setting-up-the-contact-manager-solution/_static/image2.png)
3. W **connectionStrings** elementu, sprawdź, czy parametry połączenia o nazwie **ApplicationServices** punktów w lokalnej bazie danych usług aplikacji ASP.NET.

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample2.xml)]
4. W **Eksploratora rozwiązań** okna, rozwiń węzeł **ContactManager.Service** projektu, a następnie kliknij dwukrotnie **Web.config** węzła.

    ![](setting-up-the-contact-manager-solution/_static/image3.png)
5. W **connectionStrings** elementu w parametrach połączenia o nazwie **ContactManagerContext**, upewnij się, że **źródła danych** właściwość jest ustawiona na lokalnym wystąpieniem programu SQL Server Server Express. Nie trzeba zmieniać niczego więcej w parametrach połączenia.

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample3.xml)]
6. Zapisz wszystkie otwarte pliki.

Teraz powinno być gotowe do uruchomienia rozwiązania Contact Manager na komputerze lokalnym.

> [!NOTE]
> Jeśli wykonujesz te kroki bez tworzenia bazy danych usług aplikacji ASP.NET spowoduje utworzenie bazy danych po raz pierwszy użytkownik podejmie próbę utworzenia użytkownika. Jednak ręcznego tworzenia bazy danych zapewnia znacznie większą kontrolę nad zestawu funkcji usług aplikacji, które mają być obsługiwane.


**Aby uruchomić rozwiązanie Contact Manager**

1. W programie Visual Studio 2010 naciśnij klawisz F5.
2. Program Internet Explorer jest uruchamiany i żąda adresu URL aplikacji Contact Manager ASP.NET MVC 3. Domyślnie aplikacja wyświetla **wszystkie kontakty** strony.

    ![](setting-up-the-contact-manager-solution/_static/image4.png)
3. Dodaj kilka kontakty, a następnie sprawdź, czy aplikacja działa zgodnie z oczekiwaniami.

    ![](setting-up-the-contact-manager-solution/_static/image5.png)
4. Przejdź do `http://localhost:50114/Account/Register` (zmienić adres URL, jeśli hostujesz aplikację na innym porcie). Dodaj nazwę użytkownika, adres e-mail i hasło, a następnie sprawdź, czy jesteś w stanie pomyślnie zarejestrować konto.

    ![](setting-up-the-contact-manager-solution/_static/image6.png)
5. Przejdź do `http://localhost:50114/Account/LogOn` (zmienić adres URL, jeśli hostujesz aplikację na innym porcie). Sprawdź, czy jesteś w stanie zalogować się przy użyciu konta, który został utworzony.

    ![](setting-up-the-contact-manager-solution/_static/image7.png)
6. Zamknij program Internet Explorer, aby zatrzymać debugowanie.

## <a name="conclusion"></a>Wniosek

W tym momencie rozwiązanie Contact Manager powinien być w pełni skonfigurowane do uruchomienia na komputerze lokalnym. Podczas pracy z innymi tematami w tym samouczku, można użyć rozwiązania jako odwołanie.

Następny temat [objaśnienie pliku projektu](understanding-the-project-file.md), wyjaśnia, jak niestandardowe pliki projektu aparatu Microsoft Build Engine (MSBuild) w ramach rozwiązania Contact Manager można użyć do kontrolowania procesu wdrażania.

> [!div class="step-by-step"]
> [Poprzednie](the-contact-manager-solution.md)
> [dalej](understanding-the-project-file.md)
