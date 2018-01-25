---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/protecting-connection-strings-and-other-configuration-information-cs
title: "Ochrona parametry połączenia i inne informacje o konfiguracji (C#) | Dokumentacja firmy Microsoft"
author: rick-anderson
description: "Aplikacja ASP.NET zwykle przechowuje informacje o konfiguracji w pliku Web.config. Niektóre z tych informacji jest poufne i gwarantuje ochrony. Przez domyślny."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/03/2007
ms.topic: article
ms.assetid: ad8dd396-30f7-4abe-ac02-a0b84422e5be
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/protecting-connection-strings-and-other-configuration-information-cs
msc.type: authoredcontent
ms.openlocfilehash: e3782e3d4acc2db0e744128dad64fdfae1e8766d
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="protecting-connection-strings-and-other-configuration-information-c"></a>Ochrona parametry połączenia i inne informacje o konfiguracji (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz kod](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_73_CS.zip) lub [pobierania plików PDF](protecting-connection-strings-and-other-configuration-information-cs/_static/datatutorial73cs1.pdf)

> Aplikacja ASP.NET zwykle przechowuje informacje o konfiguracji w pliku Web.config. Niektóre z tych informacji jest poufne i gwarantuje ochrony. Domyślnie ten plik nie zostanie obsłużona do odwiedzający witrynę sieci Web, ale administrator lub haker może uzyskać dostęp do systemu plików serwera sieci Web i wyświetlić zawartość pliku. W tym samouczku będziemy Dowiedz się, ASP.NET 2.0 umożliwia firmie Microsoft w celu ochrony poufnych informacji przez szyfrowanie sekcji w pliku Web.config.


## <a name="introduction"></a>Wprowadzenie

Informacje o konfiguracji dla aplikacji ASP.NET jest często przechowywane w pliku XML o nazwie `Web.config`. W trakcie tego samouczka zostały zaktualizowane `Web.config` kilka razy. Podczas tworzenia `Northwind` wpisane zestaw danych z [pierwszy samouczek](../introduction/creating-a-data-access-layer-cs.md), na przykład informacje o parametrach połączenia została automatycznie dodana do `Web.config` w `<connectionStrings>` sekcji. W dalszej [stron wzorcowych i nawigacji w witrynie](../introduction/master-pages-and-site-navigation-cs.md) samouczek, ręcznie Zaktualizowaliśmy `Web.config`, dodając `<pages>` elementu oznacza, że wszystkie strony ASP.NET w naszym projektu należy użyć `DataWebControls` motywu.

Ponieważ `Web.config` mogą zawierać poufne dane, takie jak parametry połączenia, ważne jest, że zawartość `Web.config` bezpieczne i ukrytych z nieautoryzowanym użytkownikom. Domyślnie wszystkie HTTP żądanie do pliku z `.config` rozszerzenia jest obsługiwany przez aparat programu ASP.NET, która zwraca *ten typ strony nie został obsłużony* komunikat pokazany na rysunku 1. Oznacza to, że osoby odwiedzające nie można wyświetlić z `Web.config` plików zawartości s, wprowadzając po prostu http://www.YourServer.com/Web.config do ich na pasku adresu przeglądarki s.


[![Odwiedzający Web.config za pośrednictwem przeglądarki zwraca to typ strony nie został obsłużony wiadomości](protecting-connection-strings-and-other-configuration-information-cs/_static/image2.png)](protecting-connection-strings-and-other-configuration-information-cs/_static/image1.png)

**Rysunek 1**: odwiedzający `Web.config` za pośrednictwem przeglądarki zwraca to typ strony nie został obsłużony komunikatu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](protecting-connection-strings-and-other-configuration-information-cs/_static/image3.png))


Ale co zrobić, jeśli osoba atakująca może znaleźć inne wykorzystać, które umożliwia jej wyświetlanie Twojej `Web.config` s zawartość pliku? Co można atakujący z tymi informacjami, i poznać kroki, jakie można podjąć w celu jeszcze lepiej chronić poufne informacje w ramach `Web.config`? Na szczęście większość części `Web.config` nie zawierają informacji poufnych. Co można osoba atakująca bardzo jeśli znają nazwy domyślnego motywu używanego przez stron ASP.NET?

