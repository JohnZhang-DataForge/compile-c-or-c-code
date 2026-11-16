# Compile C / C++ to a Windows EXE

Upload your project files into this repository, then open **Actions → Build Windows EXE → Run workflow**. This uses a GitHub-hosted `windows-2022` runner with Visual Studio 2022, CMake, and MSVC. No self-hosted runner or credentials are needed.

## Upload and build

1. On the repository Code page, choose **Add file → Upload files**. Upload your `.c`, `.cpp`, `.h`, and `.hpp` files, preserving folders. Commit the upload. For large projects, use Git to push the files instead.
2. Open **Actions → Build Windows EXE → Run workflow**.
3. Set **Source directory** to your project folder (`src` for the included example, or `.` for repository root).
4. Choose `auto` for a simple project, or `cmake` if your source folder contains its own `CMakeLists.txt`.
5. Enter an executable name without `.exe` in auto mode. Choose Release or Debug, then run.
6. Open the completed run and download **windows-exe** under **Artifacts**. Extract the ZIP to get the `.exe` and any DLLs produced in the build tree.

## Modes

- **auto** recursively compiles every `.c`, `.cc`, `.cpp`, and `.cxx` file in the selected source directory into one executable. It adds the source root and directories containing headers as include paths. Uses C17 / C++17 and the static MSVC runtime. Your selected folder must contain exactly one program entry point (`main`). Remove the example if you add your own program in the same source folder.
- **cmake** uses your existing CMake project. Leave **CMake target** empty to build all default targets, or enter one executable target. Configure dependencies, include paths, language standards, libraries, resources, and Windows GUI entry points in your CMakeLists.txt. This mode is appropriate for complex projects; dependencies must be made available by your project.

The workflow compiles uploaded code; it does not run the resulting program. Build failures appear in the Actions logs. Automatic builds exclude common build, dependency, and Git directories. Use CMake mode when you need more control. Do not upload passwords or private credentials with source files.

The initial repository setup also runs the included two-file C example once. Subsequent builds are started manually.
