# Accessible Advent Calendar for Target Team Members

This project is an accessible, web based Advent calendar designed for Target team members.
It delivers one small, practical accessibility tip per day across December, with role specific content that is tuned for UX, content, research, product, and engineering partners.

---

## Project goals

**Primary goals**

* Make accessibility learning **small, frequent, and practical**.
* Connect scenarios directly to **real Target contexts** (PDP, Weekly Ad, modals, forms, navigation, app flows).
* Reinforce **Target Accessibility Standards** by linking each tip to one or two relevant TAS entries.
* Support **keyboard only and screen reader use** from day one.
* Offer focused guidance for **different roles** so each partner can see themselves in the work.

**Secondary goals**

* Provide a **ready to use workshop tool** for accessibility champions, UX, QA, and engineering.
* Keep the implementation **simple, self contained, and easy to host** on any static server.
* Make it straightforward to **swap out content** while preserving the accessible shell and interaction patterns.

---

## Key features

### Calendar experience

* **31 day calendar** rendered as a grid of real HTML buttons.
* **Date aware door locking** in normal mode:

  * Before December for the configured year:

    * All doors are shown as present but locked.
    * Status text explains that doors unlock starting December 1.
  * During December:

    * For a guest visiting on December N:

      * Days 1 through N are unlocked and interactive.
      * Days N plus 1 through 31 remain locked.
    * Past days stay open so partners can catch up.
  * After December 31:

    * All 31 doors are unlocked and stay that way.
    * Status text shifts to a reference mode message.
* **Training mode** for facilitation and QA:

  * When the page is opened with `?mode=training` in the URL, all 31 doors are unlocked from the start.
  * The status region explicitly announces that training mode is active.
  * Calendar behavior, keyboard handling, and audio remain the same.

**Keyboard support**

* Tab to move focus from the intro into the calendar grid.
* Arrow keys to move between days **inside** the grid:

  * Left and Right move one day horizontally.
  * Up and Down move by row.
* Home and End to jump to the first or last day in the grid.
* Enter or Space to open a door.
* Escape to close the tip dialog.

**Day level state**

Each day tracks three simple states that are exposed visually and programmatically:

* **Locked**

  * In normal mode, future days and pre December days.
  * `aria-label` includes language like “Locked until December 12” when appropriate.
* **Not opened**

  * Current or past December days that have not been opened yet.
* **Previously opened**

  * Doors that have been opened at least once.
  * State persists in `localStorage` across refreshes.

There is also a gentle **door opening animation** for first time opens that:

* Adds a small pop and lift effect on the button.
* Respects the user’s `prefers-reduced-motion` setting.
* Avoids any continuous or distracting animations.

### Modal experience

Each door opens a rich but focused modal that feels like a Target holiday card.

**Structure**

* **Header**

  * Day badge with the day number.
  * Day title that includes the day number and tip title, for example “Day 7: Keyboard focus order”.
  * Close button labeled “Close tip” with a clear icon.
* **Meta row**

  * Compact display of the **category** (for example Design, Engineering) and any other context tags.
* **Tip content**

  * Short, concrete description of the accessibility practice.
  * Written for Target contexts and aligned with TAS.
* **Quiz**

  * Single multiple choice question per day.
  * Quiz explanation updated per role so the feedback connects clearly to the tip.
  * Success and error messages use both **icon and text**, not color only.
* **Apply this**

  * A small call to action that invites the partner to apply the tip to a real flow this week.
* **Role lenses**

  * Role specific guidance that explains how to look at the tip from the chosen role.
* **Go deeper**

  * Optional `<details>` block that suggests deeper practice, often involving pairing with a partner from a different discipline.
* **TAS block**

  * “Relevant Target Accessibility Standards” section with up to two TAS links per day.

**Modal behavior**

* Uses the native `<dialog>` element where supported, with ARIA fallbacks.
* `aria-labelledby` points to the dialog title.
* Focus moves into the dialog when opened and returns to the triggering door when closed.
* Escape key and the close button both close the dialog.
* Clicking on the backdrop closes the dialog for mouse or touch users, while preserving focus return for keyboard users.
* The entire modal content is readable with a screen reader in a logical order.

### Progress and history

* Progress counter:

  * “You have unlocked X of 31 tips.”
* Streak indicator:

  * Reports how many days have been opened **in order** starting from Day 1.
  * For example, “You have opened the first 5 days in order.”
* Category summary:

  * Summarizes how many tips you have explored by category, for example “So far you have explored 3 Design tips, 2 Engineering tips. You are viewing tips as a Product Manager.”
* Completion banner:

  * When all 31 doors are opened, a celebratory banner appears.
  * The date status line also switches to a “complete” announcement once per browser.
* History list:

  * Shows a list of opened days with “Review Day N” buttons.
  * Reopening a day uses the same modal and focus behavior as the calendar grid.
* Team activity prompt:

  * Encourages teams to open one door together during standup, refinement, or a chapter meeting.

### Global controls

