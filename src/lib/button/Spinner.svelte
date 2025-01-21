<script lang="ts">
	type Props = {
		text?: string;
		size?: 'small' | 'medium' | 'large';
		inverted?: boolean;
	};

	const { text, inverted, size }: Props = $props();
</script>

<div class="spinner" class:inverted>
	<div class={size}></div>
	{#if text}
		<p>{text}</p>
	{/if}
</div>

<style>
	@keyframes dot-float {
		20% {
			background-position:
				0% 0%,
				50% 50%,
				100% 50%;
		}
		40% {
			background-position:
				0% 100%,
				50% 0%,
				100% 50%;
		}
		60% {
			background-position:
				0% 50%,
				50% 100%,
				100% 0%;
		}
		80% {
			background-position:
				0% 50%,
				50% 50%,
				100% 100%;
		}
	}

	.spinner {
		--_color: var(--clr-primary);
		--_g: no-repeat radial-gradient(circle closest-side, var(--_color) 90%, #00000000);

		& > div {
			width: 64px;
			aspect-ratio: 2;
			background:
				var(--_g) 0% 50%,
				var(--_g) 50% 50%,
				var(--_g) 100% 50%;
			background-size: calc(100% / 3) 50%;
			animation: dot-float 1.5s infinite linear;

			&.small {
				width: 32px;
			}

			&.medium {
				width: 64px;
			}

			&.large {
				width: 100px;
			}
		}

		p {
			display: flex;
			justify-content: center;
			margin: auto;
			margin-top: 1rem;
			font-size: 1.5rem;
			font-weight: 700;
			color: var(--_color);
		}

		&.inverted {
			--_color: white;
		}
	}
</style>
