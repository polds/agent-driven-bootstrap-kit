# TENETS

Non-negotiable principles for this project. Cite numbers in PR
descriptions and commit messages. When a change appears to violate a
tenet, surface it explicitly rather than smuggle it through.

Tenets grow as the project does. Add one when you find yourself
pushing back on the same kind of decision more than twice.

<!--
Template: the eight tenets below are starter generics that apply to
most software projects. Keep the ones that fit, swap the ones that
don't, add the project-specific ones from the bootstrap interview.
-->

## Starter tenets

1. **Code over model.** Push as much logic into deterministic code
   as possible. LLM calls, AI features, and pattern-matching are
   appropriate when the work is genuinely fuzzy; they're a tax when
   the work isn't.

2. **Small surfaces.** Prefer focused functions, small interfaces
   defined at the consumer, and concrete types. Don't pre-abstract.
   An interface with one implementation is a refactor away from
   being a struct.

3. **Observable process.** If you can't explain what a piece of
   code did and why, that's a bug. Structured logging, named
   stages, inspectable artifacts. Postmortems require evidence;
   leave evidence behind.

4. **No silent failures.** Every error has a structured place to
   land. `nil` return paths that hide a partial result are a bug.
   Either succeed cleanly or fail loudly.

5. **Tests are documentation.** A failing test should make the
   intent of the original change recoverable. Avoid tests that
   only assert "this implementation is the implementation."

6. **Decay is a feature.** State accumulates. Have explicit
   retention policies. Don't add storage without thinking through
   how it gets cleaned up.

7. **Reversibility is cheap.** Make changes that are easy to undo
   if you find out you were wrong. Schema migrations have a down.
   Feature flags wrap risky launches. Hardcoded behaviors are
   harder to retract than configured ones.

8. **One change per PR.** A bug fix doesn't carry refactoring with
   it. A feature doesn't carry a cleanup with it. If you find a
   second concern, open a second PR.

## Project-specific tenets

`<PLACEHOLDER: 3-5 tenets that came out of the bootstrap interview.
Examples might be:>`

- *9. **No network calls in unit tests.** Integration tests have a
  separate target.*
- *10. **PRs that change behavior must update the changelog.***
- *11. **Schema migrations land before the code that uses them.***

Renumber if you reorder. Don't reuse numbers after deletion — they
get cited in commits and PRs and the numbers should remain stable.