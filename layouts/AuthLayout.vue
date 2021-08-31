<template>
	<v-app id="root">
		<v-app-bar
			app
			dark
		>
			<v-toolbar-title class="headline text-uppercase">
				<span>{{ $t('titles.application') }}</span>
			</v-toolbar-title>
			<v-spacer />
		</v-app-bar>
		<v-main>
			<span class="bg" />
			<v-container
				fluid
				pt-2
			>
				<router-view />
			</v-container>
		</v-main>
		<v-footer app>
			<VVersion :version="version" />
		</v-footer>
	</v-app>
</template>

<script>
import GlobalUtility from '@thzero/library_client/utility/global';

import baseLayout from '@/library_vue_components/layouts/baseLayout';

import VVersion from '@/library_vue_vuetify/components/VVersion';

export default {
	name: 'AuthLayout',
	components: {
		VVersion
	},
	extends: baseLayout,
	computed: {
		version() {
			return GlobalUtility.$store.state.version;
		}
	}
};
</script>

<style scoped>
</style>
