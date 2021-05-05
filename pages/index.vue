<template lang="pug">
v-app
  v-container
    v-card
      v-card-title.indigo.white--text.text-h3 Scheduler
      v-row.pa-5
        v-col(cols="12", md="4")
          v-date-picker(v-model="date", color="indigo")
        v-col(cols="12", md="8")
          v-text-field(v-model="task", label="Task", @change="add")
            template(v-slot:prepend)
              v-chip.indigo(dark) {{ date }}
          v-list(dense)
            v-fade-transition(group, mode="out-in")
              div(v-for="s in schedules", :key="s.id")
                v-list-item
                  v-list-item-icon
                    v-chip.indigo(dark) {{ s.date }}
                  v-list-item-content {{ s.task }}
                  v-list-item-action
                    v-btn(icon, @click="remove(s.id)")
                      v-icon mdi-close
                v-divider
</template>
<script lang="ts">
import { defineComponent, ref } from "@vue/composition-api";

export default defineComponent({
  setup() {
    const date = ref<string>(new Date().toISOString().slice(0, 10));
    const task = ref<string>("");
    const schedules = ref<{ id: number; date: string; task: string }[]>([]);
    let id = 0;
    const add = () => {
      schedules.value.push({ id: id++, date: date.value, task: task.value });
      schedules.value.sort((a, b) => (a.date < b.date ? -1 : 1));
      task.value = "";
    };
    const remove = (removeid: number) => {
      schedules.value = schedules.value.filter((n) => n.id !== removeid);
      console.log(schedules.value, removeid);
    };
    return {
      date,
      task,
      schedules,
      add,
      remove,
    };
  },
});
</script>
<style lang="sass">
.v-move
  transition: transform 1s
</style>