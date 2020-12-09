---
title: "Nuxeo: Documentos relacionados"
header:
  image: /images/nuxeo-related-documents-list-570x255.png
  og_image: /images/nuxeo-related-documents-list-570x255.png
tags:
  - Nuxeo
  - Web UI
last_modified_at: 2018-06-05-T20:59:57-04:00  
---

El **API REST de Nuxeo** proporciona 3 métodos para **consultar, crear y borrar relaciones entre documentos**, la descripción de cada una de estas operaciones se pueden consultar en Nuxeo Platform Explorer. Sin embargo en el momento de escribir este artículo, Junio de 2018, no existe un web element que encapsule la funcionalidad de documentos relacionados con un documento dado en Nuxeo. En este artículo vamos a crear nuestro propipo **web element** paso a paso.


![Nuxeo Platform Explorer](/images/nuxeo-platform-explorer-744x440.png "Nuxeo Platform Explorer")


Crearemos un nuevo web element llamado nuxeo-se-document-relations que podremos incluir en el view layout de cualquier tipo de documento.

```html
<nuxeo-se-document-relations document="[[document]]"></nuxeo-se-document-relations>
```

El resultado será un listado con los documentos relacionados, en el que se incluyen la dirección de la relación (entrante/saliente) y el tipo de relación (Basado en, Referencia…). Podremos eliminar las relaciones existentes o añadir una nueva.

> Los tipos de relaciones estan definidos en los vocabularios predicates y inverse_predicates. La definición de ambos vocabularios se puede consultar en el menú **Administración > Vocabularios** de Web UI (la interface web de Nuxeo)

En el siguiente ejemplo hemos añadido nuestro web element a la vista del tipo de documento File:

```html 
<!--
(C) Copyright 2015 Nuxeo SA (http://nuxeo.com/) and others.

Licensed under the Apache License, Version 2.0 (the 'License');
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an 'AS IS' BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
-->

<!--
`nuxeo-file-view-layout`
@group Nuxeo UI
@element nuxeo-file-view-layout
-->
<link rel="import" href="../../elements/nuxeo-relations/nuxeo-se-document-relations.html"> 

<dom-module id="nuxeo-file-view-layout">

  <template>
    <nuxeo-document-viewer role="widget" document="[[document]]"></nuxeo-document-viewer>
    <nuxeo-document-attachments role="widget" document="[[document]]"></nuxeo-document-attachments>
    
    <nuxeo-se-document-relations document="[[document]]"></nuxeo-se-document-relations>
    
  </template>

  <script>
  Polymer({
    is: 'nuxeo-file-view-layout',
    behaviors: [Nuxeo.LayoutBehavior],
    properties: {
      /**
         * @doctype File
         */
      document: Object
    }
  });
  </script>

</dom-module>
```

El resultado final tendría el siguiente aspecto:

![Nnuxeo-related-documents](/images/nuxeo-related-documents-744x440.png "nuxeo-related-documents")


##Obtener todas las relaciones

La operación *Document.GetLinkedDocuments* proporciona las relaciones de un documento en función de la dirección y el tipo de relación. Sin embargo necesitamos recuperar todas las relaciones del documento para poder mostrarlas. Por eso vamos a crear un **Automation Script**  llamado **getAllRelations** desde el menú **CONFIGURATION > Automation > Automation Scripting** de Nuxeo Studio

