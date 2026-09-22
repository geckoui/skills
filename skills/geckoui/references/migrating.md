# Migrating from v1 to v2

Anything below fails silently rather than at build time, so check by hand.

| v1                                | v2                                              |
| --------------------------------- | ----------------------------------------------- |
| `<GeckoUIPortal />`, self-closing | `<GeckoUIProvider>{children}</GeckoUIProvider>` |
| `<Alert variant="error">`         | `<Alert color="error">`                         |
| `<Drawer handleClose>`            | `<Drawer onClose>`                              |
| `<Checkbox partial>`              | `<Checkbox indeterminate>`                      |
| `<CounterInput editable>`         | `<CounterInput allowTyping>`                    |
| `CounterInput value: number`      | `value: string`                                 |
| `dismissOnEsc`                    | `dismissOnEscape`                               |
| `AlertVariantMap`                 | `AlertColorMap`                                 |
| `toast` from another package      | `import { toast } from "@geckoui/geckoui"`      |
| `BaseDateRangeInput` exported     | removed, use `DateRangeInput`                   |
| `RHFFilePicker`                   | removed, use `FileInput` / `RHFFileInput`       |
| `RHFFileInput` (the old one)      | rewritten on top of the new `FileInput`         |

The file components were rebuilt as one. `FileInput` is the field and `RHFFileInput` wraps
it, the way every other pair in the library works:

- One file by default. Pass `multiple` for a list, which also decides whether the value is
  a `PickedFile` or a `PickedFile[]`.
- `keepOldFiles` is now `append`, and `removeDuplicates` is `unique`. Both are only
  accepted alongside `multiple`.
- `preview` is off by default. `FileWithPreview` and `FilePickerFile` are both now
  `PickedFile`, with `path` always present and `preview` only when asked for.
- Drag and drop, directory picking and `accept` enforcement come with the one component.
- Files turned away reach `onReject` rather than disappearing.

Behaviour that changed without a rename:

- `Dialog.dismiss()` and `Drawer.dismiss()` only close their own type. In v1 either closed
  whatever was on top.
- Clicking inside a dialog no longer dismisses it.
- Dialogs sit above drawers, at z-index 2000 rather than 1000.
- Page scroll locks behind Dialog and Drawer, with the scrollbar width paid back as padding
  so nothing shifts.
- `ConfirmDialog` awaits `onConfirm` and `onCancel`, so an async one shows its loading state.
- `Checkbox` `indeterminate` is independent of `checked`; in v1 the dash needed `checked` too.
- Tooltips open after 200ms rather than 700ms. Pass `delayDuration={700}` for the old timing.
- `Textarea` no longer writes an inline height, so CSS can size one again. With `autoResize`
  it has `resize: none`, since the component owns the height.
- `DateInput` and `DateRangeInput` open their calendar at z-index 50 rather than an inline
  9999, so it no longer covers dialogs and toasts.
- Calendars size themselves to the month. Pass `fixedWeeks` for the old fixed height.
- Toast loses `richColors`, `theme` and `expand`; `toastOptions.className` and `.style`
  become `toastClassName` and `toastStyle`.

New in v2: `Accordion`, `Badge`, `Popover`, `Tabs`, and the `--color-success` / `--color-error` / `--color-warning`
/ `--color-info` semantic tokens.
