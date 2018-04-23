{% assign id = {{include.id}} %}
<p><b>Intermediate Differential View ({{include.type}}-{{include.id}} Profile + {{site.data.structuredefinitions.[id].basename}} Profile)</b></p>
<div id="all-tbl-diff2-inner">
  {% include {{include.type}}-cn-plus-uscore-diff.xhtml %}  
</div>
