<template>
	<v-dialog
		v-model="dialogSignal"
		persistent
		max-width="300px"
		@keydown.esc="dialogCancel()"
	>
		<ValidationObserver
			ref="obs"
			v-slot="{ invalid }"
		>
			<v-card>
				<v-card-title class="headline">
					{{ $attrs.label }}
				</v-card-title>
				<v-card-text>
					{{ message ? message : nonRecoverable ? $t('questions.areYouSureNonRecoverable') : $t('questions.areYouSure') }}
					<div
						v-for="(item, index) in serverErrors"
						:key="index"
						class="red--text text--lighten-1 v-messages"
					>
						{{ item }}
					</div>
				</v-card-text>
				<v-card-actions>
					<v-btn
						color="primary"
						text
						@click.stop="dialogCancel()"
					>
						{{ $t('buttons.cancel') }}
					</v-btn>
					<v-btn
						color="primary"
						text
						:disabled="invalid"
						@click.stop="dialogOk()"
					>
						{{ $t('buttons.ok') }}
					</v-btn>
				</v-card-actions>
			</v-card>
		</ValidationObserver>
	</v-dialog>
</template>

<script>
import VueUtility from '@thzero/library_client_vue/utility/index';

import baseConfirmationDialog from '@/library_vue/components/baseConfirmationDialog';

export default {
	name: 'VtConfirmationDialog',
	extends: baseConfirmationDialog,
	methods: {
		handleError(response, correlationId) {
			VueUtility.handleError(this.$refs.obs, this.serverErrors, response, correlationId);
		}
	}
};
</script>
