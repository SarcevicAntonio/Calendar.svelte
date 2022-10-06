<script>
	import Calendar from '$lib';
</script>

<Calendar let:date let:is_today>
	<div class="date" class:is_today>
		{date}
	</div>
	<div class="bar" />
</Calendar>

<style>
	.date {
		border-radius: 99999px;
		aspect-ratio: 1/1;
		width: 1.3em;
		margin: auto;
	}
	.is_today {
		background-color: red;
		color: white;
	}
	.bar {
		height: 1em;
		background-color: yellowgreen;
	}
</style>
