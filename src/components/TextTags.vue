<template>
    <div :class="['text-tags', alignmentClass]" ref="container">
      <template v-for="(tag, index) in visibleTags">
        <v-icon v-if="index !== 0" class="text-tags__separator">mdi-circle-small</v-icon>
        <span class="text-tags__item">
          <v-icon v-if="tag.icon" :class="['text-tags__icon', tag.icon]">{{ tag.icon }}</v-icon>
          {{ tag.text }}
        </span>
      </template>
    </div>
  </template>
  
  <script>
  export default {
    name: 'TextTags',
    props: {
      tags: {
        type: Array,
        required: true
      },
      alignment: {
        type: String,
        default: 'left', // 'left' or 'justify'
        validator: function (value) {
          return ['left', 'justify'].includes(value);
        }
      }
    },
    data() {
      return {
        containerWidth: 0,
        visibleTags: []
      };
    },
    computed: {
      alignmentClass() {
        return this.alignment === 'justify' ? 'text-tags--justify' : 'text-tags--left';
      }
    },
    mounted() {
      window.addEventListener('resize', this.updateVisibleTags);
      this.$nextTick(this.updateVisibleTags);
    },
    beforeDestroy() {
      window.removeEventListener('resize', this.updateVisibleTags);
    },
    methods: {
      updateVisibleTags() {
        this.containerWidth = this.$refs.container.offsetWidth;
        this.calculateVisibleTags();
      },
      calculateVisibleTags() {
        this.visibleTags = [];
        let width = 0;
        const separatorWidth = 24; // Width of the separator icon
  
        for (let tag of this.tags) {
          const tagWidth = this.getTagWidth(tag);
          
          if (width + tagWidth <= this.containerWidth) {
            this.visibleTags.push(tag);
            width += tagWidth + separatorWidth;
          } else {
            break;
          }
        }
      },
      getTagWidth(tag) {
        const span = document.createElement('span');
        span.className = 'text-tags__item';
        span.style.visibility = 'hidden';
        span.style.position = 'absolute';
        span.textContent = tag.text;
  
        if (tag.icon) {
          const icon = document.createElement('v-icon');
          icon.className = `text-tags__icon ${tag.icon}`;
          span.insertBefore(icon, span.firstChild);
        }
  
        document.body.appendChild(span);
        const width = span.offsetWidth;
        document.body.removeChild(span);
  
        return width;
      }
    }
  };
  </script>
  
  <style lang="scss">
  .text-tags {
    display: flex;
    flex-wrap: nowrap;
    overflow: hidden;
    white-space: nowrap;
  
    &--left {
      justify-content: flex-start;
    }
  
    &--justify {
      justify-content: space-between;
    }
  
    &__separator {
      margin: 0 8px;
    }
  
    &__item {
      display: flex;
      align-items: center;
    }
  
    &__icon {
      margin-right: 4px;
    }
  }
  </style>
  