---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-the-production-web-application-to-use-the-production-database-cs
title: Konfigurowanie produkcyjnej aplikacji internetowej do użycia z produkcyjnej bazy danych (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Zgodnie z opisem w samouczkach wcześniej, nie jest niczym niezwykłym, aby uzyskać informacje o konfiguracji mogą się różnić między środowiskami deweloperskim i produkcyjnym. Jest to es...
ms.author: riande
ms.date: 04/23/2009
ms.assetid: 0177dabd-d888-449f-91b2-24190cf5e842
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-the-production-web-application-to-use-the-production-database-cs
msc.type: authoredcontent
ms.openlocfilehash: e2560ae7a845c27f7fa5f420eaf1d89618397c60
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754063"
---
<a name="configuring-the-production-web-application-to-use-the-production-database-c"></a>Konfigurowanie produkcyjnej aplikacji internetowej do użycia z produkcyjnej bazy danych (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz program Code](http://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_08_CS.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial08_DBConfig_cs.pdf)

> Zgodnie z opisem w samouczkach wcześniej, nie jest niczym niezwykłym, aby uzyskać informacje o konfiguracji mogą się różnić między środowiskami deweloperskim i produkcyjnym. Jest to szczególnie istotne w przypadku aplikacji sieci web opartej na danych, jak parametry połączenia bazy danych różnią się między środowiskami deweloperskim i produkcyjnym. W tym samouczku przedstawiono sposoby konfigurowania środowiska produkcyjnego, aby uwzględnić parametry połączenia odpowiednie bardziej szczegółowo.


## <a name="introduction"></a>Wprowadzenie

Aplikacje sieci web opartej na danych zazwyczaj używane w innej bazy danych w przypadku tworzenia niż w środowisku produkcyjnym. W przypadku aplikacji hostowanych przez dostawcę hosta sieci web i tworzone lokalnie rozwoju bazy danych zazwyczaj znajduje się na komputerze dewelopera podczas produkcyjnej bazy danych jest hostowana na serwerze bazy danych w funkcji s firmy hostingu w sieci web. Wdrażanie aplikacji sieci web opartej na danych pociąga za sobą kopiowanie rozwoju bazy danych na serwerze bazy danych w środowisku produkcyjnym. W poprzednim samouczku przyjrzeliśmy się sposoby na osiągnięcie tego kroku.

Aplikacja internetowa używa informacji w *parametry połączenia* do nawiązania połączenia z bazą danych. Parametry połączenia, które są zwykle przechowywane w `Web.config`, określa nazwę serwera bazy danych, nazwę bazy danych, w kontekście zabezpieczeń i inne informacje. Ponieważ bazy danych używanej przez aplikację sieci web, zależy od tego, czy aplikacja sieci web jest uruchomiona w środowiskach programowania lub produkcji, parametry połączenia muszą różnić się między dwoma środowiskami.

Nie jest niczym niezwykłym, aby uzyskać informacje o konfiguracji mogą się różnić między środowiskami deweloperskim i produkcyjnym. *Typowych konfiguracji różnice między deweloperskim i produkcyjnym* samouczku omówiono technik do przechowywania informacji o konfiguracji między tych dwóch środowisk, a także krótkie omówienie na Parametry połączenia bazy danych. W tym samouczku przedstawiono sposoby konfigurowania środowiska produkcyjnego, aby uwzględnić parametry połączenia odpowiednie bardziej szczegółowo.

## <a name="examining-the-connection-string-information"></a>Badanie informacji o parametrach połączenia

Parametry połączenia używane przez aplikację sieci web przeglądy książki jest przechowywany w pliku konfiguracyjnym aplikacji s `Web.config`. `Web.config` obejmuje specjalnej sekcji do przechowywania parametrów połączenia, aptly o nazwie [ &lt;connectionStrings&gt;](https://msdn.microsoft.com/library/bf7sd233.aspx). `Web.config` Plik przeglądy książki witryny sieci Web ma jeden ciąg połączenia zdefiniowane w tej sekcji o nazwie `ReviewsConnectionString`:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-cs/samples/sample1.xml)]

Parametry połączenia — źródło danych =. \SQLEXPRESS; AttachDbFilename = | DataDirectory|\Reviews.mdf;Integrated Security = True; Wystąpienie użytkownika = True — składa się z liczbą opcji i wartości, przy użyciu opcji/pary wartości rozdzielane znakami średnika i każdej opcji i wartości rozdzielane znakiem równości. Są cztery opcje używane w tym ciągu połączenia:

- `Data Source` -Określa lokalizację serwera bazy danych i nazwę wystąpienia serwera bazy danych (jeśli istnieje). Jako wartość `.\SQLEXPRESS`, znajduje się przykład w przypadku, gdy istnieje serwer bazy danych i nazwę wystąpienia. Okres określa, że serwer bazy danych na tym samym komputerze jako aplikacji; Nazwa wystąpienia jest `SQLEXPRESS`.
- `AttachDbFilename` -Określa lokalizację pliku bazy danych. Wartość zawiera symbol zastępczy `|DataDirectory|`, który został rozwiązany na pełną ścieżkę s aplikacji `App_Data` folderu w czasie wykonywania.
- `Integrated Security` — wartość logiczna, która wskazuje, czy ma być używane określone nazwy użytkownika/hasło podczas nawiązywania połączenia z bazą danych (FAŁSZ) lub Windows bieżącego poświadczeń konta (PRAWDA).
- `User Instance` -opcji konfiguracji specyficznych dla wersji programu SQL Server Express Edition, która wskazuje, czy zezwalać na dołączanie użytkowników innych niż administracyjne na komputerze lokalnym i połączenie z bazą danych programu SQL Server Express Edition. Zobacz [wystąpienia programu SQL Server Express użytkownika](https://msdn.microsoft.com/library/ms254504.aspx) Aby uzyskać więcej informacji na temat tego ustawienia.
  

Opcje parametrów połączenia dopuszczalny rozmiar, zależą od bazy danych, z którą jest nawiązywane i dostawcy bazy danych ADO.NET, które są używane. Na przykład parametry połączenia do łączenia się z programu Microsoft SQL Server bazy danych różni się od, używany do łączenia z bazą danych Oracle. Podobnie nawiązywania połączenia z bazą danych programu Microsoft SQL Server, używając dostawcy SqlClient używa parametrów połączenia innego niż przy użyciu dostawcy OLE DB.

Parametry połączenia bazy danych można tworzyć ręcznie za pomocą witryny, takie jak [ConnectionStrings.com](http://www.connectionstrings.com/) jako zasób, jakie opcje są dostępne. Łatwiejsze podejście jest jednak dodać bazy danych do Eksploratora serwera w programie Visual Studio a następnie Pobierz parametry połączenia w oknie właściwości. Pozwól, s, użyj tej techniki te ostatnie tworzenia ciągu połączenia z serwerem bazy danych w środowisku produkcyjnym.

Otwórz program Visual Studio, a następnie przejdź do okna Eksplorator serwera (w Visual Web Developer, w tym oknie jest nazywana Eksplorator bazy danych). Kliknij prawym przyciskiem myszy opcję połączenia danych, a następnie wybierz opcję Dodaj połączenie z poziomu menu kontekstowego. Otwarte kreatora przedstawionej na rysunku 1. Wybierz odpowiednie źródło danych, a następnie kliknij przycisk Kontynuuj.


[![Wybierz dodać nową bazę danych do Eksploratora serwera](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image2.jpg)](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image1.jpg) 

**Rysunek 1**: Wybierz dodać nową bazę danych do Eksploratora serwera ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image3.jpg))


Następnie określ różne informacje o połączeniu w bazie danych (zobacz rysunek 2). Po zalogowaniu przy użyciu usługi sieci web hostingu firmy, w której należy podać informacje na temat połączenia z bazą danych — nazwa serwera bazy danych, nazwę bazy danych, nazwę użytkownika i hasło używane do łączenia z bazą danych i tak dalej. Po wprowadzeniu tych informacji kliknij przycisk OK, aby zakończyć działanie kreatora i dodawanie bazy danych do Eksploratora serwera.


[![Określ informacje dotyczące połączenia bazy danych](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image5.jpg)](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image4.jpg) 

**Rysunek 2**: Określ informacje dotyczące połączenia bazy danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image6.jpg))


Bazy danych środowiska produkcyjnego powinny teraz wyświetlany w Eksploratorze serwera. Wybierz bazę danych z poziomu Eksploratora serwera, a następnie przejdź do okna właściwości. Można znaleźć właściwości o nazwie parametrów połączenia parametrami połączenia s bazy danych. Przy założeniu, że używasz bazy danych programu Microsoft SQL Server w środowisku produkcyjnym i dostawcy SqlClient parametrów połączenia powinien wyglądać podobnie do następującego:

<strong>Źródło danych =<em>serverName</em>; Początkowy katalog =<em>databaseName</em>; Utrwal informacje zabezpieczające = True; Nazwa użytkownika =<em>username</em>; Hasło =*hasła</strong>*

Gdzie *serverName*, *databaseName*, *username*, i *hasło* są wartościami nazwy serwera bazy danych, bazy danych Nazwa i nazwę użytkownika i hasło, dostarczonych przez firmę hosta sieci web.

## <a name="deploying-the-book-reviews-web-application"></a>Wdrażanie aplikacji sieci Web przeglądy książki

Poprzedni Samouczek schodkowego przez kopiowanie rozwoju bazy danych do środowiska produkcyjnego, ale nie Eksplorowanie, wdrażanie aplikacji opartych na danych. W tym momencie w środowisku produkcyjnym znajduje się baza danych, ale przy użyciu wersji aplikacji przeglądy książki przeglądy statyczne. Należy wdrożyć aplikację nowych, opartych na danych na serwerze produkcyjnym, wraz z informacjami zaktualizowanej konfiguracji.

Poświęć chwilę na wdrażanie aplikacji opartych na danych ze środowiska projektowego do środowiska produkcyjnego. Ten proces została omówiona szczegółowo w poprzednich samouczkach. Jeśli potrzebujesz przypomnienia informacji, zobacz *Wdrażanie witryny sieci Web przy użyciu klienta FTP* lub *wdrażanie Twojej witryny sieci Web przy użyciu programu Visual Studio* samouczków. Należy upewnić się, że parametry połączenia bazy danych w środowisku produkcyjnym jest używane w środowisku produkcyjnym, co oznacza, że alternatywne `Web.config` musi zostać wdrożony plik. W szczególności zmodyfikowane `Web.config` pliku s `<connectionStrings>` element musi zawierać parametry połączenia bazy danych w środowisku produkcyjnym i powinien wyglądać podobnie do poniższego:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-cs/samples/sample2.xml)]

