---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/debugging-stored-procedures-cs
title: Debugowanie procedur składowanych (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Wersje programu Visual Studio Professional i Team System umożliwiają ustawianie punktów przerwania i kroku w celu procedur składowanych w programie SQL Server, co znacznie przyspiesza debugowanie przechowywane...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/03/2007
ms.topic: article
ms.assetid: c655c324-2ffa-4c21-8265-a254d79a693d
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/debugging-stored-procedures-cs
msc.type: authoredcontent
ms.openlocfilehash: 988de3cfea16eb623351288ab103e77230625a17
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37380736"
---
<a name="debugging-stored-procedures-c"></a>Debugowanie procedur składowanych (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz program Code](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_74_CS.zip) lub [Pobierz plik PDF](debugging-stored-procedures-cs/_static/datatutorial74cs1.pdf)

> Wersje programu Visual Studio Professional i Team System umożliwiają ustawianie punktów przerwania i kroku w celu procedur składowanych w programie SQL Server, co znacznie przyspiesza debugowanie procedur składowanych, które są równie proste jak debugowanie kodu aplikacji. Ten samouczek przedstawia bezpośredniego debugowania bazy danych i aplikacji debugowanie procedur składowanych.


## <a name="introduction"></a>Wprowadzenie

Visual Studio zapewnia zaawansowane środowisko debugowania. Za pomocą kilku naciśnięć klawiszy lub kliknięć myszą jego s możliwe używanie punktów przerwania, aby zatrzymać wykonywanie program i sprawdź jej stan i kontroli przepływu. Wraz z debugowania kodu aplikacji, program Visual Studio oferuje obsługę debugowanie procedur składowanych w programie SQL Server. Tak samo, jak można ustawić punktów przerwania w kodzie ASP.NET osobna klasa kodu lub klasy warstwy logiki biznesowej, aby za ich można umieścić w ramach procedur składowanych.

W tym samouczku przyjrzymy się Przechodzenie do procedur składowanych z poziomu Eksploratora serwera w programie Visual Studio oraz jak można ustawić punktów przerwania, które są osiągane, gdy procedura składowana jest wywoływana z działającej aplikacji platformy ASP.NET.

> [!NOTE]
> Niestety procedury składowane może być wywołanie i tylko debugowania przez wersje Professional i systemy zespołu programu Visual Studio. Jeśli używasz standardowego wersji programu Visual Studio lub Visual Web Developer, zachęcamy do odczytu wzdłuż jako części omówimy kroki niezbędne do debugowanie procedur składowanych, ale nie można replikować te kroki na komputerze.


## <a name="sql-server-debugging-concepts"></a>Pojęcia debugowanie serwera SQL

Microsoft SQL Server 2005 zaprojektowano tak, aby zapewnić integrację z usługą [środowiska uruchomieniowego języka wspólnego (CLR)](https://msdn.microsoft.com/netframework/aa497266.aspx), czyli środowiska uruchomieniowego użytego przez wszystkie zestawy .NET. W związku z tym SQL Server 2005 obsługuje obiektami zarządzanej bazy danych. Oznacza to można utworzyć obiektów bazy danych, takich jak procedury składowane i funkcje zdefiniowane przez użytkownika (UDF) jako metody w klasie języka C#. Dzięki temu tych procedur przechowywanych i funkcji zdefiniowanych przez użytkownika, korzystanie z funkcji w programie .NET Framework i z własnych niestandardowych klas. Oczywiście SQL Server 2005 także zapewnia obsługę języka T-SQL, obiektów bazy danych.

SQL Server 2005 oferuje obsługę debugowania dla języka T-SQL i obiektami zarządzanej bazy danych. Jednak te obiekty mogą być debugowane za pomocą wersji programu Visual Studio 2005 Professional i systemy zespołu. W tym samouczku będziemy sprawdzać debugowania obiekty bazy danych języka T-SQL. Kolejnym samouczku patrzy na debugowanie obiektami zarządzanej bazy danych.

[Omówienie języka T-SQL i CLR debugowania w programie SQL Server 2005](https://blogs.msdn.com/sqlclr/archive/2006/06/29/651644.aspx) wpis w blogu z [integrację środowiska CLR programu SQL Server 2005 team](https://blogs.msdn.com/sqlclr/default.aspx) wyróżnia trzy sposoby debugowania programu SQL Server 2005 obiektów z programu Visual Studio:

- **Bezpośrednie bazy danych debugowania (DDD)** — z poziomu Eksploratora serwera, możemy przejść do obiektu do bazy danych języka T-SQL, takie jak procedur przechowywanych i funkcji zdefiniowanych przez użytkownika. Będziemy sprawdzać DDD w kroku 1.
- **Debugowanie aplikacji** — firma Microsoft może ustawiać punkty przerwania w obrębie obiektu bazy danych, a następnie uruchom naszej aplikacji ASP.NET. Kiedy obiekt bazy danych jest wykonywana, punkt przerwania zostanie osiągnięty, a kontroli do debugera. Należy pamiętać, że w debugowaniu aplikacji firma Microsoft nie można wykonać kroku do obiektu bazy danych w kodzie aplikacji. Firma Microsoft musi jawnie ustawić punkty przerwania w tych procedurach składowanych lub UDF której chcemy, aby debuger zatrzymuje. Debugowanie aplikacji jest badany w kroku 2.
- **Debugowanie z projektu serwera SQL** — wersje programu Visual Studio Professional i systemy zespołu zawierać typ projekt programu SQL Server, który jest najczęściej używany do tworzenia obiektów zarządzanej bazy danych. Będziemy sprawdzać za pomocą programu SQL Server projektów i debugowanie ich zawartość w następnym samouczku.

Visual Studio umożliwia debugowanie procedur składowanych na lokalnego i zdalnego wystąpienia programu SQL Server. Lokalne wystąpienie programu SQL Server jest taki, który jest zainstalowany na tym samym komputerze co program Visual Studio. Jeśli bazy danych programu SQL Server, którego używasz, nie znajduje się na komputerze deweloperskim, następnie uważa się zdalne wystąpienie. Potrzeby tych samouczków firma Microsoft masz doświadczenie z lokalnego wystąpienia programu SQL Server. Debugowanie procedur składowanych na zdalnym wystąpieniu programu SQL server wymaga dodatkowych czynności konfiguracyjnych, niż gdy debugowanie procedur składowanych w lokalnym wystąpieniu.

Jeśli używasz lokalnego wystąpienia programu SQL Server, możesz rozpocząć od kroku 1 i działać na końcu tego samouczka. Jeśli używasz zdalnego wystąpienia programu SQL Server, należy najpierw trzeba upewnij się, że podczas debugowania można są rejestrowane na komputerze deweloperskim przy użyciu konta użytkownika Windows, który ma identyfikator logowania programu SQL Server w zdalnym wystąpieniu. Moveover, zarówno tych danych logowania w bazie danych, jak i nazwy logowania bazy danych służący do nawiązywania połączenia z bazą danych działającej aplikacji platformy ASP.NET muszą być elementami członkowskimi `sysadmin` roli. Na końcu tego samouczka, aby uzyskać więcej informacji na temat konfigurowania programu Visual Studio i programu SQL Server do debugowania zdalnego wystąpienia, zobacz obiekty bazy danych debugowania języka T-SQL w sekcji wystąpienia zdalnego.

Ponadto Dowiedz się, że obsługę debugowania dla języka T-SQL, obiektów bazy danych nie jest jako funkcja sformatowany jako obsługę debugowania dla aplikacji platformy .NET. Na przykład warunków i punktu przerwania filtry nie są obsługiwane, tylko podzbiór debugowania systemu windows są dostępne, nie można użyć, Edytuj i Kontynuuj, bezpośrednim staje się bezużyteczny i tak dalej. Zobacz [ograniczenia poleceń debugera i funkcji](https://msdn.microsoft.com/library/ms165035(VS.80).aspx) Aby uzyskać więcej informacji.

## <a name="step-1-directly-stepping-into-a-stored-procedure"></a>Krok 1: Bezpośrednio przechodzenie krok po kroku do procedury składowanej

Program Visual Studio ułatwia bezpośrednio debugować obiektu bazy danych. Pozwól s, zobacz, jak używać funkcji bezpośredniego debugowania bazy danych (DDD) Aby wkraczać do `Products_SelectByCategoryID` procedurę składowaną w bazie danych Northwind. Jak sugeruje jej nazwa, `Products_SelectByCategoryID` zwraca informacje o produkcie dla określonej kategorii; został utworzony w [za pomocą istniejących procedur składowanych dla s wpisany zestaw danych TableAdapters](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) samouczka. Zacznij od przejścia do Eksploratora serwera, a następnie rozwiń węzeł bazy danych Northwind. Następnie należy przejść do folderu procedur składowanych, kliknij prawym przyciskiem myszy `Products_SelectByCategoryID` procedury składowanej, a następnie wybierz opcję krok do procedury składowanej z menu kontekstowego. Spowoduje to uruchomienie debugera.

Ponieważ `Products_SelectByCategoryID` oczekuje procedury składowanej `@CategoryID` parametr wejściowy, firma Microsoft jest proszony o podanie tej wartości. Wprowadź wartość 1, które zostaną zwrócone informacje na temat beverages.


![Użyj wartości 1 dla @CategoryID parametru](debugging-stored-procedures-cs/_static/image1.png)

**Rysunek 1**: Użyj wartości 1 dla `@CategoryID` parametru


Po dostarczeniu wartość `@CategoryID` parametru procedury składowanej jest wykonywany. Zamiast uruchamiania do zakończenia, jednak debuger przerywa wykonywanie w pierwszej instrukcji. Należy pamiętać, żółta strzałka na marginesie, wskazujący bieżącą lokalizację w procedurze składowanej. Można wyświetlać i edytować wartości parametrów za pomocą okna czujki lub, ustawiając kursor nad nazwę parametru w procedurze składowanej.


[![Debuger został zatrzymany w pierwszej instrukcji procedury składowanej](debugging-stored-procedures-cs/_static/image3.png)](debugging-stored-procedures-cs/_static/image2.png)

**Rysunek 2**: debuger został zatrzymany w pierwszej instrukcji procedury składowanej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](debugging-stored-procedures-cs/_static/image4.png))


Do wykonania kroków procedury składowanej, jednej instrukcji w danym momencie, kliknij przycisk Przejdź na pasku narzędzi lub naciśnij klawisz F10. `Products_SelectByCategoryID` Procedury składowanej zawiera pojedynczy `SELECT` instrukcji, więc osiągnięcia F10 przechodzi przez pojedynczą instrukcję i zakończyć wykonywania procedury składowanej. Po zakończeniu procedury składowanej jego dane wyjściowe będą wyświetlane w oknie danych wyjściowych i debuger spowoduje przerwanie działania.

> [!NOTE]
> Debugowanie języka T-SQL odbywa się na poziomie poufności; nie możesz wejść w `SELECT` instrukcji.


## <a name="step-2-configuring-the-website-for-application-debugging"></a>Krok 2. Konfigurowanie witryny sieci Web do debugowania aplikacji

Podczas debugowania procedury składowanej bezpośrednio z poziomu Eksploratora serwera jest przydatne, w wielu scenariuszach Dbamy bardziej debugowania procedury składowanej, gdy jest wywoływana z naszej aplikacji ASP.NET. Możemy dodać punkty przerwania do procedury składowanej z poziomu programu Visual Studio, a następnie rozpocząć debugowanie aplikacji ASP.NET. Po wywołaniu procedury składowanej z punktami przerwania z aplikacji, wykonanie zostanie zatrzymany w punkcie przerwania, a firma Microsoft wyświetlenie i zmianę wartości parametrów procedury składowanej s i przejść przez jej instrukcji, tak samo, jak zrobiliśmy w kroku 1.

Zanim firma Microsoft będzie mogła rozpocząć debugowanie procedur składowanych wywoływać z aplikacji, należy wydać polecenie aplikacji sieci web ASP.NET w celu zintegrowania za pomocą debugera programu SQL Server. Rozpocznij, klikając prawym przyciskiem myszy nazwę witryny sieci Web w Eksploratorze rozwiązań (`ASPNET_Data_Tutorial_74_CS`). Wybierz opcję strony właściwości, z menu kontekstowego, wybierz element Opcje uruchamiania po lewej stronie i zaznacz pole wyboru programu SQL Server, w sekcji debugery (zobacz rysunek 3).


[![Zaznacz pole wyboru serwera SQL na stronach właściwości s aplikacji](debugging-stored-procedures-cs/_static/image6.png)](debugging-stored-procedures-cs/_static/image5.png)

**Rysunek 3**: zaznacz pole wyboru serwera SQL w aplikacji s strony właściwości ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](debugging-stored-procedures-cs/_static/image7.png))


