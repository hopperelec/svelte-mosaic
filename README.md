# svelte-mosaic

port of [react-mosaic](https://github.com/nomcopter/react-mosaic) to Svelte.

## API

A `Node` is compromised of two siblings, `alpha` and `beta`.
These two homogeneous siblings are the only two children of `Node`.
However, a sibling can be a `Node` itself, creating a recursive tree structure.

For example, the following creates a Node with the following structure:

```
+-----------------------+
| Element A | Element B |
|           |-----------+
|           | Element C |
+-----------+-----------+
```

```svelte
<script>
	import { Mosaic, type Tree } from 'svelte-mosaic';

	const tree: Tree = {
		direction: 'horizontal',
		alpha: {
			component: Window,
			props: { number: 1 },
			size: { min: '200px' }
		},
		beta: {
			direction: 'vertical',
			alpha: {
				component: Window,
				props: { number: 2 },
				size: { min: '250px' }
			},
			beta: {
				component: Window,
				props: { number: 3 },
				size: { min: '300px' }
			},
			size: { min: '200px' }
		}
	};
</script>

<Mosaic {tree} />
```

### Internals

Because the API is very simple and doesn't require child-parent passing, we need to keep
track of the tree structure ourselves.

Whenever a `Node` is constructed, it backpropagates itself to its parents, giving each `Node` a reference to its parent.
