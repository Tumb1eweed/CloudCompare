# AGENTS.md

## Project Context

- Repository: `CloudCompare`
- Verified on: Windows 11 x64
- Verified branch for local setup: `version_2.12.4`
- Preferred local working branch for custom changes: `2.12.4_cr`

## Verified Toolchain

- Qt: `D:\qt\5.15.2\msvc2019_64`
- Qt Creator: `D:\qt\Tools\QtCreator\bin\qtcreator.exe`
- CMake: `C:\Program Files\CMake\bin\cmake.exe`
- Ninja: `D:\qt\Tools\Ninja\ninja.exe`
- MSVC Build Tools: `C:\BuildTools`
- MSVC environment script: `C:\BuildTools\VC\Auxiliary\Build\vcvars64.bat`

## Build Notes

- CloudCompare 2.11+ requires Qt 5 with `5.9 <= Qt < 6.0`.
- A clean out-of-source build is required.
- On this machine, newer CMake 4.x requires:
  - `-DCMAKE_POLICY_VERSION_MINIMUM=3.5`
  This is needed because some bundled plugin `CMakeLists.txt` files still declare compatibility with older CMake versions.

## Verified Release Configure / Build / Install

Run from `cmd.exe`:

```bat
call C:\BuildTools\VC\Auxiliary\Build\vcvars64.bat

"C:\Program Files\CMake\bin\cmake.exe" ^
  -S D:\CloudCompare ^
  -B D:\CloudCompare\build-release ^
  -G Ninja ^
  -DCMAKE_BUILD_TYPE=Release ^
  -DCMAKE_PREFIX_PATH=D:\qt\5.15.2\msvc2019_64 ^
  -DCMAKE_INSTALL_PREFIX=D:\CloudCompare\install-release ^
  -DCMAKE_MAKE_PROGRAM=D:\qt\Tools\Ninja\ninja.exe ^
  -DCMAKE_POLICY_VERSION_MINIMUM=3.5

"C:\Program Files\CMake\bin\cmake.exe" --build D:\CloudCompare\build-release --parallel
"C:\Program Files\CMake\bin\cmake.exe" --install D:\CloudCompare\build-release
```

Main executable after install:

- `D:\CloudCompare\install-release\CloudCompare\CloudCompare.exe`

## Verified Optional Plugin Builds

### GL plugin: qEDL

Enable at configure time with:

```bat
-DPLUGIN_GL_QEDL=ON
```

Installed files:

- `D:\CloudCompare\install-release\CloudCompare\plugins\QEDL_GL_PLUGIN.dll`
- `D:\CloudCompare\install-release\CloudCompare\shaders\EDL\*`

Important:

- `qEDL` is a `GL` plugin.
- It does **not** appear in the top-level `Plugins` menu.
- Access it from `Display > Shaders & Filters`.

### Standard plugin: qHPR

Enable at configure time with:

```bat
-DPLUGIN_STANDARD_QHPR=ON
```

Installed file:

- `D:\CloudCompare\install-release\CloudCompare\plugins\QHPR_PLUGIN.dll`

Important:

- `qHPR` is a `Standard` plugin.
- It appears in the top-level `Plugins` menu as `Hidden Point Removal`.
- Its action is only enabled when exactly one point cloud is selected.

## Plugin Verification Tips

- If a plugin DLL exists under `install-release\CloudCompare\plugins`, restart CloudCompare after installing it.
- `Plugins` menu:
  - Only `Standard` plugins appear here.
- `Display > Shaders & Filters`:
  - `GL` plugins such as `qEDL` appear here.
- Some plugin actions stay disabled until the correct entity type is selected in the DB tree.

## Local Build Artifacts

These local directories are expected during development and should not be committed unless explicitly requested:

- `build-release/`
- `install-release/`
- `install/`

## Git Notes

- The upstream `origin` remote may still point to the main CloudCompare repository.
- For pushing personal changes, HTTPS worked with:
  - `https://github.com/Tumb1eweed/CloudCompare.git`
