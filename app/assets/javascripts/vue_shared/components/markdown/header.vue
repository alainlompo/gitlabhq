<script>
  import tooltip from '../../directives/tooltip';
  import toolbarButton from './toolbar_button.vue';

  export default {
    props: {
      previewMarkdown: {
        type: Boolean,
        required: true,
      },
    },
    directives: {
      tooltip,
    },
    components: {
      toolbarButton,
    },
    methods: {
      toggleMarkdownPreview(e, form) {
        if (form && !form.find('.js-vue-markdown-field').length) {
          return;
        } else if (e.target.blur) {
          e.target.blur();
        }

        this.$emit('toggle-markdown');
      },
    },
    mounted() {
      $(document).on('markdown-preview:show.vue', this.toggleMarkdownPreview);
      $(document).on('markdown-preview:hide.vue', this.toggleMarkdownPreview);
    },
    beforeDestroy() {
      $(document).on('markdown-preview:show.vue', this.toggleMarkdownPreview);
      $(document).off('markdown-preview:hide.vue', this.toggleMarkdownPreview);
    },
  };
</script>

<template>
  <div class="md-header">
    <ul class="nav-links clearfix">
      <li :class="{ active: !previewMarkdown }">
        <a
          href="#md-write-holder"
          tabindex="-1"
          @click.prevent="toggleMarkdownPreview($event)">
          Write
        </a>
      </li>
      <li :class="{ active: previewMarkdown }">
        <a
          href="#md-preview-holder"
          tabindex="-1"
          @click.prevent="toggleMarkdownPreview($event)">
          Preview
        </a>
      </li>
      <li class="pull-right">
        <div class="toolbar-group">
          <toolbar-button
            tag="**"
            button-title="Add bold text"
            icon="bold" />
          <toolbar-button
            tag="*"
            button-title="Add italic text"
            icon="italic" />
          <toolbar-button
            tag="> "
            :prepend="true"
            button-title="Insert a quote"
            icon="quote-right" />
          <toolbar-button
            tag="`"
            tag-block="```"
            button-title="Insert code"
            icon="code" />
          <toolbar-button
            tag="* "
            :prepend="true"
            button-title="Add a bullet list"
            icon="list-ul" />
          <toolbar-button
            tag="1. "
            :prepend="true"
            button-title="Add a numbered list"
            icon="list-ol" />
          <toolbar-button
            tag="* [ ] "
            :prepend="true"
            button-title="Add a task list"
            icon="check-square-o" />
        </div>
        <div class="toolbar-group">
          <button
            v-tooltip
            aria-label="Go full screen"
            class="toolbar-btn js-zen-enter"
            data-container="body"
            tabindex="-1"
            title="Go full screen"
            type="button">
            <i
              aria-hidden="true"
              class="fa fa-arrows-alt fa-fw">
            </i>
          </button>
        </div>
      </li>
    </ul>
  </div>
</template>
