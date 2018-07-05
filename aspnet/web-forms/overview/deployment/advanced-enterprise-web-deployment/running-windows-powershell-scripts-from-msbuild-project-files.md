---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
title: Uruchamianie skryptów programu Windows PowerShell z plików projektu MSBuild | Dokumentacja firmy Microsoft
author: jrjlee
description: W tym temacie opisano sposób uruchamiania skryptu programu Windows PowerShell jako część procesu kompilacji i wdrażania. Skrypt można uruchomić lokalnie (innymi słowy, na komputerze b...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 55f1ae45-fcb5-43a9-8415-fa5b935fc9c9
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
msc.type: authoredcontent
ms.openlocfilehash: ddb658d8a8f224a7c417321df3e17ce0610d2473
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362898"
---
<a name="running-windows-powershell-scripts-from-msbuild-project-files"></a>Uruchamianie skryptów programu Windows PowerShell z plików projektu MSBuild
====================
przez [Jason Lee](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie opisano sposób uruchamiania skryptu programu Windows PowerShell jako część procesu kompilacji i wdrażania. Możesz uruchomić skrypt lokalnie (innymi słowy, na serwerze kompilacji), lub zdalnie, takich jak na docelowym serwerze sieci web lub serwera bazy danych.
> 
> Istnieje wiele powodów dlaczego warto uruchomić skrypt po wdrożeniu programu Windows PowerShell. Na przykład możesz chcieć:
> 
> - Dodawanie źródła zdarzeń niestandardowych do rejestru.
> - Generowanie katalogu w systemie plików przekazywania.
> - Wyczyść katalogów kompilacji.
> - Zapisywanie wpisów w pliku dziennika niestandardowego.
> - Wysyłaj wiadomości e-mail z zaproszeniem użytkowników do aplikacji sieci web użytkownicy nowo aprowizowanych.
> - Utwórz konta użytkowników z odpowiednimi uprawnieniami.
> - Skonfiguruj replikację między wystąpieniami programu SQL Server.
> 
> W tym temacie pokazują sposób uruchamiania skryptów programu Windows PowerShell, lokalnie i zdalnie z niestandardowe obiekty docelowe w pliku projektu aparatu Microsoft Build Engine (MSBuild).


Ten temat jest częścią serii samouczków na podstawie wymagania dotyczące wdrażania enterprise fikcyjnej firmy o nazwie firmy Fabrikam, Inc. Przykładowe rozwiązanie korzysta z tej serii samouczków&#x2014; [rozwiązania Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;do reprezentowania aplikacji sieci web przy użyciu realistycznej stopień złożoności, łącznie z aplikacją ASP.NET MVC 3 komunikacji Windows Usługa Foundation (WCF), a projekt bazy danych.

Metody wdrażania w ramach tego samouczka opiera się na podejście pliku projektu Podziel opisane w [objaśnienie pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), w którym proces kompilacji jest kontrolowana przez dwa pliki projektu&#x2014;jeden zawierający Tworzenie instrukcji, które mają zastosowanie do każdego środowiska docelowego i jeden zawierający ustawienia specyficzne dla środowiska kompilacji i wdrażania. W czasie kompilacji pliku projektu specyficznymi dla środowiska jest scalana w pliku projektu niezależnego od środowiska w celu utworzenia kompletny zestaw instrukcji kompilacji.

## <a name="task-overview"></a>Omówienie zadań

Aby uruchomić skrypt programu Windows PowerShell w ramach procesu wdrażania automatycznego lub pojedynczy krok, należy wykonać te zadania wysokiego poziomu:

- Dodaj skrypt programu Windows PowerShell do rozwiązania do kontroli źródła.
- Utwórz polecenie, które wywołuje skrypt programu Windows PowerShell.
- Znak ucieczki znaków zarezerwowanych XML w poleceniu.
- Tworzenie elementu docelowego w pliku projektu MSBuild niestandardowych i używanie **Exec** zadania do uruchomienia polecenia.

W tym temacie pokazują sposób wykonania tych procedur. Zadania i wskazówki, w tym temacie założono, że znasz już właściwości i elementy docelowe programu MSBuild, i że rozumiesz, jak używać niestandardowego pliku projektu MSBuild do procesu kompilacji i wdrażania. Aby uzyskać więcej informacji, zobacz [objaśnienie pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md) i [objaśnienie procesu kompilacji](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

## <a name="creating-and-adding-windows-powershell-scripts"></a>Tworzenie i dodawanie skryptów programu Windows PowerShell

Zadania przedstawione w tym temacie Użyj przykładowy skrypt programu Windows PowerShell o nazwie **LogDeploy.ps1** ilustruje sposób uruchamiania skryptów z programu MSBuild. **LogDeploy.ps1** skrypt zawiera prostej funkcji, która zapisuje wpis jeden wiersz w pliku dziennika:


[!code-javascript[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample1.js)]


**LogDeploy.ps1** skrypt akceptuje dwa parametry. Pierwszy parametr reprezentuje pełną ścieżkę do pliku dziennika, do którego chcesz dodać wpis, a drugi parametr reprezentuje miejsce docelowe wdrożenia, które mają być rejestrowane w pliku dziennika. Po uruchomieniu skryptu, dodaje wiersz do pliku dziennika w następującym formacie:


[!code-html[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample2.html)]


Aby **LogDeploy.ps1** skryptu dostępne dla programu MSBuild, należy:

- Dodaj skrypt do kontroli źródła.
- Dodaj skrypt do rozwiązania w programie Visual Studio 2010.

Nie trzeba wdrożyć skrypt za pomocą rozwiązania zawartości, niezależnie od tego, czy jest planowane do uruchomienia skryptu na serwerze kompilacji lub na komputerze zdalnym. Jedną z opcji jest Dodawanie skryptu do folderu rozwiązania. W przykładzie Contact Manager ponieważ chcesz użyć skryptu programu Windows PowerShell jako część procesu wdrażania, dobrym pomysłem będzie Dodaj skrypt do folderu publikowania rozwiązania.

![](running-windows-powershell-scripts-from-msbuild-project-files/_static/image1.png)

Zawartość folderów rozwiązania są kopiowane do serwerów kompilacji jako materiał źródłowy. Jednak część nie żadnych danych wyjściowych projektu.

## <a name="executing-a-windows-powershell-script-on-the-build-server"></a>Wykonywanie skryptu programu Windows PowerShell na serwerze kompilacji

W niektórych przypadkach warto uruchamiać skrypty programu Windows PowerShell na komputerze, który tworzy swoje projekty. Może na przykład użyć skryptu programu Windows PowerShell, czyszczenie kompilacji folderów lub zapisywanie wpisów w pliku dziennika niestandardowego.

Pod względem składni jest taka sama jak uruchomienie skryptu programu Windows PowerShell z wiersza polecenia, regularne uruchamianie skryptu programu Windows PowerShell z pliku projektu programu MSBuild. Musisz wywołać powershell.exe pliku wykonywalnego i użyj **— polecenie** przełącznika, aby zapewnić polecenia, które mają programu Windows PowerShell do uruchamiania. (W programie Windows PowerShell w wersji 2 umożliwia także **— plik** przełącznika). Polecenie należy wykonać następujący format:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample3.cmd)]


Na przykład:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample4.cmd)]


