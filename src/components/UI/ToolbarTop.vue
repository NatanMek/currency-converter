<template>
  <header class="font-bold text-4xl justify-center text-white">
    <div>
      <h1>
        <strong>{{ title }}</strong>
      </h1>
    </div>
  </header>
</template>

<script>
export default {
  props: ['title']
}
</script>