Należy zauważyć, że parametrów połączenia w `<connectionStrings>` element samej nazwie (`ReviewsConnectionString`), ale teraz zawiera parametry połączenia bazy danych produkcyjnych zamiast parametrów połączenia bazy danych rozwoju.

Jeśli nie masz więcej formalny przepływ pracy wdrożenia, albo ręcznie zmodyfikować `Web.config` pliku, aby użyć parametrów połączenia bazy danych przed wdrożeniem (pamiętaniu przywrócić go przy użyciu parametrów połączenia bazy danych rozwoju później) lub utrzymywać osobnej `Web.config` pliku informacji konfiguracyjnych środowiska produkcyjnego, który pobiera przekazany do środowiska produkcyjnego w ramach procesu wdrażania.

> [!NOTE]
> Jeśli przypadkowo wdrożono `Web.config` pliku zawierającego parametry połączenia bazy danych rozwoju będą dostępne wystąpił błąd podczas próby połączenia z bazą danych aplikacji w środowisku produkcyjnym. Ten błąd manifesty jako `SqlException` komunikatem raportowania, że serwer nie został znaleziony lub jest on niedostępny.


Gdy witryna została wdrożona do produkcji, odwiedź witryny produkcyjnej za pośrednictwem przeglądarki. Należy wyświetlić i Ciesz się tego samego środowiska użytkownika, co po lokalnym uruchomieniu aplikacji opartych na danych. Oczywiście gdy użytkownik odwiedza witryny sieci Web w środowisku produkcyjnym ta witryna jest obsługiwana przez serwer bazy danych produkcyjnych, należy odwiedzić witrynę sieci Web w środowisku programistycznym korzysta z bazy danych w trakcie opracowywania. Rysunek 3 przedstawia *uczyć się ASP.NET 3.5 w ciągu 24 godzin* Przejrzyj stronę z witryny sieci Web w środowisku produkcyjnym (Uwaga: adres URL w pasku adresu przeglądarki s).


