<script setup lang="ts">
import { onMounted, reactive, ref } from "vue";
import JobListing from "./JobListing.vue";
import axios from "axios";
import PulseLoader from "vue-spinner/src/PulseLoader.vue";

const state = reactive({ jobs: [], isLoading: true });

const props = defineProps({
  limit: Number,
  showButton: {
    type: Boolean,
    default: false,
  },
});

onMounted(async () => {
  try {
    const res = await axios.get("/api/jobs");
    state.jobs = res.data;
  } catch (error) {
    console.error(error);
  } finally {
    state.isLoading = false;
  }
});
</script>

<template>
  <section class="bg-blue-50 px-4 py-10">
    <div class="container-xl lg:container m-auto">
      <h2 class="text-3xl font-bold text-green-500 mb-6 text-center">
        Browse Jobs
      </h2>
      <div v-if="state.isLoading" class="text-center text-gray-500 py-6">
        <PulseLoader />
      </div>
      <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
        <div
          v-for="job in state.jobs.slice(0, limit || state.jobs.length)"
          :key="job.id"
        >
          <JobListing :job="job" />
        </div>
      </div>
    </div>
  </section>
  <section v-if="showButton" class="m-auto max-w-lg my-10 px-6">
    <a
      href="/jobs"
      class="block bg-black text-white text-center py-4 px-6 rounded-xl hover:bg-gray-700"
      >View All Jobs</a
    >
  </section>
</template>