* **Role selector**

  * Label: “Choose your role”.
  * Native `<select>` element with options:

    * UX Designer
    * Content Designer
    * UX Researcher
    * Product Manager
    * Android Engineer
    * iOS Engineer
    * Web Engineer
  * Changing the role:

    * Updates the role specific lenses, quiz explanation flavor, and “Apply this” guidance.
    * Updates the history summary text to reference the selected role.
    * Persists to `localStorage` so the role is remembered on the next visit.
* **Sound toggle**

  * Controls door, jingle, success, and error sounds.
  * Uses `aria-pressed` plus a clear label, for example “Sound Effects: On”.
* **Music toggle**

  * Turns background holiday music on or off where supported.
  * Uses `aria-pressed` and a clear label, for example “Background Music: Off”.
* **Volume control**

  * Range input from 0 to 100, stepping by 5.
  * Maps to an internal volume value between 0 and 1.
  * On iOS Safari the slider is hidden and replaced with a helper message instructing partners to use device volume buttons.
* **High contrast toggle**

  * Switches to a higher contrast theme with simplified decoration.
  * Preserves the visual hierarchy, focus treatment, and component structure.
* **Date and mode status**

  * A small live region under the controls that reports:

    * Current calendar phase (before December, during December, after December).
    * Which days are unlocked or locked today.
    * Whether **training mode** is active.
    * Role selection confirmations, for example “Role set to Android Engineer. Tips and quizzes are now tuned for this role.”

---

## Roles and content

The same 31 structural tips exist for all roles, but the **lens and call to action** are tailored.

### Supported roles

* UX Designer
* Content Designer
* UX Researcher
* Product Manager
* Android Engineer
* iOS Engineer
* Web Engineer

### What changes by role

Per role, the experience adjusts:

* **Role lenses**

  * Heading text, description, and examples tuned to that role’s responsibilities.
  * For example, UX Designers are nudged toward flows and patterns, engineers toward components and testability.
* **Apply this**

  * Role specific actions, such as:

    * “Review one PDP template and capture a backlog item” for designers.
    * “Add an automated accessibility check to this Compose screen” for Android Engineers.
* **Quiz explanation flavor**

  * Same core correct answer across roles, but explanation language and examples are updated to be more relevant to each discipline.
* **History summary**

  * Mentions the selected role in the category summary so partners know the current lens.

The base tip, category, quiz question, and TAS links are shared across roles. This keeps the calendar coherent while still giving each discipline a clear, relatable path to apply the work.

---

## Accessibility notes

This project is intentionally opinionated about accessibility.

* **Calendar buttons**

  * Real `<button>` elements inside a semantic list.
  * `aria-label` announces:

    * Day number.
    * State (“Locked”, “Not opened”, “Previously opened”).
    * Date locking where relevant (“Locked until December 14”).
    * “Today” prefix for the current day in December.
* **Modal**

  * Uses `<dialog>` with `aria-modal="true"` and `aria-labelledby`.
  * Focus is moved into the dialog on open and returned to the triggering button on close.
  * Escape key always works as a way to back out.
* **Keyboard navigation**

  * Grid supports arrow navigation plus Home and End within the calendar.
  * Tabbing continues to work for partners who prefer standard tab order.
  * “Go deeper” `<details>` and `<summary>` are keyboard operable.
* **Motion and animation**

  * Door opening and modal enter animations only play when `prefers-reduced-motion` allows it.
  * No continuous or flashing animation.
* **Sound**

  * Experience remains fully usable without sound.
  * All audio is optional and controlled by the sound and music toggles.
  * Quiz feedback and completion states are always communicated visually and textually.
* **Contrast and themes**

  * Default frame uses Target holiday styling with tokens tuned for contrast.
  * High contrast mode simplifies decoration while maintaining clear hierarchy and sufficient contrast.

---

## Technology stack

* **HTML**

  * Single `index.html` file.
  * Native `<dialog>` element with ARIA wiring for accessibility.
  * Semantic markup for the calendar, history list, and controls.
* **CSS**

  * Plain CSS embedded in the page.
  * Token like variables for Target colors, spacing, and radii.
  * Modal and door animations implemented with `@keyframes` and `prefers-reduced-motion` checks.
* **JavaScript**

  * Plain vanilla JavaScript, no frameworks.
  * `adventData` array holds per day tip, quiz, and category data.
  * `tasIndex` maps each day to one or two TAS links.
  * `ROLE_CONFIG` defines the available roles and labels.
  * `ROLE_COPY` supplies role specific headings and guidance used by the modal.
  * `STATE` tracks:

    * Opened days.
    * Selected role.
    * Sound enabled flag.
    * Volume level.
    * High contrast mode.
    * Training mode flag.
    * Completion announcement flag.
  * Persistent state is stored via `localStorage` with safe fallbacks when storage is unavailable.

No build step is required. You can open `index.html` directly in a browser or host it on any static server.

---

## Getting started

1. Clone the repository.

   ```bash
   git clone <repo-url>
   cd <repo-folder>
   ```

