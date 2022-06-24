<script lang="ts">
  import HeadMenu from "../personalPage/HeadMenu.svelte"
  import FooterWithLinks from "../personalPage/FooterWithLinks.svelte"

  let data: Promise<any> = new Promise<any>(function(p1: (value: (PromiseLike<any> | any)) => void, p2: (reason?: any) => void) {
  })
  let availiableCountry: Promise<any> = new Promise<any>(function(p1: (value: (PromiseLike<any> | any)) => void, p2: (reason?: any) => void) {
  })
  let in_counrty = async (api) => {
    const response = await fetch(api)
    availiableCountry = response.json()
  }
    in_counrty("https://date.nager.at/api/v3/AvailableCountries")



  let chosen_year: string = "2000"
  let country: string = "RU"
  let years: string[] = []
  for (let i: number = 2000; i < 2022; i++) {
    years.push((i as string))
  }

  function takeFromForm(): void {
    country = country.substr(country.length - 3,2)
    console.log(country)
    const api = "https://date.nager.at/api/v3/publicholidays/" + chosen_year + "/" + country
    let in_ = async (api) => {
      const response = await fetch(api)
      data = response.json()
    }
    in_(api)
  }
</script>


<HeadMenu />
<div class="container">
  <div class="form">

    <div class="api-setting-container">
      <select class="api-setting" bind:value={chosen_year}>
        {#each years as year}
          <option value={year}>
            {year}
          </option>
        {/each}
      </select>
      <select class="api-setting" bind:value={country}>
        {#await availiableCountry}
          <option>RU</option>
        {:then objects}
          {#each objects as country_var}
            <option>{country_var.name}({country_var.countryCode})</option>
            {/each}
        {/await}
      </select>
    </div>
    <button class="api-setting" on:click={takeFromForm}>View holiday dates</button>
  </div>
  {#await data}
    <div class="table">
      <div class="table-row">
        <div class="table-header">English Name</div>
        <div class="table-header">Local Name</div>
        <div class="table-header">Date</div>
      </div>
    </div>
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

<FooterWithLinks />

<style>
    .container {
        display: flex;
        flex-direction: column;
        background-position: bottom;
        background-image: url("../IMG/background_gragiend.png");
        background-repeat: no-repeat;
        background-size: 100%;
        height: 800px;
        margin-left: 30px;
        margin-right: 30px;
    }

    .form {
        display: flex;
        flex-direction: column;
        background: #a0a6e3;
        margin-top: 30px;
        border-top-left-radius: 8px;
        border-top-right-radius: 8px;
    }

    .api-setting-container{
        display: flex;
        flex-direction: row;
        width: 30%;
        margin-left: auto;
        margin-right: auto;
        margin-top: 10px;
    }

    .api-setting {
        margin-left: auto;
        margin-right: auto;
    }

    .table {
        border-radius: 8px;
        display: table;
        border: 2px solid #333;
        border-collapse: collapse;
        width: 100%;
        margin-top: 1rem;
        margin-bottom: auto;
        background: white;
    }

    .table-row {
        display: table-row;

    }

    .table-cell,
    .table-header {
        display: table-cell;
        padding: 0.2rem;
        border: 1px solid #333;
    }

    .table-header {
        background: #c5c6ee;
        font-weight: bold;
        text-align: center;
    }

    .table-row:hover {
        display: table-row;
        background: #dbd4f8;
    }
</style>