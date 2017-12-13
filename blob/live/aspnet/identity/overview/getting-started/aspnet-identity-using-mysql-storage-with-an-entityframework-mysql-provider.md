---
uid: identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
title: "Tożsamość platformy ASP.NET: Przy użyciu magazynu MySQL przy użyciu platformy EntityFramework MySQL dostawcy (C#) | Dokumentacja firmy Microsoft"
author: maumar
description: "Ten samouczek pokazuje, jak zastąpić domyślny mechanizm magazynu danych ASP.NET Identity EntityFramework (Dostawca klienta SQL) z dostarczyć MySQL..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/10/2013
ms.topic: article
ms.assetid: 15253312-a92c-43ba-908e-b5dacd3d08b8
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
msc.type: authoredcontent
ms.openlocfilehash: ac254abcb756d048d159a9b67967a581f35ac871
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider-c"></a><span data-ttu-id="72f0f-103">Tożsamość platformy ASP.NET: Przy użyciu magazynu MySQL przy użyciu dostawcy EntityFramework MySQL (C#)</span><span class="sxs-lookup"><span data-stu-id="72f0f-103">ASP.NET Identity: Using MySQL Storage with an EntityFramework MySQL Provider (C#)</span></span>
====================
<span data-ttu-id="72f0f-104">przez [Maurycy Markowski](https://github.com/maumar), [Raquel Soares De Almeida](https://github.com/raquelsa), [Roberta Mcmurraya](https://github.com/rmcmurray)</span><span class="sxs-lookup"><span data-stu-id="72f0f-104">by [Maurycy Markowski](https://github.com/maumar), [Raquel Soares De Almeida](https://github.com/raquelsa), [Robert McMurray](https://github.com/rmcmurray)</span></span>

> <span data-ttu-id="72f0f-105">Ten samouczek pokazuje, jak zastąpić domyślny mechanizm magazynu danych dla [ **ASP.NET Identity** ](introduction-to-aspnet-identity.md) z EntityFramework (Dostawca klienta SQL) z dostawcą MySQL.</span><span class="sxs-lookup"><span data-stu-id="72f0f-105">This tutorial shows you how to replace the default data storage mechanism for [**ASP.NET Identity**](introduction-to-aspnet-identity.md) with EntityFramework (SQL client provider) with a MySQL provider.</span></span>


<span data-ttu-id="72f0f-106">Poniższe tematy zostaną omówione w tym samouczku:</span><span class="sxs-lookup"><span data-stu-id="72f0f-106">The following topics will be covered in this tutorial:</span></span>

- <span data-ttu-id="72f0f-107">Tworzenie bazy danych MySQL na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="72f0f-107">Creating a MySQL database on Azure</span></span>
- <span data-ttu-id="72f0f-108">Tworzenie aplikacji MVC przy użyciu szablonu programu Visual Studio 2013 MVC</span><span class="sxs-lookup"><span data-stu-id="72f0f-108">Creating an MVC application using Visual Studio 2013 MVC template</span></span>
- <span data-ttu-id="72f0f-109">Konfigurowanie EntityFramework do pracy z dostawcy bazy danych MySQL</span><span class="sxs-lookup"><span data-stu-id="72f0f-109">Configuring EntityFramework to work with a MySQL database provider</span></span>
- <span data-ttu-id="72f0f-110">Uruchomienie aplikacji, aby zweryfikować wyniki</span><span class="sxs-lookup"><span data-stu-id="72f0f-110">Running the application to verify the results</span></span>

<span data-ttu-id="72f0f-111">Na końcu tego samouczka będziesz mieć aplikacji MVC z zastosowaniem ASP.NET Identity przechowywania korzystającej z bazy danych MySQL hostowanej na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="72f0f-111">At the end of this tutorial, you will have an MVC application with the ASP.NET Identity store that is using a MySQL database that is hosted in Azure.</span></span>

## <a name="creating-a-mysql-database-instance-on-azure"></a><span data-ttu-id="72f0f-112">Tworzenie wystąpienia bazy danych MySQL na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="72f0f-112">Creating a MySQL database instance on Azure</span></span>

1. <span data-ttu-id="72f0f-113">Zaloguj się do [portalu Azure](https://go.microsoft.com/fwlink/?linkid=529715&amp;clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="72f0f-113">Log in to the [Azure Portal](https://go.microsoft.com/fwlink/?linkid=529715&amp;clcid=0x409).</span></span>
2. <span data-ttu-id="72f0f-114">Kliknij przycisk **nowy** w dolnej części strony, a następnie wybierz **MAGAZYNU**:</span><span class="sxs-lookup"><span data-stu-id="72f0f-114">Click **NEW** at the bottom of the page, and then select **STORE**:</span></span>  
  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.png)
3. <span data-ttu-id="72f0f-115">W **wybierz i dodatek** kreatora wybierz **baza danych ClearDB MySQL**, a następnie kliknij przycisk **dalej** strzałki w dolnej części ramki:</span><span class="sxs-lookup"><span data-stu-id="72f0f-115">In the **Choose and Add-on** wizard, select **ClearDB MySQL Database**, and then click the **Next** arrow at the bottom of the frame:</span></span>  
  
 <span data-ttu-id="72f0f-116">[Kliknij poniższy obraz, aby go rozwinąć.</span><span class="sxs-lookup"><span data-stu-id="72f0f-116">[Click the following image to expand it.</span></span> <span data-ttu-id="72f0f-117">]</span><span class="sxs-lookup"><span data-stu-id="72f0f-117">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.png)
4. <span data-ttu-id="72f0f-118">Zachowaj ustawienie domyślne **wolne** planowanie, zmień **nazwa** do **IdentityMySQLDatabase**, wybierz region, który znajduje się najbliżej możesz, a następnie kliknij przycisk **dalej** strzałki w dolnej części ramki:</span><span class="sxs-lookup"><span data-stu-id="72f0f-118">Keep the default **Free** plan, change the **NAME** to **IdentityMySQLDatabase**, select the region that is nearest to you, and then click the **Next** arrow at the bottom of the frame:</span></span>  
  
 <span data-ttu-id="72f0f-119">[Kliknij poniższy obraz, aby go rozwinąć.</span><span class="sxs-lookup"><span data-stu-id="72f0f-119">[Click the following image to expand it.</span></span> <span data-ttu-id="72f0f-120">]</span><span class="sxs-lookup"><span data-stu-id="72f0f-120">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.png)
5. <span data-ttu-id="72f0f-121">Kliknij przycisk **zakupu** znacznik wyboru, aby zakończyć tworzenie bazy danych.</span><span class="sxs-lookup"><span data-stu-id="72f0f-121">Click the **PURCHASE** checkmark to complete the database creation.</span></span>  
  
 <span data-ttu-id="72f0f-122">[Kliknij poniższy obraz, aby go rozwinąć.</span><span class="sxs-lookup"><span data-stu-id="72f0f-122">[Click the following image to expand it.</span></span> <span data-ttu-id="72f0f-123">]</span><span class="sxs-lookup"><span data-stu-id="72f0f-123">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.png)
6. <span data-ttu-id="72f0f-124">Po utworzeniu bazy danych, można zarządzać nim z **dodatki** kartę w portalu zarządzania.</span><span class="sxs-lookup"><span data-stu-id="72f0f-124">After your database has been created, you can manage it from the **ADD-ONS** tab in the management portal.</span></span> <span data-ttu-id="72f0f-125">Aby uzyskać informacje o połączeniu dla bazy danych, kliknij przycisk **informacje o połączeniu** w dolnej części strony:</span><span class="sxs-lookup"><span data-stu-id="72f0f-125">To retrieve the connection information for your database, click **CONNECTION INFO** at the bottom of the page:</span></span>  
  
 <span data-ttu-id="72f0f-126">[Kliknij poniższy obraz, aby go rozwinąć.</span><span class="sxs-lookup"><span data-stu-id="72f0f-126">[Click the following image to expand it.</span></span> <span data-ttu-id="72f0f-127">]</span><span class="sxs-lookup"><span data-stu-id="72f0f-127">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image10.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image9.png)
7. <span data-ttu-id="72f0f-128">Skopiuj parametry połączenia, klikając przycisk Kopiuj przez **CONNECTIONSTRING** pola i zapisz go; użyje tych informacji w dalszej części tego samouczka aplikacji MVC:</span><span class="sxs-lookup"><span data-stu-id="72f0f-128">Copy the connection string by clicking on the copy button by the **CONNECTIONSTRING** field and save it; you will use this information later in this tutorial for your MVC application:</span></span>  
  
 <span data-ttu-id="72f0f-129">[Kliknij poniższy obraz, aby go rozwinąć.</span><span class="sxs-lookup"><span data-stu-id="72f0f-129">[Click the following image to expand it.</span></span> <span data-ttu-id="72f0f-130">]</span><span class="sxs-lookup"><span data-stu-id="72f0f-130">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image12.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image11.png)

## <a name="creating-an-mvc-application-project"></a><span data-ttu-id="72f0f-131">Tworzenie projektu aplikacji MVC</span><span class="sxs-lookup"><span data-stu-id="72f0f-131">Creating an MVC application project</span></span>

<span data-ttu-id="72f0f-132">Aby wykonać kroki opisane w tej części samouczka, najpierw należy zainstalować [programu Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) lub [programu Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="72f0f-132">To complete the steps in this section of the tutorial, you will first need to install [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="72f0f-133">Po zainstalowaniu programu Visual Studio, aby utworzyć nowy projekt aplikacji MVC Użyj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="72f0f-133">Once Visual Studio has been installed, use the following steps to create a new MVC application project:</span></span>

1. <span data-ttu-id="72f0f-134">Otwórz program Visual Studio 2103.</span><span class="sxs-lookup"><span data-stu-id="72f0f-134">Open Visual Studio 2103.</span></span>
2. <span data-ttu-id="72f0f-135">Kliknij przycisk **nowy projekt** z **Start** strony, lub kliknij przycisk **pliku** menu, a następnie **nowy projekt**:</span><span class="sxs-lookup"><span data-stu-id="72f0f-135">Click **New Project** from the **Start** page, or you can click the **File** menu and then **New Project**:</span></span>  
  
 <span data-ttu-id="72f0f-136">[Kliknij poniższy obraz, aby go rozwinąć.</span><span class="sxs-lookup"><span data-stu-id="72f0f-136">[Click the following image to expand it.</span></span> <span data-ttu-id="72f0f-137">]</span><span class="sxs-lookup"><span data-stu-id="72f0f-137">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.jpg)
3. <span data-ttu-id="72f0f-138">Gdy **nowy projekt** zostanie wyświetlone okno dialogowe, rozwiń **Visual C#** na liście szablonów, następnie kliknij przycisk **sieci Web**i wybierz **aplikacji sieci Web ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="72f0f-138">When the **New Project** dialog box is displayed, expand **Visual C#** in the list of templates, then click **Web**, and select **ASP.NET Web Application**.</span></span> <span data-ttu-id="72f0f-139">Nazwij swój projekt **IdentityMySQLDemo** , a następnie kliknij przycisk **OK**:</span><span class="sxs-lookup"><span data-stu-id="72f0f-139">Name your project **IdentityMySQLDemo** and then click **OK**:</span></span>  
  
 <span data-ttu-id="72f0f-140">[Kliknij poniższy obraz, aby go rozwinąć.</span><span class="sxs-lookup"><span data-stu-id="72f0f-140">[Click the following image to expand it.</span></span> <span data-ttu-id="72f0f-141">]</span><span class="sxs-lookup"><span data-stu-id="72f0f-141">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image14.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image13.png)
4. <span data-ttu-id="72f0f-142">W **nowy projekt ASP.NET** okno dialogowe, wybierz opcję **MVC** templatewith domyślne opcje; spowoduje to skonfigurować **indywidualnych kont użytkowników** jako metody uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="72f0f-142">In the **New ASP.NET Project** dialog, select the **MVC** templatewith the default options; this will configure **Individual User Accounts** as the authentication method.</span></span> <span data-ttu-id="72f0f-143">Kliknij przycisk **OK**:</span><span class="sxs-lookup"><span data-stu-id="72f0f-143">Click **OK**:</span></span>  
  
 <span data-ttu-id="72f0f-144">[Kliknij poniższy obraz, aby go rozwinąć.</span><span class="sxs-lookup"><span data-stu-id="72f0f-144">[Click the following image to expand it.</span></span> <span data-ttu-id="72f0f-145">]</span><span class="sxs-lookup"><span data-stu-id="72f0f-145">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image16.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image15.png)

## <a name="configure-entityframework-to-work-with-a-mysql-database"></a><span data-ttu-id="72f0f-146">Skonfiguruj EntityFramework do pracy z bazy danych MySQL</span><span class="sxs-lookup"><span data-stu-id="72f0f-146">Configure EntityFramework to work with a MySQL database</span></span>

### <a name="update-the-entity-framework-assembly-for-your-project"></a><span data-ttu-id="72f0f-147">Aktualizacja zestawu programu Entity Framework dla projektu</span><span class="sxs-lookup"><span data-stu-id="72f0f-147">Update the Entity Framework assembly for your project</span></span>

<span data-ttu-id="72f0f-148">Aplikacji MVC, który został utworzony na podstawie szablonu Visual Studio 2013 zawiera odwołanie do [EntityFramework 6.0.0](http://www.nuget.org/packages/EntityFramework) pakietu, ale ma zostały znaczące aktualizacje do tego zestawu, ponieważ jego wersja zawierających ulepszenia wydajności.</span><span class="sxs-lookup"><span data-stu-id="72f0f-148">The MVC application that was created from the Visual Studio 2013 template contains a reference to the [EntityFramework 6.0.0](http://www.nuget.org/packages/EntityFramework) package, but there have been updates to to that assembly since its release which contain significant performance improvements.</span></span> <span data-ttu-id="72f0f-149">Aby można było używać tych najnowsze aktualizacje w aplikacji, wykonaj następujące kroki.</span><span class="sxs-lookup"><span data-stu-id="72f0f-149">In order to use these latest updates in your application, use the following steps.</span></span>

1. <span data-ttu-id="72f0f-150">Otwórz projekt MVC programu Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="72f0f-150">Open your MVC project in Visual Studio 2013.</span></span>
2. <span data-ttu-id="72f0f-151">Kliknij przycisk **narzędzia**, następnie kliknij przycisk **Menedżer pakietów biblioteki**, a następnie kliknij przycisk **Konsola Menedżera pakietów**:</span><span class="sxs-lookup"><span data-stu-id="72f0f-151">Click **TOOLS**, then click **Library Package Manager**, and then click **Package Manager Console**:</span></span>  
  
 <span data-ttu-id="72f0f-152">[Kliknij poniższy obraz, aby go rozwinąć.</span><span class="sxs-lookup"><span data-stu-id="72f0f-152">[Click the following image to expand it.</span></span> <span data-ttu-id="72f0f-153">]</span><span class="sxs-lookup"><span data-stu-id="72f0f-153">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image18.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image17.png)
3. <span data-ttu-id="72f0f-154">**Konsola Menedżera pakietów** będą wyświetlane w dolnej części programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="72f0f-154">The **Package Manager Console** will appear in the bottom section of Visual Studio.</span></span> <span data-ttu-id="72f0f-155">Typ &quot; **EntityFramework pakiet aktualizacji** &quot; i naciśnij klawisz Enter:</span><span class="sxs-lookup"><span data-stu-id="72f0f-155">Type &quot;**Update-Package EntityFramework**&quot; and press Enter:</span></span>  
  
 <span data-ttu-id="72f0f-156">[Kliknij poniższy obraz, aby go rozwinąć.</span><span class="sxs-lookup"><span data-stu-id="72f0f-156">[Click the following image to expand it.</span></span> <span data-ttu-id="72f0f-157">]</span><span class="sxs-lookup"><span data-stu-id="72f0f-157">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image20.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image19.png)

### <a name="install-the-mysql-provider-for-entityframework"></a><span data-ttu-id="72f0f-158">Zainstaluj dostawcę MySQL na platformie EntityFramework</span><span class="sxs-lookup"><span data-stu-id="72f0f-158">Install the MySQL provider for EntityFramework</span></span>

<span data-ttu-id="72f0f-159">EntityFramework do łączenia z bazą danych MySQL, należy zainstalować dostawcę MySQL.</span><span class="sxs-lookup"><span data-stu-id="72f0f-159">In order for EntityFramework to connect to MySQL database, you need to install a MySQL provider.</span></span> <span data-ttu-id="72f0f-160">Aby to zrobić, otwórz **Konsola Menedżera pakietów** i typ &quot; **Pre - Install-Package MySql.Data.Entity**&quot;, a następnie naciśnij klawisz Enter.</span><span class="sxs-lookup"><span data-stu-id="72f0f-160">To do so, open the **Package Manager Console** and type &quot;**Install-Package MySql.Data.Entity -Pre**&quot;, and then press Enter.</span></span>

> [!NOTE]
> <span data-ttu-id="72f0f-161">To jest wstępna wersja zestawu i jako taki może on zawierać błędy.</span><span class="sxs-lookup"><span data-stu-id="72f0f-161">This is a pre-release version of the assembly, and as such it may contain bugs.</span></span> <span data-ttu-id="72f0f-162">Wykryto wstępną wersję dostawcy nie należy używać w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="72f0f-162">You should not use a pre-release version of the provider in production.</span></span>


<span data-ttu-id="72f0f-163">[Kliknij poniższy obraz, aby go rozwinąć.]</span><span class="sxs-lookup"><span data-stu-id="72f0f-163">[Click the following image to expand it.]</span></span>  
  
[![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image22.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image21.png)

### <a name="making-project-configuration-changes-to-the-webconfig-file-for-your-application"></a><span data-ttu-id="72f0f-164">Wprowadzania zmian w konfiguracji projektu do pliku Web.config aplikacji</span><span class="sxs-lookup"><span data-stu-id="72f0f-164">Making project configuration changes to the Web.config file for your application</span></span>

<span data-ttu-id="72f0f-165">W tej sekcji skonfigurujesz Entity Framework do używania dostawcy MySQL, który został właśnie zainstalowany Zarejestruj fabryki dostawcy MySQL, a następnie dodaj parametry połączenia z platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="72f0f-165">In this section you will configure the Entity Framework to use the MySQL provider that you just installed, register the MySQL provider factory, and add your connection string from Azure.</span></span>

> [!NOTE]
> <span data-ttu-id="72f0f-166">Poniższe przykłady zawiera MySql.Data.dll wersji określonego zestawu.</span><span class="sxs-lookup"><span data-stu-id="72f0f-166">The following examples contain a specific assembly version for MySql.Data.dll.</span></span> <span data-ttu-id="72f0f-167">Wersja zestawu zmian, należy zmodyfikować ustawienia prawidłowej konfiguracji w odpowiedniej wersji.</span><span class="sxs-lookup"><span data-stu-id="72f0f-167">If the assembly version changes, you will need to modify the appropriate configuration settings with the correct version.</span></span>


1. <span data-ttu-id="72f0f-168">Otwórz plik Web.config dla projektu programu Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="72f0f-168">Open the Web.config file for your project in Visual Studio 2013.</span></span>
2. <span data-ttu-id="72f0f-169">Znajdź następujące ustawienia konfiguracji, które określają domyślny dostawca bazy danych i fabryki Entity Framework:</span><span class="sxs-lookup"><span data-stu-id="72f0f-169">Locate the following configuration settings, which define the default database provider and factory for the Entity Framework:</span></span>

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample1.xml)]
3. <span data-ttu-id="72f0f-170">Te ustawienia konfiguracji należy zastąpić poniższe polecenie, które służy do konfigurowania programu Entity Framework do używania dostawcy MySQL:</span><span class="sxs-lookup"><span data-stu-id="72f0f-170">Replace those configuration settings with the following, which will configure the Entity Framework to use the MySQL provider:</span></span> 

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample2.xml)]
4. <span data-ttu-id="72f0f-171">Zlokalizuj &lt;connectionStrings&gt; sekcji i zastąp go następującym kodem, który będzie definiował ciąg połączenia dla bazy danych MySQL hostowanej na platformie Azure (należy pamiętać, że wartość providerName również została zmieniona z oryginalne):</span><span class="sxs-lookup"><span data-stu-id="72f0f-171">Locate the &lt;connectionStrings&gt; section and replace it with the following code, which will define the connection string for your MySQL database that is hosted on Azure (note that providerName value has also been changed from the original):</span></span>

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample3.xml?highlight=3-4)]

