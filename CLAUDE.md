# nusa — standalone actor repository

Nusa records ritual and industrial low-THC hemp heritage. Recreational use,
high-THC cultivation, medical guidance, unlicensed cultivation, and political
advocacy are outside its scope.

## Canonical layout

- `manifest.edn`: actor metadata, gates, cells, and non-goals
- `src/nusa/`: native CLJC analyzers and state machines
- `test/nusa/`: invariant tests
- `contracts/lexicons/`: canonical EDN lexicons
- `data/`: representative graph seed, cell declarations, identity journal
- `wire/`: ATProto and DID/profile JSON interoperability representations
- `docs/adr/`: actor-owned decisions

JSON below `wire/` is not canonical metadata or contract data. Do not restore
the removed JSON-LD manifest or root website artifacts.

## Verification

Run `bb test`. The suite must preserve the low-THC enum boundary,
non-recreational purpose, member-principal/no-server-key constraints,
sourcing honesty, and outward gate.

Go, TinyGo, Python compatibility code, WASM build artifacts, and shell test
runners are deprecated and must not be added.
