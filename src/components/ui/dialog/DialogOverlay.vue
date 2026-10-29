<script setup>
    import { DialogOverlay, injectDialogRootContext } from 'reka-ui';
    import { cn } from '@/lib/utils';
    import { computed } from 'vue';
    import { reactiveOmit } from '@vueuse/core';
    import { storeToRefs } from 'pinia';
    import { useGeneralSettingsStore } from '@/stores/settings/general';

    const props = defineProps({
        forceMount: { type: Boolean, required: false },
        asChild: { type: Boolean, required: false },
        as: { type: null, required: false },
        class: { type: null, required: false }
    });

    const delegatedProps = reactiveOmit(props, 'class');
    const { disableGpuAcceleration } = storeToRefs(useGeneralSettingsStore());
    const rootContext = injectDialogRootContext();
    const overlayClass = computed(() =>
        cn(
            'data-[state=open]:animate-in data-[state=closed]:animate-out data-[state=closed]:fade-out-0 data-[state=open]:fade-in-0 fixed inset-0 z-50 bg-black/40',
            !disableGpuAcceleration.value && 'backdrop-blur-xs',
            props.class
        )
    );
</script>

<template>
    <DialogOverlay
        v-if="rootContext.modal.value"
        data-slot="dialog-overlay"
        v-bind="delegatedProps"
        :class="overlayClass">
        <slot />
    </DialogOverlay>
    <div
        v-else-if="rootContext.open.value"
        data-slot="dialog-overlay"
        data-state="open"
        :class="overlayClass">
        <slot />
    </div>
</template>
