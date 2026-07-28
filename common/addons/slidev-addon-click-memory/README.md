# slidev-addon-click-memory

Slide navigation controls that remember and restore each slide's click position.

## Features

- Adds slide-only previous and next buttons to Slidev's navigation bar
- Moves exactly one slide without stepping through click animations
- Remembers the most recently viewed click position of each slide
- Restores that position when revisiting the slide
- Uses Slidev's public navigation API

## Installation

```bash
npm install -D slidev-addon-click-memory
```

## Usage

Add the addon to the headmatter of your `slides.md`:

```yaml
---
addons:
  - slidev-addon-click-memory
---
```

The addon adds two controls to the navigation bar:

- `⇤`: move to the previous slide
- `⇥`: move to the next slide

Previously visited slides are restored to their most recently viewed click position.

## Memory lifetime

Click positions are stored in browser memory.

The state is preserved while navigating between slides, but is reset when the presentation page is reloaded.

## License

MIT