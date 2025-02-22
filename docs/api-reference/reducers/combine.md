<!-- Generated by documentation.js. Update this documentation by updating the source code. -->

### Table of Contents

- [combinedUpdaters](#combinedupdaters)
  - [addDataToMapUpdater](#adddatatomapupdater)

## combinedUpdaters

Some actions will affect the entire kepler.lg instance state.
The updaters for these actions is exported as `combinedUpdaters`. These updater take the entire instance state
as the first argument. Read more about [Using updaters][5]

**Examples**

```javascript
import keplerGlReducer, {combinedUpdaters} from '@kepler.gl/reducers';
// Root Reducer
const reducers = combineReducers({
 keplerGl: keplerGlReducer,
 app: appReducer
});

const composedReducer = (state, action) => {
 switch (action.type) {
   // add data to map after receiving data from remote sources
   case 'LOAD_REMOTE_RESOURCE_SUCCESS':
     return {
       ...state,
       keplerGl: {
         ...state.keplerGl,
         // pass in kepler.gl instance state to combinedUpdaters
         map:  combinedUpdaters.addDataToMapUpdater(
          state.keplerGl.map,
          {
            payload: {
              datasets: action.datasets,
              options: {readOnly: true},
              config: action.config
             }
           }
         )
       }
     };
 }
 return reducers(state, action);
};

export default composedReducer;
```

### addDataToMapUpdater

Combine data and full configuration update in a single action

-   **Action**: [`addDataToMap`][6]

**Parameters**

-   `state` **[Object][7]** kepler.gl instance state, containing all subreducer state
-   `action` **[Object][7]**
    -   `action.payload` **[Object][7]** `{datasets, options, config}`
        -   `action.payload.datasets` **([Array][8]&lt;[Object][7]> | [Object][7])** **\*required** datasets can be a dataset or an array of datasets
            Each dataset object needs to have `info` and `data` property.
            -   `action.payload.datasets.info` **[Object][7]** \-info of a dataset
                -   `action.payload.datasets.info.id` **[string][9]** id of this dataset. If config is defined, `id` should matches the `dataId` in config.
                -   `action.payload.datasets.info.label` **[string][9]** A display name of this dataset
            -   `action.payload.datasets.data` **[Object][7]** **\*required** The data object, in a tabular format with 2 properties `fields` and `rows`
                -   `action.payload.datasets.data.fields` **[Array][8]&lt;[Object][7]>** **\*required** Array of fields,
                    -   `action.payload.datasets.data.fields.name` **[string][9]** **\*required** Name of the field,
                -   `action.payload.datasets.data.rows` **[Array][8]&lt;[Array][8]>** **\*required** Array of rows, in a tabular format with `fields` and `rows`
        -   `action.payload.options` **[Object][7]** option object `{centerMap: true}`
        -   `action.payload.config` **[Object][7]** map config

Returns **[Object][7]** nextState

[1]: #combinedupdaters

[2]: #examples

[3]: #adddatatomapupdater

[4]: #parameters

[5]: ../advanced-usage/using-updaters.md

[6]: ../actions/actions.md#adddatatomap

[7]: https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object

[8]: https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array

[9]: https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String
