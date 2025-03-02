<template>
  <div
    :class="{
      'bg-gray-900 text-gray-100': darkMode,
      'bg-white text-gray-800': !darkMode
    }"
    class="transition-colors duration-300 flex flex-col items-center justify-center w-screen h-screen"
  >
    <div class="w-full flex justify-end mb-4">
      <button
        @click="toggleDarkMode"
        class="p-4 transition-colors duration-200"
      >
        {{ darkMode ? "☀️" : "🌙" }}
      </button>
    </div>

    <div
      class="flex-1 flex flex-col items-center justify-center w-full max-w-xl p-6 rounded-lg"
      :class="{
        'bg-gray-800 shadow-blue-500/30': darkMode,
        'bg-white shadow-lg': !darkMode
      }"
    >
      <div class="w-full text-center mb-6">
        <h1
          class="text-3xl font-bold mb-1"
          :class="{ 'text-blue-400': darkMode, 'text-blue-600': !darkMode }"
        >
          <div data-v-938b83b0="" class="min-h-15 bg-transparent">
            <div
              data-v-938b83b0=""
              class="text italic text-4xl"
              style="font-family: 'Dancing Script'"
            >
              <span data-v-938b83b0="" class="px-1">Signature Generator</span>
            </div>
          </div>
        </h1>
        <p
          class="text-sm"
          :class="{ 'text-gray-400': darkMode, 'text-gray-600': !darkMode }"
        >
          Generate a professional digital signature
        </p>
      </div>

      <div class="w-full mb-4">
        <label
          for="name"
          class="block mb-2 font-medium"
          :class="{ 'text-gray-300': darkMode, 'text-gray-700': !darkMode }"
        >
          Enter Your Name:
        </label>
        <input
          id="name"
          v-model="name"
          type="text"
          :class="{
            'bg-gray-700 border-gray-600 focus:ring-blue-500 text-white placeholder-gray-400':
              darkMode,
            'bg-white border-gray-300 focus:ring-blue-600 text-gray-900 placeholder-gray-500':
              !darkMode
          }"
          class="w-full p-2 border rounded focus:outline-none focus:ring-2 transition-colors duration-200"
          placeholder="Enter your name"
        />
      </div>

      <div class="w-full mb-4">
        <label
          for="font"
          class="block mb-2 font-medium"
          :class="{ 'text-gray-300': darkMode, 'text-gray-700': !darkMode }"
        >
          Choose a Font:
        </label>
        <select
          id="font"
          v-model="selectedFont"
          :class="{
            'bg-gray-700 border-gray-600 focus:ring-blue-500 text-white':
              darkMode,
            'bg-white border-gray-300 focus:ring-blue-600 text-gray-900':
              !darkMode
          }"
          class="w-full p-2 border rounded focus:outline-none focus:ring-2 transition-colors duration-200"
        >
          <option v-for="font in fonts" :key="font" :value="font">
            {{ font }}
          </option>
        </select>
      </div>

      <div class="w-full mb-4">
        <label
          for="style"
          class="block mb-2 font-medium"
          :class="{ 'text-gray-300': darkMode, 'text-gray-700': !darkMode }"
        >
          Signature Style:
        </label>
        <select
          id="style"
          v-model="signatureStyle"
          :class="{
            'bg-gray-700 border-gray-600 focus:ring-blue-500 text-white':
              darkMode,
            'bg-white border-gray-300 focus:ring-blue-600 text-gray-900':
              !darkMode
          }"
          class="w-full p-2 border rounded focus:outline-none focus:ring-2 transition-colors duration-200"
        >
          <option value="classic">Classic</option>
          <option value="tech">Tech-inspired</option>
        </select>
      </div>

      <div class="w-full mb-4">
        <label
          for="color"
          class="block mb-2 font-medium"
          :class="{ 'text-gray-300': darkMode, 'text-gray-700': !darkMode }"
        >
          Choose a Color:
        </label>
        <input
          id="color"
          v-model="selectedColor"
          type="color"
          :class="{
            'bg-gray-700 border-gray-600 focus:ring-blue-500 text-white placeholder-gray-400':
              darkMode,
            'bg-white border-gray-300 focus:ring-blue-600 text-gray-900 placeholder-gray-500':
              !darkMode
          }"
          class="w-full p-1 h-10 border rounded focus:outline-none focus:ring-2 transition-colors duration-200 cursor-pointer"
        />
      </div>

      <div
        class="w-full flex items-center justify-center min-h-48 signature-content bg-transparent"
      >
        <div
          v-if="signatureGenerated"
          :style="computedSignatureStyle"
          class="relative text italic px-5 text-6xl"
        >
          <span
            v-if="signatureStyle === 'tech'"
            class="absolute -top-2 -left-2 text-xl opacity-50"
            ><</span
          >
          <span class="px-1">{{ name }}</span>
          <span
            v-if="signatureStyle === 'tech'"
            class="absolute -bottom-2 -right-3 text-xl opacity-50"
            >/></span
          >
        </div>
      </div>

      <div class="w-full flex gap-4 mt-6">
        <button
          @click="exportAsPNG"
          :disabled="exportLoading"
          :class="{
            'bg-gray-700 hover:bg-gray-600 text-gray-300': darkMode,
            'bg-gray-200 hover:bg-gray-300 text-gray-800': !darkMode,
            'opacity-50 cursor-not-allowed': exportLoading
          }"
          class="flex-1 p-3 rounded transition-colors duration-200"
        >
          {{ exportLoading ? "Exporting..." : "Export as PNG" }}
        </button>
        <button
          @click="copyHTMLCode"
          :class="{
            'bg-blue-600 hover:bg-blue-700 text-white': darkMode,
            'bg-blue-500 hover:bg-blue-600 text-white': !darkMode
          }"
          class="flex-1 p-3 rounded transition-colors duration-200"
        >
          Copy HTML Code
        </button>
      </div>
    </div>

    <footer
      class="w-full text-center py-4 text-sm"
      :class="{ 'text-gray-400': darkMode, 'text-gray-600': !darkMode }"
    >
      Signature Generator v1.0 | © 2025 | Made by
      <a href="https://shinlms404.top" class="underline">Charlie Chan</a>
    </footer>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";