> Document.GetLinkedDocuments
> Get the relations for the input document. The ‘outgoing’ parameter ca be used to specify whether outgoing or incoming relations should be returned. Retuns a document list.

 
```html
function run(input, params) {

  var PREDICATES = 'predicates';
  var INVERSE_PREDICATES = 'inverse_predicates';  
    
  /**
  * Provide the  entries for the given predicates vocabulary
  * @param alias - User alias, e.g. Administrator  
  * @param predicatesName - Possible values:   'predicates' or 'inverse_predicates'
  * @return Entries for the given vocabulary
  */  
  function getPredicates(alias, predicatesName){

        var resultBlob, resultJson, resultStr;

      Auth.LoginAs(input, {'name': 'Administrator'});

      // Directory.Entries returns a `org.nuxeo.ecm.core.api.impl.blob.JSONBlob` 
      // This means it is a _java_ object. That contains (in this context) a _string_. 
      // So you must (1) Extract the string from the blob (2) Convert it to JSON. 
      // Notice that toString() will not work here, it calls the generic function. 
      // So, you must call `getString()` from the result, then `JSON.parse
      resultBlob = Directory.Entries(null, { "directoryName": predicatesName });
      
      Auth.Logout(input, {});
      
      // Get the content of the blob, as string
      resultStr = resultBlob.getString();
      
      // Parse it to get a JSON array
      resultJson = JSON.parse(resultStr);     
      
      return resultJson;
  }
  
  function getRelatedDocuments(predicates){    
    var i, j, k, predicate = null;
    var incoming = null;
    var incomings = [true, false];
    var numIncomings = incomings.length;    
    var numPredicates = predicates === null? 0 : predicates.length;
    var allRelDocs = [];
    var relDocs = [];
    var docs = [];
    var relDoc = null;
    
    Console.info("# Incomings : '" + numIncomings + "'  # Predicates: '" + numPredicates +"'");      
        
    for(i=0; i < numIncomings; i++){
      incoming = incomings[i];
      
      Console.info("incomings[" +i + "] : '" + incoming + "'");      
      
      for (j = 0; j < numPredicates; j++) {
        predicate = predicates[j];
                      
        // Operation Document.GetLinkedDocuments
        // 
        // Get the relations for the input document. 
        // The 'outgoing' parameter ca be used to specify whether outgoing 
        // or incoming relations should be returned. Retuns a document list.
        docs = Document.GetLinkedDocuments(input,
                                                {
                                                  "predicate": predicate.id,
                                                  "outgoing": incoming
                                                }
                                               );
        
        if(docs !== null && docs.length > 0){   

          relDocs = [];
          var numRelDocs = docs.length;
          
          Console.log(numRelDocs + " related docs FOUND for predicate (id): " + predicate.id );

          for(k=0; k<numRelDocs; k++){
            relDoc = {};
            relDoc.uid = docs[k].id;            
            relDoc.title = docs[k].title;            
            relDoc.path = docs[k].path;     
            relDoc.lastModified = Fn.calendar(docs[k].getProperty('dc:modified')).format("yyyy-MM-dd'T'HH:mm:ssZ");      
            
            relDoc.incoming = incoming;
            relDoc.relationId = predicate.id;            
            relDoc.relationLabel = predicate.label;     
            relDocs.push(relDoc);
          }
          
          allRelDocs = allRelDocs.concat(relDocs);                                  
        }
        else{
          Console.log("NO related docs for predicate (id): " + predicate.id );          
        }
        
      } //for i
    } //for j      
    
    return allRelDocs;
  }
  
  //===================================
  // Main
  //===================================
  
  var user = User.Get(input, {});

  //DocumentModelImpl(Administrator, path=null, title=Administrator)
  Console.log("Alias: " + user.title);  
  
  var predicates = getPredicates( user.title, PREDICATES);
  Console.log(predicates);
  Console.log(JSON.stringify(predicates) );
  var relDocuments = getRelatedDocuments(predicates);
  
  var numRelDocs = relDocuments === null? 0 : relDocuments.length; 
  Console.log("# Related documents: " + numRelDocs);              
  
  return relDocuments;
}
```
 

## Web element nuxeo-se-document-relations
Este es el código fuente de nuestro web element nuxeo-se-document-relations. Utiliza la operación javascript.getAllRelations que hemos creado para recuperar todas las relacioes del documento. Además hace referencia a dos web elements auxiliares que se encargan del borrado y la creacción de nuevas relaciones.

```html 
<!--
`nuxeo-se-document-relations`
@group Nuxeo UI
@element nuxeo-se-document-relations
-->
<link rel="import" href="nuxeo-se-add-relation-dialog.html">
<link rel="import" href="nuxeo-se-remove-relation-button.html">

