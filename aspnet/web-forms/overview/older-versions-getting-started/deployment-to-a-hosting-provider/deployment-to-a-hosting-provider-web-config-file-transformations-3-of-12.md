---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12
title: 'Wdrażanie aplikacji sieci Web ASP.NET za pomocą programu SQL Server Compact przy użyciu programu Visual Studio lub Visual Web Developer: przekształcenia pliku Web.Config - 3 12 | Dokumentacja firmy Microsoft'
author: tdykstra
description: W tej serii samouczków dowiesz się, jak wdrożyć (opublikować) programu ASP.NET projektu aplikacji sieci web, który zawiera bazę danych programu SQL Server Compact przy użyciu Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: 2b0df3d9-450b-4ea6-b315-4c9650722cad
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12
msc.type: authoredcontent
ms.openlocfilehash: 9673887fb39874e1b4f063fa9cfe3470e4ccde00
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755865"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-webconfig-file-transformations---3-of-12"></a>Wdrażanie aplikacji sieci Web ASP.NET za pomocą programu SQL Server Compact przy użyciu programu Visual Studio lub Visual Web Developer: przekształcenia pliku Web.Config - 3 12
====================
przez [Tom Dykstra](https://github.com/tdykstra)

[Pobieranie projektu startowego](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> W tej serii samouczków dowiesz się, jak wdrożyć (opublikować) ASP.NET projektu aplikacji sieci web, która zawiera bazę danych programu SQL Server Compact przy użyciu programu Visual Studio 2012 RC lub Visual Studio Express 2012 RC for Web. Umożliwia także programu Visual Studio 2010 po zainstalowaniu aktualizacji publikowania w sieci Web. Aby zapoznać się z wprowadzeniem do serii, zobacz [pierwszym samouczku tej serii](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Aby uzyskać samouczek, który zawiera funkcje wdrażania wprowadzone po wersji RC programu Visual Studio 2012, pokazuje, jak wdrażać wersje programu SQL Server, innym niż SQL Server Compact i pokazuje, jak wdrożyć w usłudze Azure App Service Web Apps, zobacz [wdrażanie aplikacji internetowych ASP.NET za pomocą programu Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Omówienie

W tym samouczku dowiesz się, jak zautomatyzować proces zmiany *Web.config* plików podczas wdrażania w środowiskach różnych miejsc docelowych. Większość aplikacji ma ustawienia *Web.config* pliku, który musi być inna, gdy aplikacja jest wdrażana. Automatyzowanie procesu wprowadzania tych zmian wymaga znajomości zrobić je ręcznie, za każdym razem, gdy zostanie wdrożony, może być uciążliwe i podatne.

Przypomnienie: Jeśli otrzymujesz komunikat o błędzie lub coś nie działa podczas wykonywania kroków samouczka, upewnij się sprawdzić [strona rozwiązywania problemów](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="webconfig-transformations-versus-web-deploy-parameters"></a>Transformacje pliku Web.config w porównaniu z sieci Web wdrażanie parametrów

Istnieją dwa sposoby, aby zautomatyzować proces zmiany *Web.config* ustawienia plików: [transformacje pliku Web.config](https://msdn.microsoft.com/library/dd465326.aspx) i [parametrów narzędzia Web Deploy](https://msdn.microsoft.com/library/ff398068.aspx). A *Web.config* plik przekształcenia zawiera znacznik XML, który określa sposób zmiany *Web.config* pliku po jego wdrożeniu. Można określić różne zmiany dotyczące określonych konfiguracje kompilacji i dla określonych profilów publikowania. Domyślne konfiguracje kompilacji są Debug i Release, i można utworzyć konfiguracji niestandardowej kompilacji. Profil publikowania zazwyczaj odpowiada środowisku docelowym. (Dowiesz się więcej na temat profilów w publikowania [wdrażanie w usługach IIS jako środowisku testowym](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) samouczek.)

Parametry wdrażania sieci Web może służyć do określania wielu różnych rodzajów ustawień, które muszą być skonfigurowane podczas wdrażania, w tym ustawienia, które znajdują się w *Web.config* plików. Gdy jest używana do określenia *Web.config* zmiany plików są bardziej złożone, aby skonfigurować parametry narzędzie Web Deploy, ale są przydatne, gdy nie wiadomo, wartość można ustawić, dopóki nie zostanie wdrożony. Na przykład w środowisku przedsiębiorstwa może utworzyć *pakietu wdrożeniowego* oferowanie osoby z działu IT do zainstalowania w środowisku produkcyjnym i dana osoba można było wprowadzić parametry połączenia lub hasła, które nie obsługują znać.

Dla tego scenariusza, który w tym samouczku opisano, wiesz wszystko, co ma wykonać, aby *Web.config* pliku, więc nie trzeba używać parametrów narzędzia Web Deploy. Należy skonfigurować niektórych przekształceń, które różnią się w zależności od konfiguracji kompilacji używany, a niektóre, które różnią się w zależności od profil publikacji używany.

## <a name="creating-transformation-files-for-publish-profiles"></a>Tworzenie plików transformacji dla profilów publikowania

W **Eksploratora rozwiązań**, rozwiń węzeł *Web.config* się *Web.Debug.config* i *Web.Release.config* pliki transformacji Domyślnie tworzone są dwa domyślne konfiguracje kompilacji.

![Web.config_transform_files](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image1.png)

Można tworzyć pliki transformacji dla konfiguracji niestandardowej kompilacji, kliknij prawym przyciskiem myszy plik Web.config i wybierając pozycję **Dodaj transformacje konfiguracji** z menu kontekstowego, ale na potrzeby tego samouczka nie trzeba to zrobić.

Dwa pliki transformacji więcej, są potrzebne do konfigurowania zmiany, które odnoszą się do lokalizacji docelowej wdrożenia, a nie w konfiguracji kompilacji. Typowym przykładem tego rodzaju ustawienie to punktu końcowego WCF, która różni się do testowania i produkcji. W kolejnych samouczkach utworzysz publikowanie profili o nazwie testowym i produkcyjnym, dlatego należy *Web.Test.config* pliku i *Web.Production.config* pliku.

Pliki transformacji, które są powiązane z profilów publikowania należy utworzyć je ręcznie. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt ContosoUniversity i wybierz **Otwórz Folder w Eksploratorze Windows**.

![Open_folder_in_Windows_Explorer](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image2.png)

W **Eksplorator Windows**, wybierz opcję *Web.Release.config* pliku, skopiuj plik, a następnie wklej dwie kopie. Zmiana nazwy tych kopii *Web.Production.config* i *Web.Test.config*, następnie Zamknij **Eksplorator Windows**.

W **Eksploratora rozwiązań**, kliknij przycisk **Odśwież** aby zobaczyć nowe pliki.

Wybierz nowe pliki, kliknij prawym przyciskiem myszy, a następnie kliknij **Include w projekcie** w menu kontekstowym.

![W tym pliki konfiguracji testowych i produkcyjnych w projekcie](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image3.png)

Aby zapobiec wdrażane tych plików, zaznacz je w **Eksploratora rozwiązań**, a następnie w polu **właściwości** okna zmiany **Build Action** właściwość **Zawartości** do **Brak**. (Pliki transformacji, które są oparte na konfiguracji kompilacji automatycznie nie wdrażania.)

Teraz można przystąpić do wprowadzania *Web.config* przekształcenia do *Web.config* pliki transformacji.

## <a name="limiting-error-log-access-to-administrators"></a>Ograniczanie dostępu do dziennika błędów administratorów

Jeśli wystąpi błąd, gdy aplikacja zostanie uruchomiona, aplikacja wyświetli stronę ogólny błąd zamiast strony błędów generowanych przez system i używa [pakiet NuGet biblioteki Elmah](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) do rejestrowania i raportowania błędów. `customErrors` Element *Web.config* plik Określa stronę błędu:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample1.xml)]

Aby wyświetlić stronę błędu, tymczasowo zmienić `mode` atrybutu `customErrors` elementu "RemoteOnly" na wartość "Włączone" i uruchamianie aplikacji w programie Visual Studio. Powodują wystąpienie błędu, wysyłając żądanie nieprawidłowego adresu URL, takich jak *Studentsxxx.aspx*. Zamiast generowanych przez usługi IIS "page nie można odnaleźć" stronę błędu, zobacz *GenericErrorPage.aspx* strony.

[![Error_page](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image5.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image4.png)

Aby wyświetlić dziennik błędów, należy zastąpić wszystko, co w adresie URL po numer portu z *elmah.axd* (na przykład na zrzucie ekranu `http://localhost:51130/elmah.axd`) i naciśnij klawisz Enter:

[![Elmah_log_page](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image7.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image6.png)

Nie należy zapominać ustawić `customErrors` element do trybu "RemoteOnly", gdy wszystko będzie gotowe.

Na komputerze deweloperskim jest wygodne zezwolić na bezpłatny dostęp do strony dziennika błędów, ale w środowisku produkcyjnym, który będzie stanowić zagrożenie bezpieczeństwa. W przypadku witryny produkcyjnej, możesz dodać regułę autoryzacji, który ogranicza dostęp do dziennika błędów tylko dla administratorów, konfigurując przekształcenie w *Web.Production.config* pliku.

Otwórz *Web.Production.config* i Dodaj nowy `location` element natychmiast po otwarciu `configuration` tag, jak pokazano poniżej. (Upewnij się, że można dodawać tylko `location` elementu, a nie otaczającego znaczników, który jest pokazany tutaj tylko w celu udostępnienia określony kontekst.)

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample2.xml?highlight=2-9)]

`Transform` Wartość atrybutu "INSERT" powoduje, że to `location` element ma zostać dodany jako element równorzędny do wszystkich istniejących `location` elementów w *Web.config* pliku. (Istnieje już `location` element, który określa autoryzacji reguł dla **aktualizacji środki na korzystanie z** strony.) Kiedy testujesz witryny produkcyjnej po wdrożeniu będzie test, aby sprawdzić, czy ta reguła autoryzacji jest skuteczne.

Nie trzeba ograniczać dostęp do dziennika błędów w środowisku testowym, aby nie musieli Dodaj następujący kod do *Web.Test.config* pliku.

> [!NOTE] 
> 
> **Uwaga dotycząca zabezpieczeń** nigdy nie są wyświetlane szczegóły błędu publicznie w aplikacji produkcyjnej lub przechowywania tych informacji w ogólnodostępnej lokalizacji. Osoby atakujące umożliwia wykrywanie luk w zabezpieczeniach w lokacji informacje o błędzie. Jeśli używasz biblioteki ELMAH we własnej aplikacji, należy zbadać sposobów, w którym można skonfigurować ELMAH, aby zminimalizować zagrożenia bezpieczeństwa. W przykładzie ELMAH, w tym samouczku nie powinny być uwzględniane zalecanej konfiguracji. To przykład, który został wybrany w celu zilustrowania sposobu obsługi aplikacji musi umożliwiać do tworzenia plików w folderze.


## <a name="setting-an-environment-indicator"></a>Ustawienie wskazuje środowiska

Jest to typowy scenariusz *Web.config* ustawienia, które muszą być różne w każdym środowisku, który można wdrożyć do pliku. Na przykład aplikację, która wywołuje usługę WCF może być konieczne innym punktem końcowym w środowiskach testowych i produkcyjnych. Aplikacja Contoso University obejmuje ustawienia tego rodzaju także. To ustawienie steruje widoczny wskaźnik na stronach witryny, informujące o środowisko, które znajdują się w, takich jak rozwój, testowym czy produkcyjnym. Wartość ustawienia określa, czy aplikacja dołączy "(Dev)" lub "(Test)" do nagłówka w *Site.Master* strona główna:

[![Environment_indicator](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image9.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image8.png)

Wskaźnik środowiska jest pomijane, gdy aplikacja jest uruchomiona w środowisku produkcyjnym.

Na stronach sieci web firmy Contoso University odczytywanie wartości ustawionej w `appSettings` w *Web.config* pliku w celu ustalenia, jakiego środowiska, aplikacja jest uruchomiona w:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample3.xml)]

Wartość powinna być "Test" w środowisku testowym, a "Prod" w środowisku produkcyjnym.

Otwórz *Web.Production.config* i Dodaj `appSettings` element bezpośrednio przed znaczniku otwierającym elementu `location` elementu dodano wcześniej:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample4.xml)]

`xdt:Transform` "SetAttributes" wskazuje, że celem tej transformacji jest zmiana wartości atrybutów istniejącego elementu w wartość atrybutu *Web.config* pliku. `xdt:Locator` Atrybutu wartości "Match(key)" oznacza, że element, który ma zostać zmodyfikowana jest ten, którego `key` atrybut dopasowania `key` atrybutu wskazanego w tym miejscu. Tylko inne atrybut `add` element jest `value`, i to, co zostanie zmienione w wdrożonych *Web.config* pliku. Ten kod powoduje `value` atrybutu `Environment` `appSettings` element miała wartość "Produkcyjne" *Web.config* pliku, który jest wdrożony w środowisku produkcyjnym.

Następnie Zastosuj tę samą zmianę do *Web.Test.config* pliku, z wyjątkiem zestawu `value` na "Test" zamiast "Prod". Gdy wszystko będzie gotowe, `appSettings` sekcji *Web.Test.config* będzie wyglądać następująco:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample5.xml)]

