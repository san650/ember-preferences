# Documentation

- [Installation](#installation)
- [Preferences as a service](#preferences-as-a-service)
- [Preferences as a mixin](#preferences-as-a-mixin)
- [Computed property](#computed-property)

## Installation

```js
$ ember install ember-preferences
```

## Preferences as a service

The addon provides an ember service which reads and writes configurations
directly to local storage.

__In
the future you will be able to choose the backend (local storage, session
storage, cookies, etc.).__

The service is already registered in the application container so it's ready to be injected on any object.

```js
import Ember from 'ember';

const { computed, inject } = Ember;

export default Ember.Component.extend({
  preferences: inject.service(),
  foo: computed.alias('preferences.foo')
});
```

You can also inject the service with a different property name

```js
import Ember from 'ember';

const { inject } = Ember;

export default Ember.Component.extend({
  userOptions: inject.service('preferences'),
  foo: computed.alias('userOptions.foo')
});
```

Where `foo` is a property which will be written and read from local storage.

## Preferences as a mixin

You can use a mixin to inject the `preferences` service, this DRYs up and gives
consistency to the code.

```js
import Ember from 'ember';
import PreferencesMixin from 'ember-preferences/mixin';

export default Ember.Component.extend(PreferencesMixin, {
  foo: computed.alias('preferences.foo')
});
```

## Computed property

The addon provides a computed property which extends the capabilities of a preference key by adding the possibility of returning a default value.

__Without default value__

```js
import Ember from 'ember';
import preference from 'ember-preferences/computed';

const { computed, inject } = Ember;

export default Ember.Component.extend({
  preferences: inject.service(),
  foo: preference('bar')
});
```

The property `foo` will read and write the preference value from `preferences.bar` (like if it was a `Ember.computed.alias('preferences.bar')`).

__With default value__

```js
import Ember from 'ember';
import preference from 'ember-preferences/computed';

const { computed, inject } = Ember;

export default Ember.Component.extend({
  preferences: inject.service(),
  foo: preference('bar', { defaultValue: 'hello world!' })
});
```

The default value will be returned when the preference is `null` or `undefined`.

You can use a function to generate the default value on first access, useful to
return mutable objects.

```js
import Ember from 'ember';
import preference from 'ember-preferences/computed';

const { computed, inject } = Ember;

export default Ember.Component.extend({
  preferences: inject.service(),
  foo: preference('bar', { defaultValue() { return ['an', 'array']; } })
});
```

Other features are planned to be added in the future like expiration based on
absolute time, preference name spacing and others.