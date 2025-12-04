```
// WORK IN PROGRESS //
```

## Symp

a programming framework from a parallel reality where symbols won

---

#### Philosophy

What if there is a world where programming has evolved around source code ergonomics rather than user interface machinery?

* In that world, computer displays never evolved beyond the monochrome text mode.
* Instead, the programming theory advanced far beyond the one from this world.
* Source code became a subject of seeking beauty in structures of its appearance.

Symp is a small, strange, and honest framework evolved from using restricted visuals in a favor of rich programming theory.
It carries a thought experiment from a reality in which symbols triumphed over flashing lights.

---

#### The Architecture

There are two kinds of components in Symp: a core and a device.
There can be many devices, all attached to the core, as shown in the diagram:

```
                  · · · · · · · · ·
                  ·               ·
                  ·  d e v i c e  ·
                  ·               ·
                  · · · · · · · · ·
                          ·
                          ·
· · · · · · · ·     · · · · · · ·     · · · · · · · ·
·             ·     ·           ·     ·             ·
· d e v i c e · · · ·  c o r e  · · · · d e v i c e ·
·             ·     ·           ·     ·             ·
· · · · · · · ·     · · · · · · ·     · · · · · · · ·
                          ·
                          ·
                  · · · · · · · · ·
                  ·               ·
                  ·  d e v i c e  ·
                  ·               ·
                  · · · · · · · · ·
```

Devices may be a front-end interface to the outer world, while the back-end core is quietly connecting all of the devices.

Devices may communicate to each other using the core as a mediating environment which performs the intermediate computations.

The core is pure, stateless, and referentially transparent computing unit, while devices may maintain states during their life cycle.

Together, they form an integrated system capable of performing any kind of computation we may require as their users.

---

#### About The Core

Symp core is an S-expression based programming framework.\
It’s austere and minimalist although computationally complete.\
A tool for building programming frameworks as well as writing in them.

---

#### What You Can Build

Symp is tiny, but composable.\
It’s a playground for symbolic systems:

* Domain specific interpreters
* Meta-compilers and term rewriters
* Theorem provers and logic engines

Anything computable using strict rules.

---

#### Inspiration

Symp is mostly inspired by relation between stateless vs. stateful programming or central processor unit vs. peripheral hardware devices design. The modern programming technologies also influenced Symp appearance to a great extent.

---

## Docs

```
// WORK IN PROGRESS //
```

* [Symp Specs]
    * [Core Specs](docs/core)
        * [Imperative Specs]
        * [Functional Specs]
        * [Rewriting Specs]
    * [Devices Specs]
        * [Pagefront Specs]
* [What Does AI Say About Symp?]

```
// WORK IN PROGRESS //
```

