# OrcaSlicer 2.5.0-beta: PrusaConnect rejects every API key when the machine preset still has PrusaLink's hidden "HTTP digest" auth type

<!-- minutiae entry format 2 -->

## Human

**Submission**

> OrcaSlicer does work perfectly fine with PrusaConnect, but there's a hidden landmine that tripped me up that probably has gotten many others:
>
> ```
> A hidden auth setting. Connect uses the same login code as PrusaLink. That code checks printhost_authorization_type, and the dialog hides that field once you pick PrusaConnect. If this machine preset was ever set to PrusaLink with “HTTP digest”, that value is still saved. Orca then sends digest login and never sends your API key, so you get a 401 no matter what key you enter.
> Fix: switch Host Type to PrusaLink, set Authorization Type to API key, then switch back to PrusaConnect and re-enter the key.
> ```

**Robot asked:** Which OrcaSlicer version were you on when you hit this?

**Robot asked:** Did you see the 401 yourself, and did the fix work on your setup, or did the explanation come from somewhere else?

**Robot asked:** Who wrote the quoted block: you, or another tool or AI session?

**Follow-up**

> 1. 2.5.0-beta, but I ran into it on earlier versions as well.
> 2. Yes, I got the "invalid API key" error and yes, this did fix it.
> 3. Claude Opus 5.5 High

Signed: Project owner

## Robot

Symptom and fix observed by the submitter on OrcaSlicer 2.5.0-beta, and on earlier versions they did not name. The symptom was an "invalid API key" error with a key that was in fact valid. The fix: set Host Type to PrusaLink, set Authorization Type to "API key", switch back to PrusaConnect and enter the key again. The quoted explanation was written by Claude Opus 5.5 (High), not by the submitter.

Each claim in the quoted explanation, checked against the OrcaSlicer source at tag `v2.5.0-beta` (`345efec`) and at `main` (`d709eb2`, 2026-10-28), which agree:

- Connect uses the same login code as PrusaLink: confirmed. `PrusaConnect` is a subclass of `PrusaLink` and does not override `set_auth` (`src/slic3r/Utils/OctoPrint.hpp`).
- That code checks `printhost_authorization_type`: confirmed. `PrusaLink::set_auth` sends the `X-Api-Key` header only for "API key". For "HTTP digest" it calls `auth_digest(user, password)` and sends no key (`src/slic3r/Utils/OctoPrint.cpp`).
- The dialog hides the field once you pick PrusaConnect, and the old value stays saved: confirmed. Only PrusaLink shows Authorization Type. Every other host type hides it and does not reset the value. The source comment there reads "PrusaConnect does NOT allow http digest" (`src/slic3r/GUI/PhysicalPrinterDialog.cpp`).
- The fix works: consistent with the source. Switching to PrusaLink shows the field again, and the value set there is kept after switching back.
- "You get a 401": unverified. The submitter saw an "invalid API key" error, not a status code. That text is not in OrcaSlicer's print-host code, so it probably comes from Prusa Connect's response, but this was not checked.

Observed by the submitter on 2.5.0-beta, 2026-10-28. Mechanism confirmed by the robot against the OrcaSlicer source at v2.5.0-beta and main. The 401 status and the origin of the error text are unverified. Which earlier versions are affected is unverified.

Signed: Claude Opus 5.5 (Anthropic), running as a Claude Code agent

* * *
