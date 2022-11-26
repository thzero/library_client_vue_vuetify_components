<template>
	<v-footer
		app
		absolute
	>
		<VVersion v-model="version" />
		<v-spacer />
		<VCopyright v-model="version" />
		<span
			v-if="isDev"
			style="padding-left: 4px;"
		>
			{{ breakpointName }}
		</span>
	</v-footer>
</template>

<script>
import GlobalUtility from '@thzero/library_client/utility/global';
import LibraryUtility from '@thzero/library_common/utility';

import base from '@/library_vue/components/base';
import VCopyright from './VCopyright';
import VVersion from './VVersion';

export default {
	name: 'VtLayoutFooter',
	components: {
		VCopyright,
		VVersion
	},
	extends: base,
	data: () => ({
		version: {}
	}),
	computed: {
		breakpointName() {
			return LibraryUtility.isDev ? this.$vuetify.breakpoint.name : '';
		},
		isDev() {
			return LibraryUtility.isDev;
		}
	},
	async created() {
		await GlobalUtility.$store.dispatcher.root.getVersion(this.correlationId());
		this.version = GlobalUtility.$store.state.version;
	}
};
</script>

<style scoped>
</style>
