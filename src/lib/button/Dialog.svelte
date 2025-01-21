<script lang="ts">
	import type { Snippet } from 'svelte';
	import { createDialog, melt } from '@melt-ui/svelte';
	import { fade, scale } from 'svelte/transition';

	import type { ButtonProps } from './Button.svelte';
	import i18n from '$lib/i18n/index.svelte';
	import Button from './Button.svelte';

	type DialogProps = ButtonProps & {
		submitLabel?: string;
		onsubmit?: () => Promise<void>;
		children: Snippet;
	};

	const { onclick, submitLabel, onsubmit, children, ...btnProps }: DialogProps = $props();

	const {
		elements: { trigger, portalled, overlay, content, close },
		states: { open }
	} = createDialog({
		role: 'alertdialog'
	});
</script>

<div use:melt={$portalled}>
	<div use:melt={$overlay} transition:fade={{ duration: 150 }} class="overlay"></div>

	<div use:melt={$content} transition:scale={{ duration: 150 }} class="dialog-container">
		<div class="dialog">
			{@render children()}

			<div class="row">
				<button class="no-style" use:melt={$close}>
					<Button dummy type="tertiary" label={i18n.t('common.cancel')} />
				</button>
				{#if onsubmit}
					<Button
						type="primary"
						label={submitLabel ?? i18n.t('common.submit')}
						onclick={async () => {
							await onsubmit();
							open.set(false);
						}}
					/>
				{/if}
			</div>
		</div>
	</div>
</div>

<style>
	.overlay {
		position: fixed;
		inset: 0;
		background: var(--clr-on-surface);
		opacity: 0.1;
	}

	.dialog-container {
		position: fixed;
		inset: 0;
		display: flex;
		justify-content: center;
		align-items: center;
	}

	.dialog {
		flex: 1;
		margin: 3rem;
		display: flex;
		flex-direction: column;
		gap: 1rem;
		background-color: var(--clr-surface-container-lowest);
		padding: 1rem;
		border-radius: 1rem;
	}

	.row {
		display: flex;
		justify-content: flex-end;
		gap: 0.5rem;

		:global(> button) {
			flex: 1;
		}
	}
</style>
