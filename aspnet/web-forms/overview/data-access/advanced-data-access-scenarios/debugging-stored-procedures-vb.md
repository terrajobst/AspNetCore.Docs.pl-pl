---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/debugging-stored-procedures-vb
title: Debugowanie procedur składowanych (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Wersje Visual Studio Professional i Team System umożliwiają Ustaw punkty przerwania i Wkrocz do procedury składowane w programie SQL Server, co debugowania przechowywane...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/03/2007
ms.topic: article
ms.assetid: 9ed8ccb5-5f31-4eb4-976d-cabf4b45ca09
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/debugging-stored-procedures-vb
msc.type: authoredcontent
ms.openlocfilehash: 3391a78eaeb0add46e75048069a614ba00628f67
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="debugging-stored-procedures-vb"></a>Debugowanie procedur składowanych (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz kod](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_74_VB.zip) lub [pobierania plików PDF](debugging-stored-procedures-vb/_static/datatutorial74vb1.pdf)

> Wersje Visual Studio Professional i Team System umożliwiają Ustaw punkty przerwania i Wkrocz do procedury składowane w programie SQL Server, co debugowanie procedur składowanych, wystarczy debugowanie kodu aplikacji. W tym samouczku przedstawiono debugowania bezpośredniego bazy danych oraz procedur składowanych debugowania aplikacji.


## <a name="introduction"></a>Wprowadzenie

Program Visual Studio udostępnia bogate środowisko debugowania. Z kilku naciśnięć klawiszy lub kliknięcia myszy go s można używać punktów przerwania, aby zatrzymać wykonanie programu i sprawdź jej stan i kontroli przepływu. Wraz z debugowanie kodu aplikacji, program Visual Studio oferuje obsługę debugowania procedur składowanych z programu SQL Server. Podobnie można ustawić punktów przerwania w kodzie ASP.NET klasę kodu lub klasy warstwy logiki biznesowej, zbyt one można umieścić w ramach procedur składowanych.

W tym samouczku przedstawiono Wkraczanie do procedur składowanych z Eksploratora serwera w programie Visual Studio oraz jak można ustawić punktów przerwania, które są osiągane, gdy procedura składowana jest wywoływana z działającej aplikacji ASP.NET.

> [!NOTE]
> Niestety procedur składowanych tylko można przeprowadził w i debugować przez wersje Professional i systemy zespołu programu Visual Studio. Jeśli używasz wersji standard programu Visual Studio lub Visual Web Developer to Zapraszamy odczytu wzdłuż możemy przeprowadzenie czynności niezbędnych do debugowania procedur składowanych, ale nie można replikować następujące kroki na komputerze.


## <a name="sql-server-debugging-concepts"></a>Pojęcia debugowanie serwera SQL

Microsoft SQL Server 2005 zaprojektowano tak, aby zapewnić integrację z [środowiska uruchomieniowego języka wspólnego (CLR)](https://msdn.microsoft.com/netframework/aa497266.aspx), który jest używany przez wszystkie zestawy .NET środowiska uruchomieniowego. W związku z tym SQL Server 2005 obsługuje obiektów zarządzanych bazy danych. Oznacza to, że można tworzyć obiektów bazy danych, takich jak procedury składowane i funkcje zdefiniowane przez użytkownika (UDF) jako metody w klasie Visual Basic. Dzięki temu te procedury składowane i funkcje UDF mogą korzystać z funkcji w programie .NET Framework i z klas niestandardowych. Oczywiście SQL Server 2005 także zapewnia obsługę T-SQL obiektów bazy danych.

SQL Server 2005 oferuje obsługę debugowania dla T-SQL i obiekty zarządzane bazy danych. Jednak te obiekty może być debugowany tylko przez wersje programu Visual Studio 2005 Professional i systemy zespołu. W tym samouczku omówione obiektów bazy danych w usłudze debugowania T-SQL. Samouczek kolejnych analizuje debugowanie zarządzane obiekty.

[Omówienie T-SQL i CLR debugowania w programie SQL Server 2005](https://blogs.msdn.com/sqlclr/archive/2006/06/29/651644.aspx) wpis w blogu z [integracji środowiska CLR programu SQL Server 2005 team](https://blogs.msdn.com/sqlclr/default.aspx) wyróżnia trzy sposoby debugowania obiektów programu SQL Server 2005 z programu Visual Studio:

- **Bezpośrednie debugowania bazy danych (DDD)** — z Eksploratora serwera, firma Microsoft może wkroczyć do obiektu do bazy danych T-SQL, takich jak procedury składowane i funkcje UDF. Omówione DDD w kroku 1.
- **Debugowanie aplikacji** -możemy ustawić punkty przerwania w ramach obiektu bazy danych, a następnie uruchom naszej aplikacji ASP.NET. Kiedy obiekt bazy danych jest wykonywane, punkt przerwania zostanie uruchomiona, a kontroli do debugera. Należy pamiętać, że z debugowaniem aplikacji firma Microsoft nie można wykonać kroku do obiektu bazy danych z kodu aplikacji. Firma Microsoft musi jawnie ustaw punkty przerwania w tych procedur składowanych lub funkcji UDF gdy chcemy debugera, aby zatrzymać. Debugowanie aplikacji jest zbadać uruchamianie w kroku 2.
- **Debugowanie z projektu programu SQL Server** — wersje programu Visual Studio Professional i systemy zespołu zawierać typu Projekt serwera SQL, który jest najczęściej używany do tworzenia obiektów zarządzanych bazy danych. Omówione zostaną przy użyciu programu SQL Server — projekty i debugowanie ich zawartość w następnym samouczku.

Visual Studio można debugować procedury przechowywane w lokalnych i zdalnych wystąpieniach programu SQL Server. Lokalne wystąpienie programu SQL Server to taki, który jest zainstalowany na tym samym komputerze co program Visual Studio. Jeśli bazy danych SQL Server, którego używasz, nie znajduje się na komputerze deweloperskim, następnie uważa się wystąpienia zdalnego. Te samouczki możemy korzystają lokalnego wystąpienia programu SQL Server. Debugowanie procedur składowanych na zdalnym wystąpieniu programu SQL server wymaga większej liczby kroków konfiguracji niż podczas debugowania procedur przechowywanych w lokalnym wystąpieniu.

Jeśli korzystasz z lokalnego wystąpienia programu SQL Server, można uruchomić z kroku 1 i skorzystać z tego samouczka na końcu. Jeśli używasz zdalnego wystąpienia programu SQL Server, jednak użytkownik będzie upewnij się, że podczas debugowania, możesz pierwszy potrzebę są rejestrowane na komputerze deweloperskim przy użyciu konta użytkownika systemu Windows ma logowania programu SQL Server w wystąpieniu zdalnym. Moveover, zarówno w przypadku tej nazwy logowania bazy danych, jak i nazwy logowania bazy danych używane do łączenia z bazą danych z działającej aplikacji ASP.NET muszą być elementami członkowskimi `sysadmin` roli. Zobacz debugowania T-SQL obiektów bazy danych w sekcji wystąpień zdalnego na końcu tego samouczka, aby uzyskać więcej informacji na temat konfigurowania programu Visual Studio i SQL Server do debugowania zdalnego wystąpienia.

Na koniec Dowiedz się, że debugowanie obsługę obiektów bazy danych T-SQL nie jest jako funkcja sformatowanego jako debugowania obsługę aplikacji .NET. Na przykład warunków punktu przerwania i filtry nie są obsługiwane, tylko podzbiór debugowania systemu windows są dostępne, nie można użyć Edytuj i Kontynuuj, w oknie bezpośrednim jest renderowany bezużyteczne itd. Zobacz [mają zastosowanie ograniczenia dotyczące poleceń debugera i funkcji](https://msdn.microsoft.com/library/ms165035(VS.80).aspx) Aby uzyskać więcej informacji.

## <a name="step-1-directly-stepping-into-a-stored-procedure"></a>Krok 1: Bezpośrednio Wkraczanie do procedury składowanej

Visual Studio ułatwia bezpośrednio debugowania obiektu bazy danych. Let s, zobacz, jak korzystać z funkcji bezpośredniego debugowania bazy danych (DDD) Aby wkraczać do `Products_SelectByCategoryID` procedury przechowywanej w bazie danych Northwind. Jak jego nazwa wskazuje, `Products_SelectByCategoryID` zwraca informacje o produkcie dla określonej kategorii; został utworzony w [za pomocą istniejących procedur składowanych s wpisane DataSet TableAdapters](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) samouczka. Uruchom, przechodząc do Eksploratora serwera, a następnie rozwiń węzeł bazy danych Northwind. Następnie należy przejść do folderu procedur składowanych, kliknij prawym przyciskiem myszy `Products_SelectByCategoryID` procedury składowanej i wybierz opcję krok do procedury składowanej z menu kontekstowego. Spowoduje to uruchomienie debugera.

Ponieważ `Products_SelectByCategoryID` oczekuje procedury składowanej `@CategoryID` parametru wejściowego, firma Microsoft jest proszony o podanie wartości. Wprowadź wartość 1, która zwraca informacje o napoje.


![Użyj wartości 1 dla @CategoryID parametru](debugging-stored-procedures-vb/_static/image1.png)

**Rysunek 1**: Użyj wartości 1 dla `@CategoryID` parametru


Po dostarczeniu wartość `@CategoryID` parametru procedury składowanej jest wykonywana. Zamiast uruchamiania do zakończenia, jednak debuger zatrzymuje wykonanie w pierwszej instrukcji. Należy pamiętać, żółta strzałka na marginesie i wskazujący bieżącą lokalizację w procedurze składowanej. Można wyświetlać i edytować wartości parametrów za pomocą okna czujki lub umieszczając kursor myszy nad nazwę parametru w procedurze składowanej.


[![Debuger został zatrzymany na pierwszej instrukcji procedury składowanej](debugging-stored-procedures-vb/_static/image3.png)](debugging-stored-procedures-vb/_static/image2.png)

**Rysunek 2**: debuger został zatrzymany na pierwszej instrukcji procedury składowanej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](debugging-stored-procedures-vb/_static/image4.png))


Do wykonania kroków opisanych jednej instrukcji procedury składowanej w czasie, kliknij przycisk Step Over w pasku narzędzi lub naciśnij klawisz F10. `Products_SelectByCategoryID` Procedury składowanej zawiera jeden `SELECT` instrukcji, więc naciśnięcie klawisza F10 będzie Przekrocz nad jednej instrukcji i zakończyć wykonywania procedury składowanej. Po zakończeniu procedury składowanej, dane wyjściowe będą wyświetlane w oknie danych wyjściowych i spowoduje przerwanie w debugerze.

> [!NOTE]
> Debugowania T-SQL występuje na poziomie instrukcji; Nie można wykonać kroku do `SELECT` instrukcji.


## <a name="step-2-configuring-the-website-for-application-debugging"></a>Krok 2: Konfigurowanie witryny sieci Web do debugowania aplikacji

Podczas debugowania procedury składowanej bezpośrednio z poziomu Eksploratora serwera jest przydatne, w wielu scenariuszach NAS interesuje więcej debugowania procedury składowanej, gdy jest wywoływana z naszej aplikacji ASP.NET. Możemy dodać punkty przerwania do procedury składowanej z poziomu programu Visual Studio, a następnie Rozpocznij debugowanie aplikacji ASP.NET. Po wywołaniu procedury składowanej z punktów przerwania z aplikacji, wykonanie spowoduje zatrzymanie na punkt przerwania i firma Microsoft wyświetlenie i zmianę wartości parametru procedury składowanej s i kroków opisanych w jego instrukcje, tak jak opisano w kroku 1.

Przed możemy rozpocząć debugowanie procedury składowane wywoływane z aplikacji, należy poinstruować aplikacja sieci web ASP.NET w celu zintegrowania z debugera programu SQL Server. Uruchomić, klikając prawym przyciskiem myszy nazwę witryny sieci Web w Eksploratorze rozwiązań (`ASPNET_Data_Tutorial_74_VB`). Wybierz opcję strony właściwości z menu kontekstowego wybierz pozycję Opcje uruchamiania po lewej stronie i zaznacz pole wyboru programu SQL Server, w sekcji debugery (patrz rysunek 3).


[![Zaznacz pole wyboru serwera SQL na stronach właściwości s aplikacji](debugging-stored-procedures-vb/_static/image6.png)](debugging-stored-procedures-vb/_static/image5.png)

**Rysunek 3**: pole wyboru programu SQL Server w aplikacji s strony właściwości ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](debugging-stored-procedures-vb/_static/image7.png))


