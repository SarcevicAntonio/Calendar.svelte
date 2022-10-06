<script>
	import Calendar from '$lib';
	import { isSameDay as is_same_day } from 'date-fns';

	// the component doesn't care about data structure.
	// it's your job to create the marking of days for example.
	const data = [
		{
			date: new Date('2022-10-01'),
			color: '#F44336'
		},
		{
			date: new Date('2022-10-02'),
			color: '#F44336'
		},
		{
			date: new Date('2022-10-03'),
			color: '#F44336'
		},
		{
			date: new Date('2022-10-20'),
			color: '#8BC34A'
		},
		{
			date: new Date('2022-11-20'),
			color: '#8BC34A'
		}
	];
</script>

<a href="https://github.com/SarcevicAntonio/Calendar.svelte"><h1>Calendar.svelte</h1></a>

<Calendar let:date let:today>
	{@const day = data.find((a) => is_same_day(a.date, date))}
	<button
		on:click={() =>
			alert(
				`You selected the date ${date.toLocaleDateString(undefined)}.` +
					(day ? `\nThis date has the color ${day?.color}.` : '') +
					(today ? `\nThis date is today!` : '')
			)}
	>
		<div class="date" class:today>
			{date.toLocaleDateString(undefined, { day: 'numeric' })}
		</div>
		<div class="dot">
			{#if day}
				•
			{/if}
		</div>
		<div class="bar" style:background-color={day?.color || null} />
	</button>
</Calendar>

<style>
	.date {
		display: grid;
		place-content: center;
		border-radius: 99999px;
		aspect-ratio: 1/1;
		width: 1.3em;
		margin: auto;
	}

	.today {
		background-color: #ff6f00;
		color: white;
	}

	.bar {
		height: clamp(0.2rem, 1vw, 3rem);
	}

	.dot {
		height: 1.4em;
	}

	button {
		width: 100%;
		height: 100%;
		border: 0;
		background-color: white;
		cursor: pointer;
		transition: all 0.2s ease-in;
		border-bottom: 1px solid gray;
		padding: 0;
	}

	button:hover {
		background-color: lightgrey;
	}
</style>
