<script>
	import "./lib/component/SearchBox.svelte";
	import { city } from "./lib/city.js";

</script>

<div class="element">
	<search-box />
</div>

<style>
	.element {
		border-radius: 10px;
		background: var(--Gray-6, #ffff);
		box-shadow: 20px 20px 120px 15px rgba(0, 0, 0, 0.05);
		padding: 19px 23px;
		height: 101px;
		align-items: center;
		display: flex;
		flex-shrink: 0;
	}
</style>
