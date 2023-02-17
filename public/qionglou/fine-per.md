---
title: "Fine-per at Qionglou"
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
