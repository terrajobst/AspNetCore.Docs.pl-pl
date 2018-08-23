---
uid: mvc/overview/older-versions-1/nerddinner/create-a-database
title: Tworzenie bazy danych | Dokumentacja firmy Microsoft
author: microsoft
description: Krok 2 przedstawiono kroki, aby utworzyć bazę danych, zawierający wszystkie dinner i Odpowiedz na zaproszenie dane dla naszej aplikacji NerdDinner.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 983f3ffa-08b8-4868-b8c9-aa34593fc683
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-database
msc.type: authoredcontent
ms.openlocfilehash: b6ab0740f251889f0fa0561809cac2bbe79bcb3a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755169"
---
<a name="create-a-database"></a>Tworzenie bazy danych
====================
przez [firmy Microsoft](https://github.com/microsoft)

[Pobierz plik PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Jest to krok 2 z BEZPŁATNEJ [samouczek aplikacji "NerdDinner"](introducing-the-nerddinner-tutorial.md) , przeszukiwania — szczegółowe instrukcje dotyczące tworzenia małych, ale ukończyć, aplikacji sieci web przy użyciu platformy ASP.NET MVC 1.
> 
> Krok 2 przedstawiono kroki, aby utworzyć bazę danych, zawierający wszystkie dinner i Odpowiedz na zaproszenie dane dla naszej aplikacji NerdDinner.
> 
> Jeśli używasz programu ASP.NET MVC 3, zaleca się wykonać [Rozpoczynanie pracy z MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) lub [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) samouczków.


## <a name="nerddinner-step-2-creating-the-database"></a>NerdDinner krok 2: Tworzenie bazy danych

Będziemy używać bazy danych do przechowywania wszystkich danych obiad i RSVP dla naszej aplikacji NerdDinner.

W poniższych krokach przedstawiono tworzenie bazy danych przy użyciu bezpłatnego programu SQL Server Express edition (który można łatwo zainstalować, za pomocą programu w wersji 2 [Instalatora platformy sieci Web firmy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)). Cały kod, będziemy pisać w programach SQL Server Express i pełnego programu SQL Server.

### <a name="creating-a-new-sql-server-express-database"></a>Tworzenie nowej bazy danych programu SQL Server Express

Firma Microsoft będzie zacząć, klikając prawym przyciskiem myszy w projekcie sieci web, a następnie wybierz **Add -&gt;nowy element** polecenia menu:

![](create-a-database/_static/image1.png)

Zostanie wyświetlone okno dialogowe "Dodaj nowy element" Visual Studio. Firma Microsoft będzie filtrować według kategorii "Dane" i wybierz szablon elementu "Bazy danych SQL Server":

![](create-a-database/_static/image2.png)

Firma Microsoft będzie nazwa programu SQL Server Express bazy danych, którą chcemy, aby utworzyć "NerdDinner.mdf" i kliknij ok. Program Visual Studio będzie następnie poproś nam Jeśli chcemy dodać ten plik do naszych \App\_katalog danych (które katalogu jest już instalacji za pomocą odczytu i zapisu listy ACL zabezpieczeń):

![](create-a-database/_static/image3.png)

Firma Microsoft będzie kliknij przycisk "Tak", i naszej nowej bazy danych zostanie utworzona i dodany do naszych Eksploratora rozwiązań:

![](create-a-database/_static/image4.png)

### <a name="creating-tables-within-our-database"></a>Tworzenie tabel w naszej bazie danych

W efekcie powstał nowej pustej bazy danych. Dodajmy kilka tabel do niego.

W tym celu firma Microsoft będzie przejdź do okna kartę "Eksploratora serwera" w programie Visual Studio, która pozwala na zarządzanie bazami danych i serwerami. Bazy danych programu SQL Server Express, przechowywane w \App\_folderu danych naszej aplikacji zostanie automatycznie pokazana w Eksploratorze serwera. Firma Microsoft Opcjonalnie użyj ikony "Połączenia do bazy danych" w górnej części okna "Eksploratora serwera", aby dodać dodatkowe bazy danych SQL Server (zarówno lokalnych, jak i zdalnych) do listy, a także:

![](create-a-database/_static/image5.png)

Dwie tabele zostanie dodany do bazy NerdDinner — jeden do przechowywania naszych kolacji, a druga do śledzenia akceptacje RSVP. Możemy tworzyć nowe tabele, klikając prawym przyciskiem myszy w folderze "Tabele" w naszej bazie danych i wybierając polecenie "Dodaj nową tabelę" w menu:

![](create-a-database/_static/image6.png)

Spowoduje to otwarcie projektanta tabel, który pozwala skonfigurował schemat tabeli. Naszym spisie "Kolacji" dodamy 10 kolumny danych:

![](create-a-database/_static/image7.png)

Chcemy, aby kolumny "DinnerID", aby mieć unikatowy klucz podstawowy dla tabeli. Firma Microsoft można skonfigurować, klikając prawym przyciskiem myszy, w kolumnie "DinnerID" i wybierając pozycję "Ustaw klucz podstawowy" element menu:

![](create-a-database/_static/image8.png)

Oprócz DinnerID klucza podstawowego, chcemy również skonfigurować ją jako kolumnę "tożsamość", którego wartość jest automatycznie zwiększany w miarę dodawania nowych wierszy danych do tabeli (to znaczy pierwszy wiersz wstawionego obiad odniesie DinnerID 1, drugi wstawionego wiersza będzie miał DinnerID 2, itp).

Możemy to zrobić, wybierając kolumnę "DinnerID", a następnie za pomocą edytora "Kolumny właściwości" można ustawić właściwości "(jest tożsamość)" dla kolumny "Yes". Będziemy używać tożsamości standardowe wartości domyślne (liczone od 1 i zwiększ 1 dla każdego nowego wiersza obiad):

![](create-a-database/_static/image9.png)

Następnie zostaną zapisane tabeli, wpisując Ctrl + S lub za pomocą **plikach&gt;Zapisz** polecenia menu. Wyświetli monit nam nazwy tabeli. Firma Microsoft będzie nadaj mu nazwę "Kolacji":

![](create-a-database/_static/image10.png)

Naszej nowej tabeli kolacji zostanie następnie wyświetlane w naszej bazie danych w Eksploratorze serwera.

Następnie utworzymy Powtórz powyższe kroki i Utwórz tabelę "RSVP". Ta tabela mająca mieć 3 kolumny. Firma Microsoft będzie skonfigurować kolumny RsvpID jako klucz podstawowy i wybierz kolumnę tożsamości:

![](create-a-database/_static/image11.png)

Firma Microsoft będzie Zapisz go i nadaj mu nazwę "RSVP".

### <a name="setting-up-a-foreign-key-relationship-between-tables"></a>Definiowanie relacji klucza obcego między tabelami

Teraz mamy dwie tabele w naszej bazie danych. Nasze ostatnim krokiem projektowania schematu będzie można skonfigurować "jeden do wielu" relacji między tymi dwiema tabelami — tak, aby firma Microsoft można skojarzyć każdy wiersz obiad z zero lub więcej wierszy RSVP, które go dotyczą. Firma Microsoft będzie to zrobić, konfigurując tabeli RSVP "DinnerID" w kolumnie relacji klucza obcego z kolumną "DinnerID" w tabeli "Kolacji".

W tym celu firma Microsoft będzie otwierają RSVP tabeli w Projektancie tabel, klikając dwukrotnie plik w Eksploratorze serwera. Następnie należy wybrać kolumnę "DinnerID" znajdujący się w nim, kliknij prawym przyciskiem myszy, a wybierz polecenia menu kontekstowego "Relationshps...":

![](create-a-database/_static/image12.png)

Pojawi się okno dialogowe, które możemy użyć do konfiguracji relacji między tabelami:

![](create-a-database/_static/image13.png)

Firma Microsoft będzie kliknij przycisk "Dodaj", aby dodać nową relację do okna dialogowego. Po dodaniu relacji firma Microsoft będzie rozwiń węzeł "Tabele i kolumny Specyfikacja" widok drzewa w siatce właściwości po prawej stronie okna dialogowego, a następnie kliknij przycisk "...", z prawej strony:

![](create-a-database/_static/image14.png)

Klikając przycisk "...", zostanie wyświetlone okno innego okna dialogowego, który pozwala określić, które tabele i kolumny są uczestniczących w relacji, a także umożliwiają nam nazwę relacji.

Firma Microsoft będzie zmienić tabeli klucza podstawowego, jako "Kolacji", a następnie wybierz kolumnę "DinnerID" w tabeli kolacji jako klucz podstawowy. Tabeli RSVP będzie Tabela klucza obcego i RSVP. Kolumna DinnerID ma być skojarzona jako klucza obcego:

![](create-a-database/_static/image15.png)

Teraz każdy wiersz w tabeli RSVP zostanie skojarzona z wiersza w tabeli obiad. Program SQL Server spowoduje zachowania więzów integralności dla nas — i uniemożliwiają nam dodanie nowego wiersza RSVP, jeśli go nie wskazuje prawidłowego wiersza obiad. On również uniemożliwi nam usunięcie wierszy obiad, jeśli są nadal RSVP wierszy, odwołując się do niego.

### <a name="adding-data-to-our-tables"></a>Dodawanie danych do naszych tabel

Teraz zakończone przez dodawanie przykładowych danych do tabeli kolacji. Możemy dodać dane do tabeli, kliknij prawym przyciskiem myszy go w Eksploratorze serwera i wybierając polecenie "Pokaż dane tabeli":

![](create-a-database/_static/image16.png)

Dodamy kilka wierszy danych obiad, które możemy użyć później Rozpoczniemy wdrażanie aplikacji:

![](create-a-database/_static/image17.png)

### <a name="next-step"></a>Następny krok

Po zakończeniu tworzenia naszej bazie danych. Utwórzmy teraz klasy modeli, których możemy użyć do wykonywania zapytań i zaktualizuj go.

> [!div class="step-by-step"]
> [Poprzednie](create-a-new-aspnet-mvc-project.md)
> [dalej](build-a-model-with-business-rule-validations.md)