Ponadto należy zaktualizować parametry połączenia bazy danych używane przez aplikację tak, czy buforowanie połączeń jest wyłączona. Zamknięcie połączenia z bazą danych odpowiadającego `SqlConnection` obiekt znajduje się w puli dostępnych połączeń. Podczas nawiązywania połączenia z bazą danych, obiektu dostępne połączenia mogą być pobierane z tej puli zamiast konieczności tworzenia i nawiązania nowego połączenia. To buforowanie obiektów połączenia jest zwiększenie wydajności i jest domyślnie włączona. Jednak w przypadku debugowania chcemy wyłączyć buforowanie połączeń, ponieważ debugowania infrastruktury jest nie poprawnie została przywrócona podczas pracy z połączenia, która została wykonana z puli.

Aby Buforowanie wyłączone połączeń, należy zaktualizować `NORTHWNDConnectionString` w `Web.config` co zawierają one ustawienia `Pooling=false` .


[!code-xml[Main](debugging-stored-procedures-vb/samples/sample1.xml)]

> [!NOTE]
> Po zakończeniu debugowania programu SQL Server za pośrednictwem aplikacji ASP.NET należy przywrócić puli połączeń przez usunięcie `Pooling` ustawienie z ciągu połączenia (lub przez ustawienie jej na `Pooling=true` ).


