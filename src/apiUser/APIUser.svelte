<script lang="ts">
  import HeadMenu from "../personalPage/HeadMenu.svelte"
  import FooterWithLinks from "../personalPage/FooterWithLinks.svelte"

  let chosen_year :string = ""
  let country: string = "RU"
  let data : Promise<any>= new Promise<any>(function(p1: (value: (PromiseLike<any> | any)) => void,p2: (reason?: any) => void){})
  let years:string[] = []
  for(let i:number = 2000; i < 2022; i++)
  {
    years.push((i as string))
  }

  function takeFromForm():void
  {
    let in_ = async () => {
      const api = 'https://date.nager.at/api/v3/publicholidays/'+ chosen_year + '/' + country
      const response = await fetch(api)
      data = response.json()
    }
    in_()
  }
</script>


<HeadMenu/>
<div id="form">
  <select id="year" bind:value={chosen_year}>
    {#each years as year}
      <option value={year}>
        {year}
      </option>
    {/each}
  </select>
  <select id="countries-id" bind:value={country}>
    <option>AT</option>
    <option>RU</option>
  </select>
  <button id="get-holidays" on:click={takeFromForm}>View holiday dates</button>
</div>
<div>
  {#await data}
  {:then objects}
    <div class="table">
      <div class="table-row">
        <div class="table-header">English Name</div>
        <div class="table-header">Local Name</div>
        <div class="table-header">Date</div>
      </div>
    {#each objects as object}
      <div class="table-row">
        <div class="table-cell">{object.name}</div>
        <div class="table-cell">{object.localName}</div>
        <div class="table-cell">{object.date}</div>
      </div>
    {/each}
    </div>
  {:catch Error}
    <p>{Error}</p>
  {/await}
</div>
<FooterWithLinks/>

<style>
  .table{
      display: table;
      border: 2px solid #333333;
      border-collapse: collapse;
      width: 100%;
  }
  .table-row{
      display: table-row;

  }
  .table-cell,
  .table-header{
      display: table-cell;
      padding: 0.2rem;
      border: 1px solid #333333;
  }
  .table-header{
      background: #c5c6ee;
      font-weight: bold;
      text-align: center  ;
  }
  .table-row:hover {
      display: table-row;
      background: #dbd4f8;
  }
</style>