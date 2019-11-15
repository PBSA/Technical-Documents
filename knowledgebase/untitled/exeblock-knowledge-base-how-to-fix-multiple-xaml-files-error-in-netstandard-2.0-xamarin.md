# EXEBlock Knowledge Base : How to fix multiple XAML files error in NetStandard 2.0 Xamarin

_when you get Duplicate XAML files in a Xamarin NetStandard 2.0 project_

## Step-by-step guide <a id="HowtofixmultipleXAMLfileserrorinNetStandard2.0Xamarin-Step-by-stepguide"></a>

Steps Involved

1. Add the below code under the PropertyGroup tag to the .csproj file:&lt;EnableDefaultEmbeddedResourceItems&gt;false&lt;/EnableDefaultEmbeddedResourceItems&gt; 
2. Add the Filename as EmbeddedResources under the ItemGroup tag as below:&lt;EmbeddedResource Include="Pages\BottomTabBar.xaml"&gt; &lt;Generator&gt;MSBuild:UpdateDesignTimeXAML&lt;/Generator&gt; &lt;/EmbeddedResource&gt;

