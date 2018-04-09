---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-the-production-web-application-to-use-the-production-database-vb
title: Konfigurowanie aplikacji sieci Web produkcji do użycia w produkcyjnej bazie danych (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Zgodnie z opisem w starszych samouczki, nie jest rzadko informacje dotyczące konfiguracji mogą się różnić między środowisk projektowania i produkcji. Jest to es...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/23/2009
ms.topic: article
ms.assetid: a64a7aa0-6608-449e-83bf-1ef8cceee504
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-the-production-web-application-to-use-the-production-database-vb
msc.type: authoredcontent
ms.openlocfilehash: b1741807fe02b4e60db7098cfd46922d3ba50ccd
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="configuring-the-production-web-application-to-use-the-production-database-vb"></a>Konfigurowanie aplikacji sieci Web produkcji do użycia w produkcyjnej bazie danych (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz kod](http://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_08_VB.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial08_DBConfig_vb.pdf)

> Zgodnie z opisem w starszych samouczki, nie jest rzadko informacje dotyczące konfiguracji mogą się różnić między środowisk projektowania i produkcji. Jest to szczególnie ważne w przypadku aplikacji opartej na danych w sieci web, jako parametry połączenia bazy danych różnią się środowisk projektowania i produkcji. W tym samouczku opisuje sposoby konfigurowania środowiska produkcyjnego, aby dołączyć odpowiednie parametry bardziej szczegółowo.


## <a name="introduction"></a>Wprowadzenie

Oparte na danych aplikacji sieci web zwykle używać innej bazy danych w przypadku rozwoju niż w środowisku produkcyjnym. W przypadku aplikacji obsługiwanych przez dostawcę hosta sieci web i opracowany lokalnie projektowej bazie danych zazwyczaj znajduje się na komputerze dewelopera s podczas produkcyjnej bazy danych jest hostowany na serwerze bazy danych w zakładzie s firmy hostingu. Wdrażanie aplikacji sieci web opartych na danych pociąga za sobą kopiowanie programowanie bazy danych na serwerze bazy danych w środowisku produkcyjnym. W poprzednich instrukcji analizujemy sposobów, aby wykonać ten krok.

Aplikacja sieci web używa informacji w *ciąg połączenia* nawiązać połączenia z bazą danych. Ciąg połączenia, który zazwyczaj znajduje się w `Web.config`, określa nazwę serwera bazy danych, nazwy bazy danych, kontekstu zabezpieczeń i inne informacje. Ponieważ bazy danych używanej przez aplikację sieci web, zależy od tego, czy aplikacja sieci web działa w środowiskach rozwoju lub produkcji, parametry połączenia musi się różnić między dwoma środowiskami.

Nie jest rzadko informacje dotyczące konfiguracji mogą się różnić między środowisk projektowania i produkcji. *Typowych konfiguracji różnice między rozwoju i produkcji* samouczku omówiono techniki do przechowywania informacji konfiguracyjnych oddzielne między tych dwóch środowisk, jak również krótkie omówienie na Parametry połączenia bazy danych. W tym samouczku opisuje sposoby konfigurowania środowiska produkcyjnego, aby dołączyć odpowiednie parametry bardziej szczegółowo.

## <a name="examining-the-connection-string-information"></a>Badanie informacji o ciągu połączenia

Ciąg połączenia używany przez aplikację sieci web przeglądami książki są przechowywane w pliku konfiguracyjnym aplikacji s `Web.config`. `Web.config` zawiera sekcja specjalne przechowywanie parametrów połączenia, aptly o nazwie [ &lt;connectionStrings&gt;](https://msdn.microsoft.com/library/bf7sd233.aspx). `Web.config` Plik przeglądami książki witryny sieci Web ma jeden ciąg połączenia zdefiniowane w tej sekcji o nazwie `ReviewsConnectionString`:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-vb/samples/sample1.xml)]

Parametry połączenia — źródło danych =. \SQLEXPRESS; AttachDbFilename = | Zabezpieczenia DataDirectory|\Reviews.mdf;Integrated = True; Wystąpienia użytkownika = True — składa się z wielu opcji i wartości, z par opcji i wartości rozdzielane średnikami i każdej opcji i wartość ograniczonej przez znak równości. Są cztery opcje używane w tym ciągu połączenia:

- `Data Source` -Określa lokalizację serwera bazy danych i nazwa wystąpienia serwera bazy danych (jeżeli istniał). Wartość, `.\SQLEXPRESS`, jest na przykład w przypadku, gdy istnieje serwer bazy danych i nazwę wystąpienia. Okres określa, że serwer bazy danych na tym samym komputerze jako aplikacji; Nazwa wystąpienia jest `SQLEXPRESS`.
- `AttachDbFilename` -Określa lokalizację plików bazy danych. Wartość zawiera symbol zastępczy `|DataDirectory|`, który został rozwiązany na pełną ścieżkę aplikacji s `App_Data` folderu w czasie wykonywania.
- `Integrated Security` — wartość logiczna, która wskazuje, czy należy użyć określonej nazwy użytkownika i hasła podczas nawiązywania połączenia bazy danych (FAŁSZ) lub Windows bieżące poświadczenia konta (PRAWDA).
- `User Instance` -Opcja konfiguracji specyficznych dla wersji SQL Server Express Edition, która wskazuje, czy zezwolić na użytkowników innych niż administracyjne na komputerze lokalnym podłączyć a bazą danych programu SQL Server Express Edition. Zobacz [wystąpienia programu SQL Server Express użytkownika](https://msdn.microsoft.com/library/ms254504.aspx) Aby uzyskać więcej informacji na temat tego ustawienia.
  

Opcje ciąg połączenia dopuszczalny zależą od łączysz się z bazy danych i [ADO.NET](http://ADO.NET) używany dostawca bazy danych. Na przykład parametry połączenia do nawiązywania połączenia z programu Microsoft SQL Server bazy danych jest inny niż używany do nawiązania połączenia z bazą danych Oracle. Podobnie nawiązywanie połączenia z bazą danych programu Microsoft SQL Server przy użyciu dostawcy SqlClient używa parametrów połączenia innego niż przy użyciu dostawcy OLE DB.

Parametry połączenia bazy danych można tworzyć ręcznie za pomocą witryny, takie jak [ConnectionStrings.com](http://www.connectionstrings.com/) jako zasób dla jakie opcje są dostępne. Łatwiejsze podejście jest jednak dodać bazy danych do Eksploratora serwera w programie Visual Studio, a następnie do pobrania parametrów połączenia z okna właściwości. Let s użyć tej metody to drugie konstruowania parametry połączenia z serwerem bazy danych w środowisku produkcyjnym.

Otwórz program Visual Studio, a następnie przejdź do okna Eksploratora serwera (w programie Visual Web Developer, to okno jest nazywane Eksploratora bazy danych). Kliknij prawym przyciskiem myszy opcję połączenia danych i wybierz opcję Dodaj połączenie z menu kontekstowego. Spowoduje to wyświetlenie kreatora pokazany na rysunku 1. Wybierz odpowiednie źródło danych, a następnie kliknij przycisk Kontynuuj.


[![Wybierz dodać nową bazę danych do Eksploratora serwera](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image2.jpg)](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image1.jpg) 

**Rysunek 1**: Wybierz dodać nową bazę danych do Eksploratora serwera ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image3.jpg))


Następnie określ różne bazy danych informacji o połączeniu (patrz rysunek 2). Po utworzeniu konta z sieci web hosting firma, którą należy podać informacje na temat połączenia z bazą danych — nazwa serwera bazy danych, nazwy bazy danych, nazwę użytkownika i hasło używane do łączenia z bazą danych i tak dalej. Po wprowadzeniu tych informacji kliknij przycisk OK, aby zakończyć działanie kreatora i Dodaj bazy danych do Eksploratora serwera.


[![Określ informacje dotyczące połączenia bazy danych](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image5.jpg)](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image4.jpg) 

**Rysunek 2**: Określ informacje dotyczące połączenia bazy danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image6.jpg))


Środowisko produkcyjną bazę danych powinien być teraz wyświetlany w Eksploratorze serwera. Wybierz bazę danych z poziomu Eksploratora serwera, a następnie przejdź do okna właściwości. Można znaleźć właściwości o nazwie parametrów połączenia parametrami połączenia s bazy danych. Przy założeniu, że używasz bazy danych programu Microsoft SQL Server w środowisku produkcyjnym i dostawca SqlClient parametrów połączenia powinien wyglądać podobnie do następującego:

<strong>Źródło danych =<em>serverName</em>; Wykaz początkowy =<em>databaseName</em>; Utrzymuj informacje o zabezpieczeniach = True; Nazwa użytkownika =<em>username</em>; Hasło =*hasła</strong>*

Gdzie *serverName*, *databaseName*, *username*, i *hasło* są wartościami dla nazwy serwera bazy danych, bazy danych Nazwa, nazwę użytkownika i hasło dostarczonych przez firmę hosta sieci web.

## <a name="deploying-the-book-reviews-web-application"></a>Wdrażanie aplikacji sieci Web przeglądami książki

Poprzedni Samouczek przeprowadził przez kopiowanie programowanie bazy danych do środowiska produkcyjnego, ale nie Eksploruj wdrażanie aplikacji opartych na danych. W tym momencie środowiska produkcyjnego zawiera bazę danych, ale korzysta z statycznych przeglądami wersja aplikacji sprawdza książki. Należy wdrożyć aplikację nowych, opartych na danych na serwerze produkcyjnym wraz z informacjami zaktualizowanej konfiguracji.

Poświęć chwilę na wdrażanie aplikacji opartych na danych z Środowisko deweloperskie do środowiska produkcyjnego. Ten proces została omówiona szczegółowo w poprzednim samouczki. Jeśli potrzebujesz odświeżacz zobacz *Wdrażanie witryny sieci Web przy użyciu klienta FTP* lub *wdrażanie Your witryny sieci Web przy użyciu programu Visual Studio* samouczki. Należy się upewnić, że parametry połączenia bazy danych produkcyjnych jest używane w środowisku produkcyjnym, co oznacza, że alternatywnym `Web.config` plik zostanie wdrożony. W szczególności zmodyfikowane `Web.config` pliku s `<connectionStrings>` element musi zawierać ciąg połączenia bazy danych w środowisku produkcyjnym i powinien być podobny do następującego:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-vb/samples/sample2.xml)]

