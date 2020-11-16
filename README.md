# <code>super-random.js</code>

> Seedable random generator supporting many common distributions. Random numbers and string generation.

Welcome to the most **random** module on npm!

## Installation

```bash
npm install --save super-random.js
```

## Usage

```js
const random = require('super-random.js')

// quick uniform shortcuts
random.float(min = 0, max = 1) // uniform float in [ min, max )
random.int(min = 0, max = 1) // uniform integer in [ min, max ]
random.boolean() // true or false

// uniform
random.uniform(min = 0, max = 1) // () => [ min, max )
random.uniformInt(min = 0, max = 1) // () => [ min, max ]
random.uniformBoolean() // () => [ false, true ]

// normal
random.normal(mu = 0, sigma = 1)
random.logNormal(mu = 0, sigma = 1)

// bernoulli
random.bernoulli(p = 0.5)
random.binomial(n = 1, p = 0.5)
random.geometric(p = 0.5)

// poisson
random.poisson(lambda = 1)
random.exponential(lambda = 1)

// misc
random.irwinHall(n)
random.bates(n)
random.pareto(alpha)
```

For convenience, several common uniform samplers are exposed directly:

```js
random.float()     // 0.2149383367670885
random.int(0, 100) // 72
random.boolean()   // true
```

**All distribution methods return a thunk** (function with no params), which will return
a series of independent, identically distributed random variables from the specified distribution.

```js
// create a normal distribution with default params (mu=1 and sigma=0)
const normal = random.normal()
normal() // 0.4855465422678824
normal() // -0.06696771815439678
normal() // 0.7350852689834705

// create a poisson distribution with default params (lambda=1)
const poisson = random.poisson()
poisson() // 0
poisson() // 4
poisson() // 1
```

Note that returning a thunk here is more efficient when generating multiple
samples from the same distribution.

You can change the underlying PRNG or its seed as follows:

```js
const seedrandom = require('seedrandom')

// change the underlying pseudo random number generator
// by default, Math.random is used as the underlying PRNG
random.use(seedrandom('foobar'))

// create a new independent random number generator (uses seedrandom under the hood)
const rng = random.clone('my-new-seed')

// create a second independent random number generator and use a seeded PRNG
const rng2 = random.clone(seedrandom('kittyfoo'))

// replace Math.random with rng.uniform
rng.patch()

// restore original Math.random
rng.unpatch()
```

## API

#### Table of Contents

