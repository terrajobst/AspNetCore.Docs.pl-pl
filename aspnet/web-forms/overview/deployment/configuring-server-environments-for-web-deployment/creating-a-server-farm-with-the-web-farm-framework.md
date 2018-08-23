---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/creating-a-server-farm-with-the-web-farm-framework
title: Tworzenie farmy serwerów za pomocą rozwiązania Web Farm Framework | Dokumentacja firmy Microsoft
author: jrjlee
description: W tym temacie opisano, jak utworzyć i skonfigurować farmę serwerów sieci web z kolekcji serwerów za pomocą sieci Web farmy Framework (WFF) w wersji 2.0.
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 656dd06d-806c-467c-863d-9fc45e5ba3ab
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/creating-a-server-farm-with-the-web-farm-framework
msc.type: authoredcontent
ms.openlocfilehash: b650a05a22f18ffdcc114a9a64054dd0a34bc041
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753732"
---
<a name="creating-a-server-farm-with-the-web-farm-framework"></a>Tworzenie farmy serwerów za pomocą rozwiązania Web Farm Framework
====================
przez [Jason Lee](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie opisano, jak utworzyć i skonfigurować farmę serwerów sieci web z kolekcji serwerów za pomocą sieci Web farmy Framework (WFF) w wersji 2.0.


WFF umożliwia synchronizowanie produktów platformy sieci web i komponentów, aplikacji sieci web, witryn sieci Web i ustawienia konfiguracji wielu serwerów sieci web z równoważeniem obciążenia. W scenariuszach, gdzie potrzebujesz więcej niż jeden serwer sieci web, np. środowiskach przejściowych i produkcyjnych to znacznie upraszczają proces wdrażania i konfiguracji. Możesz wdrożyć aplikację sieci web, na jednym serwerze&#x2014; *serwera podstawowego*&#x2014;i WFF będą automatycznie replikowane tej aplikacji sieci web na wszystkich pozostałych serwerach sieci web w farmie serwerów.

## <a name="understanding-the-web-farm-framework"></a>Opis rozwiązania Web Farm Framework

WFF 2.0 można użyć do aprowizacji, zarządzania i wdrażania zawartości do grupy serwerów sieci web. Wdrożenie WFF składa się z trzech ról serwera klucza:

- *Serwera kontrolera*. Ten serwer jest używany do tworzenia i konfigurowania farmy serwerów WFF. Serwer controller zarządza synchronizacji składników platformy sieci web, ustawienia konfiguracji i aplikacji między serwerami sieci web w farmie serwerów. Zainstaluj WFF 2.0 na serwerze kontrolera, a serwer kontrolera z kolei zainstaluje agenta WFF na wszystkich serwerach w farmie serwerów. Serwer kontrolera nie koncepcyjnie należy do żadnej farmy serwerów WFF, a serwer pojedynczy kontroler umożliwia zarządzanie wieloma farm serwerów. W tym scenariuszu umożliwia pojedynczego serwera Kontroler WFF tworzenie i zarządzanie tymczasowej farmy serwerów i farmy serwerów produkcyjnych.
- *Serwera podstawowego*. Farma serwerów każdego WFF zawiera pojedynczy serwer podstawowy. Podczas instalowania składników platformy sieci web lub wdrożyć aplikacje na serwerze podstawowym, WFF synchronizuje zmiany na inne serwery w farmie serwerów.
- *Serwer pomocniczy*. Farma serwerów każdego WFF zawiera jeden lub więcej serwerów pomocniczych. Wszelkie zmiany wprowadzone do serwera podstawowego są replikowane do każdego serwera pomocniczego w farmie serwerów.

To pokazuje, jak te role serwera odnoszą się do firmy Fabrikam, Inc. środowiskach przejściowych i produkcyjnych:

![](creating-a-server-farm-with-the-web-farm-framework/_static/image1.png)

W tym scenariuszu środowisko przejściowe i środowisko produkcyjne są konfigurowane jako farmy serwerów WFF. Pojedynczy serwer kontroler WFF zarządza zarówno farmy. W ramach każdej farmy serwerów wszelkie zmiany podstawowego serwera są replikowane do każdego serwera pomocniczego.

Przed rozpoczęciem konfigurowania środowiska przejściowego i produkcji, zaleca się, przeczytaj następujące artykuły, aby zapoznać się z najważniejsze pojęcia związane z WFF 2.0:

- [Omówienie rozwiązania Web Farm Framework w wersji 2.0 dla usług IIS 7](https://go.microsoft.com/?linkid=9805126)
- [Konfigurowanie farmy serwerów sieci Web farmy Framework 2.0 dla usług IIS 7](https://go.microsoft.com/?linkid=9805127)
- [System i wymagania dotyczące platformy dla rozwiązania Web Farm Framework w wersji 2.0 dla usług IIS 7](https://go.microsoft.com/?linkid=9805128)

## <a name="task-overview"></a>Omówienie zadań

Aby wykonać zadania i wskazówki dotyczące tego tematu, należy co najmniej trzema serwerami&#x2014;jeden kontroler WFF, co podstawowym serwerze sieci web dla farmy serwerów i jednego lub kilku serwerów pomocniczych sieci web dla farmy serwerów. W dowolnym momencie można dodać więcej serwerów pomocniczych do farmy serwerów WFF. Na wysokim poziomie, aby utworzyć i skonfigurować farmę serwerów WFF w danym środowisku przejściowych lub produkcyjnych, które będą potrzebne:

- Utwórz serwer kontrolera, instalując program Internet Information Services (IIS) 7.5 i WFF 2.0.
- Przygotuj serwery podstawowe i pomocnicze, tworząc wspólnego konta administratora i Konfigurowanie wyjątków zapory.
- Konfigurowanie farmy serwerów za pomocą Menedżera usług IIS na serwerze kontrolera.
- Konfigurowanie równoważenia obciążenia za pomocą usług IIS Routing żądań aplikacji (ARR) lub alternatywnego technologii równoważenia obciążenia.

Zadania i wskazówki, w tym temacie założono, że zaczynasz z kompilacjami czyste serwera z systemem Windows Server 2008 R2. Przed przystąpieniem do wykonywania, dla każdego serwera, upewnij się, że:

- Systemu Windows Server 2008 R2 z dodatkiem Service Pack 1 i wszystkie dostępne aktualizacje są instalowane.
- Serwer jest przyłączony do domeny.
- Serwer ma statyczny adres IP.

> [!NOTE]
> Aby uzyskać więcej informacji na temat dołączania komputerów do domeny, zobacz [łączenie komputerów do domeny i rejestrowanie na](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx). Aby uzyskać więcej informacji na temat konfigurowania statycznych adresów IP, zobacz [skonfigurować statyczny adres IP](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx).


## <a name="create-the-wff-controller-server"></a>Tworzenie serwera Kontroler WFF

Aby utworzyć serwer kontroler WFF, musisz zainstalować zarówno usług IIS 7 lub nowszy, jak i WFF 2.0 lub nowszej. Dzieje się w tle, WFF korzysta z narzędzia wdrażania sieci Web usług IIS (Web Deploy) 2.x, aby zsynchronizować serwerów w farmie. Użycie Instalatora platformy sieci Web do zainstalowania WFF, Instalator automatycznie Pobierz i zainstaluj narzędzie Web Deploy dla Ciebie.

**Aby utworzyć serwer kontroler WFF**

1. Pobierz i zainstaluj [Instalatora platformy sieci Web](https://go.microsoft.com/?linkid=9739157).
2. W górnej części **3.0 Instalatora platformy sieci Web** okna, kliknij przycisk **produktów**.
3. W lewej części okna, w okienku nawigacji kliknij **serwera**.
4. W **zalecana konfiguracja programu IIS 7** wiersz, kliknij przycisk **Dodaj**.
5. W <strong>sieci Web farmy Framework 2.</strong> <em>x</em> wiersz, kliknij przycisk <strong>Dodaj</strong>.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image2.png)
6. Kliknij przycisk **zainstalować**. Należy zauważyć, że Instalatora platformy sieci Web został dodany narzędzie Web Deployment, wraz z różnych innych zależności do listy instalacji.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image3.png)
7. Przejrzyj postanowienia licencyjne, a jeśli wyrażasz zgodę na warunki, kliknij przycisk **akceptuję**.
8. Po zakończeniu instalacji kliknij przycisk **Zakończ**, a następnie Zamknij **3.0 Instalatora platformy sieci Web** okna.

## <a name="configure-the-primary-and-secondary-servers"></a>Skonfiguruj serwery podstawowe i pomocnicze

Przed utworzeniem WFF farmy serwerów, należy wykonać niektóre zadania przygotowania, na serwerach sieci web, tworzących farmy:

- Dodać wyjątki zapory, aby umożliwić **podstawowe operacje sieciowe**, **administrację zdalną**, i **udostępnianie plików i drukarek** funkcji do komunikowania się z serwerem kontroler WFF .
- Utwórz konto domeny (na przykład **FABRIKAM\stagingfarm**) w usłudze Active Directory i dodać go do lokalnej grupy administratorów na każdym serwerze. To konto będzie używana jako konto administratora farmy serwerów, podczas tworzenia farmy serwerów.

Aby uzyskać więcej informacji na temat konfigurowania tych wyjątków zapory w Zaporze Windows, zobacz [systemu i wymagania dotyczące platformy Framework farmy sieci Web 2.0 dla usług IIS 7](https://go.microsoft.com/?linkid=9805128). Dla innych systemów zapory zapoznaj się z dokumentacją produktu.

Aby dodać konto domeny do lokalnej grupy administratorów systemu Windows Server 2008 R2, można użyć następnej procedury. Tę procedurę należy wykonać na każdym serwerze, który ma zostać dodany do farmy serwerów&#x2014;innymi słowy, dodawanie tego samego konta domeny do lokalnej grupy administratorów na serwerze podstawowym i na każdym serwerze pomocniczym.

**Aby dodać konto domeny do lokalnej grupy administratorów**

1. Na **Start** menu wskaż **narzędzia administracyjne**, a następnie kliknij przycisk **Menedżera serwera**.
2. W **Menedżera serwera** okna, w okienku widoku drzewa rozwiń **konfiguracji**, rozwiń węzeł **lokalnych użytkowników i grup**, a następnie kliknij przycisk **grup**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image4.png)
3. W **grup** okienku kliknij dwukrotnie **Administratorzy**.
4. W **właściwości: Administratorzy** okno dialogowe, kliknij przycisk **Dodaj**.
5. W **Wybieranie użytkowników, komputerów, kont usług lub grup** okno dialogowe, wpisz (lub przejdź) z kontem domeny (na przykład **FABRIKAM\stagingfarm**), a następnie kliknij przycisk **OK**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image5.png)
6. W **właściwości: Administratorzy** okno dialogowe, kliknij przycisk **OK**.

Serwery są teraz gotowe do dodania do farmy serwerów. W przypadku podstawowego serwera, można skonfigurować serwer do wymagań aplikacji przed lub po utworzeniu farmy serwerów&#x2014;w obu przypadkach WFF będzie synchronizować serwery, wdrażając produktów, składników lub konfiguracji do serwerów pomocniczych. Dla uproszczenia w tym samouczku przyjęto założenie, że należy skonfigurować serwer podstawowy po zakończeniu tworzenia farmy serwerów.

## <a name="create-the-wff-server-farm"></a>Tworzenie farmy serwerów WFF

W tym momencie wszystkie serwery są gotowe do dodania do farmy serwerów WFF:

- Po zainstalowaniu na serwerze kontroler WFF.
- Wyjątki zapory zostały skonfigurowane na serwerach sieci web podstawowego i pomocniczego.
- Konto domeny zostały dodane do lokalnej grupy administratorów na serwerach sieci web podstawowego i pomocniczego.

Następnym krokiem jest, aby utworzyć farmę serwerów w WFF. Można to zrobić z Menedżera usług IIS na serwerze kontroler WFF.

**Aby utworzyć farmę serwerów WFF**

1. Na serwerze kontroler WFF na **Start** menu wskaż **narzędzia administracyjne**, a następnie kliknij przycisk **Internet Information Services (IIS) Manager**.
2. W **połączeń** okienku rozwiń węzeł serwera lokalnego, kliknij prawym przyciskiem myszy **farm serwerów**, a następnie kliknij przycisk **Utwórz farmę serwerów**.
3. W **Utwórz farmę serwerów** okna dialogowego wpisz nazwę opisową dla farmy serwerów (na przykład **farmy przemieszczania**), a następnie wybierz pozycję **Aprowizuj farmę serwerów**.
4. Wpisz nazwę użytkownika i hasło konta domeny, który został dodany do lokalnej grupy administratorów na każdym serwerze.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image6.png)
5. Kliknij przycisk **Dalej**.
6. Na **Dodaj serwery** stronie, wpisz w pełni kwalifikowana nazwa domeny (FQDN) serwera podstawowego, wybierz **serwera podstawowego**, a następnie kliknij przycisk **Dodaj**.
7. W tym momencie WFF próbuje skontaktować się z serwerem podstawowym, przy użyciu podanych poświadczeń. Jeśli połączenie zakończy się powodzeniem, serwer podstawowy zostanie dodany do tabeli na **Dodaj serwery** strony.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image7.png)

    > [!NOTE]
    > Być może zauważono, że **serwer jest dostępny dla równoważenia obciążenia** jest domyślnie zaznaczone. WFF używa modułu ARR usług IIS, aby zaimplementować równoważenia obciążenia, a tym samym rozłożyć żądania na serwerach sieci web w farmie serwerów. W większości przypadków można tylko czyścił **serwer jest dostępny dla równoważenia obciążenia** opcję, jeśli chcesz użyć zamiast tego rozwiązania do równoważenia obciążenia innej firmy.
