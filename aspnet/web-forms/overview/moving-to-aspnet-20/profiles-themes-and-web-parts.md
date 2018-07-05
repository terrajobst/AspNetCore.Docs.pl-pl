---
uid: web-forms/overview/moving-to-aspnet-20/profiles-themes-and-web-parts
title: Profile, motywy i składniki Web Part | Dokumentacja firmy Microsoft
author: microsoft
description: Istnieją istotne zmiany w konfiguracji i instrumentacji w programie ASP.NET 2.0. Nowy interfejs API konfiguracji ASP.NET umożliwia na żądanie ściągnięcia wprowadzanie zmian w konfiguracji...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 92df4051-77c6-492c-bd34-23d24189cea4
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/profiles-themes-and-web-parts
msc.type: authoredcontent
ms.openlocfilehash: 505f03d339eebc871726fc5b3c385e595ad523d4
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37378459"
---
<a name="profiles-themes-and-web-parts"></a>Profile, motywy i składniki Web Part
====================
przez [firmy Microsoft](https://github.com/microsoft)

> Istnieją istotne zmiany w konfiguracji i instrumentacji w programie ASP.NET 2.0. Nowy interfejs API konfiguracji ASP.NET umożliwia programowe wprowadzone zmiany w konfiguracji. Ponadto istnieje wiele nowych ustawień konfiguracji umożliwiają nowe konfiguracje i instrumentacji.


Program ASP.NET 2.0 reprezentuje znaczną poprawę obszaru spersonalizowanych witryn sieci Web. Oprócz członkostwa weve funkcje opisane profilów programu ASP.NET, motywy i składniki Web Part znacznie poprawić personalizacji w witrynach sieci Web.

## <a name="aspnet-profiles"></a>Profile platformy ASP.NET

Profile programu ASP.NET są podobne do sesji. Różnica polega na profil jest trwały, natomiast sesji zostają utracone w przypadku, gdy nastąpi zamknięcie okna przeglądarki. Innego różnica między sesjami i profilów jest, że profile są silnie typizowane, dlatego dostarczanie możesz za pomocą funkcji IntelliSense podczas procesu projektowania.

Profil, który jest zdefiniowany w pliku konfiguracyjnym maszyny lub pliku web.config aplikacji. (Nie można zdefiniować profil w pliku web.config podfolderów.) Poniższy kod definiuje profilu, aby najpierw zapisać osoby odwiedzające witrynę sieci Web i nazwiska.

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample1.xml)]

Domyślny typ danych dla właściwości profilu jest System.String. W powyższym przykładzie został określony żaden typ danych. W związku z tym właściwości imię i nazwisko są typu ciąg. Jak wcześniej wspomniano, profilu, którego właściwości są silnie typizowane. Poniższy kod dodaje nową właściwość dla wiek, który jest typu Int32.

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample2.xml)]

Profile są zazwyczaj używane za pomocą uwierzytelniania formularzy programu ASP.NET. Gdy jest używana w połączeniu z uwierzytelniania formularzy, każdy użytkownik ma osobny profil skojarzony z swojego identyfikatora użytkownika. Jednak istnieje również możliwość korzystania z profilów w programie anonimowe aplikacji za pomocą &lt;anonymousIdentification&gt; elementu w pliku konfiguracji wraz z **allowAnonymous** atrybutu jako Poniżej pokazano:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample3.xml)]

Gdy użytkownik anonimowy przegląda lokacji, programu ASP.NET tworzy wystąpienie **ProfileCommon** dla użytkownika. Ten profil korzysta z unikatowego Identyfikatora przechowywane w pliku cookie w przeglądarce do identyfikowania użytkownika jako unikatowy obiekt odwiedzający. W ten sposób można przechowywać informacje o profilu dla użytkowników, którzy do przeglądania anonimowo.

## <a name="profile-groups"></a>Profil grupy

Istnieje możliwość właściwości grupy profilów. Grupowanie właściwości istnieje możliwość do symulacji wiele profilów dla określonej aplikacji.

Następująca konfiguracja konfiguruje właściwość Imię i nazwisko dwie grupy; Kupujący i perspektyw.

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample4.xml)]

Następnie jest możliwość ustawienia właściwości w określonej grupie w następujący sposób:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample5.cs)]

## <a name="storing-complex-objects"></a>Przechowywanie złożonych obiektów

Do tej pory przykłady, które Omówiliśmy zapisanych typy proste dane w profilu. Istnieje również możliwość przechowywania złożone typy danych w profilu, określając sposób za pomocą serializacji **serializeAs** atrybutu w następujący sposób:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample6.xml)]

W tym przypadku typ jest PurchaseInvoice. Klasa PurchaseInvoice musi być oznaczony jako możliwy do serializacji i może zawierać dowolną liczbę właściwości. Na przykład, jeśli PurchaseInvoice ma właściwość o nazwie **NumItemsPurchased**, można odwołać się do tej właściwości w kodzie w następujący sposób:

[!code-css[Main](profiles-themes-and-web-parts/samples/sample7.css)]

## <a name="profile-inheritance"></a>Dziedziczenie profilu

Istnieje możliwość utworzyć profil do użycia w wielu aplikacjach. Tworząc klasy profilu, która jest pochodną ProfileBase, można ponownie użyć profilu w kilku aplikacjach przy użyciu **dziedziczy** atrybutu, jak pokazano poniżej:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample8.xml)]

W tym przypadku klasy **PurchasingProfile** będzie wyglądać w następujący sposób:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample9.cs)]

## <a name="profile-providers"></a>Dostawcy profilu

Profile platformy ASP.NET przy użyciu modelu dostawcy. Domyślny dostawca przechowuje dane w bazie danych programu SQL Server Express w aplikacji\_folderu dane aplikacji sieci Web przy użyciu dostawcy SqlProfileProvider. Jeśli baza danych nie istnieje, ASP.NET automatycznie utworzy go podczas próby przechowywania informacji o profilu.

W niektórych przypadkach jednak warto opracowanie własnego dostawcy profilu. Funkcja profilu platformy ASP.NET umożliwia łatwe korzystanie z różnych dostawców.

Tworzenie niestandardowego dostawcy profilu po:

- Należy przechowywać informacje o profilu w źródle danych, takie jak w bazie danych FoxPro lub z bazą danych Oracle, który nie jest obsługiwany przez dostawców profilu dołączone do programu .NET Framework.
- Konieczne jest zarządzanie informacje o profilu przy użyciu schematu bazy danych, która różni się od schematu bazy danych używane przez dostawców dołączone do programu .NET Framework. Typowym przykładem jest o tym, że chcesz zintegrować informacje o profilu przy użyciu danych użytkownika w istniejącej bazy danych programu SQL Server.

### <a name="required-classes"></a>Wymagane klasy

Aby zaimplementować dostawcę profilu, należy utworzyć klasę, która dziedziczy z klasy abstrakcyjnej System.Web.Profile.ProfileProvider. **Dostawcę profilu** klasa abstrakcyjna dziedziczy z kolei klasa abstrakcyjna System.Configuration.SettingsProvider, która dziedziczy z klasy abstrakcyjnej System.Configuration.Provider.ProviderBase. Ze względu na to łańcuch dziedziczenia, oprócz wymaganych elementów członkowskich **dostawcę profilu** klasy należy zaimplementować wymaganych elementów członkowskich **SettingsProvider** i  **ProviderBase** klasy.

W poniższych tabelach opisano właściwości i metod, które należy zaimplementować z **ProviderBase**, **SettingsProvider**, i **dostawcę profilu** abstrakcyjne klasy.

### <a name="providerbase-members"></a>Elementy członkowskie ProviderBase

| **Element członkowski** | **Opis** |
| --- | --- |
| Initialize — Metoda | Przyjmuje jako dane wejściowe nazwę wystąpienia dostawcy i elementu NameValueCollection ustawień konfiguracji. Używane do ustawiania opcji i wartości właściwości wystąpienia dostawcy, w tym specyficzne dla implementacji wartości i opcje określone w konfiguracji komputera lub w pliku Web.config. |

### <a name="settingsprovider-members"></a>Elementy członkowskie SettingsProvider

