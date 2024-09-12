---
layout: default
---

{% assign quote = site.data.quotes | sample %}
> {{quote.text}}  
>-- {{quote.author}}

## Useful Libraries and Packages

- [Microsoft.Extensions.DependencyInjection](https://github.com/dotnet/runtime/blob/main/src/libraries/Microsoft.Extensions.DependencyInjection/README.md) - provides a simple IoC container for registering and resolving application services.

- [Microsoft.Identity.Web](/libs/microsoft-identity-web.html) - simplifies authentication and authorization for Azure AD and B2C.

- [FluentAssertions](https://fluentassertions.com/) - provides a fluent API for asserting the results of unit tests.