<template>
	<div>
		<h3>Education</h3>
		<div v-for="(education, index) in data" :key="index">
			<el-form-item label="Course Title">
				<el-input v-model="education.courseTitle" placeholder="Enter course title"></el-input>
			</el-form-item>
			<el-form-item label="Institution">
				<el-input v-model="education.institution" placeholder="Enter institution name"></el-input>
			</el-form-item>
			<el-form-item label="Qualification Type">
				<el-input v-model="education.qualificationType" placeholder="Enter qualification type"></el-input>
			</el-form-item>
			<el-form-item label="Start Date">
				<el-date-picker v-model="education.startDate" type="date"
					placeholder="Select start date"></el-date-picker>
			</el-form-item>
			<el-form-item label="End Date">
				<el-date-picker v-model="education.endDate" type="date" placeholder="Select end date"
					:disabled="education.currentStudy"></el-date-picker>
				<el-checkbox v-model="education.currentStudy">I am currently studying here</el-checkbox>
			</el-form-item>
			<el-form-item label="Location">
				<el-input v-model="education.location" placeholder="Enter location"></el-input>
			</el-form-item>
		</div>
		<el-button @click="addEducation">Add Education</el-button>
	</div>
</template>

<script lang="ts">
import { defineComponent, type PropType } from 'vue';

export default defineComponent({
	name: 'Education',
	props: {
		data: {
			type: Array as PropType<
				Array<{
					courseTitle: string;
					institution: string;
					qualificationType: string;
					startDate: string;
					endDate: string;
					location: string;
					currentStudy: boolean;
				}>
			>,
			required: true
		}
	},
	methods: {
		addEducation() {
			this.data.push({
				courseTitle: '',
				institution: '',
				qualificationType: '',
				startDate: '',
				endDate: '',
				location: '',
				currentStudy: false
			});
		}
	}
});
</script>
