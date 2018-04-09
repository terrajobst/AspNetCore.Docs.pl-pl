---
uid: web-forms/overview/moving-to-aspnet-20/master-pages
title: Strony wzorcowe | Dokumentacja firmy Microsoft
author: microsoft
description: Jednym z kluczowych składników do pomyślnego witryny sieci Web jest spójny wygląd i zachowanie. W programie ASP.NET 1.x, deweloperzy używana kontrolek użytkownika do replikowania wspólnej elem. strony.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 9c0cce4d-efd9-4c14-b0e8-a1a140abb3f4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/master-pages
msc.type: authoredcontent
ms.openlocfilehash: f45dd9704f665244d2a48ec000326f6e98984e4f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="master-pages"></a>Strony wzorcowej
====================
przez [firmy Microsoft](https://github.com/microsoft)

> Jednym z kluczowych składników do pomyślnego witryny sieci Web jest spójny wygląd i zachowanie. W programie ASP.NET 1.x, deweloperzy umożliwia kontrolek użytkownika replikowanie wspólne elementy strony w aplikacji sieci Web. Chociaż na pewno jest poprawnie rozwiązania, za pomocą formantów użytkownika ma niektóre wady. Na przykład zmiana położenia kontrolki użytkownika wymaga zmiany wiele stron w witrynie. Formanty użytkownika również nie są renderowane w widoku Projekt po wstawiane na stronie.


Jednym z kluczowych składników do pomyślnego witryny sieci Web jest spójny wygląd i zachowanie. W programie ASP.NET 1.x, deweloperzy umożliwia kontrolek użytkownika replikowanie wspólne elementy strony w aplikacji sieci Web. Chociaż na pewno jest poprawnie rozwiązania, za pomocą formantów użytkownika ma niektóre wady. Na przykład zmiana położenia kontrolki użytkownika wymaga zmiany wiele stron w witrynie. Formanty użytkownika również nie są renderowane w widoku Projekt po wstawiane na stronie.

ASP.NET 2.0 wprowadzono główny stron sposób obsługi spójny wygląd i zachowanie, jak i użytkownik wkrótce wyświetlony, wzorzec stron stanowią znaczne ulepszenia za pośrednictwem metody kontroli użytkownika.

## <a name="why-master-pages"></a>Dlaczego głównej strony?

Możesz się zastanawiać, dlaczego stron wzorcowych były potrzebne w programie ASP.NET 2.0. Po wszystkich projektantów witryn sieci Web są już za pomocą kontrolki użytkownika w programie ASP.NET 1.x udostępnianie obszarów zawartości między stronami. Brak faktycznie kilka przyczyn, dlaczego kontrolki użytkownika są mniej niż optymalne rozwiązanie do tworzenia układu wspólnej.

Formanty użytkownika nie faktycznego zdefiniowania układ strony. Zamiast tego definiują układ i funkcjonalność części strony. Różnica między nimi jest ważne, ponieważ ułatwia zarządzanie rozwiązania do kontroli użytkownika znacznie trudniejsze. Na przykład jeśli chcesz zmienić jego położenie kontrolki użytkownika na stronie musi edytować rzeczywiste strony, na której znajduje się kontrolka użytkownika. Thats drobne, jeśli masz niewielką liczbę stron, ale w dużych lokacjach szybko staje się nightmare zarządzania lokacji!

Innym wadą wykorzystania kontrolek użytkownika do definiowania układu wspólnego katalogu głównego architektury ASP.NET sam. Jeśli żadnego członka publicznego formantu użytkownika została zmieniona, wymagane jest ponowne skompilowanie wszystkich stron, które używają kontrolki użytkownika. Z kolei ASP.NET będzie następnie JIT ponowna strony po raz pierwszy są dostępne. Tworzy, jeszcze raz — Skalowalna architektura i problem zarządzania lokacji dla lokacji większy.

Oba te problemy (i wiele innych) dotyczą dobrze stron wzorcowych w programie ASP.NET 2.0.

## <a name="how-master-pages-work"></a>Jak strony wzorcowe pracy

Strona wzorcowa jest odpowiednikiem szablonu do innych stron. Elementy na stronie, które powinny być współużytkowane przez innych stron (np. menu, obramowanie itp.) są dodawane do strony wzorcowej. Po dodaniu nowych stron w witrynie, należy skojarzyć je z strony wzorcowej. Strona, która jest skojarzona z strony wzorcowej jest nazywany **strony zawartości**. Domyślnie strony zawartości ma wygląd ze strony wzorcowej. Jednak po utworzeniu strony wzorcowej można zdefiniować części strony, która strony zawartości można zastąpić własną zawartość. Te fragmenty są definiowane przy użyciu nowego formantu wprowadzone w programie ASP.NET 2.0; **ContentPlaceHolder** formantu.

Strona wzorcowa może zawierać dowolną liczbę formantów elementu ContentPlaceHolder (lub brak wcale.) Na stronie zawartości, zawartość od formantów elementu ContentPlaceHolder pojawi się wewnątrz **zawartości** formantów, inny formant nowe w programie ASP.NET 2.0. Domyślnie strony zawartości, które kontrolki zawartości są puste, dzięki czemu można podać własną zawartość. Jeśli chcesz korzystać z zawartości z strony wzorcowej wewnątrz formantów zawartości należy tak jak pokażemy w dalszej części tego modułu. Formant zawartości jest mapowany do formantu elementu ContentPlaceHolder za pomocą atrybutu ContentPlaceHolderID formantu zawartości. Kod poniżej mapy zawartość kontroli do formantu elementu ContentPlaceHolder wywołać mainBody dla strony wzorcowej.

[!code-aspx[Main](master-pages/samples/sample1.aspx)]

> [!NOTE]
> Często usłyszysz osób, które opisują stron wzorcowych jako klasę podstawową dla innych stron. Thats faktycznie nie jest wartość true. Relacja między stron wzorcowych i strony z zawartością nie jest jednym z dziedziczenia.


**Rysunek 1** zawiera strony wzorcowej i skojarzonej strony zawartości znajdujące się w programie Visual Studio 2005. Widać kontrolki elementu ContentPlaceHolder na strony wzorcowej i odpowiedniej zawartości kontrolki na stronie zawartości. Zwróć uwagę, że zawartość strony wzorcowe, która jest poza ContentPlaceHolder jest widoczny, ale wygaszone się na stronie zawartości. Tylko zawartość wewnątrz elementu ContentPlaceHolder może supplanted przez strony zawartości. Nie można modyfikować całą zawartość, która pochodzi z strony wzorcowej.


![Strona wzorcowa i skojarzone z nią zawartości strony](master-pages/_static/image1.jpg)

**Rysunek 1**: strony wzorcowej i skojarzone z nią zawartości strony


## <a name="creating-a-master-page"></a>Tworzenie strony wzorcowej

Aby utworzyć nową stronę wzorcową:

1. Otwórz program Visual Studio 2005 i Utwórz nową witrynę sieci Web.
2. Kliknij plik, nowy, plików.
3. Wybierz wzorzec pliku w oknie dialogowym Dodaj nowy element, jak pokazano w **rysunek 2**.
4. Kliknij przycisk Dodaj.


![Tworzenie nowej strony wzorcowej](master-pages/_static/image2.jpg)

**Rysunek 2**: Tworzenie nowej strony wzorcowej


Należy zauważyć, że rozszerzenie pliku dla strony wzorcowej <em>.master</em>. Jest to jeden ze sposobów strony wzorcowej różni się od zwykłej strony. Główną różnicą jest to, że miejsce z @Page zawiera dyrektywy, strony wzorcowej @Master dyrektywy. Przejdź do widoku źródłowego wzorca strony właśnie został utworzony i przejrzeć kod.

Nowej strony wzorcowej będzie mieć jeden formant ContentPlaceHolder domyślnie. W większości przypadków warto więcej najpierw utwórz wspólne elementy na stronie, a następnie wstawianie formantów elementu ContentPlaceHolder których niestandardowej zawartości jest potrzebne. W takich przypadkach deweloperzy należy usunąć formant ContentPlaceHolder domyślne i Wstaw nowe opracowanej strony. Element ContentPlaceHolder formanty nie są o zmiennych rozmiarach, mimo że wyświetlane uchwyty zmiany rozmiaru. Rozmiary formantu elementu ContentPlaceHolder automatycznie ustalane na podstawie zawartości, która zawiera z jednym wyjątkiem; Jeśli umieścisz formantu elementu ContentPlaceHolder wewnątrz elementu bloku takich jak komórki tabeli zmieni rozmiar na podstawie rozmiaru elementu.

## <a name="lab-1-working-with-master-pages"></a>Laboratorium 1 Praca z strony wzorcowej

W tym laboratorium utworzysz nową stronę wzorcową i zdefiniuj trzy formanty ContentPlaceHolder. Następnie zostanie utworzenie nowej strony zawartości i Zastąp zawartość z co najmniej jednego elementu ContentPlaceHolder formantów.

1. Tworzenie strony wzorcowej i Wstaw formanty ContentPlaceHolder. 

    1. Utwórz nową stronę wzorcową zgodnie z powyższym opisem.
    2. Usuwanie formantu elementu ContentPlaceHolder domyślne.
    3. Wybierz kontrolkę ContentPlaceHolder klikając przyciemnione górną krawędzią formantu, a następnie usuń go za pomocą klucza DEL na klawiaturze.
    4. Wstawianie nowego przy użyciu tabeli *nagłówka i po stronie* szablon, jak pokazano na rysunku 3. Zmień szerokość i wysokość do 90%, tak aby cała tabela jest widoczne w projektancie.


![](master-pages/_static/image3.jpg)

**Rysunek 3**


1. Umieść kursor w każdej komórki tabeli i ustaw *valign* właściwości *górnej*.
2. Z przybornika Wstawianie formantu elementu ContentPlaceHolder w górnym komórki tabeli (komórka nagłówka.)
3. Po wstawieniu tego formantu elementu ContentPlaceHolder, można zauważyć, że wysokość wiersza zajmuje prawie całą stronę, jak pokazano na rysunku 4. Można zajmującym się który w tym momencie.


![Puste miejsce znajduje się w tej samej komórki jako elementu ContentPlaceHolder](master-pages/_static/image1.gif)

**Rysunek 4**: jest puste miejsce w tej samej komórki jako elementu ContentPlaceHolder


1. Umieść formant ContentPlaceHolder w innych komórek. Po wstawieniu elementu ContentPlaceHolder inne formanty rozmiaru komórek tabeli powinien być można oczekiwać. Strona powinna wyglądać tak jak pokazano na stronie **rysunek 5**.


![Wzorzec o wszystkich kontrolek elementu ContentPlaceHolder. Zauważ, że wysokość komórki do komórek nagłówka jest teraz co powinno być](master-pages/_static/image2.gif)

**Rysunek 5**: wzorzec o wszystkich kontrolek elementu ContentPlaceHolder. Zauważ, że wysokość komórki do komórek nagłówka jest teraz co powinno być


1. Wprowadź tekst wybranym do każdego z trzech kontrolki elementu ContentPlaceHolder.
2. Zapisz strony wzorcowej jako exercise1.master.
3. Tworzenie nowego formularza sieci Web i powiązać ją z exercise1.master strony wzorcowej.
4. Wybierz plik, nowy, plików w programie Visual Studio 2005.
5. Wybierz **formularza sieci Web** w oknie dialogowym Dodawanie nowego elementu.
6. Upewnij się, że zaznaczone jest pole wyboru Wybierz strony wzorcowej, jak pokazano na rysunku 6.


![Dodawanie nowej zawartości strony](master-pages/_static/image3.gif)

**Rysunek 6**: Dodawanie nowej zawartości strony


1. Kliknij przycisk Dodaj.
2. Wybierz exercise1.master w polu Wybierz okno dialogowe strony wzorcowej jak pokazano na rysunku 7.
3. Kliknij przycisk OK, aby dodać nową stronę zawartości.

Nowa strona zawartości pojawi się w programie Visual Studio, jeden formant zawartości dla każdej kontrolki elementu ContentPlaceHolder na stronie głównej. Domyślnie przez formanty zawartości są puste, aby mogli dodawać własną zawartość. Jeśli youd, takich jak ich do korzystania z zawartości z formantu elementu ContentPlaceHolder na stronie głównej, po prostu kliknij symbol tagu inteligentnego (małe czarną strzałkę w prawym górnym rogu formantu) i wybierz polecenie *domyślne wzorce zawartości* za pomocą tagu inteligentnego, jak pokazano w **rysunek 8**. Po wykonaniu tej zmiany elementu menu *Utwórz niestandardowe zawartość*. W tym momencie kliknięcie przycisku usuwa zawartość z możliwość definiowania niestandardowych zawartość dla tego formantu zawartości strony wzorcowej.


![Ustawienia formantu zawartości do domyślnego wzorca zawartości strony](master-pages/_static/image4.gif)

**Rysunek 7**: ustawienia domyślne do zawartości strony wzorca formantu zawartości


## <a name="connecting-master-page-and-content-pages"></a>Połączenie strony wzorcowej oraz strony z zawartością

Skojarzenie między strony wzorcowej oraz strony zawartości można skonfigurować cztery różne sposoby:

- <strong>MasterPageFile</strong> atrybutu @Page — dyrektywa
- Ustawienie **Page.MasterPageFile** właściwości w kodzie.
- **&lt;Stron&gt;** w pliku konfiguracji aplikacji (web.config w katalogu głównym aplikacji)
- **&lt;Stron&gt;** w pliku konfiguracji podfolderach (plik web.config w podfolderze)

## <a name="masterpagefile-attribute"></a>Atrybut MasterPageFile

Atrybut MasterPageFile można łatwo zastosować stronę wzorcową do określonej strony ASP.NET. Istnieje również metoda używana do stosowania strony wzorcowej podczas sprawdzania **wybierz strony wzorcowej** wyboru podczas jak w ćwiczenie 1.

## <a name="setting-pagemasterpagefile-in-code"></a>Ustawianie Page.MasterPageFile w kodzie

Ustawiając właściwość MasterPageFile w kodzie, można zastosować do zawartości w czasie wykonywania określonej strony wzorcowej. Jest to przydatne w sytuacjach, w których konieczne może być zastosowanie określonej strony wzorcowej oparte na rolach użytkowników lub innych kryteriów. Musi być ustawiona właściwość MasterPageFile w metodzie PreInit. Jeśli jest ustawiona po metodzie PreInit, zostanie zgłoszony wyjątku InvalidOperationException. Strona, na którym ta właściwość jest ustawiona, musi mieć również zawartość formantu jako strony najwyższego poziomu. W przeciwnym razie HttpException będą zgłaszane, gdy ustawiona jest właściwość MasterPageFile.

## <a name="using-the-ltpagesgt-element"></a>Przy użyciu &lt;stron&gt; — Element

Można skonfigurować stronę wzorcową dla stron przez ustawienie atrybutu masterPageFile &lt;stron&gt; elementu w pliku web.config. Przy użyciu tej metody, należy pamiętać, że niżej w strukturze aplikacji plików web.config można zastąpić to ustawienie. Dowolny ustawiony w atrybut MasterPageFile @Page dyrektywy zostanie również zastąpić to ustawienie. Przy użyciu &lt;stron&gt; element ułatwia tworzenie <em>wzorca</em> strony głównej, która może zostać zastąpiona w razie potrzeby w szczególności folderów lub plików.

## <a name="properties-in-master-pages"></a>Właściwości strony wzorcowej

Strona wzorcowa mogą uwidaczniać właściwości dokonując po prostu te właściwości publicznych w ramach strony wzorcowej. Na przykład poniższy kod definiuje właściwość o nazwie SomeProperty:

[!code-csharp[Main](master-pages/samples/sample2.cs)]

Aby uzyskać dostęp do właściwości SomeProperty ze strony zawartość, konieczne będzie Użyj wzorca właściwości w następujący sposób:

[!code-csharp[Main](master-pages/samples/sample3.cs)]

## <a name="nesting-master-pages"></a>Strony wzorcowe zagnieżdżenia

Strony główne są to doskonałe rozwiązanie zapewniające wspólnej wygląd i działanie w dużych aplikacji sieci Web. Jednak nie jest rzadko mieć niektórych części udział dużych lokacji wspólny interfejs, podczas gdy inne części udziału innego interfejsu. Aby rozwiązać wymagających, wiele stron wzorcowych są doskonałe rozwiązanie. Jednak jeszcze tego nie adresów fakt, że dużej aplikacji może mieć niektórych składników (np. menu, na przykład), które są współużytkowane przez wszystkie strony i inne składniki, które są udostępniane tylko między niektórych części lokacji. Dla tego typu sytuacji zagnieżdżone strony wzorcowe wypełnienia konieczność dobrze. Jak przedstawiono, normalne strony wzorcowej składa się z strony wzorcowej oraz strony zawartość. W przypadku osadzonej stronie głównej istnieją dwie strony wzorcowej; wzorzec nadrzędny i wzorzec podrzędnych. Strony wzorcowej podrzędnej jest również strony zawartości i jego główny nadrzędny strony wzorcowej.

Oto kod typowe strony głównej:

[!code-aspx[Main](master-pages/samples/sample4.aspx)]

W przypadku master zagnieżdżone to wzorca macierzystego. Innej strony wzorcowej mogą używać tej strony jako jego strony wzorcowej i ten kod będzie wyglądać następująco:

[!code-aspx[Main](master-pages/samples/sample5.aspx)]

Należy pamiętać, że w tym scenariuszu, wzorzec podrzędnej jest również strony zawartości dla wzorca macierzystego. Całą zawartość wzorca podrzędnych pojawia się wewnątrz formantu zawartości, która pobiera zawartość z nadrzędnego elementu ContentPlaceHolder kontroli.

> [!NOTE]
> Obsługa projektanta nie jest dostępna dla zagnieżdżone strony wzorcowe. Podczas projektowania przy użyciu zagnieżdżonych wzorców, należy użyć widoku źródła.


To wideo pokazuje, przewodnik przy użyciu zagnieżdżone strony wzorcowe.


![](master-pages/_static/image1.png)


[Otwórz wideo na pełnym ekranie](master-pages/_static/nested1.wmv)


![Wybieranie strony wzorcowej](master-pages/_static/image4.jpg)

**Rysunek 8**: Wybieranie strony wzorcowej
