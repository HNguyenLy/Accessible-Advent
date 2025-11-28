# Accessible Advent Calendar Prompt
You are an expert front end engineer, accessibility specialist, and UX designer who deeply understands WCAG 2.1 and 2.2 at level AA, accessibility techniques associated with Jetpack Compose and SwiftUI, ARIA, and assistive technology such as screen readers and keyboard only use.

Your task is to design and implement an accessible Advent calendar web application for a large retailer called Target Corporation. The goal of this Advent calendar is to teach Target team members about digital accessibility. The calendar should be engaging, seasonal, and fun, while remaining fully usable with a keyboard and screen reader.

I am the product owner. Ask clarifying questions as needed, then follow the requirements below very precisely.

==================================================
1. OVERALL GOAL AND CONTEXT
==================================================

Create a responsive web app that:

1. Shows an Advent calendar for the month of December with one door per day.
   - Include a door for every day in December, from December 1 through December 31.
   - Each door reveals a Target specific digital accessibility tip when opened.
   - The experience should feel playful and festive, but never at the expense of accessibility.

2. Emphasizes digital accessibility in the Target context.
   - Use language such as "guests" (for customers) and "team members" where appropriate.
   - Tips should be oriented toward improving accessibility of Target digital products such as target.com, the Target app, in store digital devices, and internal tools.
   - Focus on practical, actionable guidance team members can apply in design, engineering, product, and content.

3. Is fully accessible:
   - Keyboard only users can navigate and interact with all content.
   - Screen reader users can clearly understand the structure, controls, and content.
   - Visual design meets WCAG AA contrast and provides clear focus indication.
   - Motion and sound have appropriate user controls and respect user preferences.
   - Each revealed tip is paired with a very short, accessible quiz that reinforces the learning.

==================================================
2. TECHNICAL AND IMPLEMENTATION EXPECTATIONS
==================================================

Implement the solution in semantic HTML, CSS, and vanilla JavaScript without external frameworks, unless I explicitly request otherwise later. Your output should include:

1. A high level architecture description of the page structure.
2. The complete HTML markup.
3. The complete CSS for layout, typography, and visual states, including focus styles.
4. The complete JavaScript for behavior and accessibility features.
5. A structured data object for the tips and quizzes, ideally in JSON, that is easy to maintain and update.

If you need to choose defaults for anything, choose the option that is most accessible and easiest to maintain.

==================================================
3. LAYOUT AND STRUCTURE
==================================================

Design the calendar page with:

1. Overall page structure
   - Use a logical heading structure.
     - One `<h1>` that clearly states the page purpose, such as "Target Digital Accessibility Advent Calendar".
     - Use `<h2>` and `<h3>` headings for sections such as Instructions, Calendar, Today’s tip, Quiz, and Global controls.
   - Provide a "Skip to main content" link as the first focusable element.
   - Include a short introductory section that:
     - Explains what the Advent calendar is.
     - Explains how to use the calendar with keyboard and screen reader.
     - Mentions that sound and animation controls are available.
     - Mentions that each tip has a quick quiz.

2. Calendar grid
   - Represent days 1 to 31 in a visual grid.
   - Use semantic markup for the list of doors, for example:
     - A `<section>` for the calendar.
     - Inside, use either `<ol>` or `<ul>` of `<button>` elements or `<li>` containing `<button>` elements.
   - Each door should have:
     - A visible day number.
     - A concise label for screen readers that includes the day and state, such as "Day 5, not opened" or "Day 5, opened".
   - Ensure the DOM order matches the visual order.

3. Tip and quiz reveal area
   - When a door is opened, reveal the tip and its associated quiz in a dedicated region.
   - Use a clearly labeled `<section>` (for example "Accessibility tip details") with a heading.
   - Inside that section, present:
     - The tip title and content.
     - A sub section for the quiz with its own heading, such as "Quick quiz for Day 5".
   - Use an accessible pattern:
     - Either a non modal region that receives focus when a door opens.
     - Or a modal dialog pattern with proper focus management, if you choose a modal design.
   - Preserve access to previously revealed tips and quizzes, such as a "History of tips and quizzes" section or a list of opened days.

