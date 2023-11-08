<script>
	import { createEventDispatcher } from "svelte";
	import { city } from "../city.js";

	let selected = null;
	let activeCity = null;
	const emit = createEventDispatcher();
	const choiceCountry = (item) => {
		selected = item?.label || "ae";
		activeCity = item?.id || "ae";

		emit("selected", item);
	};
</script>

<div class="city">
	{#each city as item}
		<!-- svelte-ignore a11y-click-events-have-key-events -->
		<!-- svelte-ignore a11y-no-static-element-interactions -->
		<div class="item" on:click={() => choiceCountry(item)} >
			<div class="title">{item.label}</div>
		</div>
	{/each}
</div>

<style lang="scss">
	.city {
		display: flex;
		width: 415px;
		flex-direction: column;
		align-items: flex-start;
		height: 241.491px;
		background: #f2f2f2;
		position: absolute;
		padding: 0px;
		z-index: 8;
		height: 160px;
		overflow-y: scroll;
		top: 84px;
		left: 0;
		.item {
			pointer-events: all;
			cursor: pointer;
			color: var(--Gray-1, #333);
			font-family: Roboto;
			font-size: 14px;
			font-style: normal;
			font-weight: 400;
			line-height: normal;
			padding: 8px;
			width: 100%;
			text-align: start;
			&.active {
				border-radius: 5px;
				background: var(--Gray-5, #e0e0e0);
			}
		}
	}
</style>
