<template>
  <div>
    <h3>Summary</h3>
    <el-form-item label="Summary">
      <el-input type="textarea" v-model="localData" placeholder="Enter a brief summary"></el-input>
    </el-form-item>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, watch } from 'vue';

export default defineComponent({
  name: 'Summary',
  props: {
    modelValue: {
      type: String,
      required: true
    }
  },
  setup(props, { emit }) {
    // Create a reactive reference for local data, initialized with the prop value
    const localData = ref(props.modelValue);

    // Watch for changes in the prop value and update localData accordingly
    watch(() => props.modelValue, (newValue) => {
      localData.value = newValue;
    });

    // Watch for changes in localData and emit an event to update the parent component
    watch(localData, (newValue) => {
      emit('update:modelValue', newValue);
    });

    // Return localData to be used in the template
    return {
      localData
    };
  }
});
</script>