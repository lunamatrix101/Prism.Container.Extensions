# Prism.Container.Extensions

The Prism Container Extensions provide various additional extensions making the Prism Container easier to use with Splat, IServiceCollection/IServiceProvider and in scenarios where you may require a Singleton container that may need to be initialized from Platform specific code prior to PrismApplication being created.


## Initialization

The PrismContainerExtension can be initialized automatically and accessed by simply calling `PrismContainerExtension.Current`. You can also create a new container with any of the following methods:

```cs
PrismContainerExtension.Create(new Container());

// OR

new PrismContainerExtension(new Container());
```

**NOTE** That by default the container extension will ensure that the underlying container is properly configured to work with Prism Applications.

## Modifying PrismApplication

When using the extended container extension you simply need to add the following to your PrismApplication to ensure that it uses the same instance that may have been created prior to the initialization of PrismApplication.

```cs
protected override IContainerExtension CreateContainerExtension() => PrismContainerExtension.Current;
```

## Working With Shiny

Shiny uses the Microsoft.Extensions.DependencyInjection pattern found in ASP.NET Core applications with a Startup class. This in particular is a use case in which you will need to initialize a container prior to Forms.Init being called on the native platform. To work with Shiny you simply need to do something like the following:

```cs
public class PrismStartup : Startup
{
    public override void ConfigureServices(IServiceCollection builder)
    {
        // Register services with Shiny like: services.UseGpsBackground<MyDelegate>();
    }

    public override IServiceProvider CreateServiceProvider(IServiceCollection services)
    {
        return PrismContainerExtension.Current.CreateServiceProvider(services);
    }
}
```