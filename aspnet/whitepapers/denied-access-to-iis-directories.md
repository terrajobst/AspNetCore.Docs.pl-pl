---
uid: whitepapers/denied-access-to-iis-directories
title: ASP.NET odmowa dostępu do katalogów usług IIS | Dokumentacja firmy Microsoft
author: rick-anderson
description: Ten dokument zawiera opis, co należy zrobić, jeśli żądanie do aplikacji platformy ASP.NET zwraca błąd "odmowa dostępu do katalogu DirectoryName. Nie można s...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: 3cb27b8a-354f-4332-bfe0-232b13bbf8aa
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/denied-access-to-iis-directories
msc.type: content
ms.openlocfilehash: d95423776a6b58fc67ae6c791685543dadd2480c
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/22/2018
ms.locfileid: "30070774"
---
<a name="aspnet-denied-access-to-iis-directories"></a><span data-ttu-id="13819-104">Odmowa dostępu do katalogów usług IIS programu ASP.NET</span><span class="sxs-lookup"><span data-stu-id="13819-104">ASP.NET Denied Access to IIS Directories</span></span>
====================
> <span data-ttu-id="13819-105">Ten dokument zawiera opis co należy zrobić, jeśli żądanie do aplikacji platformy ASP.NET zwraca błąd "odmowa dostępu do *DirectoryName* katalogu.</span><span class="sxs-lookup"><span data-stu-id="13819-105">This whitepaper describes what you must do if a request to your ASP.NET application returns the error, "Denied Access to *DirectoryName* directory.</span></span> <span data-ttu-id="13819-106">Nie można uruchomić monitorowania zmian w katalogach."</span><span class="sxs-lookup"><span data-stu-id="13819-106">Failed to start monitoring directory changes."</span></span>
> 
> <span data-ttu-id="13819-107">Dotyczy ASP.NET 1.0 i 1.1 programu ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="13819-107">Applies to ASP.NET 1.0 and ASP.NET 1.1.</span></span>


<span data-ttu-id="13819-108">RTM programu ASP.NET w wersji 1 są obecnie uruchamiane przy użyciu mniej uprzywilejowanego konta systemu windows — w zarejestrowany jako konto "ASPNET" na komputerze lokalnym.</span><span class="sxs-lookup"><span data-stu-id="13819-108">ASP.NET V1 RTM now runs using a less privileged windows account - registered as the "ASPNET" account on a local machine.</span></span>

<span data-ttu-id="13819-109">W przypadku niektórych zablokowana systemów, to konto może domyślnie nie ma zabezpieczeń dostęp do odczytu katalogi z zawartością witryny sieci Web, katalogu głównym aplikacji lub katalogu głównego witryny sieci web.</span><span class="sxs-lookup"><span data-stu-id="13819-109">On some locked down systems, this account may not by default have read security access to a website's content directories, the application root directory, or the web site root directory.</span></span> <span data-ttu-id="13819-110">W takim przypadku zostanie wyświetlony następujący błąd podczas żądania strony z danej aplikacji sieci web:</span><span class="sxs-lookup"><span data-stu-id="13819-110">In this case you will receive the following error when requesting pages from a given web application:</span></span>

![](denied-access-to-iis-directories/_static/image1.jpg)

<span data-ttu-id="13819-111">Aby rozwiązać ten problem, musisz zmienić uprawnienia zabezpieczeń do odpowiednich katalogów.</span><span class="sxs-lookup"><span data-stu-id="13819-111">To fix this you will need to change the security permissions on the appropriate directories.</span></span>

<span data-ttu-id="13819-112">W szczególności ASP.NET wymagane prawo do odczytu, wykonaj i listy dostępu dla konta ASPNET katalogu głównego witryny sieci web (na przykład: c:\inetpub\wwwroot lub dowolnego katalogu alternatywnych lokacji mogą mieć skonfigurowane w usługach IIS), katalog zawartości i katalog główny aplikacji Aby monitorować zmian w pliku konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="13819-112">Specifically, ASP.NET requires read, execute, and list access for the ASPNET account for the web site root (for example: c:\inetpub\wwwroot or any alternative site directory you may have configured in IIS), the content directory and the application root directory in order to monitor for configuration file changes.</span></span> <span data-ttu-id="13819-113">Katalog główny aplikacji odpowiada ścieżkę do folderu skojarzone z katalogiem wirtualnym aplikacji za pomocą narzędzia administracyjnego usług IIS (inetmgr).</span><span class="sxs-lookup"><span data-stu-id="13819-113">The application root corresponds to the folder path associated with the application virtual directory in the IIS Administration tool (inetmgr).</span></span>

<span data-ttu-id="13819-114">Rozważmy na przykład następująca hierarchia aplikacji w folderze wwwroot.</span><span class="sxs-lookup"><span data-stu-id="13819-114">For example, consider the following application hierarchy under the wwwroot folder.</span></span>