| **Element członkowski** | **Opis** |
| --- | --- |
| Właściwość ApplicationName | Nazwa aplikacji jest przechowywany z każdym profilem. Dostawca profilu używa nazwy aplikacji, do przechowywania informacji o profilu osobno dla każdej aplikacji. Dzięki temu wiele aplikacji programu ASP.NET użyć tego samego źródła danych bez powodowania konfliktów, jeśli ta sama nazwa użytkownika została utworzona w innych aplikacjach. Alternatywnie wiele aplikacji programu ASP.NET można udostępniać źródła danych profilu, określając jedną nazwą aplikacji. |
| Metoda GetPropertyValues | Przyjmuje jako danymi wejściowymi SettingsContext i SettingsPropertyCollection obiektu. **SettingsContext** zawiera informacje o użytkowniku. Informacje można użyć jako klucza podstawowego można pobrać informacji o właściwości profilu użytkownika. Użyj **SettingsContext** obiekt, aby uzyskać nazwę użytkownika i tego, czy użytkownik jest uwierzytelnieni lub anonimowi. **SettingsPropertyCollection** zawiera kolekcję obiektów SettingsProperty. Każdy **SettingsProperty** obiekt zawiera nazwę i typ właściwości, a także dodatkowe informacje, np. wartością domyślną dla właściwości oraz tego, czy właściwość jest tylko do odczytu. **GetPropertyValues** metoda wypełnia SettingsPropertyValue obiektów na podstawie SettingsPropertyValueCollection **SettingsProperty** obiektów podana jako dane wejściowe. Wartości ze źródła danych dla określonego użytkownika są przypisywane do właściwości PropertyValue dla każdego **SettingsPropertyValue** jest zwracany obiekt i całą kolekcję. Wywołanie metody aktualizuje również wartość LastActivityDate profilu określonego użytkownika do bieżącej daty i godziny. |
| Metoda SetPropertyValues | Przyjmuje jako dane wejściowe **SettingsContext** i **SettingsPropertyValueCollection** obiektu. **SettingsContext** zawiera informacje o użytkowniku. Informacje można użyć jako klucza podstawowego można pobrać informacji o właściwości profilu użytkownika. Użyj **SettingsContext** obiekt, aby uzyskać nazwę użytkownika i tego, czy użytkownik jest uwierzytelnieni lub anonimowi. **SettingsPropertyValueCollection** zawiera zbiór **SettingsPropertyValue** obiektów. Każdy **SettingsPropertyValue** obiekt zawiera nazwę, typ i wartość właściwości, a także dodatkowe informacje, np. wartością domyślną dla właściwości oraz tego, czy właściwość jest tylko do odczytu. **SetPropertyValues** metoda aktualizuje wartości właściwości profilu w źródle danych dla określonego użytkownika. Wywołanie metody również aktualizacje **LastActivityDate** i wartości LastUpdatedDate profilu określonego użytkownika, aby bieżącą datę i godzinę. |

### <a name="profileprovider-members"></a>Elementy członkowskie dostawcę profilu

| **Element członkowski** | **Opis** |
| --- | --- |
| Metoda DeleteProfiles | Przyjmuje jako dane wejściowe tablicy ciągów użytkownika nazwy i spowoduje usunięcie wszystkich profilu informacji i wartości właściwości dla określonej nazwy ze źródła danych, jeśli nazwa aplikacji odpowiada **ApplicationName** wartości właściwości. Jeśli źródło danych obsługuje transakcje, zaleca się obejmują wszystkie operacje usuwania w transakcji i Wycofaj tę transakcję i zgłosić wyjątek, jeśli żadnych operacji usuwania nie powiedzie się. |
| Metoda DeleteProfiles | Przyjmuje jako dane wejściowe zbiór ProfileInfo obiektów i spowoduje usunięcie wszystkich profilu informacji i wartości właściwości dla każdego profilu ze źródła danych, gdzie nazwa aplikacji jest zgodna **ApplicationName** wartości właściwości. Jeśli źródło danych obsługuje transakcje, zaleca się obejmują wszystkie operacje usuwania w transakcji i Wycofaj tę transakcję i zgłosić wyjątek, jeśli żadnych operacji usuwania nie powiedzie się. |
| Metoda DeleteInactiveProfiles | Przyjmuje jako dane wejściowe wartość ProfileAuthenticationOption obiektu typu DateTime i usuwa z danych źródłowych wszystkie informacje o profilu i wartości właściwości, gdzie Data ostatniej aktywności jest mniejsza niż lub równa określonej daty i godziny i gdzie nazwa aplikacji Dopasowuje **ApplicationName** wartości właściwości. **ProfileAuthenticationOption** parametr określa, czy tylko anonimowy profile uwierzytelniani tylko profile, czy wszystkie profile do usunięcia. Jeśli źródło danych obsługuje transakcje, zaleca się obejmują wszystkie operacje usuwania w transakcji i Wycofaj tę transakcję i zgłosić wyjątek, jeśli żadnych operacji usuwania nie powiedzie się. |
| Metoda GetAllProfiles | Przyjmuje jako dane wejściowe **ProfileAuthenticationOption** wartość, liczba całkowita określająca indeks strony, liczba całkowita określająca rozmiar strony, a odwołanie do liczba całkowita, która jest równa łącznej liczby profilów. Zwraca ProfileInfoCollection, który zawiera **ProfileInfo** obiektów we wszystkich profilach w źródle danych, w którym nazwa aplikacji jest zgodna **ApplicationName** wartości właściwości. **ProfileAuthenticationOption** parametr określa, czy tylko anonimowy profile uwierzytelniani tylko profile, lub wszystkie profile, które mają zostać zwrócone. Wyniki zwrócone przez **GetAllProfiles** metody są ograniczone przez indeks strony i wartości rozmiaru strony. Wartość rozmiaru strony określa maksymalną liczbę **ProfileInfo** obiekty do zwrócenia w **ProfileInfoCollection**. Strony wyników do zwrócenia, gdzie 1 identyfikuje pierwsza strona, która określa, wartość indeks strony. Parametr łącznie rekordów jest parametrem wyjściowym (możesz użyć **ByRef** w języku Visual Basic), jest ustawiona na całkowitą liczbę profilów. Na przykład, jeśli magazyn danych zawiera 13 profile aplikacji, a wartość indeksu strony to 2, 5, o rozmiarze strony **ProfileInfoCollection** zwracana zawiera od szóstego do dziesiątego profilów. Gdy metoda zwróci wartość, wartość całkowita liczba rekordów jest ustawiona na 13. |
| Metoda GetAllInactiveProfiles | Przyjmuje jako dane wejściowe **ProfileAuthenticationOption** wartość **daty/godziny** obiektu, liczba całkowita określająca indeks strony, liczba całkowita określająca rozmiar strony, a odwołanie na liczbę całkowitą, która zostanie ustawiona Aby łączna liczba profilów. Zwraca **ProfileInfoCollection** zawierający **ProfileInfo** obiektów we wszystkich profilach w źródle danych, gdzie Data ostatniej aktywności jest mniejsza lub równa określonej **daty/godziny**  , jeśli nazwa aplikacji odpowiada **ApplicationName** wartości właściwości. **ProfileAuthenticationOption** parametr określa, czy tylko anonimowy profile uwierzytelniani tylko profile, lub wszystkie profile, które mają zostać zwrócone. Wyniki zwrócone przez **GetAllInactiveProfiles** metody są ograniczone przez indeks strony i wartości rozmiaru strony. Wartość rozmiaru strony określa maksymalną liczbę **ProfileInfo** obiekty do zwrócenia w **ProfileInfoCollection**. Strony wyników do zwrócenia, gdzie 1 identyfikuje pierwsza strona, która określa, wartość indeks strony. Parametr łącznie rekordów jest parametrem wyjściowym (możesz użyć **ByRef** w języku Visual Basic), jest ustawiona na całkowitą liczbę profilów. Na przykład, jeśli magazyn danych zawiera 13 profile aplikacji, a wartość indeksu strony to 2, 5, o rozmiarze strony **ProfileInfoCollection** zwracana zawiera od szóstego do dziesiątego profilów. Gdy metoda zwróci wartość, wartość całkowita liczba rekordów jest ustawiona na 13. |
| Metoda FindProfilesByUserName | Przyjmuje jako dane wejściowe **ProfileAuthenticationOption** wartość ciąg zawierający nazwę użytkownika, liczba całkowita określająca indeks strony, liczba całkowita określająca rozmiar strony, a odwołanie na liczbę całkowitą, która jest równa łącznej liczby Profile. Zwraca **ProfileInfoCollection** zawierający **ProfileInfo** obiektów dla wszystkich profilów w danych źródła, gdzie nazwa użytkownika jest zgodna z określoną nazwą użytkownika, jeśli odpowiada nazwę aplikacji **ApplicationName** wartości właściwości. **ProfileAuthenticationOption** parametr określa, czy tylko anonimowy profile uwierzytelniani tylko profile, lub wszystkie profile, które mają zostać zwrócone. Jeśli źródło danych obsługuje możliwości wyszukiwania dodatkowe, takie jak znaki symboli wieloznacznych, możesz podać bardziej rozbudowane możliwości wyszukiwania dla nazwy użytkownika. Wyniki zwrócone przez **FindProfilesByUserName** metody są ograniczone przez indeks strony i wartości rozmiaru strony. Wartość rozmiaru strony określa maksymalną liczbę **ProfileInfo** obiekty do zwrócenia w **ProfileInfoCollection**. Strony wyników do zwrócenia, gdzie 1 identyfikuje pierwsza strona, która określa, wartość indeks strony. Parametr łącznie rekordów jest parametrem wyjściowym (możesz użyć **ByRef** w języku Visual Basic), jest ustawiona na całkowitą liczbę profilów. Na przykład, jeśli magazyn danych zawiera 13 profile aplikacji, a wartość indeksu strony to 2, 5, o rozmiarze strony **ProfileInfoCollection** zwracana zawiera od szóstego do dziesiątego profilów. Gdy metoda zwróci wartość, wartość całkowita liczba rekordów jest ustawiona na 13. |
| FindInactiveProfilesByUserName method | Przyjmuje jako dane wejściowe **ProfileAuthenticationOption** wartość, ciąg zawierający nazwę użytkownika **daty/godziny** obiektu, liczba całkowita określająca indeks strony, liczba całkowita określająca rozmiar strony, a Odwołanie do liczba całkowita, która jest równa łącznej liczby profilów. Zwraca **ProfileInfoCollection** zawierający **ProfileInfo** obiektów we wszystkich profilach w źródle danych, jeśli nazwa użytkownika odpowiada określonej nazwy użytkownika, gdzie Data ostatniej aktywności jest mniejsza niż lub równa określonej **daty/godziny**, i jeśli nazwa aplikacji odpowiada **ApplicationName** wartości właściwości. **ProfileAuthenticationOption** parametr określa, czy tylko anonimowy profile uwierzytelniani tylko profile, lub wszystkie profile, które mają zostać zwrócone. Jeśli źródło danych obsługuje możliwości wyszukiwania dodatkowe, takie jak znaki symboli wieloznacznych, możesz podać bardziej rozbudowane możliwości wyszukiwania dla nazwy użytkownika. Wyniki zwrócone przez **FindInactiveProfilesByUserName** metody są ograniczone przez indeks strony i wartości rozmiaru strony. Wartość rozmiaru strony określa maksymalną liczbę **ProfileInfo** obiekty do zwrócenia w **ProfileInfoCollection**. Strony wyników do zwrócenia, gdzie 1 identyfikuje pierwsza strona, która określa, wartość indeks strony. Parametr łącznie rekordów jest parametrem wyjściowym (możesz użyć **ByRef** w języku Visual Basic), jest ustawiona na całkowitą liczbę profilów. Na przykład, jeśli magazyn danych zawiera 13 profile aplikacji, a wartość indeksu strony to 2, 5, o rozmiarze strony **ProfileInfoCollection** zwracana zawiera od szóstego do dziesiątego profilów. Gdy metoda zwróci wartość, wartość całkowita liczba rekordów jest ustawiona na 13. |
| Metoda GetNumberOfInActiveProfiles | Przyjmuje jako dane wejściowe **ProfileAuthenticationOption** wartość i **daty/godziny** obiektu i zwraca liczbę wszystkich profili w źródle danych, gdzie jest mniejsza niż określony datęostatniejaktywności **Data i godzina** , jeśli nazwa aplikacji odpowiada **ApplicationName** wartości właściwości. **ProfileAuthenticationOption** parametr określa, czy tylko anonimowy profile uwierzytelniani tylko profile, czy wszystkie profile do zliczenia. |

