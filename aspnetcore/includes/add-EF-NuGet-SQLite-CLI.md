<span data-ttu-id="61240-101">Uruchom następujące polecenia interfejs wiersza polecenia platformy .NET Core:</span><span class="sxs-lookup"><span data-stu-id="61240-101">Run the following .NET Core CLI commands:</span></span>

```dotnetcli
dotnet tool install --global dotnet-ef
dotnet add package Microsoft.EntityFrameworkCore.SQLite
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="61240-102">Powyższe polecenia powodują dodanie:</span><span class="sxs-lookup"><span data-stu-id="61240-102">The preceding commands add:</span></span>

* <span data-ttu-id="61240-103">Narzędzie do tworzenia [szkieletu ASPNET-CodeGenerator](xref:fundamentals/tools/dotnet-aspnet-codegenerator).</span><span class="sxs-lookup"><span data-stu-id="61240-103">The [aspnet-codegenerator scaffolding tool](xref:fundamentals/tools/dotnet-aspnet-codegenerator).</span></span>
* <span data-ttu-id="61240-104">Entity Framework Core narzędzia dla interfejs wiersza polecenia platformy .NET Core.</span><span class="sxs-lookup"><span data-stu-id="61240-104">The Entity Framework Core Tools for the .NET Core CLI.</span></span>
* <span data-ttu-id="61240-105">EF Core dostawca oprogramowania SQLite, który instaluje pakiet EF Core jako zależność.</span><span class="sxs-lookup"><span data-stu-id="61240-105">The EF Core SQLite provider, which installs the EF Core package as a dependency.</span></span>
* <span data-ttu-id="61240-106">Pakiety wymagające tworzenia szkieletów `Microsoft.VisualStudio.Web.CodeGeneration.Design` : `Microsoft.EntityFrameworkCore.SqlServer`i.</span><span class="sxs-lookup"><span data-stu-id="61240-106">Packages needed for scaffolding: `Microsoft.VisualStudio.Web.CodeGeneration.Design` and `Microsoft.EntityFrameworkCore.SqlServer`.</span></span>
