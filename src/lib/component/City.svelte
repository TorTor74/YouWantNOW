<script>
	import { createEventDispatcher } from "svelte";
	import { city } from "../city.js";

    export let cities = city.map(i => i.label);
    export let value;

	let selected = null;
	let activeCity = null;
	const emit = createEventDispatcher();
	const choiceCountry = (item) => {
		// selected = item?.label || "ae";
		// activeCity = item?.id || "ae";

		emit("selected", item);
        
	};

    $: filtered = cities.filter(i => !value?.trim()?.length || i.toLowerCase()?.includes(value?.trim()?.toLowerCase()))
</script>

<div class="city">
	{#each filtered as item}
		<!-- svelte-ignore a11y-click-events-have-key-events -->
		<!-- svelte-ignore a11y-no-static-element-interactions -->
		<div class="item" on:click|preventDefault|stopPropagation={() => choiceCountry(item)} >
			<div class="title">{@html item.replace(value, `<b>${value}</b>`)}</div>
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
		top: 54px;
		left: 0;
        overflow-x: hidden;
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
