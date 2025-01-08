<template>
	<div>
		<h3>Skills</h3>
		<el-form-item label="Skill">
			<el-input v-model="newSkill" placeholder="Enter a skill"></el-input>
			<el-button @click="addSkill">Add Skill</el-button>
		</el-form-item>
		<ul>
			<li v-for="(skill, index) in data" :key="index">
				{{ skill }}
				<el-button @click="removeSkill(index)" type="danger" icon="el-icon-delete"></el-button>
			</li>
		</ul>
	</div>
</template>

<script lang="ts">
import { defineComponent, ref, type PropType } from 'vue';

export default defineComponent({
	name: 'Skills',
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
			props.data.splice(index, 1);
		};

		return {
			newSkill,
			addSkill,
			removeSkill
		};
	}
});
</script>
