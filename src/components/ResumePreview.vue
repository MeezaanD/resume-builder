<template>
	<el-dialog v-model="localVisible" title="Resume Preview" width="80%">
		<div id="resume-preview" class="box-card preview-card">
			<div class="resume-content">
				<div class="personal-details">
					<div class="info">
						<div class="profile-header">
							<h2 class="small-heading">{{ resumeData.personal.fullName }}</h2>
							<h4 class="smaller-heading">{{ resumeData.personal.jobTitle }}</h4>
						</div>
						<div class="contact-details">
							<p class="contact-detail">
								<el-icon>
									<Phone />
								</el-icon> {{ resumeData.personal.phoneNumber }}
							</p>
							<p class="contact-detail">
								<el-icon>
									<Message />
								</el-icon> {{ resumeData.personal.email }}
							</p>
							<p v-if="resumeData.personal.portfolioLink" class="contact-detail">
								<el-icon>
									<Link />
								</el-icon> {{ resumeData.personal.portfolioLink }}
							</p>
							<p class="contact-detail">
								<el-icon>
									<Location />
								</el-icon> {{ resumeData.personal.address }}
							</p>
						</div>
					</div>
					<img class="profile-img" :src="resumeData.personal.profilePhoto" alt="Profile Photo"
						v-if="resumeData.personal.profilePhoto" />
				</div>

				<div v-if="resumeData.summary" class="summary">
					<h3 class="small-heading">
						Summary
					</h3>
					<p class="summary-text">{{ resumeData.summary }}</p>
				</div>

				<div class="experience-education-container">
					<div class="experience">
						<h3 class="small-heading">
							Experience
						</h3>
						<el-timeline>
							<el-timeline-item v-for="(experience, index) in resumeData.experience" :key="index">
								<div>
									<h5 class="title">{{ experience.jobTitle }} at {{ experience.companyName }}</h5>
									<div class="timestamp-location">
										<span class="timestamp">
											<el-icon>
												<Calendar />
											</el-icon> {{ formatMonthYear(experience.startDate) + ' - ' +
												(experience.currentJob ? 'Present' : formatMonthYear(experience.endDate)) }}
										</span>
										<span v-if="experience.location" class="location">
											<el-icon>
												<Location />
											</el-icon> {{ experience.location }}
										</span>
									</div>
									<div class="details">
										<ul>
											<li class="description-text"
												v-for="point in experience.jobDescriptionPoints" :key="point">{{ point
												}}</li>
										</ul>
									</div>
								</div>
							</el-timeline-item>
						</el-timeline>
					</div>

					<div class="education-custom-sections">
						<div class="education">
							<h3 class="small-heading">
								Education
							</h3>
							<el-timeline>
								<el-timeline-item v-for="(education, index) in resumeData.education" :key="index">
									<div>
										<h5 class="title">{{ education.courseTitle }} at {{ education.institution }}
										</h5>
										<div class="timestamp-location">
											<span class="timestamp">
												<el-icon>
													<Calendar />
												</el-icon> {{ formatMonthYear(education.startDate) + ' - ' +
													(education.currentStudy ? 'Present' :
														formatMonthYear(education.endDate)) }}
											</span>
											<span v-if="education.location" class="location">
												<el-icon>
													<Location />
												</el-icon> {{ education.location }}
											</span>
										</div>
										<div class="details">
											<p>{{ education.qualificationType }}</p>
										</div>
									</div>
								</el-timeline-item>
							</el-timeline>
						</div>

						<div v-for="(section, index) in resumeData.customSections" :key="index" class="custom-section">
							<h3 class="small-heading">{{ section.name }}</h3>
							<p>{{ section.value }}</p>
						</div>
					</div>
				</div>

				<div v-if="resumeData.skills" class="skills">
					<h3 class="small-heading">
						Skills
					</h3>
					<ul class="skills-list">
						<li v-for="(skill, index) in resumeData.skills" :key="index">{{ skill }}</li>
					</ul>
				</div>
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
import { Briefcase, Phone, Message, Link, Location, Document, Suitcase, School, Star, Calendar } from '@element-plus/icons-vue';

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
		Calendar,
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
h3,
h4,
h5 {
	margin: 0;
	padding: 0;
}

h3.small-heading,
h4.smaller-heading {
	font-size: 14px;
}

.contact-detail,
.summary-text,
.description-text {
	font-size: 10.5px !important;
}

.dialog-footer {
	text-align: right;
}

.personal-details {
	display: flex;
	justify-content: space-between;
	align-items: center;
	padding-right: 1rem;
}

.profile-header {
	margin: 0;
	padding: 0;
}

.info {
	display: flex;
	flex-direction: column;
	margin: 0;
	padding: 0;
}

.contact-details {
	display: flex;
	gap: 10px;
}

.summary-text {
	word-wrap: break-word;
	max-width: 90%;
}

#resume-preview {
	width: 210mm;
	height: auto;
	background-color: white;
	margin: auto;
	padding: 5mm;
	font-size: 10px;
	overflow: hidden;
	display: flex;
	flex-direction: column;
}

.resume-content {
	display: flex;
	flex-direction: column;
}

.experience-education-container {
	display: flex;
	gap: 10px;
}

.experience,
.education-custom-sections {
	flex: 1;
	display: flex;
	flex-direction: column;
	overflow: hidden;
}

.experience-item {
	margin-top: 10px;
}

.title {
	margin-top: 0;
	padding: 0;
}

.timestamp-location {
	display: flex;
	justify-content: space-between;
	align-items: center;
	font-size: 10px;
	margin-top: 3px;
}

.timestamp-location i {
	margin-right: 2px;
}

.timestamp,
.location {
	display: flex;
	align-items: center;
	font-size: 10px;
}

.timestamp i {
	margin-right: 5px;
}

.details {
	border-top: 1px solid #ebeef5;
	font-size: 10px;
}

.custom-section {
	margin-top: 5px;
}

.skills {
	margin-top: auto;
}

.skills-list {
	display: flex;
	flex-wrap: wrap;
	list-style: none;
	gap: 5px;
	padding: 0;
}

.skills-list li {
	background-color: #f0f2f5;
	padding: 3px 5px;
	border-radius: 3px;
}

ul {
	margin-top: 5px;
	list-style-type: disc;
	padding-left: 15px;
}
</style>
