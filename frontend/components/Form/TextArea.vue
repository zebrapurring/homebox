<template>
  <div v-if="!inline" class="form-control w-full">
    <label class="label">
      <span class="label-text">{{ label }}</span>
    </label>
    <textarea ref="el" v-model="value" class="textarea textarea-bordered h-28 w-full" :placeholder="placeholder" />
    <label v-if="limit" class="label">
      <span class="label-text-alt"></span>
      <span class="label-text-alt"> {{ valueLen }}/{{ limit }}</span>
    </label>
  </div>
  <div v-else class="sm:grid sm:grid-cols-4 sm:items-start sm:gap-4">
    <label class="label">
      <span class="label-text">{{ label }}</span>
    </label>
    <textarea
      ref="el"
      v-model="value"
      class="textarea textarea-bordered col-span-3 mt-3 h-28 w-full"
      auto-grow
      :placeholder="placeholder"
      auto-height
    />
  </div>
</template>

<script lang="ts" setup>
  const emit = defineEmits(["update:modelValue"]);
  const props = defineProps({
    modelValue: {
      type: [String],
      required: true,
    },
    label: {
      type: String,
      required: true,
    },
    type: {
      type: String,
      default: "text",
    },
    limit: {
      type: [Number, String],
      default: null,
    },
    placeholder: {
      type: String,
      default: "",
    },
    inline: {
      type: Boolean,
      default: false,
    },
  });

  const el = ref();
  function setHeight() {
    el.value.style.height = "auto";
    el.value.style.height = el.value.scrollHeight + 5 + "px";
  }

  onUpdated(() => {
    if (props.inline) {
      setHeight();
    }
  });

  const value = useVModel(props, "modelValue", emit);
  const valueLen = computed(() => {
    return value.value ? value.value.length : 0;
  });
</script>
