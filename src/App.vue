<script setup lang="ts">
import * as d3 from 'd3'

const container = shallowRef<HTMLElement | undefined>()

onMounted(() => {
	const data = [
		{
			name: 'Ответы',
			count: 450,
		},
		{
			name: '"Промокод"',
			count: 990,
		},
		{
			name: '"Информация"',
			count: 1980,
		},
		{
			name: '"Не интересно"',
			count: 2970,
		},
		{
			name: 'Не прочитано',
			count: 900,
		},
		{
			name: 'Без ответа/реакции',
			count: 1710,
		},
	]

	const width = 200
	const height = 200
	const r = width / 2

	const colorScale = d3
		.scaleSequential(d3.interpolate('Indigo', 'DeepPink'))
		.domain([10, 100])

	const svg = d3
		.create('svg')
		.attr('width', width)
		.attr('height', height)
		.attr('viewBox', [0, 0, width, height])

	const graph = svg.append('g').attr('transform', `translate(${r} ${r})`)

	const arc = d3
		.arc()
		.innerRadius(r - r / 3)
		.outerRadius(r)
		.cornerRadius(2.5)
		.padAngle(0.025)

	const pie = d3.pie().value((d) => d.count)

	const arcs = graph
		.selectAll('.arc')
		.data(pie(data))
		.enter()
		.append('g')
		.attr('class', 'arc')

	arcs
		.append('path')
		.attr('d', arc)
		.attr('fill', (d) => colorScale(d.value))

	// Append the SVG element.
	container.value?.append(svg.node() as SVGSVGElement)
})
</script>

<template>
	<p>Hello world</p>
	<div ref="container" />
</template>
