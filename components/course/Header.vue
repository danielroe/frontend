<template>
	<header
		class="grid gap-5 md:gap-x-10 grid-cols-1 md:grid-cols-[200px_1fr] lg:grid-cols-[300px_1fr_auto] h-fit"
	>
		<img
			:src="image"
			alt=""
			class="object-cover h-[200px] sm:h-[250px] w-full rounded-md shadow-xl md:row-span-2 lg:row-span-1"
		/>

		<div>
			<SectionTitle full size="xl" :subheading="category" :heading="title" />
			<p>
				{{ t('Body.CreatedBy') }}
				<span class="text-accent">{{ author }}</span>
			</p>
		</div>

		<p class="text-sm text-subheading -mt-2 md:mt-2">
			{{ t('Body.LastUpdated') }}
			<span class="text-body">{{ lastUpdated }}</span>
		</p>
	</header>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import type { PropType } from 'vue';
import { useI18n } from 'vue-i18n';

export default defineComponent({
	props: {
		data: { type: Object as PropType<any>, default: null },
	},
	setup(props) {
		const { t } = useI18n();

		const image = computed(() => {
			return props.data?.image ?? `/images/about-${getRandomNumber(1, 5)}.webp`;
		});

		const category = computed(() => {
			return (props.data?.category ?? props.data?.id ?? '').replace(/_/g, ' ');
		});

		const title = computed(() => {
			return props.data?.title ?? '';
		});

		const author = computed(() => {
			return props.data?.author ?? '';
		});

		const lastUpdated = computed(() => {
			let timestamp = props.data?.last_update ?? '';
			if (!!!timestamp) return ``;

			let { month, year } = convertTimestampToDate(timestamp);
			return `${t(month.string).substring(0, 3)}, ${year}`;
		});

		return { image, title, category, author, lastUpdated, t };
	},
});
</script>

<style scoped></style>
