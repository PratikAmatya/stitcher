<template>
  <v-app>
    <v-main class="main">
      <SubmitForm />
      <AppFooter />
    </v-main>
  </v-app>
</template>

<script>
import SubmitForm from "@/components/SubmitForm.vue";

export default {
  name: "SubmitForm",
  components: {
    SubmitForm,
  },
};
</script>

<style>
.main {
  background-image: linear-gradient(
    315deg,
    hsl(5deg 100% 74%) 0%,
    hsl(334deg 96% 80%) 30%,
    hsl(303deg 89% 85%) 46%,
    hsl(271deg 74% 91%) 63%,
    hsl(240deg 11% 96%) 100%
  );
  /* background-color: #111; */
}
</style>
