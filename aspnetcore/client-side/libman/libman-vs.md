---
title: Używanie LibMan z ASP.NET Core w programie Visual Studio
author: scottaddie
description: Dowiedz się, jak używać LibMan w projekcie ASP.NET Core z programem Visual Studio.
ms.author: scaddie
ms.custom: mvc
ms.date: 08/20/2018
uid: client-side/libman/libman-vs
ms.openlocfilehash: e92e6bc28ec58b26785dd6c79e71512368202a26
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78658313"
---
# <a name="use-libman-with-aspnet-core-in-visual-studio"></a><span data-ttu-id="ddedb-103">Używanie LibMan z ASP.NET Core w programie Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ddedb-103">Use LibMan with ASP.NET Core in Visual Studio</span></span>

<span data-ttu-id="ddedb-104">Przez [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="ddedb-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="ddedb-105">Program Visual Studio ma wbudowaną obsługę [LibMan](xref:client-side/libman/index) w projektach ASP.NET Core, w tym:</span><span class="sxs-lookup"><span data-stu-id="ddedb-105">Visual Studio has built-in support for [LibMan](xref:client-side/libman/index) in ASP.NET Core projects, including:</span></span>

* <span data-ttu-id="ddedb-106">Obsługa konfigurowania i uruchamiania operacji przywracania LibMan podczas kompilacji.</span><span class="sxs-lookup"><span data-stu-id="ddedb-106">Support for configuring and running LibMan restore operations on build.</span></span>
* <span data-ttu-id="ddedb-107">Elementy menu służące do wyzwalania operacji przywracania i czyszczenia LibMan.</span><span class="sxs-lookup"><span data-stu-id="ddedb-107">Menu items for triggering LibMan restore and clean operations.</span></span>
* <span data-ttu-id="ddedb-108">Okno dialogowe wyszukiwania służące do znajdowania bibliotek i dodawania plików do projektu.</span><span class="sxs-lookup"><span data-stu-id="ddedb-108">Search dialog for finding libraries and adding the files to a project.</span></span>
* <span data-ttu-id="ddedb-109">Edytowanie obsługi *Libman. json*&mdash;pliku manifestu Libman.</span><span class="sxs-lookup"><span data-stu-id="ddedb-109">Editing support for *libman.json*&mdash;the LibMan manifest file.</span></span>

<span data-ttu-id="ddedb-110">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/client-side/libman/samples/) [(jak pobrać)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="ddedb-110">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/client-side/libman/samples/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ddedb-111">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="ddedb-111">Prerequisites</span></span>

* <span data-ttu-id="ddedb-112">[Program Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) z **ASP.NET i programowaniem aplikacji sieci Web**</span><span class="sxs-lookup"><span data-stu-id="ddedb-112">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) with the **ASP.NET and web development** workload</span></span>

## <a name="add-library-files"></a><span data-ttu-id="ddedb-113">Dodaj pliki biblioteki</span><span class="sxs-lookup"><span data-stu-id="ddedb-113">Add library files</span></span>

<span data-ttu-id="ddedb-114">Pliki bibliotek można dodać do projektu ASP.NET Core na dwa różne sposoby:</span><span class="sxs-lookup"><span data-stu-id="ddedb-114">Library files can be added to an ASP.NET Core project in two different ways:</span></span>

