# Vue3-Swipe-Sheet

## Description

A mouse and touch 2-stage swipe sheet for vue3 using [VueUse](https://vueuse.org/).

## Installation

```
npm i vue3-swipe-sheet
```

## Demo

## Usage

These are the imports you need, check out the demo for all the props and emits.

```
<script setup lang="ts">
  import {SwipeSheet} from "vue3-swipe-sheet";
  import "vue3-swipe-sheet/dist/style.css";
</script>

<template>
    <SwipeSheet>
        <template v-slot:header>
            <h1>
                this is the header
            </h1>
        </template>
        <template v-slot:content>
            <p>Lorem ipsum, dolor sit amet consectetur adipisicing elit. Eligendi iure debitis praesentium aliquid, hic
                minima neque distinctio consequuntur! Culpa, voluptatum id eaque delectus numquam itaque quis omnis a
                atque
                illum?Lorem ipsum, dolor sit amet consectetur adipisicing elit. Eligendi iure debitis praesentium aliquid,
                hic
                minima neque distinctio consequuntur! Culpa, voluptatum id eaque </p>
            </div>
        </template>
    </SwipeSheet>
</template>
```
