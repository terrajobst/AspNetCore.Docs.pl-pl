---
uid: web-forms/overview/moving-to-aspnet-20/profiles-themes-and-web-parts
title: "Profile, kompozycje i części sieci Web | Dokumentacja firmy Microsoft"
author: microsoft
description: "Są istotne zmiany w konfiguracji i instrumentacji w programie ASP.NET 2.0. Nowy interfejs API konfiguracji ASP.NET pozwala na pr wprowadzanie zmian w konfiguracji..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 92df4051-77c6-492c-bd34-23d24189cea4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/profiles-themes-and-web-parts
msc.type: authoredcontent
ms.openlocfilehash: c9fe97dbd5fe10cbde25b9daf5ddd35b2d7eaab5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="profiles-themes-and-web-parts"></a>Profile, kompozycje i części sieci Web
====================
przez [firmy Microsoft](https://github.com/microsoft)

> Są istotne zmiany w konfiguracji i instrumentacji w programie ASP.NET 2.0. Nowy interfejs API konfiguracji ASP.NET pozwala na programowo wprowadzanie zmian w konfiguracji. Ponadto istnieje wiele nowe ustawienia konfiguracji umożliwiają nowe konfiguracje i instrumentacji.


Platforma ASP.NET 2.0 reprezentuje znaczącej poprawy w obszarze spersonalizowanych witryn sieci Web. Oprócz członkostwa weve funkcje opisane profilów programu ASP.NET, kompozycje i składniki Web Part znacznie zwiększyć personalizacji w witrynach sieci Web.

## <a name="aspnet-profiles"></a>Profile platformy ASP.NET

Profile platformy ASP.NET są podobne do sesji. Różnica polega na profil jest trwały sesji jest utracone po zamknięciu przeglądarki. Inną istotną różnicą między sesjami i profilów jest czy profile są silnie typizowane, w związku z tym dostarczać IntelliSense podczas procesu projektowania.

Profil jest zdefiniowany w pliku konfiguracyjnym maszyny lub pliku web.config aplikacji. (Nie można zdefiniować profil w pliku web.config podfoldery.) Poniższy kod definiuje profilu, aby najpierw zapisać osoby odwiedzające witrynę sieci Web i nazwiska.

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample1.xml)]

Domyślny typ danych dla właściwości profilu jest typu System.String. W powyższym przykładzie został określony żaden typ danych. W związku z tym właściwości imię i nazwisko są typu ciąg. Jak wcześniej wspomniano, profil, którego właściwości są silnie typizowane. Poniższy kod dodaje nową właściwość zakres, który jest typu Int32.

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample2.xml)]

Profile są zazwyczaj używane z uwierzytelnianiem formularzy programu ASP.NET. Gdy jest używany w połączeniu z uwierzytelniania formularzy, każdy użytkownik ma osobny profil skojarzony z swojego identyfikatora użytkownika. Jednak istnieje również możliwość korzystania z profilów w anonimowego aplikacji przy użyciu &lt;anonymousIdentification&gt; w pliku konfiguracji wraz z **allowAnonymous** atrybutu jako pokazano poniżej:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample3.xml)]

Gdy użytkownik anonimowy przegląda lokacji, program ASP.NET tworzy wystąpienie **ProfileCommon** dla użytkownika. Ten profil używa unikatowego Identyfikatora przechowywane w pliku cookie w przeglądarce do identyfikowania użytkownika jako to unikatowy odwiedzający. W ten sposób można przechowywać informacje o profilu dla użytkowników, którzy są przeglądanie anonimowo.

## <a name="profile-groups"></a>Profil grupy

Istnieje możliwość do właściwości grupy profilów. Przez grupowanie właściwości jest możliwa do symulowania wielu profilów dla określonej aplikacji.

Następująca konfiguracja konfiguruje właściwość Imię i nazwisko dla dwóch grup; Kupujący i perspektyw.

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample4.xml)]

Następnie jest możliwość ustawienia właściwości w określonej grupie w następujący sposób:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample5.cs)]

## <a name="storing-complex-objects"></a>Zapisywanie obiektów złożonych

Do tej pory przykładach, możemy zostały objęte przechowywanych proste typy danych w profilu. Istnieje również możliwość przechowywania złożone typy danych w profilu, określając serializacji przy użyciu metody **serializeAs** atrybutu w następujący sposób:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample6.xml)]

W takim przypadku typ jest PurchaseInvoice. Klasa PurchaseInvoice musi być oznaczony jako możliwy do serializacji i może zawierać dowolną liczbę właściwości. Na przykład, jeśli PurchaseInvoice ma właściwość o nazwie **NumItemsPurchased**, można odwołać się do tej właściwości w kodzie w następujący sposób:

[!code-css[Main](profiles-themes-and-web-parts/samples/sample7.css)]

## <a name="profile-inheritance"></a>Dziedziczenie profilu

Istnieje możliwość tworzenia profilu do użycia w wielu aplikacjach. Tworząc profil klasę, która jest pochodną ProfileBase, można ponownie użyć profilu w kilku aplikacji przy użyciu **dziedziczy** atrybutu, jak pokazano poniżej:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample8.xml)]

W takim przypadku klasa **PurchasingProfile** będzie wyglądać w następujący sposób:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample9.cs)]

## <a name="profile-providers"></a>Profil dostawców

Profile platformy ASP.NET używają modelu dostawcy. Domyślny dostawca przechowuje dane w bazie danych programu SQL Server Express w aplikacji\_folderu dane aplikacji sieci Web przy użyciu dostawcy SqlProfileProvider. Jeśli baza danych nie istnieje, ASP.NET automatycznie utworzy go podczas próby przechowywania informacji o profilu.

W niektórych przypadkach jednak warto opracowanie własnego dostawcy profilu. Funkcja profilu platformy ASP.NET pozwala łatwo korzystać z różnych dostawców.

Tworzenie dostawcy niestandardowego profilu po:

- Należy przechowywać informacje o profilu w źródle danych, takich jak FoxPro bazy danych lub w bazie danych programu Oracle, który nie jest obsługiwany przez dostawców profilu uwzględnionych w programie .NET Framework.
- Należy zarządzać za pomocą schematu bazy danych, która różni się od schematu bazy danych używane przez dostawców uwzględnionych w programie .NET Framework informacje o profilu. Typowym przykładem jest chcesz zintegrować informacje o profilu z danych użytkowników w istniejącej bazy danych programu SQL Server.

### <a name="required-classes"></a>Wymaganych klas

Aby zaimplementować dostawcę profilu, należy utworzyć klasę, która dziedziczy z klasy abstrakcyjnej System.Web.Profile.ProfileProvider. **ProfileProvider** abstrakcyjna klasa dziedziczy z kolei klasa abstrakcyjna System.Configuration.SettingsProvider, który dziedziczy z klasy abstrakcyjnej System.Configuration.Provider.ProviderBase. Ze względu na to łańcuch dziedziczenia, oprócz wymaganych elementów członkowskich **ProfileProvider** klasy, musi implementować członków wymagane **SettingsProvider** i  **ProviderBase** klasy.

W poniższych tabelach opisano właściwości i metody, które należy zaimplementować z **ProviderBase**, **SettingsProvider**, i **ProfileProvider** abstrakcyjny klasy.

### <a name="providerbase-members"></a>Elementy członkowskie ProviderBase

| **Element członkowski** | **Opis** |
| --- | --- |
| Initialize — Metoda | Przyjmuje jako dane wejściowe nazwę wystąpienia dostawcy i NameValueCollection ustawień konfiguracji. Służy do określania opcji i wartości właściwości dla wystąpienia dostawcy, w tym wartości konkretnej implementacji i opcje określony w konfiguracji komputera lub pliku Web.config. |

### <a name="settingsprovider-members"></a>Elementy członkowskie SettingsProvider

