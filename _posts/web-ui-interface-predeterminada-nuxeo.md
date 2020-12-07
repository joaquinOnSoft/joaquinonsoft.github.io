# Usar Web UI como la interface predeterminada en Nuxeo

A partir de la versión LTS 2016 (8.10)  o posterior de Nuxeo podemos tener instalados los plugins **nuxeo-jsf-ui** y **nuxeo-web-ui**.  En este caso, tras la autenticación se nos mostrará la interface JSF. Por lo que si queremos que  **Web UI sea la interface predeterminada** debemos  crear una extensión XML .

## Crear un extensión XML
Para crear una contribución XML debemos seleccionar la opción **Advanced Settings > XML Extension** en **Nuxeo Studio**.  En primer lugar debemos pulsar el botón *Create*. Despues se nos mostrará un pop-up en el que deberemos dar nombre a la nueva extensión XML. En nuestro ejemplo la llamaremos *WebUIByDefault*.

Nuxeo Studio: Advanced setting > XML Extensions - new XML contribution pop-up

A continuación debemos pegar el siguiente XML en la pantalla de nuestra extensión y hacer clic en el botón Save.

```xml
<extension target="org.nuxeo.ecm.platform.ui.web.auth.service.PluggableAuthenticationService" point="loginScreen">
  <loginScreenConfig>
    <startupPages>
      <startupPage id="web" priority="1000" />
    </startupPages>
  </loginScreenConfig>
</extension>
```

Los valores predeterminados para el atributo **priority son 100 para JSF UI  y 10 para Web UI**.  La interface con el valor más alto del atributo priority es la que se utilizará.

![Nuxeo Studio: Advanced setting > XML Extensions - new XML contribution](/images/nuxeo-studio-new-xml-extensions-1200x885.png "Nuxeo Studio: Advanced setting > XML Extensions - new XML contribution")

## Enlaces permanentes a los documentos
Los enlaces permanentes a los documento que genera el servidor (para e-mails…) redirigen a JSF. Para generar enlaces permanentes a IU web UI, debemos crear un nueva extensión XML con el siguiente código:

```xml 
<extension target="org.nuxeo.ecm.platform.url.service.DocumentViewCodecService" point="codecs">
  <documentViewCodec name="notificationDocId"
                     enabled="true"
                     prefix="doc"
                     priority="1000"
                     class="org.nuxeo.web.ui.url.codec.WebNotificationDocumentIdCodec" />
</extension>
```