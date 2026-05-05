---
name: Tooltip Fixed Positioning
description: CSS group-hover tooltips get purged by Tailwind JIT; React state tooltips clip on parent overflow -- use position:fixed + JS bounding rect instead
type: feedback
related: feedback_css_hidden_display_override.md
originSessionId: b63c8355-4735-47ae-8aa8-c08f92fa6397
---
CSS group-hover tooltip approach (e.g. `group/tip`, `group-hover/tip:opacity-100`) gets stripped by Tailwind JIT purge when the class names are generated dynamically in JSX — tooltip never appears.

React useState approach clips when any ancestor has overflow constraints (common in card/table containers).

**Working solution:** `position: fixed` + JS `getBoundingClientRect()` measured on render:

```jsx
function CardTooltip({ text }) {
  const [visible, setVisible] = useState(false);
  return (
    <div style={{ position: 'relative', display: 'inline-flex' }}
      onMouseEnter={() => setVisible(true)}
      onMouseLeave={() => setVisible(false)}>
      <HelpCircle size={16} />
      {visible && (
        <div style={{ position: 'fixed', zIndex: 9999, ... }}
          ref={el => {
            if (el) {
              const rect = el.parentElement.getBoundingClientRect();
              el.style.left = rect.left + rect.width / 2 + 'px';
              el.style.top = rect.top - el.offsetHeight - 8 + 'px';
            }
          }}>
          {text}
        </div>
      )}
    </div>
  );
}
```

**Why:** `position:fixed` escapes all overflow/stacking contexts. Bounding rect measures the actual screen position so it always appears above the trigger regardless of scroll or nesting.

**How to apply:** Use this pattern for any tooltip or floating UI element in dashboards with card/table containers.
