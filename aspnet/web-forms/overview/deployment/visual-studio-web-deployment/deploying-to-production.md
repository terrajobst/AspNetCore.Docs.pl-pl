---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-production
title: 'Wdrażanie aplikacji internetowych ASP.NET przy użyciu programu Visual Studio: do wdrożenia produkcyjnego | Dokumentacja firmy Microsoft'
author: tdykstra
description: W tej serii samouczków dowiesz się, jak wdrożyć (opublikować) platformy ASP.NET sieci web aplikacji do usługi Azure App Service Web Apps lub dostawcy hostingu w innych firm, używane...
ms.author: aspnetcontent
ms.date: 02/15/2013
ms.assetid: 416438a1-3b2f-4d27-bf53-6b76223c33bf
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-production
msc.type: authoredcontent
ms.openlocfilehash: a91feedb9ac09b2a70ca256c72d312a1e78ecbc8
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37827880"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-to-production"></a>Wdrażanie aplikacji internetowych ASP.NET przy użyciu programu Visual Studio: do wdrożenia produkcyjnego
====================
przez [Tom Dykstra](https://github.com/tdykstra)

[Pobieranie projektu startowego](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> W tej serii samouczków dowiesz się, jak wdrożyć (opublikować) platformy ASP.NET sieci web aplikacji do usługi Azure App Service Web Apps lub innych firm dostawcy hostingu za pomocą programu Visual Studio 2012 lub Visual Studio 2010. Aby uzyskać informacje na temat serii, zobacz [pierwszym samouczku tej serii](introduction.md).


## <a name="overview"></a>Omówienie

W tym samouczku Ustaw konto Microsoft Azure, Utwórz środowiskach przejściowych i produkcyjnych i wdróż aplikację sieci web platformy ASP.NET do wdrażania przejściowego i środowisk produkcyjnych przy użyciu programu Visual Studio jednym kliknięciem publikować funkcji.

Jeśli wolisz, możesz wdrożyć u dostawcy hostingu innych firm. Większość procedur opisanych w tym samouczku, są takie same dla dostawcy hostingu lub na platformie Azure, z tą różnicą, że każdy dostawca ma własny interfejs użytkownika do zarządzania kontem i witryny sieci web. Można znaleźć dostawcę hostingu w [galerii dostawcy](https://www.microsoft.com/web/hosting) w witrynie Microsoft.com.

Przypomnienie: Jeśli otrzymujesz komunikat o błędzie lub coś nie działa podczas wykonywania kroków samouczka, koniecznie sprawdź stronę rozwiązywania problemów w tej serii samouczków.

## <a name="get-a-microsoft-azure-account"></a>Załóż konto Microsoft Azure

Jeśli nie masz jeszcze konta platformy Azure, możesz utworzyć bezpłatne konto próbne w zaledwie kilka minut. Aby uzyskać więcej informacji, zobacz [bezpłatnej wersji próbnej Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

## <a name="create-a-staging-environment"></a>Tworzenie środowiska przemieszczania

> [!NOTE]
> Ponieważ w tym samouczku został napisany, usłudze Azure App Service dodano nową funkcję umożliwiającą zautomatyzowanie wielu procesów tworzenia środowiskach przejściowych i produkcyjnych. Zobacz [Konfigurowanie środowiska tymczasowego dla aplikacji sieci web w usłudze Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-staged-publishing/).


Jak wyjaśniono w [Wdróż do samouczka środowisko testowe](deploying-to-iis.md), najbardziej środowiska testowego niezawodne to witryna sieci web u dostawcy hostingu, który ma podobnie jak w przypadku witryny sieci web w środowisku produkcyjnym. U wielu dostawców hostingu należy porównać zalety to względem znaczące dodatkowych kosztów, ale na platformie Azure możesz utworzyć dodatkowe bezpłatna aplikacja internetowa jako tymczasowe aplikacji. Należy również bazę danych, a dodatkowe koszty, w tym nad pieniędzy produkcyjnej bazy danych będzie mieć wartość Brak najwyżej minimalnych. Na platformie Azure płacisz ilości magazynu bazy danych, którego używasz, a nie dla każdej bazy danych, a ilość dodatkowego magazynu, które będą używane w środowisku tymczasowym jest minimalny.

Jak wyjaśniono w [Wdróż do samouczka dotyczącego środowiska testowego](deploying-to-iis.md)wśród środowisk przejściowych i produkcyjnych, które zamierzasz wdrożyć dwóch baz danych w jednej bazie danych. Jeśli chcesz zachować oddzielne, proces będzie taki sam poza tym, że należy utworzyć dodatkowej bazy danych dla każdego środowiska i ciąg odpowiedniego miejsca docelowego dla każdej bazy danych należy wybrać podczas tworzenia profilu publikowania.

W tej części samouczka utworzysz aplikację sieci web i bazy danych na potrzeby środowisko przejściowe i będzie wdrażanie w środowisku przejściowym i testowania istnieje, przed utworzeniem i wdrażanie w środowisku produkcyjnym.

> [!NOTE]
> Poniższe kroki pokazują jak utworzyć aplikację sieci web w usłudze Azure App Service przy użyciu portalu zarządzania systemu Azure. W najnowszej wersji zestawu Azure SDK można również w tym bez opuszczania programu Visual Studio za pomocą Eksploratora serwera. W programie Visual Studio 2013 można również utworzyć aplikację sieci web bezpośrednio z poziomu okna dialogowego publikowania. Aby uzyskać więcej informacji, zobacz [tworzenie aplikacji sieci web platformy ASP.NET w usłudze Azure App Service.](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet)


1. W [portalu zarządzania systemu Azure](https://manage.windowsazure.com/), kliknij przycisk **witryn sieci Web**, a następnie kliknij przycisk **New**.
2. Kliknij przycisk **witryny sieci Web**, a następnie kliknij przycisk **tworzenie niestandardowe**.

    **Nowa witryna — tworzenie niestandardowe** zostanie otwarty Kreator. **Tworzenie niestandardowe** Kreator pozwala na tworzenie witryny sieci web i bazy danych w tym samym czasie.
3. W **Utwórz witrynę sieci Web** kroku kreatora wprowadź ciąg w **adresu URL** pole, aby użyć jako unikatowy adres URL dla aplikacji użytkownika środowiska testowego. Na przykład wprowadź ContosoUniversity-staging123 (w tym liczb losowych na końcu, aby była unikatowa w przypadku, gdy ContosoUniversity przemieszczania jest zajęta).

    Pełny adres URL będzie składać się w tym miejscu wprowadź oraz sufiks, który zostanie wyświetlony obok pola tekstowego.
4. W **Region** listę rozwijaną, wybierz region, który jest najbliżej Ciebie.

    To ustawienie określa, które centrum danych, aplikacji sieci web jest uruchamiana w.
5. W **bazy danych** listy rozwijanej wybierz **Tworzenie nowej bazy danych SQL**.
6. W **nazwa parametrów połączenia bazy danych** pozostaw wartość domyślną *DefaultConnection*.
7. Kliknij strzałkę po prawej stronie u dołu okna.

    Poniższa ilustracja przedstawia **Utwórz witrynę sieci Web** okna dialogowego wypełniane przykładowymi wartościami w nim. Adres URL i Region, w którym zostały wprowadzone, może się różnić.

    ![Tworzenie witryny sieci Web krok](deploying-to-production/_static/image1.png)

    Kreator prowadzi do **Określ ustawienia bazy danych** kroku.
8. W **nazwa** wprowadź *ContosoUniversity* oraz liczbę losową, aby była unikatowa, na przykład *ContosoUniversity123*.
9. W **serwera** wybierz opcję **nowy serwer bazy danych SQL**.
10. Wprowadź nazwę i hasło administratora.

    Nie wprowadzasz istniejącej nazwy i hasła w tym miejscu. Podajesz nową nazwę i hasło, które definiujesz teraz do użycia w przyszłości, gdy uzyskujesz dostęp do bazy danych.
11. W **Region** wybierz w tym samym regionie, który został wybrany dla aplikacji sieci web.

    Przechowywanie serwer sieci web a serwerem bazy danych w tym samym regionie zapewnia najlepszą wydajność i minimalizuje koszty.
12. Kliknij znacznik wyboru w dolnej części pola, aby wskazać, że jesteś gotowy.

    Poniższa ilustracja przedstawia **Określ ustawienia bazy danych** okna dialogowego wypełniane przykładowymi wartościami w nim. Wartości, które zostały wprowadzone, mogą być inne.

    ![Krok ustawienia bazy danych witryny sieci Web, New - tworzenie za pomocą Kreatora bazy danych](deploying-to-production/_static/image2.png)

    Portal zarządzania zwróci do strony witryny sieci Web i **stan** kolumna pokazuje, że trwa tworzenie aplikacji sieci web. Po chwili (zazwyczaj mniej niż minutę) **stan** kolumnie jest wyświetlana w aplikacji sieci web została pomyślnie utworzona. Na pasku nawigacyjnym po lewej stronie, liczba aplikacji sieci web na swoim koncie możesz mieć pojawia się obok **witryn sieci Web** ikonę i liczby baz danych pojawia się obok **baz danych SQL** ikony.

    ![Strona witryny sieci Web portalu zarządzania, witryna sieci web utworzona](deploying-to-production/_static/image3.png)

    Nazwa aplikacji internetowej będą różnić się od Przykładowa aplikacja na ilustracji.

## <a name="deploy-the-application-to-staging"></a>Wdrażanie aplikacji w środowisku przejściowym

Teraz, gdy utworzono aplikację sieci web i bazy danych w środowisku przejściowym, można wdrożyć projektu do niego.

> [!NOTE]
> Te instrukcje przedstawiają sposób tworzenia profilu publikowania, pobierając *.publishsettings* pliku, który działa nie tylko na platformie Azure ale także w przypadku innych dostawców hostingu. Najnowszy zestaw Azure SDK umożliwia także łączenia bezpośrednio na platformie Azure w programie Visual Studio, a następnie wybierz z listy aplikacji internetowych, które mają na koncie platformy Azure. W programie Visual Studio 2013, możesz zalogować się do platformy Azure — Konferencja **Web Publish** okna dialogowego lub **Eksploratora serwera** okna. Aby uzyskać więcej informacji, zobacz [tworzenie aplikacji sieci web platformy ASP.NET w usłudze Azure App Service](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).


### <a name="download-the-publishsettings-file"></a>Pobierz plik .publishsettings

1. Kliknij nazwę aplikacji sieci web, który został utworzony.

    ![Kliknij witrynę, aby przejść do pulpitu nawigacyjnego](deploying-to-production/_static/image4.png)
2. W obszarze **Przegląd** w **pulpit nawigacyjny** kliknij pozycję **Pobierz profil publikowania**.

    ![Pobierz profil publikowania łącza](deploying-to-production/_static/image5.png)

    Ten krok spowoduje pobranie pliku, który zawiera wszystkie ustawienia które są potrzebne do wdrażania aplikacji do aplikacji sieci web. Ten plik zostanie zaimportowany do programu Visual Studio, aby nie musieli ręcznie wprowadzić te informacje.
3. Zapisz *.publishsettings* plik w folderze, w której będziesz mieć dostęp z poziomu programu Visual Studio.

    ![Zapisywanie pliku .publishsettings](deploying-to-production/_static/image6.png)

    > [!WARNING]
    > Zabezpieczenia — *.publishsettings* plik zawiera swoje poświadczenia (Niezakodowane:), które są używane do administrowania subskrypcjami platformy Azure i usług. Najlepszym rozwiązaniem bezpieczeństwa dla tego pliku jest tymczasowo przechowywane poza katalogów źródła (na przykład w folderze Libraries\Documents) i usunąć go po zakończeniu importowania. Złośliwy użytkownik, który uzyskuje dostęp do *.publishsettings* plik można edytować, tworzenia i usuwania usług platformy Azure.

### <a name="create-a-publish-profile"></a>Tworzenie profilu publikowania

1. W programie Visual Studio, kliknij prawym przyciskiem myszy projekt ContosoUniversity w **Eksploratora rozwiązań** i wybierz **Publikuj** z menu kontekstowego.

    **Publikowanie w sieci Web** zostanie otwarty Kreator.
2. Kliknij przycisk **profilu** kartę.
3. Kliknij przycisk **importu**.
4. Przejdź do *.publishsettings* pliku, dla którego wcześniej pobrany, a następnie kliknij przycisk **Otwórz**.

    ![Okno dialogowe Importuj ustawienia publikowania](deploying-to-production/_static/image7.png)
5. W **połączenia** kliknij pozycję **Waliduj połączenie** aby upewnić się, że ustawienia są poprawne.

    Po zweryfikowaniu połączenia, zielony znacznik wyboru jest wyświetlany obok pozycji **Waliduj połączenie** przycisku.

    Dla niektórych dostawców hostingu, po kliknięciu **Waliduj połączenie**, może zostać wyświetlony **błąd certyfikatu** okno dialogowe. Jeśli to zrobisz, sprawdź, czy nazwa serwera to, czego oczekiwać. Jeśli nazwa serwera jest prawidłowa, wybierz opcję **Zapisz ten certyfikat na potrzeby przyszłych sesji programu Visual Studio** i kliknij przycisk **Akceptuj**. (Ten błąd oznacza, że uniknąć konieczności zakup certyfikatu protokołu SSL dla adresu URL, który jest wdrażany z wybrał dostawcy hostingu. Jeśli chcesz nawiązać bezpiecznego połączenia przy użyciu prawidłowego certyfikatu, skontaktuj się z dostawcą hostingu.)
6. Kliknij przycisk **Dalej**.

    ![Ikona pomyślnego połączenia i przycisk Dalej na karcie Połączenie Kreatora](deploying-to-production/_static/image8.png)
7. W **ustawienia** kartę, a następnie rozwiń **opcji publikowania pliku**, a następnie wybierz pozycję **wykluczanie plików z aplikacji\_folderu danych**.

    Aby uzyskać informacje na temat innych opcji w obszarze **opcji publikowania pliku**, zobacz [wdrażanie w usługach IIS](deploying-to-iis.md) samouczka. Ten pokazuje wynik tego kroku i następujących kroków konfiguracji bazy danych znajduje się na końcu kroków konfiguracji bazy danych zrzut ekranu.
8. W obszarze **DefaultConnection** w **baz danych** sekcji, skonfiguruj wdrożenie bazy danych dla bazy danych członkostwa.
9. 1. Wybierz **Aktualizuj bazę danych**.

        **Parametry połączenia zdalnego** polu bezpośrednio pod wprowadzonymi **DefaultConnection** jest wypełniony przy użyciu parametrów połączenia z pliku .publishsettings. Parametry połączenia zawierają poświadczenia serwera SQL Server, które są przechowywane w postaci zwykłego tekstu w *.pubxml* pliku. Jeśli nie chcesz przechowywać je trwale istnieje, możesz je usunąć z profilu publikowania, po wdrożeniu bazy danych i przechowywać je na platformie Azure. Aby uzyskać więcej informacji, zobacz [jak zabezpieczyć bazę danych programu ASP.NET parametry połączenia w przypadku wdrażania na platformie Azure ze źródła](http://www.hanselman.com/blog/HowToKeepYourASPNETDatabaseConnectionStringsSecureWhenDeployingToAzureFromSource.aspx) w blogu Scotta Hanselmana.
      2. Kliknij przycisk **Konfiguruj aktualizacje bazy danych**.
      3. W **Konfiguruj aktualizacje bazy danych** okno dialogowe, kliknij przycisk **Dodaj skrypt SQL**.
      4. W **Dodaj skrypt SQL** , przejdź do *aspnet-data-prod.sql* skrypt, który wcześniej zapisany w folderze rozwiązania, a następnie kliknij przycisk **Otwórz**.
      5. Zamknij **Konfiguruj aktualizacje bazy danych** okno dialogowe.
10. W obszarze **SchoolContext** w **baz danych** zaznacz **wykonaj migracje Code First (uruchomieniu aplikacji)**.

    Visual Studio Wyświetla **wykonaj migracje Code First** zamiast **Aktualizuj bazę danych** dla `DbContext` klasy. Jeśli chcesz używać dostawcy dbDacFx zamiast migracje wdrożyć bazę danych, które są dostępne za pomocą `DbContext` klasy, zobacz [jak wdrożyć Code First bazy danych bez migracje?](https://msdn.microsoft.com/library/ee942158.aspx#deploy_code_first_without_migrations) w sieci Web wdrożenia — często zadawane pytania dla programu Visual Studio i platformy ASP.NET w witrynie MSDN.

    **Ustawienia** kartę wygląda teraz następująco:

    ![Karta Ustawienia dla przemieszczania](deploying-to-production/_static/image9.png)
11. Wykonaj poniższe kroki, aby zapisać profil i zmień jej nazwę na *przemieszczania*:

    1. Kliknij przycisk **profilu** kartę, a następnie kliknij przycisk **spravovat profily**.
    2. Importowanie tworzone są dwa profile nowy, jeden dla protokołu FTP i jeden dla narzędzia Web Deploy. Skonfigurowany profil narzędzia Web Deploy: Zmień nazwę tego profilu do *przemieszczania*.

        ![Zmienianie nazwy profilu w środowisku przejściowym](deploying-to-production/_static/image10.png)
    3. Zamknij **Edytuj profile publikowania w sieci Web** okno dialogowe.
    4. Zamknij **publikowanie w sieci Web** kreatora.

### <a name="configure-a-publish-profile-transform-for-the-environment-indicator"></a>Skonfigurować przekształcenia profil publikowania dla wskaźnika środowiska

> [!NOTE]
> W tej sekcji pokazano, jak skonfigurować przekształcenia pliku Web.config dla wskaźnika środowiska. Ponieważ wskaźnik jest w `<appSettings>` elementu, masz inną alternatywą do określania przekształcenie, podczas wdrażania w usłudze Azure App Service. Aby uzyskać więcej informacji, zobacz [określenie ustawienia pliku Web.config na platformie Azure](web-config-transformations.md#watransforms).


1. W **Eksploratora rozwiązań**, rozwiń węzeł **właściwości**, a następnie rozwiń węzeł **PublishProfiles**.
2. Kliknij prawym przyciskiem myszy *Staging.pubxml*, a następnie kliknij przycisk **Dodaj przekształcenia konfiguracji**.

    ![Dodaj przekształcenie konfiguracji na potrzeby trybu przejściowego](deploying-to-production/_static/image11.png)

    Program Visual Studio tworzy *Web.Staging.config* pliku transformacji i otwiera go.
3. W *Web.Staging.config* pliku przekształcenia, Wstaw następujący kod bezpośrednio po otwarciu `configuration` tagu.

    [!code-xml[Main](deploying-to-production/samples/sample1.xml)]

    Korzystając z przemieszczania profil publikowania, przekształcenia ustawia wskaźnik środowiska, aby "Prod". W aplikacji wdrożonej w sieci web nie będą widzieć dowolny sufiks, takie jak "(Dev)" lub "(Test)" po nagłówku H1 "Uniwersytet firmy Contoso".
4. Kliknij prawym przyciskiem myszy *Web.Staging.config* plik i kliknij przycisk **Przekształcanie wersji zapoznawczej** aby upewnić się, że przekształcenie zakodowane produkuje oczekiwane zmiany.

    **(Wersja zapoznawcza) w pliku Web.config** okno pokazuje wynik zastosowania zarówno *Web.Release.config* przekształca i *Web.Staging.config* przekształcenia.

### <a name="prevent-public-use-of-the-test-app"></a>Zapobiegaj publicznych korzystanie z aplikacji test

Ważną kwestią przemieszczania aplikacji jest będą na żywo w Internecie, że nie ma publicznego z niego korzystać. Aby zminimalizować prawdopodobieństwo, że osoby znajdzie i używać go, można użyć co najmniej jeden z następujących metod:

- Ustawianie reguł zapory, zezwalających na dostęp do aplikacji przemieszczania tylko z adresów IP, które można przetestować przemieszczania.
- Użyj zaciemnionego adresu URL, które byłyby niemożliwe do odgadnięcia.
- Tworzenie *robots.txt* pliku, aby zapewnić, że aparatów wyszukiwania będzie Przeszukuj łączy aplikację i raportu testu do niego w wynikach wyszukiwania.

Pierwsza z tych metod jest najbardziej efektywne, ale nie jest omówione w tym samouczku, ponieważ wymagałoby wdrożeniu do usługi Azure Cloud Service, a nie usługi Azure App Service. Aby uzyskać więcej informacji o usługach Cloud Services i ograniczenia adresów IP na platformie Azure, zobacz [Compute Hosting opcji udostępnianych przez platformę Azure](https://docs.microsoft.com/azure/cloud-services/cloud-services-choose-me) i [bloku konkretnych adresów IP, dostęp do roli sieci Web](https://msdn.microsoft.com/library/windowsazure/jj154098.aspx). Jeśli wdrażasz do dostawcy hostingu innych firm, skontaktuj się z dostawcą, aby dowiedzieć się, jak zaimplementować ograniczenia adresów IP.

W tym samouczku utworzysz *robots.txt* pliku.

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt ContosoUniversity i kliknij przycisk **Dodaj nowy element**.
2. Utwórz nową **plik tekstowy** o nazwie *robots.txt*i umieść następujący tekst w nim:

    [!code-console[Main](deploying-to-production/samples/sample2.cmd)]

    `User-agent` Wiersz zawiera informacje dla aparatów wyszukiwania, które reguły w pliku mają zastosowanie do wszystkich aparatu web przeszukiwarki (robotom) i `Disallow` wiersz określa, że można przeszukać stron w witrynie.

    Użytkownik chce aparaty wyszukiwania do wykazu aplikacji produkcyjnych, więc należy wykluczyć ten plik z wdrożenia produkcyjnego. Aby zrobić, należy skonfigurować profil publikowania ustawienie w środowisku produkcyjnym, podczas jego tworzenia.

### <a name="deploy-to-staging"></a>Wdrażanie w środowisku przejściowym

1. Otwórz **publikowanie w sieci Web** kreatora, kliknij prawym przyciskiem myszy projekt Contoso University i klikając **Publikuj**.
2. Upewnij się, że **przemieszczania** profilu jest zaznaczona.
3. Kliknij przycisk **publikowania**.

    **Dane wyjściowe** okno pokazuje, jakie akcje wdrażania zostały wykonane i raporty pomyślne ukończenie wdrożenia. Przeglądarka domyślna automatycznie otwiera adres URL wdrożonej aplikacji internetowej.

## <a name="test-in-the-staging-environment"></a>Testowanie w środowisku przejściowym

Zauważ, że wskaźnik środowiska nieobecne (Brak "(testu)" lub "(Dev)" po nagłówku H1, który pokazuje, że *Web.config* transformacji dla wskaźnika środowisko zakończyło się pomyślnie.

![Strona główna przemieszczania](deploying-to-production/_static/image12.png)

Uruchom **studentów** strony, aby sprawdzić, czy wdrożonej bazy danych jest nie studentów.

Uruchom **Instruktorzy** stronę, aby sprawdzić, czy Code First zasilany w bazie danych przez instruktorów:

Wybierz **dodać uczniów** z **studentów** menu Dodaj uczniem, a następnie wyświetlić nowego studenta w **studentów** stronę, aby sprawdzić, czy można pomyślnie zapisać do bazy danych .

Z **kursów** kliknij **aktualizacji środki na korzystanie z**. **Aktualizacji środki na korzystanie z** strony musi mieć uprawnienia administratora, więc **logowanie** zostanie wyświetlona strona. Wprowadź poświadczenia konta administratora, utworzony wcześniej ("admin" i "prodpwd"). **Aktualizacji środki na korzystanie z** zostanie wyświetlona strona, która sprawdza, czy konto administratora, który został utworzony w poprzednim samouczku poprawnie została wdrożona do środowiska testowego.

Nieprawidłowy adres URL do powodują wystąpienie błędu, że ELMAH będzie śledzić i następnie zażądać raportu o błędach ELMAH żądania. Jeśli wdrażasz do dostawcy hostingu innych firm, prawdopodobnie znajdziesz raport jest pusty dla tego samego powodu, który był pusty w poprzednim samouczku. Należy skonfigurować uprawnienia folderu, aby włączyć ELMAH do zapisu w folderze dziennika przy użyciu narzędzia do zarządzania konta dostawcy hostingu.

Utworzoną aplikację jest teraz uruchomiona w chmurze w aplikacji sieci web, która działa tak jak będzie on używany w środowisku produkcyjnym. Ponieważ wszystko działa poprawnie, następnym krokiem jest wdrażania w środowisku produkcyjnym.

## <a name="deploy-to-production"></a>Wdrażanie w środowisku produkcyjnym

Proces tworzenia aplikacji sieci web w środowisku produkcyjnym i wdrażania do środowiska produkcyjnego jest tak samo przemieszczania, z tą różnicą, że należy wykluczyć *robots.txt* z wdrożenia. Aby to zrobić będzie edytować plik profilu publikowania.

### <a name="create-the-production-environment-and-the-production-publish-profile"></a>Tworzenie w środowisku produkcyjnym i produkcji profil publikowania

1. Tworzenie aplikacji sieci web w środowisku produkcyjnym i bazy danych na platformie Azure, zgodnie z tej samej procedury, które jest używane jako przejściowy.

    Podczas tworzenia bazy danych, możesz wybrać go umieścić na tym samym serwerze, która została utworzona wcześniej lub utworzyć nowy serwer.
2. Pobierz *.publishsettings* pliku.
3. Utwórz profil publikowania, importując produkcji *.publishsettings* pliku, zgodnie z tej samej procedury, które jest używane jako przejściowy.

    Nie należy zapominać skonfigurować skrypt wdrażania przy użyciu danych **DefaultConnection** w **baz danych** części **ustawienia** kartę.
4. Zmień nazwę profilu publikowania do *produkcji*.
5. Konfigurowanie środowiska wskaźnika, zgodnie z tej samej procedury, które jest używane jako przejściowy przekształcenia profilu publikowania...

### <a name="edit-the-pubxml-file-to-exclude-robotstxt"></a>Edytuj plik .pubxml, aby wykluczyć robots.txt

Pliki są nazywane profil publikowania &lt;profilename&gt;*.pubxml* i znajdują się w *PublishProfiles* folderu. *PublishProfiles* znajduje się w folderze *właściwości* folderu w języku C# aplikacji sieci web projektu w obszarze *mój projekt* folderu w projekcie aplikacji sieci web VB lub równy podanemu *Aplikacji\_danych* folderu w projekcie aplikacji sieci web. Każdy *.pubxml* plik zawiera ustawienia, które są stosowane do jednego profilu publikowania. Wartości, które zostały wprowadzone w Kreatorze publikowanie w sieci Web są przechowywane w tych plikach, i można edytować ich do tworzenia lub zmiany ustawień, które nie są dostępne w interfejsie użytkownika programu Visual Studio.

Domyślnie *.pubxml* pliki znajdują się w projekcie, podczas tworzenia profilu publikowania, ale możesz je wykluczyć z projektu programu Visual Studio będą nadal z nich korzystać. Program Visual Studio szuka w *PublishProfiles* folder *.pubxml* plików, niezależnie od tego, czy są one uwzględnione w projekcie.

Dla każdego *.pubxml* pliku jest *. pubxml.user* pliku. *. Pubxml.user* plik zawiera zaszyfrowane hasło, jeśli została wybrana **Zapisz hasło** opcję i domyślnie jest ona wykluczana z projektu.

A *.pubxml* plik zawiera ustawienia, które odnoszą się do profilu publikowania określone. Jeśli chcesz skonfigurować ustawienia, które są stosowane do wszystkich profilów, możesz utworzyć *. wpp.targets* pliku. Proces kompilacji importuje te pliki do *.csproj* lub *.vbproj* pliku projektu, dzięki czemu można skonfigurować większość ustawień, które można skonfigurować w pliku projektu w tych plikach. Aby uzyskać więcej informacji na temat *.pubxml* plików i *. wpp.targets* plików, zobacz [porady: edytowanie ustawień wdrażania w plikach profilu publikacji (.pubxml) i. wpp.targets pliku w programie Visual Studio Projektów sieci Web](https://msdn.microsoft.com/library/ff398069.aspx).

1. W **Eksploratora rozwiązań**, rozwiń węzeł **właściwości** i rozwiń **PublishProfiles**.
2. Kliknij prawym przyciskiem myszy *Production.pubxml* i kliknij przycisk **Otwórz**.

    ![Otwórz plik .pubxml](deploying-to-production/_static/image13.png)
3. Kliknij prawym przyciskiem myszy *Production.pubxml* i kliknij przycisk **Otwórz**.
4. Dodaj następujące wiersze bezpośrednio przed tagiem zamykającym `PropertyGroup` elementu:

    [!code-xml[Main](deploying-to-production/samples/sample3.xml)]

    Plik .pubxml wygląda teraz następująco:

    [!code-xml[Main](deploying-to-production/samples/sample4.xml?highlight=18-20)]

    Aby uzyskać więcej informacji o tym, jak wykluczyć pliki i foldery, zobacz [czy można wykluczyć określone pliki lub foldery z wdrożenia?](https://msdn.microsoft.com/library/ee942158.aspx#can_i_exclude_specific_files_or_folders_from_deployment) w **często zadawane pytania wdrażania w sieci Web dla programu Visual Studio i platformy ASP.NET** w witrynie MSDN.

### <a name="deploy-to-production"></a>Wdrażanie w środowisku produkcyjnym

1. Otwórz **publikowanie w sieci Web** kreatora, upewnij się, że **produkcji** profil publikowania jest zaznaczone, a następnie kliknij przycisk **Uruchom Podgląd** na **Podgląd**kartę, aby sprawdzić, czy *robots.txt* pliku nie zostaną skopiowane do aplikacji produkcyjnej.

    ![Podgląd plików do opublikowania w środowisku produkcyjnym](deploying-to-production/_static/image14.png)

    Przejrzyj listę plików, które zostaną skopiowane. Zobaczysz, że wszystkie *.cs* pliki, w tym *. aspx.cs*, *. aspx.designer.cs*, *Master.cs*, i  *Master.Designer.cs* pliki są pomijane. Cały ten kod został skompilowany do *ContosoUniversity.dll* i *ContosUniversity.pdb* pliki, które znajdują się w *bin* folderu. Ponieważ tylko *.dll* jest potrzebny do uruchomienia aplikacji, a określony wcześniej powinny być wdrażane tylko pliki potrzebne do uruchomienia aplikacji, nie *.cs* pliki zostały skopiowane do lokalizacji docelowej środowisko. *Obj* folder i *ContosoUniversity.csproj* i *. csproj.user* pliki są pomijane dla tego samego powodu.

    Kliknij przycisk **Publikuj** do wdrożenia do środowiska produkcyjnego.
2. Testowanie w produkcji, zgodnie z tej samej procedury, których użyto podczas przemieszczania.

    Wszystko, co jest taka sama jak przemieszczania, z wyjątkiem adresu URL i braku *robots.txt* pliku.

## <a name="summary"></a>Podsumowanie

Teraz pomyślnie wdrożyć i przetestować aplikację sieci web i jest dostępny publicznie w Internecie.

![Strona główna w środowisku produkcyjnym](deploying-to-production/_static/image15.png)

W następnym samouczku możesz zaktualizować kod aplikacji i wdrażanie zmiany w środowiskach testowych, przejściowych i produkcyjnych.

> [!NOTE]
> Gdy aplikacja jest używana w środowisku produkcyjnym powinny być implementowanie planu odzyskiwania. Oznacza to powinny być okresowo kopie zapasowe baz danych z aplikacji produkcyjnej do lokalizacji bezpiecznego magazynu, a powinien utrzymywanie kilku generacje tych kopii zapasowych. Po zaktualizowaniu bazy danych powinno się wykonanie kopii zapasowej z bezpośrednio przed zmianą. Następnie jeśli popełnienia błędu, a nie odnaleźć go do czasu, po wdrożeniu do środowiska produkcyjnego, nadal będzie można odzyskać bazy danych do stanu, w jakim był, zanim zostanie ona uszkodzona. Aby uzyskać więcej informacji, zobacz [i przywracania kopii zapasowych bazy danych SQL Azure](https://msdn.microsoft.com/library/windowsazure/jj650016.aspx).
> 
> 
> [!NOTE]
> W tym samouczku programu SQL Server edition, w której przeprowadzasz wdrożenie to Azure SQL Database. Podczas procesu wdrażania jest podobny do innych wersjach programu SQL Server, aplikacja rzeczywistej produkcji mogą wymagać specjalnego kodu usługi Azure SQL Database w niektórych scenariuszach. Aby uzyskać więcej informacji, zobacz [pracy z usługą Azure SQL Database](../../../../whitepapers/aspnet-data-access-content-map.md#ssdb) i [Wybieranie między programami SQL Server i usługi Azure SQL Database](../../../../whitepapers/aspnet-data-access-content-map.md#ssdbchoosing).
> 
> [!div class="step-by-step"]
> [Poprzednie](setting-folder-permissions.md)
> [dalej](deploying-a-code-update.md)
