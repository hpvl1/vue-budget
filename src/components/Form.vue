<script setup>
import { reactive, ref } from '@vue/runtime-core';
import { useVuelidate } from '@vuelidate/core';
import { required, minLength, integer } from '@vuelidate/validators';

let validForm = true;

const emit = defineEmits({
  submitForm: ({ type, value, comment }) => {
    if (type && value && comment) return true;
    return false;
  },
});

const formData = reactive({
  form: {
    type: 'INCOME',
    value: null,
    comment: null,
  },
});

const rules = {
  form: {
    type: { required },
    comment: { required, minLength: minLength(6) },
    value: { required, integer },
  },
};


async function onSubmit() {
  const valid = await v$.value.$validate();
  if (valid) {
    emit('submitForm', { ...formData.form });

    formData.form.type = 'INCOME';
    formData.form.comment = null;
    formData.form.value = null;
    v$.value.$reset();
  }
}

const v$ = useVuelidate(rules, formData);

const selectItems = ['INCOME', 'OUTCOME'];
</script>

<template>
  <div class="form-wrap">
      <v-card class="mx-auto" max-width="500" width="500">
        <v-form>
          <v-select
            v-model="formData.form.type"
            :items="selectItems"
            label="Choose type"
            type="text"
          />
          <v-text-field
            label="Comments"
            @blur="v$.form.comment.$touch()"
            v-model.trim="formData.form.comment"
            prop="comment"
            density="comfortable"
          />
          <template v-if="v$.form.comment.$dirty">
            <div v-for="error of v$.form.comment.$silentErrors" :key="error.$message">
              <!-- <div>{{ error.$message }}</div> -->
              <v-alert
                dismissible
                dark
                class="text-center mb-7"
                color="red"
              >
                {{ error.$message }}
              </v-alert>
            </div>
          </template>
          <v-text-field
            label="Value"
            @blur="v$.form.value.$touch()"
            v-model.number="formData.form.value"
            prop="value"
            density="comfortable"
          />
          <template v-if="v$.form.value.$dirty">
            <div v-for="error of v$.form.value.$silentErrors" :key="error.$message">
              <v-alert
                dismissible
                dark
                class="text-center mb-5"
                color="red"
              >
                {{ error.$message }}
              </v-alert>
            </div>
          </template>
          <v-btn color="success" class="mt-12 mb-5" @click="onSubmit">Submit</v-btn>
        </v-form>
      </v-card>
  </div>
</template>

<style scoped></style>
