# @wangbog/blockcerts-verifier

基于@blockcerts/blockcerts-verifier (A standalone universal viewer &amp; verifier for blockcerts credentials)， @wangbog修改的实现。

本实现相较于官方版本，做了中文的支持。修改后的版本同样部署在了npm中（@wangbog/blockcerts-verifier），可以使用npm直接安装并使用。

值得注意的是，证书的验证逻辑实际上是在@wangbog/cert-verifier-js项目中实现的。本项目最主要的用处是基于@wangbog/cert-verifier-js，定义了<blockcerts-verifier>这个HTML标签，在您的web项目中可以在前端代码中使用该标签，用于展示、验证证书。请注意，验证过程实际上是由用户浏览器发起的。

以下文档全部fork自@blockcerts/blockcerts-verifier官方github项目，除了nmp包的scope由@blockcerts调整为@wangbog外，未做其余调整。

# Production
The component is developed with Polymer 3.
To use the component in your project, install it via:

```
  npm i @wangbog/blockcerts-verifier
```

If your project **does not require support for IE11**, you can use the following build:

```html
  <script src="node_modules/@wangbog/blockcerts-verifier/dist/main.js"></script>

  <blockcerts-verifier></blockcerts-verifier>
```
Chrome will support natively the code, but for Firefox, Safari, MS Edge (Opera and Brave), you will need to add the webcomponent loader before:

```html
    <script src="node_modules/@webcomponents/webcomponentsjs/webcomponents-loader.js"></script>
```

If your project **requires support for IE11**, you will need to use the ie11 build:
```html
  <script src="node_modules/@webcomponents/webcomponentsjs/custom-elements-es5-adapter.js"></script>
  <script src="node_modules/@webcomponents/webcomponentsjs/webcomponents-loader.js"></script>
  <script src="node_modules/@wangbog/blockcerts-verifier/dist/ie11.js"></script>
```
Please note that because this is transpiled to ES5, the custom-elements-es5-adapter code is required for it to work properly in more modern browsers.

Have a look at the [Demo Pages](/demo) to see examples of the usage

## API Usage

### Default behavior
By the default, the component will:
- Display a Blockcerts record in `card` mode (concise information)
- Will allow verification of a Blockcerts Record
- Enables auto-verification (verification as the record is loaded)

### API
The component will understand the following options:

- `allow-download`: (Boolean. default: `false`). Enables the download of the record. At this moment only records provided by Learning Machine are downloadable. 
   
   Example:
   
   ```html
   <blockcerts-verifier allow-download></blockcerts-verifier>
   ```
- `allow-social-share`: (Boolean. default: `false`). Allows sharing the record on the social networks (LinkedIn, Facebook and Twitter). 
   
   Example:
   
   ```html
   <blockcerts-verifier allow-social-share></blockcerts-verifier>
   ```
- `disable-auto-verify`: (Boolean. default: `false`). Disables starting automatically the verification sequence as the record is loaded. 
   
   Example:
   
   ```html
   <blockcerts-verifier disable-auto-verify></blockcerts-verifier>
   ```
- `disable-verify`: (Boolean. default: `false`). Disables verification of the record altogether. 
  
  Example:
  
  ```html
  <blockcerts-verifier disable-verify></blockcerts-verifier>
  ```
- `display-mode`: (String, oneOf('card', 'full', 'fullscreen'). default: `card`). 
Changes the display of a record. 
    - `card` will be a concise summary of the record with a link to the full record. 
    - `full` will show the actual record as designed by the emitter.
    - `fullscreen` will display a two-column overlay (in desktop) that takes the window dimensions.
The certificate displays similar as `full`. NOTA: only works for certificates that have a `displayHTML` property.
  
  Example:
  
  ```html
  <blockcerts-verifier display-mode="full"></blockcerts-verifier>
  ```
- `show-metadata`: (Boolean. default: `false`). Enables showing the metadata of a record.  
  
  Example:
  
  ```html
  <blockcerts-verifier show-metadata></blockcerts-verifier>
  ```
