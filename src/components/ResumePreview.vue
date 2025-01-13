<template>
  <el-dialog v-model="localVisible" title="Resume Preview" width="80%">
    <div id="resume-preview" class="box-card preview-card">
      <div class="personal-details">
        <div class="info">
          <div class="profile-header">
            <h2>CV of {{ resumeData.personal.fullName }}</h2>
				<h4>
					{{ resumeData.personal.jobTitle }}
				</h4>
          </div>
          <div class="contact-details">
            <p>
              <el-icon><Phone /></el-icon> {{ resumeData.personal.phoneNumber }}
            </p>
            <p>
              <el-icon><Message /></el-icon> {{ resumeData.personal.email }}
            </p>
            <p v-if="resumeData.personal.portfolioLink">
              <el-icon><Link /></el-icon> {{ resumeData.personal.portfolioLink }}
            </p>
            <p>
              <el-icon><Location /></el-icon> {{ resumeData.personal.address }}
            </p>
          </div>
        </div>
        <img
          class="profile-img"
          :src="resumeData.personal.profilePhoto"
          alt="Profile Photo"
          v-if="resumeData.personal.profilePhoto"
        />
      </div>

      <div v-if="resumeData.summary" class="summary">
        <h3>
          <el-icon><Document /></el-icon> Summary
        </h3>
        <p class="summary-text">{{ resumeData.summary }}</p>
      </div>

      <div class="experience-education-container">
        <div class="experience">
          <h3>
            <el-icon><Suitcase /></el-icon> Experience
          </h3>
          <el-timeline>
            <el-timeline-item
              v-for="(experience, index) in resumeData.experience"
              :key="index"
            >
              <div>
                <h5 class="title">
                  <el-icon><Briefcase /></el-icon> {{ experience.jobTitle }} at {{ experience.companyName }}
                </h5>
                <div class="timestamp-location">
                  <span class="timestamp">
                    <el-icon><Calendar /></el-icon> {{ formatMonthYear(experience.startDate) + ' - ' + (experience.currentJob ? 'Present' : formatMonthYear(experience.endDate)) }}
                  </span>
                  <span class="location">
                    <el-icon><Location /></el-icon> {{ experience.location }}
                  </span>
                </div>
                <div class="details">
                  <p>{{ experience.jobDescription }}</p>
                </div>
              </div>
            </el-timeline-item>
          </el-timeline>
        </div>

        <div class="education">
          <h3>
            <el-icon><School /></el-icon> Education
          </h3>
          <el-timeline>
            <el-timeline-item
              v-for="(education, index) in resumeData.education"
              :key="index"
            >
              <div>
                <h5 class="title">{{ education.courseTitle }} at {{ education.institution }}
                </h5>
                <div class="timestamp-location">
                  <span class="timestamp">
                    <el-icon><Calendar /></el-icon> {{ formatMonthYear(education.startDate) + ' - ' + (education.currentStudy ? 'Present' : formatMonthYear(education.endDate)) }}
                  </span>
                  <span class="location">
                    <el-icon><Location /></el-icon> {{ education.location }}
                  </span>
                </div>
                <div class="details">
                  <p>{{ education.qualificationType }}</p>
                </div>
              </div>
            </el-timeline-item>
          </el-timeline>
        </div>
      </div>

      <div v-if="resumeData.skills" class="skills">
        <h3>
          <el-icon><Star /></el-icon> Skills
        </h3>
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
import { Briefcase, Phone, Message, Link, Location, Document, Suitcase, School, Star, Calendar} from '@element-plus/icons-vue';

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
h3 {
  border-bottom: 1px solid #ebeef5;
  padding-bottom: 5px;
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
	margin: 0;
	padding: 0;
	gap: 10px;
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
  border: 1px solid black;
  background-color: white;
  margin: auto;
  padding: 15px;
  font-size: 12px;
}

.experience-education-container {
  display: flex;
  justify-content: space-between;
  gap: 20px;
}

.experience,
.education {
  width: 50%;
}

.title {
  margin: 0;
}

.timestamp-location {
  display: flex;
  justify-content: space-between;
  align-items: center;
  font-size: 12px;
  margin-top: 5px;
}

.timestamp {
  display: flex;
  align-items: center;
  font-size: 12px;
  font-weight: light;
}

.location {
  display: flex;
  align-items: center;
  font-size: 12px;
}

.details {
  border-top: 1px solid #ebeef5;
  font-size: 12px;
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