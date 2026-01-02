# Build Commands Reference

MSBuild.exe Inventec.Common.sln /p:Configuration=Release /p:Platform=AnyCPU
---

MSBuild.exe Inventec.Desktop.sln /p:Configuration=Release /p:Platform=AnyCPU
---

cd HIS/Plugins/HIS.Desktop.Plugins.Register
MSBuild.exe HIS.Desktop.Plugins.Register.csproj /p:Configuration=Debug /t:Rebuild
---

MSBuild.exe HIS.Desktop.csproj /t:Clean
---

MSBuild.exe HIS/HIS.Desktop.sln /t:Clean
MSBuild.exe MPS/MPS.sln /t:Clean
MSBuild.exe UC/HIS.UC.sln /t:Clean
MSBuild.exe Common/Inventec.Common.sln /t:Clean
---

copy bin\Debug\*.dll ..\..\bin\Debug\Plugins\
```

### Debug Configuration

For development, use Debug configuration for better debugging symbols:

```bash
MSBuild.exe HIS.Desktop.csproj /p:Configuration=Debug /p:Platform=AnyCPU
```

Debug builds include:
- Full PDB symbol files for all assemblies
- No code optimization (easier debugging)
- Debug assertions enabled
- Larger file sizes (~2x Release build)

### Plugin Development Cycle

For new plugin development workflow, see [Plugin System Architecture](../01-architecture/plugin-system.md).

Typical plugin build steps:
1. Create plugin project in `HIS/Plugins/`
2. Implement `IModule` interface
3. Build plugin: [`MSBuild.exe [PluginName].csproj`](../../MSBuild.exe [PluginName].csproj)
4. Copy DLL to `HIS/bin/Debug/Plugins/`
5. Launch `HIS.Desktop.exe` to test

### Clean Build

Remove all build artifacts:

```bash