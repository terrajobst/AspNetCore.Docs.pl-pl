---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-production
title: "Wdrażanie sieci Web ASP.NET przy użyciu programu Visual Studio: Wdrażanie do środowiska produkcyjnego | Dokumentacja firmy Microsoft"
author: tdykstra
description: "Ta seria samouczek pokazuje, jak wdrożyć platformy ASP.NET (publikowanie) aplikacji do aplikacji sieci Web usługi aplikacji Azure lub innego dostawcy hostingu sieci web przez używane..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 416438a1-3b2f-4d27-bf53-6b76223c33bf
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-production
msc.type: authoredcontent
ms.openlocfilehash: 2c49e7f6925b1ca172642747c5052ba97d70d036
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/11/2018
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-to-production"></a>Wdrażanie sieci Web ASP.NET przy użyciu programu Visual Studio: Wdrażanie w środowisku produkcyjnym
====================
przez [Dykstra niestandardowy](https://github.com/tdykstra)

[Pobierz początkowego projektu](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Ta seria samouczek pokazuje, jak wdrożyć platformy ASP.NET (publikowanie) aplikacji do aplikacji sieci Web usługi aplikacji Azure lub innego dostawcy hostingu sieci web za pomocą programu Visual Studio 2012 lub Visual Studio 2010. Aby uzyskać informacje dotyczące serii, zobacz [pierwszy samouczek z tej serii](introduction.md).


## <a name="overview"></a>Omówienie

W tym samouczku skonfigurować konto Microsoft Azure, tworzenie środowisk przemieszczania i produkcji i wdrażanie aplikacji sieci web ASP.NET do przemieszczania i środowisk produkcyjnych przy użyciu programu Visual Studio jednym kliknięciem publikowanie funkcji.

Jeśli wolisz, można wdrożyć u dostawcy hostingu innych firm. Większość procedur opisanych w tym samouczku są takie same dla dostawcy hostingu lub platformy Azure, z tą różnicą, że każdego z dostawców ma własny interfejs użytkownika zarządzania konta i witryny sieci web. Można znaleźć dostawcę hostingu w [galerii dostawców](https://www.microsoft.com/web/hosting) w witrynie Microsoft.com.

Przypomnienie: Jeśli coś nie działa podczas wykonywania kroków samouczka wyświetlony komunikat o błędzie, należy sprawdzić stronę rozwiązywania problemów w tym samouczku.

## <a name="get-a-microsoft-azure-account"></a>Załóż konto Microsoft Azure

Jeśli nie masz jeszcze konta platformy Azure, możesz utworzyć bezpłatne konto próbne w zaledwie kilka minut. Aby uzyskać więcej informacji, zobacz [bezpłatnej wersji próbnej Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

## <a name="create-a-staging-environment"></a>Tworzenie środowiska przemieszczania

> [!NOTE]
> Ponieważ w tym samouczku zostały zapisane, usługi Azure App Service dodać nową funkcję można zautomatyzować wiele procesów tworzenia środowisk przemieszczania i produkcji. Zobacz [Konfigurowanie środowiska dla aplikacji sieci web w usłudze Azure App Service przejściowe](https://azure.microsoft.com/documentation/articles/web-sites-staged-publishing/).


Zgodnie z objaśnieniem w [Wdróż do samouczka środowiska testu](deploying-to-iis.md), najbardziej środowiska testowego niezawodnej jest witryną sieci web u dostawcy hostingu, który ma podobnie jak witryny sieci web w środowisku produkcyjnym. Wielu dostawców hostingu należy porównać zalety względem dodatkowych kosztów, ale na platformie Azure można utworzyć aplikacji sieci web na wolne dodatkowych jako tymczasowej aplikacji. Należy również bazy danych, a dodatkowe wydatków dla tej za pośrednictwem wydatków produkcyjnej bazy danych będzie mieć wartość Brak lub minimalnej. Na platformie Azure płacisz ilości magazynu bazy danych, którego używasz, a nie dla każdej bazy danych, a ilość dodatkowego magazynu, które będą używane w przemieszczania jest minimalny.

Zgodnie z objaśnieniem w [Wdróż do samouczka środowiska testowego](deploying-to-iis.md), w tymczasowych i produkcyjnych, które zamierzasz wdrożyć dwóch baz danych do jednej bazy danych. Jeśli chcesz zachować oddzielne proces będzie taki sam z tą różnicą, że należy utworzyć dodatkowej bazy danych dla każdego środowiska i ciąg poprawny docelowy dla każdej bazy danych należy wybrać podczas tworzenia profilu publikowania.

W tej części samouczka utworzysz aplikację sieci web i bazy danych do użycia w środowisku przemieszczania i należy wdrożyć przemieszczania i testowania przed utworzeniem i wdrożeniem w środowisku produkcyjnym.

> [!NOTE]
> Poniższe kroki przedstawiają sposób utworzenia aplikacji sieci web w usłudze Azure App Service za pomocą portalu zarządzania Azure. W najnowszej wersji zestawu SDK platformy Azure można również zrobienie tego bez opuszczania programu Visual Studio, za pomocą Eksploratora serwera. W programie Visual Studio 2013 można również utworzyć aplikacji sieci web bezpośrednio w oknie dialogowym publikowania. Aby uzyskać więcej informacji, zobacz [tworzenie aplikacji sieci web platformy ASP.NET w usłudze Azure App Service.](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet)


1. W [portalu zarządzania Azure](https://manage.windowsazure.com/), kliknij przycisk **witryn sieci Web**, a następnie kliknij przycisk **nowy**.
2. Kliknij przycisk **witryny sieci Web**, a następnie kliknij przycisk **Utwórz niestandardowy**.

    **Nowej witryny sieci Web — Utwórz niestandardowy** zostanie otwarty Kreator. **Utwórz niestandardowy** Kreator umożliwia tworzenie witryn sieci web i bazy danych w tym samym czasie.
3. W **tworzenie witryny sieci Web** kroku kreatora, wprowadź ciąg w **adres URL** pole ma być używana jako unikatowy adres URL aplikacji do przemieszczania środowiska. Na przykład wprowadź ContosoUniversity-staging123 (w tym liczb losowych na końcu w celu zapewnienia unikatowości w przypadku, gdy wykonywana jest ContosoUniversity przemieszczania).

    Pełny adres URL będzie składać się z tutaj wprowadź oraz sufiks, który pojawi się obok pola tekstowego.
4. W **Region** listy rozwijanej wybierz region najbliższy do Ciebie.

    To ustawienie określa centrum danych, które Twoja aplikacja sieci web jest uruchamiana w.
5. W **bazy danych** listy rozwijanej wybierz pozycję **utworzyć nową bazę danych SQL**.
6. W **Nazwa ciągu połączenia bazy danych** pozostaw wartość domyślną, *połączenia DefaultConnection*.
7. Kliknij przycisk Strzałka w prawo w dolnej części pola.

    Na poniższej ilustracji pokazano **tworzenie witryny sieci Web** okno dialogowe z wartości przykładowych w nim. Adres URL i Region, który został wprowadzony może się różnić.

    ![Utwórz krok witryny sieci Web](deploying-to-production/_static/image1.png)

    Kreator przechodzi do **Określ ustawienia bazy danych** kroku.
8. W **nazwa** wprowadź *ContosoUniversity* oraz liczbę losową w celu zapewnienia unikatowości, na przykład *ContosoUniversity123*.
9. W **serwera** wybierz opcję **nowy serwer bazy danych SQL**.
10. Wprowadź nazwę i hasło administratora.

    Nie wprowadzasz istniejącej nazwy i hasła w tym miejscu. Podajesz nową nazwę i hasło, które definiujesz teraz do użycia w przyszłości podczas dostępu do bazy danych.
11. W **Region** wybierz tym samym regionie, który został wybrany dla aplikacji sieci web.

    Zachowanie serwera sieci web i serwera bazy danych w tym samym regionie zapewnia najlepszą wydajność i zmniejsza koszty.
12. Kliknij znacznik wyboru w dolnej części pola, aby wskazać, że wszystko jest gotowe.

    Na poniższej ilustracji pokazano **Określ ustawienia bazy danych** okno dialogowe z wartości przykładowych w nim. Wprowadzone wartości mogą się różnić.

    ![Krok ustawienia bazy danych nowej witryny sieci Web — tworzenie przy użyciu Kreatora bazy danych](deploying-to-production/_static/image2.png)

    Portal zarządzania zwróci do strony witryny sieci Web i **stan** kolumnie jest wyświetlana jest tworzona aplikacja sieci web. Po chwili (zazwyczaj mniej niż minutę) **stan** kolumnie jest wyświetlana aplikacja sieci web została pomyślnie utworzona. Na pasku nawigacyjnym po lewej stronie liczba aplikacji sieci web, które masz na koncie jest wyświetlany obok pozycji **witryn sieci Web** ikonę i baz danych jest wyświetlany obok pozycji **baz danych SQL** ikony.

    ![Strona witryn sieci Web w portalu zarządzania, witryna sieci web utworzona](deploying-to-production/_static/image3.png)

    Nazwa aplikacji sieci web może się różnić od przykładową aplikację na ilustracji.

## <a name="deploy-the-application-to-staging"></a>Wdrażanie aplikacji do etapu przemieszczania

Teraz, po utworzeniu aplikacji sieci web i bazy danych w środowisku przemieszczania, można wdrożyć projektu do niego.

> [!NOTE]
> Te instrukcje pokazują, jak utworzyć profil publikowania, pobierając *.publishsettings* pliku, który dotyczy nie tylko Azure ale także dla dostawców hostingu innych firm. Najnowszej zestawu SDK usługi Azure umożliwia również nawiązać bezpośrednie połączenie Azure w programie Visual Studio i wybierz z listy aplikacji sieci web, które masz na koncie Azure. W programie Visual Studio 2013, aby móc zalogować w Azure z **publikowania w sieci Web** okna dialogowego lub **Eksploratora serwera** okna. Aby uzyskać więcej informacji, zobacz [tworzenie aplikacji sieci web platformy ASP.NET w usłudze Azure App Service](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).


### <a name="download-the-publishsettings-file"></a>Pobierz plik .publishsettings

1. Kliknij nazwę aplikacji sieci web, który został właśnie utworzony.

    ![Kliknij witrynę, aby przejść do pulpitu nawigacyjnego](deploying-to-production/_static/image4.png)
2. W obszarze **szybkiego dostępu** w **pulpitu nawigacyjnego** , kliknij pozycję **pobieranie profilu publikowania**.

    ![Pobierz profil publikowania łącza](deploying-to-production/_static/image5.png)

    Ten krok pobiera plik, który zawiera wszystkie ustawienia, które są potrzebne do wdrożenia aplikacji na aplikację sieci web. Będzie zaimportować ten plik do programu Visual Studio, dzięki czemu nie trzeba ręcznie wprowadzić informacje o tym.
3. Zapisz *.publishsettings* plik w folderze, którego można korzystać z programu Visual Studio.

    ![Zapisywanie pliku .publishsettings](deploying-to-production/_static/image6.png)

    > [!WARNING]
    > Zabezpieczenia — *.publishsettings* plik zawiera swoje poświadczenia (Niezakodowane), które są używane do administrowania subskrypcji platformy Azure i usługi. Najlepszym rozwiązaniem bezpieczeństwa dla tego pliku jest tymczasowo przechowywane poza katalogów źródła (na przykład w folderze Libraries\Documents), a następnie usuń ją po zakończeniu importowania. Złośliwy użytkownik, który uzyskuje dostęp do *.publishsettings* plik można edytować, tworzenie i usuwanie usługami Azure.

### <a name="create-a-publish-profile"></a>Tworzenie profilu publikowania

1. W programie Visual Studio, kliknij prawym przyciskiem myszy projekt ContosoUniversity w **Eksploratora rozwiązań** i wybierz **publikowania** z menu kontekstowego.

    **Publikowanie w sieci Web** zostanie otwarty Kreator.
2. Kliknij przycisk **profilu** kartę.
3. Kliknij przycisk **importu**.
4. Przejdź do *.publishsettings* wcześniej pobrano pliku, a następnie kliknij przycisk **Otwórz**.

    ![Okno dialogowe Importowanie ustawień publikowania](deploying-to-production/_static/image7.png)
5. W **połączenia** , kliknij pozycję **sprawdzania poprawności połączenia** aby upewnić się, że ustawienia są poprawne.

    Po sprawdzeniu poprawności połączenia zielony znacznik wyboru jest wyświetlany obok pozycji **sprawdzania poprawności połączenia** przycisku.

    Dla niektórych dostawców hostingu, po kliknięciu **sprawdzania poprawności połączenia**, można napotkać **błąd certyfikatu** okno dialogowe. Jeśli to zrobisz, sprawdź, czy nazwa serwera jest oczekiwań. Jeśli nazwa serwera jest poprawna, wybierz **Zapisz ten certyfikat na potrzeby przyszłych sesji programu Visual Studio** i kliknij przycisk **Akceptuj**. (Ten błąd oznacza, że uniknąć konieczności zakup certyfikatu SSL dla adresu URL, który jest wdrażany z wybrał dostawcy hostingu. Jeśli wolisz ustanowienia bezpiecznego połączenia przy użyciu prawidłowego certyfikatu, skontaktuj się z dostawcą hostingu.)
6. Kliknij przycisk **Dalej**.

    ![Ikona pomyślnego połączenia i przycisk Dalej na karcie Połączenie Kreatora](deploying-to-production/_static/image8.png)
7. W **ustawienia** karcie, rozwiń **opcji publikowania pliku**, a następnie wybierz **Wyklucz pliki z aplikacji\_folderu danych**.

    Aby uzyskać informacje na temat opcji w obszarze **opcji publikowania pliku**, zobacz [wdrażania usług IIS](deploying-to-iis.md) samouczka. Zrzut ekranu pokazujący, w wyniku tego kroku i następujących kroków konfiguracji bazy danych jest na końcu kroki konfiguracji bazy danych.
8. W obszarze **połączenia DefaultConnection** w **baz danych** skonfiguruj wdrażania bazy danych dla bazy danych członkostwa.
9. 1. Wybierz **aktualizacji bazy danych**.

        **Ciąg połączenia zdalnego** bezpośrednio poniżej pola **połączenia DefaultConnection** jest wypełniane parametry połączenia z pliku .publishsettings. Parametry połączenia zawierają poświadczenia serwera SQL Server, które są przechowywane w postaci zwykłego tekstu w *.pubxml* pliku. Jeśli nie chcesz przechowywać je trwale istnieje, można je usunąć z profilu publikowania, po wdrożeniu bazy danych i przechowywać je na platformie Azure. Aby uzyskać więcej informacji, zobacz [jak chronić bazę danych programu ASP.NET parametry połączenia w przypadku wdrażania Azure ze źródła](http://www.hanselman.com/blog/HowToKeepYourASPNETDatabaseConnectionStringsSecureWhenDeployingToAzureFromSource.aspx) na blogu Scott Hanselman.
    2. Kliknij przycisk **skonfigurować aktualizacje bazy danych**.
    3. W **Konfigurowanie aktualizacji bazy danych** okno dialogowe, kliknij przycisk **Dodaj skrypt SQL**.
    4. W **Dodaj skrypt SQL** wybierz opcję *aspnet-data-prod.sql* skrypt, który wcześniej zapisany w folderze rozwiązania, a następnie kliknij przycisk **Otwórz**.
    5. Zamknij **Konfigurowanie aktualizacji bazy danych** okno dialogowe.
10. W obszarze **SchoolContext** w **baz danych** zaznacz **wykonaj migracje Code First (wywoływane po uruchomieniu aplikacji)**.

    Wyświetla programu Visual Studio **wykonaj migracje Code First** zamiast **aktualizacji bazy danych** dla `DbContext` klasy. Jeśli chcesz użyć dostawcy dbDacFx zamiast migracje do bazy danych, do którego dostęp przy użyciu wdrożenia `DbContext` , zobacz [sposób wdrażania Code First bazy danych bez migracje?](https://msdn.microsoft.com/en-us/library/ee942158.aspx#deploy_code_first_without_migrations) w sieci Web wdrożenia często zadawane pytania dla programu Visual Studio i platformy ASP.NET w witrynie MSDN.

    **Ustawienia** kartę teraz wygląda następująco:

    ![Karta Ustawienia dla przemieszczania](deploying-to-production/_static/image9.png)
11. Wykonaj poniższe kroki, aby zapisać profil i zmienić jego nazwę na *przemieszczania*:

    1. Kliknij przycisk **profilu** , a następnie kliknij pozycję **zarządzania profilami**.
    2. Importowanie utworzyć dwóch nowych profilów, co w przypadku usługi FTP i jeden dla narzędzia Web Deploy. Skonfigurowany profil narzędzia Web Deploy: Zmień nazwę tego profilu możesz *przemieszczania*.

        ![Zmień nazwę profilu na przemieszczania](deploying-to-production/_static/image10.png)
    3. Zamknij **edytować profile publikowania w sieci Web** okno dialogowe.
    4. Zamknij **publikowanie w sieci Web** kreatora.

### <a name="configure-a-publish-profile-transform-for-the-environment-indicator"></a>Skonfiguruj transformacji profil publikowania dla wskaźnika środowiska

> [!NOTE]
> W tej sekcji przedstawiono sposób konfigurowania transformacji pliku Web.config dla wskaźnika środowiska. Ponieważ wskaźnik jest w `<appSettings>` elementu, masz alternatywą dla określania transformacja podczas wdrażania usługi Azure App Service. Aby uzyskać więcej informacji, zobacz [określenie ustawienia pliku Web.config na platformie Azure](web-config-transformations.md#watransforms).


1. W **Eksploratora rozwiązań**, rozwiń węzeł **właściwości**, a następnie rozwiń węzeł **PublishProfiles**.
2. Kliknij prawym przyciskiem myszy *Staging.pubxml*, a następnie kliknij przycisk **dodać przekształcenie konfiguracji**.

    ![Dodawanie przekształcenie konfiguracji dla przemieszczania](deploying-to-production/_static/image11.png)

    Program Visual Studio tworzy *Web.Staging.config* pliku transformacji i otwarcie go.
3. W *Web.Staging.config* pliku przekształcenia, wstaw poniższy kod bezpośrednio po otwarciu `configuration` tagu.

    [!code-xml[Main](deploying-to-production/samples/sample1.xml)]

    Gdy używasz profilu publikowania przemieszczania tej transformacji ustawia wskaźnik środowiska, aby "Produkcyjną". W wdrożonej aplikacji sieci web nie będą widzieć dowolny sufiks, takie jak "(deweloperów)" lub "(Test)", po nagłówek H1 "Contoso University".
4. Kliknij prawym przyciskiem myszy *Web.Staging.config* plik i kliknij przycisk **przekształcenie Podgląd** aby upewnić się, że transformacji zostanie na stałe tworzy oczekiwanych zmian.

    **Podgląd pliku Web.config** okna przedstawia wynik zastosowania zarówno *Web.Release.config* przekształca i *Web.Staging.config* przekształca.

### <a name="prevent-public-use-of-the-test-app"></a>Zapobiegaj publicznego korzystanie z aplikacji testu

Ważną kwestią przemieszczania aplikacji jest będzie na żywo w Internecie, że nie ma publicznego z niego korzystać. Aby zminimalizować prawdopodobieństwo, że osoby znajduje i używa go, można użyć co najmniej jeden z następujących metod:

- Ustaw reguły zapory, które zezwalają na dostęp do tymczasowej aplikacjami tylko z adresów IP, których używasz do testowania przemieszczania.
- Użyj zaciemnionego adresu URL, który będzie niemożliwe do odgadnięcia.
- Utwórz *robots.txt* pliku, aby upewnić się, że aparatów wyszukiwania będzie Przeszukuj łącza aplikacji i raportu testu do niego w wynikach wyszukiwania.

Pierwsza z tych metod jest najbardziej efektywne, ale nie jest uwzględnione w tym samouczku, ponieważ wymagałyby wdrażania do usługi w chmurze platformy Azure zamiast usługi Azure App Service. Więcej informacji o usługach w chmurze oraz ograniczenia adresów IP na platformie Azure, zobacz [obliczeniowe Hosting opcje dostarczany przez platformę Azure](https://docs.microsoft.com/azure/cloud-services/cloud-services-choose-me) i [bloku określonych adresów IP, dostęp do roli sieci Web](https://msdn.microsoft.com/en-us/library/windowsazure/jj154098.aspx). Jeśli wdrażasz do innego dostawcy hostingu, skontaktuj się z dostawcą, aby dowiedzieć się, jak zaimplementować ograniczenia adresów IP.

W tym samouczku utworzysz *robots.txt* pliku.

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt ContosoUniversity i kliknij przycisk **Dodaj nowy element**.
2. Utwórz nową **pliku tekstowego** o nazwie *robots.txt*i umieszcza w nim następujący tekst:

    [!code-console[Main](deploying-to-production/samples/sample2.cmd)]

    `User-agent` Wiersza poleca aparatów wyszukiwania, które zasady w pliku dotyczą wszystkich aparat web przeszukiwarek (robotów), i `Disallow` wiersz określa, że można przeszukać stron w witrynie.

    Czy chcesz aparaty wyszukiwania do katalogu aplikacji produkcyjnych, więc należy wykluczyć ten plik z wdrożenia produkcyjnego. Aby zrobić, należy skonfigurować profil publikowania ustawienie w środowisku produkcyjnym, podczas jego tworzenia.

### <a name="deploy-to-staging"></a>Wdrażanie na przemieszczania

1. Otwórz **publikowanie w sieci Web** Kreator prawym przyciskiem myszy projekt Contoso University i klikając przycisk **publikowania**.
2. Upewnij się, że **przemieszczania** profilu jest zaznaczona.
3. Kliknij przycisk **publikowania**.

    **Dane wyjściowe** okna pokazuje, jakie akcje wdrażania zostały pobrane i raporty pomyślnego wdrożenia. Przeglądarka domyślna automatycznie otwiera adres URL wdrożonej aplikacji sieci web.

## <a name="test-in-the-staging-environment"></a>Testowanie w środowisku przemieszczania

Zauważ, że wskaźnik środowiska jest nieobecny (Brak "(testu)" lub "(deweloperów)" po nagłówku H1, która wskazuje, że *Web.config* przekształcania wskaźnika środowisko zakończyło się pomyślnie.

![Strona główna przemieszczania](deploying-to-production/_static/image12.png)

Uruchom **studentów** stronę, aby sprawdzić, czy nie studentów wdrożonej bazy danych.

Uruchom **instruktorów** stronę, aby sprawdzić Code First rozpoczęta w bazie danych instruktora:

Wybierz **dodać studentów** z **studentów** menu Dodaj Studenta, a następnie wyświetlić nowy uczniów w **studentów** stronę, aby sprawdzić, czy możesz pomyślnie zapisywać do bazy danych .

Z **kursów** kliknij przycisk **środków aktualizacji**. **Środków aktualizacji** strona musi mieć uprawnienia administratora, więc **dziennika w** zostanie wyświetlona strona. Wprowadź poświadczenia konta administratora, które zostało utworzone wcześniej ("Administrator" i "prodpwd"). **Środków aktualizacji** zostanie wyświetlona strona, która sprawdza, czy konto administratora, utworzony w poprzednim samouczek poprawnie została wdrożona do środowiska testowego.

Żądanie nieprawidłowego adresu URL spowodować błąd czy ELMAH będzie śledzić, a następnie żądają ELMAH raport o błędzie. Jeśli wdrażasz do innego dostawcy hostingu, prawdopodobnie znajdziesz raport jest pusty z tej samej przyczyny, który był pusty w poprzednim samouczka. Należy skonfigurować uprawnienia folderu umożliwiające ELMAH do zapisu w folderze dziennika przy użyciu narzędzia do zarządzania kontem dostawcy hostingu.

Utworzona aplikacja jest teraz uruchomiona w chmurze w aplikacji sieci web, który działa tak jak użyjesz do produkcji. Ponieważ wszystko działa prawidłowo, następnym krokiem jest do wdrożenia w środowisku produkcyjnym.

## <a name="deploy-to-production"></a>Wdrażanie w środowisku produkcyjnym

Proces tworzenia aplikacji sieci web w środowisku produkcyjnym i wdrażania w środowisku produkcyjnym są takie same jak przemieszczania, z wyjątkiem tego, czy należy wyłączyć *robots.txt* z wdrożenia. W tym celu zostanie zmodyfikowany plik profilu publikowania.

### <a name="create-the-production-environment-and-the-production-publish-profile"></a>Tworzenie środowiska produkcyjnego i produkcji profilu publikowania

1. Tworzenie aplikacji sieci web w środowisku produkcyjnym i bazy danych na platformie Azure, po samej procedury, która została użyta w przypadku tymczasowego.

    Podczas tworzenia bazy danych, wybierz go umieścić na tym samym serwerze, który został utworzony wcześniej, lub utworzyć nowy serwer.
2. Pobierz *.publishsettings* pliku.
3. Utwórz profil publikowania, importując produkcji *.publishsettings* plik używany do tymczasowego procedury.

    Nie zapomnij Konfigurowanie danych skrypt wdrożenia w obszarze **połączenia DefaultConnection** w **baz danych** sekcji **ustawienia** kartę.
4. Zmień nazwę profilu publikowania do *produkcji*.
5. Skonfiguruj transformacji profil publikowania dla wskaźnika środowisku i procedury używana na potrzeby przemieszczania...

### <a name="edit-the-pubxml-file-to-exclude-robotstxt"></a>Edytuj plik .pubxml, aby wykluczyć robots.txt

Pliki są nazywane profilu publikowania &lt;profilename&gt;*.pubxml* i znajdują się w *PublishProfiles* folderu. *PublishProfiles* znajduje się w folderze *właściwości* projektu folderu w języku C# aplikacji sieci web, w obszarze *mój projekt* folderu projektu aplikacji sieci web VB lub w obszarze *Aplikacji\_danych* folderu projektu aplikacji sieci web. Każdy *.pubxml* plik zawiera ustawienia, które są stosowane do jednego profilu publikowania. Wartości, które należy wprowadzić w Kreatorze publikowanie w sieci Web są przechowywane w tych plikach, i można edytować je, aby utworzyć lub zmienić ustawienia, które nie są dostępne w interfejsie użytkownika programu Visual Studio.

Domyślnie *.pubxml* pliki znajdują się w projekcie, podczas tworzenia profilu publikowania, ale można wykluczyć je z projektu programu Visual Studio będą nadal z nich korzystać. Program Visual Studio jest wyszukiwany w *PublishProfiles* folder *.pubxml* plików, niezależnie od tego, czy są one uwzględnione w projekcie.

Dla każdego *.pubxml* pliku jest *. pubxml.user* pliku. *. Pubxml.user* plik zawiera zaszyfrowane hasło, jeśli wybrano **Zapisz hasło** opcję i domyślnie jest on wykluczony z projektu.

A *.pubxml* plik zawiera ustawienia, które odnoszą się do profilu publikowania określonych. Jeśli chcesz skonfigurować ustawienia stosowane do wszystkich profilów, możesz utworzyć *. wpp.targets* pliku. Proces kompilacji importuje tych plików do *.csproj* lub *vbproj* pliku projektu, dlatego w tych plikach można skonfigurować większość ustawień, które można skonfigurować w pliku projektu. Aby uzyskać więcej informacji na temat *.pubxml* plików i *. wpp.targets* plików, zobacz [porady: edytowanie ustawień wdrażania w plikach profilu publikacji (.pubxml) i. wpp.targets pliku w programie Visual Studio Projekty w sieci Web](https://msdn.microsoft.com/en-us/library/ff398069.aspx).

1. W **Eksploratora rozwiązań**, rozwiń węzeł **właściwości** i rozwiń **PublishProfiles**.
2. Kliknij prawym przyciskiem myszy *Production.pubxml* i kliknij przycisk **Otwórz**.

    ![Otwórz plik .pubxml](deploying-to-production/_static/image13.png)
3. Kliknij prawym przyciskiem myszy *Production.pubxml* i kliknij przycisk **Otwórz**.
4. Dodaj następujące wiersze bezpośrednio przed tagiem zamykającym `PropertyGroup` elementu:

    [!code-xml[Main](deploying-to-production/samples/sample3.xml)]

    Plik .pubxml teraz wygląda następująco:

    [!code-xml[Main](deploying-to-production/samples/sample4.xml?highlight=18-20)]

    Aby uzyskać więcej informacji o sposobie wykluczyć pliki i foldery, zobacz [można wykluczyć określone pliki lub foldery z wdrożenia?](https://msdn.microsoft.com/en-us/library/ee942158.aspx#can_i_exclude_specific_files_or_folders_from_deployment) w **często zadawane pytania wdrażania w sieci Web dla platformy ASP.NET i Visual Studio** w witrynie MSDN.

### <a name="deploy-to-production"></a>Wdrażanie w środowisku produkcyjnym

1. Otwórz **publikowanie w sieci Web** kreatora upewnij się, że **produkcji** profilu publikowania jest zaznaczone, a następnie kliknij przycisk **Uruchom Podgląd** na **Podgląd**kartę, aby sprawdzić, czy *robots.txt* pliku nie zostaną skopiowane do aplikacji produkcyjnej.

    ![Podgląd plików do opublikowania w środowisku produkcyjnym](deploying-to-production/_static/image14.png)

    Zapoznaj się z listą plików, które zostaną skopiowane. Zobaczysz, że wszystkie *.cs* pliki, w tym *. aspx.cs*, *. aspx.designer.cs*, *Master.cs*, i  *Master.Designer.cs* pliki są pomijane. Wszystkie ten kod został skompilowany do *ContosoUniversity.dll* i *ContosUniversity.pdb* pliki, które znajdują się w *bin* folderu. Ponieważ tylko *.dll* jest niezbędne do uruchomienia aplikacji, a określony wcześniej powinny być wdrażane tylko pliki potrzebne do uruchomienia aplikacji, nie *.cs* pliki zostały skopiowane do lokalizacji docelowej środowisko. *Obj* folderu i *ContosoUniversity.csproj* i *. csproj.user* pliki zostały pominięte w tej samej przyczyny.

    Kliknij przycisk **publikowania** do wdrożenia do środowiska produkcyjnego.
2. Testowanie w środowisku produkcyjnym, użyta do tymczasowego procedury.

    Wszystko jest taki sam jak przemieszczania, z wyjątkiem adres URL i braku *robots.txt* pliku.

## <a name="summary"></a>Podsumowanie

Teraz pomyślnie wdrożyć i przetestować aplikację sieci web i jest dostępny publicznie w Internecie.

![Strona główna produkcji](deploying-to-production/_static/image15.png)

W następnym samouczku możesz zaktualizować kodu aplikacji i wdrożenia zmiany do środowiska testowego, tymczasową i produkcyjną.

> [!NOTE]
> Gdy aplikacja jest używana w środowisku produkcyjnym powinien być wykonawczych planu odzyskiwania. To, że użytkownik należy okresowo kopii zapasowych baz danych z aplikacji produkcyjnej do lokalizacji bezpiecznego magazynu i należy zachować kilka generacji tych kopii zapasowych. Podczas aktualizowania bazy danych należy kopii zapasowej z bezpośrednio przed zmianą. Następnie jeśli popełnienia błędu, a nie odnalezienia dopiero po wdrożeniu w środowisku produkcyjnym, nadal będzie można odzyskać bazy danych do stanu sprzed przed jego uległa uszkodzeniu. Aby uzyskać więcej informacji, zobacz [i przywracania kopii zapasowej bazy danych SQL Azure](https://msdn.microsoft.com/en-us/library/windowsazure/jj650016.aspx).


> [!NOTE]
> W tym samouczku SQL Server edition, które wdrażasz jest baza danych SQL Azure. Podczas procesu wdrażania jest podobny do innych wersji programu SQL Server, aplikacji rzeczywistej produkcji mogą wymagać specjalnego kodu bazy danych SQL Azure w niektórych scenariuszach. Aby uzyskać więcej informacji, zobacz [baza danych SQL Azure](../../../../whitepapers/aspnet-data-access-content-map.md#ssdb) i [wybór między programu SQL Server i bazy danych SQL Azure](../../../../whitepapers/aspnet-data-access-content-map.md#ssdbchoosing).

>[!div class="step-by-step"]
[Poprzednie](setting-folder-permissions.md)
[dalej](deploying-a-code-update.md)
