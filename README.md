# Compile C / C++ Projects to a Windows EXE

Build a C/C++ project on a GitHub-hosted `windows-2022` runner using Visual Studio 2022, MSVC, and CMake. The workflow supports ZIP uploads and the original repository-folder build option.

## ZIP workflow (recommended)

1. Open this repository's **Code** page.
2. Choose **Add file → Upload files** and upload your project ZIP, for example `uploads/project.zip`. Commit the ZIP to the repository. Do not extract it first.
3. Open **Actions → Build Windows EXE → Run workflow**.
4. In **ZIP path**, enter the committed repository path, such as `uploads/project.zip`.
5. If the ZIP contains several projects or the CMake project is nested, enter its relative path in **Project subdirectory**. Otherwise leave it empty. A ZIP containing one enclosing folder is detected automatically.
6. Leave **Build mode** as `auto` to use the root `CMakeLists.txt` when present, or fall back to simple recursive C/C++ compilation. Choose `cmake` to require CMake, or `simple` to force the simple builder.
7. Select Release or Debug. For CMake projects, optionally specify a **CMake target**; leave it empty to build the project's default targets.
8. Click **Run workflow**. When the run finishes, open it and download **windows-exe** from the **Artifacts** section. Extract the artifact ZIP to get the compiled `.exe` and build-produced `.dll` files.

## Existing folder-based build

The original folder option is preserved. Leave **ZIP path** empty, set **Source directory** to a repository folder (for example `src`), and run the workflow. The same build-mode options apply.

## Build modes and project requirements

- **auto**: If the selected project root contains `CMakeLists.txt`, configure and build it as a CMake project. Otherwise recursively compile `.c`, `.cc`, `.cpp`, and `.cxx` files into one executable.
- **cmake**: Requires `CMakeLists.txt` in the selected project root. Use this for real multi-target projects, libraries, generated code, resources, Windows subsystem settings, and projects with external dependencies. Set the optional CMake target only when you need a particular target.
- **simple**: Uses the repository's generic CMake wrapper to compile source files into one executable. This is intended for small projects with a single program entry point, not complex engines or projects that need custom link flags or third-party libraries.

The runner checks ZIP entry paths before extraction and rejects absolute paths, parent-directory traversal, and symbolic links. Extraction takes place under the runner's temporary directory. Git submodules referenced by the checked-out repository are initialized, but a ZIP itself does not carry Git submodule metadata: include required dependency source in the ZIP or arrange for the project's CMake build to fetch it. Network access, licenses, SDKs, game data, generated assets, and other project-specific prerequisites may still be required.

The workflow compiles but does not execute the resulting program. It collects executables and DLLs produced in the CMake build tree; it does not automatically discover arbitrary runtime assets or DLLs installed elsewhere on the runner. Review the build logs for missing dependencies and copy any required runtime data into the project build/install rules.

Do not include passwords, private keys, access tokens, or other secrets in uploaded ZIP files.