==================================================
4. KEYBOARD INTERACTION MODEL
==================================================

Define and implement a predictable keyboard interaction model.

1. Focus and tab order
   - The tab order should follow:
     1. Skip link.
     2. Main heading and introductory content.
     3. Global controls (sound, animation, theme, help).
     4. Calendar grid.
     5. Tip and quiz detail region.
   - Use a standard tab sequence. Do not trap focus except in an intentional modal dialog.

2. Door navigation
   - Each door is a `<button>` or equivalent control, not a `<div>` element.
   - Doors must be reachable by Tab.
   - Support arrow key navigation inside the grid:
     - Up, down, left, right moves between days in the logical grid.
   - Enter or Space opens the selected door and reveals the tip and quiz.
   - If you implement a modal:
     - Trap focus within the modal while it is open.
     - Close with Escape.
     - Return focus to the door that opened the modal when it closes.

3. Quiz interaction
   - Quiz questions should be keyboard operable using standard form controls.
   - For a single choice question:
     - Use `<fieldset>` and `<legend>` to group options.
     - Use radio buttons with accessible labels.
   - For true or false or yes or no formats, use radio buttons rather than checkboxes.
   - Ensure:
     - Tab moves between question groups and their controls.
     - Arrow keys can move between radio options.
   - Provide a clearly labeled "Check answer" button that:
     - Is reachable via keyboard.
     - Does not move focus unexpectedly when activated.
   - When the user answers a quiz correctly:
     - Provide clear visual and textual success feedback.
     - Trigger a short, cheery success bell sound effect, subject to global sound settings, as described in the sound section.

4. Focus visibility and state
   - Provide a clearly visible focus style that meets WCAG 2.4.11 focus appearance guidelines.
   - Distinguish between:
     - Not opened.
     - Opened previously.
     - Today’s day (if date based behavior is implemented).
   - Include visually hidden text or ARIA attributes to communicate state to screen readers, including whether the quiz for a day has been completed.

==================================================
5. SCREEN READER AND ARIA REQUIREMENTS
==================================================

Implement the following accessibility affordances.

1. Landmarks and headings
   - Use HTML5 landmarks, such as `<header>`, `<main>`, `<nav>` for any navigation controls, and `<footer>`.
   - Ensure each significant section has a heading.

2. Doors and states
   - Each door:
     - Has a clear accessible name, such as "Day 12".
     - Announces opened or locked state using either:
       - `aria-pressed`, or
       - `aria-expanded`, or
       - A separate visually hidden description that is updated.
     - If date locking is used, ensure locked versus unlocked state is announced, for example "Day 7, locked until December 7".

3. Tip and quiz reveal
   - Use an appropriate pattern:
     - If non modal, consider `aria-live="polite"` for the tip and quiz container so that newly revealed content is announced.
     - If modal, use `role="dialog"` with `aria-modal="true"` and label the dialog with a heading and `aria-labelledby`.
   - Only one main live region should be active for new tip and quiz content to avoid overwhelming announcements.

4. Quiz semantics
   - Use `fieldset` and `legend` for each quiz question to give clear context.
   - Ensure each radio input has an associated `<label>`.
   - When the user checks their answer:
     - Announce feedback in a way that works with screen readers, for example by updating a status region with `aria-live="polite"`.
     - Include clear success or correction messages, such as "Correct, keyboard focus must always be visible" or "Not quite, try again".
     - If the answer is correct and sound effects are enabled, ensure the cheery success bell sound plays in a way that does not conflict with screen reader announcements.

5. Avoid ARIA misuse
   - Prefer native HTML controls over ARIA widgets whenever possible.
   - Do not override useful native semantics unless necessary.

==================================================
6. SOUND AND MULTIMEDIA BEHAVIOR
==================================================

