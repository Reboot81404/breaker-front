<script>
  /*
    5x5 Türkçe Playfair Cipher

    Ortak hücre kullanan harfler:
    g = ğ
    i = j
    s = ş
    u = ü

    Toplam 25 karakter:
    a b c ç d
    e f g h ı
    i k l m n
    o ö p r s
    t u v y z
  */

  const ALPHABET = [
    "a", "b", "c", "ç", "d",
    "e", "f", "g", "h", "ı",
    "i", "k", "l", "m", "n",
    "o", "ö", "p", "r", "s",
    "t", "u", "v", "y", "z"
  ];

  let text = "";
  let key = "";
  let result = "";
  let matrix = [];

  function normalizeText(str) {
    return str
      .toLocaleLowerCase("tr")
      .replace(/ğ/g, "g")
      .replace(/j/g, "i")
      .replace(/ş/g, "s")
      .replace(/ü/g, "u")
      .replace(/[^abcçdefgğhıijklmnoöprsştuüvyz]/g, "");
  }

  function generateMatrix(keyword) {
    const used = new Set();
    const chars = [];

    const cleanKey = normalizeText(keyword);

    for (const char of cleanKey) {
      if (!used.has(char) && ALPHABET.includes(char)) {
        used.add(char);
        chars.push(char);
      }
    }

    for (const char of ALPHABET) {
      if (!used.has(char)) {
        used.add(char);
        chars.push(char);
      }
    }

    const grid = [];

    for (let i = 0; i < 25; i += 5) {
      grid.push(chars.slice(i, i + 5));
    }

    return grid;
  }

  function findPosition(char, matrix) {
    for (let row = 0; row < matrix.length; row++) {
      for (let col = 0; col < matrix[row].length; col++) {
        if (matrix[row][col] === char) {
          return { row, col };
        }
      }
    }

    return null;
  }

  function prepareText(input) {
  const clean = normalizeText(input);
  const pairs = [];
  let i = 0;

  while (i < clean.length) {
    const first = clean[i];
    let second = clean[i + 1];

    if (!second) {
      pairs.push([first, "y"]);
      break;
    }

    if (first === second) {
      pairs.push([first, "y"]);
      i += 1;
    }
    else {
      pairs.push([first, second]);
      i += 2;
    }
  }

  return pairs;
}

  // Encode
  function handleEncode() {
    if (!text || !key) return;

    matrix = generateMatrix(key);

    const pairs = prepareText(text);

    let output = "";

    for (const [a, b] of pairs) {
      const posA = findPosition(a, matrix);
      const posB = findPosition(b, matrix);

      if (!posA || !posB) continue;

      if (posA.row === posB.row) {
        output += matrix[posA.row][(posA.col + 1) % 5];

        output += matrix[posB.row][(posB.col + 1) % 5];
      }

      else if (posA.col === posB.col) {
        output += matrix[(posA.row + 1) % 5][posA.col];

        output += matrix[(posB.row + 1) % 5][posB.col];
      }

      else {
        output += matrix[posA.row][posB.col];

        output += matrix[posB.row][posA.col];
      }
    }

    result = output.toUpperCase();
  }
</script>

<div class="container">
  <div class="card">

    <h2>Türkçe 5x5 Playfair Cipher</h2>

    <div class="info">
      <p>Ortak hücre kullanan harfler:</p>

      <ul>
        <li>g = ğ</li>
        <li>i = j</li>
        <li>s = ş</li>
        <li>u = ü</li>
      </ul>
    </div>

    <div class="input-group">
      <label>Anahtar</label>

      <input
        type="text"
        bind:value={key}
        placeholder="Anahtar girin..."
      />
    </div>

    <div class="input-group">
      <label>Metin</label>

      <textarea
        bind:value={text}
        placeholder="Şifrelenecek metni girin..."
      ></textarea>
    </div>

    <button
      on:click={handleEncode}
      disabled={!text || !key}
    >
      Şifrele
    </button>

    {#if result}
      <div class="result-area">

        <h3>Şifrelenmiş Metin</h3>

        <textarea readonly value={result}></textarea>

        <div class="matrix-area">

          <h3>5x5 Matris</h3>

          <div class="matrix">
            {#each matrix as row}
              <div class="matrix-row">

                {#each row as char}
                  <div class="cell">
                    {char.toUpperCase()}
                  </div>
                {/each}

              </div>
            {/each}
          </div>

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
    min-height: 100vh;
    padding: 20px;
    box-sizing: border-box;
    background: #f5f5f5;
    font-family: sans-serif;
  }

  .card {
    width: 100%;
    max-width: 700px;
    background: white;
    padding: 2rem;
    border-radius: 12px;
    box-shadow: 0 4px 12px rgba(0,0,0,0.1);
  }

  h2 {
    margin-top: 0;
  }

  .info {
    background: #f7f7f7;
    border: 1px solid #ddd;
    border-radius: 8px;
    padding: 12px;
    margin-bottom: 20px;
  }

  .info ul {
    margin: 10px 0 0 20px;
    padding: 0;
  }

  .input-group {
    display: flex;
    flex-direction: column;
    margin-bottom: 16px;
  }

  label {
    font-weight: bold;
    margin-bottom: 6px;
  }

  input,
  textarea {
    width: 100%;
    padding: 12px;
    border-radius: 6px;
    border: 1px solid #ccc;
    box-sizing: border-box;
    font-family: inherit;
  }

  textarea {
    min-height: 100px;
    resize: vertical;
  }

  button {
    width: 100%;
    padding: 12px;
    border: none;
    border-radius: 6px;
    background: #222;
    color: white;
    cursor: pointer;
    font-weight: bold;
  }

  button:disabled {
    background: #bbb;
    cursor: not-allowed;
  }

  .result-area {
    margin-top: 24px;
    padding-top: 20px;
    border-top: 2px dashed #ddd;
  }

  .matrix-area {
    margin-top: 20px;
  }

  .matrix {
    display: flex;
    flex-direction: column;
    gap: 6px;
    margin-top: 10px;
  }

  .matrix-row {
    display: flex;
    gap: 6px;
  }

  .cell {
    width: 50px;
    height: 50px;
    border-radius: 6px;
    border: 1px solid #ccc;
    background: #f2f2f2;
    display: flex;
    align-items: center;
    justify-content: center;
    font-weight: bold;
  }
</style>