### <a name="adding-custom-migrationhistory-context"></a><span data-ttu-id="72f0f-172">Dodawanie niestandardowych kontekstu MigrationHistory</span><span class="sxs-lookup"><span data-stu-id="72f0f-172">Adding custom MigrationHistory context</span></span>

<span data-ttu-id="72f0f-173">Korzysta z programu Entity Framework Code First **MigrationHistory** tabeli, aby śledzić zmiany modelu i zapewnienie spójności schematu bazy danych i schematu koncepcyjnego.</span><span class="sxs-lookup"><span data-stu-id="72f0f-173">Entity Framework Code First uses a **MigrationHistory** table to keep track of model changes and to ensure the consistency between the database schema and conceptual schema.</span></span> <span data-ttu-id="72f0f-174">Jednak ta tabela nie działa dla programu MySQL domyślnie ponieważ klucz podstawowy jest za duży.</span><span class="sxs-lookup"><span data-stu-id="72f0f-174">However, this table does not work for MySQL by default because the primary key is too large.</span></span> <span data-ttu-id="72f0f-175">Aby rozwiązać ten problem, należy zmniejszyć rozmiar klucza dla tej tabeli.</span><span class="sxs-lookup"><span data-stu-id="72f0f-175">To remedy this situation, you will need to shrink the key size for that table.</span></span> <span data-ttu-id="72f0f-176">Aby to zrobić, wykonaj następujące kroki:</span><span class="sxs-lookup"><span data-stu-id="72f0f-176">To do so, use the following steps:</span></span>

