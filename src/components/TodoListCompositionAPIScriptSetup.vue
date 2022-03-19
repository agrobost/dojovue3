<template>
  <div>
    <p class="text-h6">
      {{ title }}
    </p>
    <div class="q-pb-xl">
      Test slots:
      <br>
      <slot name="slot1">
        fdsf
      </slot>
    </div>

    <q-form
      ref="qform"
      class="q-gutter-md"
      @submit="onSubmit"
      @reset="reset"
    >
      <q-input
        v-model="inputTodoItem"
        filled
        label="Your todo item *"
        lazy-rules
        :rules="[val => val && val.length > 0 || 'Please type something']"
      />
      <div>
        <q-btn
          label="Submit"
          type="submit"
          color="primary"
        />
      </div>
    </q-form>
    <div class="q-pa-md q-gutter-md">
      <q-list
        bordered
        class="rounded-borders"
        style="max-width: 600px"
      >
        <q-item
          v-for="(item, index) of items"
          :key="index"
        >
          <q-item-section>
            <q-item-label
              lines="1"
              class="q-mt-xs text-body2 text-weight-bold text-primary"
            >
              <span class="cursor-pointer">{{ item }}</span>
            </q-item-label>
          </q-item-section>

          <q-item-section side>
            <div class="text-grey-8 q-gutter-xs">
              <q-btn
                class="gt-xs"
                size="12px"
                flat
                dense
                round
                icon="delete"
                @click="removeItem(index)"
              />
            </div>
          </q-item-section>
        </q-item>
      </q-list>
      <p>Items count = {{ itemsCount }}</p>
    </div>
  </div>
</template>

<script setup>
import {
  ref, watch, computed, onMounted, useSlots, useAttrs,
} from 'vue';
import { useQuasar } from 'quasar';

const props = defineProps({
  title: {
    type: String,
    required: true,
  },
});

const $q = useQuasar();
const qform = ref();
const inputTodoItem = ref(null);
const items = ref([]);

const forbiddenWords = ['merde', 'putin', 'fuck', 'war'];
function onSubmit() {
  items.value.push(inputTodoItem.value);
  $q.notify({
    group: false,
    color: 'green-4',
    textColor: 'white',
    icon: 'cloud_done',
    message: `"${inputTodoItem.value}"" has been added to your todo list`,
  });
  qform.value.reset();
}

const emit = defineEmits(['created']);

const slots = useSlots();
const attrs = useAttrs();
console.log(`compositionAPI setup, attrs=${JSON.stringify(attrs)}}`);
console.log('compositionAPI setup, slots:');
console.log(slots);

onMounted(() => {
  console.log('composition api setup mounted OK');
  emit(('created'));
});

function reset() {
  inputTodoItem.value = null;
}

defineExpose({
  inputTodoItem, reset, qform,
});

function removeItem(index) {
  items.value.splice(index, 1);
}

watch(
  () => inputTodoItem.value,
  (value, prevValue) => {
    console.log(`watch input todo item => value: ${value}, prevValue: ${prevValue}`);
    for (let index = 0; index < forbiddenWords.length; index += 1) {
      const forbiddenWord = forbiddenWords[index];
      if (value != null && value.includes(forbiddenWord)) {
        qform.value.reset();
        $q.notify({
          group: false,
          color: 'red-4',
          textColor: 'white',
          icon: 'cloud_done',
          message: 'You should never use this word !!!!',
        });
        break;
      }
    }
  },
);

const itemsCount = computed(() => items.value.length);

</script>

<style lang="sass" scoped>

.truncate-chip-labels > .q-chip
  max-width: 140px
</style>
