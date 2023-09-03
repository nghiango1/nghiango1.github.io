---
title: Binary search
share: true
catagory: "Basic"
tags:
    - computer_sience
---

# Problem

Given a tower that have `n` floor height
Given you two Crystal ball that break if being drop from `b` floor and above
Find `b`

> You only have two Crystal ball, so if you drop and breaking both, you can't no longer testing it again 

```python
n = 200 
b = 100
```

You are provided with `checkBreak(x)` function, that can't no longer be call after two `True` return

```python
class Problem():
    def __init__(self, n, b):
        self.n = n
        self.b = b
        self.count = 2
        self.check = 1

    def checkBreak(x):
        if x >= self.b:
            assert self.count > 0 
            self.count -= 1
        return x >= self.b 

    def checkResult(x):
        assert x == self.b
        return x == self.b
```

# Solve

## Jumping in sqrt()
(language:: python) (time:: O(sqrt n))

By the define of the problem:
- If we used up one ball (one call that `checkBreak(x)` return true), the next one we have to do a linear search so that we can find the exact `b` value.
- This mean, we have to find the balancing of how much time:
    - `Ball 2`: We spending on linear search to find the exact `b` value
    - `Ball 1`: We spending quickly jump between each floor, so we can reducing total time spending on `Ball 2`

This happen that we using sqrt(n) for jump step of `Ball 1` drop. 
- Because `n / sqrt(n) == sqrt(n)`, this mean we spending `sqrt(n)` time drop `Ball 1` as the worst case.
- After `Ball 1` break, we back to last know value that Ball 1 not break and start linear search from there, this mean we only need to spend on another `sqrt(n)` range of linear search time drop `Ball 2` as the worst case.

Making this, a rare `O(sqrt n)` time complexity solvable problem

```python
import math


class Problem():
    def __init__(self, n, b):
        self.n = n
        self.b = b
        self.count = 2
        self.check = 1

    def checkBreak(self, x):
        if x >= self.b:
            assert self.count > 0
            self.count -= 1
        return x >= self.b

    def checkResult(self, x):
        assert x == self.b
        return x == self.b


def solve(n, cb, cr):
    step = math.trunc(math.sqrt(n))
    curr = 0
    for i in range(0, n, step):
        if cb(i):
            break
        curr = i
    for i in range(curr, n):
        if cb(i):
            if not cr(i):
                break
            return i
    return -1


def test():
    test = [(55, 14), (422, 12), (441, 255), (245, 112), (12421441, 244124)]
    for n, b in test:
        problem = Problem(n, b)
        cb = problem.checkBreak
        cr = problem.checkResult
        res = solve(n, cb, cr)
        assert cr(res)

    print("Done")


if __name__ == "__main__":
    test()
```

## Typescript implement
(language:: typescript) (time:: O(sqrt n))

This instead take a different way of input (and testing using jest)

```ts
import two_crystal_balls from "@code/TwoCrystalBalls";

test("two crystal balls", function () {
    let idx = Math.floor(Math.random() * 10000);
    const data = new Array(10000).fill(false);

    for (let i = idx; i < 10000; ++i) {
        data[i] = true;
    }

    expect(two_crystal_balls(data)).toEqual(idx);
    expect(two_crystal_balls(new Array(821).fill(false))).toEqual(-1);
});
```

We doing the same thing here:

```ts
export default function two_crystal_balls(breaks: boolean[]): number {
    let step = Math.floor(Math.sqrt(breaks.length));
    let res = -1;
    let curr = 0;

    for (let i = 0; i < breaks.length; i += step) {
        if (breaks[i]) break;
        curr = i;
    }
    for (let i = curr; i < breaks.length; i++) {
        if (breaks[i]) {
            res = i;
            break;
        }
    }
    return res;

}
```

## C implementation
(language:: c) (time:: O(sqrt n))

I do try using `math.h`,  but it seem my library can't even link, so I instead use a implementation for integer sqrt (Newton's method, Wikipedia version). 

Here I use same input as the python version, where we are passed with check break `cb` function (that only return `True` two time and return `-1`  to mimic the dropping) and number `n` for building height.

```c
#include <stdio.h>

int b = 0;
int n = 0;
int count = 2;

// We return 0 if False, 1 if True, -1 which mean out of ball
// As boolean in C is just 0 for False and 1 for True
int checkBreak(int x){
    if (count == 0)
        return -1;
    count = count - (x >= b);
    return x >= b;
}

// Square root of integer
unsigned int intSqrt(unsigned int s)
{
	// Zero yields zero
    // One yields one
	if (s <= 1) 
		return s;

    // Initial estimate (must be too high)
	unsigned int x0 = s / 2;

	// Update
	unsigned int x1 = (x0 + s / x0) / 2;

	while (x1 < x0)	// Bound check
	{
		x0 = x1;
		x1 = (x0 + s / x0) / 2;
	}		
	return x0;
}

int solve(int n, int (* cb)(int) ) {
    int step = intSqrt(n);
    int i = 0;

    for (i = 0; i < n; i += step) {
        if (cb(i)) break;
    }

    for (i = i - step; i < n; i ++) {
        if (cb(i)) break;
    }

    return i;
}

int main() {
    int input_n[] = {53,163,1616,7747};
    int input_b[] = {12,13,1416,247};

    for (int i = 0; i < 4; i++) {
        count = 2;
        n = input_n[i];
        b = input_b[i];
        if (b == solve(n, checkBreak)) {
            printf("Test %d pass!\n", i);
        }
        else {
            count = 2;
            printf("What, %d != %d\n", b, solve(n, checkBreak));
        }
    }
    
    return 0;
}
```