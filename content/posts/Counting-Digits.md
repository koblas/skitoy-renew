---
title: Counting Digits
date: '2023-04-18T04:00:00.000Z'
categories:
  - Development
tags:
  - golang
---

One of blogs that I enjoy reading is The Daily WTF. Recently there was this article that highlighted some weird [Geoinformatics WTF](https://thedailywtf.com/articles/projection-your-failures "Geoinformatics WTF"). What was interesting was that the comments on this post revolved around the performance of counting digits. Should somebody use Math.Log10 or do some basic counting.

```javascript
var digits = 0;
while (num >= 1) {
  digits++;
  num /= 10;
}
```

I had to wonder, how many digits before the two approaches really differ.  Since benchmarking in Go is so much easier the quick test showed that they cross around 10 digits in length.

```
BenchmarkMatrix / Log2 - 10                        225116233       5.453 ns / op
BenchmarkMatrix / Cnt2 - 10                       1000000000       0.993 ns / op
BenchmarkMatrix / Log2000 - 10                     231034142       5.212 ns / op
BenchmarkMatrix / Cnt2000 - 10                     462714898       2.549 ns / op
BenchmarkMatrix / Log2000000 - 10                  231727612       5.189 ns / op
BenchmarkMatrix / Cnt2000000 - 10                  333076740       3.620 ns / op
BenchmarkMatrix / Log2000000000 - 10               227900226       5.160 ns / op
BenchmarkMatrix / Cnt2000000000 - 10               217574472       5.470 ns / op
BenchmarkMatrix / Log2000000000000 - 10            227399866       5.184 ns / op
BenchmarkMatrix / Cnt2000000000000 - 10            158506080       7.632 ns / op
BenchmarkMatrix / Log2000000000000000 - 10         232024206       5.186 ns / op
BenchmarkMatrix / Cnt2000000000000000 - 10         100000000      10.06  ns / op
BenchmarkMatrix / Log2000000000000000000 - 10      232336196       5.175 ns / op
BenchmarkMatrix / Cnt2000000000000000000 - 10       94450698      12.48  ns / op

```

Sidebar about using floating point for counting digits, this is much harder than it looks. For example
in go (floating point) you  end up with `math.Log10(1_000_000_000_000_010)` = `15.000000000000005` and
`math.Log10(1_000_000_000_000_000)` = `14.999999999999998`. In JavaScript you get something else 
`Math.log10(1_000_000_000_000_000)` = `15` and `Math.log10(  999_999_999_999_999)` = `15`. So in general
using floating point Log10 is error prone.


The source code for reference

digits.go

```go
package digits

import (
	"math"
)

func Log(num int) int {
	return int(math.Log10(float64(num)) + 1)
}

func Count(num int) int {
	digits := 0

	for ; num >= 1; num /= 10 {
		digits++
	}

	return digits
}
```

digits\_test.go

```go
package digits

import (
	"strconv"
	"testing"
)

func IntPow(base, exp int) int {
	result := 1
	for {
		if exp&1 == 1 {
			result *= base
		}
		exp >>= 1
		if exp == 0 {
			break
		}
		base *= base
	}

	return result
}

var testValues = []int{
	2,
	2_000,
	2_000_000,
	2_000_000_000,
	2_000_000_000_000,
	// 1_000_000_000_000_000, // rounding error Math.Log10 returns 14.999999999999998
	2_000_000_000_000_000,
	2_000_000_000_000_000_000,
}

func TestBase(t *testing.T) {
	for _, val := range testValues {
		numL := Log(val)
		numC := Count(val)
		if numL != numC {
			t.Errorf("Log(%d)[%d] != Count(%d)[%d]", val, numL, val, numC)
		}
	}
}

func BenchmarkMatrix(b *testing.B) {
	for _, val := range testValues {
		b.Run("Log"+strconv.Itoa(val), func(b *testing.B) {
			for n := 0; n < b.N; n++ {
				Log(val)
			}
		})
		b.Run("Count"+strconv.Itoa(val), func(b *testing.B) {
			for n := 0; n < b.N; n++ {
				Count(val)
			}
		})
	}
}
```

