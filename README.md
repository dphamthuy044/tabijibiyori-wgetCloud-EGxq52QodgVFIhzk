## 1、Blazor 简介

Blazor 是由Microsoft开发的一款基于.NET的开源交互式Web UI框架。Blazor使开发人员能够使用C#和HTML建立全堆栈的单页应用程序，并避免使用JavaScript。Blazor基于组件模型，该模型提供了以具有强类型的符合Razor标准的页面和组件的形式构建用户界面的能力。

Blazor的加入使得.NET开发人员有机会在客户端和服务器上使用同一种编程模型，同时享受到.NET的优势，比如其功能强大的运行时，标准库，语言互操作性和辅助开发者高效开发的工具等。

**在Blazor中，有两个主要的托管模型**：

* Blazor Server: 在此模式下，应用程序在服务器端运行，并使用SignalR实时通讯框架与浏览器进行交互。这种模型要求永久的有效连接，但是客户端的计算和下载需求会大大减低，所有的逻辑运行都在服务器端。
* Blazor WebAssembly: 在此模式下，应用程序直接在客户端的WebAssembly中运行，允许C#代码在浏览器中执行，不依赖服务器。

### Blazor 支持的浏览器：

下表所示的浏览器在移动平台和桌面平台上均支持 Blazor WebAssembly 和 Blazor Server。

|  |  |
| --- | --- |
| 浏览器 | Version |
| Apple Safari | 当前版本+ |
| Google Chrome | 当前版本+ |
| Microsoft Edge | 当前版本+ |
| Mozilla Firefox | 当前版本+ |

### Blazor三种托管模型及其各自特点

#### 1、Blazor Server

##### 简介：

Blazor Server 应用程序在服务器上运行，可享受完整的 .NET Core 运行时支持。所有处理都在服务器上完成，UI/DOM 更改通过 SignalR 连接回传给客户端。这种双向 SignalR 连接是在用户第一次从浏览器中加载应用程序时建立的。 由于 .NET 代码已经在服务器上运行，因此您无需为前端创建 API。您可以直接访问服务、数据库等，并在传统的服务端技术上做任何您想做的事情。在客户端上，Blazor 脚本 (`blazor.server.js`) 与服务器建立 SignalR 连接。 脚本由 ASP.NET Core 共享框架中的嵌入资源提供给客户端应用。 客户端应用负责根据需要保持和还原应用状态。

