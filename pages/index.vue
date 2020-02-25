<template>
  <main class="flex flex-col">
    <ComponentEditor
      v-for="(component, index) in page.components"
      :key="component.name + index"
      :active="component.name === activeComponentName"
      :component="component"
      :index="index"
      :total="page.components.length"
      @action="handleEditorAction"
    >
      <component
        :is="component.name"
        v-bind="component.props"
        @click.native="toggleActiveComponent(component)"
      />
    </ComponentEditor>
  </main>
</template>
<script>
import ComponentEditor from '~/components/editor/ComponentEditor.vue'
import About from "~/components/About";
import Contact from "~/components/Contact";
import Features from "~/components/Features";
import Hero from "~/components/Hero";
import Services from "~/components/Services";
import Team from "~/components/Team";
export default {
  name: "landing-page",
  components: {
    ComponentEditor,
    About,
    Contact,
    Features,
    Hero,
    Services,
    Team
  },
  data() {
    return {
      activeComponentName: "",
      page: {
        components: [
          {
            name: "Hero",
            props: {
              backgroundColor: "#fff",
            }
          },
          {
            name: "Features",
            props: {
              backgroundColor: "#fff",
            }
          },
          {
            name: "About",
            props: {
              backgroundColor: "#fff",
            }
          },
          {
            name: "Team",
            props: {
              backgroundColor: "#fff",
            }
          },
          {
            name: "Services",
            props: {
              backgroundColor: "#fff",
            }
          },
          {
            name: "Contact",
            props: {
              backgroundColor: "#fff",
            }
          }
        ]
      }
    };
  },
  methods: {
    toggleActiveComponent(component) {
      this.activeComponentName = component.name;
    },
    handleEditorAction(action) {
      const { name, component } = action;
      const oldIndex = this.page.components.findIndex(
        c => c.name === component.name
      );
      let newIndex = oldIndex;
      if (name === "close") {
        this.activeComponentName = "";
      } else if (name === "move-down") {
        newIndex = oldIndex + 1;
        this.reOrderComponent(oldIndex, newIndex);
      } else if (name === "move-up") {
        newIndex = oldIndex - 1;
        this.reOrderComponent(oldIndex, newIndex);
      } else if (name === "delete") {
        this.page.components.splice(oldIndex, 1);
      } else if (name === "color-change") {
        this.$set(component.props, "backgroundColor", data.value);
      }
    },
    reOrderComponent(oldIndex, newIndex) {
      if (oldIndex === newIndex) {
        return;
      }
      const tempComponent = this.page.components[newIndex];
      const componentToMove = this.page.components[oldIndex];
      this.page.components.splice(newIndex, 1, componentToMove);
      this.page.components.splice(oldIndex, 1, tempComponent);
    }
  }
};
</script>