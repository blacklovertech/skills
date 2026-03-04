# States Reference — Loading, Empty, Error & Success

Every interactive artifact lives in multiple states. A page that only designs
the "happy path" — data loaded, no errors, everything perfect — fails the moment
anything else happens. Real users encounter loading delays, empty data, form
errors, and success confirmations. Each of these states needs a design, not a
developer placeholder.

States are where UI trust is built or broken. A well-designed error message
turns a frustrating moment into a controlled experience. A skeleton loading
screen makes a slow API feel faster. An empty state with a clear call to action
turns a dead end into a starting point.

---

## Loading States

### Skeleton screens (preferred over spinners for content)

A skeleton screen shows the shape of the content before it loads — placeholder
blocks in the position and approximate size of real content. It sets expectations
and reduces perceived wait time because users can begin to parse the layout
before the content arrives.

**How to build a skeleton:**
Replace content elements with same-size blocks using the border color as fill.
A headline skeleton is a rounded rectangle at the same height as the text
(approximately 1em tall) and 60–80% of the expected text width. Body text
skeletons are full-width, stacked with the line-height gap between them, with
the last line approximately 60% width to suggest a natural paragraph end.
Card thumbnail skeletons fill the full container at the same aspect ratio as
the real image.

Apply a shimmer animation — a left-to-right gradient sweep that moves across
the skeleton. This is achieved with an animated `background-position` on a
linear gradient running from the border color through a slightly lighter
midpoint and back. Duration 1.5s, ease-in-out, infinite. The shimmer suggests
activity and communicates "this is loading, not broken."

**Skeleton fidelity:** Match the skeleton layout precisely to the real content
layout. A skeleton that looks completely different from the loaded state is
worse than no skeleton — it causes a jarring layout shift when content arrives.

### Spinners (use for actions, not page loads)

Spinners communicate that something is happening in response to a user action —
a form submit, a button click, a background process. They are appropriate
for discrete operations, not for initial page or content loads.

A spinner in the context of this skill: a circular stroke, 20–24px diameter,
2px stroke width, in the accent color. The stroke is 75% of the circle
circumference, rotating 360° in 0.75s with a linear timing function. This is
cleaner than the classic dashed-circle spinner.

Place spinners inside buttons (replacing the button label when the action is
in-flight), inside small containers where a skeleton would be too complex,
or as a centered overlay on a panel that is refreshing.

**Button loading state:** When a form is submitted or a primary action fires,
the button transitions to a loading state: the label is replaced by a small
spinner, the button is disabled, and its opacity drops slightly (to 80%).
The button maintains its size so the layout does not shift. When the operation
completes, the button either returns to its normal state or transitions to a
success state (see below).

---

## Empty States

An empty state occurs when a section, list, or data display has no content to
show — a portfolio with no projects yet, a search with no results, a dashboard
with no activity.

A raw empty state (an empty container with nothing in it, or the word "None")
is a design failure. Every empty state must have three things:

**1. An illustration or icon**
A simple SVG illustration or a large (48–64px) icon that represents the type
of content that belongs here. Use the muted color at 40% opacity. Not a stock
illustration — a simple, purposeful shape. For a project gallery: a grid of
squares outline. For a message list: a speech bubble outline. For a data chart:
a simple bar chart outline with a question mark.

**2. A headline**
Short, specific, and framed as possibility rather than absence. Not "No projects
found" — instead "Your projects live here" or "Start by adding your first
project." The headline uses H3 size, primary text color.

**3. A call to action**
What should the user do to fill this state? A primary button with a specific
action label ("Add project", "Create your first post", "Connect your account").
Use the standard primary button style. If the empty state is due to a filter
or search with no results, add a secondary action to clear the filter.

**Empty state layout:** Center-aligned, within the container where content
would appear. Top and bottom padding of 3xl (9rem) if the container is the
primary page content. If it's a card or panel, xl (4rem) padding.

**Search no-results:** A specific variant — the user searched for something
and found nothing. The illustration suggests "searching" (magnifying glass).
The headline includes the search query: "No results for 'brandmark templates'."
The action: "Clear search" or "Try a different term." Do not recommend
"Check your spelling" — it's condescending.

---

## Error States

