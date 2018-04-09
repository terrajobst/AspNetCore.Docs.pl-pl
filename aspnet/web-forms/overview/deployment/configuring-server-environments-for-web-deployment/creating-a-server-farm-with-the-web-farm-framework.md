---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/creating-a-server-farm-with-the-web-farm-framework
title: Tworzenie farmy serwerów za pomocą struktury farmy sieci Web | Dokumentacja firmy Microsoft
author: jrjlee
description: W tym temacie opisano sposób użycia sieci Web farmy Framework (WFF) 2.0 do tworzenia i konfigurowania farmy serwerów sieci web z kolekcji serwerów.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 656dd06d-806c-467c-863d-9fc45e5ba3ab
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/creating-a-server-farm-with-the-web-farm-framework
msc.type: authoredcontent
ms.openlocfilehash: 53a91660953795f2c55edcd795b053641d308dfe
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="creating-a-server-farm-with-the-web-farm-framework"></a>Tworzenie farmy serwerów za pomocą struktury farmy sieci Web
====================
przez [Lewandowski Jason](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie opisano sposób użycia sieci Web farmy Framework (WFF) 2.0 do tworzenia i konfigurowania farmy serwerów sieci web z kolekcji serwerów.


WFF umożliwia synchronizowanie produktów platformy sieci web i składników, aplikacji sieci web, witryn sieci Web i ustawień konfiguracji wielu serwerów z równoważeniem obciążenia sieci web. W scenariuszach, w którym ma być więcej niż jeden serwer sieci web, takich jak środowisk przemieszczania i produkcji to znacznie uprościć proces wdrażania i konfiguracji. Można wdrożyć aplikację sieci web na jednym serwerze&#x2014; *serwera podstawowego*&#x2014;i WFF automatycznie zreplikuje tej aplikacji sieci web na wszystkich innych serwerach sieci web w farmie serwerów.

## <a name="understanding-the-web-farm-framework"></a>Opis struktura farmy sieci Web

WFF 2.0 służy do obsługi administracyjnej, zarządzania i wdrażania zawartości do grupy serwerów sieci web. Wdrożenie WFF składa się z trzech ról serwera klucza:

- *Serwera kontrolera*. Ten serwer jest używany do tworzenia i konfigurowania farmy serwerów WFF. Serwer controller zarządza synchronizacji składników platformy sieci web, ustawień konfiguracji oraz aplikacji między serwerami sieci web w farmie serwerów. Zainstaluj WFF 2.0 na serwerze kontrolera, a serwer kontrolera z kolei spowoduje zainstalowanie agenta WFF na wszystkich serwerach w farmie serwerów. Z serwerem kontrolera nie koncepcyjnie należy do farmy serwerów żadnych WFF i serwera pojedynczy kontroler można zarządzać wieloma farmy serwerów. W tym scenariuszu jeden serwer kontrolera WFF jest używany do tworzenia i zarządzania nimi tymczasowej farmy serwerów i farmy serwerów produkcyjnych.
- *Serwera podstawowego*. Każdy farmy serwerów WFF zawiera pojedynczy serwer podstawowy. Podczas instalowania składników platformy sieci web lub wdrożyć aplikacje na serwerze podstawowym, WFF synchronizuje zmiany na inne serwery w farmie serwerów.
- *Serwer pomocniczy*. Farma serwerów każdego WFF zawiera jeden lub więcej serwerów pomocniczych. Wszelkie zmiany wprowadzone do serwera podstawowego są replikowane do każdego serwera pomocniczego w farmie serwerów.

Oznacza to, jak te role serwera odnoszą się do tych środowisk przemieszczania i produkcji firmy Fabrikam, Inc.:

![](creating-a-server-farm-with-the-web-farm-framework/_static/image1.png)

W tym scenariuszu środowisko przejściowe i środowiska produkcyjnego obydwa są skonfigurowane jako WFF farmy serwerów. Pojedynczy serwer kontrolera WFF zarządza zarówno farmy. W ramach każdego farmy serwerów wszystkie zmiany do serwera podstawowego są replikowane do każdego serwera pomocniczego.

Przed rozpoczęciem konfigurowania środowiska przemieszczania i produkcji, zalecamy przeczytanie tych artykułów, aby zapoznać się z kluczowymi założeniami programu WFF 2.0:

- [Omówienie struktury farmy sieci Web 2.0 dla usług IIS 7](https://go.microsoft.com/?linkid=9805126)
- [Konfigurowanie farmy serwerów Framework 2.0 kolektywu serwerów sieci Web usług IIS 7](https://go.microsoft.com/?linkid=9805127)
- [System i wymagania dotyczące platformy Framework kolektywu serwerów sieci Web 2.0 dla usług IIS 7](https://go.microsoft.com/?linkid=9805128)

## <a name="task-overview"></a>Omówienie zadań

Aby wykonać zadania i wskazówki, w tym temacie, będziesz potrzebować co najmniej trzech serwerów&#x2014;jeden kontroler WFF, jednego podstawowego serwera sieci web dla farmy serwerów i co najmniej jeden serwer pomocniczy sieci web dla farmy serwerów. Można dodać więcej serwerów pomocniczych do farmy serwerów WFF, w dowolnym momencie. Na wysokim poziomie, aby utworzyć i skonfigurować farmę serwerów WFF w danym środowisku tymczasowym czy produkcyjnym, które będą potrzebne do:

- Tworzenie serwera kontrolera, instalując Internet informacji Services (IIS) 7.5 i WFF 2.0.
- Przygotowywanie serwerów podstawowych i pomocniczych przy tworzeniu wspólnego konta administratora i konfigurowania wyjątków zapory.
- Konfigurowanie farmy serwerów przy użyciu Menedżera usług IIS na serwerze kontrolera.
- Konfigurowanie równoważenia obciążenia przy użyciu usług IIS Routing żądań aplikacji (ARR) lub alternatywne technologii równoważenia obciążenia.

Zadania i wskazówki, w tym temacie założono zaczynasz z kompilacjami czystą serwera z systemem Windows Server 2008 R2. Przed rozpoczęciem, dla każdego serwera, upewnij się, że:

- Windows Server 2008 R2 z dodatkiem Service Pack 1 i wszystkie dostępne aktualizacje są instalowane.
- Serwer jest przyłączony do domeny.
- Serwer ma statyczny adres IP.

> [!NOTE]
> Aby uzyskać więcej informacji dotyczących dołączania komputerów do domeny, zobacz [przyłączania komputerów do domeny i rejestrowanie na](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx). Aby uzyskać więcej informacji na temat konfigurowania statycznych adresów IP, zobacz [skonfigurować statyczny adres IP](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx).


## <a name="create-the-wff-controller-server"></a>Tworzenie serwera kontrolera WFF

Aby utworzyć serwer kontrolera WFF, potrzebne do zainstalowania usług IIS 7 lub nowszy i WFF 2.0 lub nowszej. W obszarze obejmuje WFF używa narzędzia wdrażania usług IIS sieci Web (Web Deploy) 2.x do synchronizowania serwerów w farmie serwerów. Użycie Instalatora platformy sieci Web do zainstalowania WFF, Instalator automatycznie pobierze i zainstaluje Narzędzie Web Deploy w dla Ciebie.

**Aby utworzyć serwer kontrolera WFF**

1. Pobierz i zainstaluj [Instalatora platformy sieci Web](https://go.microsoft.com/?linkid=9739157).
2. W górnej części **3.0 Instalatora platformy sieci Web** okna, kliknij przycisk **produkty**.
3. Po lewej stronie okna, w okienku nawigacji kliknij **serwera**.
4. W **usług IIS 7 zalecana konfiguracja** wiersz, kliknij przycisk **Dodaj**.
5. W <strong>sieci Web farmy Framework 2.</strong> <em>x</em> wiersz, kliknij przycisk <strong>Dodaj</strong>.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image2.png)
6. Kliknij przycisk **zainstalować**. Zwróć uwagę, Instalator platformy sieci Web został dodany narzędzie Web Deployment, wraz z różnych innych zależności, do listy instalacji.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image3.png)
7. Przejrzyj postanowienia licencyjne, a użytkownik wyraża zgodę na warunki, kliknij przycisk **akceptuję**.
8. Po zakończeniu instalacji kliknij przycisk **Zakończ**, a następnie Zamknij **3.0 Instalatora platformy sieci Web** okna.

## <a name="configure-the-primary-and-secondary-servers"></a>Konfigurowanie serwerów podstawowych i pomocniczych

Przed utworzeniem WFF farmy serwerów, należy wykonać niektóre zadania przygotowania na wchodzące w skład farmy serwerów sieci web:

- Dodawanie wyjątków zapory, aby umożliwić **podstawowe operacje sieciowe**, **administracji zdalnej**, i **udostępnianie plików i drukarek** funkcje do komunikacji z serwerem kontrolera WFF .
- Utwórz konto domeny (na przykład **FABRIKAM\stagingfarm**) w usłudze Active Directory i dodaj go do lokalnej grupy administratorów na każdym serwerze. Służy to konto jako konto administratora farmy serwera podczas tworzenia farmy serwerów.

Aby uzyskać więcej informacji na temat konfigurowania tych wyjątków zapory w Zaporze systemu Windows, zobacz [systemu i wymagania dotyczące platformy Framework 2.0 kolektywu serwerów sieci Web usług IIS 7](https://go.microsoft.com/?linkid=9805128). Dla innych systemów zapory zapoznaj się z dokumentacją produktu.

Następna procedura służy do dodawania konta domeny do grupy administratorów lokalnych w systemie Windows Server 2008 R2. Tę procedurę należy wykonać na każdym serwerze, który ma zostać dodany do farmy serwerów&#x2014;innymi słowy, dodaj to samo konto domeny do lokalnej grupy administratorów na serwerze podstawowym, a na każdym serwerze pomocniczym.

**Aby dodać konto domeny do lokalnej grupy administratorów**

1. Na **Start** menu wskaż **narzędzia administracyjne**, a następnie kliknij przycisk **Menedżera serwera**.
2. W **Menedżera serwera** Rozwiń okno, w okienku widoku drzewa **konfiguracji**, rozwiń węzeł **lokalnych użytkowników i grup**, a następnie kliknij przycisk **grup**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image4.png)
3. W **grup** okienku kliknij dwukrotnie **Administratorzy**.
4. W **właściwości Administratorzy** okno dialogowe, kliknij przycisk **Dodaj**.
5. W **Wybieranie użytkowników, komputerów, kont usług lub grup** okno dialogowe, typu (lub przeglądania) na koncie domeny (na przykład **FABRIKAM\stagingfarm**), a następnie kliknij przycisk **OK**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image5.png)
6. W **właściwości Administratorzy** okno dialogowe, kliknij przycisk **OK**.

Serwery są teraz gotowe do dodania do farmy serwerów. W przypadku podstawowego serwera, można skonfigurować serwera do potrzeb aplikacji przed lub po utworzeniu farmy serwerów&#x2014;w obu przypadkach WFF zsynchronizuje serwery wdrażając produktów, składników i konfiguracji do serwerów pomocniczych. Dla uproszczenia w tym samouczku przyjęto założenie, że należy skonfigurować serwer podstawowy po zakończeniu tworzenia farmy serwerów.

## <a name="create-the-wff-server-farm"></a>Tworzenie farmy serwerów WFF

W tym momencie wszystkie serwery są gotowe do dodania do farmy serwerów WFF:

- Po zainstalowaniu WFF na serwerze kontrolera.
- Skonfigurowano wyjątki zapory na serwerach sieci web podstawowego i pomocniczego.
- Konto domeny zostały dodane do lokalnej grupy administratorów na serwerach sieci web podstawowego i pomocniczego.

Następnym krokiem jest, aby utworzyć farmę serwerów w WFF. Można w tym z Menedżera usług IIS na serwerze kontrolera WFF.

**Aby utworzyć farmę serwerów WFF**

1. Na serwerze kontrolera WFF na **Start** menu wskaż **narzędzia administracyjne**, a następnie kliknij przycisk **Internet Information Services (IIS) Manager**.
2. W **połączeń** okienku rozwiń węzeł serwera lokalnego, kliknij prawym przyciskiem myszy **farm serwerów**, a następnie kliknij przycisk **tworzenia farmy serwerów**.
3. W **tworzenia farmy serwerów** okna dialogowego wpisz nazwę opisową dla farmy serwerów (na przykład **farmy przemieszczania**), a następnie wybierz **farmy serwerów należy**.
4. Wpisz nazwę użytkownika i hasło konta domeny, który został dodany do lokalnej grupy administratorów na każdym serwerze.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image6.png)
5. Kliknij przycisk **Dalej**.
6. Na **Dodawanie serwerów** strony, wpisz w pełni kwalifikowaną nazwę (FQDN) serwera podstawowego, wybierz opcję **serwera podstawowego**, a następnie kliknij przycisk **Dodaj**.
7. W tym momencie WFF podejmie próbę skontaktowania się z serwerem podstawowym, przy użyciu podanych poświadczeń. Jeśli połączenia zakończy się powodzeniem, serwer podstawowy zostanie dodany do tabeli na **Dodawanie serwerów** strony.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image7.png)

    > [!NOTE]
    > Zwróć uwagę, że **serwer jest dostępny w programie Równoważenie obciążenia** jest domyślnie zaznaczona. WFF używa modułu ARR usług IIS do wdrożenia, równoważenia obciążenia i tym samym rozpowszechniają żądań serwerów sieci web w farmie serwerów. W większości przypadków będzie tylko wyczyść **serwer jest dostępny dla programu Równoważenie obciążenia** opcję, jeśli chcesz użyć zamiast tego rozwiązania do równoważenia obciążenia innych firm.