W tym momencie umożliwia Visual Studio, aby debugować obiektów bazy danych programu SQL Server, gdy została wywołana przy użyciu aplikacji sieci web skonfigurowano aplikacji ASP.NET. Wszystkie punkty, które pozostaje teraz jest dodanie punktu przerwania do procedury składowanej i Rozpocznij debugowanie!

## <a name="step-3-adding-a-breakpoint-and-debugging"></a>Krok 3: Dodawanie punktu przerwania i debugowania

Otwórz `Products_SelectByCategoryID` procedury składowanej i ustaw punkt przerwania na początku `SELECT` instrukcji, klikając na marginesie w odpowiednim miejscu lub umieszczając kursor na początku `SELECT` instrukcji i naciśnięcie klawisza F9. Jak pokazano na rysunku 4, punkt przerwania jest wyświetlany jako czerwone kółko na marginesie.


[![Ustaw punkt przerwania w Products_SelectByCategoryID procedury składowanej](debugging-stored-procedures-vb/_static/image9.png)](debugging-stored-procedures-vb/_static/image8.png)

**Rysunek 4**: Ustaw punkt przerwania `Products_SelectByCategoryID` procedury składowanej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](debugging-stored-procedures-vb/_static/image10.png))


Aby dla obiektu bazy danych SQL do debugowania za pośrednictwem aplikacji klienckiej konieczne jest skonfigurowania bazy danych do obsługi debugowania aplikacji. Jeśli najpierw ustawić punkt przerwania, powinien automatycznie włączyć to ustawienie, ale zaleca się dokładnie. Kliknij prawym przyciskiem myszy `NORTHWND.MDF` węzła w Eksploratorze serwera. Menu kontekstowe powinna zawierać zaznaczony element menu o nazwie debugowania aplikacji.


