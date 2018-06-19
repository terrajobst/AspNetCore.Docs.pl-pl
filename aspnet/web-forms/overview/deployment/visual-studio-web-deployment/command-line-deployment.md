---
uid: web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
title: 'Wdrażanie sieci Web ASP.NET przy użyciu programu Visual Studio: wdrożenia z wiersza polecenia | Dokumentacja firmy Microsoft'
author: tdykstra
description: Ta seria samouczek pokazuje, jak wdrożyć platformy ASP.NET (publikowanie) aplikacji do aplikacji sieci Web usługi aplikacji Azure lub innego dostawcy hostingu sieci web przez używane...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 82b8dea0-f062-4ee4-8784-3ffa30fbb1ca
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
msc.type: authoredcontent
ms.openlocfilehash: acc4a0e7f4744a3759b90e0f1b159da68b7c7362
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890531"
---
<a name="aspnet-web-deployment-using-visual-studio-command-line-deployment"></a>Wdrażanie sieci Web ASP.NET przy użyciu programu Visual Studio: wdrożenia z wiersza polecenia
====================
Przez [Dykstra niestandardowy](https://github.com/tdykstra)

[Pobierz początkowego projektu](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Ta seria samouczek pokazuje, jak wdrożyć platformy ASP.NET (publikowanie) aplikacji do aplikacji sieci Web usługi aplikacji Azure lub innego dostawcy hostingu sieci web za pomocą programu Visual Studio 2012 lub Visual Studio 2010. Aby uzyskać informacje dotyczące serii, zobacz [pierwszy samouczek z tej serii](introduction.md).


## <a name="overview"></a>Omówienie

W tym samouczku przedstawiono sposób wywołania sieci web programu Visual Studio publikowania potoku z wiersza polecenia. Jest to przydatne w scenariuszach, w którym ma zostać [zautomatyzować proces wdrażania](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) zamiast to zrobić ręcznie w programie Visual Studio, zwykle za pomocą [systemu kontroli wersji kodu źródła](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md).

## <a name="make-a-change-to-deploy"></a>Zmiany do wdrożenia

Na stronie informacje do wyświetlania kod szablonu.

![Strona z kodem szablonu — informacje](command-line-deployment/_static/image1.png)

Będzie to zastępującą z kodem, który wyświetla podsumowanie rejestracji dla użytkowników domowych.

Otwórz *About.aspx* pozycję usunięcie wszystkich znaczników wewnątrz `MainContent` `Content` elementu i Wstaw następujący kod w tym miejscu:

[!code-aspx[Main](command-line-deployment/samples/sample1.aspx)]

Uruchom projekt i wybierz **o** strony.

![Strona — informacje](command-line-deployment/_static/image2.png)

## <a name="deploy-to-test-by-using-the-command-line"></a>Wdróż do testu przy użyciu wiersza polecenia

Inna zmiana bazy danych, tak wyłączenie dbDacFx wdrożenia bazy danych dla bazy danych aspnet ContosoUniversity nie wdrażania. Otwórz **publikowanie w sieci Web** kreatora i w każdym z tych trzech profilów, wyczyść publikowania **aktualizacji bazy danych** pole wyboru na **ustawienia** kartę.

Na stronie Start systemu Windows 8, wyszukaj **wiersz polecenia dla VS2012 deweloperów**.

Kliknij prawym przyciskiem myszy ikonę **wiersz polecenia dla VS2012 deweloperów** i kliknij przycisk **Uruchom jako administrator**.

Wprowadź następujące polecenie w wierszu polecenia, zastępując ścieżkę do pliku rozwiązania ścieżka do pliku rozwiązania:

[!code-console[Main](command-line-deployment/samples/sample2.cmd)]

MSBuild rozwiązania tworzy i wdraża ją do środowiska testowego.

![Dane wyjściowe wiersza polecenia](command-line-deployment/_static/image3.png)

Otwórz przeglądarkę i przejdź do `http://localhost/ContosoUniversity`, następnie kliknij przycisk **o** stronę, aby sprawdzić, czy wdrożenie powiodło się.

Jeśli nie utworzono żadnych studentów w teście zostanie wyświetlona pusta strona w obszarze **statystyki treści uczniowie** nagłówka. Przejdź do **studentów** kliknij przycisk **dodać uczniowie**i dodać niektóre studentów, a następnie wróć do **o** stronę, aby wyświetlić statystyki dla użytkowników domowych.

![Strona w środowisku testowym — informacje](command-line-deployment/_static/image4.png)

## <a name="key-command-line-options"></a>Opcje wiersza polecenia klucza

Polecenie Wprowadzona ścieżka pliku rozwiązania i dwie właściwości przekazanego MSBuild:

[!code-console[Main](command-line-deployment/samples/sample3.cmd)]

### <a name="deploying-the-solution-versus-deploying-individual-projects"></a>Wdrażanie rozwiązania i wdrażania poszczególnych projektów

Określanie pliku rozwiązania powoduje, że wszystkie projekty w rozwiązaniu, które ma zostać utworzony. Jeśli masz wiele projektów sieci web w rozwiązaniu, stosowane są następujące rozwiązania programu MSBuild:

- Właściwości, które określają w wierszu polecenia są przekazywane do każdego projektu. W związku z tym każdy projekt sieci web musi mieć profil publikowania o tej nazwie, który określisz. Jeśli określisz `/p:PublishProfile=Test`, każdy projekt sieci web musi mieć profil publikowania o nazwie *testu*.
- Jeden projekt może opublikowanie, gdy inny nawet nie kompilacji. Aby uzyskać więcej informacji, zobacz wątku stackoverflow [MSBuild kończy się niepowodzeniem i dwa pakiety](http://stackoverflow.com/questions/14226451/msbuild-fails-with-two-packages).

Jeśli określisz poszczególne projektu zamiast rozwiązanie, należy dodać parametr, który określa wersję programu Visual Studio. Jeśli używasz programu Visual Studio 2012 w wierszu polecenia będzie podobny do poniższego przykładu:

[!code-console[Main](command-line-deployment/samples/sample4.cmd?highlight=1)]

Numer wersji dla programu Visual Studio 2010 jest 10.0. Aby uzyskać więcej informacji, zobacz [zgodności projektu programu Visual Studio i VisualStudioVersion](http://sedodream.com/2012/08/19/VisualStudioProjectCompatabilityAndVisualStudioVersion.aspx) na blogu Sayed Hashimi.

### <a name="specifying-the-publish-profile"></a>Określanie profilu publikowania

Można określić profil publikowania, według nazwy lub pełną ścieżkę do *.pubxml* plików, jak pokazano w poniższym przykładzie:

[!code-console[Main](command-line-deployment/samples/sample5.cmd?highlight=1)]

### <a name="web-publish-methods-supported-for-command-line-publishing"></a>Metody obsługiwany w przypadku publikowania wiersza polecenia publikowania w sieci Web

Publikowanie trzy metody są obsługiwane w przypadku publikowania w wierszu polecenia:

- `MSDeploy` -Publikowania za pomocą narzędzia Web Deploy.
- `Package` -Opublikuj przez utworzenie pakietu Web Deploy. Należy zainstalować pakiet niezależnie od polecenia MSBuild, który go utworzył.
- `FileSystem` -Opublikuj przez kopiowanie plików do określonego folderu.

### <a name="specifying-the-build-configuration-and-platform"></a>Określanie konfiguracji kompilacji i platform

W programie Visual Studio lub w wierszu polecenia można ustawić konfigurację kompilacji i platformy. Profile zawierają właściwości, które są nazywane `LastUsedBuildConfiguration` i `LastUsedPlatform`, ale nie może ustawić te właściwości, aby określić, jak projekt jest budowany. Aby uzyskać więcej informacji, zobacz [MSBuild: sposób ustawiania właściwości konfiguracji](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) na blogu Sayed Hashimi.

## <a name="deploy-to-staging"></a>Wdrażanie na przemieszczania

Aby wdrożyć na platformie Azure, musi dodać hasło w wierszu polecenia. Jeśli hasło jest zapisywane w profilu publikowania w programie Visual Studio, była przechowywana w postaci zaszyfrowanej w Twojej *. pubxml.user* pliku. Ten plik nie jest dostępna przez MSBuild po wykonaniu wdrożenia wiersza polecenia, dlatego należy przekazywać hasło w polu Parametry wiersza polecenia.

1. Kopiuj hasło, które należy z *.publishsettings* pliku pobranego wcześniej dla tymczasowej witryny sieci web. Hasło jest wartością `userPWD` atrybutu dla narzędzia Web Deploy `publishProfile` elementu.

    ![Web Deploy hasła](command-line-deployment/_static/image5.png)
2. Na stronie Start systemu Windows 8, wyszukaj **wiersz polecenia dla VS2012 deweloperów**i kliknij ikonę, aby otworzyć okno wiersza polecenia. (Nie trzeba otworzyć go jako administrator teraz, ponieważ nie jest wdrażana w usługach IIS na komputerze lokalnym).
3. Wprowadź następujące polecenie w wierszu polecenia, zastępując ścieżkę do pliku rozwiązania ścieżka do pliku rozwiązania i hasła przy użyciu hasła:

    [!code-console[Main](command-line-deployment/samples/sample6.cmd)]

    Powiadomienie, że ten wiersz polecenia zawiera dodatkowy parametr: `/p:AllowUntrustedCertificate=true`. Podczas zapisywania w tym samouczku `AllowUntrustedCertificate` musi być ustawiona właściwość podczas publikowania na platformie Azure z poziomu wiersza polecenia. Po zwolnieniu poprawkę dotyczącą tej usterki, nie trzeba tego parametru.
4. Otwórz przeglądarkę i przejdź do adresu URL witryny przemieszczania, a następnie kliknij **o** stronę, aby sprawdzić, czy wdrożenie powiodło się.

    Jak przedstawiono wcześniej dla środowiska testowego, być może trzeba utworzyć niektórych studentów, aby wyświetlić statystyki na **o** strony.

## <a name="deploy-to-production"></a>Wdrażanie w środowisku produkcyjnym

Proces wdrażania w środowisku produkcyjnym jest podobny do procesu przemieszczania.

1. Kopiuj hasło, które należy z *.publishsettings* pliku pobranego wcześniej dla witryny sieci web w środowisku produkcyjnym.
2. Otwórz **wiersz polecenia dla VS2012 deweloperów**.
3. Wprowadź następujące polecenie w wierszu polecenia, zastępując ścieżkę do pliku rozwiązania ścieżka do pliku rozwiązania i hasła przy użyciu hasła:

    [!code-console[Main](command-line-deployment/samples/sample7.cmd)]

    Dla lokacji rzeczywistej produkcji, jeśli została również zmianę w bazie danych, czy zwykle kopiowania *aplikacji\_offline.htm* plików do lokacji przed wdrożeniem i usuń go po pomyślnym wdrożeniu.
4. Otwórz przeglądarkę i przejdź do adresu URL witryny przemieszczania, a następnie kliknij **o** stronę, aby sprawdzić, czy wdrożenie powiodło się.

## <a name="summary"></a>Podsumowanie

Można teraz wdrożyć aktualizacji aplikacji przy użyciu wiersza polecenia.

![Strona w środowisku testowym — informacje](command-line-deployment/_static/image6.png)

W następnym samouczku zobaczysz przykładem rozszerzyć sieci web publikowanie potoku. Przykład opisano sposób wdrażania plików, które nie znajdują się w projekcie.

> [!div class="step-by-step"]
> [Poprzednie](deploying-a-database-update.md)
> [dalej](deploying-extra-files.md)