Należy pamiętać, że ciąg połączenia w `<connectionStrings>` jest taką samą nazwę elementu (`ReviewsConnectionString`), ale teraz zawiera parametry połączenia bazy danych produkcyjnej zamiast tworzenia ciągu połączenia bazy danych.

Jeśli nie masz więcej formalny przepływ pracy wdrażania, albo ręcznie zmodyfikować `Web.config` plików do używania ciągu połączenia bazy danych produkcyjnej przed wdrożeniem (Pamiętaj o tym przywrócić go przy użyciu parametrów połączenia bazy danych programowanie później) lub obsługa oddzielnej `Web.config` plik z informacjami konfiguracji środowiska produkcyjnego, który pobiera przekazany do środowiska produkcyjnego w ramach procesu wdrażania.

> [!NOTE]
> Jeśli przypadkowo wdrożono `Web.config` plik zawierający parametry połączenia bazy danych programowanie będą wystąpił błąd podczas próby połączenia z bazą danych aplikacji w środowisku produkcyjnym. Ten błąd manifesty jako `SqlException` komunikatem raportowania, że serwer nie został znaleziony lub był niedostępny.


Po stronie został wdrożony w środowisku produkcyjnym, odwiedź witrynę produkcji za pośrednictwem przeglądarki. Powinni widzieć i korzystać z tego samego środowiska użytkownika jako podczas uruchamiania lokalnego aplikacji opartych na danych. Oczywiście po odwiedzeniu witryny sieci Web w środowisku produkcyjnym lokacji jest obsługiwany przez serwer bazy danych produkcyjnej, należy odwiedzić witrynę sieci Web w środowisku programistycznym używa bazy danych do rozwoju. Rysunek 3 przedstawia *nauczyć się ASP.NET 3.5 w ciągu 24 godzin* znajdziesz na stronie z witryny sieci Web w środowisku produkcyjnym (trzeba zauważyć adres URL na pasku adresu przeglądarki s).