- `src`: (String. default: `''`). Allows loading an initial record with no further actions required. `src` can be either an absolute URL, or a relative path.  
  
  Example:
  
  ```html
  <blockcerts-verifier src='../fixtures/valid-certificate-example.json'></blockcerts-verifier>
  ```
- `theme`: (String. default: `'bright'`). Adapts to the background of the page that hosts the component. If the component is displayed on a dark background, you should use the `dark` option. If it's bright, then use the `bright` option.  
  
  Example:
  
  ```html
  <blockcerts-verifier theme='dark'></blockcerts-verifier>
  ```
  
- locale: (String. defaults to: `'auto'`, if language code not recognized will default to English (`en`)). View `src/i18n/lang` to see the list of supported languages. Contributions welcome.

  Example:
  
  ```html
  <blockcerts-verifier locale='fr'></blockcerts-verifier>
  ``` 
  
## Event Tracking API
The component will emit events on different moment of the certificate life cycle.
To subscribe and track these events you should add on your consumer page event listeners on the `window` object.

See the [event demo page](https://github.com/blockchain-certificates/blockcerts-verifier/blob/master/demo/events.html) for a working example.

The information is communicated via the `detail` key of the event.

Supported Events:
- `certificate-load`
   
   Triggered when a certificate has been loaded into the component.
   Returns:
    - the `certificateDefinition` (object) on which the action was called.
   
- `certificate-verify`
   
   Triggered when the verification process of a certificate is started.
   Returns:
    - the `certificateDefinition` (object) on which the action was called.
   
- `certificate-share`
   
   Triggered when a social network link is clicked.
   Returns:
    - the `certificateDefinition` (object) on which the action was called.
    - the `socialNetwork` (string) to which the record was shared.
   
   

# Development
## Viewing Your Element

```
npm run start
```

Will make the demo page available on http://localhost:8081/demo/.

## Modifying the Sanitizer

The `sanitizer` is used in order to protect against malicious certificates that could hold XSS attacks.
It is an overlay of the [xss](https://www.npmjs.com/package/xss) library, since at times, you might want to be able to configure or adapt the whitelist to your own needs.

To modify it, you should edit the `sanitizer/index.js` file. 

#### Whitelist CSS properties
More specifically if you wish to whitelist some CSS properties, add them to the object `whiteListedCssProperties`.

#### Generate the updated sanitizer
```
  npm run build:sanitizer
```

This will generate the `sanitizer.js` file, which is then used by the application and the tests.

If you want to work on the sanitizer in watch mode (and auto-generate your changes), use the following command:

```
  npm run build:sanitizer -- -w
```

## Running Tests

### Application Tests

```
npm run test:application
```

NOTE: application must be started to run the tests, or at the very least the mock-server via the `npm run start:mock-server` (automatically included in the `npm run start` command).

**watch mode**

```
npm run test:application:watch
```

### Component Tests
```
npm run test:components
```

**"watch" mode**
```
npm run test:components:persist
```
Will allow refreshing the test page: http://localhost:8000/components/blockcerts-verifier/generated-index.html?cli_browser_id=0

## Dealing with CSS
The `npm run start` command will also start a SASS compiler watcher, which means that any stylesheet within the `components` folder will be transpiled to a polymer component that can be reused within another component. ie:

```javascript
import CSS from './_components.button-css';
[...]
_render () {
    return html`${CSS}[...]`
}
```

### Using shared styles
To reduce the amount of code duplication, and following the ITCSS philosophy, you may need to import some of the shared-styles in your component.
To do so, in your component's SASS file, add the following instruction:

```javascript
/* in _components.my-component.sass */

@import '../../../shared-styles/objects.text';

[...component styles]

@import '../../../shared-styles/utils.a11y';
```

Please note that the SASS watcher does not observe changes in the shared styles folder, and will not automatically recompile any consumer stylesheet. You will have to recompile them yourselves (TODO: improve DevX here).

## More info
Please have a look through the [ADR](/docs/ADR) documentation to get more context around the architecture and the ways of developing a component.
