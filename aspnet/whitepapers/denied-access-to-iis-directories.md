---
uid: whitepapers/denied-access-to-iis-directories
title: Platforma ASP.NET odmawia dostępu do katalogów usług IIS | Dokumentacja firmy Microsoft
author: rick-anderson
description: Tym oficjalnym dokumencie opisano, co należy zrobić, jeśli żądanie do aplikacji platformy ASP.NET zwraca błąd "odmowa dostępu do katalogu DirectoryName. Nie udało się s...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: 3cb27b8a-354f-4332-bfe0-232b13bbf8aa
ms.technology: ''
msc.legacyurl: /whitepapers/denied-access-to-iis-directories
msc.type: content
ms.openlocfilehash: 22868738e02472f5f433c729967ac324131a0fdc
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37383065"
---
<a name="aspnet-denied-access-to-iis-directories"></a><span data-ttu-id="5179d-104">Platforma ASP.NET odmawia dostępu do katalogów usług IIS</span><span class="sxs-lookup"><span data-stu-id="5179d-104">ASP.NET Denied Access to IIS Directories</span></span>
====================
> <span data-ttu-id="5179d-105">Tym oficjalnym dokumencie opisano, co należy zrobić, jeśli żądanie do aplikacji platformy ASP.NET zwraca błąd, "odmowa dostępu do *DirectoryName* katalogu.</span><span class="sxs-lookup"><span data-stu-id="5179d-105">This whitepaper describes what you must do if a request to your ASP.NET application returns the error, "Denied Access to *DirectoryName* directory.</span></span> <span data-ttu-id="5179d-106">Nie można uruchomić monitorowania zmiany w katalogu".</span><span class="sxs-lookup"><span data-stu-id="5179d-106">Failed to start monitoring directory changes."</span></span>
> 
> <span data-ttu-id="5179d-107">Stosuje się do platformy ASP.NET w wersji 1.0 i 1.1 programu ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="5179d-107">Applies to ASP.NET 1.0 and ASP.NET 1.1.</span></span>


<span data-ttu-id="5179d-108">Teraz RTM programu ASP.NET w wersji 1 jest wykonywane przy użyciu mniej uprzywilejowanego konta systemu windows — zarejestrowany jako konto "ASPNET" na komputerze lokalnym.</span><span class="sxs-lookup"><span data-stu-id="5179d-108">ASP.NET V1 RTM now runs using a less privileged windows account - registered as the "ASPNET" account on a local machine.</span></span>

<span data-ttu-id="5179d-109">Na niektórych zablokowana systemów, to konto może domyślnie nie ma zabezpieczeń dostęp do odczytu katalogi z zawartością witryny sieci Web, katalog główny aplikacji lub katalogu głównego witryny sieci web.</span><span class="sxs-lookup"><span data-stu-id="5179d-109">On some locked down systems, this account may not by default have read security access to a website's content directories, the application root directory, or the web site root directory.</span></span> <span data-ttu-id="5179d-110">W takim przypadku zostanie wyświetlony następujący błąd podczas żądania strony z danej aplikacji sieci web:</span><span class="sxs-lookup"><span data-stu-id="5179d-110">In this case you will receive the following error when requesting pages from a given web application:</span></span>

![](denied-access-to-iis-directories/_static/image1.jpg)

<span data-ttu-id="5179d-111">Aby rozwiązać ten problem, musisz zmienić uprawnienia zabezpieczeń w odpowiednich katalogów.</span><span class="sxs-lookup"><span data-stu-id="5179d-111">To fix this you will need to change the security permissions on the appropriate directories.</span></span>

<span data-ttu-id="5179d-112">W szczególności platformy ASP.NET wymaga odczytu, wykonywanie i listy dostępu do konta ASPNET dla głównym witryny sieci web (na przykład: c:\inetpub\wwwroot lub dowolnego katalogu alternatywnych lokacji, może zostały skonfigurowane w usługach IIS), katalog zawartości i katalog główny aplikacji Aby można było monitorować zmian w pliku konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="5179d-112">Specifically, ASP.NET requires read, execute, and list access for the ASPNET account for the web site root (for example: c:\inetpub\wwwroot or any alternative site directory you may have configured in IIS), the content directory and the application root directory in order to monitor for configuration file changes.</span></span> <span data-ttu-id="5179d-113">Katalog główny aplikacji odnosi się do ścieżki folderu skojarzony z katalogiem wirtualnym aplikacji za pomocą narzędzia administracyjnego usług IIS (inetmgr).</span><span class="sxs-lookup"><span data-stu-id="5179d-113">The application root corresponds to the folder path associated with the application virtual directory in the IIS Administration tool (inetmgr).</span></span>

<span data-ttu-id="5179d-114">Na przykład należy wziąć pod uwagę następujące hierarchii aplikacji w folderze wwwroot.</span><span class="sxs-lookup"><span data-stu-id="5179d-114">For example, consider the following application hierarchy under the wwwroot folder.</span></span>

