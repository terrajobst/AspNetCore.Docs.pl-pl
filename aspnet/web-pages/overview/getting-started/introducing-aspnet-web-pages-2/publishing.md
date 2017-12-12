---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/publishing
title: "Wprowadzenie do składnika ASP.NET Web Pages — publikowania lokacji za pomocą programu WebMatrix | Dokumentacja firmy Microsoft"
author: tfitzmac
description: "W tym samouczku jest ostatnim rat w zestawie Samouczek wprowadzający stron ASP.NET Web Pages i programu Microsoft WebMatrix. Zawarto informacje, jak opublikować witrynę t..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: 7e85c70e-1a88-4408-8b3d-29611c7713ed
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/publishing
msc.type: authoredcontent
ms.openlocfilehash: 1e718c92a2f94df50fcf7af6859917746a4982ac
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="introducing-aspnet-web-pages---publishing-a-site-by-using-webmatrix"></a>Wprowadzenie do strony sieci Web ASP.NET - publikowania lokacji za pomocą programu WebMatrix
====================
przez [FitzMacken niestandardowy](https://github.com/tfitzmac)

> W tym samouczku jest ostatnim rat w zestawie Samouczek wprowadzający stron ASP.NET Web Pages i programu Microsoft WebMatrix. Zawarto informacje, jak opublikowanie witryny z Internetem, tak aby inne osoby mogą pracować z nim. Przyjęto założenie, że zostały wykonane serii za pomocą [tworzenie spójny wygląd witryn stron sieci Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=251585).
> 
> Dowiesz się, jak opublikować swoją witrynę przy użyciu:
> 
> - Microsoft Azure
> - Firmy hostingu sieci Web


## <a name="about-publishing-your-site"></a>Publikowanie witryny — informacje

Do chwili wykonaniu całą pracę na komputerze lokalnym, łącznie z testowaniem stron. Do uruchomienia programu*.cshtml* stron, użytych serwera sieci web, wbudowany w program WebMatrix, to znaczy usług IIS Express. Jednak oczywiście nie zobaczyć, witryny, do której został utworzony, chyba że użytkownik. Aby inni pracować z witryny, należy opublikować go w Internecie.

Jeśli nie masz już dostępu do serwera sieci web publiczne, publikowanie oznacza, że konto z *platformy w chmurze* lub *dostawcy hostingu*. Platformy w chmurze, takich jak Microsoft Azure oferuje infrastrukturę na żądanie do aplikacji. Dostawca usług hostingowych jest firmy, który jest właścicielem serwerów sieci web publicznie i który będzie można wynajmować miejsce dla witryny. Hosting planów wykonywania z kilku kwoty miesięcznie (lub nawet wolnego) dla małych witryn do wielu tysięcy dolarów miesięcznie dla dużych komercyjnych witryn sieci Web.

> [!NOTE]
> Może mieć dostęp do serwera sieci web publicznych za pośrednictwem usługodawcy internetowego (ISP) używanego do pobrania w domu usługi internet. Jednak Twój dostawca hostingu musi obsługiwać stron ASP.NET Web Pages. Nie wielu usługodawców internetowych, ale warto zawsze sprawdzania.


W tym samouczku przedstawimy omówienie sposobu publikowania. Nie jest praktyczne podanie szczegółowymi wszystko, ponieważ proces różni się nieco dla każdego dostawcy hostingu. Ale otrzymasz dobrze działania procesu.

Ten samouczek zawiera cztery sekcje:

1. [Konfigurowanie domyślnej strony](#defaultpage)
2. Publikowanie (wybierz jedną z następujących)  
 a. [Publikowanie witryny Microsoft Azure](#azure)  
 b. [Publikowanie witryny firmy hostingu w sieci Web](#host)
3. [Aktualizowanie na żywo witryny: ponowne publikowanie](#update)

<a id="defaultpage"></a>
## <a name="setting-up-the-default-page"></a>Konfigurowanie domyślnej strony

Gdy użytkownik przechodzi do podstawowego adresu witryny sieci web, domyślnej strony w witrynie jest wyświetlany użytkownikowi. Na przykład gdy Default.htm jest ustawiona jako stronę domyślną witryny w www.contoso.com, następnie przejść do obszaru **www.contoso.com** jest taka sama jak przejść do obszaru **www.contoso.com/Default.htm**.

Obecnie Twoja witryna wymaga **Default.cshtml** jako domyślnej strony. Ta strona jest poprawnie domyślnej strony, ale w tym samouczku nie dodano żadnej zawartości do tej strony, będzie wyświetlany w pustej strony. Otwórz Default.cshtml i Zastąp zawartość następującym kodem.

[!code-cshtml[Main](publishing/samples/sample1.cshtml)]

Teraz ta lokacja jest gotowa do opublikowania. Po pierwsze zobaczysz, jak wdrożyć witrynę Azure, a następnie wdrożyć go hostingu firmy w sieci web. Każda opcja działa dla witryny sieci web, a tylko konieczne wykonaj jedną z opcji wdrażania.

<a id="azure"></a>
## <a name="publishing-your-site-to-microsoft-azure"></a>Publikowanie witryny Microsoft Azure

W tym samouczku najpierw opisano sposób wdrażania witryny Microsoft Azure. Logując się za pomocą konta Microsoft, można utworzyć maksymalnie 10 bezpłatnych witryn na platformie Azure. Witryny wolnego te zapewniają wygodny sposób testowania witryny. Ten przykład witryny później, aby uniknąć używania wszystkich witryn wolnego zawsze można usunąć. Można utworzyć bezpłatne konto próbne w zaledwie kilka minut. Aby uzyskać więcej informacji, zobacz [bezpłatnej wersji próbnej Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

Na wstążce programu WebMatrix, kliknij przycisk **publikowania** przycisku.

![Przycisk "Publikuj" wstążce programu WebMatrix](publishing/_static/image1.png)

**Publikowania witryny** zostanie wyświetlone okno dialogowe. Jeśli użytkownik nie ma zalogowany do konta Microsoft, okno dialogowe będzie zawierać **Rozpoczynanie pracy z platformą Azure** łącza. Kliknięcie tego łącza.

![Publikowanie witryny](publishing/_static/image2.png)

Jeśli użytkownik nie ma zalogowany do konta Microsoft, podane są ponownie podpisać. Możesz zalogować się do konta Microsoft do publikowania witryny na platformie Azure.

![Rejestrowanie](publishing/_static/image3.png)

Po zalogowaniu się do swojego konta Microsoft, okno dialogowe zawiera łącza do tworzenia nowej witryny na platformie Azure lub połączyć się z jedną z istniejących witryn na platformie Azure.

![Utwórz nową witrynę](publishing/_static/image4.png)

Wybierz **Utwórz nową witrynę**.

Jeśli nazwa projektu jest **WebPagesMovies**, będzie domyślna nazwa witryny **webpagesmovies.azurewebsites.net**. Ta nazwa domyślna to najprawdopodobniej nie jest dostępny, wskazywany przez czerwony wykrzyknik.

![Domyślna nazwa witryny sieci Web](publishing/_static/image5.png)

Zmień nazwę lokacji do zasobu, który jest dostępny i wybierz lokalizację, która znajduje się w pobliżu lokalizacji.

![Nazwa witryny zmienione](publishing/_static/image6.png)

Kliknij przycisk **OK**.

Program WebMatrix performss test, aby określić, czy serwer jest zgodny z witryną.

![test zgodności](publishing/_static/image7.png)

Wybierz **kontynuować**.

Wyniki testowania zgodności są wyświetlane.

![wynik zgodności](publishing/_static/image8.png)

Wybierz **kontynuować**.

Program WebMatrix Wyświetla plików i baz danych, które zostaną opublikowane w witrynie. Ponieważ jest to w przypadku publikowania lokacji po raz pierwszy, wszystkie pliki są wyświetlane. Można usunąć zaznaczenie pola wyboru pliku, który nie jest gotowy do opublikowania. W kolejnych publikacji będą wyświetlane tylko te pliki, które zostały zmienione. Zobacz [aktualizowanie na żywo witryny: ponowne publikowanie](#update).

![wyświetlić podglądu publikowania](publishing/_static/image9.png)

Wybierz **kontynuować**.

Po wdrożeniu witryny na platformie Azure, zostanie wyświetlony komunikat, który wskazuje, że wdrożenie zostało ukończone.

![Publikowanie ukończone](publishing/_static/image10.png)

Z lokacji i bazą danych została opublikowana na platformie Azure i są teraz dostępne do publicznego. Kliknij link w komunikacie wskazujący publikowania zostało ukończone i znajduje się teraz wdrożonej witryny. Ani osób z dostępem do Internetu, można dodać lub zmodyfikować rekordy w bazie danych.

![](publishing/_static/image11.png)

<a id="host"></a>
## <a name="publishing-your-site-to-a-web-hosting-company"></a>Publikowanie witryny firmy hostingu w sieci Web

Jeśli zdecydujesz się nie publikowanie na platformie Azure, zamiast tego można opublikować witryny firmy hostingu w sieci web.

Kliknij przycisk **znaleźć usługi hostingu sieci web** łącza.

![Przycisk "Znajdź hostingu sieci web" w oknie Ustawienia publikowania](publishing/_static/image12.png)

Przejdź do strony w witrynie firmy Microsoft, który zawiera listę dostawców hostingu, które obsługują aplikacje ASP.NET.

![W witrynie firmy Microsoft, który wyświetla listę dostawców usług hostingu](publishing/_static/image13.png)

Oczywiście może być trudne teraz wiedzieć dokładnie funkcje hostingu mogą wymagać w dłuższym okresie. Oto kilka rzeczy, które należy wziąć pod uwagę:

- Do celów WebPagesMovies lokacji nie trzeba mieć osobne dodatek dla programu SQL Server, który często koszty dodatkowe. W witrynie używasz programu SQL Server Compact Edition, która jest niezależna. Jednak może być konieczne dostęp do serwera SQL dla niektórych pracy przyszłych witryny sieci Web, które należy wykonać. Jeśli uważasz, że możesz, upewnij się, że możliwości programu SQL Server można dodać później.
- Sprawdź, czy dostawca hostingu obsługuje protokół publikowania narzędzia Web Deploy. Można opublikować za pomocą protokołu FTP, ale jest bardziej wygodne użycie narzędzia Web Deploy.

Niektóre witryny oferują bezpłatny okres próbny. Bezpłatna wersja próbna jest dobrym sposobem spróbuj publikowania i hosting czasie jest nadal eksperymentowanie z programu WebMatrix i stron ASP.NET Web Pages.

Klucz, który chcesz wybrać. W tym samouczku wybrano DiscountASP.NET, ponieważ podczas możemy zostały tworzenia tego samouczka, przedsiębiorstwo podwyższania poziomu, które umożliwiają użytkownikom hosta lokację przez kilka miesięcy.

> [!NOTE]
> Nasz wybór dostawcy hostingu, w tym samouczku nie powinny być rozumiane jako poręczenia tej firmy za pośrednictwem innych. Ale było konieczne pobranie jednej ilustracyjną i DiscountASP.NET jest jednym z wielu firm, które obsługuje stron sieci Web ASP.NET i protokołu Web Deploy do opublikowania.


Zazwyczaj po zarejestrowaniu się z dostawcą hostingu, przedsiębiorstwo wysyła wiadomość e-mail, która zawiera nazwę użytkownika i hasło, adres URL serwera sieci web i tak dalej. Jeśli firma obsługuje protokołu Web Deploy, może wysyłać należy plik, który zawiera ustawienia publikowania lub pozwalają na pobranie jednej. Plik ustawień publikowania upraszcza proces dla Ciebie.

Gdy jest zapisany i jest gotowy do opublikowania, kliknij przycisk **publikowania** przycisk na wstążce programu WebMatrix. **Ustawień publikowania** zostanie wyświetlone okno dialogowe.

Jeśli dostawca hostingu wysyłane plik ustawień publikowania, kliknij przycisk **importowanie ustawień publikowania** link i zaimportuj plik. Jeśli nie masz plik ustawień publikowania, wypełnij pola przy użyciu wartości, które hostingu firmy wysyłane w wiadomościach e-mail. Oto, co **ustawień publikowania** okno dialogowe może wyglądać po zakończeniu:

![Ustawienia publikowania wprowadzone w oknie dialogowym Ustawienia publikowania](publishing/_static/image14.png)

Kliknij przycisk **Weryfikacja połączenia z**. Jeśli wszystko jest prawidłowy, okno dialogowe Raporty **Połączono pomyślnie**, co oznacza, że może komunikować się z serwerem dostawcy hostingu.

![Powodzenie komunikatów, jeśli publikowanie ustawienia są prawidłowe](publishing/_static/image15.png)

W przypadku problemu program WebMatrix zapewnia największą informujące o tym, co to jest problem:

![Komunikat o błędzie w przypadku problemu z ustawienia publikowania](publishing/_static/image16.png)

Kliknij przycisk **zapisać** Aby zapisać ustawienia. Program WebMatrix udostępnia wykonać test, aby upewnić się, że może komunikować się poprawnie w witrynie hostingu:

![Komunikat oferty wykonać test proces publikowania](publishing/_static/image17.png)

Kliknij przycisk **Tak**. Program WebMatrix przekazywać niektóre przykładowe pliki do dostawcy hostingu. Po zakończeniu testowania zgodności programu WebMatrix raportuje wyniki:

![Wyniki testu publikowania](publishing/_static/image18.png)

Jeśli wszystko jest gotowe, przejdź dalej i kliknij przycisk **Kontynuuj** rzeczywistym uruchomić proces publikowania. Program WebMatrix danych liczbowych co pliki znajdują się w witrynie i znajdują się już na serwerze hosta (od razu, none) i Podgląd proces publikowania:

![Jakie pliki, które przekaże proces publikowania w wersji zapoznawczej](publishing/_static/image19.png)

Lista plików do opublikowania zawiera stron sieci web, które po utworzeniu, takich jak *Movies.cshtml*. Lista zawiera także pliki dla wątków, które po zainstalowaniu, pliki, aby uruchomić bazy danych programu SQL Server Compact Edition i tak dalej. W związku z tym początkowej publikowania procesu może być istotne.

Kliknij przycisk **Kontynuuj**. Program WebMatrix kopiuje pliki na serwer dostawcy hostingu. Po zakończeniu, wyniki są zgłaszane w pasku stanu:

![Komunikat paska stanu, gdy proces publikowania zakończy się pomyślnie](publishing/_static/image20.png)

Aby wyświetlić witryny na żywo, kliknij łącze w pasku stanu. Dodaj *filmy* do adresu URL, i zobaczysz *Movies.cshtml* utworzony plik:

![Działającą witrynę przedstawiający stronę filmy](publishing/_static/image21.png)

<a id="update"></a>
## <a name="updating-the-live-site-republishing"></a>Aktualizowanie na żywo witryny: ponowne publikowanie

Po opublikowaniu witryny (do platformy Azure lub hostingu sieci web), są dwie kopie &mdash; wersja na komputerze oraz wersja dostawcy usługi. Prawdopodobnie należy kontynuować tworzenie lokacji (jeśli nic, jako część zestawu samouczek dalej). Po wykonaniu czynności należy ponownie opublikować witrynę, aby umożliwić kopiowanie zmian z komputera z dostawcą usług. Proces publikowania w programie WebMatrix można określić, co pliki zostały zmienione w witrynie i publikowanie tylko tych plików.

Aby zobaczyć, jak działa ponownego publikowania, otwórz *Movies.cshtml* lokacji, niektóre zmiany małe, a następnie zapisz plik. Na przykład zmień tytuł, aby `Movies - Updated`.

Kliknij przycisk **publikowania** przycisk na Wstążce. Program WebMatrix Określa, co zostało zmienione i pokazuje podgląd plików, które będą go opublikować.

![Okno dialogowe "Publikuj" przedstawiający zmienione pliki gotowe do ponownego publikowania](publishing/_static/image22.png)

> [!IMPORTANT] 
> 
> Domyślnie program WebMatrix publikowanie bazy danych (*sdf* pliku) tylko po raz pierwszy, należy opublikować witrynę. Po opublikowaniu lokacji i osób pracuje z witryny sieci Web, bazy danych lokacji na żywo zwykle ma prawdziwe dane z lokacji. Masz bardzo nie można więc zastąpienia bazy danych na żywo z *sdf* pliku znajdującego się na tym komputerze, który zwykle zawiera tylko dane testowe. Dlatego zostanie wyświetlone ostrzeżenie **publikowania spowoduje zastąpienie wszelkich zdalnymi bazami danych**, oraz Dlaczego pole wyboru dla *WebPagesMovies.sdf* jest domyślnie wyczyszczone.


Kliknij przycisk **Kontynuuj**. Program WebMatrix publikuje zmienione pliki i zawiera komunikat z potwierdzeniem, tak, jak został opublikowany po raz pierwszy.

Przejdź do witryny na żywo (można kliknąć link w komunikacie Powodzenie Jeśli nadal jest wyświetlany) i upewnij się, że ta zmiana została opublikowana.

> [!TIP] 
> 
> **Zdalne edytowanie plików**
> 
> Alternatywą wobec zmiana witryny i ponowne opublikowanie można edytować pliki zdalne bezpośrednio w programie WebMatrix. W tym scenariuszu Otwórz plik, który znajduje się w dostawcy usług, a program WebMatrix pobierze kopii do edycji. Za każdym razem, gdy zapiszesz plik programu WebMatrix przesyła zmiany do lokacji.
> 
> Zdalne edytowanie jest prosty sposób, aby wprowadzić zmiany dotyczące witryny na żywo. Jednak zmiany wprowadzone w ten sposób nie są zsynchronizowane z plikami w lokalnej witrynie. Aby zsynchronizować lokalnych plików z lokacji zdalnej, możesz pobrać pliki zdalne. Ten proces działa podobne jak w przypadku publikowania, z wyjątkiem odwrotnie.
> 
> Nie możemy opisano więcej o edycji zdalnego i zdalnego pobierania funkcji programu WebMatrix w tym miejscu. Są one bardzo przydatne, jeśli wiele osób muszą pracować na tym samym miejscu na różnych komputerach. Aby uzyskać więcej informacji, zobacz [publikowania i edytować lokacją zdalną z wersji Beta 2 programu WebMatrix](https://go.microsoft.com/fwlink/?LinkId=251591).


## <a name="additional-resources"></a>Dodatkowe zasoby

- [Forum programu WebMatrix ASP.NET Web Pages platformy ASP.NET](https://forums.asp.net/1224.aspx/1?WebMatrix+and+ASP+NET+Web+Pages), doskonałym miejscem do zadawania pytań i odpowiedzi.

>[!div class="step-by-step"]
[Poprzednie](layouts.md)
