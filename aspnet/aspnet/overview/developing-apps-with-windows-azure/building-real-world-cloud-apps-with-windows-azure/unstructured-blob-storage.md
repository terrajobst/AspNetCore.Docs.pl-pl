---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage
title: Magazyn obiektów Blob bez struktury (kompilowanie praktyczne aplikacje w chmurze platformy Azure) | Dokumentacja firmy Microsoft
author: MikeWasson
description: Kompilowanie rzeczywistych World aplikacje w chmurze z Azure Książka elektroniczna jest oparta na prezentacji opracowane przez Scott Guthrie. Wyjaśniono 13 wzorców i rozwiązań, które może on...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/30/2015
ms.topic: article
ms.assetid: 9f05ccb1-2004-4661-ad8b-c370e6c09c8e
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage
msc.type: authoredcontent
ms.openlocfilehash: c2c82a579feb586287c40bb82eba53c5f84afaba
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872575"
---
<a name="unstructured-blob-storage-building-real-world-cloud-apps-with-azure"></a>Magazyn obiektów Blob bez struktury (kompilowanie praktyczne aplikacje w chmurze platformy Azure)
====================
przez [Wasson Jan](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Dykstra niestandardowy](https://github.com/tdykstra)

[Pobieranie napraw projektu](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) lub [Pobierz książkę E](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Tworzenia rzeczywistych aplikacji w chmurze platformy Azure** Książka elektroniczna opiera się na prezentacji opracowane przez Scott Guthrie. Wyjaśniono 13 wzorców i wskazówki, które mogą pomóc Ci się pomyślnie, tworzenie aplikacji sieci web dla chmury. Informacje o Książka elektroniczna, zobacz [pierwszy rozdział](introduction.md).


W poprzednim rozdziale nasz przeglądał partycjonowania schematy i wyjaśniono, jak aplikacji Usuń magazyny obrazów w usługi obiektu Blob magazynu Azure i innych danych zadania w bazie danych SQL Azure. W tym rozdziale możemy przejść głębiej do usługi Blob i pokazać, jak jest zaimplementowana w poprawka kod projektu.

## <a name="what-is-blob-storage"></a>Co to jest magazyn obiektów Blob?

Usługa obiektu Blob magazynu Azure udostępnia sposób przechowywania plików w chmurze. Usługa Blob ma kilka zalet w porównaniu z przechowywanie plików w sieci lokalnej systemu plików:

- Jest to wysoce skalowalna. Jedno konto magazynu może przechowywać [kilkuset terabajtów](https://msdn.microsoft.com/library/windowsazure/dn249410.aspx), i może zawierać wiele kont magazynu. Niektóre z najważniejszych klientów platformy Azure przechowują setki petabajtów. Microsoft SkyDrive korzysta z magazynu obiektów blob.
- Jest trwały. Wszystkie pliki, które są przechowywane w usłudze obiektów Blob jest automatycznie do kopii zapasowej.
- Zapewnia wysoką dostępność. [Magazyn — umowa SLA](https://go.microsoft.com/fwlink/p/?linkid=159705&amp;clcid=0x409) czas działania ze zobowiązania 99,9% lub 99,99%, w zależności od opcji nadmiarowość geograficzna wybierz.
- Jest funkcją platforma jako usługa (PaaS) platformy Azure, co oznacza tylko przechowywania i pobierania plików, zwracając tylko w przypadku rzeczywista ilość magazynu, i Azure automatycznie odpowiada on za konfigurowanie i zarządzanie wszystkich maszyn wirtualnych i dysków wymaganych do Usługa.
- Za pomocą interfejsu API REST lub przy użyciu języka programowania interfejsu API można uzyskać dostępu do usługi Blob. Dla platformy .NET, Java, Ruby i innych dostępnych zestawów SDK.
- Gdy plik jest przechowywany w usłudze obiektów Blob, można łatwo udostępnić go publicznie w Internecie.
- Możesz zabezpieczyć pliki w obiekcie Blob uzyskać dostępu do usługi, mogą one tylko przez autoryzowanych użytkowników lub zapewniają tokeny tymczasowego dostępu, które udostępnia je do innej tylko na pewien czas.

W dowolnym momencie tworzysz aplikację na platformie Azure i przechowywania dużych ilości danych, które w środowisku lokalnym przejdzie w plikach — na przykład obrazów, klipów wideo, plików PDF, arkusze kalkulacyjne, itd. — należy wziąć pod uwagę usługi Blob.

## <a name="creating-a-storage-account"></a>Tworzenie konta magazynu

Aby rozpocząć pracę z usługą Blob Utwórz konto magazynu na platformie Azure. W portalu kliknij **nowy** -- **usług danych** -- **magazynu** -- **szybkie tworzenie**, a następnie wprowadź adres URL i lokalizację centrum danych. Lokalizacja centrum danych powinna być taka sama jak aplikacji sieci web.

![Tworzenie konta magazynu](unstructured-blob-storage/_static/image1.png)

Wybierz regionu podstawowego, której chcesz umieścić zawartość, a jeśli wybierzesz [— replikacja geograficzna](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx#_Geo_Redundant_Storage) opcji Azure tworzy replik wszystkich danych w centrum danych w innym regionie kraju. Na przykład jeśli Centrum danych zachodnie stany USA, gdy plik, który przechodzi ona zachodnie stany USA centrum danych, ale w tle Azure również są przechowywane kopiuje go do jednego z innych Stanów Zjednoczonych centrów danych. W przypadku awarii w jednym regionie kraju, dane są nadal bezpieczne.

Azure będą replikowane danych poza granicami politycznej geo: w przypadku lokalizacji głównej w Stanach Zjednoczonych, pliki są replikowane tylko na innego regionu w Stanach Zjednoczonych; lokalizacji głównej w przypadku klientów w Australii, plików są replikowane tylko na innym centrum danych w Australii.

Oczywiście możesz można również utworzyć konto magazynu, wykonując polecenia ze skryptu, jak widzieliśmy wcześniej. Poniżej przedstawiono polecenia programu Windows PowerShell, aby utworzyć konto magazynu:

[!code-powershell[Main](unstructured-blob-storage/samples/sample1.ps1)]

Po utworzeniu konta magazynu, można od razu rozpocząć przechowywanie plików w usłudze obiektów Blob.

## <a name="using-blob-storage-in-the-fix-it-app"></a>W aplikacji rozwiązać przy użyciu magazynu obiektów Blob

Poprawka aplikacji umożliwia przekazywanie zdjęć.

![Utwórz zadanie poprawka](unstructured-blob-storage/_static/image2.png)

Po kliknięciu **automatyczne tworzenie**, aplikacja przekazuje określony plik obrazu i zapisuje go w usłudze obiektów Blob.

### <a name="set-up-the-blob-container"></a>Konfigurowanie kontenera obiektów Blob

Aby przechowywać plik w usłudze obiektów Blob należy *kontenera* Zapisz go w. Kontener usługi Blob odpowiada folderu systemu plików. Skryptów tworzenia środowiska, które firma Microsoft sprawdzone w [zautomatyzować wszystko rozdział](automate-everything.md) utworzyć konto magazynu, ale nie ich utworzenia kontenera. Dlatego celem `CreateAndConfigure` metody `PhotoService` klasy jest aby utworzyć kontener, jeśli jeszcze nie istnieje. Ta metoda jest wywoływana z `Application_Start` metody w *Global.asax*.

[!code-csharp[Main](unstructured-blob-storage/samples/sample2.cs)]

Klucz nazwy i dostępu do konta magazynu są przechowywane w `appSettings` Kolekcja *Web.config* pliku, a kod w `StorageUtils.StorageAccount` metoda wykorzystuje te wartości do utworzenia parametrów połączenia i nawiązania połączenia:

[!code-csharp[Main](unstructured-blob-storage/samples/sample3.cs)]

`CreateAndConfigureAsync` Metoda tworzy następnie obiekt, który reprezentuje usługa Blob, a obiekt, który reprezentuje kontener (folder) o nazwie "obrazy" w usłudze obiektów Blob:

[!code-csharp[Main](unstructured-blob-storage/samples/sample4.cs)]

Jeśli kontener o nazwie "obrazy" jeszcze nie istnieje — będą spełnione przy pierwszym uruchomieniu aplikacji do nowego konta magazynu — kod tworzy kontener i ustawia uprawnienia, aby. (Domyślnie nowe kontenery obiektów blob są prywatne i są dostępne tylko dla użytkowników, którzy mają uprawnienia do uzyskania dostępu do konta magazynu)

[!code-csharp[Main](unstructured-blob-storage/samples/sample5.cs)]

### <a name="store-the-uploaded-photo-in-blob-storage"></a>Przekazane zdjęcie w magazynie obiektów Blob magazynu

Aby przekazać i Zapisz plik obrazu, aplikacja używa `IPhotoService` interfejsu i implementację interfejsu w `PhotoService` klasy. *PhotoService.cs* plik zawiera wszystkie kodu w poprawka aplikacji, która komunikuje się z usługą obiektów Blob.

Następujące metody kontrolera MVC jest wywoływana, gdy użytkownik kliknie **automatyczne tworzenie**. W tym kodzie `photoService` odwołuje się do wystąpienia `PhotoService` klasy, a `fixittask` odwołuje się do wystąpienia `FixItTask` klasy jednostka, która przechowuje dane dla nowego zadania.

[!code-csharp[Main](unstructured-blob-storage/samples/sample6.cs?highlight=8)]

`UploadPhotoAsync` Metoda `PhotoService` klasy przechowuje przekazanego pliku w usłudze obiektów Blob i zwraca adres URL wskazujący do nowego obiektu blob.

[!code-csharp[Main](unstructured-blob-storage/samples/sample7.cs)]

Podobnie jak w `CreateAndConfigure` metody kod łączy się z kontem magazynu i tworzy obiekt, który reprezentuje kontener obiektów blob "obrazy", z wyjątkiem w tym miejscu zakłada kontener już istnieje.

Następnie tworzy unikatowy identyfikator obrazu do przekazania, łącząc wartość identyfikatora GUID z rozszerzeniem pliku:

[!code-csharp[Main](unstructured-blob-storage/samples/sample8.cs)]

Następnie kod używa obiektu kontenera obiektów blob i nowy unikatowy identyfikator można utworzyć obiektu blob, ustawia atrybut z tym obiektem, wskazującą, jakiego rodzaju pliku jest, a następnie używa obiektu blob do przechowywania pliku w magazynie obiektów blob.

[!code-csharp[Main](unstructured-blob-storage/samples/sample9.cs)]

Na koniec pobiera adres URL, który odwołuje się do obiektu blob. Ten adres URL będzie przechowywana w bazie danych i pozwala na stronach sieci web napraw go wyświetlić załadowanego obrazu.

[!code-csharp[Main](unstructured-blob-storage/samples/sample10.cs)]

Ten adres URL jest przechowywana w bazie danych jako jedna z kolumn tabeli FixItTask.

[!code-csharp[Main](unstructured-blob-storage/samples/sample11.cs?highlight=10)]

Tylko adres URL w bazie danych i obrazów w magazynie obiektów Blob aplikacji Usuń zachowuje bazy danych mały, skalowalna i niedrogie, gdy obrazy są przechowywane w przypadku magazynu tanie i może obsługiwać terabajtów lub petabajtów. Jedno konto magazynu może przechowywać kilkuset terabajtów poprawka zdjęć i płacisz tylko za można użyć. Aby można było rozpocząć małych płatniczych centów 9 pierwszy gigabajtach i dodać więcej obrazów dla pojedynczych groszy za gigabajt dodatkowe.

### <a name="display-the-uploaded-file"></a>Wyświetl przekazanego pliku

Aplikacji Usuń Wyświetla plik załadowanego obrazu, gdy Wyświetla szczegóły zadania.

![Poprawka szczegóły zadania ze zdjęciem](unstructured-blob-storage/_static/image3.png)

Do wyświetlania obrazu, związana z widoku MVC jest obejmują `PhotoUrl` wartość w formacie HTML, wysyłany do przeglądarki. Serwer sieci web i bazy danych nie jest cykle używany do wyświetlania obrazu, są one tylko obsługująca się kilka bajtów na adres URL obrazu. W poniższym kodzie Razor `Model` odwołuje się do wystąpienia `FixItTask` klasy jednostka.

[!code-cshtml[Main](unstructured-blob-storage/samples/sample12.cshtml?highlight=11)]

W kodzie HTML strony, która wyświetla, możesz zobaczyć adresie URL wskazującym bezpośrednio do obrazu w magazynie obiektów blob coś w następujący sposób:

[!code-cshtml[Main](unstructured-blob-storage/samples/sample13.cshtml?highlight=11)]

## <a name="summary"></a>Podsumowanie

Przedstawiono sposób aplikacji Usuń przechowuje obrazów w usługa Blob i tylko URL obrazu w bazie danych SQL. Przy użyciu usługi obiektów Blob przechowuje znacznie mniejszym zakresie niż go w przeciwnym razie będzie umożliwia skalowanie do niemal nieograniczoną liczbę zadań i mogą być przeprowadzane bez zapisywania dużej ilości kodu bazy danych SQL.

Konto magazynu może zawierać kilkuset terabajtów, a koszt magazynowania jest znacznie mniej kosztowne niż w przypadku magazynu bazy danych SQL, zaczynając od około 3 centów za gigabajt na miesiąc oraz opłat małych transakcji. I należy pamiętać, że użytkownik nie opłaca maksymalną pojemność, ale tylko dla kwoty faktycznie przechowywane, tak że aplikacja jest gotowa do skalowania, ale nie opłaca na wszystkie te dodatkowej pojemności.

W [następnego rozdziału](design-to-survive-failures.md) będzie omawianiu znaczenie podejmowania może bezpiecznie obsługiwać błędów aplikacji w chmurze.

## <a name="resources"></a>Zasoby

Aby uzyskać więcej informacji, zobacz następujące zasoby:

- [Wprowadzenie do magazynu obiektów BLOB Azure](https://www.simple-talk.com/cloud/cloud-data/an-introduction-to-windows-azure-blob-storage-/). Blog przez drewna Jan.
- [Jak używać usługi magazynu obiektów Blob Azure w programie .NET](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs). Oficjalna dokumentacja w witrynie MicrosoftAzure.com. Krótkie wprowadzenie do obiektu blob magazynu przykłady kodu, pokazujący sposób nawiązywania połączenia z magazynu obiektów blob, a następnie utworzyć kontenerów, przekazywanie i pobrać obiekty BLOB itp.
- [Przed uszkodzeniami: Tworzenie usługi w chmurze skalowalności, odporności](https://channel9.msdn.com/Series/FailSafe). Seria filmów dziewięć części Ulrich Homann, Mercuri wytłoków i moduły SIMM znaku. Przedstawia informacje o szczegółowo pojęcia i architektury zasad w sposób bardzo dostępny i interesujące z wątków z doświadczenia zespół Advisory klienta firmy Microsoft (CAT) z konkretnymi klientami. Omówienie usługi Azure Storage i obiektów blob Zobacz epizodu 5, zaczynając od 35:13.
- [Microsoft Patterns and Practices - Azure wskazówki](https://msdn.microsoft.com/library/dn568099.aspx). Zobacz klucza Valet wzorca.

> [!div class="step-by-step"]
> [Poprzednie](data-partitioning-strategies.md)
> [dalej](design-to-survive-failures.md)
