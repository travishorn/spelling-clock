<script lang="ts">
	export interface Props {
		/**
		 * Stroke color of the clock face outline.
		 * CSS color string (named, hex, rgb[a], hsl[a], etc.).
		 * Default: 'gray'
		 */
		faceStroke?: string;

		/**
		 * Fill color of the clock face.
		 * CSS color string. Use 'none' for transparent.
		 * Default: 'none'
		 */
		faceFill?: string;

		/**
		 * Stroke width of the clock face outline, in SVG user units.
		 * Default: 1
		 */
		faceStrokeWidth?: number;

		/**
		 * Stroke color used for both hour and minute hands.
		 * CSS color string.
		 * Default: 'black'
		 */
		handStroke?: string;

		/**
		 * Stroke width of the clock hands, in SVG user units.
		 * Default: 5
		 */
		handStrokeWidth?: number;

		/**
		 * Length of the hour hand as a ratio of the radius.
		 * Range: 0.0 – 1.0 (values outside range will draw outside/inside the face).
		 * Default: 0.8
		 */
		hourLength?: number;

		/**
		 * Length of the minute hand as a ratio of the radius.
		 * Range: 0.0 – 1.0.
		 * Default: 0.94
		 */
		minuteLength?: number;

		/**
		 * Rotation of the hour hand in degrees.
		 * 0° points up; rotates clockwise. Example: 3 → 90°, 6 → 180°.
		 * Default: 225
		 */
		hourRotation?: number;

		/**
		 * Rotation of the minute hand in degrees.
		 * 0° points up; rotates clockwise. Example: 15 → 90°, 30 → 180°.
		 * Default: 225
		 */
		minuteRotation?: number;

		/**
		 * Transition duration applied to both hands when their rotation changes, in milliseconds.
		 * Default: 1000
		 */
		transitionTime?: number;
	}

	let {
		faceStroke = 'gray',
		faceFill = 'none',
		faceStrokeWidth = 1,
		handStroke = 'black',
		handStrokeWidth = 5,
		hourLength = 0.9,
		minuteLength = 0.97,
		hourRotation = 225,
		minuteRotation = 225,
		transitionTime = 1000
	}: Props = $props();

	// Always-Clockwise display angles (cumulative)
	let hourAngleDisplay = $state(hourRotation);
	let minuteAngleDisplay = $state(minuteRotation);

	$effect(() => {
		const current = ((hourAngleDisplay % 360) + 360) % 360;
		const target = ((hourRotation % 360) + 360) % 360;
		const deltaCW = (target - current + 360) % 360; // 0..359
		hourAngleDisplay += deltaCW;
	});

	$effect(() => {
		const current = ((minuteAngleDisplay % 360) + 360) % 360;
		const target = ((minuteRotation % 360) + 360) % 360;
		const deltaCW = (target - current + 360) % 360; // 0..359
		minuteAngleDisplay += deltaCW;
	});
</script>

<svg viewBox="0 0 100 100" class="h-full w-full">
	<circle
		cx="50"
		cy="50"
		r="49.5"
		stroke={faceStroke}
		stroke-width={faceStrokeWidth}
		fill={faceFill}
	/>
	<circle
		cx="50"
		cy="50"
		r={handStrokeWidth / 2 - handStrokeWidth * 0.06}
		stroke="none"
		fill={handStroke}
	/>
	<line
		x1="50"
		y1="50"
		x2="50"
		y2={50 - 50 * hourLength}
		stroke={handStroke}
		stroke-width={handStrokeWidth}
		style={`transform: rotate(${hourAngleDisplay}deg); transform-origin: 50% 50%; transition: all ${transitionTime}ms ease-in-out;`}
	/>
	<line
		x1="50"
		y1="50"
		x2="50"
		y2={50 - 50 * minuteLength}
		stroke={handStroke}
		stroke-width={handStrokeWidth}
		style={`transform: rotate(${minuteAngleDisplay}deg); transform-origin: 50% 50%; transition: all ${transitionTime}ms ease-in-out;`}
	/>
</svg>
