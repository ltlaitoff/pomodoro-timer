<script setup lang="ts">
import { LeftPanelStates } from '@pages/home/entities/LeftPanelStates.ts'
import { useUserSettingsStore } from '@shared/store/userSettingsStore.ts'

// TODO: Refactor
const props = defineProps<{
	isOpened: boolean
	mode: LeftPanelStates
	burgerOpened: boolean
}>()

// TODO: Refactor
const emits = defineEmits<{
	(event: 'click', value: LeftPanelStates): void
	(event: 'burger-toggle'): void
}>()

const userSettingsStore = useUserSettingsStore()

const data: {
	title: string
	key: LeftPanelStates
}[] = [
	{
		title: 'Settings',
		key: 'settings'
	},
	{
		title: 'Categories',
		key: 'category'
	},
	{
		title: 'Statistic',
		key: 'statistic'
	}
]
</script>

<template>
	<div class="pt-10 h-full flex flex-col justify-start gap-y-2 max-md:hidden">
		<button
			v-for="item of data"
			:key="item.key"
			class="py-3 px-4 rounded-r-xl shadow clip-your-needful-style"
			:style="{
				color: `var(--color-${userSettingsStore.getSelectedModeColor}-950)`,
				backgroundColor:
					props.isOpened === false
						? `var(--color-${userSettingsStore.getSelectedModeColor}-200)`
						: props.mode === item.key
						? `var(--color-${userSettingsStore.getSelectedModeColor}-300)`
						: `var(--color-${userSettingsStore.getSelectedModeColor}-200)`
			}"
			@click="emits('click', item.key)"
		>
			{{ item.title }}
		</button>
	</div>

	<div class="max-md:flex hidden z-10">
		<button
			:class="[
				`absolute top-5 right-5 w-8 h-8 rounded-lg z-50 flex items-center justify-center`,
				props.burgerOpened ? 'gap-y-[0px]' : 'gap-y-[5px]'
			]"
			:style="{
				backgroundColor: `var(--color-${userSettingsStore.getSelectedModeColor}-300)`
			}"
			@click="emits('burger-toggle')"
		>
			<div class="flex flex-col gap-y-[3px]">
				<div
					class="bg-black w-[15px] h-[2px] rounded-full"
					:class="props.burgerOpened ? ' rotate-45' : 'rotate-0'"
				></div>
				<div
					class="bg-black w-[15px] h-[2px] rounded-full"
					:class="props.burgerOpened ? 'hidden' : ''"
				></div>
				<div
					class="bg-black w-[15px] h-[2px] rounded-full"
					:class="props.burgerOpened ? '-mt-[5px] -rotate-45' : 'rotate-0'"
				></div>
			</div>
		</button>

		<div
			class="absolute min-w-[100vw] w-full h-full bg-white flex items-center justify-center flex-col"
			:class="props.burgerOpened ? 'top-0' : '-top-full'"
		>
			<button
				v-for="item of data"
				:key="item.key"
				class="py-6 px-4 hover:bg-slate-200 rounded-md transition-all duration-150 text-2xl w-full"
				:style="{
					color: `var(--color-${userSettingsStore.getSelectedModeColor}-950)`
				}"
				@click="emits('click', item.key)"
			>
				{{ item.title }}
			</button>
		</div>
	</div>
</template>
