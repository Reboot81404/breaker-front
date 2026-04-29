<script>
  const ALPHABET = "abcçdefgğhıijklmnoöprsştuüvyz";
  let text = "";
  let key = "";
  let result = "";

  function handleEncode() {
    if (!text || !key) return;

    let output = "";
    let keyIdx = 0;

    const cleanKey = key.toLowerCase().trim();

    for (let char of text.toLowerCase()) {
      if (ALPHABET.includes(char)) {
        let pIdx = ALPHABET.indexOf(char);
        let kChar = cleanKey[keyIdx % cleanKey.length];
        let kIdx = ALPHABET.indexOf(kChar);
        
        
        let newIdx = (pIdx + kIdx) % ALPHABET.length;
        output += ALPHABET[newIdx];
        keyIdx++;
      } else {
        
        output += char;
      }
    }
    result = output;
  }
</script>
<div class="container">
<div class="card">
  <h2>Vigenere Şifreleyici</h2>
  
  <div class="input-group">
    <label for="key">Anahtar Kelime:</label>
    <input id="key" type="text" bind:value={key} />
  </div>

  <div class="input-group">
    <label for="plain">Metin:</label>
    <textarea id="plain" bind:value={text} placeholder="Metni girin.."></textarea>
  </div>

  <button on:click={handleEncode} disabled={!text || !key}>
    Şifrele
  </button>

  {#if result}
    <div class="result-area">
      <h3>Şifrelenmiş Sonuç:</h3>
      <textarea readonly value={result}></textarea>
    </div>
  {/if}
</div>
</div>

<style>
.container {
    display: flex;
    justify-content: center; 
    align-items: center;     
    min-height: calc(100vh - 100px); 
    width: 100%;
    padding: 20px;
    box-sizing: border-box;
  }

  .card {
    background: white;
    padding: 2rem;
    border-radius: 12px;
    box-shadow: 0 4px 6px rgba(0,0,0,0.1);
    max-width: 600px;
    width: 100%; 
    margin: 0;   
  }
  .input-group {
    margin-bottom: 1rem;
    display: flex;
    flex-direction: column;
    text-align: left;
  }
  input, textarea {
    width: 100%;
    padding: 10px;
    margin-top: 5px;
    border: 1px solid #ccc;
    border-radius: 4px;
    font-family: inherit;
  }
  textarea {
    height: 100px;
    resize: vertical;
  }
  button {
    background: #4caf50;
    color: white;
    padding: 10px 20px;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    width: 100%;
    font-weight: bold;
  }
  button:disabled {
    background: #ccc;
  }
  .result-area {
    margin-top: 20px;
    border-top: 2px dashed #eee;
    padding-top: 20px;
  }
</style>