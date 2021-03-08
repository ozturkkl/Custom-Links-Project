<template>
  <floating :event="this.$store.state.events.hoverLabel.event" class="hoverLabel">
    <div>
      {{ this.$store.state.events.hoverLabel.arg.label }}
    </div>
  </floating>
</template>

<script>
import floating from "@/components/floating/Floating.vue";

export default {
  components: {
    floating,
  },
};
</script>

<style scoped>
.hoverLabel {
  min-width: 100px;
  max-width: 180px;
  background: var(--background-color);
  border-radius: 5px;
  border: 1px solid var(--background-accent);
  padding: 3px;
}
</style>
