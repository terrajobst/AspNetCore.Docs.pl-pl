### <a name="use-sqlite-for-development-sql-server-for-production"></a>Używanie oprogramowania SQLite do programowania, SQL Server do produkcji

Po wybraniu oprogramowania SQLite kod wygenerowany przez szablon jest gotowy do programowania. Poniższy kod pokazuje, jak wstrzyknąć <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> do uruchomienia. `IWebHostEnvironment` jest wprowadzany, więc `ConfigureServices` mogą używać oprogramowania SQLite w programowaniu i SQL Server w środowisku produkcyjnym.

[!code-csharp[](~/includes/RP/code/StartupDevProd.cs?name=snippet&highlight=5,10,14)]