import html2canvas from "html2canvas";

const name = ref("");
const selectedFont = ref("Dancing Script");
const signatureGenerated = ref(true);
const darkMode = ref(false);
const signatureStyle = ref("tech");
const selectedColor = ref("#3182ce");
const exportLoading = ref(false);
const exportError = ref("");

const fonts = ref([
  "Dancing Script",
  "Great Vibes",
  "Pacifico",
  "Kaushan Script",
  "Satisfy",
  "Alex Brush",
  "Allura",
  "Arizonia"
]);

const computedSignatureStyle = computed(() => {
  const styles = {
    fontFamily: selectedFont.value
  };

  switch (signatureStyle.value) {
    case "classic":
      styles.fontStyle = "italic";
      styles.fontWeight = "normal";
      styles.color =
        selectedColor.value || (darkMode.value ? "#90cdf4" : "#3182ce");
      break;
    case "tech":
      styles.fontWeight = "bold";
      styles.letterSpacing = "0.5px";
      styles.color =
        selectedColor.value || (darkMode.value ? "#4fd1c5" : "#319795");
      break;
  }

  return styles;
});

const generateSignature = () => {
  signatureGenerated.value = false;
  setTimeout(() => {
    signatureGenerated.value = true;
  }, 300);
};

watch([name, selectedFont, signatureStyle, selectedColor], () => {
  signatureGenerated.value = false;
  setTimeout(() => {
    signatureGenerated.value = true;
  }, 300);
});

const toggleDarkMode = () => {
  darkMode.value = !darkMode.value;
};

// 新增导出功能
const exportAsPNG = async () => {
  try {
    exportLoading.value = true;
    const element = document.querySelector(".signature-content");

    const canvas = await html2canvas(element, {
      scale: 3,
      backgroundColor: null,
      logging: false
    });

    const link = document.createElement("a");
    link.download = `signature-${Date.now()}.png`;
    link.href = canvas.toDataURL("image/png");
    link.click();
  } catch (err) {
    console.error("PNG导出失败:", err);
    exportError.value = "生成PNG时出错";
  } finally {
    exportLoading.value = false;
  }
};

const copyHTMLCode = () => {
  const HTMLContent = document.querySelector(".signature-content").outerHTML;
  navigator.clipboard
    .writeText(HTMLContent)
    .then(() => alert("HTML代码已复制到剪贴板"))
    .catch(err => {
      console.error("复制失败:", err);
      exportError.value = "复制HTML代码失败";
    });
};
</script>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Dancing+Script&family=Great+Vibes&family=Pacifico&family=Kaushan+Script&family=Satisfy&family=Alex+Brush&family=Allura&family=Arizonia&display=swap");
</style>
