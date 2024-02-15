<template>
	<div class="flex flex-col">
		<Title title="Products" />
		<div class="flex flex-col gap-4 mb-4">
			<div class="flex flex-row w-full gap-2">
				<input
					class="grow py-2 px-4"
					type="text"
					v-model="search"
					placeholder="Find a Product"
				/>
				<button
					class="text-white py-2 px-4 bg-[#375673] rounded-lg"
				>
					Search
				</button>
			</div>
			<div class="flex flex-row w-full gap-2">
				<input
					class="grow py-2 px-4"
					type="text"
					value=""
					placeholder="Product Name"
				/>
				<button class="text-white py-2 px-4 rounded-lg bg-green-800">
					Add Product
				</button>
			</div>
		</div>

		<div class="">
			<Accordion
				class="my-4"
				v-for="(item, index) in paginatedData()"
				:key="index"
			>
				<template v-slot:title>
					<img :src="item.image" class="h-7 w-7 rounded-lg" />
					<span
						class="my-auto mr-2 h-2 w-2 rounded-2xl"
						:class="
							item.status === 'green'
								? 'bg-green-500'
								: item.status === 'yellow'
								? 'bg-yellow-500'
								: 'bg-red-500'
						"
					></span>
					<span class="grow font-semibold">{{ item.name }}</span>
					<span class="font-semibold" v-if="item.date">
						{{ new Date(item.date * 1000).toLocaleDateString("en-US") }}</span
					>
				</template>
				<template v-slot:body>
					<div class="flex flex-col gap-4">
						<div class="flex flex-row">
							<div class="basis-2/3">
								<img
									src="https://picsum.photos/48"
									class="h-[350px] w-auto rounded-sm"
								/>
							</div>
							<div class="basis-1/3 my-auto text-center">
								<div class="flex flex-col gap-2">
									<div>
										<button
											class="bg-[#375673] rounded-md px-4 py-1.5 text-white"
										>
											Replace
										</button>
									</div>
									<div>
										<button class="bg-white px-4 py-1.5">Clear</button>
									</div>
								</div>
							</div>
						</div>
						<div class="flex flex-col gap-4 mb-7">
							<!-- Title -->
							<div class="flex flex-col gap-3">
								<h1 class="font-bold text-gray-500">Image Title</h1>
								<input
									type="text"
									class="grow ml-3"
									value=""
									placeholder="Image Title"
								/>
							</div>
							<!-- Alt Text -->
							<div class="flex flex-col gap-3">
								<h1 class="font-bold text-gray-500">Alt Text</h1>
								<input
									type="text"
									class="grow ml-3"
									value=""
									placeholder="Alt Text"
								/>
							</div>
							<!-- Link -->
							<div class="flex flex-col gap-3">
								<h1 class="font-bold text-gray-500">Link</h1>
								<input
									type="text"
									class="grow ml-3"
									value=""
									placeholder="Link"
								/>
							</div>
						</div>
					</div>
				</template>
			</Accordion>
			<div class="flex flex-row p-1 gap-2">
				<div>
					<h1 class="">
						Page <b>1</b> of <b>{{ pageCount }}</b>
					</h1>
				</div>
				<div class="grow text-center">
					<h1>
						Sory By
						<button class="font-bold" @click="sortItem(sortBy)">
							Sort Order
						</button>
					</h1>
				</div>
				<div>
					<select v-model="itemsPerPage" class="font-bold">
						<option value="10">10</option>
						<option value="15">15</option>
						<option value="20">20</option>
					</select>
				</div>
			</div>
		</div>
	</div>
</template>
<script setup>
import Accordion from "@/components/Accordion.vue";
import Title from "@/components/Title.vue";
import { computed, ref } from "vue";

const pageNumber = ref(0);
const itemsPerPage = ref(10);
const sortBy = ref(0);
const search = ref("");
const data = ref([
	{
		name: "Americano",
		image: "https://picsum.photos/48",
		status: "green",
		date: null,
	},
	{
		name: "Espresso",
		image: "https://picsum.photos/49",
		status: "yellow",
		date: null,
	},
	{
		name: "Mocha",
		image: "",
		status: "green",
		date: null,
	},
	{
		name: "White Mocha",
		image: "",
		status: "yellow",
		date: 704305433,
	},
	{
		name: "Latte",
		image: "https://picsum.photos/12",
		status: "green",
		date: 1666715033,
	},
	{
		name: "Cappucino",
		image: "https://picsum.photos/33",
		status: "green",
		date: null,
	},
	{
		name: "Walking with Giants",
		image: "https://picsum.photos/64",
		status: "green",
		date: null,
	},
	{
		name: "Turbinator 2",
		image: "https://picsum.photos/87",
		status: "red",
		date: null,
	},
	{
		name: "Super Cali Fragilistic Expialidocious",
		image: "",
		status: "red",
		date: 704305433,
	},
	{
		name: "Baseball Hat",
		image: "https://picsum.photos/78",
		status: "green",
		date: null,
	},
	{
		name: "Americano",
		image: "https://picsum.photos/46",
		status: "green",
		date: null,
	},
	{
		name: "Espresso",
		image: "https://picsum.photos/99",
		status: "yellow",
		date: null,
	},
	{
		name: "Mocha",
		image: "",
		status: "green",
		date: null,
	},
	{
		name: "White Mocha",
		image: "",
		status: "yellow",
		date: 704305433,
	},
	{
		name: "Latte",
		image: "https://picsum.photos/43",
		status: "green",
		date: 1666715033,
	},
	{
		name: "Cappucino",
		image: "https://picsum.photos/75",
		status: "green",
		date: null,
	},
	{
		name: "Walking with Giants",
		image: "https://picsum.photos/97",
		status: "green",
		date: null,
	},
	{
		name: "Turbinator 2",
		image: "https://picsum.photos/30",
		status: "red",
		date: null,
	},
	{
		name: "Super Cali Fragilistic Expialidocious",
		image: "",
		status: "red",
		date: 704305433,
	},
	{
		name: "Baseball Hat",
		image: "https://picsum.photos/74",
		status: "green",
		date: null,
	},
]);

const paginatedData = () => {
	if (search !== "") {
		const wrapper = () => {
			return data.value.filter((p) => {
				return p.name.toLowerCase().indexOf(search.value.toLowerCase()) != -1;
			})
		}
		return wrapper().slice(pageNumber.value, itemsPerPage.value);
	} else {
		return data.value.slice(pageNumber.value, itemsPerPage.value);
	}
};


const sortItem = (order) => {
	switch (order) {
		case 0:
			data.value.sort((a, b) =>
				a.name.toLowerCase() > b.name.toLowerCase() ? 1 : -1
			);
			sortBy.value++;
			break;
		case 1:
			data.value.sort((a, b) =>
				a.name.toLowerCase() < b.name.toLowerCase() ? 1 : -1
			);
			sortBy.value = 0;
		default:
			break;
	}
};

const pageCount = computed(() => {
	return Math.ceil(data.value.length / itemsPerPage.value);
});
</script>
