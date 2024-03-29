<template>
	<v-app id="root">
		<v-app-bar
			app
			dense
			dark
		>
			<v-btn
				text
				class="hidden-md-and-up"
				@click.stop="toggleDrawer"
			>
				<v-icon>menu</v-icon>
			</v-btn>
			<v-toolbar-title class="headline text-uppercase">
				<router-link
					to="/"
					class="toolbar-title"
				>
					{{ $t('titles.application') }}
				</router-link>
			</v-toolbar-title>
			<v-spacer />
			<v-menu
				bottom
				left
				:close-on-content-click="closeOnContentClick"
			>
				<template #activator="{ on }">
					<v-btn
						icon
						v-on="on"
					>
						<v-icon>more_vert</v-icon>
					</v-btn>
				</template>
				<v-list>
					<!-- <v-list-item
						v-if="isLoggedIn"
						to="/profile"
					>
						<v-list-item-action>
							<v-icon>account_circle</v-icon>
						</v-list-item-action>
						<v-list-item-content>
							<v-list-item-title>{{ $t('titles.profile') }}</v-list-item-title>
						</v-list-item-content>
					</v-list-item> -->
					<v-list-item
						v-if="isLoggedIn"
						to="/settings"
					>
						<v-list-item-action>
							<v-icon>settings</v-icon>
						</v-list-item-action>
						<v-list-item-content>
							<v-list-item-title>{{ $t('titles.settings') }}</v-list-item-title>
						</v-list-item-content>
					</v-list-item>
					<!-- <v-list-item @click="clickAbout">
						<v-list-item-action>
							<v-icon>info</v-icon>
						</v-list-item-action>
						<v-list-item-content>
							<v-list-item-title>About</v-list-item-title>
						</v-list-item-content>
					</v-list-item> -->
					<v-list-item
						v-if="!isLoggedIn"
						@click="clickSignIn"
					>
						<v-list-item-action>
							<v-icon color="green darken-2">
								power_settings_new
							</v-icon>
						</v-list-item-action>
						<v-list-item-content>
							<v-list-item-title>{{ $t('titles.signIn') }}</v-list-item-title>
						</v-list-item-content>
					</v-list-item>
					<v-list-item
						v-if="isLoggedIn"
						@click="dialogSignOut.open()"
					>
						<v-list-item-action>
							<v-icon color="red darken-2">
								power_settings_new
							</v-icon>
						</v-list-item-action>
						<v-list-item-content>
							<v-list-item-title>{{ $t('titles.signOut') }}</v-list-item-title>
						</v-list-item-content>
					</v-list-item>
				</v-list>
			</v-menu>
		</v-app-bar>

		<v-main>
			<v-container
				fluid
				pt-2
			>
				<router-view />
			</v-container>
		</v-main>

		<VConfirmationDialog
			:non-recoverable="true"
			:signal="dialogSignOut.signal"
			@cancel="dialogSignOut.cancel()"
			@ok="dialogSignOutOk"
		/>

		<VLayoutFooter />

		<VLoadingOverlay
			:signal="isAuthCompleted"
		/>
	</v-app>
</template>

<script>
import LibraryConstants from '@thzero/library_client/constants';

import GlobalUtility from '@thzero/library_client/utility/global';

import baseLayout from '@/library_vue/layouts/baseLayout';

import VConfirmationDialog from '@/library_vue_vuetify/components/VConfirmationDialog';
import VLayoutFooter from '@/library_vue_vuetify/components/VLayoutFooter';
import VLoadingOverlay from '@/library_vue_vuetify/components/VLoadingOverlay';

import DialogSupport from '@/library_vue/components/support/dialog';

const auth = GlobalUtility.$injector.getService(LibraryConstants.InjectorKeys.SERVICE_AUTH);

export default {
	name: 'AdminLayout',
	components: {
		VConfirmationDialog,
		VLayoutFooter,
		VLoadingOverlay
	},
	extends: baseLayout,
	data: () => ({
		closeOnContentClick: true,
		dialogSignOut: new DialogSupport()
	}),
	computed: {
		isAuthCompleted() {
			return GlobalUtility.$store.state.user && GlobalUtility.$store.state.user.authCompleted;
		},
		isLoggedIn() {
			return GlobalUtility.$store.state.user && GlobalUtility.$store.state.user.isLoggedIn;
		}
	},
	methods: {
		clickAbout() {
			GlobalUtility.$navRouter.push('/about');
		},
		async clickSignIn() {
			GlobalUtility.$navRouter.push('/auth');
		},
		async dialogSignOutOk() {
			this.dialogSignOut.ok();
			await auth.signOut(this.correlationId());
		}
	}
};
</script>

<style scoped>
.toolbar-title {
	color: white;
	text-decoration: none;
}
</style>
