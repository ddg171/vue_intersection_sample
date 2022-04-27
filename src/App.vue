<script setup lang="ts">
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://v3.vuejs.org/api/sfc-script-setup.html#sfc-script-setup
import { reactive, computed } from "vue";
import Content from "./components/Content.vue";
import InnerHtml from "./components/InnerHtml.vue";
import Navber from "./components/Navber.vue";

const payload =
  "<h3>コンテンツ</h3><p>ここに本文。ここに本文。ここに本文。ここに本文。ここに本文。ここに本文。</p><h4>コンテンツ</h4><p>ここに本文。ここに本文。ここに本文。ここに本文。ここに本文。ここに本文。</p><p>ここに本文。ここに本文。ここに本文。ここに本文。ここに本文。ここに本文。</p><h3>コンテンツ</h3><p>ここに本文。ここに本文。ここに本文。ここに本文。ここに本文。ここに本文。</p><p>ここに本文。ここに本文。ここに本文。ここに本文。ここに本文。ここに本文。</p>";

const state = reactive<{ innerHtmlRaw: string }>({ innerHtmlRaw: payload });

const innerHtmlList = computed((): { html: string; tag: string }[] => {
  const parser = new DOMParser();
  const doc = parser.parseFromString(state.innerHtmlRaw, "text/html");
  const elems = doc.querySelectorAll(" h2 ,h3,h4,p");
  const body = doc.querySelector("body");
  body?.querySelector("h3")?.setAttribute("id", "hoeg-h2");
  if (body) {
    console.log(
      new XMLSerializer()
        .serializeToString(body)
        .replace('<body xmlns="http://www.w3.org/1999/xhtml">', "")
        .replace("</body>", "")
    );
  }

  return Array.from(elems).map((e: HTMLElement | Element) => {
    return {
      html: `<${e.tagName}>${e.innerHTML}</${e.tagName}>`,
      tag: e.tagName,
    };
  });
});
</script>

<template>
  <Navber></Navber>
  <div id="header">
    <p>ここに良い感じの画像</p>
  </div>
  <div id="contents">
    <Content
      v-for="(c, i) in innerHtmlList"
      :key="i"
      :type="c.tag !== 'P' ? 'cover' : 'opacity'"
      :tag="c.tag"
      ><InnerHtml :inner-html="c.html"
    /></Content>
    <Content type="cover" tag="h3">l</Content>
  </div>
</template>

<style>
body {
  margin: 0;
  padding: 0;
}
#header {
  background-color: antiquewhite;
  width: 100%;
  min-height: 70vh;
  margin-bottom: 7rem;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
