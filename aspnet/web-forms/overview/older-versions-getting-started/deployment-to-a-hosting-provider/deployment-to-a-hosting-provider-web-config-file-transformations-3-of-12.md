---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12
title: "Wdrażanie aplikacji sieci Web ASP.NET przy użyciu programu Visual Studio lub Visual Web Developer programu SQL Server Compact: przekształcenia pliku Web.Config - 3 12 | Dokumentacja firmy Microsoft"
author: tdykstra
description: "Tej serii samouczków opisano sposób wdrażania platformy ASP.NET (publikowanie) projektu aplikacji sieci web, który zawiera bazę danych programu SQL Server Compact przy użyciu Visual Stu..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 2b0df3d9-450b-4ea6-b315-4c9650722cad
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12
msc.type: authoredcontent
ms.openlocfilehash: 8f68a85e44389ed17576436a9210c0ca3f414403
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-webconfig-file-transformations---3-of-12"></a>Wdrażanie aplikacji sieci Web ASP.NET przy użyciu programu Visual Studio lub Visual Web Developer programu SQL Server Compact: przekształcenia pliku Web.Config - 3 12
====================
przez [Dykstra niestandardowy](https://github.com/tdykstra)

[Pobierz początkowego projektu](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Tej serii samouczków opisano sposób wdrażania platformy ASP.NET (publikowanie) projektu aplikacji sieci web, który zawiera bazę danych programu SQL Server Compact przy użyciu programu Visual Studio 2012 RC i Visual Studio Express 2012 RC for Web. W razie musisz zainstalować aktualizację publikowania w sieci Web, można również używać programu Visual Studio 2010. Aby obejrzeć wprowadzenie do serii, zobacz [pierwszy samouczek z tej serii](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Samouczek zawiera funkcje wdrażania dodane po wydaniu programu Visual Studio 2012 RC, pokazuje, jak wdrożyć wersjach programu SQL Server innych niż SQL Server Compact, która przedstawia sposób wdrażania aplikacji sieci Web usługi aplikacji Azure, zobacz [wdrożenia sieci Web ASP.NET za pomocą programu Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Omówienie

Ten samouczek pokazuje, jak można zautomatyzować proces zmiany *Web.config* plików podczas wdrażania środowisk różnych miejsc docelowych. Większość aplikacji ma ustawienia *Web.config* pliku, który musi być inna, gdy aplikacja jest wdrażana. Automatyzowanie procesu dokonywania zmian zapewnia trzeba wykonać je ręcznie, za każdym razem zostanie wdrożony, będą niewygodny i podatna.

Przypomnienie: Jeśli coś nie działa podczas wykonywania kroków samouczka wyświetlony komunikat o błędzie, należy sprawdzić [Rozwiązywanie problemów z strony](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="webconfig-transformations-versus-web-deploy-parameters"></a>Parametry wdrożenia transformacji pliku Web.config i sieci Web

Istnieją dwa sposoby automatyzacji procesu zmiany *Web.config* ustawienia plików: [przekształcenia pliku Web.config](https://msdn.microsoft.com/en-us/library/dd465326.aspx) i [parametrów narzędzia Web Deploy](https://msdn.microsoft.com/en-us/library/ff398068.aspx). A *Web.config* plik przekształcenia zawiera kod znaczników XML, który określa, jak zmienić *Web.config* pliku podczas jego wdrażania. Można określić różne zmiany określonych konfiguracje kompilacji i dla określonych profilów publikowania. Domyślne konfiguracje kompilacji są Debug i Release i można tworzyć konfiguracje kompilacji niestandardowej. Profil publikowania zazwyczaj odpowiada środowisku docelowym. (Dowiesz się więcej na temat profilów w publikowania [wdrażania usług IIS jako środowisko testowe](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) samouczek.)

Parametry wdrażania w sieci Web można określić wiele różnych ustawień, które muszą być skonfigurowane podczas wdrażania ustawień, które znajdują się w tym *Web.config* plików. Gdy jest używany do określenia *Web.config* plik ulegnie zmianie, narzędzie Web Deploy parametry są bardziej złożone, aby skonfigurować, ale są przydatne, gdy nie znasz wartość do ustawienia, dopóki nie zostanie wdrożony. Na przykład w środowisku przedsiębiorstwa może utworzyć *pakietu wdrożeniowego* i nadaj mu do osoby w dziale IT, aby zainstalować w środowisku produkcyjnym, a ma mieć możliwość wprowadź parametry połączenia lub hasła, które nie są znać.

Ten samouczek obejmuje scenariusz wiesz, wszystkie elementy, które ma wykonać, aby *Web.config* plików, dzięki czemu nie trzeba używać parametrów narzędzia Web Deploy. Należy skonfigurować niektórych transformacji, które różnią się w zależności od konfiguracji kompilacji używany, a niektóre, które różnią się w zależności od profil publikowania używany.

## <a name="creating-transformation-files-for-publish-profiles"></a>Tworzenie plików transformacji dla profilów publikowania

W **Eksploratora rozwiązań**, rozwiń węzeł *Web.config* wyświetlić *Web.Debug.config* i *Web.Release.config* pliki transformacji są domyślnie tworzone dla konfiguracji kompilacji dwóch domyślnych.

![Web.config_transform_files](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image1.png)

Może tworzyć pliki transformacji dla konfiguracji niestandardowej kompilacji, prawym przyciskiem myszy plik Web.config i wybierając pozycję **dodać przekształca Config** z menu kontekstowego, ale w tym samouczku nie trzeba to zrobić.

Trzeba dwóch więcej plików transformacji konfigurowania zmiany, które odnoszą się do wdrożenia docelowego, a nie dla konfiguracji kompilacji. Typowym przykładem tego rodzaju ustawienie jest punktu końcowego WCF innego testu i produkcji. W kolejnych samouczkach utworzysz publikowania profile o nazwie Test i środowiska produkcyjnego, więc należy *Web.Test.config* pliku i *Web.Production.config* pliku.

Pliki transformacji, które są powiązane z profilów publikowania muszą być utworzone ręcznie. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt ContosoUniversity i wybierz **Otwórz Folder w Eksploratorze Windows**.

![Open_folder_in_Windows_Explorer](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image2.png)

W **Eksploratora Windows**, wybierz pozycję *Web.Release.config* pliku, skopiuj plik, a następnie wklej dwie kopie. Zmiana nazwy tych kopii *Web.Production.config* i *Web.Test.config*, następnie Zamknij **Eksploratora Windows**.

W **Eksploratora rozwiązań**, kliknij przycisk **Odśwież** aby zobaczyć nowe pliki.

Wybierz nowe pliki, kliknij prawym przyciskiem myszy, a następnie kliknij **Include w projekcie** w menu kontekstowym.

![W tym pliki konfiguracji testu i produkcji w projekcie](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image3.png)

Aby zapobiec wdrażany tych plików, wybierz je w **Eksploratora rozwiązań**, a następnie w **właściwości** okna zmiany **Akcja kompilacji** właściwość z **Zawartości** do **Brak**. (Pliki transformacji, które są oparte na konfiguracje kompilacji są automatycznie uniemożliwił wdrażania.)

Teraz można przystąpić do wprowadzania *Web.config* przekształceń do *Web.config* pliki transformacji.

## <a name="limiting-error-log-access-to-administrators"></a>Ograniczanie dostępu do dziennika błędów dla administratorów

Jeśli występuje błąd, gdy jest uruchomiona aplikacja, aplikacja wyświetla stronę rodzajowy komunikat o błędzie zamiast strony błędu generowanych przez system i używa [pakietu Elmah NuGet](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) błędu rejestrowania i raportowania. `customErrors` Element *Web.config* plik Określa stronę błędu:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample1.xml)]

Aby wyświetlić stronę błędu, tymczasowo zmienić `mode` atrybutu `customErrors` elementu "RemoteOnly" do "Na" i uruchamianie aplikacji w programie Visual Studio. Przyczyną błędu przez żądanie nieprawidłowego adresu URL, taki jak *Studentsxxx.aspx*. Zamiast generowanych przez usługi IIS "strony nie można odnaleźć" stronę błędu, zobacz *GenericErrorPage.aspx* strony.

[![Error_page](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image5.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image4.png)

Aby wyświetlić dziennik błędów, należy zastąpić wszystkie elementy w adresie URL po numer portu z *elmah.axd* (na przykład w zrzut ekranu przedstawiający `http://localhost:51130/elmah.axd`) i naciśnij klawisz Enter:

[![Elmah_log_page](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image7.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image6.png)

Nie zapomnij ustawić `customErrors` element do trybu "RemoteOnly", gdy wszystko będzie gotowe.

Na komputerze dewelopera jest wygodne umożliwia bezpłatny dostęp do strony dziennika błędów, ale w środowisku produkcyjnym, która mogłaby stanowić zagrożenie bezpieczeństwa. Dla lokacji produkcyjnej, można dodać reguły autoryzacji, która ogranicza dostęp do dziennika błędów tylko administratorom konfigurując przekształcenie w *Web.Production.config* pliku.

Otwórz *Web.Production.config* i Dodaj nową `location` element natychmiast po otwarciu `configuration` tagów, jak pokazano poniżej. (Upewnij się, że możesz dodać tylko `location` elementu i nie otaczającego znaczników, które tym polu jest wyświetlana tylko w celu zapewnienia część kontekstu.)

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample2.xml?highlight=2-9)]

`Transform` Wartość atrybutu "INSERT" powoduje, że to `location` element do dodania jako element równorzędny do żadnych istniejących `location` elementów w *Web.config* pliku. (Istnieje już `location` element, który określa autoryzacji zasady **środków aktualizacji** strony.) Podczas testowania lokacji produkcyjnej po wdrożeniu, przetestujesz, aby sprawdzić, czy ta reguła autoryzacji jest skuteczne.

Nie masz ograniczyć dostęp do dziennika błędów w środowisku testowym, więc nie trzeba dodać ten kod, aby *Web.Test.config* pliku.

> [!NOTE] 
> 
> **Uwaga dotycząca zabezpieczeń** nigdy nie są wyświetlane szczegóły błędu publicznie w aplikacji produkcyjnej lub przechowywania tych informacji w ogólnodostępnej lokalizacji. Osoby atakujące umożliwiających wykrywanie luk w zabezpieczeniach w lokacji informacje o błędzie. Jeśli używasz ELMAH w swojej aplikacji, należy zbadać sposoby, w którym można skonfigurować ELMAH w celu minimalizacji zagrożenia bezpieczeństwa. Przykład ELMAH w tym samouczku nie należy traktować jako zalecanej konfiguracji. Jest przykładem, który wybrano w celu zilustrowania sposobu obsługi, że aplikacja musi mieć możliwość tworzenia plików w folderze.


## <a name="setting-an-environment-indicator"></a>Ustawienie wskaźnika środowiska

Typowy scenariusz ma *Web.config* ustawienia, które muszą być różne w każdym środowisku wdrażanej w pliku. Na przykład aplikacja, która wywołuje usługę WCF może być konieczne innym punktem końcowym w środowiskach testowych i produkcyjnych. Aplikacja Contoso University obejmuje także ustawienie tego typu. To ustawienie określa wskaźnik widoczne na stronach witryny informujący środowiska, które znajdują się w, takich jak projektowanie, testów lub produkcji. Wartość ustawienia określa, czy aplikacja zostanie dołączona "(deweloperów)" lub "(Test)" do nagłówka w *Site.Master* strony głównej:

[![Environment_indicator](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image9.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image8.png)

Wskaźnik środowiska jest pominięty, gdy aplikacja działa w środowisku produkcyjnym.

Strony sieci web firmy Contoso University odczytywanie wartości ustawionej w `appSettings` w *Web.config* pliku w celu określenia, jakie środowisko aplikacja działa w:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample3.xml)]

Wartość powinna być "Test" w środowisku testowym, a "Produkcyjną" w środowisku produkcyjnym.

Otwórz *Web.Production.config* i Dodaj `appSettings` element bezpośrednio przed tagu otwierającego `location` element dodano wcześniej:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample4.xml)]

`xdt:Transform` "SetAttributes" oznacza, że celem tej transformacji jest można zmienić wartości atrybutów z istniejącego elementu na wartość atrybutu *Web.config* pliku. `xdt:Locator` "Match(key)" wskazuje, czy element ma być zmodyfikowana jest tą wartość atrybutu których `key` atrybutu dopasowań `key` atrybutu określone w tym miejscu. Tylko innych atrybut `add` jest element `value`, i jakie zostanie zmieniona w wdrożone *Web.config* pliku. Powoduje, że ten kod `value` atrybutu `Environment` `appSettings` elementu, aby ustawić ich stan jako "Produkcyjną" *Web.config* pliku, który jest wdrożony w środowisku produkcyjnym.

Następnie Zastosuj zmiany tej samej *Web.Test.config* pliku, z wyjątkiem zestawu `value` na "Test" zamiast "Produkcyjną". Gdy wszystko będzie gotowe, `appSettings` sekcji *Web.Test.config* będzie wyglądać następująco:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample5.xml)]

