<template>
	<div>
		<span v-if="innerVersion !== null">{{ $t('version.label') }} {{ $t('version.majorMinorDate', innerVersion.client) }}&nbsp;-&nbsp;{{ $t('version.majorMinorDate', innerVersion.server) }}</span>
	</div>
</template>

<script>
import base from '@/library_vue/components/base';

export default {
	name: 'VtVersion',
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
		innerVersion() {
			return this.value;
		}
	}
};
</script>

<style scoped>
</style>
