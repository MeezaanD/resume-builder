<template>
	<div>
		<h3>Summary</h3>
		<el-form-item label="Summary">
			<el-input type="textarea" v-model="data" placeholder="Enter a brief summary"></el-input>
		</el-form-item>
	</div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
	name: 'Summary',
	props: {
		data: {
			type: String,
			required: true
		}
	}
});
</script>
