<script>
	import Calendar from '$lib';
</script>

<Calendar let:date let:is_today>
	<div class:is_today>
		{date}
	</div>
</Calendar>

<style>
	div {
		border-radius: 99999px;
		aspect-ratio: 1/1;
		width: 1em;
		margin: auto;
	}
	.is_today {
		background-color: grey;
		color: white;
	}
</style>
