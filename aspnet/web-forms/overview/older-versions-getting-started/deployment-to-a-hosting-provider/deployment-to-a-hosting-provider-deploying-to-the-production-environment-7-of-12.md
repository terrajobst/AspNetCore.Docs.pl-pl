---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12
title: 'Wdrażanie aplikacji sieci Web ASP.NET przy użyciu programu Visual Studio lub Visual Web Developer programu SQL Server Compact: wdrażania w środowisku produkcyjnym - 7 12 | Dokumentacja firmy Microsoft'
author: tdykstra
description: Tej serii samouczków opisano sposób wdrażania platformy ASP.NET (publikowanie) projektu aplikacji sieci web, który zawiera bazę danych programu SQL Server Compact przy użyciu Visual Stu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: b83ab819-2b05-4776-b7b4-79ef78d457a5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12
msc.type: authoredcontent
ms.openlocfilehash: ab3b7ba332deddae7d04fc37c7aabc72bdb2d17e
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "30889686"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-to-the-production-environment---7-of-12"></a>Wdrażanie aplikacji sieci Web ASP.NET przy użyciu programu Visual Studio lub Visual Web Developer programu SQL Server Compact: wdrażania w środowisku produkcyjnym - 7 12
====================
Przez [Dykstra niestandardowy](https://github.com/tdykstra)

[Pobierz początkowego projektu](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Tej serii samouczków opisano sposób wdrażania platformy ASP.NET (publikowanie) projektu aplikacji sieci web, który zawiera bazę danych programu SQL Server Compact przy użyciu programu Visual Studio 2012 RC i Visual Studio Express 2012 RC for Web. W razie musisz zainstalować aktualizację publikowania w sieci Web, można również używać programu Visual Studio 2010. Aby obejrzeć wprowadzenie do serii, zobacz [pierwszy samouczek z tej serii](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Samouczek zawiera funkcje wdrażania dodane po wydaniu programu Visual Studio 2012 RC, pokazuje, jak wdrożyć wersjach programu SQL Server innych niż SQL Server Compact, która przedstawia sposób wdrażania aplikacji sieci Web usługi aplikacji Azure, zobacz [wdrożenia sieci Web ASP.NET za pomocą programu Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Omówienie

W tym samouczku skonfigurowane konto z dostawcą hostingu i wdrażanie programu ASP.NET funkcji publikowania aplikacji sieci web w środowisku produkcyjnym za pomocą programu Visual Studio jednym kliknięciem.

Przypomnienie: Jeśli coś nie działa podczas wykonywania kroków samouczka wyświetlony komunikat o błędzie, należy sprawdzić [Rozwiązywanie problemów z strony](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="selecting-a-hosting-provider"></a>Wybieranie dostawcy hostingu

Aplikacja Contoso University i tego samouczka serii należy dostawcy, który obsługuje platformy ASP.NET 4 oraz narzędzia Web Deploy. Określonych firma została wybrana, dzięki czemu samouczków można zilustrować pełne środowisko wdrożenia na żywo witryny sieci Web. Każda firma zapewnia różne funkcje, a środowisko wdrażania serwerów różni się nieco. Jednak proces opisany w tym samouczku jest typowa dla całego procesu. Dostawca hostingu używane w tym samouczku Cytanium.com, jest jednym z wielu, które są dostępne, a jego użycie w tym samouczku nie stanowi potwierdzenie lub zalecenia.

Gdy wszystko będzie gotowe wybrać dostawcy hostingu, możesz porównać funkcji i cen w [galerii dostawców](https://www.microsoft.com/web/hosting) w witrynie Microsoft.com/web.

## <a name="creating-an-account"></a>Tworzenie konta

Utwórz konto w wybranego dostawcy. Jeśli obsługa pełnej bazy danych SQL Server jest dodany bardzo, nie musisz wybrać go do celów tego samouczka, ale będą potrzebne dla [migracji do programu SQL Server](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md) samouczek później w tej serii.

Te samouczki nie trzeba zarejestrować nową nazwę domeny. Możesz przetestować, aby zweryfikować pomyślne wdrożenie przy użyciu tymczasowego adresu URL przypisane do lokacji przez dostawcę.

Po utworzeniu konta otrzymasz zwykle powitalnej wiadomości e-mail, który zawiera wszystkie informacje wymagane do wdrażania i zarządzania witryną. Twój dostawca hostingu wysyła informacje będą podobne do co przedstawiono w tym miejscu. Cytanium powitalnej wiadomości e-mail wysyłanej do nowego konta użytkownika zawiera następujące informacje:

- Adres URL do lokacji Panelu sterowania dostawcy, którym można zarządzać ustawieniami witryny. Identyfikator i hasło zostaną uwzględnione w tej części powitalnej wiadomości e-mail dla ułatwienia. (Zarówno zostały zmienione na wartość pokaz na tej ilustracji.)

    [![Welcome_Email_Control_Panel_URL](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image1.png)
- Domyślna wersja programu .NET Framework i informacji na temat sposobu zmiany. Wiele hostingu witryn domyślnie 2.0, który współpracuje z aplikacjami ASP.NET, które odnoszą się do programu .NET Framework 2.0, 3.0 lub 3.5. Jednak Contoso University jest aplikacji .NET Framework 4, dlatego należy zmienić to ustawienie. (Dla aplikacji ASP.NET 4.5 może użyć ustawienia programu .NET 4.0.)

    [![Welcome_Email_Framework_Version](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image3.png)
- Tymczasowy adres URL, który umożliwia dostęp do witryny sieci web. Podczas tworzenia tego konta, "contosouniversity.com" została podana jako istniejącej nazwy domeny. W związku z tym tymczasowy adres URL jest `http://contosouniversity.com.vserver01.cytanium.com`.

    [![Welcome_Email_Temporary_URL](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image6.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image5.png)
- Informacje dotyczące sposobu konfigurowania bazy danych i parametry połączenia, które są potrzebne do nich dostęp:

    [![Welcome_Email_Database_Info](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image7.png)
- Informacje dotyczące narzędzia i ustawienia wdrażania witryny. (E-mail od Cytanium zawiera również programu WebMatrix, który został pominięty w tym miejscu.)

    [![Welcome_Email_Deploy_info](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image10.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image9.png)

## <a name="setting-the-net-framework-version"></a>Ustawienie wersji programu .NET Framework

Cytanium powitalnej wiadomości e-mail zawiera łącze do instrukcji dotyczących sposobu zmiany wersji programu .NET Framework. Poniższe instrukcje wyjaśniają, czy można to zrobić za pomocą Panelu sterowania Cytanium. Innych dostawców ma Lokacje Panelu sterowania różnić lub ich może nakazać można to zrobić w inny sposób.

Przejdź do adresu URL panelu sterowania. Po zalogowaniu się swoją nazwę użytkownika i hasło, zobacz temat panelu sterowania.

[![Cytanium_Control_Panel](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image12.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image11.png)

W **Hosting spacje** wskaźnik na ikonie sieci Web i zaznacz **witryn sieci Web** z menu.

[![Cytanium_Control_Panel_selecting_Web_Sites](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image14.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image13.png)

W **witryn sieci Web** kliknij **contosouniversity.com** (nazwę lokacji, który został użyty podczas tworzenia konta).

[![Cytanium_Control_Panel_selecting_contosouniversity](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image15.png)

W **właściwości witryny sieci Web** wybierz opcję **rozszerzenia** kartę.

[![Cytanium_Control_Panel_Extensions_tab](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image17.png)

Zmień ASP.NET z **2.0 zintegrowanego potoku** do **4.0 (zintegrowanego potoku)**, a następnie kliknij przycisk **aktualizacji**.

## <a name="publishing-to-the-hosting-provider"></a>Publikowanie do dostawcy usług hosta

Powitalnej wiadomości e-mail od dostawcy hostingu zawiera wszystkie ustawienia potrzebne do publikowania projektu i wprowadź te informacje ręcznie w profilu publikowania. Jednak użyjesz łatwiejsze i mniej podatne na błędy metody do skonfigurowania wdrożenia do dostawcy: konieczne będzie pobranie *.publishsettings* plików i zaimportuj go do profilu publikowania.

W przeglądarce przejdź do panelu sterowania Cytanium i wybierz **Web** , a następnie wybierz **witryn sieci Web.**

![Panel sterowania zaznaczenie witryn sieci Web](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image19.png)

Wybierz **contosouniversity.com** witryny sieci web.

![Wybieranie contosouniversity.com Panelu sterowania](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image20.png)

Wybierz **publikowania w sieci Web** kartę.

![Karta publikowania w sieci Web Panel sterowania](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image21.png)

Utwórz poświadczenia na potrzeby publikowania, wprowadzając nazwę użytkownika i hasło w sieci web. Można użyć tych samych poświadczeń, których używasz do logowania się do panelu sterowania. Następnie kliknij przycisk **włączyć**.

![Panel sterowania Tworzenie poświadczeń publikowania](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image22.png)

Kliknij przycisk **Pobierz profil publikowania dla tej witryny sieci web**.

![Pobieranie Panelu sterowania profilu publikowania](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image23.png)

Po wyświetleniu monitu można otworzyć lub zapisać plik, należy go zapisać.

![Zapisz plik profilu publikowania](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image24.png)

W **Eksploratora rozwiązań** w programie Visual Studio kliknij prawym przyciskiem myszy projekt ContosoUniversity, a następnie wybierz **publikowania**. **Publikowanie w sieci Web** zostanie otwarte okno dialogowe na **Podgląd** karcie z **testu** wybrany, ponieważ jest to ostatni profil używany profil.

Wybierz **profilu** a następnie kliknij pozycję **importu**.

![Publikowanie przycisk Importuj Kreatora sieci Web](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image25.png)

W **importowania ustawień publikowania** okno dialogowe, wybierz opcję *.publishsettings* którego pobrano pliku, a następnie kliknij przycisk **Otwórz**. Kreator przechodzi na karcie połączenie ze wszystkimi polami wypełnione.

![Publikowanie na karcie Połączenie Kreatora sieci Web](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image26.png)

Plik .publishsettings umieszcza planowane stały adres URL witryny w polu docelowy adres URL, ale jeśli nie została jeszcze zakupiona tej domeny, zastąp wartość tymczasowego adresu URL. Na przykład adres URL jest  *[ http://contosouniversity.com.vserver01.cytanium.com ](http://contosouniversity.com.vserver01.cytanium.com).* Jedynym celem tego pola jest aby określić adres URL, które zostanie otwarta przeglądarka automatycznie po pomyślnym po wdrożeniu. Jeśli pole pozostanie puste, tylko konsekwencją jest, że przeglądarka nie uruchamia się automatycznie po wdrożeniu.

Kliknij przycisk **sprawdzania poprawności połączenia** można sprawdzić, czy ustawienia są poprawne i czy można połączyć się z serwerem. Zielony znacznik wyboru, jak przedstawiono wcześniej, sprawdza, czy połączenie zostanie nawiązane.

Kliknięcie przycisku Sprawdź poprawność połączenia można napotkać **błąd certyfikatu** okno dialogowe. Jeśli to zrobisz, sprawdź, czy nazwa serwera jest oczekiwań. Jeśli tak jest, wybierz **Zapisz ten certyfikat na potrzeby przyszłych sesji programu Visual Studio** i kliknij przycisk **Akceptuj**. (Ten błąd oznacza, że uniknąć konieczności zakup certyfikatu SSL dla adresu URL, który jest wdrażany z wybrał dostawcy hostingu. Jeśli wolisz ustanowienia bezpiecznego połączenia przy użyciu prawidłowego certyfikatu, skontaktuj się z dostawcą hostingu.)

![Błąd certyfikatu](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image27.png)

Kliknij przycisk **Dalej**.

W **baz danych** sekcji **ustawienia** wprowadź takie same wartości, które zostały wprowadzone w teście profilu publikowania. Parametry połączenia, które są potrzebne znajdują się na liście rozwijanej.

- W polu Parametry połączenia dla **SchoolContext,** wybierz `Data Source=|DataDirectory|School-Prod.sdf`
- W obszarze **SchoolContext**, wybierz pozycję **zastosować migracje Code First**.
- W polu Parametry połączenia dla **połączenia DefaultConnection**, wybierz pozycję `Data Source=|DataDirectory|aspnet-Prod.sdf`
- W obszarze **połączenia DefaultConnection**, pozostaw **aktualizacji bazy danych** wyczyszczone.

![Publikowanie karcie Ustawienia Kreatora sieci Web](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image28.png)

Kliknij przycisk **Dalej**.

W **Podgląd** , kliknij pozycję **Uruchom Podgląd** umożliwia wyświetlenie listy plików, które zostaną skopiowane. Widzisz tę samą listę wyświetlane wcześniej podczas wdrażania usług IIS na komputerze lokalnym.

Przed opublikowaniem, Zmień nazwę profilu, aby plik transformacji Web.Production.config zostaną zastosowane. Wybierz **profilu** i kliknij polecenie **zarządzania profilami**.

![Publikowanie w sieci Web Kreatora zarządzania profilami](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image29.png)

W **edytować profile publikowania w sieci Web** oknie dialogowym Wybierz profil produkcji, kliknij przycisk **zmienić**i Zmień nazwę profilu w środowisku produkcyjnym. Następnie kliknij przycisk **Zamknij**.

![Edycja profilów publikowania w sieci Web, okno dialogowe](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image30.png)

Kliknij przycisk **publikowania**.

Aplikacja jest opublikowana do dostawcy hostingu. Przedstawia wynik **dane wyjściowe** okna.

![Okno dane wyjściowe po wdrożeniu](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image31.png)

Przeglądarka automatycznie otwiera adres URL wprowadzony w **docelowy adres URL** polu na **połączenia** karcie **publikowanie w sieci Web** kreatora. Zobacz stronę główną w tym samym jako po uruchomieniu lokacji w programie Visual Studio, ale teraz Brak "(testu)" lub "(deweloperów)" środowiska wskaźnika na pasku tytułu. Oznacza to, że wskaźnik środowiska *Web.config* przekształcania działało poprawnie.

> [!NOTE]
> Jeśli nadal "(Test)" w nagłówku, Usuń *obj* folder z projektu ContosoUniversity i utwórz je ponownie. W wersji wstępnych oprogramowania plik przekształcenia zastosowane wcześniej (Web.Test.config) mogą zostać zastosowane ponownie mimo że używasz profilu produkcji.


[![Home_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image33.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image32.png)

Przed uruchomieniem strony, który powoduje, że dostęp do bazy danych, upewnij się, że Elmah będzie można rejestrować wszystkie błędy.

## <a name="setting-folder-permissions-for-elmah"></a>Ustawianie uprawnień folderów dla Elmah

Jak pamiętasz z poprzednich samouczku tej serii, należy się upewnić, że aplikacja ma uprawnienia do zapisu w aplikacji w folderze, w którym Elmah przechowuje pliki dziennika błędów. Podczas wdrażania usług IIS lokalnie na komputerze, należy ustawić uprawnienia ręcznie. W tej sekcji pojawi się, jak ustawić uprawnienia na Cytanium. (Niektóre dostawcy hostingu nie włączać w tym; mogą one oferować co najmniej jeden wstępnie zdefiniowanych folderów z uprawnieniami do zapisu. W takim przypadku trzeba zmodyfikować aplikację do używania określonych folderów.)

Można ustawić uprawnienia do folderu, w Panelu sterowania Cytanium. Przejdź do kontrolki panelu adresu URL i wybierz **Menedżera plików**.

[![Cytanium_Control_Panel_with_File_Manager_selected](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image35.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image34.png)

W **Menedżera plików** wybierz opcję **contosouniversity.com** , a następnie **wwwrooot** do folderu głównego aplikacji. Kliknij ikonę kłódki **Elmah**.

[![Cytanium_Control_Panel_File_Manager_at_root_folder](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image37.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image36.png)

W **pliku**/**uprawnienia folderu** wybierz **odczytu** i **zapisu** pól wyboru dla  **contosouniversity.com** i kliknij przycisk **Ustaw uprawnienia**.

[![Cytanium_Control_Panel_File_Folder_Permissions_Elmah](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image39.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image38.png)

Upewnij się, że Elmah ma dostęp do zapisu *Elmah* folderu powoduje błąd, a następnie wyświetlanie raportu o błędach Elmah. Nieprawidłowy adres URL, takich jak żądania *Studentsxxx.aspx*. Jak wcześniej, zobacz *GenericErrorPage.aspx* strony. Kliknij przycisk **Wyloguj** łącza, a następnie uruchom *Elmah.axd*. Możesz uzyskać **dziennika w** strony, która sprawdza, czy *Web.config* przekształcenia pomyślnie dodano Elmah autoryzacji. Po zalogowaniu zobaczysz raportu, który zawiera błąd, który spowodował tylko.

[![Elmah.axd_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image41.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image40.png)

## <a name="testing-in-the-production-environment"></a>Testowanie w środowisku produkcyjnym

Uruchom **studentów** strony. Aplikacja próbuje dostęp do bazy danych służbowych po raz pierwszy, który wyzwala migracje Code First, aby utworzyć bazę danych. Na stronie są wyświetlane po chwili opóźnienie, pokazuje, czy nie występują żadne studentów.

[![Students_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image43.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image42.png)

Uruchom **instruktorów** stronę, aby sprawdzić, czy dane inicjatora pomyślnie wstawione instruktora danych w bazie danych.

[![Instructors_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image45.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image44.png)

Tak samo jak w środowisku testowym, należy sprawdzić, czy praca aktualizacje bazy danych w środowisku produkcyjnym, ale zwykle nie chcesz wprowadzić dane testowe do produkcyjnej bazy danych. W tym samouczku użyjesz tej samej metody, które były w teście. Jednak w rzeczywistej aplikacji można znaleźć metody, która weryfikuje tej bazy danych aktualizacje są pomyślnie bez wprowadzania danych testowych do produkcyjnej bazy danych. W niektórych aplikacjach może być praktyczne coś dodać i usunąć go.

Dodaj Studenta, a następnie Wyświetl dane wprowadzane w **studentów** stronę, aby sprawdzić, czy można zaktualizować danych w bazie danych.

[![Add_Students_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image47.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image46.png)

[![Students_page_with_new_student_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image49.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image48.png)

Zweryfikuj, że reguły autoryzacji są poprawne, wybierając **środków aktualizacji** z **kursów** menu. **Dziennika w** zostanie wyświetlona strona. Wprowadź poświadczenia konta administratora, kliknij polecenie **dziennika w**i **środków aktualizacji** zostanie wyświetlona strona.

[![Log_In_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image51.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image50.png)

Jeśli użytkownik zaloguje się ponownie, **środków aktualizacji** zostanie wyświetlona strona. Oznacza to, że bazy danych członkostwa ASP.NET (przy użyciu konta administratora pojedynczego) został pomyślnie wdrożony.

[![Update_Credits_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image53.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image52.png)

Teraz pomyślnie wdrożone i przetestowane lokacji i jest dostępny publicznie w Internecie.

## <a name="creating-a-more-reliable-test-environment"></a>Tworzenie bardziej niezawodne środowisko testowe

Zgodnie z objaśnieniem w [wdrażania w środowisku testowania](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) samouczek, najbardziej niezawodnym środowiska testowego byłoby drugiego konta u dostawcy hostingu, który ma podobnie jak konta produkcji. Będzie to droższe niż za pomocą lokalnych usług IIS jako środowiska testowego, ponieważ konieczne będzie przeprowadzenie drugiego konta hostingu. Jednak jeśli zapobiega produkcji lokacji błędy lub awarie, może zdecydować, że warto kosztów.

Większość proces tworzenia i wdrażania konta testowego jest podobny do co zostało już wykonane do wdrożenia w środowisku produkcyjnym:

- Utwórz *Web.config* plik przekształcenia.
- Utwórz konto u dostawcy hostingu.
- Utwórz nowy profil publikowania i wdrażania do konta testowego.

### <a name="preventing-public-access-to-the-test-site"></a>Zapobieganie publiczny dostęp do witryny testu

Ważną kwestią testowe konto jest będzie na żywo w Internecie, że nie ma publicznego z niego korzystać. Poufne lokacji można użyć co najmniej jeden z następujących metod:

- Skontaktuj się z dostawcą hostingu, można ustawić reguły zapory, które zezwalają na dostęp do testowania lokacji tylko z adresów IP, których używasz do testowania.
- Ukryć adres URL, dzięki czemu nie jest podobny do adresu URL witryny publicznej.
- Użyj *robots.txt* pliku, aby upewnić się, że aparatów wyszukiwania będzie Przeszukuj łącza lokacji i raportu testu do niego w wynikach wyszukiwania.

Pierwsza z tych metod jest oczywiście najwyższy poziom bezpieczeństwa, ale procedury w tym są specyficzne dla każdego dostawcy hostingu i nie zostaną opisane w tym samouczku. Jeśli możesz rozmieścić z dostawcą hostingu, aby zezwolić tylko adres IP przejść do adresu URL konta testu, teoretycznie nie trzeba martwić aparaty wyszukiwania przeszukiwania go. Ale nawet w takim przypadku wdrażanie *robots.txt* plik jest dobrze jako zapasowy na wypadek, gdyby kiedykolwiek przypadkowo wyłączeniu tej reguły zapory.

*Robots.txt* plik znajduje się w folderze projektu i powinna mieć następujący tekst, w którym:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/samples/sample1.cmd)]

`User-agent` Wiersza poleca aparatów wyszukiwania, które zasady w pliku dotyczą wszystkich aparat web przeszukiwarek (robotów), i `Disallow` wiersz określa, że można przeszukać stron w witrynie.

Aparaty wyszukiwania do katalogu witryny sieci produkcyjnej, więc należy wykluczyć ten plik z wdrożenia w środowisku produkcyjnym pożądane. Aby to zrobić, zobacz **można wykluczyć określone pliki lub foldery z wdrożenia?** w [ASP.NET sieci Web aplikacji projektu wdrożenia — często zadawane pytania](https://msdn.microsoft.com/library/ee942158.aspx#can_i_exclude_specific_files_or_folders_from_deployment). Upewnij się, określ wykluczenia, tylko dla profilu publikowania produkcji.

Tworzenie drugiego konta hostingu jest podejście do pracy z środowiska testowego nie jest wymagana, ale może być warto dodano wydatków. W następujących samouczkach nastąpi przejście do używania serwera IIS jako środowiska testowego.

W następnym samouczku możesz zaktualizować kodu aplikacji i wdrażanie zmiany do tych środowisk testowych i produkcyjnych.

> [!div class="step-by-step"]
> [Poprzednie](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md)
> [dalej](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