Ponadto należy zaktualizować parametry połączenia bazy danych, które są używane przez aplikację tak, aby buforowanie połączeń jest wyłączona. Gdy połączenie z bazą danych jest zamknięty, odpowiedni `SqlConnection` obiekt jest umieszczany w puli dostępnych połączeń. Podczas nawiązywania połączenia z bazą danych, obiektu dostępnych połączeń mogą być pobierane z tej puli zamiast konieczności tworzenia i nawiązać nowe połączenie. Tej buforowanie obiektów połączeń jest zwiększenie wydajności i jest domyślnie włączona. Jednak podczas debugowania chcemy wyłączyć buforowanie połączeń, ponieważ infrastruktury debugowania nie poprawnie ustanowieniu podczas pracy z połączeniem, która została wykonana z puli.

Aby pula połączeń wyłączone, należy zaktualizować `NORTHWNDConnectionString` w `Web.config` tak, że zawiera on ustawienia `Pooling=false` .


[!code-xml[Main](debugging-stored-procedures-cs/samples/sample1.xml)]

> [!NOTE]
> Po zakończeniu debugowania programu SQL Server za pośrednictwem aplikacji ASP.NET upewnij się przywrócić buforowanie połączeń, usuwając `Pooling` ustawienie z parametrów połączenia (lub przez ustawienie `Pooling=true` ).