8. Na **Dodaj serwery** stronie, wpisz nazwę FQDN pierwszego serwera pomocniczego, a następnie kliknij **Dodaj**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image8.png)
9. Powtórz krok 7 dla pozostałych serwerów pomocniczych w farmie serwerów, a następnie kliknij przycisk **Zakończ**.

WFF farmy serwerów jest uruchomiona i działa. Wszelkie produktów platformy sieci web lub składniki, które należy zainstalować na serwerze podstawowym i aplikacji sieci web lub zawartość wdrażaną do podstawowego serwera będą automatycznie udostępniane na wszystkich serwerach pomocniczych.

WFF jest temat szerokiej i złożone i uzyskać więcej informacji na [firmy Microsoft w sieci Web farmy Framework 2.0 dla usług IIS 7](https://go.microsoft.com/?linkid=9805129) witryny sieci Web. W chwili obecnej, istnieją dwa obszary funkcji, które trzeba znać:

- *Aprowizacja aplikacji* to proces, który replikuje zawartości z serwera podstawowego, takich jak aplikacje sieci web i ustawień konfiguracji na wszystkich serwerach pomocniczych w farmie serwerów. Na przykład w przypadku wdrożenia przykładowe rozwiązanie Contact Manager podstawowego serwera przejściowego WFF procesu inicjowania obsługi administracyjnej aplikacji to rozwiązanie zostanie wdrożone do wszystkich serwerów pomocniczych przemieszczania. Domyślnie proces inicjowania obsługi administracyjnej aplikacji jest uruchamiane co 30 sekund.
- *Aprowizacja platformy* to proces, który synchronizuje produktów platformy sieci web i składniki z serwera podstawowego do serwerów pomocniczych w farmie serwerów. Na przykład po zainstalowaniu programu ASP.NET MVC 3 podstawowy serwer przemieszczania, proces inicjowania obsługi administracyjnej platformy użyje Instalatora platformy sieci Web do zainstalowania programu ASP.NET MVC 3 na wszystkich serwerach pomocniczych przemieszczania. Domyślnie proces inicjowania obsługi administracyjnej platformy jest uruchamiane co pięć minut.

Możesz zarządzać ustawienia inicjowania obsługi administracyjnej platformy i aplikacji w warstwie podstawowa z Menedżera usług IIS na serwerze kontroler WFF.

**Zapoznaj się z aplikacji i ustawień aprowizacji platformy**

1. W Menedżerze usług IIS w **połączeń** okienku zaznacz farmy serwerów.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image9.png)
2. W **farmy serwerów** okienku kliknij dwukrotnie **Aprowizacja aplikacji**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image10.png)
3. Jak widać, farma serwerów jest obecnie skonfigurowany do synchronizacji ustawień zawartość i konfigurację między serwerem podstawowym i serwerów pomocniczych sieci web co 30 sekund.
4. Kliknij przycisk **ponownie**, a następnie kliknij dwukrotnie **Aprowizacja platformy**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image11.png)
5. Jak widać, farma serwerów jest obecnie skonfigurowany do synchronizacji produktów platformy sieci web i składniki między serwerem podstawowym i serwerów pomocniczych, co pięć minut.
6. Kliknij przycisk **ponownie**.
7. Aby wymusić farmy serwerów, aby natychmiast zsynchronizować produktów platformy sieci web w **akcje** okienku kliknij **Aprowizuj platformę**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image12.png)

    > [!NOTE]
    > Aprowizacja platformy może trochę potrwać. Proces Instalator jest uruchamiany w tle na serwerach pomocniczych w farmie serwerów.