1. <span data-ttu-id="72f0f-177">Gromadzenia informacji schematu dla tej tabeli **HistoryContext**, które mogą być zmodyfikowane, jak każdy inny **DbContext**.</span><span class="sxs-lookup"><span data-stu-id="72f0f-177">The schema information for this table is captured in a **HistoryContext**, which can be modified as any other **DbContext**.</span></span> <span data-ttu-id="72f0f-178">Aby to zrobić, należy dodać nowy plik klasy o nazwie **MySqlHistoryContext.cs** do projektu i zastąp jego zawartość następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="72f0f-178">To do so, add a new class file named **MySqlHistoryContext.cs** to the project, and replace its contents with the following code:</span></span>

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample4.cs)]
2. <span data-ttu-id="72f0f-179">Obok będzie konieczne skonfigurowanie programu Entity Framework, aby użyć zmodyfikowanego **HistoryContext**, zamiast domyślnego.</span><span class="sxs-lookup"><span data-stu-id="72f0f-179">Next you will need to configure Entity Framework to use the modified **HistoryContext**, rather than default one.</span></span> <span data-ttu-id="72f0f-180">Można to zrobić przy użyciu funkcji konfiguracji opartej na kodzie.</span><span class="sxs-lookup"><span data-stu-id="72f0f-180">This can be done by leveraging code-based configuration features.</span></span> <span data-ttu-id="72f0f-181">Aby to zrobić, należy dodać nowy plik klasy o nazwie **MySqlConfiguration.cs** do projektu i zastąp jego zawartość z:</span><span class="sxs-lookup"><span data-stu-id="72f0f-181">To do so, add new class file named **MySqlConfiguration.cs** to your project and replace its contents with:</span></span>

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample5.cs)]

