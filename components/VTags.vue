<template>
	<v-combobox
		v-model="select"
		:label="label"
		multiple
		append-icon
		chips
		deletable-chips
		class="tag-input"
		:search-input.sync="value"
	/>
</template>

<script>
import base from '@/library_vue/components/base';

export default {
	name: 'VtTags',
	extends: base,
	props: {
		label: {
			type: [Object, String],
			default: ''
		},
		// must be included in props
		value: {
			type: null,
			default: null
		}
	}
};
</script>

<style scoped>
.tag-input span.chip {
	background-color: #1976d2;
	color: #fff;
	font-size: 1em;
}

.tag-input span.v-chip {
	background-color: #1976d2;
	color: #fff;
	font-size:1em;
	padding-left:7px;
}

.tag-input span.v-chip::before {
		content: "label";
		font-family: 'Material Icons';
		font-weight: normal;
		font-style: normal;
		font-size: 20px;
		line-height: 1;
		letter-spacing: normal;
		text-transform: none;
		display: inline-block;
		white-space: nowrap;
		word-wrap: normal;
		direction: ltr;
		-webkit-font-feature-settings: 'liga';
		-webkit-font-smoothing: antialiased;
}
</style>
