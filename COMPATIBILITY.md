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

## Experimental Minecraft 26.2 verification

These are observations from an experimental local Fabric port, **not an official
YSM 26.2 release or a compatibility guarantee for other implementations**. No mod
binary or mod source is distributed by this documentation change.

Repository revision tested: `1749b46467acea11694891c01ede24914b22dc6c`.

| Check | Result |
| --- | --- |
| Model files parsed | 1,114: 906 `.ysm` files and 208 ZIP models |
| `.ysm` container versions represented | v1, v2, and v3 |
| ZIP models with a single enclosing directory | 6 |
| Original asset contents | Unchanged; no model conversion was needed |
| Dedicated server | Minecraft 26.2, Fabric Loader 0.20.1, Java 25, Windows |
| Multiplayer test | Two independent clients connected to a separate loopback server |
| Models advertised in the full server test | 1,146 including 27 built-in models and 5 extra regression fixtures |
| Client model installation | No custom models preinstalled; server download and cache exercised |

The port needed archive-reader, rendering, and synchronization changes. The asset
files themselves did not need to be rewritten for Minecraft 26.2. Parsing the
entire catalog does **not** mean that every model and animation was visually
inspected. Detailed animation checks used Ame and KAngel fixtures: movement,
crouching, jumping, swinging/mining, item use, riding, sleeping, swimming, flight,
and eight extra animations. Remote model changes, alternate textures, late joins,
and returning to another player's tracking range were also exercised.

The tests used offline identities on loopback. Public authenticated hosting,
Linux hosts, proxies, and compatibility with other gameplay mods were not tested.
Any eventual public mod release needs its own support and licensing information;
these observations do not establish redistribution rights for mod code or assets.
