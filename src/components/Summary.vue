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
    const localData = ref(props.modelValue);

    watch(() => props.modelValue, (newValue) => {
      localData.value = newValue;
    });

    watch(localData, (newValue) => {
      emit('update:modelValue', newValue);
    });

    return {
      localData
    };
  }
});
</script>