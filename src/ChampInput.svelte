<script>
  import AutoComplete from "simple-svelte-autocomplete";

  export let value;
  export let champList;
</script>

<AutoComplete 
  items={champList}
  bind:value={value}
  labelFieldName="name"
  valueFieldName="id"
/>