

<center>Vue3驱动css变量</center>

[toc]

### vue3驱动css

> vue3打通css

```vue
<style scoped lang="less">
.img_box{
    width: v-bind("`${state.size}px`");
    opacity: v-bind("state.opactiy");
}
</style>
<template>
    <button @click="handleAdd">add</button>
</template>


<script setup lang="ts">
import { reactive } from 'vue'
let state = reactive({
    opactiy: 0,
    size: 100
})

const handleAdd = () => {
    if(state.opactiy == 1){
        state.opactiy = 0
    }
    state.opactiy += 0.2
    state.size += 100
}

</script>
```



