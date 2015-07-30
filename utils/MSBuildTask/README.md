# DiscUtils.MSBuildTask

## Summary
When build your project after installed this package, you can see .iso image file in output folder that contains all of output files.

## How to install

```
PM> Install-Package DiscUtils.MSBuildTask
```

## Default configuration

- The file name of .iso file is project output name.
- Volume label of the .iso image is empty.
- The .iso image include all files in output folder, except `*.iso` files.
- The .iso image spec enabled "Juliet".

## How to configure creating .iso file

If you would like to customize default behaviors of creating .iso file, you should edit project file by any text editor directly, not via projet property settings GUI on Visual Studio.

After installed this package, you will see like following block at last in .csproj file.

```xml
<!-- To modify your build process, add your task inside one of the targets below and uncomment it.
     Other similar extension points exist, see Microsoft.Common.targets.
<Target Name="BeforeBuild">
</Target>
<Target Name="AfterBuild">
</Target>
-->
<Import Project="..\packages\DiscUtils.MSBuildTask.0.11.0.0\build\DiscUtils.MSBuildTask.targets" Condition="Exists('..\packages\DiscUtils.MSBuildTask.0.11.0.0\build\DiscUtils.MSBuildTask.targets')" />
<Target Name="EnsureNuGetPackageBuildImports" BeforeTargets="PrepareForBuild">
  <PropertyGroup>
    <ErrorText>This project references NuGet package(s) that are missing on this computer. Use NuGet Package Restore to download them.  For more information, see http://go.microsoft.com/fwlink/?LinkID=322105. The missing file is {0}.</ErrorText>
  </PropertyGroup>
  <Error Condition="!Exists('..\packages\DiscUtils.MSBuildTask.0.11.0.0\build\DiscUtils.MSBuildTask.targets')" Text="$([System.String]::Format('$(ErrorText)', '..\packages\DiscUtils.MSBuildTask.0.11.0.0\build\DiscUtils.MSBuildTask.targets'))" />
</Target>
</Project>
```

To customization, you should insert `<PropertyGroup>` node with some properties that describe this section, before `<Import Project="..\**\DiscUtils.MSBuildTask.targets" .../>` node position.

The properties to customize behaviors of creating .iso file are here.

**IsoFileName**  
The full path of .iso image file name.

**IsoVolumeLabel**  
The volume label of .iso image.

**IsoUseJoliet**  
Specification of using "Juliet" or not (describe `true` or `false`).

**DisableDefaultIsoFiles**  
If this property is defined with any text value, the default definition of files that including .iso image does not defined.

You should defined `<ItemGroup>` node that has `<IsoFiles>` node instead of default settings.  
(`IsoFiles` items are including .iso image file.)

**DisableDefaultBuildIsoTarget**  
If this property is defined with any text value, the default build task that creating .iso image file is disabled.

### Example of customization

```xml
...
<PropertyGroup>
  <IsoFileName>$(OutputPath)myCustomFileName.iso</IsoFileName>
  <IsoVolumeLabel>myCustomVolume</IsoVolumeLabel>
  <IsoUseJoliet>false</IsoUseJoliet>
  <DisableDefaultIsoFiles>true</DisableDefaultIsoFiles>
</PropertyGroup>
<ItemGroup>
  <!-- include .exe file only! -->
  <IsoFile Include="$(OutputPath)**\*.exe" Exclude="**\*.iso" />
</ItemGroup>
<Import Project="..\packages\DiscUtils.MSBuildTask.0.11.0.0\build\DiscUtils.MSBuildTask.targets" Condition="Exists('..\packages\DiscUtils.MSBuildTask.0.11.0.0\build\DiscUtils.MSBuildTask.targets')" />
...
```