8. Na **Dodawanie serwerów** , wpisz nazwę FQDN pierwszego serwera pomocniczego, a następnie kliknij przycisk **Dodaj**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image8.png)
9. Powtórz krok 7 dla żadnych dodatkowych serwerów pomocniczych w farmie serwerów, a następnie kliknij przycisk **Zakończ**.

Farmy serwerów WFF jest teraz uruchomiona. Wszelkie produktów platformy sieci web lub składniki, które należy zainstalować na serwerze podstawowym oraz aplikacji sieci web lub zawartości wdrażanej na serwerze podstawowym, będą automatycznie udostępniane na wszystkich serwerach pomocniczych.

WFF jest temat szerokiej i złożone i uzyskać więcej informacji na [firmy Microsoft w sieci Web farmy Framework 2.0 dla usług IIS 7](https://go.microsoft.com/?linkid=9805129) witryny sieci Web. W chwili obecnej, istnieją dwa obszary funkcje, które należy zwrócić uwagę:

- *Inicjowanie obsługi aplikacji* to proces, który replikuje zawartość z serwera podstawowego, takich jak aplikacje sieci web i ustawień konfiguracji na wszystkich serwerach pomocniczych w farmie serwerów. Na przykład, jeśli wdrożono kontaktów Menedżerze przykładowe rozwiązanie podstawowy serwer przemieszczania, WFF proces udostępniania aplikacji to rozwiązanie zostanie wdrożone do wszystkich serwerów pomocniczych tymczasowej. Domyślnie proces udostępniania aplikacji jest uruchamiane co 30 sekund.
- *Inicjowanie obsługi administracyjnej platformy* to proces, który jest synchronizowany produktów platformy sieci web i składniki z serwera podstawowego do wszystkich serwerów pomocniczych w farmie serwerów. Na przykład po zainstalowaniu programu ASP.NET MVC 3 na serwerze podstawowym przemieszczania, proces inicjowania obsługi administracyjnej platformy użyje Instalatora platformy sieci Web do zainstalowania programu ASP.NET MVC 3 na wszystkich serwerach przemieszczania dodatkowej. Domyślnie proces inicjowania obsługi administracyjnej platformy jest uruchamiane co pięć minut.

Możesz zarządzać ustawień inicjowania obsługi administracyjnej platformy i aplikacji w warstwie podstawowa z Menedżera usług IIS na serwerze WFF kontrolera.

**Eksploruj aplikacji i inicjowania obsługi ustawień platformy**

1. W Menedżerze usług IIS w **połączeń** okienku wybierz farmy serwerów.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image9.png)
2. W **farmy serwerów** okienku kliknij dwukrotnie **Inicjowanie obsługi aplikacji**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image10.png)
3. Jak widać, farmy serwerów jest obecnie skonfigurowane do synchronizacji ustawień zawartość i konfigurację sieci web między serwerem podstawowym i serwerów pomocniczych co 30 sekund.
4. Kliknij przycisk **ponownie**, a następnie kliknij dwukrotnie **inicjowania obsługi administracyjnej platformy**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image11.png)
5. Jak widać, farmy serwerów jest obecnie skonfigurowane do synchronizacji, produktów platformy sieci web i składniki między serwerem podstawowym i serwerów pomocniczych, co pięć minut.
6. Kliknij przycisk **ponownie**.
7. Aby wymusić farmy serwerów, aby zsynchronizować produktów platformy sieci web natychmiast, w **akcje** okienku, kliknij przycisk **platformy udostępniania**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image12.png)

    > [!NOTE]
    > Inicjowanie obsługi administracyjnej platformy może zająć trochę czasu. Proces Instalator jest uruchamiany w tle na serwerach pomocniczych w farmie serwerów.
