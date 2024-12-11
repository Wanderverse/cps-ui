### Quick Start

```bash
git clone https://github.com/skipdisk/coaxium-ui-library.git
cd `directory_name`
npm install
```

### Developing

Once you've cloned the repo, run the following command.

```bash
npm start
```

This will spin up the dev server. After the initial build, a browser will open automatically. There is currently no hot module reloading (HMR), as browser's don't provide a way to reregister custom elements, but most changes to the source will reload the browser automatically.

### Building

To generate a production build, run the following command.

```bash
npm run build
```

### Creating New Components

To scaffold a new component, run the following command, replacing `sl-tag-name` with the desired tag name.

```bash
npm run create sl-tag-name
```

This will generate a source file, a stylesheet, and a docs page for you. When you start the dev server, you'll find the new component in the "Components" section of the sidebar.
