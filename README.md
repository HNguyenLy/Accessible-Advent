# Accessible Advent Calendar for Target Team Members

This project is an accessible, web based Advent calendar designed for Target team members who build digital experiences. Each day reveals a short accessibility tip, a quick quiz, and direct links into the Target Accessibility Standards (TAS). The experience is tuned for UX, content, engineering, QA, and product partners.

---

## Project goals

**Primary goals**

* Make accessibility learning **small, frequent, and practical**.
* Connect scenarios directly to **real Target contexts** (PDP, Weekly Ad, modals, forms, navigation, etc).
* Reinforce **Target Accessibility Standards** by linking each tip to 1 or 2 relevant TAS entries.
* Support **keyboard only and screen reader use** from day one.
* Offer focused guidance for **UX and engineering partners** so each role can see itself in the work.

---

## Key features

**Calendar experience**

* 31 day calendar rendered as a grid of real HTML buttons.
* Keyboard support for:

  * Tab into the grid.
  * Arrow keys to move between days.
  * Enter or Space to open a door.
  * Escape to close the tip dialog.
* Day level state:

  * Locked, Not opened, Previously opened.
  * Accessible labels updated when a door is opened.

**Tip dialog**

Each door opens a modal dialog that includes:

* Day title, category, and context (for example “Context. PDP gallery”).
* Short, focused accessibility tip written for Target contexts.
* Multiple choice quiz with:

  * Feedback text and success or error styling.
  * Success audio for correct answers and a buzzer audio for incorrect answers, controlled by the global sound toggle.
* TAS integration:

  * A “Relevant Target Accessibility Standards” block with up to two links per tip.
* New role and practice content:

  * **Role lenses**

    * For UX and content designers. Prompts to think in terms of flows, patterns, and copy.
    * For engineers and QA. Prompts to think in terms of components, templates, keyboard, and screen readers.
  * **Apply this in your work**

    * A small call to action to check one related surface and capture a backlog item or acceptance criteria.
  * **Go deeper**

    * Optional `<details>` block that encourages cross role pairing and full flow review against TAS.

**Progress and history**

* Progress counter. “You have unlocked X of 31 tips.”
* Streak indicator that reports how many days you have opened in order starting from Day 1.
* Category summary that summarizes how many tips you have seen by category (for example “So far you have explored 3 Design and content tips, 2 Engineering tips”).
* History list with “Review Day N” buttons to reopen any previously opened tip.
* A small prompt to use the calendar as a **team activity**.

  * “Consider opening one door together in standup or refinement and discussing how it applies to your current work.”

**Global controls**

* Sound toggle for sound effects (door, bells, quiz success, quiz buzzer).
* Music toggle for background audio where supported.
* High contrast toggle for a higher contrast visual theme.
* iOS specific behavior:

  * Volume slider hidden in iOS Safari, with helper text explaining that volume is controlled with device buttons.

---

## Accessibility notes

This project is intentionally opinionated about accessibility.

* Calendar uses semantic `<button>` elements with:

  * `aria-label` announcing day number and state.
  * Labels updated from “Not opened” to “Previously opened” once a door is opened.
* Modal uses the native `<dialog>` element where available:

  * `aria-labelledby` points to the dialog title.
  * Close button is keyboard accessible.
  * Focus is managed so that:

    * Opening a door moves focus into the dialog.
    * Closing the dialog returns focus to the triggering door.
* Keyboard interactions are fully documented in the introduction copy.
* Sound effects are decorative:

  * Audio is never required to understand content or complete the quiz.
  * Guests can turn sounds off globally.
* Content avoids “user” in tips and quizzes in favor of “guest” to align with Target language.

If you change structure or interactions, keep these goals in mind.

---

## Technology stack

* **HTML**

  * Single `index.html` file.
  * Native `<dialog>` element plus ARIA attributes for broad screen reader support.
* **CSS**

  * Plain CSS embedded in the page.
  * No external dependencies or frameworks.
* **JavaScript**

  * Plain vanilla JavaScript.
  * `adventData` array holds all tip and quiz content.
  * `tasIndex` maps each day to one or two TAS links.
  * `STATE` object tracks open days, audio preferences, and other UI state, with persistence via `localStorage`.

No build step is required. You can open `index.html` directly in a browser or serve it via any static host.

---

## Getting started

1. Clone the repository:

   ```bash
   git clone <repo-url>
   cd <repo-folder>
   ```

2. Open the calendar:

   * Option 1. Double click `index.html` to open it in your default browser.
   * Option 2. Serve it from a simple static server.

   ```bash
   python -m http.server 8000
   # then visit http://localhost:8000/index.html
   ```

3. Turn on sound (optional) with the sound toggle in the global controls bar.

4. Use Tab to move into the calendar and start opening doors.

---

## Content model and customization

### `adventData`

All door content lives in `const adventData = [ ... ]` inside the script.

Each entry looks roughly like:

```js
{
    day: 1,
    title: "Start with semantic HTML",
    category: "Engineering",
    context: "Buttons and interactive controls",
    content: "...tip text...",
    quiz: {
        question: "...",
        options: ["...", "...", "..."],
        correct: 1,
        explanation: "...why this is correct..."
    }
}
```

To customize content:

* Update `title`, `category`, `context`, and `content` for each day.
* Adjust `quiz` as needed:

  * `options` array, `correct` index, and `explanation`.
* Keep `day` unique and consecutive for the history and streak logic to make sense.

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
    ],
    // ...
};
```

The dialog automatically:

* Looks up `tasIndex[item.day]`.
* Renders up to **two** TAS links under “Relevant Target Accessibility Standards”.

To change TAS links for a day, adjust the corresponding entry in `tasIndex`.

---

## Working with UX and engineering partners

When evolving this project, consider:

* **UX and content designers**

  * Expand or refine `content`, `context`, and quiz copy to match real flows.
  * Use the role lenses to prompt designers to update patterns in design systems or templates.
* **Engineers and QA**

  * Use the apply prompts to drive small concrete improvements in code.
  * Treat the calendar as a “daily spike” on TAS for the current sprint.
* **Squads / teams**

  * Use the “Consider opening one door together” prompt as part of standup or refinement.
  * Capture actions in Jira or your team’s backlog directly from the “Apply this in your work” suggestions.

---

## Testing and quality checks

When you modify the code or content:

**Functional**

* Confirm you can:

  * Tab into the calendar and move between days with arrow keys.
  * Open and close dialogs with Enter, Space, and Escape.
  * Toggle sound and high contrast.
* Verify that:

  * A newly opened door updates its label to “Previously opened”.
  * Previously opened doors still read as “Previously opened” after a refresh.

**Accessibility**

* Test at least once with:

  * Keyboard only.
  * A screen reader on desktop (NVDA or JAWS on Windows, VoiceOver on macOS).
  * A screen reader on mobile for iOS and Android where possible.
* Check that:

  * Dialog content is announced when opened.
  * Focus returns to the right door when the dialog closes.
  * The “Go deeper” `<details>` element is keyboard operable and announced correctly.

---

## Contributing

When contributing changes:

1. Keep the experience **fully usable without sound**.
2. Maintain or improve **keyboard and screen reader support**, do not regress it.
3. Preserve the **guest first language** and the connection to TAS.
4. If you introduce new interaction patterns for quizzes or tips, keep them:

   * Short (2 to 3 minutes per door).
   * Testable with keyboard and screen readers.
   * Clearly explained in the tip content.

You can propose changes via pull requests and include:

* A short description of the change.
* Any new or updated day content.
* Notes on accessibility testing you performed.
