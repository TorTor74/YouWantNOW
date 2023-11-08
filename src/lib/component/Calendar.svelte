<script>
	import { createEventDispatcher } from "svelte";
	import {
		addDays,
		eachDayOfInterval,
		startOfMonth,
		startOfWeek,
		format,
		addMonths,
		subMonths,
		isSameDay,
		isWithinInterval,
		isBefore,
	} from "date-fns";
	export let date;
	export let yes;
	export let first;
	export let second;
	let today = new Date();
	let startTrip, endTrip;

	const choiceTrip = (item) => {
		emit("trip", item);
	};
	const emit = createEventDispatcher();

	$: start = startOfWeek(startOfMonth(date));
	$: end = addDays(start, 41);
	$: days = eachDayOfInterval({
		start,
		end,
	});

	const isBetween = (day, start, end) => {
		if (!start || !end) return false;
		try {
			return isWithinInterval(day, { start, end });
		} catch (error) {
			return false;
		}
	};
</script>

<div class="calendar">
	<div class="month">
		<div
			class="preview"
			on:click={() => {
				emit("preview");
			}}
		>
			<svg xmlns="http://www.w3.org/2000/svg" width="7" height="13" viewBox="0 0 7 13" fill="none">
				<path
					d="M5.99981 11.657L1.7998 6.50002L5.99981 1.34302"
					stroke="#333333"
					stroke-width="2"
					stroke-linecap="round"
					stroke-linejoin="round"
				/>
			</svg>
		</div>
		{format(date, "LLLL")}
		<div
			class="next"
			on:click={() => {
				emit("next");
			}}
		>
			<svg xmlns="http://www.w3.org/2000/svg" width="7" height="13" viewBox="0 0 7 13" fill="none">
				<path
					d="M1 11.657L5.2 6.50002L1 1.34302"
					stroke="#333333"
					stroke-width="2"
					stroke-linecap="round"
					stroke-linejoin="round"
				/>
			</svg>
		</div>
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
			<div
				class="item"
				on:click|preventDefault|stopPropagation={() => choiceTrip(day)}
				class:before={isBefore(day, today)}
				class:selected={isSameDay(day, first) || isSameDay(day, second)}
				class:between={isBetween(day, first, second)}
			>
				{format(day, "d")}
			</div>
		{/each}
	</div>
</div>

<style lang="scss">
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

			&.selected {
				background-color: #272727;
			}
			&.between {
				background-color: #7b7b7b;
			}
			&.before {
				pointer-events: none;
				cursor: default;
				opacity: 0.7;
			}
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
</style>
