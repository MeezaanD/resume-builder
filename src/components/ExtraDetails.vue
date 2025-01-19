<template>
	<div>
		<h3>
			<el-icon>
				<Document />
			</el-icon> Extra Details
		</h3>
		<ul>
			<li v-for="(detail, index) in extraDetails" :key="index">{{ detail }}</li>
		</ul>
	</div>
</template>

<script lang="ts">
import { defineComponent, PropType } from 'vue';
import { Document } from '@element-plus/icons-vue';

export default defineComponent({
	name: 'ExtraDetails',
	components: {
		Document,
	},
	props: {
		extraDetails: {
			type: Array as PropType<string[]>,
			required: true,
		},
	},
});
</script>

<style scoped>
h3 {
	border-bottom: 1px solid #ebeef5;
	padding-bottom: 5px;
}

ul {
	margin-top: 10px;
	list-style-type: disc;
	padding-left: 20px;
}
</style>
