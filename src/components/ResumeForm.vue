<template>
	<div class="resume-container">
		<el-card class="box-card form-card">
			<h2>Resume Builder</h2>
			<el-alert v-if="showSaveAlert" title="Resume saved!" type="success" show-icon
				@close="showSaveAlert = false" />
			<el-form :model="resumeData" label-width="120px">
				<PersonalDetails :data="resumeData.personal" />
				<Summary :data="resumeData.summary" />
				<Experience :data="resumeData.experience" />
				<Education :data="resumeData.education" />
				<Skills :data="resumeData.skills" />

				<!-- Save and Export Buttons -->
				<el-form-item>
					<el-button type="primary" @click="saveResume">Save</el-button>
					<el-button @click="exportToPDF">Export to PDF</el-button>
				</el-form-item>
			</el-form>
		</el-card>

		<!-- Resume Preview -->
		<el-card id="resume-preview" class="box-card preview-card">
			<div class="personal-details">
				<div class="info">
					<h2>
						CV of {{ resumeData.personal.fullName }}
					</h2>
					<p><el-icon>
							<Briefcase />
						</el-icon> {{ resumeData.personal.jobTitle }}</p>
					<p><el-icon>
							<Phone />
						</el-icon> {{ resumeData.personal.phoneNumber }}</p>
					<p><el-icon>
							<Message />
						</el-icon> {{ resumeData.personal.email }}</p>
					<p><el-icon>
							<Link />
						</el-icon> {{ resumeData.personal.portfolioLink }}</p>
					<p><el-icon>
							<Location />
						</el-icon> {{ resumeData.personal.address }}</p>
				</div>
				<img class="profile-img" :src="resumeData.personal.profilePhoto" alt="Profile Photo"
					v-if="resumeData.personal.profilePhoto" />
			</div>

			<h3><el-icon>
					<Document />
				</el-icon> Summary</h3>
			<p>{{ resumeData.summary }}</p>

			<h3><el-icon>
					<Suitcase />
				</el-icon> Experience</h3>
			<el-timeline style="max-width: 600px">
				<el-timeline-item v-for="(experience, index) in resumeData.experience" :key="index"
					:timestamp="formatMonthYear(experience.startDate) + ' - ' + (experience.currentJob ? 'Present' : formatMonthYear(experience.endDate))"
					placement="top">
					<el-card>
						<h4>{{ experience.jobTitle }} at {{ experience.companyName }}</h4>
						<p>{{ experience.location }}</p>
						<p>{{ experience.jobDescription }}</p>
					</el-card>
				</el-timeline-item>
			</el-timeline>

			<h3><el-icon>
					<School />
				</el-icon> Education</h3>
			<el-timeline style="max-width: 600px">
				<el-timeline-item v-for="(education, index) in resumeData.education" :key="index"
					:timestamp="formatMonthYear(education.startDate) + ' - ' + (education.currentStudy ? 'Present' : formatMonthYear(education.endDate))"
					placement="top">
					<el-card>
						<h4>{{ education.courseTitle }} at {{ education.institution }}</h4>
						<p>{{ education.qualificationType }}</p>
						<p>{{ education.location }}</p>
					</el-card>
				</el-timeline-item>
			</el-timeline>

			<h3><el-icon>
					<Star />
				</el-icon> Skills</h3>
			<ul>
				<li v-for="(skill, index) in resumeData.skills" :key="index">{{ skill }}</li>
			</ul>
		</el-card>
	</div>
</template>

<script lang="ts">
import { defineComponent, ref, onMounted } from 'vue';
import html2pdf from 'html2pdf.js';
import { User, Briefcase, Phone, Message, Link, Location, Document, Suitcase, School, Star } from '@element-plus/icons-vue';
import PersonalDetails from '../components/PersonalDetails.vue';
import Summary from '../components/Summary.vue';
import Experience from '../components/Experience.vue';
import Education from '../components/Education.vue';
import Skills from '../components/Skills.vue';

export default defineComponent({
	name: 'ResumeForm',
	components: {
		PersonalDetails,
		Summary,
		Experience,
		Education,
		Skills,
		User,
		Briefcase,
		Phone,
		Message,
		Link,
		Location,
		Document,
		Suitcase,
		School,
		Star
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
				profilePhoto: ''
			},
			summary: '',
			experience: [],
			education: [],
			skills: []
		});

		const showSaveAlert = ref(false);

		const saveResume = () => {
			localStorage.setItem('resumeData', JSON.stringify(resumeData.value));
			showSaveAlert.value = true;
			setTimeout(() => {
				showSaveAlert.value = false;
			}, 3000); // Hide alert after 3 seconds
		};

		const exportToPDF = () => {
			const element = document.getElementById('resume-preview') as HTMLElement;
			html2pdf().from(element).set({
				margin: 0,
				filename: 'resume.pdf',
				html2canvas: { scale: 2 },
				jsPDF: { unit: 'in', format: 'a4', orientation: 'portrait' }
			}).save();
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
			showSaveAlert,
			saveResume,
			exportToPDF,
			formatMonthYear
		};
	}
});
</script>

<style scoped>
.resume-container {
	display: flex;
	flex-direction: column;
	gap: 20px;
}

.personal-details {
	display: flex;
	justify-content: space-between;
	align-items: center;
	border: 1px solid #ebeef5;
	padding: 10px;
}

.profile-img {
	height: 150px;
	width: 150px;
	object-fit: cover;
	border-radius: 50%;
	max-width: 100%;
}

.form-card,
.preview-card {
	flex: 1;
}

#resume-preview {
	width: 210mm;
	min-height: 297mm;
	/* padding: 10mm; */
	box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
	background-color: white;
}

@media (min-width: 768px) {
	.resume-container {
		flex-direction: row;
	}
}

/* Print styles */
@media print {
	.resume-container {
		display: block;
	}

	.personal-details {
		flex-direction: column;
	}

	.form-card,
	.preview-card {
		page-break-inside: avoid;
	}

	#resume-preview {
		width: 100%;
		min-height: auto;
		padding: 0;
		box-shadow: none;
	}
}
</style>