Ścieżka do skryptu zawiera spacje, należy ująć ją w pliku w pojedynczym cudzysłowie poprzedzone handlowe "i". Nie można użyć podwójnych cudzysłowów, ponieważ został już użyty do należy wpisać polecenie:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample5.cmd)]


Istnieje kilka dodatkowych kwestii dotyczących po wywołaniu tego polecenia z programu MSBuild. Po pierwsze należy uwzględnić **— NonInteractive** flagi, aby upewnić się, że skrypt wykonuje ciche. Następnie należy uwzględnić **-ExecutionPolicy** flagi z wartością odpowiedniego argumentu. To ustawienie określa zasady wykonywania, że program Windows PowerShell będzie miała zastosowanie do skryptu i pozwala zastąpić domyślne zasady wykonywania, które mogą uniemożliwić wykonywanie skryptu. Możesz wybrać spośród tych wartości argumentu:

- Wartość **Unrestricted** umożliwi programu Windows PowerShell w celu wykonania skryptu, niezależnie od tego, czy skrypt jest podpisany.
- Wartość **RemoteSigned** umożliwi programu Windows PowerShell do wykonania niepodpisanych skryptów, które zostały utworzone na komputerze lokalnym. Jednak muszą być podpisane skrypty, które zostały utworzone w innym miejscu. (W praktyce jest bardzo mało prawdopodobne, aby zostały utworzone skrypt programu Windows PowerShell lokalnie na serwerze kompilacji).
- Wartość **wszystkie podpisane** umożliwi programu Windows PowerShell do wykonywania tylko skrypty podpisane.

Domyślne zasady wykonywania jest **ograniczeniami**, co uniemożliwia uruchamianie wszystkich plików skryptów programu Windows PowerShell.

Na koniec należy jako znak ucieczki dla wszelkich zarezerwowanych znaków XML, które występują w polecenia programu Windows PowerShell:

- Zastąp apostrofy z  **&amp;apos;**
- Zastąp znaki cudzysłowu  **&amp;quot;**
- Zastąp takie znaki z  **&amp;amp;**

