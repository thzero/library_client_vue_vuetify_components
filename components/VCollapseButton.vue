<template>
	<v-btn
		depressed
		style="min-width: 0px"
		@click="click(!innerValue)"
	>
		<span
			v-if="$vuetify.breakpoint.smAndUp"
		>
			{{ label }}
		</span>
		<v-icon
			v-if="!innerValue"
		>
			mdi-chevron-up
		</v-icon>
		<v-icon
			v-if="innerValue"
		>
			mdi-chevron-down
		</v-icon>
	</v-btn>
</template>

<script>
import LibraryUtility from '@thzero/library_common/utility';

import baseControlEdit from '@/library_vue/components/baseControlEdit';

export default {
	name: 'VtCollapseButton',
	extends: baseControlEdit,
	props: {
		// must be included in props
		value: {
			type: null,
			default: null
		},
		label: {
			type: String,
			default: null
		}
	},
	watch: {
		// Handles external model changes.
		value(newVal) {
			this.initValue(newVal);
		}
	},
	mounted() {
		this.initValue(this.value);
	},
	methods: {
		click(value) {
			//innerValue = value
			this.update(this, value);
			this.$emit('click');
		},
		update: LibraryUtility.debounce(async function(self, value) {
			self.innerValue = value;
		}, 500)
	}
};
</script>

<style scoped>
</style>
