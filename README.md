## issue

When pnpm link, global.d.ts is used, abnormal any is displayed, and no error is reported
Why? It's a bug, I'm not using it right, or something. Is your typescript working?

## environment

- system: windows 11 23H2 22635.2841 Windows Feature Experience Pack 1000.22681.1000.0
- editor: vscode 1.88.1
- vscode-typescript-plugin: 5.4.5

## directory

```
- packages
  -p1
    -src
      -index.ts
  -tsconfig.json
  -package.json
  -p2
    -src
      -index.ts
  -global.d.ts
  -tsconfig.json
  -package.json
```

## Main file content

```jsonc
// ./packages/p1/package.json
{
  "name": "p1",
  "version": "1.0.0",
  "dependencies": {
    "p2": "link:../p2"
  }
}
```

```ts
// ./packages/p1/src/index.ts
import { date } from "p2";

date;
// No errors are reported but any is displayed
/** Mouse hover type
(alias) const date: any
import date
 */
```

```jsonc
{
  "name": "p2",
  "version": "1.0.0",
  "exports": {
    "import": "./src/index.ts",
    "types": "./src/index.ts"
  }
}
```

```ts
// ./packages/p2/src/index.ts
export const date = globalFunction();

/**
const date: {
    xxx: "some data";
}
 */
```

```ts
// ./packages/p2/global.d.ts
export {};
declare global {
  const globalFunction: () => {
    xxx: "some data";
  };
}
```

```jsonc
// ./packages/p2/tsconfig.json
{
  "compilerOptions": {
    "target": "es2016" /* Set the JavaScript language version for emitted JavaScript and include compatible library declarations. */,
    "module": "ESNext" /* Specify what module code is generated. */,
    "esModuleInterop": true /* Emit additional JavaScript to ease support for importing CommonJS modules. This enables 'allowSyntheticDefaultImports' for type compatibility. */,
    "forceConsistentCasingInFileNames": true /* Ensure that casing is correct in imports. */,
    "strict": true /* Enable all strict type-checking options. */,
    "skipLibCheck": true /* Skip type checking all .d.ts files. */,
    "types": ["./global.d.ts"],
    "moduleResolution": "Bundler"
  }
}
```