### <a name="creating-a-custom-entityframework-initializer-for-applicationdbcontext"></a><span data-ttu-id="72f0f-182">Tworzenie niestandardowych inicjator EntityFramework ApplicationDbContext</span><span class="sxs-lookup"><span data-stu-id="72f0f-182">Creating a custom EntityFramework initializer for ApplicationDbContext</span></span>

<span data-ttu-id="72f0f-183">Dostawca MySQL, która jest dostępna w tym samouczku nie obsługuje obecnie w przypadku migracji z programu Entity Framework, musisz użyć inicjatory modelu w celu połączenia z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="72f0f-183">The MySQL provider that is featured in this tutorial does not currently support Entity Framework migrations, so you will need to use model initializers in order to connect to the database.</span></span> <span data-ttu-id="72f0f-184">Ponieważ w tym samouczku korzysta z wystąpienia programu MySQL na platformie Azure, będzie konieczne należy utworzyć niestandardowe inicjatora programu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="72f0f-184">Because this tutorial is using a MySQL instance on Azure, you will need need to create a custom Entity Framework initializer.</span></span>

> [!NOTE]
> <span data-ttu-id="72f0f-185">Ten krok nie jest wymagane, jeśli łączysz się z wystąpieniem programu SQL Server na platformie Azure lub jeśli używasz bazy danych, która jest hostowana na lokalnym.</span><span class="sxs-lookup"><span data-stu-id="72f0f-185">This step is not required if you are connecting to a SQL Server instance on Azure or if you are using a database that is hosted on premises.</span></span>


