# ProjectSetup
A list of things I like to set up on the projects I work on.

## General

This is a list of things I like to do on all repos, no matter the technology.

A lot of this includes verification checks in the pipeline.
Make sure the CI-pipeline is smart and only runs steps when required to save time.
For instance, don't run backend unit tests if only front end files have changed.

### Editorconfig

Make sure the `.editorconfig` file is set correctly, and enforce formatting rules in the pipeline.
This makes sure that all the developers have set up their environment correctly, with format-on-save or pre-commit hooks.
This results in git diffs that only include the intended changes, and no diffs caused by different configuration.

### Gitattributes

The `.gitattributes` file is a tool for managing how Git handles specific files within the repository.
The file can be set to automatically normalize line endings to avoid merge conflicts across different operating systems.
For example, it can be configured to convert CRLF to LF.

### Logging

[IBM's Cost of a Data Breach Report for 2024](https://www.ibm.com/downloads/documents/us-en/107a02e94948f4ec) key findings state that it took 292 days to identify and contain breaches involving stolen credentials.
Consider keeping logs longer than that, possibly for a whole year.

Remember to log successful and failed login attempt and from which IP, and add alerts for suspicious behavour.

## .NET

This is a list of things I like to do on .NET apps.

### HTTP Headers

Some HTTP-headers should be set for security reasons, and others should be removed for the same reason.

Add and configure security headers recommended by [HTTP Observatory](https://developer.mozilla.org/en-US/observatory) and [securityheaders.com](https://securityheaders.com/):
- `HSTS`(probably added by .NET-template)
- `Content-Security-Policy`
    - Consider adding `Reporting-Endpoints` as well
- `Permissions-Policy`
- `Referrer-Policy`
- `X-Frame-Options`
- `X-Content-Type-Options`
- Cross Origin
    - `Cross-Origin-Resource-Policy`
    - `Cross-Origin-Embedder-Policy`
    - `Cross-Origin-Opener-Policy`
    - CORS: Cross Origin Resource Sharing


Remove  server and technology headers (`Server Header`, `X-Powered-By`, `X-AspNet-Version` etc)

If Cookie-based authentication is used, make sure to [prevent Cross-Site Request Forgery](https://learn.microsoft.com/en-us/aspnet/core/security/anti-request-forgery).

### NuGet

Require [signed NuGet-packages](https://learn.microsoft.com/en-us/nuget/consume-packages/installing-signed-packages) in the `nuget.config`-file.
This will help protect against supply chain attacks.

Set up [Central Package Management](https://learn.microsoft.com/en-us/nuget/consume-packages/central-package-management) to ease the dependency management across projects.

### Authorize by default

To prevent the endpoint being open by default, set a [fallback policy](https://learn.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.fallbackpolicy).

### Check DI registration

Enable [ValidateOnBuild](https://learn.microsoft.com/en-us/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validateonbuild) to prevent the app from starting if the service registration is invalid.

### Indexing

Add `robots.txt`, `meta`-tags for robots and/or `X-Robots-Tag`-header to prevent indexation or control what should be indexed.

### Rate limiting

If no API gateway is configured, add [rate limiting](https://learn.microsoft.com/en-us/aspnet/core/performance/rate-limit) to the API.
There is no user that wants to log in 100 times in 24 hours, so this is probably a brute-force attack.

### Pipeline

This is .NET-checks I like to check in the pipeline.

#### ContinuousIntegrationBuild

Set `ContinuousIntegrationBuild` to `true`.
See more on [Microsoft's documentation](https://learn.microsoft.com/en-us/dotnet/devops/dotnet-cli-and-continuous-integration).

#### Warnings

Make sure that the pipeline fails in warnings, including ReSharper-warnings through [ReSharper command line tools](https://www.jetbrains.com/help/resharper/ReSharper_Command_Line_Tools.html).
This will prevent warnings from sneaking into the repo over time.

## Frontend

This is a list of things I like to do in frontend projects.

### Strict mode

Enable strict mode.

### Pa11y

[Pa11y](https://pa11y.org/) is a tool to help verify that the site follows [WCAG](https://www.uutilsynet.no/wcag-standarden/wcag-standarden/86) and is accessible to anyone.
This should be checked in the pipeline.

### Pipeline

This is frontend-checks I like to check in the pipeline.

#### Warnings and errors

Make sure that the pipeline fails on warnings and errors from TypeScript and ESLint.

#### Source maps

Export the [source maps](https://web.dev/articles/source-maps) for the front end to ease debugging in test and production environments.