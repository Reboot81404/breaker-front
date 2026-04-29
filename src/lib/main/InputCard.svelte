 <script>
export let cipherText = "";
export let speed = "medium";
export let onDecode;
export let loading = false;

const levels = ["fast", "medium", "deep"];
let sliderValue = levels.indexOf(speed);

$: speed = levels[sliderValue];
</script>
<div class="card">
<label for="cipher-input"><strong>Şifreli metni girin</strong></label>
<textarea
id="cipher-input"
bind:value={cipherText}
></textarea>

<div class="speed-selector">
  <input 
    type="range" 
    min="0" 
    max="2" 
    step="1" 
    bind:value={sliderValue}
    class="large-slider"
  />
  <div class="labels">
    <span>Hızlı</span>
    <span>Orta</span>
    <span>Derin</span>
  </div>
</div>

<button
class="btn-decode"
class:loading={loading}
on:click={() => onDecode(speed)}
disabled={!cipherText || loading}
>
metni kır
</button>
</div>


<style>
  .speed-selector {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    margin: 25px 0;
    width: 100%;
  }

  .large-slider {
    width: 50%; 
    height: 12px; 
    cursor: pointer;
    accent-color: #1E1E2E; 
    margin: 0;
  }

  .large-slider::-webkit-slider-thumb {
    width: 24px;
    height: 24px;
  }

  .labels {
    display: flex;
    justify-content: space-between;
    width: 50%;
    margin-top: 8px;
    font-size: 0.85rem;
    color: #666;
  }

button {
  font-family: inherit;
  cursor: pointer;
}


.btn-decode {
  padding: 15px 25px;
  border: unset;
  border-radius: 15px;
  color: #212121;
  z-index: 1;
  background: #e8e8e8;
  position: relative;
  font-weight: 1000;
  font-size: 17px;
  box-shadow: 4px 8px 19px -3px rgba(0,0,0,0.27);
  transition: all 250ms;
  overflow: hidden;
  width: 100%; 
}

.btn-decode::before {
  content: "";
  position: absolute;
  top: 0; left: 0;
  height: 100%; width: 0;
  border-radius: 15px;
  background-color: #212121;
  z-index: -1;
  transition: all 250ms;
}

.btn-decode:hover {
  color: #e8e8e8;
}

.btn-decode:hover::before {
  width: 100%;
}
.btn-decode.loading::before {
  width: 100% !important;
  opacity: 0.5;
}

.btn-decode:disabled {
  opacity: 0.6;
  cursor: not-allowed;
  filter: grayscale(1);
}
</style>