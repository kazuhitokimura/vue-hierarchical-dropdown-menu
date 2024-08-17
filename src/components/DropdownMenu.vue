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
    <button @click="toggleDropdown">Menu</button>

    <!--ドロップダウンメニュー-->
    <ul v-if="isOpen" class="dropdown-menu">
      <li class="dropdown-navigate-trigger">
        <button @click="navigateDropdown" class="dropdown-navigate-trigger-btn">
          navigateDropdown
        </button>
      </li>
      <li class="dropdown-menu-item">hoge</li>
      <li class="dropdown-menu-item">fuga</li>
    </ul>

    <!--サブコンテンツ-->
    <ul v-if="isSubContentsOpen" class="dropdown-sub-contents">
      <li class="dropdown-back-trigger">
        <button @click="backDropdown" class="dropdown-back-trigger-btn">backDropdown</button>
      </li>
      <li class="dropdown-sub-menu-item">subhoge <button>action</button></li>
      <li class="dropdown-sub-menu-item">subfuga <button>action</button></li>
    </ul>
  </div>
</template>

<style scoped>
.dropdown {
  position: relative;
  display: inline-block;
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
  padding: 4px 16px;
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

/* サブコンテンツ */
.dropdown-sub-contents {
  display: block;
  position: absolute;
  background-color: white;
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2);
  z-index: 1;
  padding: 4px 0px 0px 0px;
}

.dropdown-back-trigger {
  list-style: none;
  padding: 0px;
}

.dropdown-back-trigger-btn {
  list-style: none;
  padding: 4px 16px;
  background-color: white;
  border: none;
  color: var(--color-text);
  line-height: 1.6;
  font-size: 15px;
  cursor: pointer;
  width: 100%;
  text-align: left;
}

.dropdown-back-trigger-btn:hover {
  background-color: blue;
  color: white;
}

.dropdown-sub-menu-item {
  list-style: none;
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  padding: 8px 16px;
  gap: 24px;
}
</style>
