<template>
    <div :id="`item-${slug}`" class="hover:translate-x-1 py-0.5 text-green-500 transform">
        <a :href="`#${slug}`" class="hover:underline">{{ command.name }}</a>
    </div>
</template>

<script>
export default {
    props: ['command'],
    computed: {
    slug() {
      return this.command.name.replace(':', '')
    },
  },

}
</script>

<style>

</style>