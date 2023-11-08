<svelte:options customElement="search-box" />

<script>
// @ts-nocheck

	import City from "./City.svelte";
	import { getContext } from "svelte";
	import { setDefaultOptions } from "date-fns";
	import { ru } from "date-fns/locale";
	import { setContext } from "svelte";
	import { writable } from "svelte/store";

	setDefaultOptions({ locale: ru, weekStartsOn: 1 });

	const today = writable(new Date());

	setContext("today", today);
	import { addDays, eachDayOfInterval, startOfMonth, startOfWeek, format, addMonths } from "date-fns";

	let start = startOfWeek(startOfMonth($today));
	let end = addDays(start, 41);
	let startNext = addMonths(startOfMonth($today),1);
	let endNext = addDays(startOfWeek(startNext), 41);
	const days = eachDayOfInterval({
		start,
		end,
	});
	const daysNext = eachDayOfInterval({
		start: startOfWeek(startNext),
		end: endNext,
	});
	let city;
	let firstCity;
	let secondCity;
	let openDate = false;
	let open = false;
	let yes = true;
	let startTrip, endTrip;
	const selectCity = (e) => {
		const { iso2 } = e.detail;
		city = iso2;
		console.log(e.detail);
	};
	$: console.log(addMonths(startOfMonth($today),1));
	const choiceTrip = (item) => {
		if (startTrip != undefined && yes) {
			endTrip = item;
		} else if(endTrip==undefined ) {
			startTrip = item;
		}
	};

	$: !yes ? (endTrip = "") : endTrip;
</script>

