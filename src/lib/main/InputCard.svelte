
<script>
  export let cipherText = "";
  export let speed = "medium";
  export let onDecode;
  export let loading = false;
  export let isAdvanced = false;
  export let ngramWeights = {
    bigram: 0,
    trigram: 0,
    quadgram: 1
  };

  const CONFIG_DEFAULTS = {
    fast: { max_time: 1.5, stagnation_limit: 2, local_time: 0.4, k_candidates: 1, time_multiplier: 0.5, max_k: 10 },
    medium: { max_time: 5.0, stagnation_limit: 4, local_time: 0.6, k_candidates: 2, time_multiplier: 1.0, max_k: 20 },
    deep: { max_time: 15.0, stagnation_limit: 10, local_time: 1.0, k_candidates: 4, time_multiplier: 3.0, max_k: 30 }
  };

  export let customParams = { ...CONFIG_DEFAULTS.medium };

  const levels = ["fast", "medium", "deep"];
  let sliderValue = levels.indexOf(speed);

  const ngramModes = [
    { id: "bigram", label: "Bigram" },
    { id: "trigram", label: "Trigram" },
    { id: "quadgram", label: "Quadgram" }
  ];

  const modes = [
    { id: 'affine', label: 'Affine' },
    { id: 'playfair', label: 'Playfair' },
    { id: 'sub', label: 'Substitution' },
    { id: 'vig', label: 'Vigenere' },
    { id: 'both', label: 'Substitution & Vigenere' }
  ];
  let selectedMode = 'both'; 
  $: {
    speed = levels[sliderValue];
    customParams = { ...CONFIG_DEFAULTS[speed] };
  }
</script>

