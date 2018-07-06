---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/protecting-connection-strings-and-other-configuration-information-vb
title: Ochrona parametrów połączeń i innych informacji o konfiguracji (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Aplikacja ASP.NET zazwyczaj przechowuje informacje o konfiguracji w pliku Web.config. Niektóre z tych informacji jest wielkość liter i gwarantuje ochronę. Przez def....
ms.author: aspnetcontent
ms.date: 08/03/2007
ms.assetid: cd17dbe1-c5e1-4be8-ad3d-57233d52cef1
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/protecting-connection-strings-and-other-configuration-information-vb
msc.type: authoredcontent
ms.openlocfilehash: 7007496ad3aa137d8a6a9d4ba8e486d6b32ec295
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37810907"
---
<a name="protecting-connection-strings-and-other-configuration-information-vb"></a>Ochrona parametrów połączeń i innych informacji o konfiguracji (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz program Code](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_73_VB.zip) lub [Pobierz plik PDF](protecting-connection-strings-and-other-configuration-information-vb/_static/datatutorial73vb1.pdf)

> Aplikacja ASP.NET zazwyczaj przechowuje informacje o konfiguracji w pliku Web.config. Niektóre z tych informacji jest wielkość liter i gwarantuje ochronę. Domyślnie ten plik będzie nie być udostępniane odwiedzający witrynę sieci Web, ale administrator lub haker może uzyskać dostęp do systemu plików na serwerze sieci Web i wyświetlić zawartość pliku. W tym samouczku dowie się, że program ASP.NET 2.0 pozwala nam chronić poufne informacje, szyfrując sekcji w pliku Web.config.


## <a name="introduction"></a>Wprowadzenie

Informacje o konfiguracji dla aplikacji ASP.NET jest zazwyczaj przechowywane w pliku XML o nazwie `Web.config`. W miarę upływu tych samouczków Zaktualizowaliśmy `Web.config` kilka razy. Podczas tworzenia `Northwind` wpisana zestawu danych w [pierwszego samouczka dotyczącego](../introduction/creating-a-data-access-layer-vb.md), na przykład informacje o parametrach połączenia została automatycznie dodana do `Web.config` w `<connectionStrings>` sekcji. W dalszej części [strony wzorcowe i nawigacja w witrynie](../introduction/master-pages-and-site-navigation-vb.md) samouczku ręcznie Zaktualizowaliśmy `Web.config`, dodając `<pages>` element wskazujący wszystkie strony ASP.NET w projekcie należy użyć `DataWebControls` motywu.

Ponieważ `Web.config` mogą zawierać poufne dane, takie jak parametry połączeń, ważne jest, zawartość `Web.config` bezpieczne i ukrytych z nieautoryzowanych osób przeglądających. Domyślnie wszystkie żądania HTTP do pliku za pomocą `.config` rozszerzenia odbywa się przez aparat programu ASP.NET, która zwraca *nie jest obsługiwany przez ten typ strony* komunikat przedstawionej na rysunku 1. Oznacza to, że goście nie można wyświetlić swoje `Web.config` zawartość s pliku po prostu wpisując http://www.YourServer.com/Web.config do ich paska adresu przeglądarki s.


[![Odwiedzający Web.config za pośrednictwem przeglądarki zwraca to typ strony nie został obsłużony, wiadomości](protecting-connection-strings-and-other-configuration-information-vb/_static/image2.png)](protecting-connection-strings-and-other-configuration-information-vb/_static/image1.png)

**Rysunek 1**: odwiedzający `Web.config` za pośrednictwem przeglądarki zwraca to typ strony nie został obsłużony, komunikat ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](protecting-connection-strings-and-other-configuration-information-vb/_static/image3.png))


Ale co zrobić, jeśli osoba atakująca może znaleźć inne luki, które umożliwia jej wyświetlanie Twojej `Web.config` s zawartość pliku? Co można osoba atakująca dzięki tym informacjom, i poznać kroki, jakie można podjąć w celu jeszcze lepiej chronić informacje poufne, w ramach `Web.config`? Na szczęście większości sekcji w `Web.config` nie zawierają informacji poufnych. Co można osoba atakująca bardzo jeśli znają nazwy domyślnego motywu używanego przez strony ASP.NET?

