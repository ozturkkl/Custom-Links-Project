<template>
  <div class="container" :style="containerPosition" ref="container" @click.stop>
    <div class="header">Assigned Tags</div>
    <div
      class="tagEntry"
      v-for="tagID in this.assignedTags"
      :key="tagID"
      v-on:click="unassignTag(tagID)"
    >
      <div
        class="checkedCircle"
        :style="{ 'background-color': $store.getters.getTagColor(tagID) }"
        ></div>
      <div class="assignedTagLabel">
        {{ $store.getters.getTagName(tagID) }}</div
      >
    </div>
    <div class="header nonAssignedTags">Nonassigned Tags</div>
    <div
      class="tagEntry"
      v-for="nonassignedTagID in this.nonassignedTags"
      :key="'nonAssigned' + nonassignedTagID"
      v-on:click="assignTag(nonassignedTagID)"
    >
      <div
        class="uncheckedCircle"
        :style="{ 'background-color': $store.getters.getTagColor(nonassignedTagID) }"
        ></div>
      <div class="nonassignedTagLabel">
        {{ $store.getters.getTagName(nonassignedTagID) }}</div
      >
    </div>
  </div>
</template>

<script>
export default {
  data: function () {
    return {
      containerPosition: {
        top: "200px",
        left: "250px",
      },
      linkID: null,
      assignedTags: null,
      nonassignedTags: null,
      focusedTag: null,
    };
  },
  mounted: function () {
    this.setContainerPosition(this.$store.state.events.assignedTagsMenu.event);
    this.linkID = this.$store.state.events.assignedTagsMenu.arg.element.id;
    this.assignedTags = this.$store.getters.getTagsofLink(this.linkID);
    this.nonassignedTags = this.$store.getters.getTagsNotofLink(this.linkID);
  },
  updated: function () {
    this.setContainerPosition(this.$store.state.events.assignedTagsMenu.event);
  },
  methods: {
    unassignTag(tagID) {
      this.$store.commit("unassignTag", {
        tagID,
        linkID: this.linkID,
      });
      this.assignedTags = this.$store.getters.getTagsofLink(this.linkID);
      this.nonassignedTags = this.$store.getters.getTagsNotofLink(this.linkID);
    },
    assignTag(tagID) {
      this.$store.commit("assignTag", {
        tagID,
        linkID: this.linkID,
      });
      this.assignedTags = this.$store.getters.getTagsofLink(this.linkID);
      this.nonassignedTags = this.$store.getters.getTagsNotofLink(this.linkID);
    },
    setContainerPosition(event) {
      const mouseX = event.clientX;
      const mouseY = event.clientY;

      const windowW = window.innerWidth;
      const windowH = window.innerHeight;

      const menuW = this.$refs.container.getBoundingClientRect().width;
      const menuH = this.$refs.container.getBoundingClientRect().height;

      if (mouseX + menuW >= windowW) {
        this.containerPosition.left = mouseX - menuW + "px";
      } else {
        this.containerPosition.left = mouseX + "px";
      }

      if (mouseY + menuH >= windowH) {
        this.containerPosition.top = windowH - menuH - 5 + "px";
      } else {
        this.containerPosition.top = mouseY + "px";
      }
    },
  },
};
</script>


<style scoped>
.container {
  position: absolute;
  min-width: 100px;
  max-width: 180px;
  background: var(--dark-background-color);
  border-radius: 5px;
  border: 1px solid var(--active-background-color);
  flex-direction: column;
  padding: 0;
  overflow-y: scroll;
  max-height: 350px;
}
.container > div {
  padding: 2px;
  width: 100%;
}
.header {
  padding: 0;
  font-size: 12px;
  font-style: italic;
}
.tagEntry {
  align-items: center;
  justify-content: center;
  cursor: pointer;
}
.tagEntry:hover {
  background-color: var(--active-background-color);
}
.assignedTagLabel {
  cursor: pointer;
  margin-left: 10px;
  font-size: 14px;
  
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  display: inline-block;
  flex-grow: 2;
  margin-top: 4px;
  margin-bottom: 0px;
}
.checkedCircle {
  height: 21px;
  width: 21px;
  min-width: 21px;
  margin-left: 4px;
  border-radius: 50%;
  display: inline-block;
  border: solid var(--active-background-color) 1px;
}
.nonAssignedTags {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: var(--active-background-color);
  margin-top: 12px;
}
.nonassignedTagLabel {
  cursor: pointer;
  margin-left: 10px;
  font-size: 12px;
  filter: brightness(50%);
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  display: inline-block;
  flex-grow: 2;
  margin-top: 4px;
  margin-bottom: 0px;
}
.uncheckedCircle {
  height: 16px;
  width: 16px;
  min-width: 16px;
  margin-left: 7px;
  border-radius: 50%;
  display: inline-block;
}
</style>
