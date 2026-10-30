> [!WARNING]
> **This package is obsolete and this repository is archived.**
>
> Please migrate to **[QuickLook.MediaInfo](https://www.nuget.org/packages/QuickLook.MediaInfo)** instead.
>
> **Why archived:** `MediaInfoDLL` is no longer maintained. Ongoing MediaInfo .NET packaging and updates continue under `QuickLook.MediaInfo`. New projects should depend on that package; existing consumers should migrate when convenient.

[![NuGet](https://img.shields.io/nuget/v/QuickLook.MediaInfo.svg)](https://www.nuget.org/packages/QuickLook.MediaInfo) [![Platform](https://img.shields.io/badge/platform-Windows-blue?logo=windowsxp&color=1E9BFA)](https://dotnet.microsoft.com/en-us/download/dotnet/latest/runtime)

# MediaInfoDLL (obsolete)

Minimal MediaInfo .NET APIs wrapped from [MediaArea.net](https://mediaarea.net/en/MediaInfo/Download/Windows).

> Historical note: this package previously supported Windows Vista, 7, 8, 10, 11 with x86, x64, and ARM64. Prefer [QuickLook.MediaInfo](https://www.nuget.org/packages/QuickLook.MediaInfo) for current usage.

## Usage (legacy)

1. Provide the Media Inform.

```c#
using MediaInfoLib;

using MediaInfo lib = new();
lib.Open(@"C:\media.mp4");
Console.WriteLine(lib.Inform());
```

Useful options.

```c#
using MediaInfoLib;

using MediaInfo lib = new();
lib.Open(@"C:\media.mp4");
lib.Option("Complete", "1"); // Set complete output.
lib.Option("Inform", Inform_Format.HTML.ToString()); // Set format to HTML.
lib.Option("Language", Language_ISO639.ChineseSimplified.ToIso639()); // Set language to Chinese.
Console.WriteLine(lib.Inform());
```

2. Check audio track exists.

```C#
using MediaInfoLib;

using MediaInfo lib = new();
bool hasAudio = lib.WithOpen(fileName).Count_Get(StreamKind.Audio) > 0;
Console.WriteLine(hasAudio);
```

3. Get audio track parameter.

```C#
using MediaInfoLib;

using MediaInfo lib = new();
lib.Open(fileName);
_ = double.TryParse(lib.Get(StreamKind.Audio, 0, "BitRate"), out double bitRate);
Console.WriteLine(bitRate);
```

## Examples

1. [ConsoleDemo](https://github.com/lemutec/MediaInfoNet/tree/main/demo/ConsoleDemo) for Console Application.
2. [WinFormsDemo](https://github.com/lemutec/MediaInfoNet/tree/main/demo/WinFormsDemo) for [WinForms](https://github.com/dotnet/winforms) Application.
3. [VSEnc](https://github.com/lemutec/VSEnc) for [WPF](https://github.com/dotnet/wpf) Application.
4. [QuickLook](https://github.com/QL-Win/QuickLook) for [WPF](https://github.com/dotnet/wpf) Application.
5. [LyricStudio](https://github.com/lemutec/LyricStudio) for [Avalonia](https://github.com/AvaloniaUI/Avalonia) Application.

## Runtimes (legacy)

How to include the all `MediaInfo.dll` runtime native libraries in `.csproj`:

```xml
<ItemGroup>
    <Content Include="$(NuGetPackageRoot)\MediaInfoDLL\25.7.0\lib\netstandard2.0\x64\MediaInfo.dll">
        <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
        <DestinationFolder>$(OutDir)MediaInfo-x64\</DestinationFolder>
        <Link>runtime-x64\MediaInfo.dll</Link>
    </Content>
    <Content Include="$(NuGetPackageRoot)\MediaInfoDLL\25.7.0\lib\netstandard2.0\x86\MediaInfo.dll">
        <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
        <DestinationFolder>$(OutDir)MediaInfo-x86\</DestinationFolder>
        <Link>runtime-x86\MediaInfo.dll</Link>
    </Content>
    <Content Include="$(NuGetPackageRoot)\MediaInfoDLL\25.7.0\lib\netstandard2.0\arm64\MediaInfo.dll">
        <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
        <DestinationFolder>$(OutDir)MediaInfo-ARM64\</DestinationFolder>
        <Link>runtime-arm64\MediaInfo.dll</Link>
    </Content>
</ItemGroup>
```

and remove the auto copying library:

```xml
<Target Name="ReduceReleasePackaging" AfterTargets="Build">
    <!-- MediaInfoDLL will copy the MediaInfo.dll file according to the architecture, we do not use this usage so delete it manually -->
    <Delete Files="$(OutputPath)\MediaInfo.dll" Condition="Exists('$(OutputPath)\MediaInfo.dll')" />
</Target>
```

## References

https://github.com/MediaArea/MediaInfoLib/blob/master/Source/MediaInfo/MediaInfo_Config.h

https://github.com/MediaArea/MediaInfoLib/blob/master/Source/MediaInfo/File__Analyse_Automatic.h

https://github.com/MediaArea/MediaInfo/tree/master/Source/Resource/Plugin/Language

https://github.com/MediaArea/MediaInfo/blob/master/Source/Resource/Language.csv

## License

MediaInfoLib - https://github.com/MediaArea/MediaInfoLib

Copyright (c) MediaArea.net SARL. All Rights Reserved.

This program is freeware under BSD-2-Clause license conditions.

See License.html for more information