Niektóre `Web.config` sekcje, jednak zawierają poufne informacje, które mogą zawierać parametrów połączenia, nazwy użytkownika, hasła, nazwy serwerów, klucze szyfrowania i tak dalej. Te informacje zazwyczaj znajduje się w następującym `Web.config` sekcje:

- `<appSettings>`
- `<connectionStrings>`
- `<identity>`
- `<sessionState>`

W tym samouczku przyjrzymy się technik ochrony takie informacje poufne dane konfiguracyjne. Jak widać, .NET Framework w wersji 2.0 obejmuje system konfiguracji chronionych, który sprawia, że sekcje wybranej konfiguracji programowo szyfrowanie i odszyfrowywanie szybka i bezproblemowa.

> [!NOTE]
> W tym samouczku kończy się za pomocą przyjrzeć się zalecenia s firmy Microsoft do łączenia z bazą danych z aplikacji ASP.NET. Oprócz szyfrowania parametry połączenia, mogą pomóc utrwalanie systemu poprzez zapewnienie, że łączysz się z bazą danych w bezpieczny sposób.


## <a name="step-1-exploring-aspnet-20-s-protected-configuration-options"></a>Krok 1: Poznawanie platformy ASP.NET 2.0 s chronione opcje konfiguracji

Platforma ASP.NET 2.0 obejmuje system konfiguracji chronionej do szyfrowania i odszyfrowywania informacje o konfiguracji. Dotyczy to metod w .NET Framework, która może służyć do programowego zaszyfrować lub odszyfrować informacje o konfiguracji. System konfiguracji chronionych używa [modelu dostawca](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), które umożliwia deweloperom wybierz, jakie kryptograficznych implementacja jest użyta.

.NET Framework jest dostarczany z dwóch dostawców konfiguracji chronionych:

- [`RSAProtectedConfigurationProvider`](https://msdn.microsoft.com/library/system.configuration.rsaprotectedconfigurationprovider.aspx) -używa asymetrycznego [algorytmu RSA](http://en.wikipedia.org/wiki/Rsa) do szyfrowania i odszyfrowywania.
- [`DPAPIProtectedConfigurationProvider`](https://msdn.microsoft.com/system.configuration.dpapiprotectedconfigurationprovider.aspx) -Windows używa [interfejsu API ochrony danych (DPAPI)](https://msdn.microsoft.com/library/ms995355.aspx) do szyfrowania i odszyfrowywania.

Ponieważ system konfiguracji chroniony implementuje wzorzec projektowy dostawcy, jest możliwość utworzenia własnego dostawcę konfiguracji chronionych i podłącz go do aplikacji. Zobacz [Implementowanie dostawcę konfiguracji chronionych](https://msdn.microsoft.com/library/wfc2t3az(VS.80).aspx) Aby uzyskać więcej informacji na temat tego procesu.

Dostawcy algorytmu RSA, a DPAPI używają kluczy dla procedur ich szyfrowania i odszyfrowywania, a te klucze mogą być przechowywane na maszynie lub użytkownika poziomie. Klucze maszyn na poziomie są doskonałe dla scenariuszy, w którym działa aplikacja sieci web na swój własny dedykowany serwer lub jeśli jest wiele aplikacji na serwerze, który należy udostępnić zaszyfrowane informacje. Poziom użytkownika klucze są bezpieczniejsze w udostępnionych środowiskach hostingu, gdzie inne aplikacje na tym samym serwerze nie należy może odszyfrować sekcji konfiguracji s chronione Twojej aplikacji.

W tym samouczku naszych przykładach użyje DPAPI dostawcy i klucze poziomie komputera. W szczególności przyjrzymy się zaszyfrowanie `<connectionStrings>` sekcji `Web.config`, mimo że system konfiguracji chronionych może służyć do szyfrowania, większość dowolne `Web.config` sekcji. Informacji na temat przy użyciu kluczy na poziomie użytkownika, czy dostawca RSA można znaleźć w sekcji odczyty dalsze zasoby na końcu tego samouczka.

> [!NOTE]
> `RSAProtectedConfigurationProvider` i `DPAPIProtectedConfigurationProvider` dostawcy są zarejestrowane w usłudze `machine.config` pliku przy użyciu nazwy dostawcy `RsaProtectedConfigurationProvider` i `DataProtectionConfigurationProvider`, odpowiednio. Podczas szyfrowania lub odszyfrowywania informacje o konfiguracji, firma Microsoft będzie należy podać nazwę odpowiedniego dostawcy (`RsaProtectedConfigurationProvider` lub `DataProtectionConfigurationProvider`) zamiast nazwą rzeczywistego typu (`RSAProtectedConfigurationProvider` i `DPAPIProtectedConfigurationProvider`). Możesz znaleźć `machine.config` w pliku `$WINDOWS$\Microsoft.NET\Framework\version\CONFIG` folderu.


## <a name="step-2-programmatically-encrypting-and-decrypting-configuration-sections"></a>Krok 2: Programowe szyfrowanie i odszyfrowywanie sekcji konfiguracji

Przy użyciu kilku wierszy kodu firma Microsoft może szyfrowania lub odszyfrowywania sekcji szczególną konfigurację przy użyciu określonego dostawcy. Kod, jak firma Microsoft pojawi się wkrótce, po prostu musi odwoływać się do programowego sekcji odpowiednia konfiguracja wywołać jej `ProtectSection` lub `UnprotectSection` metody, a następnie wywołania `Save` metodę, aby utrwalić zmiany. Ponadto program .NET Framework zawiera narzędzie przydatne wiersza polecenia, który może szyfrowanie i odszyfrowywanie informacji o konfiguracji. Przeanalizujemy to narzędzie wiersza polecenia w kroku 3.

Aby zilustrować programowo ochronę informacji o konfiguracji, s umożliwiają tworzenie strony ASP.NET, który zawiera przyciski do szyfrowania i odszyfrowywania `<connectionStrings>` sekcji `Web.config`.

Zacznij od otwarcia `EncryptingConfigSections.aspx` stronie `AdvancedDAL` folderu. Przeciągnij formant TextBox z przybornika w projektancie, ustawiając jego `ID` właściwości `WebConfigContents`, jego `TextMode` właściwości `MultiLine`i jego `Width` i `Rows` właściwości do 95% i 15, odpowiednio. Ten formant TextBox wyświetli zawartość `Web.config` może szybko sprawdzić, jeśli zawartość jest zaszyfrowane, czy nie. Oczywiście w rzeczywistej aplikacji nigdy nie chcesz wyświetlać zawartość `Web.config`.

Poniżej pola tekstowego, należy dodać dwie kontrolki przycisku o nazwie `EncryptConnStrings` i `DecryptConnStrings`. Ustawiać ich właściwości tekstu, aby parametry połączenia szyfrowanie i odszyfrowywanie ciągów połączenia.

W tym momencie ekran powinien wyglądać podobnie jak na rysunku 2.


[![Na stronie Dodaj pole tekstowe oraz dwóch przycisków umieszczonych w sieci Web](protecting-connection-strings-and-other-configuration-information-vb/_static/image5.png)](protecting-connection-strings-and-other-configuration-information-vb/_static/image4.png)

**Rysunek 2**: Dodaj pole tekstowe oraz dwie kontrolki sieci Web przycisku do strony ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](protecting-connection-strings-and-other-configuration-information-vb/_static/image6.png))


Następnie należy napisać kod, który ładuje i wyświetla zawartość `Web.config` w `WebConfigContents` pole tekstowe, gdy strona zostanie załadowana. Dodaj następujący kod do klasy CodeBehind s strony. Ten kod dodaje metodę o nazwie `DisplayWebConfig` i określa je z `Page_Load` programu obsługi zdarzeń podczas `Page.IsPostBack` jest `False`:


[!code-vb[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample1.vb)]

`DisplayWebConfig` Metoda używa [ `File` klasy](https://msdn.microsoft.com/library/system.io.file.aspx) otworzyć s aplikacji `Web.config` pliku [ `StreamReader` klasy](https://msdn.microsoft.com/library/system.io.streamreader.aspx) odczytanie jego zawartości w ciągu i [ `Path` klasy](https://msdn.microsoft.com/library/system.io.path.aspx) do generowania ścieżkę fizyczną do `Web.config` pliku. Te trzy klasy zostaną znalezione w [ `System.IO` przestrzeni nazw](https://msdn.microsoft.com/library/system.io.aspx). W związku z tym, należy dodać `Imports``System.IO` instrukcji na górze z kodem klasę lub alternatywnie, te klasy nazw z prefiksem `System.IO.`

Następnie należy dodać obsługę zdarzeń dla dwóch kontrolek przycisku `Click` zdarzeń i dodać niezbędny kod do szyfrowania i odszyfrowywania `<connectionStrings>` sekcji przy użyciu klucza poziomie komputera za pomocą dostawcy DPAPI. Przy użyciu projektanta, kliknij dwukrotnie listę przycisków, aby dodać `Click` programu obsługi zdarzeń w związanym z kodem klasę, a następnie dodaj następujący kod:


[!code-vb[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample2.vb)]

Kod używany w procedurze obsługi dwóch zdarzeń jest niemal identyczne. Obaj użytkownicy od zebrania informacji o bieżącym s aplikacji `Web.config` plików za pośrednictwem [ `WebConfigurationManager` klasy](https://msdn.microsoft.com/library/system.web.configuration.webconfigurationmanager.aspx) s [ `OpenWebConfiguration` metoda](https://msdn.microsoft.com/library/system.web.configuration.webconfigurationmanager.openwebconfiguration.aspx). Ta metoda zwraca plik konfiguracji sieci web dla określonej ścieżki wirtualnej. Następnie `Web.config` pliku s `<connectionStrings>` sekcji jest dostępna za pośrednictwem [ `Configuration` klasy](https://msdn.microsoft.com/library/system.configuration.configuration.aspx) s [ `GetSection(sectionName)` metoda](https://msdn.microsoft.com/library/system.configuration.configuration.getsection.aspx), co powoduje zwrócenie [ `ConfigurationSection` ](https://msdn.microsoft.com/library/system.configuration.configurationsection.aspx) obiektu.

`ConfigurationSection` Obiekt zawiera [ `SectionInformation` właściwość](https://msdn.microsoft.com/library/system.configuration.configurationsection.sectioninformation.aspx) zapewnia dodatkowe informacje i funkcje dotyczące sekcji konfiguracji. Jak kod powyżej przedstawiono, możemy określić, czy sekcja konfiguracji ma być zaszyfrowany, sprawdzając `SectionInformation` właściwości s `IsProtected` właściwości. Ponadto sekcji mogą być szyfrowane lub odszyfrować za pośrednictwem `SectionInformation` właściwości s `ProtectSection(provider)` i `UnprotectSection` metody.

`ProtectSection(provider)` Metoda przyjmuje jako dane wejściowe ciąg określający nazwę dostawcy konfiguracji chronionej do szyfrowania. W `EncryptConnString` przekazujemy DataProtectionConfigurationProvider do obsługi zdarzeń przycisku s `ProtectSection(provider)` metody, aby dostawca DPAPI jest używany. `UnprotectSection` Metoda można określić dostawcę, który został użyty do zaszyfrowania sekcji konfiguracji i w związku z tym nie wymaga żadnego parametrów wejściowych.

Po wywołaniu `ProtectSection(provider)` lub `UnprotectSection` metody, należy wywołać `Configuration` obiektu s [ `Save` metoda](https://msdn.microsoft.com/library/system.configuration.configuration.save.aspx) aby utrwalić zmiany. Po szyfrowane lub odszyfrować informacje konfiguracji i zmiany zostały zapisane, nazywamy `DisplayWebConfig` załadować zaktualizowane `Web.config` zawartości w formancie pola tekstowego.

Po wprowadzeniu powyższego kodu, możesz go przetestować, przechodząc `EncryptingConfigSections.aspx` strony za pośrednictwem przeglądarki. Początkowo wyświetlony strona, która wyświetla zawartość `Web.config` z `<connectionStrings>` sekcji wyświetlane w postaci zwykłego tekstu (zobacz rysunek 3).


[![Na stronie Dodaj pole tekstowe oraz dwóch przycisków umieszczonych w sieci Web](protecting-connection-strings-and-other-configuration-information-vb/_static/image8.png)](protecting-connection-strings-and-other-configuration-information-vb/_static/image7.png)

**Rysunek 3**: Dodaj pole tekstowe oraz dwie kontrolki sieci Web przycisku do strony ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](protecting-connection-strings-and-other-configuration-information-vb/_static/image9.png))


Teraz kliknij przycisk szyfrowania parametrów połączenia. Jeśli weryfikacja żądania jest włączona, znaczniki odesłana `WebConfigContents` pola tekstowego powoduje wygenerowanie `HttpRequestValidationException`, powoduje wyświetlenie komunikatu, potencjalnie niebezpiecznych `Request.Form` wartość została wykryta przez klienta. Sprawdzanie poprawności żądań, które jest domyślnie włączona w programie ASP.NET 2.0, zabrania ogłaszania zwrotnego, które zawierają HTML usunięcie zakodowanym i ma na celu zapobieganie atakom uruchomienie skryptu. Na stronie - lub -poziomie aplikacji można wyłączyć ten test. Aby ją wyłączyć dla tej strony, należy ustawić `ValidateRequest` ustawienie `False` w `@Page` dyrektywy. `@Page` Dyrektywy znajduje się w górnej części oznaczeniu deklaracyjnym strony s.


[!code-aspx[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample3.aspx)]

Aby uzyskać więcej informacji na temat weryfikacji żądań, jego przeznaczenie, wyłącz ją na stronie aplikacji poziomie i, jak oraz w formacie HTML kodowanie znaczników, zobacz [żądanie weryfikacji — zapobieganie atakom skryptów](../../../../whitepapers/request-validation.md).

Po wyłączeniu weryfikację żądań dla strony, spróbuj ponowne kliknięcie przycisku Szyfruj parametry połączenia. Na odświeżenie strony, będzie dostępna w pliku konfiguracji i jego `<connectionStrings>` sekcji szyfrowane przy użyciu dostawcy DPAPI. Pole tekstowe jest następnie zaktualizowany i będzie pokazywał nowe `Web.config` zawartości. Jak pokazano na rysunku 4, `<connectionStrings>` informacje są teraz szyfrowane.


[![Kliknięcie przycisku Szyfruj połączenie ciągów przycisk szyfruje &lt;connectionString&gt; sekcji](protecting-connection-strings-and-other-configuration-information-vb/_static/image11.png)](protecting-connection-strings-and-other-configuration-information-vb/_static/image10.png)

**Rysunek 4**: kliknięcie przycisku Szyfruj połączenie ciągów przycisk szyfruje `<connectionString>` sekcji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](protecting-connection-strings-and-other-configuration-information-vb/_static/image12.png))


Wybrany zaszyfrowany `<connectionStrings>` poniżej sekcji generowane na tym komputerze, ale część zawartości w `<CipherData>` element został usunięty w celu skrócenia programu:


[!code-xml[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample4.xml)]

> [!NOTE]
> `<connectionStrings>` Element określa dostawcę używanego do szyfrowania (`DataProtectionConfigurationProvider`). Te informacje są używane przez `UnprotectSection` metody, gdy kliknięto przycisk odszyfrować parametry połączenia.


Gdy informacje o parametrach połączenia jest dostępny z `Web.config` — za pomocą kodu pisania, z kontrolki SqlDataSource lub automatycznie wygenerowany kod z TableAdapters w naszym wpisanych zestawów danych — jest automatycznie odszyfrowany. Krótko mówiąc, nie musimy dodać dowolnego dodatkowego kodu lub logiki można odszyfrować zaszyfrowanego `<connectionString>` sekcji. Aby to wykazać, można znaleźć w jednym z samouczków wcześniej w tej chwili, takich jak samouczek proste wyświetlana w sekcji podstawowe Raportowanie (`~/BasicReporting/SimpleDisplay.aspx`). Jak pokazano na rysunku 5, samouczek działa dokładnie tak, czy oczekujemy, wskazujący, że informacje o parametrach połączenia szyfrowanego jest automatycznie odszyfrowywany przez strony ASP.NET.


[![Warstwa dostępu do danych automatycznie odszyfrowuje informacje o parametrach połączenia](protecting-connection-strings-and-other-configuration-information-vb/_static/image14.png)](protecting-connection-strings-and-other-configuration-information-vb/_static/image13.png)

**Rysunek 5**: Warstwa dostępu do danych automatycznie odszyfrowuje informacje o parametrach połączenia ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](protecting-connection-strings-and-other-configuration-information-vb/_static/image15.png))