## <a name="disabling-debug-mode"></a>Wyłączenie trybu debugowania

Dla kompilacji wydania nie ma włączonym debugowaniem niezależnie od tego, które środowiska wdrażania na. Domyślnie *Web.Release.config* z kodem, które usuwa jest automatycznie tworzony plik transformacji `debug` atrybutu z `compilation` elementu:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample6.xml)]

`Transform` Atrybutu przyczyny `debug` atrybut można pominąć z wdrożone *Web.config* pliku przy każdym wdrożeniu kompilacji wydania.

Tego samego przekształcenia jest w testowych i produkcyjnych pliki transformacji ponieważ utworzony przez skopiowanie pliku transformacji wersji. Nie trzeba go istnieje zduplikowana, każdego z tych plików otwórz tak, Usuń **kompilacji** element i Zapisz i zamknij każdego pliku.

## <a name="setting-connection-strings"></a>Ustawianie parametrów połączenia

W większości przypadków nie trzeba skonfigurować przekształcenia ciąg połączenia, ponieważ parametry połączenia można określić w profilu publikowania. Brak Wystąpił wyjątek podczas wdrażania bazy danych programu SQL Server Compact, a do aktualizacji bazy danych na serwerze docelowym używasz migracje Code First Framework jednostki. W tym scenariuszu należy określić dodatkowe parametry, który będzie używany na serwerze aktualizacji schematu bazy danych. Aby skonfigurować ten przekształcania, Dodaj  **&lt;connectionStrings&gt;**  element natychmiast po otwarciu  **&lt;konfiguracji&gt;**  tag zarówno *Web.Test.config* i *Web.Production.config* pliki transformacji:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample7.xml)]

`Transform` Atrybut określa, że te parametry połączenia zostaną dodane do *connectionStrings* element wdrożone *Web.config* pliku. (Proces publikowania tworzy automatycznie te dodatkowe parametry, jeśli nie istnieje, ale domyślnie **providerName** atrybut zostanie ustawiony na `System.Data.SqlClient`, które nie działa nie dla programu SQL Server Compact. Dodając parametry połączenia ręcznie, należy dysponować procesu wdrażania tworzenia nazwy dostawcy niewłaściwego elementu ciągu połączenia.)

Teraz określono wszystkie *Web.config* transformacje, których potrzebujesz do wdrażania aplikacji Contoso University do testowania i produkcji. W poniższych instrukcji zajmiesz zadań konfiguracji wdrożenia, które wymaga ustawienia właściwości projektu.

## <a name="more-information"></a>Więcej informacji

Aby uzyskać więcej informacji o tematach opisane w tym samouczku, zobacz scenariusz transformacji pliku Web.config w [Mapa zawartości platformy ASP.NET wdrożenia](https://msdn.microsoft.com/en-us/library/bb386521.aspx).

>[!div class="step-by-step"]
[Poprzednie](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md)
[dalej](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md)
