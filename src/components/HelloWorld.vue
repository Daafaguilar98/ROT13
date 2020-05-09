<template>
  <div class="matrix">
    <div class="matrix__input">
      <p class="matrix__input__label">Valiu as <strong>Matrix</strong></p>
      <input type="text" placeholder="Enter encrypted text" v-model="encryptedText">
      <button @click="encodingText()">DECODE</button>
    </div>
    <div class="matrix__output">
      <p class="matrix__output__text">{{ newText }}</p>
      <canvas></canvas>
    </div>
  </div>
</template>

<script>

export default {
  mounted() {
    // Initialising the canvas
    const canvas = document.querySelector('canvas');
    const ctx = canvas.getContext('2d');

    // Setting the width and height of the canvas
    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight;

    // Setting up the letters
    let letters = this.letters;
    letters = letters.split('');

  // Setting up the columns
  const fontSize = 10;
  const columns = canvas.width / fontSize;

  // Setting up the drops
  const drops = [];
  for (let i = 0; i < columns; i++) {
    drops[i] = 1;
  }

    // Setting up the draw function
    function draw() {
      ctx.fillStyle = 'rgba(0, 0, 0, .1)';
      ctx.fillRect(0, 0, canvas.width, canvas.height);
      for (let i = 0; i < drops.length; i++) {
        const text = letters[Math.floor(Math.random() * letters.length)];
        ctx.fillStyle = '#0f0';
        ctx.fillText(text, i * fontSize, drops[i] * fontSize);
        drops[i]++;
      }
    }

    // Loop the animation
    setInterval(draw, 50);
  },
  data() {
    return {
      showCanvas: true,
      encryptedText: "",
      newText: "",
      letters: "ABCDEFGHIJKLMNOPQRSTUVXYZABCDEFGHIJKLMNOPQRSTUVXYZABCDEFGHIJKLMNOPQRSTUVXYZABCDEFGHIJKLMNOPQRSTUVXYZABCDEFGHIJKLMNOPQRSTUVXYZABCDEFGHIJKLMNOPQRSTUVXYZ"
    }
  },
  watch: {
    encryptedText(newValue) {
      this.newText = newValue
    },
  },
  methods: {
    async encodingText() {
      const letters = this.letters.toLowerCase().split('');

      for(let i = 0; i < this.encryptedText.length; i++) {
          const lastCharacter = this.newText[i].toLowerCase();
          const text = this.newText.split("");
          for(let i2 = 0; i2 < 6; i2++) {
            text[i] = letters[Math.floor(Math.random() * letters.length)];
            this.newText = text.join("");
            await this.sleep(100)
          }
          text[i] = this.rationCharacterNtimes(lastCharacter, 13);
            this.newText = text.join("");
      }
    },
    rationCharacterNtimes(c, n) {
      return String.fromCharCode((c.charCodeAt(0) + n - 97) % 26 + 97)
    },
    sleep(ms) {
      return new Promise(resolve => setTimeout(resolve, ms));
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.matrix__text{
  color: #00FF41;
  display: grid;
  grid-template-rows: auto 1fr;
}
.matrix__input{
  padding: 20px;
  height: 20vh;

  &__label{
    font-size: 22px;
    margin-bottom: 20px;

    strong {
      color: #00FF41;
    }
  }

  input{
    background-color: rgba(255,255,255,0.2);
    padding: 10px 15px;
    font-size: 22px;
    border-radius: 4px;
    border: 1px solid #fff;
    outline-color: #00FF41;
    color: #fff;
    margin-right: 5px;
  }

  button{
    padding: 10px 15px;
    font-size: 22px;
    border-radius: 4px;
    outline: none;
    border-style: none;
    cursor: pointer;
  }

}
.matrix__output{
  width: 100%;
  height: 80vh;
  position: relative;
  display: flex;
  justify-content: center;

  &__text{
    position: absolute;
    font-size: 33px;
  }
}
.matrix__output canvas{
  width: 100%;
  height: 99%;
}
@media(max-width: 900px) {
  .matrix__input{

    input{
      margin-bottom: 10px;
    }
  }
}
</style>
