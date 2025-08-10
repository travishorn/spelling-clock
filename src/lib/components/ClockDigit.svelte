<script lang="ts">
	import Clock from '$lib/components/Clock.svelte';

	export interface Props {
		/**
		 * The digit to display (0–9). Pass null/undefined to render a blank digit.
		 * Default: 0
		 */
		digit?: number | null;

		/**
		 * The transition time in milliseconds.
		 * Default: 1000
		 */
		transitionTime?: number;
	}

	let { digit, transitionTime = 1000 }: Props = $props();

	type ClockCell = { hourRotation: number; minuteRotation: number; handStroke?: string };

	// Grid: 3 columns x 6 rows (row-major order)
	const COLUMNS = 3;
	const ROWS = 6;
	const totalClocks = COLUMNS * ROWS;
	const indices = Array.from({ length: totalClocks }, (_, i) => i);

	// Common cell definitions used to build digits
	const CELL: Record<string, ClockCell> = {
		BLANK: { hourRotation: 225, minuteRotation: 225, handStroke: '#666666' },
		VERTICAL: { hourRotation: 0, minuteRotation: 180 },
		HORIZONTAL: { hourRotation: 270, minuteRotation: 90 },
		CORNER_TL: { hourRotation: 90, minuteRotation: 180 },
		CORNER_TR: { hourRotation: 270, minuteRotation: 180 },
		CORNER_BL: { hourRotation: 0, minuteRotation: 90 },
		CORNER_BR: { hourRotation: 0, minuteRotation: 270 },
		VERTICAL_HALF_UP: { hourRotation: 0, minuteRotation: 0 },
		VERTICAL_HALF_DOWN: { hourRotation: 180, minuteRotation: 180 },
		VERTICAL_DIAG_DOWN_RIGHT: { hourRotation: 135, minuteRotation: 0 },
		VERTICAL_DIAG_DOWN_LEFT: { hourRotation: 225, minuteRotation: 0 },
		VERTICAL_DIAG_UP_RIGHT: { hourRotation: 45, minuteRotation: 180 },
		VERTICAL_DIAG_UP_LEFT: { hourRotation: 315, minuteRotation: 180 }
	};

	// Definition for digit 0
	const DIGIT_0: ClockCell[] = [
		// Row 1
		CELL.CORNER_TL,
		CELL.HORIZONTAL,
		CELL.CORNER_TR,
		// Row 2
		CELL.VERTICAL,
		CELL.VERTICAL_HALF_DOWN,
		CELL.VERTICAL,
		// Row 3
		CELL.VERTICAL,
		CELL.VERTICAL,
		CELL.VERTICAL,
		// Row 4
		CELL.VERTICAL,
		CELL.VERTICAL,
		CELL.VERTICAL,
		// Row 5
		CELL.VERTICAL,
		CELL.VERTICAL_HALF_UP,
		CELL.VERTICAL,
		// Row 6
		CELL.CORNER_BL,
		CELL.HORIZONTAL,
		CELL.CORNER_BR
	];

	// Definition for digit 1
	const DIGIT_1: ClockCell[] = [
		// Row 1
		CELL.BLANK,
		CELL.CORNER_TL,
		CELL.CORNER_TR,
		// Row 2
		CELL.BLANK,
		CELL.VERTICAL,
		CELL.VERTICAL,
		// Row 3
		CELL.BLANK,
		CELL.VERTICAL,
		CELL.VERTICAL,
		// Row 4
		CELL.BLANK,
		CELL.VERTICAL,
		CELL.VERTICAL,
		// Row 5
		CELL.BLANK,
		CELL.VERTICAL,
		CELL.VERTICAL,
		// Row 6
		CELL.BLANK,
		CELL.CORNER_BL,
		CELL.CORNER_BR
	];

	// Definition for digit 2
	const DIGIT_2: ClockCell[] = [
		// Row 1
		CELL.CORNER_TL,
		CELL.HORIZONTAL,
		CELL.CORNER_TR,
		// Row 2
		CELL.CORNER_BL,
		CELL.CORNER_TR,
		CELL.VERTICAL,
		// Row 3
		CELL.CORNER_TL,
		CELL.CORNER_BR,
		CELL.VERTICAL,
		// Row 4
		CELL.VERTICAL,
		CELL.CORNER_TL,
		CELL.CORNER_BR,
		// Row 5
		CELL.VERTICAL,
		CELL.CORNER_BL,
		CELL.CORNER_TR,
		// Row 6
		CELL.CORNER_BL,
		CELL.HORIZONTAL,
		CELL.CORNER_BR
	];

	// Definition for digit 3
	const DIGIT_3: ClockCell[] = [
		// Row 1
		CELL.CORNER_TL,
		CELL.HORIZONTAL,
		CELL.CORNER_TR,
		// Row 2
		CELL.CORNER_BL,
		CELL.CORNER_TR,
		CELL.VERTICAL,
		// Row 3
		CELL.CORNER_TL,
		CELL.CORNER_BR,
		CELL.VERTICAL,
		// Row 4
		CELL.CORNER_BL,
		CELL.CORNER_TR,
		CELL.VERTICAL,
		// Row 5
		CELL.CORNER_TL,
		CELL.CORNER_BR,
		CELL.VERTICAL,
		// Row 6
		CELL.CORNER_BL,
		CELL.HORIZONTAL,
		CELL.CORNER_BR
	];

	// Definition for digit 4
	const DIGIT_4: ClockCell[] = [
		// Row 1
		CELL.CORNER_TL,
		CELL.CORNER_TR,
		CELL.CORNER_TR,
		// Row 2
		CELL.VERTICAL,
		CELL.VERTICAL,
		CELL.VERTICAL,
		// Row 3
		CELL.VERTICAL,
		CELL.VERTICAL_HALF_UP,
		CELL.VERTICAL,
		// Row 4
		CELL.CORNER_BL,
		CELL.CORNER_TR,
		CELL.VERTICAL,
		// Row 5
		CELL.BLANK,
		CELL.VERTICAL,
		CELL.VERTICAL,
		// Row 6
		CELL.BLANK,
		CELL.CORNER_BL,
		CELL.CORNER_BR
	];

	// Definition for digit 5
	const DIGIT_5: ClockCell[] = [
		// Row 1
		CELL.CORNER_TL,
		CELL.HORIZONTAL,
		CELL.CORNER_TR,
		// Row 2
		CELL.VERTICAL,
		CELL.CORNER_TL,
		CELL.CORNER_BR,
		// Row 3
		CELL.VERTICAL,
		CELL.CORNER_BL,
		CELL.CORNER_TR,
		// Row 4
		CELL.CORNER_BL,
		CELL.CORNER_TR,
		CELL.VERTICAL,
		// Row 5
		CELL.CORNER_TL,
		CELL.CORNER_BR,
		CELL.VERTICAL,
		// Row 6
		CELL.CORNER_BL,
		CELL.HORIZONTAL,
		CELL.CORNER_BR
	];

	// Definition for digit 6
	const DIGIT_6: ClockCell[] = [
		// Row 1
		CELL.CORNER_TL,
		CELL.HORIZONTAL,
		CELL.CORNER_TR,
		// Row 2
		CELL.VERTICAL,
		CELL.CORNER_TL,
		CELL.CORNER_BR,
		// Row 3
		CELL.VERTICAL,
		CELL.CORNER_BL,
		CELL.CORNER_TR,
		// Row 4
		CELL.VERTICAL,
		CELL.VERTICAL_HALF_DOWN,
		CELL.VERTICAL,
		// Row 5
		CELL.VERTICAL,
		CELL.VERTICAL_HALF_UP,
		CELL.VERTICAL,
		// Row 6
		CELL.CORNER_BL,
		CELL.HORIZONTAL,
		CELL.CORNER_BR
	];

	// Definition for digit 7
	const DIGIT_7: ClockCell[] = [
		// Row 1
		CELL.CORNER_TL,
		CELL.HORIZONTAL,
		CELL.CORNER_TR,
		// Row 2
		CELL.CORNER_BL,
		CELL.CORNER_TR,
		CELL.VERTICAL,
		// Row 3
		CELL.BLANK,
		CELL.VERTICAL,
		CELL.VERTICAL,
		// Row 4
		CELL.BLANK,
		CELL.VERTICAL,
		CELL.VERTICAL,
		// Row 5
		CELL.BLANK,
		CELL.VERTICAL,
		CELL.VERTICAL,
		// Row 6
		CELL.BLANK,
		CELL.CORNER_BL,
		CELL.CORNER_BR
	];

	// Definition for digit 8
	const DIGIT_8: ClockCell[] = [
		// Row 1
		CELL.CORNER_TL,
		CELL.HORIZONTAL,
		CELL.CORNER_TR,
		// Row 2
		CELL.VERTICAL,
		CELL.VERTICAL_HALF_DOWN,
		CELL.VERTICAL,
		// Row 3
		CELL.VERTICAL_DIAG_DOWN_RIGHT,
		CELL.VERTICAL_HALF_UP,
		CELL.VERTICAL_DIAG_DOWN_LEFT,
		// Row 4
		CELL.VERTICAL_DIAG_UP_RIGHT,
		CELL.VERTICAL_HALF_DOWN,
		CELL.VERTICAL_DIAG_UP_LEFT,
		// Row 5
		CELL.VERTICAL,
		CELL.VERTICAL_HALF_UP,
		CELL.VERTICAL,
		// Row 6
		CELL.CORNER_BL,
		CELL.HORIZONTAL,
		CELL.CORNER_BR
	];

	// Definition for digit 9
	const DIGIT_9: ClockCell[] = [
		// Row 1
		CELL.CORNER_TL,
		CELL.HORIZONTAL,
		CELL.CORNER_TR,
		// Row 2
		CELL.VERTICAL,
		CELL.VERTICAL_HALF_DOWN,
		CELL.VERTICAL,
		// Row 3
		CELL.VERTICAL,
		CELL.VERTICAL_HALF_UP,
		CELL.VERTICAL,
		// Row 4
		CELL.CORNER_BL,
		CELL.CORNER_TR,
		CELL.VERTICAL,
		// Row 5
		CELL.BLANK,
		CELL.VERTICAL,
		CELL.VERTICAL,
		// Row 6
		CELL.BLANK,
		CELL.CORNER_BL,
		CELL.CORNER_BR
	];

	const DIGITS: Record<number, ClockCell[]> = {
		0: DIGIT_0,
		1: DIGIT_1,
		2: DIGIT_2,
		3: DIGIT_3,
		4: DIGIT_4,
		5: DIGIT_5,
		6: DIGIT_6,
		7: DIGIT_7,
		8: DIGIT_8,
		9: DIGIT_9
	};

	const blankCell: ClockCell = CELL.BLANK;
	const activeDigit: ClockCell[] = $derived.by(() => {
		if (digit == null) {
			return Array.from({ length: totalClocks }, () => blankCell);
		}
		return DIGITS[digit] ?? Array.from({ length: totalClocks }, () => blankCell);
	});
</script>

<div class="grid grid-cols-3 grid-rows-6">
	{#each indices as i}
		<Clock
			hourRotation={activeDigit[i]?.hourRotation}
			minuteRotation={activeDigit[i]?.minuteRotation}
			handStroke={activeDigit[i]?.handStroke}
			{transitionTime}
		/>
	{/each}
</div>