Error states tell the user that something went wrong and what to do about it.
They should be precise, calm, and actionable. Never blame the user.

### Inline field errors (form validation)

The error appears directly below the relevant field — not in a banner at the
top of the form, not as a modal, not as a toast. Proximity to the field is
what makes inline errors usable.

Visual treatment: the field border changes to the error color (a desaturated
red that fits the palette — for Crisp: `#DC2626`, for Studio: `#EF4444` toned
down, for Warm: `#B91C1C`). The error message appears below in the small type
size using the same error color. The field label optionally changes to the
error color too. An error icon (a circle with an exclamation mark) precedes
the message text.

Error message copy rules: specific and corrective, never generic. "Please
enter a valid email address" — good. "Invalid input" — bad. "Email addresses
look like name@example.com" — better, because it shows rather than tells.
Never write "Error: [field] is required" — write "Please enter your [field]."

### Page-level errors (network, server, auth)

When an action fails at the system level (API error, network timeout, server
error), display a banner at the top of the affected section. The banner uses
a soft error-tinted background (error color at 10% opacity), the error color
as border-left (4px), and contains an icon, a headline, a detail message, and
a retry or dismiss action.

The headline: "Something went wrong" is acceptable but vague. Prefer specific:
"Couldn't save your changes" / "Unable to load projects" / "Connection lost."
The detail message explains what to do: "Your changes weren't saved. Try
again, or refresh the page if the problem continues."

### 404 and error pages

For artifacts that have multiple views, the 404 or error page uses the full
page layout with the same nav and footer. Center-aligned: a large, decorative
numeral ("404") in the accent color at an enormous size (8–12rem, the heading
font, weight 700), functioning as a piece of graphic design rather than
informational text. Below it: a plain headline ("Page not found"), one sentence
of context, and a button back to the home view.

Do not use stock error imagery (broken robot, confused astronaut, crying
computer). The typographic 404 is cleaner, more on-brand, and more readable.

---

## Success States

Success states confirm that an action completed. They should be proportionate
to the significance of the action — a minor success doesn't warrant a modal
celebration, a major milestone might.

### Toast notifications (for most successes)

A toast is a temporary notification that appears in a corner of the screen
(bottom-right or top-right), delivers its message, and disappears after 4–5
seconds. It should also have a dismiss button.

Visual treatment: the surface color as background, a 1px border in the border
color, Shadow 4 (float elevation), 4px border-left in the success color (a
desaturated green: `#16A34A` for Crisp/Warm, `#22C55E` toned for Studio).
An icon on the left (a circle with a checkmark), the message as body text,
and an X button to dismiss.

The message: specific about what succeeded. "Changes saved" — good. "Success!"
— bad. "Project published to your portfolio" — better.

### Inline confirmations (for small, frequent actions)

When a user saves a setting, follows a person, or likes a post, the confirmation
happens in-place — the button label changes ("Saved ✓"), the icon fills in, or
the element briefly highlights in the accent color then settles. Duration:
300ms transition in, hold 1–2 seconds, transition back or stay in the confirmed
state. No full-page feedback needed.

### Success screens (for significant actions)

Form submission, account creation, purchase completion — these warrant a
dedicated success view or section. Replace the form area with a centered
confirmation: a large success icon (the circle checkmark in the accent color
at 64px), a headline ("You're in" / "Project submitted" / "All done"), a
specific detail sentence, and the logical next action button.

Do not show confetti, animated fireworks, or celebratory animations unless
the context genuinely calls for celebration (a first purchase, a launch day,
a completed onboarding). Overuse of celebration animations erodes their impact
and can feel manipulative when applied to routine actions.

---

## Disabled States

Elements that are disabled (cannot currently be interacted with) must communicate
that clearly — not just visually, but in a way that explains why.

Visual treatment: opacity reduced to 40–50%. Cursor changes to `not-allowed`.
No hover effects. For buttons: the accent fill becomes the border color fill —
the element retains its shape but loses its color and energy.

When possible, add a tooltip on hover over a disabled element that explains
why it is disabled and what the user needs to do to enable it. "Connect your
account to export" is more useful than a greyed-out button with no explanation.

Do not use color alone to communicate disabled — the opacity reduction is what
communicates it, not a color change. This matters for color-blind users.
