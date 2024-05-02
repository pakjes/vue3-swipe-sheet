<script setup lang="ts">
import { computed, onMounted, ref } from 'vue'
import { onClickOutside, usePointerSwipe } from '@vueuse/core'
import { useElementSize } from '@vueuse/core'
import type { UseSwipeDirection } from '@vueuse/core'

const props = defineProps({
    ratioSheetHeight: { type: Number, default: 0.9 },
})
const overlay = ref<HTMLElement | null>(null)

const container = ref<HTMLElement | null>(null)
const { height: containerHeight } = useElementSize(container)
const headersheet = ref<HTMLElement | null>(null)
const contentsheet = ref<HTMLElement | null>(null)

const { height: headerHeight } = useElementSize(headersheet)

const bottomModifier = ref<number>(0)


const maxSheetHeight = computed(() => {

    return containerHeight.value * props.ratioSheetHeight
})

const contentContainerHeight = computed(() => {
    return `${maxSheetHeight.value - headerHeight.value}px`
})

const bottom = computed(() => {
    const maxBottomOffset = -containerHeight.value + headerHeight.value
    const bottomOffset = maxBottomOffset + bottomModifier.value
    if (maxBottomOffset < 0) {
        return `${bottomOffset}px`
    }
    else {
        return `${-containerHeight.value + bottomModifier.value}px`
    }

})

const animateSheet = ref(false)

const emit = defineEmits(["clickedOutside", "draggedDown", "expanded", "minimized"])

onClickOutside(overlay, () => emit("clickedOutside"))

const { distanceY } = usePointerSwipe(
    headersheet,
    {
        threshold: 20,
        onSwipe() {

            animateSheet.value = false
            if (containerHeight.value) {
                if (distanceY.value > 0) {
                    const length = distanceY.value
                    bottomModifier.value = length
                } else {
                    bottomModifier.value -= 3
                }

            }
        },
        onSwipeEnd(_e: PointerEvent, direction: UseSwipeDirection) {
            animateSheet.value = true
            if (distanceY.value > 0 && (distanceY.value / containerHeight.value) >= 0.3) {

                bottomModifier.value = maxSheetHeight.value - headerHeight.value
            }
            else if (bottomModifier.value < 0 && direction == "down") {
                bottomModifier.value = 0
                emit("draggedDown")
            }
            else {
                bottomModifier.value = 0
            }
        },
    },
)

onMounted(() => {
    overlay.value?.addEventListener("transitionend", () => {
        if (bottomModifier.value == maxSheetHeight.value - headerHeight.value) {
            emit("expanded")
        } else if (bottomModifier.value == 0) {
            emit("minimized")
        }
    })
})

defineOptions({
    inheritAttrs: false
})
</script>


<template>
    <div ref="container" class="container">
        <div ref="overlay" class="overlay" v-bind="$attrs" :class="{ animated: animateSheet }" :style="{ bottom }">
            <div ref="headersheet">
                <slot name="header"></slot>
            </div>
            <div ref="contentsheet" class="content-wrapper">
                <slot name="content">
                </slot>
            </div>
        </div>
    </div>


</template>

<style scoped>
.container {
    position: relative;
    overflow: hidden;
    height: 100%;
    width: 100%;
    z-index: 1000;
    bottom: 0%;
    pointer-events: none;
}

.overlay {
    width: 100%;
    height: 100%;
    position: absolute;
    display: grid;
    grid-template-rows: auto 1fr;
    pointer-events: auto;
    background-color: transparent;
}

.overlay.animated {
    transition: all 0.5s ease-in-out;
}

.content-wrapper {
    height: v-bind(contentContainerHeight);
    overflow-y: scroll;
}
</style>