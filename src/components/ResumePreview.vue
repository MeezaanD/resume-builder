<template>
	<el-dialog v-model="localVisible" title="Resume Preview" width="80%">
		<div id="resume-preview" class="box-card preview-card">
			<div class="personal-details">
				<div class="info">
					<h2>CV of {{ resumeData.personal.fullName }}</h2>
					<p><el-icon>
							<Briefcase />
						</el-icon> {{ resumeData.personal.jobTitle }}</p>
					<p><el-icon>
							<Phone />
						</el-icon> {{ resumeData.personal.phoneNumber }}</p>
					<p><el-icon>
							<Message />
						</el-icon> {{ resumeData.personal.email }}</p>
					<p v-if="resumeData.personal.portfolioLink"><el-icon>
							<Link />
						</el-icon> {{ resumeData.personal.portfolioLink }}</p>
					<p><el-icon>
							<Location />
						</el-icon> {{ resumeData.personal.address }}</p>
				</div>
				<img class="profile-img" :src="resumeData.personal.profilePhoto" alt="Profile Photo"
					v-if="resumeData.personal.profilePhoto" />
			</div>

			<div v-if="resumeData.summary" class="summary">
				<h3><el-icon>
						<Document />
					</el-icon> Summary</h3>
				<p class="summary-text">{{ resumeData.summary }}</p>
			</div>

			<div class="experience">
				<h3><el-icon>
						<Suitcase />
					</el-icon> Experience</h3>
				<el-timeline style="max-width: 600px; margin-top: 20px;">
					<el-timeline-item v-for="(experience, index) in resumeData.experience" :key="index"
						:timestamp="formatMonthYear(experience.startDate) + ' - ' + (experience.currentJob ? 'Present' : formatMonthYear(experience.endDate))">
						<div>
							<h4>{{ experience.jobTitle }} at {{ experience.companyName }}</h4>
							<p>{{ experience.location }}</p>
							<p>{{ experience.jobDescription }}</p>
						</div>
					</el-timeline-item>
				</el-timeline>
			</div>

			<div class="education">
				<h3><el-icon>
						<School />
					</el-icon> Education</h3>
				<el-timeline style="max-width: 600px; margin-top: 20px;">
					<el-timeline-item v-for="(education, index) in resumeData.education" :key="index"
						:timestamp="formatMonthYear(education.startDate) + ' - ' + (education.currentStudy ? 'Present' : formatMonthYear(education.endDate))">
						<div>
							<h4>{{ education.courseTitle }} at {{ education.institution }}</h4>
							<p>{{ education.location }}</p>
							<p>{{ education.qualificationType }}</p>
						</div>
					</el-timeline-item>
				</el-timeline>
			</div>

			<div v-if="resumeData.skills" class="skills">
				<h3><el-icon>
						<Star />
					</el-icon> Skills</h3>
				<ul class="skills-list">
					<li v-for="(skill, index) in resumeData.skills" :key="index">{{ skill }}</li>
				</ul>
			</div>
		</div>
		<template #footer>
			<div class="dialog-footer">
				<el-button @click="localVisible = false">Close</el-button>
			</div>
		</template>
	</el-dialog>
</template>

<script lang="ts">
import { defineComponent, PropType, ref, watch } from 'vue';
import { Briefcase, Phone, Message, Link, Location, Document, Suitcase, School, Star } from '@element-plus/icons-vue';

export default defineComponent({
	name: 'ResumePreview',
	components: {
		Briefcase,
		Phone,
		Message,
		Link,
		Location,
		Document,
		Suitcase,
		School,
		Star,
	},
	props: {
		visible: {
			type: Boolean as PropType<boolean>,
			required: true,
		},
		resumeData: {
			type: Object as PropType<any>,
			required: true,
		},
	},
	setup(props, { emit }) {
		const localVisible = ref(props.visible);

		watch(() => props.visible, (newVal) => {
			localVisible.value = newVal;
		});

		watch(localVisible, (newVal) => {
			emit('update:visible', newVal);
		});

		return {
			localVisible,
		};
	},
	methods: {
		formatMonthYear(date: string) {
			if (!date) return ''; // Handle empty date
			const options = { month: 'long', year: 'numeric' } as const;
			return new Date(date).toLocaleDateString('en-US', options);
		},
	},
});
</script>

<style scoped>
.dialog-footer {
	text-align: right;
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

.summary-text {
	word-wrap: break-word;
}

.preview-card {
	flex: 3;
}

#resume-preview {
	width: 210mm;
	min-height: 297mm;
	/* box-shadow: 0 0 10px rgba(0, 0, 0, 0.1); */
	border: 1px solid black;
	background-color: white;
	margin: auto;
}

.skills-list {
	display: flex;
	flex-wrap: wrap;
	list-style: none;
	gap: 10px;
	padding: 0;
}

.skills-list li {
	background-color: #f0f2f5;
	padding: 5px 10px;
	border-radius: 5px;
}
</style>
