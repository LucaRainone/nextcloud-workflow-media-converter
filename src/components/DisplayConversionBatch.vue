<template>
	<div class="wmc-display-conversion-batch">
		<div class="wmc-display-conversion-batch__status-container">
			<div class="wmc-display-conversion-batch__status">
				<StatusBadge :status="conversionBatch.status" />
			</div>
			<div class="wmc-display-conversion-batch__progress">
				<ProgressBar size="medium" :value="((conversionBatch.converted / conversionBatch.unconverted) || 0) * 100" :error="['has-errors', 'failed'].includes(conversionBatch.status)" />
				<div v-if="conversionBatch.status !== 'queued'" class="wmc-display-conversion-batch__progress-labels">
					<small class="wmc-display-conversion-batch__progress-labels-converted">
						{{ t('workflow_media_converter', 'Converted') }}: {{ conversionBatch.converted }}
					</small>
					<small class="wmc-display-conversion-batch__progress-labels-total">
						{{ t('workflow_media_converter', 'Total') }}: {{ conversionBatch.unconverted }}
					</small>
				</div>
			</div>
			<div class="wmc-display-conversion-batch__delete-button">
				<Actions>
					<ActionButton icon="icon-delete" @click="$emit('remove')" />
				</Actions>
			</div>
		</div>
		<div v-if="conversionBatch.error" class="wmc-display-conversion-batch__error-message">
			<p>
				{{ t('workflow_media_converter', '') }} ({{ t('workflow_media_converter', 'Error code') }} {{ conversionBatch.error.code }}) {{ conversionBatch.error.message }}
			</p>
		</div>
	</div>
</template>

<script>
import Actions from '@nextcloud/vue/dist/Components/Actions'
import ActionButton from '@nextcloud/vue/dist/Components/ActionButton'
import StatusBadge from './StatusBadge.vue'
import ProgressBar from '@nextcloud/vue/dist/Components/ProgressBar'

export default {
	components: { Actions, ActionButton, StatusBadge, ProgressBar },

	props: {
		conversionBatch: {
			required: true,
			type: Object,
		},
	},
}
</script>

<style lang="scss">
.wmc-display-conversion-batch {
	position: relative;
	border-radius: .5em;

	&__error-message {
		p {
			color: #ff4402;
		}
	}

	&__status {
		display: flex;
		align-items: center;
		justify-content: center;

		&-container {
			display: grid;
			grid-template-columns: 10em 1fr 4em;
			grid-template-rows: 1fr;
			align-items: center;
		}
	}

	&__progress {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;

		&-labels {
			display: flex;
			flex-direction: row;
			width: 100%;

			&-converted {
				display: block;
			}

			&-total {
				display: block;
				margin-left: auto;
			}
		}
	}

	&__delete-button {
		margin-left: auto;
	}
}
</style>
