# raylib-carp

WIP [Carp](https://github.com/carp-lang/Carp) bindings for [the raylib gamedev library](https://github.com/raysan5/raylib).

## Current state

These bindings are still a major work in progress. Expect bugs.

There is tentative support for raylib 3.0, but it has not been tested as of yet.

### Things that I'm aware might not work

* The `BoneInfo` struct.
* The `VrDeviceInfo` struct.
* The `TraceLogCallback` typedef.
* The `RAudioBuffer` struct does not have a `copy` function yet.

As of right now, only a few structs have equality functions implemented. More will be added as it becomes apparent that they are needed.

I almost definitely need to re-evaluate how structs with static-length array members are initialized.

Despite these issues, you should already be more than capable of creating 2D games with these bindings.

## Requirements

* [Carp](https://github.com/carp-lang/Carp)
* LLVM
* [raylib](https://github.com/raysan5/raylib)
* (Optional) [raygui](https://github.com/raysan5/raygui)

## Quirks

There are a few things that I've had to do to get this working. They are as follows.

  * I've had to modify Carp's `core.h` to remove Windows headers that contain functions that cause naming collisions with raylib functions.

  The changes I've made are:

  ```
  #define NODRAWTEXT
  #define NOGDI
  #define NOUSER
  #define NOSOUND

  #include <windows.h>
  #undef PlaySound
  ```

  I've provided my `core.h` for convenience. On non-Windows platforms, this does not apply. Keep in mind that I cannot guarantee that this will not cause problems for certain programs.

  * I've also had to create a helper header called `raylib_helper.h`. This contains a few defines and functions that allow the bindings to work properly. Like the Carp side of the bindings, this header is a work in progress.

Additionally, there are a few stylistic quirks that you should be aware of.

  * I made a choice to group functions into modules. If you do not explicitly state `(use module)` under the file import, you will need to prepend the module name to each function.
  * In keeping with Lisp tradition, all functions are `kebab-case`. I'm still trying to decide on a stylistic choice for constants; right now, all of them are `kebab-case` except for raylib's built-in colors, which are simply `lowercase` due to my indecision about whether to call `RAYWHITE` `raywhite` or `ray-white`.

## Other raylib modules

Bindings for raygui are available. If you want to use them standalone, simply add `#define RAYGUI_STANDALONE` to the top of `raygui_helper.h`.

I have not yet created bindings for raymath or rlgl.