| **Element członkowski** | **Opis** |
| --- | --- |
| Właściwość ApplicationName | Nazwa aplikacji, który jest przechowywany z każdym profilem. Dostawca profilu używa nazwy aplikacji do przechowywania informacji o profilu osobno dla każdej aplikacji. Dzięki temu wiele aplikacji programu ASP.NET użyć tego samego źródła danych bez konflikt, jeśli ta sama nazwa użytkownika jest tworzona w innych aplikacjach. Alternatywnie wiele aplikacji programu ASP.NET można udostępniać źródło danych profilu, określając nazwę tej samej aplikacji. |
| GetPropertyValues — metoda | Przyjmuje jako dane wejściowe, a SettingsContext i obiektu SettingsPropertyCollection. **SettingsContext** zawiera informacje o użytkowniku. Informacje można użyć jako klucza podstawowego można pobrać informacji o właściwości profilu dla użytkownika. Użyj **SettingsContext** obiekt, aby uzyskać nazwę użytkownika i czy użytkownik jest uwierzytelnieni lub anonimowi. **SettingsPropertyCollection** zawiera kolekcję obiektów SettingsProperty. Każdy **SettingsProperty** obiektu zawiera nazwę i typ właściwości, a także dodatkowe informacje, takie jak wartość domyślna właściwości oraz tego, czy właściwość jest tylko do odczytu. **GetPropertyValues** metody wypełnia SettingsPropertyValue obiektów na podstawie SettingsPropertyValueCollection **SettingsProperty** obiektów podana jako dane wejściowe. Wartości ze źródła danych dla określonego użytkownika są przypisane do właściwości PropertyValue dla każdego **SettingsPropertyValue** jest zwracany obiekt i całą kolekcję. Wywołanie metody aktualizuje również wartość LastActivityDate profilu określonego użytkownika do bieżącej daty i godziny. |
| SetPropertyValues — metoda | Przyjmuje jako dane wejściowe **SettingsContext** i **SettingsPropertyValueCollection** obiektu. **SettingsContext** zawiera informacje o użytkowniku. Informacje można użyć jako klucza podstawowego można pobrać informacji o właściwości profilu dla użytkownika. Użyj **SettingsContext** obiekt, aby uzyskać nazwę użytkownika i czy użytkownik jest uwierzytelnieni lub anonimowi. **SettingsPropertyValueCollection** zawiera kolekcję **SettingsPropertyValue** obiektów. Każdy **SettingsPropertyValue** obiektu zawiera nazwę, typ i wartość właściwości, a także dodatkowe informacje, takie jak wartość domyślna właściwości oraz tego, czy właściwość jest tylko do odczytu. **SetPropertyValues** metody aktualizacji wartości właściwości profilu w źródle danych dla określonego użytkownika. Wywołanie metody również aktualizacje **LastActivityDate** i LastUpdatedDate wartości dla profilu określonego użytkownika do bieżącej daty i godziny. |

### <a name="profileprovider-members"></a>Elementy członkowskie ProfileProvider

| **Element członkowski** | **Opis** |
| --- | --- |
| DeleteProfiles — metoda | Przyjmuje jako dane wejściowe w tablicy ciągów użytkownika nazwy i usuwa ze źródła danych, wszystkie profilu informacji i wartości właściwości dla określonej nazwy, jeśli nazwa aplikacji odpowiada **ApplicationName** wartości właściwości. Jeśli źródło danych obsługuje transakcje, zalecane jest obejmują wszystkie operacje usuwania w transakcji i Wycofaj tę transakcję i Zgłoś wyjątek, jeśli nie powiedzie się żadnej operacji usuwania. |
| DeleteProfiles — metoda | Przyjmuje jako dane wejściowe zbiór ProfileInfo obiekty i usuwa ze źródła danych, wszystkie profilu informacji i wartości właściwości dla każdego profilu, jeśli nazwa aplikacji odpowiada **ApplicationName** wartości właściwości. Jeśli źródło danych obsługuje transakcje, zalecane jest obejmują wszystkie operacje usuwania w transakcji i Wycofaj tę transakcję i zgłosić wyjątek, jeśli żadnej operacji usuwania nie powiedzie się. |
| DeleteInactiveProfiles — metoda | Przyjmuje jako dane wejściowe wartość ProfileAuthenticationOption obiekt DateTime i usuwa z danych źródłowych wszystkie informacje o profilu i wartości właściwości, gdzie datę ostatniej aktywności jest mniejsza niż lub równa określonej daty i godziny a nazwę aplikacji Dopasowuje **ApplicationName** wartości właściwości. **ProfileAuthenticationOption** parametr określa, czy tylko anonimowe profile uwierzytelniona tylko profile, czy wszystkie profile do usunięcia. Jeśli źródło danych obsługuje transakcje, zalecane jest obejmują wszystkie operacje usuwania w transakcji i Wycofaj tę transakcję i zgłosić wyjątek, jeśli żadnej operacji usuwania nie powiedzie się. |
| GetAllProfiles — metoda | Przyjmuje jako dane wejściowe **ProfileAuthenticationOption** wartość, liczba całkowita określająca indeks strony, liczba całkowita określająca rozmiar strony, a odwołania do liczba całkowita, która jest równa łącznej liczby profilów. Zwraca ProfileInfoCollection, który zawiera **ProfileInfo** obiektów dla wszystkich profilów w źródle danych, jeśli nazwa aplikacji odpowiada **ApplicationName** wartości właściwości. **ProfileAuthenticationOption** parametr określa, czy tylko anonimowe profile uwierzytelniona tylko profile, lub wszystkich profilów, które mają zostać zwrócone. Wyniki zwrócone przez **GetAllProfiles** metody są ograniczone przez indeks strony i wartości rozmiaru strony. Wartość rozmiaru strony określa maksymalną liczbę **ProfileInfo** obiektów, aby uzyskać **ProfileInfoCollection**. Określa wartość indeksu strony, strony wyników do zwrócenia, gdzie 1 identyfikuje pierwszej strony, która. Parametr całkowita liczba rekordów jest parametrem wyjściowym (można użyć **ByRef** w języku Visual Basic) który ma ustawioną wartość całkowita liczba profili. Na przykład, jeśli magazyn danych zawiera 13 profile aplikacji i strony wartość indeksu jest 2, używając strony o wielkości 5 **ProfileInfoCollection** zwrócił zawiera do szóstego do dziesiątego profilów. Wartość całkowita liczba rekordów jest ustawiona na 13, gdy metoda zwróci wartość. |
| GetAllInactiveProfiles — metoda | Przyjmuje jako dane wejściowe **ProfileAuthenticationOption** wartość **DateTime** obiektu, liczba całkowita określająca indeks strony, liczba całkowita określająca rozmiar strony, a odwołania do liczba całkowita, która zostanie ustawiona Całkowita liczba profili. Zwraca **ProfileInfoCollection** zawierający **ProfileInfo** obiektów dla wszystkich profilów w źródle danych, gdy datę ostatniej aktywności jest mniejsza niż lub równa do określonego **daty i godziny**  , jeśli nazwa aplikacji odpowiada **ApplicationName** wartości właściwości. **ProfileAuthenticationOption** parametr określa, czy tylko anonimowe profile uwierzytelniona tylko profile, lub wszystkich profilów, które mają zostać zwrócone. Wyniki zwrócone przez **GetAllInactiveProfiles** metody są ograniczone przez indeks strony i wartości rozmiaru strony. Wartość rozmiaru strony określa maksymalną liczbę **ProfileInfo** obiektów, aby uzyskać **ProfileInfoCollection**. Określa wartość indeksu strony, strony wyników do zwrócenia, gdzie 1 identyfikuje pierwszej strony, która. Parametr całkowita liczba rekordów jest parametrem wyjściowym (można użyć **ByRef** w języku Visual Basic) który ma ustawioną wartość całkowita liczba profili. Na przykład, jeśli magazyn danych zawiera 13 profile aplikacji i strony wartość indeksu jest 2, używając strony o wielkości 5 **ProfileInfoCollection** zwrócił zawiera do szóstego do dziesiątego profilów. Wartość całkowita liczba rekordów jest ustawiona na 13, gdy metoda zwróci wartość. |
| FindProfilesByUserName — metoda | Przyjmuje jako dane wejściowe **ProfileAuthenticationOption** wartość ciąg zawierający nazwę użytkownika, liczba całkowita określająca indeks strony, liczba całkowita określająca rozmiar strony, a odwołania do liczba całkowita, która jest równa łącznej liczby Profile. Zwraca **ProfileInfoCollection** zawierający **ProfileInfo** obiekty źródła wszystkie profile w danych, jeśli nazwa użytkownika odpowiada określonej nazwy użytkownika, jeśli odpowiada nazwa aplikacji **ApplicationName** wartości właściwości. **ProfileAuthenticationOption** parametr określa, czy tylko anonimowe profile uwierzytelniona tylko profile, lub wszystkich profilów, które mają zostać zwrócone. Jeśli źródło danych obsługuje możliwości wyszukiwania dodatkowe, takie jak symbole wieloznaczne, zapewniają bardziej rozbudowane funkcje wyszukiwania dla nazwy użytkownika. Wyniki zwrócone przez **FindProfilesByUserName** metody są ograniczone przez indeks strony i wartości rozmiaru strony. Wartość rozmiaru strony określa maksymalną liczbę **ProfileInfo** obiektów, aby uzyskać **ProfileInfoCollection**. Określa wartość indeksu strony, strony wyników do zwrócenia, gdzie 1 identyfikuje pierwszej strony, która. Parametr całkowita liczba rekordów jest parametrem wyjściowym (można użyć **ByRef** w języku Visual Basic) który ma ustawioną wartość całkowita liczba profili. Na przykład, jeśli magazyn danych zawiera 13 profile aplikacji i strony wartość indeksu jest 2, używając strony o wielkości 5 **ProfileInfoCollection** zwrócił zawiera do szóstego do dziesiątego profilów. Wartość całkowita liczba rekordów jest ustawiona na 13, gdy metoda zwróci wartość. |
| FindInactiveProfilesByUserName — metoda | Przyjmuje jako dane wejściowe **ProfileAuthenticationOption** wartość, ciąg zawierający nazwę użytkownika, **DateTime** obiektu, liczba całkowita określająca indeks strony, liczba całkowita określająca rozmiar strony, a Odwołanie do liczba całkowita, która jest równa łącznej liczby profilów. Zwraca **ProfileInfoCollection** zawierający **ProfileInfo** obiektów dla wszystkich profilów w źródle danych, jeśli nazwa użytkownika odpowiada określonej nazwy użytkownika, gdzie datę ostatniej aktywności jest mniejsza niż lub równe określonym **DateTime**, jeśli nazwa aplikacji odpowiada **ApplicationName** wartości właściwości. **ProfileAuthenticationOption** parametr określa, czy tylko anonimowe profile uwierzytelniona tylko profile, lub wszystkich profilów, które mają zostać zwrócone. Jeśli źródło danych obsługuje możliwości wyszukiwania dodatkowe, takie jak symbole wieloznaczne, zapewniają bardziej rozbudowane funkcje wyszukiwania dla nazwy użytkownika. Wyniki zwrócone przez **FindInactiveProfilesByUserName** metody są ograniczone przez indeks strony i wartości rozmiaru strony. Wartość rozmiaru strony określa maksymalną liczbę **ProfileInfo** obiektów, aby uzyskać **ProfileInfoCollection**. Określa wartość indeksu strony, strony wyników do zwrócenia, gdzie 1 identyfikuje pierwszej strony, która. Parametr całkowita liczba rekordów jest parametrem wyjściowym (można użyć **ByRef** w języku Visual Basic) który ma ustawioną wartość całkowita liczba profili. Na przykład, jeśli magazyn danych zawiera 13 profile aplikacji i strony wartość indeksu jest 2, używając strony o wielkości 5 **ProfileInfoCollection** zwrócił zawiera do szóstego do dziesiątego profilów. Wartość całkowita liczba rekordów jest ustawiona na 13, gdy metoda zwróci wartość. |
| GetNumberOfInActiveProfiles — metoda | Przyjmuje jako dane wejściowe **ProfileAuthenticationOption** wartość i **DateTime** obiektu i zwraca liczbę wszystkich profili w źródle danych, gdzie jest mniejsza niż określony datęostatniejaktywności **Data i godzina** , jeśli nazwa aplikacji odpowiada **ApplicationName** wartości właściwości. **ProfileAuthenticationOption** parametr określa, czy tylko anonimowe profile uwierzytelniona tylko profile, czy wszystkie profile do zliczenia. |

