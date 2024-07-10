<template>
  <section
    style="
      display: flex;
      justify-content: center;
      align-items: center;
      height: 90vh;
      width: 100vw;
    "
  >
    <v-card class="submit-card">
      <section>
        <h2>Stitcher</h2>
      </section>
      <section>
        <v-file-input
          label="Click here to select Image"
          v-model="imgFile"
          variant="underlined"
          :error="errorObj.hasError"
          :show-size="true"
          prepend-icon=""
          :error-messages="errorObj.errorMessage"
        ></v-file-input>
      </section>

      <!-- * Submit Button -->
      <section class="mt-5">
        <v-btn
          variant="flat"
          @click="handleSubmit"
          append-icon="mdi-arrow-right"
          color="#645bff"
          class="submit-button"
          :loading="isLoading"
        >
          Submit
        </v-btn>
      </section>
    </v-card>
  </section>
</template>

<script>
import axios from "axios";
export default {
  name: "SubmitForm",
  data() {
    return {
      imgFile: null,
      validFileTypes: ["jpg", "jpeg", "png", "webp", "heic", "heif"],
      errorObj: {
        hasError: false,
        errorMessage: [],
      },
      isLoading: false,
    };
  },
  watch: {
    imgFile() {
      this.validateFile();
    },
  },
  methods: {
    validateFile() {
      if (!this.imgFile) {
        this.errorObj.hasError = true;
        this.errorObj.errorMessage = ["Please select an image"];
      } else {
        // Extract the file extension
        const fileExtension = this.imgFile.type.split("/").pop().toLowerCase();

        console.log(fileExtension);
        // Check if the extension is in the array of valid image file types
        if (this.validFileTypes.includes(fileExtension)) {
          this.errorObj.hasError = false;
          this.errorObj.errorMessage = [];
        } else {
          this.errorObj.hasError = true;
          this.errorObj.errorMessage = ["Please select a valid image"];
        }
      }
    },
    async handleSubmit() {
      this.validateFile();

      if (!this.errorObj.hasError) {
        const formData = new FormData();
        formData.append("image", this.imgFile);

        this.isLoading = true;

        const response = await axios.post(
          "https://api.stitcher.twelveletter.co/stitcher",
          formData,
          { responseType: "blob" }
        );

        this.isLoading = false;
        // Handle the response (which is a blob)
        const url = window.URL.createObjectURL(new Blob([response.data]));
        const link = document.createElement("a");
        link.href = url;
        link.setAttribute("download", "assets.zip"); // specify the filename
        document.body.appendChild(link);
        link.click();
        link.remove(); // Clean up the element
      }
    },
  },
};
</script>

<style>
.submit-button:hover {
  background: #111 !important;
}
.submit-card {
  display: flex !important;
  justify-content: center !important;
  align-items: left !important;
  padding: 2rem !important;
  flex-direction: column;
  height: 40%;
  width: 50%;
  box-shadow: rgba(0, 0, 0, 0.2) 0px 12px 28px 0px,
    rgba(0, 0, 0, 0.1) 0px 2px 4px 0px,
    rgba(255, 255, 255, 0.05) 0px 0px 0px 1px inset !important;
}

/* On screens that are 992px wide or less, go from four columns to two columns */
@media screen and (max-width: 992px) {
  .submit-card {
    width: 100%;
    height: 40%;
  }
}

/* On screens that are 600px wide or less, make the columns stack on top of each other instead of next to each other */
@media screen and (max-width: 600px) {
  .submit-card {
    width: 100%;
    height: 50%;
  }
}
</style>
