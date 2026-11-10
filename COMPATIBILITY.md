# Model formats, Minecraft versions, and server installation

This repository contains model assets. A compatible YSM implementation is also
required for the Minecraft version and mod loader you use. Changing a model's
filename or a mod's version constraint does not port the mod to a new game version.

## Archive layouts

Keep each `.ysm` file intact. ZIP models may have either the model files at the
archive root or a single enclosing model directory. Some older ZIP archives also
use legacy Chinese filename encoding. A reader that assumes UTF-8 names or only
accepts model files at the archive root can reject an otherwise valid model.

If an archive fails to load, include its repository path and the mod, loader, and
Minecraft versions in your report. Preserve the original archive for diagnosis.
Do not assume that every other model in the same category is broken.

The ZIP downloaded using GitHub's **Download ZIP** contains this entire repository;
it is not itself an individual YSM model. Extract the repository archive and use
the individual model files and category directories inside it.

## Installation and multiplayer

Follow the installation instructions of your chosen YSM version. Implementations
using `config/yes_steve_model/custom/` expect individual supported model files or
model directories there, rather than the repository's enclosing download folder.

For a dedicated server, the server must support that same Minecraft version and
have a server-compatible implementation of the mod. Every player also needs the
compatible client mod. Whether models are downloaded automatically from the server
depends on that implementation and its configuration; copying models to the server
alone does not add model rendering to an unmodified client.

## Experimental Minecraft 26.1.x, 26.2 and 26.3 verification

These are observations from an experimental local Fabric port, **not an official
YSM release or a compatibility guarantee for other implementations**. No mod
binary or mod source is distributed by this documentation change.

Repository revision tested: `1749b46467acea11694891c01ede24914b22dc6c`.

| Check | Result |
| --- | --- |
| Model files parsed | 1,114: 906 `.ysm` files and 208 ZIP models |
| `.ysm` container versions represented | v1, v2, and v3 |
| ZIP models with a single enclosing directory | 6 |
| Original asset contents | Unchanged; no model conversion was needed |
| Dedicated server | Minecraft 26.1, 26.1.1, 26.1.2, 26.2 and 26.3; Fabric Loader 0.20.1, Java 25, Windows |
| Multiplayer test | Two independent clients connected to a separate loopback server |
| Models advertised in the full server test | 1,146 including 27 built-in models and 5 extra regression fixtures |
| Client model installation | No custom models preinstalled; server download and cache exercised |

The port needed archive-reader, rendering, and synchronization changes. The asset
files themselves did not need to be rewritten for Minecraft 26.1.x, 26.2 or 26.3. Parsing the
entire catalog does **not** mean that every model and animation was visually
inspected. In 26.2, detailed animation checks used Ame and KAngel fixtures: movement,
crouching, jumping, swinging/mining, item use, riding, sleeping, swimming, flight,
and eight extra animations. Remote model changes, alternate textures, late joins,
and returning to another player's tracking range were also exercised in 26.2.
The 26.3 checks repeated ordinary movement, hand swings/mining, bow/shield use,
boat riding, swimming, creative flight and extra0–extra7, plus remote textures,
late join, tracking-range return, and cached resync. Sleep/elytra and every
possible model animation have not been revalidated in 26.3.

The 26.1 family was tested with separate production JARs and separate dedicated
servers for **26.1, 26.1.1 and 26.1.2**, each with two independent clients. Checks
covered the full catalog from empty client caches, fresh-process cache loading,
remote movement/swings, bow/shield use, extra0-extra7, client-originated model
selection, alternate textures, GUI previews/search, tracking-range return and
responsive server reloads. Other animation categories were not all revalidated.
The final clients and servers exited cleanly.

The 26.1.x mod contribution is
[proposed separately](https://github.com/qbb95/OpenYSM-Updated/pull/2).
Its three **unofficial alpha test JARs** are available in
[the mod fork's 26.1.x Release](https://github.com/IsSlashy/OpenYSM-Updated/releases/tag/fabric-26.1.x-alpha.1).
Choose the file for the exact game version; these are not official YSM releases.

The 26.2 and 26.3 mod proposals are tracked separately:
[26.2 draft PR](https://github.com/qbb95/OpenYSM-Updated/pull/1) and
[26.3 draft PR](https://github.com/qbb95/OpenYSM-Updated/pull/3).
Their **unofficial alpha test JARs** are now available in the
[26.2 Release](https://github.com/IsSlashy/OpenYSM-Updated/releases/tag/fabric-26.2-alpha.4)
and [26.3 Release](https://github.com/IsSlashy/OpenYSM-Updated/releases/tag/fabric-26.3-alpha.2).
All three version-family Releases include English, French, Simplified Chinese
and Japanese notes and installation instructions.

The final 26.2/26.3 Windows tests repeated complete empty-cache loading and
fresh-process reconnects, with two independent clients and clean exits. The
verified HotSpot client configuration uses `-XX:StackShadowPages=32`: 26.3's
Mojang launch metadata already includes it, and custom launchers must preserve
it. The 26.2 test configuration adds the same margin explicitly. A prior native
JVM failure was reproduced without YSM before correcting the test launcher's
omission; see the mod's
[launcher diagnosis](https://github.com/IsSlashy/OpenYSM-Updated/blob/codex/fabric-26.3-port/verification/LAUNCHING.md).
This does not change these model files or require a model conversion.
Unofficial version-specific test JARs belong in
[the mod fork's Releases](https://github.com/IsSlashy/OpenYSM-Updated/releases).
This asset repository does not need copies of those JARs or Minecraft-specific
copies of unchanged model files.

Loading speed depends on the mod's synchronization and rendering code, as well
as the connection and computer. The experimental port prioritizes the player's
and nearby players' model data before the remaining catalog. That optimization
does not require converting, recompressing or renaming this repository's assets.

The tests used offline identities on loopback. Public authenticated hosting,
Linux hosts, proxies, and compatibility with other gameplay mods were not tested.
Any eventual public mod release needs its own support and licensing information;
these observations do not establish redistribution rights for mod code or assets.
