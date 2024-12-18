<template>
  <div>
    <button @click="toggleTheme">
      {{ currentTheme === "light" ? "🌞 Светлая тема" : "🌙 Тёмная тема" }}
    </button>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, onMounted } from "vue";

export default defineComponent({
  name: "ThemeSwitcher",
  setup() {
    const currentTheme = ref<"light" | "dark">("light");

    const applyTheme = (theme: "light" | "dark") => {
      document.body.classList.remove("light-theme", "dark-theme");
      document.body.classList.add(`${theme}-theme`);
    };

    const toggleTheme = () => {
      currentTheme.value = currentTheme.value === "light" ? "dark" : "light";
      applyTheme(currentTheme.value);
      localStorage.setItem("theme", currentTheme.value);
    };

    onMounted(() => {
      const savedTheme = localStorage.getItem("theme") as
        | "light"
        | "dark"
        | null;
      if (savedTheme) {
        currentTheme.value = savedTheme;
        applyTheme(savedTheme);
      } else {
        applyTheme(currentTheme.value);
      }
    });

    return {
      currentTheme,
      toggleTheme,
    };
  },
});
</script>

<style scoped></style>