---
meta:
  title: Localization
  description: Discover how to localize Coaxium with minimal effort.
---

# Localization

Components can be localized by importing the appropriate translation file and setting the desired [`lang` attribute](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/lang) and/or [`dir` attribute](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/dir) on the `<html>` element. Here's an example that renders Coaxium components in Spanish.

```html
<html lang="es">
  <head>
    <script type="module" src="/path/to/coaxium/dist/translations/es.js"></script>
  </head>

  <body>
    ...
  </body>
</html>
```

Through the magic of a mutation observer, changing the `lang` attribute will automatically update all localized components to use the new locale.

## Available Translations

Coaxium supports English and Spanish. The default is English (US), which also serves as the fallback locale. As such, you do not need to import the English translation.

The location of translations depends on how you're consuming Coaxium.

- If you're using the CDN, [import them from the CDN](https://www.jsdelivr.com/package/npm/coaxium-ui-library-pilot?path=%CDNDIR%%2Ftranslations)
- If you're using a bundler, import them from `coaxium-ui-library/%NPMDIR%/translations/[lang].js`

You do not need to load translations up front. You can import them dynamically even after updating the `lang` attribute. Once a translation is registered, localized components will update automatically.

```js
// Same as setting <html lang="de">
document.documentElement.lang = 'de';

// Import the translation
import('/path/to/coaxium/dist/translations/de.js');
```

### Translation Resolution

The locale set by `<html lang="...">` is the default locale for the document. If a country code is provided, e.g. `es` for Spanish, the localization library will resolve it like this:

1. Look for `es`
2. Fall back to `en`

Coaxium uses English as a fallback to provide a better experience than rendering nothing or throwing an error.

## Multiple Locales Per Page

You can use a different locale for an individual component by setting its `lang` and/or `dir` attributes. Here's a contrived example to demonstrate.

```html
<html lang="es">
  ...

  <body>
    <sl-button><!-- Spanish --></sl-button>
    <sl-button lang="ru"><!-- Russian --></sl-button>
  </body>
</html>
```

For performance reasons, the `lang` and `dir` attributes must be on the component itself, not on an ancestor element.

```html
<html lang="es">
  ...

  <body>
    <div lang="ru">
      <sl-button><!-- still in Spanish --></sl-button>
    </div>
  </body>
</html>
```

This limitation exists because there's no efficient way to determine the current locale of a given element in a DOM tree. I consider this a gap in the platform and [I've proposed a couple properties](https://github.com/whatwg/html/issues/7039) to make this possible.

## Creating Your Own Translations

You can provide your own translations if you have specific needs or if you don't want to wait for a translation to land upstream. The easiest way to do this is to copy `src/translations/en.ts` into your own project and translate the terms inside. When your translation is done, you can import it and use it just like a built-in translation.

Let's create a Spanish translation as an example. The following assumes you're using TypeScript, but you can also create translations with regular JavaScript.

```js
import { registerTranslation } from 'coaxium-ui-library-pilot/dist/utilities/localize';
import type { Translation } from 'coaxium-ui-library-pilot/dist/utilities/localize';

const translation: Translation = {
  $code: 'es',
  $name: 'Español',
  $dir: 'ltr',

  term1: '...',
  term2: '...',
  ...
};

registerTranslation(translation);

export default translation;
```

Once your translation has been compiled to JavaScript, import it and activate it like this.

```html
<html lang="es">
  <head>
    <script type="module" src="/path/to/es.js"></script>
  </head>

  <body>
    ...
  </body>
</html>
```

:::tip
If your translation isn't working, make sure you're using the same localize module when importing `registerTranslation`. If you're using a different module, your translation won't be recognized.
:::
