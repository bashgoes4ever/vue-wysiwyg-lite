<template lang="pug">
div(@mousedown="onBtnClick")
	a(:class="['vw-btn-'+module.title, {'active': isActive()}]", v-html="module.icon")

	.dashboard(
		v-show="showDashboard",
		ref="dashboard"
	)
		component(
      v-if="module.render",
      v-once,
      ref="moduleDashboard",
      :is="module",
      @exec="exec",
      :uid="uid"
      :options="options"
    )

</template>
<script>
import bus from 'src/editor/bus.js';

export default {
  props: ["module", "options", "selectionOptions"],

  data() {
    return {
      showDashboard: false,
    }
  },

  computed: {
    uid() {
      return this.$parent._uid
    }
  },

  methods: {
    isActive() {
      if (this.module.title === 'link' && this.selectionOptions.node === 'A') return true
      if (this.module.title === 'headings' && (this.selectionOptions.node && this.selectionOptions.node[0] === 'H')) return true
      if (this.module.title === 'italic' && this.selectionOptions.node === 'I') return true
      if (this.module.title === 'bold' && this.selectionOptions.node === 'B') return true
      if (this.module.title === 'underline' && this.selectionOptions.node === 'U') return true
      if (this.module.title === 'justifyLeft' && this.selectionOptions.textAlign === 'left') return true
      if (this.module.title === 'justifyCenter' && this.selectionOptions.textAlign === 'center') return true
      if (this.module.title === 'justifyRight' && this.selectionOptions.textAlign === 'right') return true
      return false
    },

    closeDashboard() {
      this.showDashboard = false;
    },

    openDashboard() {
      this.showDashboard = true;
    },

    exec() {
      console.log(arguments);
      this.$parent.exec.apply(null, arguments)
      this.$emit('updateOptions', {node: bus.selectedNode.nodeName, textAlign: bus.selectedNode.style.textAlign || false})
    },

    onBtnClick($event) {
      this.selection = window.getSelection();

      if (document.activeElement.classList.contains('editr--content')) {
        bus.selectedNode = this.selection.getRangeAt(0).commonAncestorContainer.parentElement;
        bus.selectedText = this.selection.getRangeAt(0).toString();
        if (!$event.target.nodeName == 'INPUT') {
          $event.preventDefault();
        }
      }

      if (this.module.action !== undefined)
        this.exec.apply(null, this.module.action);

      else if (this.module.customAction !== undefined) {
        this.module.customAction(bus.utils).forEach(a => this.exec.apply(null, a));
      } else if (
        this.module.render !== undefined &&
        (!this.$refs.dashboard || !this.$refs.dashboard.contains($event.target))
      ) {
        this.showDashboard = !this.showDashboard;
        bus.emit(`${this.uid}_${this.showDashboard ? "show" : "hide"}_dashboard_${this.module.title}`);
        return;
      }
    }
  }
}

</script>


<style>
.active {
  background: rgba(0,0,0,0.2);
}
</style>

