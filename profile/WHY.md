# Why This Exists

🏠 **[EGS Overview](https://github.com/enduring-game-standard)** · 📍 **[Status](./STATUS.md)** · ⚖️ **[Governance](./GOVERNANCE.md)** · ❓ **[FAQ](./FAQ.md)**

This is the diagnosis the Enduring Game Standard answers, in about 1,500 words. It requires no position on money, cryptography, or decentralization — only the observation of how games die and how, for most of human history, they didn't.

## Three Deaths

**The Crew** (Ubisoft, 2014) sold to an audience of roughly twelve million players. In December 2023 it was delisted; on March 31, 2024 its servers went dark, and with them the game — including for players who had bought it and wanted only to drive alone. Nothing about the game was broken. The company that held its architecture decided the numbers no longer justified the electricity, and no one else was *able* to disagree: not the players, not archivists, not the developers who built it. The game did not decline. It was revoked.

**Telltale Games** (2018) was shut down in a single day — the majority of its staff dismissed without severance, *The Walking Dead*'s final season half-released — when its investors withdrew. The studio had shipped beloved, critically successful work for over a decade. What killed it was not the audience, which existed, nor the craft, which was proven. It was a funding structure whose clock ran out faster than the work could mature.

**38 Studios** (2012) shipped *Kingdoms of Amalur: Reckoning*, which sold 1.2 million copies in its first 90 days — a genuine commercial success. The studio collapsed anyway: its loan terms required roughly three million copies just to break even. A game could succeed with players and still kill its maker, because the capital behind it demanded returns on a schedule the medium cannot honor.

Three different failure surfaces — a platform revocation, a funding collapse, a debt structure — and the pattern generalizes. The Video Game History Foundation estimates that about 87% of classic video games are commercially unavailable. This is not the ordinary churn of a competitive art form. Books survive their publishers. Films survive their studios. Songs survive their labels. Games die *with* their companies, and that is an architectural fact, not a market one.

## Creative Amnesia

Industries built on durable substrates get *creative destruction*: the firm dies, the knowledge transfers, the next firm starts further ahead. The game industry gets something worse — call it **creative amnesia**. When a studio closes:

- Its **games** stop working, because they were welded to engines, servers, and licenses the studio no longer maintains.
- Its **objects** vanish, because every sword, save, and unlock lived in a proprietary database that is now a liability to be decommissioned.
- Its **craft** scatters, because the design knowledge — why the mechanics worked, what was tried and rejected, how the systems balanced — existed only in the heads and codebase of a team that no longer exists.

Each generation of developers starts nearly from scratch, reverse-engineering feel from finished products the way musicians would have to reconstruct harmony from concert recordings if notation had never been invented. The medium has produced fifty years of masterpieces and accumulated almost none of the *transmissible* craft that fifty years of chess or five centuries of written music produced.

## The Three Conditions

The games that survived millennia are not better games. They are games whose structure satisfies three conditions.

**Patient capital.** The Royal Game of Ur, chess, Go, and soccer never depended on a patron with a deadline. No funding round had to exit; no quarterly target forced monetization into the rules. The resources sustaining them did not expire. Modern game funding is the opposite: venture lifecycles, publisher milestone gates, and live-service revenue targets all put a clock on the work — and the clock does not merely pressure developers, it *filters* them. A developer who refuses to promise timelines incompatible with craft never gets funded at all. Telltale and 38 Studios were not uniquely reckless; they were what the filter selects.

**Durable substrate.** Every surviving game runs on materials anyone can reproduce without permission. A chess set can be carved, molded, or scratched in dirt; a pitch can be a street or a beach. When one board is destroyed, the game survives, because the substrate can be rebuilt by anyone, anywhere. Digital games inverted this: the substrate is closed code on revocable licenses and servers controlled by a single company. *The Crew* died of substrate failure. So does every game whose "shutdown" makes headlines each year. Nothing about being digital requires this — plain text, open specifications, and reproducible builds are as rebuildable as wood and dirt — but the prevailing business model requires enclosure, and enclosure guarantees eventual death.

**Cumulative craft.** Chess strategy compounds across 1,500 years of recorded analysis; a Babylonian tablet (BM 33333B, deciphered by Irving Finkel at the British Museum) preserves a rule set for the Game of Ur written when the game was already two thousand years old. Craft compounds only when it can be *written down* in a form that outlives its practitioners. Game design has no such notation in common use. The mechanics live inside executable code, inseparable from one implementation, unreadable apart from it.

The conditions are jointly necessary. Patient capital without a durable substrate dies with its hardware. A durable substrate without transmissible craft resets every generation. Craft with no capital never gets built. Modern video games are the medium in history that violates all three at once — which is why the most technologically capable era of games is also the most disposable.

## The Proof It Was Ever Otherwise

The golden age of modding (roughly 1996–2004) is the digital counterexample. One engine — Half-Life's — produced Counter-Strike, Team Fortress, Day of Defeat, and Natural Selection. Warcraft III's editor produced DOTA, and with it an entire genre no company planned. Open substrate, permissionless variation, compounding craft: the three conditions, briefly and partially satisfied, produced more genre innovation in eight years than the following two decades of enclosed live services. The era ended because the economics changed, not because the model failed. Enclosure became the revenue mechanism, and the revenue mechanism guarantees the death.

## What the Standard Does About It

Each condition maps to a separation, and each separation is a protocol:

- **Durable substrate** → separate the game's logic from any engine ([RUNS](https://github.com/enduring-game-standard/runs-spec)) and the game's objects from any server ([AEMS](https://github.com/enduring-game-standard/aems-schema)). Rules and things become plain-text, signed, reproducible artifacts that outlive every runtime that executes them.
- **Cumulative craft** → separate design knowledge from studios ([MAPS](https://github.com/enduring-game-standard/maps-notation)) — a notation in which mechanics can be written, studied, and forked the way music and chess have been for centuries.
- **Patient capital** → separate infrastructure coordination from any single company ([WOCS](https://github.com/enduring-game-standard/wocs-protocol)), so a living game's upkeep — hosting, moderation, tournaments — can be funded and provided by an open market rather than one balance sheet. The deeper capital question — where money patient enough for century-scale craft comes from at all — is an economic argument, not a protocol, and it lives in the book, not the specs.

The protocols are independent, minimal, and MIT-licensed; each one is adoptable without the others, and each is described declaratively in its own repository. What they share is the diagnosis above: games die because their layers are welded together, and every weld is severable.

## The Long Form

This page is the compressed diagnosis. The full argument — the historical record, the funding math, the economic thesis the protocols themselves do not depend on — is the book: ***Enduring Games: Patient Capital, Durable Substrate, and the Fix for an Industry That Eats Itself*** (Scott Sheppard, 2026).

**MIT License**
