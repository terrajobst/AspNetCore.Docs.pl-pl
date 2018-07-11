---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12
title: 'Wdrażanie aplikacji sieci Web ASP.NET za pomocą programu SQL Server Compact przy użyciu programu Visual Studio lub Visual Web Developer: Wdrażanie aktualizacji Code-Only - 8 z 12 | Dokumentacja firmy Microsoft'
author: tdykstra
description: W tej serii samouczków dowiesz się, jak wdrożyć (opublikować) programu ASP.NET projektu aplikacji sieci web, który zawiera bazę danych programu SQL Server Compact przy użyciu Visual Stu...
ms.author: aspnetcontent
ms.date: 11/17/2011
ms.assetid: ddf6252f-9413-4c0c-a360-2cef8d231717
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12
msc.type: authoredcontent
ms.openlocfilehash: 16a9e48034536964d526717ecde2a9b813818963
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37816087"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-code-only-update---8-of-12"></a>Wdrażanie aplikacji sieci Web ASP.NET za pomocą programu SQL Server Compact przy użyciu programu Visual Studio lub Visual Web Developer: Wdrażanie aktualizacji Code-Only - 8 z 12
====================
przez [Tom Dykstra](https://github.com/tdykstra)

[Pobieranie projektu startowego](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> W tej serii samouczków dowiesz się, jak wdrożyć (opublikować) ASP.NET projektu aplikacji sieci web, która zawiera bazę danych programu SQL Server Compact przy użyciu programu Visual Studio 2012 RC lub Visual Studio Express 2012 RC for Web. Umożliwia także programu Visual Studio 2010 po zainstalowaniu aktualizacji publikowania w sieci Web. Aby zapoznać się z wprowadzeniem do serii, zobacz [pierwszym samouczku tej serii](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Aby uzyskać samouczek, który zawiera funkcje wdrażania wprowadzone po wersji RC programu Visual Studio 2012, pokazuje, jak wdrażać wersje programu SQL Server, innym niż SQL Server Compact i pokazuje, jak wdrożyć w usłudze Azure App Service Web Apps, zobacz [wdrażanie aplikacji internetowych ASP.NET za pomocą programu Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Omówienie

Po początkowym wdrożeniu kontynuuje swoją pracę, obsługi i tworzenia witryny sieci web, a niedługo będzie, aby wdrożyć aktualizację. Ten samouczek przeprowadzi Cię przez proces wdrażania aktualizacji w kodzie aplikacji. Ta aktualizacja nie wiąże się zmianę w bazie danych; zobaczysz, jakie są różnice dotyczące wdrażania w następnym samouczku zmianę w bazie danych.

Przypomnienie: Jeśli otrzymujesz komunikat o błędzie lub coś nie działa podczas wykonywania kroków samouczka, upewnij się sprawdzić [strona rozwiązywania problemów](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="making-a-code-change"></a>Dzięki czemu zmiany w kodzie

Prostym przykładem aktualizacji do aplikacji, należy dodać do **Instruktorzy** listę kursom prowadzonym przez instruktora wybranej strony.

Po uruchomieniu **Instruktorzy** strony, można zauważyć, że istnieją **wybierz** łącza w siatce, ale ich nie robi niczego poza upewnij wiersz szary Włącz w tle.

[![Instructors_page](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image1.png)

Teraz dodasz kod, który jest uruchamiany, gdy **wybierz** łącze kliknięciu i wyświetla listę kursom prowadzonym przez instruktora wybrane.

W *Instructors.aspx*, Dodaj następujący kod bezpośrednio po **ErrorMessageLabel** `Label` sterowania:

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/samples/sample1.aspx)]

Uruchom strony i wybierz pod kierunkiem instruktora. Zobaczysz listę kursom prowadzonym przez instruktora tego.

[![Instructors_page_with_courses](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image3.png)

## <a name="deploying-the-code-update-to-the-test-environment"></a>Wdrażanie aktualizacji kodu do środowiska testowego

Wdrażanie w środowisku testów jest ponownie opublikować polegać na uruchamianie jednym kliknięciem. Aby ułatwić ten proces szybciej, można użyć **Web publikacja w pojedynczym kliknięciem** paska narzędzi.

W **widoku** menu, wybierz **pasków narzędzi** , a następnie wybierz **Web publikacja w pojedynczym kliknięciem**.

![Selecting_One_Click_Publish_toolbar](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image5.png)

W **Eksploratora rozwiązań**, wybierz projekt ContosoUniversity.

**Web publikacja w pojedynczym kliknięciem** narzędzi, wybierz **testu** profil publikowania, a następnie kliknij przycisk **publikowanie w sieci Web** (ikona za pomocą strzałki w lewo i w prawo).

![Web_One_Click_Publish_toolbar](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image6.png)

Program Visual Studio wdroży zaktualizowaną aplikację, i automatycznie otwiera się przeglądarka do strony głównej. Uruchom stronę instruktorów i wybierz pod kierunkiem instruktora, aby sprawdzić, czy aktualizacja została wdrożona pomyślnie.

[![Instructors_page_with_courses_Test](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image7.png)

Normalny również wykonać testowanie regresji (oznacza to, że test pozostałych lokacji, aby upewnić się, że nowa zmiana nie przerwać wszelkie istniejące funkcje). Jednak na potrzeby tego samouczka będziesz pominąć ten krok i przejdź do wdrażania aktualizacji w środowisku produkcyjnym.

## <a name="preventing-redeployment-of-the-initial-database-state-to-production"></a>Uniemożliwia ponowne początkowy stan bazy danych do środowiska produkcyjnego

W rzeczywistej aplikacji użytkownikom interakcję z witryny produkcyjnej po początkowym wdrożeniu i baz danych są wypełniane przy użyciu danych na żywo. W związku z tym nie chcesz ponownie wdrożyć bazy danych członkostwa w stanie początkowym spowoduje usunięcie wszystkich danych na żywo. Ponieważ bazy danych programu SQL Server Compact to pliki w *aplikacji\_danych* folderu, masz tego uniknąć, zmieniając ustawienia wdrożenia, które pliki *aplikacji\_danych* folderu nie są wdrażane.

Otwórz **właściwości projektu** okna ContosoUniversity projektu i wybierz **pakowaniu/publikowaniu Web** kartę. Upewnij się, że **konfiguracji** pole listy rozwijanej utracił **aktywny (wersja)** lub **wersji** wybrana, wybierz **wykluczanie plików z aplikacji\_Folderu danych**.

![Exclude_files_from_the_App_Data_folder](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image9.png)

W przypadku, gdy użytkownik chce wdrożyć kompilację debugowania w przyszłości, to dobry pomysł, aby wprowadzić tę samą zmianę dla konfiguracji kompilacji debugowania: Zmień **konfiguracji** do **debugowania** , a następnie wybierz **wykluczenia pliki z poziomu aplikacji\_folderu danych**.

Zapisz i Zamknij **pakowaniu/publikowaniu Web** kartę.

> [!NOTE] 
> 
> [!IMPORTANT]
> Upewnij się, że nie masz **Usuń dodatkowe pliki w lokalizacji docelowej** wybrane w profilach publikowania. Jeśli wybierzesz tę opcję, baz danych, które mają w aplikacji spowoduje to usunięcie procesu wdrażania\_danych w witrynie wdrożone, a spowoduje usunięcie aplikacji\_sam folder.


## <a name="preventing-user-access-to-the-production-site-during-update"></a>Blokowanie dostępu użytkownika do witryny produkcyjnej podczas aktualizacji

Zmiana, którą jest wdrażany jest teraz prostą zmianę do jednej strony. Jednak czasami wdrażanie większe zmiany i w takim przypadku lokacji, może zachowywać się dziwny Jeśli użytkownik zgłasza żądanie strony przed zakończeniem wdrażania. Aby tego uniknąć, można użyć *aplikacji\_offline.htm* pliku. Po umieszczeniu pliku o nazwie *aplikacji\_offline.htm* w folderze głównym aplikacji, usługi IIS automatycznie wyświetla ten plik, zamiast uruchamiania aplikacji. Tak, aby uniemożliwić dostęp podczas wdrażania, możesz umieścić *aplikacji\_offline.htm* w folderze głównym Uruchom proces wdrażania, a następnie usuń *aplikacji\_offline.htm*.

W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy rozwiązanie (nie jednego z projektów) i wybierz **nowy Folder rozwiązania**.

![Creating_a_solution_folder](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image10.png)

Nazwa folderu *SolutionFiles*.

W nowym folderze utwórz stronę HTML o nazwie *aplikacji\_offline.htm*. Zastąp istniejącą zawartość następującym kodem:

[!code-html[Main](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/samples/sample2.html)]

Możesz skopiować *aplikacji\_offline.htm* plik do witryny za pomocą połączenia FTP lub **Menedżera plików** narzędzie w Panelu sterowania dostawcy hostingu. W tym samouczku użyjesz **Menedżera plików**.

Otwórz panel sterowania i wybierz **Menedżera plików** tak jak w [wdrażanie w środowisku produkcyjnym](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) samouczka. Wybierz **contosouniversity.com** i następnie **wwwroot** uzyskać dostęp do folderu głównego aplikacji, a następnie kliknij przycisk **przekazywanie**.

[![Upload_button_in_File_Manager](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image12.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image11.png)

W **Przekaż plik** okno dialogowe, wybierz opcję *aplikacji\_offline.htm* pliku, a następnie kliknij przycisk **przekazywanie**.

[![Upload_dialog_box_in_File_Manager](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image14.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image13.png)

Przejdź do adresu URL witryny. Zobaczysz, że *aplikacji\_offline.htm* zamiast strony głównej zostanie teraz wyświetlona strona.

[![App_offline.htm_page_in_production](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image15.png)

Teraz można przystąpić do wdrażania w środowisku produkcyjnym.

## <a name="deploying-the-code-update-to-the-production-environment"></a>Wdrażanie aktualizacji kodu do środowiska produkcyjnego

W **Web publikacja w pojedynczym kliknięciem** narzędzi, wybierz **produkcji** profil publikowania, a następnie kliknij przycisk **publikowanie w sieci Web**.

Programu Visual Studio wdroży zaktualizowaną aplikację i otworzy w przeglądarce do strony głównej. *Aplikacji\_offline.htm* pliku jest wyświetlany. Zanim można przetestować, aby zweryfikować pomyślne wdrożenie, musisz usunąć *aplikacji\_offline.htm* pliku.

Wróć do **Menedżera plików** aplikacji w Panelu sterowania. Wybierz **contosouniversity.com** i **wwwroot**, wybierz opcję **aplikacji\_offline.htm**, a następnie kliknij przycisk **Usuń**.

[![Deleting_app_offline.htm](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image17.png)

W przeglądarce otwórz stronę Instruktorzy w publicznej witrynie, a następnie wybierz pod kierunkiem instruktora, aby sprawdzić, czy aktualizacja została wdrożona pomyślnie.

[![Instructors_page_with_courses_Prod](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image19.png)

Teraz udało Ci się wdrożyć aktualizację aplikacji, która nie obejmują zmianę w bazie danych. Następnym samouczku dowiesz się, jak wdrożyć zmianę w bazie danych.

> [!div class="step-by-step"]
> [Poprzednie](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)
> [dalej](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)