![Upewnij się, że jest włączona opcja debugowania aplikacji](debugging-stored-procedures-vb/_static/image11.png)

**Rysunek 5**: Upewnij się, że jest włączona opcja debugowania aplikacji


Ustaw punkt przerwania i włączyć opcję debugowania aplikacji firma Microsoft przystąpić do debugowania procedury składowanej przy wywołaniach z aplikacji ASP.NET. Uruchom debuger, przechodząc do menu Debugowanie i wybieranie Rozpocznij debugowanie przez naciskać klawisz F5 lub przez kliknięcie przycisku zielonego odtwarzania ikonę na pasku narzędzi. To uruchomienia debugera i uruchomić witryny sieci Web.

`Products_SelectByCategoryID` Procedura składowana została utworzona w [za pomocą istniejących procedur składowanych s wpisane DataSet TableAdapters](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) samouczka. Odpowiedniej strony sieci web (`~/AdvancedDAL/ExistingSprocs.aspx`) zawiera element GridView wyświetlający wyników zwróconych przez tę procedurę składowaną. Odwiedź stronę tej strony za pomocą przeglądarki. Na stronie punkt przerwania w osiągnięciu `Products_SelectByCategoryID` procedury składowanej zostanie uruchomiona i kontroli zwróciła dla programu Visual Studio. Podobnie jak w kroku 1 można krok za pomocą procedury składowanej s instrukcje i widoku i modyfikować wartości parametrów.


[![Na stronie ExistingSprocs.aspx początkowo wyświetlane napoje](debugging-stored-procedures-vb/_static/image13.png)](debugging-stored-procedures-vb/_static/image12.png)