Niektóre `Web.config` sekcje, jednak zawierają poufne informacje, które mogą zawierać parametrów połączenia, nazwy użytkowników, hasła, nazwy serwerów, kluczy szyfrowania i tak dalej. Te informacje zazwyczaj znajduje się w następujących `Web.config` sekcje:

- `<appSettings>`
- `<connectionStrings>`
- `<identity>`
- `<sessionState>`

W tym samouczku przedstawiono techniki takich informacji poufnych konfiguracji ochrony. Jak zostanie wyświetlone, .NET Framework w wersji 2.0 obejmuje systemu chronionego konfiguracji, który sprawia, że sekcje wybranej konfiguracji programowo szyfrowania i odszyfrowywania błyskawicznie.

> [!NOTE]
> Ten samouczek zawiera wyglądu w Microsoft s zalecenia dotyczące połączenia z bazą danych z aplikacji ASP.NET. Oprócz szyfrowania parametry połączenia, możesz pomóc zabezpieczyć systemu zapewniając, że łączysz się z bazą danych w sposób bezpieczny.


## <a name="step-1-exploring-aspnet-20-s-protected-configuration-options"></a>Krok 1: Eksploracji ASP.NET 2.0 s chronione opcje konfiguracji

Platforma ASP.NET 2.0 zawiera system konfiguracji chronionych do szyfrowania i odszyfrowywania informacji o konfiguracji. Dotyczy to również metody w programie .NET Framework, który może służyć do programowego zaszyfrować lub odszyfrować informacje o konfiguracji. System konfiguracji chronionych za pomocą [modelu dostawcy](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), który umożliwia deweloperom wybierz, jakie implementacji usług kryptograficznych jest używany.

.NET Framework jest dostarczany z dwóch dostawców chronionych konfiguracji:

