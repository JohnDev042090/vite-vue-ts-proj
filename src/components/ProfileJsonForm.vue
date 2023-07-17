  <template>
    <div>
      <JsonForms :schema="schema" :uischema="uischema" :data="data" :renderers=irenderers></JsonForms>
    </div>
  </template>
  
  <script lang="ts">
  import '@progress/kendo-licensing';
  import '@progress/kendo-svg-icons';
  import { JsonForms } from '@jsonforms/vue';
  import { ref } from 'vue';

  import { vanillaRenderers } from "@jsonforms/vue-vanilla";

  // mergeStyles combines all classes from both styles definitions into one
  // const myStyles = mergeStyles(defaultStyles, { control: { label: "mylabel" } });

  export default {
    components: {
      JsonForms: JsonForms,
      vanillaRenderers: vanillaRenderers
    },
    setup() {
      const schema = {
        "properties": {
            "firstName": {
            "type": "string"
            },
            "lastName": {
            "type": "string"
            },
            "email": {
            "type": "string"
            },
            "birthDate": {
              "type": "string",
              "format": "date"
            }
        }
      };
      const uischema = {
        type: 'VerticalLayout',
        elements: [
          {
            type: 'Control',
            scope: '#/properties/firstName',
            label: 'First Name'
          },
          {
            type: 'Control',
            scope: '#/properties/lastName',
            label: 'Last Name'
          },
          {
            type: 'Control',
            scope: '#/properties/email',
            label: 'Email'
          },
          {
            "type": "Control",
            "label": "Birth Date",
            "scope": "#/properties/birthDate"
          }
        ]
      };
      const data = ref({
        firstName: 'John',
        lastName: 'Herrera',
        birthDate: null,
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
  