- Po wprowadzeniu tych zmian, polecenie będzie wyglądać następująco:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample6.cmd)]


W ramach Twojego niestandardowego pliku projektu MSBuild, można utworzyć nowy obiekt docelowy i użyć **Exec** zadania do uruchomienia tego polecenia:


[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample7.xml)]


W tym przykładzie należy zauważyć, że:

- Wszelkie zmienne, takie jak wartości parametrów i lokalizację pliku wykonywalnego programu Windows PowerShell, są deklarowane jako właściwości programu MSBuild.
- Aby umożliwić użytkownikom zastąpić te wartości w wierszu polecenia znajdują się warunki.
- **MSDeployComputerName** właściwość jest deklarowana w innym miejscu w pliku projektu.

Po wykonaniu ten element docelowy jako część procesu kompilacji programu Windows PowerShell Uruchom polecenie i zapisać wpisu dziennika do podanego pliku.

## <a name="executing-a-windows-powershell-script-on-a-remote-computer"></a>Wykonywanie skryptu programu Windows PowerShell na komputerze zdalnym

Programu Windows PowerShell jest możliwe uruchamianie skryptów na komputerach zdalnych za pośrednictwem [Windows Remote Management](https://msdn.microsoft.com/library/windows/desktop/aa384426.aspx) (WinRM). Aby to zrobić, należy użyć [Invoke-Command](https://technet.microsoft.com/library/dd347578.aspx) polecenia cmdlet. Umożliwia to wykonywanie skryptu względem co najmniej jeden komputer zdalny bez kopiowania skryptu z komputerami zdalnymi. Wszystkie wyniki są zwracane do komputera lokalnego, z którego uruchomiono skrypt.

> [!NOTE]
> Przed użyciem **Invoke-Command** polecenia cmdlet programu Windows PowerShell do wykonywania skryptów na komputerze zdalnym, należy skonfigurować odbiornik usługi WinRM do akceptowania wiadomości zdalne. Można to zrobić, uruchamiając polecenie **winrm quickconfig** na komputerze zdalnym. Aby uzyskać więcej informacji, zobacz [instalacji i konfiguracji dla Windows zdalne zarządzanie](https://msdn.microsoft.com/library/windows/desktop/aa384372(v=vs.85).aspx).


W oknie programu Windows PowerShell, ta składnia będzie używane do uruchamiania **LogDeploy.ps1** skryptu na komputerze zdalnym:


[!code-powershell[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample8.ps1)]


> [!NOTE]
> Istnieją różne sposoby korzystania z **Invoke-Command** do uruchamiania skryptu pliku, ale ta metoda jest najbardziej proste kiedy trzeba będzie podać wartości parametrów i zarządzanie nimi ścieżki zawierające spacje.


Po uruchomieniu tego z poziomu wiersza polecenia, musisz wywołać z programu Windows PowerShell pliku wykonywalnego i użyj **— polecenie** parametru, aby dołączyć instrukcje:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample9.cmd)]


Jak wcześniej, musisz podać kilka dodatkowych przełączników i znak ucieczki znaków zarezerwowanych XML, po uruchomieniu polecenia z programu MSBuild:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample10.cmd)]


Na koniec, tak jak poprzednio, możesz użyć **Exec** zadanie w ramach niestandardowy cel programu MSBuild do wykonania polecenia:


[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample11.xml)]


Po wykonaniu ten element docelowy jako część procesu kompilacji programu Windows PowerShell uruchomić skrypt na komputerze określonym w **– computername** argumentu.

## <a name="conclusion"></a>Wniosek

W tym temacie opisano sposób uruchamiania skryptu programu Windows PowerShell z pliku projektu programu MSBuild. Ta metoda służy do uruchamiania skryptu programu Windows PowerShell, lokalnie lub na komputerze zdalnym, w ramach automatycznych lub pojedynczy krok procesu kompilowania i wdrażania.

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać wskazówki dotyczące podpisywania skryptów programu Windows PowerShell i zarządzanie zasadami wykonywania, zobacz [uruchamiania skryptów programu Windows PowerShell](https://technet.microsoft.com/library/ee176949.aspx). Aby uzyskać wskazówki na temat uruchamiania poleceń programu Windows PowerShell z komputera zdalnego, zobacz [uruchamianie poleceń zdalnych](https://technet.microsoft.com/library/dd819505.aspx).

Aby uzyskać więcej informacji na temat korzystania z niestandardowych plików projektu MSBuild do kontrolowania procesu wdrażania, zobacz [objaśnienie pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md) i [objaśnienie procesu kompilacji](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

> [!div class="step-by-step"]
> [Poprzednie](taking-web-applications-offline-with-web-deploy.md)
> [dalej](troubleshooting-the-packaging-process.md)