<dom-module id="nuxeo-se-document-relations">
  <template>
    <style>
      *[role=widget] {
        padding: 5px;
      }
      
      .container {
        display: flex;
        flex-direction: column;
        justify-content: flex-end;         
      }
      
      .left-box {
        margin-left: auto;
        margin-bottom: 2px;          
      }      
    </style>


    <nuxeo-connection id="nxcon"></nuxeo-connection>
    
    <nuxeo-operation auto
                     id="opGetAllRelations"
                     op="javascript.getAllRelations"
                     input="[[document]]"
                     params="{}"
                     on-response="_handleRelationsRecovered">
    </nuxeo-operation>
                                         
    <h3>[[i18n('label.relation.relatedDocuments')]]</h3>
    
    <div class="container">
      
      <iron-icon id="addRelationIcon" icon="icons:add-circle-outline" class="left-box" on-tap="_toggleDialog"></iron-icon>
    
      <nuxeo-data-table id="relationsTable" items="[[_relations]]" emptyLabel="[[i18n('label.relation.noRelatedDocuments')]]">
        <nuxeo-data-table-column name="Icon" flex="2" width="50px">
          <template>
            <a href="[[_getDocURL(item.path)]]">
              <iron-icon src="[[_thumbnail(item)]]"/>
            </a>
          </template>
        </nuxeo-data-table-column>

        <nuxeo-data-table-column name="[[i18n('title')]]" flex="54">
          <template>
            <a href="[[_getDocURL(item.path)]]">[[item.title]]</a>
          </template>
        </nuxeo-data-table-column>

        <nuxeo-data-table-column name="[[i18n('label.dublincore.lastModified')]]" flex="28">
          <template>[[_getUserFriendlyDate(item.lastModified)]]</template>
        </nuxeo-data-table-column>

        <nuxeo-data-table-column name="[[i18n('label.relation.type')]]" flex="10" >
          <template>
            [[i18n(item.relationLabel)]]
          </template>
        </nuxeo-data-table-column>          
        
        <nuxeo-data-table-column name="[[i18n('label.relation.direction')]]" flex="2" width="50px">
          <template>
            <template is="dom-if" if="[[item.incoming]]">
	            <iron-icon id="[[item.uid]]" icon="icons:arrow-back"></iron-icon>
            </template>            
            <template is="dom-if" if="[[!item.incoming]]">
	            <iron-icon id="[[item.uid]]" icon="icons:arrow-forward"></iron-icon>            
            </template>            
          </template>
        </nuxeo-data-table-column>                  
        
        <nuxeo-data-table-column name="[[i18n('label.relation.removeRelation')]]" flex="2" width="50px">
          <template>
            <nuxeo-se-remove-relation-button id="removeRelation[[index]]"
                                             from-doc-id="[[document.path]]"
                                             to-doc-id="[[item.uid]]" 
                                             incoming="[[item.incoming]]" 
                                             predicate="[[item.relationId]]">
            </nuxeo-se-remove-relation-button>
          </template>
        </nuxeo-data-table-column>      
      </nuxeo-data-table>    

    </div>

 
    <!-- 
			Dialog: Add relation 
		-->       
    <nuxeo-se-add-relation-dialog id="dialog" document="[[document]]"></nuxeo-se-add-relation-dialog>    
    
  </template>

  <script>
    Polymer({
      is: 'nuxeo-se-document-relations',
      behaviors: [Nuxeo.LayoutBehavior],
      properties: {
        /**
         * @schema dublincore
         */
        document: {
          type: Object
        }, 
                              
        _relations: {
          type: Array
        },                
      },
      
      ready: function(){
        this.$.dialog.addEventListener('relation-added', event => this._handleRelationsChanged(event));
        

        
        this.addEventListener('dom-change', function(event){
	    
          var removeRelButtons = Polymer.dom(this.root).querySelectorAll('nuxeo-se-remove-relation-button');
          console.log(removeRelButtons);
          
          if(removeRelButtons != null && removeRelButtons != undefined && removeRelButtons.length > 0){
            var i=0;
            var numRelations = removeRelButtons.length;
            for(i=0; i < numRelations; i++){
              console.log("Relation event listener added for relation: " + i);
              removeRelButtons[i].addEventListener('relation-removed', event => this._handleRelationsChanged(event));
            }
          }
        });        
      },      
      
			_thumbnail: function (doc) {
        if (doc && doc.uid) {
          var baseUrl = this.$.nxcon.url;
          return baseUrl + '/api/v1/id/' + doc.uid + '/@rendition/thumbnail';
        }
      },      
      
      /**
	    * Convert UTC date to mm/dd/yyyy
  	  * SEE: https://stackoverflow.com/questions/13874480/converting-utc-date-to-mm-dd-yyyy
    	*/
      _getUserFriendlyDate: function(utcDate){
        var d2='Not set';

        if(undefined !== utcDate && utcDate !== null){
          d2 = utcDate.substring(8,10) +
            '/' +
            utcDate.substring(5,7) +
            '/' +
            utcDate.substring(0,4);        
        }


        return d2;
      },      
      
      _getDocURL: function(path){
        return "/nuxeo/ui/#!/browse" + path;
      },      
                  
      _toggleDialog: function() {
        this.$.dialog.toggle();
      },      

      _handleRelationsRecovered: function(e){
        console.log("_handleRelationsRecovered: ");
        console.log(e.detail.response);
        
        this.set('_relations', e.detail.response.value);            
      },
                  
      _handleRelationsChanged: function(e){
        console.log("_handleRelationsChanged: ");
        console.log(e.detail);   
        
        this.$.opGetAllRelations.execute();        
      }
    });
  </script>
