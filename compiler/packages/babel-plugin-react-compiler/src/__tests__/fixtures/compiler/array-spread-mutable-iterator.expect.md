
## Input

```javascript
/**
 * TODO: object spreads should have conditionally mutate semantics
 *   Found differences in evaluator results
 *   Non-forget (expected):
 *   (kind: ok) [3,1,5,4]
 *   [3,1,5,4]
 *   [4,1,5,4]
 *   Forget:
 *   (kind: ok) [3,1,5,4]
 *   [3,1,5,4]
 *   [4]
 */

function useBar({arg}) {
  'use memo';

  /**
   * Note that mutableIterator is mutated by the later object spread. Therefore,
   * `s.values()` should be memoized within the same block as the object spread.
   * In terms of compiler internals, they should have the same reactive scope.
   */
  const s = new Set([1, 5, 4]);
  const mutableIterator = s.values();

  return [arg, ...mutableIterator];
}

export const FIXTURE_ENTRYPOINT = {
  fn: useBar,
  params: [{arg: 3}],
  sequentialRenders: [{arg: 3}, {arg: 3}, {arg: 4}],
};

```

## Code

```javascript
import { c as _c } from "react/compiler-runtime"; /**
 * TODO: object spreads should have conditionally mutate semantics
 *   Found differences in evaluator results
 *   Non-forget (expected):
 *   (kind: ok) [3,1,5,4]
 *   [3,1,5,4]
 *   [4,1,5,4]
 *   Forget:
 *   (kind: ok) [3,1,5,4]
 *   [3,1,5,4]
 *   [4]
 */

function useBar(t0) {
  "use memo";
  const $ = _c(2);
  const { arg } = t0;
  let t1;
  if ($[0] !== arg) {
    const s = new Set([1, 5, 4]);
    const mutableIterator = s.values();

    t1 = [arg, ...mutableIterator];
    $[0] = arg;
    $[1] = t1;
  } else {
    t1 = $[1];
  }
  return t1;
}

export const FIXTURE_ENTRYPOINT = {
  fn: useBar,
  params: [{ arg: 3 }],
  sequentialRenders: [{ arg: 3 }, { arg: 3 }, { arg: 4 }],
};

```
      
### Eval output
(kind: ok) [3,1,5,4]
[3,1,5,4]
[4,1,5,4]