<span data-ttu-id="72f0f-186">Aby utworzyć niestandardowe inicjatora programu Entity Framework dla programu MySQL, użyj następujących kroków:</span><span class="sxs-lookup"><span data-stu-id="72f0f-186">To create a custom Entity Framework initializer for MySQL, use the following steps:</span></span>

1. <span data-ttu-id="72f0f-187">Dodaj nowy plik klasy o nazwie **MySqlInitializer.cs** do projektu i Zastąp jest zawartość następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="72f0f-187">Add a new class file named **MySqlInitializer.cs** to the project, and replace it's contents with the following code:</span></span> 

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample6.cs?highlight=23)]
2. <span data-ttu-id="72f0f-188">Otwórz **IdentityModels.cs** pliku projektu, który znajduje się w **modele** katalogu i zastąp jego zawartości z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="72f0f-188">Open the **IdentityModels.cs** file for your project, which is located in the **Models** directory, and replace it's contents with the following:</span></span> 

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample7.cs)]

## <a name="running-the-application-and-verifying-the-database"></a><span data-ttu-id="72f0f-189">Uruchamianie aplikacji i weryfikowanie bazy danych</span><span class="sxs-lookup"><span data-stu-id="72f0f-189">Running the application and verifying the database</span></span>

<span data-ttu-id="72f0f-190">Po wykonaniu czynności opisane w poprzednich sekcjach, należy przetestować bazę danych.</span><span class="sxs-lookup"><span data-stu-id="72f0f-190">Once you have completed the steps in the preceding sections, you should test your database.</span></span> <span data-ttu-id="72f0f-191">Aby to zrobić, wykonaj następujące kroki:</span><span class="sxs-lookup"><span data-stu-id="72f0f-191">To do so, use the following steps:</span></span>