[![Data-driven aplikacji jest teraz dostępna w środowisku produkcyjnym!](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image8.jpg)](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image7.jpg) 

**Rysunek 3**: The Data-Driven aplikacja jest teraz dostępna w środowisku produkcyjnym! ([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image9.jpg))


### <a name="storing-connection-strings-in-a-separate-configuration-file"></a>Przechowywanie parametrów połączenia w pliku konfiguracji oddzielne

Typowe techniki do przechowywania informacji dotyczących środowiska projektowania i produkcji oddzielnych konfiguracji ma dwie wersje `Web.config`: jeden dla środowiska programowania i jeden w środowisku produkcyjnym. W czasie wdrażania odpowiednie `Web.config` wersji mogą zostać skopiowane do środowiska produkcyjnego. W idealnym przypadku tego procesu będzie można zautomatyzować jako część przepływu pracy wdrażania.

Zamiast utrzymania dwa oddzielne `Web.config` plików można, opcjonalnie, podaj bardziej szczegółowego różnice. Elementy wchodzące w skład `Web.config` plików można zdefiniować w plikach konfiguracji zewnętrznych, które są następnie używane w `Web.config` pliku. Mówiąc ma `Web.config` pliku dla obu środowiskach, które odwołuje się do pliku databaseConnectionStrings.config, który zawiera parametry połączenia używane przez aplikację, a będzie unikatowy dla każdego środowiska. Można znaleźć, aby rozdzielić różne informacje o konfiguracji do oddzielnych plików zawiera pisma odręcznego i łatwiejsze `Web.config` plików i inne wyraźnie opisano różnice środowisk projektowania i produkcji w konfiguracji.

