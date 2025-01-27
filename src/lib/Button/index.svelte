<script lang="ts" module>
	import type { ConfirmProps } from '$lib/Confirm/index.svelte';

	export type ButtonProps = HTMLButtonAttributes & {
		Icon?: Component;
		label?: string;
		variant?: 'primary' | 'secondary' | 'tertiary' | 'fab';
		colorSchema?: 'primary' | 'secondary' | 'tertiary';
		confirm?: Omit<ConfirmProps, 'onclose' | 'onsubmit'>;
		onclick?: () => Promise<void> | void;
	};
</script>

<script lang="ts">
	import type { Component } from 'svelte';
	import type { HTMLButtonAttributes } from 'svelte/elements';

	import Confirm from '../Confirm/index.svelte';

	const {
		Icon,
		label,
		variant = 'secondary',
		colorSchema = 'primary',
		disabled,
		confirm,
		onclick,
		...buttonProps
	}: ButtonProps = $props();

	let loading = $state(false);

	let isDiplayingConfirm = $state(false);
</script>

{#if confirm && isDiplayingConfirm}
	<Confirm
		{...confirm}
		submitLabel={confirm.submitLabel ?? label}
		onclose={() => (isDiplayingConfirm = false)}
		onsubmit={() => {
			onclick?.();
		}}
	/>
{/if}

<button
	{...buttonProps}
	class="variant-{variant} color-schema-{colorSchema}"
	disabled={disabled || loading}
	onclick={async (event) => {
		if (confirm) {
			isDiplayingConfirm = true;
			return;
		} else if (onclick) {
			loading = true;
			try {
				await onclick(event);
			} finally {
				loading = false;
			}
		}
	}}
>
	{#if Icon}
		<span class="icon">
			<Icon />
		</span>
	{/if}
	{#if label}
		<span class="label">
			{label}
		</span>
	{/if}
</button>

<style>
	button,
	.button {
		position: relative;
		display: flex;
		justify-content: center;
		gap: 0.5rem;
		padding: 0.5rem 1rem;
		border-radius: 0.5rem;
		background-color: transparent;
		border: none;
		color: var(--clr-on-surface-variant);

		&:active {
			transform: scale(0.95);
			background-color: var(--clr-surface-container-highest);
			opacity: 0.9;
		}

		&.fab {
			font-weight: 600;
			color: var(--clr-on-primary);
			background-color: var(--clr-primary);
			padding-left: 1rem;
			padding-right: 1rem;
			border-radius: 2rem;
		}

		&.variant-primary {
			font-weight: 600;

			&.color-schema-primary {
				color: var(--clr-on-primary);
				background-color: var(--clr-primary);
			}

			&.color-schema-secondary {
				color: var(--clr-on-secondary);
				background-color: var(--clr-secondary);
			}

			&.color-schema-tertiary {
				color: var(--clr-on-tertiary);
				background-color: var(--clr-tertiary);
			}
		}

		&.variant-secondary {
			background-color: var(--clr-surface-container-highest);
		}

		&:disabled {
			opacity: 0.5;
		}
	}

	.icon {
		font-size: 1.25rem;
		flex-shrink: 0;
	}

	.spinner-container {
		position: absolute;
		inset: 0;
		display: flex;
		justify-content: center;
		align-items: center;
		background-color: rgba(255, 255, 255, 0.5);
	}
</style>
