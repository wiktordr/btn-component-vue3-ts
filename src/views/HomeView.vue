<script lang="ts" setup>
import AppButton from "@/components/app-button/AppButton.vue";

const onClick = (event: MouseEvent) => {
  console.log(event)
}
</script>

<template>
  <div class="home">
    <AppButton color="red" @click="onClick">
      Text text text text text text
    </AppButton>

    <AppButton
      size="large"
      color="red"
      disabled
    >
      Text
    </AppButton>

    <AppButton
      variant="outline"
      size="small"
      to="/about"
      block
    >
      Go to about
    </AppButton>
  </div>
</template>
