<script>
    import InputCard from "./main/InputCard.svelte";
    import DetailsCard from "./main/DetailsCard.svelte";
    import SolvedCard from "./main/SolvedCard.svelte";
 //import ProgressGraph from "./lib/main/ProgressGraph.svelte";

  let cipherText =
    "iüöçzçegg üpgsy pekpg şçüücbcşy kgppıi, fbrikty jcdkpg lzm pmı.Siüöbvst lzdmidtc yg röpysunröaıs jkehoicüci.";
  let cipherType = "";
  let cipherKey = "";
  let solvedText = "";
  //let logs = [];
  let loading = false;
  let speed = "medium";
  let isAdvanced = false;
  let customParams = {};


  async function decode(selectedSpeed, advancedMode, params,mode) {
    loading = true;
    //logs = [];

    try {
        const res = await fetch("/api/solve", {  
        //const res = await fetch("http://127.0.0.1:8000/solve", {
        method: "POST",
        headers: {
          "Content-Type": "application/json"
        },
        body: JSON.stringify({ 
          cipher: cipherText,
          speed: selectedSpeed,
          is_advanced: advancedMode,
          custom_params: params,
          mode: mode
         })
      });

      const data = await res.json();

      cipherType = data.type;
      cipherKey = data.key;
      solvedText = data.plain;
      // logs = data.logs || [];
    } catch (err) {
      console.error(err);
    } finally {
      loading = false;
    }
  }

</script>

<main>
  <div class="card-container">
    <InputCard 
  bind:cipherText 
  bind:speed 
  bind:isAdvanced 
  bind:customParams 
  onDecode={decode} 
  {loading} 
/>

    <DetailsCard {cipherType} {cipherKey} />

    <SolvedCard {solvedText} />

  </div>
</main>

<style>
  .card-container {
    display: flex;
    flex-direction: column;
    gap: 20px;
  }

  /* .combined-card {
    padding: 16px;
    background: #fff;
    border-radius: 12px;
    box-shadow: 0 3px 10px rgba(0, 0, 0, 0.1);
  }

  .combined-card hr {
    border: none;
    border-top: 1px solid #ddd;
    margin: 20px 0;
  }  */
</style>