<template>
	<div class="relative">
    <BaseTooltip text="Settings">
      <button
        @click="isOpen = !isOpen"
        class="relative p-2 focus:outline-none"
      >
        <BaseIcon name="dotsVertical" class="w-5 h-5" />
      </button>
    </BaseTooltip>
    <transition
      enter-active-class="transition ease-out duration-100"
      enter-from-class="transition opacity-0 scale-95"
      enter-to-class="transform opacity-100 scale-100"
      leave-active-class="transition ease-in duration-75"
      leave-from-class="transform opacity-100 scale-100"
      leave-to-class="transform opacity-0 scale-95"
    >
      <div
        v-show="isOpen"
        ref="dropdown"
        @keydown.esc="isOpen = false"
        tabindex="-1"
        :class="dropdownClasses"
      >
        <section class="py-2 border-b">
          <ul>
            <DropdownSettingsListItem
              v-for="listItem in listItems.slice(0, 8)"
              :key="listItem.label"
              :label="listItem.label"
              :icon="listItem.icon"
              :with-sub-menu="listItem.withSubMenu"
            />
          </ul>
        </section>
        <section class="py-2">
          <ul>
            <DropdownSettingsListItem
              :label="listItems[8].label"
              :with-sub-menu="listItems[8].withSubMenu"
            />
          </ul>
        </section>
      </div>
    </transition>
  </div>
</template>

<script>
import BaseIcon from "./BaseIcon.vue"
import BaseTooltip from "./BaseTooltip.vue"
import DropdownSettingsListItem from "./DropdownSettingsListItem.vue"

export default {
  components: {
    BaseIcon,
    BaseTooltip,
    DropdownSettingsListItem
  },
  data() {
    return {
      isOpen: false,
      dropdownClasses: [
        "z-10",
        "absolute",
        "top-9",
        "-right-full",
        "sm:right-0",
        "bg-white",
        "w-72",
        "border",
        "border-t-0",
        'focus:outline-none'
      ],
      listItems: [
        {
          label: 'Appearance: Light',
          icon: 'sun',
          withSubMenu: true
        },
        {
          label: 'Language: English',
          icon: 'translate',
          withSubMenu: true
        },
        {
          label: 'Location: Kazakhstan',
          icon: 'globeAlt',
          withSubMenu: true
        },
        {
          label: 'Settings',
          icon: 'cog',
          withSubMenu: false
        },
        {
          label: 'Your data in YouTube',
          icon: 'shieldCheck',
          withSubMenu: false
        },
        {
          label: 'Help',
          icon: 'questionMarkCircle',
          withSubMenu: false
        },
        {
          label: 'Send Feedback',
          icon: 'chatAlt',
          withSubMenu: false
        },
        {
          label: 'Keyboard shortcuts',
          icon: 'calculator',
          withSubMenu: false
        },
        {
          label: 'Restricted Mode: Off',
          icon: null,
          withSubMenu: true
        }
      ]
    }
  },
  watch: {
    isOpen () {
      this.$nextTick(() => this.isOpen && this.$refs.dropdown.focus())
    }
  },
  mounted () {
    window.addEventListener('click', event => {
      if (!this.$el.contains(event.target)) {
        this.isOpen = false
      }
    })
  }
}
</script>