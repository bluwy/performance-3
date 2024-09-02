---
# You can also start simply with 'default'
theme: default
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: /intro.png
# some information about your slides (markdown enabled)
title: Performance! Performance! Performance!
info:
# apply unocss classes to the current slide
class: text-left
# https://sli.dev/features/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations.html#slide-transitions
transition: fade
# enable MDC Syntax: https://sli.dev/features/mdc
mdc: true
---

<h1 class="font-semibold">
Performance! <br/>
Performance! <br/>
Performance!
</h1>

<p class="text-xl opacity-100!">Performance for everyone</p>

<p class="fixed left-8 bottom-2">Bjorn Lu - ViteConf 2024</p>

<img src="/ts-logo.svg" width="35" class="opacity-30 fixed -rotate-2 right-73 top-59" />
<img src="/js-logo.png" width="35" class="opacity-30 fixed -rotate-3 right-88 top-61 rounded" />

---
class: text-center flex flex-col justify-center
---

<h2 class="opacity-50 text-xl!">Topics</h2>

Kinds of performance

<Arrow x1="490" y1="185" x2="490" y2="225" />
<br>

Identify them

<Arrow x1="490" y1="270" x2="490" y2="310" />
<br>

Measure them

<Arrow x1="490" y1="350" x2="490" y2="389" />
<br>

Fix them

<style>
  p { font-size: 1.5rem}
  </style>

---
layout: cover
background: /char_intro.jpg
class: text-center
---

<h1 class="font-semibold">
Performance 
Characteristics
</h1>

---
layout: full
---

# Characteristics

<div class="text-2xl opacity-80">

<ul>
<li v-click="1"> Efficient &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-> Do things directly and minimally</li>
<v-click at="4">
    <li class="text-lg mt-2 mb-6 opacity-70 ml-14!">Example: Instead of <code>.map().filter().reduce()</code>, use a for loop and an external variable</li>
</v-click>

<li v-click="2"> Less &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-> Use less code to do the same thing </li>
<v-click at="5">
    <li class="text-lg mt-2 mb-6 opacity-70 ml-14!">Example: If you're only using a subset of library features</li>
</v-click>

<li v-click="3"> Resourceful &nbsp;-> Make full use of what's available </li>
<v-click at="6">
    <li class="text-lg mt-2 mb-6 opacity-70 ml-14!">Example: Use worker threads to run heavy tasks in parallel</li>
</v-click>

</ul>

</div>

<img v-click="7" src="/cpu-0.jpg" width="300" class="fixed top-4 right-8" />

---
layout: cover
background: /search_intro.jpg
class: text-center
---

<h1 class="font-semibold">
Identify 
Opportunities
</h1>

---
layout: two-cols-header
---

# Opportunities

::left::
<v-click>

### Vague

- The UI feels sluggish
- It takes very long to send a form
- `npm install` is slow

</v-click>
::right::

<v-click>

### Specific

- Every input starts an unnecessary re-render
- The server takes a long time to process the data
- A large dependency is imported

</v-click>

<Arrow v-click x1="350" y1="350" x2="470" y2="350" width="5" />

<style global>
  .col-header {
    height: 100px !important;
  }
</style>

---
layout: full
---

# Opportunities

### Tools

- Chrome/Firefox DevTools
- `node --inspect-brk`
- `chrome://inspect`
- https://perf.link
- https://packagephobia.com
- https://npmgraph.js.org
- https://speedscope.app
- `rollup-plugin-visualizer`
- `eslint-plugin-depend`
- `npx renoma` <span class="opacity-50">(my custom tool)</span>

<v-clicks>
<img class="fixed top-5 right-60" src="/chrome_devtools_memory.png" width="400" />
<img class="fixed top-15 right-10" src="/chrome_devtools_performance.png" width="400" />
<img class="fixed top-30 right-50" src="/firefox_devtools.png" width="400" />
<img class="fixed top-40 right-8" src="/perflink.png" width="400" />
<img class="fixed top-70 right-65" src="/packagephobia.png" width="400" />
<img class="fixed top-60 right-45" src="/npmgraph.png" width="400" />
</v-clicks>

