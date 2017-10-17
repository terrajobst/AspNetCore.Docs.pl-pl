## <a name="register-the-database-context"></a>Zarejestruj kontekst bazy danych

Aby wstawić kontekst bazy danych do kontrolera, należy zarejestrować go z [iniekcji zależności](xref:fundamentals/dependency-injection) kontenera. Zarejestruj kontekst bazy danych z kontenerem usługi przy użyciu wbudowaną obsługę [iniekcji zależności](xref:fundamentals/dependency-injection). Zastąp zawartość *Startup.cs* pliku następującym kodem:

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Startup.cs?highlight=2,4,12)]

Poprzedni kod:

* Usuwa kod, który firma Microsoft nie korzysta.
* Określa, że bazy danych w pamięci są wstrzykiwane do kontenera usług.
