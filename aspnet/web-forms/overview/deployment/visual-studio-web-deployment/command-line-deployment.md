---
uid: web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
title: 'Wdrażanie aplikacji internetowych ASP.NET przy użyciu programu Visual Studio: wdrażanie z wiersza polecenia | Dokumentacja firmy Microsoft'
author: tdykstra
description: W tej serii samouczków dowiesz się, jak wdrożyć (opublikować) platformy ASP.NET sieci web aplikacji do usługi Azure App Service Web Apps lub dostawcy hostingu w innych firm, używane...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 82b8dea0-f062-4ee4-8784-3ffa30fbb1ca
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
msc.type: authoredcontent
ms.openlocfilehash: 5673a4733257fae88fb66a3da43dfceb98c3b37a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37390877"
---
<a name="aspnet-web-deployment-using-visual-studio-command-line-deployment"></a>Wdrażanie aplikacji internetowych ASP.NET przy użyciu programu Visual Studio: wdrażanie z wiersza polecenia
====================
przez [Tom Dykstra](https://github.com/tdykstra)

[Pobieranie projektu startowego](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> W tej serii samouczków dowiesz się, jak wdrożyć (opublikować) platformy ASP.NET sieci web aplikacji do usługi Azure App Service Web Apps lub innych firm dostawcy hostingu za pomocą programu Visual Studio 2012 lub Visual Studio 2010. Aby uzyskać informacje na temat serii, zobacz [pierwszym samouczku tej serii](introduction.md).


## <a name="overview"></a>Omówienie

W tym samouczku przedstawiono sposób wywołania sieci web programu Visual Studio publikowania potoku z poziomu wiersza polecenia. Jest to przydatne w scenariuszach, w którym chcesz [zautomatyzować proces wdrażania](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) zamiast to zrobić ręcznie w programie Visual Studio, zwykle za pomocą [systemu kontroli wersji kodu źródła](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md).

## <a name="make-a-change-to-deploy"></a>Wprowadź zmiany do wdrożenia

Obecnie na stronie Informacje przedstawia kod szablonu.

![Informacje o stronie przy użyciu kodu szablonu](command-line-deployment/_static/image1.png)

Zastąpisz, z kodem, który wyświetla podsumowanie rejestracji dla uczniów.

Otwórz *About.aspx* strony, Usuń wszystkie znaczniki wewnątrz `MainContent` `Content` elementu i Wstaw następujący kod w jego miejsce:

[!code-aspx[Main](command-line-deployment/samples/sample1.aspx)]

Uruchom projekt i wybierz **o** strony.

![Informacje o stronie](command-line-deployment/_static/image2.png)

## <a name="deploy-to-test-by-using-the-command-line"></a>Wdrażanie do testu przy użyciu wiersza polecenia

Nie będzie wdrażanie inną zmianę w bazie danych, tak wyłączenie dbDacFx wdrożenia bazy danych dla bazy danych aspnet ContosoUniversity. Otwórz **publikowanie w sieci Web** kreatora i we wszystkich trzech profilów, wyczyść publikowania **Aktualizuj bazę danych** pole wyboru na **ustawienia** kartę.

Na stronie początkowy systemu Windows 8, wyszukaj **wiersz polecenia programisty dla VS2012**.

Kliknij prawym przyciskiem myszy ikonę **wiersz polecenia programisty dla VS2012** i kliknij przycisk **Uruchom jako administrator**.

Wprowadź następujące polecenie w wierszu polecenia, zastępując ścieżkę do pliku rozwiązania przy użyciu ścieżki do pliku rozwiązania:

[!code-console[Main](command-line-deployment/samples/sample2.cmd)]

Program MSBuild kompiluje rozwiązanie i wdraża ją do środowiska testowego.

![Dane wyjściowe wiersza polecenia](command-line-deployment/_static/image3.png)

Otwórz przeglądarkę i przejdź do `http://localhost/ContosoUniversity`, następnie kliknij przycisk **o** stronę, aby sprawdzić, czy wdrożenie zakończyło się pomyślnie.

Jeśli nie utworzono żadnych studentów w teście, zobaczysz pusta strona w obszarze **statystyki treści uczniów** nagłówka. Przejdź do **studentów** kliknij **dodać uczniów**i Dodaj pewne studenci, a następnie wróć do **o** stronę, aby wyświetlić statystyki dla uczniów.

![Informacje o stronie w środowisku testowym](command-line-deployment/_static/image4.png)

## <a name="key-command-line-options"></a>Opcje klucza wiersza polecenia

Polecenie, które zostały wprowadzone, przekazywane do MSBuild ścieżka pliku rozwiązania i dwie właściwości:

[!code-console[Main](command-line-deployment/samples/sample3.cmd)]

### <a name="deploying-the-solution-versus-deploying-individual-projects"></a>Wdrażanie rozwiązania i wdrażania poszczególnych projektów

Określanie pliku rozwiązania powoduje, że wszystkie projekty w rozwiązaniu, które ma zostać utworzony. Jeśli masz wiele projektów sieci web w rozwiązaniu, mają zastosowanie następujące zachowanie programu MSBuild:

- Właściwości, należy określić w wierszu polecenia, które są przekazywane do każdego projektu. W związku z tym każdy projekt sieci web musi mieć profil publikowania o podanej nazwie. Jeśli określisz `/p:PublishProfile=Test`, każdy projekt sieci web musi mieć profil publikowania o nazwie *testu*.
- Jeden projekt może opublikowanie, gdy inny nawet nie da się skompilować. Aby uzyskać więcej informacji, zobacz wątek w witrynie stackoverflow [MSBuild nie powiedzie się z dwóch pakietów](http://stackoverflow.com/questions/14226451/msbuild-fails-with-two-packages).

Jeśli określisz pojedynczego projektu zamiast rozwiązania, należy dodać parametr określający używaną wersję programu Visual Studio. Jeśli używasz programu Visual Studio 2012 w wierszu polecenia będzie podobny do poniższego przykładu:

[!code-console[Main](command-line-deployment/samples/sample4.cmd?highlight=1)]

Numer wersji dla programu Visual Studio 2010 jest 10.0. Aby uzyskać więcej informacji, zobacz [ze zgodnością projektów programu Visual Studio i VisualStudioVersion](http://sedodream.com/2012/08/19/VisualStudioProjectCompatabilityAndVisualStudioVersion.aspx) na blogu Sayed Hashimi.

### <a name="specifying-the-publish-profile"></a>Określanie profilu publikowania

Można określić profil publikowania, według nazwy lub pełną ścieżkę do *.pubxml* pliku, jak pokazano w poniższym przykładzie:

[!code-console[Main](command-line-deployment/samples/sample5.cmd?highlight=1)]

### <a name="web-publish-methods-supported-for-command-line-publishing"></a>W sieci Web publikowanie metody obsługiwany w przypadku publikowania wiersza polecenia

Publikowanie trzy metody są obsługiwane w przypadku publikowania w wierszu polecenia:

- `MSDeploy` -Publikowanie za pomocą narzędzia Web Deploy.
- `Package` -Opublikować, tworząc pakiet wdrażania sieci Web. Musisz zainstalować pakiet, niezależnie od polecenia programu MSBuild, które tworzy go.
- `FileSystem` -Opublikować kopiować pliki do określonego folderu.

### <a name="specifying-the-build-configuration-and-platform"></a>Określenie konfiguracji kompilacji i platformy

W programie Visual Studio lub w wierszu polecenia można ustawić konfigurację kompilacji i platformy. Profile publikowania zawierają właściwości, które są nazywane `LastUsedBuildConfiguration` i `LastUsedPlatform`, ale nie ustawisz tych właściwości w celu określenia, jak projekt jest kompilowany. Aby uzyskać więcej informacji, zobacz [MSBuild: sposób ustawiania właściwości konfiguracji](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) na blogu Sayed Hashimi.

## <a name="deploy-to-staging"></a>Wdrażanie w środowisku przejściowym

Aby wdrożyć na platformie Azure, należy dodać hasło w wierszu polecenia. Jeśli hasło jest zapisywane w profilu publikowania w programie Visual Studio, została zapisana w zaszyfrowanym formacie w swojej *. pubxml.user* pliku. Ten plik nie jest dostępny przez program MSBuild, po wykonaniu wdrażanie z wiersza polecenia, więc trzeba przekazać hasło parametr wiersza polecenia.

1. Skopiuj hasło, które wymagają od *.publishsettings* pliku, który został pobrany wcześniej dla tymczasowej witryny sieci web. Hasło jest wartością `userPWD` atrybutu dla narzędzia Web Deploy `publishProfile` elementu.

    ![Web Deploy hasła](command-line-deployment/_static/image5.png)
2. Na stronie początkowy systemu Windows 8, wyszukaj **wiersz polecenia programisty dla VS2012**, a następnie kliknij ikonę, aby otworzyć wiersz polecenia. (Nie trzeba otworzyć go jako administratora tej chwili, ponieważ nie są wdrażanie w usługach IIS, na komputerze lokalnym).
3. Wprowadź następujące polecenie w wierszu polecenia, zastępując ścieżkę do pliku rozwiązania przy użyciu ścieżki do pliku rozwiązania i hasło, za pomocą hasła:

    [!code-console[Main](command-line-deployment/samples/sample6.cmd)]

    Zwróć uwagę, że ten wiersz polecenia zawiera dodatkowy parametr: `/p:AllowUntrustedCertificate=true`. Podczas zapisywania w tym samouczku `AllowUntrustedCertificate` podczas publikowania na platformie Azure z poziomu wiersza polecenia, należy ustawić właściwość. Po zwolnieniu poprawkę dotyczącą tej usterki nie wymaga tego parametru.
4. Otwórz przeglądarkę i przejdź do adresu URL witryny tymczasowej, a następnie kliknij **o** stronę, aby sprawdzić, czy wdrożenie zakończyło się pomyślnie.

    Jak przedstawiono wcześniej dla środowiska testowego, być może trzeba utworzyć Niektórzy studenci, aby wyświetlić statystyki na **o** strony.

## <a name="deploy-to-production"></a>Wdrażanie w środowisku produkcyjnym

Proces wdrażania w środowisku produkcyjnym jest podobny do procesu przejściowego.

1. Skopiuj hasło, które wymagają od *.publishsettings* pliku, który został pobrany wcześniej dla witryny sieci web w środowisku produkcyjnym.
2. Otwórz **wiersz polecenia programisty dla VS2012**.
3. Wprowadź następujące polecenie w wierszu polecenia, zastępując ścieżkę do pliku rozwiązania przy użyciu ścieżki do pliku rozwiązania i hasło, za pomocą hasła:

    [!code-console[Main](command-line-deployment/samples/sample7.cmd)]

    Dla witryny rzeczywistej produkcji, jeśli została również zmianę w bazie danych, będzie zazwyczaj kopiujesz *aplikacji\_offline.htm* plików do lokacji, przed przystąpieniem do wdrożenia i usuniesz go po pomyślnym wdrożeniu.
4. Otwórz przeglądarkę i przejdź do adresu URL witryny tymczasowej, a następnie kliknij **o** stronę, aby sprawdzić, czy wdrożenie zakończyło się pomyślnie.

## <a name="summary"></a>Podsumowanie

Masz teraz wdrożyć aktualizację aplikacji przy użyciu wiersza polecenia.

![Informacje o stronie w środowisku testowym](command-line-deployment/_static/image6.png)

W następnym samouczku, zostanie wyświetlony przykładem sposobu rozszerzenia sieci web potoku publikowania. Przykład będzie pokazują, jak wdrożyć pliki, które nie są uwzględnione w projekcie.

> [!div class="step-by-step"]
> [Poprzednie](deploying-a-database-update.md)
> [dalej](deploying-extra-files.md)
