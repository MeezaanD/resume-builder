<template>
	<div>
		<h3>Custom Sections</h3>
		<el-button @click="addCustomSection" style="margin-bottom: 20px;">Add Section</el-button>
		<ul class="custom-sections-list-form">
			<li v-for="(section, index) in data" :key="index">
				<el-form-item label="Section Name">
					<el-input v-model="section.name" placeholder="Enter section name"></el-input>
				</el-form-item>
				<el-form-item label="Section Value">
					<el-input v-model="section.value" placeholder="Enter section value"></el-input>
				</el-form-item>
				<el-popconfirm v-if="data.length > 0" title="Are you sure to delete this?"
					@confirm="removeCustomSection(index)" placement="top">
					<template #reference>
						<el-button class="delete-btn" type="danger" circle>
							<el-icon>
								<Delete />
							</el-icon>
						</el-button>
					</template>
				</el-popconfirm>
			</li>
		</ul>
	</div>
</template>

<script lang="ts">
import { defineComponent, type PropType } from 'vue';
import { Delete } from '@element-plus/icons-vue';

export default defineComponent({
	name: 'CustomSections',
	components: { Delete },
	props: {
		data: {
			type: Array as PropType<Array<{ name: string; value: string }>>,
			required: true,
		},
	},
	setup(props) {
		const addCustomSection = () => {
			props.data.push({ name: '', value: '' });
		};

		const removeCustomSection = (index: number) => {
			if (index > -1 && index < props.data.length) {
				props.data.splice(index, 1);
			}
		};

		return {
			addCustomSection,
			removeCustomSection,
		};
	},
});
</script>

<style scoped>
h3 {
	border-bottom: 1px solid #ebeef5;
	padding-bottom: 5px;
}

.el-button {
	margin-top: 10px;
}

.custom-sections-list-form {
	list-style-type: none;
	padding: 0;
}

.custom-sections-list-form li {
	margin-bottom: 20px;
}
</style>
