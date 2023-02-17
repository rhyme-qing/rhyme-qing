---
title: "Scroll Trigger 的最小实现"
author:
  - Rhyme
date: 2023-02-16
---

# GSAP 早期接触
很早就希望学习 GSAP 了，最早接触 GSAP 的原有是看到了 Filecoin[^1] 非常流畅的交互让我感到惊呼 GSAP 的流畅和性能（在此之前我的项目通过使用 D3 Geo 来进行实现发现一些硬件较差的用户浏览器崩溃，所以对于动画的性能有一些隔阂，但 GSAP 解决了我这个问题）

在之后了解自然语言处理的过程中，浏览了很多学者和研究员所使用的学术主页模板[^2]来作为其个人介绍，这让我感觉 NPO.NETWORK 下的研究员或工程师、开发者们也需要一个相关的自我介绍页面。

在设计上我希望我们的 NPO.NETWORK 会更加的现代化和极具未来感，但是如果像是学术主页模板的设计又让我感觉有些许的正式，这会导致我们的参与者会和其他想加入进来的贡献者们产生隔阂。但是如果是采用更加现代化的如微软研究院[^3]的学者主页则有些专注于内容的介绍，可能会降低可读性和专注。

因此我想到了 Open Collective[^4] 页面所应用的类似 Scroll Trigger[^5] 技术来弥补这个问题，也就是我们需要一个良好的交互和设计来解决这种经历上的隔阂和保证每个人的经历可读性。

# Research at NPO.NETWORK
![首页](https://raw.githubusercontent.com/rhyme-qing/rhyme-qing/main/assets/202302160812154.png "首页")

如图一我们的设计，上面我们分别划分为导航栏、用户页头、子导航、侧栏、内容等五个区域作为首页，合理划分了功能区域和内容，使得可以更加直观的看到内容和分别连接。

同时，为了使得信息在滚动下时也可以展示完整还需设计一个 Scroll Trigger 的过渡后的效果效果，但在后期开发中，可以使用 GSAP 的相关特性和方法来实现具体的过渡动画：

![过渡效果](https://raw.githubusercontent.com/rhyme-qing/rhyme-qing/main/assets/202302160817851.png "过渡效果")

在过渡的效果后，具体的移动是侧栏的变化和用户页头的移动和转换，因此在 GSAP 的设计中，我们需要将过渡效果应用在用户页头（用户卡）区域内，其余的则是使用 Sticky[^6] 进行实现。

# sticky

sticky 部分的实现主要有导航栏、子导航、侧栏、内容等四个部分，也就是说实际上需要用得到 Scroll Trigger 的地方只有用户页头和一个 About Me 部分，其余的都可以通过 Sticky 来进行实现。

在实现 sticky 的部分中，需要为指定的元素设置 `top` 以及 `height` 来保证他们之间的定位而不会被挤掉，共享同一个 sticky 容器：

```css
.sticky-container {
  position: sticky;
  top: 0;
}

.sticky-child-1 {
  height: 100px;
  background-color: #ff9999;
}

.sticky-child-2 {
  height: 200px;
  background-color: #99ff99;
}
```

```html
<div class="sticky-container">
  <div class="sticky-child-1">Sticky Child 1</div>
  <div class="sticky-child-2">Sticky Child 2</div>
</div>
```

# ScrollTrigger 

![最小 DEMO](https://raw.githubusercontent.com/rhyme-qing/rhyme-qing/main/assets/202302172307623.gif "最小 DEMO")

在 Research at NPO.NETWORK 的这个项目中，我们的主要问题和需求就是根据滚轮来触发动画和样式的切换，因此需要使用到 GSAP 中的 `ScrollTrigger` 来实现其滚轮触发的效果，其中主要用得到的方法有：

| Name                | Info                                |
|---------------------|-------------------------------------|
| trigger             | 当 `scroller-start` 到此的时候触发（Class 类） |
| endTrigger          | 当 `scroller-end` 到此的时候触发（Class 类）   |
| onEnter/onEnterBack |  触发和未触发时的 `gasp.to`                 |
|                     |                                     |
那么基于上述所列举的方法，就可以实现上述的需求，并且根据下述的这套 Code Snips[^7] 可以进行延申至完成这个项目的开发需求：

```javascript
<script setup>  
import { gsap } from "gsap";  
import { ScrollTrigger } from "gsap/ScrollTrigger";  
import { onMounted, ref } from "vue";  
  
const container = ref(null);  
const content = ref(null);  
  
onMounted(() =\> {  
  // 获取动画元素  
   const myElement = document.querySelector(".inside");  
  // 注册 ScrollTrigger   
  gsap.registerPlugin(ScrollTrigger);  
  ScrollTrigger.create({  
    // 当用户页头到子导航栏的时候触发  
   trigger: ".fixation-about",  
    // 当到用户页头区域时返回其预设样式。  
   endTrigger: '.external',  
    start: "top 30%",  
    end: "top 10%",  
    // 开启标注
    markers: true,   
   onEnter: () =\> {  
      gsap.to(myElement, {  
        y: 900,  
        rotation: 90,  
        duration: 1  
   });  
    },  
    onEnterBack: () =\> {  
      gsap.to(myElement, { y: 0, rotation: 0 });  
    },  
    onLeave: () =\> {  
      myElement.classList.add('active');  
    },  
    onLeaveBack: () =\> {  
      myElement.classList.remove('active');  
    }  
  });  
});  
</script>
```

在上述的 Code Snips 中的 `.inside` 是我们用户页头的类，因此我们设置了一个触发的界限，只有当用户页头到达子导航栏的时候才会触发，以及当 `scroller-end` 临近用户页头区域时返回其预设样式。基于上述方法和回调，可以分别实现其原型的 Scroll Trigger 效果。

# Link
[^1]: [A decentralized storage network for humanity's most important information | Filecoin](https://filecoin.io/)
[^2]: [academicpages/academicpages.github.io: Github Pages template for academic personal websites, forked from mmistakes/minimal-mistakes](https://github.com/academicpages/academicpages.github.io)
[^3]: [Xu Tan at Microsoft Research](https://www.microsoft.com/en-us/research/people/xuta/)
[^4]: [Chrome Frameworks Fund - Open Collective](https://opencollective.com/chrome/projects/2021-frameworks-fund)
[^5]: [ScrollTrigger - Plugins - GreenSock](https://greensock.com/scrolltrigger/)
[^6]: [position - CSS: Cascading Style Sheets | MDN (mozilla.org)](https://developer.mozilla.org/en-US/docs/Web/CSS/position)
[^7]: 基于 Scroll Trigger 的最小实现