<template>
  <v-footer height="30" app>
    <div
      class="text-caption text-disabled w-100 d-flex justify-center align-center"
    >
      &copy; {{ new Date().getFullYear() }}

      <a
        class="text-decoration-none on-surface ml-2"
        href="https://beamlab.co/"
        rel="noopener noreferrer"
        target="_blank"
      >
        Beam Lab Pvt. Ltd.
      </a>
    </div>
  </v-footer>
</template>

<script setup></script>

<style scoped lang="sass">
.social-link :deep(.v-icon)
  color: rgba(var(--v-theme-on-background), var(--v-disabled-opacity))
  text-decoration: none
  transition: .2s ease-in-out

  &:hover
    color: rgba(25, 118, 210, 1)
</style>
