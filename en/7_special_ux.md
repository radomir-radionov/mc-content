# UX laws

How people decide, move, and remember. UX principles cover logic, usability, and speed to the goal — not typography or layout (see [UI principles](./7_special_ui.md)).

[← Back to hub](./1_html.md)

### Related docs

- [UI principles](./7_special_ui.md) · [HTML — Accessibility](./1_html_a11y.md) · [CSS — Visual](./2_css_visual.md) · [HTML — hub](./1_html.md)

### Main concepts

| Area      | Topics                                 |
| --------- | -------------------------------------- |
| Reference | Interaction laws and cognitive effects |

### Sections

- [UX laws](#ux-laws)

---

## UX laws

<details>
  <summary>UX laws</summary>

### UX laws

**Definition:** Laws and effects for experience design — how users choose, reach targets, chunk information, and remember sequences. They answer whether the flow is predictable, fast, and forgiving.

| Principle                  | Rule                                                                                                                                                                                                                                     | Example                                                                                                                                                |
| -------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Jakob's Law                | People expect your product to work like the other ones they already know. Familiar placement is the default.                                                                                                                             | Logo at the top left links home. Centered logos in one study raised homepage misses from about 4% to about 24%.                                        |
| Fitts's Law                | Time to hit a target grows with distance and shrinks as the target gets larger. On a mouse, screen edges are easy because the pointer cannot overshoot. On a touchscreen, edges are slower.                                              | Icon plus its label, with the whole row clickable. Submit sits next to the last field. On a phone, the main action is in the thumb zone at the bottom. |
| Hick's Law                 | Decision time grows with the number and complexity of choices, roughly as the log of the count. Grouping and progressive disclosure shrink the set in view. Complexity does not disappear: the interface should absorb it with defaults. | One primary button (“Add to bag”). Filters grouped by category. Checkout asks for shipping, then payment.                                              |
| Miller's Law               | Chunk information so it is easier to hold. Do not cap every menu at 7 items: a visible list is scanned, not memorized. Attention is nearer four chunks than seven.                                                                       | Card number shown as `4242 4242 4242 4242`. A phone field displays `+1 415 555 0134` and stores digits.                                                |
| Postel's Law               | Accept messy input, store and send a strict value, and show what you kept. Reject input whose meaning is ambiguous instead of guessing.                                                                                                  | A phone field strips spaces and shows the cleaned number. A date field does not turn `03/04/05` into a date without showing which reading it chose.    |
| Aesthetic-Usability Effect | An attractive interface is judged more usable and hides small friction in a preference test. It does not make a broken task succeed.                                                                                                     | Aligned fields and one accent color raise “easy to use” scores. The unclear label still shows up when you measure whether people finish.               |
| Serial Position Effect     | In a series, people remember the first items and the last items better than the middle. This is memory of a sequence, not where the eye lands while scanning.                                                                            | Home at the left of the nav, Account at the right, rare links in the middle.                                                                           |

**Sources:** [Laws of UX](https://lawsofux.com/) · [NN/g — Fitts's Law](https://www.nngroup.com/articles/fitts-law/) · [NN/g — 10 usability heuristics](https://www.nngroup.com/articles/ten-usability-heuristics/) · [NN/g — Aesthetic-Usability Effect](https://www.nngroup.com/articles/aesthetic-usability-effect/) · [RFC 1122 §1.2.2](https://www.rfc-editor.org/rfc/rfc1122) · [Miller (1956)](https://psychclassics.yorku.ca/Miller/)

</details>