`C:\inetpub\wwwroot\myapp\default.aspx`

<span data-ttu-id="5179d-115">W tym przykładzie konto ASPNET wymaga uprawnień odczytu zdefiniowanych powyżej dla zawartości zarówno w myapp, jak i w katalogu wwwroot.</span><span class="sxs-lookup"><span data-stu-id="5179d-115">For this example, the ASPNET account needs the read permissions defined above for content in both the myapp and the wwwroot directory.</span></span> <span data-ttu-id="5179d-116">Pojedynczy dziedziczone listy ACL dla folderu głównego Opcjonalnie można także w przypadku obu katalogów, jeśli są one zagnieżdżone.</span><span class="sxs-lookup"><span data-stu-id="5179d-116">A single inherited ACL on the root folder can also be optionally used for both directories if they're nested.</span></span>

<span data-ttu-id="5179d-117">Aby dodać uprawnienia do katalogu, wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="5179d-117">To add permissions to a directory, perform the following steps:</span></span>

- <span data-ttu-id="5179d-118">W Eksploratorze Windows przejdź do katalogu</span><span class="sxs-lookup"><span data-stu-id="5179d-118">Using the Windows explorer, navigate to the directory</span></span>
- <span data-ttu-id="5179d-119">Kliknij prawym przyciskiem folder katalogu i wybierz pozycję "Właściwości"</span><span class="sxs-lookup"><span data-stu-id="5179d-119">Right click on the directory folder and choose "Properties"</span></span>
- <span data-ttu-id="5179d-120">Przejdź do karty "Zabezpieczenia" w oknie dialogowym właściwości</span><span class="sxs-lookup"><span data-stu-id="5179d-120">Navigate to the "Security" tab on the property dialog</span></span>
- <span data-ttu-id="5179d-121">Kliknij przycisk "Dodaj" i wprowadź nazwę komputera, a następnie według nazwy konta ASPNET.</span><span class="sxs-lookup"><span data-stu-id="5179d-121">Click the "Add" button and enter the machine name followed by the ASPNET account name.</span></span> <span data-ttu-id="5179d-122">Na przykład na komputerze o nazwie "webdev", może wprowadzić urzWeb\ASPNET i kliknę przycisk "OK".</span><span class="sxs-lookup"><span data-stu-id="5179d-122">For example, on a machine named "webdev", you would enter webdev\ASPNET and hit "OK".</span></span>
- <span data-ttu-id="5179d-123">Upewnij się, że konto ASPNET ma "odczytu &amp; wykonania", "Wyświetlanie zawartości folderu" i "Odczyt" odpowiednie pola wyboru zaznaczone.</span><span class="sxs-lookup"><span data-stu-id="5179d-123">Ensure that the ASPNET account has the "Read &amp; Execute", "List Folder Contents", and "Read" checkboxes checked.</span></span>
- <span data-ttu-id="5179d-124">Kliknę przycisk OK, aby zamknąć okno dialogowe i zapisać zmiany.</span><span class="sxs-lookup"><span data-stu-id="5179d-124">Hit OK to dismiss the dialog and save the changes.</span></span>

![](denied-access-to-iis-directories/_static/image2.jpg)

<span data-ttu-id="5179d-125">Jeśli to konieczne, te zmiany można zautomatyzować za pomocą skryptów lub narzędzi "cacls.exe", który jest dostarczany z Windows.</span><span class="sxs-lookup"><span data-stu-id="5179d-125">If desired, these changes can be automated using scripts or the "cacls.exe" tool that ships with Windows.</span></span> <span data-ttu-id="5179d-126">Aby uzyskać więcej informacji na temat konto ASPNET, zobacz [dokumentu — często zadawane pytania](https://go.microsoft.com/fwlink/?LinkId=5828).</span><span class="sxs-lookup"><span data-stu-id="5179d-126">For more information on the ASPNET account, please see the [FAQ document](https://go.microsoft.com/fwlink/?LinkId=5828).</span></span>

<span data-ttu-id="5179d-127">Jeśli danej aplikacji sieci web opiera się na potrzeby zapisu lub modyfikować uprawnienia do określonego folderu lub pliku, mogą być udzielane zgodnie z tą samą procedurą, a następnie zaznaczając pole wyboru "Write" i/lub "Modyfikuj".</span><span class="sxs-lookup"><span data-stu-id="5179d-127">If a given web application relies on having write or modify permissions to a particular folder or file, this can be granted by following the same procedure and checking the "Write" and/or "Modify" checkboxes.</span></span>

<span data-ttu-id="5179d-128">W przypadku maszyn, które zezwalają na wszystkich użytkowników lub dostęp do odczytu grupę użytkowników do tych katalogów, (które jest konfiguracja domyślna), zostanie napotkana żadnych problemów i powyższe kroki nie będą wymagane.</span><span class="sxs-lookup"><span data-stu-id="5179d-128">On machines that allow Everyone or the Users group read access to these directories (which is the default configuration), no issues will be encountered and the above steps will not be required.</span></span>
