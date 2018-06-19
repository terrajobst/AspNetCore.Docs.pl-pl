---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
title: Uruchomionych skryptów PowerShell systemu Windows z plików projektu MSBuild | Dokumentacja firmy Microsoft
author: jrjlee
description: W tym temacie opisano sposób uruchamiania skryptu programu Windows PowerShell jako część procesu kompilacji i wdrożenia. Skrypt można uruchomić lokalnie (innymi słowy, na b...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 55f1ae45-fcb5-43a9-8415-fa5b935fc9c9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
msc.type: authoredcontent
ms.openlocfilehash: c8ef22cfbba7b3b85944ea4c49f3183e5a6aafbb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890362"
---
<a name="running-windows-powershell-scripts-from-msbuild-project-files"></a>Uruchomionych skryptów PowerShell systemu Windows z plików projektów MSBuild
====================
przez [Lewandowski Jason](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie opisano sposób uruchamiania skryptu programu Windows PowerShell jako część procesu kompilacji i wdrożenia. Możesz uruchomić skrypt lokalnie (innymi słowy, na serwerze kompilacji) lub zdalnie, takich jak na docelowym serwerze sieci web lub serwer bazy danych.
> 
> Istnieje wiele przyczyn, dlaczego warto Uruchom skrypt programu Windows PowerShell po wdrożeniu. Na przykład możesz chcieć:
> 
> - Dodawanie źródła zdarzeń niestandardowych w rejestrze.
> - Generowanie katalogu w systemie plików przekazywania.
> - Czyszczenie kompilacji katalogów.
> - Zapisywanie wpisów w pliku dziennika niestandardowego.
> - Wysyłaj wiadomości e-mail zapraszanie użytkowników do aplikacji sieci web nowo udostępnione.
> - Utwórz konta użytkowników z odpowiednimi uprawnieniami.
> - Konfigurowanie replikacji między wystąpieniami programu SQL Server.
> 
> W tym temacie opisano sposób uruchamiania skryptów programu Windows PowerShell lokalnie i zdalnie niestandardowy element docelowy w pliku projektu Microsoft kompilacji Engine (MSBuild).


Ten temat jest częścią serii samouczków na podstawie tych wymagań związanych z przedsiębiorstwa wdrażaniem fikcyjnej firmy o nazwie firmy Fabrikam, Inc. Przykładowe rozwiązanie korzysta z tego samouczka serii&#x2014; [rozwiązania kontaktów Menedżerze](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;do reprezentowania aplikacji sieci web z realistyczne poziom złożoności, w tym aplikacji ASP.NET MVC 3, Windows Communication Usługa Foundation (WCF), a projekt bazy danych.

Istotą te samouczki metody wdrażania opiera się na podejście pliku projektu podziału opisane w [opis pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), w którym jest kontrolowany przez proces kompilacji dwa pliki projektu&#x2014;jeden zawierający Tworzenie instrukcji, które mają zastosowanie do każdego środowiska docelowego i dysk zawierający ustawienia kompilacji i wdrożenia określonego środowiska. W czasie kompilacji pliku projektu określonego środowiska jest scalany pliku projektu niezależny od środowiska pełny zestaw instrukcji kompilacji.

## <a name="task-overview"></a>Omówienie zadań

Aby uruchomić skrypt programu Windows PowerShell w ramach procesu wdrażania automatycznego lub pojedynczy krok, należy wykonać te zadania wysokiego poziomu:

- Dodaj skrypt programu Windows PowerShell do rozwiązania i do kontroli źródła.
- Utwórz polecenie, które wywołuje skrypt programu Windows PowerShell.
- Usuń wszystkie zastrzeżone znaki XML polecenia.
- Tworzenie obiektu docelowego w pliku projektu MSBuild niestandardowych i używanie **Exec** zadań do uruchomienia polecenia.

W tym temacie opisano sposób wykonywania tych procedur. Zadania i wskazówki, w tym temacie założono, że znasz już docelowych elementów MSBuild i właściwości i że rozumiesz, jak używać niestandardowego pliku projektu MSBuild do dysków z procesem kompilacji i wdrażania. Aby uzyskać więcej informacji, zobacz [opis pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md) i [opis procesu kompilacji](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

## <a name="creating-and-adding-windows-powershell-scripts"></a>Tworzenie i dodawanie skryptów programu Windows PowerShell

Zadania w tym temacie używają przykładowy skrypt programu Windows PowerShell o nazwie **LogDeploy.ps1** ilustrujący sposób uruchamiania skryptów z programu MSBuild. **LogDeploy.ps1** skryptu zawiera proste funkcję, która zapisuje wpis jeden wiersz w pliku dziennika:


[!code-javascript[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample1.js)]


**LogDeploy.ps1** skrypt akceptuje dwa parametry. Pierwszy parametr reprezentuje pełną ścieżkę do pliku dziennika, do której chcesz dodać wpis, a drugi parametr reprezentuje miejsce docelowe wdrożenia, które mają być rejestrowane w pliku dziennika. Po uruchomieniu skryptu dodaje wiersza do pliku dziennika w następującym formacie:


[!code-html[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample2.html)]


Aby **LogDeploy.ps1** skryptów dostępne dla programu MSBuild, konieczne jest:

- Dodaj skrypt do kontroli źródła.
- Dodaj skrypt do rozwiązania w programie Visual Studio 2010.

Nie trzeba wdrażać skryptu zawartości rozwiązania, niezależnie od tego, czy zamierzasz uruchomić skrypt na serwerze kompilacji lub na komputerze zdalnym. Jedną z opcji jest dodać skrypt do folderu rozwiązania. W przykładzie kontaktów Menedżerze ponieważ chcesz użyć skryptu programu Windows PowerShell w ramach procesu wdrażania warto dodać skrypt do folderu rozwiązania publikowania.

![](running-windows-powershell-scripts-from-msbuild-project-files/_static/image1.png)

Zawartość folderów rozwiązania są kopiowane do tworzenia serwery jako źródło materiału. Jednak tworzą one żadna część żadnych danych wyjściowych projektu.

## <a name="executing-a-windows-powershell-script-on-the-build-server"></a>Wykonywanie skryptu programu Windows PowerShell na serwerze kompilacji

W niektórych scenariuszach możesz uruchamiać skrypty programu Windows PowerShell na komputerze, który tworzy projektów. Na przykład można użyć skryptu programu Windows PowerShell czyszczenie kompilacji folderów lub tworzyć wpisy w pliku dziennika niestandardowego.

Pod względem składni uruchamiając skrypt programu Windows PowerShell z pliku projektu MSBuild jest taka sama jak wykonywanie skryptu programu Windows PowerShell z regularnych wiersza polecenia. Należy wywołać powershell.exe pliku wykonywalnego i użyj **— polecenie** przełącznik, aby zapewnić polecenia, które mają środowiska Windows PowerShell do uruchamiania. (W programie Windows PowerShell v2, można również użyć **— plik** przełącznika). Polecenie należy wykonać ten format:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample3.cmd)]


Na przykład:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample4.cmd)]


