<svelte:options customElement="search-box" />

<script>
	/**
	 * Import Svelte
	 */

	import { createEventDispatcher, setContext } from "svelte";
	import { fade, fly } from "svelte/transition";

	/**
	 * Data fns import and settings
	 */

	import { setDefaultOptions, format, addMonths, subMonths } from "date-fns";
	import { bg, ru } from "date-fns/locale";
	setDefaultOptions({ locale: ru, weekStartsOn: 1 });

	/**
	 * Imports comonents
	 */

	import City from "./City.svelte";
	import Calendar from "./Calendar.svelte";

	/**
	 * Export
	 */

	export let cities;

	/**
	 * Event
	 */

	const globalEmit = createEventDispatcher();
	setContext("globalEmit", globalEmit);

	let currentDay = new Date();
	let firstCity;
	let secondCity;
	let openDate = false;
	let openFirst = false;
	let openSecond = false;
	let yes = true;
	let startTrip, endTrip;
	let showGood = false;
	let errorFirst = false,
		errorSecond = false,
		errorStart = false,
		errorEnd = false;

	/**
	 * Events for calendar : select, next, prev
	 */

	const choiceTrip = (e) => {
		let item = e.detail;
		if (startTrip > endTrip) {
			endTrip = startTrip;
			startTrip = undefined;
		} else if (startTrip != undefined && yes) {
			endTrip = item;
		} else if (startTrip == undefined) {
			startTrip = item;
		} else if (!yes) {
			startTrip = item;
		}
	};

	const prevMonth = () => {
		currentDay = subMonths(currentDay, 1);
	};
	const nextMonth = () => {
		currentDay = addMonths(currentDay, 1);
	};

	$: !yes ? (endTrip = "") : endTrip;
</script>

