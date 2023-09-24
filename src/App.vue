<script setup lang="ts">
import { onMounted, onUnmounted, ref, computed } from "vue";

const x = ref<number | null>(null);
const y = ref<number | null>(null);
const top = ref<number | null>(null);
const left = ref<number | null>(null);
const isCoordsSet = ref(false);
const width = ref("100px");
const height = ref("100px");

console.log(parseInt(height.value) / 2);
function setMousePostion(event: MouseEvent) {
	isCoordsSet.value = true;
	const currentTargetRect = (
		event.currentTarget as HTMLElement
	).getBoundingClientRect();
	top.value = event.pageX - currentTargetRect.left;
	left.value = event.pageY - currentTargetRect.top;
}
function updateCoords(event: MouseEvent) {
	if (isCoordsSet.value) {
		x.value = event.pageX - top.value!;
		y.value = event.pageY - left.value!;
	}
}
function onMouseRelease(event: MouseEvent) {
	isCoordsSet.value = false;
}

const values = computed(() => {
	return {
		top: `${y.value}px`,
		bottom: `${y.value}px`,
		left: `${x.value}px`,
		right: `${x.value}px`,
	};
});
onMounted(() => {
	document.addEventListener("mousemove", updateCoords);
	document.addEventListener("mouseup", onMouseRelease);
});

onUnmounted(() => {
	document.removeEventListener("mousemove", updateCoords);
	document.removeEventListener("mouseup", onMouseRelease);
});
</script>

<template>
	<div
		:style="values"
		@mousedown="setMousePostion"
		@mousemove="updateCoords"
		class="box"
	></div>
</template>

<style>
body {
	margin: 0;
	padding: 0;
	box-sizing: border-box;
	overflow: hidden;
}
#app {
	min-height: 100vh;
}
[draggable] {
	appearance: none;
}
.box {
	width: v-bind(width);
	height: v-bind(height);
	background-color: rgb(96, 160, 255);
	position: relative;
	cursor: move;
	border-radius: 50%;
}
</style>