### <a name="applicationname"></a>ApplicationName

Ponieważ dostawcach profilów przechowywać informacje o profilu osobno dla każdej aplikacji, upewnij się że schematu danych zawiera nazwę aplikacji i zapytań i aktualizacji również obejmować nazwę aplikacji. Na przykład następujące polecenie służy do pobierania wartości właściwości z bazy danych na podstawie nazwy użytkownika i tego, czy profil, który jest anonimowa i zapewnia, że **ApplicationName** wartość jest uwzględniona w zapytaniu.

[!code-sql[Main](profiles-themes-and-web-parts/samples/sample10.sql)]

## <a name="aspnet-themes"></a>Motywów programu ASP.NET

## <a name="what-are-aspnet-20-themes"></a>Co to jest ASP.NET 2.0 motywy?

Jednym z najważniejszych aspektów aplikacji sieci Web jest spójny wygląd i zachowanie całej witryny. ASP.NET 1.x Deweloperzy zazwyczaj umożliwia kaskadowych arkuszy stylów (CSS) implementuje spójny wygląd i zachowanie. Program ASP.NET 2.0 motywy znacznie ulepszyć CSS, ponieważ zapewniają programowaniem w technologii ASP.NET możliwość określenia jej wyglądu kontrolki serwera ASP.NET, a także elementów HTML. Motywów programu ASP.NET może być stosowane do poszczególnych formantów, określona strona sieci Web lub całej aplikacji sieci Web. Motywy użyć kombinacji plików CSS, plik skin opcjonalne i opcjonalnie katalog obrazów, jeśli potrzebne są obrazy. Plik skórki kontroluje wygląd formanty serwera ASP.NET.

## <a name="where-are-themes-stored"></a>Gdzie są przechowywane motywy?

Lokalizację przechowywania motywy różnią się w zależności od ich zakres. Motywy, które mogą być stosowane do dowolnej aplikacji są przechowywane w następującym folderze:

`C:\WINDOWS\Microsoft.NET\Framework\v2.x.xxxxx\ASP.NETClientFiles\Themes\<Theme_Name>`

Motyw, które są specyficzne dla określonej aplikacji jest przechowywany w `App\_Themes\<Theme\_Name>` katalogu w katalogu głównym witryny sieci Web.

> [!NOTE]
> Plik skórki powinny być modyfikowane tylko właściwości kontrolki serwera, które wpływają na wygląd.

Motyw globalny jest motyw, który można zastosować do dowolnej aplikacji lub witryny sieci Web uruchomiony na serwerze sieci Web. Motywy te są domyślnie przechowywane w katalogu ASP.NETClientfiles\Themes wewnątrz katalogu v2.x.xxxxx. Alternatywnie, można umieścić pliki motyw do aspnet\_system klienta/\_sieci web / /Themes/ [wersja] [motyw\_name] folder w katalogu głównym witryny sieci Web.

Motywy specyficzne dla aplikacji będzie stosowany tylko do aplikacji, w którym znajdują się pliki. Te pliki są przechowywane w `App\_Themes/<theme\_name>` katalogu w katalogu głównym witryny sieci Web.

## <a name="the-components-of-a-theme"></a>Składniki motywu

Motyw składa się z co najmniej jednego pliku CSS, plik skin opcjonalne i opcjonalny folder obrazów. Pliki CSS może być dowolną nazwę musi znajdować się w katalogu głównym folderu, kompozycje i uzupełniać (tj. default.css lub theme.css itp.). Pliki CSS są używane do definiowania zwykłych klas CSS oraz atrybutów dla określonych selektorów. Aby zastosować jedną z klas CSS do elementu strony **CSSClass** właściwość jest używana.

Plik skórki jest plik XML, który zawiera definicje właściwości formantów serwera ASP.NET. Kod przedstawiony poniżej jest przykładowy plik skin.

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample11.aspx)]

**Rysunek 1** poniżej przedstawia małe strony ASP.NET przeglądania bez zastosowania motywu. **Rysunek 2** pokazuje tego samego pliku przy użyciu motyw. Kolor tła i kolor tekstu są skonfigurowane przy użyciu pliku CSS. Wygląd przycisk i pole tekstowe są skonfigurowane przy użyciu plik skin wymienionych powyżej.


