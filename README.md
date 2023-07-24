# Animation Speed Manager

Can be used to easily change the animation speed of animations in Valheim. Compatible with other mods that use this library.

### Merging the DLL into your mod

Download the AnimationSpeedManager.dll from the release section to the right.
Including the DLL is best done via ILRepack (https://github.com/ravibpatel/ILRepack.Lib.MSBuild.Task). You can load this package (ILRepack.Lib.MSBuild.Task) from NuGet.

If you have installed ILRepack via NuGet, simply create a file named `ILRepack.targets` in your project and copy the following content into the file

```xml
<?xml version="1.0" encoding="utf-8"?>
<Project xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
    <Target Name="ILRepacker" AfterTargets="Build">
        <ItemGroup>
            <InputAssemblies Include="$(TargetPath)" />
            <InputAssemblies Include="$(OutputPath)\AnimationSpeedManager.dll" />
        </ItemGroup>
        <ILRepack Parallel="true" DebugInfo="true" Internalize="true" InputAssemblies="@(InputAssemblies)" OutputFile="$(TargetPath)" TargetKind="SameAsPrimaryAssembly" LibraryPath="$(OutputPath)" />
    </Target>
</Project>
```

Make sure to set the AnimationSpeedManager.dll in your project to "Copy to output directory" in the properties of the DLLs and to add a reference to it.

## Example project

This increases the attack speed by 100%.

```csharp

namespace MyAmazingMod
{
	[BepInPlugin(ModGUID, ModName, ModVersion)]
	public class MyAmazingMod : BaseUnityPlugin
	{
		private const string ModName = "MyAmazingMod";
		private const string ModVersion = "1.0.0";
		private const string ModGUID = "org.bepinex.plugins.myamazingmod";
		
		public void Awake()
		{
			AnimationSpeedManager.Add((character, speed) =>
			{
				if (character is not Player player || !player.InAttack() || player.m_currentAttack is null)
				{
					return speed;
				}

				return speed * 2;
			});
		}
	}
}
```
