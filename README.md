# Static libraries from Serious Engine 1 games

This is a collection of static libraries (`.lib`) generated from dynamic link libraries (`.dll`) from different builds of games on Serious Engine 1.

The repository also includes `.def` and `.exports.txt` files containing lists of mangled symbols of exported C++ functions and variables.

*You can check [**this repository**](https://github.com/DreamyCecil/SE1-LibrarySymbolDifferences) to see which functions and variables have been added and exported in each game compared to original Serious Sam engines that these games were based on.*

# Notes

- Most games are built using MSVC 6.0 compiler from **Microsoft Visual C++ 6.0**.
  - Latest **Serious Sam Classics: Revolution** libraries are built using MSVC 12.0 from **Visual Studio 2013**.
  - Some other games might have been built using MSVC 10.0 from **Visual Studio 2010**.
- Engine libraries for **Serious Sam** games (from `TFE100` to `TSE107` under `SeriousSam`) should in theory be completely identical to the libraries included in official SDK packages.
  - There are multiple versions of **Serious Engine 1.00**: `TFE100`, `TFE100a`, `TFE100b` and `TFE100c`. They are all completely identical symbol-wise, though `TFE100` does not have a `CInput::GetAxisTransName()` method.
- **Serious Sam Classics: Revolution** libraries (`SeriousSam/Rev2019`) are generated from the currently latest Steam build, labelled as a 1.03 update.
- **Bird Hunter 2003** (`BirdHunter2003`) and **Deer Hunter 2003** (`DeerHunter2003`) run on Serious Engine 1.07 instead of 1.50, despite the exclusive usage of SKA (skeletal animation) models in those games.
- **Bird Hunter 2003** build includes some extra developer files (`BirdHunter2003/Original`), which were most likely left there by mistake.
  - `GameEXE.map` is particularly interesting because it exposes names of all symbols within the game executable, which would have otherwise been removed because the executable file does not export any symbols.
- **Carnivores: Cityscape** (`Carnivores`) runs on Serious Engine 1.02 but with an interesting twist to it.
  - `LoadAny3DFormat_t` and `LoadModelAnimationData_t` symbols are completely identical to those from Serious Engine 1.04, which could mean that **Carnivores: Cityscape** runs on a hypothetical 1.03 version of the engine. But since the differences are so minimal, it still counts as 1.02.
- Beta 1.50 patch for **Serious Sam** (`SeriousSam/TSE150`) may be the most pure version of Serious Engine 1.50 available to the public.
- Engine from the demo version of **Nitro Family** (`NF_Demo`) may be the closest to the original Serious Engine 1.50 in terms of amount of new functionality in third party games.
- **EuroCops** (`EuroCops`) is based on a slightly newer version of the engine from **Alpha Black Zero** (`AlphaBlackZero`).
- **Rakion** (`Rakion_2019`) libraries are generated from a build available on the official site, which hasn't been kept up-to-date with the Steam version since 2019.
  - It may actually be abandoned altogether in favor of the Steam build. But this site build contains `SeriousEditor.exe`, `SeriousModeler.exe` and `SeriousSkaStudio.exe`, unlike Steam builds, which were presumably left there by mistake, making it a unique build.
- **Last Chaos** libraries (`LastChaos`) are generated from a leaked build from around 2017 with a working Serious Editor. Not particularly useful other than for comparing exported symbols with pure Serious Engine 1.50.
  - It additionally includes static libraries of the "Shaders" project from 2005 (`LastChaos/2005`) that are generated from built libraries found under the "LCadd" project in the leaked 2009 sources. Not very useful.

# How to generate static libraries

1. Open x86 Native Tools Command Prompt from any Visual Studio available to you (recommended **Visual Studio 2010** or newer) as Administrator.
2. Set path to the directory with a dynamic link file (DLL), for example: `cd /d E:\Bin\`.
3. Run this sort of script and wait for it to finish the execution:
```bat
:: Let's assume your library is "Engine.dll"
:: Replace this value with the name of your DLL file; make sure it's all capitalized
set LibName=ENGINE

dumpbin /exports %LibName%.dll > %LibName%.exports.txt
echo LIBRARY %LibName% > %LibName%.def
echo EXPORTS >> %LibName%.def
for /f "skip=19 tokens=4" %A in (%LibName%.exports.txt) do echo %A >> %LibName%.def

lib /def:%LibName%.def /out:%LibName%.lib /machine:x86

:: Include a linebreak at the end here to make it execute the last command as well upon pasting the whole script
```

# License

Since files in this repository have been created from third party libraries using third party tools, it doesn't make sense for them to be licensed under any specific license.

But for the sake of having any license, these files are in the public domain (see `LICENSE`).