1. <span data-ttu-id="72f0f-192">Naciśnij klawisz **Ctrl + F5** Aby skompilować i uruchomić aplikację sieci web.</span><span class="sxs-lookup"><span data-stu-id="72f0f-192">Press **Ctrl + F5** to build and run the web application.</span></span>
2. <span data-ttu-id="72f0f-193">Kliknij przycisk **zarejestrować** kartę w górnej części strony:</span><span class="sxs-lookup"><span data-stu-id="72f0f-193">Click the **Register** tab on the top of the page:</span></span>  
  
 <span data-ttu-id="72f0f-194">[Kliknij poniższy obraz, aby go rozwinąć.</span><span class="sxs-lookup"><span data-stu-id="72f0f-194">[Click the following image to expand it.</span></span> <span data-ttu-id="72f0f-195">]</span><span class="sxs-lookup"><span data-stu-id="72f0f-195">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.jpg)
3. <span data-ttu-id="72f0f-196">Wprowadź nową nazwę użytkownika i hasło, a następnie kliknij przycisk **zarejestrować**:</span><span class="sxs-lookup"><span data-stu-id="72f0f-196">Enter a new user name and password, and then click **Register**:</span></span>  
  
 <span data-ttu-id="72f0f-197">[Kliknij poniższy obraz, aby go rozwinąć.</span><span class="sxs-lookup"><span data-stu-id="72f0f-197">[Click the following image to expand it.</span></span> <span data-ttu-id="72f0f-198">]</span><span class="sxs-lookup"><span data-stu-id="72f0f-198">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image24.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image23.png)