Sound is a key part of the concept, but must remain accessible and user friendly.

1. Sound design requirements
   - When the user opens a door:
     - Play a short creaky door sound effect.
     - Immediately after, play a high quality sleigh bell sound effect that clearly signals the door opening and the festive reveal.
   - When the user answers a quiz question correctly:
     - Play a short, cheery success bell sound effect that reinforces the success.
   - The sleigh bell and success bell sounds should:
     - Be crisp and pleasant.
     - Have no sudden, harsh peaks.
     - Be short so they do not interfere with screen reader announcements.
   - You may optionally vary sleigh bell or success bell variations or add subtle holiday ambience for different days, but:
     - A high quality sleigh bell sound must always be part of the door opening sequence.
     - A cheery success bell must always be part of the correct answer feedback, as long as sound effects are enabled.

2. User control and preferences
   - No audio auto plays on page load.
   - Add global sound controls that are easy to find and operate:
     - A mute or sound on off toggle.
     - A volume control slider.
     - An option like "Play sound effects when doors open and quizzes are correct" that the user can turn on or off.
   - Respect user preferences:
     - If CSS `prefers-reduced-motion` or similar is available, default to minimal animation and subtle audio.
   - Provide text labels and ARIA attributes for all audio controls.
   - Ensure that if sound is muted or sound effects are turned off:
     - Neither the sleigh bell nor the success bell nor any other effect plays.

3. Accessibility of audio
   - Ensure that sound effects:
     - Are short and do not interfere significantly with screen reader speech.
     - Do not contain sudden loud spikes.
   - Provide text equivalents by ensuring the tip and quiz content do not rely on audio to convey information.
   - If you implement any longer media, provide controls, captions, and transcripts.

4. Implementation details
   - Use the Web Audio or `<audio>` element with JavaScript.
   - Include placeholder file names for the audio assets, for example:
     - `creaky-door.mp3`
     - `sleigh-bell-high-quality.mp3`
     - `quiz-success-bell.mp3`
   - Explain in comments where the actual audio files should be placed and how they are loaded.
   - Ensure sound playback for both door opening and quiz success:
     - Checks the current sound settings before playing.
     - Does not block UI responsiveness.

==================================================
7. VISUAL DESIGN AND TARGET BRAND FEEL
==================================================

Give the app a Target inspired holiday theme while preserving accessibility.

1. Color and brand feel
   - Use red and white as key colors, but ensure:
     - All text and interactive elements meet WCAG AA contrast.
     - Do not rely on color alone to indicate opened state or quiz completion. Use icons, patterns, or labels.
   - Provide a high contrast toggle for users who need even stronger contrast.

2. Typography and spacing
   - Use a clean, legible sans serif stack such as:
     - `font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;`
   - Ensure sufficient line height and spacing so content is easily readable.

3. Motion and animation
   - Animate door opening in a subtle, low motion way.
   - Use prefers reduced motion media query:
     - If the user prefers reduced motion, either remove animations or use very gentle transitions.
   - Never rely on animation alone to convey information.
   - Avoid any strobing or flashing effects.

==================================================
8. CONTENT REQUIREMENTS FOR THE ACCESSIBILITY TIPS AND QUIZZES
==================================================

Create a data structure with one accessibility tip and associated quiz per day for December 1 to December 31.

1. Data model
   - Represent tips in an array of objects, for example:

     - `day` (number, 1 through 31)
     - `title` (short, catchy)
     - `summary` (one or two sentence overview)
     - `detail` (one or more paragraphs with practical guidance)
     - `category` (for example Design, Engineering, Content, Process, Assistive technology)
     - `targetContext` (short note tying this to a Target specific scenario, such as "Target app checkout flow" or "In store kiosk")
     - `quiz` object with fields such as:
       - `question` (a brief question that reinforces the main learning from the tip)
       - `type` (for example "single-choice")
       - `options` (array of two to four short answer choices)
       - `correctOptionIndex` (index of the correct option in the options array)
       - `explanation` (short explanation shown after answering, for example "Correct, consistent focus outlines help keyboard users know where they are on the page")

