<template>
	<ValidationProvider
		ref="prv"
		:vid="vid"
		:name="$attrs.label"
		:rules="rules"
		:bail="rulesBail"
		:immediate="true"
	>
		<v-text-field
			v-model="innerValue"
			slot-scope="{ errors, valid }"
			:error-messages="errors"
			:readonly="readonly"
			:success="valid"
			v-bind="$attrs"
			v-on="$listeners"
  			@blur="blur"
		/>
	</ValidationProvider>
</template>

<script>
import baseControlEdit from '@/library_vue/components/baseControlEdit';

export default {
	name: 'VtTextFieldWithValidation',
	extends: baseControlEdit,
	props: {
		rules: {
			type: [Object, String],
			default: ''
		},
		rulesBail: {
			type: Boolean,
			default: true
		},
		rulesImmediate: {
			type: Boolean,
			default: false
		},
		blur: {
			type: Function,
			default: null
		},
		// must be included in props
		value: {
			type: null,
			default: null
		},
		readonly: {
			type: Boolean,
			default: false
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
		validation() {
			return this.$refs.prv;
		}
	}
};
</script>

<style scoped>
</style>
