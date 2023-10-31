<template>
  <q-page class="bg-grey-3 column">
    <div class="row q-pa-sm bg-primary">
      <q-input
        @keyup.enter="addTask"
        v-model="newTask"
        bg-color="white"
        class="col"
        dense
        filled
        placeholder="Adicionar Tarefa"
        square
      >
        <template v-slot:append>
          <q-btn @click="addTask" round dense flat icon="add" />
        </template>
      </q-input>
    </div>

    <q-list class="bg-white" separator bordered>
      <q-item
        v-for="(task, index) in tasks"
        :key="task.title"
        @click="task.done = !task.done"
        :class="{ 'done bg-blue-1': task.done }"
        clickable
        v-ripple
      >
        <q-item-section avatar>
          <q-checkbox
            v-model="task.done"
            class="no-pointer-events"
            color="primary"
          />
        </q-item-section>

        <q-item-section>
          <q-item-label>{{ task.title }}</q-item-label>
        </q-item-section>

        <q-item-section v-if="task.done" side>
          <q-btn
            @click.stop="deleteTask(index)"
            color="primary"
            dense
            flat
            icon="delete"
            round
          />
        </q-item-section>
      </q-item>
    </q-list>

    <div v-if="!tasks.length" class="no-tasks absolute-center">
      <q-icon name="check" size="100px" color="primary" />
      <div class="text-h5 text-primary text-center">
        Você, esta sem tarafas!
      </div>
    </div>
  </q-page>
</template>

<script setup>
import { useQuasar } from "quasar";
import { reactive, ref } from "vue";
const $q = useQuasar();

const newTask = ref("");
const tasks = reactive([]);

const deleteTask = (index) => {
  const target = tasks.at(index);

  $q.dialog({
    title: "Confirmação",
    message: "Tens a certeza?",
    cancel: true,
    persistent: true,
  }).onOk(() => {
    tasks.splice(index, 1);
    $q.notify("Tarefa deletada!");
  });
};

const addTask = () => {
  tasks.push({ title: newTask.value, done: false });
  newTask.value = "";
};
</script>

<style lang="scss">
.done {
  .q-item__label {
    text-decoration: line-through;
    color: #bbb;
  }
}
.no-tasks {
  opacity: 0.5;
}
</style>