Aby przywrócić `<connectionStrings>` sekcji jego reprezentację w postaci zwykłego tekstu, kliknij przycisk odszyfrować parametry połączenia. Na zwrot powinien zostać wyświetlony parametry połączenia w `Web.config` w postaci zwykłego tekstu. W tym momencie ekran powinien wyglądać tak, jak podczas odwiedzania najpierw na tej stronie (zobacz rysunek 3).

## <a name="step-3-encrypting-configuration-sections-usingaspnetregiisexe"></a>Krok 3: Szyfrowanie sekcji konfiguracji za pomocą`aspnet_regiis.exe`

Program .NET Framework zawiera szereg narzędzi wiersza polecenia w `$WINDOWS$\Microsoft.NET\Framework\version\` folderu. W [przy użyciu zależności pamięci podręcznej SQL](../caching-data/using-sql-cache-dependencies-vb.md) samouczek, na przykład analizujemy przy użyciu `aspnet_regsql.exe` narzędzia wiersza polecenia, aby dodać infrastrukturę niezbędną do zależności pamięci podręcznej SQL. Kolejnym narzędziem przydatne w wierszu polecenia w tym folderze jest [narzędzie rejestracji programu ASP.NET w usługach IIS (`aspnet_regiis.exe`)](https://msdn.microsoft.com/library/k6h9cz8h(VS.80).aspx). Jak sugeruje jej nazwa, narzędzie rejestracji programu ASP.NET w usługach IIS przede wszystkim służy do rejestrowania aplikacji programu ASP.NET 2.0 Microsoft s profesjonalne serwera sieci Web usług IIS. Oprócz jej funkcji związanych z usługami IIS, narzędzie rejestracji programu ASP.NET w usługach IIS można również zaszyfrować lub odszyfrować sekcje określonej konfiguracji w `Web.config`.

Poniższa instrukcja pokazuje składnię ogólną używany do szyfrowania sekcji konfiguracji, za pomocą `aspnet_regiis.exe` narzędzia wiersza polecenia:


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample5.cmd)]

*sekcja* sekcję konfiguracji do szyfrowania (na przykład connectionStrings) *fizycznych\_katalogu* jest pełna, fizyczną ścieżkę do katalogu głównego aplikacji s sieci web i *dostawcy*  jest nazwą dostawcy konfiguracji chronionej do użycia (na przykład DataProtectionConfigurationProvider). Alternatywnie w aplikacji sieci web jest zarejestrowana w usługach IIS można wprowadzić ścieżkę wirtualną zamiast ścieżki fizycznej przy użyciu następującej składni:


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample6.cmd)]

Następujące `aspnet_regiis.exe` szyfruje przykład `<connectionStrings>` sekcji przy użyciu dostawcy interfejsu DPAPI z kluczem poziomie komputera:


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample7.cmd)]

Podobnie `aspnet_regiis.exe` narzędzia wiersza polecenia może służyć do odszyfrowywania sekcji konfiguracji. Zamiast używania `-pef` przełącznika, użyj `-pdf` (lub zamiast `-pe`, użyj `-pd`). Należy również zauważyć, że nazwa dostawcy nie jest konieczne, podczas odszyfrowywania.


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample8.cmd)]

> [!NOTE]
> Ponieważ używamy dostawcy DPAPI, który korzysta z kluczami specyficzne dla komputera, należy uruchomić `aspnet_regiis.exe` z tym samym komputerze, z którego są obsługiwane na stronach sieci web. Na przykład jeśli uruchamiaj ten program wiersza polecenia z lokalnej maszynie do programowania, a następnie przekaż zaszyfrowanego pliku Web.config na serwerze produkcyjnym, serwer produkcyjny nie będzie można odszyfrować informacje o parametrach połączenia, ponieważ został on zaszyfrowany przy użyciu kluczy określonych na maszynie deweloperskiej. Dostawca RSA nie ma tego ograniczenia, ponieważ jest możliwe do eksportowania kluczy RSA do innego komputera.


## <a name="understanding-database-authentication-options"></a>Opcje uwierzytelniania bazy danych opis

Zanim każda aplikacja może wystawiać `SELECT`, `INSERT`, `UPDATE`, lub `DELETE` zapytań w bazie danych programu Microsoft SQL Server, bazy danych należy najpierw należy zidentyfikować osoby zgłaszającej żądanie. Ten proces jest nazywany *uwierzytelniania* i programu SQL Server udostępnia dwie metody uwierzytelniania:

- **Uwierzytelnianie Windows** — proces, w którym aplikacja jest uruchomiona, jest używany do komunikacji z bazą danych. Podczas uruchamiania aplikacji ASP.NET przy użyciu programu Visual Studio 2005 s ASP.NET Development Server, aplikacja ASP.NET zakłada tożsamości aktualnie zalogowanego użytkownika. W przypadku aplikacji ASP.NET w Microsoft Internet Information Server (IIS) aplikacji ASP.NET zwykle przyjąć tożsamość `domainName``\MachineName` lub `domainName``\NETWORK SERVICE`, chociaż mogą być dostosowywane.
- **Uwierzytelnianie SQL** — wartości Identyfikatora i hasła użytkownika są dostarczane jako poświadczeń do uwierzytelniania. Przy użyciu uwierzytelniania SQL identyfikator użytkownika i hasło są dostarczane w parametrach połączenia.

Uwierzytelnianie Windows jest preferowane niż w przypadku uwierzytelniania programu SQL, ponieważ jest bardziej bezpieczne. Przy użyciu uwierzytelniania Windows parametry połączenia są wolne od nazwy użytkownika i hasła, a jeśli serwer sieci web a serwerem bazy danych znajdują się na dwóch różnych komputerach, poświadczenia nie są wysyłane przez sieć w postaci zwykłego tekstu. Przy użyciu uwierzytelniania SQL jednak poświadczenia uwierzytelniania są zakodowane w parametrach połączenia i są przesyłane z serwera sieci web na serwerze bazy danych w postaci zwykłego tekstu.

Te samouczki, że używasz uwierzytelniania Windows. Aby sprawdzić, jakie tryb uwierzytelniania jest używany, sprawdzając parametry połączenia. Parametry połączenia w `Web.config` dla naszych samouczków:

`Data Source=.\SQLEXPRESS; AttachDbFilename=|DataDirectory|\NORTHWND.MDF; Integrated Security=True; User Instance=True`

Zabezpieczenia zintegrowane = True i braku nazwy użytkownika i hasła wskazują, że jest używane uwierzytelnianie Windows. W połączeniu z niektóre ciągi termin zaufanych połączeń = Yes lub zabezpieczenia zintegrowane = SSPI jest używana zamiast zabezpieczenia zintegrowane = True, ale wszystkich trzech wskazują korzystanie z uwierzytelniania Windows.

Poniższy przykład przedstawia parametry połączenia, który używa uwierzytelniania SQL. Należy zwrócić uwagę, poświadczenia osadzone w ciągu połączenia:

`Server=serverName; Database=Northwind; uid=userID; pwd=password`

Wyobraź sobie, że osoba atakująca jest możliwość wyświetlania s Twojej aplikacji `Web.config` pliku. Jeśli używasz uwierzytelniania SQL nawiązać połączenia z bazą danych, który jest dostępny za pośrednictwem Internetu, osoba atakująca służy te parametry połączenia do łączenia z bazą danych programu SQL Management Studio lub ze strony ASP.NET na własnej witryny sieci Web. Aby ułatwić uniknięcie tego zagrożenia, szyfrowanie informacje o parametrach połączenia w `Web.config` przy użyciu systemu konfiguracji chronionych.

> [!NOTE]
> Aby uzyskać więcej informacji na temat różnych typów uwierzytelniania dostępnych w programie SQL Server, zobacz [tworzenie zabezpieczanie aplikacji ASP.NET: uwierzytelniania, autoryzacji i bezpieczna komunikacja](https://msdn.microsoft.com/library/aa302392.aspx). Dodatkowo połączenia ciągu przykłady ilustrujące różnic między składnią uwierzytelnianie SQL i Windows, można znaleźć [ConnectionStrings.com](http://www.connectionstrings.com/).


## <a name="summary"></a>Podsumowanie

Domyślnie pliki z `.config` rozszerzenia w aplikacji ASP.NET nie są dostępne za pośrednictwem przeglądarki. Te typy plików nie są zwracane, ponieważ mogą one zawierać poufne informacje, takie jak parametry połączenia bazy danych, nazwy użytkowników i hasła i tak dalej. System konfiguracji chronionych w programie .NET 2.0 pomaga jeszcze lepiej chronić informacje poufne, umożliwiając sekcji określonego konfiguracji szyfrowania. Istnieją dwa dostawców wbudowanych konfiguracji chronionych: jeden, który używa algorytmu RSA i jedną, która używa interfejsu API ochrony danych Windows (DPAPI).

W tym samouczku zobaczyliśmy, jak szyfrowanie i odszyfrowywanie ustawienia konfiguracji przy użyciu dostawcy DPAPI. Można to zrobić programowo, zgodnie z widzieliśmy w kroku 2, jak również za pośrednictwem `aspnet_regiis.exe` narzędzia wiersza polecenia, co zostało omówione w kroku 3. Aby uzyskać więcej informacji na temat przy użyciu kluczy na poziomie użytkownika, czy dostawca RSA zamiast tego Zobacz zasoby przedstawione w sekcji dalsze informacje.

Wszystkiego najlepszego programowania!

## <a name="further-reading"></a>Dalsze informacje

Więcej informacji na tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Tworzenie aplikacji bezpiecznego ASP.NET: Uwierzytelniania, autoryzacji i bezpiecznej komunikacji](https://msdn.microsoft.com/library/aa302392.aspx)
- [Szyfrowanie informacji o konfiguracji w programie ASP.NET 2.0 aplikacji](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx)
- [Szyfrowanie `Web.config` wartości na platformie ASP.NET 2.0](https://weblogs.asp.net/scottgu/archive/2006/01/09/434893.aspx)
- [Instrukcje: Szyfrowanie sekcji konfiguracyjnych w programie ASP.NET 2.0 przy użyciu interfejsu DPAPI](https://msdn.microsoft.com/library/ms998280.aspx)
- [Instrukcje: Szyfrowanie sekcji konfiguracyjnych w programie ASP.NET 2.0 przy użyciu RSA](https://msdn.microsoft.com/library/ms998283.aspx)
- [Konfiguracja interfejsu API na platformie .NET w wersji 2.0](http://www.odetocode.com/Articles/418.aspx)
- [Ochrona danych Windows](https://msdn.microsoft.com/library/ms995355.aspx)

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedem ASP/ASP.NET książek i założycielem [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). ADAM można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Wiodące osób dokonujących przeglądu, w tym samouczku zostały Teresa Murphy i Randy Schmidt. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb.md)
> [dalej](debugging-stored-procedures-vb.md)
