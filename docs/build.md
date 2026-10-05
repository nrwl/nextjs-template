# Building the Next.js app

Run `npx nx run-many -t build` after `npm ci`. The web app's build depends on
`^build` and `^typecheck`, so Nx prepares its dependencies before `next build`.

`packages/ui` is a non-buildable React library, matching
`nx g @nx/react:lib packages/ui --bundler=none`. Its package exports point to
TypeScript source, which Next.js bundles directly. The app also references the
library in its TypeScript configuration, so Next.js's production type check
requires the library's emitted declarations.

The library's inferred `typecheck` target runs
`tsc --build tsconfig.json --emitDeclarationOnly` and caches those declarations.
The `^typecheck` dependency makes them available on a clean build, avoiding
TS6305. The `^build` dependency also supports libraries with their own build
targets. Source exports and TypeScript project references remain available for
development and editor navigation.
