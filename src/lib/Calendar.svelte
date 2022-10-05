<script>
	import { addMonths as add_months, isSameMonth as is_same_month } from 'date-fns';
	import Grid from './Grid.svelte';
	import Header from './Header.svelte';
	let view_date = new Date();
	let today_date = new Date();

	const set_next_month = () => (view_date = add_months(view_date, 1));
	const set_prev_month = () => (view_date = add_months(view_date, -1));
	const set_today = () => (view_date = new Date(today_date));
	$: view_is_same_month = is_same_month(view_date, today_date);
</script>

<Header
	{view_date}
	{today_date}
	{view_is_same_month}
	on:today={set_today}
	on:prev_month={set_prev_month}
	on:next_month={set_next_month}
/>
<Grid {view_date} {today_date} {view_is_same_month} let:date let:is_today>
	<slot {date} {is_today}>{date}</slot>
</Grid>
