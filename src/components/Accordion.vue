<template>
	<div class="cursor-pointer" @click="toggleAccordion()">
		<div
			class="flex items-center space-x-3"
			:aria-expanded="isOpen"
			:aria-controls="`collapse${_uid}`"
			:class="{ 'mb-7': isOpen }"
		>
			<slot name="title" :class="{ 'mb-4': isOpen }" />
			<svg
				class="h-4 w-4 translate transition-all duration-200 text-gray-500"
				:class="{
					'rotate-180': isOpen,
					'rotate-0': !isOpen,
				}"
				fill="none"
				stroke="currentColor"
				xmlns="http://www.w3.org/2000/svg"
				view-box="0 0 16 10"
				aria-hidden="true"
			>
				<path
					d="M15 1.2l-7 7-7-7"
					stroke-width="2"
					stroke-linecap="round"
					stroke-linejoin="round"
				/>
			</svg>
		</div>

		<Transition
			name="accordion"
			@before-enter="beforeEnter"
			@enter="enter"
			@before-leave="beforeLeave"
			@leave="leave"
		>
			<div v-show="isOpen" :id="`collapse${_uid}`" class="accordion-body">
				<slot name="body" />
			</div>
		</Transition>
	</div>
</template>
<script setup>
import { ref } from "vue";

const isOpen = ref(false);

const toggleAccordion = () => {
	isOpen.value = !isOpen.value;
};

const beforeEnter = (el) => {
	el.style.height = 0;
};

const enter = (el) => {
	el.style.height = el.scrollHeight + "px";
};

const beforeLeave = (el) => {
	el.style.height = el.scrollHeight + "px";
};

const leave = (el) => {
	el.style.height = 0;
};
</script>

<style scoped>
.accordion-body {
	transition: 150ms ease-out;
	overflow: hidden;
}
</style>
