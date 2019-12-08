% title: Better Python with types
% subtitle: Getting the most out of mypy
% author: Brian Zeligson
% author: ISC Tech Strategy

---
title: Why I love types


---
title: Static knowledge

Typechecker looks at code without running

Software runs many times, what you know before running, you know _every_ time it runs

How much can you really know before running?

Just like how much tests can do for you, depends on how you write your code.

---
title: Agenda

- Counting classes (analysis)
<pre class="prettyprint" data-lang="python">
class ThingA:
  def thingB(self, x: str) -> int ...
</pre>
- Reducing counts (practice)

---
title: Types are Sets
build_lists: true

- $str=\{, a, b, ..., aa, ab, ...\}$
- $int=\{..., -2, -1, 0, 1, 2, ...\}$
- $bool=\{true, false\}$

---
title: Example Types
subtitle: Two to Three

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

<pre class="prettyprint" data-lang="python">
from enum import Enum
TL = Enum('TL', 'red yellow green')
Dir = Enum('Dir', 'x y')
class Intersection:
  x_light: TL
  y_light: TL
  x_waiting: int
  y_waiting: int
  threshold: int
  bias: Dir

  def should_change(self: Intersection) -> bool: ...
  def change_light(self: Intersection) -> Intersection: ...
  def direct_traffic(self: Intersection, cars: int, dir: Dir) -> Intersection: ...
</pre>
---
title: Reducing Counts
subtitle: Making classes countable

<pre class="prettyprint" data-lang="python">
from enum import Enum
TL = Enum('TL', 'red yellow green')
Dir = Enum('Dir', 'x y')
class Intersection:
  x_light: TL
  y_light: TL
  x_waiting: int
  y_waiting: int
  threshold: int
  bias: Dir

def should_change(i: Intersection) -> bool: ...
def change_light(i: Intersection) -> Intersection: ...
def direct_traffic(i: Intersection, cars: int, dir: Dir) -> Intersection: ...
</pre>

---
title: Reducing counts
subtitle: Making classes smaller - valid states

Count pair of TLs. How many are valid?

<pre class="prettyprint" data-lang="python">
from enum import Enum
TL = Enum('TL', 'red yellow green')
Dir = Enum('Dir', 'x y')
class Intersection:
  x_light: TL
  y_light: TL
  x_waiting: int
  y_waiting: int
  threshold: int
  bias: Dir

def should_change(i: Intersection) -> bool: ...
def change_light(i: Intersection) -> Intersection: ...
def direct_traffic(i: Intersection, cars: int, dir: Dir) -> Intersection: ...
</pre>

---
title: Reducing counts
subtitle: Making classes smaller - valid states

Make invalid states unrepresentable

<pre class="prettyprint" data-lang="python">
from enum import Enum
Dir = Enum('Dir', 'x y')
class Intersection:
  moving_dir: Dir
  is_yellow: bool
  x_waiting: int
  y_waiting: int
  threshold: int
  bias: Dir

def should_change(i: Intersection) -> bool: ...
def change_light(i: Intersection) -> Intersection: ...
def direct_traffic(i: Intersection, cars: int, dir: Dir) -> Intersection: ...
</pre>

---
title: Reducing counts
subtitle: Identity 
build_lists: true

How many ways can we implement...

- <pre class="prettyprint" data-lang="python">
    def id(x: str) -> str: ...
  </pre>
- <pre class="prettyprint" data-lang="python">
    def id(x: int) -> int: ...
  </pre>
- <pre class="prettyprint" data-lang="python">
    def id(x: TL) -> TL: ...
  </pre>
- <pre class="prettyprint" data-lang="python">
    def id(x: A) -> A: ...
  </pre>

---
title: Reducing counts
subtitle: Eval 
build_lists: true

How many ways can we implement...

- <pre class="prettyprint" data-lang="python">
    def eval(x: str, f: Callable[[str], int]) -> int: ...
  </pre>
- <pre class="prettyprint" data-lang="python">
    def eval(x: int, f: Callable[[int], str]) -> str: ...
  </pre>
- <pre class="prettyprint" data-lang="python">
    def eval(x: TL, f: Callable[[TL], bool]) -> bool: ...
  </pre>
- <pre class="prettyprint" data-lang="python">
    def eval(x: A, f: Callable[[A], B]) -> B: ...
  </pre>

---
title: Reducing counts
subtitle: Making classes smaller - Parametricity

<pre class="prettyprint" data-lang="python">
from enum import Enum
from typing import Generic, TypeVar
A = TypeVar('A')
Dir = Enum('Dir', 'x y')
class Intersection(Generic[A]):
  moving_dir: Dir
  is_yellow: bool 
  x_waiting: A
  y_waiting: A
  threshold: A
  bias: Dir 

def should_change(i: Intersection[A]) -> bool: ...
def change_light(i: Intersection[A]) -> Intersection[A]: ...
def direct_traffic(i: Intersection[int], cars: int, dir: Dir) -> Intersection[int]: ...
</pre>

---
title: Closing thoughts 

If you can make the same progress spending 2 minutes thinking and 2 hours coding,

that you can spending 2 hours thinking and 2 minutes coding,

take the latter.
