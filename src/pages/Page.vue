<template>
  <q-page class="q-pa-lg">
    <h5 class="q-mt-none">Adicionar</h5>
    <q-input
      v-model="newPage"
      @keyup.enter="addPage"
      square
      filled
      bg-color="primary"
      placeholder="Adicionar"
      dense
    >
      <template v-slot:append>
        <q-btn @click="addPage" round dense flat icon="add" />
      </template>
    </q-input>
    <q-list class="bg-white" separator bordered>
      <q-item
        v-for="(page, index) in pages"
        :key="page.title"
        @click="page.done = !page.done"
        :class="{ 'done bg-green-1': page.done }"
        clickable
        v-ripple
      >
        <q-item-section avatar>
          <q-checkbox v-model="page.done" />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ page.title }}</q-item-label>
        </q-item-section>
        <q-item-section v-if="page.done" side>
          <q-btn
            @click.stop="editPage(index)"
            flat
            round
            dense
            color="primary"
            icon="edit"
          />
          <q-btn
            @click.stop="deletePage(index)"
            flat
            round
            dense
            color="negative"
            icon="delete"
          />
        </q-item-section>
      </q-item>
    </q-list>
  </q-page>
</template>

<script>
import { defineComponent } from "vue";

export default defineComponent({
  name: "indexPage",

  data() {
    return {
      newPage: "",
      pages: JSON.parse(localStorage.getItem("pages")) || [],
    };
  },
  methods: {
    deletePage(index) {
      this.$q
        .dialog({
          title: "Confirmação",
          message: "Tens a certeza de que queres apagar?",
          cancel: true,
          persistent: true,
        })
        .onOk(() => {
          this.pages.splice(index, 1);
          localStorage.setItem("pages", JSON.stringify(this.pages));
          this.$q.notify("Apagado com sucesso!");
        });
    },
    editPage(index) {
      const newTitle = this.$q
        .dialog({
          title: "Edição",
          prompt: {
            model: this.pages[index].title,
            type: "text",
          },
          cancel: true,
          persistent: true,
        })
        .onOk((data) => {
          this.pages[index].title = data;
          localStorage.setItem("pages", JSON.stringify(this.pages));
          this.$q.notify("Editado com sucesso!");
        });
    },
    addPage() {
      this.pages.push({
        title: this.newPage,
        done: false,
      });
      this.newPage = "";
      localStorage.setItem("pages", JSON.stringify(this.pages));
    },
  },
});
</script>

<style lang="scss">
.done {
  .q-item_label {
    text-decoration: line-through;
    color: rgba(109, 138, 7, 0.671);
  }

  .no-page {
    opacity: 0.5;
  }
}
</style>
