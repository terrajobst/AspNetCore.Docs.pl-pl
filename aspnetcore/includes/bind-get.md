> [!WARNING]
> Ze względów bezpieczeństwa należy zgody na uczestnictwo w powiązania `GET` żądanie danych na stronie właściwości modelu. Sprawdź dane wejściowe użytkownika przed mapowania ich właściwości. Włączenie w celu `GET` powiązania jest przydatne w przypadku, gdy adresowania scenariusze, które zależą od wartości trasy lub ciągu zapytania.
>
> Można powiązać właściwości `GET` żądania, ustaw [[BindProperty]](/dotnet/api/microsoft.aspnetcore.mvc.bindpropertyattribute) atrybutu `SupportsGet` właściwości `true`: `[BindProperty(SupportsGet = true)]`