**Rysunek 6**: `ExistingSprocs.aspx` początkowo zostanie wyświetlona strona napoje ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](debugging-stored-procedures-vb/_static/image14.png))


[![S procedury składowanej punkt przerwania został osiągnięty](debugging-stored-procedures-vb/_static/image16.png)](debugging-stored-procedures-vb/_static/image15.png)

**Rysunek 7**: s procedury składowanej punkt przerwania został osiągnięty ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](debugging-stored-procedures-vb/_static/image17.png))


Okno czujki w przedstawiono na rysunku nr 7, wartość `@CategoryID` parametru to 1. Jest to spowodowane `ExistingSprocs.aspx` początkowo zostanie wyświetlona strona produktów w należące do tej kategorii, który ma `CategoryID` wartość 1. Wybierz inną kategorię z listy rozwijanej. To powoduje odświeżenie strony, a następnie ponownie wykonuje `Products_SelectByCategoryID` procedury składowanej. Punkt przerwania zostaje trafiony ponownie, ale tym razem `@CategoryID` wartość parametru s odzwierciedla element wybranej listy rozwijanej s `CategoryID`.


[![Wybierz inną kategorię z listy rozwijanej](debugging-stored-procedures-vb/_static/image19.png)](debugging-stored-procedures-vb/_static/image18.png)

**Rysunek 8**: Wybierz inną kategorię z listy rozwijanej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](debugging-stored-procedures-vb/_static/image20.png))


[![@CategoryID Parametr odzwierciedla wybór kategorii na stronie sieci Web](debugging-stored-procedures-vb/_static/image22.png)](debugging-stored-procedures-vb/_static/image21.png)

**Rysunek 9**: `@CategoryID` parametru odzwierciedla wybrać kategorię ze strony sieci Web ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](debugging-stored-procedures-vb/_static/image23.png))


> [!NOTE]
> Jeśli punkt przerwania w `Products_SelectByCategoryID` procedury składowanej nie zostaje trafiony podczas odwiedzania `ExistingSprocs.aspx` upewnij się, że pole wyboru programu SQL Server została sprawdzona w sekcji debugery s stronę właściwości aplikacji ASP.NET, czy buforowanie połączeń został wyłączone i włączenie bazy danych s opcji debugowania aplikacji. Jeśli środowisko odzyskiwania nadal występują problemy, uruchom ponownie program Visual Studio i spróbuj ponownie.


## <a name="debugging-t-sql-database-objects-on-remote-instances"></a>T-SQL bazy danych obiektów w wystąpieniach zdalnego debugowania

Debugowanie obiektów bazy danych za pomocą programu Visual Studio jest bardzo prosta, gdy wystąpienie bazy danych programu SQL Server znajduje się w tym samym komputerze co program Visual Studio. Jednak jeśli program SQL Server i Visual Studio znajdują się na różnych komputerach dokładne konfiguracyjnych jest wymagane do pobrania wszystko działa prawidłowo. Istnieją dwa podstawowych zadań, które firma Microsoft muszą stawiać czoła:

- Upewnij się, że nazwa logowania używana do łączenia z bazą danych za pomocą ADO.NET należy do `sysadmin` roli.
- Upewnij się, że konto użytkownika systemu Windows, które są używane przez program Visual Studio na komputerze deweloperskim jest prawidłowe konto logowania programu SQL Server, który należy do `sysadmin` roli.

Pierwszym krokiem jest stosunkowo proste. Ustalenie konto użytkownika używane do połączenia z bazą danych z aplikacji ASP.NET, a następnie, z programu SQL Server Management Studio, dodaj to konto logowania do `sysadmin` roli.

Drugie zadanie wymaga, aby konto użytkownika systemu Windows, którego używasz do debugowania aplikacji prawidłową nazwą logowania na zdalnej bazy danych. Jednak prawdopodobnie po zalogowaniu do stacji roboczej z konta systemu Windows nie jest prawidłową nazwą logowania na serwerze SQL. Zamiast dodawać konta określonego logowania do programu SQL Server, lepszym rozwiązaniem byłoby wyznaczyć niektóre konta użytkownika systemu Windows jako konto debugowania programu SQL Server. Następnie Aby debugować obiektów bazy danych, zdalnego wystąpienia programu SQL Server, należy uruchomić program Visual Studio przy użyciu tego konta s poświadczenia logowania systemu Windows.