8. Gdy zezwolono wystarczająco dużo czasu na ukończenie procesu aprowizacji, można sprawdzić, czy produktów i składników, które zostały dodane do serwera podstawowego teraz zostały zreplikowane na serwerach pomocniczych. Na przykład, zaloguj się na serwer pomocniczy i użyj **Menedżera serwera** okna, aby zweryfikować, że została zainstalowana rola serwera sieci web.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image13.png)
9. Możesz również sprawdzić listę zainstalowanych programów, aby zweryfikować, że dodane zostały różnych składników platformy sieci web.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image14.png)

## <a name="configure-load-balancing"></a>Konfigurowanie równoważenia obciążenia

Podczas tworzenia kolektywu serwerów sieci web, musisz skonfigurować pewnego rodzaju obciążenia równoważenia do rozkładania żądań HTTP między Twoimi serwerami sieci web. Może to być system Windows Server 2008 Równoważenie obciążenia sieci, ARR usług IIS lub innych opartych na oprogramowaniu lub oparte na sprzęcie rozwiązanie do równoważenia obciążenia.

WFF został zaprojektowany do integrowania ściśle, za pomocą usług IIS w module ARR. Aby móc korzystać z tej integracji, musisz zainstalować moduł ARR na serwerze kontroler WFF. Następnie kierować cały ruch z sieci web na serwerze kontrolera zwykle, konfigurując rekordy systemu nazw domen (DNS, Domain Name System). Serwer kontrolera będzie następnie dystrybuować przychodzące żądania między serwerami w farmie serwerów, na podstawie dostępności serwera i różnych innych kryteriów.

