<script setup lang="ts">
import "markstream-vue/index.css";
import {MarkdownRender, setCustomComponents} from "markstream-vue";
import RefNode from "@/components/chat/message_list_comps/RefNode.vue";
import ThreadNode from "@/components/chat/message_list_comps/ThreadNode.vue";
import ThemeAwareMarkdownCodeBlock from "@/components/shared/ThemeAwareMarkdownCodeBlock.vue";
import {onMounted, ref} from "vue";
import {useRoute} from 'vue-router';
import {useCustomizerStore} from '@/stores/customizer';

const route = useRoute();
const customizer = useCustomizerStore();
const customHtmlTags = ref([] as string[]);
const print_content = ref("");

setCustomComponents("chat-message", {
  ref: RefNode,
  thread: ThreadNode,
  code_block: ThemeAwareMarkdownCodeBlock,
});

onMounted(() => {
  const name = route.query?.name ? route.query?.name as string : 'default';
  print_content.value = localStorage.getItem(name) ?? "";
});
function printPage() {
  window.print();
}
</script>

<template>
  <v-app :theme="customizer.uiTheme" style="height: 100%; width: 100%;">
    <div style="padding: 20px;">
      <MarkdownRender
        custom-id="chat-message"
        :content="print_content"
        :custom-html-tags="customHtmlTags"
        :is-dark="false"
        :typewriter="false"
        :max-live-nodes="0"
    />
    <div class="bottom-print-bar">
      <button class="btn" @click="printPage">打印本页</button>
    </div>
    </div>
  </v-app>
</template>
<style lang="scss">
.bottom-print-bar {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  background: rgb(0 0 0 / 0.5);
  padding: 30px;
  display: flex;
  justify-content: center;
  align-items: center;

  .btn {
    background: #43b48c;
    color: #fff;
    padding: 10px 20px;
    border-radius: 10px;
  }
}
@media print {
  .bottom-print-bar {
    display: none;
  }
}
</style>