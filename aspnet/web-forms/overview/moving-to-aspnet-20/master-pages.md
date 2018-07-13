---
uid: web-forms/overview/moving-to-aspnet-20/master-pages
title: Strony wzorcowe | Dokumentacja firmy Microsoft
author: microsoft
description: 'Jednym z kluczowych składników do pomyślnego witryny sieci Web jest spójny wygląd i zachowanie. W programie ASP.NET: 1.x, deweloperzy używane kontrolki użytkownika do replikowania wspólnej elem. strony...'
ms.author: aspnetcontent
ms.date: 02/20/2005
ms.assetid: 9c0cce4d-efd9-4c14-b0e8-a1a140abb3f4
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/master-pages
msc.type: authoredcontent
ms.openlocfilehash: 631aa39c51601f1ae843079d8cd930f77b1093ab
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37819218"
---
<a name="master-pages"></a>Strony wzorcowe
====================
przez [firmy Microsoft](https://github.com/microsoft)

> Jednym z kluczowych składników do pomyślnego witryny sieci Web jest spójny wygląd i zachowanie. W programie ASP.NET: 1.x, deweloperzy umożliwia kontrolki użytkownika replikować wspólnych elementów strony w aplikacji sieci Web. Chociaż to oczywiście zwiększą rozwiązania, za pomocą kontrolki użytkownika ma pewne wady. Na przykład zmiana położenia kontrolki użytkownika wymaga zmian na wielu stronach w lokacji. Formanty użytkownika również nie są renderowane w widoku Projekt po wstawiane na stronie.


Jednym z kluczowych składników do pomyślnego witryny sieci Web jest spójny wygląd i zachowanie. W programie ASP.NET: 1.x, deweloperzy umożliwia kontrolki użytkownika replikować wspólnych elementów strony w aplikacji sieci Web. Chociaż to oczywiście zwiększą rozwiązania, za pomocą kontrolki użytkownika ma pewne wady. Na przykład zmiana położenia kontrolki użytkownika wymaga zmian na wielu stronach w lokacji. Formanty użytkownika również nie są renderowane w widoku Projekt po wstawiane na stronie.

ASP.NET 2.0 wprowadzono główny strony sposób utrzymania spójny wygląd i zachowanie, jak i użytkownik zostanie wkrótce znajduje się główny strony reprezentują znacznej poprawy za pośrednictwem metody kontroli użytkownika.

## <a name="why-master-pages"></a>Dlaczego Master strony?

Możesz się zastanawiać, dlaczego potrzebne były stron wzorcowych platformy ASP.NET w wersji 2.0. Po wszystkich projektantów witryn sieci Web jest już używany kontrolki użytkownika w programie ASP.NET: 1.x do udostępniania zawartości obszarów między stronami. Istnieją faktycznie kilka powodów dlaczego kontrolki użytkownika są mniej niż optymalne rozwiązanie do tworzenia typowych układu.

Formanty użytkownika nie definiowania układu strony. Zamiast tego mogą określać układ i funkcje dla części strony. Różnica między tymi dwoma jest ważne, ponieważ dzięki niemu możliwości rozwiązania do kontroli użytkownika dużo bardziej skomplikowane. Na przykład jeśli chcesz zmienić położenie kontrolki użytkownika na stronie, możesz edytować rzeczywistą stronę, na którym jest wyświetlany formantu użytkownika. Thats poprawnie, jeśli masz niewielką liczbę stron, ale w dużych witrynach szybko staje się okropnej zarządzania lokacji!

Innym wadą wykorzystania samej kontrolki użytkownika do definiowania układu typowych zostaje umieszczone w architekturze platformy ASP.NET, sam. Jeśli żadnego publicznego elementu członkowskiego kontrolki użytkownika zostało zmienione, należy ponownie skompilować wszystkie strony, które używają kontrolki użytkownika. Z kolei ASP.NET będzie, a następnie ponownie JIT stron sieci po raz pierwszy są dostępne. Tworzy, jeszcze raz nieskalowalny architektury i problem zarządzania lokacji dla lokacji większe.

Oba te problemy (i wielu innych) dotyczą dobrze stron wzorcowych platformy ASP.NET w wersji 2.0.

## <a name="how-master-pages-work"></a>Jak strony wzorcowe pracy

Strony wzorcowej jest analogiczne do szablonu dla innych stron. Elementy strony, które powinny być współużytkowane przez inne strony (np. menu, obramowania itp.) są dodawane do strony wzorcowej. Po dodaniu nowych stron w witrynie, można skojarzyć je z strony wzorcowej. Nosi nazwę strony, który jest skojarzony ze stroną wzorcową **strony zawartości**. Domyślnie strony zawartości ma wygląd ze strony wzorcowej. Jednak po utworzeniu strony wzorcowej, można zdefiniować części strony, strony zawartości można zastąpić własną zawartość. Te fragmenty są definiowane przy użyciu nowej kontrolki wprowadzone w programie ASP.NET 2.0; **ContentPlaceHolder** kontroli.

Strona wzorcowa może zawierać dowolną liczbę kontrolek ContentPlaceHolder (lub brak wszystkie). Na stronie zawartości, zawartość z kontrolek ContentPlaceHolder pojawia się wewnątrz **zawartości** formantów, innego nowej kontrolki w programie ASP.NET 2.0. Domyślnie strony zawartości, które kontrolki zawartości są puste, dzięki czemu możesz podać własną zawartość. Jeśli chcesz korzystać z zawartości z strony wzorcowej wewnątrz formanty zawartości, możesz to zrobić tak, jak widać w dalszej części tego modułu. Formant zawartości jest zamapowana na formant ContentPlaceHolder za pomocą atrybutu ContentPlaceHolderID zawartości kontrolki. Kod poniżej mapy do formantu ContentPlaceHolder o nazwie mainBody na stronie wzorcowej kontrolować zawartość.

[!code-aspx[Main](master-pages/samples/sample1.aspx)]

> [!NOTE]
> Często otrzymasz od osób, które opisują strony wzorcowe, jako klasę bazową dla innych stron. Thats faktycznie nie jest wartość true. Relacja między stron wzorcowych i stronach zawartości nie jest jednym z dziedziczenia.


**Rysunek 1** pokazuje stronę wzorcową i skojarzonej strony zawartości, w jakiej występują w programie Visual Studio 2005. Możesz zobaczyć kontrolki ContentPlaceHolder na stronie wzorcowej i odpowiedni zawartości kontrolki na stronie zawartości. Należy zauważyć, że zawartość strony wzorcowe, która jest poza ContentPlaceHolder jest widoczne, ale są wygaszone się na stronie zawartości. Tylko zawartość wewnątrz ContentPlaceHolder można supplanted przez strony zawartości. Cała zawartość, która pochodzi z strony wzorcowej jest niezmienny.


![Strona wzorcowa i skojarzone z nią zawartości strony](master-pages/_static/image1.jpg)

**Rysunek 1**: stronę wzorcową i skojarzone z nią zawartości strony


## <a name="creating-a-master-page"></a>Tworzenie strony wzorcowej

Aby utworzyć nową stronę wzorcową:

1. Otwórz program Visual Studio 2005 i Utwórz nową witrynę sieci Web.
2. Kliknij plik, nowy, plików.
3. Wybierz plik głównej z poziomu okna dialogowego Dodawanie nowego elementu, jak pokazano na **rysunek 2.**.
4. Kliknij przycisk Dodaj.


![Tworzenie nowej strony wzorcowej](master-pages/_static/image2.jpg)

**Rysunek 2**: Tworzenie nowej strony wzorcowej


Należy zauważyć, że plik ma rozszerzenie dla strony wzorcowej <em>.master</em>. Jest jednym ze sposobów, które strony wzorcowej różni się od zwykłej strony. Główną różnicą jest to, że proceduralny @Page dyrektywy, zawiera strony wzorcowej @Master dyrektywy. Przejdź do widoku źródłowego dla głównego strony właśnie został utworzony i przeglądać kod.

Domyślnie nowe strony wzorcowej będzie mieć jeden formant ContentPlaceHolder. W większości przypadków warto więcej najpierw utworzyć wspólne elementy strony, a następnie Wstawianie kontrolek ContentPlaceHolder gdzie pożądana jest zawartości niestandardowej. W takich przypadkach deweloperzy będą chcieli usunąć domyślny formant ContentPlaceHolder i Wstaw nowe opracowanej strony. Kontrolek ContentPlaceHolder nie są o zmiennym rozmiarze, pomimo faktu, że wyświetlają one uchwyty zmiany rozmiaru. Rozmiary kontroli ContentPlaceHolder automatycznie na podstawie zawartości, która zawiera się on z jednym wyjątkiem; Jeśli umieścisz kontrolki ContentPlaceHolder w elemencie bloku, takich jak komórki tabeli, będzie to rozmiar zgodnie z rozmiarem elementu.

## <a name="lab-1-working-with-master-pages"></a>Laboratorium 1 pracy za pomocą stron wzorcowych

W tym środowisku laboratoryjnym utworzysz nową stronę wzorcową i zdefiniuj trzech kontrolek ContentPlaceHolder. Następnie utworzysz nową stronę zawartości i Zastąp zawartość z co najmniej jednej z kontrolek ContentPlaceHolder.

1. Utwórz stronę wzorcową i Wstawianie kontrolek ContentPlaceHolder. 

    1. Utwórz nową stronę wzorcową, zgodnie z powyższym opisem.
    2. Usuń domyślny formant ContentPlaceHolder.
    3. Zaznacz formant ContentPlaceHolder, klikając przyciemnione krawędzi górnego obramowania kontrolki, a następnie usuń ją, naciskając klawisz DEL na klawiaturze.
    4. Wstawianie nowej tabeli za pomocą *nagłówek i po stronie* szablonu, jak pokazano na rysunku 3. Zmień szerokość i wysokość do 90%, tak, aby cała tabela jest widoczne w projektancie.


![](master-pages/_static/image3.jpg)

**Rysunek 3.**


1. Umieść kursor w każdej komórce tabeli i ustaw *dopasowanie w pionie* właściwości *górnej*.
2. Z przybornika Wstawianie formantu ContentPlaceHolder górnej komórki tabeli (komórki nagłówka.)
3. Podczas wstawiania tego formantu ContentPlaceHolder można zauważyć, że wiersze o nierównej wysokości zajmuje prawie całej strony, jak pokazano na rysunku 4. Dont wiedzieć, w tym momencie.


![Puste miejsce znajduje się w tej samej komórki jako ContentPlaceHolder](master-pages/_static/image1.gif)

**Rysunek 4**: puste miejsce znajduje się w tej samej komórki jako ContentPlaceHolder


1. Umieść kontroli ContentPlaceHolder w innych komórek. Po wstawieniu innych kontrolek ContentPlaceHolder rozmiaru komórek tabeli należy, jak można oczekiwać. Strona powinna teraz wyglądać strona wyświetlona w **rysunek 5**.


![Głównego przy użyciu wszystkich kontrolek ContentPlaceHolder. Zauważ, że wysokość komórki dla komórki nagłówka teraz czymś co powinno być](master-pages/_static/image2.gif)

**Rysunek 5**: głównego przy użyciu wszystkich kontrolek ContentPlaceHolder. Zauważ, że wysokość komórki dla komórki nagłówka teraz czymś co powinno być


1. Wprowadź jakiś tekst wybranego do każdego z trzech kontrolek ContentPlaceHolder.
2. Zapisz stronę wzorcową jako exercise1.master.
3. Tworzenie nowego formularza sieci Web i skojarz go ze stroną wzorcową exercise1.master.
4. Wybierz plik, nowy, plik w programie Visual Studio 2005.
5. Wybierz **formularz sieci Web** w oknie dialogowym Dodaj nowy element.
6. Upewnij się, że zaznaczone jest pole wyboru Wybierz stronę wzorcową, jak pokazano na rysunku 6.


![Dodawanie nowej strony zawartości](master-pages/_static/image3.gif)

**Rysunek 6**: Dodawanie nowej strony zawartości


1. Kliknij przycisk Dodaj.
2. Wybierz exercise1.master wybierz w oknie dialogowym strona wzorcowa jak pokazano na rysunku 7.
3. Kliknij przycisk OK, aby dodać nową stronę zawartości.

Z jednym formantem zawartości dla każdego formantu ContentPlaceHolder na stronie wzorcowej w programie Visual Studio zostanie wyświetlona nowa strona zawartości. Domyślnie formanty zawartości są puste, aby dodać własną zawartość. Jeżeli youd musiały korzystać z zawartości z formantu ContentPlaceHolder strony wzorcowej, po prostu kliknij symbol tagu inteligentnego (małe czarną strzałkę w prawym górnym rogu formantu) i wybierz polecenie *domyślną zawartość wzorców* za pomocą tagu inteligentnego, jak pokazano na **rysunek 8**. Jeśli tak zrobisz, element menu zmienia się na *Utwórz niestandardowe zawartość*. W tym momencie klikając polecenie usuwa zawartość z strony wzorcowej, umożliwiając Definiowanie niestandardowej zawartości dla tego określonego formantu zawartości.


![Ustawienia formantu zawartości domyślne, aby zawartość strony główne](master-pages/_static/image4.gif)

**Rysunek 7**: ustawienie kontrolki zawartości domyślną zawartość strony główne


## <a name="connecting-master-page-and-content-pages"></a>Łączenie z strony wzorcowej oraz strony z zawartością

Skojarzenie między strony wzorcowej oraz strony zawartości można skonfigurować w jednej z czterech sposobów:

- <strong>MasterPageFile</strong> atrybutu @Page — dyrektywa
- Ustawienie **Page.MasterPageFile** właściwości w kodzie.
- **&lt;Stron&gt;** elementu w pliku konfiguracyjnym aplikacji (web.config w folderze głównym aplikacji)
- **&lt;Stron&gt;** elementu w pliku konfiguracji (web.config w podfolderze) podfoldery

## <a name="masterpagefile-attribute"></a>Atrybut MasterPageFile

Ten atrybut MasterPageFile ułatwia stosowanie strony wzorcowej do konkretnej strony ASP.NET. Warto również metodę używaną do zastosowania strony wzorcowej, podczas sprawdzania **wybierz stronę wzorcową** pole wyboru co Ty, jak w ćwiczenie 1.

## <a name="setting-pagemasterpagefile-in-code"></a>Ustawianie Page.MasterPageFile w kodzie

Ustawiając właściwość MasterPageFile w kodzie, można zastosować określonej strony wzorcowej do zawartości w czasie wykonywania. Jest to przydatne w przypadkach, w których konieczne może być zastosowanie określonej strony wzorcowej, na podstawie roli użytkowników lub innych kryteriów. Właściwość MasterPageFile musi być ustawiona w metodzie PreInit. Jeśli jest ustawiona po metodzie PreInit, zostanie zgłoszony InvalidOperationException. Strona, na którym ta właściwość jest ustawiona, również muszą mieć zawartość formantu jako formant najwyższego poziomu dla strony. W przeciwnym razie httpexception — zostanie zgłoszony, jeśli ustawiono właściwość MasterPageFile.

## <a name="using-the-ltpagesgt-element"></a>Za pomocą &lt;stron&gt; — Element

Można skonfigurować stronę wzorcową stron, ustawiając atrybut masterPageFile w &lt;stron&gt; elementu w pliku web.config. Przy użyciu tej metody, należy pamiętać o tym, że w plikach web.config niżej w strukturze aplikacji można zastąpić to ustawienie. Dowolny atrybut MasterPageFile w @Page dyrektywy zostanie również zastąpić to ustawienie. Za pomocą &lt;stron&gt; element upraszcza tworzenie <em>wzorca</em> strona wzorcowa, która może zostać zastąpiona w razie potrzeby w szczególności folderów lub plików.

## <a name="properties-in-master-pages"></a>Właściwości na stronach wzorcowych

Strona wzorcowa można ujawnić właściwości podejmując po prostu te właściwości publicznej w obrębie strony wzorcowej. Na przykład poniższy kod definiuje właściwość o nazwie SomeProperty:

[!code-csharp[Main](master-pages/samples/sample2.cs)]

Dostęp do właściwości SomeProperty z poziomu strony zawartości, należy użyć wzorca właściwości w następujący sposób:

[!code-csharp[Main](master-pages/samples/sample3.cs)]

## <a name="nesting-master-pages"></a>Strony wzorcowe zagnieżdżania

Strony wzorcowe są idealnym rozwiązaniem dla zapewnienia wspólne wyglądu i działania w dużych aplikacji sieci Web. Jednak nie jest niczym niezwykłym mają niektórych części udział w przypadku dużych witryn wspólny interfejs, a inne części udostępnianie innego interfejsu. Aby spełnić te potrzeby, wiele stron wzorcowych są idealne rozwiązanie. Jednak że nadal nie adresów fakt, że dużych aplikacji może mieć niektóre składniki (takie jak menu, na przykład), które są współużytkowane przez wszystkie strony i inne składniki, które są udostępniane tylko wśród niektóre sekcje witryny. W tej sytuacji zagnieżdżone strony wzorcowe wypełnienia potrzebę dobrze. Jak wiesz, normalne strony wzorcowej składa się z strony wzorcowej oraz strony zawartości. W sytuacji zagnieżdżonej strony wzorcowej istnieją dwie strony wzorcowe; wzorzec nadrzędnego i wzorzec podrzędnych. Strona wzorcowa podrzędne również strony zawartości i jego główny jest nadrzędny strony wzorcowej.

Poniżej przedstawiono kod dla typowych strony wzorcowej:

[!code-aspx[Main](master-pages/samples/sample4.aspx)]

W przypadku wzorca zagnieżdżonych będzie to główny nadrzędnej. Innej strony wzorcowej mogą używać tej strony jako jego strony głównej, a ten kod będzie wyglądać następująco:

[!code-aspx[Main](master-pages/samples/sample5.aspx)]

Należy pamiętać, że w tym scenariuszu wzorzec podrzędne również strony zawartości dla wzorca macierzystego. Całą zawartość wzorca podrzędnych pojawia się wewnątrz formantu zawartości, która pobiera zawartość z elementu nadrzędnego ContentPlaceHolder kontroli.

> [!NOTE]
> Obsługa projektanta nie jest dostępna dla zagnieżdżone strony wzorcowe. Gdy jest tworzona przy użyciu zagnieżdżonych wzorców, należy użyć widoku źródła.


Ten film pokazuje Przewodnik po użyciu zagnieżdżone strony wzorcowe.


![](master-pages/_static/image1.png)


[Otwórz wideo pełnego ekranu](master-pages/_static/nested1.wmv)


![Wybieranie strony wzorcowej](master-pages/_static/image4.jpg)

**Rysunek 8**: Wybieranie strony wzorcowej
