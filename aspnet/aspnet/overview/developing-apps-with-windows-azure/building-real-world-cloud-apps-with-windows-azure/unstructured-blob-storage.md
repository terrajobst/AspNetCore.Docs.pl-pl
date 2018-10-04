---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage
title: Magazyn obiektów Blob bez struktury (tworzenie rzeczywistych aplikacji w chmurze dzięki platformie Azure) | Dokumentacja firmy Microsoft
author: MikeWasson
description: Tworzenie rzeczywistych aplikacji w chmurze za pomocą platformy Azure Książka elektroniczna jest oparta na prezentacji, opracowane przez Scotta Guthrie. Wyjaśniono 13 wzorców i praktyk, które może on...
ms.author: riande
ms.date: 03/30/2015
ms.assetid: 9f05ccb1-2004-4661-ad8b-c370e6c09c8e
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage
msc.type: authoredcontent
ms.openlocfilehash: 1a56a76c9bf27fdd7afb090ca473ebeee4065fe2
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577421"
---
<a name="unstructured-blob-storage-building-real-world-cloud-apps-with-azure"></a>Magazyn obiektów Blob bez struktury (tworzenie rzeczywistych aplikacji w chmurze dzięki platformie Azure)
====================
przez [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)

[Pobierz go naprawić projektu](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) lub [Pobierz książkę elektroniczną](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Tworzenie rzeczywistych aplikacji w chmurze dzięki platformie Azure** Książka elektroniczna jest oparta na prezentacji opracowany przez Scotta Guthrie. Wyjaśniono 13 wzorców i praktyk, które mogą pomóc Ci odnieść sukces, tworzenie aplikacji sieci web w chmurze. Aby uzyskać informacji o książce elektronicznej, zobacz [pierwszy rozdział](introduction.md).


W poprzednim rozdziale firma Microsoft przyjrzano się poszczególne schematy partycjonowania i wyjaśniono, jak aplikacja naprawić obrazy będą przechowywane w usługi Azure Storage Blob i innych danych zadania usługi Azure SQL Database. W tym rozdziale możemy bardziej szczegółowo omawiają na usługę Blob service i pokazują, jak zaimplementowano w kodzie projektu rozwiązać go.

## <a name="what-is-blob-storage"></a>Co to jest magazyn obiektów Blob?

Usługa Azure Storage Blob umożliwia przechowywanie plików w chmurze. Usługa Blob ma kilka zalet w stosunku do przechowywania plików w systemie plików sieci lokalnej:

- Jest to o wysokim stopniu skalowalności. Jedno konto magazynu może przechowywać [setki terabajtów](https://msdn.microsoft.com/library/windowsazure/dn249410.aspx), i może mieć wielu kont magazynu. Niektóre z największych klientów platformy Azure przechowywać setki petabajtów. Microsoft SkyDrive korzysta z magazynu obiektów blob.
- Jest trwały. Każdy plik, które są przechowywane w usłudze obiektów Blob jest automatycznie do kopii zapasowej.
- Zapewnia wysoką dostępność. [Magazyn — umowa SLA](https://go.microsoft.com/fwlink/p/?linkid=159705&amp;clcid=0x409) obietnic co najmniej 99,9% dostępność przez 99,99% czasu, w zależności od wybranej opcji nadmiarowości geograficznej wybierzesz.
- Jest funkcją platforma jako usługa (PaaS) platformy Azure, co oznacza po prostu przechowywania i pobierania plików, aby płacić tylko za rzeczywiste ilość miejsca, możesz użyć, a platforma Azure automatycznie zajmie się konfiguracji i wszystkich maszyn wirtualnych i dysków wymaganych do zarządzania Usługa.
- Za pomocą interfejsu API REST lub przy użyciu interfejsu API języka programowania, aby uzyskać dostęp usługi obiektów Blob. Zestawy SDK są dostępne dla platformy .NET, Java, Ruby i innych.
- Gdy plik jest przechowywany w usłudze obiektów Blob, można łatwo udostępnić je publicznie za pośrednictwem Internetu.
- Możesz zabezpieczyć pliki w obiekcie Blob service, dzięki czemu mogą oni dostępny tylko dla autoryzowanych użytkowników lub można podać tokeny tymczasowego dostępu, które udostępnia je do innej tylko przez ograniczony okres czasu.

W dowolnym czasie w przypadku tworzenia aplikacji dla platformy Azure, a użytkownik chce przechowują duże ilości danych w środowisku lokalnym przejdzie w plikach — takich jak obrazy, wideo, pliki PDF, arkusze kalkulacyjne itd. — należy wziąć pod uwagę na usługę Blob service.

## <a name="creating-a-storage-account"></a>Tworzenie konta magazynu

Aby rozpocząć pracę z usługą Blob należy utworzyć konto magazynu na platformie Azure. W portalu, kliknij przycisk **New** -- **usług danych** -- **magazynu** -- **szybkie tworzenie**, a następnie wprowadź adres URL i lokalizację centrum danych. Lokalizacja centrum danych powinna być taka sama, jak Twoja aplikacja sieci web.

![Tworzenie konta magazynu](unstructured-blob-storage/_static/image1.png)

Wybierz region podstawowy, którym chcesz umieścić zawartość, a jeśli wybierzesz [geografickou replikaci](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx#_Geo_Redundant_Storage) opcji, platforma Azure tworzy replik wszystkich danych w centrum danych w innym regionie kraju. Na przykład jeśli wybierzesz centrum danych regionie zachodnie stany USA, gdy zapisujesz plik, który zostanie wprowadzona do centrum danych w regionie zachodnie stany USA, ale w tle Azure również kopiuje go do innego centrum danych USA. W przypadku awarii w jednym regionie w kraju, Twoje dane są nadal bezpiecznego.

Azure nie będzie replikować dane granice polityczne geograficznej: Jeśli lokalizacji głównej znajduje się w Stanach Zjednoczonych, pliki tylko są replikowane do innego regionu na terenie Stanów Zjednoczonych; Jeśli lokalizacji głównej jest Australia, pliki są tylko replikowane do innego centrum danych w Australii.

Oczywiście możesz można również utworzyć konto magazynu, wykonując polecenia ze skryptu, jak widzieliśmy wcześniej. Poniżej przedstawiono polecenia programu Windows PowerShell, aby utworzyć konto magazynu:

[!code-powershell[Main](unstructured-blob-storage/samples/sample1.ps1)]

Po utworzeniu konta magazynu, możesz od razu zacząć, przechowywanie plików w usłudze obiektów Blob.

## <a name="using-blob-storage-in-the-fix-it-app"></a>W aplikacji rozwiązać za pomocą magazynu obiektów Blob

Aplikacja naprawić umożliwia przekazywanie zdjęć.

![Utwórz zadanie poprawka](unstructured-blob-storage/_static/image2.png)

Po kliknięciu **automatyczne tworzenie**, aplikacja przekazuje plik określony obraz i zapisuje go w usłudze obiektów Blob.

### <a name="set-up-the-blob-container"></a>Konfigurowanie kontenera obiektów Blob

Aby można było przechowywać plik w usłudze obiektów Blob, należy *kontenera* Zapisz go w. Kontener obiektów Blob usługi odnosi się do folderu systemu plików. Skryptów tworzenia środowiska, które zapoznaliśmy się w [Automatyzowanie wszystkiego rozdział](automate-everything.md) Tworzenie konta magazynu, ale nie mogą tworzyć kontener. Dlatego celem `CreateAndConfigure` metody `PhotoService` klasy jest utworzyć kontener, jeśli jeszcze nie istnieje. Ta metoda jest wywoływana z `Application_Start` method in Class metoda *Global.asax*.

[!code-csharp[Main](unstructured-blob-storage/samples/sample2.cs)]

Klucz nazwy i dostęp do konta magazynu są przechowywane w `appSettings` zbiór *Web.config* plików i kodu w `StorageUtils.StorageAccount` metoda używa tych wartości do tworzenia parametrów połączenia i nawiązania połączenia:

[!code-csharp[Main](unstructured-blob-storage/samples/sample3.cs)]

`CreateAndConfigureAsync` Metoda następnie tworzy obiekt, który reprezentuje usługę Blob service, a obiekt, który reprezentuje kontener (folder) o nazwie "obrazy" w usłudze obiektów Blob:

[!code-csharp[Main](unstructured-blob-storage/samples/sample4.cs)]

Jeśli kontener o nazwie "obrazy" nie istnieje jeszcze — będzie znajdował się pierwszym uruchomieniu aplikacji na nowe konto magazynu — kod tworzy kontener i ustawia uprawnienia do Zmień ją na publiczną. (Domyślnie nowe kontenery obiektów blob są prywatne i są dostępne tylko dla użytkowników, którzy mają uprawnienia dostępu do konta magazynu.)

[!code-csharp[Main](unstructured-blob-storage/samples/sample5.cs)]

### <a name="store-the-uploaded-photo-in-blob-storage"></a>Store przekazanego zdjęcia w magazynie obiektów Blob

Aby przekazać, a następnie zapisz plik obrazu, ta aplikacja używa `IPhotoService` interfejsu i implementację interfejsu w `PhotoService` klasy. *PhotoService.cs* plik zawiera cały kod w poprawka aplikacji, która komunikuje się z usługą Blob.

Następujące metody kontrolera MVC jest wywoływana, gdy użytkownik kliknie **automatyczne tworzenie**. W tym kodzie `photoService` odwołuje się do wystąpienia `PhotoService` klasy, a `fixittask` odwołuje się do wystąpienia `FixItTask` Klasa jednostki, która przechowuje dane dla nowego zadania.

[!code-csharp[Main](unstructured-blob-storage/samples/sample6.cs?highlight=8)]

`UploadPhotoAsync` Method in Class metoda `PhotoService` klasy przekazanego pliku są przechowywane w usłudze obiektów Blob i zwraca adres URL, który wskazuje na nowy obiekt blob.

[!code-csharp[Main](unstructured-blob-storage/samples/sample7.cs)]

Podobnie jak w `CreateAndConfigure` metody kod nawiązuje połączenie z kontem magazynu i tworzy obiekt, który reprezentuje kontener obiektów blob "obrazy", z wyjątkiem tutaj przyjęto założenie, kontener już istnieje.

Następnie tworzy unikatowy identyfikator obrazu, który ma zostać przekazany przez złączenie nową wartość identyfikatora GUID z rozszerzeniem pliku:

[!code-csharp[Main](unstructured-blob-storage/samples/sample8.cs)]

Następnie kod używa obiektu kontenera obiektów blob i nowy unikatowy identyfikator, aby utworzyć obiekt blob, ustawia atrybut obiektu, na którym wskazujący rodzaj pliku ona jest, a następnie używa obiektu blob do przechowywania pliku w magazynie obiektów blob.

[!code-csharp[Main](unstructured-blob-storage/samples/sample9.cs)]

Na koniec pobiera adres URL, który odwołuje się do obiektu blob. Ten adres URL będą przechowywane w bazie danych i pozwala na stronach sieci web rozwiązać go wyświetlić przekazanego obrazu.

[!code-csharp[Main](unstructured-blob-storage/samples/sample10.cs)]

Ten adres URL są przechowywane w bazie danych jako jedna z kolumn tabeli FixItTask.

[!code-csharp[Main](unstructured-blob-storage/samples/sample11.cs?highlight=10)]

Przy użyciu tylko adresu URL w bazie danych i obrazów w magazynie obiektów Blob aplikacji naprawić przechowuje bazy danych małe, skalowalne i tanie, gdy obrazy są przechowywane w którym magazyn to tania i może obsługiwać terabajtów lub petabajtów. Jedno konto magazynu może przechowywać setki terabajtów zdjęcia napraw go, i płacisz tylko za rzeczywiste użycie. Aby można było rozpocząć małych płatniczej centów 9 pierwszy gigabajtach i dodać więcej obrazów dla ułamków monet za każdy gigabajt dodatkowe.

### <a name="display-the-uploaded-file"></a>Przekazany plik do wyświetlenia

Plik przekazany obraz jest wyświetlany w aplikacji rozwiązać, gdy Wyświetla szczegóły zadania.

![Poprawka szczegóły zadania ze zdjęciem](unstructured-blob-storage/_static/image3.png)

Aby wyświetlić obraz, musi wykonać widoku MVC wystarczy obejmują `PhotoUrl` wartość w formacie HTML, wysyłany do przeglądarki. Serwer sieci web i baza danych nie korzystają z cykle do wyświetlania obrazu, tylko używasz się kilka bajtów do adresu URL obrazu. W poniższym kodzie Razor `Model` odwołuje się do wystąpienia `FixItTask` Klasa jednostki.

[!code-cshtml[Main](unstructured-blob-storage/samples/sample12.cshtml?highlight=11)]

Jeśli spojrzysz na kod HTML strony, w której są wyświetlane, zostanie wyświetlony adres URL wskazujący bezpośrednio do obrazu w usłudze blob storage, podobnie do następującej:

[!code-cshtml[Main](unstructured-blob-storage/samples/sample13.cshtml?highlight=11)]

## <a name="summary"></a>Podsumowanie

Wiesz, jak aplikacja naprawić obrazy będą przechowywane w usłudze obiektów Blob i tylko URL obrazu w usłudze SQL database. Przy użyciu usługi obiektów Blob przechowuje bazy danych SQL znacznie mniejszym zakresie niż go w przeciwnym razie byłyby umożliwia skalowanie w górę do niemal nieograniczoną liczbę zadań i może odbywać się bez konieczności pisania duże ilości kodu.

Masz setki terabajtów na koncie magazynu, a koszt magazynowania jest znacznie mniej kosztowne niż w przypadku magazynu bazy danych SQL, zaczynając od około 3 centów za gigabajt na miesiąc plus opłata małych transakcji. I należy pamiętać, że jesteś bez poświęcania maksymalną pojemność, ale tylko w przypadku kwotę, które faktycznie są przechowywane, dzięki czemu aplikacja jest gotowa do skalowania, ale nie opłaca dla dodatkowej pojemności.

W [następny rozdział](design-to-survive-failures.md) omówimy znaczenie podejmowania zdolne do poprawnego działania obsługi błędów aplikacji w chmurze.

## <a name="resources"></a>Resources

Aby uzyskać więcej informacji, zobacz następujące zasoby:

- [Wprowadzenie do usługi Azure BLOB Storage](https://www.simple-talk.com/cloud/cloud-data/an-introduction-to-windows-azure-blob-storage-/). Blog przez Mike'a drewna.
- [Jak używać usługi Azure Blob Storage na platformie .NET](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs). Oficjalna dokumentacja w witrynie MicrosoftAzure.com. Krótkie wprowadzenie do usługi blob storage, przykłady kodu, w którym pokazano, jak połączyć się z magazynu obiektów blob, a następnie utworzyć kontenery, przekazywanie i pobieranie obiektów blob itp.
- [Przed uszkodzeniami: Tworzenie usługi w chmurze skalowalne, odporne](https://channel9.msdn.com/Series/FailSafe). Seria filmów dziewięć części Ulrich Homann, Marc Mercuri i — Markiem Simmsem. Przedstawia informacje o szczegółowo pojęcia i zasady dotyczące architektury w sposób bardzo dostępny i interesujące z historii z doświadczenia zespołu doradczego klientów firmy Microsoft (CAT) z klientów. Omówienie usługi Azure Storage i obiektów blob Zobacz odcinek 5 zaczynając od 35:13.
- [Microsoft Patterns and Practices — wskazówki dotyczące platformy Azure](https://msdn.microsoft.com/library/dn568099.aspx). Zobacz wzorzec klucza Portiera.

> [!div class="step-by-step"]
> [Poprzednie](data-partitioning-strategies.md)
> [dalej](design-to-survive-failures.md)