### <a name="applicationname"></a>ApplicationName

Ponieważ dostawcach profilów przechowywać informacje o profilu osobno dla każdej aplikacji, musisz zapewnić, że schemat danych zawiera nazwę aplikacji i zapytań oraz aktualizacji również obejmować nazwę aplikacji. Na przykład następujące polecenie służy do pobierania wartości właściwości z bazy danych na podstawie nazwy użytkownika i profil jest anonimowy i upewnia się, że **ApplicationName** wartość jest uwzględnione w zapytaniu.

[!code-sql[Main](profiles-themes-and-web-parts/samples/sample10.sql)]

## <a name="aspnet-themes"></a>Motywy ASP.NET

## <a name="what-are-aspnet-20-themes"></a>Co to są ASP.NET 2.0 motywy?

Jednym z najważniejszych aspektów aplikacji sieci Web jest spójny wygląd i zachowanie całej witryny. ASP.NET 1.x Deweloperzy zazwyczaj użycie kaskadowych arkuszy stylów (CSS) do zaimplementowania spójny wygląd i zachowanie. Platforma ASP.NET 2.0 motywów znacznie ulepszyć CSS ponieważ stanowią one deweloperów platformy ASP.NET możliwość określenia jej wyglądu kontrolek serwera ASP.NET, a także elementów HTML. Motywy ASP.NET może odnosić się do pojedynczych formantów, określona strona sieci Web lub całej aplikacji sieci Web. Jeśli potrzebne są obrazy motywów należy zastosować kombinację plików CSS, pliku skórki opcjonalne i opcjonalnie katalog obrazów. Plik skórki określa wygląd kontrolek serwera ASP.NET.

## <a name="where-are-themes-stored"></a>Gdzie są przechowywane motywy?

Lokalizacja przechowywania motywów różni się zależności w ich zakresie. Kompozycje, które można zastosować do aplikacji są przechowywane w następującym folderze:

`C:\WINDOWS\Microsoft.NET\Framework\v2.x.xxxxx\ASP.NETClientFiles\Themes\<Theme_Name>`

Motyw jest specyficzna dla konkretnej aplikacji są przechowywane w aplikacji\_motywy\&lt; Motyw\_nazwa&gt; katalogu w katalogu głównym witryny sieci Web.

> [!NOTE]
> Plik skórki tylko należy zmodyfikować właściwości formantu serwera, które mają wpływ na wygląd.

Motyw globalny jest motywu, który można zastosować do dowolnej aplikacji lub witryny sieci Web uruchomiony na serwerze sieci Web. Te kompozycje są domyślnie przechowywane w katalogu ASP.NETClientfiles\Themes, który jest wewnątrz katalogu v2.x.xxxxx. Alternatywnie można przenosić pliki motyw do aspnet\_system klienta/\_sieci web / /Themes/ [wersja] [motywu\_name] folder w katalogu głównym witryny sieci Web.

Motywy specyficzne dla aplikacji można stosować do aplikacji, w którym znajdują się pliki. Pliki te są przechowywane w aplikacji\_motywów /&lt;motyw\_nazwa&gt; katalogu w katalogu głównym witryny sieci Web.

## <a name="the-components-of-a-theme"></a>Składniki kompozycji

Motyw składa się z jednego lub więcej plików CSS, plik skórki opcjonalne i opcjonalne folderu obrazów. Pliki CSS może być dowolną nazwą ma (tj. default.css lub theme.css itp.) i musi być w katalogu głównym folderu motywów. Pliki CSS są używane do definiowania klas CSS zwykłej i atrybuty dla określonych selektorów. Aby zastosować jedną z klas CSS do elementu strony **CSSClass** właściwość jest używana.

Plik skórki jest plik XML, który zawiera definicje właściwości kontrolek serwera ASP.NET. Kod przedstawiony poniżej jest przykładowy plik skórki.

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample11.aspx)]

**Rysunek 1** poniżej przedstawia małych strony ASP.NET przeglądany bez zastosowania motywu. **Rysunek 2** przedstawia tego samego pliku z motyw. Kolor tła i kolor tekstu są skonfigurowane za pomocą pliku CSS. Wygląd przycisku i pole tekstowe są skonfigurowane przy użyciu pliku skórki wymienionych powyżej.


![Brak motywu](profiles-themes-and-web-parts/_static/image1.gif)

**Rysunek 1**: Brak motywu


![Zastosowano motyw](profiles-themes-and-web-parts/_static/image2.gif)

