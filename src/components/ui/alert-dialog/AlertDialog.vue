<script setup>
    import { DialogRoot, useForwardPropsEmits } from 'reka-ui';
    import { useDialogPopoutModal } from '@/composables/useDialogPopoutModal';

    import AlertDialogStateProvider from './AlertDialogStateProvider.vue';

    const props = defineProps({
        open: { type: Boolean, required: false },
        defaultOpen: { type: Boolean, required: false }
    });
    const emits = defineEmits(['update:open']);

    const forwarded = useForwardPropsEmits(props, emits);

    const resolvedModal = useDialogPopoutModal(props);
</script>

<template>
    <DialogRoot v-slot="slotProps" data-slot="alert-dialog" v-bind="forwarded" :modal="resolvedModal">
        <AlertDialogStateProvider :open="slotProps.open">
            <slot v-bind="slotProps" />
        </AlertDialogStateProvider>
    </DialogRoot>
</template>
