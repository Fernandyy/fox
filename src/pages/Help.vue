<template>
  <q-page class="q-pa-lg">
    <h5 class="q-mt-none">Help</h5>
    <q-form class="bg-secondary q-pa-lg" @submit="onSubmit">
      <q-input
        label="Name"
        v-model="name"
        required
        color="primary"
        font-size="16px"
        font-weight="600"
      />
      <q-input
        label="Email"
        v-model="email"
        required
        color="primary"
        font-size="16px"
        font-weight="600"
      />
      <q-input
        label="Message"
        v-model="message"
        required
        color="primary"
        font-size="16px"
        font-weight="600"
      />
      <q-btn
        type="submit"
        label="Submit"
        color="primary"
        font-size="16px"
        font-weight="600"
      />
    </q-form>
    <div class="q-pa-lg">
      <q-alert color="green" show> Mensagem enviada com sucesso! </q-alert>
    </div>
  </q-page>
</template>

<script>
import { defineComponent } from "vue";

export default defineComponent({
  name: "PagePage",
  data() {
    return {
      name: "",
      email: "",
      message: "",
    };
  },
  methods: {
    onSubmit() {
      // Guardar os dados no LocalStorage
      this.$localStorage.setItem("name", this.name);
      this.$localStorage.setItem("email", this.email);
      this.$localStorage.setItem("message", this.message);

      // Exibir a mensagem de confirmação
      this.showMessage();
    },
    showMessage() {
      this.$refs.alert.show();
    },
  },
});
</script>
