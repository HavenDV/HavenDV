# Using the Visual Designer in a WinUI Application
You can create a separate UWP project and use shared project for .xaml and .cs and some #ifdef. 
Then you will have access to a visual designer, while the application will be built under WindowsAppSDK WinUI3.
You can view the project structure and approaches to organize this here: https://github.com/HavenDV/ratbuddyssey

The first time you use it, you need to select UWP from the dropdown on the top left, then close and reopen the file:  
<img width="184" alt="image" src="https://user-images.githubusercontent.com/3002068/153543386-c816cb56-908b-4144-9b99-5d48cd7af131.png">

### Global usings
With this file, you can set global usings for each platform without the need for manual #ifdef statements in each file. 
Need to add GlobalUsings.cs to shared project like this:
```cs
#if HAS_WINUI
global using Microsoft.UI;
global using Microsoft.UI.Xaml;
global using Microsoft.UI.Xaml.Data;
global using Microsoft.UI.Xaml.Media;
global using Microsoft.UI.Xaml.Controls;
global using LaunchActivatedEventArgs = Microsoft.UI.Xaml.LaunchActivatedEventArgs;
#else
global using Windows.UI;
global using Windows.UI.Xaml;
global using Windows.UI.Xaml.Data;
global using Windows.UI.Xaml.Media;
global using Windows.UI.Xaml.Controls;
#endif
```

In my experience, this causes almost no name collisions. 
In special cases, you should use constructions like:
```cs
global using LaunchActivatedEventArgs = Microsoft.UI.Xaml.LaunchActivatedEventArgs;
```
this will resolve the collision and set the priority

Also you will need to add this to WinUI 3 projects:
```xml
<PropertyGroup>
  <DefineConstants>$(DefineConstants);HAS_WINUI</DefineConstants>
</PropertyGroup>
```

### Community Toolkit Controls
In order to use the Community Toolkit Controls, I create a file with the following content:
```cs
#if !HAS_WINUI
namespace CommunityToolkit.WinUI.UI.Controls;

public partial class Expander : Microsoft.Toolkit.Uwp.UI.Controls.Expander
{
}

public partial class DockPanel : Microsoft.Toolkit.Uwp.UI.Controls.DockPanel
{
}

public partial class DataGrid : Microsoft.Toolkit.Uwp.UI.Controls.DataGrid
{
}

public partial class GridSplitter : Microsoft.Toolkit.Uwp.UI.Controls.GridSplitter
{
}

public partial class HeaderedContentControl : Microsoft.Toolkit.Uwp.UI.Controls.HeaderedContentControl
{
}

public partial class DataGridCheckBoxColumn : Microsoft.Toolkit.Uwp.UI.Controls.DataGridCheckBoxColumn
{
}

public partial class DataGridTextColumn : Microsoft.Toolkit.Uwp.UI.Controls.DataGridTextColumn
{
}

public partial class DataGridTemplateColumn : Microsoft.Toolkit.Uwp.UI.Controls.DataGridTemplateColumn
{
}

public partial class WrapPanel : Microsoft.Toolkit.Uwp.UI.Controls.WrapPanel
{
}

public partial class UniformGrid : Microsoft.Toolkit.Uwp.UI.Controls.UniformGrid
{
}
#endif
```

### Known limitations
You can't use `InfoBadge`: it is in WinUI 2.x and not in WinUI 3

### Example apps
- https://github.com/HavenDV/H.OxyPlot/tree/main/src/apps  
- https://github.com/HavenDV/ratbuddyssey/tree/master/src/apps  
- https://github.com/HavenDV/H.ReactiveUI.CommonInteractions/tree/master/src/apps  
