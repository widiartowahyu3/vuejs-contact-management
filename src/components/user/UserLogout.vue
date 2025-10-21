<template></template>

<script setup>
import { useLocalStorage } from "@vueuse/core";
import { userLogout } from "../../lib/api/userapi";
import { useRouter } from "vue-router";
import { alertError } from "../../lib/alert";
import { onBeforeMount } from "vue";

const token = useLocalStorage("token", "");
const router = useRouter();

async function handleLogout() {
  const response = await userLogout(token.value);
  const responseBody = await response.json();
  console.log(responseBody);

  if (response.status === 200) {
    token.value = "";
    await router.push({
      path: "/login",
    });
  } else {
    await alertError(responseBody.errors);
  }
}

onBeforeMount(async () => {
  await handleLogout();
});
</script>

<style scoped></style>
