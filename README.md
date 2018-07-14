<p align="center">
    <a href="#"><img src="./logo/refract-logo-colour.png" height="70" /></a>
</p><br/>

<p align="center">
    Master your React app's effects through the<br/>
    power of reactive programming.
</p>
<br/>

<p align="center">
    <a href="#why"><strong>Why?</strong></a> ·
    <a href="#installation"><strong>Install</strong></a> ·
    <a href="#the-gist"><strong>The gist</strong></a> ·
    <a href="#learn-refract"><strong>Learn</strong></a> ·
    <a href="#contributing"><strong>Contribute</strong></a> ·
    <a href="#discuss"><strong>Discuss</strong></a>
</p>
<br/>

<p align="center">
    <img src="https://img.shields.io/bundlephobia/minzip/refract-rxjs.svg" alt="Library size">
    <a href="https://opensource.org/licenses/MIT">
        <img src="https://img.shields.io/badge/License-MIT-blue.svg" alt="MIT License">
    </a>
    <a href="https://github.com/prettier/prettier">
        <img src="https://img.shields.io/badge/styled_with-prettier-ff69b4.svg" alt="Styled with Prettier">
    </a>
</p>
<br/>

Refract lets you isolate your app's side effects - API calls, analytics, logging, etc - so that you can write your code in a clear, pure, and declarative fashion by using reactive programming.

Refract is an extensible library built for React. In addition we provide a Redux integration, which can also serve as a template for integrations with other libraries.

# Why?

Component-based architecture and functional programming have become an increasingly popular approach for building UIs. They help make apps more predictable, more testable, and more maintainable.

However, our apps don't exist in a vacuum! They need to make network requests, handle data persistence, log analytics, deal with changing time, and so on. Any non-trivial app has to handle any number of these external effects.

These side-effects hold us back from writing fully declarative code. Wouldn't it be nice to cleanly separate them from our apps?

Refract solves this problem for you. [For an in-depth introduction, head to `Why Refract`.](./docs/introduction/why-refract.md)

# Installation

Refract is available for a number of reactive programming libraries. For each library, a Refract integration is available for both React and Redux.

Available packages:

<!-- prettier-ignore-start -->
| | [React](https://github.com/facebook/react) | [Redux](https://github.com/reduxjs/redux) |
| --- | --- | --- |
| **[Callbag](https://github.com/callbag/callbag)** | refract-callbag | refract-redux-callbag |
| **[Most](https://github.com/cujojs/most)** | refract-most | refract-redux-most |
| **[RxJS](https://github.com/reactivex/rxjs)** | refract-rxjs | refract-redux-rxjs |
| **[xstream](https://github.com/staltz/xstream)** | refract-xstream | refract-redux-xstream |
<!-- prettier-ignore-end -->

To use the latest stable version, simply `npm install` the package you want to use:

```
npm install --save refract-rxjs
```

# The Gist

The example below uses `refract-rxjs` to send data to localstorage.

Every time the `username` prop changes, its new value is sent into the stream. The stream debounces the input for two seconds, then maps it into an object (with a `type` of `localstorage`) under the key `payload`. Each time an effect is emitted from this pipeline, the handler calls `localstorage.setItem` with the effect's `payload` property.

```js
const aperture = initialProps => component => {
    return component.observe('username').pipe(
        debounce(2000),
        map(username => ({
            type: 'localstorage',
            name: 'username',
            value: username
        }))
    )
}

const handler = initialProps => effect => {
    switch (effect.type) {
        case 'localstorage':
            localstorage.setItem(effect.name, effect.value)
            return
    }
}

const WrappedComponent = withEffects(handler)(aperture)(BaseComponent)
```

### Aperture

An `aperture` controls the streams of data entering Refract. It is a function which observes data sources within your app, passes this data through any necessary logic flows, and outputs a stream of `effect`s.

Signature: `(initialProps) => (component) => { return effectStream }`.

*   The `initialProps` are all props passed into the `WrappedComponent`.
*   The `component` is an object which lets you observe your React component.
*   Within the body of the function, you observe the event source you choose, pipe the events through your stream library of choice, and return a single stream of effects.

### Handler

A `handler` is a function which causes side-effects in response to any `effect` object output by the `aperture`.

Signature: `(initialProps) => (effect) => { /* handle effects here */ }`.

*   The `initialProps` are all props passed into the `WrappedComponent`.
*   The `effect` is each event emitted by your `aperture`.
*   Within the body of the function, you call any side-effects imperatively.

### withEffects

The `withEffects` higher-order component implements your side-effect logic as a React component.

Signature: `(handler) => (aperture) => (Component) => { return WrappedComponent }`

*   The hoc takes in three curried arguments:
    *   A `handler` function
    *   An `aperture` function
    *   A React `Component`
*   The hoc returns a `WrappedComponent` - an enhanced version of your original `Component` which includes your side-effect logic.

# Learn Refract

_Links through to tutorial._

# Documentation

_Links through to docs sub-pages._

# Examples

# Contributing

### Guidelines

We welcome many forms of contribution from anyone who wishes to get involved.

Before getting started, please read through our [contributing guidelines](CONTRIBUTING.md) and [code of conduct](CODE_OF_CONDUCT.md).

### Contributors

_Links to contributors._

# Links

### Logo

[The Refract logo is available in the Logo directory](/logo/).

### License

[Refract is available under the MIT license.](LICENSE)

### Discuss

[Everyone is welcome to join our discussion channel - `#refract` on the Reactiflux Discord server.](https://discord.gg/fqk86GH)

### Articles

*   [The introduction to Reactive Programming you've been missing
    ](https://gist.github.com/staltz/868e7e9bc2a7b8c1f754) by [@andrestaltz](https://twitter.com/andrestaltz)