**Rysunek 2**: zastosowano motyw


Plik skórki wymienionych powyżej definiuje karnacji domyślnego dla wszystkich kontrolek TextBox i kontrolek przycisku. Oznacza to, że każdy formantu TextBox i Button — formant wstawione na stronie spowoduje przejście na wygląd. Można również zdefiniować skórki, który można zastosować do tych kontrolek przy użyciu określonych wystąpień **SkinID** właściwości formantu.

Poniższy kod definiuje karnacji formantu przycisku. Przycisk tylko formanty z **SkinID** właściwość **goButton** spowoduje przejście na wygląd skórki.

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample12.aspx)]

Może mieć tylko jeden skórki domyślne na serwerze — typ formantu. Jeśli potrzebujesz dodatkowych karnacji, należy użyć właściwości SkinID.

## <a name="applying-themes-to-pages"></a>Stosowanie motywów do stron

Można zastosować motyw przy użyciu jednej z następujących metod:

- W &lt;stron&gt; elementu w pliku web.config
- W @Page dyrektywy strony
- Programowe

## <a name="applying-a-theme-in-the-configuration-file"></a>Zastosowanie motywu w pliku konfiguracji

Aby zastosować motyw w pliku konfiguracyjnym aplikacji, należy użyć następującej składni:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample13.xml)]

Nazwa motywu określone w tym miejscu musi odpowiadać nazwie folderu motywów. Ten folder może istnieć albo w jednym z lokalizacji wymienione we wcześniejszej części tego kursu. Jeśli użytkownik spróbuje zastosować motywu, który nie istnieje, zostanie przeprowadzona błąd konfiguracji.

## <a name="applying-a-theme-in-the-page-directive"></a>Zastosowanie motywu w dyrektywie Page

Można również zastosować motywu w dyrektywy @ Page. Ta metoda umożliwia korzystanie z tematów określonej strony.

Aby zastosować motyw w @Page dyrektywy, należy użyć następującej składni:

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample14.aspx)]

Jeszcze raz motywu określone w tym miejscu musi być zgodna folderu motywu, jak wspomniano powyżej. Jeśli do stosowania motywu, który nie istnieje, wystąpi błąd kompilacji. Visual Studio utworzy również zaznacz atrybut i powiadomienia, czy istnieje bez takich motywu.

## <a name="applying-a-theme-programmatically"></a>Zastosowanie motywu programowo

Aby zastosować motyw programowo, należy określić **motyw** właściwości strony w **strony\_PreInit** metody.

Aby zastosować motyw programowo, należy użyć następującej składni:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample15.cs)]

Należy zastosować motyw w metodzie PreInit z powodu cyklu życia strony. W przypadku zastosowania później, motywu strony będzie zostały już zastosowane w czasie wykonywania i zmiany w tym momencie jest za późno w cyklu życia. W przypadku zastosowania motywu, który nie istnieje, **HttpException** występuje. Ostrzeżenie kompilacji w motyw jest stosowany programowo, nastąpi Jeśli określona właściwość SkinID wszystkich formantów serwera. Dotyczy to ostrzeżenie informujące, że nie motyw deklaratywnie jest stosowana i można go zignorować.

## <a name="exercise-1--applying-a-theme"></a>Ćwiczenie 1: Stosowanie motywu

W tym ćwiczeniu zostanie zastosowana kompozycji ASP.NET do witryny sieci Web.

> [!IMPORTANT]
> Korzystania z programu Microsoft Word do wprowadzania informacji w pliku skórki, upewnij się, że użytkownik nie zastępuje cudzysłowy regularnych z cudzysłowami inteligentne. Inteligentne cudzysłowy powoduje problemów z plikach skórki.

1. Utwórz nową witrynę sieci Web programu ASP.NET.
2. Kliknij prawym przyciskiem myszy projekt w Eksploratorze rozwiązań i wybierz polecenie Dodaj nowy element.
3. Wybierz plik konfiguracji sieci Web z listy plików, a następnie kliknij przycisk Dodaj.
4. Kliknij prawym przyciskiem myszy projekt w Eksploratorze rozwiązań i wybierz polecenie Dodaj nowy element.
5. Wybierz plik skórki, a następnie kliknij przycisk Dodaj.
6. Kliknij przycisk Tak, po otrzymaniu monitu, jeśli chcesz umieścić plik w aplikacji youd\_folderu motywów.
7. Kliknij prawym przyciskiem myszy w folderze SkinFile wewnątrz aplikacji\_motywów folder w Eksploratorze rozwiązań i wybierz polecenie Dodaj nowy element.
8. Wybierz arkusz stylów z listy plików, a następnie kliknij przycisk Dodaj. Masz teraz wszystkie pliki niezbędne do zaimplementowania nowy motyw. Jednak program Visual Studio ma o nazwie folderu motywy SkinFile. Kliknij prawym przyciskiem myszy na wybrany folder i Zmień nazwę CoolTheme.
9. Otwórz plik SkinFile.skin i Dodaj następujący kod na końcu pliku: 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample16.aspx)]
10. Zapisz plik SkinFile.skin.
11. Otwórz StyleSheet.css.
12. Zastąp cały tekst w nim następujące czynności: 

    [!code-css[Main](profiles-themes-and-web-parts/samples/sample17.css)]
13. Zapisz plik StyleSheet.css.
14. Otwórz stronę Default.aspx.
15. Dodawanie kontrolki pola tekstowego i kontrolkę przycisku.
16. Zapisz stronę. Przeglądać strony Default.aspx. Powinna zostać wyświetlona jako normalne formularza sieci Web.
17. Otwórz plik web.config.
18. Dodaj następujący kod bezpośrednio pod otwarcia `<system.web>` tagu: 

    [!code-xml[Main](profiles-themes-and-web-parts/samples/sample18.xml)]
19. Zapisz plik web.config. Przeglądać strony Default.aspx. Powinna zostać wyświetlona z zastosowano motyw.
20. Jeśli go nie jest jeszcze otwarty, otwórz stronę Default.aspx w programie Visual Studio.
21. Wybierz przycisk.
22. Zmień **SkinID** goButton dla właściwości. Zwróć uwagę, że program Visual Studio udostępnia listy rozwijanej prawidłowymi wartościami SkinID kontrolki przycisku.
23. Zapisz stronę. Teraz ponownie Wyświetl podgląd strony w przeglądarce. Przycisk teraz powinny przekazać komunikat "Przejdź do" i powinien być szersze wyglądu.

Przy użyciu **SkinID** właściwości, łatwo można skonfigurować różne karnacje dla różnych wystąpień określonego typu serwera.

## <a name="the-stylesheettheme-property"></a>Element StyleSheetTheme właściwości

Do tej pory zajmowaliśmy tylko stosowanie motywów przy użyciu właściwości motywu. Za pomocą właściwości motywu, plik skórki zastępują ustawienia deklaratywne kontrolki serwera. Na przykład w ćwiczenie 1 określone SkinID "goButton" dla formantu przycisku i zmieniło tekst przycisku "Przejdź do". Można zauważyć, że właściwość tekst przycisku w Projektancie była ustawiona na "Button", ale overrode motywu, który. Motyw zawsze spowoduje zastąpienie ustawień właściwości w projektancie.

Jeśli chcesz mieć możliwość zastąpienie właściwości zdefiniowane w pliku skórki motywu z właściwości określone w projektancie, można użyć **element StyleSheetTheme** właściwości zamiast właściwości motywu. Właściwość element StyleSheetTheme jest taka sama jak właściwość motyw z tą różnicą, że nie zastępuje wszystkie ustawienia jawną właściwość, takie jak właściwości kompozycji jest.

Aby wyświetlić to działanie, otwórz plik web.config z projektu w ćwiczenie 1 i zmień &lt;stron&gt; element do następującego:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample19.xml)]

Teraz przejdź strony Default.aspx i zobaczysz formantu przycisku ponownie ma właściwość tekst "Button". Wynika to z ustawieniem jawną właściwość w Projektancie zastępuje właściwość Text ustawione przez goButton SkinID.

## <a name="overriding-themes"></a>Zastępowanie motywów

