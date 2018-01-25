---
uid: web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations
title: "Wdrażanie sieci Web ASP.NET przy użyciu programu Visual Studio: przekształcenia pliku Web.config | Dokumentacja firmy Microsoft"
author: tdykstra
description: "Ta seria samouczek pokazuje, jak wdrożyć platformy ASP.NET (publikowanie) aplikacji do aplikacji sieci Web usługi aplikacji Azure lub innego dostawcy hostingu sieci web przez używane..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 5a2a927b-14cb-40bc-867a-f0680f9febd7
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations
msc.type: authoredcontent
ms.openlocfilehash: a526275d76618c325a6b00f33cc550f28ab0cc00
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-web-deployment-using-visual-studio-webconfig-file-transformations"></a>Wdrażanie sieci Web ASP.NET przy użyciu programu Visual Studio: przekształcenia pliku Web.config
====================
przez [Dykstra niestandardowy](https://github.com/tdykstra)

[Pobierz początkowego projektu](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Ta seria samouczek pokazuje, jak wdrożyć platformy ASP.NET (publikowanie) aplikacji do aplikacji sieci Web usługi aplikacji Azure lub innego dostawcy hostingu sieci web za pomocą programu Visual Studio 2012 lub Visual Studio 2010. Aby uzyskać informacje dotyczące serii, zobacz [pierwszy samouczek z tej serii](introduction.md).


## <a name="overview"></a>Omówienie

Ten samouczek pokazuje, jak można zautomatyzować proces zmiany *Web.config* plików podczas wdrażania środowisk różnych miejsc docelowych. Większość aplikacji ma ustawienia *Web.config* pliku, który musi być inna, gdy aplikacja jest wdrażana. Automatyzowanie procesu dokonywania zmian zapewnia trzeba wykonać je ręcznie, za każdym razem zostanie wdrożony, będą niewygodny i podatna.

Przypomnienie: Jeśli coś nie działa podczas wykonywania kroków samouczka wyświetlony komunikat o błędzie, należy sprawdzić [Rozwiązywanie problemów z strony](troubleshooting.md).

## <a name="webconfig-transformations-versus-web-deploy-parameters"></a>Przekształcenia pliku Web.config i parametrów narzędzia Web Deploy

Istnieją dwa sposoby automatyzacji procesu zmiany *Web.config* ustawienia plików: [przekształcenia pliku Web.config](https://msdn.microsoft.com/library/dd465326.aspx) i [parametrów narzędzia Web Deploy](https://msdn.microsoft.com/library/ff398068.aspx). A *Web.config* plik przekształcenia zawiera kod znaczników XML, który określa, jak zmienić *Web.config* pliku podczas jego wdrażania. Można określić różne zmiany określonych konfiguracje kompilacji i dla określonych profilów publikowania. Domyślne konfiguracje kompilacji są Debug i Release i można tworzyć konfiguracje kompilacji niestandardowej. Profil publikowania zazwyczaj odpowiada środowisku docelowym. (Dowiesz się więcej na temat profilów w publikowania [wdrażania usług IIS jako środowisko testowe](deploying-to-iis.md) samouczek.)

Parametry wdrażania w sieci Web można określić wiele różnych ustawień, które muszą być skonfigurowane podczas wdrażania ustawień, które znajdują się w tym *Web.config* plików. Gdy jest używany do określenia *Web.config* plik ulegnie zmianie, narzędzie Web Deploy parametry są bardziej złożone, aby skonfigurować, ale są przydatne, gdy nie znasz wartość do ustawienia, dopóki nie zostanie wdrożony. Na przykład w środowisku przedsiębiorstwa może utworzyć *pakietu wdrożeniowego* i nadaj mu do osoby w dziale IT, aby zainstalować w środowisku produkcyjnym, a ma mieć możliwość wprowadź parametry połączenia lub hasła, które nie są znać.

Scenariusz obejmuje tego samouczka serii, wiesz, z wyprzedzeniem wszystkie elementy, które ma wykonać, aby *Web.config* plików, dzięki czemu nie trzeba używać parametrów narzędzia Web Deploy. Należy skonfigurować niektórych transformacji, które różnią się w zależności od konfiguracji kompilacji używany, a niektóre, które różnią się w zależności od profil publikowania używany.

<a id="watransforms"></a>

## <a name="specifying-webconfig-settings-in-azure"></a>Określanie ustawień w pliku Web.config na platformie Azure

Jeśli *Web.config* znajdują się w pliku ustawień, które chcesz zmienić `<connectionStrings>` lub `<appSettings>` elementu i wdrażania aplikacji sieci Web w usłudze Azure App Service, ma inną opcją w przypadku zmiany podczas automatyzacji wdrożenia. Możesz wprowadzić ustawienia, które ma zostać zastosowana do platformy Azure w **Konfiguruj** kartę strony portalu zarządzania dla aplikacji sieci web (Przewiń w dół do **ustawień aplikacji** i **parametry połączenia**  sekcji). Podczas wdrażania projektu Azure automatycznie stosuje zmiany. Aby uzyskać więcej informacji, zobacz [Windows Azure Web Sites: jak ciągi aplikacji i pracy ciągów połączenia](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx).

## <a name="default-transformation-files"></a>Domyślne pliki transformacji

W **Eksploratora rozwiązań**, rozwiń węzeł *Web.config* wyświetlić *Web.Debug.config* i *Web.Release.config* pliki transformacji są domyślnie tworzone dla konfiguracji kompilacji dwóch domyślnych.

![Web.config_transform_files](web-config-transformations/_static/image1.png)

Może tworzyć pliki transformacji dla konfiguracji niestandardowej kompilacji, prawym przyciskiem myszy plik Web.config i wybierając pozycję **dodać przekształca Config** z menu kontekstowego. W tym samouczku nie trzeba to zrobić, a opcja menu jest wyłączona, ponieważ nie utworzono żadnych konfiguracji niestandardowej kompilacji.

Później zostaną utworzone trzy dodatkowe przekształcania pliki, jeden dla testu, przemieszczania i produkcji profilów publikowania. Typowym przykładem ustawienie, które może obsłużyć w pliku transformacji profil publikowania, ponieważ zależy od środowiska docelowego jest punktu końcowego WCF innego testu i produkcji. Utworzysz publikowania plików transformacji profilu w kolejnych samouczkach po utworzeniu profilów publikowania, które komputery przechodzą z.

## <a name="disable-debug-mode"></a>Wyłącz tryb debugowania

Przykład to ustawienie zależy od konfiguracji kompilacji, a nie jest środowisko docelowe `debug` atrybutu. Dla kompilacji wydania zazwyczaj mają debugowania wyłączone niezależnie od tego, które środowiska wdrażania na. W związku z tym domyślnie programu Visual Studio szablony projektu Utwórz *Web.Release.config* plików z kodem, które usuwa transformacji `debug` atrybutu z `compilation` elementu. W tym miejscu jest ustawieniem domyślnym *Web.Release.config*: oprócz niektórych przykładowy przekształcania kod, który jest oznaczone jako komentarz, zawiera kod w `compilation` element, który usuwa `debug` atrybutu:

[!code-xml[Main](web-config-transformations/samples/sample1.xml?highlight=18)]

`xdt:Transform="RemoveAttributes(debug)"` Atrybut określa, że `debug` atrybutu, aby były usuwane z `system.web/compilation` element wdrożone *Web.config* pliku. Spowoduje to zrobić za każdym razem, gdy wdrażanie kompilacji wydania.

## <a name="limit-error-log-access-to-administrators"></a>Ograniczenie dostępu do dziennika błędów administratorów

Jeśli występuje błąd, gdy jest uruchomiona aplikacja, aplikacja wyświetla stronę rodzajowy komunikat o błędzie zamiast strony błędu generowanych przez system i używa [pakietu Elmah NuGet](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) błędu rejestrowania i raportowania. `customErrors` Elementu w aplikacji *Web.config* plik Określa stronę błędu:

[!code-xml[Main](web-config-transformations/samples/sample2.xml)]

Aby wyświetlić stronę błędu, tymczasowo zmienić `mode` atrybutu `customErrors` elementu "RemoteOnly" do "Na" i uruchamianie aplikacji w programie Visual Studio. Przyczyną błędu przez żądanie nieprawidłowego adresu URL, taki jak *Studentsxxx.aspx*. Zamiast błędu generowanych przez usługi IIS "nie można odnaleźć zasobu" widoczna *GenericErrorPage.aspx* strony.

![Strona błędu](web-config-transformations/_static/image2.png)

Aby wyświetlić dziennik błędów, należy zastąpić wszystkie elementy w adresie URL po numer portu z *elmah.axd* (na przykład `http://localhost:51130/elmah.axd`) i naciśnij klawisz Enter:

![Strona ELMAH](web-config-transformations/_static/image3.png)

Nie zapomnij ustawić `customErrors` element do trybu "RemoteOnly", gdy wszystko będzie gotowe.

Na komputerze dewelopera jest wygodne umożliwia bezpłatny dostęp do strony dziennika błędów, ale w środowisku produkcyjnym, która mogłaby stanowić zagrożenie bezpieczeństwa. Dla lokacji produkcyjnej chcesz dodać regułę autoryzacji, który ogranicza dostęp do dziennika błędów dla administratorów i upewnij się, że ograniczenie działa, możesz będzie w testowych i przejściowych również. W związku z tym jest inna zmiana przewidzianą do wdrożenia za każdym razem, gdy wdrażanie kompilacji wydania, w związku z czym należy ono do *Web.Release.config* pliku.

Otwórz *Web.Release.config* i Dodaj nową `location` element bezpośrednio przed tagiem zamykającym `configuration` tagów, jak pokazano poniżej.

[!code-xml[Main](web-config-transformations/samples/sample3.xml?highlight=27-34)]

`Transform` Wartość atrybutu "INSERT" powoduje, że to `location` element do dodania jako element równorzędny do żadnych istniejących `location` elementów w *Web.config* pliku. (Istnieje już `location` element, który określa autoryzacji zasady **środków aktualizacji** strony.)

Można teraz wyświetlać podgląd transformacji do upewnij się, że kodowany on poprawnie.

W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *Web.Release.config* i kliknij przycisk **przekształcenie Podgląd**.

![Menu Przekształcanie wersji zapoznawczej](web-config-transformations/_static/image4.png)

Zostanie otwarta strona, które pokazuje rozwoju *Web.config* plik po lewej i co wdrożone *Web.config* pliku będzie wyglądać po prawej stronie, z wyróżnionym zmiany.

![Podgląd transformacji debugowania](web-config-transformations/_static/image5.png)

![Podgląd transformacji lokalizacji](web-config-transformations/_static/image6.png)

(W wersji zapoznawczej, można zauważyć pewne dodatkowe zmiany, które nie zostały pisać przekształca dla: te zwykle obejmują usunięcie biały znak, który nie ma wpływu na funkcjonalność.)

Podczas testowania lokacji po wdrożeniu, ponadto przetestujesz, aby sprawdzić obowiązuje reguły autoryzacji.

> [!NOTE] 
> 
> **Uwaga dotycząca zabezpieczeń** nigdy nie są wyświetlane szczegóły błędu publicznie w aplikacji produkcyjnej lub przechowywania tych informacji w ogólnodostępnej lokalizacji. Osoby atakujące umożliwiających wykrywanie luk w zabezpieczeniach w lokacji informacje o błędzie. Jeśli używasz ELMAH w swojej aplikacji, należy skonfigurować ELMAH, aby zminimalizować zagrożenia bezpieczeństwa. Przykład ELMAH w tym samouczku nie należy traktować jako zalecanej konfiguracji. Jest przykładem, który wybrano w celu zilustrowania sposobu obsługi, że aplikacja musi mieć możliwość tworzenia plików w folderze. Aby uzyskać więcej informacji, zobacz [zabezpieczanie punktu końcowego ELMAH](https://code.google.com/p/elmah/wiki/SecuringErrorLogPages).


## <a name="a-setting-that-youll-handle-in-publish-profile-transformation-files"></a>Pliki transformacji profilu publikowania ustawienie, który będzie obsługiwać w

Typowy scenariusz ma *Web.config* ustawienia, które muszą być różne w każdym środowisku wdrażanej w pliku. Na przykład aplikacja, która wywołuje usługę WCF może być konieczne innym punktem końcowym w środowiskach testowych i produkcyjnych. Aplikacja Contoso University obejmuje także ustawienie tego typu. To ustawienie określa wskaźnik widoczne na stronach witryny informujący środowiska, które znajdują się w, takich jak projektowanie, testów lub produkcji. Wartość ustawienia określa, czy aplikacja zostanie dołączona "(deweloperów)" lub "(Test)" do nagłówka w *Site.Master* strony głównej:

![Wskaźnik środowiska](web-config-transformations/_static/image7.png)

Wskaźnik środowiska jest pominięty, gdy aplikacja jest uruchomiona w tymczasowym czy produkcyjnym.

Strony sieci web firmy Contoso University odczytywanie wartości ustawionej w `appSettings` w *Web.config* pliku w celu określenia, jakie środowisko aplikacja działa w:

[!code-xml[Main](web-config-transformations/samples/sample4.xml)]

Wartość powinna być "Test" w środowisku testowym, a "Produkcyjnego" dla tymczasowych i produkcyjnych.

Następujący kod w pliku transformacji wdroży tej transformacji:

[!code-xml[Main](web-config-transformations/samples/sample5.xml)]

`xdt:Transform` "SetAttributes" oznacza, że celem tej transformacji jest można zmienić wartości atrybutów z istniejącego elementu na wartość atrybutu *Web.config* pliku. `xdt:Locator` "Match(key)" wskazuje, czy element ma być zmodyfikowana jest tą wartość atrybutu których `key` atrybutu dopasowań `key` atrybutu określone w tym miejscu. Tylko innych atrybut `add` jest element `value`, i jakie zostanie zmieniona w wdrożone *Web.config* pliku. Kod przyczyny pokazane `value` atrybutu `Environment` `appSettings` elementu na wartość "Test" *Web.config* plików, które zostało wdrożone.

Tej transformacji należy w plikach transformacji profil publikowania, które nie został jeszcze utworzony. Zostanie utworzona i zaktualizować pliki transformacji, które implementuje tę zmianę, podczas tworzenia profilów publikowania dla środowiska testowego, tymczasową i produkcyjną. Można to zrobić w [wdrażania usług IIS](deploying-to-iis.md) i [wdrożenia do produkcji](deploying-to-production.md) samouczki.

> [!NOTE]
> Ponieważ to ustawienie znajduje się w `<appSettings>` elementu, masz alternatywą dla określania transformacja podczas wdrażania aplikacji sieci Web w Azure App Service zobacz [określenie ustawienia pliku Web.config na platformie Azure](#watransforms) we wcześniejszej części w tym temacie.


## <a name="setting-connection-strings"></a>Ustawianie parametrów połączenia

Domyślny plik transformacji zawiera przykład pokazujący sposób zaktualizować parametry połączenia, w większości przypadków nie trzeba skonfigurować przekształcenia ciąg połączenia, ponieważ parametry połączenia można określić w profilu publikowania. Można to zrobić w [wdrażania usług IIS](deploying-to-iis.md) i [wdrożenia do produkcji](deploying-to-production.md) samouczki.

## <a name="summary"></a>Podsumowanie

Teraz wykonaniu jak w przypadku *Web.config* przekształcenia przed tworzenia profilów publikowania, a w tym samouczku podglądu jaki będzie w wdrożonym pliku Web.config.

![Podgląd transformacji lokalizacji](web-config-transformations/_static/image8.png)

W poniższych instrukcji zajmiesz zadań konfiguracji wdrożenia, które wymaga ustawienia właściwości projektu.

## <a name="more-information"></a>Więcej informacji

Aby uzyskać więcej informacji o tematach opisane w tym samouczku, zobacz [przekształcenia przy użyciu pliku Web.config, aby zmienić ustawienia w pliku Web.config docelowego lub pliku app.config podczas wdrażania](https://go.microsoft.com/fwlink/p/?LinkId=282413#transforms) w planie zawartości sieci Web wdrożenia dla Program Visual Studio i platformy ASP.NET.

>[!div class="step-by-step"]
[Poprzednie](preparing-databases.md)
[dalej](project-properties.md)
