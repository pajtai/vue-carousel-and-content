<template>
    <div class="slider">
        <div class="slide" v-for="(slide, index) in slides" :key="index">
            <div @animationend="doneWithAnimation" :class="animationClass" class="image"  :style="'background-image:url(\'' + slide.img + '\')'">
                &nbsp;
                <div v-if="slide.title" class="title" v-html="slide.title"></div>
                <div v-if="slide.content" @click="down" class="down">
                    <slot name="down">&darr;</slot>
                </div>
            </div>
            <div :class="animationClass" class="content" v-html="slide.content"></div>
        </div>
    </div>
</template>