Motyw globalny może zostać przesłonięta przez zastosowanie motywu o tej samej nazwie w aplikacji\_motywów folderu aplikacji. Motyw nie została zastosowana w scenariuszu zastąpienie wartość true. Jeśli środowisko uruchomieniowe wykryje motywu plików w aplikacji\_folderu kompozycje, zostanie zastosowany motyw, przy użyciu tych plików i zignoruje motyw globalny.

Właściwość element StyleSheetTheme jest możliwym do zastąpienia i może zostać zastąpiona w kodzie w następujący sposób:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample20.cs)]

## <a name="web-parts"></a>Części sieci Web

Części sieci Web ASP.NET jest zintegrowany zestaw służy do tworzenia witryn sieci Web, które umożliwiają użytkownikom końcowym zmodyfikować zawartość, wyglądu i zachowania stron sieci Web z przeglądarki. Zmiany można zastosować do wszystkich użytkowników w witrynie lub pojedynczych użytkowników. Użytkownikom modyfikowanie stron i kontrolek, można zapisać ustawienia przechowywania preferencji użytkownika wielu sesjach przeglądarki w przyszłości, funkcji zwanej personalizacji. Te możliwości części sieci Web oznacza, że deweloperzy nadania uprawnień użytkownikom końcowym spersonalizować aplikacji sieci Web dynamicznie, bez interwencji dewelopera lub administratora.

Przy użyciu zestawu kontroli części sieci Web, Projektant umożliwia użytkownikom końcowym:

- Dostosowanie strony zawartości. Użytkowników można Dodawanie nowych formantów składników Web Part do strony, usuń je, je ukryć lub minimalizację jak zwykłe systemu windows.
- Personalizowanie układ strony. Użytkownicy mogą przeciągnij formant składników Web Part do innej strefy na stronie lub zmienić wygląd, właściwości i zachowanie.
- Eksportowanie i importowanie kontrolki. Użytkownicy mogą importowania lub eksportowania części sieci Web ustawienia kontroli do użycia w innych stron lub witryn, zachowując właściwości, wygląd i nawet dane w formantach. Zmniejsza to żądań zapisu i konfiguracji danych użytkowników końcowych.
- Utwórz połączenia. Użytkownicy mogą nawiązywać połączenia między formantami, tak, aby na przykład formant wykresu można wyświetlić wykresu dla danych w formancie giełdowych. Użytkownicy można personalizować, nie tylko połączenia, ale wygląd i szczegółowe informacje dotyczące sposobu formantu wykres przedstawia dane.
- Zarządzanie i spersonalizować ustawienia na poziomie witryny. Autoryzowani użytkownicy mogą skonfigurować ustawienia na poziomie witryny, określić kto dostęp do witryny lub strony, ustaw opartej na rolach dostęp do formantów i tak dalej. Na przykład użytkownik z rolą administracyjną można ustawić formantu części sieci Web, który ma być współużytkowane przez wszystkich użytkowników, a uniemożliwić użytkownikom, którzy nie są administratorami z personalizowanie kontrolowania udostępnionego.

Zazwyczaj pracy ze składnikami Web Part w jeden z trzech sposobów: tworzenie stron używające formanty składników Web Part, tworzenie pojedynczych formantów części sieci Web lub tworzenia aplikacji sieci Web pełną, zawierający personalizowalne elementy, takie jak portal.

## <a name="page-development"></a>Tworzenie stron

Strona deweloperzy mogą używać narzędzi wizualnego projektu, takich jak Microsoft Visual Studio 2005 do tworzenia stron korzystających z części sieci Web. Jedną z zalet w przy użyciu narzędzia, takiego jak Visual Studio jest czy części sieci Web kontrolować zestaw udostępnia funkcje dotyczące konfiguracji formantów składników Web Part w wizualnego projektanta i tworzenia przeciągania i upuszczania. Na przykład można przeciągnij strefy składników Web Part lub kontrolce edytora części sieci Web na powierzchnię projektu za pomocą projektanta, a następnie skonfiguruj kontroli w prawo w Projektancie za pomocą interfejsu użytkownika, dostarczone przez składniki Web Part kontrolować zestawu. Może to przyspieszyć tworzenie aplikacji składników Web Part i zmniejszyć ilość kodu, które trzeba zapisać.

## <a name="control-development"></a>Programowanie formantu

Można użyć dowolnego istniejącego formantu ASP.NET jako formant części sieci Web, w tym standardowych kontrolek serwera sieci Web, niestandardowe kontrolki serwera i kontrolki użytkownika. Maksymalna sterowanie programowe środowiska może spowodować niestandardowych formantów składników Web Part, które pochodzi z klasy składnika Web Part. Rozwoju kontroli poszczególnych części sieci Web możesz będzie zazwyczaj Tworzenie formantu użytkownika i używać go jako formant części sieci Web albo utworzyć niestandardowego formantu części sieci Web.

Na przykład powstania kontrolek niestandardowych składników Web Part można utworzyć formantu zapewnia funkcje oferowane przez innych kontrolek serwera ASP.NET, które mogą być przydatne do pakietu jako wartość formantu części sieci Web: kalendarzy, list, informacje finansowe wiadomości, kalkulatory, kontrolek tekstu sformatowanego aktualizowania zawartości, można edytować siatki, które łączyć się baz danych, wykresy dynamicznie aktualizować ich Wyświetla lub pogodowe i są przesyłane informacje. Jeśli podasz wizualnego projektanta z formantu Każdy deweloper strony za pomocą programu Visual Studio można po prostu przeciągnij formant do strefy składników Web Part i skonfiguruj go w czasie projektowania, bez konieczności pisania kodu dodatkowe.

Personalizacja jest podstawą funkcji części sieci Web. Umożliwia użytkownikom modyfikowanie--lub spersonalizować — układ, wygląd i zachowanie formantów składników Web Part na stronie. Ustawienia spersonalizowane są długotrwałe: zostają one zachowane nie tylko podczas bieżącej sesji przeglądarki (tak jak stan widoku), ale także w pamięci nieulotnej tak, aby ustawienia użytkownika są zapisywane również przeglądarki przyszłych sesji. Personalizacja jest włączona domyślnie dla stron sieci Web Part.

Strukturalne składniki interfejsu użytkownika zależą od personalizacji i podaj podstawowej struktury i wymagane przez wszystkie formanty składników Web Part usług. Jeden składnik strukturalnych interfejsu użytkownika wymagane na każdej stronie składników Web Part jest formantem WebPartManager. Mimo że nigdy nie jest widoczne, ten formant ma krytyczne zadanie koordynowania wszystkie formanty składników Web Part na stronie. Na przykład śledzi wszystkie pojedynczych formantów składników Web Part. Zarządza strefy składników Web Part (regiony zawierające formantów składników Web Part na stronie), które są strefy, które środki. Również śledzi i formanty trybów wyświetlania, strona może być w taki sposób, jak Przeglądaj, Połącz, Edytuj lub katalogu trybu i czy Personalizacja zmiany są stosowane do wszystkich użytkowników lub pojedynczych użytkowników. Na koniec inicjuje i śledzi połączenia i komunikacji między formantami części sieci Web.

Drugi rodzaj strukturalnych składnik interfejsu użytkownika jest strefa. Strefy działa jako menedżerów układu na stronie składników Web Part. Mają one i organizowanie formantów, które pochodzi z klasy części (część formantów) i umożliwiają czy układ strony moduły w orientacji poziomej lub pionowej. Strefy także oferowanie wspólny i spójny elementy interfejsu użytkownika (na przykład styl nagłówków i stopek, title, styl obramowania, przycisków akcji i tak dalej) dla każdego formantu, które zawierają; wspólne elementy są określane jako chrome formantu. Kilka typów specjalnych strefy są używane w trybów wyświetlania i z różnych formantów. Różne typy stref zostały opisane w poniższej sekcji Formanty podstawowych części sieci Web.

Formanty interfejsu użytkownika części sieci Web, które pochodzi od **części** klasy, w skład głównej interfejsu użytkownika na stronie części sieci Web. Zestaw kontroli części sieci Web są elastyczne i włącznie w opcjach daje przy tworzeniu formantów części. Oprócz tworzenia własnych niestandardowych formantów składników Web Part, umożliwia także istniejących kontrolek serwera ASP.NET, kontrolek użytkownika lub niestandardowe kontrolki serwera jako formanty części sieci Web. W następnej sekcji opisano istotne formantów, które są najczęściej używane do tworzenia stron sieci Web Part.

