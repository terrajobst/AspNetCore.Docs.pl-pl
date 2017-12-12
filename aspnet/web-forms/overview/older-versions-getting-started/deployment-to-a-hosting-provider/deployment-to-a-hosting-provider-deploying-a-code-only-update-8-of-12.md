---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12
title: "Wdrażanie aplikacji sieci Web ASP.NET przy użyciu programu Visual Studio lub Visual Web Developer programu SQL Server Compact: Wdrażanie aktualizacji Code-Only — 8 12 | Dokumentacja firmy Microsoft"
author: tdykstra
description: "Tej serii samouczków opisano sposób wdrażania platformy ASP.NET (publikowanie) projektu aplikacji sieci web, który zawiera bazę danych programu SQL Server Compact przy użyciu Visual Stu..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: ddf6252f-9413-4c0c-a360-2cef8d231717
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12
msc.type: authoredcontent
ms.openlocfilehash: a07ac968482866ba9a4eca25722d99fd641080e6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-code-only-update---8-of-12"></a>Wdrażanie aplikacji sieci Web ASP.NET przy użyciu programu Visual Studio lub Visual Web Developer programu SQL Server Compact: Wdrażanie aktualizacji Code-Only — 8 12
====================
przez [Dykstra niestandardowy](https://github.com/tdykstra)

[Pobierz początkowego projektu](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Tej serii samouczków opisano sposób wdrażania platformy ASP.NET (publikowanie) projektu aplikacji sieci web, który zawiera bazę danych programu SQL Server Compact przy użyciu programu Visual Studio 2012 RC i Visual Studio Express 2012 RC for Web. W razie musisz zainstalować aktualizację publikowania w sieci Web, można również używać programu Visual Studio 2010. Aby obejrzeć wprowadzenie do serii, zobacz [pierwszy samouczek z tej serii](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Samouczek zawiera funkcje wdrażania dodane po wydaniu programu Visual Studio 2012 RC, pokazuje, jak wdrożyć wersjach programu SQL Server innych niż SQL Server Compact, która przedstawia sposób wdrażania aplikacji sieci Web usługi aplikacji Azure, zobacz [wdrożenia sieci Web ASP.NET za pomocą programu Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Omówienie

Po początkowym wdrożeniu kontynuuje pracę utrzymanie i opracowanie witryny sieci web, a niedługo należy wdrożyć aktualizację. Ten samouczek przeprowadza użytkownika przez proces wdrażania aktualizacji w kodzie aplikacji. Ta aktualizacja nie wymaga zmiany bazy danych; zobaczysz, co różni się o wdrażaniu w następnym samouczku zmianę w bazie danych.

Przypomnienie: Jeśli coś nie działa podczas wykonywania kroków samouczka wyświetlony komunikat o błędzie, należy sprawdzić [Rozwiązywanie problemów z strony](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="making-a-code-change"></a>Wprowadzenie zmiany kodu

Jako prosty przykład aktualizacji do aplikacji, warto **instruktorów** listę kursów nauczanych przez instruktora wybranej strony.

Po uruchomieniu **instruktorów** strony, można zauważyć, że istnieją **wybierz** łącza w siatce, ale nie zrobią nic innego niż upewnij szary Włącz tła wiersza.

[![Instructors_page](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image1.png)

Teraz dodasz kod wykonywany kiedy **wybierz** łącza zostanie kliknięty i wyświetla listę kursów nauczanych przy wybranym instruktorze.

W *Instructors.aspx*, Dodaj następujący kod bezpośrednio po **ErrorMessageLabel** `Label` sterowania:

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/samples/sample1.aspx)]

Uruchom strony i wybierz instruktora. Zostanie wyświetlona lista kursów nauczanych przy tym instruktora.

[![Instructors_page_with_courses](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image3.png)

## <a name="deploying-the-code-update-to-the-test-environment"></a>Wdrażanie aktualizacji kod do środowiska testowego

Wdrażanie w środowisku testowym jest polegać na jednym kliknięciem uruchomiona ponownie opublikować. Aby ułatwić ten proces szybsze, można użyć **jeden kliknij publikowania w sieci Web** paska narzędzi.

W **widoku** menu, wybierz **paski narzędzi** , a następnie wybierz **jeden kliknij publikowania w sieci Web**.

![Selecting_One_Click_Publish_toolbar](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image5.png)

W **Eksploratora rozwiązań**, wybierz projekt ContosoUniversity.

**jeden kliknij publikowania w sieci Web** narzędzi wybierz **testu** profil publikowania, a następnie kliknij przycisk **publikowanie w sieci Web** (ikona z strzałki w lewo i w prawo).

![Web_One_Click_Publish_toolbar](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image6.png)

Program Visual Studio wdroży zaktualizowaną aplikację i przeglądarka automatycznie otwiera do strony głównej. Uruchom instruktorów strony i wybierz instruktora, aby zweryfikować, że aktualizacja została pomyślnie wdrożona.

[![Instructors_page_with_courses_Test](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image7.png)

Normalny również wykonać testów regresyjnych (to znaczy test reszty lokacji, aby upewnić się, że nowe zmiany nie Przerwij wszystkie istniejące funkcje). Jednak w tym samouczku będzie pominąć ten krok i przejdź do wdrożenia aktualizacji w środowisku produkcyjnym.

## <a name="preventing-redeployment-of-the-initial-database-state-to-production"></a>Uniemożliwia ponowne wdrożenie początkowy stan bazy danych do środowiska produkcyjnego

W rzeczywistej aplikacji użytkownicy korzystają z witryny produkcji po początkowym wdrożeniu i baz danych są wypełniane przy użyciu bieżących danych. W związku z tym nie chcesz ponownie wdrożyć bazy danych członkostwa w stanie początkowym spowoduje usunięcie wszystkich danych na żywo. Ponieważ bazy danych programu SQL Server Compact to pliki w *aplikacji\_danych* folderu, masz tego uniknąć, zmieniając ustawienia wdrożenia, w którym pliki *aplikacji\_danych* folderu nie są wdrażane.

Otwórz **właściwości projektu** ContosoUniversity projektu i wybierz w oknie **pakowaniu/publikowaniu Web** kartę. Upewnij się, że **konfiguracji** ma pole listy rozwijanej, albo **aktywny (wersja)** lub **wersji** wybrana, wybierz **Wyklucz pliki z aplikacji\_Folderu danych**.

![Exclude_files_from_the_App_Data_folder](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image9.png)

Jeśli zdecydujesz się wdrożyć kompilację debugowania w przyszłości, to warto wprowadzić dla konfiguracji kompilacji debugowania: Zmień **konfiguracji** do **debugowania** , a następnie wybierz **wykluczenia pliki z aplikacji\_folderu danych**.

Zapisz i Zamknij **pakowaniu/publikowaniu Web** kartę.

> [!NOTE] 
> 
> [!IMPORTANT]
> Upewnij się, że nie masz **Usuń dodatkowe pliki w miejscu docelowym** wybrane do profilu publikowania. Jeśli wybierzesz tę opcję, proces wdrażania spowoduje usunięcie bazy danych, które ma aplikacja\_danych w lokacji wdrożone, a spowoduje usunięcie aplikacji\_sam folder.


## <a name="preventing-user-access-to-the-production-site-during-update"></a>Zapobieganie dostępowi użytkownika do miejsca produkcji podczas aktualizacji

Zmiana, którą jest wdrażany jest teraz prosta zmiana na jednej stronie. Ale czasami wdrażanie większych zmian, a w takim przypadku lokacji, może zachowywać się dziwny Jeśli użytkownik zażąda strony przed zakończeniem wdrażania. Aby tego uniknąć, można użyć *aplikacji\_offline.htm* pliku. Jeśli możesz umieścić plik o nazwie *aplikacji\_offline.htm* w folderze głównym aplikacji, usługi IIS automatycznie wyświetla ten plik zamiast uruchamiania aplikacji. Tak, aby uniemożliwić dostęp podczas wdrażania, możesz zaznaczyć *aplikacji\_offline.htm* w folderze głównym Uruchom proces wdrażania, a następnie usuń *aplikacji\_offline.htm*.

W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy rozwiązanie (nie jeden z projektów) i wybierz **nowy Folder rozwiązania**.

![Creating_a_solution_folder](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image10.png)

Nazwa folderu *SolutionFiles*.

W nowym folderze utwórz stronę HTML o nazwie *aplikacji\_offline.htm*. Zastąp istniejącą zawartość następujący kod:

[!code-html[Main](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/samples/sample2.html)]

Możesz skopiować *aplikacji\_offline.htm* pliku do lokacji przy użyciu połączenia FTP lub **Menedżera plików** narzędzia w Panelu sterowania dostawcy hostingu. W tym samouczku użyjesz **Menedżera plików**.

Otwórz panel sterowania i wybierz **Menedżera plików** tak samo jak [wdrażania w środowisku produkcyjnym](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) samouczka. Wybierz **contosouniversity.com** , a następnie **wwwroot** uzyskać dostęp do folderu głównego aplikacji, a następnie kliknij przycisk **przekazać**.

[![Upload_button_in_File_Manager](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image12.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image11.png)

W **Przekaż plik** okno dialogowe, wybierz opcję *aplikacji\_offline.htm* pliku, a następnie kliknij przycisk **przekazać**.

[![Upload_dialog_box_in_File_Manager](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image14.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image13.png)

Przejdź do adresu URL witryny. Zostanie wyświetlony *aplikacji\_offline.htm* teraz zostanie wyświetlona strona zamiast strony głównej.

[![App_offline.htm_page_in_production](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image15.png)

Teraz można przystąpić do wdrażania w środowisku produkcyjnym.

## <a name="deploying-the-code-update-to-the-production-environment"></a>Wdrażanie aktualizacji kod do środowiska produkcyjnego

W **jeden kliknij publikowania w sieci Web** narzędzi wybierz **produkcji** profil publikowania, a następnie kliknij przycisk **publikowanie w sieci Web**.

Visual Studio wdraża zaktualizowaną aplikację i otwiera przeglądarkę do strony głównej. *Aplikacji\_offline.htm* plik jest wyświetlany. Aby przeprowadzić test, aby zweryfikować pomyślne wdrożenie, należy usunąć *aplikacji\_offline.htm* pliku.

Wróć do **Menedżera plików** aplikacji w Panelu sterowania. Wybierz **contosouniversity.com** i **wwwroot**, wybierz pozycję **aplikacji\_offline.htm**, a następnie kliknij przycisk **usunąć**.

[![Deleting_app_offline.htm](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image17.png)

W przeglądarce otwórz stronę instruktorów w publicznej witryny i wybierz instruktora, aby zweryfikować, że aktualizacja została pomyślnie wdrożona.

[![Instructors_page_with_courses_Prod](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image19.png)

Teraz wdrożeniu aktualizacji aplikacji, która nie obejmować zmianę w bazie danych. Następny samouczek pokazuje, jak wdrożyć zmianę w bazie danych.

>[!div class="step-by-step"]
[Poprzednie](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)
[dalej](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)
