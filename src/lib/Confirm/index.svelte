<script lang="ts" module>
	export type ConfirmProps = {
		message: string;
		cancelLabel?: string;
		submitLabel?: string;
		onsubmit?: () => Promise<void> | void;
		onclose: () => void;
	};
</script>

<script lang="ts">
	import { createDialog, melt } from '@melt-ui/svelte';
	import { fade, scale } from 'svelte/transition';

	import Button from '../Button/index.svelte';

	const { message, cancelLabel, submitLabel, onsubmit, onclose }: ConfirmProps = $props();

	const {
		elements: { portalled, overlay, content }
	} = createDialog({
		role: 'alertdialog'
	});
</script>

<div use:melt={$portalled}>
	<div use:melt={$overlay} transition:fade={{ duration: 150 }} class="overlay"></div>
	<div use:melt={$content} transition:scale={{ duration: 150 }} class="dialog-container">
		<main class="dialog">
			{message}

			<footer>
				<Button variant="tertiary" label={cancelLabel ?? 'Cancel'} onclick={onclose} />
				{#if onsubmit}
					<Button
						variant="primary"
						label={submitLabel ?? 'OK'}
						onclick={async () => {
							await onsubmit();
							onclose();
						}}
					/>
				{/if}
			</footer>
		</main>
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

	footer {
		display: flex;
		justify-content: flex-end;
		gap: 0.5rem;
	}
</style>