- [`RSAProtectedConfigurationProvider`](https://msdn.microsoft.com/library/system.configuration.rsaprotectedconfigurationprovider.aspx)-używa asymetrycznego [algorytmu RSA](http://en.wikipedia.org/wiki/Rsa) do szyfrowania i odszyfrowywania.
- [`DPAPIProtectedConfigurationProvider`](https://msdn.microsoft.com/system.configuration.dpapiprotectedconfigurationprovider.aspx)-korzysta z systemu Windows [interfejsu API ochrony danych (DPAPI)](https://msdn.microsoft.com/library/ms995355.aspx) do szyfrowania i odszyfrowywania.

Ponieważ system konfiguracji chronionych implementuje wzorzec projektowy dostawcy, jest możliwość tworzenia własnego dostawcę konfiguracji chronionych i podłącz go do aplikacji. Zobacz [implementacja dostawcę konfiguracji chronione](https://msdn.microsoft.com/library/wfc2t3az(VS.80).aspx) Aby uzyskać więcej informacji na temat tego procesu.

RSA i DPAPI dostawcami używać kluczy dla procedury ich szyfrowania i odszyfrowywania, a te klucze mogą być przechowywane w maszyny - lub -poziomie użytkownika. Poziom maszyny klucze są idealne rozwiązanie w przypadku scenariuszy, w którym działa aplikacja sieci web dedykowany serwer lub jeśli istnieje wiele aplikacji na serwerze, na które należy udostępnić zaszyfrowane informacji. Na poziomie użytkownika klucze są bezpieczniejsze w udostępnionych środowiskach hostingu, gdy inne aplikacje na tym samym serwerze nie należy może odszyfrować s chronione sekcje konfiguracji aplikacji.

W tym samouczku naszych przykładach użyje DPAPI dostawcy i klucze poziom maszyny. W szczególności zajmiemy szyfrowania `<connectionStrings>` sekcji `Web.config`, chociaż system konfiguracji chronionych można używać do szyfrowania większości żadnego `Web.config` sekcji. Informacje na przy użyciu poziomu użytkownika lub dostawca RSA można znaleźć zasobów w sekcji dalsze odczyty na końcu tego samouczka.

> [!NOTE]
> `RSAProtectedConfigurationProvider` i `DPAPIProtectedConfigurationProvider` dostawcy są zarejestrowani w `machine.config` pliku z nazwami dostawców `RsaProtectedConfigurationProvider` i `DataProtectionConfigurationProvider`odpowiednio. Podczas szyfrowania lub odszyfrowywania firma Microsoft będzie konieczne podanie nazwy dostawcy odpowiednie informacje o konfiguracji (`RsaProtectedConfigurationProvider` lub `DataProtectionConfigurationProvider`) zamiast nazwy typu rzeczywistego (`RSAProtectedConfigurationProvider` i `DPAPIProtectedConfigurationProvider`). Można znaleźć `machine.config` w pliku `$WINDOWS$\Microsoft.NET\Framework\version\CONFIG` folderu.


## <a name="step-2-programmatically-encrypting-and-decrypting-configuration-sections"></a>Krok 2: Programowe szyfrowanie i odszyfrowywanie sekcji konfiguracji

Przy użyciu kilku wierszy kodu możemy szyfrowania lub odszyfrowywania sekcji określoną konfigurację przy użyciu określonego dostawcy. Kod, jak firma Microsoft będzie wkrótce, po prostu musi odwoływać się do programowo sekcji odpowiednia konfiguracja wywołać jej `ProtectSection` lub `UnprotectSection` metody, a następnie wywołania `Save` metodę, aby utrwalić zmiany. Ponadto .NET Framework zawiera narzędzia pomocne wiersza polecenia, który można szyfrowania i odszyfrowywania informacji o konfiguracji. Przeanalizujemy to narzędzie wiersza polecenia w kroku 3.

Aby zilustrować programowo ochrona informacji o konfiguracji, umożliwiają s Tworzenie strony platformy ASP.NET, która zawiera przyciski do szyfrowania i odszyfrowywania `<connectionStrings>` sekcji `Web.config`.

Uruchamianie przez otwarcie `EncryptingConfigSections.aspx` strony `AdvancedDAL` folderu. Przeciągnij kontrolki pola tekstowego z przybornika do projektanta, ustawienie jej `ID` właściwości `WebConfigContents`, jego `TextMode` właściwości `MultiLine`i jego `Width` i `Rows` właściwości 95% i 15, odpowiednio. Tego formantu TextBox wyświetli zawartość `Web.config` pozwalając nam szybko sprawdzić, czy zawartość są szyfrowane, czy nie. Oczywiście w rzeczywistej aplikacji nigdy nie należy wyświetlić zawartość `Web.config`.

Poniżej pola tekstowego, dodaj dwa formanty przycisk o nazwie `EncryptConnStrings` i `DecryptConnStrings`. Ustaw właściwości ich tekst na parametry połączenia z szyfrowania i odszyfrowywania parametry połączenia.

W tym momencie ekranie powinien wyglądać podobnie do rysunku 2.


[![Dodaj pole tekstowe i dwóch przycisk formantów sieci Web do strony](protecting-connection-strings-and-other-configuration-information-cs/_static/image5.png)](protecting-connection-strings-and-other-configuration-information-cs/_static/image4.png)

**Rysunek 2**: na stronie Dodaj pole tekstowe i dwóch formantów sieci Web przycisk ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](protecting-connection-strings-and-other-configuration-information-cs/_static/image6.png))


Następnie należy napisać kod, który ładuje i wyświetla zawartość `Web.config` w `WebConfigContents` pole tekstowe, gdy strona zostanie załadowana. Dodaj następujący kod do klasy związane z kodem s strony. Ten kod dodaje metodę o nazwie `DisplayWebConfig` i wywołuje ona z `Page_Load` obsługi zdarzeń podczas `Page.IsPostBack` jest `false`:


[!code-csharp[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample1.cs)]

`DisplayWebConfig` Używa metody [ `File` klasy](https://msdn.microsoft.com/library/system.io.file.aspx) otworzyć s aplikacji `Web.config` pliku [ `StreamReader` klasy](https://msdn.microsoft.com/library/system.io.streamreader.aspx) Aby przeczytać jego zawartość do ciągu i [ `Path` klasy](https://msdn.microsoft.com/library/system.io.path.aspx) do ścieżki fizycznej do generowania `Web.config` pliku. Te trzy klasy zostały znalezione w [ `System.IO` przestrzeni nazw](https://msdn.microsoft.com/library/system.io.aspx). W związku z tym, należy dodać `using` `System.IO` instrukcji u góry klasy związane z kodem, lub te klasy nazw z prefiksem `System.IO.` .

Następnie należy dodać obsługę zdarzeń dla dwóch formantów przycisk `Click` zdarzeń i Dodaj kod niezbędne do szyfrowania i odszyfrowywania `<connectionStrings>` sekcji przy użyciu klucza na poziomie maszyny z dostawcą DPAPI. Przy użyciu projektanta, kliknij dwukrotnie przycisków, aby dodać `Click` obsługi zdarzeń w CodeBehind klasy, a następnie dodaj następujący kod:


[!code-csharp[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample2.cs)]

Kod używany w obsłudze dwa zdarzenia jest niemal identyczna. Oba uruchomić pobieranie informacji na temat bieżącego s aplikacji `Web.config` plików za pomocą [ `WebConfigurationManager` klasy](https://msdn.microsoft.com/library/system.web.configuration.webconfigurationmanager.aspx) s [ `OpenWebConfiguration` metody](https://msdn.microsoft.com/library/system.web.configuration.webconfigurationmanager.openwebconfiguration.aspx). Ta metoda zwraca pliku konfiguracji sieci web dla określonej ścieżki wirtualnej. Następnie `Web.config` pliku s `<connectionStrings>` części jest dostępny za pośrednictwem [ `Configuration` klasy](https://msdn.microsoft.com/library/system.configuration.configuration.aspx) s [ `GetSection(sectionName)` — metoda](https://msdn.microsoft.com/library/system.configuration.configuration.getsection.aspx), która zwraca wartość [ `ConfigurationSection` ](https://msdn.microsoft.com/library/system.configuration.configurationsection.aspx) obiektu.

`ConfigurationSection` Obiekt zawiera [ `SectionInformation` właściwość](https://msdn.microsoft.com/library/system.configuration.configurationsection.sectioninformation.aspx) zapewnia dodatkowe informacje i funkcje dotyczące sekcji konfiguracji. Jako kod powyżej przedstawiono, możemy określić, czy sekcja konfiguracji jest szyfrowany przez sprawdzenie `SectionInformation` właściwości s `IsProtected` właściwości. Ponadto można zaszyfrowanie lub odszyfrowanie za pośrednictwem sekcji `SectionInformation` właściwości s `ProtectSection(provider)` i `UnprotectSection` metody.

`ProtectSection(provider)` Metoda przyjmuje jako dane wejściowe ciąg określający nazwę dostawcy konfiguracji chronionych używanego do szyfrowania. W `EncryptConnString` przekazywana DataProtectionConfigurationProvider do obsługi zdarzeń przycisk s `ProtectSection(provider)` metody, aby dostawca DPAPI jest używany. `UnprotectSection` Metody można określić dostawcę, który został użyty do zaszyfrowania sekcji konfiguracji i w związku z tym nie wymaga żadnego parametrów wejściowych.

Po wywołaniu `ProtectSection(provider)` lub `UnprotectSection` metody należy wywołać `Configuration` obiektu s [ `Save` metody](https://msdn.microsoft.com/library/system.configuration.configuration.save.aspx) aby utrwalić zmiany. Gdy został zaszyfrowany lub odszyfrować informacje o konfiguracji i zmiany zapisane, nazywamy `DisplayWebConfig` załadować zaktualizowane `Web.config` zawartości w formancie TextBox.

Po wprowadzeniu kodu powyżej, przetestować go odwiedzając `EncryptingConfigSections.aspx` strony za pośrednictwem przeglądarki. Powinny pojawić początkowo strona, która wyświetla zawartość `Web.config` z `<connectionStrings>` sekcji wyświetlany w postaci zwykłego tekstu (patrz rysunek 3).


[![Dodaj pole tekstowe i dwóch przycisk formantów sieci Web do strony](protecting-connection-strings-and-other-configuration-information-cs/_static/image8.png)](protecting-connection-strings-and-other-configuration-information-cs/_static/image7.png)

**Rysunek 3**: na stronie Dodaj pole tekstowe i dwóch formantów sieci Web przycisk ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](protecting-connection-strings-and-other-configuration-information-cs/_static/image9.png))


Teraz kliknij przycisk szyfrowania parametrów połączenia. Jeśli weryfikacja żądania jest włączona, znaczników odesłana `WebConfigContents` utworzy pole tekstowe `HttpRequestValidationException`, która wyświetla komunikat potencjalnie niebezpiecznych `Request.Form` wartość Wykryto od klienta. Sprawdzanie poprawności żądań, które jest domyślnie włączona w programie ASP.NET 2.0, zabrania ogłaszania zwrotnego obejmujących cofanie zakodowany w formacie HTML i ma na celu zapobieganie atakom uruchomienie skryptu. Na stronie - lub -poziomie aplikacji można wyłączyć ten test. Aby ją wyłączyć na tej stronie, ustaw `ValidateRequest` ustawienie `false` w `@Page` dyrektywy. `@Page` Dyrektywy znajduje się w górnej części s deklaratywne znaczników.


[!code-aspx[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample3.aspx)]

Więcej informacji o weryfikacji żądań, jej celem ją wyłączyć w strony - i -poziomie aplikacji, jak oraz do formatu HTML kodowania znaczników, zobacz [weryfikacji żądań - zapobieganie atakom skryptu](../../../../whitepapers/request-validation.md).

Wyłączenie sprawdzania poprawności żądania na stronie, a następnie spróbuj ponowne kliknięcie przycisku szyfrowania parametrów połączenia. Strony, będzie dostępna w pliku konfiguracji i jego `<connectionStrings>` sekcji zaszyfrowane za pomocą dostawcy DPAPI. Pole tekstowe jest następnie aktualizowany, aby wyświetlić nowe `Web.config` zawartości. Jak pokazano na rysunku 4, `<connectionStrings>` informacji jest teraz zaszyfrowana.


[![Kliknięcie przycisku szyfrowanie połączenia ciągi przycisk szyfruje &lt;connectionString&gt; sekcji](protecting-connection-strings-and-other-configuration-information-cs/_static/image11.png)](protecting-connection-strings-and-other-configuration-information-cs/_static/image10.png)

**Rysunek 4**: kliknięcie przycisku szyfrowanie połączenia ciągi przycisk szyfruje `<connectionString>` sekcji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](protecting-connection-strings-and-other-configuration-information-cs/_static/image12.png))


Zaszyfrowanego `<connectionStrings>` sekcji generowane na tym komputerze jest zgodny, mimo że niektóre zawartości w `<CipherData>` element został usunięty w celu jego skrócenia:


[!code-xml[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample4.xml)]

> [!NOTE]
> `<connectionStrings>` Element określa dostawcę, który umożliwia wykonanie szyfrowania (`DataProtectionConfigurationProvider`). Te informacje są używane przez `UnprotectSection` metody po kliknięciu przycisku odszyfrować parametry połączenia.


Jeśli informacje o parametrach połączenia jest dostępny z `Web.config` — za pomocą kodu możemy zapisać z formantu SqlDataSource lub automatycznie wygenerowany kod z TableAdapters w naszym wpisanych zestawów danych - jest automatycznie odszyfrowany. Krótko mówiąc, firma Microsoft nie trzeba dodawać żaden dodatkowy kod lub logiki można odszyfrować zaszyfrowanego `<connectionString>` sekcji. Aby to wykazać, odwiedź jedną z wcześniej samouczków w tej chwili, takich jak z podstawowym raportowaniem sekcji samouczka proste wyświetlania (`~/BasicReporting/SimpleDisplay.aspx`). Jak pokazano na rysunku nr 5, samouczka działa dokładnie tak, jak firma Microsoft mogą spodziewać, wskazujący, że zaszyfrowanego ciągu połączenia jest automatycznie odszyfrowywany za pomocą stron ASP.NET.


[![Warstwa dostępu do danych automatycznie odszyfrowuje ciągu połączenia](protecting-connection-strings-and-other-configuration-information-cs/_static/image14.png)](protecting-connection-strings-and-other-configuration-information-cs/_static/image13.png)

**Rysunek 5**: Warstwa dostępu do danych automatycznie odszyfrowuje ciągu połączenia ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](protecting-connection-strings-and-other-configuration-information-cs/_static/image15.png))


