---
theme: post
published: true
title: Things learned – February 2019
---
# 6 February

### Adding Typescript support

Add new dependency: `typescript`

`devDependencies` for types:

```json
{
  "@types/jest": "^23.3.9",
  "@types/lodash.get": "^4.4.4",
  "@types/lodash.isempty": "^4.4.4",
  "@types/node": "^10.12.9",
  "@types/react": "^16.7.6",
  "@types/react-dom": "^16.0.9",
  "@types/react-redux": "^6.0.9",
  "@types/react-router": "^4.4.1",
  "@types/react-router-dom": "^4.3.1",
}
```

`tsconfig.json` enables Typescript support:

```json
{
	"compilerOptions": {
		"target": "es5",
		"allowJs": true,
		"baseUrl": "./src",
		"skipLibCheck": false,
		"esModuleInterop": true,
		"allowSyntheticDefaultImports": true,
		"strict": true, // can be commented for ignoring 'any' errors
		"forceConsistentCasingInFileNames": true,
		"module": "esnext",
		"moduleResolution": "node",
		"resolveJsonModule": true,
		"isolatedModules": true,
		"noEmit": true,
		"jsx": "preserve",
		"lib": ["es2018", "dom"]
	},
	"include": ["src"],
	"exclude": ["build"]
}
```

`src/global.d.ts` for Sass modules support:
```ts
declare module '*.scss' {
  const content: {[className: string]: string};
  export = content;
}
```

---

# 5 February

### Component re-renders instead of remounting on path change in `react-router`

(https://stackoverflow.com/a/49441836)[SO answer]

```jsx
// entry in react-router-config
{
  path: '/foo/:bar/baz',
  exact: true,
  render: props => (<Component key={props.match.params.bar} {...props} />)
}
```