![Brak motywu](profiles-themes-and-web-parts/_static/image1.gif)

**Rysunek 1**: Brak motywu


![Motyw](profiles-themes-and-web-parts/_static/image2.gif)

**Rysunek 2**: motyw


Plik skórki wymienionych powyżej definiuje powłoki domyślnego dla wszystkich pól tekstowych i formanty przycisków. Oznacza to, czy każdy formant pola tekstowego i formant przycisku wstawione na stronie spowoduje przejście na wygląd. Można również zdefiniować skórki, który można zastosować do konkretnych wystąpień tych kontrolek przy użyciu **wartość SkinID** właściwości formantu.

Poniższy kod definiuje powłoki dla kontrolki przycisku. Formanty tylko przycisków z **wartość SkinID** właściwość **goButton** spowoduje przejście na wygląd skórki.

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample12.aspx)]

Może mieć tylko jedną skórki domyślne na serwerze — typ formantu. Jeśli potrzebujesz dodatkowych skórki, należy użyć właściwości wartość SkinID.

## <a name="applying-themes-to-pages"></a>Stosowanie motywów do stron

Motyw można zastosować za pomocą jednej z następujących metod:

- W &lt;stron&gt; elementu w pliku web.config
- W @Page dyrektywy strony
- Programowe

## <a name="applying-a-theme-in-the-configuration-file"></a>Zastosowanie motywu w pliku konfiguracji

Aby zastosować motyw w pliku konfiguracyjnym aplikacji, użyj następującej składni:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample13.xml)]

Nazwa motywu, określone w tym miejscu musi odpowiadać nazwie folderu motywów. Ten folder może znajdować się w jednej z lokalizacji wymienionych wcześniej w ramach tego kursu. Jeśli spróbujesz zastosować motyw, który nie istnieje, wystąpi błąd konfiguracji.

## <a name="applying-a-theme-in-the-page-directive"></a>Zastosowanie motywu w dyrektywie Page

Można także zastosować motywu w dyrektywy @ Page. Ta metoda umożliwia motyw dla określonej strony.

Aby zastosować motyw w @Page dyrektywy, należy użyć następującej składni:

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample14.aspx)]

Jeszcze raz motyw określone w tym miejscu musi być zgodna folderu motywu, jak wspomniano wcześniej. Jeśli spróbujesz zastosować motyw, który nie istnieje, wystąpi błąd kompilacji. Visual Studio będzie również zaznacz atrybut i informujące o tym, czy brak takiego motywu, istnieje.

## <a name="applying-a-theme-programmatically"></a>Programowe zastosowanie motywu

Aby zastosować motyw programowo, należy określić **motyw** właściwości dla strony w **strony\_PreInit** metody.

Aby zastosować motyw programowo, użyj następującej składni:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample15.cs)]

Jest to konieczne zastosować motyw w metodzie PreInit z powodu cyklu życia strony. Jeśli zostanie zastosowane po tym punkcie, motywu strony będzie zostały już zastosowane przez środowisko uruchomieniowe i zmiany w tym momencie jest za późno w cyklu życia. Jeśli zastosujesz motywu, który nie istnieje, **httpexception —** występuje. Jeśli motyw jest stosowany programowo, ostrzeżenia kompilacji wystąpi, jeśli wszystkie formanty serwera ma wartość SkinID właściwości. To ostrzeżenie jest przeznaczona do informujące, że nie motyw jest stosowany do deklaratywnie i można go zignorować.

## <a name="exercise-1--applying-a-theme"></a>Ćwiczenie 1: Zastosowanie motywu

W tym ćwiczeniu zastosuje motyw programu ASP.NET w witrynie sieci Web.

> [!IMPORTANT]
> Jeśli używasz programu Microsoft Word do wprowadzania informacji w plik skin, upewnij się, że nie zastępujesz regularne ofert z inteligentne cudzysłowy. Inteligentne cudzysłowy spowoduje, że problemy z plikami skórki.

1. Utwórz nową witrynę sieci Web platformy ASP.NET.
2. Kliknij prawym przyciskiem myszy projekt w Eksploratorze rozwiązań i wybierz polecenie Dodaj nowy element.
3. Wybierz plik konfiguracji sieci Web z listy plików, a następnie kliknij przycisk Dodaj.
4. Kliknij prawym przyciskiem myszy projekt w Eksploratorze rozwiązań i wybierz polecenie Dodaj nowy element.
5. Wybierz plik Skin, a następnie kliknij przycisk Dodaj.
6. Kliknij przycisk Tak, po wyświetleniu monitu, jeśli chcesz umieścić plik w aplikacji youd\_folder motywów.
7. Kliknij prawym przyciskiem myszy w folderze SkinFile wewnątrz aplikacji\_folder motywów w Eksploratorze rozwiązań i wybierz polecenie Dodaj nowy element.
8. Wybierz arkusz stylów z listy plików, a następnie kliknij przycisk Dodaj. Masz teraz wszystkie pliki niezbędne do zaimplementowania nowy motyw. Jednak program Visual Studio przyznała SkinFile folderze motywów. Kliknij prawym przyciskiem myszy, w tym folderze, a następnie zmień nazwę na CoolTheme.
9. Otwórz plik SkinFile.skin i Dodaj następujący kod na końcu pliku: 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample16.aspx)]
10. Zapisz plik SkinFile.skin.
11. Otwórz StyleSheet.css.
12. Zastąp cały tekst w nim następujące czynności: 

    [!code-css[Main](profiles-themes-and-web-parts/samples/sample17.css)]
13. Zapisz plik StyleSheet.css.
14. Otwórz stronę Default.aspx.
15. Dodaj formant TextBox i formant przycisku.
16. Zapisz stronę. Teraz przejdź do strony Default.aspx. Powinna zostać wyświetlona jako normalny formularz sieci Web.
17. Otwórz plik web.config.
18. Dodaj następujący kod bezpośrednio pod otwarcia `<system.web>` tag: 

    [!code-xml[Main](profiles-themes-and-web-parts/samples/sample18.xml)]
19. Zapisz plik web.config. Teraz przejdź do strony Default.aspx. Powinna zostać wyświetlona z motywem stosowane.
20. Jeśli nie jest jeszcze otwarty, otwórz stronę Default.aspx w programie Visual Studio.
21. Wybierz przycisk.
22. Zmiana **wartość SkinID** właściwość goButton. Należy zauważyć, że program Visual Studio udostępnia listę rozwijaną prawidłowymi wartościami wartość SkinID dla kontrolki przycisku.
23. Zapisz stronę. Teraz ponownie Wyświetl podgląd strony w przeglądarce. Przycisk teraz powinna być widoczna nazwa "Przejdź" i powinna być szersze wygląd.

Za pomocą **wartość SkinID** właściwości, można łatwo skonfigurować różne skórek dotyczy to różnych wystąpień danego typu formantem serwera.

## <a name="the-stylesheettheme-property"></a>Właściwość StyleSheetTheme

Do tej pory zajmowaliśmy tylko stosowanie motywów przy użyciu właściwości motywu. Korzystając z właściwości motywu, plik skin spowoduje przesłonięcie ustawienia deklaratywne kontrolek serwerowych. Na przykład w ćwiczeniu 1 określona wartość SkinID elementu "goButton" dla przycisku kontroli i zmieniło tekstu przycisku "Przejdź". Być może zauważono, że właściwość tekst przycisku w Projektancie została ustawiona na "Button", ale overrode motywu, który. Motyw zawsze spowoduje zastąpienie wszelkich ustawień właściwości w projektancie.

Jeśli chcesz można było zastąpienie właściwości zdefiniowane w pliku skórki motywu za pomocą właściwości określone w projektancie, możesz użyć **StyleSheetTheme** właściwości zamiast właściwości motywu. Właściwość StyleSheetTheme jest taka sama jak właściwość motywu, z tą różnicą, że nie zastępuje wszystkie ustawienia jawną właściwość, takie jak właściwości motywu jest.

Aby to zobaczyć w działaniu, otwórz plik web.config z projektu w ćwiczeniu 1 i zmień `<pages>` element do następującego:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample19.xml)]

Teraz przejdź strony Default.aspx, a zobaczysz, że formant przycisku ponownie ma właściwość tekst "Button". Wynika to z właściwością Text goButton wartość SkinID przesłania ustawienie właściwości jawne, w projektancie.