4. <span data-ttu-id="72f0f-199">W tym momencie ASP.NET Identity tabele są tworzone w bazie danych MySQL, a użytkownik jest zarejestrowany, a zalogowany do aplikacji:</span><span class="sxs-lookup"><span data-stu-id="72f0f-199">At this point the ASP.NET Identity tables are created on the MySQL Database, and the user is registered and logged into the application:</span></span>  
  
 <span data-ttu-id="72f0f-200">[Kliknij poniższy obraz, aby go rozwinąć.</span><span class="sxs-lookup"><span data-stu-id="72f0f-200">[Click the following image to expand it.</span></span> <span data-ttu-id="72f0f-201">]</span><span class="sxs-lookup"><span data-stu-id="72f0f-201">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.jpg)

### <a name="installing-mysql-workbench-tool-to-verify-the-data"></a><span data-ttu-id="72f0f-202">Instalowanie narzędzia MySQL Workbench do sprawdzania danych</span><span class="sxs-lookup"><span data-stu-id="72f0f-202">Installing MySQL Workbench tool to verify the data</span></span>

1. <span data-ttu-id="72f0f-203">Zainstaluj **MySQL Workbench** narzędzia z [MySQL pobiera strony](http://dev.mysql.com/downloads/windows/installer/)</span><span class="sxs-lookup"><span data-stu-id="72f0f-203">Install the **MySQL Workbench** tool from the [MySQL downloads page](http://dev.mysql.com/downloads/windows/installer/)</span></span>
2. <span data-ttu-id="72f0f-204">W Kreatorze instalacji: **wybór funkcji** wybierz opcję **MySQL Workbench** w obszarze **aplikacji** sekcji.</span><span class="sxs-lookup"><span data-stu-id="72f0f-204">In the installation wizard: **Feature Selection** tab, select **MySQL Workbench** under **applications** section.</span></span>
3. <span data-ttu-id="72f0f-205">Uruchom aplikację i Dodaj nowe połączenie przy użyciu ciągu połączenia danych z bazy danych MySQL na platformie Azure utworzonych na żebranie tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="72f0f-205">Launch the app and add a new connection using the connection string data from the Azure MySQL database you created at the begging of this tutorial.</span></span>
4. <span data-ttu-id="72f0f-206">Po ustanowieniu połączenia, sprawdź **ASP.NET Identity** tabele utworzone na **IdentityMySQLDatabase.**</span><span class="sxs-lookup"><span data-stu-id="72f0f-206">After establishing the connection, inspect the **ASP.NET Identity** tables created on the **IdentityMySQLDatabase.**</span></span>
5. <span data-ttu-id="72f0f-207">Zobaczysz, że wszystkie tożsamości ASP.NET wymagane tabele są tworzone, jak pokazano na poniższej ilustracji:</span><span class="sxs-lookup"><span data-stu-id="72f0f-207">You will see that all ASP.NET Identity required tables are created as shown in the image below:</span></span>  
  
 <span data-ttu-id="72f0f-208">[Kliknij poniższy obraz, aby go rozwinąć.</span><span class="sxs-lookup"><span data-stu-id="72f0f-208">[Click the following image to expand it.</span></span> <span data-ttu-id="72f0f-209">]</span><span class="sxs-lookup"><span data-stu-id="72f0f-209">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.jpg)
6. <span data-ttu-id="72f0f-210">Sprawdź **aspnetusers** tabeli, na przykład aby wyszukać wpisy, jak zarejestrować nowych użytkowników.</span><span class="sxs-lookup"><span data-stu-id="72f0f-210">Inspect the **aspnetusers** table for instance to check for the entries as you register new users.</span></span>  
  
 <span data-ttu-id="72f0f-211">[Kliknij poniższy obraz, aby go rozwinąć.</span><span class="sxs-lookup"><span data-stu-id="72f0f-211">[Click the following image to expand it.</span></span> <span data-ttu-id="72f0f-212">]</span><span class="sxs-lookup"><span data-stu-id="72f0f-212">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image26.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image25.png)
