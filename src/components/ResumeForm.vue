<template>
	<div class="resume-container">
		<el-card class="box-card form-card">
			<h2>Resume Builder</h2>
			<el-form :model="resumeData" label-width="120px">
				<PersonalDetails :data="resumeData.personal" />
				<Summary v-model:modelValue="resumeData.summary" />
				<Experience :data="resumeData.experience" />
				<Education :data="resumeData.education" />
				<Skills :data="resumeData.skills" />
			</el-form>
		</el-card>

		<!-- Fixed Save and Export Buttons -->
		<div class="fixed-buttons">
			<el-button @click="dialogVisible = true" round>Preview</el-button>
			<el-button @click="exportToPDF" round>Export to PDF</el-button>
			<el-button type="primary" @click="saveResume" round>Save</el-button>
		</div>

		<!-- Resume Preview Dialog -->
		<ResumePreview :visible="dialogVisible" :resumeData="resumeData" @update:visible="dialogVisible = $event" />
	</div>
</template>

<script lang="ts">
import { defineComponent, ref, onMounted } from 'vue';
import html2pdf from 'html2pdf.js';
import { ElMessage } from 'element-plus';
import PersonalDetails from '../components/PersonalDetails.vue';
import Summary from '../components/Summary.vue';
import Experience from '../components/Experience.vue';
import Education from '../components/Education.vue';
import Skills from '../components/Skills.vue';
import ResumePreview from '../components/ResumePreview.vue';

export default defineComponent({
	name: 'ResumeForm',
	components: {
		PersonalDetails,
		Summary,
		Experience,
		Education,
		Skills,
		ResumePreview,
	},
	setup() {
		const resumeData = ref<any>({
			personal: {
				fullName: '',
				jobTitle: '',
				phoneNumber: '',
				email: '',
				portfolioLink: '',
				address: '',
				profilePhoto: '',
			},
			summary: '',
			experience: [],
			education: [],
			skills: [],
		});

		const dialogVisible = ref(false);

		const saveResume = () => {
			localStorage.setItem('resumeData', JSON.stringify(resumeData.value));
			ElMessage({
				message: `Resume saved for ${resumeData.value.personal.fullName}`,
				type: 'success',
				duration: 3000,
			});
		};

		const exportToPDF = () => {
			const element = document.getElementById('resume-preview') as HTMLElement;
			html2pdf()
				.from(element)
				.set({
					margin: 0,
					filename: 'resume.pdf',
					html2canvas: { scale: 2 },
					jsPDF: { unit: 'in', format: 'a4', orientation: 'portrait' },
				})
				.save();
		};

		const formatMonthYear = (date: string) => {
			if (!date) return ''; // Handle empty date
			const options = { month: 'long', year: 'numeric' } as const;
			return new Date(date).toLocaleDateString('en-US', options);
		};

		onMounted(() => {
			const savedData = localStorage.getItem('resumeData');
			if (savedData) {
				resumeData.value = JSON.parse(savedData);
			}
		});

		return {
			resumeData,
			saveResume,
			exportToPDF,
			formatMonthYear,
			dialogVisible,
		};
	},
});
</script>

<style scoped>
.resume-container {
	display: flex;
	flex-direction: column;
	align-items: center;
	gap: 20px;
	max-width: 500px;
	/* Set a fixed width for larger screens */
	width: 100%;
	/* Full width on smaller screens */
	margin: auto;
	/* Center the form horizontally */
	padding: 25px 0;
}

.box-card {
	margin-top: 10px;
	padding: 0 25px !important;
	width: 100%;
	/* Ensure the card takes full width */
}

.fixed-buttons {
	display: flex;
	gap: 5px;
	position: fixed;
	top: 10px;
	left: 50%;
	transform: translateX(-50%);
	padding: 10px 0;
	margin: auto;
	max-width: 350px;
	width: 100%;
	background: linear-gradient(90deg, #e2e2e2, #f8f8f8);
	border-radius: 8px;
	box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
	z-index: 1000;
	justify-content: center;
	align-items: center;
	flex-wrap: wrap;
	/* Allow buttons to wrap on smaller screens */
}

.fixed-buttons el-button {
	background: transparent;
	border: none;
	color: #000;
	font-weight: bold;
	padding: 5px 10px;
	border-radius: 4px;
	transition: background 0.2s ease, color 0.2s ease;
}

.fixed-buttons el-button:hover {
	background: rgba(0, 0, 0, 0.1);
	color: #fff;
	transform: translateY(-2px);
}

@media (max-width: 768px) {
	.resume-container {
		padding: 10px 0;
	}

	.fixed-buttons {
		top: 0;
		/* Adjust the position for mobile */
		left: 0;
		transform: none;
		/* Reset the transform */
		width: 100%;
		max-width: 500px;
		/* Full width on mobile */
		justify-content: center;
		/* Center the buttons */
	}

	.fixed-buttons el-button {
		margin: 5px 0;
		/* Add margin for better spacing on mobile */
	}
}

/* Print styles */
@media print {
	.resume-container {
		display: block;
	}

	.form-card,
	.preview-card {
		page-break-inside: avoid;
	}

	.fixed-buttons {
		display: none;
	}
}
</style>