[![Aplikacja ze jest teraz dostępna w środowisku produkcyjnym.](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image8.jpg)](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image7.jpg) 

**Rysunek 3**: The Data-Driven aplikacja jest teraz dostępna w środowisku produkcyjnym! ([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image9.jpg))


### <a name="storing-connection-strings-in-a-separate-configuration-file"></a>Przechowywanie parametrów połączenia w oddzielnych pliku konfiguracji

Stosuje się technikę do przechowywania informacji o konfiguracji oddzielnych środowisk deweloperskich i produkcyjnych na dwie wersje `Web.config`: jeden dla środowiska programistycznego i jeden dla środowiska produkcyjnego. W czasie wdrażania odpowiednie `Web.config` wersji mogą być kopiowane do środowiska produkcyjnego. W idealnym przypadku ten proces spowoduje automatyczne jako część przepływu pracy wdrażania.

Zamiast zajmować się utrzymywaniem dwa oddzielne `Web.config` pliki, które użytkownik może opcjonalnie zapewniają bardziej szczegółową różnice. Elementy, które tworzą `Web.config` plików można zdefiniować w plikach konfiguracyjnych zewnętrznych, które są następnie używane w `Web.config` pliku. Mówiąc mają `Web.config` pliku obydwu środowisk, które odwołuje się do pliku databaseConnectionStrings.config i będzie zawierać parametry połączenia używane przez aplikację i będzie unikatowy dla każdego środowiska. Mogę się dowiedzieć, oddzielając różne informacje o konfiguracji w osobnych plikach udostępnia pisma odręcznego i prostsze `Web.config` pliku i nie tylko wyraźnie opisano różnice konfiguracji między środowiskami deweloperskim i produkcyjnym.

