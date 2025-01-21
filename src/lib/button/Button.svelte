<script lang="ts" module>
	export type ButtonProps = {
		Icon?: Component;
		label?: string;
		variant?: 'primary' | 'secondary' | 'tertiary' | 'fab';
		colorSchema?: 'primary' | 'secondary' | 'tertiary';
		disabled?: boolean;
		submit?: boolean;
		ariaLabel?: string;
		onclick?: MouseEventHandler<HTMLButtonElement>;
	};
</script>

<script lang="ts">
	import type { Component } from 'svelte';
	import type { MouseEventHandler } from 'svelte/elements';
	import Spinner from './Spinner.svelte';

	const {
		Icon,
		label,
		variant = 'secondary',
		colorSchema = 'primary',
		disabled = false,
		ariaLabel,
		submit,
		onclick
	}: ButtonProps = $props();

	let loading = $state(false);
</script>

{#snippet Content()}
	{#if loading}
		<div class="spinner-container">
			<Spinner />
		</div>
	{/if}
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
{/snippet}

<button
	type={submit ? 'submit' : 'button'}
	class="{variant} color-schema-{colorSchema}"
	disabled={disabled || loading}
	aria-label={ariaLabel}
	onclick={async (event) => {
		if (onclick) {
			loading = true;
			try {
				await onclick(event);
			} finally {
				loading = false;
			}
		}
	}}
>
	{@render Content()}
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

		&.primary {
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

		&.secondary {
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

	.loading-contents {
		flex-grow: 1;
		display: flex;
		min-height: 1.5em;
		justify-content: center;
		align-items: center;
		gap: 1rem;
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
