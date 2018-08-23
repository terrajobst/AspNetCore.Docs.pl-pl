---
uid: web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations
title: 'Wdrażanie aplikacji internetowych ASP.NET przy użyciu programu Visual Studio: przekształcenia pliku Web.config | Dokumentacja firmy Microsoft'
author: tdykstra
description: W tej serii samouczków dowiesz się, jak wdrożyć (opublikować) platformy ASP.NET sieci web aplikacji do usługi Azure App Service Web Apps lub dostawcy hostingu w innych firm, używane...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: 5a2a927b-14cb-40bc-867a-f0680f9febd7
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations
msc.type: authoredcontent
ms.openlocfilehash: 58f3daac5e9fbe6d327f19b6ae78ea17a26cd7e7
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753610"
---
<a name="aspnet-web-deployment-using-visual-studio-webconfig-file-transformations"></a>Wdrażanie aplikacji internetowych ASP.NET przy użyciu programu Visual Studio: przekształcenia pliku Web.config
====================
przez [Tom Dykstra](https://github.com/tdykstra)

[Pobieranie projektu startowego](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> W tej serii samouczków dowiesz się, jak wdrożyć (opublikować) platformy ASP.NET sieci web aplikacji do usługi Azure App Service Web Apps lub innych firm dostawcy hostingu za pomocą programu Visual Studio 2012 lub Visual Studio 2010. Aby uzyskać informacje na temat serii, zobacz [pierwszym samouczku tej serii](introduction.md).


## <a name="overview"></a>Omówienie

W tym samouczku dowiesz się, jak zautomatyzować proces zmiany *Web.config* plików podczas wdrażania w środowiskach różnych miejsc docelowych. Większość aplikacji ma ustawienia *Web.config* pliku, który musi być inna, gdy aplikacja jest wdrażana. Automatyzowanie procesu wprowadzania tych zmian wymaga znajomości zrobić je ręcznie, za każdym razem, gdy zostanie wdrożony, może być uciążliwe i podatne.

Przypomnienie: Jeśli otrzymujesz komunikat o błędzie lub coś nie działa podczas wykonywania kroków samouczka, upewnij się sprawdzić [strona rozwiązywania problemów](troubleshooting.md).

## <a name="webconfig-transformations-versus-web-deploy-parameters"></a>Transformacje pliku Web.config i parametrów narzędzia Web Deploy

Istnieją dwa sposoby, aby zautomatyzować proces zmiany *Web.config* ustawienia plików: [transformacje pliku Web.config](https://msdn.microsoft.com/library/dd465326.aspx) i [parametrów narzędzia Web Deploy](https://msdn.microsoft.com/library/ff398068.aspx). A *Web.config* plik przekształcenia zawiera znacznik XML, który określa sposób zmiany *Web.config* pliku po jego wdrożeniu. Można określić różne zmiany dotyczące określonych konfiguracje kompilacji i dla określonych profilów publikowania. Domyślne konfiguracje kompilacji są Debug i Release, i można utworzyć konfiguracji niestandardowej kompilacji. Profil publikowania zazwyczaj odpowiada środowisku docelowym. (Dowiesz się więcej na temat profilów w publikowania [wdrażanie w usługach IIS jako środowisku testowym](deploying-to-iis.md) samouczek.)

Parametry wdrażania sieci Web może służyć do określania wielu różnych rodzajów ustawień, które muszą być skonfigurowane podczas wdrażania, w tym ustawienia, które znajdują się w *Web.config* plików. Gdy jest używana do określenia *Web.config* zmiany plików są bardziej złożone, aby skonfigurować parametry narzędzie Web Deploy, ale są przydatne, gdy nie wiadomo, wartość można ustawić, dopóki nie zostanie wdrożony. Na przykład w środowisku przedsiębiorstwa może utworzyć *pakietu wdrożeniowego* oferowanie osoby z działu IT do zainstalowania w środowisku produkcyjnym i dana osoba można było wprowadzić parametry połączenia lub hasła, które nie obsługują znać.

Dla tego scenariusza, który opisano w tej serii samouczków, możesz wiedzieć z wyprzedzeniem wszystko, co ma wykonać, aby *Web.config* pliku, więc nie trzeba używać parametrów narzędzia Web Deploy. Należy skonfigurować niektórych przekształceń, które różnią się w zależności od konfiguracji kompilacji używany, a niektóre, które różnią się w zależności od profil publikacji używany.

<a id="watransforms"></a>

## <a name="specifying-webconfig-settings-in-azure"></a>Określanie ustawień pliku Web.config na platformie Azure

Jeśli *Web.config* ustawienia plików, które chcesz zmienić znajdują się w `<connectionStrings>` lub `<appSettings>` elementu i wdrażania aplikacji sieci Web w usłudze Azure App Service ma inną opcją do automatyzowania zmiany podczas wdrożenie. Możesz wprowadzić ustawienia, które mają zostać zastosowane na platformie Azure w **Konfiguruj** karty na stronie portalu zarządzania dla aplikacji sieci web (Przewiń w dół do **ustawienia aplikacji** i **parametry połączenia**  sekcji). Podczas wdrażania projektu Azure automatycznie stosuje te zmiany. Aby uzyskać więcej informacji, zobacz [Windows Azure Web Sites: How Application Strings and Connection Strings Work](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx).

## <a name="default-transformation-files"></a>Domyślne pliki transformacji

W **Eksploratora rozwiązań**, rozwiń węzeł *Web.config* się *Web.Debug.config* i *Web.Release.config* pliki transformacji Domyślnie tworzone są dwa domyślne konfiguracje kompilacji.

![Web.config_transform_files](web-config-transformations/_static/image1.png)

Można tworzyć pliki transformacji dla konfiguracji niestandardowej kompilacji, kliknij prawym przyciskiem myszy plik Web.config i wybierając pozycję **Dodaj transformacje konfiguracji** z menu kontekstowego. Na potrzeby tego samouczka nie trzeba to zrobić, a opcja menu jest wyłączona, ponieważ nie utworzono żadnych konfiguracji niestandardowej kompilacji.

Później utworzysz trzy więcej pliki transformacji, jeden dla testów, przemieszczania i produkcji profilów publikowania. Typowym przykładem ustawienie, który będzie obsługiwać w pliku przekształcenia profilu publikowania, ponieważ zależy od środowiska docelowego jest punktu końcowego WCF, która różni się do testowania i produkcji. Utworzysz Publikuj pliki transformacji profilu w kolejnych samouczkach po utworzeniu profilów publikowania, które są zawsze pod ręką.

## <a name="disable-debug-mode"></a>Wyłącz tryb debugowania

Przykładem ustawienie zależy od konfiguracji kompilacji, a nie jest środowisko docelowe `debug` atrybutu. Dla kompilacji oficjalnej zazwyczaj chcesz wyłączonym debugowaniem niezależnie od tego, środowisko, które wdrażasz na. Dlatego domyślnie Visual Studio szablonów projektu utworzyć *Web.Release.config* plików z kodem, który usuwa transformacji `debug` atrybut z `compilation` elementu. W tym miejscu jest ustawieniem domyślnym *Web.Release.config*: oprócz przykładowy kod transformacji, który został umieszczony w komentarzach, obejmuje kod w `compilation` element, który usuwa `debug` atrybutu:

[!code-xml[Main](web-config-transformations/samples/sample1.xml?highlight=18)]

`xdt:Transform="RemoveAttributes(debug)"` Atrybut określa, że `debug` atrybutu, aby były usuwane z `system.web/compilation` element wdrożonych *Web.config* pliku. Ta czynność zostanie wykonana za każdym razem, gdy wdrażanie kompilacji wydania.

## <a name="limit-error-log-access-to-administrators"></a>Ogranicz dostęp do dziennika błędów dla administratorów

Jeśli wystąpi błąd, gdy aplikacja zostanie uruchomiona, aplikacja wyświetli stronę ogólny błąd zamiast strony błędów generowanych przez system i używa [pakiet NuGet biblioteki Elmah](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) do rejestrowania i raportowania błędów. `customErrors` Elementu w aplikacji *Web.config* plik Określa stronę błędu:

[!code-xml[Main](web-config-transformations/samples/sample2.xml)]

Aby wyświetlić stronę błędu, tymczasowo zmienić `mode` atrybutu `customErrors` elementu "RemoteOnly" na wartość "Włączone" i uruchamianie aplikacji w programie Visual Studio. Powodują wystąpienie błędu, wysyłając żądanie nieprawidłowego adresu URL, takich jak *Studentsxxx.aspx*. Zamiast błędu generowanych przez usługi IIS "nie można odnaleźć zasobu" stronie zostanie wyświetlony *GenericErrorPage.aspx* strony.

![Strona błędu](web-config-transformations/_static/image2.png)

Aby wyświetlić dziennik błędów, należy zastąpić wszystko, co w adresie URL po numer portu z *elmah.axd* (na przykład `http://localhost:51130/elmah.axd`) i naciśnij klawisz Enter:

![Strona ELMAH](web-config-transformations/_static/image3.png)

Nie należy zapominać ustawić `customErrors` element do trybu "RemoteOnly", gdy wszystko będzie gotowe.

Na komputerze deweloperskim jest wygodne zezwolić na bezpłatny dostęp do strony dziennika błędów, ale w środowisku produkcyjnym, który będzie stanowić zagrożenie bezpieczeństwa. Dla witryny produkcyjnej chcesz dodać regułę autoryzacji, który ogranicza dostęp do dziennika błędów dla administratorów i upewnij się, że działa ograniczenie, można będzie w testowych i przejściowych również. W związku z tym jest inna zmiana, którą chcesz wdrożyć za każdym razem, gdy wdrażanie kompilacji wydania, a więc należy ono do *Web.Release.config* pliku.

Otwórz *Web.Release.config* i Dodaj nowy `location` element bezpośrednio przed tagiem zamykającym `configuration` tag, jak pokazano poniżej.

[!code-xml[Main](web-config-transformations/samples/sample3.xml?highlight=27-34)]

`Transform` Wartość atrybutu "INSERT" powoduje, że to `location` element ma zostać dodany jako element równorzędny do wszystkich istniejących `location` elementów w *Web.config* pliku. (Istnieje już `location` element, który określa autoryzacji reguł dla **aktualizacji środki na korzystanie z** strony.)

Można teraz wyświetlać podgląd przekształcenia się upewnić, że kodowany je poprawnie.

W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *Web.Release.config* i kliknij przycisk **Przekształcanie wersji zapoznawczej**.

![Menu przekształcenia (wersja zapoznawcza)](web-config-transformations/_static/image4.png)

Zostanie otwarta strona, pokazujące rozwoju *Web.config* pliku po lewej, a co wdrożonych *Web.config* plik będzie wyglądać po prawej stronie, z wyróżnioną pozycją zmiany.

![Podgląd przekształcenia debugowania](web-config-transformations/_static/image5.png)

![Podgląd przekształcenia lokalizacji](web-config-transformations/_static/image6.png)

(W wersji zapoznawczej, można zauważyć pewne dodatkowe zmiany, które nie zostały zapisu przekształca dla: te obejmują na ogół usunięcie biały znak, który nie ma wpływu na funkcjonalność.)

Kiedy testujesz lokacji po wdrożeniu Ponadto przetestujesz, aby sprawdzić, czy obowiązujące reguły autoryzacji.

> [!NOTE] 
> 
> **Uwaga dotycząca zabezpieczeń** nigdy nie są wyświetlane szczegóły błędu publicznie w aplikacji produkcyjnej lub przechowywania tych informacji w ogólnodostępnej lokalizacji. Osoby atakujące umożliwia wykrywanie luk w zabezpieczeniach w lokacji informacje o błędzie. Jeśli używasz biblioteki ELMAH we własnej aplikacji, należy skonfigurować ELMAH, aby zminimalizować zagrożenia bezpieczeństwa. W przykładzie ELMAH, w tym samouczku nie powinny być uwzględniane zalecanej konfiguracji. To przykład, który został wybrany w celu zilustrowania sposobu obsługi aplikacji musi umożliwiać do tworzenia plików w folderze. Aby uzyskać więcej informacji, zobacz [zabezpieczenia punktu końcowego ELMAH](https://code.google.com/p/elmah/wiki/SecuringErrorLogPages).


## <a name="a-setting-that-youll-handle-in-publish-profile-transformation-files"></a>To ustawienie, które zostanie omówione w publikowanie pliki transformacji profilu

Jest to typowy scenariusz *Web.config* ustawienia, które muszą być różne w każdym środowisku, który można wdrożyć do pliku. Na przykład aplikację, która wywołuje usługę WCF może być konieczne innym punktem końcowym w środowiskach testowych i produkcyjnych. Aplikacja Contoso University obejmuje ustawienia tego rodzaju także. To ustawienie steruje widoczny wskaźnik na stronach witryny, informujące o środowisko, które znajdują się w, takich jak rozwój, testowym czy produkcyjnym. Wartość ustawienia określa, czy aplikacja dołączy "(Dev)" lub "(Test)" do nagłówka w *Site.Master* strona główna:

![Wskaźnik na środowisko](web-config-transformations/_static/image7.png)

Wskaźnik środowiska jest pomijane, gdy aplikacja jest uruchomiona w tymczasowym lub produkcyjnym.

Na stronach sieci web firmy Contoso University odczytywanie wartości ustawionej w `appSettings` w *Web.config* pliku w celu ustalenia, jakiego środowiska, aplikacja jest uruchomiona w:

[!code-xml[Main](web-config-transformations/samples/sample4.xml)]

Wartość powinna być "Test" w środowisku testowym i "Prod" dla środowisk przejściowych i produkcyjnych.

Następujący kod w pliku przekształcenia wdroży tej transformacji:

[!code-xml[Main](web-config-transformations/samples/sample5.xml)]

`xdt:Transform` "SetAttributes" wskazuje, że celem tej transformacji jest zmiana wartości atrybutów istniejącego elementu w wartość atrybutu *Web.config* pliku. `xdt:Locator` Atrybutu wartości "Match(key)" oznacza, że element, który ma zostać zmodyfikowana jest ten, którego `key` atrybut dopasowania `key` atrybutu wskazanego w tym miejscu. Tylko inne atrybut `add` element jest `value`, i to, co zostanie zmienione w wdrożonych *Web.config* pliku. Kod przedstawiony tutaj powoduje, że `value` atrybutu `Environment` `appSettings` element, należy ustawić na "Test" *Web.config* pliku, który jest wdrożony.

Ta transformacja jest powiązana pliki transformacji profilu publikowania, które nie został jeszcze utworzony. Można tworzyć i zaktualizuj pliki transformacji, które implementują tę zmianę, podczas tworzenia profilów publikowania dla środowisk testowych, przejściowe i produkcyjne. Możesz to zrobić w [wdrażanie w usługach IIS](deploying-to-iis.md) i [wdrażania w środowisku produkcyjnym](deploying-to-production.md) samouczków.

> [!NOTE]
> Ponieważ to ustawienie znajduje się w `<appSettings>` elementu, masz inną alternatywą do określania przekształcenie, podczas wdrażania Web Apps usługi Azure App Service — zobacz [określenie ustawienia pliku Web.config na platformie Azure](#watransforms) we wcześniejszej części w tym temacie.


## <a name="setting-connection-strings"></a>Ustawianie parametrów połączenia

Mimo że domyślny plik transformacji zawiera przykład, który pokazuje, jak zaktualizować parametry połączenia, w większości przypadków nie trzeba skonfigurować przekształcenia parametrów połączenia, ponieważ parametry połączenia można określić w profilu publikowania. Możesz to zrobić w [wdrażanie w usługach IIS](deploying-to-iis.md) i [wdrażania w środowisku produkcyjnym](deploying-to-production.md) samouczków.

## <a name="summary"></a>Podsumowanie

Teraz zrobisz podobnie jak za pomocą *Web.config* przekształcenia przed tworzenia profilów publikowania, a w tym samouczku (wersja zapoznawcza), z czym będą znajdować się w wdrożonym pliku Web.config.

![Podgląd przekształcenia lokalizacji](web-config-transformations/_static/image8.png)

W tym samouczku poniższy zajmiesz zadań konfiguracji wdrożenia, które wymaga ustawienia właściwości projektu.

## <a name="more-information"></a>Więcej informacji

Aby uzyskać więcej informacji na temat tematy poruszane w ramach tego samouczka, zobacz [przekształcenia przy użyciu pliku Web.config, aby zmienić ustawienia w pliku app.config lub pliku Web.config docelowego podczas wdrażania](https://go.microsoft.com/fwlink/p/?LinkId=282413#transforms) w planie zawartości sieci Web wdrożenia dla Visual Studio i platformy ASP.NET.

> [!div class="step-by-step"]
> [Poprzednie](preparing-databases.md)
> [dalej](project-properties.md)