## <a name="web-parts-essential-controls"></a>Formanty podstawowych składników Web Part

Zestaw kontroli części sieci Web jest rozbudowany, ale niektóre kontrolki są niezbędne, ponieważ są wymagane dla składników Web Part do pracy lub są one formanty najczęściej używanych na stronach składników Web Part. Możesz rozpocząć korzystanie z części sieci Web i tworzenie stron podstawowych składników Web Part, warto należy zapoznać się z podstawowych formantów składników Web Part opisane w poniższej tabeli.

| **Formant części sieci Web** | **Opis** |
| --- | --- |
| WebPartManager | Zarządza wszystkie formanty składników Web Part na stronie. Jeden (i tylko jeden) **WebPartManager** formant jest wymagany dla każdej strony części sieci Web. |
| CatalogZone | Zawiera formanty CatalogPart. Użyj tej strefy, aby utworzyć wykaz formantów części sieci Web, z których użytkownicy mogą wybrać kontrolki przeznaczone do dodania do strony. |
| Edytora EditorZone | Zawiera formanty EditorPart. Użyj tej strefy, aby umożliwić użytkownikom do edycji i spersonalizować formantów składników Web Part na stronie. |
| WebPartZone | Zawiera i udostępnia ogólny układ dla formantów składnika Web Part, które tworzą głównego interfejsu użytkownika strony. Użyj tej strefy, gdy utworzysz stron z formantami części sieci Web. Strony mogą zawierać co najmniej jedna strefa. |
| Strefy ConnectionsZone | Zawiera formanty WebPartConnection i udostępnia interfejs do zarządzania połączeniami. |
| Składnik Web Part (elementu GenericWebPart.) | Renderuje podstawowego interfejsu użytkownika; Większość formantów interfejsu użytkownika części sieci Web należą do tej kategorii. Dla maksymalnej sterowanie programowe, można utworzyć Kontrolki niestandardowe części sieci Web pochodzące od podstawy **składnika Web Part** formantu. Jak formanty składników Web Part, można użyć istniejącego kontrolki serwera, kontrolek użytkownika lub Kontrolki niestandardowe. Jeśli którykolwiek z tych formantów są umieszczane w strefie, **WebPartManager** kontroli jest automatycznie zawijany je za pomocą **elementu GenericWebPart** formanty w czasie wykonywania, dzięki czemu można ich używać z funkcjami części sieci Web. |
| Element CatalogPart | Zawiera listę dostępnych formantów składników Web Part, które użytkownicy mogą dodawać do strony. |
| WebPartConnection | Tworzy połączenie między dwoma formantów składników Web Part na stronie. Połączenie definiuje formantów składników Web Part jako dostawca (dane), a drugi jako klient. |
| Element EditorPart | Służy jako klasa podstawowa dla formantów specjalne edytora. |
| Formanty EditorPart (AppearanceEditorPart, LayoutEditorPart BehaviorEditorPart i PropertyGridEditorPart) | Zezwalaj użytkownikom na spersonalizować różnych aspektów formantów interfejsu użytkownika części sieci Web na stronie |

## <a name="lab-create-a-web-part-page"></a>Laboratorium: Tworzenie strony sieci Web

W tym laboratorium utworzy strona części sieci Web, który będzie aktualny informacji za pomocą profilów programu ASP.NET.

### <a name="creating-a-simple-page-with-web-parts"></a>Tworzenie prostego strony z części sieci Web

W tej części przewodnika tworzenia strona używa kontrolki części sieci Web do wyświetlenia zawartości statycznej. Pierwszym krokiem podczas pracy ze składnikami Web Part jest do utworzenia strony dwóch wymaganych elementów strukturalnych. Strona części sieci Web musi najpierw formantu WebPartManager do śledzenia i koordynowania wszystkie formanty części sieci Web. Drugie strony składników Web Part musi mieć co najmniej jeden stref, które są formanty złożone zawiera składnik Web Part lub innych kontrolek serwera, które zajmują określonego regionu strony.

> [!NOTE]
> Nie trzeba wykonywać żadnych czynności, aby włączyć personalizacji części sieci Web; jego jest domyślnie włączona dla zestaw kontroli części sieci Web. Przy pierwszym uruchomieniu strony składników Web Part w witrynie, ASP.NET ustawienie domyślnego dostawcy personalizacji do przechowywania ustawień personalizacji użytkownika. Aby uzyskać więcej informacji na temat personalizacji Zobacz Omówienie personalizacji części sieci Web.


### <a name="to-create-a-page-for-containing-web-parts-controls"></a>Aby utworzyć stronę zawierającą formanty części sieci Web

1. Zamknij stronę domyślną i Dodaj nową stronę do witryny o nazwie WebPartsDemo.aspx.
2. Przełącz się do **projekt** widoku.
3. Z **widoku** menu, upewnij się, że **formanty inne niż Visual** i **szczegóły** są zaznaczone opcje, dzięki czemu układu znaczniki i kontrolki, których nie ma interfejsu użytkownika.
4. Umieść punkt wstawiania przed  **&lt;div&gt;**  znaczniki na powierzchni projektu, a następnie naciśnij klawisz ENTER, aby dodać nowy wiersz. Umieść punkt wstawiania przed znaku nowego wiersza, kliknij przycisk **Block Format** kontroli w menu listy rozwijanej i wybierz **Nagłówek 1** opcji. W nagłówku, Dodaj tekst **strony pokaz części sieci Web**.
5. Z **składników Web Part** karcie przybornika, przeciągnij **WebPartManager** sterowania na stronę, umieszczając ją zaraz po znaku nowego wiersza i przed  **&lt;div&gt;**  tagów.   
  
 **WebPartManager** formantu nie jest renderowana żadnych danych wyjściowych, jest on wyświetlany jako okno szare na powierzchnię projektanta.
6. Umieść punkt wstawiania w ramach  **&lt;div&gt;**  tagów.
7. W **układu** menu, kliknij przycisk **Wstaw tabelę**i Utwórz nową tabelę, która ma jeden wiersz i trzy kolumny. Kliknij przycisk **właściwości komórki** przycisku Wybierz **górnej** z **wyrównanie w pionie** listy rozwijanej, kliknij przycisk **OK**i kliknij przycisk **OK** ponownie w celu utworzenia tabeli.
8. Przeciągnij formant WebPartZone do kolumny tabeli po lewej stronie. Kliknij prawym przyciskiem myszy **WebPartZone** sterowania, wybierz **właściwości**i ustaw następujące właściwości:   
  
 Identyfikator: SidebarZone   
  
 Właściwość HeaderText: paska bocznego
9. Przeciągnij drugi **WebPartZone** kontroli do kolumny tabeli Środkowej i ustaw następujące właściwości:   
  
 Identyfikator: MainZone   
  
 Właściwość HeaderText: Main
10. Zapisz plik.

Strony zawiera teraz dwa różne stref, które można kontrolować osobno. Jednak strefa ani nie ma żadnej zawartości, następnym krokiem jest utworzenie zawartości. W ramach tego przewodnika, możesz pracować z formantami składników Web Part, które zawierają tylko zawartość statyczną.

Układ strefy składników Web Part jest określona przez  **&lt;szablon zonetemplate&gt;**  elementu. Wewnątrz Szablon strefy żadnego formantu, ASP.NET, można dodać kontrolkę niestandardową części sieci Web, kontrolkę użytkownika lub istniejące kontrolki serwera. Zwróć uwagę, że się tutaj za pomocą formantu etykiety, a w tym przypadku po prostu dodawania tekst statyczny. Po umieszczeniu formantu regularne serwera w **WebPartZone** strefy, ASP.NET traktuje formant jako formant części sieci Web w czasie wykonywania, która umożliwia korzystanie z funkcji składników Web Part w formancie.

**Aby utworzyć zawartość głównym strefy**