## <a name="disabling-debug-mode"></a>Wyłączenie trybu debugowania

Dla wersji kompilacji ma włączonym debugowaniem niezależnie od tego, środowisko, które wdrażasz na. Domyślnie *Web.Release.config* pliku transformacji jest tworzone automatycznie za pomocą kodu, który usuwa `debug` atrybut z `compilation` elementu:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample6.xml)]

`Transform` Atrybutu powoduje, że `debug` atrybutu, aby pominąć od wdrożonych *Web.config* plików zawsze wtedy, gdy wdrażanie kompilacji wydania.

Tej samej transformacji jest w testowych oraz produkcyjnych na pliki transformacji utworzona przez skopiowanie wersji pliku przekształcenia. Nie są potrzebne istnieje zduplikowana każdego z tych plików otwórz tak, Usuń **kompilacji** elementu, Zapisz i zamknij każdy plik.

## <a name="setting-connection-strings"></a>Ustawianie parametrów połączenia

W większości przypadków nie trzeba skonfigurować przekształcenia parametrów połączenia, ponieważ parametry połączenia można określić w profilu publikowania. Jednak występuje wyjątek podczas wdrażania bazy danych programu SQL Server Compact i używasz migracje Code First Framework jednostki można zaktualizować bazy danych na serwerze docelowym. W tym scenariuszu należy określić parametry połączenia dodatkowe, które będą używane na serwerze do aktualizowania schematu bazy danych. Aby skonfigurować takie przekształcenie, Dodaj **&lt;connectionStrings&gt;** element natychmiast po otwarciu **&lt;konfiguracji&gt;** tagu w obu *Web.Test.config* i *Web.Production.config* plików transformacji:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample7.xml)]

`Transform` Atrybut określa, że te parametry połączenia zostaną dodane do *connectionStrings* element wdrożonych *Web.config* pliku. (Proces publikowania te dodatkowe parametry automatycznie tworzy za Ciebie, jeśli nie istnieje, ale domyślnie **providerName** pobiera ustawioną wartość atrybutu `System.Data.SqlClient`, które nie działa dla programu SQL Server Compact. Dodając parametry połączenia ręcznie, należy dysponować procesu wdrażania od tworzenia element ciągu połączenia z nazwą dostawcy problem.)

Teraz mają wszystkie określone *Web.config* przekształceń, które są potrzebne do wdrażania aplikacji Contoso University do testowania i produkcji. W tym samouczku poniższy zajmiesz zadań konfiguracji wdrożenia, które wymaga ustawienia właściwości projektu.

## <a name="more-information"></a>Więcej informacji

Aby uzyskać więcej informacji na temat tematy poruszane w ramach tego samouczka, zobacz scenariusz przekształcenia pliku Web.config w [Mapa zawartości platformy ASP.NET wdrożenia](https://msdn.microsoft.com/library/bb386521.aspx).

> [!div class="step-by-step"]
> [Poprzednie](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md)
> [dalej](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md)
