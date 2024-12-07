# Welcome to SFZBuilder Manual

Here you will find all the features SFZBuilder can offer.

## Getting started

1. Download [SFZBuilder](https://github.com/michael02022/sfzbuilder) (can be the compiled binaries or directly from Python source code)
2. [Get the init folder](https://github.com/michael02022/sfzbuilder-init-folder)

Now you can install SFZPacks. A SFZPack is a collection of mappings and presets that SFZBuilder can load and use them.

A good start are:

* [SFZBuilder Factory Library DEMO](https://huggingface.co/datasets/michl1149/SFZBuilder-Factory-Library-Demo/resolve/main/SFZBuilder%20Factory%20Library%20Demo.zip?download=true)
* [gm.dls](https://github.com/sfzbuilder/sfzpack-gm.dls) (need sample extraction)
* [Fairlight IIx](https://github.com/sfzbuilder/sfzpack-Fairlight_IIx)

After you have some stuff to work with, run SFZBuilder.

Now in <kbd>Main Folder</kbd> button, select the init folder you downloaded before and installed SFZPacks and restart the software. Now you're ready.

## Simple introduction to SFZ

SFZ is designed to be human readable enough and easy to edit for end users (no need for anything special other than a text editor). Although the standard is giving a whole Graphical User Interface to end users to design and tweak the settings for their samples.

SFZBuilder let you have the best of both worlds: the flexibility and freedom of a text based format and the convenience of a Graphical User Interface to design sounds.

You have the SFZ file, where the information is stored, and then the SFZ Player, which is the engine that let you hear what you have in a SFZ file.

SFZBuilder covers the two main players: [sfizz](https://github.com/sfztools/sfizz-ui/releases) and [Sforzando](https://www.plogue.com/products/sforzando.html), *all of them free of charge.*

The syntax is very simple, there are two types: the *header* and the *opcode*

The header is the way to assing opcodes, the most basic one being `<region>`.

The name explains itself, is where you set the regions for your samples.

Under any region, you have to write opcodes, the structure of a opcode is `type_of_data=value`

Example:
```
<region> sample=my/path/to/sample.wav lokey=60 hikey=72 lovel=50 hivel=80 // and so on...
```

If you do not write one of the hundred of opcodes, the SFZ player will set the default values of each one automatically for you.

There are other headers than `<region>`, such as `<group>` which essentially is "every opcode I write under this header, will be heredated to the regions"

This is the most basic introduction, for detailed information and the full list of documented opcodes, check the [sfz format website](https://sfzformat.com/)

## SFZ structure used

SFZBuilder has this hierarchy using the available headers of the SFZ format, being:

| header | usage
| -------- | ------- |
|`<global>`| for values that needs to be applied to **all** mappings
|`<master>`| every configuration for each individual mapping
|`<group>`| header to enable FX tricks using a pitch LFO (it duplicates the same mapping but with different configurations)
|`<region>`| the sample and mapping information, being a SFZ file called from `MappingPool` folder


## SFZBuilder interface

### 1. SFZ info

* <kbd>Autoname</kbd>: once pressed, it will extract the name of the current mapping the user has selected in the `List of mappings`, as a shortcut for file names.

* <b>SFZ Name</b>: here you write the name you want for your SFZ preset.

* <b>Author</b>: here you write your name or nickname, you can set a default one where SFZBuilder will load it everytime it runs through the <kbd>Set Default</kbd> button.

### 2. Global

This section is for everything that is set under the `<global>` header.

* <b>Keyswitch</b>: You set the global range of such keyswitchs and the default key for it.
* **Oversampling**: This one is exclusive from Sforzando. It's using the opcode  `hint_min_samplerate`. The purpose is to reduce aliasing for high frequencies.
* **Portamento**: This one uses a template available in *Opcodes* tab. You set the time of such portamento or assign a *Continuous Controller* to set it.

### 3. Mappings
**Type of map selector**: Here you select which kind of map you want to load.

1. **Map**: melodic mappings, it is a collection of regions.
2. **Percussion**: percussive mappings, this one is intended to be customizable the root note of the samples to create your own percussion mappings.
3. **Wavetable**: mappings where it uses a wavetable WAV file or a collection of wavetables inside a SFZ file as regions using the opcode `oscillator_on`. *This feature is implemented correctly only in* **sfizz.**

### 4. Mapping features
1. **Volume**: Set the volume of the mapping.
2. **Mute Map**: Mutes the map. Useful when debugging a preset.
3. **Key**: This one sets a key range of the mapping. It uses the `locc` `hicc` opcode with the extended CC **133**
4. **Vel**: This one sets a velocity range of the mapping. It uses the `locc` `hicc` opcode with the extended CC **131**.
**NOTE:**
This opcode with extended CC (131) doesn't mute the last sample when switching between velocities, this is not helpful for certain contexts or could have polyphony issues.

5. **note on**: This one let you set a range of CC where the regions of the mapping can be played if is under that value range. Normally used to switch between pedal and sustain mappings in pianos.
6. **Program Change**: It let you set a program change to the mapping if enabled. This is useful to create a Soundfont style bank in a SFZ file. There's a box where you have a list of `.ins` files to select with *Cakewalk's Instrument Definition specification* that gives a name for each program change value. The factory ones are **General MIDI** and **Roland MT-32**. **NOTE FOR SFIZZ USERS:** *The program change feature is implemented only in the LV2 version.*

### 5. Mapping List
