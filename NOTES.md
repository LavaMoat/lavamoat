# the big ones
- [x] autogen granularity
- [x] sourcemaps
- [x] do call with agoric/MM
- [ ] (external) SES prototype.toString etc
- [ ] perf

# make perf better
  - reduce instantiations
    - allow module cache under certain conditions
      - cant harden exports?
    - lazy instantiation via "universal proxy"
      - my guess: we use most things on boot
  - improve instantiation perf
    - transform src with endowments injection (build time slow down)

# dangers of module cache
- mutating the exports
- cant be done if it includes unhardenables (?)

# sesify prelude/kernel
- [x] pass custom endowments at require time
- [x] pass custom endowments at config time
  - [x] get config into bundle
  - [x] lookup config by module id / dep path
  - [?] how to deal with entry point name if entries specified by id / multiple entry points
- [x] include SES in prelude
- [x] share realm for all files in module?
- [x] make global module config as well
- [x] allow some sort of global realm sharing
- [x] set custom prelude in browserify via plugin
  - [x] works but sometimes breaks things...
  - [x] plugin without breaking things via b.reset()?  
- [x] need to not break sourcemaps
  - [x] good enough for now
  - [x] handle module names with @xyz/abc format
- [x] lockdown everything thats passed to module initializer
  - [x] wrap newRequire, etc
  - [x] remove excessive + dangerous moduleInitializer args
    - [x] investigate why corejs was using arguments[4] and see if others are too
- [x] cleanup prelude
- [x] is global caching safe? (no)
- [x] try using the frozen realm + container architecture
- [x] battletest via metamask
  - [x] background boot works : )
  - [x] sent first tx for background-only sesified
  - [x] contentscript doesnt?
  - [x] find sane default endowments

- [ ] support granular config
  - [x] actually expose api from granular config
  - [x] ensure we keep the "this" context, esp for deepGets
  - [ ] ensure we dont break Constructors with our "this" fix
- [x] browserify insertGlobal is ruining the parsing of properties on global
- [ ] sourcemaps
  - [x] needs to be able to compose over existing sourcemaps
  - [ ] needs to work when there are no existing sourcemaps
  - [x] config to specify inline or file
  - [x] config to dump map somewhere file
  - [?] ahhhhh nested inline sourcemaps?? not my problem??

- [ ] (external) allow less restrictive sandboxing modes (prototype.toString())
- [ ] (external) closer control over global? pass in "window" such that (window.Object === Object)

- [?] browserify the prelude

# tofu parser
- [x] mvp
  - [x] analyze required files for platform API usage
  - [x] use this to spit out a sesify config file (or something)
  - [x] get dependency info
  - [x] use generated config
- [ ] not terrible
  - [ ] more granular autogen config
    - [x] detect API usage on global
    - [x] dont pass window if no property accessed
    - [x] granularity on certain apis, e.g. document
    - [ ] raise platform api granularity to common denominator (e.g. dedupe "location" and "location.href"), including defaultGlobals
    - [ ] maybe limit granularity to actual platform API surface (e.g. not "location.href.indexOf")
    - [ ] browserify insertGlobal is ruining the parsing of properties on global
        - bc declaring the global object and passing it into a closure causes acorn-globals to ignore the uses of the global var
  - [x] user config defaultGlobals
  - [ ] location and document.location is redundant
  - [ ] location and location.href trigers page reload < !!! wow ouch !!! >
  - [ ] easy user override
    - [ ] likely need revDeps pointers at run time
  - [ ] use SES.confine instead of realm.evaluate
  - [ ] update ses
  - [ ] basic safety review
  - [ ] use autogen config if set to generate ?
- [ ] fancy
  - [ ] permissions as higher abstractions (network, persistence, DOM)
  - [ ] permissions sorted by risk (?)

# usability
  - [ ] cli support
    - [x] config gen
    - [ ] config read