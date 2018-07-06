---
uid: web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
title: Wywoływanie interfejsu API sieci Web z Windows Phone 8 aplikacji (C#) | Dokumentacja firmy Microsoft
author: rmcmurray
description: Utwórz całego scenariusza end-to-end, składający się z aplikacji interfejsu API sieci Web platformy ASP.NET, która zawiera katalog książek do aplikacji Windows Phone 8.
ms.author: aspnetcontent
ms.date: 10/09/2013
ms.assetid: b9775f41-352a-4f82-baa6-23e95b342e20
msc.legacyurl: /web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
msc.type: authoredcontent
ms.openlocfilehash: 6b7a833818424cbf3a3bf9e1e14e5b2864742c38
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37805045"
---
<a name="calling-web-api-from-a-windows-phone-8-application-c"></a>Wywoływanie interfejsu API sieci Web z aplikacji Windows Phone 8 (C#)
====================
przez [Robert McMurray](https://github.com/rmcmurray)

W tym samouczku dowiesz się, jak utworzyć całego scenariusza end-to-end, składający się z aplikacji interfejsu API sieci Web platformy ASP.NET, która zawiera katalog książek do aplikacji Windows Phone 8.

### <a name="overview"></a>Omówienie

RESTful usług, takich jak ASP.NET Web API upraszczają tworzenie aplikacji opartych na protokole HTTP dla deweloperów, zapewniając abstrakcyjność architektury dla aplikacji po stronie serwera i klienta. Zamiast tworzyć własności protokołu na podstawie gniazda do komunikacji, internetowego interfejsu API deweloperzy muszą po prostu publikowania wymaganego metody HTTP w swojej aplikacji, (na przykład: GET, POST, PUT, DELETE), a deweloperzy aplikacji klienckich tylko muszą korzystać z metody HTTP, które są niezbędne do ich aplikacji.

W tym samouczku end-to-end dowiesz się, jak używać interfejsu API sieci Web do tworzenia następujących projektów:

- W [pierwszej części tego samouczka](#STEP1), utworzy aplikację interfejsu API sieci Web platformy ASP.NET, która obsługuje wszystkie operacje tworzenia, odczytu, aktualizowania lub usuwania (CRUD) do zarządzania katalogiem książki. Ta aplikacja będzie używać [przykładowy plik XML (books.xml)](https://msdn.microsoft.com/library/windows/desktop/ms762271.aspx) z witryny MSDN.
- W [druga część tego samouczka](#STEP2), utworzy interaktywną aplikację Windows Phone 8, która pobiera dane z aplikacji interfejsu API sieci Web.

#### <a name="prerequisites"></a>Wymagania wstępne

- Visual Studio 2013 z zainstalowany zestaw Windows Phone 8 SDK
- Windows 8 lub nowszy na 64-bitowym systemie przy użyciu funkcji Hyper-V jest zainstalowana
- Aby uzyskać listę dodatkowych wymagań, zobacz *wymagania systemowe* sekcji na [Windows Phone SDK 8.0](https://www.microsoft.com/download/details.aspx?id=35471) strony pobierania.

> [!NOTE]
> Jeśli użytkownik chce przetestować łączność między interfejsu API sieci Web i projektów Windows Phone 8 w systemie lokalnym, konieczne będzie postępuj zgodnie z instrukcjami w *[nawiązywania połączenia aplikacji interfejsu API sieci Web na lokalnym Emulator Windows Phone 8 Komputer](https://go.microsoft.com/fwlink/?LinkId=324014)* artykuł, aby skonfigurować środowiska testowego.


<a id="STEP1"></a>
### <a name="step-1-creating-the-web-api-bookstore-project"></a>Krok 1: Tworzenie projektu księgarni interfejsu API sieci Web

Pierwszym krokiem w tym samouczku end-to-end jest utworzenie projektu składnika Web API, która obsługuje wszystkie operacje CRUD; należy pamiętać, że doda projekt aplikacji Windows Phone w tym rozwiązaniu [kroku 2](#STEP2) po ukończeniu tego samouczka.

1. Otwórz **programu Visual Studio 2013**.
2. Kliknij przycisk **pliku**, następnie **nowe**, a następnie **projektu**.
3. Gdy **nowy projekt** zostanie wyświetlone okno dialogowe, rozwiń **zainstalowane**, następnie **szablony**, następnie **Visual C#**, a następnie **Web**.


   | [![](calling-web-api-from-a-windows-phone-8-application/_static/image2.png)](calling-web-api-from-a-windows-phone-8-application/_static/image1.png) |
   |-----------------------------------------------------------------------------------------------------------------------------------------------------|
   |                                                                Kliknij obraz, aby rozwinąć                                                                |


4. Wyróżnij **aplikacji sieci Web ASP.NET**, wprowadź **księgarni** nazwę projektu, a następnie kliknij przycisk **OK**.
5. Gdy **nowy projekt ASP.NET** zostanie wyświetlone okno dialogowe, wybierz **interfejsu API sieci Web** szablonu, a następnie kliknij **OK**.


   | [![](calling-web-api-from-a-windows-phone-8-application/_static/image4.png)](calling-web-api-from-a-windows-phone-8-application/_static/image3.png) |
   |-----------------------------------------------------------------------------------------------------------------------------------------------------|
   |                                                                Kliknij obraz, aby rozwinąć                                                                |


6. Po otwarciu projektu składnika Web API, należy usunąć kontroler próbki z projektu:

    1. Rozwiń **kontrolerów** folder w Eksploratorze rozwiązań.
    2. Kliknij prawym przyciskiem myszy **ValuesController.cs** pliku, a następnie kliknij przycisk **Usuń**.
    3. Kliknij przycisk **OK** po wyświetleniu monitu o potwierdzenie usunięcia.
7. Dodawanie pliku danych XML do projektu interfejsu API sieci Web; Ten plik zawiera zawartość katalogu księgarni:

   1. Kliknij prawym przyciskiem myszy **aplikacji\_danych** folder w Eksploratorze rozwiązań, następnie kliknij przycisk **Dodaj**, a następnie kliknij przycisk **nowy element**.
   2. Gdy **Dodaj nowy element** zostanie wyświetlone okno dialogowe, zaznacz **pliku XML** szablonu.
   3. Nadaj plikowi nazwę **Books.xml**, a następnie kliknij przycisk **Dodaj**.
   4. Gdy **Books.xml** plik jest otwarty, Zastąp kod w pliku XML z próbki **books.xml** pliku w witrynie MSDN: 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample1.xml)]
   5. Zapisz i zamknij plik XML.

8. Dodawanie modelu księgarni do projektu interfejsu API sieci Web; Ten model zawiera logikę aplikacji księgarni tworzenia, odczytu, aktualizowania lub usuwania (CRUD):

   1. Kliknij prawym przyciskiem myszy **modeli** folder w Eksploratorze rozwiązań, następnie kliknij przycisk **Dodaj**, a następnie kliknij przycisk **klasy**.
   2. Gdy **Dodaj nowy element** zostanie wyświetlone okno dialogowe, określ nazwę pliku klasy **BookDetails.cs**, a następnie kliknij przycisk **Dodaj**.
   3. Gdy **BookDetails.cs** plik jest otwarty, Zastąp kod w pliku następującym kodem: 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample2.cs)]
   4. Zapisz i Zamknij **BookDetails.cs** pliku.

9. Dodaj kontroler księgarni z projektem interfejsu API sieci Web:

   1. Kliknij prawym przyciskiem myszy **kontrolerów** folder w Eksploratorze rozwiązań, następnie kliknij przycisk **Dodaj**, a następnie kliknij przycisk **kontrolera**.
   2. Gdy **Dodawanie szkieletu** zostanie wyświetlone okno dialogowe, zaznacz **kontroler internetowego interfejsu API 2 — pusty**, a następnie kliknij przycisk **Dodaj**.
   3. Gdy **Dodaj kontroler** zostanie wyświetlone okno dialogowe, określ nazwę kontrolera **BooksController**, a następnie kliknij przycisk **Dodaj**.
   4. Gdy **BooksController.cs** plik jest otwarty, Zastąp kod w pliku następującym kodem: 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample3.cs)]
   5. Zapisz i Zamknij **BooksController.cs** pliku.

10. Utwórz aplikację interfejsu API sieci Web, aby sprawdzić błędy.

<a id="STEP2"></a>
### <a name="step-2-adding-the-windows-phone-8-bookstore-catalog-project"></a>Krok 2: Dodawanie projektu wykazu systemu Windows Phone 8 księgarni

Następnym krokiem w tym scenariuszu end-to-end jest utworzyć katalogu aplikacji dla Windows Phone 8. Ta aplikacja będzie używać *Windows Phone z danymi aplikacji* szablon domyślny interfejs użytkownika który będzie używać aplikacji internetowego interfejsu API, który został utworzony w [kroku 1](#STEP1) po ukończeniu tego samouczka jako źródło danych.

1. Kliknij prawym przyciskiem myszy **księgarni** rozwiązania w Eksploratorze rozwiązań, następnie kliknij przycisk **Dodaj**, a następnie **nowy projekt**.
2. Gdy **nowy projekt** zostanie wyświetlone okno dialogowe, rozwiń **zainstalowane**, następnie **Visual C#**, a następnie **Windows Phone**.
3. Wyróżnij **Windows Phone z danymi aplikacji**, wprowadź **BookCatalog** nazwę, a następnie kliknij przycisk **OK**.
4. Dodaj pakiet NuGet struktury Json.NET do **BookCatalog** projektu:

    1. Kliknij prawym przyciskiem myszy **odwołania** dla **BookCatalog** projektu w Eksploratorze rozwiązań, a następnie kliknij przycisk **Zarządzaj pakietami NuGet**.
    2. Gdy **Zarządzaj pakietami NuGet** zostanie wyświetlone okno dialogowe, rozwiń **Online** sekcji, a następnie zaznacz **nuget.org**.
    3. Wprowadź **Json.NET** w wyszukiwaniu pola, a następnie kliknij ikonę wyszukiwania.
    4. Wyróżnij **Json.NET** w wynikach wyszukiwania, a następnie kliknij **zainstalować**.
    5. Po zakończeniu instalacji kliknij przycisk **Zamknij**.
5. Dodaj **BookDetails** modelu do **BookCatalog** projektu; zawiera model ogólny klasy księgarni:

   1. Kliknij prawym przyciskiem myszy **BookCatalog** projektu w Eksploratorze rozwiązań, a następnie kliknij przycisk **Dodaj**, a następnie kliknij przycisk **nowy Folder**.
   2. Nadaj nazwę nowego folderu **modeli**.
   3. Kliknij prawym przyciskiem myszy **modeli** folder w Eksploratorze rozwiązań, następnie kliknij przycisk **Dodaj**, a następnie kliknij przycisk **klasy**.
   4. Gdy **Dodaj nowy element** zostanie wyświetlone okno dialogowe, określ nazwę pliku klasy **BookDetails.cs**, a następnie kliknij przycisk **Dodaj**.
   5. Gdy **BookDetails.cs** plik jest otwarty, Zastąp kod w pliku następującym kodem: 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample4.cs)]
   6. Zapisz i Zamknij **BookDetails.cs** pliku.

6. Aktualizacja **MainViewModel.cs** klasy, aby uwzględnić funkcje w celu komunikowania się z aplikacją interfejsu API sieci Web księgarni:

   1. Rozwiń **modele widoków** folder w Eksploratorze rozwiązań, a następnie kliknij dwukrotnie plik **MainViewModel.cs** pliku.
   2. Gdy **MainViewModel.cs** plik jest otwarty, Zastąp kod w pliku następującym kodem; należy pamiętać, że konieczne będzie zaktualizowanie wartości `apiUrl` stałej przy użyciu rzeczywistego adresu URL interfejsu API sieci Web: 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample5.cs)]
   3. Zapisz i Zamknij **MainViewModel.cs** pliku.

