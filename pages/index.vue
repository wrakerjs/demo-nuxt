<script setup lang="ts">
import { Wraker } from "wraker";
import defaultWorker from "~/public/default.worker?url";

const data = ref<any>(null);
const error = ref<string | null>(null);

onMounted(() => {
  const worker = new Wraker(defaultWorker, {
    name: "my-worker",
    type: "module",
  });

  worker.fetch("/hello").then((res) => {
    if (res.error) error.value = res.error;
    else data.value = res.body;
  });
});
</script>

<template>
  <p id="data" v-if="data">{{ data }}</p>
  <p id="error" v-if="error">{{ error }}</p>
</template>

<style scoped>
p {
  font-size: 2rem;
}

#data {
  color: green;
}

#error {
  color: red;
}
</style>
