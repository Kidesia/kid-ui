<script module>
	import { defineMeta } from '@storybook/addon-svelte-csf';
	import Button from './Button.svelte';
	import { fn } from '@storybook/test';

	const { Story } = defineMeta({
		title: 'Example/Button',
		component: Button,
		tags: ['autodocs'],
		argTypes: {
			variant: ['primary', 'secondary', 'tertiary', 'fab'],
			colorSchema: ['primary', 'secondary', 'tertiary'],
			disabled: { control: 'boolean' }
		},
		args: {
			onclick: fn()
		}
	});
</script>

<Story name="Primary" args={{ variant: 'primary', label: 'Button' }} />
<Story name="Secondary" args={{ label: 'Button' }} />
<Story name="Tertiary" args={{ variant: 'tertiary', label: 'Button' }} />
<Story name="Fab" args={{ variant: 'fab', label: 'Button' }} />
