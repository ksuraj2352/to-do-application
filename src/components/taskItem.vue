<template>
  <v-list-item>
    <div class="d-flex justify-space-between flex-grow-1 flex-wrap">
      <v-btn
        :class="[
          isCompleted ? 'text-decoration-line-through' : 'text-decoration-none',
        ]"
        @click="$emit('toggleTaskStatus')"
        :color="!isCompleted ? 'error' : 'grey'"
        small
        plain
      >
        <v-icon class="me-3">mdi-calendar-check-outline</v-icon>
        {{ task.taskName }}
      </v-btn>

      <v-btn color="error" small plain @click="$emit('remove')">
        <v-icon class="me-3">mdi-delete</v-icon>
      </v-btn>
    </div>
  </v-list-item>
</template>

<script>
export default {
  props: ["task"],
  emits : ['toggleTaskStatus','remove'],
  data() {
    return {};
  },

  computed: {
    isCompleted() {
      return this.task.isCompleted;
    },
  },
  
};
</script>

<style scoped>
.btnchange {
  width: 36px;
  min-width: 36px;
}
</style>