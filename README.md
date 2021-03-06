# nuxt-jsonld

[![version](https://img.shields.io/npm/v/nuxt-jsonld.svg)](https://www.npmjs.com/package/nuxt-jsonld)
[![dependencies](https://david-dm.org/ymmooot/nuxt-jsonld/status.svg)](https://david-dm.org/ymmooot/nuxt-jsonld)
[![downloads](https://img.shields.io/npm/dt/nuxt-jsonld.svg)](https://www.npmjs.com/package/nuxt-jsonld)
[![CircleCI](https://circleci.com/gh/ymmooot/nuxt-jsonld.svg?style=shield)](https://circleci.com/gh/ymmooot/nuxt-jsonld)
[![codecov](https://codecov.io/gh/ymmooot/nuxt-jsonld/branch/master/graph/badge.svg)](https://codecov.io/gh/ymmooot/nuxt-jsonld)


A Nuxt.js plugin to manage jsonld in Vue component.

## Installation

```bash
$ yarn add nuxt-jsonld
or
$ npm install nuxt-jsonld
```

```js
// plugins/jsonld.js
import Vue from 'vue';
import NuxtJsonld from 'nuxt-jsonld';

Vue.use(NuxtJsonld);

// you can set the indentation
Vue.use(NuxtJsonld, {
  space: process.env.NODE_ENV === 'production' ? 0 : 2, // default: 2
});
```

Then, add plugin config to `nuxt.cofig.js`.

```js
  plugins: ['~/plugins/jsonld'],
```

## Usage

Make a jsonld method to your Vue components and return structured data object.

```vue
<template>
  ...
</template>

<script>
export default {
  data() {
    return {
      breadcrumbs: [
        {
          url: 'https://example.com',
          text: 'top page',
        },
        {
          url: 'https://example.com/foo',
          text: 'foo',
        },
        {
          url: 'https://example.com/foo/bar',
          text: 'bar',
        },
      ],
    };
  },
  jsonld() {
    const items = this.breadcrumbs.map((item, index) => ({
      '@type': 'ListItem',
      position: index + 1,
      item: {
        '@id': item.url,
        name: item.text,
      },
    }));
    return {
      '@context': 'http://schema.org',
      '@type': 'BreadcrumbList',
      itemListElement: items,
    };
  },
};
</script>
```

🎉 You will get the jsonld tag! 🎉

```html
<script type="application/ld+json">
{
  "@context": "http://schema.org",
  "@type": "BreadcrumbList",
  "itemListElement": [
    {
      "@type": "ListItem",
      "position": 1,
      "item": {
        "@id": "https://example.com",
        "name": "top page"
      }
    },
    {
      "@type": "ListItem",
      "position": 2,
      "item": {
        "@id": "https://example.com/foo",
        "name": "foo"
      }
    },
    {
      "@type": "ListItem",
      "position": 3,
      "item": {
        "@id": "https://example.com/foo/bar",
        "name": "bar"
      }
    },
  ]
}
</script>
```

If you do not want to make jsonld tag, just return null;

```html
<script>
export default {
  props: ['foo'],
  jsonld() {
    if (!this.foo) {
      return null;
    }

    return {
      '@context': 'http://schema.org',
      '@type': 'SomeType',
      body: foo,
    };
  },
};
</script>
```


### TypeScript

with `Vue.extned`

```html
<script lang="ts">
export default Vue.extend({
  jsonld() {
    return {
      '@context': "http://schema.org",
      body: 'some text',
    };
  },
});
</script>
```

with `vue-property-decorator`

```html
<script lang="ts">
import { Component, Vue } from 'vue-property-decorator'
import { Jsonld } from 'nuxt-jsonld';

@Jsonld
@Component
export default class Sample extends Vue {
  jsonld() {
    return {
      '@context': "http://schema.org",
      body: 'some text',
    };
  }
};
</script>
```