1. [<span data-ttu-id="ddedb-115">Korzystanie z okna dialogowego Dodawanie biblioteki po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="ddedb-115">Use the Add Client-Side Library dialog</span></span>](#use-the-add-client-side-library-dialog)
1. [<span data-ttu-id="ddedb-116">Ręczne konfigurowanie wpisów pliku manifestu LibMan</span><span class="sxs-lookup"><span data-stu-id="ddedb-116">Manually configure LibMan manifest file entries</span></span>](#manually-configure-libman-manifest-file-entries)

### <a name="use-the-add-client-side-library-dialog"></a><span data-ttu-id="ddedb-117">Korzystanie z okna dialogowego Dodawanie biblioteki po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="ddedb-117">Use the Add Client-Side Library dialog</span></span>

<span data-ttu-id="ddedb-118">Wykonaj następujące kroki, aby zainstalować bibliotekę po stronie klienta:</span><span class="sxs-lookup"><span data-stu-id="ddedb-118">Follow these steps to install a client-side library:</span></span>

* <span data-ttu-id="ddedb-119">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy folder projektu, w którym należy dodać pliki.</span><span class="sxs-lookup"><span data-stu-id="ddedb-119">In **Solution Explorer**, right-click the project folder in which the files should be added.</span></span> <span data-ttu-id="ddedb-120">Wybierz pozycję **dodaj** > **bibliotekę po stronie klienta**.</span><span class="sxs-lookup"><span data-stu-id="ddedb-120">Choose **Add** > **Client-Side Library**.</span></span> <span data-ttu-id="ddedb-121">Zostanie wyświetlone okno dialogowe **Dodawanie biblioteki po stronie klienta** :</span><span class="sxs-lookup"><span data-stu-id="ddedb-121">The **Add Client-Side Library** dialog appears:</span></span>

  ![Okno dialogowe Dodawanie biblioteki po stronie klienta](_static/add-library-dialog.png)

* <span data-ttu-id="ddedb-123">Wybierz dostawcę biblioteki z listy rozwijanej **dostawca** .</span><span class="sxs-lookup"><span data-stu-id="ddedb-123">Select the library provider from the **Provider** drop down.</span></span> <span data-ttu-id="ddedb-124">CDNJS jest dostawcą domyślnym.</span><span class="sxs-lookup"><span data-stu-id="ddedb-124">CDNJS is the default provider.</span></span>
* <span data-ttu-id="ddedb-125">Wpisz nazwę biblioteki do pobrania w polu tekstowym **Biblioteka** .</span><span class="sxs-lookup"><span data-stu-id="ddedb-125">Type the library name to fetch in the **Library** text box.</span></span> <span data-ttu-id="ddedb-126">Technologia IntelliSense udostępnia listę bibliotek zaczynających się od podanego tekstu.</span><span class="sxs-lookup"><span data-stu-id="ddedb-126">IntelliSense provides a list of libraries beginning with the provided text.</span></span>
* <span data-ttu-id="ddedb-127">Wybierz bibliotekę z listy IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="ddedb-127">Select the library from the IntelliSense list.</span></span> <span data-ttu-id="ddedb-128">Zwróć uwagę na to, że nazwa biblioteki jest poddana sufiksowi `@` i najnowszej stabilnej wersji znanej dla wybranego dostawcy.</span><span class="sxs-lookup"><span data-stu-id="ddedb-128">Notice the library name is suffixed with the `@` symbol and the latest stable version known to the selected provider.</span></span>
* <span data-ttu-id="ddedb-129">Wybieranie plików do uwzględnienia:</span><span class="sxs-lookup"><span data-stu-id="ddedb-129">Decide which files to include:</span></span>
  * <span data-ttu-id="ddedb-130">Zaznacz przycisk radiowy **Dołącz wszystkie pliki bibliotek** , aby uwzględnić wszystkie pliki biblioteki.</span><span class="sxs-lookup"><span data-stu-id="ddedb-130">Select the **Include all library files** radio button to include all of the library's files.</span></span>
  * <span data-ttu-id="ddedb-131">Wybierz przycisk radiowy **Wybierz określone pliki** , aby dołączyć podzestaw plików biblioteki.</span><span class="sxs-lookup"><span data-stu-id="ddedb-131">Select the **Choose specific files** radio button to include a subset of the library's files.</span></span> <span data-ttu-id="ddedb-132">Po wybraniu przycisku radiowego drzewo selektora plików jest włączone.</span><span class="sxs-lookup"><span data-stu-id="ddedb-132">When the radio button is selected, the file selector tree is enabled.</span></span> <span data-ttu-id="ddedb-133">Zaznacz pola po lewej stronie nazw plików do pobrania.</span><span class="sxs-lookup"><span data-stu-id="ddedb-133">Check the boxes to the left of the file names to download.</span></span>
* <span data-ttu-id="ddedb-134">Określ folder projektu do przechowywania plików w polu tekstowym **Lokalizacja docelowa** .</span><span class="sxs-lookup"><span data-stu-id="ddedb-134">Specify the project folder for storing the files in the **Target Location** text box.</span></span> <span data-ttu-id="ddedb-135">Zgodnie z zaleceniami należy przechowywać każdą bibliotekę w osobnym folderze.</span><span class="sxs-lookup"><span data-stu-id="ddedb-135">As a recommendation, store each library in a separate folder.</span></span>

  <span data-ttu-id="ddedb-136">Sugerowany folder **lokalizacji docelowej** jest oparty na lokalizacji, w której uruchomiono okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="ddedb-136">The suggested **Target Location** folder is based on the location from which the dialog launched:</span></span>

  * <span data-ttu-id="ddedb-137">Jeśli jest uruchamiany z poziomu głównego projektu:</span><span class="sxs-lookup"><span data-stu-id="ddedb-137">If launched from the project root:</span></span>
    * <span data-ttu-id="ddedb-138">plik *wwwroot/lib* jest używany, jeśli istnieje plik *wwwroot* .</span><span class="sxs-lookup"><span data-stu-id="ddedb-138">*wwwroot/lib* is used if *wwwroot* exists.</span></span>
    * <span data-ttu-id="ddedb-139">*Biblioteka lib* jest używana, jeśli plik *wwwroot* nie istnieje.</span><span class="sxs-lookup"><span data-stu-id="ddedb-139">*lib* is used if *wwwroot* doesn't exist.</span></span>
  * <span data-ttu-id="ddedb-140">W przypadku uruchomienia z folderu projektu zostanie użyta odpowiednia nazwa folderu.</span><span class="sxs-lookup"><span data-stu-id="ddedb-140">If launched from a project folder, the corresponding folder name is used.</span></span>

  <span data-ttu-id="ddedb-141">Sugestia folderu jest sufiksem z nazwą biblioteki.</span><span class="sxs-lookup"><span data-stu-id="ddedb-141">The folder suggestion is suffixed with the library name.</span></span> <span data-ttu-id="ddedb-142">W poniższej tabeli przedstawiono sugestie dotyczące folderów podczas instalowania jQuery w projekcie Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="ddedb-142">The following table illustrates folder suggestions when installing jQuery in a Razor Pages project.</span></span>
  
  |<span data-ttu-id="ddedb-143">Lokalizacja uruchamiania</span><span class="sxs-lookup"><span data-stu-id="ddedb-143">Launch location</span></span>                           |<span data-ttu-id="ddedb-144">Sugerowany folder</span><span class="sxs-lookup"><span data-stu-id="ddedb-144">Suggested folder</span></span>      |
  |------------------------------------------|----------------------|
  |<span data-ttu-id="ddedb-145">Katalog główny projektu (Jeśli folder *wwwroot* istnieje)</span><span class="sxs-lookup"><span data-stu-id="ddedb-145">project root (if *wwwroot* exists)</span></span>        |<span data-ttu-id="ddedb-146">*wwwroot/lib/jQuery/*</span><span class="sxs-lookup"><span data-stu-id="ddedb-146">*wwwroot/lib/jquery/*</span></span> |
  |<span data-ttu-id="ddedb-147">Katalog główny projektu (Jeśli folder *wwwroot* nie istnieje)</span><span class="sxs-lookup"><span data-stu-id="ddedb-147">project root (if *wwwroot* doesn't exist)</span></span> |<span data-ttu-id="ddedb-148">*lib/jQuery/*</span><span class="sxs-lookup"><span data-stu-id="ddedb-148">*lib/jquery/*</span></span>         |
  |<span data-ttu-id="ddedb-149">Folder *stron* w projekcie</span><span class="sxs-lookup"><span data-stu-id="ddedb-149">*Pages* folder in project</span></span>                 |<span data-ttu-id="ddedb-150">*Strony/jQuery/*</span><span class="sxs-lookup"><span data-stu-id="ddedb-150">*Pages/jquery/*</span></span>       |

* <span data-ttu-id="ddedb-151">Kliknij przycisk **Instaluj** , aby pobrać pliki zgodnie z konfiguracją w pliku *Libman. JSON*.</span><span class="sxs-lookup"><span data-stu-id="ddedb-151">Click the **Install** button to download the files, per the configuration in *libman.json*.</span></span>
* <span data-ttu-id="ddedb-152">Zapoznaj się z informacjami dotyczącymi instalacji w oknie **danych wyjściowych** programu **Library Manager** .</span><span class="sxs-lookup"><span data-stu-id="ddedb-152">Review the **Library Manager** feed of the **Output** window for installation details.</span></span> <span data-ttu-id="ddedb-153">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="ddedb-153">For example:</span></span>

  ```console
  Restore operation started...
  Restoring libraries for project LibManSample
  Restoring library jquery@3.3.1... (LibManSample)
  wwwroot/lib/jquery/jquery.min.js written to destination (LibManSample)
  wwwroot/lib/jquery/jquery.js written to destination (LibManSample)
  wwwroot/lib/jquery/jquery.min.map written to destination (LibManSample)
  Restore operation completed
  1 libraries restored in 2.32 seconds
  ```

### <a name="manually-configure-libman-manifest-file-entries"></a><span data-ttu-id="ddedb-154">Ręczne konfigurowanie wpisów pliku manifestu LibMan</span><span class="sxs-lookup"><span data-stu-id="ddedb-154">Manually configure LibMan manifest file entries</span></span>

<span data-ttu-id="ddedb-155">Wszystkie operacje LibMan w programie Visual Studio są oparte na zawartości manifestu LibMan elementu głównego projektu (*LibMan. JSON*).</span><span class="sxs-lookup"><span data-stu-id="ddedb-155">All LibMan operations in Visual Studio are based on the content of the project root's LibMan manifest (*libman.json*).</span></span> <span data-ttu-id="ddedb-156">Można ręcznie edytować plik *Libman. JSON* w celu skonfigurowania plików biblioteki dla projektu.</span><span class="sxs-lookup"><span data-stu-id="ddedb-156">You can manually edit *libman.json* to configure library files for the project.</span></span> <span data-ttu-id="ddedb-157">Program Visual Studio przywraca wszystkie pliki bibliotek po zapisaniu pliku *Libman. JSON* .</span><span class="sxs-lookup"><span data-stu-id="ddedb-157">Visual Studio restores all library files once *libman.json* is saved.</span></span>

<span data-ttu-id="ddedb-158">Aby otworzyć plik *Libman. JSON* do edycji, istnieją następujące opcje:</span><span class="sxs-lookup"><span data-stu-id="ddedb-158">To open *libman.json* for editing, the following options exist:</span></span>

* <span data-ttu-id="ddedb-159">Kliknij dwukrotnie plik *Libman. JSON* w **Eksplorator rozwiązań**.</span><span class="sxs-lookup"><span data-stu-id="ddedb-159">Double-click the *libman.json* file in **Solution Explorer**.</span></span>
* <span data-ttu-id="ddedb-160">Kliknij prawym przyciskiem myszy projekt w **Eksplorator rozwiązań** i wybierz pozycję **Zarządzaj bibliotekami po stronie klienta**.</span><span class="sxs-lookup"><span data-stu-id="ddedb-160">Right-click the project in **Solution Explorer** and select **Manage Client-Side Libraries**.</span></span> <span data-ttu-id="ddedb-161">**&#8224;**</span><span class="sxs-lookup"><span data-stu-id="ddedb-161">**&#8224;**</span></span>
* <span data-ttu-id="ddedb-162">Wybierz pozycję **Zarządzaj bibliotekami po stronie klienta** z menu **projektu** programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ddedb-162">Select **Manage Client-Side Libraries** from the Visual Studio **Project** menu.</span></span> <span data-ttu-id="ddedb-163">**&#8224;**</span><span class="sxs-lookup"><span data-stu-id="ddedb-163">**&#8224;**</span></span>

<span data-ttu-id="ddedb-164">**&#8224;** Jeśli plik *Libman. JSON* nie istnieje już w katalogu głównym projektu, zostanie utworzony przy użyciu domyślnej zawartości szablonu elementu.</span><span class="sxs-lookup"><span data-stu-id="ddedb-164">**&#8224;** If the *libman.json* file doesn't already exist in the project root, it will be created with the default item template content.</span></span>

<span data-ttu-id="ddedb-165">Program Visual Studio oferuje zaawansowane funkcje edycji JSON, takie jak kolorowanie, formatowanie, IntelliSense i walidacja schematu.</span><span class="sxs-lookup"><span data-stu-id="ddedb-165">Visual Studio offers rich JSON editing support such as colorization, formatting, IntelliSense, and schema validation.</span></span> <span data-ttu-id="ddedb-166">Schemat JSON manifestu LibMan został znaleziony w [https://json.schemastore.org/libman](https://json.schemastore.org/libman).</span><span class="sxs-lookup"><span data-stu-id="ddedb-166">The LibMan manifest's JSON schema is found at [https://json.schemastore.org/libman](https://json.schemastore.org/libman).</span></span>

<span data-ttu-id="ddedb-167">Przy użyciu następującego pliku manifestu LibMan pobiera pliki według konfiguracji zdefiniowanej we właściwości `libraries`.</span><span class="sxs-lookup"><span data-stu-id="ddedb-167">With the following manifest file, LibMan retrieves files per the configuration defined in the `libraries` property.</span></span> <span data-ttu-id="ddedb-168">Wyjaśnienie literałów obiektów zdefiniowanych w `libraries` następujące:</span><span class="sxs-lookup"><span data-stu-id="ddedb-168">An explanation of the object literals defined within `libraries` follows:</span></span>

* <span data-ttu-id="ddedb-169">Podzestaw [jQuery](https://jquery.com/) w wersji 3.3.1 jest pobierany z dostawcy CDNJS.</span><span class="sxs-lookup"><span data-stu-id="ddedb-169">A subset of [jQuery](https://jquery.com/) version 3.3.1 is retrieved from the CDNJS provider.</span></span> <span data-ttu-id="ddedb-170">Podzestaw jest zdefiniowany we właściwości `files`&mdash;*jQuery. min. js*, *jQuery. js*i *jQuery. min. map*.</span><span class="sxs-lookup"><span data-stu-id="ddedb-170">The subset is defined in the `files` property&mdash;*jquery.min.js*, *jquery.js*, and *jquery.min.map*.</span></span> <span data-ttu-id="ddedb-171">Pliki są umieszczane w folderze *wwwroot/lib/jQuery* projektu.</span><span class="sxs-lookup"><span data-stu-id="ddedb-171">The files are placed in the project's *wwwroot/lib/jquery* folder.</span></span>
* <span data-ttu-id="ddedb-172">W [całości wersja 4.1.3](https://getbootstrap.com/) jest pobierana i umieszczana w folderze *wwwroot/lib/Bootstrap* .</span><span class="sxs-lookup"><span data-stu-id="ddedb-172">The entirety of [Bootstrap](https://getbootstrap.com/) version 4.1.3 is retrieved and placed in a *wwwroot/lib/bootstrap* folder.</span></span> <span data-ttu-id="ddedb-173">Właściwość `provider` literału obiektu zastępuje wartość właściwości `defaultProvider`.</span><span class="sxs-lookup"><span data-stu-id="ddedb-173">The object literal's `provider` property overrides the `defaultProvider` property value.</span></span> <span data-ttu-id="ddedb-174">LibMan pobiera pliki Bootstrap z dostawcy unpkg.</span><span class="sxs-lookup"><span data-stu-id="ddedb-174">LibMan retrieves the Bootstrap files from the unpkg provider.</span></span>
* <span data-ttu-id="ddedb-175">Podzbiór [Lodash](https://lodash.com/) został zatwierdzony przez organ regulujący w organizacji.</span><span class="sxs-lookup"><span data-stu-id="ddedb-175">A subset of [Lodash](https://lodash.com/) was approved by a governing body within the organization.</span></span> <span data-ttu-id="ddedb-176">*Lodash. js* i *lodash. min. js* są pobierane z lokalnego systemu plików w lokalizacji *C:\\temp\\lodash\\* .</span><span class="sxs-lookup"><span data-stu-id="ddedb-176">The *lodash.js* and *lodash.min.js* files are retrieved from the local file system at *C:\\temp\\lodash\\*.</span></span> <span data-ttu-id="ddedb-177">Pliki są kopiowane do folderu *wwwroot/lib/lodash* projektu.</span><span class="sxs-lookup"><span data-stu-id="ddedb-177">The files are copied to the project's *wwwroot/lib/lodash* folder.</span></span>

[!code-json[](samples/LibManSample/libman.json)]

> [!NOTE]
> <span data-ttu-id="ddedb-178">LibMan obsługuje tylko jedną wersję każdej biblioteki z każdego dostawcy.</span><span class="sxs-lookup"><span data-stu-id="ddedb-178">LibMan only supports one version of each library from each provider.</span></span> <span data-ttu-id="ddedb-179">Walidacja schematu pliku *Libman. JSON* kończy się niepowodzeniem, jeśli zawiera dwie biblioteki o tej samej nazwie biblioteki dla danego dostawcy.</span><span class="sxs-lookup"><span data-stu-id="ddedb-179">The *libman.json* file fails schema validation if it contains two libraries with the same library name for a given provider.</span></span>

## <a name="restore-library-files"></a><span data-ttu-id="ddedb-180">Przywróć pliki biblioteki</span><span class="sxs-lookup"><span data-stu-id="ddedb-180">Restore library files</span></span>

<span data-ttu-id="ddedb-181">Aby przywrócić pliki bibliotek z programu Visual Studio, w katalogu głównym projektu musi istnieć prawidłowy plik *Libman. JSON* .</span><span class="sxs-lookup"><span data-stu-id="ddedb-181">To restore library files from within Visual Studio, there must be a valid *libman.json* file in the project root.</span></span> <span data-ttu-id="ddedb-182">Przywrócone pliki są umieszczane w projekcie w lokalizacji określonej dla każdej biblioteki.</span><span class="sxs-lookup"><span data-stu-id="ddedb-182">Restored files are placed in the project at the location specified for each library.</span></span>

<span data-ttu-id="ddedb-183">Pliki bibliotek można przywrócić w projekcie ASP.NET Core na dwa sposoby:</span><span class="sxs-lookup"><span data-stu-id="ddedb-183">Library files can be restored in an ASP.NET Core project in two ways:</span></span>

1. [<span data-ttu-id="ddedb-184">Przywróć pliki podczas kompilacji</span><span class="sxs-lookup"><span data-stu-id="ddedb-184">Restore files during build</span></span>](#restore-files-during-build)
1. [<span data-ttu-id="ddedb-185">Ręczne przywracanie plików</span><span class="sxs-lookup"><span data-stu-id="ddedb-185">Restore files manually</span></span>](#restore-files-manually)

### <a name="restore-files-during-build"></a><span data-ttu-id="ddedb-186">Przywróć pliki podczas kompilacji</span><span class="sxs-lookup"><span data-stu-id="ddedb-186">Restore files during build</span></span>

<span data-ttu-id="ddedb-187">LibMan może przywrócić zdefiniowane pliki biblioteki w ramach procesu kompilacji.</span><span class="sxs-lookup"><span data-stu-id="ddedb-187">LibMan can restore the defined library files as part of the build process.</span></span> <span data-ttu-id="ddedb-188">Domyślnie zachowanie funkcji *Przywróć przy kompilacji* jest wyłączone.</span><span class="sxs-lookup"><span data-stu-id="ddedb-188">By default, the *restore-on-build* behavior is disabled.</span></span>

<span data-ttu-id="ddedb-189">Aby włączyć i przetestować zachowanie funkcji przywracania po kompilacji:</span><span class="sxs-lookup"><span data-stu-id="ddedb-189">To enable and test the restore-on-build behavior:</span></span>

* <span data-ttu-id="ddedb-190">Kliknij prawym przyciskiem myszy plik *Libman. JSON* w **Eksplorator rozwiązań** i wybierz opcję **Włącz przywracanie bibliotek po stronie klienta w kompilacji** z menu kontekstowego.</span><span class="sxs-lookup"><span data-stu-id="ddedb-190">Right-click *libman.json* in **Solution Explorer** and select **Enable Restore Client-Side Libraries on Build** from the context menu.</span></span>
* <span data-ttu-id="ddedb-191">Po wyświetleniu monitu o zainstalowanie pakietu NuGet kliknij przycisk **tak** .</span><span class="sxs-lookup"><span data-stu-id="ddedb-191">Click the **Yes** button when prompted to install a NuGet package.</span></span> <span data-ttu-id="ddedb-192">Pakiet NuGet [Microsoft. Web. librarymanager. Build](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Build/) został dodany do projektu:</span><span class="sxs-lookup"><span data-stu-id="ddedb-192">The [Microsoft.Web.LibraryManager.Build](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Build/) NuGet package is added to the project:</span></span>

  [!code-xml[](samples/LibManSample/LibManSample.csproj?name=snippet_RestoreOnBuildPackage)]

* <span data-ttu-id="ddedb-193">Skompiluj projekt, aby upewnić się, że następuje przywracanie pliku LibMan.</span><span class="sxs-lookup"><span data-stu-id="ddedb-193">Build the project to confirm LibMan file restoration occurs.</span></span> <span data-ttu-id="ddedb-194">Pakiet `Microsoft.Web.LibraryManager.Build` wprowadza obiekt docelowy MSBuild, który uruchamia LibMan podczas operacji kompilowania projektu.</span><span class="sxs-lookup"><span data-stu-id="ddedb-194">The `Microsoft.Web.LibraryManager.Build` package injects an MSBuild target that runs LibMan during the project's build operation.</span></span>
* <span data-ttu-id="ddedb-195">Przejrzyj źródło danych **kompilacji** w oknie **danych wyjściowych** dla dziennika aktywności LibMan:</span><span class="sxs-lookup"><span data-stu-id="ddedb-195">Review the **Build** feed of the **Output** window for a LibMan activity log:</span></span>

  ```console
  1>------ Build started: Project: LibManSample, Configuration: Debug Any CPU ------
  1>
  1>Restore operation started...
  1>Restoring library jquery@3.3.1...
  1>Restoring library bootstrap@4.1.3...
  1>
  1>2 libraries restored in 10.66 seconds
  1>LibManSample -> C:\LibManSample\bin\Debug\netcoreapp2.1\LibManSample.dll
  ========== Build: 1 succeeded, 0 failed, 0 up-to-date, 0 skipped ==========
  ```

<span data-ttu-id="ddedb-196">Gdy włączone jest zachowanie przywracania po kompilacji, w menu kontekstowym *Libman. JSON* zostanie wyświetlona opcja **Wyłącz Przywracanie bibliotek po stronie klienta w ramach kompilacji** .</span><span class="sxs-lookup"><span data-stu-id="ddedb-196">When the restore-on-build behavior is enabled, the *libman.json* context menu displays a **Disable Restore Client-Side Libraries on Build** option.</span></span> <span data-ttu-id="ddedb-197">Wybranie tej opcji spowoduje usunięcie odwołania do pakietu `Microsoft.Web.LibraryManager.Build` z pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="ddedb-197">Selecting this option removes the `Microsoft.Web.LibraryManager.Build` package reference from the project file.</span></span> <span data-ttu-id="ddedb-198">W związku z tym biblioteki po stronie klienta nie są już przywracane dla każdej kompilacji.</span><span class="sxs-lookup"><span data-stu-id="ddedb-198">Consequently, the client-side libraries are no longer restored on each build.</span></span>

<span data-ttu-id="ddedb-199">Niezależnie od ustawienia Przywróć na kompilację można ręcznie przywrócić w dowolnym momencie z menu kontekstowego *Libman. JSON* .</span><span class="sxs-lookup"><span data-stu-id="ddedb-199">Regardless of the restore-on-build setting, you can manually restore at any time from the *libman.json* context menu.</span></span> <span data-ttu-id="ddedb-200">Aby uzyskać więcej informacji, zobacz [Przywracanie plików ręcznie](#restore-files-manually).</span><span class="sxs-lookup"><span data-stu-id="ddedb-200">For more information, see [Restore files manually](#restore-files-manually).</span></span>

### <a name="restore-files-manually"></a><span data-ttu-id="ddedb-201">Ręczne przywracanie plików</span><span class="sxs-lookup"><span data-stu-id="ddedb-201">Restore files manually</span></span>

<span data-ttu-id="ddedb-202">Aby ręcznie przywrócić pliki biblioteki:</span><span class="sxs-lookup"><span data-stu-id="ddedb-202">To manually restore library files:</span></span>

* <span data-ttu-id="ddedb-203">Dla wszystkich projektów w rozwiązaniu:</span><span class="sxs-lookup"><span data-stu-id="ddedb-203">For all projects in the solution:</span></span>
  * <span data-ttu-id="ddedb-204">Kliknij prawym przyciskiem myszy nazwę rozwiązania w **Eksplorator rozwiązań**.</span><span class="sxs-lookup"><span data-stu-id="ddedb-204">Right-click the solution name in **Solution Explorer**.</span></span>
  * <span data-ttu-id="ddedb-205">Wybierz opcję **Przywróć biblioteki po stronie klienta** .</span><span class="sxs-lookup"><span data-stu-id="ddedb-205">Select the **Restore Client-Side Libraries** option.</span></span>
* <span data-ttu-id="ddedb-206">Dla określonego projektu:</span><span class="sxs-lookup"><span data-stu-id="ddedb-206">For a specific project:</span></span>
  * <span data-ttu-id="ddedb-207">Kliknij prawym przyciskiem myszy plik *Libman. JSON* w **Eksplorator rozwiązań**.</span><span class="sxs-lookup"><span data-stu-id="ddedb-207">Right-click the *libman.json* file in **Solution Explorer**.</span></span>
  * <span data-ttu-id="ddedb-208">Wybierz opcję **Przywróć biblioteki po stronie klienta** .</span><span class="sxs-lookup"><span data-stu-id="ddedb-208">Select the **Restore Client-Side Libraries** option.</span></span>

<span data-ttu-id="ddedb-209">Podczas gdy operacja przywracania jest uruchomiona:</span><span class="sxs-lookup"><span data-stu-id="ddedb-209">While the restore operation is running:</span></span>

* <span data-ttu-id="ddedb-210">Ikona centrum stanu zadań (TSC) na pasku stanu programu Visual Studio zostanie animowana i zostanie *rozpoczęta operacja przywracania*.</span><span class="sxs-lookup"><span data-stu-id="ddedb-210">The Task Status Center (TSC) icon on the Visual Studio status bar will be animated and will read *Restore operation started*.</span></span> <span data-ttu-id="ddedb-211">Kliknięcie ikony powoduje otwarcie etykietki narzędzia zawierającego listę znanych zadań w tle.</span><span class="sxs-lookup"><span data-stu-id="ddedb-211">Clicking the icon opens a tooltip listing the known background tasks.</span></span>
* <span data-ttu-id="ddedb-212">Komunikaty będą wysyłane do paska stanu i źródła danych programu **Library Manager** okna **danych wyjściowych** .</span><span class="sxs-lookup"><span data-stu-id="ddedb-212">Messages will be sent to the status bar and the **Library Manager** feed of the **Output** window.</span></span> <span data-ttu-id="ddedb-213">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="ddedb-213">For example:</span></span>

  ```console
  Restore operation started...
  Restoring libraries for project LibManSample
  Restoring library jquery@3.3.1... (LibManSample)
  wwwroot/lib/jquery/jquery.min.js written to destination (LibManSample)
  wwwroot/lib/jquery/jquery.js written to destination (LibManSample)
  wwwroot/lib/jquery/jquery.min.map written to destination (LibManSample)
  Restore operation completed
  1 libraries restored in 2.32 seconds
  ```

## <a name="delete-library-files"></a><span data-ttu-id="ddedb-214">Usuń pliki biblioteki</span><span class="sxs-lookup"><span data-stu-id="ddedb-214">Delete library files</span></span>

<span data-ttu-id="ddedb-215">Aby wykonać operację *czyszczenia* , która spowoduje usunięcie plików biblioteki, które zostały wcześniej przywrócone w programie Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="ddedb-215">To perform the *clean* operation, which deletes library files previously restored in Visual Studio:</span></span>

* <span data-ttu-id="ddedb-216">Kliknij prawym przyciskiem myszy plik *Libman. JSON* w **Eksplorator rozwiązań**.</span><span class="sxs-lookup"><span data-stu-id="ddedb-216">Right-click the *libman.json* file in **Solution Explorer**.</span></span>
* <span data-ttu-id="ddedb-217">Wybierz opcję **Wyczyść biblioteki po stronie klienta** .</span><span class="sxs-lookup"><span data-stu-id="ddedb-217">Select the **Clean Client-Side Libraries** option.</span></span>

<span data-ttu-id="ddedb-218">Aby zapobiec przypadkowemu usunięciu plików nienależących do biblioteki, operacja czyszczenia nie usuwa całych katalogów.</span><span class="sxs-lookup"><span data-stu-id="ddedb-218">To prevent unintentional removal of non-library files, the clean operation doesn't delete whole directories.</span></span> <span data-ttu-id="ddedb-219">Usuwa tylko te pliki, które zostały uwzględnione w poprzednim przywracaniu.</span><span class="sxs-lookup"><span data-stu-id="ddedb-219">It only removes files that were included in the previous restore.</span></span>

<span data-ttu-id="ddedb-220">Podczas gdy operacja czyszczenia jest uruchomiona:</span><span class="sxs-lookup"><span data-stu-id="ddedb-220">While the clean operation is running:</span></span>

* <span data-ttu-id="ddedb-221">Ikona TSC na pasku stanu programu Visual Studio będzie animowana i zostanie *rozpoczęta operacja odczytywania bibliotek klienckich*.</span><span class="sxs-lookup"><span data-stu-id="ddedb-221">The TSC icon on the Visual Studio status bar will be animated and will read *Client libraries operation started*.</span></span> <span data-ttu-id="ddedb-222">Kliknięcie ikony powoduje otwarcie etykietki narzędzia zawierającego listę znanych zadań w tle.</span><span class="sxs-lookup"><span data-stu-id="ddedb-222">Clicking the icon opens a tooltip listing the known background tasks.</span></span>
* <span data-ttu-id="ddedb-223">Komunikaty są wysyłane do paska stanu i źródła danych programu **Library Manager** okna **danych wyjściowych** .</span><span class="sxs-lookup"><span data-stu-id="ddedb-223">Messages are sent to the status bar and the **Library Manager** feed of the **Output** window.</span></span> <span data-ttu-id="ddedb-224">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="ddedb-224">For example:</span></span>

```console
Clean libraries operation started...
Clean libraries operation completed
2 libraries were successfully deleted in 1.91 secs
```

<span data-ttu-id="ddedb-225">Operacja czyszczenia usuwa tylko pliki z projektu.</span><span class="sxs-lookup"><span data-stu-id="ddedb-225">The clean operation only deletes files from the project.</span></span> <span data-ttu-id="ddedb-226">Pliki biblioteki znajdują się w pamięci podręcznej w celu szybszego pobierania podczas przyszłych operacji przywracania.</span><span class="sxs-lookup"><span data-stu-id="ddedb-226">Library files stay in the cache for faster retrieval on future restore operations.</span></span> <span data-ttu-id="ddedb-227">Aby zarządzać plikami biblioteki przechowywanymi w pamięci podręcznej komputera lokalnego, użyj [interfejsu wiersza polecenia LibMan](xref:client-side/libman/libman-cli).</span><span class="sxs-lookup"><span data-stu-id="ddedb-227">To manage library files stored in the local machine's cache, use the [LibMan CLI](xref:client-side/libman/libman-cli).</span></span>

## <a name="uninstall-library-files"></a><span data-ttu-id="ddedb-228">Odinstaluj pliki biblioteki</span><span class="sxs-lookup"><span data-stu-id="ddedb-228">Uninstall library files</span></span>

<span data-ttu-id="ddedb-229">Aby odinstalować pliki biblioteki:</span><span class="sxs-lookup"><span data-stu-id="ddedb-229">To uninstall library files:</span></span>

* <span data-ttu-id="ddedb-230">Otwórz plik *Libman. JSON*.</span><span class="sxs-lookup"><span data-stu-id="ddedb-230">Open *libman.json*.</span></span>
* <span data-ttu-id="ddedb-231">Umieść karetkę wewnątrz odpowiedniego literału obiektu `libraries`.</span><span class="sxs-lookup"><span data-stu-id="ddedb-231">Position the caret inside the corresponding `libraries` object literal.</span></span>
* <span data-ttu-id="ddedb-232">Kliknij ikonę żarówki, która pojawia się na lewym marginesie, a następnie wybierz pozycję **odinstaluj \<library_name > @\<library_version >** :</span><span class="sxs-lookup"><span data-stu-id="ddedb-232">Click the light bulb icon that appears in the left margin, and select **Uninstall \<library_name>@\<library_version>**:</span></span>

  ![Opcja menu kontekstowego odinstalowywania biblioteki](_static/uninstall-menu-option.png)

<span data-ttu-id="ddedb-234">Alternatywnie możesz ręcznie edytować i zapisać manifest LibMan (*LibMan. JSON*).</span><span class="sxs-lookup"><span data-stu-id="ddedb-234">Alternatively, you can manually edit and save the LibMan manifest (*libman.json*).</span></span> <span data-ttu-id="ddedb-235">[Operacja przywracania](#restore-library-files) jest uruchamiana, gdy plik zostanie zapisany.</span><span class="sxs-lookup"><span data-stu-id="ddedb-235">The [restore operation](#restore-library-files) runs when the file is saved.</span></span> <span data-ttu-id="ddedb-236">Pliki bibliotek, które nie są już zdefiniowane w *Libman. JSON* , są usuwane z projektu.</span><span class="sxs-lookup"><span data-stu-id="ddedb-236">Library files that are no longer defined in *libman.json* are removed from the project.</span></span>

## <a name="update-library-version"></a><span data-ttu-id="ddedb-237">Zaktualizuj wersję biblioteki</span><span class="sxs-lookup"><span data-stu-id="ddedb-237">Update library version</span></span>

<span data-ttu-id="ddedb-238">Aby sprawdzić dostępność zaktualizowanej wersji biblioteki:</span><span class="sxs-lookup"><span data-stu-id="ddedb-238">To check for an updated library version:</span></span>

* <span data-ttu-id="ddedb-239">Otwórz plik *Libman. JSON*.</span><span class="sxs-lookup"><span data-stu-id="ddedb-239">Open *libman.json*.</span></span>
* <span data-ttu-id="ddedb-240">Umieść karetkę wewnątrz odpowiedniego literału obiektu `libraries`.</span><span class="sxs-lookup"><span data-stu-id="ddedb-240">Position the caret inside the corresponding `libraries` object literal.</span></span>
* <span data-ttu-id="ddedb-241">Kliknij ikonę żarówki, która pojawia się na lewym marginesie.</span><span class="sxs-lookup"><span data-stu-id="ddedb-241">Click the light bulb icon that appears in the left margin.</span></span> <span data-ttu-id="ddedb-242">Umieść kursor nad **sprawdzaniem dostępności aktualizacji**.</span><span class="sxs-lookup"><span data-stu-id="ddedb-242">Hover over **Check for updates**.</span></span>

<span data-ttu-id="ddedb-243">LibMan sprawdza, czy wersja biblioteki jest nowsza niż zainstalowana wersja.</span><span class="sxs-lookup"><span data-stu-id="ddedb-243">LibMan checks for a library version newer than the version installed.</span></span> <span data-ttu-id="ddedb-244">Mogą wystąpić następujące wyniki:</span><span class="sxs-lookup"><span data-stu-id="ddedb-244">The following outcomes can occur:</span></span>

* <span data-ttu-id="ddedb-245">Jeśli Najnowsza wersja jest już zainstalowana, zostanie wyświetlony komunikat " **nie znaleziono aktualizacji** ".</span><span class="sxs-lookup"><span data-stu-id="ddedb-245">A **No updates found** message is displayed if the latest version is already installed.</span></span>
* <span data-ttu-id="ddedb-246">Najnowsza stabilna wersja jest wyświetlana, jeśli nie została jeszcze zainstalowana.</span><span class="sxs-lookup"><span data-stu-id="ddedb-246">The latest stable version is displayed if not already installed.</span></span>

  ![Opcja menu kontekstowego wyszukiwania aktualizacji](_static/update-menu-option.png)

* <span data-ttu-id="ddedb-248">Jeśli dostępna jest wersja wstępna nowsza niż zainstalowana, zostanie wyświetlona wersja wstępna.</span><span class="sxs-lookup"><span data-stu-id="ddedb-248">If a pre-release newer than the installed version is available, the pre-release is displayed.</span></span>

<span data-ttu-id="ddedb-249">Aby dokonać obniżenia poziomu do starszej wersji biblioteki, ręcznie Edytuj plik *Libman. JSON* .</span><span class="sxs-lookup"><span data-stu-id="ddedb-249">To downgrade to an older library version, manually edit the *libman.json* file.</span></span> <span data-ttu-id="ddedb-250">Po zapisaniu pliku LibMan [operacji przywracania](#restore-library-files):</span><span class="sxs-lookup"><span data-stu-id="ddedb-250">When the file is saved, the LibMan [restore operation](#restore-library-files):</span></span>

* <span data-ttu-id="ddedb-251">Usuwa nadmiarowe pliki z poprzedniej wersji.</span><span class="sxs-lookup"><span data-stu-id="ddedb-251">Removes redundant files from the previous version.</span></span>
* <span data-ttu-id="ddedb-252">Dodaje nowe i zaktualizowane pliki z nowej wersji.</span><span class="sxs-lookup"><span data-stu-id="ddedb-252">Adds new and updated files from the new version.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ddedb-253">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="ddedb-253">Additional resources</span></span>

* <xref:client-side/libman/libman-cli>
* [<span data-ttu-id="ddedb-254">Repozytorium GitHub LibMan</span><span class="sxs-lookup"><span data-stu-id="ddedb-254">LibMan GitHub repository</span></span>](https://github.com/aspnet/LibraryManager)