W tym momencie aplikacji ASP.NET skonfigurowano programowi Visual Studio do debugowania obiekty bazy danych programu SQL Server, gdy wywoływane za pośrednictwem aplikacji sieci web. Nadal będzie teraz wystarczy dodać punkt przerwania do procedury składowanej i Rozpocznij debugowanie!

## <a name="step-3-adding-a-breakpoint-and-debugging"></a>Krok 3: Dodawanie punktu przerwania i debugowania

Otwórz `Products_SelectByCategoryID` procedury składowanej i ustaw punkt przerwania na początku `SELECT` instrukcji, klikając na marginesie w odpowiednim miejscu lub umieszczając kursor na początku `SELECT` instrukcji i naciskając klawisz F9. Rysunek 4 przedstawia, punkt przerwania jest pokazywana jako czerwone kółko na marginesie.


[![Ustaw punkt przerwania w Products_SelectByCategoryID procedury składowanej](debugging-stored-procedures-cs/_static/image9.png)](debugging-stored-procedures-cs/_static/image8.png)

**Rysunek 4**: Ustaw punkt przerwania `Products_SelectByCategoryID` Stored Procedure ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](debugging-stored-procedures-cs/_static/image10.png))


Aby dla obiektu bazy danych SQL do debugowania za pomocą aplikacji klienckiej konieczne jest skonfigurowania bazy danych do obsługi debugowania aplikacji. Jeśli najpierw ustaw punkt przerwania, to ustawienie powinno automatycznie być włączone, ale zaleca się dokładnie. Kliknij prawym przyciskiem myszy `NORTHWND.MDF` węzła w Eksploratorze serwera. Menu kontekstowe powinno obejmować zaznaczony element menu o nazwie debugowania aplikacji.