8. Gdy już mieć wystarczającą ilość czasu na ukończenie procesu inicjowania obsługi administracyjnej, możesz sprawdzić, czy produktów i składników, które zostały dodane do serwera podstawowego zostały teraz zreplikowane na serwerach pomocniczych. Na przykład, zaloguj się na serwerze pomocniczym i użyj **Menedżera serwera** okno, aby zweryfikować, że została zainstalowana rola serwera sieci web.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image13.png)
9. Możesz również sprawdzić na liście zainstalowanych programów, aby sprawdzić, czy dodano różnych składników platformy sieci web.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image14.png)

## <a name="configure-load-balancing"></a>Konfigurowanie równoważenia obciążenia

Podczas tworzenia kolektywu serwerów sieci web, musisz zdefiniować niektórych dystrybucji żądań HTTP między serwerami sieci web funkcji równoważenia obciążenia. Może to być równoważenia IIS ARR obciążenia sieciowego systemu Windows Server 2008 lub rozwiązania do równoważenia obciążenia opartych na oprogramowaniu lub oparte na sprzęcie innych firm.

WFF został zaprojektowany do integrowania ściśle z usług IIS ARR. Aby móc korzystać z tej integracji, musisz zainstalować moduł ARR na serwerze kontrolera WFF. Następnie kierować cały ruch z sieci web na serwerze kontrolera zwykle przez skonfigurowanie rekordy systemu nazw domen (DNS, Domain Name System). Z serwerem kontrolera rozpowszechnianiem przychodzące żądania między serwerami w farmie serwerów, na podstawie dostępności serwera i różnych innych kryteriów.

