<a name="cs"></a>
### <a name="add-a-database-connection-string"></a><span data-ttu-id="39a20-101">Dodaj parametry połączenia bazy danych</span><span class="sxs-lookup"><span data-stu-id="39a20-101">Add a database connection string</span></span>

<span data-ttu-id="39a20-102">Dodaj parametry połączenia, aby *appsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="39a20-102">Add a connection string to the *appsettings.json* file.</span></span>

[!code-json[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

<a name="reg"></a>
###  <a name="register-the-database-context"></a><span data-ttu-id="39a20-103">Zarejestruj kontekst bazy danych</span><span class="sxs-lookup"><span data-stu-id="39a20-103">Register the database context</span></span>

<span data-ttu-id="39a20-104">Zarejestruj kontekst bazy danych za pomocą [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) kontenera w *Startup.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="39a20-104">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in the *Startup.cs* file.</span></span>