Aby użyć tej metody, należy uruchomić, tworząc nowy folder w aplikacji sieci web o nazwie `ConfigSections`. Następnie dodaj dwa pliki, aby ten nowy folder o nazwie databaseConnectionStrings.dev.config i databaseConnectionStrings.production.config. Następnie skopiuj `<connectionStrings>` elementu z `Web.config` databaseConnectionStrings.dev.config i databaseConnectionStrings.production.config plików, a następnie zmodyfikuj parametry połączenia w databaseConnectionStrings.production.config pliku, tak aby określa parametry połączenia bazy danych w środowisku produkcyjnym. Na przykład plik databaseConnectionStrings.dev.config powinien zawierać tylko `<connectionStrings>` elementu przy użyciu parametrów połączenia, który odwołuje się do rozwoju bazy danych:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-cs/samples/sample3.xml)]

Podobnie, plik databaseConnectionStrings.production.config powinien zawierać tylko `<connectionStrings>` element, ale taki, który ma parametry połączenia bazy danych w środowisku produkcyjnym.

Utwórz kopię pliku databaseConnectionStrings.dev.config i nadaj mu nazwę databaseConnectionStrings.config.

> [!NOTE]
> Możesz nazwać plik konfiguracyjny coś innego niż databaseConnectionStrings.config, opcjonalnie d, takich jak `connectionStrings.config` lub `dbInfo.config`. Jednak pamiętaj nazwać plik za pomocą `.config` rozszerzenie jako `.config` pliki domyślnie nie obsługiwanych przez aparat programu ASP.NET. Jeśli nazwa pliku, coś innego, takich jak `connectionStrings.txt`, użytkownik może wskazywać przeglądarki do [www.yoursite.com/ConfigSettings/connectionStrings.txt](http://www.yoursite.com/ConfigSettings/connectionStrings.txt) i wyświetlić zawartość pliku!


W tym momencie `ConfigSections` folder powinien zawierać trzy pliki (zobacz rysunek 4). Pliki databaseConnectionStrings.dev.config i databaseConnectionStrings.production.config zawierają parametry połączenia dla środowisk deweloperskich i produkcyjnych, odpowiednio. Plik databaseConnectionStrings.config zawiera informacje o parametrach połączenia, który będzie używany przez aplikację sieci web w czasie wykonywania. W związku z tym plik databaseConnectionStrings.config powinny być identyczne z pliku databaseConnectionStrings.dev.config w środowisku programistycznym do produkcji pliku databaseConnectionStrings.config powinna być taka sama jak databaseConnectionStrings.production.config.


[![ConfigSections](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image11.jpg)](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image10.jpg) 

**Rysunek 4**: ConfigSections ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image12.jpg))


