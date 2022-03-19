<template>
  <div>
    <p class="text-h6">
      {{ title }}
    </p>
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

<script>
/* eslint-disable no-console */
import { defineComponent } from 'vue';
import { useQuasar } from 'quasar';

const forbiddenWords = ['merde', 'putin', 'fuck', 'war'];
// const $q = useQuasar();
export default defineComponent({
  props: {
    title: {
      type: String,
      required: true,
    },
  },
  emits: {
    // no validation
    eventWithNoValidation: null,

    // with validation
    // eslint-disable-next-line arrow-body-style
    created: (payload) => {
      // eslint-disable-next-line no-constant-condition
      return true;
    },
  },
  data() {
    return {
      items: [], qform: null, inputTodoItem: null, quasar: useQuasar(),
    };
  },
  expose: ['$'],

  computed: {
    itemsCount() {
      return this.items.length;
    },
  },

  watch: {
    inputTodoItem(value, prevValue) {
      console.log(`watch input todo item => value: ${value}, prevValue: ${prevValue}`);
      for (let index = 0; index < forbiddenWords.length; index++) {
        const forbiddenWord = forbiddenWords[index];
        if (value != null && value.includes(forbiddenWord)) {
          this.$refs.qform.reset();
          this.quasar.notify({
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
  },

  mounted() {
    console.log('option api mounted OK');
    this.$emit('created', 'my params');
    this.$emit('eventWithNoValidation');
  },
  methods: {
    reset() {
      this.inputTodoItem = null;
    },
    removeItem(index) {
      this.items.splice(index, 1);
    },
    onSubmit() {
      this.items.push(this.inputTodoItem);
      this.quasar.notify({
        group: false,
        color: 'green-4',
        textColor: 'white',
        icon: 'cloud_done',
        message: `"${this.inputTodoItem}"" has been added to your todo list`,
      });
      this.$refs.qform.reset();
    },
  },
});
</script>

<style lang="sass" scoped>

.truncate-chip-labels > .q-chip
  max-width: 140px
</style>