![Upewnij się, że włączono opcję debugowania aplikacji](debugging-stored-procedures-cs/_static/image11.png)

**Rysunek 5**: Upewnij się, że włączono opcję debugowania aplikacji


Ustaw punkt przerwania i włączoną opcją debugowanie aplikacji możemy przystąpić do debugowania procedury składowanej, gdy zostanie wywołana z poziomu aplikacji ASP.NET. Uruchom debuger, przechodząc do menu Debuguj i wybierając Rozpocznij debugowanie przez naciskać klawisz F5 lub klikając na zielony wskaźnik odtwarzania ikonę na pasku narzędzi. Spowoduje to uruchomienia debugera i uruchomienie witryny sieci Web.

`Products_SelectByCategoryID` Procedura składowana została utworzona w [za pomocą istniejących procedur składowanych dla s wpisany zestaw danych TableAdapters](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) samouczka. Jego odpowiadającą mu stronę sieci web (`~/AdvancedDAL/ExistingSprocs.aspx`) zawiera GridView wyświetlający wyników zwróconych przez tę procedurę składowaną. Odwiedź tę stronę za pośrednictwem przeglądarki. Gdy wykorzystasz na stronie punkt przerwania w `Products_SelectByCategoryID` procedury składowanej spowoduje osiągnięcie i kontroli zwrócił do programu Visual Studio. Podobnie jak w kroku 1, można przejść przez procedurę składowaną s instrukcji i widoku i modyfikowanie wartości parametrów.


[![Na stronie ExistingSprocs.aspx początkowo wyświetlane są Beverages](debugging-stored-procedures-cs/_static/image13.png)](debugging-stored-procedures-cs/_static/image12.png)

