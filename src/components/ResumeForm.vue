<template>
	<div class="resume-container">
		<el-card class="box-card form-card">
			<h2>Resume Builder</h2>
			<el-form :model="resumeData" label-width="120px">
				<el-collapse v-model="activeSections">
					<el-collapse-item title="Personal Details" name="1">
						<PersonalDetails :data="resumeData.personal" />
					</el-collapse-item>
					<el-collapse-item title="Summary" name="2">
						<Summary v-model:modelValue="resumeData.summary" />
					</el-collapse-item>
					<el-collapse-item title="Experience" name="3">
						<Experience :data="resumeData.experience" />
					</el-collapse-item>
					<el-collapse-item title="Education" name="4">
						<Education :data="resumeData.education" />
					</el-collapse-item>
					<el-collapse-item title="Custom Sections" name="5">
						<CustomSections :data="resumeData.customSections" />
					</el-collapse-item>
					<el-collapse-item title="Skills" name="6">
						<Skills :data="resumeData.skills" />
					</el-collapse-item>
				</el-collapse>
			</el-form>
		</el-card>

		<!-- Fixed Save and Export Buttons -->
		<div class="fixed-buttons">
			<el-button @click="dialogVisible = true" round>Preview</el-button>
			<el-button @click="exportToPDF" round>Export to PDF</el-button>
			<el-button type="primary" @click="saveResume" round>Save</el-button>
		</div>

		<!-- Resume Preview Dialog -->
		<ResumePreview ref="resumePreview" :visible="dialogVisible" :resumeData="resumeData"
			@update:visible="dialogVisible = $event" />
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
import CustomSections from '../components/CustomSections.vue';
import ResumePreview from '../components/ResumePreview.vue';

export default defineComponent({
	name: 'ResumeForm',
	components: {
		PersonalDetails,
		Summary,
		Experience,
		Education,
		Skills,
		CustomSections,
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
			customSections: [],
		});

		const dialogVisible = ref(false);
		const resumePreviewRef = ref<InstanceType<typeof ResumePreview> | null>(null);
		const activeSections = ref<string[]>(['1', '2', '3', '4', '5', '6']);

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
			if (!date) return '';
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
			resumePreviewRef,
			activeSections,
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
	width: 100%;
	margin: auto;
	padding: 25px 20px;
	background: white;
}

.box-card {
	margin-top: 10px;
	padding: 0 25px !important;
	width: 100%;
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
		padding: 10px 20px;
	}

	.fixed-buttons {
		top: 0;
		left: 0;
		transform: none;
		width: 100%;
		max-width: 500px;
		justify-content: center;
	}

	.fixed-buttons el-button {
		margin: 5px 0;
	}
}

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
