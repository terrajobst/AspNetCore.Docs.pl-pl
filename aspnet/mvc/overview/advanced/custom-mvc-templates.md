---
uid: mvc/overview/advanced/custom-mvc-templates
title: Szablon niestandardowy MVC | Dokumentacja firmy Microsoft
author: joeloff
description: "Utwórz szablon jako rozszerzenia VSIX."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/10/2012
ms.topic: article
ms.assetid: b0a214c7-2f38-4dbc-b47f-bd7bd9df97bd
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/advanced/custom-mvc-templates
msc.type: authoredcontent
ms.openlocfilehash: c3ddd4e341511f520927e924b25d890088adb69e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="custom-mvc-template"></a>Szablon niestandardowy MVC
====================
przez [Eloff wybieramy](https://github.com/joeloff)

Wersja aktualizacji narzędzi programu MVC 3 dla programu Visual Studio 2010 wprowadzono kreatora oddzielny projekt dla projektów MVC. Zmiana została regulowane przez dwa czynniki. Po pierwsze wprowadzenie nowych szablonów w MVC 3 i obsługę aparatów widoków dodatkowe, takie jak Razor prowadzić do overcrowding okna dialogowego Nowy projekt w programie Visual Studio. Po drugie klienci miał podanie punkty rozszerzeń i Kreator nowych projektów MVC może umożliwić nam odpowiedzieć na te żądania.

Dodawanie niestandardowych szablonów został uciążliwe procesu, który zależał od za pomocą rejestru, aby wyświetlić nowe szablony Kreator projektów MVC. Tworzenie nowego szablonu było zawijany MSI, aby upewnić się, że wpisy rejestru niezbędne zostałyby utworzone w czasie instalacji. Alternatywne został plik ZIP zawierający dostępny szablon i mieć ręcznie utworzyć wpisy rejestru wymagane przez użytkownika.

Żadna z wyżej wymienionych metod doskonale, zdecydowaliśmy się korzystać z niektórych istniejącą infrastrukturę [VSIX](https://msdn.microsoft.com/library/ff363239.aspx) rozszerzenia, aby ułatwić autora, dystrybucji i zainstalować szablonów niestandardowych MVC, począwszy od MVC 4 dla programu Visual Studio 2012. Niektóre z zalet tej metody to:

- Rozszerzenie VSIX może zawierać wiele szablonów, które włączenia obsługi różnych języków (C# i Visual Basic) i wiele aparatów widoku (ASPX i Razor).
- Rozszerzenie VSIX celem może być wiele jednostki SKU programu Visual Studio Express SKU włącznie.
- [Galerii programu Visual Studio](https://visualstudiogallery.msdn.microsoft.com/) ułatwia dystrybucję rozszerzenia do szerokiej grupy odbiorców.
- Rozszerzenia VSIX można uaktualnić, co ułatwia tworzenie poprawek i aktualizacji do szablonów niestandardowych.

## <a name="prerequisites"></a>Wymagania wstępne

- Użytkownicy muszą znać opracowania szablonów projektu, w tym kod znaczników wymagane pliki vstemplate itp.
- Użytkownicy będą musiały korzystać z programu Visual Studio Professional i nowszym. Express SKU nie obsługują tworzenia projektów VSIX.
- [Visual Studio 2012 SDK](https://www.microsoft.com/download/details.aspx?id=30668) zainstalowane.

## <a name="example"></a>Przykład

Pierwszym krokiem jest utworzenie nowego projektu VSIX przy użyciu języka C# lub Visual Basic. Wybierz **Plik > Nowy projekt**, następnie kliknij przycisk **rozszerzalności** w okienku po lewej stronie i wybierz **projektu VSIX**.

![Nowy projekt](custom-mvc-templates/_static/image1.jpg)

Po utworzeniu projektu zostaną otwarte projektanta VSIX.

![Metadane projektanta projektu](custom-mvc-templates/_static/image2.jpg)

Projektant służy do edytowania niektórych ogólne właściwości rozszerzenia, która będzie wyświetlana użytkownikom podczas instalowania rozszerzenia lub Przeglądaj zainstalowanych rozszerzeń programu Visual Studio (**Narzędzia > rozszerzenia i aktualizacje**). Po zakończeniu informacje ogólne kliknij **kartę zainstalować obiekty docelowe**.

![Obiekty docelowe instalacji projektanta projektu](custom-mvc-templates/_static/image3.jpg)

Ta karta służy do określania jednostki SKU i wersje programu Visual Studio, które są obsługiwane przez rozszerzenie. Zaznacz pole wyboru **ten plik VSIX jest instalowany dla wszystkich użytkowników** umożliwiające instaluje na komputerze z pliku VSIX. Polecenie **nowy** przycisku z prawej strony, aby dodać dodatkowe jednostki SKU takich jak Express deweloperów sieci Web (VWD).

![Dodawanie nowego celu instalacji](custom-mvc-templates/_static/image4.jpg)

Jeśli zamierzasz obsługiwać wszystkie Professional i nowsze wersje produktu (Professional, Premium i Ultimate) należy wybrać minimalną wersję produktu z rodziny **Microsoft.VisualStudio.Pro**. Pamiętaj, aby zapisać wszystkie zmiany po ukończeniu celów instalacji.

![Obiekty docelowe instalacji projektanta projektu](custom-mvc-templates/_static/image5.jpg)

**Zasoby** karta służy do dodawania wszystkie pliki zawartości do pliku VSIX. Ponieważ MVC wymaga metadanych niestandardowych można będzie edycji raw XML w pliku manifestu VSIX zamiast **zasoby** kartę, aby dodać zawartość. Rozpocznij od dodania zawartość szablonu projektu VSIX. Należy pamiętać, że struktura folderu i zawartość duplikatów układ projektu. Poniższy przykład zawiera cztery szablony projektów, utworzone na podstawie szablonu projektu podstawowe MVC. Upewnij się, że wszystkie pliki wchodzące w skład szablonu projektu (wszystko pod folderem ProjectTemplates) są dodawane do **zawartości** itemgroup w pliku VSIX projektu pliku oraz że każdy element zawiera  **CopyToOutputDirectory** i **IncludeInVsix** zestawu metadanych, jak pokazano w poniższym przykładzie.

&lt;Content Include=&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicWeb.config&quot;&gt;

&lt;CopyToOutputDirectory&gt;Always&lt;/CopyToOutputDirectory&gt;

&lt;IncludeInVSIX&gt;true&lt;/IncludeInVSIX&gt;

&lt;/Content&gt;

W przeciwnym razie IDE spróbuje Kompilacja zawartości szablonu podczas tworzenia pliku VSIX i prawdopodobnie zostanie wyświetlony komunikat o błędzie. Pliki kodu w szablonach często zawierają specjalne [parametrów szablonu](https://msdn.microsoft.com/library/eehb4faa(v=vs.110).aspx) używana przez program Visual Studio, gdy szablon projektu zostanie uruchomiony i nie można skompilować w IDE.

![Eksplorator rozwiązań](custom-mvc-templates/_static/image6.jpg)

Zamknij projektanta VSIX, a następnie kliknij prawym przyciskiem myszy **source.extension.manifest** w pliku **Eksploratora rozwiązań** i wybierz **Otwórz za pomocą** i wybierz polecenie **XML ( Edytor tekstów)** opcji.

![Otwórz za pomocą okna dialogowego](custom-mvc-templates/_static/image7.jpg)

Utwórz  **&lt;zasoby&gt;**  element i Dodaj  **&lt;zasobów&gt;**  elementu dla każdego pliku, który musi być uwzględniona w pliku VSIX. **Typu** atrybut każdego  **&lt;zasobów&gt;**  element musi być ustawiony na **Microsoft.VisualStudio.Mvc.Template**. Jest to niestandardowej przestrzeni nazw, która rozumie, Kreator projektu MVC. Zapoznaj się z dokumentacją schematu 2.0 VSIX, aby uzyskać dodatkowe informacje na temat struktury i układ pliku manifestu.

Tylko dodanie plików do pliku VSIX nie wystarcza do rejestracji szablonów przy użyciu Kreatora MVC. Musisz podać informacje, takie jak nazwa szablonu, opis i aparatów widoków obsługiwanych język programowania, w Kreatorze programu MVC. Te informacje są przenoszone w niestandardowe atrybuty powiązane z  **&lt;zasobów&gt;**  elementu dla każdego **vstemplate** pliku.

&lt;Asset d:VsixSubPath=&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx&quot;

Type=&quot;Microsoft.VisualStudio.Mvc.Template&quot;

d:Source=&quot;File&quot;

Path=&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicMvcWebApplicationProjectTemplate.11.csaspx.vstemplate&quot;

ProjectType=&quot;MVC&quot;

Language=&quot;C#&quot;

ViewEngine=&quot;Aspx&quot;

TemplateId=&quot;MyMvcApplication&quot;

Tytuł =&quot;niestandardowej podstawowe aplikacji sieci Web&quot;

Opis elementu =&quot;szablon niestandardowy pochodzące z aplikacji sieci web MVC podstawowe (Razor)&quot;

Wersja =&quot;4.0&quot;/&gt;

Poniżej znajdują się objaśnienia dotyczące atrybutów niestandardowych, które muszą być obecne:

- **Typ projektu** musi mieć ustawioną MVC.
- **Język** wyznacza języka programowania, obsługiwane przez szablon. Prawidłowe wartości to C# lub VB.
- **ViewEngine** wyznacza aparat widoku obsługiwane przez szablon, takie jak Aspx lub Razor. Można określić niestandardowe wartości dla tego pola.
- **TemplateId** służy do grupowania szablonów. Jeśli wartość pasuje do istniejącego Identyfikatora szablonu będzie zastąpić szablony zarejestrowana przy użyciu Kreatora MVC.
- **Tytuł** wyznacza krótki opis wyświetlany w Kreatorze MVC poniżej każdego szablonu projektu.
- **Opis elementu** określa dokładniejszy opis szablonu.

Po dodaniu wszystkich plików do manifestu i zapisaniu, można zauważyć, że **zasoby** kartę w Projektancie spowoduje wyświetlenie wszystkich plików, ale atrybutów nie niestandardowych, można dodać do  **&lt;zasobów&gt;**  elementy **vstemplate** plików.

![Zasoby projektanta projektu](custom-mvc-templates/_static/image8.jpg)

Wszystkie opcje, które pozostaje jest teraz do skompilowania projektu VSIX i zainstaluj go.

Upewnij się, że na komputerze są zamykane wszystkie wystąpienia programu Visual Studio gdy chcesz przetestować rozszerzenia VSIX. Visual Studio skanowania pod kątem nowych rozszerzeń podczas uruchamiania, więc jeśli IDE jest otwarty podczas instalowania VSIX będzie konieczne ponowne uruchomienie programu Visual Studio. W Eksploratorze, kliknij dwukrotnie plik VSIX można uruchomić **Instalatora VSIX**, kliknij przycisk **zainstalować** , a następnie uruchom program Visual Studio.

![Instalator VSIX](custom-mvc-templates/_static/image9.jpg)

Wybierz z menu **Narzędzia > rozszerzenia i aktualizacje** aby upewnić się, że rozszerzenie zostało zainstalowane. Jeśli Instalator VSIX zgłaszane błędy podczas instalowania rozszerzenia można wyświetlić dziennik Instalatora VSIX, aby uzyskać więcej informacji. Dziennik jest domyślnie tworzone w **% temp %** folderów użytkownika, dla którego zainstalowano rozszerzenie, na przykład **C:\Users\Bob\AppData\Local\Temp**.

![Rozszerzenia i aktualizacje](custom-mvc-templates/_static/image10.jpg)

Po zamknięciu okna można tworzyć projektów MVC 4, czy nowe szablony są wyświetlane w Kreatorze MVC.

![Nowy projekt ASP.NET MVC 4](custom-mvc-templates/_static/image11.jpg)

## <a name="limitations"></a>Ograniczenia

1. Kreator MVC nie obsługuje zlokalizowanych szablonów niestandardowych.
2. Kreator nie będzie zgłaszać wszelkie błędy, jeśli go nie powiedzie się zlokalizować szablonów niestandardowych. Jeśli którekolwiek z wymaganych atrybutów niestandardowych są nieobecne, szablon po prostu będzie wykluczony z kreatora.