<div class="search-box">
	<div class="place">
		<label>
			<div class="title">Откуда</div>
			<!-- svelte-ignore a11y-click-events-have-key-events -->
			<!-- svelte-ignore a11y-no-static-element-interactions -->
			<div
				class="select"
				class:error={errorFirst}
				on:click|stopPropagation={() => {
					openFirst = true;
					openSecond = false;
					openDate = false;
				}}
			>
				<svg xmlns="http://www.w3.org/2000/svg" width="11" height="13" viewBox="0 0 11 13" fill="none">
					<path
						d="M5.48161 0.0561523C7.00223 0.0561523 8.30212 0.474642 9.38127 1.31162C10.4604 2.1486 11 3.16356 11 4.35649C11 4.95296 10.8099 5.64082 10.4298 6.42008C10.0496 7.19933 9.58974 7.92087 9.05017 8.58468C8.51059 9.24849 7.97101 9.87382 7.43144 10.4607C6.89186 11.0475 6.43813 11.5141 6.07023 11.8604L5.48161 12.3511C5.33445 12.2356 5.13824 12.0625 4.89298 11.8316C4.64771 11.6007 4.21237 11.1533 3.58696 10.4895C2.96154 9.82571 2.40357 9.18114 1.91304 8.55582C1.42252 7.93049 0.981048 7.22338 0.588629 6.43451C0.19621 5.64563 0 4.95296 0 4.35649C0 3.16356 0.533445 2.1486 1.60033 1.31162C2.66722 0.474642 3.96098 0.0561523 5.48161 0.0561523ZM5.48161 5.88614C6.02118 5.88614 6.48718 5.73702 6.8796 5.43879C7.27202 5.14056 7.46823 4.77979 7.46823 4.35649C7.46823 3.93319 7.27202 3.57242 6.8796 3.27419C6.48718 2.97596 6.02118 2.82684 5.48161 2.82684C4.94203 2.82684 4.48216 2.97596 4.10201 3.27419C3.72185 3.57242 3.53177 3.93319 3.53177 4.35649C3.53177 4.77979 3.72185 5.14056 4.10201 5.43879C4.48216 5.73702 4.94203 5.88614 5.48161 5.88614Z"
						fill="#333333"
					/>
				</svg>
				<input type="text" bind:value={firstCity} />
				{#if openFirst}
					<City
						value={firstCity}
						{cities}
						on:selected={(e) => {
							firstCity = e.detail;
							openFirst = false;
						}}
					/>
				{/if}
			</div>
		</label>
		<!-- svelte-ignore a11y-label-has-associated-control -->
		<label>
			<div class="title">Куда</div>
			<!-- svelte-ignore a11y-click-events-have-key-events -->
			<!-- svelte-ignore a11y-no-static-element-interactions -->
			<div
				class="select"
				class:error={errorSecond}
				on:click={() => {
					openFirst = false;
					openSecond = true;
					openDate = false;
				}}
			>
				<svg xmlns="http://www.w3.org/2000/svg" width="11" height="13" viewBox="0 0 11 13" fill="none">
					<path
						d="M5.48161 0.0561523C7.00223 0.0561523 8.30212 0.474642 9.38127 1.31162C10.4604 2.1486 11 3.16356 11 4.35649C11 4.95296 10.8099 5.64082 10.4298 6.42008C10.0496 7.19933 9.58974 7.92087 9.05017 8.58468C8.51059 9.24849 7.97101 9.87382 7.43144 10.4607C6.89186 11.0475 6.43813 11.5141 6.07023 11.8604L5.48161 12.3511C5.33445 12.2356 5.13824 12.0625 4.89298 11.8316C4.64771 11.6007 4.21237 11.1533 3.58696 10.4895C2.96154 9.82571 2.40357 9.18114 1.91304 8.55582C1.42252 7.93049 0.981048 7.22338 0.588629 6.43451C0.19621 5.64563 0 4.95296 0 4.35649C0 3.16356 0.533445 2.1486 1.60033 1.31162C2.66722 0.474642 3.96098 0.0561523 5.48161 0.0561523ZM5.48161 5.88614C6.02118 5.88614 6.48718 5.73702 6.8796 5.43879C7.27202 5.14056 7.46823 4.77979 7.46823 4.35649C7.46823 3.93319 7.27202 3.57242 6.8796 3.27419C6.48718 2.97596 6.02118 2.82684 5.48161 2.82684C4.94203 2.82684 4.48216 2.97596 4.10201 3.27419C3.72185 3.57242 3.53177 3.93319 3.53177 4.35649C3.53177 4.77979 3.72185 5.14056 4.10201 5.43879C4.48216 5.73702 4.94203 5.88614 5.48161 5.88614Z"
						fill="#333333"
					/>
				</svg>
				<input type="text" bind:value={secondCity} />

				{#if openSecond}
					<City
						value={secondCity}
						{cities}
						on:selected={(e) => {
							secondCity = e.detail;
							openSecond = false;
						}}
					/>
				{/if}
			</div>
		</label>
	</div>
	<!-- svelte-ignore a11y-label-has-associated-control -->

	<!-- svelte-ignore a11y-label-has-associated-control -->
	<div class="times">
		<div class="title">Даты</div>
		<!-- svelte-ignore a11y-click-events-have-key-events -->
		<!-- svelte-ignore a11y-no-static-element-interactions -->
		<div
			class="date"
			class:error={errorStart || errorEnd}
			on:click={() => {
				openFirst = false;
				openSecond = false;
				openDate = true;
			}}
		>
			<div class="start">
				{#if startTrip}
					<svg xmlns="http://www.w3.org/2000/svg" width="12" height="13" viewBox="0 0 12 13" fill="none">
						<path
							d="M11.4707 2.61266C11.389 2.53194 11.2909 2.46782 11.1822 2.42421C11.0735 2.38059 10.9565 2.3584 10.8384 2.35898H9.93274V1.71766C9.93482 1.577 9.90643 1.43744 9.84934 1.30758C9.79225 1.17771 9.70767 1.0603 9.60081 0.962588C9.49754 0.861539 9.37352 0.781587 9.23637 0.727661C9.09922 0.673734 8.95186 0.646976 8.80338 0.649036H8.3521C8.20356 0.647073 8.05617 0.673949 7.91902 0.728005C7.78187 0.782061 7.65787 0.862152 7.55467 0.963336C7.44777 1.06102 7.36317 1.17843 7.30607 1.3083C7.24898 1.43817 7.22062 1.57775 7.22274 1.7184V2.35972H4.51274V1.71766C4.51481 1.577 4.48643 1.43744 4.42934 1.30758C4.37225 1.17771 4.28766 1.0603 4.1808 0.962588C4.07763 0.861631 3.95373 0.781733 3.81672 0.72781C3.67972 0.673887 3.53251 0.647079 3.38416 0.649036H2.93289C2.78434 0.647073 2.63695 0.673949 2.4998 0.728005C2.36265 0.782061 2.23865 0.862152 2.13545 0.963336C2.02859 1.06105 1.94401 1.17846 1.88692 1.30833C1.82983 1.43819 1.80145 1.57775 1.80352 1.7184V2.35972H0.900184C0.782128 2.35914 0.665186 2.38134 0.556485 2.42495C0.447783 2.46857 0.349602 2.53269 0.267929 2.61341C0.182682 2.69074 0.114961 2.78371 0.0688993 2.88663C0.0228373 2.98956 -0.000601975 3.10029 1.05193e-05 3.21207V11.7663C0.00305796 11.9918 0.0987552 12.2073 0.266805 12.3671C0.434854 12.5269 0.662039 12.6185 0.900184 12.6224H10.8353C11.0739 12.6193 11.3017 12.5281 11.4704 12.3684C11.6391 12.2087 11.7354 11.9929 11.7386 11.767V3.21432C11.7397 3.10203 11.7164 2.99073 11.6704 2.88726C11.6243 2.78379 11.5563 2.69034 11.4707 2.61266ZM2.93526 11.7663H0.900184V9.8423H2.9321L2.93526 11.7663ZM2.93526 9.41426H0.900184V7.27627H2.9321L2.93526 9.41426ZM2.93526 6.84822H0.900184V4.92426H2.9321L2.93526 6.84822ZM2.77719 3.79128C2.75581 3.77199 2.73882 3.74877 2.72726 3.72303C2.71571 3.69729 2.70984 3.66958 2.71002 3.64162V1.71766C2.71083 1.66113 2.73491 1.60714 2.77712 1.56717C2.81934 1.5272 2.87635 1.50441 2.93605 1.50363H3.38416C3.44385 1.50441 3.50087 1.5272 3.54308 1.56717C3.5853 1.60714 3.60937 1.66113 3.61019 1.71766V3.64237C3.60937 3.69889 3.5853 3.75288 3.54308 3.79285C3.50087 3.83282 3.44385 3.85561 3.38416 3.85639H2.93289C2.90304 3.85662 2.87345 3.85102 2.84599 3.83995C2.81852 3.82888 2.79376 3.81257 2.77324 3.79203L2.77719 3.79128ZM5.64526 11.7655H3.38416V9.8423H5.6421L5.64526 11.7655ZM5.64526 9.41351H3.38416V7.27627H5.6421L5.64526 9.41351ZM5.64526 6.84747H3.38416V4.92426H5.6421L5.64526 6.84747ZM8.35527 11.7648H6.09337V9.8423H8.35131L8.35527 11.7648ZM8.35527 9.41276H6.09337V7.27627H8.35131L8.35527 9.41276ZM8.35527 6.84673H6.09337V4.92426H8.35131L8.35527 6.84673ZM8.1972 3.78979C8.17543 3.77105 8.15789 3.74833 8.14566 3.72299C8.13344 3.69765 8.12677 3.67022 8.12607 3.64237V1.71766C8.12669 1.66107 8.1507 1.60698 8.19296 1.56696C8.23521 1.52695 8.29235 1.50422 8.3521 1.50363H8.80338C8.86313 1.50422 8.92027 1.52695 8.96252 1.56696C9.00478 1.60698 9.02879 1.66107 9.02941 1.71766V3.64237C9.02879 3.69895 9.00478 3.75305 8.96252 3.79306C8.92027 3.83307 8.86313 3.8558 8.80338 3.85639H8.3521C8.32225 3.85666 8.29265 3.85109 8.26518 3.84001C8.2377 3.82894 8.21295 3.8126 8.19246 3.79203L8.1972 3.78979ZM10.84 11.764H8.80338V9.8423H10.8353L10.84 11.764ZM10.84 9.41201H8.80338V7.27627H10.8353L10.84 9.41201ZM10.84 6.84598H8.80338V4.92426H10.8353L10.84 6.84598Z"
							fill="#333333"
						/>
					</svg>
				{/if}{startTrip ? format(startTrip, "EEEEEE d/MM") : ""}
			</div>
			<span />
			<div class="end">
				{#if endTrip}
					<svg xmlns="http://www.w3.org/2000/svg" width="12" height="13" viewBox="0 0 12 13" fill="none">
						<path
							d="M11.4707 2.61266C11.389 2.53194 11.2909 2.46782 11.1822 2.42421C11.0735 2.38059 10.9565 2.3584 10.8384 2.35898H9.93274V1.71766C9.93482 1.577 9.90643 1.43744 9.84934 1.30758C9.79225 1.17771 9.70767 1.0603 9.60081 0.962588C9.49754 0.861539 9.37352 0.781587 9.23637 0.727661C9.09922 0.673734 8.95186 0.646976 8.80338 0.649036H8.3521C8.20356 0.647073 8.05617 0.673949 7.91902 0.728005C7.78187 0.782061 7.65787 0.862152 7.55467 0.963336C7.44777 1.06102 7.36317 1.17843 7.30607 1.3083C7.24898 1.43817 7.22062 1.57775 7.22274 1.7184V2.35972H4.51274V1.71766C4.51481 1.577 4.48643 1.43744 4.42934 1.30758C4.37225 1.17771 4.28766 1.0603 4.1808 0.962588C4.07763 0.861631 3.95373 0.781733 3.81672 0.72781C3.67972 0.673887 3.53251 0.647079 3.38416 0.649036H2.93289C2.78434 0.647073 2.63695 0.673949 2.4998 0.728005C2.36265 0.782061 2.23865 0.862152 2.13545 0.963336C2.02859 1.06105 1.94401 1.17846 1.88692 1.30833C1.82983 1.43819 1.80145 1.57775 1.80352 1.7184V2.35972H0.900184C0.782128 2.35914 0.665186 2.38134 0.556485 2.42495C0.447783 2.46857 0.349602 2.53269 0.267929 2.61341C0.182682 2.69074 0.114961 2.78371 0.0688993 2.88663C0.0228373 2.98956 -0.000601975 3.10029 1.05193e-05 3.21207V11.7663C0.00305796 11.9918 0.0987552 12.2073 0.266805 12.3671C0.434854 12.5269 0.662039 12.6185 0.900184 12.6224H10.8353C11.0739 12.6193 11.3017 12.5281 11.4704 12.3684C11.6391 12.2087 11.7354 11.9929 11.7386 11.767V3.21432C11.7397 3.10203 11.7164 2.99073 11.6704 2.88726C11.6243 2.78379 11.5563 2.69034 11.4707 2.61266ZM2.93526 11.7663H0.900184V9.8423H2.9321L2.93526 11.7663ZM2.93526 9.41426H0.900184V7.27627H2.9321L2.93526 9.41426ZM2.93526 6.84822H0.900184V4.92426H2.9321L2.93526 6.84822ZM2.77719 3.79128C2.75581 3.77199 2.73882 3.74877 2.72726 3.72303C2.71571 3.69729 2.70984 3.66958 2.71002 3.64162V1.71766C2.71083 1.66113 2.73491 1.60714 2.77712 1.56717C2.81934 1.5272 2.87635 1.50441 2.93605 1.50363H3.38416C3.44385 1.50441 3.50087 1.5272 3.54308 1.56717C3.5853 1.60714 3.60937 1.66113 3.61019 1.71766V3.64237C3.60937 3.69889 3.5853 3.75288 3.54308 3.79285C3.50087 3.83282 3.44385 3.85561 3.38416 3.85639H2.93289C2.90304 3.85662 2.87345 3.85102 2.84599 3.83995C2.81852 3.82888 2.79376 3.81257 2.77324 3.79203L2.77719 3.79128ZM5.64526 11.7655H3.38416V9.8423H5.6421L5.64526 11.7655ZM5.64526 9.41351H3.38416V7.27627H5.6421L5.64526 9.41351ZM5.64526 6.84747H3.38416V4.92426H5.6421L5.64526 6.84747ZM8.35527 11.7648H6.09337V9.8423H8.35131L8.35527 11.7648ZM8.35527 9.41276H6.09337V7.27627H8.35131L8.35527 9.41276ZM8.35527 6.84673H6.09337V4.92426H8.35131L8.35527 6.84673ZM8.1972 3.78979C8.17543 3.77105 8.15789 3.74833 8.14566 3.72299C8.13344 3.69765 8.12677 3.67022 8.12607 3.64237V1.71766C8.12669 1.66107 8.1507 1.60698 8.19296 1.56696C8.23521 1.52695 8.29235 1.50422 8.3521 1.50363H8.80338C8.86313 1.50422 8.92027 1.52695 8.96252 1.56696C9.00478 1.60698 9.02879 1.66107 9.02941 1.71766V3.64237C9.02879 3.69895 9.00478 3.75305 8.96252 3.79306C8.92027 3.83307 8.86313 3.8558 8.80338 3.85639H8.3521C8.32225 3.85666 8.29265 3.85109 8.26518 3.84001C8.2377 3.82894 8.21295 3.8126 8.19246 3.79203L8.1972 3.78979ZM10.84 11.764H8.80338V9.8423H10.8353L10.84 11.764ZM10.84 9.41201H8.80338V7.27627H10.8353L10.84 9.41201ZM10.84 6.84598H8.80338V4.92426H10.8353L10.84 6.84598Z"
							fill="#333333"
						/>
					</svg>
				{/if}{endTrip ? format(endTrip, "EEEEEE d/MM") : ""}
			</div>

			{#if openDate}
				<div class="openDate" transition:fade|local>
					<div class="calendars">
						<Calendar
							on:preview={prevMonth}
							on:next={nextMonth}
							on:trip={choiceTrip}
							{yes}
							date={currentDay}
							first={startTrip}
							second={endTrip}
						/>
						<Calendar
							on:preview={prevMonth}
							on:next={nextMonth}
							on:trip={choiceTrip}
							{yes}
							date={addMonths(currentDay, 1)}
							first={startTrip}
							second={endTrip}
						/>
					</div>
					<svg xmlns="http://www.w3.org/2000/svg" width="690" height="2" viewBox="0 0 690 2" fill="none">
						<path d="M0 1H689.5" stroke="#BDBDBD" stroke-width="0.5" />
					</svg>
					<div class="bottom">
						<div class="chekbox"><input type="checkbox" bind:checked={yes} /> Без конечной даты</div>

						<button on:click|preventDefault|stopPropagation={() => (openDate = false)}> Готово </button>
					</div>
				</div>
			{/if}
		</div>
	</div>
	<button
		on:click={() => {
			openDate = false;
			openFirst = false;
			openSecond = false;
			if (!firstCity?.trim()?.length) {
				errorFirst = true;
				alert("Не все поля заполнены");
				return;
			} else if (!secondCity?.trim()?.length) {
				errorSecond = true;
			} else if (!startTrip) {
				errorStart = true;
			} else if (yes && !endTrip) {
				errorEnd = true;
			}

			showGood = true;
			globalEmit("search", {
				firstCity,
				secondCity,
				startTrip,
				endTrip,
			});
		}}>Найти</button
	>
</div>
{#if showGood}
	<div class="bg" transition:fly|local on:click|stopPropagation|preventDefault={() => (showGood = false)} />
	<div class="modal" transition:fade|local>
		<div>{firstCity}</div>
		<div>{secondCity}</div>
		<div>{format(startTrip, "EEEEEE d/MM/yy")}</div>
		<div>{format(endTrip, "EEEEEE d/MM/yy")}</div>
	</div>
{/if}

<style lang="scss">
	.search-box {
		display: flex;
		gap: 16px;
		flex-shrink: 0;
		align-items: flex-end;
		.error {
			border: 1px solid red;
		}
		.place {
			display: flex;
			gap: 10px;
			position: relative;
			label {
				display: flex;
				flex-direction: column;
				align-items: flex-start;
				.title {
					color: var(--Gray-1, #333);
					font-family: Roboto;
					font-size: 14px;
					font-style: normal;
					font-weight: 500;
					line-height: normal;
					padding-bottom: 9px;
				}

				.select {
					display: inline-flex;
					padding: 5.186px 0px 6.05px 18px;
					justify-content: flex-start;
					align-items: center;
					gap: 8px;
					border-radius: 5px;
					background: var(--Gray-5, #e0e0e0);
					min-width: 201px;
					height: 32px;
					pointer-events: all;
					cursor: pointer;
					position: relative;
				}
				input {
					background: transparent;
					border: none;
					outline: none;
				}
			}
		}
		.times {
			display: flex;
			flex-direction: column;
			align-items: flex-start;
			color: var(--Gray-1, #333);
			font-family: Roboto;
			font-size: 14px;
			font-style: normal;
			font-weight: 600;
			line-height: normal;
			.title {
				color: var(--Gray-1, #333);
				font-family: Roboto;
				font-size: 14px;
				font-style: normal;
				font-weight: 500;
				line-height: normal;
				padding-bottom: 9px;
			}

			.date {
				display: flex;
				width: 386px;
				padding: 5.186px 0px 6.05px 18px;

				height: 32px;
				justify-content: space-around;
				align-items: center;
				gap: 10px;
				border-radius: 5px;
				background: var(--Gray-5, #e0e0e0);
				pointer-events: all;
				cursor: pointer;
				position: relative;
				text-transform: capitalize;
				span {
					width: 20.4px;
					height: 0.838px;
					background: #bdbdbd;
				}
			}
			.openDate {
				display: flex;
				padding: 33px 56px;
				position: absolute;
				top: 54px;
				left: 0;
				display: flex;
				width: 802px;
				height: 442px;
				flex-direction: column;
				justify-content: space-evenly;
				align-items: center;
				flex-shrink: 0;
				border-radius: 5px;
				background: var(--Gray-5, #e0e0e0);
				.calendars {
					display: flex;
					width: 100%;
					gap: 87px;
				}
				.bottom {
					display: flex;
					width: 100%;
					justify-content: space-between;
					align-items: end;
				}
			}
		}
		button {
			display: flex;
			width: 153px;
			padding: 14px 40px;
			justify-content: center;
			align-items: center;
			gap: 10px;
			border-radius: 5px;
			background: var(--Gray-1, #333);
			color: var(--Gray-6, #f2f2f2);
			text-align: center;
			font-family: Roboto;
			font-size: 16px;
			font-style: normal;
			font-weight: 700;
			line-height: normal;
			height: 47px;
			pointer-events: all;
			cursor: pointer;
		}
	}
	.bg {
		background: #333;
		opacity: 0.3;
		width: 100vw;
		height: 100vh;
		left: 0;
		position: fixed;
		top: 0;
	}
	.modal {
		position: absolute;
		left: 50%;
		top: 50%;
		transform: translate(-50%, -50%);
		background-color: #fff;
		width: 300px;
		height: 200px;
		border-radius: 10px;
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
	}
</style>
