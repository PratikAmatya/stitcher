<template>
  <section
    style="
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      width: 100vw;
    "
  >
    <v-card
      min-width="500"
      max-width="900"
      min-height="300"
      class="submit-card"
    >
      <v-form ref="form">
        <section class="pb-8">
          <h2>Stitcher</h2>
        </section>
        <section class="d-block">
          <v-file-input
            label="File input"
            v-model="imgFile"
            style="overflow-x: hidden; width: 25rem"
            variant="underlined"
            :rules="[rules.required]"
          ></v-file-input>
        </section>

        <!-- * Submit Button -->
        <section class="mt-5">
          <button @click="handleSubmit">
            Submit
            <div class="arrow-wrapper">
              <div class="arrow"></div>
            </div>
          </button>
        </section>
      </v-form>
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
      rules: {
        required: (value) => !!value || "Required.",
      },
    };
  },
  methods: {
    async handleSubmit() {
      const { valid } = await this.$refs.form.validate();
      if (valid) {
        const formData = new FormData();
        formData.append("file", this.imgFile);

        await apiManager.axios
          .post("/v1/models", newModel)
          .then(() => {
            return "success";
          })
          .catch((err) => {
            return (
              err.response.data.message ||
              err.response.data.error.message ||
              "Network error"
            );
          });
        await axios({
          method: "POST",
          url: "https://api.stitcher.twelveletter.co/stitcher",
          data: formData,
          headers: {
            "Content-Type": "multipart/form-data",
          },
        });
      }
    },
  },
};
</script>

<style>
.submit-card {
  display: flex !important;
  justify-content: center !important;
  align-items: center !important;
  flex-direction: column;
  border-radius: 0 !important;
  box-shadow: rgba(0, 0, 0, 0.2) 0px 12px 28px 0px,
    rgba(0, 0, 0, 0.1) 0px 2px 4px 0px,
    rgba(255, 255, 255, 0.05) 0px 0px 0px 1px inset !important;
}

button {
  --primary-color: #645bff;
  --secondary-color: #fff;
  --hover-color: #111;
  --arrow-width: 10px;
  --arrow-stroke: 2px;
  box-sizing: border-box;
  border: 0;
  border-radius: 0px;
  color: var(--secondary-color);
  padding: 1em 1.8em;
  background: var(--primary-color);
  display: flex;
  transition: 0.2s background;
  align-items: center;
  gap: 0.6em;
  font-weight: bold;
}

button .arrow-wrapper {
  display: flex;
  justify-content: center;
  align-items: center;
}

button .arrow {
  margin-top: 1px;
  width: var(--arrow-width);
  background: var(--primary-color);
  height: var(--arrow-stroke);
  position: relative;
  transition: 0.2s;
}

button .arrow::before {
  content: "";
  box-sizing: border-box;
  position: absolute;
  border: solid var(--secondary-color);
  border-width: 0 var(--arrow-stroke) var(--arrow-stroke) 0;
  display: inline-block;
  top: -3px;
  right: 3px;
  transition: 0.2s;
  padding: 3px;
  transform: rotate(-45deg);
}

button:hover {
  background-color: var(--hover-color);
}

button:hover .arrow {
  background: var(--secondary-color);
}

button:hover .arrow:before {
  right: 0;
}
</style>