> [!NOTE]
> Nie trzeba używać ARR z WFF; można skonfigurować WFF do pracy z rozwiązań do równoważenia obciążenia innych firm. Aby uzyskać więcej informacji, zobacz [omówienie 2.0 Framework kolektywu serwerów sieci Web usług IIS 7](https://go.microsoft.com/?linkid=9805126).


Równoważenie obciążenia sieciowego przy użyciu funkcji ARR jest złożone tematu, większość z nich wykracza poza zakres tego samouczka. Jednak dalej procedura służy do zainstalowania modułu ARR i rozpocząć pracę z równoważeniem obciążenia.

**Aby skonfigurować równoważenie na serwerze kontrolera WFF obciążenia**

1. Na serwerze kontrolera WFF uruchom Instalatora platformy sieci Web.
2. W górnej części **3.0 Instalatora platformy sieci Web** okna, kliknij przycisk **produkty**.
3. Po lewej stronie okna, w okienku nawigacji kliknij **serwera**.
4. W **2.5 Routing żądań aplikacji** wiersz, kliknij przycisk **Dodaj**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image15.png)
5. Kliknij przycisk **zainstalować**, a następnie postępuj zgodnie z instrukcjami wyświetlanymi w **instalacja platformy sieci Web** okna.
6. Po zakończeniu instalacji uruchom Menedżera usług IIS i w **połączeń** okienku, kliknij węzeł serwera farmy. Należy zauważyć, że dodano kilka nowych ikony **farmy serwerów** okienka.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image16.png)
7. W **farmy serwerów** okienku kliknij dwukrotnie **Równoważenie obciążenia**.
8. W **Równoważenie obciążenia** okienku, wybierz obciążenia saldo algorytm (na przykład **najmniej bieżącego żądania**).

    > [!NOTE]
    > Aby uzyskać więcej informacji dotyczących równoważenia obciążenia algorytmów i innych ustawień konfiguracyjnych, zobacz [moduł Routing żądań aplikacji](https://go.microsoft.com/?linkid=9805130).

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image17.png)
9. W **akcje** okienku, kliknij przycisk **Zastosuj**.

