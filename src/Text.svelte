<script>
  export let type = ''
</script>

<span class:danger={type=='danger'}>
  <slot/>
</span>

<style>
  .danger {
    color: #e5332b;
  }
</style>