7. Aktualizacja **MainPage.xaml** plik, aby dostosować nazwę aplikacji:

   1. Kliknij dwukrotnie **MainPage.xaml** plik w Eksploratorze rozwiązań.
   2. Gdy **MainPage.xaml** plik jest otwarty, Znajdź następujące wiersze kodu: 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample6.xml)]
   3. Zastąp te wiersze następujących czynności: 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample7.xml)]
   4. Zapisz i Zamknij **MainPage.xaml** pliku.

8. Aktualizacja **DetailsPage.xaml** plik, aby dostosować wyświetlane elementy:

   1. Kliknij dwukrotnie **DetailsPage.xaml** plik w Eksploratorze rozwiązań.
   2. Gdy **DetailsPage.xaml** plik jest otwarty, Znajdź następujące wiersze kodu: 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample8.xml)]
   3. Zastąp te wiersze następujących czynności: 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample9.xml)]
   4. Zapisz i Zamknij **DetailsPage.xaml** pliku.

9. Tworzenie aplikacji Windows Phone, aby sprawdzić błędy.

### <a name="step-3-testing-the-end-to-end-solution"></a>Krok 3: Rozwiązania End-to-End do testowania

Jak wspomniano w *wymagania wstępne* części tego samouczka, podczas testowania łączności między interfejsu API sieci Web i Windows Phone 8 projekty w systemie lokalnym, musisz wykonaj instrukcje z sekcji *[ Łączenie z emulatora systemu Windows Phone 8 do aplikacji interfejsu API sieci Web na komputerze lokalnym](https://go.microsoft.com/fwlink/?LinkId=324014)* artykuł, aby skonfigurować środowiska testowego.

Po utworzeniu, aby skonfigurować środowisko testowe, należy skonfigurować aplikację Windows Phone jako projekt startowy. Aby to zrobić, zaznacz **BookCatalog** w Eksploratorze rozwiązań, a następnie kliknij przycisk **Ustaw jako projekt startowy**:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image6.png)](calling-web-api-from-a-windows-phone-8-application/_static/image5.png) |
| --- |
| Kliknij obraz, aby rozwinąć |