1. W **projekt** wyświetlić, przeciągnij **etykiety** kontrolować z **standardowe** karcie przybornika do obszaru zawartości strefy których **identyfikator** właściwości ustawiono MainZone.
2. Przełącz się do **źródła** widoku. Zwróć uwagę, że  **&lt;szablon zonetemplate&gt;**  element został dodany do zakodowania **etykiety** kontrolki MainZone.
3. Dodaj atrybut o nazwie **tytuł** do  **&lt;asp: label&gt;**  element i ustaw dla niego wartość zawartości. Usuń tekst = atrybut "Label" z  **&lt;asp: label&gt;**  elementu. Pomiędzy otwierającym, a zamykającym tagiem elementu  **&lt;asp: label&gt;**  elementu, takie jak dodawanie tekst **powitalną wyświetloną stroną główną** w pary  **&lt;h2 &gt;**  znaczniki elementów. Kod powinien wyglądać w następujący sposób. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample21.aspx)]
4. Zapisz plik.

Następnie należy utworzyć kontrolkę użytkownika, który można również dodać do strony jako formant części sieci Web.

### <a name="to-create-a-user-control"></a>Aby utworzyć kontrolkę użytkownika

1. Dodaj kontrolkę użytkownika sieci Web do swojej witryny jako formant wyszukiwania. Odznacz opcję **umieścić w osobnym pliku kodu źródłowego**. Dodaj ją w tym samym katalogu co strona WebPartsDemo.aspx i nadaj mu nazwę SearchUserControl.ascx.   
  
    > [!NOTE]
    > Kontrola użytkownika w ramach tego przewodnika nie implementuje funkcje wyszukiwania rzeczywiste; służy jedynie, aby zademonstrować funkcje części sieci Web.
2. Przełącz się do **projekt** widoku. Z **standardowe** karcie przybornika, przeciągnij kontrolki pola tekstowego na stronie.
3. Umieść punkt wstawiania po pola tekstowego, który właśnie został dodany, a następnie naciśnij klawisz ENTER, aby dodać nowy wiersz.
4. Przeciągnij kontrolkę przycisku na stronie w nowym wierszu poniżej pola tekstowego, który właśnie został dodany.
5. Przełącz się do **źródła** widoku. Upewnij się, że kod źródłowy formant użytkownika wygląda jak w następującym przykładzie. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample22.aspx)]
6. Zapisz i zamknij plik.

Teraz można Dodaj formanty części sieci Web do strefy paska bocznego. Dwa formanty dodawane do strefy paska bocznego, zawierający listę łącza, a drugi zawierającą kontrolki użytkownika został utworzony w poprzedniej procedurze. Łącza są dodawane jako standard **etykiety** formantu serwera, podobnie jak utworzyć tekst statyczny dla strefy Main. Jednak mimo że formanty pojedynczego serwera zawarta w kontrolki użytkownika może znajdować się bezpośrednio w strefie (na przykład etykieta), w tym przypadku nie są. Zamiast tego są one częścią kontrolki użytkownika utworzonego w poprzedniej procedurze. Oznacza to często stosowana metoda pakietu niezależnie od formantów i dodatkowe funkcje w formancie użytkownika, a następnie odwołać się do tego formantu w strefie jako formant części sieci Web.

W czasie wykonywania zestaw kontroli części sieci Web koduje zarówno formantów za pomocą formantów elementu GenericWebPart. Gdy **elementu GenericWebPart** kontrolki koduje kontrolki serwera sieci Web, kontroli uniwersalnej części jest formant nadrzędny i kontrolki serwera są dostępne za pośrednictwem właściwości ChildControl kontrolki nadrzędnej. Standardowe formanty serwera sieci Web mają takie samo zachowanie podstawowe i atrybuty jako formantów składników Web Part, które pochodzą z umożliwia to korzystanie z formantów uniwersalnej części **składnika Web Part** klasy.

### <a name="to-add-web-parts-controls-to-the-sidebar-zone"></a>Aby dodać kontrolki części sieci Web do strefy paska bocznego

1. Otwórz stronę WebPartsDemo.aspx.
2. Przełącz się do **projekt** widoku.
3. Przeciągnij formant stronie użytkownika została utworzona, SearchUserControl.ascx, z **Eksploratora rozwiązań** do strefy których **identyfikator** właściwość ma ustawioną wartość SidebarZone i upuść go brak.
4. Zapisz stronę WebPartsDemo.aspx.
5. Przełącz się do **źródła** widoku.
6. Wewnątrz  **&lt;asp: webpartzone&gt;**  elementu SidebarZone, nad odwołanie do formantu użytkownika, Dodaj  **&lt;asp: label&gt;**  element z zawartych w niej łącza, jak pokazano w poniższym przykładzie. Ponadto Dodaj **tytuł** atrybut do tagu kontrolki użytkownika, z wartością **wyszukiwania**, jak pokazano. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample23.aspx)]
7. Zapisz i zamknij plik.

Teraz możesz przetestować strony przy użyciu przeglądania w przeglądarce. Na stronie są wyświetlane dwie strefy. Poniższy zrzut ekranu przedstawia strony.

**Strona pokaz części sieci Web z dwiema strefami**


![Zrzut ekranu wskazówki 1 VS części sieci Web](profiles-themes-and-web-parts/_static/image3.gif)

**Rysunek 3**: zrzut ekranu wskazówki 1 części programu VS w sieci Web


W tytule pasek każdej kontrolki jest strzałka w dół, zapewniający dostęp do menu zleceń dostępne akcje, które można wykonywać w formancie. Kliknij menu zleceń dla formantów, a następnie kliknij przycisk **Minimalizuj** zlecenie i należy pamiętać, że kontrolka jest zminimalizowany. W menu zleceń kliknij **przywrócić**, formantu powrót do normalnego rozmiaru.

### <a name="enabling-users-to-edit-pages-and-change-layout"></a>Umożliwienie użytkownikom do edytowania stron i zmiana układu

Części sieci Web zapewnia możliwość zmiany układu formantów składników Web Part, przeciągając je z jedną strefę na inny przez użytkowników. Oprócz umożliwienia użytkownikom przechodzenie **składnika Web Part** kontrolek z jednej strefy do innego, można zezwolić użytkownikom można edytować różne właściwości formantów, łącznie z ich wyglądu, układu i działania. Zestaw kontroli części sieci Web zapewnia podstawowe funkcje edycji dla **składnika Web Part** kontrolki. Mimo że w tym przewodniku nie będzie wykonywał tak, można również utworzyć niestandardowego edytora formantów, które umożliwia użytkownikom edytowanie funkcji **składnika Web Part** kontrolki. W przypadku zmiany lokalizacji **składnika Web Part** kontroli, edytowanie właściwości formantu zależy od personalizacji ASP.NET, aby zapisać zmiany, które użytkownicy wykonują.

W tej części przewodnika dodaniu przez użytkowników edytować podstawowymi charakterystykami dowolnego **składnika Web Part** formantu na stronie. Aby włączyć te funkcje, należy dodać innego użytkownika niestandardowego formantu do strony, wraz z  **&lt;asp: edytora editorzone&gt;**  elementu i dwóch formantów edycyjnych.

### <a name="to-create-a-user-control-that-enables-changing-page-layout"></a>Tworzenie formantu użytkownika, który umożliwia zmienianie układ strony

1. W programie Visual Studio na **pliku** menu, wybierz opcję **nowy** podmenu, a następnie kliknij przycisk **pliku** opcji.
2. W **Dodaj nowy element** okno dialogowe, wybierz opcję **kontrolka użytkownika sieci Web**. Nazwa nowego pliku DisplayModeMenu.ascx. Odznacz opcję **umieścić w osobnym pliku kodu źródłowego**.
3. Kliknij przycisk Dodaj, aby utworzyć nowy formant.
4. Przełącz się do **źródła** widoku.
5. Usuń istniejący kod w nowym pliku, a następnie wklej poniższy kod. Ten kod sterowania użytkownika używa funkcje zestaw kontroli części sieci Web, które umożliwiają strony zmienić jego widoku lub tryb wyświetlania i umożliwia także zmienić wygląd fizyczny i układ strony podczas pracy w niektórych trybów wyświetlania. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample24.aspx)]
6. Zapisz plik, klikając pozycję Zapisz ikonę na pasku narzędzi lub wybierając **zapisać** na **pliku** menu.

### <a name="to-enable-users-to-change-the-layout"></a>Aby umożliwić użytkownikom zmienianie układu

