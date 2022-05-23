<template>
	<div class="container" ref="container" :style="{
		'display': isOpen ? 'block' : 'none',
		'left': `${position.x}px`,
		'top': `${position.y}px`,
	}">
		<slot />
	</div>
</template>

<script setup>
import { ref, onMounted } from 'vue'

let container = ref()
let isOpen = ref(false)
let position = ref({
	x: 0,
	y: 0
})

function open(event) {
	if (event) {
		position.value.x = event.pageX;
		position.value.y = event.pageY;
	}

	// ctx.value = ctx;
	isOpen.value = true;
}

function close() {
	isOpen.value = false;
}

defineExpose({
	open,
	close
})

onMounted(() => {
	document.addEventListener('click', e => {
		if (!container.value || !isOpen.value)
			return;

		const insideMenu = container.value.contains(e.target);

		if (!insideMenu)
			isOpen.value = false;
	});
})
</script>

<style scoped>
.container {
	position: absolute;
}
</style>
