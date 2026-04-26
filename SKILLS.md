# Agent Skills Library

A comprehensive, reusable skills framework for agents. Each skill is modular, composable, and can be applied across different automation scenarios.

---

## 📋 Skill Categories

### 1. **DOM Manipulation Skills**
Core skills for interacting with page elements.

| Skill | Purpose | Input | Output | Use Case |
|-------|---------|-------|--------|----------|
| **findElement** | Locate a single DOM element | CSS selector (string) | Element or null | Finding specific component |
| **findElements** | Locate multiple DOM elements | CSS selector (string) | Element[] | Batch operations on similar elements |
| **updateAttribute** | Set HTML attributes | Element, attribute name, value | boolean | Change href, src, data-* attributes |
| **updateTextContent** | Change element text | Element, new text | boolean | Update labels, titles, content |
| **addClass** | Add CSS class to element | Element, class name | boolean | Apply styling, state changes |
| **removeClass** | Remove CSS class | Element, class name | boolean | Remove styling, state changes |

**Example Scenarios:**
- Update all button labels at once
- Change link destinations dynamically
- Apply active states to nav items

---

### 2. **Scroll & Navigation Skills**
Handle page navigation and viewport-aware actions.

| Skill | Purpose | Input | Output | Use Case |
|-------|---------|-------|--------|----------|
| **scrollToElement** | Smooth scroll to target | Element, offset (optional) | boolean | Navigate to sections |
| **getCurrentScrollSection** | Detect active section | Selector (optional) | string | Highlight nav based on scroll |

**Example Scenarios:**
- Scroll to contact form when user clicks CTA
- Update nav highlighting as user scrolls
- Jump to specific project/experience entry

---

### 3. **Event Handling Skills**
Attach and manage user interactions.

| Skill | Purpose | Input | Output | Use Case |
|-------|---------|-------|--------|----------|
| **onElementClick** | Attach click handler | Element, handler function | boolean | Trigger actions on click |
| **onElementHover** | Attach hover handlers | Element, onEnter/onLeave functions | boolean | Show tooltips, animations on hover |
| **batchElementEvents** | Attach event to many elements | Selector, event type, handler | number (count) | Apply same interaction to all buttons |

**Example Scenarios:**
- Track clicks on project cards
- Show/hide tooltips on skill tags
- Bulk apply event listeners to all links

---

### 4. **Animation & Visibility Skills**
Control animations and detect when elements enter viewport.

| Skill | Purpose | Input | Output | Use Case |
|-------|---------|-------|--------|----------|
| **observeElementVisibility** | Watch for elements entering viewport | Selector, callbacks, options | IntersectionObserver | Trigger animations when scrolled into view |
| **triggerAnimationSequence** | Chain animations with delays | Element[], class name, delay | Promise<void> | Stagger animations (like your portfolio cards) |

**Example Scenarios:**
- Fade in experience items as user scrolls
- Cascade animation of project cards
- Lazy-load images when visible

---

### 5. **Data Extraction Skills**
Read and extract data from the page.

| Skill | Purpose | Input | Output | Use Case |
|-------|---------|-------|--------|----------|
| **extractTextContent** | Get text from elements | CSS selector | string[] | Collect all headings, tags, or labels |
| **extractAttributes** | Get attribute values | Selector, attribute name | (string or null)[] | Collect all links, images, data attributes |
| **getElementStructure** | JSON representation of element | Element | JSON object | Debug/audit page structure |

**Example Scenarios:**
- Extract all skills/tags for display
- Collect all project titles
- Audit page for missing alt text

---

### 6. **Validation & State Checking Skills**
Verify element state and conditions.

| Skill | Purpose | Input | Output | Use Case |
|-------|---------|-------|--------|----------|
| **validateElement** | Check if element meets criteria | Element, criteria object | boolean | Verify element has required class/attribute |
| **isElementInViewport** | Check if currently visible | Element | boolean | Conditional actions based on visibility |

**Example Scenarios:**
- Verify element has proper accessibility attributes
- Only animate if element is visible (performance)
- Check if form section is in viewport before scrolling

---

### 7. **Composite Skills** (Multi-step operations)
High-level skills combining multiple basic skills.

| Skill | Purpose | Input | Output | Use Case |
|-------|---------|-------|--------|----------|
| **highlightElement** | Visual feedback animation | Element, highlight class | Promise<boolean> | Flash element to draw attention |
| **navigateToSection** | Scroll + highlight nav | Section ID, offset | boolean | Complete section navigation flow |
| **batchUpdateElements** | Update multiple elements | Selector, update function | number (count) | Bulk edit operation |

**Example Scenarios:**
- Highlight a newly added project
- Navigate to experience section with active nav feedback
- Update all price tags, labels, or timestamps

---

## 🔗 Skill Composition Patterns

