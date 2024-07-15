
# 超级简单的实现Swagger分组

Swagger 是一个流行的 API 文档生成工具，它可以帮助开发者快速生成 RESTful API 的文档。在大型项目中，我们可能需要对 API 进行分组，以便于更好地组织和管理。以下是如何在 ASP.NET Core 应用程序中使用 Swashbuckle 实现 Swagger 分组的详细步骤。

## 注册Swagger服务

首先，在 `Startup.cs` 或程序的构建容器中注册 Swagger 服务。这包括添加 Swagger Generator 服务和一个自定义的配置服务。

```csharp
public class Startup
{
    // 其他配置...

    public void ConfigureServices(IServiceCollection services)
    {
        // 添加 Swagger Generator 服务
        services.AddSwaggerGen();

        // 注入自定义的 Swagger 配置选项服务
        services.Configure<SwaggerGenOptions>(options =>
        {
            // 这里可以添加一些默认的 SwaggerGen 配置
        });

        // 注册自定义的 Swagger 配置选项实现
        services.AddSingleton<IConfigureOptions<SwaggerGenOptions>, SwaggerConfigureOptions>();
    }

    public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
    {
        // 使用 Swagger
        app.UseSwagger();

        // 使用 Swagger UI
        app.UseSwaggerUI(options =>
        {
            // Swagger UI 配置将在这里设置
        });
    }
}
```

## 配置Swagger分组

接下来，我们将创建一个自定义的 `IConfigureOptions<SwaggerGenOptions>` 实现，用于配置 Swagger 的分组。

```csharp
public class SwaggerConfigureOptions : IConfigureOptions<SwaggerGenOptions>
{
    private readonly IApiDescriptionGroupCollectionProvider _provider;

    public SwaggerConfigureOptions(IApiDescriptionGroupCollectionProvider provider)
    {
        _provider = provider;
    }

    public void Configure(SwaggerGenOptions options)
    {
        // 为每个 API 描述组创建一个 Swagger 文档
        foreach (var group in _provider.ApiDescriptionGroups.Items)
        {
            if (!string.IsNullOrEmpty(group.GroupName))
            {
                options.SwaggerDoc(group.GroupName, new OpenApiInfo
                {
                    Title = $"{group.GroupName} API",
                    Version = group.GroupName,
                    Description = $"Documentation for {group.GroupName} version of the API"
                });
            }
        }

        // 为没有明确分组的控制器创建默认分组
        options.SwaggerDoc("Default", new OpenApiInfo
        {
            Title = "Default API",
            Version = "Default",
            Description = "Default API documentation for unspecified versions"
        });
    }
}
```

## 使用中间件配置Swagger UI

在 `Configure` 方法中，我们将使用 Swagger UI 中间件来配置和启动 Swagger UI。

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    // 其他配置...

    // 配置 Swagger UI
    app.UseSwaggerUI(c =>
    {
        var provider = app.ApplicationServices.GetRequiredService<IApiDescriptionGroupCollectionProvider>();
        foreach (var group in provider.ApiDescriptionGroups.Items)
        {
            if (!string.IsNullOrEmpty(group.GroupName))
            {
                c.SwaggerEndpoint($"/swagger/{group.GroupName}/swagger.json", group.GroupName);
            }
        }

        // 为默认分组设置端点
        c.SwaggerEndpoint("/swagger/Default/swagger.json", "Default API");
    });
}
```

## 扩展

将同一个组的控制器放在一个文件夹中，通过命名空间来实现分组。
<https://www.cnblogs.com/dashiell-zhang/p/16520415.html>

### 推荐阅读

- [看看这样的Dotnet后台管理，那真是叫一个清新优雅高颜值！！！](https://mp.weixin.qq.com/s/XBwMUMWnakmLrTjQzz7xWg)
- [超级漂亮干净的Dotnet学习网站](https://mp.weixin.qq.com/s/wfcYZLPok8VnTK4fwDMp_A)
