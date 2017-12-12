---
uid: mvc/overview/older-versions-1/nerddinner/create-a-database
title: "Utwórz bazę danych | Dokumentacja firmy Microsoft"
author: microsoft
description: "Krok 2 przedstawiono kroki, aby utworzyć bazę danych zawierający wszystkie obiad i RSVP danych dla aplikacji NerdDinner."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 983f3ffa-08b8-4868-b8c9-aa34593fc683
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-database
msc.type: authoredcontent
ms.openlocfilehash: 7635722fc357356edd06fb4cff301a8c4dfebbef
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="create-a-database"></a>Tworzenie bazy danych
====================
przez [firmy Microsoft](https://github.com/microsoft)

[Pobierz plik PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Jest to krok 2 z bezpłatny ["NerdDinner" samouczek aplikacji](introducing-the-nerddinner-tutorial.md) który przeszukiwań przez proces kompilacji mały, ale ukończyć, aplikacji sieci web przy użyciu platformy ASP.NET MVC 1.
> 
> Krok 2 przedstawiono kroki, aby utworzyć bazę danych zawierający wszystkie obiad i RSVP danych dla aplikacji NerdDinner.
> 
> Jeśli używasz programu ASP.NET MVC 3, zaleca się wykonanie [pobierania uruchomiona z MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) lub [magazynu utworów muzycznych MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) samouczki.


## <a name="nerddinner-step-2-creating-the-database"></a>NerdDinner krok 2: Tworzenie bazy danych

Bazy danych będzie używany do przechowywania wszystkich danych obiad i RSVP w naszej aplikacji NerdDinner.

W poniższych krokach przedstawiono tworzenie bazy danych przy użyciu bezpłatna wersja programu SQL Server Express (które można łatwo zainstalować przy użyciu V2 z [Instalatora platformy sieci Web firmy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)). Cały kod przedstawiono tworzenie firma Microsoft współpracuje z programu SQL Server Express i pełnego serwera SQL.

### <a name="creating-a-new-sql-server-express-database"></a>Tworzenie nowej bazy danych programu SQL Server Express

Firma Microsoft będzie rozpocząć, klikając prawym przyciskiem myszy na naszych projektu sieci web, a następnie wybierz **Add -&gt;nowy element** polecenie:

![](create-a-database/_static/image1.png)

Zostanie wyświetlone okno dialogowe "Dodaj nowy element" programu Visual Studio. Firma Microsoft będzie filtrować według kategorii "Dane" i wybierz szablon elementu "Bazy danych SQL Server":

![](create-a-database/_static/image2.png)

Firma Microsoft będzie nazwa programu SQL Server Express bazy danych, którą chcemy, aby utworzyć "NerdDinner.mdf" i kliknij przycisk ok. Visual Studio zostanie następnie poprosić nam Jeśli chcemy dodać ten plik do naszej \App\_katalog danych (która jest już katalog instalacja przy użyciu odczytu i zapisu zabezpieczeń listy ACL):

![](create-a-database/_static/image3.png)

Firma Microsoft będzie kliknij przycisk "Tak" i naszej nowej bazy danych zostanie utworzona i dodane do naszych Eksploratora rozwiązań:

![](create-a-database/_static/image4.png)

### <a name="creating-tables-within-our-database"></a>Tworzenie tabel w naszej bazie danych

Mamy teraz nowe pustej bazy danych. Dodajmy niektóre tabele do niego.

W tym celu firma Microsoft będzie przejdź do okna kartę "Eksploratora serwera" w programie Visual Studio, która umożliwia firmie Microsoft w celu zarządzania bazami danych i serwerami. Bazy danych programu SQL Server Express, przechowywane w \App\_folderu danych aplikacji automatycznie będzie wyświetlany w Eksploratorze serwera. Firma Microsoft Opcjonalnie użyj ikony "Połączenia do bazy danych" w górnej części okna "Eksploratora serwera", aby dodać dodatkowe baz danych programu SQL Server (lokalne i zdalne) do listy również:

![](create-a-database/_static/image5.png)

Dodamy dwóch tabel do NerdDinner bazy jest kompletny — jeden do przechowywania naszych kolacji, a drugi do śledzenia RSVP akceptacji. Można utworzyć nowe tabele przez kliknięcie prawym przyciskiem myszy folder "Tabele" w naszej bazie danych i wybierając polecenie "Dodaj nową tabelę":

![](create-a-database/_static/image6.png)

Spowoduje to otwarcie projektanta tabeli, który pozwala skonfigurować schemat naszych tabeli. Dla tabeli naszych "Kolacji" dodamy 10 kolumny danych:

![](create-a-database/_static/image7.png)

Chcemy kolumny "DinnerID", która ma być unikatowy klucz podstawowy dla tabeli. Można to skonfigurować prawym przyciskiem myszy w kolumnie "DinnerID" i wybierając polecenie "Ustaw klucz podstawowy":

![](create-a-database/_static/image8.png)

Oprócz tworzenia DinnerID klucza podstawowego, również chcemy, aby go skonfigurować jako kolumnę "identity", którego wartość jest automatycznie zwiększany w miarę dodawania nowych wierszy danych w tabeli (to znaczy pierwszy wiersz wstawiony obiad będzie mieć DinnerID 1, drugi wstawionego wiersza będą mieć DinnerID 2, itp).

Firma Microsoft to zrobić, wybierając kolumnę "DinnerID", a następnie za pomocą edytora "Kolumny właściwości" Ustaw właściwość "(jest tożsamość)" w kolumnie "Yes". Będą używane wartości domyślne standardowe tożsamości (liczone od 1 i zwiększyć wartości 1 dla każdego nowego wiersza obiad):

![](create-a-database/_static/image9.png)

Firma Microsoft będzie zapisaniu naszych tabelę, wpisując polecenie Ctrl-S lub za pomocą **pliku -&gt;zapisać** polecenia menu. Wyświetla monit nam w nazwie tabeli. Firma Microsoft będzie nadaj mu nazwę "Kolacji":

![](create-a-database/_static/image10.png)

Naszej nowej tabeli kolacji następnie pojawi się w naszej bazie danych w Eksploratorze serwera.

Firma Microsoft będzie następnie powtórz powyższe kroki i Utwórz tabelę "RSVP". Ta tabela z ma 3 kolumny. Firma Microsoft będzie konfiguracja kolumny RsvpID jako klucz podstawowy i wybierz kolumny tożsamości.

![](create-a-database/_static/image11.png)

Firma Microsoft będzie zapisać go i nadaj mu nazwę "RSVP".

### <a name="setting-up-a-foreign-key-relationship-between-tables"></a>Definiowanie relacji klucza obcego między tabelami

Mamy teraz dwie tabele w naszej bazie danych. Naszych ostatni krok projektowania schematu będzie można skonfigurować relację "jeden do wielu" tych dwóch tabel — tak, aby każdy wiersz obiad można powiązane z zero lub więcej wierszy RSVP, które mają zastosowanie do niej. Spowoduje to wykonanie przez skonfigurowanie tabeli RSVP "DinnerID" kolumnie relacji klucza obcego z kolumną "DinnerID" w tabeli "Kolacji".

W tym celu otworzymy tabeli RSVP w Projektancie tabel kliknij go dwukrotnie w Eksploratorze serwera. Następnie wybierzemy kolumny "DinnerID", kliknij prawym przyciskiem myszy i wybierz polecenie "Relationshps..." polecenia menu kontekstowe:

![](create-a-database/_static/image12.png)

Zostanie wyświetlone okno dialogowe, które możemy użyć do instalacji relacje między tabelami:

![](create-a-database/_static/image13.png)

Firma Microsoft będzie kliknij przycisk "Dodaj", aby dodać nową relację do okna dialogowego. Po dodaniu relacji firma Microsoft będzie rozwiń węzeł "Tabele i kolumny specyfikacji" widok drzewa w siatce właściwości z prawej strony okna dialogowego, a następnie kliknij przycisk "..." z prawej strony:

![](create-a-database/_static/image14.png)

Klikając przycisk "...", zostanie wyświetlone okno dialogowe innego, która pozwala określić, które tabele i kolumny są uczestniczących w relacji, a także zezwolić nam na nazwę relacji.

Firma Microsoft będzie zmienić tabeli klucza podstawowego jako "Kolacji", a następnie wybierz kolumny "DinnerID" w tabeli kolacji jako klucz podstawowy. Nasze tabeli RSVP będzie tabeli klucza obcego i RSVP. Kolumna DinnerID zostaną skojarzone jako klucza obcego:

![](create-a-database/_static/image15.png)

Teraz zostanie skojarzona z wiersza w tabeli obiad każdego wiersza w tabeli RSVP. SQL Server będzie utrzymanie integralności referencyjnej nam — i uniemożliwiają nam dodanie nowego wiersza RSVP, jeśli go nie wskazuje prawidłowego wiersza obiad. Ponadto uniemożliwi to nam usuwanie wiersza obiad, jeśli występują nadal RSVP wierszy odwołujące się do niego.

### <a name="adding-data-to-our-tables"></a>Dodawanie danych do naszej tabel

Teraz zakończyć dodawanie przykładowych danych do tabeli naszych kolacji. Firma Microsoft dodać dane do tabeli, klikając go w Eksploratorze serwera i wybierając polecenie "Pokaż danych tabeli":

![](create-a-database/_static/image16.png)

Dodamy kilka wierszy obiad możemy użyć później Rozpoczniemy wdrażania aplikacji:

![](create-a-database/_static/image17.png)

### <a name="next-step"></a>Następny krok

Po zakończeniu tworzenia naszej bazie danych. Teraz Utwórzmy klasy modeli, które możemy użyć do wykonywania zapytań i zaktualizować go.

>[!div class="step-by-step"]
[Poprzednie](create-a-new-aspnet-mvc-project.md)
[dalej](build-a-model-with-business-rule-validations.md)
