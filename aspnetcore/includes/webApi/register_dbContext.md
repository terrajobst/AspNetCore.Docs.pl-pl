## <a name="register-the-database-context"></a><span data-ttu-id="c6d4b-101">Zarejestruj kontekst bazy danych</span><span class="sxs-lookup"><span data-stu-id="c6d4b-101">Register the database context</span></span>

<span data-ttu-id="c6d4b-102">W tym kroku kontekst bazy danych jest zarejestrowany w [iniekcji zależności](xref:fundamentals/dependency-injection) kontenera.</span><span class="sxs-lookup"><span data-stu-id="c6d4b-102">In this step, the database context is registered with the [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="c6d4b-103">Usługi (takie jak kontekst bazy danych), które są zarejestrowane w usłudze kontenera iniekcji (Podpisane) zależności są dostępne do kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="c6d4b-103">Services (such as the DB context) that are registered with the dependency injection (DI) container are available to the controllers.</span></span>

<span data-ttu-id="c6d4b-104">Zarejestruj kontekst bazy danych z kontenerem usługi przy użyciu wbudowaną obsługę [iniekcji zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="c6d4b-104">Register the DB context with the service container using the built-in support for [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="c6d4b-105">Zastąp zawartość *Startup.cs* pliku następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="c6d4b-105">Replace the contents of the *Startup.cs* file with the following code:</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="c6d4b-106">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Startup.cs?highlight=3,5,13-14)]</span><span class="sxs-lookup"><span data-stu-id="c6d4b-106">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Startup.cs?highlight=3,5,13-14)]</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="c6d4b-107">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Startup.cs?highlight=2,4,12-13)]</span><span class="sxs-lookup"><span data-stu-id="c6d4b-107">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Startup.cs?highlight=2,4,12-13)]</span></span>

::: moniker-end  

<span data-ttu-id="c6d4b-108">Poprzedni kod:</span><span class="sxs-lookup"><span data-stu-id="c6d4b-108">The preceding code:</span></span>

* <span data-ttu-id="c6d4b-109">Usunięcie nieużywanych kodu.</span><span class="sxs-lookup"><span data-stu-id="c6d4b-109">Removes the unused code.</span></span>
* <span data-ttu-id="c6d4b-110">Określa, że bazy danych w pamięci są wstrzykiwane do kontenera usług.</span><span class="sxs-lookup"><span data-stu-id="c6d4b-110">Specifies an in-memory database is injected into the service container.</span></span>
