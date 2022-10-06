<script>
	import { isSameYear as is_same_year } from 'date-fns';
	import { createEventDispatcher as create_event_dispatcher } from 'svelte';

	const dispatch = create_event_dispatcher();

	/** @type {Date}*/
	export let view_date;

	/** @type {Date} */
	export let today_date;

	/** @type {boolean} */
	export let view_is_same_month;

	$: view_month_string = view_date.toLocaleDateString(undefined, { month: 'long' });
	$: view_year_string = view_date.toLocaleDateString(undefined, { year: 'numeric' });
	$: view_is_different_year = !is_same_year(view_date, today_date);
</script>

<section>
	<h2>
		{view_month_string}
		{#if view_is_different_year}
			{view_year_string}
		{/if}
	</h2>

	<div>
		<button on:click={() => dispatch('prev_month')} aria-label="Previous Month">⬅️</button>
		<button on:click={() => dispatch('today')} aria-label="today" disabled={view_is_same_month}>
			🏠
		</button>
		<button on:click={() => dispatch('next_month')} aria-label="Next Month">➡️</button>
	</div>
</section>

<style>
	section {
		display: flex;
		justify-content: space-between;
		align-items: center;
		margin-bottom: 2em;
	}

	h2 {
		margin: 0;
	}
</style>
