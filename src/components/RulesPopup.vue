<template>
    <div :class="`overlay position-fixed text-center ${openPopup ? 'open' : ''}`"></div>
    <div :class="`rules-popup p-4 rounded-3 position-fixed ${openPopup ? 'open' : ''}`">
        <div class="d-flex align-items-center justify-content-between mb-4">
            <h2 class="text-uppercase fw-bold">Rules</h2>
            <img class="close" @click="$emit('close-popup')" src="/images/icon-close.svg" alt="">
        </div>
        <img class="rules-image" src="/images/image-rules.svg" alt="">
    </div>
</template>
<script setup>
defineEmits(['close-popup'])
defineProps(['openPopup'])
</script>
<style lang="scss" scoped>
.overlay {
    width: 100%;
    height: 100%;
    left: 0;
    top: 0;
    background-color: #0000008f;
    transform: translateY(100%);
    opacity: 0;
    transition: 0.2s;

    &.open {
        opacity: 1;
        transform: translateY(0);
    }
}

.rules-popup {
    left: 50%;
    top: -50%;
    transform: translate(-50%, -50%);
    background-color: white;
    opacity: 0;
    transition: 0.2s;

    &.open {
        top: 50%;
        opacity: 1;
    }

    h2 {
        color: var(--Dark-Text);
    }

    &>div {
        img {
            cursor: pointer;
        }
    }

}

@media (max-width: 767px) {
    .overlay {
        display: none;
    }

    .rules-popup {
        border-radius: initial !important;
        width: 100% !important;
        height: 100% !important;
        &>div {
            justify-content: center !important;
        }
        .rules-image {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%,-50%);
        }
        .close {
            position: absolute;
            bottom: 24px;
            left: 50%;
            transform: translateX(-50%);
        }
    }
}
</style>