# Test genres

Adjust this list to the project after reading a sample. Check the keep rules first: they win over every genre.

## Keep

- Touches money or credits, auth and permissions, moderation, or data loss.
- Checks an action: request sent, component event emitted, row written, navigation, refusal, with a branch.
- Protects a calculation or a decision that changes what the user gets: cost, generation parameters, routing.
- Guards a real fixed bug, named by the test or its comment, on behavior rather than display.
- Holds a contract no type checks, between two services or two copies of a value, including a field of an emitted analytics event.
- Pure interface logic with a branch: timers, gestures, scroll. It can break silently.
- Is named by a registry, a coverage check or a catalog, or is such a guard.

## Delete

| Genre | What it looks like | Why it goes |
| --- | --- | --- |
| Source grep | Reads a source file or template and looks for a string | Passes while the feature is broken, fails on a refactor |
| Admin render | Renders an internal tool page and looks for HTML | Its one user sees a break at once |
| Front display | The only outcome checked is a text, class, attribute, initial state or element presence | Fails on design decisions |
| Cosmetic format | Display formatting, truncation, shortened name, display sort, date | Low damage, and visible |
| Trivial empty case | `None` or empty returns empty or a default, in a helper with no consequence | The branch is the test's only reader |
| Trivial lookup | Accessor, mapping, default value, pass-through | Restates the code |
| Registry shape | The form of a registry or a naming convention | Polices code, catches no bug. Mind the keep rule on guards |
| Absent telemetry, flags | An event is not sent, or a telemetry or log flag | Rarely fails for a real reason |
| Design e2e | End-to-end check of a visual constraint | Slow and brittle, with no bug behind it |
| Prompt wording | Looks for a word in a prompt or instruction, with no branch | Fails on every copy edit |
| Copy of a constant | Restates a value, a price table, a schema or a config | Only a deliberate change can break it |
| Framework | Proves the framework or a library works | Not our code |
| Redundant | Same branch and outcome as another test, opened and named | Pays twice |
| No real assertion | Nothing to fail, or always true | Reads as coverage, proves nothing |
| Dead | `skip` or `fixme` with no condition and no date. A skip tied to a variable or a feature's return is alive | Never runs |

## Worth rescuing by hand

Rules miss these, so look for them when auditing:

- SEO and crawler configuration checked by reading a template. A grep can be the only protection a server config has.
- Analytics counting rules, such as the first page load not counting as a visit.
- A flag that turns error reporting on in production.
- A registration order a security or moderation route depends on.
