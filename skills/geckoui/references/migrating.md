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

Behaviour that changed without a rename:

- `Dialog.dismiss()` and `Drawer.dismiss()` only close their own type. In v1 either closed
  whatever was on top.
- Clicking inside a dialog no longer dismisses it.
- Dialogs sit above drawers, at z-index 2000 rather than 1000.
- Page scroll locks behind Dialog and Drawer, with the scrollbar width paid back as padding
  so nothing shifts.
- `ConfirmDialog` awaits `onConfirm` and `onCancel`, so an async one shows its loading state.
- `Checkbox` `indeterminate` is independent of `checked`; in v1 the dash needed `checked` too.
- Calendars size themselves to the month. Pass `fixedWeeks` for the old fixed height.
- Toast loses `richColors`, `theme` and `expand`; `toastOptions.className` and `.style`
  become `toastClassName` and `toastStyle`.

New in v2: `Accordion`, `Badge`, `Popover`, `Tabs`, and the `--color-success` / `--color-error` / `--color-warning`
/ `--color-info` semantic tokens.
