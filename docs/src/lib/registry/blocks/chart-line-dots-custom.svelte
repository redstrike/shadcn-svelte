<script lang="ts">
	import { LineChart, Points } from "layerchart";
	import TrendingUpIcon from "@lucide/svelte/icons/trending-up";
	import GitCommitVerticalIcon from "@lucide/svelte/icons/git-commit-vertical";
	import { scaleUtc } from "d3-scale";
	import { curveNatural } from "d3-shape";
	import * as Chart from "$lib/registry/ui/chart/index.js";
	import * as Card from "$lib/registry/ui/card/index.js";

	const chartData = [
		{ date: new Date("2024-01-01"), desktop: 186 },
		{ date: new Date("2024-02-01"), desktop: 305 },
		{ date: new Date("2024-03-01"), desktop: 237 },
		{ date: new Date("2024-04-01"), desktop: 73 },
		{ date: new Date("2024-05-01"), desktop: 209 },
		{ date: new Date("2024-06-01"), desktop: 214 },
	];

	const chartConfig = {
		desktop: { label: "Desktop", color: "var(--chart-1)" },
	} satisfies Chart.ChartConfig;
</script>

<Card.Root>
	<Card.Header>
		<Card.Title>Line Chart - Dots Custom</Card.Title>
		<Card.Description>Showing total visitors for the last 6 months</Card.Description>
	</Card.Header>
	<Card.Content>
		<Chart.Container config={chartConfig}>
			<LineChart
				data={chartData}
				x="date"
				xScale={scaleUtc()}
				axis="x"
				series={[
					{
						key: "desktop",
						label: "Desktop",
						color: chartConfig.desktop.color,
					},
				]}
				props={{
					spline: { curve: curveNatural, motion: "tween", strokeWidth: 2 },
					highlight: {
						points: {
							motion: "none",
							r: 3,
						},
					},
					xAxis: {
						format: (v: Date) => v.toLocaleDateString("en-US", { month: "short" }),
					},
				}}
			>
				{#snippet tooltip()}
					<Chart.Tooltip hideLabel />
				{/snippet}
				{#snippet points({ visibleSeries, getPointsProps })}
					{#each visibleSeries as s, i (i)}
						<Points {...getPointsProps(s, i)}>
							{#snippet children({ points })}
								{#each points as p, i (i)}
									{@const r = 24}
									<GitCommitVerticalIcon
										x={p.x - r / 2}
										y={p.y - r / 2}
										width={r}
										height={r}
										fill="var(--background)"
										color="var(--color-desktop)"
									/>
								{/each}
							{/snippet}
						</Points>
					{/each}
				{/snippet}
			</LineChart>
		</Chart.Container>
	</Card.Content>
	<Card.Footer>
		<div class="flex w-full items-start gap-2 text-sm">
			<div class="grid gap-2">
				<div class="flex items-center gap-2 font-medium leading-none">
					Trending up by 5.2% this month <TrendingUpIcon class="size-4" />
				</div>
				<div class="text-muted-foreground flex items-center gap-2 leading-none">
					January - June 2024
				</div>
			</div>
		</div>
	</Card.Footer>
</Card.Root>