> [!NOTE]
> Nie musisz za pomocą WFF; za pomocą funkcji ARR można skonfigurować WFF do pracy z rozwiązań do równoważenia obciążenia innej firmy. Aby uzyskać więcej informacji, zobacz [omówienie 2.0 Framework kolektywu serwerów sieci Web dla usług IIS 7](https://go.microsoft.com/?linkid=9805126).


Równoważenia obciążenia za pomocą funkcji ARR jest złożoną kwestią, najbardziej, w których wykracza poza zakres tego samouczka. Jednak dalej procedura służy do zainstalowania modułu ARR i rozpoczynanie pracy z usługą równoważenia obciążenia.

**Do skonfigurowania równoważenia obciążenia na serwerze kontroler WFF**

1. Na serwerze kontroler WFF uruchom Instalatora platformy sieci Web.
2. W górnej części **3.0 Instalatora platformy sieci Web** okna, kliknij przycisk **produktów**.
3. W lewej części okna, w okienku nawigacji kliknij **serwera**.
4. W **2.5 Routing żądań aplikacji** wiersz, kliknij przycisk **Dodaj**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image15.png)
5. Kliknij przycisk **zainstalować**, a następnie postępuj zgodnie z instrukcjami wyświetlanymi w **instalacja platformy sieci Web** okna.
6. Po zakończeniu instalacji uruchom Menedżera usług IIS, a następnie w **połączeń** okienka kliknij węzeł farmy serwera. Należy zauważyć, że dodano kilka nowych ikon **farmy serwerów** okienka.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image16.png)
7. W **farmy serwerów** okienku kliknij dwukrotnie **Równoważenie obciążenia**.
8. W **równoważenia obciążenia** okienku wybierz obciążenia równoważenia algorytmu (na przykład **najmniej bieżące żądanie**).

    > [!NOTE]
    > Aby uzyskać więcej informacji na temat równoważenia obciążenia algorytmów i innych ustawień konfiguracyjnych, zobacz [moduł Routing żądań aplikacji](https://go.microsoft.com/?linkid=9805130).

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image17.png)
9. W **akcje** okienku kliknij **Zastosuj**.

