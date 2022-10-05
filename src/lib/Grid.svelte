<script>
	import {
		getDate as get_date,
		getDay as get_week_day,
		getDaysInMonth as get_days_in_month,
		setDate as set_date
	} from 'date-fns';

	/** @type {Date}*/
	export let view_date;

	/** @type {Date} */
	export let today_date;

	/** @type {boolean} */
	export let view_is_same_month;

	const WEEK_DAYS = ['M', 'D', 'M', 'D', 'F', 'S', 'S'];

	$: days_in_month = get_days_in_month(view_date);
	$: first_week_day = get_week_day(set_date(view_date, 1)) || 7;
</script>

<div class="grid">
	{#each WEEK_DAYS as day}
		<div class="week-days">
			{day}
		</div>
	{/each}
	{#each Array(first_week_day - 1) as pad}
		<div class="cell" />
	{/each}
	{#each Array(days_in_month) as _, i}
		{@const date = i + 1}
		{@const is_today = view_is_same_month ? get_date(today_date) === date : false}
		<div class="cell">
			<slot {date} {is_today} />
		</div>
	{/each}
</div>

<style>
	.grid {
		display: grid;
		grid-template-columns: repeat(7, 1fr);
		align-items: center;
		justify-items: center;
	}

	.week-days {
		color: gray;
		font-weight: 600;
	}

	.cell {
		height: clamp(5rem, 7vw, 7.5rem);
		text-align: center;
		width: 100%;
	}
</style>
