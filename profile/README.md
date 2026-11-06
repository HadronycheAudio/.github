# Hadronyche

Hadronyche is where I bring my creative tools together: a small, growing set of audio plug-ins that I use in my own music and sound design, and that work as one workflow.

I design each tool's internal architecture, from its signal path and what happens inside its feedback loops to how it behaves in a saved project. Each plug-in does one idea thoroughly, and sounds that way because of how it's built.

## The plug-ins

All of them are free VST® 3 plug-ins for 64-bit Windows 10 or later, under the MIT licence.

| Plug-in | Type | What it does |
| --- | --- | --- |
| [**RabidPhatt**](https://github.com/HadronycheAudio/RabidPhatt) | Instrument | A monophonic synth with 66 sawtooth oscillators that a single Motion control pulls apart and brings back together across the stereo field. |
| [**PartialResynth**](https://github.com/HadronycheAudio/PartialResynth) | Effect | Listens to its input, finds the five loudest partials and plays each with its own synth voice. The analysis is a port of `fiddle~` from Pure Data. |
| [**DiffusionDelay**](https://github.com/HadronycheAudio/DiffusionDelay) | Effect | An echo with a lossless diffusion network inside its feedback loop. At 100% feedback, the repeats never end and dissolve into a mass of sound. |
| [**LoopTexturer**](https://github.com/HadronycheAudio/LoopTexturer) | Effect | An effects chain plus a tape-style looper. Drag the loop onto the timeline to get it as a WAV file, as you hear it. |

## How they fit together

- **Generate:** RabidPhatt makes moving, unstable material from a single note.
- **Transform:** PartialResynth rebuilds any sound from its partials.
- **Dissolve:** DiffusionDelay turns repeats into texture.
- **Capture:** LoopTexturer records and degrades loops, then sends the results back to the timeline as audio.

## How they're built

- **Design first:** each plug-in has a behaviour specification, `docs/signal-path.md`, that marks which behaviour is a deliberate design requirement.
- **No framework:** they're written in C++17 directly on the Steinberg VST 3 SDK and VSTGUI, without JUCE.
- **Safe for real projects:** saved projects keep loading and sounding the same across versions, and compatibility tests guard the parameters and saved format.
- **Tested like a host would use them:** the tests load the built plug-in and drive it through its VST 3 interfaces, including clicking on its real panel. They measure the output rather than trusting the code.
- **Safe on the audio thread:** the audio code doesn't allocate, lock or block.
- **Checked before release:** CI builds every push, runs the Steinberg validator and all the tests, then installs the installer and tests the installed copy.

Shared code is now moving into a common library, HadronycheCore, so that a fix lands once and new tools start from less boilerplate. A null test compares each migrated plug-in with its previous build, so users get exactly the same sound and saved projects.

## Get them

Each repository has an installer under its latest release, along with usage notes and build instructions. The installers aren't code-signed, so Windows will probably warn you: choose **More info**, then **Run anyway**.

So far, they've only been built and used on Windows, and most of the real-world use has been in Cubase.

<img src="https://raw.githubusercontent.com/HadronycheAudio/DiffusionDelay/main/docs/images/VST_Compatible_Logo_Steinberg_with_TM.png" alt="VST Compatible" width="110">

VST is a registered trademark of Steinberg Media Technologies GmbH.

To show this on the org page, put it in profile/README.md in a public repo named .github under the organization.

Check these before you publish:

- Workflow order: the Generate, Transform, Dissolve and Capture sequence is my own framing, based on what each plug-in does. If you chain them differently, change that section to match.
- "Use in my own music and sound design": I inferred this from your request and the repos' notes about Cubase testing, so adjust it if it's not quite right.
- HadronycheCore: I named it without a link because the GitHub repo is still empty. The null test comparison has also only been run on DiffusionDelay so far, though the sentence describes the method in general. Add the link once the library is pushed.