Ścieżka do skryptu zawiera spacje, należy ująć ją w pliku w apostrofy poprzedzone znakiem. Nie można użyć podwójnego cudzysłowu, ponieważ został już użyty do umieść polecenie:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample5.cmd)]


Istnieje kilka uwagi dodatkowe po wywołaniu tego polecenia programu MSBuild. Najpierw należy dołączyć **— NonInteractive** flagę, aby upewnić się, że skrypt jest wykonywany ciche. Następnie należy uwzględnić **-ExecutionPolicy** flagę wartość argumentu odpowiednie. To ustawienie określa zasad wykonywania, czy środowisko Windows PowerShell będą stosowane do skryptu i pozwala zastąpić domyślne zasady wykonywania, które mogą uniemożliwić wykonanie skryptu. Możesz wybrać te wartości argumentu:

- Wartość **bez ograniczeń** umożliwi środowiska Windows PowerShell do wykonywania skryptu, niezależnie od tego, czy skrypt ma podpis.
- Wartość **RemoteSigned** umożliwi środowiska Windows PowerShell do wykonywania niepodpisanych skryptów, które zostały utworzone na komputerze lokalnym. Jednak skrypty, które zostały utworzone w innym miejscu muszą być podpisane. (W praktyce, wszystko jest mało prawdopodobne, aby został utworzony skrypt programu Windows PowerShell lokalnie na serwerze kompilacji).
- Wartość **AllSigned** umożliwi środowiska Windows PowerShell do wykonywania tylko skrypty podpisane.

Domyślne zasady wykonywania jest **Restricted**, co uniemożliwia uruchomienie żadnych plików skryptów środowiska Windows PowerShell.

Na koniec należy Usuń wszystkie zastrzeżone znaki XML występujących polecenia programu Windows PowerShell:

- Zastąp apostrofy z  **&amp;apos;**
- Zastąp znaki cudzysłowu  **&amp;quot;**
- Zastąp takie znaki z  **&amp;amp;**

- Po wprowadzeniu tych zmian, polecenie będzie wyglądać następująco:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample6.cmd)]


