---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12
title: 'Wdrażanie aplikacji sieci Web ASP.NET za pomocą programu SQL Server Compact przy użyciu programu Visual Studio lub Visual Web Developer: Wdrażanie w środowisku produkcyjnym — 7 12 | Dokumentacja firmy Microsoft'
author: tdykstra
description: W tej serii samouczków dowiesz się, jak wdrożyć (opublikować) programu ASP.NET projektu aplikacji sieci web, który zawiera bazę danych programu SQL Server Compact przy użyciu Visual Stu...
ms.author: aspnetcontent
ms.date: 11/17/2011
ms.assetid: b83ab819-2b05-4776-b7b4-79ef78d457a5
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12
msc.type: authoredcontent
ms.openlocfilehash: d60161744a1aae564613239d17067c7f1515571a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830571"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-to-the-production-environment---7-of-12"></a>Wdrażanie aplikacji sieci Web ASP.NET za pomocą programu SQL Server Compact przy użyciu programu Visual Studio lub Visual Web Developer: Wdrażanie w środowisku produkcyjnym — 7 12
====================
przez [Tom Dykstra](https://github.com/tdykstra)

[Pobieranie projektu startowego](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> W tej serii samouczków dowiesz się, jak wdrożyć (opublikować) ASP.NET projektu aplikacji sieci web, która zawiera bazę danych programu SQL Server Compact przy użyciu programu Visual Studio 2012 RC lub Visual Studio Express 2012 RC for Web. Umożliwia także programu Visual Studio 2010 po zainstalowaniu aktualizacji publikowania w sieci Web. Aby zapoznać się z wprowadzeniem do serii, zobacz [pierwszym samouczku tej serii](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Aby uzyskać samouczek, który zawiera funkcje wdrażania wprowadzone po wersji RC programu Visual Studio 2012, pokazuje, jak wdrażać wersje programu SQL Server, innym niż SQL Server Compact i pokazuje, jak wdrożyć w usłudze Azure App Service Web Apps, zobacz [wdrażanie aplikacji internetowych ASP.NET za pomocą programu Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Omówienie

W ramach tego samouczka, możesz skonfigurować konto u dostawcy usług hostingowych i wdrożyć swoich aplikacjach ASP.NET aplikacji sieci web w środowisku produkcyjnym za pomocą programu Visual Studio jednym kliknięciem publikować funkcji.

Przypomnienie: Jeśli otrzymujesz komunikat o błędzie lub coś nie działa podczas wykonywania kroków samouczka, upewnij się sprawdzić [strona rozwiązywania problemów](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="selecting-a-hosting-provider"></a>Wybieranie dostawcy usług hostingowych

Aplikacja Contoso University i w tej serii samouczków należy dostawcę, który obsługuje platformy ASP.NET 4 i narzędzia Web Deploy. Określone firmę hostingową został wybrany tak, aby samouczków można zilustrować kompleksowe środowisko wdrażania w aktywnej witrynie sieci Web. Każdej firmy hostingowej zawiera różne funkcje i środowisko wdrażania serwerów różni się nieco. Jednak procesu opisanego w tym samouczku jest typowe dla całego procesu. Dostawcy hostingu używane w tym samouczku Cytanium.com, jest jednym z wielu, które są dostępne, a jej użycie w ramach tego samouczka nie stanowi poręczenia lub zalecenia.

Gdy jesteś gotowy do wybrania dostawcy hostingu, możesz porównać funkcje i rabaty w [galerii dostawcy](https://www.microsoft.com/web/hosting) witrynie Microsoft.com/web.

## <a name="creating-an-account"></a>Tworzenie konta

Utwórz konto u wybranego dostawcy. Jeśli obsługa pełnej bazy danych SQL Server jest dodano dodatkowych, nie musisz wybrać go na potrzeby tego samouczka, ale będzie to niezbędne dla [migracji do programu SQL Server](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md) samouczków w dalszej części tej serii.

Te samouczki nie trzeba zarejestrować nową nazwę domeny. Możesz przetestować, aby zweryfikować pomyślne wdrożenie przy użyciu tymczasowego adresu URL, przypisany do lokacji przez dostawcę.

Po utworzeniu konta otrzymasz zazwyczaj powitalnej wiadomości e-mail, który zawiera wszystkie informacje potrzebne do wdrożenia i Zarządzaj swoją witryną. Twój dostawca hostingu wyśle do Ciebie informacje będą podobne do przedstawionego w tym miejscu. Cytanium powitalnej wiadomości e-mail wysyłane w przypadku nowego konta użytkownika zawiera następujące informacje:

- Adres URL do witryny panelu kontroli dostawcy, gdzie możesz zmieniać ustawienia dla danej witryny. Identyfikator i hasło określone znajdują się w tej części powitalnej wiadomości e-mail, aby uzyskać łatwy dostęp. (Zarówno zostały zmienione na wartość pokaz na tej ilustracji.)

    [![Welcome_Email_Control_Panel_URL](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image1.png)
- Domyślna wersja .NET Framework i informacje o tym, jak je zmienić. Wiele hostingu witryn domyślnie 2.0, który działa z aplikacjami ASP.NET na platformie .NET Framework 2.0, 3.0 lub 3.5. Jednak University firmy Contoso jest w przypadku aplikacji .NET Framework 4, więc trzeba zmienić to ustawienie. (Dla aplikacji ASP.NET 4.5 należy użyć ustawień programu .NET 4.0.)

    [![Welcome_Email_Framework_Version](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image3.png)
- Tymczasowe adres URL umożliwia dostęp do witryny sieci web. Podczas tworzenia tego konta, "contosouniversity.com" została podana jako istniejącej nazwy domeny. W związku z tym tymczasowe adres URL jest `http://contosouniversity.com.vserver01.cytanium.com`.

    [![Welcome_Email_Temporary_URL](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image6.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image5.png)
- Informacje dotyczące sposobu konfigurowania bazy danych i parametry połączenia potrzebne w celu uzyskania dostępu do nich:

    [![Welcome_Email_Database_Info](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image7.png)
- Informacje o narzędziach i ustawienia wdrażania witryny. (Wiadomości e-mail z Cytanium zawiera również WebMatrix pominiętą w tym miejscu.)

    [![Welcome_Email_Deploy_info](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image10.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image9.png)

## <a name="setting-the-net-framework-version"></a>Ustawienie wersji programu .NET Framework

Cytanium powitalną wiadomość e-mail zawiera link do instrukcji dotyczących sposobu zmiany wersji programu .NET Framework. W instrukcjach wyjaśniono, że można to zrobić za pomocą Panelu sterowania Cytanium. Innych dostawców ma witryny Panelu sterowania, które wyglądają inaczej, ale można też ich może można to zrobić w inny sposób.

Przejdź do adresu URL panelu sterowania. Po zalogowaniu się przy użyciu nazwy użytkownika i hasła, zostanie wyświetlony panel sterowania.

[![Cytanium_Control_Panel](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image12.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image11.png)

W **hostingu miejsca do magazynowania** kursora myszy na ikonie na sieci Web i zaznacz **witryn sieci Web** z menu.

[![Cytanium_Control_Panel_selecting_Web_Sites](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image14.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image13.png)

W **witryn sieci Web** kliknij **contosouniversity.com** (nazwa lokacji, która została użyta podczas tworzenia konta).

[![Cytanium_Control_Panel_selecting_contosouniversity](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image15.png)

W **właściwości witryny sieci Web** wybierz opcję **rozszerzenia** kartę.

[![Cytanium_Control_Panel_Extensions_tab](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image17.png)

Zmiana platformy ASP.NET z **2.0 zintegrowanego potoku** do **4.0 (zintegrowanego potoku)**, a następnie kliknij przycisk **aktualizacji**.

## <a name="publishing-to-the-hosting-provider"></a>Publikowanie dostawcy hostingu

Powitalnej wiadomości e-mail od dostawcy hostingu zawiera wszystkie ustawienia potrzebne, aby opublikować projekt, a następnie możesz ręcznie wprowadzić te informacje do profilu publikowania. Ale użyjemy łatwiejsze i mniej podatne na błędy metoda Aby skonfigurować wdrożenie dostawcy: konieczne będzie pobranie *.publishsettings* pliku i zaimportować go do profilu publikowania.

W przeglądarce przejdź do panelu sterowania Cytanium, a następnie wybierz pozycję **Web** , a następnie wybierz **witryn sieci Web.**

![Panel sterowania z wybraniu witryny sieci Web](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image19.png)

Wybierz **contosouniversity.com** witryny sieci web.

![Panel sterowania z wybraniu contosouniversity.com](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image20.png)

Wybierz **publikowania w sieci Web** kartę.

![Karta publikowania w sieci Web Panel sterowania](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image21.png)

Utwórz poświadczenia na potrzeby publikowania, wprowadzając nazwę użytkownika i hasło w sieci web. Możesz wprowadzić te same poświadczenia, których używasz do logowania się do panelu sterowania. Następnie kliknij przycisk **Włącz**.

![Panel sterowania Tworzenie poświadczeń publikowania](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image22.png)

Kliknij przycisk **Pobierz profil publikowania dla tej witryny sieci web**.

![Kontrolka panelu Pobierz profil publikowania](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image23.png)

Po wyświetleniu monitu, aby otworzyć lub zapisać plik, zapisz go.

![Zapisz plik profilu publikowania](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image24.png)

W **Eksploratora rozwiązań** w programie Visual Studio kliknij prawym przyciskiem myszy projekt ContosoUniversity, a następnie wybierz **Publikuj**. **Publikowanie w sieci Web** otworzy się okno dialogowe **Podgląd** kartę za pomocą **testu** wybrano, ponieważ jest to ostatni profilu, możesz użyć profilu.

Wybierz **profilu** kartę, a następnie kliknij przycisk **importu**.

![Publikowanie przycisk Importuj Kreatora sieci Web](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image25.png)

W **importowanie ustawień publikowania** okno dialogowe, wybierz opcję *.publishsettings* którego pobrano plik, a następnie kliknij przycisk **Otwórz**. Kreator prowadzi do na karcie połączenie ze wszystkimi polami wypełnione.

![Opublikuj na karcie Połączenie Kreatora sieci Web](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image26.png)

Plik .publishsettings umieszcza planowane stały adres URL witryny w polu docelowy adres URL, ale jeśli jeszcze nie zostało zakupione tej domeny, zastąp wartość symbolu tymczasowego adresu URL. W tym przykładzie adres URL jest  *[ http://contosouniversity.com.vserver01.cytanium.com ](http://contosouniversity.com.vserver01.cytanium.com).* Jedynym celem to pole jest Podaj adres URL, które zostanie otwarta przeglądarka automatycznie po pomyślnym po wdrożeniu. Jeśli pole pozostanie puste, konsekwencją tylko jest to, przeglądarka nie uruchamia się automatycznie po wdrożeniu.

Kliknij przycisk **Waliduj połączenie** można sprawdzić, czy ustawienia są poprawne i czy można połączyć się z serwerem. Jak wcześniej, zielony znacznik wyboru sprawdza, czy połączenie zostanie nawiązane.

Po kliknięciu Waliduj połączenie, można napotkać **błąd certyfikatu** okno dialogowe. Jeśli to zrobisz, sprawdź, czy nazwa serwera to, czego oczekiwać. Jeśli tak jest, wybierz **Zapisz ten certyfikat na potrzeby przyszłych sesji programu Visual Studio** i kliknij przycisk **Akceptuj**. (Ten błąd oznacza, że uniknąć konieczności zakup certyfikatu protokołu SSL dla adresu URL, który jest wdrażany z wybrał dostawcy hostingu. Jeśli chcesz nawiązać bezpiecznego połączenia przy użyciu prawidłowego certyfikatu, skontaktuj się z dostawcą hostingu.)

![Błąd certyfikatu](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image27.png)

Kliknij przycisk **Dalej**.

W **baz danych** części **ustawienia** wprowadź w taki sam profil publikowania wartości, które zostały wprowadzone dla testu. Parametry połączenia, czego potrzebujesz znajdziesz w listach rozwijanych.

- W polu ciągu połączenia dla **SchoolContext,** wybierz `Data Source=|DataDirectory|School-Prod.sdf`
- W obszarze **SchoolContext**, wybierz opcję **zastosować migracje Code First**.
- W polu ciągu połączenia dla **DefaultConnection**, wybierz opcję `Data Source=|DataDirectory|aspnet-Prod.sdf`
- W obszarze **DefaultConnection**, pozostaw **Aktualizuj bazę danych** wyczyszczone.

![Publikowanie na karcie Ustawienia kreatora w sieci Web](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image28.png)

Kliknij przycisk **Dalej**.

W **Podgląd** kliknij pozycję **Uruchom Podgląd** umożliwia wyświetlenie listy plików, które zostaną skopiowane. Zobaczysz tę samą listę, która była widoczna wcześniej podczas wdrażania do usług IIS na komputerze lokalnym.

Przed jego opublikowaniem, Zmień nazwę profilu, tak, aby plik transformacji Web.Production.config zostaną zastosowane. Wybierz **profilu** kartę, a następnie kliknij przycisk **spravovat profily**.

![Publikowanie w sieci Web Kreator spravovat profily](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image29.png)

W **Edytuj profile publikowania w sieci Web** okna dialogowego pole, wybierz profil produkcji, kliknij przycisk **Zmień nazwę**i Zmień nazwę profilu w środowisku produkcyjnym. Następnie kliknij przycisk **Zamknij**.

![Edytuj profile publikowania w sieci Web, okno dialogowe](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image30.png)

Kliknij przycisk **publikowania**.

Aplikacja została opublikowana do dostawcy hostingu. Wynik zawiera w **dane wyjściowe** okna.

![Okno danych wyjściowych po wdrożeniu](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image31.png)

Przeglądarka automatycznie otwiera adres URL, który został wprowadzony w **docelowy adres URL** polu na **połączenia** karcie **publikowanie w sieci Web** kreatora. Wyświetlona ta sama strona główna jako po uruchomieniu lokacji w programie Visual Studio, z wyjątkiem obecnie nie ma żadnych "(Test)" lub "(Dev)" wskaźnik środowiska na pasku tytułu. Oznacza to, że wskaźnik środowiska *Web.config* przekształcania działało poprawnie.

> [!NOTE]
> Jeśli nadal widzisz "(Test)" w nagłówku, Usuń *obj* folderu z ContosoUniversity projektu i ponowne wdrażanie. W wersji wstępnej wersji oprogramowania zastosowane wcześniej transformacji pliku (Web.Test.config) mogą zostać zastosowane ponownie mimo, że używasz profilu w środowisku produkcyjnym.


[![Home_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image33.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image32.png)

Przed uruchomieniem strony, który powoduje, że dostęp do bazy danych, upewnij się, że Elmah będą mogli rejestrować wszystkie błędy.

## <a name="setting-folder-permissions-for-elmah"></a>Ustawianie uprawnień do folderów dla biblioteki Elmah

Jak pamiętasz z poprzedniego samouczka z tej serii, należy się upewnić, że aplikacja ma uprawnienia do zapisu dla folderu w aplikacji, w którym Elmah przechowuje pliki dziennika błędów danych. Podczas wdrażania do usług IIS lokalnie na komputerze, możesz ustawić te uprawnienia ręcznie. W tej sekcji pokazano, jak można ustawić uprawnień w Cytanium. Niektórzy dostawcy hostingu może nie pozwalać na to, (może oferują one co najmniej jeden wstępnie zdefiniowany folder z uprawnieniami do zapisu. W takim przypadku trzeba zmodyfikować aplikację do określonych folderów.)

Można ustawić uprawnienia do folderu, w Panelu sterowania Cytanium. Przejdź do kontrolki panelu adresu URL, a następnie wybierz pozycję **Menedżera plików**.

[![Cytanium_Control_Panel_with_File_Manager_selected](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image35.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image34.png)

W **Menedżera plików** wybierz opcję **contosouniversity.com** i następnie **wwwrooot** się folderze głównym. Kliknij ikonę kłódki **Elmah**.

[![Cytanium_Control_Panel_File_Manager_at_root_folder](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image37.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image36.png)

W **pliku**/**uprawnienia do folderu** wybierz **odczytu** i **zapisu** pola wyboru dla  **contosouniversity.com** i kliknij przycisk **Set Permissions**.

[![Cytanium_Control_Panel_File_Folder_Permissions_Elmah](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image39.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image38.png)

Upewnij się, że Elmah ma dostęp do zapisu *Elmah* folderu, co powoduje wystąpienie błędu, a następnie wyświetlając raport o błędach Elmah. Nieprawidłowy adres URL, takich jak żądania *Studentsxxx.aspx*. Jak widać, *GenericErrorPage.aspx* strony. Kliknij przycisk **Wyloguj** łącze, a następnie uruchom *Elmah.axd*. Możesz uzyskać **logowanie** najpierw strony, która sprawdza, czy *Web.config* przekształcenie pomyślnie dodano Elmah autoryzacji. Po zalogowaniu zobaczysz raport, który pokazuje błąd, po prostu spowodowane.

[![Elmah.axd_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image41.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image40.png)

## <a name="testing-in-the-production-environment"></a>Testowanie w środowisku produkcyjnym

Uruchom **studentów** strony. Aplikacja próbuje dostęp do bazy danych School po raz pierwszy, co powoduje wyzwolenie migracje Code First, aby utworzyć bazę danych. Zostanie wyświetlona strona opóźnieniem moment, pokazuje, że nie istnieją żadne studentów.

[![Students_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image43.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image42.png)

Uruchom **Instruktorzy** stronę, aby sprawdzić, czy dane inicjatora pomyślnie wstawione przez instruktorów danych w bazie danych.

[![Instructors_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image45.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image44.png)

Tak jak w środowisku testowym, którą chcesz zweryfikować, że aktualizacje bazy danych działa w środowisku produkcyjnym, ale zwykle nie chcesz wprowadzić dane testowe do produkcyjnej bazy danych. W tym samouczku użyjesz tej samej metody, które były wykonane w teście. Ale w rzeczywistej aplikacji możesz chcieć znaleźć metodę, która sprawdza poprawność tej bazy danych są aktualizacje pomyślnie bez wprowadzania danych testowych do produkcyjnej bazy danych. W niektórych aplikacjach może być niepraktyczne coś, co dodać i usunąć go.

Dodać Studenta, a następnie Wyświetl dane wprowadzone w **studentów** stronę, aby sprawdzić, czy można aktualizować dane w bazie danych.

[![Add_Students_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image47.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image46.png)

[![Students_page_with_new_student_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image49.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image48.png)

Zweryfikuj, że reguły autoryzacji są poprawne, wybierając **aktualizacji środki na korzystanie z** z **kursów** menu. **Logowanie** zostanie wyświetlona strona. Wprowadź poświadczenia konta administratora, kliknij przycisk **logowanie**i **aktualizacji środki na korzystanie z** zostanie wyświetlona strona.

[![Log_In_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image51.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image50.png)

Jeśli nazwa logowania to się powiedzie, **aktualizacji środki na korzystanie z** zostanie wyświetlona strona. Oznacza to, że bazy danych członkostwa platformy ASP.NET (za pomocą konta jednego administratora) został pomyślnie wdrożony.

[![Update_Credits_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image53.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image52.png)

Zostały pomyślnie wdrożone i przetestowane witryny i jest dostępny publicznie w Internecie.

## <a name="creating-a-more-reliable-test-environment"></a>Tworzenie bardziej niezawodne środowisko testowe

Jak wyjaśniono w [wdrażanie w środowisku testowania](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) samouczku najbardziej niezawodnym środowisku testowym będzie drugiego konta u dostawcy hostingu, który ma podobnie jak w przypadku konta w środowisku produkcyjnym. Jest to bardziej kosztowne niż za pomocą lokalnych usług IIS jako środowisku testowym, ponieważ musiałaby drugiego konta hostingu. Ale jeśli zapobiega produkcji lokacji błędy lub awarie, można zdecydować o jest warte.

Większość procesów tworzenia i wdrażania, aby konto testowe są podobne do jakie czynności zostały już wykonane do wdrażania w środowisku produkcyjnym:

- Tworzenie *Web.config* plik przekształcenia.
- Utwórz konto u dostawcy hostingu.
- Utwórz nowy profil publikowania i wdrażania do konta testowego.

### <a name="preventing-public-access-to-the-test-site"></a>Zapobieganie publicznego dostępu do witryny testu

Ważną kwestią do konta testowego jest będą na żywo w Internecie, że nie ma publicznego z niego korzystać. Aby zapewnić prywatność lokacji można użyć co najmniej jeden z następujących metod:

- Skontaktuj się z dostawcy hostingu, ustawianie reguł zapory, które umożliwiają dostęp do testowania witryny tylko z adresów IP, których można używać do testowania.
- Adres URL ukryć, tak, aby nie jest on podobny do adresu URL witryny publicznych.
- Użyj *robots.txt* pliku, aby zapewnić, że aparatów wyszukiwania będzie Przeszukuj łączy lokacji i raportu testu do niego w wynikach wyszukiwania.

Pierwsza z tych metod jest oczywiście najbezpieczniejsza opcja, ale procedura, jest przeznaczony dla każdego dostawcy hostingu i nie zostały omówione w tym samouczku. Jeśli zostały rozmieszczone z dostawcą hostingu, aby zezwolić tylko adresu IP w celu przejdź do adresu URL konta testowego, teoretycznie nie masz już martwić się o wyszukiwarki przeszukiwania go. Ale nawet w takim przypadku wdrożenie *robots.txt* pliku dobrym pomysłem jest do przechowywania kopii zapasowych w przypadku tej reguły zapory kiedykolwiek przypadkowo jest wyłączona.

*Robots.txt* plików znajduje się w folderze projektu i powinien mieć następujący tekst w nim:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/samples/sample1.cmd)]

`User-agent` Wiersz zawiera informacje dla aparatów wyszukiwania, które reguły w pliku mają zastosowanie do wszystkich aparatu web przeszukiwarki (robotom) i `Disallow` wiersz określa, że można przeszukać stron w witrynie.

Prawdopodobnie chcesz aparaty wyszukiwania wykazu witrynę produkcyjną, więc należy wykluczyć ten plik z wdrożenia produkcyjnego. Aby to zrobić, zobacz **czy można wykluczyć określone pliki lub foldery z wdrożenia?** w [ASP.NET Web Application projektu wdrożenia — często zadawane pytania](https://msdn.microsoft.com/library/ee942158.aspx#can_i_exclude_specific_files_or_folders_from_deployment). Upewnij się, że wykluczenie tylko w przypadku profilu publikowania w środowisku produkcyjnym.

Tworzenie drugiego konta hostingu to podejście pracę z usługą środowiska testowego, który nie jest wymagana, ale może być warte włożonej dodano wydatków. W ramach następujących samouczków będziesz nadal używać usług IIS jako środowisku testowym.

W następnym samouczku możesz zaktualizować kod aplikacji i wdrażanie zmiany w środowiskach testowych i produkcyjnych.

> [!div class="step-by-step"]
> [Poprzednie](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md)
> [dalej](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