2. Open the calendar locally.

   ```bash
   # Simple option
   open index.html

   # Or run a static server
   python -m http.server 8080
   ```

3. Visit the page in your browser.

   * Normal mode (date aware):

     * `http://localhost:8080/index.html`

   * Training and facilitation mode (all doors unlocked):

     * `http://localhost:8080/index.html?mode=training`

4. Use Tab to move into the calendar and start opening doors.

---

## Content model and customization

### `adventData`

All door content lives in `const adventData = [ ... ]` inside the script.

Each entry looks roughly like:

```js
{
    day: 1,
    title: "Semantic HTML first",
    category: "Engineering",
    context: "Building a new component in the Target app.",
    content: "Short, Target specific tip copy.",
    quiz: {
        question: "Which element should you use for a clickable action that submits a form?",
        options: [
            "<div onclick='submit()'>",
            "<button type='submit'>",
            "<a href='#'>"
        ],
        correct: 1,
        explanation: "Clear, role neutral explanation of the correct answer."
    },
    roleGuidance: {
        // Optional per day overrides for headings or copy
        // when a more specific role treatment is needed.
    }
}
```

Notes:

* `day` must be unique and between 1 and 31.
* `category` is used by the category summary in the history panel.
* `quiz.explanation` should remain correct for all roles; role specific nuance comes from `ROLE_COPY`.

### Role configuration

Roles are defined in two main places.

* `ROLE_CONFIG`

  ```js
  const ROLE_CONFIG = [
      { id: "ux-designer", label: "UX Designer" },
      { id: "content-designer", label: "Content Designer" },
      { id: "ux-researcher", label: "UX Researcher" },
      { id: "product-manager", label: "Product Manager" },
      { id: "android-engineer", label: "Android Engineer" },
      { id: "ios-engineer", label: "iOS Engineer" },
      { id: "web-engineer", label: "Web Engineer" }
  ];
  ```

  To add or rename a role, update this array and the corresponding copy in `ROLE_COPY`.

* `ROLE_COPY`

  Each entry provides role specific lenses and calls to action. An entry looks like:

  ```js
  "android-engineer": {
      heading: "How to look at this as an Android Engineer",
      dt: "For Android Engineers",
      howToLook: "Guidance that ties the tip to Compose, TalkBack, Dynamic Type, and Target Android practices.",
      applyHeading: "Apply this in your Android work",
      applyText: "Concrete steps to take in a screen, component, or test suite."
  }
  ```

  You can override the heading for a specific day using `item.roleGuidance.heading` if needed.

### TAS mapping

`tasIndex` is a simple mapping from day number to an array of TAS link objects:

```js
const tasIndex = {
    1: [
        { title: "Accessibility at Target", url: "..." },
        { title: "Accessibility standards overview", url: "..." }
    ],
    2: [
        { title: "Images Accessibility Standard", url: "..." }
    ]
    // Additional days
};
```

The modal will render up to **two** links under “Relevant Target Accessibility Standards”.
To change TAS links for a day, adjust the corresponding entry in `tasIndex`.

---

## Testing and quality checks

When changing content or behavior, test both **normal** and **training** modes.

**State and behavior**

* Open a few doors, refresh the page, and confirm:

  * Opened state persists.
  * Role selection persists.
  * Completion banner appears only when all 31 days are opened.
* Confirm that date locking logic behaves as expected when you adjust your system date.

**Keyboard**

* Tab into the header, controls, and calendar.
* Use arrows, Home, and End to move inside the grid.
* Confirm focus is always visible and never trapped.
* Validate that the history list and “Review Day N” buttons are reachable and usable.

**Screen readers**

* Test with at least one desktop and one mobile screen reader.
* Confirm:

  * Day labels describe state and date locking correctly.
  * Modal titles and content are announced in a useful order.
  * Quiz feedback is announced when answers are selected.
  * “Go deeper” details are announced and can be toggled.

---

## Working with UX and engineering partners

When evolving this project, keep in mind:

* **UX and content designers**

  * Expand or refine `content`, `context`, and quiz copy to match real flows.
  * Use the role lenses to prompt updates in design systems or templates.
* **Engineers**

  * Keep keyboard, focus, storage, and screen reader support at least as strong as the current implementation.
  * Use the calendar as a working example when building similar experiences in Canvas or Nicollet.
* **Product managers and leaders**

  * Treat the calendar as a reusable learning path for teams.
  * Encourage completion badges or follow up commitments to ship at least one change.

---

## Contributing

When you propose changes, please:

1. Keep the experience **fully usable without sound**.
2. Maintain or improve **keyboard and screen reader support**.
3. Preserve the **connection to TAS** and Target standards.
4. Keep door interactions:

   * Short, roughly two to three minutes per day.
   * Easy to test with keyboard and screen readers.
   * Clearly explained in the tip content.

Include in your pull request:

* A short description of the change.
* Any new or updated day content.
* Notes on accessibility testing you performed, including which assistive technologies you used.
