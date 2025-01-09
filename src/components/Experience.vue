<template>
	<div>
		<h3>Experience</h3>

		<!-- Add Experience Button -->
		<el-button @click="addExperience" style="margin-bottom: 20px;">Add Experience</el-button>

		<div v-for="(experience, index) in data" :key="index">
			<el-form-item label="Job Title">
				<el-input v-model="experience.jobTitle" placeholder="Enter job title"></el-input>
			</el-form-item>
			<el-form-item label="Company">
				<el-input v-model="experience.companyName" placeholder="Enter company name"></el-input>
			</el-form-item>
			<el-form-item label="Start Date">
				<el-date-picker v-model="experience.startDate" type="date"
					placeholder="Select start date"></el-date-picker>
			</el-form-item>
			<el-form-item label="End Date">
				<el-date-picker v-model="experience.endDate" type="date" placeholder="Select end date"
					:disabled="experience.currentJob"></el-date-picker>
				<el-checkbox style="margin-left: 10px;" v-model="experience.currentJob">I currently work here</el-checkbox>
			</el-form-item>
			<el-form-item label="Location">
				<el-input v-model="experience.location" placeholder="Enter location"></el-input>
			</el-form-item>
			<el-form-item label="Description">
				<el-input type="textarea" v-model="experience.jobDescription"
					placeholder="Enter job description"></el-input>
			</el-form-item>
			<!-- Popconfirm for deleting experience -->
			<div class="action-btns">
				<el-popconfirm v-if="data.length > 0" title="Are you sure to delete this?"
					@confirm="deleteExperience(index)" placement="top">
					<template #reference>
						<el-button class="delete-btn" type="danger">
							<el-icon>
								<Delete />
							</el-icon> Remove
						</el-button>
					</template>
				</el-popconfirm>
			</div>
		</div>
	</div>
</template>

<script lang="ts">
import { defineComponent, type PropType } from 'vue';
import { Delete } from '@element-plus/icons-vue';

export default defineComponent({
	name: 'Experience',
	components: {
		Delete
	},
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
		},

		deleteExperience(index: number) {
			if (index > -1 && index < this.data.length) { // so that we delete the correct experience
				this.data.splice(index, 1);
			}
		}
	}
});
</script>
