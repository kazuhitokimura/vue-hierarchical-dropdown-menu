<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue'

const isOpen = ref(false)
const isSubContentsOpen = ref(false)

const toggleDropdown = () => {
  // isOpen または isSubContentsOpen が true の場合、両方を false にする
  if (isOpen.value || isSubContentsOpen.value) {
    isOpen.value = false
    isSubContentsOpen.value = false
  } else {
    // そうでない場合は通常通りのトグル操作
    isOpen.value = !isOpen.value
  }
}

const navigateDropdown = () => {
  isOpen.value = false
  isSubContentsOpen.value = true
}

const backDropdown = () => {
  isOpen.value = true
  isSubContentsOpen.value = false
}

const closeDropdown = (event) => {
  // クリックした場所がドロップダウンメニュー内でない場合、メニューを閉じる
  if (!event.target.closest('.dropdown-menu') && !event.target.closest('.dropdown')) {
    isOpen.value = false
    isSubContentsOpen.value = false
  }
}

onMounted(() => {
  document.addEventListener('click', closeDropdown)
})

onBeforeUnmount(() => {
  document.removeEventListener('click', closeDropdown)
})
</script>

<template>
  <div class="dropdown">
    <button class="trigger" @click="toggleDropdown">Menu</button>

    <!--ドロップダウンメニュー-->
    <ul v-if="isOpen" class="dropdown-menu">
      <li class="dropdown-navigate-trigger">
        <button @click="navigateDropdown" class="dropdown-navigate-trigger-btn">
          contentsforaction
          <span class="material-symbols-outlined"> chevron_right </span>
        </button>
      </li>
      <li class="dropdown-menu-item">action</li>
      <li class="dropdown-menu-item">action</li>
    </ul>

    <!--サブコンテンツ-->
    <ul v-if="isSubContentsOpen" class="dropdown-sub-contents">
      <li class="dropdown-back-trigger">
        <button @click="backDropdown" class="dropdown-back-trigger-btn">
          <span class="material-symbols-outlined"> chevron_left </span></button
        >contentsforaction
      </li>
      <li class="dropdown-sub-menu-item">hoge <button class="action-sample">action</button></li>
      <li class="dropdown-sub-menu-item">fuga <button class="action-sample">action</button></li>
    </ul>
  </div>
</template>

<style scoped>
.trigger:focus-visible {
  outline-offset: 2px;
  outline: 2px solid blue;
}

/* ドロップダウンメニュー */
.dropdown-menu {
  display: block;
  position: absolute;
  background-color: white;
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2);
  z-index: 1;
  padding: 4px 0px 0px 0px;
}

.dropdown-menu-item {
  list-style: none;
  padding: 8px 16px;
  cursor: pointer;
}

.dropdown-menu-item:hover {
  background-color: blue;
  color: white;
}

.dropdown-navigate-trigger {
  list-style: none;
  padding: 0px;
}

.dropdown-navigate-trigger-btn {
  list-style: none;
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
  padding: 8px 16px;
  gap: 12px;
  background-color: white;
  border: none;
  color: var(--color-text);
  line-height: 1.6;
  font-size: 15px;
  cursor: pointer;
  width: 100%;
  text-align: left;
}

.dropdown-navigate-trigger-btn:hover {
  background-color: blue;
  color: white;
}

.dropdown-navigate-trigger-btn:focus-visible {
  background-color: blue;
  color: white;
}

/* サブコンテンツ */
.dropdown-sub-contents {
  display: block;
  position: absolute;
  background-color: white;
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2);
  z-index: 1;
  padding: 4px 0px 4px 0px;
}

.dropdown-back-trigger {
  display: flex;
  flex-direction: row;
  gap: 4px;
  align-items: center;
  list-style: none;
  padding: 4px 32px 4px 8px;
  font-weight: bold;
}

.dropdown-back-trigger-btn {
  list-style: none;
  display: flex;
  align-items: center;
  justify-content: center;
  width: 32px;
  height: 32px;
  background-color: white;
  border: none;
  color: var(--color-text);
  line-height: 1.6;
  font-size: 15px;
  cursor: pointer;
}

.dropdown-back-trigger-btn:hover {
  background-color: whitesmoke;
}

.dropdown-back-trigger-btn:focus-visible {
  outline-offset: 2px;
  outline: 2px solid blue;
}

.dropdown-sub-menu-item {
  list-style: none;
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  padding: 8px 16px;
  gap: 12px;
}

.action-sample:focus-visible {
  outline-offset: 2px;
  outline: 2px solid blue;
}

.material-symbols-outlined {
  line-height: 0;
}
</style>
