<template>
  <div
    :id="refId"
    ref="content"
    class="content"
    :type="type"
    :tag="tag"
    :visible="state.visible"
  >
    <slot class="content-body" />
  </div>
</template>

<script setup lang="ts">
import { ref, reactive, onMounted, onUnmounted } from "vue";

defineProps<{ type: string; tag: string }>();
type State = {
  visible: boolean;
  obs: IntersectionObserver | null;
};
const state = reactive<State>({
  visible: false,
  obs: null,
});
const content = ref<Element | null>(null);

// ランダム文字列でIDを作る関数
const randomStr = (): string => {
  const S = "abcdefghijklmnopqrstuvwxyz";
  const N = 16;
  return Array.from(Array(N))
    .map(() => S[Math.floor(Math.random() * S.length)])
    .join("");
};

const idTemp = randomStr();
const refId = ref<string>(idTemp);
const intersectionHandler = (e: IntersectionObserverEntry[]) => {
  const isIntersecting = !!e[0].isIntersecting;
  const bottom = e[0].boundingClientRect.bottom;
  // スクロール位置より上なら必ず表示
  if (bottom < 0) {
    state.visible = true;
    return;
  }
  // 画面内に入ったら表示
  if (isIntersecting) {
    state.visible = true;
    return;
  }
};

onMounted(() => {
  // const elem = document.querySelector(`#${idTemp}`);
  const elem = content.value;
  if (!elem) return;
  state.obs = new IntersectionObserver(intersectionHandler, {
    root: null,
    rootMargin: "0px",
    threshold: 0.3,
  });

  state.obs.observe(elem);
});

onUnmounted(() => {
  const elem = document.querySelector(`#${idTemp}`);
  if (!elem) return;
  if (!state.obs) return;
  state.obs.unobserve(elem);
});
</script>

<style acoped>
.content {
  position: relative;
  display: flex;
  background: lightgray;
  flex-direction: column;
  justify-content: space-between;
  width: 100%;
  margin: 0.75rem auto;
  border: solid 1px black;
  overflow: hidden;
}

.content[tag="P"] {
  min-height: 40vh;
}

.content[tag="H2"] {
  margin-top: 8rem;
}

.content[tag="H3"] {
  margin-top: 5rem;
}
.content-body {
  position: absolute;
  top: 1;
  width: 100%;
  height: 100%;
}

/* 透明度 */
.content[type="opacity"] {
  top: 0;
  opacity: 1;
  transition: opacity 1s, top 1s;
  z-index: -1;
}

.content[visible="false"][type="opacity"] {
  opacity: 0;
  top: 50px;
}

.content[type="cover"]::after {
  display: block;
  position: absolute;
  left: -50%;
  width: 200%;
  height: 100%;
  z-index: 1;
  background-color: lightgreen;
  content: "";
  transform: skewX(-45deg);
}

.content[type="cover"][visible="true"]::after {
  width: 0;
  transition: width 1s, height 1s;
}
</style>
