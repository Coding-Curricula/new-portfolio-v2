# New Portfolio - v2 - forked from cboydstun

A static, hand-authored portfolio site built with HTML and CSS. Zero JavaScript — interactivity (theme toggle, mobile nav, project filters) is implemented with hidden checkbox/radio inputs and the CSS general-sibling (`~`) combinator.

## Stack

- HTML5
- CSS3 with custom properties for design tokens
- No build step, no package manager, no dependencies

## Layout

- `index.html` — the entire page
- `css/reset.css` — minimal modern reset
- `css/tokens.css` — design tokens (color, type, spacing, motion) and light/dark theme variables
- `css/base.css` — element defaults and reusable primitives
- `css/layout.css` — page chrome, container, breakpoints
- `css/components.css` — section/component styles *(to be authored)*
- `css/interactive.css` — `:checked`-driven behavior, including the dark-theme override *(to be authored)*

## Preview locally

Open `index.html` directly in a browser, or serve the directory with any static server:

```sh
python3 -m http.server 8000
# then visit http://localhost:8000
```

## License

All Rights Reserved

Copyright (c) [2026] [cboydstun]

THE CONTENTS OF THIS PROJECT ARE PROPRIETARY AND CONFIDENTIAL.
UNAUTHORIZED COPYING, TRANSFERRING OR REPRODUCTION OF THE CONTENTS OF THIS PROJECT, VIA ANY MEDIUM IS STRICTLY PROHIBITED.

The receipt or possession of the source code and/or any parts thereof does not convey or imply any right to use them
for any purpose other than the purpose for which they were provided to you.

The software is provided "AS IS", without warranty of any kind, express or implied, including but not limited to
the warranties of merchantability, fitness for a particular purpose and non infringement.
In no event shall the authors or copyright holders be liable for any claim, damages or other liability,
whether in an action of contract, tort or otherwise, arising from, out of or in connection with the software
or the use or other dealings in the software.

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the software.
