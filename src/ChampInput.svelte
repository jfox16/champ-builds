<script>
  import AutoComplete from "simple-svelte-autocomplete";

  export let value;
  export let champList;
</script>

<style>
  div {
    max-width: 500px;
  }
</style>

<div>
  <AutoComplete 
    items={champList}
    bind:value={value}
    labelFieldName="name"
    valueFieldName="id"
  />
</div>