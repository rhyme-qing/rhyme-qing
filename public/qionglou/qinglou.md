---
title: "Qionglou"
author:
  - Rhyme
date: 2023-02-16
tag: 
 - overall-layout
---

# fine-per

```vue
// 自定义单位为 10%
<fine-per wide="10" unit="%" on>  
    <div>hello,world 1</div>  
</fine-per>  
  
<fine-per wide="5">  
  <div>hello,world 2</div>  
</fine-per>  
  
<fine-per wide="10">  
  <div>hello,world 3</div>  
</fine-per>
```

| Name     | Props | Default | type   | Info          |
|----------|-------|---------|--------|---------------|
| fine-per | wide  | 10      | Number | 宽度            |
|          | unit  | 0%      | String | 自定义单位（%.px……) |
|          | on    |         |        |               |

```javascript
<script setup>  
import fineHeader from "@qionglou/components/src/fineHeader.vue";  
  
const customBreakpoints = {  
  320: 1,  
  482: 2,  
  648: 3,  
  768: 4,  
  1024: 5,  
  1280: 6  
};  
defineExpose({ customBreakpoints });  
</script>  
  
<template>  
  <fine-header  
   gap="20"  
   column="3"  
   :breakpoint="customBreakpoints"  
   >  
    <div>Column 1</div>  
    <div>Column 2</div>  
    <div>Column 3</div>  
    <div>Column 4</div>  
    <div>Column 5</div>  
    <div>Column 6</div>  
  </fine-header>  
</template>  
  
<style>  
  
</style>
```