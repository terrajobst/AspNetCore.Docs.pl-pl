---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/publishing
title: Wprowadzenie do składnika ASP.NET Web Pages — publikowanie witryny za pomocą programu WebMatrix | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: Niniejszy samouczek jest ostateczny PAYG w zestawie samouczków, która wprowadza stron ASP.NET Web Pages i programu Microsoft WebMatrix. Omówiono w nim sposób publikowania witryny t...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: 7e85c70e-1a88-4408-8b3d-29611c7713ed
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/publishing
msc.type: authoredcontent
ms.openlocfilehash: fe196e5db8fd1cecbe84b2eb970939303f9313d1
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021459"
---
<a name="introducing-aspnet-web-pages---publishing-a-site-by-using-webmatrix"></a>Wprowadzenie do wzorca ASP.NET Web Pages — publikowanie witryny za pomocą programu WebMatrix
====================
przez [Tom FitzMacken](https://github.com/tfitzmac)

> Niniejszy samouczek jest ostateczny PAYG w zestawie samouczków, która wprowadza stron ASP.NET Web Pages i programu Microsoft WebMatrix. Omówiono w nim sposób publikowania witryny z Internetem, aby inne osoby mogą pracować z nim. Przyjęto założenie, że zostały wykonane serii za pośrednictwem [tworzenia spójnego wyszukiwania do witryn stron sieci Web platformy ASP.NET](https://go.microsoft.com/fwlink/?LinkId=251585).
> 
> Dowiesz się, jak opublikować swoją witrynę przy użyciu:
> 
> - Microsoft Azure
> - Firma hostingu w sieci Web


## <a name="about-publishing-your-site"></a>Publikowanie witryny — informacje

Do chwili obecnej należy wykonać całą pracę na komputerze lokalnym, w tym Testowanie stron internetowych. Aby uruchomić usługi<em>.cshtml</em> stron, użytych serwera sieci web, która jest wbudowana w program WebMatrix, a mianowicie usług IIS Express. Ale oczywiście nie widać witryny, do której został utworzony, chyba że użytkownik. Aby inni pracować z witryny, należy opublikować ją w Internecie.

Jeśli nie masz już dostępu do serwera sieci web publicznych, publikowanie oznacza, że musisz mieć konto w *platforma usług w chmurze* lub *dostawcy usług hostingowych*. Platforma w chmurze, takich jak Microsoft Azure stanowi infrastrukturę na żądanie dla aplikacji. Dostawca hostingu jest firmy, który jest właścicielem serwerów sieci web dostępny publicznie i który będzie wynajmowanie możesz miejsca dla danej witryny. Plany, uruchom go z kilka dolarów miesięcznie (lub nawet bezpłatne) hostingu dla małych witryn do kilkuset dolarów miesięcznie dla dużej liczby komercyjnych witryn sieci Web.

> [!NOTE]
> Możesz mieć dostępu do serwera internetowego publicznej za pośrednictwem dostawcy usług internetowych (ISP), który umożliwia uzyskiwanie usługi internetowe w domu. Jednak Twój dostawca hostingu musi obsługiwać stron ASP.NET Web Pages. Nie wielu usługodawców internetowych, ale warto zawsze sprawdzania.


W tym samouczku przedstawimy omówienie sposobu publikowania. Nie jest praktyczne zapewnienie szczegółowymi informacjami na temat do wszystkiego, ponieważ proces różni się nieco dla każdego dostawcy hostingu. Ale zapewnisz sobie dobrze, jak działa ten proces.

Ten samouczek zawiera cztery sekcje:

1. [Konfigurowanie domyślnej strony](#defaultpage)
2. Publikowanie (wybierz jedną z następujących)  
 a. [Publikowanie witryny w systemie Microsoft Azure](#azure)  
 b. [Publikowanie witryny firmy hostingu w sieci Web](#host)
3. [Aktualizowanie w działającej witrynie: ponowne publikowanie](#update)

<a id="defaultpage"></a>
## <a name="setting-up-the-default-page"></a>Konfigurowanie domyślnej strony

Gdy użytkownik przechodzi do adres podstawowy dla witryny sieci web, do użytkownika jest wyświetlona domyślna strona dla danej witryny. Na przykład, gdy Default.htm jest ustawiona jako domyślna strona dla tej witryny w www.contoso.com, przechodząc do <strong>www.contoso.com</strong> jest taka sama jak przejść do <strong>www.contoso.com/Default.htm</strong>.

Obecnie Twoja witryna wymaga **Default.cshtml** jako domyślnej strony. Ta strona jest w dobrym stanie, domyślnej strony, ale w tym samouczku nie dodano żadnej zawartości do tej strony, będzie wyświetlany w pustej strony. Otwórz Default.cshtml i Zastąp zawartość następującym kodem.

[!code-cshtml[Main](publishing/samples/sample1.cshtml)]

Teraz Twoja witryna jest gotowy do publikacji. Po pierwsze zobaczysz, jak wdrożyć witrynę na platformie Azure, a następnie wdrożyć go w sieci web firma zapewniająca hosting. Albo działa opcja dla witryny sieci web, a wystarczy wykonać jedną z opcji wdrażania.

<a id="azure"></a>
## <a name="publishing-your-site-to-microsoft-azure"></a>Publikowanie witryny w systemie Microsoft Azure

W tym samouczku zostanie najpierw pokazano, jak do wdrożenia witryny w systemie Microsoft Azure. Logując się przy użyciu konta Microsoft, można utworzyć maksymalnie 10 bezpłatnych witryn na platformie Azure. Witryny te bezpłatne zapewniają wygodny sposób testowania lokacje. Zawsze możesz usunąć ten przykład witryny później, aby unikać wszystkich bezpłatnych witryn. Można utworzyć bezpłatne konto próbne w zaledwie kilka minut. Aby uzyskać więcej informacji, zobacz [bezpłatnej wersji próbnej Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

Na wstążce programu WebMatrix kliknij **Publikuj** przycisku.

![Przycisk "Publikuj" na wstążce programu WebMatrix](publishing/_static/image1.png)

**Publikowanie witryny za** zostanie wyświetlone okno dialogowe. Jeśli nie zarejestrowano do Twojego konta Microsoft, okno dialogowe będzie zawierać **Rozpoczynanie pracy z usługą Azure** łącza. Kliknięcie tego łącza.

![Publikowanie witryny](publishing/_static/image2.png)

Jeśli nie masz logowanie do konta Microsoft możesz są ponownie możliwość do logowania. Musisz się zarejestrować się do konta Microsoft do opublikowania witryny na platformie Azure.

![Rejestrowanie](publishing/_static/image3.png)

Po zalogowaniu się do swojego konta Microsoft, okno dialogowe zawiera łącza do tworzenia nowej witryny na platformie Azure lub połącz się z jednym z istniejących witryn na platformie Azure.

![Utwórz nową witrynę](publishing/_static/image4.png)

Wybierz **Utwórz nową witrynę**.

Jeśli nazwa projektu jest **WebPagesMovies**, będzie domyślna nazwa witryny **webpagesmovies.azurewebsites.net**. Ta nazwa domyślna jest najprawdopodobniej nie jest dostępny, wskazane przez czerwony wykrzyknik.

![Domyślna nazwa witryny sieci Web](publishing/_static/image5.png)

Zmień nazwę lokacji coś, który jest dostępny, a następnie wybierz lokalizację, która znajduje się w pobliżu danej lokalizacji.

![Nazwa witryny zmienione](publishing/_static/image6.png)

Kliknij przycisk **OK**.

Program WebMatrix performss test, aby określić, czy serwer jest zgodny z witryny.

![test zgodności](publishing/_static/image7.png)

Wybierz **nadal**.

Wyniki testowania zgodności są wyświetlane.

![wynik zgodności](publishing/_static/image8.png)

Wybierz **nadal**.

Program WebMatrix Wyświetla pliki i bazy danych, które mają zostać opublikowane w witrynie. Ponieważ jest to publikujesz witrynę po raz pierwszy, są wyświetlane wszystkie pliki. Możesz usunąć zaznaczenie pliku, który nie jest gotowa do opublikowania. W kolejnej publikacji będą wyświetlane tylko te pliki, które uległy zmianie. Zobacz [aktualizowania witryny na żywo: ponowne publikowanie](#update).

![Podgląd publikacji](publishing/_static/image9.png)

Wybierz **nadal**.

Po wdrożeniu witryny na platformie Azure, zostanie wyświetlony komunikat, który wskazuje, że wdrożenie zostało zakończone.

![Publikowanie ukończone](publishing/_static/image10.png)

Witryny i bazy danych zostały opublikowane na platformie Azure, a teraz są dostępne publicznie. Kliknij link w komunikacie wskazujący publikowanie zostało zakończone i zostanie wyświetlona witryna wdrożone. Użytkownik lub każda osoba mająca dostęp do Internetu można dodawać lub modyfikować rekordy w bazie danych.

![](publishing/_static/image11.png)

<a id="host"></a>
## <a name="publishing-your-site-to-a-web-hosting-company"></a>Publikowanie witryny firmy hostingu w sieci Web

Jeśli zdecydujesz się nie publikować na platformie Azure, zamiast tego można opublikować witryny firmy hostingu w sieci web.

Kliknij przycisk **znaleźć hostingu internetowego** łącza.

![Przycisk "Znajdź hosting sieci web" w oknie dialogowym Ustawienia publikowania](publishing/_static/image12.png)

Przejdź do strony w witrynie firmy Microsoft, który zawiera listę dostawców hostingu, które obsługują aplikacje ASP.NET.

![Strony w witrynie firmy Microsoft, zawierającego dostawców usług hostingu](publishing/_static/image13.png)

Oczywiście może być trudne, teraz wiedzieć dokładnie funkcjach hostingu mogą być wymagane przez długi czas. Oto kilka rzeczy, które należy wziąć pod uwagę:

- Na potrzeby witryny WebPagesMovies nie trzeba mieć osobne dodatek dla programu SQL Server, które często kosztów dodatkowych. W witrynie usługi używasz programu SQL Server Compact Edition, która jest niezależna. Jednak może być konieczne dostępu do serwera SQL dla niektórych pracy przyszłości witryny sieci Web, co możesz zrobić. Jeśli uważasz, że być może, upewnij się, że później możesz dodać możliwości programu SQL Server.
- Sprawdź, czy dostawca hostingu obsługuje protokół publikowania narzędzia Web Deploy. Można opublikować za pomocą protokołu FTP, ale jest to bardziej wygodne użyć narzędzia Web Deploy.

Niektóre witryny oferuje bezpłatny okres próbny. Bezpłatna wersja próbna jest dobrym sposobem na spróbuj opublikować i hostingu podczas one nadal eksperymentować z programu WebMatrix i stron ASP.NET Web Pages.

Taki, który chcesz pobrać. W tym samouczku wybrano DiscountASP.NET, ponieważ gdy firma Microsoft tworzenia tego samouczka, tej firmy podwyższania poziomu, które umożliwiają użytkownikom hostować lokację w ciągu kilku miesięcy.

> [!NOTE]
> Nasz wybór dostawcy hostingu, w tym samouczku nie powinien interpretowane jako poręczenia tej firmy za pośrednictwem innych. Ale musimy wybrać jeden z nich do celów ilustracyjnych i DiscountASP.NET jest jednym z wielu firm, które obsługuje stron ASP.NET Web Pages i protokołu Web Deploy dla publikowania.


Zazwyczaj po zalogowaniu przy użyciu dostawcy hostingu, przedsiębiorstwo wyśle do Ciebie wiadomość e-mail, który zawiera nazwę użytkownika i hasło, adres URL serwera sieci web i tak dalej. Jeśli firmy hostingowej obsługuje protokołu Web Deploy, może wysyłać należy plik, który zawiera ustawienia publikowania lub umożliwiają pobieranie jeden. Plik ustawień publikowania upraszcza proces.

Jeśli zarejestrowano się i jest gotowe do opublikowania, kliknij przycisk **Publikuj** przycisk na wstążce programu WebMatrix. **Ustawień publikowania** zostanie wyświetlone okno dialogowe.

Jeśli dostawca hostingu przesłany plik ustawień publikowania, kliknij przycisk **importowanie ustawień publikowania** link i zaimportuj plik. Jeśli nie masz plik ustawień publikowania, wypełnij pola za pomocą wartości, które firmy hostingowej wysłać w wiadomości e-mail. Oto, co **ustawień publikowania** okno dialogowe może wyglądać po wykonaniu tych czynności:

![Ustawienia publikowania wprowadzone w oknie dialogowym Ustawienia publikowania](publishing/_static/image14.png)

Kliknij przycisk **sprawdzanie poprawności połączenia**. Wszystko, co jest dobrym, okno dialogowe Raporty **pomyślnie połączono z**, co oznacza, że może komunikować się z serwerem dostawcy hostingu.

![Powodzenie komunikat, jeśli publikowanie ustawienia są poprawne](publishing/_static/image15.png)

W przypadku problemu program WebMatrix wykonuje doskonale informujące o tym, co to jest problem:

![Komunikat o błędzie, jeśli występuje problem z ustawień publikowania](publishing/_static/image16.png)

Kliknij przycisk **Zapisz** można zapisać ustawień. Program WebMatrix udostępnia wykonać test, aby upewnić się, że może komunikować się poprawnie w witrynie hostingu:

![Komunikat, oferty wykonać test proces publikowania](publishing/_static/image17.png)

Kliknij przycisk **Tak**. Program WebMatrix przekazuje niektóre pliki przykładowe do dostawcy hostingu. Po zakończeniu testowania zgodności programu WebMatrix zgłasza wyniki:

![Wyniki testu publikowania](publishing/_static/image18.png)

Jeśli jesteś gotowy, przejdź dalej i kliknij przycisk **Kontynuuj** aby rozpocząć proces publikowania rzeczywistych. Program WebMatrix wpadł na pomysł co pliki znajdują się w witrynie znajdują się już na serwerze hosta (od razu, none) i wyświetlany jest podgląd proces publikowania:

![Jakie przekazujących proces publikowania plików w wersji zapoznawczej](publishing/_static/image19.png)

Lista plików do opublikowania w tym stron sieci web, które zostały utworzone, takich jak *Movies.cshtml*. Lista zawiera również pliki dla wątków, które po zainstalowaniu, pliki, aby uruchomić program SQL Server Compact Edition dla bazy danych i tak dalej. Co w efekcie początkowego publikowania procesu mogą być istotne.

Kliknij przycisk **Kontynuuj**. Program WebMatrix kopiuje pliki do serwera dostawcy hostingu. Gdy wszystko będzie gotowe, wyniki są zgłaszane w pasku stanu:

![Komunikat pasek stanu, gdy proces publikowania zakończy się pomyślnie](publishing/_static/image20.png)

Aby wyświetlić witryny na żywo, kliknij link na pasku stanu. Dodaj *filmy* do adresu URL, a zobaczysz *Movies.cshtml* pliku, który został utworzony:

![Witryna na żywo, przedstawiający stronę filmy](publishing/_static/image21.png)

<a id="update"></a>
## <a name="updating-the-live-site-republishing"></a>Aktualizowanie w działającej witrynie: ponowne publikowanie

Po opublikowaniu lokacji (do platformy Azure lub hostingu firmy), istnieją dwie kopie &mdash; wersji na komputerze i wersji na dostawcę usług. Prawdopodobnie będziesz chciał kontynuować tworzenie lokacji (jeśli niczego, w ramach następnego zestawu samouczków). Po wykonaniu, należy ponownie opublikować witrynę w celu kopiowania zmian z komputera do dostawcy usług. Proces publikowania w programie WebMatrix można określić, jakie pliki zostały zmienione w witrynie i opublikować tylko te pliki.

Aby zobaczyć, jak ponowne publikowanie działa, otwórz *Movies.cshtml* lokacji, wprowadzić zmianę niektórych mała liczba godzin, a następnie zapisz plik. Na przykład zmień tytuł na `Movies - Updated`.

Kliknij przycisk **Publikuj** przycisk na Wstążce. Program WebMatrix Określa, co zostało zmienione i pokazuje podgląd plików, który będzie publikować uprawnienia.

![Okno dialogowe "Publikuj", przedstawiający zmienione pliki gotowe do ponownego publikowania](publishing/_static/image22.png)

> [!IMPORTANT] 
> 
> Domyślnie program WebMatrix publikowanie bazy danych (*.sdf* pliku) tylko po raz pierwszy, należy opublikować witrynę. Po opublikowaniu lokacji i osób wchodzą w interakcje z witryny sieci Web, bazy danych w aktywnej witrynie zwykle zawiera rzeczywiste dane lokacji. Masz bardzo nie podawaj zastąpienia bazy danych na żywo za pomocą *.sdf* pliku, który znajduje się na tym komputerze, który zwykle zawiera tylko dane testowe. Dlatego zapoznaj się z ostrzeżeniem **publikowania spowoduje zastąpienie wszelkich zdalnych baz danych**, i dlaczego pole wyboru dla *WebPagesMovies.sdf* jest domyślnie wyczyszczone.


Kliknij przycisk **Kontynuuj**. Program WebMatrix publikuje zmienionych plików i zostanie wyświetlony komunikat o powodzeniu, tak, jak został opublikowany po raz pierwszy.

Przejdź do aktywnej witryny (można kliknąć link w komunikat o powodzeniu, jeśli jest nadal wyświetlana) i sprawdź, czy zmiany zostały opublikowane.

> [!TIP] 
> 
> **Zdalne edytowanie plików**
> 
> Jako alternatywę do zmiany lokacji i ponowne opublikowanie możesz edytować pliki zdalne bezpośrednio w programie WebMatrix. W tym scenariuszu możesz otworzyć plik, który znajduje się na dostawcę usług, a program WebMatrix pobierze jej kopię do edycji. Za każdym razem, gdy plik zostanie zapisany, program WebMatrix wysyła zmiany do witryny.
> 
> Zdalnie edytować to prosty sposób, aby wprowadzić zmiany w działającej witryny. Jednak zmiany wprowadzone w ten sposób nie są zsynchronizowane z plikami w lokacji lokalnej. Aby zsynchronizować pliki lokalne z lokacji zdalnej, możesz pobrać pliki zdalne. Ten proces działa podobnie do publikowania, z wyjątkiem w odwrotnej kolejności.
> 
> Firma Microsoft nie będzie Opisz bardziej edycji zdalnego i zdalnego pobierania urządzeń programu WebMatrix w tym miejscu. Są one bardzo przydatne, gdy wiele osób ponosi do pracy w tej samej lokacji, na różnych komputerach. Aby uzyskać więcej informacji, zobacz [publikowania i edytować lokacji zdalnej za pomocą programu WebMatrix 2 Beta](https://go.microsoft.com/fwlink/?LinkId=251591).


## <a name="additional-resources"></a>Dodatkowe zasoby

- [Forum programu WebMatrix składnika ASP.NET Web Pages platformy ASP.NET](https://forums.asp.net/1224.aspx/1?WebMatrix+and+ASP+NET+Web+Pages)doskonałym miejscem do zadawania pytań i odpowiedzi.

> [!div class="step-by-step"]
> [Poprzednie](layouts.md)