-   [Random](#random)
    -   [rng](#rng)
    -   [clone](#clone)
    -   [use](#use)
    -   [patch](#patch)
    -   [unpatch](#unpatch)
    -   [next](#next)
    -   [float](#float)
    -   [int](#int)
    -   [integer](#integer)
    -   [bool](#bool)
    -   [boolean](#boolean)
    -   [uniform](#uniform)
    -   [uniformInt](#uniformint)
    -   [uniformBoolean](#uniformboolean)
    -   [normal](#normal)
    -   [logNormal](#lognormal)
    -   [bernoulli](#bernoulli)
    -   [binomial](#binomial)
    -   [geometric](#geometric)
    -   [poisson](#poisson)
    -   [exponential](#exponential)
    -   [irwinHall](#irwinhall)
    -   [bates](#bates)
    -   [pareto](#pareto)

### Random

Seedable random number generator supporting many common distributions.

Defaults to Math.random as its underlying pseudorandom number generator.

Type: `function (rng)`

-   `rng` **(RNG | [function](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Statements/function))** Underlying pseudorandom number generator. (optional, default `Math.random`)

* * *

#### rng

Type: `function ()`

* * *

#### clone

-   **See: RNG.clone**

Creates a new `Random` instance, optionally specifying parameters to
set a new seed.

Type: `function (args, seed, opts): Random`

-   `args` **...any**
-   `seed` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)?** Optional seed for new RNG.
-   `opts` **[object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)?** Optional config for new RNG options.

* * *

#### use

Sets the underlying pseudorandom number generator used via
either an instance of `seedrandom`, a custom instance of RNG
(for PRNG plugins), or a string specifying the PRNG to use
along with an optional `seed` and `opts` to initialize the
RNG.

Type: `function (args)`

-   `args` **...any**

Example:

```javascript
const random = require('super-random.js')

random.use('example_seedrandom_string')
// or
random.use(seedrandom('kittens'))
// or
random.use(Math.random)
```

* * *

#### patch

Patches `Math.random` with this Random instance's PRNG.

Type: `function ()`

* * *

#### unpatch

Restores a previously patched `Math.random` to its original value.

Type: `function ()`

* * *

#### next

Convenience wrapper around `this.rng.next()`

Returns a floating point number in \[0, 1).

Type: `function (): number`

* * *

#### float

Samples a uniform random floating point number, optionally specifying
lower and upper bounds.

Convence wrapper around `random.uniform()`

Type: `function (min, max): number`

-   `min` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** Lower bound (float, inclusive) (optional, default `0`)
-   `max` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** Upper bound (float, exclusive) (optional, default `1`)

* * *

#### int

Samples a uniform random integer, optionally specifying lower and upper
bounds.

Convence wrapper around `random.uniformInt()`

Type: `function (min, max): number`

-   `min` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** Lower bound (integer, inclusive) (optional, default `0`)
-   `max` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** Upper bound (integer, inclusive) (optional, default `1`)

* * *

#### integer

Samples a uniform random integer, optionally specifying lower and upper
bounds.

Convence wrapper around `random.uniformInt()`

Type: `function (min, max): number`

-   `min` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** Lower bound (integer, inclusive) (optional, default `0`)
-   `max` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** Upper bound (integer, inclusive) (optional, default `1`)

* * *

#### bool

Samples a uniform random boolean value.

Convence wrapper around `random.uniformBoolean()`

Type: `function (): boolean`

* * *

#### boolean

Samples a uniform random boolean value.

Convence wrapper around `random.uniformBoolean()`

Type: `function (): boolean`

* * *

#### uniform

Generates a [Continuous uniform distribution](https://en.wikipedia.org/wiki/Uniform_distribution_(continuous)).

Type: `function (min, max): function`

-   `min` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** Lower bound (float, inclusive) (optional, default `0`)
-   `max` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** Upper bound (float, exclusive) (optional, default `1`)

* * *

#### uniformInt

Generates a [Discrete uniform distribution](https://en.wikipedia.org/wiki/Discrete_uniform_distribution).

Type: `function (min, max): function`

-   `min` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** Lower bound (integer, inclusive) (optional, default `0`)
-   `max` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** Upper bound (integer, inclusive) (optional, default `1`)

* * *

#### uniformBoolean

Generates a [Discrete uniform distribution](https://en.wikipedia.org/wiki/Discrete_uniform_distribution),
with two possible outcomes, `true` or \`false.

This method is analogous to flipping a coin.

Type: `function (): function`

* * *

#### normal

Generates a [Normal distribution](https://en.wikipedia.org/wiki/Normal_distribution).

Type: `function (mu, sigma): function`

-   `mu` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** Mean (optional, default `0`)
-   `sigma` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** Standard deviation (optional, default `1`)

* * *

#### logNormal

Generates a [Log-normal distribution](https://en.wikipedia.org/wiki/Log-normal_distribution).

Type: `function (mu, sigma): function`

-   `mu` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** Mean of underlying normal distribution (optional, default `0`)
-   `sigma` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** Standard deviation of underlying normal distribution (optional, default `1`)

* * *

#### bernoulli

Generates a [Bernoulli distribution](https://en.wikipedia.org/wiki/Bernoulli_distribution).

Type: `function (p): function`

-   `p` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** Success probability of each trial. (optional, default `0.5`)

* * *

#### binomial

Generates a [Binomial distribution](https://en.wikipedia.org/wiki/Binomial_distribution).

Type: `function (n, p): function`

-   `n` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** Number of trials. (optional, default `1`)
-   `p` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** Success probability of each trial. (optional, default `0.5`)

* * *

#### geometric

Generates a [Geometric distribution](https://en.wikipedia.org/wiki/Geometric_distribution).

Type: `function (p): function`

-   `p` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** Success probability of each trial. (optional, default `0.5`)

* * *

#### poisson

Generates a [Poisson distribution](https://en.wikipedia.org/wiki/Poisson_distribution).

Type: `function (lambda): function`

-   `lambda` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** Mean (lambda > 0) (optional, default `1`)

* * *

#### exponential

Generates an [Exponential distribution](https://en.wikipedia.org/wiki/Exponential_distribution).

Type: `function (lambda): function`

-   `lambda` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** Inverse mean (lambda > 0) (optional, default `1`)

* * *

#### irwinHall

Generates an [Irwin Hall distribution](https://en.wikipedia.org/wiki/Irwin%E2%80%93Hall_distribution).

Type: `function (n): function`

-   `n` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** Number of uniform samples to sum (n >= 0) (optional, default `1`)

* * *

#### bates

Generates a [Bates distribution](https://en.wikipedia.org/wiki/Bates_distribution).

Type: `function (n): function`

-   `n` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** Number of uniform samples to average (n >= 1) (optional, default `1`)

* * *

#### pareto

Generates a [Pareto distribution](https://en.wikipedia.org/wiki/Pareto_distribution).

Type: `function (alpha): function`

-   `alpha` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** Alpha (optional, default `1`)