W pliku projektu MSBuild niestandardowego można utworzyć nowy obiekt docelowy i użyj **Exec** zadań, aby uruchomić to polecenie:


[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample7.xml)]


W tym przykładzie należy pamiętać, że:

- Wszelkie zmienne, takie jak wartości parametrów i lokalizacji pliku wykonywalnego programu Windows PowerShell, są deklarowane jako właściwości programu MSBuild.
- Aby umożliwić użytkownikom przesłonić te wartości w wierszu polecenia znajdują się warunki.
- **MSDeployComputerName** właściwości zadeklarowano w innym miejscu w pliku projektu.

Po wykonaniu tego obiektu docelowego jako część procesu kompilacji programu Windows PowerShell Uruchom polecenie i zapisać wpisu dziennika do podanego pliku.

## <a name="executing-a-windows-powershell-script-on-a-remote-computer"></a>Wykonywanie skryptu programu Windows PowerShell na komputerze zdalnym

Program Windows PowerShell jest może uruchamiać skrypty na komputerach zdalnych za pośrednictwem [zdalnego zarządzania systemem Windows](https://msdn.microsoft.com/library/windows/desktop/aa384426.aspx) (WinRM). Aby to zrobić, należy użyć [Invoke-Command](https://technet.microsoft.com/library/dd347578.aspx) polecenia cmdlet. Dzięki temu można wykonać skryptu na co najmniej jeden komputer zdalny bez kopiowania skrypt do komputerów zdalnych. Wyniki są zwracane do komputera lokalnego, z którego uruchomiono skrypt.

> [!NOTE]
> Przed użyciem **Invoke-Command** skrypty polecenia cmdlet do wykonania programu Windows PowerShell na komputerze zdalnym, należy skonfigurować odbiornik usługi WinRM do akceptowania zdalnego wiadomości. Można to zrobić, uruchamiając polecenie **winrm quickconfig** na komputerze zdalnym. Aby uzyskać więcej informacji, zobacz [instalacji i konfiguracji do zdalnego zarządzania systemem Windows](https://msdn.microsoft.com/library/windows/desktop/aa384372(v=vs.85).aspx).


Z okna programu Windows PowerShell, należy użyć następującej składni, aby Uruchom **LogDeploy.ps1** skryptu na komputerze zdalnym:


[!code-powershell[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample8.ps1)]


> [!NOTE]
> Istnieją różne sposoby używania **Invoke-Command** do uruchamiania skryptu pliku, ale ta metoda jest najbardziej oczywistym kiedy trzeba podać wartości parametrów i zarządzanie nimi ścieżki zawierające spacje.


Po uruchomieniu tej z wiersza polecenia, należy wywołać pliku wykonywalnego programu Windows PowerShell i użyć **— polecenie** parametr, aby dołączyć instrukcje:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample9.cmd)]


Ponieważ przed, musisz podać kilka dodatkowych przełączników i uniknięcie znaków zarezerwowanych XML, po uruchomieniu polecenia z MSBuild:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample10.cmd)]


Ostatecznie, jak poprzednio, korzystając z **Exec** zadanie w ramach niestandardowych docelowy programu MSBuild można wykonać polecenia:


[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample11.xml)]


Po wykonaniu tego obiektu docelowego jako część procesu kompilacji programu Windows PowerShell Uruchom skrypt na komputerze określonym w **-computername** argumentu.

## <a name="conclusion"></a>Wniosek

W tym temacie opisano sposób uruchamiania skryptu programu Windows PowerShell z pliku projektu programu MSBuild. Takie podejście umożliwia lokalnie lub na komputerze zdalnym, uruchom skrypt programu Windows PowerShell w trakcie procesu automatyczna lub pojedynczy krok kompilacji i wdrożenia.

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać wskazówki dotyczące podpisywania skryptów programu Windows PowerShell i zarządzanie zasadami wykonywania, zobacz [uruchomione skrypty programu Windows PowerShell](https://technet.microsoft.com/library/ee176949.aspx). Aby uzyskać wskazówki na temat uruchamiania poleceń programu Windows PowerShell z komputera zdalnego, zobacz [uruchamiania poleceń zdalnych](https://technet.microsoft.com/library/dd819505.aspx).

Aby uzyskać więcej informacji na temat używania niestandardowe pliki projektu MSBuild kontrolować proces wdrażania, zobacz [opis pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md) i [opis procesu kompilacji](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

> [!div class="step-by-step"]
> [Poprzednie](taking-web-applications-offline-with-web-deploy.md)
> [dalej](troubleshooting-the-packaging-process.md)
