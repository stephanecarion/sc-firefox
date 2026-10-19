# sc-firefox

Personal Firefox CSS customizations, shared for others to use and adapt.

## macOS fullscreen overlay

[chrome/macos_fullscreen_overlay.css](chrome/macos_fullscreen_overlay.css) keeps the browser toolbar from shifting down when the macOS menu bar appears in native fullscreen. With `browser.fullscreen.autohide` enabled, the browser toolbars also overlay the page instead of pushing its content down.

### Why the original workaround stopped working

In the inspected Firefox 158 implementation, the macOS fullscreen menu-bar offset uses the CSS `translate` property, which operates independently of `transform`. Resetting the toolbar's margin or changing its `transform` no longer cancels that offset.

Firefox also adds inline styles, making the original `[style=""]` selector unreliable. This version:

- Disables the extra `translate` in macOS native fullscreen.
- Clears the matching `--fullscreen-menubar-shift` offset used by vertical tabs.
- Uses `fullscreenNavToolboxHidden` to track toolbar visibility instead of relying on an empty style attribute.

### Installation

1. Open `about:support` in Firefox and locate your **Profile Folder**.
2. Create a `chrome` folder inside that profile if it does not exist.
3. Copy the contents of this repository's [chrome folder](chrome) into that folder. The included [userChrome.css](chrome/userChrome.css) imports the hack stylesheet.
4. If you already have a `userChrome.css`, keep it instead of overwriting it. Copy the hack stylesheet alongside it and add the following import **at the beginning** of your existing file, before other CSS rules:

   ```css
   @import url("macos_fullscreen_overlay.css");
   ```

   CSS does not support wildcard imports. Add one `@import` per new hack in `userChrome.css`, or comment out an import to disable it.

5. In `about:config`, set `toolkit.legacyUserProfileCustomizations.stylesheets` to `true`.
6. Fully quit Firefox with **Cmd+Q**, then reopen it.

If you already use the original `overlay_fullscreen_toolbars.css` hack, remove its import or inline rules to avoid running both versions together. Back up your existing customization before making changes.

### Compatibility

The changes were based on the source bundled with Firefox 158 on macOS. Visual behavior still needs confirmation after restarting Firefox. Other versions and combinations with additional toolbar customizations have not been verified.

Firefox's internal UI selectors and behavior can change between releases. These customizations are unofficial and may require updates.

### Credits and license

Adapted from [MrOtherGuy/firefox-csshacks: overlay_fullscreen_toolbars.css](https://github.com/MrOtherGuy/firefox-csshacks/blob/master/chrome/overlay_fullscreen_toolbars.css).

The adapted CSS file remains available under the [Mozilla Public License 2.0](https://www.mozilla.org/MPL/2.0/). Its original attribution is preserved in the file. Local modifications add the macOS menu-bar offset fix and update the toolbar visibility selectors.
