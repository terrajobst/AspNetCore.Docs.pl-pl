---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
title: "Wdrażanie sieci Web ASP.NET przy użyciu programu Visual Studio: Wdrażanie aktualizacji kod | Dokumentacja firmy Microsoft"
author: tdykstra
description: "Ta seria samouczek pokazuje, jak wdrożyć platformy ASP.NET (publikowanie) aplikacji do aplikacji sieci Web usługi aplikacji Azure lub innego dostawcy hostingu sieci web przez używane..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: c76dbc35-a914-4ee3-919c-4f4d1fa05104
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
msc.type: authoredcontent
ms.openlocfilehash: 10da2b5013ae1348b69ea4f456d81bb4c4b73df6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-code-update"></a>Wdrażanie sieci Web ASP.NET przy użyciu programu Visual Studio: Wdrażanie aktualizacji kodu
====================
przez [Dykstra niestandardowy](https://github.com/tdykstra)

[Pobierz początkowego projektu](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Ta seria samouczek pokazuje, jak wdrożyć platformy ASP.NET (publikowanie) aplikacji do aplikacji sieci Web usługi aplikacji Azure lub innego dostawcy hostingu sieci web za pomocą programu Visual Studio 2012 lub Visual Studio 2010. Aby uzyskać informacje dotyczące serii, zobacz [pierwszy samouczek z tej serii](introduction.md).


## <a name="overview"></a>Omówienie

Po początkowym wdrożeniu kontynuuje pracę utrzymanie i opracowanie witryny sieci web, a niedługo należy wdrożyć aktualizację. Ten samouczek przeprowadza użytkownika przez proces wdrażania aktualizacji w kodzie aplikacji. Aktualizację, która implementuje i wdrożyć w tym samouczku nie wiąże się zmianę w bazie danych; zobaczysz, co różni się o wdrażaniu w następnym samouczku zmianę w bazie danych.

Przypomnienie: Jeśli coś nie działa podczas wykonywania kroków samouczka wyświetlony komunikat o błędzie, należy sprawdzić [Rozwiązywanie problemów z strony](troubleshooting.md).

## <a name="make-a-code-change"></a>Należy zmienić kod

Jako prosty przykład aktualizacji do aplikacji, warto **instruktorów** listę kursów nauczanych przez instruktora wybranej strony.

Po uruchomieniu **instruktorów** strony, można zauważyć, że istnieją **wybierz** łącza w siatce, ale nie zrobią nic innego niż upewnij szary Włącz tła wiersza.

![Strona instruktorów z zaznaczenia](deploying-a-code-update/_static/image1.png)

Teraz dodasz kod wykonywany kiedy **wybierz** łącza zostanie kliknięty i wyświetla listę kursów nauczanych przy wybranym instruktorze.

1. W *Instructors.aspx*, Dodaj następujący kod bezpośrednio po **ErrorMessageLabel** `Label` sterowania:

    [!code-aspx[Main](deploying-a-code-update/samples/sample1.aspx)]
2. Uruchom strony i wybierz instruktora. Zostanie wyświetlona lista kursów nauczanych przy tym instruktora.

    ![Strona instruktorów z kursów nauczanych](deploying-a-code-update/_static/image2.png)
3. Zamknij przeglądarkę.

## <a name="deploy-the-code-update-to-the-test-environment"></a>Wdrażanie aktualizacji kod do środowiska testowego

Zanim użyjesz do profilu publikowania do wdrożenia na test, przemieszczania i produkcji, należy zmienić opcje publikowania bazy danych. Nie trzeba uruchamiać skrypty wdrażania danych i grant dla bazy danych członkostwa.

1. Otwórz **publikowanie w sieci Web** Kreator prawym przyciskiem myszy projekt ContosoUniversity, a następnie klikając polecenie **publikowania**.
2. Kliknij przycisk **testu** profilu w **profilu** listy rozwijanej.
3. Kliknij przycisk **ustawienia** kartę.
4. W obszarze **połączenia DefaultConnection** w **baz danych** sekcji, wyczyść **aktualizacji bazy danych** pole wyboru.
5. Kliknij przycisk **profilu** , a następnie kliknij pozycję **przemieszczania** profilu w **profilu** listy rozwijanej.
6. Gdy zapyta, czy chcesz zapisać zmiany wprowadzone w **testu** profilu, kliknij przycisk **tak**.
7. Wprowadzanie tych samych zmian w profilu tymczasowego.
8. Powtórz ten proces, aby udostępnić te same zmiany w profilu produkcji.
9. Zamknij **publikowanie w sieci Web** kreatora.

Wdrażanie w środowisku testowym jest teraz polegać na jednym kliknięciem uruchomiona ponownie opublikować. Aby ułatwić ten proces szybsze, można użyć **jeden kliknij publikowania w sieci Web** paska narzędzi.

1. W **widoku** menu, wybierz **paski narzędzi** , a następnie wybierz **jeden kliknij publikowania w sieci Web**.

    ![Selecting_One_Click_Publish_toolbar](deploying-a-code-update/_static/image3.png)
2. W **Eksploratora rozwiązań**, wybierz projekt ContosoUniversity.
3. **jeden kliknij publikowania w sieci Web** narzędzi wybierz **testu** profil publikowania, a następnie kliknij przycisk **publikowanie w sieci Web** (ikona z strzałki w lewo i w prawo).

    ![Web_One_Click_Publish_toolbar](deploying-a-code-update/_static/image4.png)
4. Program Visual Studio wdroży zaktualizowaną aplikację i przeglądarka automatycznie otwiera do strony głównej.
5. Uruchom instruktorów strony i wybierz instruktora, aby zweryfikować, że aktualizacja została pomyślnie wdrożona.

Normalny również wykonać testów regresyjnych (to znaczy test reszty lokacji, aby upewnić się, że nowe zmiany nie Przerwij wszystkie istniejące funkcje). Jednak w tym samouczku będzie pominąć ten krok i przejdź do wdrożenia aktualizacji tymczasowych i produkcyjnych.

Podczas ponownego wdrażania, narzędzie Web Deploy automatycznie określa, które pliki zostały zmienione i kopie tylko zmienione pliki na serwerze. Domyślnie narzędzie Web Deploy korzysta z ostatniej zmiany daty na plikach do określenia, które zostały zmienione. Niektórych systemów kontroli źródła zmiany pliku dat, nawet jeśli nie zmienisz zawartość pliku. W takim przypadku można skonfigurować narzędzie Web Deploy używać sum kontrolnych plików w celu określenia, które pliki zostały zmienione. Aby uzyskać więcej informacji, zobacz [Dlaczego wszystkie pliki uzyskać wdrożone mimo że nie je zmienić?](https://msdn.microsoft.com/en-us/library/ee942158.aspx#use_checksum) w często zadawane pytania dotyczące wdrożenia programu ASP.NET.

## <a name="take-the-application-offline-during-deployment"></a>Przejdź do trybu offline podczas wdrażania

Zmiana, którą jest wdrażany jest teraz prosta zmiana na jednej stronie. Czasami wdrażanie większych zmian lub wdrażania zmian w kodzie i bazy danych — a lokacji może działać niepoprawnie Jeśli użytkownik zażąda strony przed zakończeniem wdrażania. Aby uniemożliwić użytkownikom dostęp do witryny, w trakcie wdrażania, można użyć *aplikacji\_offline.htm* pliku. Jeśli możesz umieścić plik o nazwie *aplikacji\_offline.htm* w folderze głównym aplikacji, usługi IIS automatycznie wyświetla ten plik zamiast uruchamiania aplikacji. Tak, aby uniemożliwić dostęp podczas wdrażania, możesz zaznaczyć *aplikacji\_offline.htm* w folderze głównym Uruchom proces wdrażania, a następnie usuń *aplikacji\_offline.htm* po pomyślnym wdrożenia.

Można skonfigurować narzędzie Web Deploy, aby automatycznie wprowadzić domyślny *aplikacji\_offline.htm* plików na serwerze po rozpoczęciu, wdrażanie i usuń go po jej zakończeniu. Zrobić, że wszystkie musisz wykonać jest dodać następujących elementów XML do publikowania pliku profilu (.pubxml):

[!code-xml[Main](deploying-a-code-update/samples/sample2.xml)]

W tym samouczku zobaczysz sposobu tworzenia i używania niestandardowego *aplikacji\_offline.htm* pliku.

Przy użyciu *aplikacji\_offline.htm* w tymczasowej lokacji nie jest wymagane, ponieważ nie masz dostępu do lokalizacji tymczasowej. Ale dobrym rozwiązaniem na potrzeby przemieszczania przetestuj wszystkie elementy w taki sposób, który planujesz wdrożyć w środowisku produkcyjnym.

### <a name="create-appofflinehtm"></a>Tworzenie aplikacji\_offline.htm

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy rozwiązanie i kliknij przycisk **Dodaj**, a następnie kliknij przycisk **nowy element**.
2. Utwórz **strony HTML** o nazwie *aplikacji\_offline.htm* (Usuń final "l" w *.html* rozszerzenia, które domyślnie tworzy Visual Studio).
3. Zastąp następujący kod znaczników szablonu:

    [!code-html[Main](deploying-a-code-update/samples/sample3.html)]
4. Zapisz i zamknij plik.

### <a name="copy-appofflinehtm-to-the-root-folder-of-the-web-site"></a>Kopiowanie aplikacji\_offline.htm na folder główny witryny sieci web

Można użyć dowolnego narzędzia FTP do kopiowania plików do witryny sieci web. [FileZilla](http://filezilla-project.org/) jest narzędziem popularnych FTP i jest wyświetlany w zrzutów ekranu.

Aby użyć narzędzia FTP, niezbędne są trzy elementy: adres URL FTP, nazwę użytkownika i hasło.

Adres URL jest wyświetlany na stronie pulpitu nawigacyjnego witryny sieci web w portalu zarządzania Azure i nazwę użytkownika i hasło dla FTP można znaleźć w *.publishsettings* wcześniej pobranego pliku. Poniższe kroki pokazują, jak uzyskać te wartości.

1. W portalu zarządzania Azure, kliknij przycisk **witryn sieci Web** a następnie kliknij pozycję przemieszczania witryny sieci web.
2. Na **pulpitu nawigacyjnego** strony, przewiń w dół do znajdowania nazwy hosta FTP **szybki przegląd** sekcji.

    ![Nazwa hosta FTP](deploying-a-code-update/_static/image5.png)
3. Otwórz przejściowym *.publishsettings* pliku w Notatniku lub w innym edytorze tekstu.
4. Znajdź `publishProfile` elementu profilu FTP.
5. Kopiuj `userName` i `userPWD` wartości.

    ![Poświadczenia FTP](deploying-a-code-update/_static/image6.png)
6. Otworzyć narzędzia FTP i zaloguj się do adresu URL usługi FTP.
7. Kopiuj *aplikacji\_offline.htm* z folderu rozwiązania */lokacji/wwwroot* folder w witrynie tymczasowej.

    ![Skopiuj app_offline](deploying-a-code-update/_static/image7.png)
8. Przejdź do adresu URL witryny tymczasowej. Zostanie wyświetlony *aplikacji\_offline.htm* teraz zostanie wyświetlona strona zamiast strony głównej.

    ![app_offline.htm w oknie przeglądarki](deploying-a-code-update/_static/image8.png)

Teraz można przystąpić do wdrażania na przemieszczania.

## <a name="deploy-the-code-update-to-staging-and-production"></a>Wdrożenia aktualizacji kodu tymczasowych i produkcyjnych

1. W **jeden kliknij publikowania w sieci Web** narzędzi wybierz **przemieszczania** profil publikowania, a następnie kliknij przycisk **publikowanie w sieci Web**.

    Visual Studio wdraża zaktualizowaną aplikację i otwiera przeglądarkę do strony głównej. *Aplikacji\_offline.htm* plik jest wyświetlany. Aby przeprowadzić test, aby zweryfikować pomyślne wdrożenie, należy usunąć *aplikacji\_offline.htm* pliku.
2. Powróć do własnych narzędzi FTP i Usuń **aplikacji\_offline.htm** z tymczasowej lokacji.
3. W przeglądarce otwórz stronę instruktorów przemieszczania witryny i wybierz instruktora, aby zweryfikować, że aktualizacja została pomyślnie wdrożona.
4. Wykonaj tę samą procedurę w środowisku produkcyjnym, tak samo jak przemieszczania.

<a id="specificfiles"></a>

## <a name="reviewing-changes-and-deploying-specific-files"></a>Przeglądanie zmian i wdrażanie określonych plików

Program Visual Studio 2012 zapewnia również możliwość wdrażania poszczególnych plików. Dla wybranego pliku można wyświetlać różnice między lokalną wersją i wdrożoną wersję, Wdróż plik za pomocą środowiska docelowego lub skopiuj plik ze środowiska docelowego do lokalnego projektu. W tej części samouczka widać, jak używać tych funkcji.

### <a name="make-a-change-to-deploy"></a>Zmiany do wdrożenia

1. Otwórz *Content/Site.css*i Znajdź blok dla `body` tagu.
2. Zmień wartość atrybutu `background-color` z `#fff` do `darkblue`.

    [!code-css[Main](deploying-a-code-update/samples/sample4.css?highlight=2)]

### <a name="view-the-change-in-the-publish-preview-window"></a>Przeglądanie zmian w oknie Podgląd publikowania

Jeśli używasz **publikowanie w sieci Web** kreatora, aby opublikować projekt, widać zmiany będą publikowane przez dwukrotne kliknięcie pliku w **Podgląd** okna.

1. Kliknij prawym przyciskiem myszy projekt ContosoUniversity, a następnie kliknij przycisk **publikowania**.
2. Z **profilu** listy rozwijanej wybierz **testu** profilu publikowania.
3. Kliknij przycisk **Podgląd**, a następnie kliknij przycisk **Uruchom Podgląd**.
4. W **Podgląd** okienku kliknij dwukrotnie **Site.css**.

    ![Kliknij dwukrotnie Site.css](deploying-a-code-update/_static/image9.png)

    **Podgląd zmian** okna dialogowego Podgląd zmian, które zostaną wdrożone.

    ![Podgląd zmian do Site.css](deploying-a-code-update/_static/image10.png)

    Po dwukrotnym kliknięciu *Web.config* pliku **podgląd zmian** okno dialogowe przedstawia wynik kompilacji przekształcenia konfiguracji i przekształcenia profilu publikowania. W tym momencie nie wykonano żadnych czynności, które mogłyby spowodować *Web.config* pliku na serwerze, aby zmienić, więc powinny być widoczne żadne zmiany. Jednak **podgląd zmian** okno niepoprawnie zawiera dwie zmiany. Wygląda na to, dwa elementy XML zostaną usunięte. Te elementy są dodawane przez proces publikowania, po wybraniu **wykonaj migracje Code First na uruchamianie aplikacji** Code First klasy kontekstu. Porównanie odbywa się przed proces publikowania dodaje tych elementów, prawdopodobnie są one usuwane mimo że nie zostaną usunięte. Ten błąd zostanie rozwiązany w przyszłej wersji.
5. Kliknij przycisk **Zamknij**.
6. Kliknij przycisk **publikowania**.
7. Po otwarciu przeglądarki do strony głównej witryny testu, naciśnij kombinację klawiszy CTRL + F5, aby spowodować twardych odświeżania, aby zobaczyć efekt zmian CSS.

    ![Wpływ zmian CSS](deploying-a-code-update/_static/image11.png)
8. Zamknij przeglądarkę.

### <a name="publish-specific-files-from-solution-explorer"></a>Publikowanie określonych plików w Eksploratorze rozwiązań

Załóżmy, że nie podoba niebieskie tło i chcesz przywrócić oryginalny kolor. W tej sekcji będzie przywrócić pierwotne ustawienia publikując określonego pliku bezpośrednio z **Eksploratora rozwiązań**.

1. Otwórz *Content/Site.css* i przywrócić `background-color` ustawienie `#fff`.
2. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *Content/Site.css* pliku.

    Menu kontekstowe pokazuje trzy opcje publikowania.

    ![Opcje w Eksploratorze rozwiązań publikowania](deploying-a-code-update/_static/image12.png)
3. Kliknij przycisk **podgląd zmian Site.css**.

    Zostanie otwarte okno, aby przedstawiał różnice między lokalnego pliku i wersja go w środowisku docelowym.

    ![Diff-Content/Site.css](deploying-a-code-update/_static/image13.png)
4. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **Site.css** ponownie i kliknij przycisk **publikowania Site.css**.

    **Aktywności publikowania w sieci Web** okna pokazuje, że plik został opublikowany.

    ![Okno działania publikowania w sieci Web](deploying-a-code-update/_static/image14.png)
5. Otwórz w przeglądarce `http://localhost/contosouniversity` adresu URL i naciśnij klawisze CTRL + F5, aby spowodować twardy Odśwież, aby zobaczyć efekt CSS zmienić.

    ![Strona główna z normalnym CSS](deploying-a-code-update/_static/image15.png)
6. Zamknij przeglądarkę.

## <a name="summary"></a>Podsumowanie

Teraz przedstawiono kilka sposobów wdrażania aktualizacji aplikacji, która nie obejmuje zmianę w bazie danych, oraz przedstawiono sposób podgląd zmian, aby sprawdzić, jakie zostaną zaktualizowane jest oczekiwań. Na stronie instruktorów ma teraz **nauczanych kursów** sekcji.

![Strona instruktorów z kursów nauczanych](deploying-a-code-update/_static/image16.png)

Następny samouczek przedstawia sposób wdrażania zmianę w bazie danych: pole Data urodzenia zostanie dodana do bazy danych i do strony instruktorów.

>[!div class="step-by-step"]
[Poprzednie](deploying-to-production.md)
[dalej](deploying-a-database-update.md)
