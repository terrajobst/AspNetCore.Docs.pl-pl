---
title: Zarządzanie odwołaniami narzędzia Protobuf za pomocą narzędzia dotnet-GRPC
author: juntaoluo
description: Dowiedz się więcej na temat dodawania, aktualizowania, usuwania i wyświetlania protobuf odwołań za pomocą narzędzia dotnet-GRPC Global.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 10/17/2019
uid: grpc/dotnet-grpc
ms.openlocfilehash: 994597c854a95bb33de1686ab025cb3744cf6845
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78667336"
---
# <a name="manage-protobuf-references-with-dotnet-grpc"></a><span data-ttu-id="d4b48-103">Zarządzanie odwołaniami narzędzia Protobuf za pomocą narzędzia dotnet-GRPC</span><span class="sxs-lookup"><span data-stu-id="d4b48-103">Manage Protobuf references with dotnet-grpc</span></span>

<span data-ttu-id="d4b48-104">Przez [Jan Luo](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="d4b48-104">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="d4b48-105">`dotnet-grpc` to globalne narzędzie platformy .NET Core do zarządzania odwołaniami [protobuf ( *. proto*)](xref:grpc/basics#proto-file) w ramach projektu .NET gRPC.</span><span class="sxs-lookup"><span data-stu-id="d4b48-105">`dotnet-grpc` is a .NET Core Global Tool for managing [Protobuf (*.proto*)](xref:grpc/basics#proto-file) references within a .NET gRPC project.</span></span> <span data-ttu-id="d4b48-106">Narzędzie może służyć do dodawania, odświeżania, usuwania i wyświetlania listy odwołań protobuf.</span><span class="sxs-lookup"><span data-stu-id="d4b48-106">The tool can be used to add, refresh, remove, and list Protobuf references.</span></span>

## <a name="installation"></a><span data-ttu-id="d4b48-107">Instalacja</span><span class="sxs-lookup"><span data-stu-id="d4b48-107">Installation</span></span>

<span data-ttu-id="d4b48-108">Aby zainstalować [Narzędzie globalne `dotnet-grpc` .NET Core](/dotnet/core/tools/global-tools), uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="d4b48-108">To install the `dotnet-grpc` [.NET Core Global Tool](/dotnet/core/tools/global-tools), run the following command:</span></span>

```dotnetcli
dotnet tool install -g dotnet-grpc
```

## <a name="add-references"></a><span data-ttu-id="d4b48-109">Dodaj odwołania</span><span class="sxs-lookup"><span data-stu-id="d4b48-109">Add references</span></span>

<span data-ttu-id="d4b48-110">`dotnet-grpc` można użyć, aby dodać odwołania protobuf jako elementy `<Protobuf />` do pliku *csproj* :</span><span class="sxs-lookup"><span data-stu-id="d4b48-110">`dotnet-grpc` can be used to add Protobuf references as `<Protobuf />` items to the *.csproj* file:</span></span>

```xml
<Protobuf Include="Protos\greet.proto" GrpcServices="Server" />
```

<span data-ttu-id="d4b48-111">Odwołania do protobuf są używane do generowania zasobów C# klienta i/lub serwera.</span><span class="sxs-lookup"><span data-stu-id="d4b48-111">The Protobuf references are used to generate the C# client and/or server assets.</span></span> <span data-ttu-id="d4b48-112">Narzędzie `dotnet-grpc` może:</span><span class="sxs-lookup"><span data-stu-id="d4b48-112">The `dotnet-grpc` tool can:</span></span>

* <span data-ttu-id="d4b48-113">Utwórz odwołanie protobuf z plików lokalnych na dysku.</span><span class="sxs-lookup"><span data-stu-id="d4b48-113">Create a Protobuf reference from local files on disk.</span></span>
* <span data-ttu-id="d4b48-114">Utwórz odwołanie protobuf z pliku zdalnego określonego przez adres URL.</span><span class="sxs-lookup"><span data-stu-id="d4b48-114">Create a Protobuf reference from a remote file specified by a URL.</span></span>
* <span data-ttu-id="d4b48-115">Upewnij się, że odpowiednie zależności pakietu gRPC są dodawane do projektu.</span><span class="sxs-lookup"><span data-stu-id="d4b48-115">Ensure the correct gRPC package dependencies are added to the project.</span></span>

<span data-ttu-id="d4b48-116">Na przykład pakiet `Grpc.AspNetCore` zostanie dodany do aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="d4b48-116">For example, the `Grpc.AspNetCore` package is added to a web app.</span></span> <span data-ttu-id="d4b48-117">`Grpc.AspNetCore` zawiera biblioteki i serwery gRPC oraz obsługę narzędzi.</span><span class="sxs-lookup"><span data-stu-id="d4b48-117">`Grpc.AspNetCore` contains gRPC server and client libraries and tooling support.</span></span> <span data-ttu-id="d4b48-118">Alternatywnie do aplikacji konsolowej są dodawane pakiety `Grpc.Net.Client`, `Grpc.Tools` i `Google.Protobuf`, które zawierają tylko biblioteki i narzędzia klienckie gRPC.</span><span class="sxs-lookup"><span data-stu-id="d4b48-118">Alternatively, the `Grpc.Net.Client`, `Grpc.Tools` and `Google.Protobuf` packages, which contain only the gRPC client libraries and tooling support, are added to a Console app.</span></span>

### <a name="add-file"></a><span data-ttu-id="d4b48-119">Dodaj plik</span><span class="sxs-lookup"><span data-stu-id="d4b48-119">Add file</span></span>

<span data-ttu-id="d4b48-120">`add-file` polecenie służy do dodawania plików lokalnych na dysku jako odwołań protobuf.</span><span class="sxs-lookup"><span data-stu-id="d4b48-120">The `add-file` command is used to add local files on disk as Protobuf references.</span></span> <span data-ttu-id="d4b48-121">Podane ścieżki plików:</span><span class="sxs-lookup"><span data-stu-id="d4b48-121">The file paths provided:</span></span>

* <span data-ttu-id="d4b48-122">Może być względna względem bieżącego katalogu lub ścieżek bezwzględnych.</span><span class="sxs-lookup"><span data-stu-id="d4b48-122">Can be relative to the current directory or absolute paths.</span></span>
* <span data-ttu-id="d4b48-123">Mogą zawierać symbole wieloznaczne dla [obsługi symboli wieloznacznych](https://wikipedia.org/wiki/Glob_(programming))plików opartych na wzorcu.</span><span class="sxs-lookup"><span data-stu-id="d4b48-123">May contain wild cards for pattern-based file [globbing](https://wikipedia.org/wiki/Glob_(programming)).</span></span>

<span data-ttu-id="d4b48-124">Jeśli jakiekolwiek pliki znajdują się poza katalogiem projektu, zostanie dodany element `Link`, aby wyświetlić plik w folderze `Protos` w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d4b48-124">If any files are outside the project directory, a `Link` element is added to display the file under the folder `Protos` in Visual Studio.</span></span>

### <a name="usage"></a><span data-ttu-id="d4b48-125">Sposób użycia</span><span class="sxs-lookup"><span data-stu-id="d4b48-125">Usage</span></span>

```dotnetcli
dotnet grpc add-file [options] <files>...
```

#### <a name="arguments"></a><span data-ttu-id="d4b48-126">Argumenty</span><span class="sxs-lookup"><span data-stu-id="d4b48-126">Arguments</span></span>

| <span data-ttu-id="d4b48-127">Argument</span><span class="sxs-lookup"><span data-stu-id="d4b48-127">Argument</span></span> | <span data-ttu-id="d4b48-128">Opis</span><span class="sxs-lookup"><span data-stu-id="d4b48-128">Description</span></span> |
|-|-|
| <span data-ttu-id="d4b48-129">files</span><span class="sxs-lookup"><span data-stu-id="d4b48-129">files</span></span> | <span data-ttu-id="d4b48-130">Odwołuje się do pliku protobuf.</span><span class="sxs-lookup"><span data-stu-id="d4b48-130">The protobuf file references.</span></span> <span data-ttu-id="d4b48-131">Mogą to być ścieżki do globalizowania dla lokalnych plików protobuf.</span><span class="sxs-lookup"><span data-stu-id="d4b48-131">These can be a path to glob for local protobuf files.</span></span> |

#### <a name="options"></a><span data-ttu-id="d4b48-132">Opcje</span><span class="sxs-lookup"><span data-stu-id="d4b48-132">Options</span></span>

| <span data-ttu-id="d4b48-133">Opcja krótka</span><span class="sxs-lookup"><span data-stu-id="d4b48-133">Short option</span></span> | <span data-ttu-id="d4b48-134">Opcja Long</span><span class="sxs-lookup"><span data-stu-id="d4b48-134">Long option</span></span> | <span data-ttu-id="d4b48-135">Opis</span><span class="sxs-lookup"><span data-stu-id="d4b48-135">Description</span></span> |
|-|-|-|
| <span data-ttu-id="d4b48-136">-p</span><span class="sxs-lookup"><span data-stu-id="d4b48-136">-p</span></span> | <span data-ttu-id="d4b48-137">--projekt</span><span class="sxs-lookup"><span data-stu-id="d4b48-137">--project</span></span> | <span data-ttu-id="d4b48-138">Ścieżka do pliku projektu, na którym mają być wykonywane działania.</span><span class="sxs-lookup"><span data-stu-id="d4b48-138">The path to the project file to operate on.</span></span> <span data-ttu-id="d4b48-139">Jeśli plik nie zostanie określony, polecenie przeszukuje bieżący katalog.</span><span class="sxs-lookup"><span data-stu-id="d4b48-139">If a file is not specified, the command searches the current directory for one.</span></span>
| <span data-ttu-id="d4b48-140">-s</span><span class="sxs-lookup"><span data-stu-id="d4b48-140">-s</span></span> | <span data-ttu-id="d4b48-141">--usługi</span><span class="sxs-lookup"><span data-stu-id="d4b48-141">--services</span></span> | <span data-ttu-id="d4b48-142">Typ usług gRPC, które mają zostać wygenerowane.</span><span class="sxs-lookup"><span data-stu-id="d4b48-142">The type of gRPC services that should be generated.</span></span> <span data-ttu-id="d4b48-143">Jeśli określono `Default`, `Both` jest używany dla projektów sieci Web i `Client` jest używany dla projektów innych niż sieci Web.</span><span class="sxs-lookup"><span data-stu-id="d4b48-143">If `Default` is specified, `Both` is used for Web projects and `Client` is used for non-Web projects.</span></span> <span data-ttu-id="d4b48-144">Akceptowane wartości to `Both`, `Client`, `Default`, `None`, `Server`.</span><span class="sxs-lookup"><span data-stu-id="d4b48-144">Accepted values are `Both`, `Client`, `Default`, `None`, `Server`.</span></span>
| <span data-ttu-id="d4b48-145">-i</span><span class="sxs-lookup"><span data-stu-id="d4b48-145">-i</span></span> | <span data-ttu-id="d4b48-146">--dodatkowe-import-katalogów</span><span class="sxs-lookup"><span data-stu-id="d4b48-146">--additional-import-dirs</span></span> | <span data-ttu-id="d4b48-147">Dodatkowe katalogi, które mają być używane podczas rozpoznawania importu dla plików protobuf.</span><span class="sxs-lookup"><span data-stu-id="d4b48-147">Additional directories to be used when resolving imports for the protobuf files.</span></span> <span data-ttu-id="d4b48-148">Jest to rozdzielana średnikami lista ścieżek.</span><span class="sxs-lookup"><span data-stu-id="d4b48-148">This is a semicolon separated list of paths.</span></span>
| | <span data-ttu-id="d4b48-149">--dostęp</span><span class="sxs-lookup"><span data-stu-id="d4b48-149">--access</span></span> | <span data-ttu-id="d4b48-150">Modyfikator dostępu, który ma być używany dla C# wygenerowanych klas.</span><span class="sxs-lookup"><span data-stu-id="d4b48-150">The access modifier to use for the generated C# classes.</span></span> <span data-ttu-id="d4b48-151">Wartością domyślną jest `Public`.</span><span class="sxs-lookup"><span data-stu-id="d4b48-151">The default value is `Public`.</span></span> <span data-ttu-id="d4b48-152">Akceptowane wartości to `Internal` i `Public`.</span><span class="sxs-lookup"><span data-stu-id="d4b48-152">Accepted values are `Internal` and `Public`.</span></span>

### <a name="add-url"></a><span data-ttu-id="d4b48-153">Dodawanie adresu URL</span><span class="sxs-lookup"><span data-stu-id="d4b48-153">Add URL</span></span>

<span data-ttu-id="d4b48-154">`add-url` polecenie służy do dodawania pliku zdalnego określonego przez źródłowy adres URL jako odwołanie protobuf.</span><span class="sxs-lookup"><span data-stu-id="d4b48-154">The `add-url` command is used to add a remote file specified by an source URL as Protobuf reference.</span></span> <span data-ttu-id="d4b48-155">Należy podać ścieżkę pliku, aby określić, gdzie pobrać plik zdalny.</span><span class="sxs-lookup"><span data-stu-id="d4b48-155">A file path must be provided to specify where to download the remote file.</span></span> <span data-ttu-id="d4b48-156">Ścieżka pliku może być względna względem bieżącego katalogu lub ścieżki bezwzględnej.</span><span class="sxs-lookup"><span data-stu-id="d4b48-156">The file path can be relative to the current directory or an absolute path.</span></span> <span data-ttu-id="d4b48-157">Jeśli ścieżka pliku znajduje się poza katalogiem projektu, zostanie dodany element `Link`, aby wyświetlić plik w folderze wirtualnym `Protos` w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d4b48-157">If the file path is outside the project directory, a `Link` element is added to display the file under the virtual folder `Protos` in Visual Studio.</span></span>

### <a name="usage"></a><span data-ttu-id="d4b48-158">Sposób użycia</span><span class="sxs-lookup"><span data-stu-id="d4b48-158">Usage</span></span>

```dotnetcli
dotnet-grpc add-url [options] <url>
```

#### <a name="arguments"></a><span data-ttu-id="d4b48-159">Argumenty</span><span class="sxs-lookup"><span data-stu-id="d4b48-159">Arguments</span></span>

| <span data-ttu-id="d4b48-160">Argument</span><span class="sxs-lookup"><span data-stu-id="d4b48-160">Argument</span></span> | <span data-ttu-id="d4b48-161">Opis</span><span class="sxs-lookup"><span data-stu-id="d4b48-161">Description</span></span> |
|-|-|
| <span data-ttu-id="d4b48-162">url</span><span class="sxs-lookup"><span data-stu-id="d4b48-162">url</span></span> | <span data-ttu-id="d4b48-163">Adres URL pliku protobuf zdalnego.</span><span class="sxs-lookup"><span data-stu-id="d4b48-163">The URL to a remote protobuf file.</span></span> |

#### <a name="options"></a><span data-ttu-id="d4b48-164">Opcje</span><span class="sxs-lookup"><span data-stu-id="d4b48-164">Options</span></span>

| <span data-ttu-id="d4b48-165">Opcja krótka</span><span class="sxs-lookup"><span data-stu-id="d4b48-165">Short option</span></span> | <span data-ttu-id="d4b48-166">Opcja Long</span><span class="sxs-lookup"><span data-stu-id="d4b48-166">Long option</span></span> | <span data-ttu-id="d4b48-167">Opis</span><span class="sxs-lookup"><span data-stu-id="d4b48-167">Description</span></span> |
|-|-|-|
| <span data-ttu-id="d4b48-168">-o</span><span class="sxs-lookup"><span data-stu-id="d4b48-168">-o</span></span> | <span data-ttu-id="d4b48-169">--output</span><span class="sxs-lookup"><span data-stu-id="d4b48-169">--output</span></span> | <span data-ttu-id="d4b48-170">Określa ścieżkę pobierania pliku protobuf zdalnego.</span><span class="sxs-lookup"><span data-stu-id="d4b48-170">Specifies the download path for the remote protobuf file.</span></span> <span data-ttu-id="d4b48-171">Ta opcja jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="d4b48-171">This is a required option.</span></span>
| <span data-ttu-id="d4b48-172">-p</span><span class="sxs-lookup"><span data-stu-id="d4b48-172">-p</span></span> | <span data-ttu-id="d4b48-173">--projekt</span><span class="sxs-lookup"><span data-stu-id="d4b48-173">--project</span></span> | <span data-ttu-id="d4b48-174">Ścieżka do pliku projektu, na którym mają być wykonywane działania.</span><span class="sxs-lookup"><span data-stu-id="d4b48-174">The path to the project file to operate on.</span></span> <span data-ttu-id="d4b48-175">Jeśli plik nie zostanie określony, polecenie przeszukuje bieżący katalog.</span><span class="sxs-lookup"><span data-stu-id="d4b48-175">If a file is not specified, the command searches the current directory for one.</span></span>
| <span data-ttu-id="d4b48-176">-s</span><span class="sxs-lookup"><span data-stu-id="d4b48-176">-s</span></span> | <span data-ttu-id="d4b48-177">--usługi</span><span class="sxs-lookup"><span data-stu-id="d4b48-177">--services</span></span> | <span data-ttu-id="d4b48-178">Typ usług gRPC, które mają zostać wygenerowane.</span><span class="sxs-lookup"><span data-stu-id="d4b48-178">The type of gRPC services that should be generated.</span></span> <span data-ttu-id="d4b48-179">Jeśli określono `Default`, `Both` jest używany dla projektów sieci Web i `Client` jest używany dla projektów innych niż sieci Web.</span><span class="sxs-lookup"><span data-stu-id="d4b48-179">If `Default` is specified, `Both` is used for Web projects and `Client` is used for non-Web projects.</span></span> <span data-ttu-id="d4b48-180">Akceptowane wartości to `Both`, `Client`, `Default`, `None`, `Server`.</span><span class="sxs-lookup"><span data-stu-id="d4b48-180">Accepted values are `Both`, `Client`, `Default`, `None`, `Server`.</span></span>
| <span data-ttu-id="d4b48-181">-i</span><span class="sxs-lookup"><span data-stu-id="d4b48-181">-i</span></span> | <span data-ttu-id="d4b48-182">--dodatkowe-import-katalogów</span><span class="sxs-lookup"><span data-stu-id="d4b48-182">--additional-import-dirs</span></span> | <span data-ttu-id="d4b48-183">Dodatkowe katalogi, które mają być używane podczas rozpoznawania importu dla plików protobuf.</span><span class="sxs-lookup"><span data-stu-id="d4b48-183">Additional directories to be used when resolving imports for the protobuf files.</span></span> <span data-ttu-id="d4b48-184">Jest to rozdzielana średnikami lista ścieżek.</span><span class="sxs-lookup"><span data-stu-id="d4b48-184">This is a semicolon separated list of paths.</span></span>
| | <span data-ttu-id="d4b48-185">--dostęp</span><span class="sxs-lookup"><span data-stu-id="d4b48-185">--access</span></span> | <span data-ttu-id="d4b48-186">Modyfikator dostępu, który ma być używany dla C# wygenerowanych klas.</span><span class="sxs-lookup"><span data-stu-id="d4b48-186">The access modifier to use for the generated C# classes.</span></span> <span data-ttu-id="d4b48-187">Wartość domyślna to `Public`.</span><span class="sxs-lookup"><span data-stu-id="d4b48-187">Default value is `Public`.</span></span> <span data-ttu-id="d4b48-188">Akceptowane wartości to `Internal` i `Public`.</span><span class="sxs-lookup"><span data-stu-id="d4b48-188">Accepted values are `Internal` and `Public`.</span></span>

## <a name="remove"></a><span data-ttu-id="d4b48-189">Remove</span><span class="sxs-lookup"><span data-stu-id="d4b48-189">Remove</span></span>

<span data-ttu-id="d4b48-190">Polecenie `remove` służy do usuwania odwołań protobuf z pliku *csproj* .</span><span class="sxs-lookup"><span data-stu-id="d4b48-190">The `remove` command is used to remove Protobuf references from the *.csproj* file.</span></span> <span data-ttu-id="d4b48-191">Polecenie akceptuje argumenty ścieżki i źródłowe adresy URL jako argumenty.</span><span class="sxs-lookup"><span data-stu-id="d4b48-191">The command accepts path arguments and source URLs as arguments.</span></span> <span data-ttu-id="d4b48-192">Narzędzie:</span><span class="sxs-lookup"><span data-stu-id="d4b48-192">The tool:</span></span>

* <span data-ttu-id="d4b48-193">Usuwa odwołanie protobuf.</span><span class="sxs-lookup"><span data-stu-id="d4b48-193">Only removes the Protobuf reference.</span></span>
* <span data-ttu-id="d4b48-194">Nie usuwa pliku *. proto* , nawet jeśli został pierwotnie pobrany ze zdalnego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="d4b48-194">Does not delete the *.proto* file, even if it was originally downloaded from a remote URL.</span></span>

### <a name="usage"></a><span data-ttu-id="d4b48-195">Sposób użycia</span><span class="sxs-lookup"><span data-stu-id="d4b48-195">Usage</span></span>

```dotnetcli
dotnet-grpc remove [options] <references>...
```

### <a name="arguments"></a><span data-ttu-id="d4b48-196">Argumenty</span><span class="sxs-lookup"><span data-stu-id="d4b48-196">Arguments</span></span>

| <span data-ttu-id="d4b48-197">Argument</span><span class="sxs-lookup"><span data-stu-id="d4b48-197">Argument</span></span> | <span data-ttu-id="d4b48-198">Opis</span><span class="sxs-lookup"><span data-stu-id="d4b48-198">Description</span></span> |
|-|-|
| <span data-ttu-id="d4b48-199">odwołania</span><span class="sxs-lookup"><span data-stu-id="d4b48-199">references</span></span> | <span data-ttu-id="d4b48-200">Adresy URL lub ścieżki plików odwołań protobuf do usunięcia.</span><span class="sxs-lookup"><span data-stu-id="d4b48-200">The URLs or file paths of the protobuf references to remove.</span></span> |

### <a name="options"></a><span data-ttu-id="d4b48-201">Opcje</span><span class="sxs-lookup"><span data-stu-id="d4b48-201">Options</span></span>

| <span data-ttu-id="d4b48-202">Opcja krótka</span><span class="sxs-lookup"><span data-stu-id="d4b48-202">Short option</span></span> | <span data-ttu-id="d4b48-203">Opcja Long</span><span class="sxs-lookup"><span data-stu-id="d4b48-203">Long option</span></span> | <span data-ttu-id="d4b48-204">Opis</span><span class="sxs-lookup"><span data-stu-id="d4b48-204">Description</span></span> |
|-|-|-|
| <span data-ttu-id="d4b48-205">-p</span><span class="sxs-lookup"><span data-stu-id="d4b48-205">-p</span></span> | <span data-ttu-id="d4b48-206">--projekt</span><span class="sxs-lookup"><span data-stu-id="d4b48-206">--project</span></span> | <span data-ttu-id="d4b48-207">Ścieżka do pliku projektu, na którym mają być wykonywane działania.</span><span class="sxs-lookup"><span data-stu-id="d4b48-207">The path to the project file to operate on.</span></span> <span data-ttu-id="d4b48-208">Jeśli plik nie zostanie określony, polecenie przeszukuje bieżący katalog.</span><span class="sxs-lookup"><span data-stu-id="d4b48-208">If a file is not specified, the command searches the current directory for one.</span></span>

## <a name="refresh"></a><span data-ttu-id="d4b48-209">Odświeżanie</span><span class="sxs-lookup"><span data-stu-id="d4b48-209">Refresh</span></span>

<span data-ttu-id="d4b48-210">Polecenie `refresh` służy do aktualizowania odwołania zdalnego z najnowszą zawartością ze źródłowego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="d4b48-210">The `refresh` command is used to update a remote reference with the latest content from the source URL.</span></span> <span data-ttu-id="d4b48-211">Aby określić odwołanie, które ma zostać zaktualizowane, można użyć zarówno ścieżki pliku pobierania, jak i źródłowego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="d4b48-211">Both the download file path and the source URL can be used to specify the reference to be updated.</span></span> <span data-ttu-id="d4b48-212">Uwaga:</span><span class="sxs-lookup"><span data-stu-id="d4b48-212">Note:</span></span>

* <span data-ttu-id="d4b48-213">Skróty zawartości plików są porównywane, aby określić, czy plik lokalny powinien zostać zaktualizowany.</span><span class="sxs-lookup"><span data-stu-id="d4b48-213">The hashes of the file contents are compared to determine whether the local file should be updated.</span></span>
* <span data-ttu-id="d4b48-214">Nie są porównywane żadne informacje o znaczniku czasu.</span><span class="sxs-lookup"><span data-stu-id="d4b48-214">No timestamp information is compared.</span></span>

<span data-ttu-id="d4b48-215">Narzędzie zawsze zastępuje plik lokalny plikiem zdalnym, jeśli jest wymagana aktualizacja.</span><span class="sxs-lookup"><span data-stu-id="d4b48-215">The tool always replaces the local file with the remote file if an update is needed.</span></span>

### <a name="usage"></a><span data-ttu-id="d4b48-216">Sposób użycia</span><span class="sxs-lookup"><span data-stu-id="d4b48-216">Usage</span></span>

```dotnetcli
dotnet-grpc refresh [options] [<references>...]
```

### <a name="arguments"></a><span data-ttu-id="d4b48-217">Argumenty</span><span class="sxs-lookup"><span data-stu-id="d4b48-217">Arguments</span></span>

| <span data-ttu-id="d4b48-218">Argument</span><span class="sxs-lookup"><span data-stu-id="d4b48-218">Argument</span></span> | <span data-ttu-id="d4b48-219">Opis</span><span class="sxs-lookup"><span data-stu-id="d4b48-219">Description</span></span> |
|-|-|
| <span data-ttu-id="d4b48-220">odwołania</span><span class="sxs-lookup"><span data-stu-id="d4b48-220">references</span></span> | <span data-ttu-id="d4b48-221">Adresy URL lub ścieżki plików do zdalnych odwołań protobuf, które powinny zostać zaktualizowane.</span><span class="sxs-lookup"><span data-stu-id="d4b48-221">The URLs or file paths to remote protobuf references that should be updated.</span></span> <span data-ttu-id="d4b48-222">Pozostaw ten argument pusty, aby odświeżyć wszystkie odwołania zdalne.</span><span class="sxs-lookup"><span data-stu-id="d4b48-222">Leave this argument empty to refresh all remote references.</span></span> |

### <a name="options"></a><span data-ttu-id="d4b48-223">Opcje</span><span class="sxs-lookup"><span data-stu-id="d4b48-223">Options</span></span>

| <span data-ttu-id="d4b48-224">Opcja krótka</span><span class="sxs-lookup"><span data-stu-id="d4b48-224">Short option</span></span> | <span data-ttu-id="d4b48-225">Opcja Long</span><span class="sxs-lookup"><span data-stu-id="d4b48-225">Long option</span></span> | <span data-ttu-id="d4b48-226">Opis</span><span class="sxs-lookup"><span data-stu-id="d4b48-226">Description</span></span> |
|-|-|-|
| <span data-ttu-id="d4b48-227">-p</span><span class="sxs-lookup"><span data-stu-id="d4b48-227">-p</span></span> | <span data-ttu-id="d4b48-228">--projekt</span><span class="sxs-lookup"><span data-stu-id="d4b48-228">--project</span></span> | <span data-ttu-id="d4b48-229">Ścieżka do pliku projektu, na którym mają być wykonywane działania.</span><span class="sxs-lookup"><span data-stu-id="d4b48-229">The path to the project file to operate on.</span></span> <span data-ttu-id="d4b48-230">Jeśli plik nie zostanie określony, polecenie przeszukuje bieżący katalog.</span><span class="sxs-lookup"><span data-stu-id="d4b48-230">If a file is not specified, the command searches the current directory for one.</span></span>
| | <span data-ttu-id="d4b48-231">--osuszyć-Run</span><span class="sxs-lookup"><span data-stu-id="d4b48-231">--dry-run</span></span> | <span data-ttu-id="d4b48-232">Wyświetla listę plików, które zostaną zaktualizowane bez pobierania nowej zawartości.</span><span class="sxs-lookup"><span data-stu-id="d4b48-232">Outputs a list of files that would be updated without downloading any new content.</span></span>

## <a name="list"></a><span data-ttu-id="d4b48-233">List</span><span class="sxs-lookup"><span data-stu-id="d4b48-233">List</span></span>

<span data-ttu-id="d4b48-234">Polecenie `list` służy do wyświetlania wszystkich odwołań protobuf w pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="d4b48-234">The `list` command is used to display all the Protobuf references in the project file.</span></span> <span data-ttu-id="d4b48-235">Jeśli wszystkie wartości w kolumnie są wartościami domyślnymi, kolumna może zostać pominięta.</span><span class="sxs-lookup"><span data-stu-id="d4b48-235">If all values of a column are default values, the column may be omitted.</span></span>

### <a name="usage"></a><span data-ttu-id="d4b48-236">Sposób użycia</span><span class="sxs-lookup"><span data-stu-id="d4b48-236">Usage</span></span>

```dotnetcli
dotnet-grpc list [options]
```

### <a name="options"></a><span data-ttu-id="d4b48-237">Opcje</span><span class="sxs-lookup"><span data-stu-id="d4b48-237">Options</span></span>

| <span data-ttu-id="d4b48-238">Opcja krótka</span><span class="sxs-lookup"><span data-stu-id="d4b48-238">Short option</span></span> | <span data-ttu-id="d4b48-239">Opcja Long</span><span class="sxs-lookup"><span data-stu-id="d4b48-239">Long option</span></span> | <span data-ttu-id="d4b48-240">Opis</span><span class="sxs-lookup"><span data-stu-id="d4b48-240">Description</span></span> |
|-|-|-|
| <span data-ttu-id="d4b48-241">-p</span><span class="sxs-lookup"><span data-stu-id="d4b48-241">-p</span></span> | <span data-ttu-id="d4b48-242">--projekt</span><span class="sxs-lookup"><span data-stu-id="d4b48-242">--project</span></span> | <span data-ttu-id="d4b48-243">Ścieżka do pliku projektu, na którym mają być wykonywane działania.</span><span class="sxs-lookup"><span data-stu-id="d4b48-243">The path to the project file to operate on.</span></span> <span data-ttu-id="d4b48-244">Jeśli plik nie zostanie określony, polecenie przeszukuje bieżący katalog.</span><span class="sxs-lookup"><span data-stu-id="d4b48-244">If a file is not specified, the command searches the current directory for one.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d4b48-245">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="d4b48-245">Additional resources</span></span>

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/aspnetcore>