**Rysunek 6**: `ExistingSprocs.aspx` strony wyświetli początkowo Beverages ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](debugging-stored-procedures-cs/_static/image14.png))


[![S procedury składowanej punkt przerwania zostanie osiągnięty](debugging-stored-procedures-cs/_static/image16.png)](debugging-stored-procedures-cs/_static/image15.png)

**Rysunek 7**: procedura składowana s zostanie osiągnięty punkt przerwania ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](debugging-stored-procedures-cs/_static/image17.png))


Jak okna czujki w przedstawia rysunek 7, wartość `@CategoryID` parametru to 1. Jest to spowodowane `ExistingSprocs.aspx` strony początkowo wyświetlane produkty należące do tej kategorii, która ma `CategoryID` wartość 1. Wybierz inną kategorię z listy rozwijanej. Ten sposób powoduje odświeżenie strony i ponownie uruchamia `Products_SelectByCategoryID` procedury składowanej. Punkt przerwania zostaje trafiony ponownie, ale tym razem `@CategoryID` wartość s parametru odzwierciedla element do wybranej listy rozwijanej s `CategoryID`.


[![Wybierz inną kategorię z listy rozwijanej](debugging-stored-procedures-cs/_static/image19.png)](debugging-stored-procedures-cs/_static/image18.png)

**Rysunek 8**: Wybierz inną kategorię z listy rozwijanej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](debugging-stored-procedures-cs/_static/image20.png))


[![@CategoryID Parametr odzwierciedla kategorii wybrać na stronie sieci Web](debugging-stored-procedures-cs/_static/image22.png)](debugging-stored-procedures-cs/_static/image21.png)

**Rysunek 9**: `@CategoryID` parametru odzwierciedla wybrać kategorię ze strony internetowej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](debugging-stored-procedures-cs/_static/image23.png))


> [!NOTE]
> Jeśli punkt przerwania w `Products_SelectByCategoryID` procedury składowanej nie zostanie osiągnięty, gdy użytkownik odwiedzi `ExistingSprocs.aspx` strony, upewnij się, że pole wyboru programu SQL Server został zaewidencjonowany debugery części aplikacji ASP.NET s strony właściwości, że zostało puli połączeń wyłączone i włączenie bazy danych s opcji debugowania aplikacji. Jeśli ponownie nadal występują problemy, uruchom ponownie program Visual Studio i spróbuj ponownie.


## <a name="debugging-t-sql-database-objects-on-remote-instances"></a>Obiekty bazy danych języka T-SQL w wystąpieniach zdalnego debugowania

Debugowanie obiektów bazy danych za pomocą programu Visual Studio jest dość prosta, gdy wystąpienie bazy danych programu SQL Server znajduje się na tym samym komputerze co program Visual Studio. Jednak jeśli program SQL Server i Visual Studio znajdują się na różnych maszynach niektóre dokładnej konfiguracji jest wymagane uzyskanie wszystko działa prawidłowo. Istnieją dwa podstawowych zadaniach, które napotykają firma Microsoft:

- Upewnij się, że nazwa logowania używana do łączenia z bazą danych za pomocą ADO.NET należy do `sysadmin` roli.
- Upewnij się, że konto użytkownika Windows, które są używane przez program Visual Studio na komputerze deweloperskim prawidłowe konto logowania programu SQL Server, który należy do `sysadmin` roli.

Pierwszym krokiem jest stosunkowo prosta. Najpierw należy zidentyfikować konto użytkownika używane do łączenia z bazą danych z aplikacji ASP.NET i następnie z programu SQL Server Management Studio, dodaj to konto logowania do `sysadmin` roli.

Drugie zadanie wymaga Windows konto użytkownika, którego używasz do debugowania aplikacji prawidłową nazwą logowania na zdalnej bazy danych. Jednak jest szansa, że użytkownik zalogowany na stacji roboczej za pomocą konta Windows nie jest prawidłową nazwą logowania w programie SQL Server. Zamiast dodawać konta określonego logowania do programu SQL Server, lepszym rozwiązaniem byłoby wyznaczyć niektóre konta użytkownika Windows jako konto debugowania programu SQL Server. Następnie Aby debugować obiekty bazy danych, zdalnego wystąpienia programu SQL Server, należy uruchomić programu Visual Studio przy użyciu poświadczeń konta s logowania tego Windows.

