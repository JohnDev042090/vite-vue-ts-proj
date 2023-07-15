  <template>
    <div>
      <jsonforms :schema="schema" :uischema="uischema" :data="data" :renderers=irenderers></jsonforms>
    </div>
  </template>
  
  <script lang="ts">
  import '@progress/kendo-licensing';
  import '@progress/kendo-svg-icons';
  import { JsonForms } from '@jsonforms/vue';
  import { ref } from 'vue';
  // import { Input } from '@progress/kendo-vue-inputs';

  import { vanillaRenderers } from "@jsonforms/vue-vanilla";

  // mergeStyles combines all classes from both styles definitions into one
  // const myStyles = mergeStyles(defaultStyles, { control: { label: "mylabel" } });

  export default {
    components: {
        jsonforms: JsonForms,
        vanillaRenderers: vanillaRenderers
    },
    setup() {
      const schema = {
        "properties": {
            "name": {
            "type": "string"
            },
            "email": {
            "type": "string"
            }
        }
      };
      const uischema = {
        type: 'VerticalLayout',
        elements: [
          {
            type: 'Control',
            scope: '#/properties/name',
            label: 'Name'
          },
          {
            type: 'Control',
            scope: '#/properties/email',
            label: 'Email'
          }
        ]
      };
      const data = ref({
        name: 'John',
        email: 'John@gmail.com'
      });
      const irenderers = {
        ...vanillaRenderers
      };
      return {
        schema,
        uischema,
        data,
        irenderers
      };
    }
  };
  </script>
  