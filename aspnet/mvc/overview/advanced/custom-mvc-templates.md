---
uid: mvc/overview/advanced/custom-mvc-templates
title: Niestandardowy szablon MVC | Dokumentacja firmy Microsoft
author: joeloff
description: Utwórz szablon jako rozszerzenia VSIX.
ms.author: aspnetcontent
ms.date: 12/10/2012
ms.assetid: b0a214c7-2f38-4dbc-b47f-bd7bd9df97bd
msc.legacyurl: /mvc/overview/advanced/custom-mvc-templates
msc.type: authoredcontent
ms.openlocfilehash: 67cb01e0fae036f01b1851695ae5a4358136d28e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37809780"
---
<a name="custom-mvc-template"></a>Niestandardowy szablon MVC
====================
przez [Eloff wybieramy](https://github.com/joeloff)

Wersja MVC 3 Tools Update dla programu Visual Studio 2010 wprowadzono kreatora oddzielnego projektu dla projektów MVC. Zmiana została napędzana przez dwa czynniki. Po pierwsze wprowadzenia nowych szablonów w MVC 3 i pomoc techniczna dla aparatów widoków dodatkowe, takie jak Razor spowodować overcrowding oknie dialogowym Nowy projekt w programie Visual Studio. Po drugie klienci miał o punkty rozszerzeń i Kreator nowego projektu MVC czy umożliwić nam odpowiedzieć na te żądania.

Dodawanie niestandardowych szablonów jest uciążliwy procesu, który opierał się na korzystanie z rejestru, aby uwidocznić nowych szablonów w Kreatorze projektu MVC. Tworzenie nowego szablonu musiał zawijany MSI, aby upewnić się, że niezbędne wpisy rejestru zostałyby utworzone w czasie instalacji. Alternatywnie był na plik ZIP zawierający szablon dostępne i mają ręcznie utworzyć wpisy rejestru wymaganych przez użytkownika końcowego.

Żadna z wyżej wymienionych metod jest idealnym rozwiązaniem, dlatego zdecydowaliśmy się korzystać z niektórych z istniejącej infrastruktury zapewnianej przez [VSIX](https://msdn.microsoft.com/library/ff363239.aspx) rozszerzenia, aby ułatwić Autor, rozpowszechniać i instalować szablonów niestandardowych MVC, począwszy od platformy MVC 4 dla programu Visual Studio 2012. Niektóre z zalet tego podejścia to:

- Rozszerzenie VSIX może zawierać wiele szablonów, które obsługują różne języki (C# i Visual Basic) i wielu aparatów widoku (ASPX i Razor).
- Rozszerzenie VSIX można wskazać wiele jednostek SKU programu Visual Studio Express SKU w tym.
- [Galerii Visual Studio](https://visualstudiogallery.msdn.microsoft.com/) ułatwia dystrybucję rozszerzenie do szerszej publiczności.
- Ułatwia tworzenie poprawek i aktualizacji do szablonów niestandardowych można uaktualnić rozszerzenia VSIX.

## <a name="prerequisites"></a>Wymagania wstępne

- Użytkownicy muszą znać Tworzenie szablonów projektu, w tym wymagane znaczniki dla plików vstemplate itp.
- Użytkownicy będą musiały korzystać z programu Visual Studio Professional i nowszym. Wersje produktu Express nie obsługują tworzenia projektów VSIX.
- [Visual Studio 2012 SDK](https://www.microsoft.com/download/details.aspx?id=30668) zainstalowane.

## <a name="example"></a>Przykład

Pierwszym krokiem jest utworzenie nowego projektu VSIX przy użyciu języka C# lub Visual Basic. Wybierz **Plik > Nowy projekt**, następnie kliknij przycisk **rozszerzalności** w okienku po lewej stronie i wybierz pozycję **projekt VSIX**.

![Nowy projekt](custom-mvc-templates/_static/image1.jpg)

Po utworzeniu projektu zostanie otwarty projektant VSIX.

![Metadane projektanta projektu](custom-mvc-templates/_static/image2.jpg)

Projektant może służyć do edycji niektórych właściwości ogólnych rozszerzenia, które będą wyświetlane użytkownikom podczas instalowania rozszerzenia, lub Przeglądaj zainstalowanych rozszerzeń w programie Visual Studio (**Narzędzia > rozszerzenia i aktualizacje**). Po zakończeniu informacje ogólne kliknij **kartę Instaluj obiekty docelowe**.

![Elementy docelowe instalacji projektanta projektu](custom-mvc-templates/_static/image3.jpg)

Ta karta służy do określenia jednostki SKU i wersji programu Visual Studio, które są obsługiwane przez Twoje rozszerzenie. Zaznacz pole wyboru **ten VSIX są zainstalowane dla wszystkich użytkowników** umożliwiające instaluje na komputerze VSIX. Kliknij pozycję **New** przycisk po prawej stronie, aby dodać dodatkowe jednostki SKU takich jak Web Developer Express (wersji VWD).

![Dodaj nowe miejsce docelowe instalacji](custom-mvc-templates/_static/image4.jpg)

Jeśli zamierzasz obsługiwać wszystkie Professional lub nowszy jednostek SKU (Professional, Premium i Ultimate) należy wybrać minimalną jednostki SKU z rodziny **Microsoft.VisualStudio.Pro**. Pamiętaj, aby zapisać wszystkie zmiany, po zakończeniu Instaluj obiekty docelowe.

![Elementy docelowe instalacji projektanta projektu](custom-mvc-templates/_static/image5.jpg)

**Zasoby** karta jest używana, aby dodać wszystkie pliki zawartości do pliku VSIX. Ponieważ platforma MVC wymaga niestandardowych metadanych można będzie modyfikowana nieprzetworzonym kodzie XML w pliku manifestu VSIX, zamiast **zasoby** kartę, aby dodać zawartość. Rozpocznij, dodając zawartość szablonu do projektu VSIX. Należy pamiętać, że strukturę folderu i zawartość duplikatów układ projektu. Poniższy przykład zawiera cztery szablony projektów, utworzone na podstawie szablonu projektu MVC podstawowe. Upewnij się, że wszystkie pliki wchodzące w skład szablonu projektu (wszystko pod folderem ProjectTemplates) są dodawane do **zawartości** itemgroup w pliku VSIX project, plik i że każdy element zawiera  **CopyToOutputDirectory** i **IncludeInVsix** zestawu metadanych, jak pokazano w poniższym przykładzie.

&lt;Content Include=&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicWeb.config&quot;&gt;

&lt;CopyToOutputDirectory&gt;zawsze&lt;/CopyToOutputDirectory&gt;

&lt;IncludeInVSIX&gt;true&lt;/IncludeInVSIX&gt;

&lt;/Content&gt;

W przeciwnym razie IDE podejmie próbę skompilować treści szablonu podczas tworzenia pliku VSIX i prawdopodobnie zostanie wyświetlony błąd. Pliki kodu w szablonach często zawierają specjalne [parametry szablonu](https://msdn.microsoft.com/library/eehb4faa(v=vs.110).aspx) używany przez Visual Studio, gdy szablon projektu zostanie uruchomiony i nie można skompilować w środowisku IDE.

![Eksplorator rozwiązań](custom-mvc-templates/_static/image6.jpg)

Zamknij projektanta VSIX, a następnie kliknij prawym przyciskiem myszy **source.extension.manifest** w pliku **Eksploratora rozwiązań** i wybierz **Otwórz za pomocą** i wybierz polecenie **XML ( Edytor tekstu)** opcji.

![Otwórz za pomocą okna dialogowego](custom-mvc-templates/_static/image7.jpg)

Tworzenie **&lt;zasoby&gt;** elementu i Dodaj **&lt;zasobów&gt;** elementu dla każdego pliku musi być zawarty w pliku VSIX. **Typu** atrybut każdego **&lt;zasobów&gt;** element musi być ustawiony na **Microsoft.VisualStudio.Mvc.Template**. Jest to niestandardowej przestrzeni nazw, który rozumie, Kreator projektu MVC. Zapoznaj się z dokumentacją schematu 2.0 VSIX, aby uzyskać dodatkowe informacje na temat struktury i układ pliku manifestu.

Po prostu Dodawanie plików do pliku VSIX nie jest wystarczające, aby zarejestrować szablony przy użyciu Kreatora MVC. Musisz podać informacje takie jak nazwa szablonu, opis, aparaty widoku obsługiwanych i języka programowania do kreatora MVC. Te informacje są przenoszone w niestandardowe atrybuty powiązane z **&lt;zasobów&gt;** elementu dla każdego **vstemplate** pliku.

&lt;Asset d:VsixSubPath=&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx&quot;

Type=&quot;Microsoft.VisualStudio.Mvc.Template&quot;

d:Source =&quot;pliku&quot;

Path=&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicMvcWebApplicationProjectTemplate.11.csaspx.vstemplate&quot;

ProjectType=&quot;MVC&quot;

Język =&quot;C#&quot;

ViewEngine=&quot;Aspx&quot;

TemplateId=&quot;MyMvcApplication&quot;

Tytuł =&quot;niestandardowe Podstawowa aplikacja sieci Web&quot;

Opis =&quot;niestandardowy szablon utworzony na podstawie aplikacji sieci web podstawowe MVC (Razor)&quot;

Wersja =&quot;4.0&quot;/&gt;

Poniżej przedstawiono omówienie atrybutów niestandardowych, które muszą być obecne:

- **ProjectType** musi być ustawione na MVC.
- **Język** wyznacza języka programowania, obsługiwane przez szablon. Prawidłowe wartości to C# lub VB.
- **ViewEngine** wyznacza aparat widoku, obsługiwane przez szablon, takie jak Aspx lub Razor. Można określić niestandardową wartość dla tego pola.
- **TemplateId** służy do grupowania szablonów. Jeśli wartość jest zgodna istniejący identyfikator szablonu, który będzie zastępują szablony wcześniej zarejestrowane za pomocą Kreatora MVC.
- **Tytuł** wyznacza krótki opis wyświetlany w Kreatorze MVC poniżej każdy szablon projektu.
- **Opis** wyznacza bardziej szczegółowy opis szablonu.

Po dodaniu wszystkich plików w manifeście i zapisany, zauważysz, że **zasoby** kartę w Projektancie spowoduje wyświetlenie wszystkich plików, ale nie niestandardowe atrybuty dodawane do **&lt;zasobów&gt;** elementy **vstemplate** plików.

![Zasoby projektanta projektu](custom-mvc-templates/_static/image8.jpg)

Teraz pozostaje tylko do kompilowania projektu VSIX i zainstaluj go.

Upewnij się, że wszystkie wystąpienia programu Visual Studio są zamknięte na komputerze mającym służyć do testowania rozszerzenie VSIX. Program Visual Studio skanuje w poszukiwaniu nowych rozszerzeń podczas uruchamiania, więc jeśli IDE jest otwarty podczas instalowania VSIX będzie konieczne ponowne uruchomienie programu Visual Studio. W Eksploratorze, kliknij dwukrotnie plik VSIX w celu uruchomienia **Instalator VSIX**, kliknij przycisk **zainstalować** , a następnie uruchom program Visual Studio.

![Instalator VSIX](custom-mvc-templates/_static/image9.jpg)

W menu, wybierz opcję **Narzędzia > rozszerzenia i aktualizacje** aby upewnić się, czy rozszerzenie zostało zainstalowane. Jeśli Instalator VSIX zgłaszane błędy podczas instalowania rozszerzenia można wyświetlić dziennika Instalatora VSIX, aby uzyskać więcej informacji. Dziennik jest zazwyczaj tworzony w **% temp %** folderów użytkownika, że zainstalowano rozszerzenie, na przykład **C:\Users\Bob\AppData\Local\Temp**.

![Rozszerzenia i aktualizacje](custom-mvc-templates/_static/image10.jpg)

Po zamknięciu okna można utworzyć projektu MVC 4 w taki sposób, aby zobaczyć, czy nowe szablony są wyświetlane w Kreatorze MVC.

![Nowy projekt ASP.NET MVC 4](custom-mvc-templates/_static/image11.jpg)

## <a name="limitations"></a>Ograniczenia

1. Kreator MVC nie obsługuje zlokalizowane szablonów niestandardowych.
2. Kreator nie będzie raportować wszelkie błędy, jeśli go nie powiedzie się zlokalizować szablonów niestandardowych. Jeśli którekolwiek z wymaganych atrybutów niestandardowych są nieobecne, szablon będzie po prostu można wykluczyć za pomocą kreatora.