Aby użyć tej metody, Rozpocznij od utworzenia nowego folderu w aplikacji sieci web o nazwie `ConfigSections`. Następnie dodaj dwa pliki w tym nowym folderze o nazwie databaseConnectionStrings.dev.config i databaseConnectionStrings.production.config. Następnie skopiuj `<connectionStrings>` element z `Web.config` w plikach databaseConnectionStrings.dev.config i databaseConnectionStrings.production.config, a następnie zmodyfikuj parametry połączenia w databaseConnectionStrings.production.config plików, dzięki czemu określa parametry połączenia bazy danych w środowisku produkcyjnym. Na przykład plik databaseConnectionStrings.dev.config powinien zawierać tylko `<connectionStrings>` elementu przy użyciu parametrów połączenia, który odwołuje się do rozwoju bazy danych:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-vb/samples/sample3.xml)]

Podobnie, plik databaseConnectionStrings.production.config powinien zawierać tylko `<connectionStrings>` elementu, ale taki, który ma parametry połączenia bazy danych w środowisku produkcyjnym.

Utwórz kopię pliku databaseConnectionStrings.dev.config i nadaj mu nazwę databaseConnectionStrings.config.

> [!NOTE]
> Można nazwę pliku konfiguracji inną niż databaseConnectionStrings.config, opcjonalnie d, takich jak `connectionStrings.config` lub `dbInfo.config`. Jednak pamiętaj nazwy pliku z `.config` rozszerzenia jako `.config` pliki domyślnie nie obsługiwane przez aparat programu ASP.NET. Jeśli nazwa pliku, czegoś innego, takich jak `connectionStrings.txt`, użytkownik może punktu przeglądarki do [www.yoursite.com/ConfigSettings/connectionStrings.txt](http://www.yoursite.com/ConfigSettings/connectionStrings.txt) i sprawdź zawartość pliku!


W tym momencie `ConfigSections` folder powinien zawierać trzy pliki (patrz rysunek 4). Pliki databaseConnectionStrings.dev.config i databaseConnectionStrings.production.config zawierają ciągi połączenia dla środowisk projektowania i produkcji, odpowiednio. Plik databaseConnectionStrings.config zawiera informacje o parametrach połączenia, który będzie używany przez aplikację sieci web w czasie wykonywania. W rezultacie pliku databaseConnectionStrings.config powinny być identyczne z pliku databaseConnectionStrings.dev.config w środowisku programistycznym produkcyjnych pliku databaseConnectionStrings.config powinny być takie same jak databaseConnectionStrings.production.config.


[![ConfigSections](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image11.jpg)](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image10.jpg) 

**Rysunek 4**: ConfigSections ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image12.jpg))


