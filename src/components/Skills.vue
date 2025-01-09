<template>
	<div>
		<h3>Skills</h3>
		<el-form-item label="Skill">
			<el-input v-model="newSkill" placeholder="Enter a skill"></el-input>
			<el-button @click="addSkill">Add Skill</el-button>
		</el-form-item>
		<ul class="skills-list-form">
			<li v-for="(skill, index) in data" :key="index">
				<p>{{ skill }}</p>
				<el-popconfirm
					v-if="data.length > 0"
					title="Are you sure to delete this?"
					@confirm="removeSkill(index)"
					placement="top"
				>
					<template #reference>
						<el-button class="delete-btn" type="danger" circle>
							<el-icon><Delete /></el-icon>
						</el-button>
					</template>
				</el-popconfirm>
			</li>
		</ul>
	</div>
</template>

<script lang="ts">
import { defineComponent, ref, type PropType } from 'vue';
import { Delete } from '@element-plus/icons-vue';

export default defineComponent({
	name: 'Skills',
	components: {
		Delete
	},
	props: {
		data: {
			type: Array as PropType<string[]>,
			required: true
		}
	},
	setup(props) {
		const newSkill = ref<string>('');

		const addSkill = () => {
			if (newSkill.value) {
				props.data.push(newSkill.value);
				newSkill.value = '';
			}
		};

		const removeSkill = (index: number) => {
			if (index > -1 && index < props.data.length) {
				props.data.splice(index, 1);
			}
		};

		return {
			newSkill,
			addSkill,
			removeSkill
		};
	}
});
</script>