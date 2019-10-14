---
title: Zarządzanie odwołaniami protobuf za pomocą programu dotnet-GRPC
author: juntaoluo
description: Dowiedz się więcej na temat dodawania, aktualizowania, usuwania i wyświetlania protobuf odwołań za pomocą narzędzia dotnet-GRPC Global.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 09/24/2019
uid: grpc/dotnet-grpc
ms.openlocfilehash: ebd57419be24f7f4ed9765e36cf14189be8438b1
ms.sourcegitcommit: 020c3760492efed71b19e476f25392dda5dd7388
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/12/2019
ms.locfileid: "72290062"
---
# <a name="manage-protobuf-references-with-dotnet-grpc"></a><span data-ttu-id="72879-103">Zarządzanie odwołaniami protobuf za pomocą programu dotnet-GRPC</span><span class="sxs-lookup"><span data-stu-id="72879-103">Manage Protobuf references with dotnet-grpc</span></span>

<span data-ttu-id="72879-104">Przez [Jan Luo](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="72879-104">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="72879-105">`dotnet-grpc` to globalne narzędzie platformy .NET Core do zarządzania odwołaniami protobuf w ramach projektu .NET gRPC.</span><span class="sxs-lookup"><span data-stu-id="72879-105">`dotnet-grpc` is a .NET Core Global Tool for managing Protobuf references within a .NET gRPC project.</span></span> <span data-ttu-id="72879-106">Narzędzie może służyć do dodawania, odświeżania, usuwania i wyświetlania listy odwołań protobuf.</span><span class="sxs-lookup"><span data-stu-id="72879-106">The tool can be used to add, refresh, remove, and list Protobuf references.</span></span>

## <a name="installation"></a><span data-ttu-id="72879-107">Instalacja</span><span class="sxs-lookup"><span data-stu-id="72879-107">Installation</span></span>

<span data-ttu-id="72879-108">Aby zainstalować [Narzędzie globalne programu .NET Core](/dotnet/core/tools/global-tools)`dotnet-grpc`, uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="72879-108">To install the `dotnet-grpc` [.NET Core Global Tool](/dotnet/core/tools/global-tools), run the following command:</span></span>

```dotnetcli
dotnet tool install -g dotnet-grpc
```

## <a name="add-references"></a><span data-ttu-id="72879-109">Dodaj odwołania</span><span class="sxs-lookup"><span data-stu-id="72879-109">Add references</span></span>

<span data-ttu-id="72879-110">`dotnet-grpc` może służyć do dodawania odwołań protobuf jako elementów `<Protobuf />` do pliku *. csproj* :</span><span class="sxs-lookup"><span data-stu-id="72879-110">`dotnet-grpc` can be used to add Protobuf references as `<Protobuf />` items to the *.csproj* file:</span></span>

```xml
<Protobuf Include="..\Proto\count.proto" GrpcServices="Server" Link="Protos\count.proto" />
```

<span data-ttu-id="72879-111">Odwołania do protobuf są używane do generowania zasobów C# klienta i/lub serwera.</span><span class="sxs-lookup"><span data-stu-id="72879-111">The Protobuf references are used to generate the C# client and/or server assets.</span></span> <span data-ttu-id="72879-112">@No__t-0tool może:</span><span class="sxs-lookup"><span data-stu-id="72879-112">The `dotnet-grpc`tool can:</span></span>

* <span data-ttu-id="72879-113">Utwórz odwołanie protobuf z plików lokalnych na dysku.</span><span class="sxs-lookup"><span data-stu-id="72879-113">Create a Protobuf reference from local files on disk.</span></span>
* <span data-ttu-id="72879-114">Utwórz odwołanie protobuf z pliku zdalnego określonego przez adres URL.</span><span class="sxs-lookup"><span data-stu-id="72879-114">Create a Protobuf reference from a remote file specified by a URL.</span></span>
* <span data-ttu-id="72879-115">Upewnij się, że odpowiednie zależności pakietu gRPC są dodawane do projektu.</span><span class="sxs-lookup"><span data-stu-id="72879-115">Ensure the correct gRPC package dependencies are added to the project.</span></span>

<span data-ttu-id="72879-116">Na przykład pakiet `Grpc.AspNetCore` jest dodawany do aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="72879-116">For example, the `Grpc.AspNetCore` package is added to a web app.</span></span> <span data-ttu-id="72879-117">`Grpc.AspNetCore` zawiera gRPC serwer i biblioteki klienta oraz narzędzia obsługi.</span><span class="sxs-lookup"><span data-stu-id="72879-117">`Grpc.AspNetCore` contains gRPC server and client libraries and tooling support.</span></span> <span data-ttu-id="72879-118">Alternatywnie do aplikacji konsolowej są dodawane pakiety `Grpc.Net.Client`, `Grpc.Tools` i `Google.Protobuf`, które zawierają tylko biblioteki i narzędzia klienckie gRPC.</span><span class="sxs-lookup"><span data-stu-id="72879-118">Alternatively, the `Grpc.Net.Client`, `Grpc.Tools` and `Google.Protobuf` packages, which contain only the gRPC client libraries and tooling support, are added to a Console app.</span></span>

### <a name="add-file"></a><span data-ttu-id="72879-119">Dodaj plik</span><span class="sxs-lookup"><span data-stu-id="72879-119">Add file</span></span>

<span data-ttu-id="72879-120">Polecenie `add-file` służy do dodawania plików lokalnych na dysku jako odwołań protobuf.</span><span class="sxs-lookup"><span data-stu-id="72879-120">The `add-file` command is used to add local files on disk as Protobuf references.</span></span> <span data-ttu-id="72879-121">Podane ścieżki plików:</span><span class="sxs-lookup"><span data-stu-id="72879-121">The file paths provided:</span></span>

* <span data-ttu-id="72879-122">Może być względna względem bieżącego katalogu lub ścieżek bezwzględnych.</span><span class="sxs-lookup"><span data-stu-id="72879-122">Can be relative to the current directory or absolute paths.</span></span>
* <span data-ttu-id="72879-123">Mogą zawierać symbole wieloznaczne dla [obsługi symboli wieloznacznych](https://wikipedia.org/wiki/Glob_(programming))plików opartych na wzorcu.</span><span class="sxs-lookup"><span data-stu-id="72879-123">May contain wild cards for pattern-based file [globbing](https://wikipedia.org/wiki/Glob_(programming)).</span></span>

<span data-ttu-id="72879-124">Jeśli jakiekolwiek pliki znajdują się poza katalogiem projektu, zostanie dodany element `Link` w celu wyświetlenia pliku w folderze `Protos` w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="72879-124">If any files are outside the project directory, a `Link` element is added to display the file under the folder `Protos` in Visual Studio.</span></span>

### <a name="usage"></a><span data-ttu-id="72879-125">Użycie</span><span class="sxs-lookup"><span data-stu-id="72879-125">Usage</span></span>

```dotnetcli
dotnet grpc add-file [options] <files>...
```

#### <a name="arguments"></a><span data-ttu-id="72879-126">Argumenty</span><span class="sxs-lookup"><span data-stu-id="72879-126">Arguments</span></span>

| <span data-ttu-id="72879-127">Argument</span><span class="sxs-lookup"><span data-stu-id="72879-127">Argument</span></span> | <span data-ttu-id="72879-128">Opis</span><span class="sxs-lookup"><span data-stu-id="72879-128">Description</span></span> |
|-|-|
| <span data-ttu-id="72879-129">plikach</span><span class="sxs-lookup"><span data-stu-id="72879-129">files</span></span> | <span data-ttu-id="72879-130">Odwołuje się do pliku protobuf.</span><span class="sxs-lookup"><span data-stu-id="72879-130">The protobuf file references.</span></span> <span data-ttu-id="72879-131">Mogą to być ścieżki do globalizowania dla lokalnych plików protobuf.</span><span class="sxs-lookup"><span data-stu-id="72879-131">These can be a path to glob for local protobuf files.</span></span> |

#### <a name="options"></a><span data-ttu-id="72879-132">Opcje</span><span class="sxs-lookup"><span data-stu-id="72879-132">Options</span></span>

| <span data-ttu-id="72879-133">Opcja krótka</span><span class="sxs-lookup"><span data-stu-id="72879-133">Short option</span></span> | <span data-ttu-id="72879-134">Opcja Long</span><span class="sxs-lookup"><span data-stu-id="72879-134">Long option</span></span> | <span data-ttu-id="72879-135">Opis</span><span class="sxs-lookup"><span data-stu-id="72879-135">Description</span></span> |
|-|-|-|
| <span data-ttu-id="72879-136">-p</span><span class="sxs-lookup"><span data-stu-id="72879-136">-p</span></span> | <span data-ttu-id="72879-137">--projekt</span><span class="sxs-lookup"><span data-stu-id="72879-137">--project</span></span> | <span data-ttu-id="72879-138">Ścieżka do pliku projektu, na którym mają być wykonywane działania.</span><span class="sxs-lookup"><span data-stu-id="72879-138">The path to the project file to operate on.</span></span> <span data-ttu-id="72879-139">Jeśli plik nie zostanie określony, polecenie przeszukuje bieżący katalog.</span><span class="sxs-lookup"><span data-stu-id="72879-139">If a file is not specified, the command searches the current directory for one.</span></span>
| <span data-ttu-id="72879-140">-s</span><span class="sxs-lookup"><span data-stu-id="72879-140">-s</span></span> | <span data-ttu-id="72879-141">--usługi</span><span class="sxs-lookup"><span data-stu-id="72879-141">--services</span></span> | <span data-ttu-id="72879-142">Typ usług gRPC, które mają zostać wygenerowane.</span><span class="sxs-lookup"><span data-stu-id="72879-142">The type of gRPC services that should be generated.</span></span> <span data-ttu-id="72879-143">Jeśli określono `Default`, `Both` jest używany dla projektów sieci Web i `Client` jest używany dla projektów innych niż sieci Web.</span><span class="sxs-lookup"><span data-stu-id="72879-143">If `Default` is specified, `Both` is used for Web projects and `Client` is used for non-Web projects.</span></span> <span data-ttu-id="72879-144">Akceptowane wartości to `Both`, `Client`, `Default`, `None`, `Server`.</span><span class="sxs-lookup"><span data-stu-id="72879-144">Accepted values are `Both`, `Client`, `Default`, `None`, `Server`.</span></span>
| <span data-ttu-id="72879-145">-i</span><span class="sxs-lookup"><span data-stu-id="72879-145">-i</span></span> | <span data-ttu-id="72879-146">--dodatkowe-import-katalogów</span><span class="sxs-lookup"><span data-stu-id="72879-146">--additional-import-dirs</span></span> | <span data-ttu-id="72879-147">Dodatkowe katalogi, które mają być używane podczas rozpoznawania importu dla plików protobuf.</span><span class="sxs-lookup"><span data-stu-id="72879-147">Additional directories to be used when resolving imports for the protobuf files.</span></span> <span data-ttu-id="72879-148">Jest to rozdzielana średnikami lista ścieżek.</span><span class="sxs-lookup"><span data-stu-id="72879-148">This is a semicolon separated list of paths.</span></span>
| | <span data-ttu-id="72879-149">--dostęp</span><span class="sxs-lookup"><span data-stu-id="72879-149">--access</span></span> | <span data-ttu-id="72879-150">Modyfikator dostępu, który ma być używany dla C# wygenerowanych klas.</span><span class="sxs-lookup"><span data-stu-id="72879-150">The access modifier to use for the generated C# classes.</span></span> <span data-ttu-id="72879-151">Wartość domyślna to `Public`.</span><span class="sxs-lookup"><span data-stu-id="72879-151">The default value is `Public`.</span></span> <span data-ttu-id="72879-152">Akceptowane wartości to `Internal` i `Public`.</span><span class="sxs-lookup"><span data-stu-id="72879-152">Accepted values are `Internal` and `Public`.</span></span>

### <a name="add-url"></a><span data-ttu-id="72879-153">Dodaj adres URL</span><span class="sxs-lookup"><span data-stu-id="72879-153">Add URL</span></span>

<span data-ttu-id="72879-154">Polecenie `add-url` służy do dodawania pliku zdalnego określonego przez źródłowy adres URL jako odwołanie protobuf.</span><span class="sxs-lookup"><span data-stu-id="72879-154">The `add-url` command is used to add a remote file specified by an source URL as Protobuf reference.</span></span> <span data-ttu-id="72879-155">Należy podać ścieżkę pliku, aby określić, gdzie pobrać plik zdalny.</span><span class="sxs-lookup"><span data-stu-id="72879-155">A file path must be provided to specify where to download the remote file.</span></span> <span data-ttu-id="72879-156">Ścieżka pliku może być względna względem bieżącego katalogu lub ścieżki bezwzględnej.</span><span class="sxs-lookup"><span data-stu-id="72879-156">The file path can be relative to the current directory or an absolute path.</span></span> <span data-ttu-id="72879-157">Jeśli ścieżka pliku znajduje się poza katalogiem projektu, zostanie dodany element `Link` w celu wyświetlenia pliku w folderze wirtualnym `Protos` w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="72879-157">If the file path is outside the project directory, a `Link` element is added to display the file under the virtual folder `Protos` in Visual Studio.</span></span>

### <a name="usage"></a><span data-ttu-id="72879-158">Użycie</span><span class="sxs-lookup"><span data-stu-id="72879-158">Usage</span></span>

```dotnetcli
dotnet-grpc add-url [options] <url>
```

#### <a name="arguments"></a><span data-ttu-id="72879-159">Argumenty</span><span class="sxs-lookup"><span data-stu-id="72879-159">Arguments</span></span>

| <span data-ttu-id="72879-160">Argument</span><span class="sxs-lookup"><span data-stu-id="72879-160">Argument</span></span> | <span data-ttu-id="72879-161">Opis</span><span class="sxs-lookup"><span data-stu-id="72879-161">Description</span></span> |
|-|-|
| <span data-ttu-id="72879-162">url</span><span class="sxs-lookup"><span data-stu-id="72879-162">url</span></span> | <span data-ttu-id="72879-163">Adres URL pliku protobuf zdalnego.</span><span class="sxs-lookup"><span data-stu-id="72879-163">The URL to a remote protobuf file.</span></span> |

#### <a name="options"></a><span data-ttu-id="72879-164">Opcje</span><span class="sxs-lookup"><span data-stu-id="72879-164">Options</span></span>

| <span data-ttu-id="72879-165">Opcja krótka</span><span class="sxs-lookup"><span data-stu-id="72879-165">Short option</span></span> | <span data-ttu-id="72879-166">Opcja Long</span><span class="sxs-lookup"><span data-stu-id="72879-166">Long option</span></span> | <span data-ttu-id="72879-167">Opis</span><span class="sxs-lookup"><span data-stu-id="72879-167">Description</span></span> |
|-|-|-|
| <span data-ttu-id="72879-168">-o</span><span class="sxs-lookup"><span data-stu-id="72879-168">-o</span></span> | <span data-ttu-id="72879-169">--output</span><span class="sxs-lookup"><span data-stu-id="72879-169">--output</span></span> | <span data-ttu-id="72879-170">Określa ścieżkę pobierania pliku protobuf zdalnego.</span><span class="sxs-lookup"><span data-stu-id="72879-170">Specifies the download path for the remote protobuf file.</span></span> <span data-ttu-id="72879-171">Jest to wymagana opcja.</span><span class="sxs-lookup"><span data-stu-id="72879-171">This is a required option.</span></span>
| <span data-ttu-id="72879-172">-p</span><span class="sxs-lookup"><span data-stu-id="72879-172">-p</span></span> | <span data-ttu-id="72879-173">--projekt</span><span class="sxs-lookup"><span data-stu-id="72879-173">--project</span></span> | <span data-ttu-id="72879-174">Ścieżka do pliku projektu, na którym mają być wykonywane działania.</span><span class="sxs-lookup"><span data-stu-id="72879-174">The path to the project file to operate on.</span></span> <span data-ttu-id="72879-175">Jeśli plik nie zostanie określony, polecenie przeszukuje bieżący katalog.</span><span class="sxs-lookup"><span data-stu-id="72879-175">If a file is not specified, the command searches the current directory for one.</span></span>
| <span data-ttu-id="72879-176">-s</span><span class="sxs-lookup"><span data-stu-id="72879-176">-s</span></span> | <span data-ttu-id="72879-177">--usługi</span><span class="sxs-lookup"><span data-stu-id="72879-177">--services</span></span> | <span data-ttu-id="72879-178">Typ usług gRPC, które mają zostać wygenerowane.</span><span class="sxs-lookup"><span data-stu-id="72879-178">The type of gRPC services that should be generated.</span></span> <span data-ttu-id="72879-179">Jeśli określono `Default`, `Both` jest używany dla projektów sieci Web i `Client` jest używany dla projektów innych niż sieci Web.</span><span class="sxs-lookup"><span data-stu-id="72879-179">If `Default` is specified, `Both` is used for Web projects and `Client` is used for non-Web projects.</span></span> <span data-ttu-id="72879-180">Akceptowane wartości to `Both`, `Client`, `Default`, `None`, `Server`.</span><span class="sxs-lookup"><span data-stu-id="72879-180">Accepted values are `Both`, `Client`, `Default`, `None`, `Server`.</span></span>
| <span data-ttu-id="72879-181">-i</span><span class="sxs-lookup"><span data-stu-id="72879-181">-i</span></span> | <span data-ttu-id="72879-182">--dodatkowe-import-katalogów</span><span class="sxs-lookup"><span data-stu-id="72879-182">--additional-import-dirs</span></span> | <span data-ttu-id="72879-183">Dodatkowe katalogi, które mają być używane podczas rozpoznawania importu dla plików protobuf.</span><span class="sxs-lookup"><span data-stu-id="72879-183">Additional directories to be used when resolving imports for the protobuf files.</span></span> <span data-ttu-id="72879-184">Jest to rozdzielana średnikami lista ścieżek.</span><span class="sxs-lookup"><span data-stu-id="72879-184">This is a semicolon separated list of paths.</span></span>
| | <span data-ttu-id="72879-185">--dostęp</span><span class="sxs-lookup"><span data-stu-id="72879-185">--access</span></span> | <span data-ttu-id="72879-186">Modyfikator dostępu, który ma być używany dla C# wygenerowanych klas.</span><span class="sxs-lookup"><span data-stu-id="72879-186">The access modifier to use for the generated C# classes.</span></span> <span data-ttu-id="72879-187">Wartość domyślna to `Public`.</span><span class="sxs-lookup"><span data-stu-id="72879-187">Default value is `Public`.</span></span> <span data-ttu-id="72879-188">Akceptowane wartości to `Internal` i `Public`.</span><span class="sxs-lookup"><span data-stu-id="72879-188">Accepted values are `Internal` and `Public`.</span></span>

## <a name="remove"></a><span data-ttu-id="72879-189">Usuń</span><span class="sxs-lookup"><span data-stu-id="72879-189">Remove</span></span>

<span data-ttu-id="72879-190">Polecenie `remove` służy do usuwania odwołań protobuf z pliku *csproj* .</span><span class="sxs-lookup"><span data-stu-id="72879-190">The `remove` command is used to remove Protobuf references from the *.csproj* file.</span></span> <span data-ttu-id="72879-191">Polecenie akceptuje argumenty ścieżki i źródłowe adresy URL jako argumenty.</span><span class="sxs-lookup"><span data-stu-id="72879-191">The command accepts path arguments and source URLs as arguments.</span></span> <span data-ttu-id="72879-192">Narzędzie:</span><span class="sxs-lookup"><span data-stu-id="72879-192">The tool:</span></span>

* <span data-ttu-id="72879-193">Usuwa odwołanie protobuf.</span><span class="sxs-lookup"><span data-stu-id="72879-193">Only removes the Protobuf reference.</span></span>
* <span data-ttu-id="72879-194">Nie usuwa pliku *. proto* , nawet jeśli został pierwotnie pobrany ze zdalnego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="72879-194">Does not delete the *.proto* file, even if it was originally downloaded from a remote URL.</span></span>

### <a name="usage"></a><span data-ttu-id="72879-195">Użycie</span><span class="sxs-lookup"><span data-stu-id="72879-195">Usage</span></span>

```dotnetcli
dotnet-grpc remove [options] <references>...
```

### <a name="arguments"></a><span data-ttu-id="72879-196">Argumenty</span><span class="sxs-lookup"><span data-stu-id="72879-196">Arguments</span></span>

| <span data-ttu-id="72879-197">Argument</span><span class="sxs-lookup"><span data-stu-id="72879-197">Argument</span></span> | <span data-ttu-id="72879-198">Opis</span><span class="sxs-lookup"><span data-stu-id="72879-198">Description</span></span> |
|-|-|
| <span data-ttu-id="72879-199">wołują</span><span class="sxs-lookup"><span data-stu-id="72879-199">references</span></span> | <span data-ttu-id="72879-200">Adresy URL lub ścieżki plików odwołań protobuf do usunięcia.</span><span class="sxs-lookup"><span data-stu-id="72879-200">The URLs or file paths of the protobuf references to remove.</span></span> |

### <a name="options"></a><span data-ttu-id="72879-201">Opcje</span><span class="sxs-lookup"><span data-stu-id="72879-201">Options</span></span>

| <span data-ttu-id="72879-202">Opcja krótka</span><span class="sxs-lookup"><span data-stu-id="72879-202">Short option</span></span> | <span data-ttu-id="72879-203">Opcja Long</span><span class="sxs-lookup"><span data-stu-id="72879-203">Long option</span></span> | <span data-ttu-id="72879-204">Opis</span><span class="sxs-lookup"><span data-stu-id="72879-204">Description</span></span> |
|-|-|-|
| <span data-ttu-id="72879-205">-p</span><span class="sxs-lookup"><span data-stu-id="72879-205">-p</span></span> | <span data-ttu-id="72879-206">--projekt</span><span class="sxs-lookup"><span data-stu-id="72879-206">--project</span></span> | <span data-ttu-id="72879-207">Ścieżka do pliku projektu, na którym mają być wykonywane działania.</span><span class="sxs-lookup"><span data-stu-id="72879-207">The path to the project file to operate on.</span></span> <span data-ttu-id="72879-208">Jeśli plik nie zostanie określony, polecenie przeszukuje bieżący katalog.</span><span class="sxs-lookup"><span data-stu-id="72879-208">If a file is not specified, the command searches the current directory for one.</span></span>

## <a name="refresh"></a><span data-ttu-id="72879-209">Odśwież</span><span class="sxs-lookup"><span data-stu-id="72879-209">Refresh</span></span>

<span data-ttu-id="72879-210">Polecenie `refresh` służy do aktualizowania odwołania zdalnego z najnowszą zawartością ze źródłowego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="72879-210">The `refresh` command is used to update a remote reference with the latest content from the source URL.</span></span> <span data-ttu-id="72879-211">Aby określić odwołanie, które ma zostać zaktualizowane, można użyć zarówno ścieżki pliku pobierania, jak i źródłowego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="72879-211">Both the download file path and the source URL can be used to specify the reference to be updated.</span></span> <span data-ttu-id="72879-212">Uwaga:</span><span class="sxs-lookup"><span data-stu-id="72879-212">Note:</span></span>

* <span data-ttu-id="72879-213">Skróty zawartości plików są porównywane, aby określić, czy plik lokalny powinien zostać zaktualizowany.</span><span class="sxs-lookup"><span data-stu-id="72879-213">The hashes of the file contents are compared to determine whether the local file should be updated.</span></span>
* <span data-ttu-id="72879-214">Nie są porównywane żadne informacje o znaczniku czasu.</span><span class="sxs-lookup"><span data-stu-id="72879-214">No timestamp information is compared.</span></span>

<span data-ttu-id="72879-215">Narzędzie zawsze zastępuje plik lokalny plikiem zdalnym, jeśli jest wymagana aktualizacja.</span><span class="sxs-lookup"><span data-stu-id="72879-215">The tool always replaces the local file with the remote file if an update is needed.</span></span>

### <a name="usage"></a><span data-ttu-id="72879-216">Użycie</span><span class="sxs-lookup"><span data-stu-id="72879-216">Usage</span></span>

```dotnetcli
dotnet-grpc refresh [options] [<references>...]
```

### <a name="arguments"></a><span data-ttu-id="72879-217">Argumenty</span><span class="sxs-lookup"><span data-stu-id="72879-217">Arguments</span></span>

| <span data-ttu-id="72879-218">Argument</span><span class="sxs-lookup"><span data-stu-id="72879-218">Argument</span></span> | <span data-ttu-id="72879-219">Opis</span><span class="sxs-lookup"><span data-stu-id="72879-219">Description</span></span> |
|-|-|
| <span data-ttu-id="72879-220">wołują</span><span class="sxs-lookup"><span data-stu-id="72879-220">references</span></span> | <span data-ttu-id="72879-221">Adresy URL lub ścieżki plików do zdalnych odwołań protobuf, które powinny zostać zaktualizowane.</span><span class="sxs-lookup"><span data-stu-id="72879-221">The URLs or file paths to remote protobuf references that should be updated.</span></span> <span data-ttu-id="72879-222">Pozostaw ten argument pusty, aby odświeżyć wszystkie odwołania zdalne.</span><span class="sxs-lookup"><span data-stu-id="72879-222">Leave this argument empty to refresh all remote references.</span></span> |

### <a name="options"></a><span data-ttu-id="72879-223">Opcje</span><span class="sxs-lookup"><span data-stu-id="72879-223">Options</span></span>

| <span data-ttu-id="72879-224">Opcja krótka</span><span class="sxs-lookup"><span data-stu-id="72879-224">Short option</span></span> | <span data-ttu-id="72879-225">Opcja Long</span><span class="sxs-lookup"><span data-stu-id="72879-225">Long option</span></span> | <span data-ttu-id="72879-226">Opis</span><span class="sxs-lookup"><span data-stu-id="72879-226">Description</span></span> |
|-|-|-|
| <span data-ttu-id="72879-227">-p</span><span class="sxs-lookup"><span data-stu-id="72879-227">-p</span></span> | <span data-ttu-id="72879-228">--projekt</span><span class="sxs-lookup"><span data-stu-id="72879-228">--project</span></span> | <span data-ttu-id="72879-229">Ścieżka do pliku projektu, na którym mają być wykonywane działania.</span><span class="sxs-lookup"><span data-stu-id="72879-229">The path to the project file to operate on.</span></span> <span data-ttu-id="72879-230">Jeśli plik nie zostanie określony, polecenie przeszukuje bieżący katalog.</span><span class="sxs-lookup"><span data-stu-id="72879-230">If a file is not specified, the command searches the current directory for one.</span></span>
| | <span data-ttu-id="72879-231">--osuszyć-Run</span><span class="sxs-lookup"><span data-stu-id="72879-231">--dry-run</span></span> | <span data-ttu-id="72879-232">Wyświetla listę plików, które zostaną zaktualizowane bez pobierania nowej zawartości.</span><span class="sxs-lookup"><span data-stu-id="72879-232">Outputs a list of files that would be updated without downloading any new content.</span></span>

## <a name="list"></a><span data-ttu-id="72879-233">Lista</span><span class="sxs-lookup"><span data-stu-id="72879-233">List</span></span>

<span data-ttu-id="72879-234">Polecenie `list` służy do wyświetlania wszystkich odwołań protobuf w pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="72879-234">The `list` command is used to display all the Protobuf references in the project file.</span></span> <span data-ttu-id="72879-235">Jeśli wszystkie wartości w kolumnie są wartościami domyślnymi, kolumna może zostać pominięta.</span><span class="sxs-lookup"><span data-stu-id="72879-235">If all values of a column are default values, the column may be omitted.</span></span>

### <a name="usage"></a><span data-ttu-id="72879-236">Użycie</span><span class="sxs-lookup"><span data-stu-id="72879-236">Usage</span></span>

```dotnetcli
dotnet-grpc list [options]
```

### <a name="options"></a><span data-ttu-id="72879-237">Opcje</span><span class="sxs-lookup"><span data-stu-id="72879-237">Options</span></span>

| <span data-ttu-id="72879-238">Opcja krótka</span><span class="sxs-lookup"><span data-stu-id="72879-238">Short option</span></span> | <span data-ttu-id="72879-239">Opcja Long</span><span class="sxs-lookup"><span data-stu-id="72879-239">Long option</span></span> | <span data-ttu-id="72879-240">Opis</span><span class="sxs-lookup"><span data-stu-id="72879-240">Description</span></span> |
|-|-|-|
| <span data-ttu-id="72879-241">-p</span><span class="sxs-lookup"><span data-stu-id="72879-241">-p</span></span> | <span data-ttu-id="72879-242">--projekt</span><span class="sxs-lookup"><span data-stu-id="72879-242">--project</span></span> | <span data-ttu-id="72879-243">Ścieżka do pliku projektu, na którym mają być wykonywane działania.</span><span class="sxs-lookup"><span data-stu-id="72879-243">The path to the project file to operate on.</span></span> <span data-ttu-id="72879-244">Jeśli plik nie zostanie określony, polecenie przeszukuje bieżący katalog.</span><span class="sxs-lookup"><span data-stu-id="72879-244">If a file is not specified, the command searches the current directory for one.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="72879-245">Zasoby dodatkowe</span><span class="sxs-lookup"><span data-stu-id="72879-245">Additional resources</span></span>

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/aspnetcore>