Aby przywrócić `<connectionStrings>` sekcji do jej reprezentacji w postaci zwykłego tekstu, kliknij przycisk odszyfrować parametry połączenia. Strony powinna zostać wyświetlona parametry połączenia w `Web.config` w postaci zwykłego tekstu. W tym momencie ekranie powinna wyglądać tak, jak podczas odwiedzania najpierw tej strony (patrz rysunek 3).

## <a name="step-3-encrypting-configuration-sections-usingaspnetregiisexe"></a>Krok 3: Szyfrowanie przy użyciu sekcji konfiguracji`aspnet_regiis.exe`

.NET Framework zawiera szereg narzędzi wiersza polecenia w `$WINDOWS$\Microsoft.NET\Framework\version\` folderu. W [przy użyciu zależności buforu SQL](../caching-data/using-sql-cache-dependencies-cs.md) samouczek, na przykład, analizujemy przy użyciu `aspnet_regsql.exe` narzędzia wiersza polecenia, aby dodać infrastrukturę niezbędną do zależności buforu SQL. Kolejnym narzędziem wiersza polecenia przydatne w tym folderze jest [narzędzie rejestracji programu ASP.NET w usługach IIS (`aspnet_regiis.exe`)](https://msdn.microsoft.com/library/k6h9cz8h(VS.80).aspx). Jak jego nazwa wskazuje, narzędzie rejestracji programu ASP.NET w usługach IIS głównie służy do rejestrowania aplikacji programu ASP.NET 2.0 z serwera sieci Web Microsoft s klasy professional, usługi IIS. Oprócz jego funkcji związanych z usługami IIS, narzędzie rejestracji programu ASP.NET w usługach IIS można również zaszyfrować lub odszyfrować sekcji konfiguracji określonej w `Web.config`.

Następująca instrukcja przedstawiono składnię ogólne używany do szyfrowania sekcji konfiguracji z `aspnet_regiis.exe` narzędzia wiersza polecenia:


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample5.cmd)]

*sekcja* jest sekcja konfiguracyjna do szyfrowania (na przykład connectionStrings), *fizycznych\_katalogu* jest pełna, fizyczną ścieżkę do katalogu głównego aplikacji s sieci web i *dostawcy*  jest nazwą dostawcy chronionych konfiguracji do użycia (na przykład DataProtectionConfigurationProvider). Alternatywnie w aplikacji sieci web jest zarejestrowana w usługach IIS można wprowadzić ścieżkę wirtualną zamiast ścieżki fizycznej, używając następującej składni:


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample6.cmd)]

Następujące `aspnet_regiis.exe` szyfruje przykład `<connectionStrings>` sekcji przy użyciu dostawcy DPAPI przy użyciu klucza na poziomie komputera:


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample7.cmd)]

Podobnie `aspnet_regiis.exe` narzędzia wiersza polecenia może służyć do odszyfrowywania sekcji konfiguracyjnych. Zamiast `-pef` przełącznika, użyj `-pdf` (lub zamiast `-pe`, użyj `-pd`). Należy również zauważyć, że nazwa dostawcy nie jest konieczne podczas odszyfrowywania.


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample8.cmd)]

> [!NOTE]
> Ponieważ używamy dostawcy DPAPI, która używa kluczy specyficzne dla komputera, należy uruchomić `aspnet_regiis.exe` z tym samym komputerze, z którego są obsługiwane na stronach sieci web. Na przykład jeśli uruchomienia tego programu wiersza polecenia z komputera lokalnego programowanie, a następnie przekaż zaszyfrowany plik Web.config na serwerze produkcyjnym, serwer produkcyjny nie będzie może odszyfrować ciągu połączenia, ponieważ został on zaszyfrowany przy użyciu kluczy określonych na komputerze deweloperskim. Dostawca RSA nie ma tego ograniczenia, jak można wyeksportować kluczy RSA do innej maszyny.


## <a name="understanding-database-authentication-options"></a>Opcje uwierzytelniania opis bazy danych

Przed dowolnej aplikacji, można wystawiać `SELECT`, `INSERT`, `UPDATE`, lub `DELETE` najpierw zapytania do bazy danych programu Microsoft SQL Server, bazy danych należy określić obiekt żądający. Ten proces jest nazywany *uwierzytelniania* i SQL Server udostępnia dwie metody uwierzytelniania:

- **Uwierzytelnianie systemu Windows** — proces, na którym uruchomiono aplikację jest używany do komunikacji z bazą danych. Podczas uruchamiania aplikacji ASP.NET przy użyciu programu Visual Studio 2005 s ASP.NET Development Server, aplikacja ASP.NET zakłada tożsamości aktualnie zalogowanego użytkownika. Dla aplikacji ASP.NET w programie Microsoft Internet Information Server (IIS), aplikacji programu ASP.NET zwykle przyjąć tożsamość `domainName``\MachineName` lub `domainName``\NETWORK SERVICE`, mimo to można dostosować.
- **Uwierzytelnianie SQL** -wartości ID i hasło użytkownika są określane jako poświadczeń do uwierzytelniania. Z uwierzytelnianiem SQL identyfikator użytkownika i hasło są zawarte w parametrach połączenia.

Uwierzytelnianie systemu Windows jest preferowana przez uwierzytelnianie SQL, ponieważ jest bardziej bezpieczne. Z uwierzytelnianiem systemu Windows ciąg połączenia jest wolny od nazwy użytkownika i hasła i jeśli serwer sieci web i serwera bazy danych znajdują się na dwóch różnych komputerach, poświadczenia nie są wysyłane przez sieć jako zwykły tekst. Z uwierzytelnianiem SQL jednak poświadczenia uwierzytelniania są zakodowane na stałe w parametrach połączenia i są przesyłane z serwera sieci web na serwerze bazy danych w postaci zwykłego tekstu.

Te samouczki były używane uwierzytelnianie systemu Windows. Można ustalić trybu uwierzytelniania jest używany przez badanie parametry połączenia. Parametry połączenia w `Web.config` dla naszych samouczków został:

`Data Source=.\SQLEXPRESS; AttachDbFilename=|DataDirectory|\NORTHWND.MDF; Integrated Security=True; User Instance=True`

Zintegrowane zabezpieczenia = True i braku nazwy użytkownika i hasła wskazują, że jest używane uwierzytelnianie systemu Windows. Niektóre połączenia ciągi termin zaufanego połączenia = Yes "lub" Integrated Security = SSPI jest używany zamiast Integrated Security = True, ale wszystkie trzy wskazuje używanie uwierzytelniania systemu Windows.

W poniższym przykładzie przedstawiono parametry połączenia, które korzysta z uwierzytelniania SQL. Należy zwrócić uwagę, poświadczenia osadzone w ciągu połączenia:

`Server=serverName; Database=Northwind; uid=userID; pwd=password`

Załóżmy, że osoba atakująca jest możliwość wyświetlania s Twojej aplikacji `Web.config` pliku. Jeśli używane jest uwierzytelnianie SQL do połączenia z bazą danych, który jest dostępny w Internecie, osoba atakująca można użyć tego ciągu połączenia do połączenia z bazą danych przy użyciu programu SQL Management Studio lub ze stron ASP.NET na własne witryny sieci Web. Aby ułatwić uniknięcie tego zagrożenia, należy zaszyfrować ciągu połączenia w `Web.config` przy użyciu systemu konfiguracji chronionych.

> [!NOTE]
> Aby uzyskać więcej informacji na temat różnych typów uwierzytelniania dostępnych w programie SQL Server, zobacz [tworzenie zabezpieczanie aplikacji ASP.NET: uwierzytelnianie, autoryzację i bezpieczna komunikacja](https://msdn.microsoft.com/library/aa302392.aspx). Dalsze połączenia ciąg przykłady ilustrujące różnice między składni uwierzytelniania systemu Windows i SQL, można znaleźć w temacie [ConnectionStrings.com](http://www.connectionstrings.com/).


## <a name="summary"></a>Podsumowanie

Domyślnie pliki z `.config` nie można uzyskać dostępu do rozszerzenia w aplikacji ASP.NET za pośrednictwem przeglądarki. Nie są zwracane tych typów plików, ponieważ mogą zawierać poufne informacje, takie jak parametry połączenia bazy danych, nazwy użytkowników i hasła i tak dalej. System konfiguracji chronionych w .NET 2.0 pomaga jeszcze lepiej chronić informacje poufne, zezwalając sekcje określoną konfigurację do zaszyfrowania. Dwóch dostawców wbudowanych konfiguracji chronionych:, który używa algorytmu RSA, a drugi używa interfejsu API ochrony danych systemu Windows (DPAPI).

W tym samouczku analizujemy jak do szyfrowania i odszyfrowywania przy użyciu dostawcy DPAPI ustawienia konfiguracji. Można to osiągnąć programowo, jak widzieliśmy w kroku 2, jak również jako za pomocą `aspnet_regiis.exe` narzędzia wiersza polecenia, które zostało omówione w kroku 3. Aby uzyskać więcej informacji na przy użyciu poziomu użytkownika lub dostawca RSA zamiast tego Zobacz zasoby w sekcji dalsze informacje.

Programowanie przyjemność!

## <a name="further-reading"></a>Dalsze informacje

Więcej informacji dotyczących tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Tworzenie aplikacji ASP.NET bezpiecznego: Uwierzytelniania, autoryzacji i bezpiecznej komunikacji](https://msdn.microsoft.com/library/aa302392.aspx)
- [Szyfrowanie informacji o konfiguracji w programie ASP.NET 2.0 aplikacji](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx)
- [Szyfrowanie `Web.config` wartości w programie ASP.NET 2.0](https://weblogs.asp.net/scottgu/archive/2006/01/09/434893.aspx)
- [Porady: Szyfrowanie sekcji konfiguracyjnych w programie ASP.NET 2.0 przy użyciu DPAPI](https://msdn.microsoft.com/library/ms998280.aspx)
- [Porady: Szyfrowanie sekcji konfiguracyjnych w programie ASP.NET 2.0 przy użyciu RSA](https://msdn.microsoft.com/library/ms998283.aspx)
- [Interfejs API konfiguracji w programie .NET 2.0](http://www.odetocode.com/Articles/418.aspx)
- [Ochrona danych systemu Windows](https://msdn.microsoft.com/library/ms995355.aspx)

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Prowadzić osób dokonujących przeglądu, w tym samouczku zostały Teresa Murphy i Randy Schmidt. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Poprzednie](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs.md)
[dalej](debugging-stored-procedures-cs.md)
