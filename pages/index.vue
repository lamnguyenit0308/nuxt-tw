<template>
  <div class="min-h-full">
    <Disclosure as="nav" class="bg-gray-800" v-slot="{ open }">
      <div class="mx-auto max-w-7xl px-4 sm:px-6 lg:px-8">
        <div class="flex h-16 items-center justify-between">
          <div class="flex items-center">
            <div class="shrink-0">
              <img class="size-8" src="https://tailwindcss.com/plus-assets/img/logos/mark.svg?color=indigo&shade=500"
                alt="Your Company" />
            </div>
            <div class="hidden md:block">
              <div class="ml-10 flex items-baseline space-x-4">
                <a v-for="item in navigation" :key="item.name" :href="item.href" :class="[
                  item.current
                    ? 'bg-gray-900 text-white'
                    : 'text-gray-300 hover:bg-gray-700 hover:text-white',
                  'rounded-md px-3 py-2 text-sm font-medium',
                ]" :aria-current="item.current ? 'page' : undefined">{{ item.name }}</a>
              </div>
            </div>
          </div>
          <div class="hidden md:block">
            <div class="ml-4 flex items-center md:ml-6">
              <button type="button"
                class="relative rounded-full bg-gray-800 p-1 text-gray-400 hover:text-white focus:ring-2 focus:ring-white focus:ring-offset-2 focus:ring-offset-gray-800 focus:outline-hidden">
                <span class="absolute -inset-1.5" />
                <span class="sr-only">View notifications</span>
                <BellIcon class="size-6" aria-hidden="true" />
              </button>

              <!-- Profile dropdown -->
              <Menu as="div" class="relative ml-3">
                <div>
                  <MenuButton
                    class="relative flex max-w-xs items-center rounded-full bg-gray-800 text-sm focus:ring-2 focus:ring-white focus:ring-offset-2 focus:ring-offset-gray-800 focus:outline-hidden">
                    <span class="absolute -inset-1.5" />
                    <span class="sr-only">Open user menu</span>
                    <img class="size-8 rounded-full" :src="user.imageUrl" alt="" />
                  </MenuButton>
                </div>
                <transition enter-active-class="transition ease-out duration-100"
                  enter-from-class="transform opacity-0 scale-95" enter-to-class="transform opacity-100 scale-100"
                  leave-active-class="transition ease-in duration-75" leave-from-class="transform opacity-100 scale-100"
                  leave-to-class="transform opacity-0 scale-95">
                  <MenuItems
                    class="absolute right-0 z-10 mt-2 w-48 origin-top-right rounded-md bg-white py-1 ring-1 shadow-lg ring-black/5 focus:outline-hidden">
                    <MenuItem v-for="item in userNavigation" :key="item.name" v-slot="{ active }">
                    <a :href="item.href" :class="[
                      active ? 'bg-gray-100 outline-hidden' : '',
                      'block px-4 py-2 text-sm text-gray-700',
                    ]">{{ item.name }}</a>
                    </MenuItem>
                  </MenuItems>
                </transition>
              </Menu>
            </div>
          </div>
          <div class="-mr-2 flex md:hidden">
            <!-- Mobile menu button -->
            <DisclosureButton
              class="relative inline-flex items-center justify-center rounded-md bg-gray-800 p-2 text-gray-400 hover:bg-gray-700 hover:text-white focus:ring-2 focus:ring-white focus:ring-offset-2 focus:ring-offset-gray-800 focus:outline-hidden">
              <span class="absolute -inset-0.5" />
              <span class="sr-only">Open main menu</span>
              <Bars3Icon v-if="!open" class="block size-6" aria-hidden="true" />
              <XMarkIcon v-else class="block size-6" aria-hidden="true" />
            </DisclosureButton>
          </div>
        </div>
      </div>

      <DisclosurePanel class="md:hidden">
        <div class="space-y-1 px-2 pt-2 pb-3 sm:px-3">
          <DisclosureButton v-for="item in navigation" :key="item.name" as="a" :href="item.href" :class="[
            item.current
              ? 'bg-gray-900 text-white'
              : 'text-gray-300 hover:bg-gray-700 hover:text-white',
            'block rounded-md px-3 py-2 text-base font-medium',
          ]" :aria-current="item.current ? 'page' : undefined">{{ item.name }}</DisclosureButton>
        </div>
        <div class="border-t border-gray-700 pt-4 pb-3">
          <div class="flex items-center px-5">
            <div class="shrink-0">
              <img class="size-10 rounded-full" :src="user.imageUrl" alt="" />
            </div>
            <div class="ml-3">
              <div class="text-base/5 font-medium text-white">
                {{ user.name }}
              </div>
              <div class="text-sm font-medium text-gray-400">
                {{ user.email }}
              </div>
            </div>
            <button type="button"
              class="relative ml-auto shrink-0 rounded-full bg-gray-800 p-1 text-gray-400 hover:text-white focus:ring-2 focus:ring-white focus:ring-offset-2 focus:ring-offset-gray-800 focus:outline-hidden">
              <span class="absolute -inset-1.5" />
              <span class="sr-only">View notifications</span>
              <BellIcon class="size-6" aria-hidden="true" />
            </button>
          </div>
          <div class="mt-3 space-y-1 px-2">
            <DisclosureButton v-for="item in userNavigation" :key="item.name" as="a" :href="item.href"
              class="block rounded-md px-3 py-2 text-base font-medium text-gray-400 hover:bg-gray-700 hover:text-white">
              {{ item.name }}</DisclosureButton>
          </div>
        </div>
      </DisclosurePanel>
    </Disclosure>

    <main>
      <div class="container">
        <div class="text-section">
          <h2>Điền vào chỗ trống</h2>
          <p>
            Thủ đô của Việt Nam là
            <draggable class="blank" :list="blanks[0]" group="answers" :animation="300" item-key="id"
              ghost-class="ghost" chosen-class="chosen" @change="handleChange(0, $event)">
              <template #item="{ element }">
                <span class="filled">{{ element.text }}</span>
              </template>
            </draggable>
            và nổi tiếng với
            <draggable class="blank" :list="blanks[1]" group="answers" :animation="300" item-key="id"
              ghost-class="ghost" chosen-class="chosen" @change="handleChange(1, $event)">
              <template #item="{ element }">
                <span class="filled">{{ element.text }}</span>
              </template>
            </draggable>
            .
          </p>
        </div>
        <div class="answers-section">
          <h2>Đáp án</h2>
          <draggable :list="answers" group="answers" :animation="200" item-key="id">
            <template #item="{ element }">
              <div class="answer">{{ element.text }}</div>
            </template>
          </draggable>
        </div>
      </div>
    </main>
  </div>
