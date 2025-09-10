### Publishing your mod

#### Best practices

- Name your mod using the pattern `<author>.<modname>`, e.g., `R5Valk.Client`, `YourName.CustomServer`.
  - The `R5Valk.*` namespace is reserved for official mods bundled with the R5Valk install. Do not use it for third‑party mods.
- Host your source code on a public repository (e.g., [GitHub](https://github.com/)) so users can file issues, suggest changes, and contribute.
- Showcase your changes with screenshots, short videos, or GIFs in your README. This helps users understand what your mod does at a glance.
  - Upload media to a reliable host (GitHub, Imgur, Discord CDN) and embed it in your README:

```markdown
![Alt text shown if the image fails to load](https://example.com/path/to/image-or-gif.gif)
```

---

### Thunderstore

The recommended place to publish is [Thunderstore](https://thunderstore.io/) (for R5 Valkyrie: `https://thunderstore.io/c/r5valk`). You’ll need to package your mod as a `.zip` with a specific structure. You can set this up manually or use the [R5Valk mod template](https://github.com/r5valkyrie/r5valk-mod-template).

#### Package structure

Your zip should look like this:

```
<modfiles>
icon.png
manifest.json
README.md
```

- `<modfiles>`: your mod files.
- `icon.png`: 256×256 icon for your mod.
- `README.md`: your mod’s description page.
- `manifest.json`: Thunderstore manifest.

#### Uploading

1. Prepare the folder structure above and zip it.
2. Go to `https://thunderstore.io/c/r5valk`, sign in (Discord or GitHub).
3. Click Upload and select your zip. The site will validate your package.
4. Once validation passes, publish.

#### Updating

- Bump your version in both:
  - Your mod’s `mod.vdf`
  - The Thunderstore `manifest.json`
- Rebuild the zip and upload again with the same mod name. Thunderstore will publish a new version for the same package.
