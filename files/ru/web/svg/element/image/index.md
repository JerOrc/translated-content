---
title: <image>
slug: Web/SVG/Element/image
tags:
  - NeedsUpdate
  - SVG
translation_of: Web/SVG/Element/image
original_slug: Web/SVG/Элемент/image
---
<div>{{SVGRef}}</div>

<p>Элемент &lt;image&gt; позволяет включить растровые изображения в SVG документ.</p>

<h2 id="Usage_context">Usage context</h2>

<p>{{svginfo}}</p>

<h2 id="Атрибуты">Атрибуты</h2>

<h3 id="Глобальные_атрибуты">Глобальные атрибуты</h3>

<ul>
 <li><a href="/en-US/docs/Web/SVG/Attribute#ConditionalProccessing">Conditional processing attributes</a> »</li>
 <li><a href="/en-US/docs/Web/SVG/Attribute#Core">Core attributes</a> »</li>
 <li><a href="/en-US/docs/Web/SVG/Attribute#GraphicalEvent">Graphical event attributes</a> »</li>
 <li><a href="/en-US/docs/Web/SVG/Attribute#XLink">Xlink attributes</a> »</li>
 <li><a href="/en-US/docs/Web/SVG/Attribute#Presentation">Presentation attributes</a> »</li>
 <li>{{SVGAttr("class")}}</li>
 <li>{{SVGAttr("style")}}</li>
 <li>{{SVGAttr("externalResourcesRequired")}}</li>
 <li>{{SVGAttr("transform")}}</li>
</ul>

<h3 id="Специфичные_атрибуты">Специфичные атрибуты</h3>

<ul>
 <li>{{SVGAttr("x")}}</li>
 <li>{{SVGAttr("y")}}</li>
 <li>{{SVGAttr("width")}}</li>
 <li>{{SVGAttr("height")}}</li>
 <li>{{SVGAttr("xlink:href")}}</li>
 <li>{{SVGAttr("preserveAspectRatio")}}</li>
</ul>

<h2 id="DOM_Interface">DOM Interface</h2>

<p>This element implements the <code><a href="/en-US/docs/Web/API/SVGImageElement">SVGImageElement</a></code> interface.</p>

<h2 id="Пример">Пример</h2>

<p>Базовый рендеринг PNG изображения в качестве объекта SVG:</p>

<h3 id="SVG">SVG</h3>

<pre class="brush: html">&lt;svg width="200" height="200"
  xmlns="http://www.w3.org/2000/svg"&gt;
  &lt;image href="https://mdn.mozillademos.org/files/6457/mdn_logo_only_color.png" height="200" width="200"/&gt;
&lt;/svg&gt;
</pre>

<h3 id="Результат">Результат</h3>

<p>{{EmbedLiveSample("Пример", 250, 260)}}</p>

<h2 id="Спецификации">Спецификации</h2>

{{Specifications}}

<h2 id="Browser_compatibility">Browser compatibility</h2>

<p>{{Compat}}</p>