<template>
  <div class="it-tooltip">
    <span
      ref="trigger"
      class="it-tooltip-trigger"
      @mouseenter="handleMouseEnter"
      @mouseleave="handleMouseLeave"
    >
      <slot></slot>
    </span>

    <transition :name="transition">
      <div
        v-show="show"
        ref="popover"
        class="it-tooltip-popper"
        :class="[placement && `it-tooltip--${placement}`]"
        @mouseenter="handleMouseEnter"
        @mouseleave="handleMouseLeave"
      >
        <div class="it-tooltip-content">
          <slot name="content">
            <div>{{ content }}</div>
          </slot>
        </div>
      </div>
    </transition>
  </div>
</template>

<script lang="ts">
import { defineComponent, onMounted } from 'vue'
import { usePopover } from '@/hooks'
import { Positions } from '@/models/enums'

export default defineComponent({
  name: 'it-tooltip',
  props: {
    content: [String, Number],
    disabled: Boolean,
    hoverable: Boolean,
    transition: String,
    placement: {
      type: String,
      default: Positions.T,
      validator: (value: Positions) =>
        [Positions.B, Positions.L, Positions.R, Positions.T].includes(value),
    },
    permanent: Boolean,
  },
  setup(props) {
    const {
      show,
      placement,
      disabled,
      clickable,
      transition,
      visionTimer,
      popover,
      trigger,
      permanent,
      position,
      handleMouseEnter,
      handleMouseLeave,
      hidePopover,
      showPopover,
      setPopoverPosition,
    } = usePopover(props)

    onMounted(() => {
      if (permanent.value) {
        showPopover()
      }
    })

    return {
      show,
      placement,
      disabled,
      clickable,
      transition,
      visionTimer,
      popover,
      trigger,
      position,
      handleMouseEnter,
      handleMouseLeave,
      hidePopover,
      showPopover,
      setPopoverPosition,
    }
  },
})
</script>