<div class="search-box">
	<div class="place">
		<label>
			<div class="title">Откуда</div>
			<!-- svelte-ignore a11y-click-events-have-key-events -->
			<!-- svelte-ignore a11y-no-static-element-interactions -->
			<div class="select" on:click={() => (open = true)}>
				<svg xmlns="http://www.w3.org/2000/svg" width="11" height="13" viewBox="0 0 11 13" fill="none">
					<path
						d="M5.48161 0.0561523C7.00223 0.0561523 8.30212 0.474642 9.38127 1.31162C10.4604 2.1486 11 3.16356 11 4.35649C11 4.95296 10.8099 5.64082 10.4298 6.42008C10.0496 7.19933 9.58974 7.92087 9.05017 8.58468C8.51059 9.24849 7.97101 9.87382 7.43144 10.4607C6.89186 11.0475 6.43813 11.5141 6.07023 11.8604L5.48161 12.3511C5.33445 12.2356 5.13824 12.0625 4.89298 11.8316C4.64771 11.6007 4.21237 11.1533 3.58696 10.4895C2.96154 9.82571 2.40357 9.18114 1.91304 8.55582C1.42252 7.93049 0.981048 7.22338 0.588629 6.43451C0.19621 5.64563 0 4.95296 0 4.35649C0 3.16356 0.533445 2.1486 1.60033 1.31162C2.66722 0.474642 3.96098 0.0561523 5.48161 0.0561523ZM5.48161 5.88614C6.02118 5.88614 6.48718 5.73702 6.8796 5.43879C7.27202 5.14056 7.46823 4.77979 7.46823 4.35649C7.46823 3.93319 7.27202 3.57242 6.8796 3.27419C6.48718 2.97596 6.02118 2.82684 5.48161 2.82684C4.94203 2.82684 4.48216 2.97596 4.10201 3.27419C3.72185 3.57242 3.53177 3.93319 3.53177 4.35649C3.53177 4.77979 3.72185 5.14056 4.10201 5.43879C4.48216 5.73702 4.94203 5.88614 5.48161 5.88614Z"
						fill="#333333"
					/>
				</svg>
				<input type="text" bind:value={firstCity} />
				{#if open}
					<City
						on:selected={(e) => {
							const { iso2 } = e.detail;
							firstCity = iso2;
							// console.log(iso2)
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
			<div class="select" on:click={() => (open = true)}>
				<svg xmlns="http://www.w3.org/2000/svg" width="11" height="13" viewBox="0 0 11 13" fill="none">
					<path
						d="M5.48161 0.0561523C7.00223 0.0561523 8.30212 0.474642 9.38127 1.31162C10.4604 2.1486 11 3.16356 11 4.35649C11 4.95296 10.8099 5.64082 10.4298 6.42008C10.0496 7.19933 9.58974 7.92087 9.05017 8.58468C8.51059 9.24849 7.97101 9.87382 7.43144 10.4607C6.89186 11.0475 6.43813 11.5141 6.07023 11.8604L5.48161 12.3511C5.33445 12.2356 5.13824 12.0625 4.89298 11.8316C4.64771 11.6007 4.21237 11.1533 3.58696 10.4895C2.96154 9.82571 2.40357 9.18114 1.91304 8.55582C1.42252 7.93049 0.981048 7.22338 0.588629 6.43451C0.19621 5.64563 0 4.95296 0 4.35649C0 3.16356 0.533445 2.1486 1.60033 1.31162C2.66722 0.474642 3.96098 0.0561523 5.48161 0.0561523ZM5.48161 5.88614C6.02118 5.88614 6.48718 5.73702 6.8796 5.43879C7.27202 5.14056 7.46823 4.77979 7.46823 4.35649C7.46823 3.93319 7.27202 3.57242 6.8796 3.27419C6.48718 2.97596 6.02118 2.82684 5.48161 2.82684C4.94203 2.82684 4.48216 2.97596 4.10201 3.27419C3.72185 3.57242 3.53177 3.93319 3.53177 4.35649C3.53177 4.77979 3.72185 5.14056 4.10201 5.43879C4.48216 5.73702 4.94203 5.88614 5.48161 5.88614Z"
						fill="#333333"
					/>
				</svg>
				{#if open}
					<City
						on:selected={(e) => {
							const { iso2 } = e.detail;
							secondCity = iso2;
							console.log(secondCity);
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
			on:click={() => {
				openDate = true;
			}}
		>
			<div class="start">{startTrip ? format(startTrip, "EEEEEE d/MM") : ""}</div>
			-
			<div class="end">{endTrip ? format(endTrip, "EEEEEE d/MM") : ""}</div>

			{#if openDate}
				<div class="openDate">
					<div class="calendars">
						<div class="calendar">
							<div class="month">
								<svg
									xmlns="http://www.w3.org/2000/svg"
									width="7"
									height="13"
									viewBox="0 0 7 13"
									fill="none"
								>
									<path
										d="M5.99981 11.657L1.7998 6.50002L5.99981 1.34302"
										stroke="#333333"
										stroke-width="2"
										stroke-linecap="round"
										stroke-linejoin="round"
									/>
								</svg>{format($today, "LLLL")}
								<svg
									xmlns="http://www.w3.org/2000/svg"
									width="7"
									height="13"
									viewBox="0 0 7 13"
									fill="none"
								>
									<path
										d="M1 11.657L5.2 6.50002L1 1.34302"
										stroke="#333333"
										stroke-width="2"
										stroke-linecap="round"
										stroke-linejoin="round"
									/>
								</svg>
							</div>

							<div class="days">
								<div class="day">Пн</div>
								<div class="day">Вт</div>
								<div class="day">Ср</div>
								<div class="day">Чт</div>
								<div class="day">Пт</div>
								<div class="day">Сб</div>
								<div class="day">Вс</div>
							</div>
							<div class="grid-calendar">
								{#each days as day}
									<div class="item" on:click={() => choiceTrip(day)}>
										{format(day, "d")}
									</div>
								{/each}
							</div>
						</div>
						<div class="calendar">
							<div class="month">
								<svg
									xmlns="http://www.w3.org/2000/svg"
									width="7"
									height="13"
									viewBox="0 0 7 13"
									fill="none"
								>
									<path
										d="M5.99981 11.657L1.7998 6.50002L5.99981 1.34302"
										stroke="#333333"
										stroke-width="2"
										stroke-linecap="round"
										stroke-linejoin="round"
									/>
								</svg>{format(startNext, "LLLL")}
								<svg
									xmlns="http://www.w3.org/2000/svg"
									width="7"
									height="13"
									viewBox="0 0 7 13"
									fill="none"
								>
									<path
										d="M1 11.657L5.2 6.50002L1 1.34302"
										stroke="#333333"
										stroke-width="2"
										stroke-linecap="round"
										stroke-linejoin="round"
									/>
								</svg>
							</div>

							<div class="days">
								<div class="day">Пн</div>
								<div class="day">Вт</div>
								<div class="day">Ср</div>
								<div class="day">Чт</div>
								<div class="day">Пт</div>
								<div class="day">Сб</div>
								<div class="day">Вс</div>
							</div>
							<div class="grid-calendar">
								{#each daysNext as day}
									<div class="item" on:click={() => choiceTrip(day)}>
										{format(day, "d")}
									</div>
								{/each}
							</div>
						</div>
					</div>
					<svg xmlns="http://www.w3.org/2000/svg" width="690" height="2" viewBox="0 0 690 2" fill="none">
						<path d="M0 1H689.5" stroke="#BDBDBD" stroke-width="0.5" />
					</svg>
					<div class="bottom">
						<div class="chekbox"><input type="checkbox" bind:checked={yes} /> Без конечной даты</div>

						<button> Готово </button>
					</div>
				</div>
			{/if}
		</div>
	</div>
	<button>Найти</button>
</div>

<style lang="scss">
	.search-box {
		display: flex;
		gap: 16px;
		flex-shrink: 0;
		align-items: flex-end;

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
				align-items: flex-start;
				gap: 10px;
				border-radius: 5px;
				background: var(--Gray-5, #e0e0e0);
				pointer-events: all;
				cursor: pointer;
				position: relative;
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
					.calendar {
						.month {
							display: flex;
							justify-content: space-between;
							padding-bottom: 40px;
						}
						.days {
							color: var(--Gray-1, #333);
							font-family: Roboto;
							font-size: 15px;
							font-style: normal;
							font-weight: 500;
							line-height: normal;
							padding-bottom: 18px;
						}
					}
				}
				.bottom {
					display: flex;
					width: 100%;
					justify-content: space-between;
					align-items: end;
				}
			}
			.grid-calendar {
				display: grid;
				grid-template-columns: repeat(7, 1fr);
				grid-template-rows: repeat(6, 1fr);
				justify-content: flex-end;
				align-items: flex-start;
				grid-gap: 1px;
				width: 100%;
				.item {
					padding: 6px 11px;
					width: -webkit-fill-available;
					height: -webkit-fill-available;
					color: var(--Gray-1, #333);
					font-family: Roboto;
					font-size: 15px;
					font-style: normal;
					font-weight: 500;
					line-height: normal;
					&.notNow {
						color: #bdbdbd;
						text-align: right;
						font-family: Helvetica;
						font-size: 16px;
						font-style: normal;
						font-weight: 400;
						line-height: normal;
					}
					&.today {
						color: #ff0000 !important;
						text-align: right;
						font-family: Helvetica;
						font-size: 16px;
						font-style: normal;
						font-weight: 400;
						line-height: normal;
					}
					.month {
						padding-left: 5px;
						text-transform: lowercase;
					}
				}
				.weekend {
					background: #f5f5f5;
					color: #7b7b7b;
					text-align: right;
					font-family: Helvetica;
					font-size: 16px;
					font-style: normal;
					font-weight: 400;
					line-height: normal;
				}
			}
			.days {
				display: grid;
				grid-template-columns: repeat(7, 1fr);
				grid-template-rows: 1fr;
				justify-items: end;
				align-items: end;
				width: 100%;
				height: auto;
				color: #272727;
				font-family: Helvetica;
				font-size: 16px;
				font-style: normal;
				font-weight: 400;
				line-height: normal;
				.day {
					padding: 0 10px;
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
</style>
