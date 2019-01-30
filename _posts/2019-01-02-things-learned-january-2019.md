---
theme: post
published: true
title: Things learned – January 2019
---
# January 30

### Prevent re-renders because of arrow functions v2

Shorter way, maybe hooks will provide something even better

```jsx
// in render()
<RemoveFieldButton
  fieldId={fieldId}
  removeField={this.removeField}
/>

// outside rendering class
class RemoveEnvironmentButton extends React.PureComponent {
	removeField = () => { this.props.removeField(this.props.ldapLogin); };

	render() {
		return (
			<Button onClick={this.removeField}>
				Remove field
			</Button>
		);
	}
}
```

# January 17

### Prevent re-renders because of arrow functions

Old:
```jsx
// in render()
fields.map(fieldId => <Button onClick={this.removeField(fieldId)} />)
```

New:
```jsx
// in render()
<RemoveFieldButton
  fieldId={fieldId}
  removeField={this.removeField}
/>

// outside rendering class
class RemoveEnvironmentButton extends React.PureComponent {
	handleOnClick = () => {
		const { removeField, fieldId } = this.props;

		removeField(fieldId);
	};

	render() {
		return (
			<Button onClick={this.handleOnClick}>
				Remove field
			</Button>
		);
	}
}
```

### ESLint rule to disallow binds in `render`

```json
"react/jsx-no-bind": [
  1
]
```

---

# January 9

### Code-splitting using `React.lazy()`

```jsx
// import
// can make lambda async and use await for things like sleep()
const ComponentName = React.lazy(() => import('./FileNameWithDefaultExport'));


// usage in render
<React.Suspense fallback={<Spin />}>
  <ComponentName />
</React.Suspense>
```

---

# January 3

### `position: fixed` won't work when CSS filters are applied in parent

This behaviour is strange, but accords to specification.

[SO](https://stackoverflow.com/questions/52937708/css-filter-on-parent-breaks-child-positioning)

---

# January 2

### Capturing page snapshot with delay

Put this into console and put your page into state you want to snapshot, before breakpoint triggers.
```javascript
const delay = 4000;
setTimeout(() => { debugger; }, delay);
```