<div class="mode-selector-container">
  <div class="tabs">
    {#each modes as mode, i}
      <input 
        type="radio" 
        id="mode-{mode.id}" 
        name="modes" 
        value={mode.id} 
        bind:group={selectedMode} 
      />
      <label class="tab" for="mode-{mode.id}">
        {mode.label}
      </label>
    {/each}
    <span 
      class="glider" 
      style="
        width: calc(100% / {modes.length} - 10px);
        left: calc({modes.findIndex(m => m.id === selectedMode)} * (100% / {modes.length}) + 5px);
      "
    ></span>
  </div>
</div>

<div class="card main-input-card">
  <div class="top-right-toggle">
    <label class="checkbox-wrapper">
      <input type="checkbox" bind:checked={isAdvanced} />
      <div class="checkmark">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor"><path d="M20 6L9 17L4 12" stroke-width="3" stroke-linecap="round" stroke-linejoin="round"></path></svg>
      </div>
      <span class="label">Expert Mode</span>
    </label>
  </div>

  <label for="cipher-input"><strong>Enter the cipher</strong></label>
  <textarea id="cipher-input" bind:value={cipherText} placeholder="Buraya yapıştırın..."></textarea>

  <div class="speed-selector">
    <input type="range" min="0" max="2" step="1" bind:value={sliderValue} class="large-slider" />
    <div class="labels">
      <span class:active={speed === 'fast'}>Hızlı</span>
      <span class:active={speed === 'medium'}>Orta</span>
      <span class:active={speed === 'deep'}>Derin</span>
    </div>
  </div>

  <button
    class="btn-decode"
    class:loading
    on:click={() => onDecode(speed, isAdvanced, customParams, selectedMode, ngramWeights)}
    disabled={!cipherText || loading}
  >
    {loading ? 'Solving...' : 'Break cipher'}
  </button>
</div>

{#if isAdvanced}
  <div class="card expert-card">
    <h3 class="expert-title">Expert Mode</h3>
    
    <div class="expert-section full-width" style="margin-bottom: 15px;">
      <span class="section-tag">N gram Weights</span>
      <div class="ngram-grid">
        {#each Object.entries(ngramWeights) as [key, value]}
          <div class="ngram-item">
            <label class="checkbox-line">
              <input
                type="checkbox"
                checked={value > 0}
                on:change={(e) => {
                  if (e.target.checked) {
                    ngramWeights[key] = 1;
                  } else {
                    ngramWeights[key] = 0;
                  }
                  ngramWeights = { ...ngramWeights };
                }}
              />
              <span>{key}</span>
            </label>
            <input
              type="number"
              min="0"
              step="0.1"
              bind:value={ngramWeights[key]}
              disabled={ngramWeights[key] === 0}
              class="weight-input"
            />
          </div>
        {/each}
      </div>
    </div>

    <div class="expert-grid">
      {#if selectedMode === 'sub' || selectedMode === 'both'}
        <div class="expert-section">
          <span class="section-tag">Substitution</span>
          <div class="input-row">
            <div class="field">
              <label for="max-time">Max Time</label>
              <input id="max-time" type="number" step="0.1" bind:value={customParams.max_time} />
            </div>
            <div class="field">
              <label for="stop-time">Stagnation Limit</label>
              <input id="stop-time" type="number" step="1" bind:value={customParams.stagnation_limit} />
            </div>
            <div class="field">
              <label for="sensivity">Sensitivity (local_time)</label>
              <input id="sensivity" type="number" step="0.1" bind:value={customParams.local_time} />
            </div>
          </div>
        </div>
      {/if}

      {#if selectedMode === 'vig' || selectedMode === 'both'}
        <div class="expert-section">
          <span class="section-tag">Vigenere</span>
          <div class="input-row">
            <div class="field">
              <label for="k_candidate">K Candidates</label>
              <input id="k_candidate" type="number" step="1" bind:value={customParams.k_candidates} />
            </div>
            <div class="field">
              <label for="time_multp">Time Multiplier</label>
              <input id="time_multp" type="number" step="0.1" bind:value={customParams.time_multiplier} />
            </div>
            <div class="field">
              <label for="max_k_lenght">Max K length</label>
              <input id="max_k_lenght" type="number" step="1" bind:value={customParams.max_k} />
            </div>
          </div>
        </div>
      {/if}
    </div>
  </div>
{/if}

<style>
  .card {
    position: relative; 
    padding-top: 40px; 
    margin-bottom: 20px; 
  }

  .expert-card {
    padding: 20px;
  }

  .expert-title {
    margin-top: 0;
    margin-bottom: 15px;
    font-size: 1.2rem;
    text-align: center;
  }

  .full-width {
    flex: none;
    width: 100%;
    box-sizing: border-box;
  }

  .ngram-grid {
    display: flex;
    gap: 15px;
    margin-top: 10px;
  }

  .ngram-item {
    display: flex;
    align-items: center;
    gap: 10px;
    background: rgba(255, 255, 255, 0.5);
    padding: 5px 10px;
    border-radius: 6px;
    flex: 1;
  }

  .checkbox-line {
    display: flex;
    align-items: center;
    gap: 5px;
    cursor: pointer;
    text-transform: capitalize;
    font-size: 0.9rem;
  }

  .weight-input {
    width: 60px;
    padding: 2px 5px;
    border: 1px solid #ccc;
    border-radius: 4px;
  }

  .top-right-toggle {
    position: absolute;
    top: 10px;
    right: 10px;
    z-index: 10;
  }

  .checkbox-wrapper {
    --checkbox-size: 20px; 
    --checkbox-color: #1f1f1f;
    --checkbox-shadow: rgba(0, 255, 136, 0.3);
    --checkbox-border: rgba(0, 255, 136, 0.5);
    display: flex;
    align-items: center;
    position: relative;
    cursor: pointer;
  }

  .checkbox-wrapper input {
    position: absolute;
    opacity: 0;
    cursor: pointer;
    height: 0;
    width: 0;
  }

  .checkbox-wrapper .checkmark {
    position: relative;
    width: var(--checkbox-size);
    height: var(--checkbox-size);
    border: 2px solid var(--checkbox-border);
    border-radius: 6px;
    transition: all 0.4s cubic-bezier(0.68, -0.55, 0.265, 1.55);
    display: flex;
    justify-content: center;
    align-items: center;
    background: rgba(0, 0, 0, 0.2);
    box-shadow: 0 0 10px var(--checkbox-shadow);
    overflow: hidden;
  }

  .checkbox-wrapper .checkmark::before {
    content: "";
    position: absolute;
    width: 100%;
    height: 100%;
    background: linear-gradient(45deg, var(--checkbox-color), #00ffcc);
    opacity: 0;
    transition: all 0.4s cubic-bezier(0.68, -0.55, 0.265, 1.55);
    transform: scale(0) rotate(-45deg);
  }

  .checkbox-wrapper input:checked ~ .checkmark::before {
    opacity: 1;
    transform: scale(1) rotate(0);
  }

  .checkbox-wrapper .checkmark svg {
    width: 0;
    height: 0;
    color: #1a1a1a;
    z-index: 1;
    transition: all 0.4s cubic-bezier(0.68, -0.55, 0.265, 1.55);
  }

  .checkbox-wrapper input:checked ~ .checkmark svg {
    width: 14px;
    height: 14px;
    transform: rotate(360deg);
  }

  .checkbox-wrapper:hover .checkmark {
    border-color: var(--checkbox-color);
    transform: scale(1.1);
    box-shadow: 0 0 15px var(--checkbox-shadow);
  }

  .checkbox-wrapper .label {
    margin-left: 10px;
    font-family: "Segoe UI", sans-serif;
    color: var(--checkbox-color);
    font-size: 14px;
    text-shadow: 0 0 8px var(--checkbox-shadow);
    opacity: 0.8;
    transition: all 0.3s;
  }

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
  }

  .labels {
    display: flex;
    justify-content: space-between;
    width: 50%;
    margin-top: 8px;
    font-size: 0.85rem;
    color: #666;
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

  .expert-grid {
    display: flex;
    gap: 20px; 
    justify-content: space-between;
    align-items: flex-start;
  }

  .expert-section {
    flex: 1; 
    background: rgba(0, 0, 0, 0.05); 
    padding: 15px;
    border-radius: 10px;
    border: 1px solid rgba(0, 0, 0, 0.1);
  }

  .input-row {
    display: flex;
    flex-direction: column; 
    gap: 10px;
  }

  .field {
    display: flex;
    flex-direction: column;
    gap: 5px;
  }

  .field label {
    font-size: 0.8rem;
    font-weight: bold;
  }

  .section-tag {
    font-weight: bold;
    font-size: 0.85rem;
    color: #333;
    display: block;
    margin-bottom: 8px;
  }

  @media (max-width: 600px) {
    .expert-grid {
      flex-direction: column;
    }
    .ngram-grid {
      flex-direction: column;
    }
  }

  .mode-selector-container {
    display: flex;
    justify-content: center;
    margin: 20px 0;
  }

  .tabs {
    display: flex;
    position: relative;
    background-color: #fff;
    box-shadow: 0 0 1px 0 rgba(0,0,0, 0.1), 0 4px 12px 0 rgba(0,0,0, 0.05);
    padding: 0.5rem;
    border-radius: 99px;
    width: 100%; 
    max-width: 500px;
  }

  .tabs * {
    z-index: 2;
  }

  input[type="radio"] {
    display: none;
  }

  .tab {
    display: flex;
    align-items: center;
    justify-content: center;
    flex: 1; 
    height: 40px;
    font-size: .85rem;
    color: #666;
    font-weight: 500;
    border-radius: 99px;
    cursor: pointer;
    transition: color 0.15s ease-in;
    text-align: center;
  }

  input[type="radio"]:checked + .tab {
    color: #1a1a1a;
  }

  .glider {
    position: absolute;
    height: 40px;
    background-color: #e6eef9;
    z-index: 1;
    border-radius: 99px;
    transition: all 0.25s ease-out;
  }

  .btn-decode.loading {
    color: #e8e8e8; 
    cursor: not-allowed;
  }

  .btn-decode.loading::before {
    width: 100%;
    background-color: #212121; 
  }

  .btn-decode.loading:hover::before {
    background-color: #212121;
  }

  @media (max-width: 600px) {
    .tab {
      font-size: 0.7rem;
      height: 35px;
    }
    .glider {
      height: 35px;
    }
  }

  .btn-decode:hover { color: #e8e8e8; }
  .btn-decode:hover::before { width: 100%; }
</style>