---
layout: cover
class: text-center
background: /think_intro.jpg
---

<h1 class="font-semibold">
Figure it out
</h1>

<div v-click class="text-xl opacity-100">
with examples
</div>

---
layout: full
---

1. Run the project with `node --inspect-brk`
2. Open `chrome://inspect`
3. Open the inspector and click "record" <br> in performance tab
4. Download the `.cpuprofile` and view on <br> https://speedscope.app

<img v-click src="/flow-inspect.png" width="500" class="fixed top-0 right-0" />
<img v-click src="/flow-chrome.png" width="400" class="fixed top-20 right-0" />
<img v-click src="/flow-perf.png" width="400" class="fixed top-45 right-60" />
<img v-click src="/flow-profile-before.png" width="500" class="fixed top-55 left-10" />

---
layout: two-cols
---

# Resolve package.json efficiently

https://github.com/vitejs/vite/pull/12441

1. We're using a package to resolve a library's `package.json` (includes other unnecessary code to resolve other files).
2. Implement simpler logic to do only that directly.
3. Up to 4x faster performance!

<br>
<p v-click class="text-2xl! border rounded p-4">
  Efficient and less!
</p>

<img src="/vite-resolve-pr.png" width="500" class="fixed top-0 right-0" />

---
layout: image
image: /flow-profile-before.png
---

## Before

---
layout: image
image: /flow-profile-after.png
---

## After

---
layout: two-cols
---

# Use worker threads

https://github.com/vitejs/vite/pull/13584

1. When processing large amounts of CSS, it slows down the server and main thread.
2. Offload CSS processing to a worker thread.
3. Up to 1.4x faster performance!

<br>
<p v-click class="text-2xl! border rounded p-4">
  Resourceful!
</p>

<img src="/vite-css-thread-pr.png" width="500" class="fixed top-0 right-0" />

---
layout: two-cols
---

# Reduce memory usage

https://github.com/rollup/rollup/pull/4762

1. The AST of all modules were cached in the entire lifetime of the build.
2. Reduce the number of cache and dynamically compute where needed.
3. Less out-of-memory errors!

<br>
<p v-click class="text-2xl! border rounded p-4">
  Resourceful!
</p>

<img src="/rollup-pr.png" width="500" class="fixed top-0 right-0" />

---
layout: two-cols
---

# Prefer strings to ReadableStream

https://github.com/withastro/astro/pull/10796

1. Astro prerenders pages to the filesystem using a ReadableStream, which involves a lot of string -> buffer manipulation.
2. Switch to prerendering with strings directly.
3. Up to 26% faster performance!

<br>
<p v-click class="text-2xl! border rounded p-4">
  Efficient!
</p>

<img src="/astro-ssg-pr.png" width="500" class="fixed top-0 right-0" />

---
layout: cover
background: /discuss_intro.jpg
class: text-center
---

<h1 class="font-semibold">
Considerations, <br/>
Ecosystem, & <br/>
Community
</h1>

---
layout: full
---

### Considerations

- Every improvement compounds no matter how small
- Compare the tradeoffs, e.g. maintenance, readability, stability

<br>
<br>

<v-click>

### Ecosystem & Community

- Share your findings with others
  - https://marvinh.dev/blog/speeding-up-javascript-ecosystem/
  - https://sun0day.github.io/blog/vite/why-vite4_3-is-faster.html
- Join a community of like-minded folks!

</v-click>

<v-click>

<img class="fixed right-10 bottom-35" src="/logo.svg" width="280" />

<a class="fixed right-90 bottom-40 font-3xl" href="https://chat.e18e.dev">
https://chat.e18e.dev
</a>

<Arrow x1="370" y1="460" x2="420" y2="420"></Arrow>
<Arrow x1="490" y1="480" x2="490" y2="420"></Arrow>
<Arrow x1="600" y1="470" x2="580" y2="420"></Arrow>

</v-click>

---
layout: section
---

# Thank You

Enjoy the rest of ViteConf!

<img src="/viteconf.png" width="200" class="mx-auto" />

<p class="fixed left-8 bottom-2 text-left">

Slides: https://performance-3.bjornlu.com

Repo: https://github.com/bluwy/performance-3

</p>