Przykład powinny pomóc w wyjaśnienia rzeczy. Załóżmy, że istnieje konto systemu Windows o nazwie `SQLDebug` w domenie systemu Windows. To konto będzie należy dodać do zdalnego wystąpienia programu SQL Server jako prawidłową nazwą logowania i jako element członkowski `sysadmin` roli. Następnie do debugowania zdalnego wystąpienia programu SQL Server w programie Visual Studio, czy należy uruchomić program Visual Studio jako `SQLDebug` użytkownika. Można to zrobić przez wylogowanie naszych stacji roboczej, logować się ponownie jako `SQLDebug`, a następnie uruchamiania programu Visual Studio, ale prostsze będzie zalogować się do naszej stacji roboczej przy użyciu własnej poświadczeń, a następnie użyć `runas.exe` można uruchomić programu Visual Studio jako `SQLDebug` użytkownika. `runas.exe` Umożliwia określonej aplikacji ma być wykonywana w ramach podszywając innego konta użytkownika. Aby uruchomić program Visual Studio jako `SQLDebug`, można wprowadzić następującą instrukcję w wierszu polecenia:


[!code-console[Main](debugging-stored-procedures-vb/samples/sample2.cmd)]

Aby uzyskać bardziej szczegółowy opis w tym procesie, zobacz [Vaughn R. łączy](http://betav.com/BLOG/billva/) s *Hitchhiker s Przewodnik po Visual Studio i SQL Server Edition siódmego* oraz [jak: Ustawianie uprawnień serwera SQL debugowanie](https://msdn.microsoft.com/library/w1bhybwz(VS.80).aspx).

> [!NOTE]
> Jeśli na komputerze deweloperskim działa dodatku Service Pack 2 dla systemu Windows XP należy skonfigurować zaporę połączenia internetowego w celu zezwolenia na debugowanie zdalne. [Jak do: Włączanie debugowania programu SQL Server 2005](https://msdn.microsoft.com/library/s0fk6z6e(VS.80).aspx) zawiera informacje dotyczące artykułu, ten proces obejmuje dwa kroki: () na komputerze hosta programu Visual Studio, należy dodać `Devenv.exe` do listy wyjątków i otwórz port TCP 135; oraz (b) na komputerze zdalnym (SQL), należy otworzyć TCP 135 portu, a następnie dodaj `sqlservr.exe` do listy wyjątków. Jeśli zasady domeny wymaga komunikacji sieciowej można zrobić za pomocą protokołu IPSec, należy otworzyć porty UDP 4500 i UDP 500.


## <a name="summary"></a>Podsumowanie

Oprócz zapewnienia obsługę debugowania dla kodu aplikacji platformy .NET, program Visual Studio oferuje szeroką gamę opcji debugowania dla programu SQL Server 2005. W tym samouczku analizujemy dwa z tych opcji: bezpośrednie debugowania bazy danych i debugowania aplikacji. Bezpośrednio debugowania T-SQL obiektu bazy danych, zlokalizuj obiektu za pomocą Eksploratora serwera, a następnie kliknij prawym przyciskiem myszy na nim i wybierz polecenie Step Into. Uruchomienie debugera i zatrzymuje na pierwszą instrukcją w obiekcie bazy danych, w takim przypadku można kroków opisanych w instrukcji s obiektów i widoku i modyfikować wartości parametrów. W kroku 1 użyliśmy tej metody, aby wkraczać do `Products_SelectByCategoryID` procedury składowanej.

Debugowanie aplikacji umożliwia punktów przerwania, aby ustawić bezpośrednio w obiektach bazy danych. Po wywołaniu obiektu bazy danych z punktów przerwania z aplikacji klienta (np. aplikacji sieci web ASP.NET), program zostaje zatrzymana, ponieważ debuger przejmuje. Debugowanie aplikacji jest przydatne, ponieważ więcej wyraźnie pokazuje, jakie działania aplikacji powoduje, że obiekt określoną bazę danych do wywołania. Jednak wymaga nieco większej konfiguracji i instalacji niż bezpośrednie debugowania bazy danych.

Obiekty bazy danych również można debugować za pośrednictwem projektów serwera SQL. Firma Microsoft będzie wyglądać na przy użyciu programu SQL Server — projekty i jak ich używać do tworzenia i debugowania zarządzane obiekty bazy danych w następnym samouczku.

Programowanie przyjemność!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Poprzednie](protecting-connection-strings-and-other-configuration-information-vb.md)
> [dalej](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb.md)
