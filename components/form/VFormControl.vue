<template>
	<vue-fragment>
		<ValidationObserver
			ref="obs"
			v-slot="{ invalid }"
		>
			<v-card>
				<v-card-title class="headline">
					{{ label }}
				</v-card-title>
				<v-card-text>
					<v-form>
						<slot />
						<div
							v-for="(item, index) in serverErrors"
							:key="index"
							class="red--text text--lighten-1 v-messages"
						>
							{{ item }}
						</div>
					</v-form>
				</v-card-text>
				<v-card-actions
					v-if="!autoSave"
				>
					<v-spacer />
					<v-btn
						v-if="buttonClear"
						color="primary lighten-1"
						text
						@click="clear"
					>
						{{ $t('buttons.clear') }}
					</v-btn>
					<v-btn
						v-if="buttonCancel"
						color="primary lighten-1"
						text
						@click="cancel"
					>
						{{ $t('buttons.cancel') }}
					</v-btn>
					<v-btn
						v-if="buttonOk"
						:disabled="invalid || disabled"
						color="green darken-1"
						text
						@click="submit"
					>
						{{ $t('buttons.ok') }}
					</v-btn>
				</v-card-actions>
				<v-overlay
					absolute
					:value="overlaySave"
				>
					<v-card
        				color="primary"
						dark
					>
						<v-card-text>
							Saving...
							<v-progress-linear
								indeterminate
								color="white"
								class="mb-0"
							></v-progress-linear>
						</v-card-text>
					</v-card>
				</v-overlay>
			</v-card>
		</ValidationObserver>
	</vue-fragment>
</template>

<script>
import VueUtility from '@thzero/library_client_vue/utility/index';

import baseEdit from '@/library_vue_components/components/baseEdit';

export default {
	name: 'VtFormControl',
	extends: baseEdit,
	props: {
		autoSave: {
			type: Boolean,
			default: false
		},
		buttonCancel: {
			type: Boolean,
			default: true
		},
		buttonClear: {
			type: Boolean,
			default: true
		},
		buttonOk: {
			type: Boolean,
			default: true
		},
		label: {
			type: String,
			default: ''
		},
		preCompleteOk: {
			type: Function,
			default: null
		}
	},
	data: () => ({
        isSaving: false,
		disabled: false
	}),
	computed: {
		overlaySave() {
			return this.isSaving && this.autoSave;
		}
	},
	methods: {
		cancel() {
			const correlationId = this.correlationId();
			this.serverErrors = [];
			this.clear(correlationId);
			this.logger.debug('FormControl', 'cancel', null, null, correlationId);
			this.$emit('cancel');
			this.isSaving = false;
		},
		async clear(correlationId) {
			this.serverErrors = [];
			this.logger.debug('FormControl', 'clear', null, null, correlationId);
			this.$nextTick(async () => {
				await this.$refs.obs.reset(correlationId);
			});
			this.isSaving = false;
		},
		observer() {
			this.$refs.obs;
		},
		reset(correlationId, value) {
			const timer = setInterval(async () => {
				clearInterval(timer);
				const el = document.getElementsByClassName('v-card__text');
				if (el && el.length > 0)
					el[0].scrollTop = 0;
			}, 25);
			this.resetControl(correlationId, value);
			this.isSaving = false;
		},
		// eslint-disable-next-line
		resetControl(correlationId, value) {
		},
		setErrors(errors) {
			this.$refs.obs.setErrors(errors);
			this.isSaving = false;
		},
		async submit() {
			this.isSaving = true;
			this.serverErrors = [];

			const correlationId = this.correlationId();

			const result = await this.$refs.obs.validate(correlationId);
			this.logger.debug('FormControl', 'submit', 'result', result, correlationId);
			if (!result)
				return;

			if (this.preCompleteOk) {
				const response = await this.preCompleteOk(correlationId);
				this.logger.debug('FormControl', 'submit', 'response', response, correlationId);
				if (this.hasFailed(response)) {
					VueUtility.handleError(this.$refs.obs, this.serverErrors, response, correlationId);
					this.isSaving = false;
					return;
				}
			}

			this.logger.debug('FormControl', 'submit', 'ok', null, correlationId);
			this.$emit('ok');
			this.clear(correlationId);
			this.isSaving = false;
		}
	}
};
</script>

<style scoped>
</style>