Teraz skonfigurowano podstawowe równoważenia obciążenia dla serwerów w farmie serwerów. Jeśli bezpośrednie wszystkich ruchu kolektywu serwerów sieci web z serwerem kontrolera, żądania będą dystrybuowane między serwerami w farmie, w zależności od dostępności i wybranego algorytmu równoważenia obciążenia.

Aby uzyskać więcej informacji na temat konfigurowania równoważenia obciążenia z modułem ARR, zobacz [moduł Routing żądań aplikacji](https://go.microsoft.com/?linkid=9805130).

## <a name="monitor-the-server-farm"></a>Monitor farmy serwerów

Można monitorować kondycję farmy serwerów w dowolnej chwili za pomocą Menedżera usług IIS na serwerze kontrolera. W **połączeń** okienku rozwiń farmy serwerów, a następnie kliknij przycisk **serwerów**. Środkowym okienku zostaną wyświetlone podsumowanie każdego serwera w farmie wraz z dziennik śledzenia ostatnią aktywność.

![](creating-a-server-farm-with-the-web-farm-framework/_static/image18.png)

## <a name="conclusion"></a>Wniosek

Farmy serwerów WFF powinno być teraz uruchomiona. Można skonfigurować serwera podstawowego do obsługi sposób wdrażania wolisz&#x2014;dalsze informacje szczegółowe informacje zawiera sekcja&#x2014;i konfiguracji zostaną zreplikowane na każdym serwerze pomocniczym w farmie serwerów.

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej pomocy na temat wszystkich aspektów Konfigurowanie i używanie WFF, zobacz [firmy Microsoft w sieci Web farmy Framework 2.0 dla usług IIS 7](https://go.microsoft.com/?linkid=9805129) witryny sieci Web.

> [!div class="step-by-step"]
> [Poprzednie](configuring-a-database-server-for-web-deploy-publishing.md)
> [dalej](configuring-deployment-properties-for-a-target-environment.md)
