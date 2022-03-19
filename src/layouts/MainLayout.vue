<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated>
      <q-toolbar>
        <q-btn
          flat
          dense
          round
          icon="menu"
          aria-label="Menu"
          @click="toggleLeftDrawer"
        />

        <q-toolbar-title>Quasar App</q-toolbar-title>

        <div>Quasar v{{ $q.version }}</div>
      </q-toolbar>
    </q-header>

    <q-drawer
      v-model="leftDrawerOpen"
      show-if-above
      bordered
    >
      <q-list>
        <q-item>
          <q-btn @click="resetTodoList">
            Reset todo lists
          </q-btn>
        </q-item>
      </q-list>
    </q-drawer>

    <q-page-container>
      <q-page padding>
        <!-- <TodoListCompositionAPI
          id="id1"
          ref="composition"
          title="[TodoListCompositionAPI]"
          @created="log('event [created] works for composition setup')"
        >
          <template #slot1="slotProps">
            {{ slotProps.text }}
            [named slot example]
          </template>
        </TodoListCompositionAPI> -->

        <!-- <TodoListCompositionAPIScriptSetup
          id="id2"
          ref="compositionSetup"
          title="[TodoListCompositionAPIScriptSetup]"
          @created="log('event [created] works for composition API setup')"
        >
          [slot example]
          <template #slot1>
            [named slot example]
          </template>
        </TodoListCompositionAPIScriptSetup> -->

        <!-- <TodoListOptionAPI
          id="id3"
          ref="option"
          title="[TodoListOptionAPI]"
          @created="log('event [created] works for option API')"
        /> -->
        <DojoComponent />
      </q-page>
    </q-page-container>
  </q-layout>
</template>

<script setup>
import TodoListCompositionAPI from 'components/TodoListCompositionAPI.vue';
import TodoListCompositionAPIScriptSetup from 'components/TodoListCompositionAPIScriptSetup.vue';
import TodoListOptionAPI from 'components/TodoListOptionAPI.vue';
import DojoComponent from 'components/DojoComponent.vue';
import { ref } from 'vue';

const leftDrawerOpen = ref(false);

const composition = ref(null);
const compositionSetup = ref(null);
const option = ref(null);

function resetTodoList() {
  console.log(composition.value);
  console.log(compositionSetup.value);
  console.log(option.value);
  if (composition.value !== null) { composition.value.qform.reset(); }
  if (compositionSetup.value !== null) { compositionSetup.value.qform.reset(); }
  if (option.value !== null) { option.value.$refs.qform.reset(); }
}
function toggleLeftDrawer() {
  leftDrawerOpen.value = !leftDrawerOpen.value;
}
function log(message) {
  console.log(message);
}
</script>
