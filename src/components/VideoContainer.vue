<script setup lang="ts">
import { getVideoPaths } from '@/assets/utils/api'
import type { Ref } from 'vue'
import { inject, ref } from 'vue'

const changeVisibleParent = inject('changeVisible') as Function
const flag = inject('flag') as boolean
const flag2 = inject('flag2') as boolean
var layerVisible: Ref<boolean[]> = ref([true, false, false])
function changeVisible(layer: number) {
    layerVisible.value[layer] = !layerVisible.value[layer]
}

const arona_ended = () => {
    changeVisible(1)
    changeVisible(0)
}

const wait_ended = () => {
    changeVisible(2)
    changeVisible(1)
}

const sign_ended = () => {
    changeVisible(2)
    changeVisibleParent(1)
    changeVisibleParent(2)
}

const videoPaths = getVideoPaths()
</script>

<template>
    <div class="video">
        <video muted autoplay v-show="layerVisible[0]" @ended="arona_ended()" @click="arona_ended()">
            <source
                :src="flag === flag2 ? videoPaths.arona_normal : videoPaths.arona_special"
                type="video/mp4"
            />
        </video>
        <video muted autoplay v-show="layerVisible[1]" @click="wait_ended()" loop="true">
            <source
                :src="flag === flag2 ? videoPaths.wait_normal : videoPaths.wait_special"
                type="video/mp4"
            />
        </video>
        <video muted autoplay v-show="layerVisible[2]" @ended="sign_ended()">
            <source
                :src="
                    (flag2 && videoPaths.sign_special2) ||
                    (flag && videoPaths.sign_special) ||
                    videoPaths.sign_normal
                "
                type="video/mp4"
            />
        </video>
    </div>
</template>

<style scoped lang="scss">
.video {
    @include full-screen;
    overflow: hidden;

    video {
        object-fit: fill;
        @include full-screen;
    }
}
</style>
