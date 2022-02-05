# C
You can create a separate UWP project and use shared project for .xaml and .cs and some #ifdef. 
Then you will have access to a visual designer, while the application will be built under WindowsAppSDK WinUI3.
You can view the project structure and approaches to organize this here: https://github.com/HavenDV/ratbuddyssey

With the help of global usings, you can easily make the transition to WinUI 3 and save the UWP project for work in the designer.
It might be worth adding this to the migration documentation.
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

and this to WinUI 3 projects:
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