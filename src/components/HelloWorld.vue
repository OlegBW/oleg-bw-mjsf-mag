<script setup>
import { ref } from "vue";

defineProps({
  msg: String,
});

const count = ref(0);
</script>

<template>
  <h1>{{ msg }}</h1>

  <div class="card">
    <button type="button" @click="count++">count is {{ count }}</button>
    <p>
      Edit
      <code>components/HelloWorld.vue</code> to test HMR
    </p>
  </div>

  <!-- Personal information section -->
  <div class="info">
    <h2>Інформація про себе</h2>
    <ul>
      <li><strong>Спеціальність:</strong> Інженерія програмного забезпечення</li>
      <li><strong>Курс:</strong> 2 (Магістратура)</li>
      <li><strong>Група:</strong> 8.1214</li>
      <li><strong>ПІБ:</strong> Коростильов Олег Владиславович</li>
    </ul>
  </div>

  <p>
    Check out
    <a href="https://vuejs.org/guide/quick-start.html#local" target="_blank"
      >create-vue</a
    >, the official Vue + Vite starter
  </p>
  <p>
    Learn more about IDE Support for Vue in the
    <a
      href="https://vuejs.org/guide/scaling-up/tooling.html#ide-support"
      target="_blank"
      >Vue Docs Scaling up Guide</a
    >.
  </p>
  <p class="read-the-docs">Click on the Vite and Vue logos to learn more</p>
</template>

<style scoped>
.read-the-docs {
  color: #888;
}

.info {
  margin-top: 20px;
  padding: 12px;
  border: 1px solid #ddd;
  border-radius: 8px;
  background: #f9f9f9;
  color: #000;
}
.info h2 {
  margin-bottom: 8px;
}
.info ul {
  list-style-type: none;
  padding: 0;
}
.info li {
  margin-bottom: 4px;
}
</style>