Przykład powinny pomóc w wyjaśnianiu rzeczy. Wyobraź sobie, że istnieje konto Windows o nazwie `SQLDebug` w obrębie domeny Windows. To konto będzie należy dodać do zdalnego wystąpienia programu SQL Server jako prawidłową nazwą logowania i jako członek `sysadmin` roli. Następnie do debugowania zdalnego wystąpienia programu SQL Server w programie Visual Studio, czy należy uruchomić program Visual Studio jako `SQLDebug` użytkownika. Można to zrobić, logując się z naszym stacji roboczej, logowanie w trybie `SQLDebug`, a następnie uruchamiania programu Visual Studio, ale prostszej metody będzie zalogować się do naszych stacji roboczej przy użyciu własnych poświadczeń, a następnie użyć `runas.exe` można uruchomić programu Visual Studio jako `SQLDebug` użytkownika. `runas.exe` zezwala na konkretnej aplikacji mają być wykonane w ramach podszywając innego konta użytkownika. Aby uruchomić program Visual Studio jako `SQLDebug`, można wprowadzić następującą instrukcję w wierszu polecenia:


[!code-console[Main](debugging-stored-procedures-cs/samples/sample2.cmd)]

Aby uzyskać więcej szczegółowych informacji na temat tego procesu, zobacz [William R. Vaughn](http://betav.com/BLOG/billva/) s *Hitchhiker s Przewodnik po Visual Studio i SQL Server, Wydanie siódme* także [How to: Ustawianie uprawnień programu SQL Server debugowanie](https://msdn.microsoft.com/library/w1bhybwz(VS.80).aspx).

> [!NOTE]
> Jeśli na komputerze deweloperskim działa Windows XP Service Pack 2 należy skonfigurować zapory w celu zezwolenia na debugowanie zdalne. [Jak się: Włącz debugowanie programu SQL Server 2005](https://msdn.microsoft.com/library/s0fk6z6e(VS.80).aspx) artykułu — informacje o ten proces obejmuje dwa kroki: () na maszynie hosta programu Visual Studio, należy dodać `Devenv.exe` do listy wyjątków i otwórz port TCP 135; oraz (b) na komputerze zdalnym (SQL), należy otworzyć TCP 135 portu i Dodaj `sqlservr.exe` do listy wyjątków. Jeśli zasady domeny wymaga komunikacji w sieci, można wykonać za pomocą protokołu IPSec, należy otworzyć porty UDP 4500 i protokołu UDP 500.


## <a name="summary"></a>Podsumowanie

Ponadto, aby zapewnić obsługę debugowania dla kodu aplikacji platformy .NET, Visual Studio udostępnia szereg opcji debugowania dla programu SQL Server 2005. W tym samouczku przyjrzeliśmy się dwa z tych opcji: do bezpośredniego debugowania bazy danych i debugowania aplikacji. Aby debugować bezpośrednio obiektu bazy danych języka T-SQL, zlokalizować obiektu za pomocą Eksploratora serwera, a następnie kliknij prawym przyciskiem myszy na nim, a następnie wybierz Step Into. Spowoduje to uruchomienie debugera i zatrzymuje się na pierwszej instrukcji w obiekcie bazy danych, w tym momencie można przejrzeć instrukcje s obiektów i widoku i modyfikować wartości parametrów. W kroku 1 użyliśmy tej metody, aby wkraczać do `Products_SelectByCategoryID` procedurę składowaną.

Debugowanie aplikacji umożliwia punktów przerwania, aby ustawić bezpośrednio z poziomu obiektów bazy danych. Po wywołaniu obiektu bazy danych z punktami przerwania od aplikacji klienckiej, (na przykład aplikacja sieci web ASP.NET), program zatrzymuje jako przejmuje debugera. Debugowanie aplikacji jest przydatne, ponieważ bardziej wyraźnie pokazuje, jakie działania aplikacji powoduje, że obiekt określoną bazę danych do wywołania. Jednak wymaga nieco więcej konfiguracją i instalacją, niż bezpośredniego debugowania bazy danych.

Obiekty bazy danych można także debugować za pomocą projektów programu SQL Server. Przyjrzymy się przy użyciu projektów programu SQL Server i jak ich używać do tworzenia i debugowania zarządzanych obiektów bazy danych w następnym samouczku.

Wszystkiego najlepszego programowania!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedem ASP/ASP.NET książek i założycielem [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). ADAM można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Poprzednie](protecting-connection-strings-and-other-configuration-information-cs.md)
> [dalej](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs.md)
