<template>
  <div>
    <h3>Personal Details</h3>
    <el-form-item label="Full Name">
      <el-input v-model="data.fullName" placeholder="Enter your full name" class="auto-width"></el-input>
    </el-form-item>
    <el-form-item label="Job Title">
      <el-input v-model="data.jobTitle" placeholder="Enter your job title" class="auto-width"></el-input>
    </el-form-item>
    <el-form-item label="Phone Number">
      <el-input v-model="data.phoneNumber" placeholder="Enter your phone number" class="auto-width"></el-input>
    </el-form-item>
    <el-form-item label="Email">
      <el-input v-model="data.email" placeholder="Enter your email" class="auto-width"></el-input>
    </el-form-item>
    <el-form-item label="Portfolio Link">
      <el-input v-model="data.portfolioLink" placeholder="Enter your portfolio link" class="auto-width"></el-input>
    </el-form-item>
    <el-form-item label="Address">
      <el-input v-model="data.address" placeholder="Enter your address" class="auto-width"></el-input>
    </el-form-item>
    <el-form-item label="Profile Photo">
      <input type="file" @change="handleFileUpload" />
      <img :src="data.profilePhoto" alt="Profile Photo" v-if="data.profilePhoto" />
    </el-form-item>
  </div>
</template>

<script lang="ts">
import { defineComponent, type PropType } from 'vue';

export default defineComponent({
  name: 'PersonalDetails',
  props: {
    data: {
      type: Object as PropType<{
        fullName: string;
        jobTitle: string;
        phoneNumber: string;
        email: string;
        portfolioLink: string;
        address: string;
        profilePhoto: string;
      }>,
      required: true
    }
  },
  methods: {
    handleFileUpload(event: Event) {
      const file = (event.target as HTMLInputElement).files?.[0];
      if (file) {
        const reader = new FileReader();
        reader.onload = (e) => {
          this.data.profilePhoto = e.target?.result as string;
        };
        reader.readAsDataURL(file);
      }
    }
  }
});
</script>