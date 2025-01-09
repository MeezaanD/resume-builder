<template>
  <div>
    <h3>Education</h3>
    <!-- Add Education Button -->
	<el-button @click="addEducation" style="margin-bottom: 20px;">Add Education</el-button>

    <div v-for="(education, index) in data" :key="index">
      <el-form-item label="Course Title">
        <el-input v-model="education.courseTitle" placeholder="Enter course title"></el-input>
      </el-form-item>
      <el-form-item label="Institution">
        <el-input v-model="education.institution" placeholder="Enter institution name"></el-input>
      </el-form-item>
      <el-form-item label="Qualification">
        <el-input v-model="education.qualificationType" placeholder="Enter qualification type"></el-input>
      </el-form-item>
      <el-form-item label="Start Date">
        <el-date-picker v-model="education.startDate" type="date" placeholder="Select start date"></el-date-picker>
      </el-form-item>
      <el-form-item label="End Date">
        <el-date-picker v-model="education.endDate" type="date" placeholder="Select end date" :disabled="education.currentStudy"></el-date-picker>
        <el-checkbox v-model="education.currentStudy">I am currently studying here</el-checkbox>
      </el-form-item>
      <el-form-item label="Location">
        <el-input v-model="education.location" placeholder="Enter location"></el-input>
      </el-form-item>
		<!-- Popconfirm for deleting education -->
	  <div class="action-btns">
      <el-popconfirm
        title="Are you sure to delete this?"
        @confirm="deleteEducation(index)"
        placement="top"
      >
        <template #reference>
          <el-button class="delete-btn" type="danger">
				<el-icon><Delete /></el-icon> Remove
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
  name: 'Education',
  components: {
	Delete
  },
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
    },
    deleteEducation(index: number) {
      if (index > -1 && index < this.data.length) {
        this.data.splice(index, 1);
      }
    }
  }
});
</script>