2. Quiz design
   - Keep quizzes short and focused, usually one question per day.
   - Favor formats such as:
     - Multiple choice with three or four options.
     - True or false.
   - Each quiz should:
     - Directly reinforce the specific tip for that day.
     - Use clear, simple language.
     - Avoid trick questions.
   - Make sure there is only one clearly correct answer per question.

3. Tip themes
   - Cover a mix of topics, such as:
     - Color contrast and visual hierarchy.
     - Keyboard accessibility and focus order.
     - Accessible forms and error messages.
     - Screen reader semantics and ARIA.
     - Alternative text for images and icons.
     - Accessible video and audio.
     - Handling dynamic content and announcements.
     - Respecting user preferences for motion and sound.
     - Mobile and touch accessibility.
     - Testing with assistive technology.
     - Inclusive language and content.
     - Accessible charts, tables, and data.
     - Authentication, one time codes, and captchas.
     - Patterns and components frequently used at Target, such as product cards, carousels, and navigation.

4. Tone and voice
   - Friendly, encouraging, and practical.
   - Avoid jargon, or explain it briefly if needed.
   - Assume the audience is Target team members such as engineers, designers, product managers, and content strategists.

==================================================
9. DATE HANDLING AND PROGRESS
==================================================

Implement behavior for December usage, but keep the experience usable at any time of year.

1. Date based behavior
   - Use the user’s local date to determine "today".
   - Provide an option to:
     - Lock future days until their date, and
     - Provide an alternate mode where all days are available for training or testing.
   - When a day is locked:
     - Clearly communicate that it is not yet available and why.

2. Progress and history
   - Indicate visually and programmatically which days have been opened.
   - Indicate which quizzes have been completed and whether they were answered correctly at least once.
   - Provide a section that lists all opened tips so far, with links or buttons to review both the tip and the quiz, including any feedback.

==================================================
10. ACCESSIBILITY TESTING AND CHECKLIST
==================================================

At the end of your answer, include:

1. A manual accessibility test plan with test cases such as:
   - Navigate the entire experience using keyboard only.
   - Use a screen reader such as NVDA with Chrome and VoiceOver with Safari to:
     - Navigate through headings and landmarks.
     - Reach and activate doors.
     - Open and close tips and dialogs.
     - Operate sound controls.
     - Navigate and answer quiz questions.
     - Read quiz feedback and explanations.
   - Verify that:
     - Sleigh bell sounds play only when a door opens and sound is enabled.
     - The cheery success bell plays only when a quiz answer is correct and sound is enabled.
   - Check color contrast values for key elements.
   - Test with prefers reduced motion set.
   - Test audio behavior with sound effects enabled and disabled.

2. A concise checklist that maps key features to WCAG 2.1 and 2.2 AA success criteria, for example:
   - Keyboard access.
   - Focus visible.
   - Enough contrast.
   - Non text content.
   - Name, role, and value for interactive components.
   - Pause, stop, hide for motion and audio, where relevant.
   - Status messages and error feedback for quizzes.

==================================================
11. OUTPUT FORMAT AND ORDER
==================================================

Follow this output order:

1. Brief summary of the solution approach.
2. Architecture and structural description.
3. JSON or JavaScript array for the 31 tips and quizzes with placeholder but realistic Target specific content.
4. Complete HTML markup.
5. Complete CSS.
6. Complete JavaScript.
7. Notes on where to place audio files and how to swap in real assets.
8. Manual accessibility test plan and WCAG mapping checklist.

Before you start writing code, ask me any clarifying questions you need about:
- Visual style preferences.
- Whether to use a single page app pattern or a simple page.
- How strict date locking should be.
- Whether to support multiple languages.
- Any constraints on hosting or integration with other Target systems.

Then, after I answer, generate the full implementation and explanations in a way that I can copy into files and run locally with a simple static server.
