
# Alteruna Multiplayer releases

Downloads for [Alteruna Multiplayer](https://www.alteruna.com), the realtime networking solution for games and
interactive applications.

Get the latest version from the **[Releases](https://github.com/Alteruna/multiplayer-releases/releases)** page.

| Platform | Package |
|---|---|
| .NET | [`Alteruna.Multiplayer`](https://www.nuget.org/packages/Alteruna.Multiplayer) on NuGet; the `.nupkg` is also attached to each release |
| C / C++ | `alteruna-multiplayer-cpp-<version>-<platform>.zip` for Windows x64, Linux x64 and macOS (Apple Silicon and Intel) |
| Unity | [multiplayer-sdk-unity-package](https://github.com/Alteruna/multiplayer-sdk-unity-package) |

## C / C++

Each zip contains:

```
alteruna_multiplayer.dll / .so / .dylib   shared library (no .NET runtime required)
alteruna_multiplayer.lib                  import library (Windows)
include/alteruna.h                        C API
include/alteruna.hpp                      header-only C++17 wrapper
cmake/AlterunaConfig.cmake                CMake package
samples/cpp/                              LAN host/client sample
```

Use it from CMake:

```cmake
set(Alteruna_DIR "<extracted zip>/cmake")
find_package(Alteruna REQUIRED)
target_link_libraries(game PRIVATE Alteruna::Multiplayer)

# Copy the shared library next to the executable.
add_custom_command(TARGET game POST_BUILD COMMAND ${CMAKE_COMMAND} -E copy_if_different
    $<TARGET_FILE:Alteruna::Multiplayer> $<TARGET_FILE_DIR:game>)
```

A minimal client:

```cpp
#include <alteruna.hpp>

struct Events : alteruna::Listener {
    void on_room_joined(const alteruna::RoomInfo& room, const alteruna::UserInfo& me) override {}
};

Events events;
alteruna::Service service(&events);
service.verify_project_id("<your project id>");
service.start();
service.open_port();

while (running) {
    service.update();
    std::this_thread::sleep_for(std::chrono::milliseconds(8)); // roughly 120 Hz
}
```

See `samples/cpp` in the zip for synchronization and remote procedure calls.

## Links

* Website: https://www.alteruna.com
* Documentation: https://alteruna.github.io/au-multiplayer-api-docs
* Unity SDK: https://github.com/Alteruna/multiplayer-sdk-unity-package

## License

Use of Alteruna Multiplayer is subject to the [Alteruna Multiplayer SDK EULA](https://www.alteruna.com/alteruna-multiplayer-sdk-eula).