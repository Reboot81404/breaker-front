<script>
  const ALPHABET = "abcçdefgğhıijklmnoöprsştuüvyz";
  let text = "";
  let result = "";
  let currentKey = "";

  // Rastgele bir anahtar (karıştırılmış alfabe) üretir
  function generateRandomKey() {
    let arr = ALPHABET.split("");
    for (let i = arr.length - 1; i > 0; i--) {
      const j = Math.floor(Math.random() * (i + 1));
      [arr[i], arr[j]] = [arr[j], arr[i]];
    }
    return arr.join("");
  }

  function handleEncode() {
    if (!text) return;

    // Her şifrelemede yeni bir anahtar oluşturur
    const key = generateRandomKey();
    currentKey = key; 
    let output = "";

    for (let char of text) {
      const lowerChar = char.toLocaleLowerCase("tr");
      const idx = ALPHABET.indexOf(lowerChar);

      if (idx !== -1) {
        const targetChar = key[idx];
        // Büyük/küçük harf koruması
        output += (char === lowerChar) 
          ? targetChar 
          : targetChar.toLocaleUpperCase("tr");
      } else {
        // Alfabe dışı karakterleri (boşluk, nokta vb.) olduğu gibi bırak
        output += char;
      }
    }
    result = output;
  }
</script>

<div class="container">
  <div class="card">
    <h2>Substitution Şifreleyici</h2>
    

    <div class="input-group">
      <label for="plain">Metin:</label>
      <textarea id="plain" bind:value={text} placeholder="Şifrelenecek metni girin..."></textarea>
    </div>

    <button on:click={handleEncode} disabled={!text}>
      Şifrele
    </button>

    {#if result}
      <div class="result-area">
        <h3>Şifrelenmiş Sonuç:</h3>
        <textarea readonly value={result}></textarea>
        
        <div class="key-info">
          <strong>Kullanılan Anahtar:</strong>
          <code>{currentKey}</code>
        </div>
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
    font-family: sans-serif;
  }

  .card {
    background: white;
    padding: 2rem;
    border-radius: 12px;
    box-shadow: 0 4px 6px rgba(0,0,0,0.1);
    max-width: 600px;
    width: 100%; 
  }

  h2 { margin-top: 0; color: #333; }
  
  .input-group {
    margin-bottom: 1rem;
    display: flex;
    flex-direction: column;
    text-align: left;
  }

  label { font-weight: bold; margin-bottom: 5px; color: #555; }

  textarea {
    width: 100%;
    padding: 12px;
    border: 1px solid #ccc;
    border-radius: 6px;
    font-family: inherit;
    height: 100px;
    resize: vertical;
    box-sizing: border-box;
  }

  button {
    background: #242424; /* Substitution için farklı bir renk */
    color: white;
    padding: 12px 20px;
    border: none;
    border-radius: 6px;
    cursor: pointer;
    width: 100%;
    font-weight: bold;
  }

  .result-area {
    margin-top: 20px;
    border-top: 2px dashed #eee;
    padding-top: 20px;
  }

  .key-info {
    margin-top: 15px;
    font-size: 0.85rem;
    background: #f9f9f9;
    padding: 10px;
    border-radius: 4px;
    border: 1px solid #eee;
    word-break: break-all;
  }

  code {
    display: block;
    margin-top: 5px;
    color: #e91e63;
    font-family: monospace;
    letter-spacing: 1px;
  }
</style>