---
title: Nuxt.js examples
date: 2018-07-13 00:18:24
tags:
- Web
- 前端
- Nuxt
- Vue
---

这里都是Nuxt.js自带的例子。
例子用途 | 本体

# async-component-injection
异步组件注入 | NUXT BLOG

```vue
// /pages/_slug.vue

<template>
  <div class="post">
    <component :is="component"/>
  </div>
</template>

<script>
// 果然是注入。。。
const getPost = (slug) => ({
  component: import(`@/posts/${slug}`),
  error: require('@/posts/404')
})

export default {
  beforeCreate() {
    this.component = () => getPost(this.$route.params.slug)
  }
}
</script>
```

_slug.vue通过注入加载/posts里面的博客内容（vue组件只写模板）

# async-data
异步数据

```vue
// /pages/_id.vue
<script>
import axios from 'axios'

export default {
  async asyncData({ params }) {
    // We can use async/await ES6 feature
    let { data } = await axios.get(`https://jsonplaceholder.typicode.com/posts/${params.id}`)
    return { post: data }
  },
  head() {
    return {
      title: this.post.title
    }
  }
}
</script>
```

asyncData返回一个JSON？
哇哦，竟然可以import。