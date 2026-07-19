# Services confirm dialogs & toasts

Do not use `window.confirm()` or `alert()` in services UI. Use the shared `ConfirmDialog` and Sonner toasts so feedback matches the app design.

## Destructive actions → ConfirmDialog

Use `@/components/ui/confirm-dialog` for delete/remove confirmations.

```tsx
// ✅ GOOD — controlled dialog, destructive styling
const [deleteOpen, setDeleteOpen] = useState(false)

function handleDeleteClick() {
  setDeleteOpen(true)
}

function executeDelete() {
  // fetch DELETE, then toast + state update
}

<ConfirmDialog
  open={deleteOpen}
  onOpenChange={setDeleteOpen}
  title="delete this note?"
  description="this cannot be undone."
  confirmLabel="delete"
  destructive
  onConfirm={executeDelete}
/>

// ❌ BAD — native browser dialog
if (!confirm('delete this note?')) return
```

**Pattern:**
- Button click opens dialog (`setOpen(true)`); API call runs only in `onConfirm`.
- Use `destructive` for delete/remove actions (red confirm button).
- Copy is lowercase; title is the question, description is the consequence.

**Reference:** `components/ui/confirm-dialog.tsx`, `app/(services)/services/notes/[id]/_components/note-editor-client.tsx`

## Create / delete feedback → Sonner toasts

```tsx
import { toast } from 'sonner'

// ✅ GOOD
if (!res.ok) {
  toast.error(json.error ?? 'failed to delete')
  return
}
toast.success('note deleted')

// ❌ BAD — silent success after create/delete
onDeleted(id)
```

**When to toast:**
- **Success:** category/note/influencer created, renamed, deleted, captured.
- **Error:** any failed API mutation.

Do not toast autosave or inline typing; use existing inline save status for that.

## Dialog styling (ConfirmDialog)

Built on `@/components/ui/dialog` (Radix):

- Lowercase labels (`cancel`, `delete`, `confirm`)
- `font-mono text-xs` buttons
- Cancel: bordered, `text-rurikon-400`
- Confirm: `bg-rurikon-600` (default) or `bg-red-500` when `destructive`
- `max-w-sm` content width

## Notes: reserved category name `stock`

The name `stock` is special in the notes service. When a user creates a category named `stock`, show an inline amber hint before submit:

> `"stock" is a special name — notes in this category will display stock ticker mentions and performance data.`

`StockMentionsSection` renders only when the note's category name is `stock` (case-insensitive).

```tsx
// ✅ GOOD
{categories.find(c => c.id === categoryId)?.name?.toLowerCase() === 'stock' && (
  <StockMentionsSection ... />
)}
```