## <a name="overriding-themes"></a>Zastępowanie motywów

Motyw globalny może zostać przesłonięta przez zastosowanie motywu o tej samej nazwie w aplikacji\_folder motywów aplikacji. Motyw nie jest stosowana w przypadku zastąpienia wartość true. Jeśli środowisko wykonawcze napotka plików w aplikacji\_folder motywów zastosuje motyw przy użyciu tych plików i będzie ignorować motyw globalny.

Właściwość StyleSheetTheme jest możliwym do zastąpienia i może zostać przesłonięta w kodzie w następujący sposób:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample20.cs)]

## <a name="web-parts"></a>Części sieci Web

Części sieci Web platformy ASP.NET jest zintegrowany zestaw formantów do tworzenia witryn sieci Web, które umożliwiają użytkownikom końcowym zmodyfikować zawartość, wygląd i zachowanie stron sieci Web bezpośrednio w przeglądarce. Zmiany można zastosować do wszystkich użytkowników w witrynie lub pojedynczych użytkowników. Gdy użytkownicy zmodyfikują stron i kontrolek, można zapisać ustawienia i zachować osobistych preferencji użytkownika w wielu sesjach przeglądarki w przyszłości, funkcja o nazwie personalizacji. Te funkcje składników Web Part oznacza dać użytkownikom końcowym personalizowanie aplikacji sieci Web dynamicznie, bez konieczności interwencji deweloperowi lub administratorowi deweloperów.

Za pomocą zestaw formantów części sieci Web, jako programista umożliwia użytkownikom końcowym:

- Personalizacja zawartości strony. Użytkowników można Dodawanie nowych formantów składników Web Part do strony, usuń je, je ukryć lub zminimalizować je jak zwykły system windows.
- Spersonalizuj układu strony. Użytkowników można przeciągać formantów Web Part do innej strefy na stronie, lub zmienić wygląd, właściwości i zachowanie.
- Eksportowanie i importowanie kontrolki. Użytkownicy mogą importować lub eksportować składników Web Part ustawienia kontroli do użytku w innych stron lub witryn, zachowując właściwości, wygląd i nawet danych w kontrolkach. Zmniejsza to wzrostu wejścia i konfiguracji danych użytkowników końcowych.
- Tworzenie połączeń. Użytkownikom można ustanowić połączenia między kontrolkami, tak, aby na przykład formant wykresu można wyświetlić wykres dla danych w kontrolce giełdowej. Użytkownicy mogą personalizować nie tylko połączenie, ale wygląd i szczegóły jak formant wykresu Wyświetla określone dane.
- Zarządzanie i Personalizuj ustawienia na poziomie witryny. Autoryzowani użytkownicy można skonfigurować ustawienia na poziomie witryny, określić, kto dostęp do strony lub witryny, ustaw opartej na rolach dostęp do kontrolek i tak dalej. Na przykład użytkownik w roli administracyjnej może zestawu formantów Web Part być współużytkowane przez wszystkich użytkowników i uniemożliwić użytkownikom, którzy nie są administratorami personalizowanie wspólna kontrola.

Programy zwykle współdziałają ze składnikami Web Part w jeden z trzech sposobów: tworzenie stron używające formantów części sieci Web, tworzenie pojedynczych formantów części sieci Web lub tworzenia aplikacji sieci Web pełną, personalizowalne, takie jak portal.

## <a name="page-development"></a>Tworzenie stron

Strona deweloperzy mogą używać narzędzi projektowania wizualnego, takich jak Microsoft Visual Studio 2005, do tworzenia stron, które używają składników Web Part. Jedną z zalet przy użyciu narzędzia, takiego jak Visual Studio jest zestaw formantów części sieci Web oferuje funkcje przeciągania i upuszczania, tworzenie i konfigurowanie formantów składników Web Part w projektanta wizualnego. Na przykład można przeciągnąć strefy składników Web Part lub kontrolka edytora składników Web Part na powierzchni projektowej za pomocą projektanta, a następnie skonfiguruj kontrolkę z prawej zestaw formantów w projektancie, za pomocą interfejsu użytkownika, dostarczone przez składniki Web Part. Może to przyspieszyć rozwój aplikacji Web Part i zmniejszyć ilość kodu, który trzeba napisać.

## <a name="control-development"></a>Tworzenie formantów

Dowolny formant ASP.NET, istniejące służy jako kontrola części sieci Web, w tym standardowe formanty serwera sieci Web, niestandardowych kontrolek serwera i kontrolki użytkownika. Dla maksymalnej programistyczną kontrolę środowiska można również utworzyć niestandardowe formanty składników Web Part, które pochodzą z klasy składnika Web Part. Dla poszczególnych Tworzenie formantów części sieci Web można będzie zazwyczaj utworzyć kontrolkę użytkownika i używać go jako kontrola części sieci Web albo tworzenie niestandardowych formantów Web Part.

Na przykład tworzenia niestandardowych formantów Web Part można utworzyć formant do żadnych funkcji oferowanych przez inne formanty serwera ASP.NET, które mogą być przydatne do pakietu jako personalizowalne formantów Web Part: kalendarze, listy, informacji finansowych wiadomości, kalkulatory, kontrolek tekstu sformatowanego aktualizowania zawartości, można edytować siatki które łączą się z bazami danych, wykresy, które są dynamicznie zaktualizować ich Wyświetla pogodowe lub przesyłane informacje. Jeśli podasz projektanta wizualnego, za pomocą kontrolki, każdy Deweloper strony za pomocą programu Visual Studio można po prostu przeciągnij formant do strefy składników Web Part i konfigurować w czasie projektowania, bez konieczności pisania dodatkowego kodu.

Personalizacja jest podstawą funkcji składników Web Part. Umożliwia użytkownikom zmodyfikować — lub spersonalizować — układ, wygląd i zachowanie formantów składników Web Part na stronie. Spersonalizowane ustawienia są długotrwałe: zostaną utrwalone nie tylko podczas bieżącej sesji przeglądarki (tak jak stan widoku), ale także w pamięci nieulotnej tak, aby ustawienia użytkownika są zapisywane dla sesji przeglądarki przyszłych także. Personalizacja jest domyślnie włączone dla strony części sieci Web.

Elementy strukturalne interfejsu użytkownika zależą od personalizacji i świadczenie podstawowej struktury i wymagane przez wszystkie formanty części sieci Web. Jeden składnik strukturalnych interfejsu użytkownika na każdej stronie składników Web Part wymagane jest formant WebPartManager. Mimo że to nigdy nie jest widoczny, ten formant ma zadanie krytyczne koordynacji wszystkie formanty składników Web Part na stronie. Na przykład śledzi wszystkie pojedynczych formantów części sieci Web. Zarządza strefy Web Part (regiony, które zawierają formanty składników Web Part na stronie), które są strefy, które środki. Ponadto śledzi i kontroluje trybów wyświetlania, które może się znajdować strona w takich jak przeglądanie, łączenie, edytować lub katalogu tryb i czy zmiany personalizacji są stosowane do wszystkich użytkowników lub pojedynczych użytkowników. Na koniec inicjuje i śledzi połączenia i komunikacji między formantów składników Web Part.

Drugi typ strukturalnych składnik interfejsu użytkownika jest strefa. Strefy pełnić rolę menedżerów układu strony składników Web Part. Mogą zawierać organizują formanty, które pochodzą z klasy części (part formantów) i zapewniają możliwość robienia układ strony moduły w orientacji poziomej lub pionowej. Strefy udostępniają także wspólny i spójny elementy interfejsu użytkownika (takie jak style nagłówków i stopek, tytuł, styl obramowania, przyciski akcji i tak dalej) dla każdej kontrolki, które zawierają; wspólne elementy są nazywane chrome kontrolki. Kilka typów wyspecjalizowane strefy służą trybów wyświetlania oraz pochodzących od różnych formantów. Różne rodzaje stref zostały opisane w poniższej sekcji Formanty istotnych części sieci Web.

Formanty części sieci Web UI, które pochodzą z **część** klas, obejmuje podstawowy interfejs użytkownika na stronie sieci Web Part. Zestaw formantów części sieci Web jest elastyczny i włącznie w opcjach oferuje do tworzenia kontrolek części. Oprócz tworzenia własnych niestandardowych formantów składników Web Part, umożliwia także istniejących kontrolek serwera ASP.NET, kontrolki użytkownika lub niestandardowych kontrolek serwera jako formantów części sieci Web. W następnej sekcji opisano podstawowe formanty, które są najczęściej używane do tworzenia stron składników Web Part.