Skonfigurowano podstawowego równoważenia obciążenia dla serwerów w farmie serwerów. Jeśli możesz przekazać wszystkie ruchu kolektywu serwerów sieci web na serwerze kontrolera, żądania będą dystrybuowane między serwerami w farmie, według dostępności i algorytm równoważenia obciążenia, które wybrano.

Aby uzyskać więcej informacji na temat konfigurowania równoważenia obciążenia za pomocą modułu AAR, zobacz [moduł Routing żądań aplikacji](https://go.microsoft.com/?linkid=9805130).

## <a name="monitor-the-server-farm"></a>Monitor farmy serwerów

Możesz monitorować kondycję swojej farmy serwera, w dowolnym momencie za pomocą Menedżera usług IIS na serwerze kontrolera. W **połączeń** okienku rozwiń węzeł farmy serwerów, a następnie kliknij przycisk **serwerów**. W okienku Centrum pokaże podsumowanie każdego serwera w farmie wraz z dziennik śledzenia ostatnią aktywność.

![](creating-a-server-farm-with-the-web-farm-framework/_static/image18.png)

## <a name="conclusion"></a>Wniosek

Farmy serwerów WFF powinno być teraz pracę. Można skonfigurować serwera podstawowego do obsługi niezależnie od metody wdrożenia, które użytkownik sobie tego życzy&#x2014;sekcja dalsze informacje, aby uzyskać szczegółowe informacje&#x2014;i konfiguracji, które będą replikowane na każdym z serwerów pomocniczych w farmie serwerów.

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej wskazówek na temat wszystkich aspektów konfigurowania i używania WFF zobacz [firmy Microsoft w sieci Web farmy Framework 2.0 dla usług IIS 7](https://go.microsoft.com/?linkid=9805129) witryny sieci Web.

> [!div class="step-by-step"]
> [Poprzednie](configuring-a-database-server-for-web-deploy-publishing.md)
> [dalej](configuring-deployment-properties-for-a-target-environment.md)
