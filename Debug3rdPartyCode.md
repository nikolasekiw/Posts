# Debug 3rd party code

## Windows + Visual Studio
### Clone the source code (windows)
1. Clone the repository of the third-party library.
2. Add the library’s .csproj file to your solution.
3. Replace the PackageReference in your .csproj with a ProjectReference to the cloned .csproj.
4. Rebuild your solution.
5. You can now set breakpoints and debug the library as if it were your own code. This gives full access to the source and allows deep debugging.

### Use a Symbol Package
This method works if the library provides symbol packages and supports Source Link.
1. In Visual Studio (not directly applicable in VS Code, but useful if switching):
   - Go to Tools > Options > Debugging.
   - Uncheck "Enable Just My Code".
   - Go to Debugging > Symbols and configure symbol paths.
2. Set a breakpoint in the external code.
3. Use Debug > Windows > Modules to verify that symbols are loaded.
4. Once symbols are loaded, the breakpoint will trigger. This allows stepping into external code without needing the full source.

#### Include Symbols in Your Own Code
If you're publishing your own libraries:
- Publish .snupkg files to a symbol server.
- Embed PDBs directly into the DLL for easier debugging.

Benefits:
- Consumers of your library can debug it more easily.
- Improves transparency and supportability.

## MacOS + Visual Studio Code
### Enable stepping into external code
- To allow debugging into third-party libraries, configure launch.json.
- In your .vscode/launch.json, add or modify:
```json
{
  "justMyCode": false;
}
```
This tells the debugger to allow stepping into non-user code.

### Use Source Link (if supported by library)
- Many modern NuGet packages include Source Link metadata, which allows the debugger to download and step into the original source code.
- Requirements:
-   .NET SDK 5.0 or later
-   Debug build (dotnet build --configuration Debug)
-   The NuGet package must support Source Link
If supported, VS Code will automatically fetch the source files during debugging.

### Reference Local Source Instead of NuGet
If Source Link isn’t available or you want full control:
- Clone the third-party library’s repository.
- Add the .csproj file to your solution.
- In your project’s .csproj, replace:
```xml
<PackageReference Include="LibraryName" Version="x.x.x" />
```
with:
```xml
<ProjectReference Include="../path-to-cloned-library/LibraryName.csproj" />
```
Rebuild solution:
```shell
dotnet build --configuration Debug
```
You can now set breakpoints and debug the library.

## External tools
- ILspy
- dotPeek

## Best practices
- Prefer Source Link if available.
- For full control, clone the source and use ProjectReference.
- Use justMyCode: false to enable stepping into third-party code.
- Use external tools like ILSpy or dotPeek for inspection.
