<template>
  <div class="spacing-settings-plugin">
    <div class="_margin">
      <div class="_inputs-group">
        <div class="_top-section">
          <input
            type="text"
            name="margin-top"
            tabindex="1"
            v-model="model.margin_top"
            :class="{ '-invalid': !isValueValid(model.margin_top) }"
          />
        </div>

        <div class="_middle-section">
          <input
            type="text"
            name="margin-left"
            tabindex="4"
            v-model="model.margin_left"
            :class="{ '-invalid': !isValueValid(model.margin_left) }"
          />

          <div class="_padding">
            <div class="_inputs-group">
              <div class="_top-section">
                <input
                  type="text"
                  name="padding-top"
                  tabindex="5"
                  v-model="model.padding_top"
                  :class="{ '-invalid': !isValueValid(model.padding_top) }"
                />
              </div>

              <div class="_middle-section">
                <input
                  type="text"
                  name="padding-left"
                  tabindex="8"
                  v-model="model.padding_left"
                  :class="{ '-invalid': !isValueValid(model.padding_left) }"
                />

                <input
                  type="text"
                  name="padding-right"
                  tabindex="6"
                  v-model="model.padding_right"
                  :class="{ '-invalid': !isValueValid(model.padding_right) }"
                />
              </div>

              <div class="_bottom-section">
                <input
                  type="text"
                  name="padding-bottom"
                  tabindex="7"
                  v-model="model.padding_bottom"
                  :class="{ '-invalid': !isValueValid(model.padding_bottom) }"
                />
              </div>
            </div>

            <fieldset class="_legend">
              <legend>Padding</legend>
            </fieldset>
          </div>

          <input
            type="text"
            name="margin-right"
            tabindex="2"
            v-model="model.margin_right"
            :class="{ '-invalid': !isValueValid(model.margin_right) }"
          />
        </div>

        <div class="_bottom-section">
          <input
            type="text"
            name="margin-bottom"
            tabindex="3"
            v-model="model.margin_bottom"
            :class="{ '-invalid': !isValueValid(model.margin_bottom) }"
          />
        </div>
      </div>

      <fieldset class="_legend">
        <legend>Margin</legend>
      </fieldset>
    </div>
  </div>
</template>

<script>
export default {
  mixins: [window.Storyblok.plugin],
  methods: {
    initWith() {
      return {
        // needs to be equal to your storyblok plugin name
        plugin: "spacing-settings",
        title: "",
        description: "",
        margin_top: "",
        margin_right: "",
        margin_bottom: "",
        margin_left: "",
        padding_top: "",
        padding_right: "",
        padding_bottom: "",
        padding_left: "",
      };
    },
    pluginCreated() {
      // eslint-disable-next-line
      console.log(
        "View source and customize: https://github.com/storyblok/storyblok-fieldtype"
      );
    },
    isValueValid(value) {
      if (value === "") {
        return true;
      }

      const validator = /^-?\d{1,4}(px|%|em|ex|rem|vw|vh)?$/;

      return validator.test(value);
    },
    isModelValid() {
      const fields = [
        "margin_top",
        "margin_right",
        "margin_bottom",
        "margin_left",
        "padding_top",
        "padding_right",
        "padding_bottom",
        "padding_left",
      ];

      for (const field of fields) {
        if (!this.isValueValid(this.model[field])) {
          return false;
        }
      }

      return true;
    },
  },
  watch: {
    model: {
      handler: function (value) {
        if (!this.isModelValid()) {
          return;
        }

        this.$emit("changed-model", value);
      },
      deep: true,
    },
  },
};
</script>

<style lang="scss" scoped>
.spacing-settings-plugin {
  ._margin,
  ._padding {
    position: relative;

    ._legend {
      position: absolute;
      top: 0.5em;
      bottom: 1.7em;
      left: 2.1em;
      right: 2.1em;
      padding: 1.25rem;
      box-sizing: border-box;
      border: 1px solid #cacaca;
      z-index: 0;

      legend {
        font-size: 0.9em;
        margin: 0;
        margin-left: -1.8rem;
        padding: 0 0.1875rem;
        width: min-content;

        &::after {
          display: none;
        }
      }
    }
  }

  ._margin {
    font-size: 0.9em;
  }

  ._padding {
    margin: 0.5em;
  }

  ._inputs-group {
    display: flex;
    flex-direction: column;
    justify-content: stretch;
    align-items: stretch;
    position: relative;
    z-index: 1;

    input {
      flex-grow: 0;
      width: 4.2em;
      padding: 0.5em;
      text-align: center;

      &.-invalid {
        background: #fff1f0;
        color: #d85030;
        border: 1px solid #d85030;
      }
    }
  }

  ._top-section,
  ._bottom-section {
    flex-grow: 0;
    text-align: center;
  }

  ._middle-section {
    flex-grow: 1;
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: center;

    ._padding {
      flex-grow: 1;
    }
  }
}
</style>
