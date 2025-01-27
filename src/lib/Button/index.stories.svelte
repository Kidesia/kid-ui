<script module>
	import { defineMeta } from '@storybook/addon-svelte-csf';
	import Button from './index.svelte';
	import { fn } from '@storybook/test';

	const { Story } = defineMeta({
		title: 'Example/Button',
		component: Button,
		tags: ['autodocs'],
		argTypes: {
			label: { control: 'text' },
			variant: ['primary', 'secondary', 'tertiary', 'fab'],
			colorSchema: ['primary', 'secondary', 'tertiary'],
			disabled: { control: 'boolean' },
			confirm: {
				control: 'object',
				description: 'Confirm dialog settings',
				table: {
					type: { summary: 'object' },
					defaultValue: undefined
				},
				defaultValue: {
					message: 'Are you sure?',
					submitLabel: 'Yes',
					cancelLabel: 'No'
				},
				message: {
					control: 'text',
					description: 'The confirmation message',
					table: {
						type: { summary: 'string' },
						defaultValue: { summary: 'Are you sure?' }
					}
				},
				submitLabel: {
					control: 'text',
					description: 'Text for the confirm button',
					table: {
						type: { summary: 'string' },
						defaultValue: { summary: 'Yes' }
					}
				},
				cancelLabel: {
					control: 'text',
					description: 'Text for the cancel button',
					table: {
						type: { summary: 'string' },
						defaultValue: { summary: 'No' }
					}
				}
			}
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
