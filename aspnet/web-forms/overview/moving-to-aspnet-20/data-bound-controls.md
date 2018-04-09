---
uid: web-forms/overview/moving-to-aspnet-20/data-bound-controls
title: Kontrolki powiązane dane | Dokumentacja firmy Microsoft
author: microsoft
description: Większość aplikacji ASP.NET polegają na pewien stopień prezentację danych ze źródła danych zaplecza. Formanty powiązane z danymi zostały istotną częścią wchodzącymi w interakcje w...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 0e23ff32-646d-43f3-8bec-6b2313d3abd6
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/data-bound-controls
msc.type: authoredcontent
ms.openlocfilehash: 5c3f6aad4b87450149189352e86106f46c765fb8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="data-bound-controls"></a>Kontrolki powiązane dane
====================
przez [firmy Microsoft](https://github.com/microsoft)

> Większość aplikacji ASP.NET polegają na pewien stopień prezentację danych ze źródła danych zaplecza. Formanty powiązane z danymi zostały istotną częścią interakcji z danymi w dynamicznych aplikacji sieci Web. Platforma ASP.NET 2.0 wprowadzono niektóre istotne ulepszenia formantów powiązanych z danymi, w tym nową klasę BaseDataBoundControl i składni deklaratywnej.


Większość aplikacji ASP.NET polegają na pewien stopień prezentację danych ze źródła danych zaplecza. Formanty powiązane z danymi zostały istotną częścią interakcji z danymi w dynamicznych aplikacji sieci Web. Platforma ASP.NET 2.0 wprowadzono niektóre istotne ulepszenia formantów powiązanych z danymi, w tym nową klasę BaseDataBoundControl i składni deklaratywnej.

BaseDataBoundControl działa jako klasę podstawową dla obiekt DataBoundControl klasa i klasy element HierarchicalDataBoundControl. W tym module omówimy następujące klasy, które pochodzą z DataBoundControl:

- AdRotator
- Formanty listy
- GridView
- FormView
- Widoku DetailsView

Również omówimy następujące klasy, które pochodzi z klasy element HierarchicalDataBoundControl:

- TreeView
- Menu
- SiteMapPath

## <a name="databoundcontrol-class"></a>Obiekt DataBoundControl — klasa

Klasa DataBoundControl jest klasą abstrakcyjną (oznaczony MustInherit w języku VB) służy do interakcji z tabelarycznym lub dane styl listy. Następujące sterowniki są niektóre formanty, które pochodzą z DataBoundControl.

## <a name="adrotator"></a>AdRotator

Sterowanie AdRotator umożliwia wyświetlanie transparentu grafiki na stronie sieci Web, która jest połączona z określonym adresem URL. Grafika, która jest wyświetlana jest obracana przy użyciu właściwości formantu. Można skonfigurować częstotliwość wyświetlania ad określonej na stronie, używając **wrażenia** właściwości i reklam można filtrować przy użyciu słowa kluczowego filtrowania.

AdRotator formantów za pomocą pliku XML lub tabeli w bazie danych. Następujące atrybuty są używane w plikach XML do konfigurowania kontroli AdRotator.

### <a name="imageurl"></a>ImageUrl
Adres URL obrazu do wyświetlenia dla usługi ad.

### <a name="navigateurl"></a>NavigateUrl
Adres URL, który użytkownik należy podjąć w celu po kliknięciu ad. Powinno to być zakodowane w adresie URL.

### <a name="alternatetext"></a>AlternateText
Tekst alternatywny wyświetlany w etykietce narzędzia i odczytywane przez czytników ekranu. Również wyświetlana, gdy nie jest określony przez ImageUrl obrazu.

### <a name="keyword"></a>Keyword
Definiuje — słowo kluczowe, które mogą być używane podczas filtrowania — słowo kluczowe. Jeśli jest określony, będą wyświetlane tylko reklam ze słowem kluczowym zgodny z filtrem — słowo kluczowe.

### <a name="impressions"></a>Odciski
Liczba wagi, która określa, jak często określonego ad prawdopodobnie są wyświetlane. Jest względną wrażenie innych reklam, w tym samym pliku. Wartość maksymalna zbiorowe odcisków dla wszystkich ogłoszeń w pliku XML jest 2,048,000,000 1.

### <a name="height"></a>Wysokość
Wysokość ad w pikselach.

### <a name="width"></a>Szerokość
Szerokość ad w pikselach.


> [!NOTE]
> Atrybuty wysokości i szerokości przesłonić wysokość i szerokość dla AdRotator formancie.


Typowy pliku XML może wyglądać następująco:

[!code-xml[Main](data-bound-controls/samples/sample1.xml)]

W powyższym przykładzie ad "contoso" prawdopodobnie dwukrotnie jako są wyświetlane jako ad dla witryny sieci Web platformy ASP.NET z powodu wartość atrybutu wrażenia.

Aby wyświetlić reklam z tego pliku XML, Dodaj formant AdRotator do strony i ustaw **AdvertisementFile** właściwości, aby wskazywał na plik XML, jak pokazano poniżej:

[!code-aspx[Main](data-bound-controls/samples/sample2.aspx)]

Jeśli chcesz użyć tabeli bazy danych jako źródło danych dla formantu AdRotator, należy najpierw konfigurowania bazy danych przy użyciu następującego schematu:

| **Nazwa kolumny** | **Typ danych** | **Opis** |
| --- | --- | --- |
| ID | int | Klucz podstawowy. W tej kolumnie mogą mieć dowolną nazwę. |
| ImageUrl | nvarchar (*długość*) | Względny lub bezwzględny adres URL obrazu do wyświetlenia dla usługi ad. |
| NavigateUrl | nvarchar (*długość*) | Docelowy adres URL dla usługi ad. Jeśli wartość nie zostanie określona, usługi ad nie jest hiperłącze. |
| AlternateText | nvarchar (*długość*) | Tekst wyświetlany, jeśli nie można odnaleźć obrazu. W niektórych przeglądarkach tekst jest wyświetlany jako etykietka narzędzia. Tekst alternatywny służy także do ułatwień dostępu, aby użytkownicy, którzy nie widzi grafiki słyszalny opis Czytaj na głos. |
| Keyword | nvarchar (*długość*) | Kategoria usług AD, w którym można filtrować strony. |
| Odciski | int(4) | Liczba, która określa prawdopodobieństwo częstotliwość ad jest wyświetlany. Im większa liczba, częściej ad będzie wyświetlany. Sumę wszystkich wartości wrażenia w pliku XML nie może przekraczać 2,048,000,000-1. |
| Szerokość | int(4) | Szerokość obrazu w pikselach. |
| Wysokość | int(4) | Wysokość obrazu w pikselach. |

W przypadkach, gdy masz już bazę danych z innym schematem, można użyć **AlternateTextField**, **ImageUrlField**, i **NavigateUrlField** właściwości do mapowania AdRotator atrybuty do istniejącej bazy danych. Aby wyświetlić dane z bazy danych w formancie AdRotator, dodać formantu źródła danych do strony, skonfigurować parametry połączenia dla formantu źródła danych wskazywał bazy danych i ustawianie formantu AdRotator **DataSourceID** Właściwość ID formantu źródła danych. W przypadkach, gdy istnieje potrzeba programowe Konfigurowanie AdRotator reklam przy użyciu zdarzenia AdCreated. Zdarzenie AdCreated przyjmuje dwa parametry; jeden obiekt, a inne wystąpienia AdCreatedEventArgs. AdCreatedEventArgs jest odwołanie do usługi ad, która jest tworzona.

Poniższy fragment kodu ustawia ImageUrl, NavigateUrl i AlternateText ad programowo:

[!code-csharp[Main](data-bound-controls/samples/sample3.cs)]

## <a name="list-controls"></a>Formanty listy

Formanty listy obejmują ListBox, DropDownList, CheckBoxList, RadioButtonList i listy BulletedList. Każdy z tych kontrolek może być danymi powiązanymi ze źródłem danych. Użyj jedno pole w źródle danych jako tekst wyświetlany, a drugie pole Opcjonalnie można użyć jako wartości elementu. Elementy można również dodać statycznie w czasie projektowania i można łączyć elementów statycznych i dynamicznych elementów dodanych ze źródła danych.

Dane bind formant listy, na stronie Dodaj formant źródła danych. Określ polecenie SELECT do kontroli źródła danych, a następnie ustaw właściwości DataSourceID kontrolki listy identyfikator formantu źródła danych. Użyj **DataTextField** i **DataValueField** właściwości, aby zdefiniować wyświetlany tekst i wartość dla formantu. Ponadto można użyć **DataTextFormatString** właściwości, aby sterować wyglądem wyświetlania tekstu w następujący sposób:

| **Expression** | **Opis** |
| --- | --- |
| Cena: {0: c} | Aby uzyskać dane liczbowe/dziesiętnych. Wyświetla literału "Cena:" następuje liczb w formacie waluty. Formacie waluty zależy od ustawienia kultury określonej w atrybucie kultury na **strony** dyrektywy lub w pliku Web.config. |
| {0:D4} | Dla danych liczb całkowitych. Nie można używać z liczb dziesiętnych. Liczby całkowite są wyświetlane w dopełniane zero pola, które jest cztery znaki dwubajtowe. |
| {0:N2}% | Aby uzyskać dane liczbowe. Wyświetla liczbę o 2 dziesiętnego dokładności następuje literał "%". |
| {0:000.0} | Aby uzyskać dane liczbowe/dziesiętnych. Numery są zaokrąglane do jednego miejsca dziesiętnego. Liczby mniejsze niż trzech cyfr. dopełniane zero. |
| {0:D} | Dla danych daty/godziny. Format daty długiej Wyświetla ("czwartek 06 sierpnia 1996"). Format daty zależy od ustawienia kulturowe strony lub pliku Web.config. |
| {0:d} | Dla danych daty/godziny. Data krótka Wyświetla formatu ("12/31/99"). |
| {0:yy-MM-dd} | Dla danych daty/godziny. Wyświetla datę w formacie liczbowym rok, miesiąc, dzień (96-08-06) |

## <a name="gridview"></a>GridView

Formant widoku GridView umożliwia wyświetlanie danych tabelarycznych i edytowanie za pomocą metody deklaratywne i zastępuje formantu DataGrid. Następujące funkcje są dostępne w kontrolce GridView.

- Powiązanie z danymi kontroli źródła, takich jak SqlDataSource.
- Wbudowane funkcje sortowania.
- Wbudowane aktualizowania i usuwania możliwości.
- Wbudowane funkcje stronicowania.
- Możliwości wyboru wbudowanych wiersza.
- Programowy dostęp do modelu obiektu GridView dynamicznie ustawiania właściwości, obsługi zdarzeń i tak dalej.
- Używanie wielu pól kluczy.
- Wiele pól danych dla kolumny hiperłącza.
- Można dostosować wygląd za pośrednictwem kompozycje i style.

**Pola kolumn**

Każdej kolumny w widoku GridView kontrolki jest reprezentowana przez obiekt DataControlField. Domyślnie ma ustawioną właściwość AutoGenerateColumns miała właściwości **true**, która tworzy obiekt AutoGeneratedField dla każdego pola w źródle danych. Każde pole jest następnie renderowane jako kolumny w widoku GridView formantu w kolejności, w wyświetlonym każde pole w źródle danych. Można również ręcznie kontrolować kolumny, które pola są wyświetlane w **widoku GridView** kontroli przez ustawienie **właściwość AutoGenerateColumns miała** właściwości **false** , a następnie własne Kolekcja pola kolumny. Typy pól innej kolumny określają zachowanie kolumn w formancie.

W poniższej tabeli wymieniono typy pól innej kolumny, które mogą być używane.

| **Typ pola kolumn** | **Opis** |
| --- | --- |
| Pole BoundField | Wyświetla wartość pola w źródle danych. Jest to domyślny typ kolumny formantu widoku GridView. |
| ButtonField | Przedstawia przycisk polecenia dla każdego elementu w kontrolce GridView. Dzięki temu można utworzyć kolumny formantów przycisk niestandardowe, takie jak dodawanie lub przycisk Usuń. |
| Pole CheckBoxField | Wyświetla pole wyboru dla każdego elementu w kontrolce GridView. Ten typ pola kolumny często jest używany do wyświetlania pola z wartością logiczną. |
| CommandField | Wyświetla wstępnie zdefiniowane przyciski poleceń do wykonania, wybierając, edytowanie lub usuwanie operacji. |
| Pole hiperłącza HyperLinkField | Wyświetla wartość pola w źródle danych jako hiperłącze. Ten typ pola kolumny umożliwia tworzenie powiązań drugie pole do adresu URL hiperłącza. |
| ImageField | Wyświetla obraz dla każdego elementu w kontrolce GridView. |
| Pole TemplateField | Wyświetla zawartość zdefiniowane przez użytkownika dla każdego elementu w kontrolce GridView zgodnie z określonego szablonu. Ten typ pola kolumny służy do tworzenia pole niestandardowe kolumny. |

Aby zdefiniować deklaratywnie kolekcji pól kolumn, najpierw dodać otwierające i zamykające **&lt;kolumn&gt;** tagi pomiędzy otwierającym, a zamykającym tagiem kontrolki widoku siatki. Następnie na liście pól kolumn, które chcesz uwzględnić między otwarcia i zamknięcia **&lt;kolumn&gt;** tagów. Kolumny określone są dodawane do kolekcji kolumn w podanej kolejności. **Kolumn** kolekcji przechowywane są wszystkie kolumny pola w formancie i można programowo zarządzać pola kolumn w kontrolce GridView.

Pola jawnie zadeklarowana kolumn mogą być wyświetlane w połączeniu z kolumny automatycznie wygenerowanego pola. Gdy są używane, jawnie zadeklarowane kolumny pola mają być renderowane najpierw następuje pola automatycznie generowanych kolumn.

## <a name="binding-to-data"></a>Wiązanie z danymi

Kontrolki widoku siatki może być powiązana z kontrolą źródła danych (takich jak **SqlDataSource**, **ObjectDataSource**i tak dalej), oraz jak dowolnego źródła danych, który implementuje System.Collections.IEnumerable Interfejs (na przykład System.Data.DataView, System.Collections.ArrayList lub System.Collections.Hashtable). Do wiązania kontrolki GridView typowi źródła danych, użyj jednej z następujących metod:

- Aby powiązać z kontroli źródła danych, ustawioną właściwość DataSourceID formantu widoku GridView wartości Identyfikatora formantu źródła danych. Kontrolki widoku siatki wiąże określone dane kontroli źródła i automatycznie korzystać ze źródła danych funkcje formantu, aby wykonać sortowania, aktualizowanie, usuwanie i funkcje stronicowania. Jest to preferowana metoda można powiązać z danymi.
- Aby powiązać ze źródłem danych, który implementuje interfejs System.Collections.IEnumerable, programowo ustaw właściwość formantu widoku GridView ze źródłem danych, a następnie wywołaj metodę DataBind. Przy użyciu tej metody, kontrolki widoku siatki nie zawiera wbudowane sortowanie, aktualizowanie, usuwanie i stronicowania funkcji. Należy podać tę funkcję, samodzielnie.

## <a name="data-operations"></a>Operacje na danych

Kontrolki widoku siatki udostępnia wiele wbudowanych możliwości, które umożliwiają użytkownikom sortowania, aktualizacji, usuwania, wybierz i przeglądania elementów w formancie. Gdy formant widoku GridView jest powiązana z kontroli źródła danych, kontrolki widoku siatki można wykorzystać źródła danych funkcje formantu i zapewnić automatyczne sortowanie, aktualizowanie i usuwanie funkcji.

> [!NOTE]
> Kontrolki widoku siatki można zapewnić obsługę sortowania, aktualizowanie i usuwanie z innych typów źródeł danych; jednak należy zapewnić program obsługi zdarzeń odpowiedniej implementacji dla tych operacji.


Sortowanie pozwala użytkownikowi do sortowania elementów w kontrolce GridView w odniesieniu do określonej kolumny przez kliknięcie nagłówka kolumny. Aby włączyć sortowanie, ustaw właściwość AllowSorting **true**.

Funkcje automatyczne aktualizowanie, usuwanie i zaznaczenia jest włączone, gdy przycisk w **ButtonField** lub **TemplateField** pole kolumny o nazwie polecenia "Edytuj", "Delete" i "Wybierz" odpowiednio zostanie kliknięty. Kontrolki widoku siatki można automatycznie dodać **CommandField** kolumny pola edycji, usuwania lub przycisk wyboru, jeśli właściwość AutoGenerateEditButton, AutoGenerateDeleteButton lub AutoGenerateSelectButton jest ustawiona na **true**odpowiednio.

> [!NOTE]
> Wstawianie rekordów do źródła danych nie jest bezpośrednio obsługiwana przez formant widoku GridView. Jednak jest możliwe do wstawiania rekordów za pomocą kontrolki widoku siatki w połączeniu z widoku DetailsView lub formant FormView.


Zamiast wyświetlanie wszystkie rekordy w źródle danych, w tym samym czasie, kontrolki widoku siatki można automatycznie podzielić rekordy na strony. Aby włączyć stronicowanie, ustaw właściwość właściwość AllowPaging **true**.

## <a name="customizing-the-user-interface"></a>Personalizowanie interfejsu użytkownika

Można dostosować wygląd formantu widoku GridView przez ustawienie właściwości stylu dla różnych części kontrolki. W poniższej tabeli wymieniono właściwości inny styl.

| **Właściwości stylu** | **Opis** |
| --- | --- |
| AlternatingRowStyle | Ustawienia stylu dla przemiennych wierszy danych w kontrolce GridView. Gdy ta właściwość jest ustawiona, wiersze danych są wyświetlane przemian ustawienia RowStyle i **AlternatingRowStyle** ustawienia. |
| EditRowStyle | Ustawienia stylu dla wiersza edytowanej w kontrolce GridView. |
| EmptyDataRowStyle | Ustawienia stylu dla pustym wierszu danych wyświetlanych w formancie GridView, gdy źródło danych nie zawiera żadnych rekordów. |
| FooterStyle | Ustawienia stylu dla wierszy stopkę formantu widoku GridView. |
| HeaderStyle | Ustawienia stylu dla formantu widoku GridView wiersz nagłówka. |
| PagerStyle | Ustawienia stylu dla wierszy pagera formantu widoku GridView. |
| RowStyle | Ustawienia stylu dla wierszy danych w kontrolce GridView. Gdy **AlternatingRowStyle** właściwość również jest ustawiona, wiersze danych są wyświetlane przemian **RowStyle** ustawienia i **AlternatingRowStyle** ustawienia. |
| SelectedRowStyle | Ustawienia stylu dla wybranego wiersza w kontrolce GridView. |

Możesz również wyświetlić lub ukryć różnych części kontrolki. W poniższej tabeli wymieniono właściwości, które kontrolują, które fragmenty są pokazywane lub ukryte.

| **Właściwość** | **Opis** |
| --- | --- |
| ShowFooter | Pokazuje lub ukrywa stopce kontrolki widoku siatki. |
| ShowHeader | Pokazuje lub ukrywa sekcji nagłówka kontrolki widoku siatki. |

### <a name="events"></a>Zdarzenia

Kontrolki widoku siatki udostępnia kilka zdarzeń, które można zaprogramować przed. Pozwala na uruchamianie procedury niestandardowe, przy każdym wystąpieniu zdarzenia. W poniższej tabeli wymieniono zdarzenia obsługiwane przez formant widoku GridView.

| **Zdarzenia** | **Opis** |
| --- | --- |
| PageIndexChanged | Występuje, gdy jeden z przycisków modułu stronicowania zostanie kliknięty, ale po kontrolki widoku siatki obsługuje stronicowanie. To zdarzenie to powszechnie używane, gdy trzeba wykonać zadanie po użytkownik przechodzi do innej strony w formancie. |
| PageIndexChanging | Występuje, gdy jeden z przycisków modułu stronicowania zostanie kliknięty, ale przed widoku GridView formantu obsługuje stronicowanie. To zdarzenie jest często używane do anulowania operacji stronicowania. |
| RowCancelingEdit | Występuje po kliknięciu przycisku Anuluj wiersza, ale przed kontrolki widoku siatki zamyka tryb edycji. To zdarzenie jest często używane, aby zatrzymać operację anulowania. |
| RowCommand | Występuje, gdy przycisk zostanie kliknięty w kontrolce GridView. To zdarzenie jest często używany do wykonywania zadań, gdy przycisk zostanie kliknięty w formancie. |
| RowCreated | Występuje po utworzeniu nowego wiersza w kontrolce GridView. To zdarzenie jest często używane do modyfikowania zawartości wiersza, podczas tworzenia wiersza. |
| RowDataBound | Występuje, gdy wiersz danych jest powiązany z danymi w kontrolce GridView. To zdarzenie jest często używane do modyfikowania zawartości wiersza, gdy wiersz jest powiązany z danymi. |
| RowDeleted | Występuje, gdy zostanie kliknięty przycisk usuwania wiersza, ale po kontrolki widoku siatki usuwa rekord ze źródła danych. To zdarzenie jest często używana do sprawdzania wyniki operacji usuwania. |
| RowDeleting | Występuje, gdy zostanie kliknięty przycisk usuwania wiersza, ale przed widoku GridView kontroli usuwa rekord ze źródła danych. To zdarzenie jest często używane, aby anulować operację usuwania. |
| RowEditing | Występuje, gdy zostanie kliknięty przycisk Edytuj wiersz, ale przed widoku GridView formant przechodzi do trybu edycji. To zdarzenie jest często używane, aby anulować operację edycji. |
| RowUpdated | Występuje po kliknięciu przycisku Aktualizuj wiersza, ale po kontrolki widoku siatki aktualizacji wiersza. To zdarzenie jest często używana do sprawdzania wyników operacji aktualizacji. |
| RowUpdating | Występuje po kliknięciu przycisku Aktualizuj wiersza, ale przed widoku GridView kontroli aktualizuje wiersza. To zdarzenie jest często używane do anulowania operacji aktualizacji. |
| SelectedIndexChanged | Występuje po kliknięciu przycisku Wybierz wiersza, ale po operacji select obsługuje kontrolki widoku siatki. To zdarzenie jest często używany do wykonywania zadań, po wybraniu wiersza w formancie. |
| SelectedIndexChanging | Występuje, gdy zostanie kliknięty przycisk Wybierz wiersza, ale przed widoku GridView kontroli obsługi operacji select. To zdarzenie jest często używane do anulowania operacji wyboru. |
| Sortowane | Występuje po kliknięciu hiperłącze, aby posortować kolumnę, ale po kontrolki widoku siatki obsługi operacji sortowania. To zdarzenie jest najczęściej używany do wykonywania zadań po użytkownik klika hiperłącze, aby posortować kolumnę. |
| Sortowanie | Występuje, gdy zostanie kliknięty hiperłącze, aby posortować kolumnę, ale przed widoku GridView kontroli obsługi operacji sortowania. To zdarzenie jest często używane, aby anulować operację sortowania lub wykonać niestandardowe procedury sortowania. |

## <a name="formview"></a>FormView

Formant FormView służy do wyświetlania pojedynczego rekordu ze źródła danych. Jest on podobny do formantu widoku DetailsView, z wyjątkiem Wyświetla zdefiniowane przez użytkownika szablonów, zamiast pola wiersza. Tworzenie własnych szablonów zapewnia większą elastyczność kontroli, jak dane są wyświetlane. Formant FormView obsługuje następujące funkcje:

- Powiązanie z danymi kontroli źródła, takie jak SqlDataSource i ObjectDataSource.
- Wbudowane możliwości Wstawianie.
- Wbudowane aktualizowania i usuwania możliwości.
- Wbudowane funkcje stronicowania.
- Programowy dostęp do modelu obiektu FormView dynamicznie ustawiania właściwości, obsługi zdarzeń i tak dalej.
- Można dostosować wygląd za pomocą szablonów zdefiniowanych przez użytkownika, kompozycje i style.

## <a name="templates"></a>Szablony

Kontrolki widoku FormView do wyświetlania zawartości należy utworzyć szablony dla różnych części kontrolki. Większość szablonów jest opcjonalna. jednak należy utworzyć szablon dla tryb, w którym skonfigurowano formantu. Na przykład FormView formant, który obsługuje Wstawianie rekordów musi mieć zdefiniowanego szablonu elementu insert. W poniższej tabeli wymieniono różne szablony, które można utworzyć.

| **Typ szablonu** | **Opis** |
| --- | --- |
| EditItemTemplate | Definiuje zawartość wiersza danych, gdy formant FormView jest w trybie edycji. Ten szablon zawiera zwykle kontrolki wejściowe i przyciski poleceń, z którą użytkownik może edytować istniejącego rekordu. |
| EmptyDataTemplate | Określa zawartość pustym wierszu danych wyświetlane, gdy kontrolka FormView jest powiązana ze źródłem danych, która nie zawiera żadnych rekordów. Ten szablon zawiera zwykle zawartości, aby ostrzec użytkownika, że źródło danych nie zawiera żadnych rekordów. |
| Elementu FooterTemplate | Określa zawartość wiersza stopki. Ten szablon zawiera zwykle dodatkowej zawartości, którą chcesz wyświetlić w wierszu stopki. Alternatywnie można po prostu określić tekst do wyświetlenia w wierszu stopki przez ustawienie właściwości FooterText. |
| Właściwość HeaderTemplate | Określa zawartość nagłówka wiersza. Ten szablon zawiera zwykle dodatkowej zawartości, którą chcesz wyświetlić w wierszu nagłówka. Alternatywnie można po prostu określić tekst do wyświetlenia w wierszu nagłówka, ustawiając właściwość HeaderText. |
| ItemTemplate | Definiuje zawartość wiersza danych, gdy formant FormView jest w trybie tylko do odczytu. Ten szablon zawiera zwykle zawartości do wyświetlania wartości istniejącego rekordu. |
| InsertItemTemplate | Definiuje zawartość wiersza danych, gdy formant FormView jest w trybie wstawiania. Ten szablon zawiera zwykle kontrolki wejściowe i przyciski poleceń, z którą użytkownik może dodać nowy rekord. |
| PagerTemplate | Definiuje zawartość wiersza pagera wyświetlane, gdy jest włączona funkcja stronicowania (Jeśli właściwość AllowPaging właściwości jest równa **true**). Ten szablon zawiera zazwyczaj formanty, z którymi użytkownik można przejść do innego rekordu. |

Kontrolki wejściowe w pliku szablonu elementu edycji i Wstaw szablon elementu może być powiązana z polami źródła danych przy użyciu wyrażenia powiązania dwukierunkowego. Umożliwia to kontrolę FormView automatycznie Wyodrębnij wartości kontrolki wprowadzania aktualizacji lub operacji wstawiania. Wiązanie dwukierunkowe wyrażenia również umożliwić kontrolki wejściowe w edycji szablonu elementów do wyświetlenia automatycznie oryginalnej wartości pól.

### <a name="binding-to-data"></a>Wiązanie z danymi

Formant FormView może być powiązany do kontroli źródła danych (takich jak **SqlDataSource**, AccessDataSource, **ObjectDataSource** i tak dalej), lub do dowolnego źródła danych, który implementuje Interfejsu System.Collections.IEnumerable (na przykład System.Data.DataView, System.Collections.ArrayList i System.Collections.Hashtable). Do wiązania kontrolki FormView typowi źródła danych, użyj jednej z następujących metod:

- Aby powiązać z kontroli źródła danych, ustawioną właściwość DataSourceID formantu FormView wartości Identyfikatora formantu źródła danych. Formant FormView wiąże określone dane kontroli źródła i automatycznie korzystać ze źródła danych funkcje formantu do wykonania, wstawianie, aktualizowanie, usuwanie i funkcje stronicowania. Jest to preferowana metoda można powiązać z danymi.
- Aby powiązać ze źródłem danych, który implementuje **System.Collections.IEnumerable** interfejsu programowo ustaw właściwość formantu FormView ze źródłem danych, a następnie wywołaj metodę DataBind. Przy użyciu tej metody, formancie FormView nie zawiera wbudowane Wstawianie, aktualizowanie, usuwanie i funkcje stronicowania. Musisz podać te funkcje przy użyciu odpowiedniego zdarzenia.

## <a name="data-operations"></a>Operacje na danych

Formant FormView udostępnia wiele wbudowanych możliwości, które umożliwiają użytkownikom aktualizacji, usuwanie, wstawić i przeglądania elementów w formancie. Gdy kontrolka FormView jest powiązana z kontroli źródła danych, kontroli FormView można wykorzystać źródła danych funkcje formantu i zapewnić automatyczne aktualizowanie, usuwanie, wstawiania i stronicowania funkcji. Formant FormView można zapewnić obsługę update, delete, insert i operacji stronicowania z innych typów źródeł danych; jednak należy podać program obsługi zdarzeń odpowiednie z implementacją tych operacji.

Ponieważ w formancie FormView korzysta z szablonów, nie ma sposób automatycznego generowania przyciski poleceń do wykonania, aktualizowanie, usuwanie i wstawianie operacji. Należy ręcznie dołączyć te przyciski poleceń w odpowiedni szablon. Formant FormView rozpoznaje niektórych przycisków, które mają ich **CommandName** właściwości mają określone wartości. W poniższej tabeli wymieniono przycisków poleceń, które rozpoznaje formancie FormView.

| **Przycisk** | **Wartość CommandName** | **Opis** |
| --- | --- | --- |
| Anuluj | "Cancel" | Używane w aktualizacji lub operacji wstawiania, aby anulować operację i odrzucić wartości wprowadzonej przez użytkownika. Formant FormView następnie powraca do trybu określoną przez właściwość właściwości DefaultMode. |
| Usuwanie | "Delete" | Używane w operacji usuwania wyświetlanych usunięty ze źródła danych. Informuje o zdarzeniach ItemDeleting i ItemDeleted. |
| Edytowanie | "Edit" | Używane w aktualizacji operacji put formancie FormView w trybie edycji. Zawartość określona w **EditItemTemplate** właściwość jest wyświetlana dla wiersza danych. |
| Insert | "Insert" | Używane w operacji wstawiania, aby wstawić nowy rekord w źródle danych, używając wartości podanego przez użytkownika. Informuje o zdarzeniach ItemInserting i ItemInserted. |
| New | "New" | Używane w Wstawianie operacje put w trybie wstawiania w formancie FormView. Zawartość określona w **InsertItemTemplate** właściwość jest wyświetlana dla wiersza danych. |
| Strona | "Page" | Używana w operacjach stronicowania do reprezentowania przycisk w wierszu pagera, który wykonuje stronicowania. Aby określić stronicowanie, ustaw **CommandArgument** właściwość przycisk "Dalej", "Prev", "Pierwszy", "Ostatni" lub indeks strony, do którego należy przejść. Informuje o zdarzeniach PageIndexChanging i PageIndexChanged. |
| Aktualizacja | "Update" | Używane podczas operacji aktualizowania aby podjąć próbę zaktualizowania rekordu wyświetlanych w źródle danych przy użyciu wartości podanego przez użytkownika. Informuje o zdarzeniach ItemUpdating i ItemUpdated. |

W przeciwieństwie do usunięcia przycisku (które powoduje usunięcie rekordu wyświetlany natychmiast), po kliknięciu przycisku Edytuj lub nowy FormView formant przechodzi do edycji lub tryb wstawiania odpowiednio. W trybie edycji zawartości znajdujących się w **EditItemTemplate** właściwość jest wyświetlana dla bieżącego elementu danych. Zazwyczaj Edytuj szablon elementu jest zdefiniowany tak, aby przycisk Edytuj jest zastępowany aktualizację, a przycisk Anuluj. Wartość pola użytkownik może zmodyfikować również zazwyczaj są wyświetlane kontrolki wejściowe, które są odpowiednie dla typu danych w polu (np. pola tekstowego lub kontrolkę pola wyboru). Kliknięcie przycisku Aktualizuj aktualizacji rekordu w źródle danych, a kliknięcie przycisku Anuluj odstępuje wszelkie zmiany.

Podobnie, zawartość w **InsertItemTemplate** wyświetlane właściwości elementu danych, gdy formant jest w trybie wstawiania. Wstaw szablon elementu jest zazwyczaj definiowane w taki sposób, że nowy przycisk jest zastępowany Insert i przycisk Anuluj, a pusty kontrolki wejściowe są wyświetlane dla użytkownika o wprowadzenie wartości w nowym rekordzie. Kliknięcie przycisku Wstaw w źródle danych wstawia rekordu, a kliknięcie przycisku Anuluj odstępuje wszelkie zmiany.

Kontrola FormView zapewnia funkcji stronicowania, który umożliwia użytkownikowi nawigację do innych rekordów w źródle danych. Po włączeniu wiersza pagera jest wyświetlany w formancie FormView, który zawiera formanty nawigacji strony. Aby włączyć stronicowanie, ustaw **właściwość AllowPaging** właściwości **true**. Przez ustawienie właściwości obiektów zawartych w PagerStyle i właściwości PagerSettings można dostosować wiersza modułu stronicowania. Zamiast używać wiersza pagera wbudowanego interfejsu użytkownika, można tworzyć własnego interfejsu użytkownika przy użyciu **PagerTemplate** właściwości.

## <a name="customizing-the-user-interface"></a>Personalizowanie interfejsu użytkownika

Przez ustawienie właściwości stylu dla różnych części formantu można dostosować wygląd formantu FormView. W poniższej tabeli wymieniono właściwości inny styl.

| **Właściwości stylu** | **Opis** |
| --- | --- |
| EditRowStyle | Ustawienia stylu dla wiersza danych, gdy formant FormView jest w trybie edycji. |
| EmptyDataRowStyle | Ustawienia stylu dla pustym wierszu danych wyświetlanych w formancie FormView, gdy źródło danych nie zawiera żadnych rekordów. |
| FooterStyle | Ustawienia stylu dla wierszy stopkę formantu FormView. |
| HeaderStyle | Ustawienia stylu dla wiersza nagłówka w formancie FormView. |
| InsertRowStyle | Ustawienia stylu dla wiersza danych, gdy formant FormView jest w trybie wstawiania. |
| PagerStyle | Ustawienia stylu dla wierszy pagera wyświetlanych w formancie FormView po włączeniu funkcji stronicowania. |
| RowStyle | Ustawienia stylu dla wiersza danych, gdy formant FormView jest w trybie tylko do odczytu. |

## <a name="events"></a>Zdarzenia

Kontrola FormView zapewnia kilka zdarzeń, które można zaprogramować przed. Pozwala na uruchamianie procedury niestandardowe, przy każdym wystąpieniu zdarzenia. W poniższej tabeli wymieniono zdarzenia jest obsługiwana przez kontrolkę FormView.

| **Zdarzenia** | **Opis** |
| --- | --- |
| ItemCommand | Występuje, gdy kliknięto przycisk w formancie FormView. To zdarzenie jest często używany do wykonywania zadań, gdy przycisk zostanie kliknięty w formancie. |
| ItemCreated | Występuje po utworzeniu wszystkie obiekty FormViewRow w formancie FormView. To zdarzenie jest często używane do modyfikowania wartości rekordu, przed wyświetleniem. |
| ItemDeleted | Występuje, gdy przycisk Usuń (przycisk z jego **CommandName** właściwość "Delete") zostanie kliknięty, ale po formancie FormView usuwa rekord ze źródła danych. To zdarzenie jest często używana do sprawdzania wyniki operacji usuwania. |
| ItemDeleting | Występuje, gdy zostanie kliknięty przycisk Usuń, ale przed FormView kontroli usuwa rekord ze źródła danych. To zdarzenie jest często używane, aby anulować operację usuwania. |
| ItemInserted | Występuje, gdy przycisk Wstaw (przycisk z jego **CommandName** właściwość "Insert") zostanie kliknięty, ale po formancie FormView wstawia rekordu. To zdarzenie jest często używana do sprawdzania wyniki operacji insert. |
| ItemInserting | Występuje, gdy zostanie kliknięty przycisk Wstaw, ale przed FormView kontroli wstawia rekordu. To zdarzenie jest często używane do anulowania operacji insert. |
| ItemUpdated | Występuje, gdy przycisk Aktualizuj (przycisk z jego **CommandName** właściwość "Update") zostanie kliknięty, ale po formancie FormView aktualizacji wiersza. To zdarzenie jest często używana do sprawdzania wyników operacji aktualizacji. |
| ItemUpdating | Występuje, gdy zostanie kliknięty przycisk Aktualizuj, ale przed FormView kontroli aktualizacji rekordu. To zdarzenie jest często używane do anulowania operacji aktualizacji. |
| ModeChanged | Występuje po formancie FormView zmian tryby (w celu edycji, wstawiania lub w trybie tylko do odczytu). To zdarzenie jest często używane do wykonywania zadań zmiany tryby w formancie FormView. |
| ModeChanging | Występuje przed formancie FormView zmienia tryby (w celu edycji, wstawiania lub w trybie tylko do odczytu). To zdarzenie jest często używana Anuluj zmiany trybu. |
| PageIndexChanged | Występuje, gdy jeden z przycisków modułu stronicowania zostanie kliknięty, ale po formancie FormView obsługuje stronicowanie. To zdarzenie to powszechnie używane, gdy trzeba wykonać zadanie po użytkownik przechodzi do innego rekordu w formancie. |
| PageIndexChanging | Występuje, gdy jeden z przycisków modułu stronicowania zostanie kliknięty, ale przed FormView formantu obsługuje stronicowanie. To zdarzenie jest często używane do anulowania operacji stronicowania. |

## <a name="detailsview"></a>Widoku DetailsView

Formant widoku DetailsView jest używany do wyświetlania pojedynczego rekordu ze źródła danych w tabeli, którym każdego pola rekordu jest wyświetlana w wierszu tabeli. Można można użyć w połączeniu z kontrolki widoku siatki w scenariuszach główny szczegółowy. Formant widoku DetailsView obsługuje następujące funkcje:

- Powiązanie z danymi kontroli źródła, takich jak SqlDataSource.
- Wbudowane możliwości Wstawianie.
- Wbudowane aktualizowania i usuwania możliwości.
- Wbudowane funkcje stronicowania.
- Programowy dostęp do widoku DetailsView modelu obiektu dynamicznie ustawiania właściwości, obsługi zdarzeń i tak dalej.
- Można dostosować wygląd za pośrednictwem kompozycje i style.

## <a name="row-fields"></a>Pola wierszy

Każdy wiersz danych w formancie widoku DetailsView jest tworzony przez zadeklarowanie formantu pola. Typy pól innego wiersza określają zachowanie wierszy w formancie. Formanty pól pochodzić od DataControlField. W poniższej tabeli wymieniono typy pól innego wiersza, które mogą być używane.

| **Typ pola kolumn** | **Opis** |
| --- | --- |
| Pole BoundField | Wyświetla wartość pola w źródle danych jako tekst. |
| ButtonField | Przedstawia przycisk polecenia w kontrolce widoku DetailsView. Dzięki temu można pobierać za pomocą formantu przycisku niestandardowe, takie jak dodawanie lub przycisk Usuń. |
| Pole CheckBoxField | Wyświetla pole wyboru w formancie widoku DetailsView. Ten typ pola wiersza często służy do wyświetlania pola z wartością logiczną. |
| CommandField | Wyświetla wbudowanego polecenia przycisków przeprowadzenie edycji, wstawiania lub usuwania operacji w kontrolce widoku DetailsView. |
| Pole hiperłącza HyperLinkField | Wyświetla wartość pola w źródle danych jako hiperłącze. Ten typ pola wiersza, umożliwia tworzenie powiązań drugie pole do adresu URL hiperłącza. |
| ImageField | Wyświetla obraz w formancie widoku DetailsView. |
| Pole TemplateField | Wyświetla zawartość zdefiniowane przez użytkownika dla wiersza w kontrolce widoku DetailsView zgodnie z określonego szablonu. Ten typ pola wiersza, umożliwia tworzenie pole niestandardowe wiersza. |

Domyślnie ma ustawioną właściwość AutoGenerateRows **true**, który automatycznie generuje obiekt pola powiązanego wiersza dla każdego pola, które można powiązać typu źródła danych. Prawidłowe typy powiązania to ciąg, DateTime, Decimal, identyfikator Guid i zestawu typów pierwotnych. Każde pole jest następnie wyświetlane w wierszu jako tekst w kolejności, w której każde pole pojawia się w źródle danych.

Automatyczne generowanie wiersze zapewnia szybki i łatwy sposób wyświetlania każdego pola w rekordzie. Aby użyć widoku DetailsView formantu zaawansowanych funkcji, musisz jawnie zadeklarować pól wierszy do uwzględnienia w kontrolce widoku DetailsView. Aby zadeklarować pól wierszy, najpierw ustaw **AutoGenerateRows** właściwości **false**. Następnie dodaj otwierające i zamykające **&lt;pola&gt;** tagi pomiędzy otwierającym, a zamykającym tagiem formantu widoku DetailsView. Na koniec listy pól wierszy, które chcesz uwzględnić między otwarcia i zamknięcia **&lt;pola&gt;** tagów. Określone pola wiersza są dodawane do kolekcji pól w podanej kolejności. **Pola** kolekcji umożliwia programowego zarządzania pola wiersza w kontrolce widoku DetailsView.

> [!NOTE]
> Automatycznie generowane pola nie są dodawane do kolekcji pól wiersza.


## <a name="binding-to-data"></a>Wiązanie z danymi

Formant widoku DetailsView może być powiązana do kontroli źródła danych, takich jak **SqlDataSource** lub AccessDataSource, lub do dowolnego źródła danych, który implementuje interfejs System.Collections.IEnumerable, takich jak System.Data.DataView, System.Collections.ArrayList i System.Collections.Hashtable.

Do wiązania kontrolki widoku DetailsView typowi źródła danych, użyj jednej z następujących metod:

- Aby powiązać z kontroli źródła danych, ustawioną właściwość DataSourceID formantu widoku DetailsView wartości Identyfikatora formantu źródła danych. Kontrolki widoku szczegółów automatycznie wiąże określone dane kontroli źródła. Jest to preferowana metoda można powiązać z danymi.
- Aby powiązać ze źródłem danych, który implementuje **System.Collections.IEnumerable** interfejsu programowo ustaw właściwość formantu widoku DetailsView ze źródłem danych, a następnie wywołaj metodę DataBind.

## <a name="security"></a>Zabezpieczenia

Ten formant może służyć do wyświetlania danych wejściowych użytkownika, która może obejmować skryptu złośliwego klienta. Sprawdź wszystkie informacje, które są wysyłane z klienta do pliku wykonywalnego skrypt, instrukcji SQL lub inny kod przed wyświetleniem go w aplikacji. Program ASP.NET udostępnia funkcję sprawdzania poprawności żądanie wejściowe do bloku skryptu, jak i HTML w danych wejściowych użytkownika.

## <a name="data-operations"></a>Operacje na danych

Kontrolki widoku szczegółów oferuje wbudowane funkcje, które umożliwiają użytkownikom aktualizacji, usuwanie, wstawić i przeglądania elementów w formancie. Gdy formant widoku DetailsView jest powiązana z kontroli źródła danych, formantu widoku DetailsView można wykorzystać źródła danych funkcje formantu i zapewnić automatyczne aktualizowanie, usuwanie, wstawiania i stronicowania funkcji.

Formant widoku DetailsView można zapewnić obsługę update, delete, insert i operacji stronicowania z innych typów źródeł danych; jednak musi zapewniać implementację dla tych operacji w obsłudze zdarzeń odpowiednie.

Formant widoku DetailsView może automatycznie dodać **CommandField** wiersza pola edycji, usuwania lub nowy przycisk przez ustawienie właściwości AutoGenerateEditButton, AutoGenerateDeleteButton lub AutoGenerateInsertButton do **true**odpowiednio. W przeciwieństwie do Usuń przycisk (który spowoduje usunięcie wybranego rekordu natychmiast), po kliknięciu przycisku Edytuj lub nowego widoku DetailsView formant przechodzi do edycji lub trybie, wstawiania odpowiednio. W trybie edycji przycisk Edytuj jest zastępowany aktualizację, a przycisk Anuluj. Wartość pola użytkownik może zmodyfikować są wyświetlane kontrolki wejściowe, które są odpowiednie dla typu danych w polu (np. pola tekstowego lub kontrolkę pola wyboru). Kliknięcie przycisku Aktualizuj aktualizacji rekordu w źródle danych, a kliknięcie przycisku Anuluj odstępuje wszelkie zmiany. Podobnie w trybie wstawiania nowy przycisk jest zastępowany Insert i przycisk Anuluj, a pusty kontrolki wejściowe są wyświetlane dla użytkownika o wprowadzenie wartości w nowym rekordzie.

Kontrolki widoku szczegółów zapewnia funkcji stronicowania, który umożliwia użytkownikowi nawigację do innych rekordów w źródle danych. Po włączeniu formantów nawigacji strony są wyświetlane w wierszu modułu stronicowania. Aby włączyć stronicowanie, ustaw właściwość właściwość AllowPaging **true**. Wiersz pagera można dostosować za pomocą właściwości PagerStyle i PagerSettings.

## <a name="customizing-the-user-interface"></a>Personalizowanie interfejsu użytkownika

Przez ustawienie właściwości stylu dla różnych części formantu można dostosować wygląd formantu widoku DetailsView. W poniższej tabeli wymieniono właściwości inny styl.

| **Właściwości stylu** | **Opis** |
| --- | --- |
| AlternatingRowStyle | Ustawienia stylu dla przemiennych wierszy danych w formancie widoku DetailsView. Gdy ta właściwość jest ustawiona, wiersze danych są wyświetlane przemian ustawienia RowStyle i **AlternatingRowStyle** ustawienia. |
| CommandRowStyle | Ustawienia stylu dla wierszy zawierających wbudowanego polecenia przycisków w formancie widoku DetailsView. |
| EditRowStyle | Ustawienia stylu dla wierszy danych, gdy formant widoku DetailsView jest w trybie edycji. |
| EmptyDataRowStyle | Ustawienia stylu dla pustym wierszu danych wyświetlanych w formancie widoku DetailsView, gdy źródło danych nie zawiera żadnych rekordów. |
| FooterStyle | Ustawienia stylu dla wierszy stopkę formantu widoku DetailsView. |
| HeaderStyle | Ustawienia stylu dla formantu widoku DetailsView wiersz nagłówka. |
| InsertRowStyle | Ustawienia stylu dla wierszy danych, gdy formant widoku DetailsView jest w trybie wstawiania. |
| PagerStyle | Ustawienia stylu dla wierszy pagera formantu widoku DetailsView. |
| RowStyle | Ustawienia stylu dla wierszy danych w formancie widoku DetailsView. Gdy **AlternatingRowStyle** właściwość również jest ustawiona, wiersze danych są wyświetlane przemian **RowStyle** ustawienia i **AlternatingRowStyle** ustawienia. |
| FieldHeaderStyle | Ustawienia styl nagłówka kolumny formantu widoku DetailsView. |

## <a name="events"></a>Zdarzenia

Formant widoku DetailsView udostępnia kilka zdarzeń, które można zaprogramować przed. Pozwala na uruchamianie procedury niestandardowe, przy każdym wystąpieniu zdarzenia. W poniższej tabeli wymieniono zdarzenia obsługiwane przez formant widoku DetailsView. Kontrolki widoku szczegółów dziedziczy także te zdarzenia z jej klas podstawowych: wiązania danych, z danymi usunięty, Init, Load, PreRender i renderowania.

| **Zdarzenia** | **Opis** |
| --- | --- |
| ItemCommand | Występuje, gdy przycisk zostanie kliknięty w formancie widoku DetailsView. |
| ItemCreated | Występuje, gdy wszystkie obiekty DetailsViewRow są tworzone w kontrolce widoku DetailsView. To zdarzenie jest często używane do modyfikowania wartości rekordu, przed wyświetleniem. |
| ItemDeleted | Występuje, gdy zostanie kliknięty przycisk Usuń, ale po formantu widoku DetailsView usuwa rekord ze źródła danych. To zdarzenie jest często używana do sprawdzania wyniki operacji usuwania. |
| ItemDeleting | Występuje, gdy zostanie kliknięty przycisk Usuń, ale przed widoku DetailsView kontroli usuwa rekord ze źródła danych. To zdarzenie jest często używane, aby anulować operację usuwania. |
| ItemInserted | Występuje, gdy zostanie kliknięty przycisk Wstaw, ale po formantu widoku DetailsView wstawia rekordu. To zdarzenie jest często używana do sprawdzania wyniki operacji insert. |
| ItemInserting | Występuje, gdy zostanie kliknięty przycisk Wstaw, ale przed widoku DetailsView kontroli wstawia rekordu. To zdarzenie jest często używane do anulowania operacji insert. |
| ItemUpdated | Występuje po kliknięciu przycisku Aktualizuj, ale po formantu widoku DetailsView aktualizacji wiersza. To zdarzenie jest często używana do sprawdzania wyników operacji aktualizacji. |
| ItemUpdating | Występuje, gdy zostanie kliknięty przycisk Aktualizuj, ale przed widoku DetailsView kontroli aktualizacji rekordu. To zdarzenie jest często używane do anulowania operacji aktualizacji. |
| ModeChanged | Występuje, gdy formant widoku DetailsView zmienia tryby (Edycja, wstawiania lub trybie tylko do odczytu). To zdarzenie jest często używane do wykonywania zadań tryby zmianie formantu widoku DetailsView. |
| ModeChanging | Występuje, zanim formant widoku DetailsView zmienia tryby (Edycja, wstawiania lub trybie tylko do odczytu). To zdarzenie jest często używana Anuluj zmiany trybu. |
| PageIndexChanged | Występuje, gdy jeden z przycisków modułu stronicowania zostanie kliknięty, ale po formantu widoku DetailsView obsługuje stronicowanie. To zdarzenie to powszechnie używane, gdy trzeba wykonać zadanie po użytkownik przechodzi do innego rekordu w formancie. |
| PageIndexChanging | Występuje, gdy jeden z przycisków modułu stronicowania zostanie kliknięty, ale przed widoku DetailsView formantu obsługuje stronicowanie. To zdarzenie jest często używane do anulowania operacji stronicowania. |

## <a name="the-menu-control"></a>Formant Menu

Formant Menu w programie ASP.NET 2.0 została zaprojektowana jako system kompletne nawigacji. Może być powiązany z danymi łatwo ze źródłami danych hierarchiczne, takich jak SiteMapDataSource.

Struktura kontrolek Menu można definiować deklaratywnie lub dynamicznie i składa się na jednym węźle głównym i dowolną liczbę węzłów podrzędnych. Poniższy kod definiuje deklaratywnie menu dla Menu formantu.

[!code-aspx[Main](data-bound-controls/samples/sample4.aspx)]

W powyższym przykładzie węzeł Home.aspx jest węzła głównego. Wszystkie inne węzły są zagnieżdżone w obrębie węzła głównego na różnych poziomach.

Istnieją dwa typy menu, które kontrolki Menu umożliwiający renderowanie; menu statycznych i dynamicznych menu. Menu statyczne składają się z elementów menu, które są zawsze widoczne. Menu dynamiczne składają się z elementów menu, które są widoczne tylko gdy użytkownik przesuwa się nad nimi przy użyciu myszy. Klienci mogą często należy mylić menu statyczne z menu zdefiniowane deklaratywnie i menu dynamiczne z menu, które są z danymi w czasie wykonywania. W rzeczywistości dynamiczną i statyczną menu są związane z metodą wypełniania. Warunki *statycznych* i *dynamiczne* odwoływać się tylko do tego, czy menu jest wyświetlane domyślnie czy tylko wyświetlane, gdy użytkownik wykona akcję.

**Wartości StaticDisplayLevels** właściwość jest używana do konfigurowania, ile poziomów w menu są statyczne i dlatego jest wyświetlane domyślnie. W powyższym przykładzie ustawienie **wartości StaticDisplayLevels** właściwości na wartość 2 spowodowałoby menu statycznie wyświetlić głównej węzła, węzeł utworów muzycznych i węzła filmów. Wszystkie inne węzły będzie wyświetlana dynamicznie, gdy użytkownik znajduje się nad węzła nadrzędnego.

**MaximumDynamicDisplayLevels** właściwość konfiguruje maksymalną liczbę poziomów dynamiczna jest umożliwiająca wyświetlanie menu. Wszelkie menu dynamiczne na poziomie wyższym niż wartość określoną przez **MaximumDynamicDisplayLevels** właściwości zostaną odrzucone.

> [!NOTE]
> Jest niemal pewność, że mogą wystąpić sytuacje, gdy menu nie są wyświetlane do renderowania z powodu właściwości MaximumDynamicDisplayLevels. W takich przypadkach upewnij się, że właściwość jest ustawiona wystarczająco, aby umożliwić wyświetlanie menu klientów.


## <a name="data-binding-the-menu-control"></a>Formant Menu powiązanie danych

Formant Menu może być powiązana do dowolnego źródła danych hierarchiczne, takie jak SiteMapDataSource lub formantu XMLDataSource. SiteMapDataSource jest najczęściej używane metody do wiązania danych formantu Menu go źródła poza pliku Web.sitemap, ponieważ jego schemat zawiera znane interfejsu API do kontrolki Menu. Lista poniżej zawiera prostego pliku Web.sitemap.

[!code-xml[Main](data-bound-controls/samples/sample5.xml)]

Należy zauważyć, że istnieje tylko jeden siteMapNode element główny, w tym przypadku element głównej. Dla każdego elementu siteMapNode można skonfigurować kilka atrybutów. Najczęściej używane atrybuty:

- **adres URL** Określa adres URL wyświetlany, gdy użytkownik kliknie elementu menu. Jeśli ten atrybut nie jest obecny, węzeł po prostu zostanie wysłany ponownie po kliknięciu.
- **Tytuł** Określa tekst, który jest wyświetlany w elemencie menu.
- **Opis elementu** użyty jako dokumentacji dla węzła. Również wyświetlane jako etykietka narzędzia, gdy wskaźnik myszy znajduje się myszy nad węzłem.
- **siteMapFile** zezwala na zagnieżdżone map. Ten atrybut musi wskazywać poprawnie sformułowany plik mapy witryny ASP.NET.
- **role** umożliwia wyglądu węzła kontrolowany przez przycinanie zabezpieczeń platformy ASP.NET.

Należy pamiętać, że te atrybuty są wszystkie opcjonalne, zachowania menu nie mogą być oczekiwano, jeśli nie określono. Na przykład jeśli *adres url* atrybut został określony, ale *opis* nie jest atrybutem, węzeł nie będzie widoczny i nie będzie można przejść na adres URL określony.

## <a name="controlling-a-menus-operation"></a>Kontrolowanie operacji menu

Istnieje kilka właściwości, które wpływają na działanie kontrolkę ASP.NET Menu; **orientacji** właściwość **DisappearAfter** właściwość **StaticItemFormatString** właściwości oraz **StaticPopoutImageUrl**właściwości to tylko niektóre z nich.

- **Orientacji** może być ustawiona jako *poziome* lub *pionowy* i określa, czy statycznych elementów menu są ułożone poziomo w wierszu, czy w pionie i skumulowany po siebie nawzajem. Ta właściwość nie ma wpływu na menu dynamiczne.
- **DisappearAfter** właściwość określa, jak długo menu dynamiczne powinny być widoczne, po myszy odsunąć od go. Wartość jest określona w milisekundach, a wartość domyślna to 500. Ustawienie tej właściwości na wartość -1 spowoduje, że nigdy nie znikają automatycznie w menu. W takim przypadku menu zniknie tylko gdy użytkownik kliknie poza menu.
- **StaticItemFormatString** właściwości ułatwia Obsługa spójny sposób wyrażania w systemie menu. Podczas określania tej właściwości *{0}* powinny być wprowadzane zamiast opis, który w źródle danych. Na przykład aby mieć element menu z powiedzieć ćwiczenie 1 odwiedź nasz strony produktów, itp., należy określić odwiedź stronę StaticItemFormatString {0}. W czasie wykonywania ASP.NET spowoduje zastąpienie wszelkich wystąpienie {0} prawidłowy opis elementu menu.
- **StaticPopoutImageUrl** właściwość określa obrazu, który służy do wskazywania, czy węzeł określonego menu ma węzły podrzędne, które mogą być udostępniane przez umieszczenie na nim wskaźnika. Menu dynamiczne będzie w dalszym ciągu korzystać z domyślnego obrazu.

## <a name="templated-menu-controls"></a>Formanty Menu opartego na szablonie

Formant Menu jest kontrolki z szablonem i pozwala na dwóch różnych elementów; StaticItemTemplate i DynamicItemTemplate. Za pomocą tych szablonów, można łatwo dodać kontrolki serwera lub kontrolki użytkownika do menu.

Aby edytować szablony w programie Visual Studio .NET, kliknij przycisk tagów inteligentnych menu i albo włączyć Edytuj szablony. Następnie można wybrać edycji StaticItemTemplate lub DynamicItemTemplate.

Wszystkie formanty dodane do StaticItemTemplate będą wyświetlane w menu statyczne podczas ładowania strony. Wszystkie formanty dodane do DynamicItemTemplate będą wyświetlane na wszystkich menu podręczne.

## <a name="menu-events"></a>Zdarzenia menu

Formant Menu ma dwa zdarzenia, które są unikatowe **MenuItemClicked** i **MenuItemDatabound** zdarzeń.

MenuItemClicked zdarzenie jest wywoływane po kliknięciu elementu menu. MenuItemDatabound zdarzenie jest wywoływane, gdy element menu jest powiązany z danymi. **MenuEventArgs** przekazanego do zdarzenia obsługi zapewnia dostęp do elementu menu za pomocą właściwości elementu.

## <a name="controlling-a-menus-appearance"></a>Kontrolowanie wyglądu menu

Można również na wygląd formantu menu przy użyciu co najmniej jednego z wielu stylów dostępne w menu format. Należą do nich **StaticMenuStyle**, **DynamicMenuStyle**, **DynamicMenuItemStyle**, **DynamicSelectedStyle**i **DynamicHoverStyle**. Te właściwości są konfigurowane przy użyciu standardowego ciągu stylu w kodzie HTML. Na przykład następujące wpłynie na styl menu dynamiczne.

[!code-aspx[Main](data-bound-controls/samples/sample6.aspx)]

> [!NOTE]
> Jeśli używasz Styl przesuwania, należy dodać &lt;head&gt; element na stronę z *runat* element ustawiony na wartość *serwera*.


Formanty menu obsługują również kompozycji ASP.NET 2.0.

## <a name="the-treeview-control"></a>TreeView — formant

TreeView — kontrolka wyświetla dane w strukturze drzewa. Podobnie jak w przypadku kontrolka Menu będzie można łatwo danymi powiązanymi z dowolnego źródła hierarchiczne dane, takie jak SiteMapDataSource.

Pierwsze pytanie, którego klienci mogą poprosić informacje o formancie TreeView w programie ASP.NET 2.0 jest, czy powiązany TreeView WebControl programu Internet Explorer, która była dostępna dla platformy ASP.NET 1.x. Nie jest. Kontrolki ASP.NET 2.0 TreeView został napisany od podstaw i oferuje znaczne ulepszenia w stosunku do TreeView WebControl programu Internet Explorer, która wcześniej była dostępna.

Nie można przejść do szczegółów jak powiązać TreeView — formant do mapy witryny, ponieważ jest wykonywane w taki sam sposób, jak kontrolka Menu. TreeView — kontrolka ma jednak pewne różnice distinct w taki sposób, który działa.

TreeView — kontrolka pojawia się domyślnie rozwinięty. Aby zmienić poziom rozwinięcia podczas ładowania początkowego, zmodyfikuj **ExpandDepth** właściwości formantu. Jest to szczególnie ważne w przypadku elementu TreeView z danymi po rozwinięciu określonych węzłów.

## <a name="databinding-the-treeview-control"></a>Powiązania danych w formancie TreeView

W przeciwieństwie do sterowania Menu element TreeView jest przydatna do obsługi dużych ilości danych. W związku z tym oprócz wiązania z danymi SiteMapDataSource lub elementu XMLDataSource, element TreeView jest często dane powiązane z zestawu danych lub innych danych relacyjnych. W przypadkach, gdy kontrolka TreeView jest powiązana z dużych ilości danych najlepiej powiązać tylko z danych, które są rzeczywiście widoczne w formancie. Mogą być następnie danych powiązać do dodatkowych danych, ponieważ są rozwinięte węzły elementu TreeView.

W takich przypadkach **PopulateOnDemand** powinien mieć ustawioną właściwość elementu TreeView *true*. Następnie musisz określić implementację **TreeNodePopulate** metody.

## <a name="data-binding-without-postback"></a>Powiązanie bez odświeżania danych

Zauważyć, że po rozwinięciu węzła w poprzednim przykładzie po raz pierwszy, strony powrotem zapisuje a odświeża. Thats nie ma problemu, w tym przykładzie, ale można Załóżmy, że może być w środowisku produkcyjnym z dużą ilością danych. Byłoby lepsze scenariusz w którym element TreeView będzie nadal dynamicznie wypełnić jego węzłów, ale bez post z powrotem do serwera.

Przez ustawienie **PopulateNodesFromClient** i **PopulateOnDemand** właściwości na wartość true, formantu ASP.NET TreeView będzie dynamicznie zapełniać węzły bez ogłaszanie zwrotne. Po rozwinięciu węzła nadrzędnego XMLHttp żądań od klienta i OnTreeNodePopulate zdarzenie jest wywoływane. Serwer odpowiada przy Wyspy danych XML, który jest następnie używany do danych powiązania węzłów podrzędnych.

Program ASP.NET tworzy dynamicznie kodu klienta, który implementuje tę funkcję. &lt;Skryptu&gt; tagi, które zawierają skrypt są generowane, wskazując plik AXD. Na przykład lista poniżej zawiera skrypt łącza dla kodu skryptu, który generuje żądanie XMLHttp.

[!code-html[Main](data-bound-controls/samples/sample7.html)]

Jeśli wyszukiwanie w pliku AXD powyżej w przeglądarce i otwórz go, zobaczysz kod, który implementuje XMLHttp żądania. Ta metoda uniemożliwia klientom modyfikowania pliku skryptu.

## <a name="controlling-the-operation-of-the-treeview-control"></a>Kontrolowanie operacji formantu TreeView

TreeView — kontrolka ma kilka właściwości, które wpływają na działanie formantu. Najbardziej oczywisty właściwości są **ShowCheckBoxes**, **ShowExpandCollapse**, i **ShowLines**.

**ShowCheckBoxes** właściwość ma wpływ na, czy węzły wyświetlane pole wyboru, gdy są renderowane. Prawidłowe wartości dla tej właściwości to **Brak**, **głównego**, **nadrzędnej**, **liścia**, i **wszystkich**. Wpływają one na formancie TreeView w następujący sposób:

| **Wartość właściwości** | **Effect** |
| --- | --- |
| Brak | Element CheckBoxes nie są wyświetlane na żadnych węzłów. To jest ustawienie domyślne. |
| główny | Pole wyboru jest wyświetlane tylko dla węzła głównego. |
| Nadrzędny | Pole wyboru jest wyświetlane tylko na tych węzłach, które mają węzłów podrzędnych. Te węzły podrzędne może być węzłów nadrzędnych lub węzłów liści. |
| Liścia | Pole wyboru jest wyświetlane tylko na tych węzłach, które mają bez węzłów podrzędnych. |
| Wszystkie | Pole wyboru jest wyświetlane we wszystkich węzłach. |

W przypadku używania pola wyboru **CheckedNodes** właściwość, którą będzie zwracać kolekcję TreeView węzłów, które są sprawdzane podczas ogłaszania zwrotnego.

**ShowExpandCollapse** właściwość określa wygląd obrazu Rozwiń/Zwiń obok węzłów głównych i nadrzędnej. Jeśli ta właściwość jest ustawiona na **false**, węzły elementu TreeView są renderowane jako hiperłącze i są rozwinięte zwinięte, klikając łącze.

**ShowLines** właściwość określa, czy są wyświetlane linie łączące węzły nadrzędne do węzłów podrzędnych. Gdy **false** (ustawienie domyślne), są wyświetlane żadne wiersze. Gdy **true**, TreeView — kontrolka będzie używać obrazów wierszy w folderze określonym przez **LineImagesFolder** właściwości.

Aby dostosować wygląd TreeView wiersze, Visual Studio .NET 2005 zawiera narzędzie projektanta wiersza. Można uzyskać dostępu do tego narzędzia, za pomocą przycisku tagów inteligentnych w formancie TreeView jako poniżej.


![](data-bound-controls/_static/image1.jpg)

**Rysunek 1**


Po **dostosować obrazy liniowe** opcji menu narzędzia Projektant wiersza zostanie uruchomiona, umożliwiając konfigurowanie wyglądu wierszy w elemencie TreeView.

## <a name="treeview-events"></a>Zdarzenia TreeView

TreeView — kontrolka ma unikatowy następujące zdarzenia:

- Na podstawie SelectedNodeChanged występuje po wybraniu węzła **SelectAction** właściwości.
- TreeNodeCheckChanged występuje, gdy stan checkboxs węzłów zostanie zmieniona.
- Na podstawie TreeNodeExpanded występuje, gdy węzeł jest rozwinięty **SelectAction** właściwości.
- TreeNodeCollapsed występuje, gdy węzeł jest zwinięty.
- TreeNodeDataBound występuje, gdy węzeł ma dane powiązane.
- TreeNodePopulate występuje, gdy węzeł zostanie wypełnione.

**SelectAction** właściwości można skonfigurować, które zdarzenie jest wywoływane po wybraniu węzła. Właściwość SelectAction zapewnia następujące akcje:

- TreeNodeSelectAction.Expand zgłasza TreeNodeExpanded po wybraniu węzła.
- TreeNodeSelectAction.None zgłasza nie zdarzenie po wybraniu węzła.
- TreeNodeSelectAction.Select wywołuje zdarzenie SelectedNodeChanged po wybraniu węzła.
- TreeNodeSelectAction.SelectExpand zgłasza zarówno SelectedNodeChanged zdarzeń i TreeNodeExpanded po wybraniu węzła.

## <a name="controlling-appearance-with-styles"></a>Kontrolowanie wyglądu przy użyciu stylów

TreeView — formant zawiera wiele właściwości sterujące wyglądu formantu przy użyciu stylów. Dostępne są następujące właściwości.

| **Nazwa właściwości** | **Kontrolki** |
| --- | --- |
| HoverNodeStyle | Kontroluje styl węzłów, gdy wskaźnik myszy znajduje się myszy nad nimi. |
| LeafNodeStyle | Określa styl węzłów liści. |
| NodeStyle | Określa styl dla wszystkich węzłów. Style określonego węzła (na przykład LeafNodeStyle) musi zostać zastąpiona tym stylem. |
| ParentNodeStyle | Określa styl dla wszystkich węzłów nadrzędnych. |
| RootNodeStyle | Określa styl węzła głównego. |
| SelectedNodeStyle | Określa styl dla wybranego węzła. |

Każdy z tych właściwości jest tylko do odczytu. Jednak zostaną każdorazowym **TreeNodeStyle** obiekt i właściwości tego obiektu może zostać zmodyfikowany za pomocą *podwłaściwości właściwości* format. Na przykład, aby ustawić **ForeColor** właściwość **SelectedNodeStyle**, należy użyć następującej składni:

[!code-aspx[Main](data-bound-controls/samples/sample8.aspx)]

Zwróć uwagę, nie zamknięto tagu powyżej. Wynika to z podczas za pomocą składni deklaratywnej pokazane, można dołączyć węzły TreeViews również kodu HTML.

Właściwości stylu można również określić za pomocą kodu *property.subproperty* format. Na przykład, aby ustawić **ForeColor** właściwość **RootNodeStyle** w kodzie, może użyć następującej składni:

[!code-csharp[Main](data-bound-controls/samples/sample9.cs)]

> [!NOTE]
> Aby uzyskać pełną listę właściwości innego stylu w dokumentacji MSDN w obiekcie TreeNodeStyle.


## <a name="the-sitemappath-control"></a>Kontrolki ścieżki mapy witryny

Kontrolki ścieżki mapy witryny udostępnia kontrolkę nawigacji Okruchy chleb dla deweloperów platformy ASP.NET. Podobnie jak inne formanty nawigacji można go łatwo danych powiązany z danymi hierarchicznymi źródeł, takich jak SiteMapDataSource lub elementu XmlDataSource.

Kontrolki ścieżki mapy witryny składa się z SiteMapNodeItem obiektów. Istnieją trzy typy węzłów; węzeł główny, węzły nadrzędne i bieżącego węzła. Węzeł główny jest węzłem w górnej części strukturze hierarchicznej. Bieżący węzeł reprezentuje bieżącej strony. Wszystkie inne węzły są węzłów nadrzędnych.

## <a name="controlling-the-operation-of-the-sitemappath-control"></a>Kontrolowanie operacji kontrolki ścieżki mapy witryny

Właściwości służące do sterowania działaniem kontrolki ścieżki mapy witryny, są następujące:

| **Właściwość** | **Opis właściwości** |
| --- | --- |
| ParentLevelsDisplayed | Określa, jak wiele węzłów nadrzędnych są wyświetlane. Wartość domyślna to -1, która nakłada nie ograniczenia liczby wyświetlane węzły nadrzędne. |
| PathDirection | Określa kierunek ścieżki mapy witryny. Prawidłowe wartości to RootToCurrent (ustawienie domyślne) i CurrentToRoot. |
| PathSeparator | Ciąg, który określa znak oddzielający węzłów kontrolki ścieżki mapy witryny. Wartość domyślna to:. |
| RenderCurrentNodeAsLink | Wartość logiczna, która kontroluje, czy bieżący węzeł jest traktowany jako łącze. Wartość domyślna to False. |
| SkipLinkText | Ułatwia utrzymanie ułatwień dostępu, gdy strona zostanie wyświetlony czytników ekranu. Ta właściwość umożliwia czytników ekranu pominąć kontrolki ścieżki mapy witryny. Aby wyłączyć tę funkcję, ustaw właściwość String.Empty. |

## <a name="templated-sitemappath-controls"></a>Kontrolki ścieżki mapy witryny opartego na szablonie

SiteMapControl jest kontrolki z szablonem, a w efekcie można zdefiniować różne szablony do użycia w wyświetlanie formantu. Aby edytować szablony formantu ścieżki mapy witryny, kliknij przycisk tagów inteligentnych w formancie i wybierz polecenie Edytuj szablony w menu. Wyświetla SiteMapTasks menu, jak pokazano poniżej której można wybrać różne dostępne szablony.


![](data-bound-controls/_static/image2.jpg)

**Rysunek 2**


**NodeTemplate** szablonu odwołuje się do dowolnego węzła w ścieżki mapy witryny. Jeśli węzeł jest węzłem głównym lub bieżącego węzła oraz **RootNodeTemplate** lub **CurrentNodeTemplate** jest skonfigurowany, NodeTemplate zostanie zastąpiona.

## <a name="sitemappath-events"></a>Zdarzenia ścieżki mapy witryny

Kontrolki ścieżki mapy witryny ma dwa zdarzenia, które nie pochodzą z klasy formantu; **ItemCreated** zdarzeń i **ItemDataBound** zdarzeń. ItemCreated zdarzenie jest wywoływane po utworzeniu elementu ścieżki mapy witryny. ItemDataBound jest wywoływane, gdy wywoływana jest metoda DataBind, podczas tworzenia powiązań danych z węzłem ścieżki mapy witryny. A **SiteMapNodeItemEventArgs** obiektu zapewnia dostęp do określonych SiteMapNodeItem za pomocą właściwości elementu.

## <a name="controlling-appearance-with-styles"></a>Kontrolowanie wyglądu przy użyciu stylów

Następujące style są dostępne dla kontrolki ścieżki mapy witryny.

| **Nazwa właściwości** | **Kontrolki** |
| --- | --- |
| CurrentNodeStyle | Określa styl tekstu dla bieżącego węzła. |
| RootNodeStyle | Określa styl tekstu dla węzła głównego. |
| NodeStyle | Określa styl tekstu dla wszystkich węzłów, przy założeniu, że CurrentNodeStyle lub RootNodeStyle nie ma zastosowania. |

Właściwość NodeStyle zostanie zastąpiona przez CurrentNodeStyle lub RootNodeStyle. Każdej z tych właściwości jest tylko do odczytu i zwraca **styl** obiektu. Wpływ na wygląd węzła przy użyciu jednej z tych właściwości, należy ustawić właściwości obiektu styl, który jest zwracany. Na przykład poniższy kod zmienia właściwości forecolor bieżącego węzła.

[!code-aspx[Main](data-bound-controls/samples/sample10.aspx)]

Właściwości można zastosować też programowo w następujący sposób:

[!code-csharp[Main](data-bound-controls/samples/sample11.cs)]

> [!NOTE]
> Jeśli szablon jest stosowany, styl nie będą stosowane.


## <a name="lab-1-configuring-an-aspnet-menu-control"></a>Laboratorium 1: Konfigurowanie kontrolkę ASP.NET Menu

1. Utwórz nową witrynę sieci Web.
2. Dodaj plik mapy witryny, wybierając plik, nowy, pliku i wybierając pozycję mapy witryny z listy szablonów pliku.
3. Otwórz mapy witryny (Web.sitemap domyślnie) i zmodyfikuj go, aby wygląda jak poniżej listy. Strony, na którym jest nawiązywane połączenie, w pliku mapy witryny naprawdę nie istnieją, ale które nie będą problemem w przypadku tego ćwiczenia.

    [!code-xml[Main](data-bound-controls/samples/sample12.xml)]
4. Otwórz domyślnego formularza sieci Web w widoku Projekt.
5. W sekcji nawigacji przybornika należy dodać nowy formant Menu do strony.
6. W sekcji danych przybornika dodać nowe SiteMapDataSource. SiteMapDataSource automatycznie zostanie użyty plik Web.sitemap w witrynie. (Plik Web.sitemap *musi* znajdować się w folderze głównym witryny.)
7. Kliknij Menu sterowania, a następnie kliknij przycisk tagów inteligentnych, aby wyświetlić okno dialogowe Menu zadania.
8. Na liście rozwijanej wybierz źródło danych wybierz SiteMapDataSource1.
9. Kliknij łącze Autoformatowanie, a następnie wybierz format menu.
10. W okienku właściwości ustaw **wartości StaticDisplayLevels** właściwości do 2. Formant Menu teraz powinien być wyświetlany węzeł głównej, produktów i usług w projektancie.
11. Przejdź na stronę w przeglądarce, aby skorzystać z menu. (Ponieważ strony był dodany do mapy witryny faktycznie nie istnieje, zobaczysz wystąpił błąd podczas próby przeglądania ich.)

Eksperymentować zmiana wartości StaticDisplayLevels i właściwości MaximumDynamicDisplayLevels i ich wpływ na sposób menu jest renderowany w temacie.

## <a name="lab-2-dynamically-binding-a-treeview-control"></a>Laboratorium 2: Dynamiczne powiązanie formantu TreeView

Tym ćwiczeniu przyjęto założenie, że program SQL Server działa lokalnie i że bazy danych Northwind jest dostępny w wystąpieniu programu SQL Server. Jeśli te warunki nie są spełnione, Zmień parametry połączenia w próbce. Należy pamiętać, że może być również konieczne Określ uwierzytelniania programu SQL Server zamiast zaufanego połączenia.

1. Utwórz nową witrynę sieci Web.
2. Przełącz do widoku kodu dla Default.aspx i Zastąp cały kod z kodem wymienionych poniżej. 

    [!code-aspx[Main](data-bound-controls/samples/sample13.aspx)]
3. Zapisz strony jako treeview.aspx.
4. Przejdź na stronę.
5. Po wyświetleniu strony Wyświetl źródło strony w przeglądarce. Należy pamiętać, że tylko widoczne węzły zostały wysłane do klienta.
6. Kliknij znak plus obok każdego węzła.
7. Wyświetl źródło na stronie ponownie. Zwróć uwagę, czy nowo wyświetlane węzły są obecne.

## <a name="lab-3-details-view-and-editing-data-using-a-gridview-and-detailsview"></a>Laboratorium 3: Szczegóły wyświetlanie i edytowanie danych przy użyciu widoku GridView i widoku DetailsView

1. Utwórz nową witrynę sieci Web.
2. Dodaj nowy plik web.config witryny sieci Web.
3. Dodaj parametry połączenia do pliku web.config, jak pokazano poniżej: 

    [!code-xml[Main](data-bound-controls/samples/sample14.xml)]

    > [!NOTE]
    > Może być konieczna zmiana parametry połączenia oparte na środowisku.
4. Zapisz i zamknij plik web.config.
5. Otwórz plik Default.aspx i dodać nową kontrolkę SqlDataSource.
6. Zmień identyfikator formantu SqlDataSource **produkty**.
7. W **zadania SqlDataSource** menu, kliknij przycisk **skonfiguruj źródło danych**.
8. Wybierz **Northwind** na liście rozwijanej połączeń i kliknij przycisk Dalej.
9. Wybierz **produktów** z **nazwa** listy rozwijanej i sprawdź **ProductID**, **ProductName**, **UnitPrice**, i **StanMagazynu** pola wyboru, jak pokazano poniżej. 

![](data-bound-controls/_static/image3.jpg)

    **Figure 3**
10. Kliknij przycisk **Dalej**.
11. Kliknij przycisk **Zakończ**.
12. Przejdź do widoku źródłowego i sprawdź, czy kod, który został wygenerowany. Powiadomienie **SelectCommand**, **elementu DeleteCommand**, **InsertCommand**, i **UpdateCommand** do SqlDataSource dodano formant. Ponadto parametrów, które zostały dodane.
13. Przełącz do widoku projektu i dodać nowe kontrolki widoku siatki do strony.
14. Wybierz **produktów** z **wybierz źródło danych** listy rozwijanej.
15. Sprawdź **włączyć stronicowanie** i **Włącz zaznaczanie** jak pokazano poniżej. 

![](data-bound-controls/_static/image4.jpg)

    **Figure 4**
16. Kliknij przycisk **Edytowanie kolumn** łącza i upewnij się, że **automatycznie wygenerować pola** jest zaznaczony.
17. Kliknij przycisk **OK**.
18. Za pomocą formantu widoku GridView zaznaczone, kliknij przycisk **DataKeyNames** właściwości w okienku właściwości.
19. Wybierz **ProductID** z **dostępnych polach danych** listy i kliknij przycisk **&gt;** przycisk, aby dodać go.
20. Kliknij przycisk OK.
21. Dodaj nową kontrolkę SqlDataSource do strony.
22. Zmień identyfikator formantu SqlDataSource **szczegóły**.
23. Z menu zadania SqlDataSource wybierz **skonfiguruj źródło danych**.
24. Wybierz **Northwind** z listy rozwijanej i kliknij przycisk **dalej**.
25. Wybierz <strong>produktów</strong> z <strong>nazwa</strong> listy rozwijanej i sprawdź <strong> \</ strong > * checkbox w <strong>kolumn</strong> listbox.
26. Kliknij przycisk **gdzie** przycisku.
27. Wybierz **ProductID** z **kolumny** listy rozwijanej.
28. Wybierz **=** na liście rozwijanej operatora.
29. Wybierz **kontroli** z **źródła** listy rozwijanej.
30. Wybierz **GridView1** z **Identyfikatora formantu** listy rozwijanej.
31. Kliknij przycisk **Dodaj** przycisk, aby dodać klauzuli WHERE.
32. Kliknij przycisk **OK**.
33. Kliknij przycisk **zaawansowane** przycisk i sprawdź **Generuj instrukcje INSERT, UPDATE i DELETE** wyboru.
34. Kliknij przycisk **OK**.
35. Kliknij przycisk **dalej** i kliknij przycisk **Zakończ**.
36. Na stronie Dodaj formant widoku DetailsView.
37. W **wybierz źródło danych** listy rozwijanej wybierz **szczegóły**.
38. Sprawdź **Włącz edytowanie** pole wyboru, jak pokazano poniżej. 

![](data-bound-controls/_static/image1.gif)

    **Figure 5**
39. Strony i Przeglądaj Default.aspx.
40. Kliknij przycisk **wybierz** link obok różnych rekordów w celu wyświetlenia aktualizacji widoku DetailsView automatycznie.
41. Kliknij przycisk **Edytuj** łącze w kontrolce widoku DetailsView.
42. Zmiany w rekordzie, a następnie kliknij przycisk **aktualizacji**.
