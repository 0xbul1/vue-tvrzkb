<template>
  <div class="fixed inset-0 flex items-center justify-center">
    <button
      type="button"
      @click="openModal"
      class="
        px-4
        py-2
        text-sm
        font-medium
        text-white
        bg-black
        rounded-md
        bg-opacity-20
        hover:bg-opacity-30
        focus:outline-none
        focus-visible:ring-2
        focus-visible:ring-white
        focus-visible:ring-opacity-75
      "
    >
      Open
    </button>
  </div>
  <TransitionRoot appear :show="isOpen" as="template">
    <Dialog as="div" @close="closeModal">
      <div class="fixed inset-0 z-10 overflow-y-auto">
        <div class="min-h-screen px-4 text-center">
          <TransitionChild
            as="template"
            enter="duration-300 ease-out"
            enter-from="opacity-0"
            enter-to="opacity-100"
            leave="duration-200 ease-in"
            leave-from="opacity-100"
            leave-to="opacity-0"
          >
            <DialogOverlay class="fixed inset-0" />
          </TransitionChild>

          <span class="inline-block h-screen align-middle" aria-hidden="true">
            &#8203;
          </span>

          <TransitionChild
            as="template"
            enter="duration-300 ease-out"
            enter-from="opacity-0 scale-95"
            enter-to="opacity-100 scale-100"
            leave="duration-200 ease-in"
            leave-from="opacity-100 scale-100"
            leave-to="opacity-0 scale-95"
          >
            <div
              class="
                inline-block
                w-full
                max-w-md
                p-6
                my-8
                overflow-hidden
                text-left
                align-middle
                transition-all
                transform
                bg-white
                shadow-xl
                rounded-2xl
              "
            >
              <DialogTitle
                as="h3"
                class="text-lg font-medium leading-6 text-gray-900"
              >
                Test speaker
              </DialogTitle>
              <div class="mt-2">
                <textarea text-sm text-gray-500 v-model="lyric" />
              </div>

              <div class="mt-4">
                <button
                  type="button"
                  class="
                    inline-flex
                    justify-center
                    px-4
                    py-2
                    text-sm
                    font-medium
                    text-blue-900
                    bg-blue-100
                    border border-transparent
                    rounded-md
                    hover:bg-blue-200
                    focus:outline-none
                    focus-visible:ring-2
                    focus-visible:ring-offset-2
                    focus-visible:ring-blue-500
                  "
                  @click="closeModal"
                >
                  Stop
                </button>
                <button
                  type="button"
                  class="
                    ml-8
                    inline-flex
                    justify-center
                    px-4
                    py-2
                    text-sm
                    font-medium
                    text-blue-900
                    bg-blue-100
                    border border-transparent
                    rounded-md
                    hover:bg-blue-200
                    focus:outline-none
                    focus-visible:ring-2
                    focus-visible:ring-offset-2
                    focus-visible:ring-blue-500
                  "
                  @click="speak"
                >
                  Start
                </button>
              </div>
            </div>
          </TransitionChild>
        </div>
      </div>
    </Dialog>
  </TransitionRoot>
</template>

<script>
import { ref } from 'vue';
import {
  TransitionRoot,
  TransitionChild,
  Dialog,
  DialogOverlay,
  DialogTitle,
} from '@headlessui/vue';
import { speaker, start, pause, resume } from '../hooks/speak.js';

export default {
  components: {
    TransitionRoot,
    TransitionChild,
    Dialog,
    DialogOverlay,
    DialogTitle,
  },

  setup() {
    const isOpen = ref(false);
    const lyric = ref('枭枭牛掰');

    return {
      lyric,
      isOpen,
      closeModal() {
        isOpen.value = false;
        pause();
      },
      openModal() {
        isOpen.value = true;
      },
      speak() {
        speaker(lyric.value);
        start();
        resume();
      },
    };
  },
};
</script>