### Pattern 1: Sequential Execution
Chain skills to accomplish multi-step tasks.

```
1. findElement('.contact-form')
2. scrollToElement(form, 100)
3. highlightElement(form, 'pulse')
4. onElementClick(form, submitHandler)
```

### Pattern 2: Batch Operations
Apply same skill to multiple elements.

```
1. findElements('.project-card')
2. triggerAnimationSequence(cards, 'fade-in', 150)
3. batchElementEvents('.project-card', 'click', expandProject)
```

### Pattern 3: Conditional Actions
Combine validation with actions.

```
1. findElement('#section')
2. isElementInViewport(section) → true
3. observeElementVisibility(section, animateIn, animateOut)
```

### Pattern 4: Data Collection + Transform
Extract + Process data.

```
1. extractTextContent('.tag')
2. Filter duplicates
3. Format for display/export
```

---

## 🎯 Real-World Examples from Your Portfolio

### Example 1: Section Navigation on Nav Click
```
When user clicks nav link:
→ findElement('a[href="#experience"]')
→ scrollToElement(target_section, 100)
→ getCurrentScrollSection() to update active state
→ highlightElement(nav_item, 'active')
```

### Example 2: Experience Card Reveal on Scroll
```
On page load:
→ findElements('.exp-item')
→ observeElementVisibility('.exp-item', addClass('visible'))
→ triggerAnimationSequence(items, 'fadeUp', 100ms between)
```

### Example 3: Update Contact Info
```
Bulk update:
→ batchUpdateElements('.contact-link', el => {
    updateAttribute(el, 'href', newValue)
    updateTextContent(el.querySelector('.link-value'), newText)
  })
```

### Example 4: Interactive Project Search
```
Filter projects:
→ extractTextContent('.project-title')
→ findElements('.project-card')
→ Filter by match
→ addClass(matches, 'visible')
→ removeClass(non_matches, 'visible')
```

---

## 📊 Skill Dependency Map

```
Basic Skills (single action)
├── DOM: findElement, findElements, updateAttribute, updateTextContent, addClass, removeClass
├── Scroll: scrollToElement, getCurrentScrollSection
├── Events: onElementClick, onElementHover, batchElementEvents
├── Animation: observeElementVisibility, triggerAnimationSequence
├── Extraction: extractTextContent, extractAttributes, getElementStructure
└── Validation: validateElement, isElementInViewport

    ↓ (can be combined)

Composite Skills (multi-step)
├── highlightElement (uses: addClass, removeClass)
├── navigateToSection (uses: findElement, scrollToElement, highlightElement)
└── batchUpdateElements (uses: findElements)
```

---

## 🚀 Usage Recommendations

### For Portfolio/Personal Site:
- **Navigation**: `scrollToElement`, `navigateToSection`, `getCurrentScrollSection`
- **Animations**: `observeElementVisibility`, `triggerAnimationSequence`
- **Interactions**: `onElementClick`, `onElementHover`, `batchElementEvents`

### For Data-Driven Apps:
- **Extraction**: `extractTextContent`, `extractAttributes`
- **Validation**: `validateElement`, `getElementStructure`
- **Batch Operations**: `batchUpdateElements`, `batchElementEvents`

### For Performance-Critical Sites:
- **Visibility Check**: `isElementInViewport` (before animations)
- **Lazy Operations**: `observeElementVisibility` (defer until needed)
- **Batch Execution**: All `batch*` skills reduce reflow/repaint

---

## 🔧 Skill Registry Reference

Quick lookup structure:

```
skillRegistry.dom                    // Element finding/modification
skillRegistry.scroll                 // Page scrolling
skillRegistry.events                 // Event attachment
skillRegistry.animation              // Scroll reveals, sequences
skillRegistry.extraction             // Data retrieval
skillRegistry.validation             // State checking
skillRegistry.composite              // High-level operations
```

---

## 📝 Best Practices

1. **Use Batch Skills** for multiple elements (better performance)
2. **Validate Before Acting** (check element exists first)
3. **Check Viewport** before expensive animations
4. **Chain Skills** for complex workflows
5. **Error Handling** — all skills return safe defaults (null, false, 0)

---

## 🔄 When to Use Each Category

| Scenario | Skills to Use |
|----------|---------------|
| User scrolls page | `getCurrentScrollSection` → update nav |
| User clicks button | `onElementClick` → `scrollToElement` |
| Page loads | `observeElementVisibility` → `triggerAnimationSequence` |
| Update from API | `batchUpdateElements` → `extractTextContent` |
| Debug/audit | `getElementStructure`, `validateElement` |
| Show/hide content | `addClass`/`removeClass` (combined with detection) |

---

**Last Updated:** April 26, 2026  
**Status:** Ready for production use  
**Compatibility:** Modern browsers (ES6+, IntersectionObserver support)
