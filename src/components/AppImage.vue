<script>
export default {
  data() {
    return {
      selectedImage: null,
      brightness: 100,
      contrast: 100,
      invert: 0,
      opacity: 100,
      saturate: 100,
      blur: 0,
      hue: 0,
      sepia: 0,
      defaultValues: {
        brightness: 100,
        contrast: 100,
        invert: 0,
        opacity: 100,
        saturate: 100,
        blur: 0,
        hue: 0,
        sepia: 0,
      },
    };
  },
  mounted() {
    this.setupEventListeners();
  },
  methods: {
    handleImageUpload() {
      const imageInput = document.getElementById("imageInput");

      // Verifica se un file è stato selezionato
      if (imageInput.files.length > 0) {
        const selectedImage = imageInput.files[0];

        // Resetta i filtri ai valori predefiniti
        this.brightness = this.defaultValues.brightness;
        this.contrast = this.defaultValues.contrast;
        this.invert = this.defaultValues.invert;
        this.opacity = this.defaultValues.opacity;
        this.saturate = this.defaultValues.saturate;
        this.blur = this.defaultValues.blur;
        this.hue = this.defaultValues.hue;
        this.sepia = this.defaultValues.sepia;

        // Leggi il file come URL data (base64)
        const reader = new FileReader();
        reader.onload = (e) => {
          // Imposta il risultato della lettura come sorgente dell'immagine
          this.selectedImage = e.target.result;

          // Applica i filtri utilizzando i valori correnti degli input
          this.applyFilters();
        };
        reader.readAsDataURL(selectedImage);
      } else {
        // Se nessun file è stato selezionato, azzera la sorgente dell'immagine
        this.selectedImage = null;
      }
    },
    setupEventListeners() {
      const imageInput = document.getElementById("imageInput");
      const image = document.getElementById("image");

      imageInput.addEventListener("change", () => {
        const file = imageInput.files[0];
        const reader = new FileReader();

        reader.onload = (e) => {
          if (image) {
            image.src = e.target.result;
          }
        };

        reader.readAsDataURL(file);
      });
    },
    updateBrightness(event) {
      this.brightness = event.target.value;
      this.applyFilters();
    },
    updateContrast(event) {
      this.contrast = event.target.value;
      this.applyFilters();
    },
    updateInvert(event) {
      this.invert = event.target.value;
      this.applyFilters();
    },
    updateOpacity(event) {
      this.opacity = event.target.value;
      this.applyFilters();
    },
    updateSaturate(event) {
      this.saturate = event.target.value;
      this.applyFilters();
    },
    updateBlur(event) {
      this.blur = event.target.value;
      this.applyFilters();
    },
    updateHue(event) {
      this.hue = event.target.value;
      this.applyFilters();
    },
    updateSepia(event) {
      this.sepia = event.target.value;
      this.applyFilters();
    },
    applyFilters() {
      const image = document.getElementById("displayImage");

      if (image) {
        const filters = [];

        if (this.brightness !== 0) {
          filters.push(`brightness(${this.brightness}%)`);
        }

        if (this.contrast !== 0) {
          filters.push(`contrast(${this.contrast}%)`);
        }

        if (this.invert !== 0) {
          filters.push(`invert(${this.invert}%)`);
        }

        if (this.opacity !== 0) {
          filters.push(`opacity(${this.opacity}%)`);
        }

        if (this.saturate !== 0) {
          filters.push(`saturate(${this.saturate}%)`);
        }

        if (this.blur !== 0) {
          filters.push(`blur(${this.blur}px)`);
        }

        if (this.hue !== 0) {
          filters.push(`hue-rotate(${this.hue}deg)`);
        }

        if (this.sepia !== 0) {
          filters.push(`sepia(${this.sepia}%)`);
        }

        const filterValue = filters.join(" ");
        image.style.filter = filterValue;

        return filters.join(" ");
      }
    },
    resetFilters() {
      // Reimposta i filtri ai valori predefiniti
      this.brightness = this.defaultValues.brightness;
      this.contrast = this.defaultValues.contrast;
      this.invert = this.defaultValues.invert;
      this.opacity = this.defaultValues.opacity;
      this.saturate = this.defaultValues.saturate;
      this.blur = this.defaultValues.blur;
      this.hue = this.defaultValues.hue;
      this.sepia = this.defaultValues.sepia;

      // Applica i filtri
      this.applyFilters();
    },
    downloadImage() {
      if (this.selectedImage) {
        const canvas = document.createElement("canvas");
        const ctx = canvas.getContext("2d");
        const img = new Image();

        img.onload = () => {
          // Imposta le dimensioni del canvas come quelle dell'immagine
          canvas.width = img.width;
          canvas.height = img.height;

          // Applica i filtri all'immagine sul canvas
          const filterValue = this.applyFilters();
          ctx.filter = filterValue;
          ctx.drawImage(img, 0, 0, img.width, img.height);

          // Crea un link per il download dell'immagine
          const a = document.createElement("a");
          a.href = canvas.toDataURL(); // Converte il canvas in un URL dati
          a.download = "filtered_image.png"; // Imposta il nome del file
          a.click();
        };

        img.src = this.selectedImage;
      }
    },
  },
};
</script>
<template>
  <div>
    <div class="image d-flex justify-content-center mt-3 mb-4">
      <input
        type="file"
        id="imageInput"
        @change="handleImageUpload"
        class="form-control w-25"
      />
    </div>

    <div class="d-flex justify-content-center my-5">
      <img
        id="displayImage"
        :src="selectedImage"
        alt="Selected Image"
        class="rounded img-fluid"
        v-if="selectedImage"
      />
    </div>

    <div class="controls d-flex gap-3 justify-content-between mx-5">
      <div class="d-flex flex-column">
        <label for="brightness" class="form-label">Brightness</label>
        <input
          type="range"
          id="brightness"
          min="0"
          max="200"
          v-model="brightness"
          @input="updateBrightness"
        />
      </div>

      <div class="d-flex flex-column">
        <label for="contrast" class="form-label">Contrast</label>
        <input
          type="range"
          id="contrast"
          min="0"
          max="200"
          v-model="contrast"
          @input="updateContrast"
        />
      </div>

      <div class="d-flex flex-column">
        <label for="invert" class="form-label">Invert</label>
        <input
          type="range"
          id="invert"
          min="0"
          max="100"
          v-model="invert"
          @input="updateInvert"
        />
      </div>

      <div class="d-flex flex-column">
        <label for="opacity" class="form-label">Opacity</label>
        <input
          type="range"
          id="opacity"
          min="0"
          max="100"
          v-model="opacity"
          @input="updateOpacity"
        />
      </div>

      <div class="d-flex flex-column">
        <label for="saturate" class="form-label">Saturate</label>
        <input
          type="range"
          id="saturate"
          min="0"
          max="200"
          v-model="saturate"
          @input="updateSaturate"
        />
      </div>

      <div class="d-flex flex-column">
        <label for="blur" class="form-label">Blur</label>
        <input
          type="range"
          id="blur"
          min="0"
          max="20"
          v-model="blur"
          @input="updateBlur"
        />
      </div>

      <div class="d-flex flex-column">
        <label for="hue" class="form-label">Hue</label>
        <input
          type="range"
          id="hue"
          min="0"
          max="360"
          v-model="hue"
          @input="updateHue"
        />
      </div>

      <div class="d-flex flex-column">
        <label for="sepia" class="form-label">Sepia</label>
        <input
          type="range"
          id="sepia"
          min="0"
          max="100"
          v-model="sepia"
          @input="updateSepia"
        />
      </div>
    </div>
    <div class="d-flex justify-content-center gap-4 my-5">
      <button @click="resetFilters" class="btn btn-primary">
        Resetta Filtri
      </button>

      <button @click="downloadImage" class="btn btn-warning">
        Scarica Immagine
      </button>
    </div>
  </div>
</template>

<style lang="scss" scoped>
#displayImage {
  max-width: 800px;
  max-height: 400px;
}
</style>
