<template>
	<span v-if="innerVersion !== null">
		© <span v-if="hasCopyright">{{ copyright }}</span>
		<a
			v-if="hasAuthor"
			class="ml-1"
			target="_blank"
			:href="authorUrl"
		>{{ author }}</a>
	</span>
</template>

<script>
import base from '@/library_vue/components/base';

export default {
	name: 'VtCopyright',
	extends: base,
	props: {
		value: {
			type: Object,
			default: null
		}
	},
	data () {
		return {};
	},
	computed: {
		author() {
			return this.value.client && this.value.client.author ? this.value.client.author : '';
		},
		authorUrl() {
			return this.value.client && this.value.client.author ? this.value.client.author_url : '';
		},
		copyright() {
			return this.value.client && this.value.client.copyright ? this.value.client.copyright : '';
		},
		hasAuthor() {
			return this.value.client && this.value.client.author && this.value.client.author_url;
		},
		hasCopyright() {
			return this.value.client && this.value.client.copyright;
		},
		innerVersion() {
			return this.value;
		}
	}
};
</script>

<style scoped>
</style>