1. Otwórz stronę WebPartsDemo.aspx i przejdź do **projekt** widoku.
2. Umieść punkt wstawiania w **projekt** wyświetlić tuż po **WebPartManager** kontroli dodanego wcześniej. Dodaj końca linii po tekście, tak aby jest puste wiersze po **WebPartManager** formantu. Umieść kursor w pustym wierszu.
3. Przeciągnij kontrolki użytkownika właśnie utworzony (plik ma nazwę DisplayModeMenu.ascx) do WebPartsDemo.aspx strony i upuść ją na pusty wiersz.
4. Przeciągnij formant EditorZone z **składników Web Part** sekcji przybornika do pozostałych komórki otwartej tabeli na stronie WebPartsDemo.aspx.
5. Z **składników Web Part** sekcji przybornika, przeciągnij formant LayoutEditorPart w i kontroli AppearanceEditorPart **edytora EditorZone** formantu.
6. Przełącz się do **źródła** widoku. W komórce tabeli wynikowy kod powinien wyglądać podobnie do poniższego kodu. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample25.aspx)]
7. Zapisz plik WebPartsDemo.aspx. Utworzono kontrolkę użytkownika, która umożliwia zmianę trybów wyświetlania i zmianę układ strony i ma odwołanie do sterowania na głównej stronie sieci Web.

Teraz możesz przetestować możliwość edytowania strony i zmienić układ.

### <a name="to-test-layout-changes"></a>Aby przetestować zmiany układu

1. Ładowanie strony w przeglądarce.
2. Kliknij przycisk **tryb wyświetlania** menu rozwijane i wybierz **Edytuj**. Tytuły strefy są wyświetlane.
3. Przeciągnij **Moje łącza** sterowanie przy pasku tytułu ze strefy paska bocznego do dołu głównym strefy. Strony powinien wyglądać jak na poniższym zrzucie ekranu.

### <a name="web-parts-demo-page-with-my-links-control-moved"></a>Strony pokaz części sieci Web za pomocą formantu Moje łącza przenieść


![Zrzut ekranu wskazówki 2 VS części sieci Web](profiles-themes-and-web-parts/_static/image4.gif)

**Rysunek 4**: zrzut ekranu wskazówki 2 części programu VS w sieci Web


1. Kliknij przycisk **tryb wyświetlania** menu rozwijane i wybierz **Przeglądaj**. Strona zostanie odświeżona, nazwy stref znikają i **Moje łącza** kontrolować pozostaje gdzie ustawiony.
2. Aby zademonstrować, że personalizacja działa, zamknij przeglądarkę, a następnie ponownie załadować stronę. Wprowadzone zmiany są zapisywane w przeglądarce przyszłych sesji.
3. Z **tryb wyświetlania** menu, wybierz opcję **Edytuj**.   
  
 Każdej kontrolce na stronie zostaną wyświetlone w dół strzałkę na pasku tytułu, który zawiera menu rozwijanego zleceń.
4. Kliknij strzałkę, aby wyświetlić menu zleceń na **Moje łącza** formantu. Kliknij przycisk **Edytuj** zlecenia.   
  
 **Edytora EditorZone** formant jest widoczny, wyświetlanie EditorPart można dodać kontrolki.
5. W **wygląd** części kontrolki edycji zmiany **tytuł** moich ulubionych, użyj **Typ Chrome** listy rozwijanej, aby wybrać **tytuł tylko**, a następnie kliknij przycisk **Zastosuj**. Poniższy zrzut ekranu przedstawia stronę w trybie edycji.

### <a name="web-parts-demo-page-in-edit-mode"></a>Strona pokaz części sieci Web w trybie edycji


![Zrzut ekranu wskazówki 3 VS części sieci Web](profiles-themes-and-web-parts/_static/image5.gif)

**Rysunek 5**: zrzut ekranu wskazówki 3 części programu VS w sieci Web


1. Kliknij przycisk **tryb wyświetlania** menu, a następnie wybierz **Przeglądaj** aby powrócić do trybu przeglądania.
2. Kontrolka teraz ma zaktualizowany tytuł i obramowanie, jak pokazano na poniższym zrzucie ekranu.

### <a name="edited-web-parts-demo-page"></a>Edytowana strona pokaz części sieci Web


![Zrzut ekranu wskazówki 4 VS części sieci Web](profiles-themes-and-web-parts/_static/image6.gif)

**Rysunek 4**: zrzut ekranu wskazówki 4 części programu VS w sieci Web


### <a name="adding-web-parts-at-run-time"></a>Dodawanie składników Web Part w czasie wykonywania

Można również zezwolić użytkownikom na dodawanie formantów składników Web Part do strony ich w czasie wykonywania. Aby to zrobić, należy skonfigurować stronę z części sieci Web katalog, który zawiera listę formantów składników Web Part, które mają być dostępne dla użytkowników.

**Aby zezwolić użytkownikom na dodawanie składników Web Part w czasie wykonywania**

1. Otwórz stronę WebPartsDemo.aspx i przejdź do **projekt** widoku.
2. Z **składników Web Part** karcie przybornika, przeciągnij formant CatalogZone w prawej kolumnie tabeli, podrzędne **edytora EditorZone** formantu.   
  
 Oba formanty może być w tej samej komórce tabeli, ponieważ nie będą wyświetlane w tym samym czasie.
3. W okienku właściwości przypisać ciąg **Dodawanie składników Web Part** z właściwością HeaderText **CatalogZone** formantu.
4. Z **składników Web Part** sekcji przybornika, przeciągnij formant DeclarativeCatalogPart do obszaru zawartości **CatalogZone** formantu.
5. Kliknij strzałkę w prawym górnym rogu **DeclarativeCatalogPart** sterowania do udostępnienia jej menu zadania, a następnie wybierz **Edytuj szablony**.
6. Z **standardowe** sekcji przybornika, przeciągnij **przekazywaniem plików** kontroli i **kalendarza** sterowania do **WebPartsTemplate** sekcja **DeclarativeCatalogPart** formantu.
7. Przełącz się do **źródła** widoku. Sprawdź kod źródłowy  **&lt;asp: catalogzone&gt;**  elementu. Zwróć uwagę, że **DeclarativeCatalogPart** formant zawiera  **&lt;webpartstemplate&gt;**  element z dwóch formantów objętego serwera, które można dodać do strony z katalogu.
8. Dodaj **tytuł** właściwości każdej z formanty dodane do katalogu, przy użyciu wartości ciąg wyświetlany dla każdego tytułu w poniższym przykładzie kodu. Mimo że tytuł nie jest właściwością zwykle można ustawić tych kontrolek serwerowe w czasie projektowania, gdy użytkownik dodaje te formanty **WebPartZone** strefy z katalogu w czasie wykonywania, ich są każde jest ujęte w  **Elementu GenericWebPart** formantu. Umożliwia pełnienie formantów składników Web Part, będą oni mogli do wyświetlenia tytułów.   
  
 Kod dwa formanty zawarte w **DeclarativeCatalogPart** formantu powinna wyglądać w następujący sposób. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample26.aspx)]
9. Zapisz stronę.

Teraz możesz przetestować katalogu.

### <a name="to-test-the-web-parts-catalog"></a>Aby przetestować wykaz części sieci Web

1. Ładowanie strony w przeglądarce.
2. Kliknij przycisk **tryb wyświetlania** menu rozwijane i wybierz **katalogu**.   
  
 Wykaz zatytułowany **Dodawanie składników Web Part** jest wyświetlany.
3. Przeciągnij **moich ulubionych** kontroli ze strefy Main powrót do początku strefy paska bocznego i upuść go brak.
4. W **Dodawanie składników Web Part** katalogu, zaznacz pola wyboru, a następnie wybierz **Main** z listy rozwijanej, która zawiera dostępne strefy.
5. Kliknij przycisk **Dodaj** w katalogu. Formanty są dodawane do strefy Main. Jeśli chcesz, możesz dodać wiele wystąpień formantów z katalogu do strony.   
  
 Poniższy zrzut ekranu przedstawia strona zawierająca formant przekazywania plików i kalendarza w strefie Main. 

![Formanty dodawane do strefy Main z katalogu](profiles-themes-and-web-parts/_static/image7.gif)

    **Figure 5**: Controls added to Main zone from the catalog
6. Kliknij przycisk **tryb wyświetlania** menu rozwijane i wybierz **Przeglądaj**. Zniknie katalogu i odświeżeniu strony.
7. Zamknij przeglądarkę. Ponownie załadować stronę. Wprowadzone zmiany będą się powtarzać.
