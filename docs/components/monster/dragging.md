### 拖拽排序

在工作中，拖拽排序是我们前端常常会遇到的业务需求。与这个需求交手我略显束手无策，哎呀这东西好像好难啊，该怎么做啊啊啊啊:confounded::confounded:。遇事不要慌先打开好助手百度娘娘，发现实现起来也没那么困难嘛。下面咱们由简入深，慢慢加大难度研究。

#### HTML5拖放API

>**将元素设置为可拖放**

让一个元素能被拖放需要设置 `draggable` 属性为`true`(文本、图片和链接的`draggable`默认就是`true`)

    <div draggable="true">能被拖放的元素</div>

>**拖放事件**

拖放涉及到两种元素，一种是被拖拽元素（源对象），一种是放置区元素（目标对象）,按住A元素往B元素拖拽，A元素即为源对象，B元素即为目标对象。不同的对象产生不同的拖放事件。(下面只是简单记录一些事件，如果想深入了解可以先阅读[HTML拖放API](https://developer.mozilla.org/zh-CN/docs/Web/API/HTML_Drag_and_Drop_API))

|触发对象 |事件名称|说明 |
| :---- | :---- | :----|
|源对象| dragstart|源对象开始被拖动时触发|
||drag|源对象被拖动过程中反复触发|
||dragend|源对象拖动结束时触发|
|目标对象|dragenter|源对象开始进入目标对象范围内触发|
||dragover|源对象在目标对象范围内移动时触发|
||dragleave|源对象离开目标对象范围时触发|
||drop|源对象在目标对象范围内被释放时触发|

>需要注意的是：`dragenter`和`dragover`事件的默认行为是拒绝接受任何被拖放的元素。因此，我们要在这两个拖放事件中使用`preventDefault`来阻止浏览器的默认行为；而且目标对象想要变成可释放区域，必须设置`dragover` 和 `drop` 事件处理程序属性。:bulb:注意：当从操作系统向浏览器中拖拽文件时，不会触发 dragstart 和dragend 事件。

>**拖拽排序**

下面我们利用拖放API来实现列表的拖拽排序，代码基于Vue实现。
先不考虑排序动画。代码并不复杂，就全部贴了出来，然后解释一下实现思路：

1. 由于拖动是实时的，所以没有使用`drop`而是使用了`dragenter`触发排序。
2. 在源对象开始被拖拽时记录其索引`dragIndex`，当它进入目标对象时（对应`dragenter`事件），将其插入到目标对象的位置。
3. 其中`dragenter`方法中有一个判断`this.dragIndex !== index`(index为当前目标对象的索引)，这是因为源对象同时也是目标对象，当没有这个判断时，源对象开始被拖拽时就会立刻触发自身的dragenter事件，这是不合理的。

最终效果如下图所示：

![效果图](../../assets/gif/dragenter.awebp)
```
<template>
  <ul class="list">
    <li
      @dragenter="dragenter($event, index)"
      @dragover="dragover($event, index)"
      @dragstart="dragstart(index)"
      draggable
      v-for="(item, index) in list"
      :key="item.label"
      class="list-item"
     >
      {{item.label}}
    </li>
  </ul>
</template>
<script>
export default {
  data() {
    return {
      list: [
        { label: '列表1' },
        { label: '列表2' },
        { label: '列表3' },
        { label: '列表4' },
        { label: '列表5' },
        { label: '列表6' },
      ],
      dragIndex: '',
      enterIndex: '',
    };
  },
  methods: {
    dragstart(index) {
      this.dragIndex = index;
    },
    dragenter(e, index) {
      e.preventDefault();
      // 避免源对象触发自身的dragenter事件
      if (this.dragIndex !== index) {
        const source = this.list[this.dragIndex];
        this.list.splice(this.dragIndex, 1);
        this.list.splice(index, 0, source);
        // 排序变化后目标对象的索引变成源对象的索引
        this.dragIndex = index;
      }
    },
    dragover(e, index) {
      e.preventDefault();
    },
  },
};
</script>
<style lang="scss" scoped>
.list {
  list-style: none;
  .list-item {
    cursor: move;
    width: 300px;
    background: #EA6E59;
    border-radius: 4px;
    color: #FFF;
    margin-bottom: 6px;
    height: 50px;
    line-height: 50px;
    text-align: center;
  }
}
</style>
```

减去HTML和CSS，核心的`dragenter`只有不超过15行的代码，就可实现一个拖拽排序功能。当然这个拖拽效果有点生硬，下面使用Vue自带的`transition-group`组件，给排序添加平滑的过渡效果。

>**排序动画**

如果不熟悉Vue的[transition-group](https://vuejs.org/guide/built-ins/transition-group.html#move-transitions)，请先学习Vue的列表的排序过渡，这里不再赘述。
为了便于和上面的代码进行比较，同样一次性把全部代码贴出，可以看到代码变动并不大，只需把HTML的ul元素改为`transition-group`，在methods中新增shuffle方法，在CSS中新增一个过渡`ransition:transform .3s`;即可实现开头第一张图所展示的拖拽排序效果了。
你可以点击[此链接](https://codesandbox.io/s/gallant-ptolemy-43tdw?file=/src/App.vue:0-1515)进行在线体验。

```
<template>
  <div>
    <transition-group name="drag" class="list" tag="ul">
      <li
        @dragenter="dragenter($event, index)"
        @dragover="dragover($event, index)"
        @dragstart="dragstart(index)"
        draggable
        v-for="(item, index) in list"
        :key="item.label"
        class="list-item"
      >
        {{ item.label }}
      </li>
    </transition-group>
  </div>
</template>
<script>
export default {
  data() {
    return {
      list: [
        { label: "列表1" },
        { label: "列表2" },
        { label: "列表3" },
        { label: "列表4" },
        { label: "列表5" },
        { label: "列表6" },
      ],
      dragIndex: "",
      enterIndex: "",
    };
  },
  methods: {
    dragstart(index) {
      this.dragIndex = index;
    },
    dragenter(e, index) {
      e.preventDefault();
      if (this.dragIndex !== index) {
        const moving = this.list[this.dragIndex];
        this.list.splice(this.dragIndex, 1);
        this.list.splice(index, 0, moving);
        this.dragIndex = index;
      }
    },
    dragover(e, index) {
      e.preventDefault();
    },
  },
};
</script>
<style lang="scss" scoped>
.list {
  list-style: none;
  .drag-move {
    transition: transform 0.3s;
  }
  .list-item {
    cursor: move;
    width: 300px;
    background: #EA6E59;
    border-radius: 4px;
    color: #FFF;
    margin-bottom: 6px;
    height: 50px;
    line-height: 50px;
    text-align: center;
  }
}
</style>

```

#### 参考链接

[基于Vue快速实现列表拖拽排序](https://juejin.cn/post/6909287804510371847)