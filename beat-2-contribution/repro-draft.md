# Baseline report for OpenChemistry/avogadrolibs #3184

## Environment

- Microsoft Windows 11 Pro 10.0.26100 with Docker Desktop
- Fork: `jaskhetani/avogadrolibs`
- Upstream commit: `cb0e94852889da9f7288bd1e58241e69990935a7`
- Local image: `ai301-avogadro-qt:24.04` (Ubuntu 24.04, Qt `uic` 6.4.2, CMake 3.28.3, Ninja 1.11.1, and libxml 2.9.14)
- Repository mounted read-only at `/workspace`

## Steps

1. Parse the two claimed UI files as XML:

   ```text
   xmllint --noout avogadro/qtplugins/surfaces/surfacedialog.ui avogadro/qtplugins/surfaces/orbitalwidget.ui
   ```

2. Generate C++ headers from each unmodified Qt Designer file:

   ```text
   /usr/lib/qt6/libexec/uic avogadro/qtplugins/surfaces/surfacedialog.ui -o /tmp/ui_surfacedialog.h
   /usr/lib/qt6/libexec/uic avogadro/qtplugins/surfaces/orbitalwidget.ui -o /tmp/ui_orbitalwidget.h
   ```

3. Inspect the label widgets in both source files. `surfacedialog.ui` has seven `QLabel` widgets and `orbitalwidget.ui` has one; none currently has a `buddy` property.

## Evidence

The baseline command completed with:

```text
BASELINE_UI_VALIDATION=PASS
  422 /tmp/ui_surfacedialog.h
  143 /tmp/ui_orbitalwidget.h
  565 total
```

**Expected after the fix:** Each field label in the claimed files names its corresponding input widget through a Qt `buddy` property, so its mnemonic can move keyboard focus to that control. Descriptive or group labels remain unlinked.

**Actual at the pinned baseline:** Both files are valid XML and compile through Qt `uic`, but all eight field-label candidates lack a `buddy` property. This is static evidence for the accessibility gap; keyboard-focus behavior must still be verified in a built application after the human-authored change.
