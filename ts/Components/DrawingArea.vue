<template>
	<div>
		<canvas ref="canvas"
			style="touch-action:none"
			width="800"
			height="600"
			@pointerdown.prevent="onMousedown"
			@pointermove.prevent="onMousemove"
		></canvas>
	</div>
</template>

<script lang="ts">
import Vue from 'vue'
import Component from 'vue-class-component'
import { Prop } from 'vue-property-decorator'

@Component
export default class ComponentsDrawingArea extends Vue {
	protected canvas: any

	protected ctx: any

	protected drawing: boolean = false

	protected mounted() : void {
		this.canvas = this.$refs.canvas
		this.ctx = this.canvas.getContext('2d')

		this.canvas.width = 800
		this.canvas.height = 600

		document.addEventListener('pointerup', this.onMouseup)
	}

	protected destroyed() : void {
		document.removeEventListener('pointerup', this.onMouseup)
	}

	protected onMousedown(e: any) : void {
		this.drawing = true

		const rect = this.canvas.getBoundingClientRect()
		const x = e.pageX - rect.left
		const y = e.pageY - rect.top - window.scrollY

		this.ctx.beginPath()
		this.ctx.moveTo(x, y)

		// draw a simple dot (useful when only clicking/tapping instead of dragging)
		this.ctx.lineTo(x + 2, y + 2)
		this.ctx.stroke()
	}

	protected onMousemove(e: any) {
		if (! this.drawing) return

		const rect = this.canvas.getBoundingClientRect()

		const x = e.pageX - rect.left
		const y = e.pageY - rect.top - window.scrollY

		this.ctx.lineTo(x, y)
		this.ctx.stroke()
	}

	protected onMouseup() : void {
		this.drawing = false

		console.log(this.canvas.toDataURL())
	}
}
</script>
