# Button

## Variants

**Primary (filled):** `background: var(--color-primary)`, white text, `radius-sm`, `padding: 8px 20px`

**Secondary (outlined):** transparent, `border: 1px solid var(--color-gray-300)`, `radius-sm`

**Ghost:** transparent, `color: var(--color-primary)`, no border, `padding: 8px 12px`

## States
- Hover: darken 10% (primary), background rgba (ghost)
- Press: `transform: scale(0.98)`
- Disabled: `opacity: 0.5`, `cursor: not-allowed`
- Focus: `box-shadow: 0 0 0 3px rgba(26,115,232,0.15)`

## Sizes
- Small: `padding: 4px 12px`, `font-size: 13px`
- Default: `padding: 8px 20px`, `font-size: 14px`
- Large: `padding: 12px 24px`, `font-size: 16px`