</template>

<script setup>
import {
  Disclosure,
  DisclosureButton,
  DisclosurePanel,
  Menu,
  MenuButton,
  MenuItem,
  MenuItems,
} from "@headlessui/vue";
import { Bars3Icon, BellIcon, XMarkIcon } from "@heroicons/vue/24/outline";

import { ref } from 'vue';



const user = {
  name: "Tom Cook",
  email: "tom@example.com",
  imageUrl:
    "https://images.unsplash.com/photo-1472099645785-5658abf4ff4e?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=facearea&facepad=2&w=256&h=256&q=80",
};
const navigation = [
  { name: "Dashboard", href: "#", current: true },
  { name: "Team", href: "#", current: false },
  { name: "Projects", href: "#", current: false },
  { name: "Calendar", href: "#", current: false },
  { name: "Reports", href: "#", current: false },
];
const userNavigation = [
  { name: "Your Profile", href: "#" },
  { name: "Settings", href: "#" },
  { name: "Sign out", href: "#" },
];

////
////////////////
//
////
////////////////
const items = ref([
  { id: 1, name: 'Mục 1' },
  { id: 2, name: 'Mục 2' },
  { id: 3, name: 'Mục 3' },
  { id: 4, name: 'Mục 4' },
]);

const onDragEnd = () => {
  console.log('Danh sách mới:', items.value);
};

// Dữ liệu đáp án
const answers = ref([
  { id: 1, text: 'Hà Nội' },
  { id: 2, text: 'Hồ Gươm' },
  { id: 3, text: 'TP. Hồ Chí Minh' },
  { id: 4, text: 'Bến Nhà Rồng' },
]);

// Dữ liệu cho các ô trống
const blanks = ref([[], []]); // Mảng cho 2 ô trống

// Xử lý thay đổi khi kéo thả
const handleChange = (blankIndex, event) => {
  if (event.added) {
    // Khi thêm đáp án mới vào ô
    const newAnswer = event.added.element;
    if (blanks.value[blankIndex].length > 1) {
      // Nếu ô đã có đáp án, trả đáp án cũ về answers
      const oldAnswer = blanks.value[blankIndex].find(
        (item) => item.id !== newAnswer.id
      );
      if (oldAnswer) {
        blanks.value[blankIndex] = [newAnswer]; // Giữ lại đáp án mới
        answers.value.push(oldAnswer); // Đẩy đáp án cũ về answers
      }
    }
  } else if (event.removed) {
    // Khi kéo đáp án ra khỏi ô, đảm bảo ô trống
    blanks.value[blankIndex] = [];
  }
}
</script>

<style scoped>
.container {
  display: flex;
  justify-content: space-between;
  max-width: 1000px;
  margin: 20px auto;
  font-family: Arial, sans-serif;
}

.text-section,
.answers-section {
  width: 45%;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 8px;
}

.text-section {
  background-color: #f9f9f9;
}

.answers-section {
  background-color: #e6f3ff;
}

.blank {
  display: inline-block;
  width: 100px;
  height: 30px;
  border: 2px dashed #666;
  margin: 0 5px;
  vertical-align: middle;
  text-align: center;
  line-height: 30px;
  background-color: #fff;
}

.answer {
  background-color: #4caf50;
  color: white;
  padding: 8px 15px;
  margin: 5px;
  border-radius: 5px;
  cursor: move;
  display: inline-block;
}

/* .filled {
  background-color: #4caf50;
  color: white;
  border: none;
  padding: 8px 15px;
  border-radius: 5px;
} */

.ghost {
  opacity: 0.5;
  background-color: #a5d6a7;
}

.chosen {
  box-shadow: 0 0 5px rgba(0, 0, 0, 0.3);
}

h2 {
  margin-top: 0;
}
</style>
