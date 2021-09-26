# 2021-09-26 Changes

## Tool Updates
- Fixed number input issue in session files.

- Modified `Exe.FindSpace` function to include the size allocated as the 3rd element in the returned list.

- Added `Exe.Reserve` function for performing allocation + reserve address in DIFF section without needing to set any data.

- Added variants of `Exe.Add*` functions which performs the allocation along with insertion and returns the addr & size list identical to `Exe.FindSpace`.


# 2021-08-31 Changes

## Tool Updates
- Fixed 1 non-critical issue with the GATE dll.


# 2021-08-27 Changes

## Tool Updates
- Added `Warp.BlockMsgs` & `Warp.AllowMsgs` function to allow partial blocking of logging messages.


# 2021-08-26 Changes

## Script Updates
- Added extra functions to `Function`, `Map` & `Set` types.

	| Function           | Description |
	| ----------------   | ----------- |
	| `<map>.open`       | Provides a proxy for destructuring the keys out similar to an `Object` |
	| `<map>.hasValue`   | Check if the map has the specified value (alternative to the `has` function |
	| `<set>.get`        | Retrieve a value using it's index |
	| `<func>.call_self` | Enables call a function with itself as the `this` pointer |

- Added `MakeMap` function to create a `Map` object in a clearer form.

- Added `ReloadPatch` function to clear & load an already selected patch.

- Fixed bug in `OpData` class for 1 of the Scale index forms.

- Fixed bugs in some instruction generators.

## Tool Updates
- Added 2 new options related to errors in [Test Bench].

	- `Show only errors & warnings` => As the name sounds, it allows for skipping 'success' & 'ignored' messages for each individual patch/extension during tests.

	- `Show lines with error` => Option to display the linenumber & filename from where the error originated.

- Modified the way the exe file names are displayed during test => Now the test directory being used is displayed first and only the filenames are reported seperately.


# 2021-08-22 Changes

## Script Updates
- Added `PatchReporter` function which uses the logging mechanism to report the changes staged by the tool.

- Added a new `CACHE` module for storing data persistent per session i.e. It will remain in there as long as you don't load a new client.

  The module also helps with setting up shared data & changes amongst related patches by means of multiple Vaults & a User Registration system.

## Tool Updates
- Added `Exe.HasTag` function to check for existing tags.

- Enabled the use of `this` pointer in all the patch & extension functions. It will point to the main function for each.

- Modified the change reporting mechanism to use a Scripted function called `PatchReporter` instead.

- Now evaluation errors displays the line number & filename the error came from properly.


# 2021-08-20 Changes

## Script Updates
- Shifted `Log` and `OpCodeList` to module files

- Added depth settings to `Log` for hiding unnecessary level of support function logs. All the support functions have been updated.

- Added `getReg` functon to `ModRM` and `SIBase` classes to get their respective member registers quickly.

  It accepts 1 letter as argument ('R' & 'O' for `ModRm` , `I` & `B` for `SIBase`)

- Added `PH_Regs` map to get Placeholder registers quickly based on bit sizes.

- Removed `TAB` constant since it is not needed anymore.


# 2021-08-19 Changes

## Tool Updates
- Changed the root node of **`Patches.yml` & `Extensions.yml`** to a map. No more hyphens needed for Group names and Extension names.

- Added support for including/importing additional YAML files into current one for **`Style` & `Language`** files as well as **`Patches.yml`** by means of the **`include`** key.

  It can be either 1 name of list of names.

  **`Extensions.yml` do not have it yet since it will rarely get big enough to split into multiple files.**

- Session files now also records the selected **`extensions`**, the **`Test Dir`** as well as the selected **`tester exes`** when saving the file in [Test Bench].

  This will help to quickly start the tests when re-launched, instead of selecting everything again from scratch.

- Updated the inbuilt extension for converting NEMO profiles to WARP session files. Inputs are also mapped properly now (aside from **`D_MultiChoice`**).

- Fixed issue with session file generation of **`Target Exe`** when the option is enabled.

- Added support for fully encrypted scripts with the suffix **`.ejs`**. It can also have an optional disclaimer/license header.

  **Do not try to create this manually.**

- Added **`Warp.EncryptFile`** function for generating **`.ejs`** files from the specified source file. Any disclaimer/license header present will be retained as is.

- Added **`Warp.LoadEJS`** function for loading **`.ejs`** files explicitly from scripts or the **`Script Editor`**.

- Added support for module scripts with the suffix **`.mjs`**.

	- Modules are a handy way of encapsulating 'Singleton' objects and as such you are likely to see more use of these in future.

	- The name of the module can be defined inside the **`.mjs`** file by adding the following in a seperate line:

	   `// MODULE_NAME => put_the_name_here`

	- In case this line is not there, then the tool will use the **basename** of the file as the module name.

	- Be aware that modules only get loaded once and the variables exported can only be updated from within the module.

	- Since the loading process is different, module scripts cannot be encrypted into **`.ejs`**.


# 2021-08-18 Changes

## Tool Updates
- Added overload of **`Exe.FindHexN`, `Exe.FindLastHexN`, `Exe.FindTextN` & `Exe.FindLastTextN`** functions to accept a `min`, `max` pair as the count.

  These will return an empty list if the number of elements are < `min` or > `max`.

- Fixed bugs and modified the inner workings of **`D_Color`** type. It only takes the following constraints now:

	- `format` => Previously called `order`. Indicates the format in which the components need to be kept from MSB to LSB.

	- `R`, `G`, `B` & `A` => Optional keys to set the default value for any missing components in the format (need this for the color `ColorPanel` being displayed).

- Similarly the color value can be provided in one of 3 forms:

	- `[component list]` => The components need to follow the `format` specified. i.e. for `RGB`, it should be [r,g,b]

	- `0xnumber` => Same point here the bytes need to follow the `format` specified from MSB to LSB

	- `'#hexcode'` => Standard coloring hex code . The `format` is only used for determining the internal byte representation.

- Changed `align` constraint to `pad` for string types.

- `align` constraint now represents the horizontal alignment of the string values inside the respective textboxes of the **`Input Dialog`**


# 2021-08-17 Changes

## Tool Updates
- Fixed the visual bug with List Panel (used for **`D_Choice & D_MultiChoice`** types) where the frame was coming out and the window just kept enlarging too much.

- Disabled the corner size-grips from going beyond specified `maxWidth` & `maxHeight` if any.

- Added a **"Global"** patch that is now available by default when an exe is loaded.

	- The purpose of this patch is to allow for a common patch to stage changes shared by multiple patches.

  	- Therefore it will not be available for selection in the **`Patch List`**.

- Added **`Exe.ActivateGlobal` & `Exe.ClearGlobal`** functions to work with the aforementioned **"Global"** patch (as counterparts to **SetActivePatch** & **ClearPatch**.

  These also selects & deselects the **"Global"** patch respectively unlike their counterparts.

- Added **`ActivePatch`** property to **`Exe`** object which reflects the name of the currently active patch.

  You can also set it directly instead of using **SetActivePatch** function.

- **`Exe.ClearPatch` & `Exe.SetActivePatch`** can now be invoked without arguments. Behavior for empty argument is as follows:

	- **`Exe.ClearPatch`** => will clear the changes in the active patch.

	- **`Exe.SetActivePatch`** => will keep no patch as active. Same thing happens if you assign **`Exe.ActivePatch`** member directly.

- Added **`Exe.UndoChanges`** function to revert the changes setup for a range of addresses.

- Added **`Exe.FreeUp`** function to revert the changes setup & free up a range of addresses from `DIFF` section.

- Added **`Exe.BeginTag` , `Exe.EndTag` , `Exe.DelTag`** for associating tag names with set of changes & address reservations (in `DIFF` section) to tag names.

	- The first 2 are used to mark the beginning & ending of the tagging process.

	- If **`Exe.BeginTag`** is again invoked with the same name, the previous changes gets wiped (and you can optionally `FreeUp` any reservations too).

	- The 3rd one is for deleting a tag. With this you have the option of either preserving or discarding the staged changes & reservations respectively.


# 2021-08-16 Changes

## Tool Updates
- Added overloads for **`Warp.Define` , `Warp.Encrypt` & `Warp.Execute`** functions to accept list of strings (which gets concatenated internally).

- Added support for links in titles, tooltips & descriptions of **`Patch List & Extension List`** (latter only makes sense while testing though).

- A `linkText` key is now available for specifying color for these link texts (items in the lists) inside a **`Style file`**

- Right clicking on the items in **`Patch List, Extension List & Exe List`** will now copy the details to the clipboard.

- All **`patch`** functions & sub-functions as well as **`extension`** functions get their respective titles as optional 2nd argument now.

- Added **`allowSkip`** key for use in **`Patches.yml` & `Extensions.yml`** to allow skipping of patches & extensions respectively without reporting it as a warning.

  The **`SkippedPatches.log`** file will still have the details.

- Fixed the issue where patches & extensions could not be fully empty.

  Now you can just specify the `<name>` alone without any keys attached to use the defaults for the other details.

- Fixed the issue with `Ctrl+Q` interruption mechanism.

- Fixed the issue with empty string auto-returning false for all the string types in **`Exe.GetUserInput`**

- Modified the usage of **`D_Hex`** type. Now it only has 2 constraints to guide it

	- **`endian`** => Indicates the endianness of the displayed values. This can be either `little` or `big` . Default is `big` .

	- **`byteCount`** => The number of bytes expected to be stored. Default is 1

- Added optional `stepSize0`, `stepSize1`, `stepSize2` & `stepSize3` constraints to use for the respective individual elements of **`D_Vec\*`** types.

- Enabled global `min`, `max` and `stepSize` constraints to **`D_Vec\*`** as well.

  If the individual constraint is not available, the elements will pick up these.

- The escape characters `\n` and `\t` now gets displayed properly in the **`Output`** section. Same goes for blank spaces.

- Added `maxWidth` and `maxHeight` constraints in the **`Input Dialog`** for all types.

- Added **`Warp.clearEditor` & `Warp.clearOutput`** functions for clearing the **`Script Editor` & `Output`** sections respectively from Scripts or the editor itself.

  **Please note that the starting letter is small unlike other functions** (need this way, since these serve as signals)


# 2021-08-15 Changes

## Tool Updates
- Added option in the **`Settings`** dialog to show the modifications setup via **`Exe.Set\*` & `Exe.Add\*`** functions.

- Added **`Exe.ProtectChanges`** function to selectively avoid the above switch when needed. It gets automatically re-enabled when the patch/extension function call is over.

- Added **`Exe.SetFloat` & `Exe.GetFloat`** functions. Also added **`Exe.SetBytes`** function for list of bytes.

- Added **`Exe.Add\*`** variants of all the numeric types (including float) as well as list of bytes.


# 2021-07-25 Changes

## Tool Updates
- Added `findAs` function to `Array` types as an extended version of `find`. The function provided as argument can return the result required instead of `true`.

- Forgot to identify `GetInstr` function earlier. Fixed now.


# 2021-07-24 Changes

## Tool Updates
- Corrected 1 bug in `OpData` class.


# 2021-07-22 Changes

## Tool Updates
- Changed the placeholder functions to use `_` & `_.` instead of `?` to avoid clashing with regular wildcards.

- Removed `SwapFiller` & `SetFillTarget` function (kind of redundant with the other one without much benefits).

- Changed the way the byte count is sent to `SwapFillers` and `SetFillTargets`, now the byte count can be clubbed with the index as a string key => `"index, bc"`

- Also, for `SetFillTargets`, the starting address needs to be provided in the map argument itself using the key `start`. If it's not there then `0` is assumed.

- Converted a lot of `let` to `const`

- Changed some of the `forEach` functions to `for of` loops.

- Changed `LOCK`, `REPE` & `REPN` to `ILOCK`, `IREPE` & `IREPN` respectively. The first 3 are now functions instead to automatically prefix these values.

  `REP` function has also been provided as an alias to `REPE`. Check the wiki for more details.

- Added string instructions to use with the `REP*` functions.


# 2021-07-07 Changes

## Tool Updates
- Added thai language file with translations for the new entries.

- Changed the pattern for 1 byte fillers to use `?.` prefix . The earlier pattern was creating chaos when the bytes are clubbed together.


# 2021-07-06 Changes

## Tool Updates
- Added [Exe.ClearSavedInput](https://github.com/Neo-Mind/WARP/wiki/Exe-Object#patch-related) for clearing existing inputs. Useful in Test Bench.

- Fixed the bug with WARP crashing when running **`Exe.GetUserInput`** from [Script Window](https://github.com/Neo-Mind/WARP/wiki/Script-Window).


# 2021-07-05 Changes

## Tool Updates
- Added few instruction constants.

	- **`FP_START`** = Frame pointer begins (`push ebp` followed by `mov ebp, esp`)

	- **`FP_STOP`**  = Frame pointer ends (`mov esp, ebp` followed by `pop ebp`)

	- **`POP_EAX`**  = Obvious no?

	- **`CDQ`**

	- **`INT3`**

- Changed [Exe.IsSelected](https://github.com/Neo-Mind/WARP/wiki/Exe-Object#patch-related) function to [Warp.GetPatchState](https://github.com/Neo-Mind/WARP/wiki/Warp-Object#functions) for logical reasons.

- Changed [Exe.TestMode](https://github.com/Neo-Mind/WARP/wiki/Exe-Object#properties) to [Warp.TestMode](https://github.com/Neo-Mind/WARP/wiki/Warp-Object#properties) as well.

- Added 2 functions for displaying messages from patch/extension scripts.

	- **`Warp.InformUser`** = Used for information messages

	- **`Warp.WarnUser`**   = Used for warning messages

- Added support for user interrupts with **`Ctrl+Q`** sequence while selecting multiple patches in [Main GUI] and running tests in [Test Bench].

- Added switches for **`RegEx`** & **`Case sensitivity`** in all the filter and search inputs.

- Updated **`Dark_Mode`** style for the new entries in UI (for e.g. the filter/search options).

- Updated the templates for [Language](https://github.com/Neo-Mind/WARP/wiki/Language-File) & [Style](https://github.com/Neo-Mind/WARP/wiki/Style-File) files.


# 2021-07-02 Changes

## Tool Updates
- Removed [NO_ALLOC](https://github.com/Neo-Mind/WARP/wiki/Scripted-API#strings--error-messages) constant since it is no longer needed.

- Changed the pattern generated by [Filler](https://github.com/Neo-Mind/WARP/wiki/Scripted-Functions#filler-functions) function for bc = 1. Now it looks like ?01 and ?121 etc.

- Added the reflection options in [Instr](https://github.com/Neo-Mind/WARP/wiki/Instr) class and [CaseAddr](https://github.com/Neo-Mind/WARP/wiki/Scripted-Functions#extractors) function.

- Added [Exe.GetSavedInput](https://github.com/Neo-Mind/WARP/wiki/Exe-Object#user-input) function to retrieve the value of a previously saved user input (either obtained from `session file` or using [Exe.GetUserInput](https://github.com/Neo-Mind/WARP/wiki/Exe-Object#user-input)

- Added support for encrypted scripting. To achieve this, following 3 functions have been added:

	- **`Warp.Encrypt`** = Converts a script code into it's equivalent encrypted bytes. Output is in hex form.

	- **`Warp.Execute`** = Evaluates the provided encrypted hex in the underlying JS engine and return the result.

	- **`Warp.Define`**  = Execute the provided encrypted hex and assign the result to the specified global variable. Returns false if an error occured or if the result was `undefined`.

- Added **`UserChoice`** function as a quick wrapper for yes/no questions to user. Primarily used in extensions.


# 2021-06-30 Changes

## Tool Updates
- Modified Language **`translations`** to check for **`find`** patterns case-insensitively.

- Fixed bug with filters not looking for translated text. Now they will look for both translated & original texts.

- Fixed bug with **`D_Color`** type when returning default value.


# 2021-06-26 Changes

## Tool Updates
- Added *numeric vector* input [DataTypes](https://github.com/Neo-Mind/WARP/wiki/Inbuilt-API#datatype)

	- **`D_VecI8, D_VecI16, D_VecI32`**

	- **`D_VecI8, D_VecI16, D_VecU32`**

	- **`D_VecF`**

- All the **`D_Vec`** can have upto `4` elements. The size is determined by the default value provided.

- All of them have individual constraints for setting **`min & max`** values as well as specifying a **`name`**.
  For e.g. `index 1` can be setup as `min1: 3, name1: "X Coord"`. If the `name` is not provided then it defaults to `Index1`


# 2021-06-25 Changes

## Tool Updates
- Added `view` button along with `browse` button for [D_InFile & D_OutFile](https://github.com/Neo-Mind/WARP/wiki/Inbuilt-API#datatype) types to open currently specified filename.

- Fixed bug with saving user inputs of [D_Choice & D_MultiChoice](https://github.com/Neo-Mind/WARP/wiki/Inbuilt-API#datatype) types.


# 2021-06-24 Changes

## Tool Updates
- Added reflection support to all the **`Exe.Get`** functions

	- i.e. any existing changes staged by patches can now be `reflected` while retrieving the values.

	- To do this an additional (optional) boolean argument has been added to all of the **`Exe.Get`** functions.

- Renamed **`D_List & D_MultiList`** types to [D_Choice & D_MultiChoice](https://github.com/Neo-Mind/WARP/wiki/Inbuilt-API#datatype) respectively.

	- Also added `Selected value` display and filtering support (similar to `Patch List`) for both of them.


# 2021-06-23 Changes

## Tool Updates
- Added [Warp.SetPatchState](https://github.com/Neo-Mind/WARP/wiki/Warp-Object#functions) function for updating the 'selection' state from script.

- Added dependency chain support (using **`'needs'`** key in **`Patches.yml`**

- Added case-insensitive search option to [Exe.FindText & Exe.FindTextN] functions (only for default encoding i.e. [ASCII](https://github.com/Neo-Mind/WARP/wiki/Inbuilt-API#encoding)).

	- [CASE_SENSITIVE & CASE_INSENSITIVE](https://github.com/Neo-Mind/WARP/wiki/Inbuilt-API#sensitivity) keywords have been added to support this.


# 2021-06-22 Changes

## Tool Updates
- Added **`D_Float`** user input [DataType](https://github.com/Neo-Mind/WARP/wiki/Inbuilt-API#datatype)

- Added **`'stepSize'`** constraint for all numeric inputs.

- Added [System.Trash](https://github.com/Neo-Mind/WARP/wiki/System-Object#modifications) command for moving files to `Recycle Bin`

- Added an optional ***`Build Version`*** display in [Main GUI]

- Added **`Settings`** dialog containing the following options & buttons :

	- [Main GUI]

		- Option to show `Build Version` along with `Build Date`.
		- Option to enable/disable usage of EPI.
		- Option to enable/disable generation of .secure.txt file along with **Target Exe**.
		- Option to enable/disable generation of session files along with **Target Exe**.
		- Option to keep the inputs as-is while loading session files.
		- Button for saving current resolution of **Main & Script** windows as the default.

	- [Test Bench]

		- Option to keep the inputs as-is while loading session files.
		- Option to stop running tests when the first error is encountered.
		- Button for saving current resolution as the default.


# 2021-06-21 Changes

## Tool Updates
- Added **`Settings`** & **`Donate`** buttons to both GUIs

- Modified [Exe.FindSpace](https://github.com/Neo-Mind/WARP/wiki/Exe-Extractors#content-query) function to return **`[PHYSICAL, VIRTUAL]`** address pair

- **`Exe.FindSpace`** also throws an error automatically in case it fails.


# 2021-06-20 Changes

## Tool Updates
- Updated [SwapFiller & SetFillTarget] functions to accept array of strings.


# 2021-06-14 Changes

## Tool Updates
- Fixed 1 bug in **`<number>.toIEEE`** function for conversion of float to IEEE hex string.


# 2021-06-12 Changes

## Tool Updates
- Updated signature of [SwapFiller & SetFillTarget] functions to accept index & bytecount together as a tuple (2 element array).


# 2021-06-05 Changes

## Tool Updates
- Added link to Changelog in [README](README.md).

[Main GUI](https://github.com/Neo-Mind/WARP/wiki/Main-GUI)
[Test Bench](https://github.com/Neo-Mind/WARP/wiki/Test-Bench)
[SwapFiller & SetFillTarget](https://github.com/Neo-Mind/WARP/wiki/Scripted-Functions#filler-functions)
