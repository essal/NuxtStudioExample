<template>
    <article class="w-full relative p-10" :style="{'min-height':height}">
        <div class="inner relative z-1 mx-auto" :class="{container}">
            <ContentSlot :use="$slots.default" />
        </div>
        <div v-if="image" class="background absolute w-full h-full top-0 left-0 -z-10">
            <Parallax v-if="parallax" :src="image" />
            <NuxtPicture v-else :src="image" :imgAttrs="{class:'object-cover w-full h-full'}" cover class="w-full h-full"/>
        </div>
    </article>
</template>
<script setup>
const props = defineProps({
    image:String,
    height:String,
    container:Boolean,
    parallax:Boolean,
});

</script>
<style lang="scss" scoped>

</style>