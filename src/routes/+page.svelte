<script lang="ts">
	import ClockDigit from '$lib/components/ClockDigit.svelte';

	let digit0: number | null = $state(null);
	let digit1: number | null = $state(null);
	let digit2: number | null = $state(null);
	let digit3: number | null = $state(null);

	function updateTime() {
		const now = new Date();
		const hours = now.getHours();
		const minutes = now.getMinutes();

		// Convert to 12-hour format
		const hour12 = hours === 0 ? 12 : hours > 12 ? hours - 12 : hours;
		const minuteStr = minutes.toString().padStart(2, '0');

		// Parse hour (no leading zero)
		if (hour12 < 10) {
			digit0 = null;
			digit1 = hour12;
		} else {
			digit0 = Math.floor(hour12 / 10);
			digit1 = hour12 % 10;
		}

		// Parse minutes (with leading zero)
		digit2 = parseInt(minuteStr[0]);
		digit3 = parseInt(minuteStr[1]);
	}

	$effect(() => {
		// Update time immediately
		updateTime();

		// Update time every second
		const interval = setInterval(updateTime, 1000);

		// Cleanup interval on component unmount
		return () => clearInterval(interval);
	});
</script>

<div class="container mx-auto flex min-h-screen items-center justify-center p-4">
	<div class="grid grid-cols-4 grid-rows-1">
		<ClockDigit digit={digit0} transitionTime={10000} />
		<ClockDigit digit={digit1} transitionTime={10000} />
		<ClockDigit digit={digit2} transitionTime={10000} />
		<ClockDigit digit={digit3} transitionTime={10000} />
	</div>
</div>