</dom-module>
```

## Añadir relación
El web element **nuxeo-se-add-relation-dialog** muestra un pop-up para añadir una nueva relación espeficando la dirección y el tipo de relación. Para crear la relación utiliza la operation Document.AddRelation. Al finalizar la creación de la relación  lanzaremos un **evento personalizado** llamado **relation-added** para notificar que la relación ha sido creada.

![nuxeo-add-related-document](/images/nuxeo-add-related-document-744x584.png  "nuxeo-add-related-document")


> La operación Document.AddRelation utiliza lógica negativa en el parámetro outgoing, lo que hace más dificil  seguir la lógica.

Este es el código fuente del web element **nuxeo-se-add-relation-dialog**:

```html
<!--
@license
(C) Copyright Nuxeo Corp. (http://nuxeo.com/)
Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at
    http://www.apache.org/licenses/LICENSE-2.0
Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
-->
<!--
`nuxeo-se-add-relation-dialog`
@group Nuxeo UI
@element nuxeo-se-add-relation-dialog
-->
<dom-module id="nuxeo-se-add-relation-dialog">
  <template>
    <style>
      *[role=widget] {
        padding: 5px;
      }
    </style>

    <nuxeo-operation id="opCreateRelation"
                     op="Document.AddRelation"                      
                     input="[[document.uid]]" 
                     params='{"predicate": "[[predicate]]", "object": "[[toDoc.uid]]", "outgoing": "[[_getDirectionAsBool(direction)]]"}'
                     on-response="_handleRelationCreated">
    </nuxeo-operation>
                    
    <!-- 
			Dialog: Add relation 
		-->
    <nuxeo-dialog id="dialog" with-backdrop>
      <h2>[[i18n('label.relation.addRelation')]]</h2>


      <div role="widget">
        <nuxeo-document-suggestion label="[[i18n('label.relation.pickDocument')]]"
                                   value="{{path}}" 
                                   min-chars="0"
                                   selected-item="{{toDoc}}" >
        </nuxeo-document-suggestion>
      </div>
      
      <div role="widget">
        <label id="label-direction">[[i18n('label.relation.direction')]]:</label>
        <paper-radio-group aria-labelledby="label-direction" selected="{{direction}}">
          <paper-radio-button name="outgoing">[[i18n('label.relation.outgoing')]]</paper-radio-button>
          <paper-radio-button name="incoming">[[i18n('label.relation.incoming')]]</paper-radio-button>
        </paper-radio-group>          
      </div>
			

      <nuxeo-directory-suggestion directory-name="[[_getPredicatesDirectory(direction)]]"
                                  label="[[i18n('label.relation.type')]]"
                                  min-chars="0" 
                                  required="true"
                                  value="{{predicate}}">
      </nuxeo-directory-suggestion>        
      
      <div class="buttons">
        <paper-button dialog-dismiss>[[i18n('command.cancel')]]</paper-button>
        <paper-button id="addRelationButton" class="primary" on-tap="_addRelation">[[i18n('accept')]]</paper-button>
      </div>
    </nuxeo-dialog>      
    
  </template>

  <script>
    Polymer({
      is: 'nuxeo-se-add-relation-dialog',
      behaviors: [Nuxeo.LayoutBehavior],
      properties: {
        /**
         * @schema dublincore
         */
        document: {
          type: Object
        },
        
        path: {
          type: String,
          value: '/'
        },
        
        direction: {
          type: String,
          value: 'incoming',
          observer: '_resetSelectedPredicate'
        }
      },      
                 
      toggle: function() {
        this.$.dialog.toggle();
      },  
      
      _getPredicatesDirectory: function(direct){
        console.log('_getPredicatesDirectory(' + direct +')');
        return (direct == 'incoming') ? 'inverse_predicates' : 'predicates';
      },
      
      _getDirectionAsBool: function(direct){
        // SEE: http://explorer.nuxeo.com/nuxeo/site/distribution/Nuxeo%20DM-8.1/viewOperation/Document.AddRelation
        //
        // The 'outgoing' flag indicates the direction of the relation - 
        // the default is false which means the relation will go from the 
        //input object to the object specified as 'object' parameter. 
        return (direct == 'incoming') ? false : true;
      },      
      
      _resetSelectedPredicate: function(){
        this.set("predicate", "");
      },
      
      _addRelation: function(){        
        console.log("Creating relation ("+ this.predicate+") with doc: " + this.path);
              
        this.$.opCreateRelation.execute();
      },    
      
      _handleRelationCreated: function(e){
        console.log("Relation successfully created");
        
        this.dispatchEvent(new CustomEvent('relation-added', {detail: {document: e.detail.response}}));
                
        this.toggle();
      }
    });
  </script>
</dom-module>
```

## Borrar relación
Por último necesitamos crear un web element encargado de borrar relaciones, **nuxeo-se-remove-relation-button**. Para el borrado utlizaremos la operación Relations.DeleteRelation. Al finalizar el borrado de la relación lanzaremos un **evento personalizado** llamado **relation-removed** para notificar que la relación ha sido eliminada.

```html
<!--
@license
(C) Copyright Nuxeo Corp. (http://nuxeo.com/)
Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at
    http://www.apache.org/licenses/LICENSE-2.0
Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
-->
<!--
`nuxeo-se-remove-relation-button`
@group Nuxeo UI
@element nuxeo-se-remove-relation-button
-->
<dom-module id="nuxeo-se-remove-relation-button">
  <template>
    <style>
      *[role=widget] {
        padding: 5px;
      }
    </style>
    
    <nuxeo-operation id="opDeleteRelation"
                     op="Relations.DeleteRelation"                      
                     input="[[fromDocId]]" 
                     params='{"predicate": "[[predicate]]", "object": "[[toDocId]]", "outgoing": "[[incoming]]"}'
                     on-response="_handleRelationRemoved">
    </nuxeo-operation>    

    <iron-icon icon="icons:remove-circle-outline" 
               on-tap="_removeRelation"></iron-icon>    

  </template>

  <script>
    Polymer({
      is: 'nuxeo-se-remove-relation-button',
      behaviors: [Nuxeo.LayoutBehavior],
      properties: {
        /**
         * @schema dublincore
         */
        document: {
          type: Object
        },

        fromDocId: {
          type: String
        },
        
        toDocId: {
          type: String
        },
        
        predicate: {
          type: String
        },
        
        incoming: {
          type: Boolean
        }
      },
      
      _removeRelation: function(e){
        console.log("_removeRelation: ");               
        
        this.$.opDeleteRelation.execute();
      },
      
      _handleRelationRemoved: function(event){
        console.log("_handleRelationRemoved fire event 'relation-removed'");
        
        this.dispatchEvent(new CustomEvent('relation-removed', {detail: {document: event.detail.response}}));
      }
    });
  </script>
</dom-module>
```

## Traducciones
Nuxeo proporciona soporte multi-idioma. Tan sólo debemos crear el fichero **messages-es-ES.json** en el menú **UI > Translations** de View Designer para que nuestro web element se traduzca al español.

```json
{
  "label.relation.relatedDocuments": "Documentos relacionados",
  "label.relation.noRelatedDocuments": "Sin documentos relacionados",  
  "label.relation.removeRelation": "Borrar relación",  
  "label.relation.addRelation": "Añadir relación", 
  "label.relation.direction": "Dirección",
  "label.relation.type": "Tipo Rel.",  
  "label.relation.outgoing": "Saliente",  
  "label.relation.incoming": "Entrante",  
  "label.relation.pickDocument": "Elige un documento",       
  
  "label.dublincore.lastModified": "Últ. modificación",
  
	"label.relation.predicate.inverse.References": "Referencia",
	"label.relation.predicate.inverse.IsBasedOn": "Basado en",
	"label.relation.predicate.inverse.Replaces": "Reemplaza",
	"label.relation.predicate.inverse.Requires": "Requiere",
	"label.relation.predicate.inverse.ConformsTo": "De acuerdo a",

	"label.relation.predicate.References": "Referencia",
	"label.relation.predicate.IsBasedOn": "Basado en",
	"label.relation.predicate.Replaces": "Reemplaza",
	"label.relation.predicate.Requires": "Requiere",
	"label.relation.predicate.ConformsTo": "De acuerdo a"
}
```
 
> Si quieres saber más sobre como traducir Nuxeo puedes consultar el artículo: [Traducción de la interface de usuario de Nuxeo](./traduccion-interface-usuario-nuxeo)

## Traducciones de vocabularios
Los tipos de relaciones estar definidos en dos vocabularios: **predicates** y **inverse_predicates**.

Los vocabularios representan una excepción al sistema general del soporte multi-idioma de nuxeo. Por ello debemos crear un fichero de texto denominado messages_es_ES.properties que adjuntaremos en el menú CONFIGURATION > Resources > I18n Files de Nuxeo Studio y cuyo contenido es el siguiente:

```sh
label.relation.predicate.inverse.References=Referencia
label.relation.predicate.inverse.IsBasedOn=Basado en
label.relation.predicate.inverse.Replaces=Reemplaza
label.relation.predicate.inverse.Requires=Requiere
label.relation.predicate.inverse.ConformsTo=De acuerdo a

label.relation.predicate.References=Referencia
label.relation.predicate.IsBasedOn=Basado en
label.relation.predicate.Replaces=Reemplaza
label.relation.predicate.Requires=Requiere
label.relation.predicate.ConformsTo=De acuerdo a
```

> Si quieres saber más sobre como traducir vocabularios en Nuxeo puedes consultar el artículo: [Nuxeo: Soporte multi-idioma en vocabularios](./nuxeo-soporte-multiidioma-en-vocabularios)