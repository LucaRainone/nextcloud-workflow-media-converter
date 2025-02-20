<template>
	<div>
		<div class="wmc-conversion-batch__delete-button">
			<Actions>
				<ActionButton icon="icon-delete" @click="$emit('remove')" />
			</Actions>
		</div>
		<div class="grid">
			<div class="column">
				<div class="wmc-conversion-batch__source-directory">
					<span>{{ t('workflow_media_converter', `Convert all files in this folder`) }}</span>
					<div>
						<button @click="openFilePicker('sourceFolder')">
							{{ t('workflow_media_converter', 'Choose Folder') }}
						</button>
						<span>{{ sourceFolder }}</span>
					</div>
				</div>
				<CheckboxRadioSwitch :checked.sync="convertMediaInSubFolders">
					{{ t('workflow_media_converter', 'Convert media in sub-folders') }}
				</CheckboxRadioSwitch>
				<div class="wmc-conversion-batch__from-format">
					<label>{{ t('workflow_media_converter', 'Find source files with this extension/format') }}</label>
					<select v-model="sourceExtension" class="wmc-conversion-batch__from-format-picker">
						<option value="" />
						<option
							v-for="(format, index) in formats"
							:key="`format-from-${index}`"
							:value="format.extension">
							<span>.{{ format.extension }} ({{ format.label }})</span>
						</option>
					</select>
				</div>
				<div class="wmc-conversion-batch__to-format">
					<label>{{ t('workflow_media_converter', 'Store converted output in this extension/format') }}</label>
					<select v-model="outputExtension" class="wmc-conversion-batch__to-format-picker">
						<option value="" />
						<option
							v-for="(format, index) in formats"
							:key="`format-to-${index}`"
							:value="format.extension">
							<span>.{{ format.extension }} ({{ format.label }})</span>
						</option>
					</select>
				</div>
			</div>
			<div class="column">
				<PostConversionRules v-model="postConversionRules" />
			</div>
		</div>
		<div class="wmc-conversion-batch__actions">
			<button v-if="!conversionBatch.id" class="save" @click="$emit('save')">
				{{ t('workflow_media_converter', 'Save') }}
			</button>
		</div>
	</div>
</template>

<script>
import Actions from '@nextcloud/vue/dist/Components/Actions'
import ActionButton from '@nextcloud/vue/dist/Components/ActionButton'
import CheckboxRadioSwitch from '@nextcloud/vue/dist/Components/CheckboxRadioSwitch'
import formats from '../constants/formats'
import filepicker from '../mixins/filepicker'
import PostConversionRules from './PostConversionRules.vue'

export default {
	components: { Actions, ActionButton, CheckboxRadioSwitch, PostConversionRules },

	mixins: [filepicker],

	props: {
		conversionBatch: {
			required: true,
			type: Object,
		},
	},

	data: () => ({
		formats,
	}),

	computed: {
		sourceFolder: {
			get() {
				return this.conversionBatch.sourceFolder
			},
			set(sourceFolder) {
				this.commit({ sourceFolder })
			},
		},
		convertMediaInSubFolders: {
			get() {
				return this.conversionBatch.convertMediaInSubFolders
			},
			set(convertMediaInSubFolders) {
				this.commit({ convertMediaInSubFolders })
			},
		},
		sourceExtension: {
			get() {
				return this.conversionBatch.sourceExtension
			},
			set(sourceExtension) {
				this.commit({ sourceExtension })
			},
		},
		outputExtension: {
			get() {
				return this.conversionBatch.outputExtension
			},
			set(outputExtension) {
				this.commit({ outputExtension })
			},
		},
		postConversionRules: {
			get() {
				return this.conversionBatch
			},
			set(change) {
				this.commit(change)
			},
		},
	},

	methods: {
		commit(mutation) {
			if (this.conversionBatch.id) {
				return
			}

			this.$emit('change', { ...this.conversionBatch, ...mutation })
		},
	},
}
</script>

<style lang="scss">
.wmc-conversion-batch {
	.grid {
		display: grid;
		grid-template-columns: 1fr 1fr;
		grid-gap: 4em;
	}

	&__from-format {
		margin-top: 2em;
	}
}
</style>
