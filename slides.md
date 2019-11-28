% title: Better Python with types
% subtitle: Getting the most out of mypy
% author: Brian Zeligson
% author: ISC Tech Strategy
% thankyou: Thanks everyone!

---
title: Agenda

- Motivation
- Types as Sets
- Tuples
- Functions
- Classes
- Shrinking Sets, forgetting on purpose
- Examples, how many ways can we implement this?
- There's more - Addition? Algebra? Factorization and Reduction?

---
title: Static knowledge

Typechecker looks at code without running

Software runs many times, what you know before running, you know _every_ time it runs

How much can you really know before running?

---
title: Types are Sets
build_lists: true

- $str=\{, a, b, ..., aa, ab, ...\}$
- $int=\{..., -2, -1, 0, 1, 2, ...\}$
- $bool=\{true, false\}$
- $void=\{\}$
- $unit=\{0\}$

---
title: Example Types
subtitle: Zero to Three

- $void=\{\}$
- $unit=\{0\}$
- $bool=\{true, false\}$
- $tl=\{red, yellow, green\}$

---
title: Composite Types
subtitle: Tuple
build_lists: true

$(bool, bool)$

- $(true, true)$
- $(true, false)$
- $(false, true)$
- $(false, false)$

---
title: Composite Types
subtitle: Tuple

$(bool, tl)$

---
title: Composite Types
subtitle: Tuple

$(bool, tl)$

- $(true, red)$
- $(true, green)$
- $(true, yellow)$
- $(false, red)$
- $(false, green)$
- $(false, yellow)$

---
title: Composite Types
subtitle: Tuple

- $(bool, bool) = (2, 2) = 4$
- $(bool, tl) = (2, 3) = 6$
- $(A, B) = A * B$

---
title: Composite Types
subtitle: Tuple = Product 

- $(A, B) = A * B$

---
title: Composite Types
subtitle: Product
build_lists: true

$(unit, tl)$

- $(0, red)$
- $(0, yellow)$
- $(0, green)$

---
title: Composite Types
subtitle: Product

$(void, tl)$

---
title: Composite Types
subtitle: Function
build_lists: true

- Input set $A$
- Output set $B$

---
title: Composite Types
subtitle: Function

- Input set $A$
- Output set $B$
- For each element $a$ in $A$, picks one element $b$ in $B$

<image src="/330px-Codomain2.SVG.png" />

---
title: Composite Types
subtitle: Function 
build_lists: true

$tl \rightarrow bool$

- For each element $l$ in $tl$, pick one element $b$ in $bool$
- ex: $\{(red, false), (yellow, true), (green, true)\}$
- ex: $\{(red, false), (yellow, false), (green, true)\}$
- $\{(false, true, true), (false, false, true)\}$
- $(tl \rightarrow bool) = (bool, bool, bool) = 2 * 2 * 2$

---
title: Composite Types
subtitle: Function 
build_lists: true

$bool \rightarrow tl$

- For each element $b$ in $bool$, pick one element $l$ in $tl$
- ex: $\{(false, red), (true, yellow)\}$
- ex: $\{(false, yellow), (true, green)\}$
- $\{(red, yellow), (yellow, green)\}$
- $(bool \rightarrow tl) = (tl, tl) = 3 * 3$

---
title: Composite Types
subtitle: Function

- $(tl \rightarrow bool) = (bool, bool, bool) = (2 * 2 * 2) = 8$
- $(bool \rightarrow tl) = (tl, tl) = (3 * 3) = 9$
- $(A \rightarrow B) = B ^ A$

---
title: Composite Types
subtitle: Function = Exponent

- $(A \rightarrow B) = B ^ A$

---
title: Composite Types
subtitle: Classes


---
title: Code examples
subtitle: (Mono | Poly)morphism

- Id
- Modus Ponens 
- Semigroupal append
- Composition
- Functoriality (use class from class counting)

---
title: When to use this

Are you implementing or defining?

- Call existing function with different value in a new place? Implementing
- Call many existing functions in a new place? Likely defining
- Add new required parameter to function called ubiquitously? Defining

Implementation (almost) always follows immediately from definition.
Define as much as possible to be as generic as possible, and implementation
is often just a few lines of code, a few minutes of time, and working
correctly from the very first run.

---
title: Bonus examples

Actual code, Datadog instrumentation and/or declarative connection
Lexical scope in classes, adjusted algebra

---
title: Maybe some code?

press 'h' to highlight an important section (that is highlighted
with &lt;b&gt;...&lt;/b&gt; tags)

<pre class="prettyprint" data-lang="javascript">
function isSmall() {
  return window.matchMedia("(min-device-width: ???)").matches;
}

<b>function hasTouch() {
  return Modernizr.touch;
}</b>

function detectFormFactor() {
  var device = DESKTOP;
  if (hasTouch()) {
    device = isSmall() ? PHONE : TABLET;
  }
  return device;
}
</pre>