![](https://cdn.nlark.com/yuque/0/2025/png/61146596/1760338133603-59082a67-c49e-444e-8e19-a406a8948548.png)

##### Blazor Server 托管模型优点：

* 下载项大小明显小于 Blazor WebAssembly 应用，且应用加载速度快得多。
* 应用可以充分利用服务器功能，包括对 .NET Core API 的使用。
* 服务器上的 .NET Core 用于运行应用，因此调试等现有 .NET 工具可按预期正常工作。
* 支持瘦客户端。 例如，Blazor Server 应用适用于不支持 WebAssembly 的浏览器以及资源受限的设备。
* 应用的 .NET/C# 代码库（其中包括应用的组件代码）不适用于客户端。

##### Blazor Server 托管模型局限性：

* 通常延迟较高。 每次用户交互都涉及到网络跃点。
* 不支持脱机工作。 如果客户端连接失败，应用会停止工作。
* 若要缩放具有许多用户的应用，需要使用服务器资源处理多个客户端连接和客户端状态。
* 需要 ASP.NET Core 服务器为应用提供服务。 无服务器部署方案不可行，例如通过内容分发网络 (CDN) 为应用提供服务的方案。

#### 2、Blazor WebAssembly

##### 简介：

Blazor WebAssembly（WASM）应用程序在浏览器中基于WebAssembly的.NET运行时运行客户端。Blazor应用程序及其依赖项和.NET运行时被下载到浏览器中。该应用程序直接在浏览器的UI线程上执行。UI更新和事件处理在同一进程中进行。应用程序的资产被作为静态文件部署到能够为客户提供静态内容的网络服务器或服务上。当Blazor WebAssembly应用被创建用于部署，而没有后端ASP.NET Core应用为其提供文件时，该应用被称为独立的Blazor WebAssembly应用。当应用程序被创建用于部署，并有一个后端应用程序为其提供文件时，该应用程序被称为托管的Blazor WebAssembly应用程序。

![](https://cdn.nlark.com/yuque/0/2025/png/61146596/1760338234176-23b280ad-d128-488e-b93d-f4b6b69a2072.png)

##### Blazor WebAssembly 托管模型优点：

* 从服务器下载应用后，没有 .NET 服务器端依赖项，因此，如果服务器脱机，应用将保持正常运行。
* 可充分利用客户端资源和功能。
* 工作可从服务器转移到客户端。
* 无需 ASP.NET Core Web 服务器即可托管应用。 无服务器部署方案可行，例如通过内容分发网络 (CDN) 为应用提供服务的方案。

##### Blazor WebAssembly 托管模型局限性：

* 应用仅可使用浏览器功能。
* 需要可用的客户端硬件和软件（例如 WebAssembly 支持）。
* 下载项大小较大，应用加载耗时较长。

Blazor WebAssembly 支持预先 (AOT) 编译，你可以直接将 .NET 代码编译到 WebAssembly 中。 AOT 编译会提高运行时性能，代价是应用大小增加。

#### 3、Blazor Hybrid

##### 简介：

* Blazor 还可用于使用混合方法生成本机客户端应用。 混合应用是利用 Web 技术实现其功能的本机应用。 在 Blazor Hybrid 应用中，Razor 组件与任何其他 .NET 代码一起直接在本机应用中（而不在 WebAssembly 上）运行，并通过本地互操作通道基于 HTML 和 CSS 将 Web UI 呈现到嵌入式 Web View 控件。
* 可以使用不同的 .NET 本机应用框架（包括 .NET MAUI、WPF 和 Windows 窗体）生成 Blazor Hybrid 应用。 Blazor 提供 `BlazorWebView` 控件，将 Razor 组件添加到使用这些框架生成的应用。 通过结合使用 Blazor 和 .NET MAUI，可以便捷地生成适用于移动和桌面的跨平台 Blazor Hybrid 应用，而将 Blazor 与 WPF 和 Windows 窗体集成可以更好地实现现有应用的现代化。
* 由于 Blazor Hybrid 应用是本机应用，它们可以支持只有 Web 平台所没有的功能。 通过正常的 .NET API，Blazor Hybrid 应用对本机平台功能具有完全访问权限。 Blazor Hybrid 应用还可以与现有 Blazor Server 或 Blazor WebAssembly 应用共享和重复使用组件。 Blazor Hybrid 应用结合了 Web、本机应用和 .NET 平台的优点。

![](https://cdn.nlark.com/yuque/0/2025/png/61146596/1760338309545-02c658c1-1d11-4945-901c-9bb7d4135c2f.png)

##### Blazor Hybrid 托管模型优点：

* 重复使用可在移动、桌面和 Web 之间共享的现有组件。
* 利用 Web 开发技能、体验和资源。
* 应用对设备的本机功能具有完全访问权限。

##### Blazor Hybrid 托管模型局限性：

* 必须为每个目标平台生成、部署和维护单独的本机客户端应用。
* 与在浏览器中访问 Web 应用相比，查找、下载和安装本机客户端应用通常需要更长的时间。

## 2、安装 .NET SDK 和 Visual Studio/VS Code

### 如何安装.NET SDK？

是否单独安装.NET SDK取决于你使用的开发工具，如果选择使用Visual Studio开发，则不需要单独安装.NET SDK；如果选择使用VS Code进行开发，则需要单独安装.NET SDK。

#### 1. 下载 .NET SDK

1. 打开浏览器，访问 [.NET 官方下载页面](https://dotnet.microsoft.com/download/dotnet)。
2. 在页面中选择最新版本的 `.NET SDK`，通常会有多个版本可供选择。建议选择最新或稳定版本（根据你的需求而定）。

#### 2. 安装 .NET SDK

1. 下载完成后，双击运行下载的安装程序。
2. 按照安装向导的提示进行安装。一般情况下，我们可以接受默认设置。
3. 安装完成后，重新启动计算机（如果有提示的话）。

#### 3. 验证安装

安装完成后，我们需要验证 `.NET SDK` 是否安装成功。打开命令提示符（`cmd`）或 PowerShell，输入以下命令：

`dotnet --version`

如果您看到版本号的输出，那就表示您成功安装了 `，。`

`![](https://cdn.nlark.com/yuque/0/2025/png/61146596/1760334977353-0be75c42-5e69-45d3-a831-7d00e37794db.png)`

### `如何安装Visual Studio？`

`这里我们以VS2022为例：`

#### `1. 下载`

1. `打开官网，下滑，找到如下界面，点击下载下拉菜单，选择Community2022（这个是免费的）进行下载。`
2. `下载后得到安装包VisualStudioSetup.exe，如下图所示。`

#### `2. 安装`

1. `双击下载的安装包打开==》点击继续，等待下载安装完成==》选择需要的应用程序和组件，以及相应的SDK，左下角修改安装路径和缓存路径。`
2. `在选择安装路径时，要注意以下两点：`

1. `缓存文件与安装文件不能放一起，即二者的安装路径不能一样！`
2. `安装路径中不要出现中文，否则之后容易报错或者安装不成功！`

3. `路径选择好后，点击右下角安装，出现如下安装界面，等待安装即可（安装时间较长，请耐心等待）`
4. `安装完成后会出现“安装完毕”的提示，点击确定即可。`
5. `下一步点击“重启”按钮即可。`

### `如何安装VS Code？`

`下载并安装：在官网首页，选择适合你操作系统的版本进行下载。下载完成后，双击安装包进行安装。`

#### `1.下载`

`访问VSCode官网：首先，打开浏览器访问VSCode官网，地址：https://code.visualstudio.com/:slowerssr加速器官网，点击Download下载`

#### `2.安装`

1. `下载后直接点击打开文件`
2. `将选项都选中，尤其注意添加到PATH。然后点击下一步`
3. `进入安装页面，等待安装完毕。`
4. `安装完成，点击完成并打开vscode。`

## `3、创建第一个 Blazor Server 和 Blazor WebAssembly 项目`

`已Visual Studio 2022 为例`

### `1.Blazor Server 项目`

#### `在这里，我们创建一个Blazor Server 模式的程序。`

1. `启动 Visual Studio 2022 并选择“创建新项目”。`

`![](https://cdn.nlark.com/yuque/0/2025/png/61146596/1760339027790-f05b303a-8ec2-49c5-ad2b-26d83cba27b2.png)`

2. `在“创建新项目”窗口中，在搜索框中键入 Blazor Server，然后按 Enter。`
3. `选择“Blazor Server 应用”模板并选择“下一步”。`

`![](https://cdn.nlark.com/yuque/0/2025/png/61146596/1760339129286-d911453b-53bc-4f90-9a1f-36698b4a9b0b.png)`

4. `在“配置新项目”窗口中，输入 BlazorApp 作为项目名称，然后选择“下一步”。`

`![](https://cdn.nlark.com/yuque/0/2025/png/61146596/1760339225645-358fe56b-2f48-495f-a275-111c00767285.png)`

5. `在“其他信息”窗口中，如果尚未选择，则在“框架”下拉列表中选择“.NET 6.0 ”，然后单击“创建”按钮。`

`![](https://cdn.nlark.com/yuque/0/2025/png/61146596/1760339317555-e0e13e43-da13-4012-9a9e-adb3940fb563.png)`

#### `项目文件说明`

`![](https://cdn.nlark.com/yuque/0/2025/png/61146596/1760339377547-2e810f03-e401-44ff-9224-ab4272200fa1.png)`

* `wwwroot 文件夹：包含应用的公共静态资产，如 CSS、JavaScript 文件等。`
* `Program.cs 是启动服务器以及在其中配置应用服务和中间件的应用的入口点。`
* `App.razor 为应用的根组件。`
* `Pages 目录包含应用的一些示例网页。`
* `BlazorApp.csproj 定义应用项目及其依赖项，且可以通过双击解决方案资源管理器中的 BlazorApp 项目节点进行查看。`
* `Properties 目录中的 launchSettings.json 文件为本地开发环境定义不同的配置文件设置。创建项目时会自动分配端口号并将其保存在此文件上。`

#### `运行应用`

`单击 Visual Studio 调试工具栏中的“开始调试”按钮(绿色箭头)以运行应用。`

`![](https://cdn.nlark.com/yuque/0/2025/png/61146596/1760339529688-b8aee3db-543d-4494-b7b6-5b355ea62e98.png)`

`首次在 Visual Studio 中运行 Web 应用时，它将设置用于通过 HTTPS 托管应用的开发证书，然后提示你信任该证书。建议同意信任该证书。证书将仅用于本地开发，如果没有证书，大多数浏览器都会对网站的安全性进行投诉。`

`等待应用在浏览器中启动。转到以下页面后，你已成功运行第一个 Blazor 应用!`

`![](https://cdn.nlark.com/yuque/0/2025/png/61146596/1760339619953-55880bd1-9db9-46ed-905d-f6ba372a55a9.png)`

### `2.Blazor WebAssembly 项目`

#### `添加新项目`

`上面我们已经创建了一个Blazor Server的项目，现在我们可以直接在当前解决方案中继续创建Blazor WebAssembly 项目，或者重新打开VS 2022 进行创建都是可以的，这里我们就直接在当前解决方案中来进行创建新项目。`

1. `点击解决方案==》鼠标右键==》添加==》新建项目`
2. `在“创建新项目”窗口中，在搜索框中键入 Blazor WebAssembly，然后按 Enter。`
3. `选择“Blazor WebAssembly 独立应用”模板并选择“下一步”。`

`![](https://cdn.nlark.com/yuque/0/2025/png/61146596/1760340401113-31b2dc62-2648-492d-a3be-4bfbaeb5fa93.png)`

4. `在“配置新项目”窗口中，输入 BlazorAppWasm 作为项目名称，然后选择“下一步”。`

`![](https://cdn.nlark.com/yuque/0/2025/png/61146596/1760340489592-6ada0660-82f4-44b2-b475-0158d6b7ef8f.png)`

5. `在“其他信息”窗口中，如果尚未选择，则在“框架”下拉列表中选择“.NET 6.0 ”，然后单击“创建”按钮。`

`![](https://cdn.nlark.com/yuque/0/2025/png/61146596/1760340520077-05268b8c-f8ac-42ad-a404-5599c9d68032.png)`

#### `项目文件说明`

`![](https://cdn.nlark.com/yuque/0/2025/png/61146596/1760340554573-ae4d2e6f-526e-4131-bdea-eae7cc45c3fa.png)`

* `wwwroot 文件夹：包含应用的公共静态资产，如 CSS、JavaScript 文件等。`
* `Pages 文件夹：包含可路由的 Razor 组件，每个页面的路由通过 @page 指令指定。`
* `Shared 文件夹：包含共享组件，如布局组件。`
* `Program.cs：应用的入口点，用于设置 WebAssembly 主机，注册服务和配置应用。`
* `App.razor：应用的根组件，使用 Router 组件设置客户端路由。`

#### `运行应用`

`将此项目设为启动项，单击 Visual Studio 调试工具栏中的“开始调试”按钮(绿色箭头)以运行应用。`

`![](https://cdn.nlark.com/yuque/0/2025/png/61146596/1760340976828-28568587-c03f-41fe-a39a-153613ec3f69.png)`

`等待应用在浏览器中启动。转到以下页面后，你已成功运行第一个 Blazor WebAssembly 应用!`

`![](https://cdn.nlark.com/yuque/0/2025/png/61146596/1760341056294-ef14ee28-7c50-4d84-900e-fb4431797d81.png)`

`使用VS Code创建Blazor项目教程 ：`

`https://dotnet.microsoft.com/zh-cn/learn/aspnet/blazor-tutorial/create`

### `3.如何选择要使用的托管模型？`

`根据应用的功能要求选择 Blazor 托管模型。 下表显示了选择托管模型的主要注意事项。`

`Blazor Hybrid 应用包括 .NET MAUI、WPF 和 Windows 窗体框架应用。`

|  |  |  |  |
| --- | --- | --- | --- |
| 功能 | Blazor Server | Blazor WebAssembly (WASM) | Blazor Hybrid |
| 与 .NET API 完全兼容 | ✔️支持 | ❌❌ | ✔️支持 |
| 直接访问服务器和网络资源 | ✔️支持 | ❌不支持† | ❌不支持† |
| 较小的有效负载，较快的初始加载速度 | ✔️支持 | ❌❌ | ❌❌ |
| 接近本机执行速度 | ✔️支持 | ✔️支持‡ | ✔️支持 |
| 服务器上安全且专用的应用代码 | ✔️支持 | ❌不支持† | ❌不支持† |
| 下载后即可脱机运行应用 | ❌❌ | ✔️支持 | ✔️支持 |
| 静态站点托管 | ❌❌ | ✔️支持 | ❌❌ |
| 将处理过程转移至客户端 | ❌❌ | ✔️支持 | ✔️支持 |
| 对本机客户端功能具有完全访问权限 | ❌❌ | ❌❌ | ✔️支持 |
| 基于 Web 的部署 | ✔️支持 | ✔️支持 | ❌❌ |

`†Blazor WebAssembly 和 Blazor Hybrid 应用可以使用基于服务器的 API 来访问服务器/网络资源并访问专用和安全的应用代码。`

`‡Blazor WebAssembly 仅通过预先 (AOT) 编译达到接近本机性能。`

`总之，Blazor 的三种模式各有特点，可以根据应用场景选择适当的模式。如果需要访问服务器端资源或者需要实现实时通信功能，可以选择 Server 模式；如果需要实现离线访问或者减少网络流量，可以选择 WebAssembly 模式；如果需要兼顾两种模式的优势，可以选择 Hybrid 模式。`

## `4、理解组件化开发思想`

### `1. 组件化核心理念`

#### `什么是组件化？`

`组件化是将UI拆分为独立、可复用、自包含的代码单元的思想。在Blazor中，每个组件都是一个完整的功能模块。`

```
// 传统方式 vs 组件化方式
// 传统：整个页面写在一起
// 组件化：拆分为多个职责单一的小组件
// 传统方式 vs 组件化方式
// 传统：整个页面写在一起
// 组件化：拆分为多个职责单一的小组件
```

### `2. Blazor组件的基本结构`

#### `Razor组件示例`

```
### 计数器组件



当前计数: @currentCount

@code {
    private int currentCount = 0;
    
    [Parameter]
    public int InitialCount { get; set; }
    
    [Parameter]
    public EventCallback OnCountChanged { get; set; }

    private void IncrementCount()
    {
        currentCount++;
        OnCountChanged.InvokeAsync(currentCount);
    }
    
    protected override void OnInitialized()
    {
        currentCount = InitialCount;
    }
}


### 计数器组件



当前计数: @currentCount

@code {
    private int currentCount = 0;
    
    [Parameter]
    public int InitialCount { get; set; }
    
    [Parameter]
    public EventCallback OnCountChanged { get; set; }

    private void IncrementCount()
    {
        currentCount++;
        OnCountChanged.InvokeAsync(currentCount);
    }
    
    protected override void OnInitialized()
    {
        currentCount = InitialCount;
    }
}
```

### `3. 组件化的核心特征`

#### `3.1 封装性`

```
![@UserName](@AvatarUrl)

@code {
    [Parameter] public string UserName { get; set; } = string.Empty;
    [Parameter] public string Email { get; set; } = string.Empty;
    [Parameter] public string AvatarUrl { get; set; } = string.Empty;
    [Parameter] public DateTime JoinDate { get; set; }
}


![@UserName](@AvatarUrl)

@code {
    [Parameter] public string UserName { get; set; } = string.Empty;
    [Parameter] public string Email { get; set; } = string.Empty;
    [Parameter] public string AvatarUrl { get; set; } = string.Empty;
    [Parameter] public DateTime JoinDate { get; set; }
}
```

#### `3.2 可复用性`

```

```

#### `3.3 数据传递`

```
@code {
    private List todos = new();
    
    private void HandleItemAdded(TodoItem newItem)
    {
        todos.Add(newItem);
        StateHasChanged();
    }
    
    private void HandleItemDeleted(TodoItem deletedItem)
    {
        todos.Remove(deletedItem);
        StateHasChanged();
    }
}



@code {
    private List todos = new();
    
    private void HandleItemAdded(TodoItem newItem)
    {
        todos.Add(newItem);
        StateHasChanged();
    }
    
    private void HandleItemDeleted(TodoItem deletedItem)
    {
        todos.Remove(deletedItem);
        StateHasChanged();
    }
}
```

#### `3.4 组件组合`

### `4. 高级组件化模式`

#### `4.1 渲染片段`

```
### @Title



@ChildContent



@FooterContent

@code {
    [Parameter] public string Title { get; set; } = string.Empty;
    [Parameter] public bool IsVisible { get; set; }
    [Parameter] public RenderFragment ChildContent { get; set; }
    [Parameter] public RenderFragment FooterContent { get; set; }
    [Parameter] public EventCallback OnClose { get; set; }

    private void Close() => OnClose.InvokeAsync();
}


### @Title



@ChildContent



@FooterContent

@code {
    [Parameter] public string Title { get; set; } = string.Empty;
    [Parameter] public bool IsVisible { get; set; }
    [Parameter] public RenderFragment ChildContent { get; set; }
    [Parameter] public RenderFragment FooterContent { get; set; }
    [Parameter] public EventCallback OnClose { get; set; }

    private void Close() => OnClose.InvokeAsync();
}
```

#### `4.2 泛型组件`

```
@typeparam TItem

|@foreach (var column in Columns)
                { @column.Title |}
| --- |
@foreach (var item in Items)
            {|@foreach (var column in Columns)
                    { @column.Value(item) |}
}

@code {
    [Parameter] public IEnumerable Items { get; set; } = null!;
    [Parameter] public List> Columns { get; set; } = new();
}

public class ColumnDefinition
{
    public string Title { get; set; } = string.Empty;
    public Func Value { get; set; } = null!;
}

@typeparam TItem

|@foreach (var column in Columns)
                { @column.Title |}
| --- |
@foreach (var item in Items)
            {|@foreach (var column in Columns)
                    { @column.Value(item) |}
}

@code {
    [Parameter] public IEnumerable Items { get; set; } = null!;
    [Parameter] public List> Columns { get; set; } = new();
}

public class ColumnDefinition
{
    public string Title { get; set; } = string.Empty;
    public Func Value { get; set; } = null!;
}
```

## `5、学习 Razor 语法基础`

### `Razor语法简介`

`Razor 是一种轻量级的标记语法，用于将服务器端代码（如 C# 或 VB.NET）嵌入到 HTML 页面中。它广泛用于 ASP.NET Web 应用程序开发，支持动态内容生成。`

### `核心语法规则`

* `Razor 代码块包含在 @{ ... } 中`
* `内联表达式（变量和函数）以 @ 开头，例如 @DateTime.Now`
* `代码语句用分号结束`
* `变量使用 var 关键字声明`
* `字符串用引号括起来`
* `C# 代码区分大小写`
* `C# 文件的扩展名是 .cshtml`

### `使用示例`

#### `基本用法示例`

```
@{
   var UserName = "CSharp精选营";
   var currentTime = DateTime.Now;
}

欢迎来到@UserName 当前时间是: @currentTime

@{
   var UserName = "CSharp精选营";
   var currentTime = DateTime.Now;
}

欢迎来到@UserName 当前时间是: @currentTime
```

`输出: Hello, Razor! 当前时间是: 2023-10-13 10:30:00`

#### `条件判断 @if, else if, else, and @switch`

```
@{
   var hour = DateTime.Now.Hour;
   var message = hour > 12 ? "下午好" : "上午好";
}

@message

@{
   var hour = DateTime.Now.Hour;
   var message = hour > 12 ? "下午好" : "上午好";
}

@message
```

`输出: 根据当前时间显示“上午好”或“下午好”。`

#### `循环结构`

`Razor 支持多种循环，如 for, foreach, while，do while 等。`

```
@for (int i = 1; i <= 5; i++) {* 第 @i 项
}

@for (int i = 1; i <= 5; i++) {* 第 @i 项
}
```

`输出: 生成一个包含 5 项的无序列表。`

#### `动态用户输入处理`

`Razor 可以通过 Request["参数名"] 获取用户输入。`

```
@{
   var name = Request["name"] ?? "游客";
}

欢迎, @name!

@{
   var name = Request["name"] ?? "游客";
}

欢迎, @name!
```

`用户提交表单后，页面会动态显示欢迎信息。`

`更多Razor语法请查看ASP.NET Core 的 Razor 语法👉:`[ASP.NET Core 的 Razor 语法参考 | Microsoft Learn](https://github.com)