Po naciśnięciu klawisza F5, program Visual Studio będzie uruchomić zarówno Windows Phone Emulator, co spowoduje wyświetlenie &quot;Czekaj&quot; komunikatu, gdy dane aplikacji są pobierane z internetowego interfejsu API:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image8.png)](calling-web-api-from-a-windows-phone-8-application/_static/image7.png) |
| --- |
| Kliknij obraz, aby rozwinąć |

Jeśli wszystko, co zakończy się pomyślnie, powinien zostać wyświetlony katalogu wyświetlane:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image10.png)](calling-web-api-from-a-windows-phone-8-application/_static/image9.png) |
| --- |
| Kliknij obraz, aby rozwinąć |

Naciśnij tytuł wszystkie książki aplikacja wyświetla opis książki:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image12.png)](calling-web-api-from-a-windows-phone-8-application/_static/image11.png) |
| --- |
| Kliknij obraz, aby rozwinąć |

Jeśli aplikacja nie może komunikować się z interfejsem API sieci Web, zostanie wyświetlony komunikat o błędzie:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image14.png)](calling-web-api-from-a-windows-phone-8-application/_static/image13.png) |
| --- |
| Kliknij obraz, aby rozwinąć |

Jeśli naciśniesz pozycję dotyczącą komunikatu o błędzie, będą wyświetlane dodatkowe szczegóły dotyczące błędu:


| [![](calling-web-api-from-a-windows-phone-8-application/_static/image16.png)](calling-web-api-from-a-windows-phone-8-application/_static/image15.png) |
|-------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                                                 Kliknij obraz, aby rozwinąć                                                                 |

