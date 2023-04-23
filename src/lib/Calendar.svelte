<script>
	import {
		addMonths,
		getDate,
		getDay,
		getDaysInMonth,
		isSameMonth,
		isSameYear,
		setDate
	} from 'date-fns';

	export let start_on_sunday = false;
	export let view_date = new Date();
	let today_date = new Date();

	const WEEK_DAYS = ['M', 'D', 'M', 'D', 'F', 'S', 'S'];
	const set_next_month = () => (view_date = addMonths(view_date, 1));
	const set_prev_month = () => (view_date = addMonths(view_date, -1));
	const set_today = () => (view_date = new Date(today_date));

	if (start_on_sunday) {
		const s = WEEK_DAYS.splice(WEEK_DAYS.length - 1, 1);
		WEEK_DAYS.unshift(s[0]);
	}

	$: view_month_string = view_date.toLocaleDateString(undefined, { month: 'long' });
	$: view_year_string = view_date.toLocaleDateString(undefined, { year: 'numeric' });
	$: view_is_different_year = !isSameYear(view_date, today_date);
	$: view_is_same_month = isSameMonth(view_date, today_date);
	$: days_in_month = getDaysInMonth(view_date);
	$: first_week_day = getDay(setDate(view_date, 1)) || (start_on_sunday ? 0 : 7);
</script>

<div class="header">
	<span class="month">
		{view_month_string}
		{#if view_is_different_year}
			{view_year_string}
		{/if}
	</span>

	<div>
		<button on:click={set_prev_month} aria-label="Previous Month"> ⬅️ </button>
		<button on:click={set_today} aria-label="today" disabled={view_is_same_month}> 🏠 </button>
		<button on:click={set_next_month} aria-label="Next Month"> ➡️ </button>
	</div>
</div>

<div class="grid">
	{#each WEEK_DAYS as day}
		<div class="week-days">
			{day}
		</div>
	{/each}
	{#each Array(start_on_sunday ? first_week_day : first_week_day - 1) as _}
		<div class="cell" />
	{/each}
	{#each Array(days_in_month) as _, i}
		{@const date = setDate(view_date, i + 1)}
		{@const today = view_is_same_month ? getDate(today_date) === i + 1 : false}
		<div class="cell">
			<slot {date} {today}>
				{date.toLocaleDateString(undefined, { day: 'numeric' })}
			</slot>
		</div>
	{/each}
</div>

<style>
	.header {
		display: flex;
		justify-content: space-between;
		align-items: center;
		margin: var(--header-margin, 0 0 1.5rem 0);
	}

	.month {
		font-weight: var(--month-font-weight, 600);
		font-size: var(--month-font-size, 1.5rem);
	}

	.grid {
		display: grid;
		grid-template-columns: repeat(7, 1fr);
		align-items: center;
		justify-items: center;
	}

	.week-days {
		color: var(--week-days-color, gray);
		font-weight: var(--week-days-font-weight, 600);
		margin: var(--week-days-margin, 0 0 0.5rem 0);
	}

	.cell {
		height: var(--cell-height, clamp(4rem, 7vw, 8rem));
		text-align: center;
		width: 100%;
	}
</style>