Teraz musisz poinstruować `Web.config` na potrzeby pliku databaseConnectionStrings.config jego magazynu ciągów połączenia. Otwórz `Web.config` , zastępując istniejącą `<connectionStrings>` element następującym kodem:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-cs/samples/sample4.xml)]

`configSource` Atrybut określa ścieżkę fizyczną względem `Web.config` pliku. Jeżeli zewnętrzne `.config` plik znajduje się w tym samym katalogu co `Web.config` następnie ustaw ten atrybut na nazwę pliku `.config` pliku. Jeśli go s w podkatalogu, w przypadku databaseConnectionStrings.config, określ podfolderu do rozdzielenia nazwy plików i folderów, takie jak ConfigSections\databaseConnectionStrings.config znakiem ukośnika odwrotnego.

Ta modyfikacja środowisk deweloperskich i produkcyjnych zawiera takie same `Web.config` pliku. Obecnie jedyną różnicą jest to plik databaseConnectionStrings.config. Skopiuj plik databaseConnectionStrings.production.config do środowiska produkcyjnego i zmień jego nazwę na databaseConnectionStrings.config. Jeśli w przyszłości, istnieją zmiany parametrów połączenia bazy danych należy wprowadzić je w pliku databaseConnectionStrings.production.config, a następnie przekaż ten plik do środowiska produkcyjnego, zmiana jego nazwy databaseConnectionStrings.config.

> [!NOTE]
> Można określić informacje dla każdego `Web.config` element w osobnym pliku i użyj `configSource` atrybut odwołanie do tego pliku z poziomu `Web.config`.


## <a name="summary"></a>Podsumowanie

Aplikacje oparte na danych zazwyczaj używać różnych baz danych w środowiskach deweloperskich i produkcyjnych. W związku z tym parametry połączenia bazy danych, które są przechowywane w konfiguracji s aplikacji sieci web musi być unikatowa dla środowiska. W tym samouczku zobaczyliśmy, jak określić parametry połączenia bazy danych w środowisku produkcyjnym i sposoby, aby zachować informacje o parametrach połączenia unikatowy w dwóch środowiskach.

Wszystkiego najlepszego programowania!

#### <a name="further-reading"></a>Dalsze informacje

Więcej informacji na tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Parametry połączenia i pliki konfiguracji](https://msdn.microsoft.com/library/ms254494.aspx)
- [Informacje o @ ConnectionStrings.com ciągi, konfiguracja bazy danych](http://www.connectionstrings.com/)
- [Przenoszenie ustawień z pliku Web.config](http://www.asp101.com/tips/index.asp?id=154)
- [Dokumentacja techniczna dotycząca &lt;connectionStrings&gt; — Element](https://msdn.microsoft.com/library/bf7sd233.aspx)

> [!div class="step-by-step"]
> [Poprzednie](deploying-a-database-cs.md)
> [dalej](configuring-a-website-that-uses-application-services-cs.md)
