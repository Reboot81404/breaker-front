<script>
  const ALPHABET = "abcçdefgğhıijklmnoöprsştuüvyz"; // m = 29

  let text = "";
  let a = 5;
  let b = 8;
  let result = "";

  function handleEncode() {
    if (!text) return;

    // inputlardan gelen değerleri sayıya zorla
    const numA = Number(a);
    const numB = Number(b);
    const m = ALPHABET.length;
    let output = "";

    // 29 harf için aralarında asallık kontrolü (a, 29'un katı olmamalı)
    if (numA % m === 0) {
      alert(" 'a' değeri 29'un katı olamaz, şifre çözülemez hale gelir!");
      return;
    }

    for (let char of text) {
      const lowerChar = char.toLocaleLowerCase("tr");
      const idx = ALPHABET.indexOf(lowerChar);

      if (idx !== -1) {
        // Matematiksel işlem öncesi sayı güvenliği sağlandı
        const newIdx = (numA * idx + numB) % m;
        const encodedChar = ALPHABET[newIdx];

        output += (char === lowerChar)
          ? encodedChar
          : encodedChar.toLocaleUpperCase("tr");
      } else {
        output += char;
      }
    }

    result = output;
  }
</script>

<div class="container">
  <div class="card">

    <h2>Affine Şifreleyici</h2>

    <div class="input-group">
      <label>a değeri:</label>
      <input type="number" bind:value={a} min="1" max="28" />
    </div>

    <div class="input-group">
      <label>b değeri:</label>
      <input type="number" bind:value={b} min="0" max="28" />
    </div>

    <div class="input-group">
      <label>Metin:</label>
      <textarea
        bind:value={text}
        placeholder="Şifrelenecek metni girin..."
      ></textarea>
    </div>

    <button on:click={handleEncode}>
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
    box-sizing: border-box;
  }

  textarea {
    height: 100px;
    resize: vertical;
  }

  button {
    background: #242424;
    color: white;
    padding: 10px 20px;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    width: 100%;
    font-weight: bold;
  }

  .result-area {
    margin-top: 20px;
    border-top: 2px dashed #eee;
    padding-top: 20px;
  }
</style>