## <a name="web-parts-essential-controls"></a>Podstawowe formanty składników Web Part

Zestaw formantów części sieci Web są obszerne, ale niektóre kontrolki są istotne, ponieważ są wymagane dla składników Web Part do pracy lub są one kontrolki najczęściej używane na stronach części sieci Web. Możesz rozpocząć korzystanie z części sieci Web i tworzenie stron podstawowych składników Web Part, warto znać podstawowe formanty składników Web Part, opisane w poniższej tabeli.

| **Formant Web Part** | **Opis** |
| --- | --- |
| WebPartManager | Zarządza wszystkie formanty składników Web Part na stronie. Jeden (i tylko jeden) **WebPartManager** kontroli jest wymagana dla każdej strony składników Web Part. |
| CatalogZone | Zawiera formanty CatalogPart. Użyj tej strefy, aby utworzyć wykaz formantów części sieci Web, z których użytkownicy mogą wybrać kontrolki przeznaczone do dodania do strony. |
| Edytora EditorZone | Zawiera formanty EditorPart. Aby umożliwić użytkownikom edytowanie i Personalizuj formantów składników Web Part na stronie, należy użyć tej strefy. |
| WebPartZone | Zawiera i zapewnia ogólny układ dla formantów składników Web Part, które tworzą głównego interfejsu użytkownika strony. Zawsze twórz strony z formantów składników Web Part, należy używać tej strefy. Strony mogą zawierać jedną lub więcej stref. |
| Strefy ConnectionsZone | Zawiera formanty WebPartConnection i udostępnia interfejs wielokrotnego użytku Zarządzanie połączeniami. |
| Składnik Web Part (składnika GenericWebPart) | Renderuje podstawowego interfejsu użytkownika; Większość formantów części sieci Web UI należą do tej kategorii. Maksymalna programistyczną kontrolę, można utworzyć niestandardowe formanty części sieci Web, które pochodzą od podstawy **składnika Web Part** kontroli. Zgodnie z formantów części sieci Web, można użyć istniejących kontrolek serwera, kontrolki użytkownika lub niestandardowych formantów. Zawsze, gdy którykolwiek z tych formantów są umieszczane w strefie, **WebPartManager** kontroli jest automatycznie zawijany je za pomocą **składnika GenericWebPart** formantów w czasie wykonywania, dzięki czemu mogą być używane z funkcje składników Web Part. |
| CatalogPart | Zawiera listę dostępnych formantów składników Web Part, które użytkownicy mogą dodawać do strony. |
| WebPartConnection | Tworzy połączenie między dwoma formantów składników Web Part na stronie. Połączenie definiuje formantów części sieci Web jako dostawca (dane), a drugi jako konsument. |
| EditorPart | Służy jako klasa bazowa dla kontrolek specjalnego edytora. |
| Formanty EditorPart (AppearanceEditorPart, LayoutEditorPart, BehaviorEditorPart i PropertyGridEditorPart) | Użytkownicy mogą personalizować różnych aspektów formantów części sieci Web UI, na stronie |

## <a name="lab-create-a-web-part-page"></a>Laboratorium: Tworzenie strony składników Web Part

W tym środowisku laboratoryjnym utworzysz strony Web part, który będzie aktualny informacji za pomocą profilów platformy ASP.NET.

### <a name="creating-a-simple-page-with-web-parts"></a>Tworzenie prostego strony ze składnikami Web Part

W tej części instruktażu utworzysz stronę, która używa formantów składników Web Part, aby wyświetlić zawartość statyczną. Pierwszym krokiem w pracy ze składnikami Web Part jest tworzenie strony dwóch wymaganych elementów strukturalnych. Po pierwsze strona części sieci Web wymaga formant WebPartManager, śledzenie i koordynowania wszystkie formanty części sieci Web. Po drugie strona części sieci Web musi mieć jeden lub więcej stref, które są formanty złożone, zawiera składnik Web Part lub innych formantów serwera, które zajmują określonego regionu strony.

> [!NOTE]
> Nie trzeba nic robić, aby włączyć personalizacji składników Web Part; jego jest domyślnie włączona, aby uzyskać zestaw formantów części sieci Web. Przy pierwszym uruchomieniu strony składników Web Part w witrynie, ASP.NET ustawia domyślnego dostawcę personalizacji do przechowywania ustawień personalizacji użytkownika. Aby uzyskać więcej informacji na temat personalizacji Zobacz Omówienie personalizacji części sieci Web.


### <a name="to-create-a-page-for-containing-web-parts-controls"></a>Aby utworzyć stronę dla zawierające formanty składników Web Part

1. Zamknij stronę domyślną, a następnie dodaj nową stronę do witryny o nazwie WebPartsDemo.aspx.
2. Przełącz się do **projektowania** widoku.
3. Z **widoku** menu, upewnij się, że **formanty wizualizacji inne niż** i **szczegóły** są zaznaczone opcje, dzięki czemu można przeglądać tagi układ i formanty, które nie mają interfejsu użytkownika.
4. Umieść kursor przed `<div>` tagów na powierzchni projektowej, a następnie naciśnij klawisz ENTER, aby dodać nowy wiersz. Umieść punkt wstawiania przed znak nowego wiersza, kliknij przycisk **Format bloku** sterowania z menu listy rozwijanej i wybierz **Nagłówek 1** opcji. W nagłówku, Dodaj tekst **strona pokazu części sieci Web**.
5. Z **składników Web Part** kartę przybornika przeciągnij **WebPartManager** kontroli na stronę, umieszczając go zaraz po znak nowego wiersza i przed `<div>`tagów.   
  
   **WebPartManager** kontroli są renderowane żadnych danych wyjściowych, więc pojawia się ono jako szary prostokąt na powierzchni projektowej.
6. Umieść punkt wstawiania w ramach `<div>` tagów.
7. W **układ** menu, kliknij przycisk **Wstaw tabelę**i Utwórz nową tabelę, która ma jeden wiersz i trzy kolumny. Kliknij przycisk **właściwości komórki** przycisku Wybierz **górnej** z **wyrównanie w pionie do** listy rozwijanej kliknij **OK**i kliknij przycisk **OK** ponownie, aby utworzyć tabelę.
8. Przeciągnij formant WebPartZone do kolumny tabeli po lewej stronie. Kliknij prawym przyciskiem myszy **WebPartZone** sterowania, wybierz **właściwości**i ustaw następujące właściwości:   
  
   Identyfikator: SidebarZone   
  
   HeaderText: pasek boczny
9. Przeciągnij sekundy **WebPartZone** do kolumny tabeli Środkowej i ustaw następujące właściwości:   
  
   Identyfikator: MainZone   
  
   HeaderText: Main
10. Zapisz plik.

Strona ma teraz dwie odrębne strefy, którymi można sterować oddzielnie. Jednak strefy nie ma żadnej zawartości, dlatego tworzenie zawartości jest kolejnym krokiem. W ramach tego przewodnika, możesz pracować z formantów składników Web Part, które wyświetlają tylko zawartość statyczną.

Układ strefy składników Web Part jest określona przez &lt;szablon zonetemplate&gt; elementu. Wewnątrz szablonu strefy można dodać dowolny formant ASP.NET, czy jest ono niestandardowych formantów Web Part, kontrolki użytkownika lub formant serwera. Należy zauważyć, że w tym miejscu używasz formant etykiety i, po prostu dodany tekst statyczny. Po umieszczeniu formant regularne serwera w **WebPartZone** strefy, ASP.NET traktuje formantu jako kontrola części sieci Web w czasie wykonywania, który udostępnia funkcje składników Web Part w formancie.

**Aby utworzyć zawartość dla głównego strefy**

1. W **projektowania** wyświetlić, przeciągnij **etykiety** kontrolować z **standardowa** kartę przybornika do obszaru zawartości strefy którego **identyfikator** właściwości ustawiono MainZone.
2. Przełącz się do **źródła** widoku. Należy zauważyć, że &lt;szablon zonetemplate&gt; element został dodany do opakowania **etykiety** formantu w MainZone.
3. Dodaj atrybut o nazwie **tytuł** do &lt;asp: label&gt; elementu i określanie jego wartości do zawartości. Usuń tekst = atrybut "Label" z &lt;asp: label&gt; elementu. Pomiędzy otwierającym, a zamykającym tagiem elementu &lt;asp: label&gt; elementu, takie jak dodać fragment tekstu **Moja strona główna — Zapraszamy** w parę &lt;h2&gt; tagi elementów. Kod powinien wyglądać w następujący sposób. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample21.aspx)]
4. Zapisz plik.