Teraz należy poinstruować `Web.config` Aby użyć pliku databaseConnectionStrings.config do jego magazynu ciągów połączenia. Otwórz `Web.config` i Zastąp istniejące `<connectionStrings>` element z następujących czynności:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-vb/samples/sample4.xml)]

`configSource` Atrybut określa ścieżkę fizyczną względem `Web.config` pliku. Jeśli zewnętrznej `.config` plik znajduje się w tym samym katalogu co `Web.config` następnie ten atrybut zostanie ustawiony na nazwę pliku `.config` pliku. Jeśli go s w podkatalogu, w przypadku databaseConnectionStrings.config, określ podfolder Rozdziel nazwy plików i folderów, takie jak ConfigSections\databaseConnectionStrings.config za pomocą ukośnik odwrotny.

Ta modyfikacja środowisk projektowania i produkcji zawiera takie same `Web.config` pliku. Jedyną różnicą jest teraz plik databaseConnectionStrings.config. Skopiuj plik databaseConnectionStrings.production.config do środowiska produkcyjnego i zmień jego nazwę na databaseConnectionStrings.config. Jeśli w przyszłości, istnieją zmiany, aby parametry połączenia bazy danych w środowisku produkcyjnym należy wprowadzić je w pliku databaseConnectionStrings.production.config, a następnie przekaż ten plik do środowiska produkcyjnego, zmiana jego nazwy databaseConnectionStrings.config.

> [!NOTE]
> Możesz określić informacje dla każdego `Web.config` element w osobnym pliku i użyj `configSource` atrybutu odwołanie do tego pliku z poziomu `Web.config`.


## <a name="summary"></a>Podsumowanie

Aplikacje oparte na danych zwykle użyć różnych baz danych w środowisk projektowania i produkcji. W rezultacie parametry połączenia bazy danych, które są przechowywane w konfiguracji s aplikacji sieci web musi być unikatowe w każdym środowisku. W tym samouczku analizujemy jak ustalić parametry połączenia bazy danych w środowisku produkcyjnym i sposobów, aby zachować informacje o parametrach połączenia unikatowy w tych dwóch środowisk.

Programowanie przyjemność!

### <a name="further-reading"></a>Dalsze informacje

Więcej informacji dotyczących tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Parametry połączenia i pliki konfiguracji](https://msdn.microsoft.com/library/ms254494.aspx)
- [Informacje o @ ConnectionStrings.com ciągi konfiguracji bazy danych](http://www.connectionstrings.com/)
- [Przenoszenie ustawień z pliku Web.config](http://www.asp101.com/tips/index.asp?id=154)
- [Dokumentacja techniczna dotycząca &lt;connectionStrings&gt; — Element](https://msdn.microsoft.com/library/bf7sd233.aspx)

> [!div class="step-by-step"]
> [Poprzednie](deploying-a-database-vb.md)
> [dalej](configuring-a-website-that-uses-application-services-vb.md)
