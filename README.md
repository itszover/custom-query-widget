# Custom ArcGis Web App Builder Query Widget
## Widget Query personalizado para atender a uma demanda para um aplicativo específico no ArcGis Portal.
### **Funcionalidade:** Quando o resultado de uma pesquisa é clicado, a camada relacionada a linha do resultado se torna visível.
### **Código adicionado:**
* Dentro de _SingleQueryResult.js_
  ```js
  _onResultItemClick: function(event) {
    const feature = event.target.closest('.jimu-table-row').feature;
    const categoria = feature.attributes.attribute;
    const mainLayerNode = this.layerStructure.getNodeById('SEU_ID');
  
    mainLayerNode.getSubNodes().forEach(function (node) {
      if (node.title.toLowerCase() === categoria.toLowerCase()) 
        { node.show(); } 
      else 
        { node.hide(); }
    });
  },
  ```
* Dentro da função  `_createQueryResultItem(options, i)`
    ```js
    // ...
    // Código existente
    html.place(trItem, this.resultsTbody);
    trItem.feature = feature;
    
    // Código adicionado
    var tableItem = query('.query-result-item-table', trItem)[0];
    on(tableItem, 'click', lang.hitch(this, function(event) {
      this._onResultItemClick(event);
    }))
    
    // Restante da função
    // ...
    ```
