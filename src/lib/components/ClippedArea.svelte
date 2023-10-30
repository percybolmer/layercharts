<script lang="ts">
	import {
		Chart,
		Svg,
		Area,
		Highlight,
		Tooltip,
		LinearGradient,
		RectClipPath
	} from 'layerchart';
	import { format, PeriodType } from 'svelte-ux';
	import { scaleTime } from 'd3-scale';
	import { format as formatDate } from 'date-fns';
	import { data } from './dateSeries.js';
</script>

<h1 class="text-3xl font-bold underline text-blue-400">
    Hello LayerCharts!
  </h1>
<div class="h-[300px] border rounded bg-slate-700">
	<Chart
		{data}
		x="date"
		xScale={scaleTime()}
		y="value"
		yDomain={[0, null]}
		yNice
		padding={{ top: 48 }}
		tooltip={{ snapToDataX: true }}
		let:width
		let:height
		let:tooltip
	>
		<Svg>
			<!-- This Class is for styling the INFILL area below the value, so Y0 -> Y current value -->
			<LinearGradient class="from-accent-500/50 to-accent-500/0" let:url>
				<!-- This Class styles is the small "border" that highlights the Graphs value AFTER hover location -->
				<Area line={{ class: 'stroke-2 stroke-accent-500 opacity-20' }} fill={url} />
				<RectClipPath x={0} y={0} width={tooltip.data ? tooltip.left : width} {height} spring={false}>
					<!-- This class styles is the small "border" that highlights the Graphs current value UP UNTIL Hover location -->
					<Area line={{ class: 'stroke-2 stroke-accent-500' }} fill={url} />
				</RectClipPath>
			</LinearGradient>
			<!-- This is the LINE Showing the current value at Hover location -->
			<Highlight points lines={{ class: 'stroke-accent-500 [stroke-dasharray:unset]' }} />
		</Svg>
		<!-- This tooltip shows the date at hover location in the top right corner -->
		<Tooltip top={4} left={4} variant="none" class="text-sm font-semibold leading-3" let:data>
			{formatDate(data.date, 'eee, MMMM do')}
		</Tooltip>
		<!-- This tooltip shows the current value at the Hover location -->
		<Tooltip
			top={48}
			leftOffset={4}
			variant="none"
			class="text-sm font-semibold text-accent-700 leading-3"
			let:data
		>
			{format(data.value, 'currency')}
		</Tooltip>
	</Chart>
</div>

