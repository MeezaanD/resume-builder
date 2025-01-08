<template>
	<div>
		<h3>Experience</h3>
		<div v-for="(experience, index) in data" :key="index">
			<el-form-item label="Job Title">
				<el-input v-model="experience.jobTitle" placeholder="Enter job title"></el-input>
			</el-form-item>
			<el-form-item label="Company Name">
				<el-input v-model="experience.companyName" placeholder="Enter company name"></el-input>
			</el-form-item>
			<el-form-item label="Start Date">
				<el-date-picker v-model="experience.startDate" type="date"
					placeholder="Select start date"></el-date-picker>
			</el-form-item>
			<el-form-item label="End Date">
				<el-date-picker v-model="experience.endDate" type="date" placeholder="Select end date"
					:disabled="experience.currentJob"></el-date-picker>
				<el-checkbox v-model="experience.currentJob">I currently work here</el-checkbox>
			</el-form-item>
			<el-form-item label="Location">
				<el-input v-model="experience.location" placeholder="Enter location"></el-input>
			</el-form-item>
			<el-form-item label="Job Description">
				<el-input type="textarea" v-model="experience.jobDescription"
					placeholder="Enter job description"></el-input>
			</el-form-item>
		</div>
		<el-button @click="addExperience">Add Experience</el-button>
	</div>
</template>

<script lang="ts">
import { defineComponent, type PropType } from 'vue';

export default defineComponent({
	name: 'Experience',
	props: {
		data: {
			type: Array as PropType<
				Array<{
					jobTitle: string;
					companyName: string;
					startDate: string;
					endDate: string;
					location: string;
					jobDescription: string;
					currentJob: boolean;
				}>
			>,
			required: true
		}
	},
	methods: {
		addExperience() {
			this.data.push({
				jobTitle: '',
				companyName: '',
				startDate: '',
				endDate: '',
				location: '',
				jobDescription: '',
				currentJob: false
			});
		}
	}
});
</script>
