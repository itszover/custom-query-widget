# Custom ArcGis Web AppBuilder Query Widget
Widget Query personalizado para atender a uma demanda para um aplicativo 
específico no ArcGis Portal.<br>
Quando o resultado de uma pesquisa é clicado, a camada relacionada a categoria
do resultado se torna visível.
## **Código adicionado:**
* Dentro de **SingleQueryResult.js**:

  ```js
  _onResultItemClick: function (event) {
    const feature = event.target.closest('.jimu-table-row')?.feature;
    if (!feature) return;

    const categoria = feature.attributes?.CATEGORIA;
    if (!categoria) return;

    const layerNodes = this.layerStructure.getLayerNodes();
  
    for (const layer of layerNodes) {
      for (const subNode of layer.getSubNodes()) {
        if (subNode.title.toLowerCase().trim() === categoria.toLowerCase().trim()) {
          subNode.show();
        } else {
          subNode.hide();
        }
      }
    }
  },
  ```
  A função captura o clique e procura pelo **'.jimu-table-row'** mais próximo e
  atribui a categoria para a variável **categoria**. Depois, é realizado uma
  iteração para cada camada, e também, para cada subcamada. É verificado se a
  **categoria** é igual ao **título** da subcamada, se for, a camada é mostrada,
  senão é escondida.
<hr>

* Dentro da função  **_createQueryResultItem**

    ```js
    // ... Código existente
    html.place(trItem, this.resultsTbody);
    trItem.feature = feature;
    
    // Código adicionado
    var tableItem = query('td', trItem)[0];
    on(tableItem, 'click', lang.hitch(this, function(event) {
      this._onResultItemClick(event);
    }))
    // Fim do código adicionado
    // Restante da função ...
    ```
    Nesse código, é adicionado uma captura de evento do tipo **click** para
    os elementos **'td'** dentro de **trItem**. Quando for clicado, a função 
    **_onResultItemClick** é executada.
