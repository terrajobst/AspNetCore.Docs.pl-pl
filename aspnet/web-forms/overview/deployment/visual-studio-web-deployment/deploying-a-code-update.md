---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
title: 'Wdrażanie aplikacji internetowych ASP.NET przy użyciu programu Visual Studio: Wdrażanie aktualizacji kodu | Dokumentacja firmy Microsoft'
author: tdykstra
description: W tej serii samouczków dowiesz się, jak wdrożyć (opublikować) platformy ASP.NET sieci web aplikacji do usługi Azure App Service Web Apps lub dostawcy hostingu w innych firm, używane...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: c76dbc35-a914-4ee3-919c-4f4d1fa05104
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
msc.type: authoredcontent
ms.openlocfilehash: 3649f0250e830b76c42d832e51038c2c52ed4561
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368217"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-code-update"></a>Wdrażanie aplikacji internetowych ASP.NET przy użyciu programu Visual Studio: Wdrażanie aktualizacji kodu
====================
przez [Tom Dykstra](https://github.com/tdykstra)

[Pobieranie projektu startowego](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> W tej serii samouczków dowiesz się, jak wdrożyć (opublikować) platformy ASP.NET sieci web aplikacji do usługi Azure App Service Web Apps lub innych firm dostawcy hostingu za pomocą programu Visual Studio 2012 lub Visual Studio 2010. Aby uzyskać informacje na temat serii, zobacz [pierwszym samouczku tej serii](introduction.md).


## <a name="overview"></a>Omówienie

Po początkowym wdrożeniu kontynuuje swoją pracę, obsługi i tworzenia witryny sieci web, a niedługo będzie, aby wdrożyć aktualizację. Ten samouczek przeprowadzi Cię przez proces wdrażania aktualizacji w kodzie aplikacji. Aktualizację, implementowania i wdrażania w ramach tego samouczka nie wiąże się zmianę w bazie danych; zobaczysz, jakie są różnice dotyczące wdrażania w następnym samouczku zmianę w bazie danych.

Przypomnienie: Jeśli otrzymujesz komunikat o błędzie lub coś nie działa podczas wykonywania kroków samouczka, upewnij się sprawdzić [strona rozwiązywania problemów](troubleshooting.md).

## <a name="make-a-code-change"></a>Wprowadź zmiana w kodzie

Prostym przykładem aktualizacji do aplikacji, należy dodać do **Instruktorzy** listę kursom prowadzonym przez instruktora wybranej strony.

Po uruchomieniu **Instruktorzy** strony, można zauważyć, że istnieją **wybierz** łącza w siatce, ale ich nie robi niczego poza upewnij wiersz szary Włącz w tle.

![Strona Instruktorzy z zaznaczenia](deploying-a-code-update/_static/image1.png)

Teraz dodasz kod, który jest uruchamiany, gdy **wybierz** łącze kliknięciu i wyświetla listę kursom prowadzonym przez instruktora wybrane.

1. W *Instructors.aspx*, Dodaj następujący kod bezpośrednio po **ErrorMessageLabel** `Label` sterowania:

    [!code-aspx[Main](deploying-a-code-update/samples/sample1.aspx)]
2. Uruchom strony i wybierz pod kierunkiem instruktora. Zobaczysz listę kursom prowadzonym przez instruktora tego.

    ![Strona Instruktorzy z kursom prowadzonym](deploying-a-code-update/_static/image2.png)
3. Zamknij przeglądarkę.

## <a name="deploy-the-code-update-to-the-test-environment"></a>Wdrażanie aktualizacji kodu do środowiska testowego

Zanim użyjesz profilów publikowania do wdrożenia do testowania, przejściowego i produkcji, należy zmienić opcje publikowania bazy danych. Nie trzeba uruchamiać skrypty wdrażania danych i udzielanie dla bazy danych członkostwa.

1. Otwórz **publikowanie w sieci Web** kreatora, kliknij prawym przyciskiem myszy projekt ContosoUniversity i klikając **Publikuj**.
2. Kliknij przycisk **testu** profil w **profilu** listy rozwijanej.
3. Kliknij przycisk **ustawienia** kartę.
4. W obszarze **DefaultConnection** w **baz danych** sekcji wyczyść **Aktualizuj bazę danych** pole wyboru.
5. Kliknij przycisk **profilu** kartę, a następnie kliknij przycisk **przemieszczania** profil w **profilu** listy rozwijanej.
6. Gdy zostanie wyświetlony monit Jeśli chcesz zapisać zmiany wprowadzone do **testu** profilu, kliknij przycisk **tak**.
7. Wprowadź tę samą zmianę w profilu tymczasowego.
8. Powtórz te czynności, aby wprowadzić tę samą zmianę w profilu w środowisku produkcyjnym.
9. Zamknij **publikowanie w sieci Web** kreatora.

Wdrażanie do środowiska testowego jest już polegać na uruchamianie jednym kliknięciem opublikuj go ponownie. Aby ułatwić ten proces szybciej, można użyć **Web publikacja w pojedynczym kliknięciem** paska narzędzi.

1. W **widoku** menu, wybierz **pasków narzędzi** , a następnie wybierz **Web publikacja w pojedynczym kliknięciem**.

    ![Selecting_One_Click_Publish_toolbar](deploying-a-code-update/_static/image3.png)
2. W **Eksploratora rozwiązań**, wybierz projekt ContosoUniversity.
3. **Web publikacja w pojedynczym kliknięciem** narzędzi, wybierz **testu** profil publikowania, a następnie kliknij przycisk **publikowanie w sieci Web** (ikona za pomocą strzałki w lewo i w prawo).

    ![Web_One_Click_Publish_toolbar](deploying-a-code-update/_static/image4.png)
4. Program Visual Studio wdroży zaktualizowaną aplikację, i automatycznie otwiera się przeglądarka do strony głównej.
5. Uruchom stronę instruktorów i wybierz pod kierunkiem instruktora, aby sprawdzić, czy aktualizacja została wdrożona pomyślnie.

Normalny również wykonać testowanie regresji (oznacza to, że test pozostałych lokacji, aby upewnić się, że nowa zmiana nie przerwać wszelkie istniejące funkcje). Jednak na potrzeby tego samouczka będziesz pominąć ten krok i przejdź do wdrażania aktualizacji przejściowym i produkcyjnym.

Podczas ponownego wdrażania, narzędzie Web Deploy automatycznie określa które pliki uległy zmianie, a kopie tylko zmienione pliki na serwer. Domyślnie narzędzie Web Deploy korzysta z daty ostatniej zmiany plików w celu określenia, które uległy zmianie. Niektóre systemów kontroli źródła zmienić plik daty, nawet gdy nie zmieniaj zawartości pliku. W takim przypadku można skonfigurować narzędzie Web Deploy używać sum kontrolnych plików do określenia, które pliki uległy zmianie. Aby uzyskać więcej informacji, zobacz [Dlaczego wszystkie pliki Rozpoczynanie ponownego wdrożenia, ale nie je zmienić?](https://msdn.microsoft.com/library/ee942158.aspx#use_checksum) w często zadawanych Pytaniach wdrożenia dla platformy ASP.NET.

## <a name="take-the-application-offline-during-deployment"></a>Nastavit aplikaci offline podczas wdrażania

Zmiana, którą jest wdrażany jest teraz prostą zmianę do jednej strony. Czasami wdrażanie większe zmiany lub wdrażanie zmian w kodzie i bazy danych — a lokacji może działać nieprawidłowo, jeśli użytkownik zgłasza żądanie strony przed zakończeniem wdrażania. Aby uniemożliwić użytkownikom dostęp do witryny, w trakcie wdrażania, można użyć *aplikacji\_offline.htm* pliku. Po umieszczeniu pliku o nazwie *aplikacji\_offline.htm* w folderze głównym aplikacji, usługi IIS automatycznie wyświetla ten plik, zamiast uruchamiania aplikacji. Tak, aby uniemożliwić dostęp podczas wdrażania, możesz umieścić *aplikacji\_offline.htm* w folderze głównym Uruchom proces wdrażania, a następnie usuń *aplikacji\_offline.htm* po pomyślnym wdrożenie.

Można skonfigurować narzędzie Web Deploy, aby automatycznie wprowadzić domyślny *aplikacji\_offline.htm* plików na serwerze, po uruchomieniu, wdrażanie i usunąć go po zakończeniu tej operacji. Zrobić, że wszystkie trzeba jest Dodaj następujący element XML do pliku (.pubxml) profil publikowania:

[!code-xml[Main](deploying-a-code-update/samples/sample2.xml)]

W tym samouczku pokazano, jak utworzyć i używać niestandardowego *aplikacji\_offline.htm* pliku.

Za pomocą *aplikacji\_offline.htm* w witrynie etapowej nie jest wymagane, ponieważ nie masz użytkowników uzyskujących dostęp do tymczasowej witryny. Ale jest dobrą praktyką na potrzeby przemieszczania przetestować wszystko, co w sposób, który ma zostać umieszczony w środowisku produkcyjnym.

### <a name="create-appofflinehtm"></a>Tworzenie aplikacji\_offline.htm

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy rozwiązanie i kliknij przycisk **Dodaj**, a następnie kliknij przycisk **nowy element**.
2. Tworzenie **strony HTML** o nazwie *aplikacji\_offline.htm* (Usuń końcowe "l" w *.html* rozszerzenia, które program Visual Studio tworzy domyślnie).
3. Zastąp kod znaczników szablonu następującym kodem:

    [!code-html[Main](deploying-a-code-update/samples/sample3.html)]
4. Zapisz i zamknij plik.

### <a name="copy-appofflinehtm-to-the-root-folder-of-the-web-site"></a>Kopiowanie aplikacji\_offline.htm do głównego folderu witryny sieci web

Można użyć dowolnego narzędzia FTP do kopiowania plików do witryny sieci web. [FileZilla](http://filezilla-project.org/) to popularne narzędzie FTP i przedstawiono zrzuty ekranu.

Aby użyć narzędzia FTP, potrzebne są trzy rzeczy: adres URL protokołu FTP, nazwę użytkownika i hasło.

Adres URL jest wyświetlany na stronie pulpitu nawigacyjnego witryny sieci web w portalu zarządzania systemu Azure, a nazwę użytkownika i hasło dla połączenia FTP można znaleźć w *.publishsettings* pliku, który został wcześniej pobrany. Poniższe kroki pokazują sposób uzyskiwania tych wartości.

1. W portalu zarządzania Azure kliknij **witryn sieci Web** kartę, a następnie kliknij przycisk tymczasowej witryny sieci web.
2. Na **pulpit nawigacyjny** strony, przewiń w dół do znajdowania nazwy hosta FTP **Quick Glance** sekcji.

    ![Nazwa hosta FTP](deploying-a-code-update/_static/image5.png)
3. Otwórz pomostowego *.publishsettings* pliku w Notatniku lub innym edytorze tekstu.
4. Znajdź `publishProfile` elementu profilu FTP.
5. Kopiuj `userName` i `userPWD` wartości.

    ![Poświadczenia protokołu FTP](deploying-a-code-update/_static/image6.png)
6. Otwórz swoje narzędzia FTP i zaloguj się do adresu URL FTP.
7. Kopiuj *aplikacji\_offline.htm* z folderu rozwiązania, aby */site/wwwroot* folder w witrynie etapowej.

    ![Skopiuj app_offline](deploying-a-code-update/_static/image7.png)
8. Przejdź do adresu URL witryny przemieszczania. Zobaczysz, że *aplikacji\_offline.htm* zamiast strony głównej zostanie teraz wyświetlona strona.

    ![app_offline.htm w oknie przeglądarki](deploying-a-code-update/_static/image8.png)

Teraz można przystąpić do wdrażania przejściowego.

## <a name="deploy-the-code-update-to-staging-and-production"></a>Wdrażanie aktualizacji kodu przejściowym i produkcyjnym

1. W **Web publikacja w pojedynczym kliknięciem** narzędzi, wybierz **przemieszczania** profil publikowania, a następnie kliknij przycisk **publikowanie w sieci Web**.

    Programu Visual Studio wdroży zaktualizowaną aplikację i otworzy w przeglądarce do strony głównej. *Aplikacji\_offline.htm* pliku jest wyświetlany. Zanim można przetestować, aby zweryfikować pomyślne wdrożenie, musisz usunąć *aplikacji\_offline.htm* pliku.
2. Wróć do swojego narzędzia FTP i Usuń **aplikacji\_offline.htm** z tymczasowej witryny.
3. W przeglądarce otwórz stronę Instruktorzy w witrynie etapowej, a następnie wybierz pod kierunkiem instruktora, aby sprawdzić, czy aktualizacja została wdrożona pomyślnie.
4. Postępuj zgodnie z tą samą procedurą w środowisku produkcyjnym, co na karcie tymczasowej.

<a id="specificfiles"></a>

## <a name="reviewing-changes-and-deploying-specific-files"></a>Przeglądanie zmian i wdrażanie określonych plików

Program Visual Studio 2012 zapewnia również możliwość wdrażania poszczególnych plików. Dla wybranego pliku można wyświetlać różnice między lokalną wersją a wersją wdrożone, wdrożyć plik do środowiska docelowego lub skopiuj plik ze środowiska docelowego do lokalnego projektu. W tej części samouczka zobaczysz, jak korzystać z tych funkcji.

### <a name="make-a-change-to-deploy"></a>Wprowadź zmiany do wdrożenia

1. Otwórz *Content/Site.css*i Znajdź blok dla `body` tagu.
2. Zmień wartość `background-color` z `#fff` do `darkblue`.

    [!code-css[Main](deploying-a-code-update/samples/sample4.css?highlight=2)]

### <a name="view-the-change-in-the-publish-preview-window"></a>Wyświetl zmiany w oknie Podgląd publikowania

Kiedy używasz **publikowanie w sieci Web** kreatora, aby opublikować projekt, możesz zobaczyć, jakie zmiany będą publikowane przez dwukrotne kliknięcie pliku w **(wersja zapoznawcza)** okna.

1. Kliknij prawym przyciskiem myszy projekt ContosoUniversity, a następnie kliknij przycisk **Publikuj**.
2. Z **profilu** listy rozwijanej wybierz **testu** profilu publikowania.
3. Kliknij przycisk **Podgląd**, a następnie kliknij przycisk **Uruchom Podgląd**.
4. W **Podgląd** okienku kliknij dwukrotnie **Site.css**.

    ![Kliknij dwukrotnie plik Site.css](deploying-a-code-update/_static/image9.png)

    **Podgląd zmian** okna dialogowego pokazuje jego podgląd zmian, które zostaną wdrożone.

    ![Podgląd zmian w pliku Site.css](deploying-a-code-update/_static/image10.png)

    Po dwukrotnym kliknięciu *Web.config* pliku **podgląd zmian** okna dialogowego pokazuje wpływ kompilacji przekształcenia konfiguracji i przekształcenia profilu publikowania. W tym momencie nie zostało zrobione wszystko, co mogłoby spowodować *Web.config* plików na serwerze, aby się zmieniają, dlatego powinna się pojawić bez zmian. Jednak **podgląd zmian** oknie nieprawidłowo wyświetlane dwie zmiany. Wygląda na to, dwa elementy XML zostaną usunięte. Te elementy są dodawane przez proces publikowania, po wybraniu **wykonaj migracje Code First w aplikacji w menu start** Code First klasy kontekstu. Porównanie jest wykonywane, zanim proces publikowania dodaje te elementy, więc prawdopodobnie są są usuwane, mimo że nie zostaną usunięte. Ten błąd zostanie rozwiązany w przyszłej wersji.
5. Kliknij przycisk **Zamknij**.
6. Kliknij przycisk **publikowania**.
7. Po otwarciu przeglądarki do strony głównej witryny testu, naciśnij klawisze CTRL + F5, aby spowodować twardych odświeżania, aby zobaczyć efekt zmian CSS.

    ![Efekt zmiany CSS](deploying-a-code-update/_static/image11.png)
8. Zamknij przeglądarkę.

### <a name="publish-specific-files-from-solution-explorer"></a>Publikowanie określonych plików w Eksploratorze rozwiązań

Załóżmy, że nie niebieskim tłem, takich jak i chcesz przywrócić oryginalny kolor. W tej sekcji zostaną przywrócone oryginalne ustawienia, publikując określonego pliku bezpośrednio z **Eksploratora rozwiązań**.

1. Otwórz *Content/Site.css* i przywracanie `background-color` ustawienie `#fff`.
2. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *Content/Site.css* pliku.

    Menu kontekstowe pokazuje trzy opcje publikowania.

    ![Publikowanie opcji dostępnych w Eksploratorze rozwiązań](deploying-a-code-update/_static/image12.png)
3. Kliknij przycisk **(wersja zapoznawcza) zmieni się na Site.css**.

    Zostanie otwarte okno, aby wyświetlić różnice między lokalnego pliku i wersja go w środowisku docelowym.

    ![Diff zawartość/Site.css](deploying-a-code-update/_static/image13.png)
4. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **Site.css** ponownie i kliknij przycisk **publikowania pliku Site.css**.

    **Działania publikowania internetowego** okno pokazuje, że plik został opublikowany.

    ![Okno działania publikowania w sieci Web](deploying-a-code-update/_static/image14.png)
5. Otwórz w przeglądarce `http://localhost/contosouniversity` adresu URL i naciśnij klawisze CTRL + F5, aby spowodować, że twardy odświeżąć, aby zobaczyć efekt arkusze CSS, Zmień.

    ![Strona główna z normalnym CSS](deploying-a-code-update/_static/image15.png)
6. Zamknij przeglądarkę.

## <a name="summary"></a>Podsumowanie

Teraz Przedstawiliśmy kilka metod wdrażania aktualizacji aplikacji, które nie obejmują zmianę w bazie danych. Ponadto przedstawiono sposób podgląd zmian, aby sprawdzić, jakie zostaną zaktualizowane jest oczekiwanych. Na stronie Instruktorzy ma teraz **kursy prowadzone** sekcji.

![Strona Instruktorzy z kursom prowadzonym](deploying-a-code-update/_static/image16.png)

Następnym samouczku dowiesz się, jak wdrożyć zmianę w bazie danych: należy dodać pole daty urodzenia do bazy danych i do strony instruktorów.

> [!div class="step-by-step"]
> [Poprzednie](deploying-to-production.md)
> [dalej](deploying-a-database-update.md)
