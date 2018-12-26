> [!WARNING]
> <span data-ttu-id="a0965-101">Ze względów bezpieczeństwa należy zgody na uczestnictwo w powiązania `GET` żądanie danych na stronie właściwości modelu.</span><span class="sxs-lookup"><span data-stu-id="a0965-101">For security reasons, you must opt in to binding `GET` request data to page model properties.</span></span> <span data-ttu-id="a0965-102">Sprawdź dane wejściowe użytkownika przed mapowania ich właściwości.</span><span class="sxs-lookup"><span data-stu-id="a0965-102">Verify user input before mapping it to properties.</span></span> <span data-ttu-id="a0965-103">Włączenie w celu `GET` powiązania jest przydatne w przypadku, gdy adresowania scenariusze, które zależą od wartości trasy lub ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="a0965-103">Opting in to `GET` binding is useful when addressing scenarios which rely on query string or route values.</span></span>
>
> <span data-ttu-id="a0965-104">Można powiązać właściwości `GET` żądania, ustaw [[BindProperty]](/dotnet/api/microsoft.aspnetcore.mvc.bindpropertyattribute) atrybutu `SupportsGet` właściwości `true`: `[BindProperty(SupportsGet = true)]`</span><span class="sxs-lookup"><span data-stu-id="a0965-104">To bind a property on `GET` requests, set the [[BindProperty]](/dotnet/api/microsoft.aspnetcore.mvc.bindpropertyattribute) attribute's `SupportsGet` property to `true`: `[BindProperty(SupportsGet = true)]`</span></span>