Następnie utwórz kontrolkę użytkownika, które mogą być również dodawane do strony jako kontrola części sieci Web.

### <a name="to-create-a-user-control"></a>Aby utworzyć kontrolkę użytkownika

1. Dodawanie nowej kontrolki użytkownika sieci Web do swojej witryny, która będzie służyć jako kontrolka wyszukiwania. Usuń zaznaczenie opcji **umieść kod źródłowy w oddzielnym pliku**. Dodaj ją w tym samym katalogu co stronę WebPartsDemo.aspx i nadaj mu nazwę SearchUserControl.ascx.   
  
    > [!NOTE]
    > Kontrolki użytkownika, w tym przewodniku nie implementuje funkcjonalności rzeczywistego wyszukiwania. jest on używany tylko w celu zademonstrowania funkcji składników Web Part.
2. Przełącz się do **projektowania** widoku. Z **standardowa** kartę z przybornika przeciągnij formant TextBox na stronę.
3. Umieść kursor po pola tekstowego, który właśnie został dodany, a następnie naciśnij klawisz ENTER, aby dodać nowy wiersz.
4. Przeciągnij formant przycisku na stronę w nowym wierszu poniżej pola tekstowego, który właśnie został dodany.
5. Przełącz się do **źródła** widoku. Upewnij się, że kod źródłowy dla kontrolki użytkownika będzie wyglądać jak w poniższym przykładzie. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample22.aspx)]
6. Zapisz i zamknij plik.

Teraz można dodać formanty części sieci Web do strefy pasku bocznym. Przystępując do dodawania dwóch kontrolek do strefy pasku bocznym, zawierające listę łącza, a drugi, która jest kontrola użytkownika utworzonego w poprzedniej procedurze. Łącza są dodawane jako standard **etykiety** formant serwera, podobnie jak utworzyć tekst statyczny dla strefy głównej. Jednak mimo że formanty poszczególnych serwerów znajdujących się w kontrolkę użytkownika może znajdować się bezpośrednio w strefie (takie jak formant etykiety), w tym przypadku nie są one. Zamiast tego są one częścią kontrolki użytkownika, utworzony w poprzedniej procedurze. W tym przykładzie pokazano, jak typowym sposobem pakowanie dowolnych kontrolek i dodatkowe funkcje, które mają w kontrolce użytkownika, a następnie Odwołaj się tej kontrolki w strefie jako kontrola części sieci Web.

W czasie wykonywania zestaw formantów części sieci Web opakowuje obie kontrolki za pomocą składnika GenericWebPart kontrolek. Gdy **składnika GenericWebPart** kontroli opakowuje formant serwera sieci Web, część ogólna formantu do kontrolki nadrzędnej, i dostęp do kontrolki serwera za pomocą właściwości ChildControl kontrolki nadrzędnej. To wykorzystania formanty części ogólnej umożliwia standardowe formanty serwera sieci Web ma takie samo zachowanie podstawowe i atrybutów jako formantów składników Web Part, które wynikają z **składnika Web Part** klasy.

### <a name="to-add-web-parts-controls-to-the-sidebar-zone"></a>Aby dodać formanty części sieci Web do strefy paska bocznego

1. Otwórz stronę WebPartsDemo.aspx.
2. Przełącz się do **projektowania** widoku.
3. Przeciągnij formant strony użytkownika została utworzona, SearchUserControl.ascx, z **Eksploratora rozwiązań** do strefy którego **identyfikator** właściwość jest ustawiona na SidebarZone i upuść ją tam.
4. Zapisz stronę WebPartsDemo.aspx.
5. Przełącz się do **źródła** widoku.
6. Wewnątrz &lt;asp: webpartzone&gt; elementu SidebarZone, tuż nad odwołanie do kontrolki użytkownika, Dodaj &lt;asp: label&gt; element z zawarte łącza, jak pokazano w poniższym przykładzie. Ponadto Dodaj **tytuł** atrybut do tagu kontrolki użytkownika o wartości **wyszukiwania**, jak pokazano. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample23.aspx)]
7. Zapisz i zamknij plik.

Teraz można przetestować stronę, przechodząc do niego w przeglądarce. Na stronie są wyświetlane dwie strefy. Poniższy zrzut ekranu przedstawia stronę.

**Strona pokazu części sieci Web z dwiema strefami**


![Przewodnik po 1 Web Part w programie VS — zrzut ekranu](profiles-themes-and-web-parts/_static/image3.gif)

**Rysunek 3**: składników Web Part w programie VS zrzut ekranu z przewodnika 1


Tytuł paska każdej kontrolki jest strzałki w dół, zapewniający dostęp do menu zleceń, dostępne akcje, które można wykonywać na kontrolce. Kliknij menu zleceń dla formantów, a następnie kliknij przycisk **Minimalizuj** zlecenie i zwróć uwagę, że formant jest zminimalizowany. Menu zleceń, kliknij **przywrócić**, a sterowanie powraca do normalnego rozmiaru.

### <a name="enabling-users-to-edit-pages-and-change-layout"></a>Umożliwienie użytkownikom edytowanie stron i Zmień układ

Części sieci Web zapewnia możliwości dla użytkowników zmienić układ formantów składników Web Part, przeciągając je z jedną strefę do innego. Oprócz umożliwienia użytkownikom przenoszenie **składnika Web Part** formantów z jednej strefie do innego, umożliwić użytkownikom edytowanie różne cechy formantów, łącznie z ich wygląd, układ i zachowanie. Zestaw formantów części sieci Web udostępnia podstawowe funkcje edycji dla **składnika Web Part** kontrolki. Mimo, że użytkownik nie będzie tak w tym instruktażu, można również utworzyć formanty niestandardowy edytor, które umożliwiają użytkownikom edytowanie funkcji **składnika Web Part** kontrolki. Podobnie jak w przypadku zmiany lokalizacji **składnika Web Part** kontrolki, edytowanie właściwości kontrolki zależy od personalizacji ASP.NET, aby zapisać zmiany, które użytkownicy wykonują.

W tej części tego przewodnika, możesz dodać możliwości dla użytkowników edytować podstawowe właściwości dowolnego **składnika Web Part** formantu na stronie. Aby włączyć te funkcje, należy dodać inny formant użytkownika niestandardowego do strony, wraz z &lt;asp: edytora editorzone&gt; elementu i dwóch kontrolek edycji.

### <a name="to-create-a-user-control-that-enables-changing-page-layout"></a>Aby utworzyć kontrolkę użytkownika, który umożliwia zmienianie układu strony

1. W programie Visual Studio na **pliku** menu, wybierz opcję **New** podmenu, a następnie kliknij przycisk **pliku** opcji.
2. W **Dodaj nowy element** okno dialogowe, wybierz opcję **kontrolka użytkownika sieci Web**. Nazwa nowego pliku DisplayModeMenu.ascx. Usuń zaznaczenie opcji **umieść kod źródłowy w oddzielnym pliku**.
3. Kliknij przycisk Dodaj, aby utworzyć nowego formantu.
4. Przełącz się do **źródła** widoku.
5. Usuń istniejący kod w nowym pliku i wklej następujący kod. Ten kod kontroli użytkownika korzysta z funkcji zestaw formantów części sieci Web, umożliwiające strony zmienić jej widok lub tryb wyświetlania i umożliwia również zmienić wygląd fizyczny i układ strony podczas znajdują się w niektórych tryby wyświetlania. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample24.aspx)]
6. Zapisz plik, klikając pozycję Zapisz ikonę na pasku narzędzi lub wybierając **Zapisz** na **pliku** menu.

### <a name="to-enable-users-to-change-the-layout"></a>Aby umożliwić użytkownikom zmienić układ

