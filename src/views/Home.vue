<template>
  <ResumeForm />
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import ResumeForm from '../components/ResumeForm.vue';

export default defineComponent({
  name: 'Home',
  components: {
    ResumeForm
  }
});
</script>