`C:\inetpub\wwwroot\myapp\default.aspx`

<span data-ttu-id="13819-115">Na przykład konto ASPNET wymaga uprawnień odczytu zdefiniowanych powyżej dla zawartości w Moja_aplikacja i katalogu wwwroot.</span><span class="sxs-lookup"><span data-stu-id="13819-115">For this example, the ASPNET account needs the read permissions defined above for content in both the myapp and the wwwroot directory.</span></span> <span data-ttu-id="13819-116">Pojedynczy dziedziczone listy ACL w folderze głównym Opcjonalnie można także dla obu katalogów, jeśli są one zagnieżdżone.</span><span class="sxs-lookup"><span data-stu-id="13819-116">A single inherited ACL on the root folder can also be optionally used for both directories if they're nested.</span></span>

<span data-ttu-id="13819-117">Aby dodać uprawnienia do katalogu, wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="13819-117">To add permissions to a directory, perform the following steps:</span></span>

- <span data-ttu-id="13819-118">Korzystając z Eksploratora Windows, przejdź do katalogu</span><span class="sxs-lookup"><span data-stu-id="13819-118">Using the Windows explorer, navigate to the directory</span></span>
- <span data-ttu-id="13819-119">Kliknij prawym przyciskiem myszy folder katalogu, a następnie wybierz pozycję "Właściwości"</span><span class="sxs-lookup"><span data-stu-id="13819-119">Right click on the directory folder and choose "Properties"</span></span>
- <span data-ttu-id="13819-120">Przejdź na kartę "Zabezpieczenia" w oknie dialogowym właściwości</span><span class="sxs-lookup"><span data-stu-id="13819-120">Navigate to the "Security" tab on the property dialog</span></span>
- <span data-ttu-id="13819-121">Kliknij przycisk "Dodaj" i wprowadź nazwę komputera, a następnie nazwę konta ASPNET.</span><span class="sxs-lookup"><span data-stu-id="13819-121">Click the "Add" button and enter the machine name followed by the ASPNET account name.</span></span> <span data-ttu-id="13819-122">Czy na przykład na komputerze o nazwie "webdev", wprowadź urzWeb\ASPNET i kliknij przycisk "OK".</span><span class="sxs-lookup"><span data-stu-id="13819-122">For example, on a machine named "webdev", you would enter webdev\ASPNET and hit "OK".</span></span>
- <span data-ttu-id="13819-123">Upewnij się, że konto ASPNET ma "odczytu &amp; Execute", "Wyświetlanie zawartości folderu" i "Odczyt" pola wyboru zaznaczone.</span><span class="sxs-lookup"><span data-stu-id="13819-123">Ensure that the ASPNET account has the "Read &amp; Execute", "List Folder Contents", and "Read" checkboxes checked.</span></span>
- <span data-ttu-id="13819-124">Kliknij przycisk OK, aby zamknąć okno dialogowe i zapisać zmiany.</span><span class="sxs-lookup"><span data-stu-id="13819-124">Hit OK to dismiss the dialog and save the changes.</span></span>

![](denied-access-to-iis-directories/_static/image2.jpg)

<span data-ttu-id="13819-125">W razie potrzeby, te zmiany można zautomatyzować za pomocą skryptów lub narzędzia "cacls.exe", która jest dostarczana z systemem Windows.</span><span class="sxs-lookup"><span data-stu-id="13819-125">If desired, these changes can be automated using scripts or the "cacls.exe" tool that ships with Windows.</span></span> <span data-ttu-id="13819-126">Aby uzyskać więcej informacji dotyczących konto ASPNET, zobacz [dokumentu — często zadawane pytania](https://go.microsoft.com/fwlink/?LinkId=5828).</span><span class="sxs-lookup"><span data-stu-id="13819-126">For more information on the ASPNET account, please see the [FAQ document](https://go.microsoft.com/fwlink/?LinkId=5828).</span></span>

<span data-ttu-id="13819-127">Jeśli aplikacja sieci web danego opiera się na potrzeby zapisu lub modyfikowania uprawnień do danego folderu lub pliku, może zostać przydzielony procedury i sprawdzając pola wyboru "Write" i/lub "Modyfikuj".</span><span class="sxs-lookup"><span data-stu-id="13819-127">If a given web application relies on having write or modify permissions to a particular folder or file, this can be granted by following the same procedure and checking the "Write" and/or "Modify" checkboxes.</span></span>

<span data-ttu-id="13819-128">Na maszynach, które umożliwia Wszyscy lub dostęp do odczytu grupy użytkowników do tych katalogów (co to jest konfiguracja domyślna), będzie napotkano brak problemów i powyższe kroki nie będą wymagane.</span><span class="sxs-lookup"><span data-stu-id="13819-128">On machines that allow Everyone or the Users group read access to these directories (which is the default configuration), no issues will be encountered and the above steps will not be required.</span></span>