1. Otwórz stronę WebPartsDemo.aspx i przełącz się do **projektowania** widoku.
2. Umieść punkt wstawiania w **projektowania** wyświetlić tuż za **WebPartManager** formant, który dodano wcześniej. Dodawanie znaku końca po tekście, tak że są puste wiersze po **WebPartManager** kontroli. Umieść kursor w pustym wierszu.
3. Przeciągnij formant użytkownika, które właśnie utworzony (plik nosi DisplayModeMenu.ascx) do WebPartsDemo.aspx strony i upuść je na pusty wiersz.
4. Przeciągnij formant EditorZone z **składników Web Part** sekcji przybornika do pozostałych komórki tabeli otwarty na stronie WebPartsDemo.aspx.
5. Z **składników Web Part** sekcji w przyborniku, przeciągnij formant AppearanceEditorPart i formant LayoutEditorPart do **edytora EditorZone** kontroli.
6. Przełącz się do **źródła** widoku. Wynikowy kod w komórce tabeli powinny wyglądać podobnie do poniższego kodu. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample25.aspx)]
7. Zapisz plik WebPartsDemo.aspx. Utworzono kontrolkę użytkownika, która pozwala na zmianę trybów wyświetlania i zmiany układu strony i ma odwołanie do formantu do głównej strony sieci Web.

Teraz można przetestować możliwość edycji stron i Zmień układ.

### <a name="to-test-layout-changes"></a>Aby przetestować zmiany układu

1. Ładowanie strony w przeglądarce.
2. Kliknij przycisk **tryb wyświetlania** menu rozwijanego, a następnie wybierz **Edytuj**. Tytuły strefy są wyświetlane.
3. Przeciągnij **Moje łącza** kontrola paska tytułu ze strefy paska bocznego w dół strefy Main. Strona powinien wyglądać jak na poniższym zrzucie ekranu.

### <a name="web-parts-demo-page-with-my-links-control-moved"></a>Strony pokaz części sieci Web za pomocą kontrolki Moje łącza przeniesiony


![Przewodnik po 2 Web Part w programie VS — zrzut ekranu](profiles-themes-and-web-parts/_static/image4.gif)

**Rysunek 4**: składników Web Part w programie VS zrzut ekranu z przewodnika 2


1. Kliknij przycisk **tryb wyświetlania** menu rozwijanego, a następnie wybierz **Przeglądaj**. Strona zostanie odświeżona, nazwy stref znikną i **Moje łącza** kontrolować pozostaje, gdzie umieszczony.
2. Aby zademonstrować, czy działa personalizacji, zamknij przeglądarkę, a następnie ponownie załadować stronę. Wprowadzone zmiany są zapisywane w przeglądarce przyszłych sesji.
3. Z **tryb wyświetlania** menu, wybierz opcję **Edytuj**.   
  
   Na stronie każdego formantu jest teraz wyświetlany za pomocą strzałki w dół na pasku tytułu, który zawiera menu rozwijane zleceń.
4. Kliknij strzałkę, aby wyświetlić menu zleceń na **Moje łącza** kontroli. Kliknij przycisk **Edytuj** zlecenie.   
  
   **Edytora EditorZone** formant jest widoczny, wyświetlanie EditorPart dodaniu kontrolki.
5. W **wygląd** sekcji formantu edycji, zmień **tytuł** moich ulubionych, użyj **typu Chrome** listy rozwijanej, aby wybrać **tylko tytułu**, a następnie kliknij przycisk **Zastosuj**. Poniższy zrzut ekranu przedstawia stronę w trybie edycji.

### <a name="web-parts-demo-page-in-edit-mode"></a>Strona pokazu części sieci Web w trybie edycji


![Przewodnik po 3 Web Part w programie VS — zrzut ekranu](profiles-themes-and-web-parts/_static/image5.gif)

**Rysunek 5**: zrzut ekranu przewodnika 3 części programu VS w sieci Web


1. Kliknij przycisk **tryb wyświetlania** menu, a następnie wybierz **Przeglądaj** aby powrócić do trybu przeglądania.
2. Kontrolki są ma teraz zaktualizowany tytuł, bez obramowania, jak pokazano na poniższym zrzucie ekranu.

### <a name="edited-web-parts-demo-page"></a>Edytowanej strony części sieci Web w wersji demonstracyjnej


![Przewodnik po 4 Web Part w programie VS — zrzut ekranu](profiles-themes-and-web-parts/_static/image6.gif)

**Rysunek 4**: zrzut ekranu przewodnika 4 części programu VS w sieci Web


### <a name="adding-web-parts-at-run-time"></a>Dodawanie składników Web Part w czasie wykonywania

Możesz również zezwolić użytkownikom na dodawanie formantów składników Web Part do ich strony w czasie wykonywania. Aby to zrobić, należy skonfigurować stronę z katalogu części sieci Web, która zawiera listę formantów składników Web Part, które chcesz udostępnić użytkownikom.

**Aby zezwolić użytkownikom na dodawanie składników Web Part w czasie wykonywania**

1. Otwórz stronę WebPartsDemo.aspx i przełącz się do **projektowania** widoku.
2. Z **składników Web Part** kartę z przybornika przeciągnij formant CatalogZone w prawej kolumnie tabeli, podrzędne **edytora EditorZone** kontroli.   
  
   Obie kontrolki może być w tej samej komórki tabeli, ponieważ nie będą wyświetlane w tym samym czasie.
3. W okienku właściwości przypisać ciąg **Dodawanie składników Web Part** właściwością HeaderText **CatalogZone** kontroli.
4. Z **składników Web Part** sekcji z przybornika przeciągnij formant DeclarativeCatalogPart do obszaru zawartości **CatalogZone** kontroli.
5. Kliknij strzałkę w prawym górnym rogu **DeclarativeCatalogPart** kontrolować umożliwiającymi jego menu zadań, a następnie wybierz **Edytuj szablony**.
6. Z **standardowa** sekcji przybornika przeciągnij **FileUpload** kontroli i **kalendarza** sterowania do **WebPartsTemplate** części **DeclarativeCatalogPart** kontroli.
7. Przełącz się do **źródła** widoku. Zbadaj kod źródłowy &lt;asp: catalogzone&gt; elementu. Należy zauważyć, że **DeclarativeCatalogPart** kontrolka zawiera &lt;webpartstemplate&gt; element z dwoma formantami serwera ujęty, które będzie można dodać do strony z wykazu.
8. Dodaj **tytuł** właściwość do każdego z formantów dodanych do katalogu, przy użyciu wartości ciągu wyświetlane dla każdego tytułu w poniższym przykładzie kodu. Nawet jeśli tytuł nie jest właściwością zwykle można ustawić na tych dwóch serwerów formantów w czasie projektowania, gdy użytkownik doda te formanty do **WebPartZone** strefy z katalogu w czasie wykonywania, ich są każde jest ujęte w  **Składnika GenericWebPart** kontroli. Dzięki temu je do działania jako formantów składników Web Part, dzięki czemu będą one możliwość wyświetlania tytułów.   
  
   Kod dla dwóch kontrolek znajdujących się w **DeclarativeCatalogPart** kontrolki powinien wyglądać jak poniżej. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample26.aspx)]
9. Zapisz stronę.

Teraz możesz przetestować katalogu.

### <a name="to-test-the-web-parts-catalog"></a>Aby testować wykaz w części sieci Web

1. Ładowanie strony w przeglądarce.
2. Kliknij przycisk **tryb wyświetlania** menu rozwijanego, a następnie wybierz **katalogu**.   
  
   Katalog o nazwie **Dodawanie składników Web Part** jest wyświetlana.
3. Przeciągnij **moich ulubionych** z głównego strefy kontrolować powrót do początku strefy paska bocznego i upuść je ma.
4. W **Dodawanie składników Web Part** katalogu, zaznacz oba pola wyboru, a następnie wybierz **Main** z listy rozwijanej, która zawiera strefy dostępności.
5. Kliknij przycisk **Dodaj** w wykazie. Formanty są dodawane do strefy głównej. Jeśli chcesz, można dodać wiele wystąpień kontrolek z katalogu, do strony.   
  
   Poniższy zrzut ekranu przedstawia strony za pomocą formantu przekazywania plików i kalendarza w strefie głównej. 

![Formanty dodany do strefy głównej z katalogu](profiles-themes-and-web-parts/_static/image7.gif)

    **Figure 5**: Controls added to Main zone from the catalog
6. Kliknij przycisk **tryb wyświetlania** menu rozwijanego, a następnie wybierz **Przeglądaj**. Katalog znika i odświeżeniu strony.
7. Zamknij przeglądarkę. Załaduj ponownie stronę. Zmiany wprowadzone w utrwalania.
