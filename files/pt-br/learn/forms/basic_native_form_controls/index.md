---
title: Os widgets nativos
slug: Learn/Forms/Basic_native_form_controls
tags:
  - Aprender
  - Contrôles
  - Exemplos
  - Guía
  - HTML
  - Iniciantes
  - Intermediário
  - Web
translation_of: Learn/Forms/Basic_native_form_controls
original_slug: Web/Guide/HTML/Forms/Os_widgets_nativos
---
<div>{{LearnSidebar}}</div>

<div>{{PreviousMenuNext("Learn/HTML/Forms/How_to_structure_an_HTML_form", "Learn/HTML/Forms/Sending_and_retrieving_form_data", "Learn/HTML/Forms")}}</div>

<p class="summary">Veremos agora, detalhadamente, a funcionalidade dos diferentes widgets dos formulários, observando as opções disponíveis para coletar diferentes tipos de dados. Este guia é um tanto exaustivo, cobrindo todos os widgets de formulários nativos disponíveis.</p>

<table class="learn-box standard-table">
 <tbody>
  <tr>
   <th scope="row">Pré-requisitos:</th>
   <td>Alfabetização básica em informática e um <a href="/en-US/docs/Learn/HTML/Introduction_to_HTML">entendimento básico de HTML.</a></td>
  </tr>
  <tr>
   <th scope="row">Objetivo:</th>
   <td>Entender quais são os widgets nativos de formulário disponiveis nos navegadores para coletar dados, e como implementa-los usando HTML.</td>
  </tr>
 </tbody>
</table>

<p><span class="tlid-translation translation" lang="pt"><span title="">Aqui vamos nos concentrar nos widgets de formulário baseados em navegador, mas como os formulários HTML são muito limitados e a qualidade da implementação pode ser muito diferente de navegador para navegador, os desenvolvedores da Web às vezes criam seus próprios widgets de formulários </span></span>— Veja <a href="/en-US/docs/HTML/Forms/How_to_build_custom_form_widgets" title="/en-US/docs/HTML/Forms/How_to_build_custom_form_widgets">How to build custom form widgets</a> <span class="tlid-translation translation" lang="pt"><span title="">posteriormente neste mesmo módulo para obter mais idéias sobre isso.</span></span></p>

<div class="note">
<p><strong>Nota</strong>: A maioria dos recursos discutidos neste artigo possui <span class="tlid-translation translation" lang="pt"><span title="">amplo suporte nos navegadores</span></span>; destacaremos as exceções existentes. Se você quiser mais detalhes, consulte nosso artigo <a href="https://developer.mozilla.org/en-US/docs/Web/HTML/Element#Forms">referência aos elementos de formulário HTML</a>, <span class="tlid-translation translation" lang="pt"><span title="">e, em particular, nossa extensa referência de </span></span><a href="/en-US/docs/Web/HTML/Element/input">tipos &lt;input&gt;</a>.</p>
</div>

<h2 id="Atributos_comuns">Atributos comuns</h2>

<p><span class="tlid-translation translation" lang="pt"><span title="">Muitos dos elementos usados para definir widgets de formulário têm seus próprios atributos</span></span>. Entretanto, <span class="tlid-translation translation" lang="pt"><span title="">há um conjunto de atributos comuns a todos os elementos do formulário</span></span>, os quais permitem certo controle sobre os widgets. <span class="tlid-translation translation" lang="pt"><span title="">Aqui está uma lista desses atributos comuns</span></span>:</p>

<table>
 <thead>
  <tr>
   <th scope="col">Nome do atributo</th>
   <th scope="col">Valor padrão</th>
   <th scope="col">Descrição</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td><code>autofocus</code></td>
   <td>(falso)</td>
   <td>Este é um atributo booleano que permite especificar automaticamente qual elemento deverá ter o foco quando a página carregar, a menos que o usuário a substitua, por exemplo <span class="tlid-translation translation" lang="pt"><span title=""> digitando sobre um controle diferente</span></span>. <span class="tlid-translation translation" lang="pt"><span title="">Somente um elemento associado ao formulário em um documento pode ter esse atributo especificado</span></span>.</td>
  </tr>
  <tr>
   <td><code>disabled</code></td>
   <td>(<em>falso</em>)</td>
   <td>Este é um atributo booleano que indica <span class="tlid-translation translation" lang="pt"><span title="">que o usuário não pode interagir com este elemento</span></span>. Se este atributo não estiver especificado, o elemento, então, herda a configuração do elemento que o contém, por exemplo, {{HTMLElement("fieldset")}}; se o elemento que o contém não possuir o atributo <code>disabled</code>, então o elemento é ativado.</td>
  </tr>
  <tr>
   <td><code>form</code></td>
   <td></td>
   <td><span class="tlid-translation translation" lang="pt"><span title="">O elemento do formulário ao qual o widget está associado</span></span>. O valor do atributo deve ser o atributo <code>id</code> de um {{HTMLElement("form")}} no mesmo documento. <span class="tlid-translation translation" lang="pt"><span title="">Em teoria, permite colocar um widget fora de um elemento</span></span> {{HTMLElement("form")}}. <span class="tlid-translation translation" lang="pt"><span title="">Na prática, no entanto, não há navegador que suporte esse recurso.</span></span></td>
  </tr>
  <tr>
   <td><code>name</code></td>
   <td></td>
   <td><span class="tlid-translation translation" lang="pt"><span title="">O nome do elemento. Este atributo é</span><span title=""> enviado com os dados do formulário.</span></span></td>
  </tr>
  <tr>
   <td><code>value</code></td>
   <td></td>
   <td>O Valor inicial do elemento.</td>
  </tr>
 </tbody>
</table>

<h2 id="Text_input_fields">Text input fields</h2>

<p>Text {{htmlelement("input")}} fields are the most basic form widgets. They are a very convenient way to let the user enter any kind of data. However, some text fields can be specialized to achieve particular needs. We've already seen a few simple examples</p>

<div class="note">
<p><strong>Note</strong>: HTML form text fields are simple plain text input controls. This means that you cannot use them to perform <a href="/en-US/docs/Rich-Text_Editing_in_Mozilla" title="/en-US/docs/Rich-Text_Editing_in_Mozilla">rich editing</a> (bold, italic, etc.). All rich text editors you'll encounter out there are custom widgets created with HTML, CSS, and JavaScript.</p>
</div>

<p>All text fields share some common behaviors:</p>

<ul>
 <li>They can be marked as {{htmlattrxref("readonly","input")}} (the user cannot modify the input value) or even {{htmlattrxref("disabled","input")}} (the input value is never sent with the rest of the form data).</li>
 <li>They can have a {{htmlattrxref("placeholder","input")}}; this is text that appears inside the text input box that describes the purpose of the box briefly.</li>
 <li>They can be constrained in {{htmlattrxref("size","input")}} (the physical size of the box) and <a href="/en-US/docs/HTML/Element/input#attr-maxlength" title="/en-US/docs/HTML/Element/input#attr-maxlength">length</a> (the maximum number of characters that can be entered into the box).</li>
 <li>They can benefit from <a href="/en-US/docs/HTML/Element/input#attr-spellcheck" title="/en-US/docs/HTML/Element/input#attr-spellcheck">spell checking</a>, if the browser supports it.</li>
</ul>

<div class="note">
<p><strong>Note</strong>: The {{htmlelement("input")}} element is special because it can be almost anything. By simply setting its <code>type</code> attribute, it can change radically, and it is used for creating most types of form widget including single line text fields, controls without text input, time and date controls, and buttons. However, there are some exceptions, like {{htmlelement("textarea")}} for multi-line inputs. Take careful note of these as you read the article.</p>
</div>

<h3 id="Single_line_text_fields">Single line text fields</h3>

<p>A single line text field is created using an {{HTMLElement("input")}} element whose {{htmlattrxref("type","input")}} attribute value is set to <code>text</code> (also, if you don't provide the {{htmlattrxref("type","input")}} attribute, <code>text</code> is the default value). The value <code>text</code> for this attribute is also the fallback value if the value you specify for the {{htmlattrxref("type","input")}} attribute is unknown by the browser (for example if you specify <code>type="date"</code> and the browser doesn't support native date pickers).</p>

<div class="note">
<p><strong>Note</strong>: You can find examples of all the single line text field types on GitHub at <a href="https://github.com/mdn/learning-area/blob/master/html/forms/native-form-widgets/single-line-text-fields.html">single-line-text-fields.html</a> (<a href="https://mdn.github.io/learning-area/html/forms/native-form-widgets/single-line-text-fields.html">see it live also</a>).</p>
</div>

<p>Here is a basic single line text field example:</p>

<pre class="brush: html">&lt;input type="text" id="comment" name="comment" value="I'm a text field"&gt;</pre>

<p>Single line text fields have only one true constraint: if you type text with line breaks, the browser removes those line breaks before sending the data.</p>

<p><img alt="Screenshots of single line text fields on several platforms." src="/files/4273/all-single-line-text-field.png" style="height: 235px; width: 655px;"></p>

<p>HTML5 enhances the basic single line text field by adding special values for the {{htmlattrxref("type","input")}} attribute. Those values still turn an {{HTMLElement("input")}} element into a single line text field but they add a few extra constraints and features to the field.</p>

<h4 id="E-mail_address_field">E-mail address field</h4>

<p>This type of field is set with the value <code>email</code> for the {{htmlattrxref("type","input")}} attribute:</p>

<pre class="brush: html">&lt;input type="email" id="email" name="email" multiple&gt;</pre>

<p>When this <code>type</code> is used, the user is required to type a valid e-mail address into the field; any other content causes the browser to display an error when the form is submitted. Note that this is client-side error validation, performed by the browser:</p>

<p><img alt="An invalid email input showing the message Please enter an email address." src="https://mdn.mozillademos.org/files/14781/email-invalid.png" style="border-style: solid; border-width: 1px; display: block; margin: 0px auto;"></p>

<p>It's also possible to let the user type several e-mail addresses into the same input (separated by commas) by including the {{htmlattrxref("multiple","input")}} attribute.</p>

<p>On some devices (especially on mobile), a different virtual keypad might be presented that is more suitable for entering email addresses.</p>

<div class="note">
<p><strong>Note</strong>: You can find out more about form validation in the article <a href="/en-US/docs/Learn/HTML/Forms/Form_validation">Form data validation</a>.</p>
</div>

<h4 id="Password_field">Password field</h4>

<p>This type of field is set using the value <code>password</code> for the {{htmlattrxref("type","input")}} attribute:</p>

<pre class="brush: html">&lt;input type="password" id="pwd" name="pwd"&gt;</pre>

<p>It doesn't add any special constraints to the entered text, but it does obscure the value entered into the field (e.g. with dots or asterisks) so it can't be read by others.</p>

<p>Keep in mind this is just a user interface feature; unless you submit your form securely, it will get sent in plain text, which is bad for security — a malicious party could intercept your data and steal passwords, credit card details, or whatever else you've submitted. The best way to protect users from this is to host any pages involving forms over a secure connection (i.e. at an https:// ... address), so the data is encrypted before it is sent.</p>

<p>Modern browsers recognize the security implications of sending form data over an insecure connection, and have implemented warnings to deter users from using insecure forms. For more information on what Firefox implements, see <a href="/en-US/docs/Web/Security/Insecure_passwords">Insecure passwords</a>.</p>

<h4 id="Search_field">Search field</h4>

<p>This type of field is set by using the value <code>search</code> for the {{htmlattrxref("type","input")}} attribute:</p>

<pre class="brush: html">&lt;input type="search" id="search" name="search"&gt;</pre>

<p>The main difference between a text field and a search field is how the browser styles it — often, search fields are rendered with rounded corners, and/or given an "x" to press to clear the entered value. However, there is another added feature worth noting: their values can be automatically saved to be auto completed across multiple pages on the same site.</p>

<p><img alt="Screenshots of search fields on several platforms." src="/files/4269/all-search-field.png" style="height: 235px; width: 655px;"></p>

<h4 id="Phone_number_field">Phone number field</h4>

<p>This type of field is set using <code>tel</code> as the value of the {{htmlattrxref("type","input")}} attribute:</p>

<pre class="brush: html">&lt;input type="tel" id="tel" name="tel"&gt;</pre>

<p>Due to the wide variety of phone number formats around the world, this type of field does not enforce any constraints on the value entered by a user (this can include letters, etc.). This is primarily a semantic difference, although on some devices (especially on mobile), a different virtual keypad might be presented that is more suitable for entering phone numbers.</p>

<h4 id="URL_field">URL field</h4>

<p>This type of field is set using the value <code>url</code> for the {{htmlattrxref("type","input")}} attribute:</p>

<pre class="brush: html">&lt;input type="url" id="url" name="url"&gt;</pre>

<p>It adds special validation constraints to the field, with the browser reporting an error if invalid URLs are entered.</p>

<div class="note"><strong>Note:</strong> Just because the URL is well-formed doesn't necessarily mean that it refers to a location that actually exists.</div>

<div class="note">
<p><strong>Note</strong>: Fields that have special constraints and are in error prevent the form from being sent; in addition, they can be styled so as to make the error clear. We will discuss this in detail in the article: <a href="/en-US/docs/HTML/Forms/Data_form_validation" title="/en-US/docs/HTML/Forms/Data_form_validation">Data form validation</a>.</p>
</div>

<h3 id="Multi-line_text_fields">Multi-line text fields</h3>

<p>A multi-line text field is specified using a {{HTMLElement("textarea")}} element, rather than using the {{HTMLElement("input")}} element.</p>

<pre class="brush: html">&lt;textarea cols="30" rows="10"&gt;&lt;/textarea&gt;</pre>

<p>The main difference between a textarea and a regular single line text field is that users are allowed to type text that includes hard line breaks (i.e. pressing return).</p>

<p><img alt="Screenshots of multi-lines text fields on several platforms." src="/files/4271/all-multi-lines-text-field.png" style="height: 330px; width: 745px;"></p>

<div class="note">
<p><strong>Note</strong>: You can find an example of a multi-line text field on GitHub at <a href="https://github.com/mdn/learning-area/blob/master/html/forms/native-form-widgets/multi-line-text-field.html">multi-line-text-field.html</a> (<a href="https://mdn.github.io/learning-area/html/forms/native-form-widgets/multi-line-text-field.html">see it live also</a>). Have a look at it, and notice how in most browsers, the text area is given a drag handle on the bottom right to allow the user to resize it. This resizing ability can be turned off by setting the text area's {{cssxref("resize")}} property to <code>none</code> using <a href="/en-US/docs/Learn/CSS">CSS</a>.</p>
</div>

<p>{{htmlelement("textarea")}} also accepts a few extra attributes to control its rendering across several lines (in addition to several others):</p>

<table>
 <caption>Attributes for the {{HTMLElement("textarea")}} element</caption>
 <thead>
  <tr>
   <th scope="col">Attribute name</th>
   <th scope="col">Default value</th>
   <th scope="col">Description</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td>{{htmlattrxref("cols","textarea")}}</td>
   <td><code>20</code></td>
   <td>The visible width of the text control, in average character widths.</td>
  </tr>
  <tr>
   <td>{{htmlattrxref("rows","textarea")}}</td>
   <td></td>
   <td>The number of visible text lines for the control.</td>
  </tr>
  <tr>
   <td>{{htmlattrxref("wrap","textarea")}}</td>
   <td><code>soft</code></td>
   <td>Indicates how the control wraps text. Possible values are: <code>hard</code> or <code>soft</code></td>
  </tr>
 </tbody>
</table>

<p>Note that the {{HTMLElement("textarea")}} element is written a bit differently from the {{HTMLElement("input")}} element. The {{HTMLElement("input")}} element is an empty element, which means that it cannot contain any child elements. On the other hand, the {{HTMLElement("textarea")}} element is a regular element that can contain text content children.</p>

<p>There are two key related points to note here:</p>

<ul>
 <li>If you want to define a default value for an {{HTMLElement("input")}} element, you have to use the <code>value</code> attribute; for a {{HTMLElement("textarea")}} element on the other hand you put the default text between the starting tag and the closing tag of the {{HTMLElement("textarea")}}.</li>
 <li>Because of its nature, the {{HTMLElement("textarea")}} element only accepts text content; this means that any HTML content put inside a {{HTMLElement("textarea")}} is rendered as if it was plain text content.</li>
</ul>

<h2 id="Drop-down_content">Drop-down content</h2>

<p>Drop-down widgets are a simple way to let users select one of many options without taking up much space in the user interface. HTML has two forms of drop down content: the <strong>select box</strong>, and <strong>autocomplete box</strong>. In both cases the interaction is the same — once the control is activated, the browser displays a list of values the user can select between.</p>

<div class="note">
<p>Note: You can find examples of all the drop-down box types on GitHub at <a href="https://github.com/mdn/learning-area/blob/master/html/forms/native-form-widgets/drop-down-content.html">drop-down-content.html</a> (<a href="https://mdn.github.io/learning-area/html/forms/native-form-widgets/drop-down-content.html">see it live also</a>).</p>
</div>

<h3 id="Select_box">Select box</h3>

<p>A select box is created with a {{HTMLElement("select")}} element with one or more {{HTMLElement("option")}} elements as its children, each of which specifies one of its possible values.</p>

<pre class="brush: html">&lt;select id="simple" name="simple"&gt;
  &lt;option&gt;Banana&lt;/option&gt;
  &lt;option&gt;Cherry&lt;/option&gt;
  &lt;option&gt;Lemon&lt;/option&gt;
&lt;/select&gt;</pre>

<p>If required, the default value for the select box can be set using the {{htmlattrxref("selected","option")}} attribute on the desired {{HTMLElement("option")}} element — this option is then preselected when the page loads. The {{HTMLElement("option")}} elements can also be nested inside {{HTMLElement("optgroup")}} elements to create visually associated groups of values:</p>

<pre class="brush: html">&lt;select id="groups" name="groups"&gt;
  &lt;optgroup label="fruits"&gt;
    &lt;option&gt;Banana&lt;/option&gt;
    &lt;option selected&gt;Cherry&lt;/option&gt;
    &lt;option&gt;Lemon&lt;/option&gt;
  &lt;/optgroup&gt;
  &lt;optgroup label="vegetables"&gt;
    &lt;option&gt;Carrot&lt;/option&gt;
    &lt;option&gt;Eggplant&lt;/option&gt;
    &lt;option&gt;Potato&lt;/option&gt;
  &lt;/optgroup&gt;
&lt;/select&gt;</pre>

<p><img alt="Screenshots of single line select box on several platforms." src="/files/4517/all-select.png" style="height: 636px; width: 887px;"></p>

<p>If an {{HTMLElement("option")}} element is set with a <code>value</code> attribute, that attribute's value is sent when the form is submitted. If the <code>value</code> attribute is omitted, the content of the {{HTMLElement("option")}} element is used as the select box's value.</p>

<p>On the {{HTMLElement("optgroup")}} element, the <code>label</code> attribute is displayed before the values, but even if it looks somewhat like an option, it is not selectable.</p>

<h3 id="Multiple_choice_select_box">Multiple choice select box</h3>

<p>By default, a select box only lets the user select a single value. By adding the {{htmlattrxref("multiple","select")}} attribute to the {{HTMLElement("select")}} element, you can allow users to select several values, by using the default mechanism provided by the operating system (e.g. holding down <kbd>Cmd</kbd>/<kbd>Ctrl</kbd> and clicking multiple values).</p>

<p>Note: In the case of multiple choice select boxes, the select box no longer displays the values as drop-down content — instead, they are all displayed at once in a list.</p>

<pre class="brush: html">&lt;select multiple id="multi" name="multi"&gt;
  &lt;option&gt;Banana&lt;/option&gt;
  &lt;option&gt;Cherry&lt;/option&gt;
  &lt;option&gt;Lemon&lt;/option&gt;
&lt;/select&gt;</pre>

<p><img alt="Screenshots of multi-lines select box on several platforms." src="/files/4559/all-multi-lines-select.png" style="height: 531px; width: 734px;"></p>

<div class="note"><strong>Note:</strong> All browsers that support the {{HTMLElement("select")}} element also support the {{htmlattrxref("multiple","select")}} attribute on it.</div>

<h3 id="Autocomplete_box">Autocomplete box</h3>

<p>You can provide suggested, automatically-completed values for form widgets using the {{HTMLElement("datalist")}} element with some child {{HTMLElement("option")}} elements to specify the values to display.</p>

<p>The data list is then bound to a text field (usually an <code>&lt;input&gt;</code> element) using the {{htmlattrxref("list","input")}} attribute.</p>

<p>Once a data list is affiliated with a form widget, its options are used to auto-complete text entered by the user; typically, this is presented to the user as a drop-down box listing possible matches for what they've typed into the input.</p>

<pre class="brush: html">&lt;label for="myFruit"&gt;What's your favorite fruit?&lt;/label&gt;
&lt;input type="text" name="myFruit" id="myFruit" list="mySuggestion"&gt;
&lt;datalist id="mySuggestion"&gt;
  &lt;option&gt;Apple&lt;/option&gt;
  &lt;option&gt;Banana&lt;/option&gt;
  &lt;option&gt;Blackberry&lt;/option&gt;
  &lt;option&gt;Blueberry&lt;/option&gt;
  &lt;option&gt;Lemon&lt;/option&gt;
  &lt;option&gt;Lychee&lt;/option&gt;
  &lt;option&gt;Peach&lt;/option&gt;
  &lt;option&gt;Pear&lt;/option&gt;
&lt;/datalist&gt;</pre>

<div class="note"><strong>Note:</strong> According to <a href="http://www.w3.org/TR/html5/common-input-element-attributes.html#attr-input-list" rel="external">the HTML specification</a>, the {{htmlattrxref("list","input")}} attribute and the {{HTMLElement("datalist")}} element can be used with any kind of widget requiring a user input. However, it is unclear how it should work with controls other than text (color or date for example), and different browsers behave differently from case to case. Because of that, be cautious using this feature with anything but text fields.</div>

<div><img alt="Screenshots of datalist on several platforms." src="/files/4593/all-datalist.png" style="height: 329px; width: 437px;"></div>



<h4 id="Datalist_support_and_fallbacks">Datalist support and fallbacks</h4>

<p>The  {{HTMLElement("datalist")}} element is a very recent addition to HTML forms, so browser support is a bit more limited than what we saw earlier. Most notably, it isn't supported in IE versions below 10, and Safari still doesn't support it at the time of writing.</p>

<p>To handle this, here is a little trick to provide a nice fallback for those browsers:</p>

<pre class="brush:html;">&lt;label for="myFruit"&gt;What is your favorite fruit? (With fallback)&lt;/label&gt;
&lt;input type="text" id="myFruit" name="fruit" list="fruitList"&gt;

&lt;datalist id="fruitList"&gt;
  &lt;label for="suggestion"&gt;or pick a fruit&lt;/label&gt;
  &lt;select id="suggestion" name="altFruit"&gt;
    &lt;option&gt;Apple&lt;/option&gt;
    &lt;option&gt;Banana&lt;/option&gt;
    &lt;option&gt;Blackberry&lt;/option&gt;
    &lt;option&gt;Blueberry&lt;/option&gt;
    &lt;option&gt;Lemon&lt;/option&gt;
    &lt;option&gt;Lychee&lt;/option&gt;
    &lt;option&gt;Peach&lt;/option&gt;
    &lt;option&gt;Pear&lt;/option&gt;
  &lt;/select&gt;
&lt;/datalist&gt;
</pre>

<p>Browsers that support the {{HTMLElement("datalist")}} element will ignore all the elements that are not {{HTMLElement("option")}} elements and will work as expected. On the other hand, browsers that do not support the {{HTMLElement("datalist")}} element will display the label and the select box. Of course, there are other ways to handle the lack of support for the {{HTMLElement("datalist")}} element, but this is the simplest (others tend to require JavaScript).</p>

<table>
 <tbody>
  <tr>
   <th scope="row">Safari 6</th>
   <td><img alt="Screenshot of the datalist element fallback with Safari on Mac OS" src="/files/4583/datalist-safari.png" style="height: 32px; width: 495px;"></td>
  </tr>
  <tr>
   <th scope="row">Firefox 18</th>
   <td><img alt="Screenshot of the datalist element with Firefox on Mac OS" src="/files/4581/datalist-firefox-macos.png" style="height: 102px; width: 353px;"></td>
  </tr>
 </tbody>
</table>

<h2 id="Checkable_items">Checkable items</h2>

<p>Checkable items are widgets whose state you can change by clicking on them. There are two kinds of checkable item: the check box and the radio button. Both use the {{htmlattrxref("checked","input")}} attribute to indicate whether the widget is checked by default or not.</p>

<p>It's worth noting that these widgets do not behave exactly like other form widgets. For most form widgets, once the form is submitted all widgets that have a {{htmlattrxref("name","input")}} attribute are sent, even if no value has been filled out. In the case of checkable items, their values are sent only if they are checked. If they are not checked, nothing is sent, not even their name.</p>

<div class="note">
<p><strong>Note</strong>: You can find the examples from this section on GitHub as <a href="https://github.com/mdn/learning-area/blob/master/html/forms/native-form-widgets/checkable-items.html">checkable-items.html</a> (<a href="https://mdn.github.io/learning-area/html/forms/native-form-widgets/checkable-items.html">see it live also</a>).</p>
</div>

<p>For maximum usability/accessibility, you are advised to surround each list of related items in a {{htmlelement("fieldset")}}, with a {{htmlelement("legend")}} providing an overall description of the list.  Each individual pair of {{htmlelement("label")}}/{{htmlelement("input")}} elements should be contained in its own list item (or similar). This is shown in the examples. </p>

<p>You also need to provide values for these kinds of inputs inside the <code>value</code> attribute if you want them to be meaningful — if no value is provided, check boxes and radio buttons are given a value of <code>on</code>.</p>

<h3 id="Check_box">Check box</h3>

<p>A check box is created using the {{HTMLElement("input")}} element with its {{htmlattrxref("type","input")}} attribute set to the value <code>checkbox</code>.</p>

<pre class="brush: html">&lt;input type="checkbox" checked id="carrots" name="carrots" value="carrots"&gt;
</pre>

<p>Including the <code>checked</code> attribute makes the checkbox checked automatically when the page loads.</p>

<p><img alt="Screenshots of check boxes on several platforms." src="/files/4595/all-checkbox.png" style="height: 198px; width: 352px;"></p>

<h3 id="Radio_button">Radio button</h3>

<p>A radio button is created using the {{HTMLElement("input")}} element with its {{htmlattrxref("type","input")}} attribute set to the value <code>radio</code>.</p>

<pre class="brush: html">&lt;input type="radio" checked id="soup" name="meal"&gt;</pre>

<p>Several radio buttons can be tied together. If they share the same value for their {{htmlattrxref("name","input")}} attribute, they will be considered to be in the same group of buttons. Only one button in a given group may be checked at the same time; this means that when one of them is checked all the others automatically get unchecked. When the form is sent, only the value of the checked radio button is sent. If none of them are checked, the whole pool of radio buttons is considered to be in an unknown state and no value is sent with the form.</p>

<pre class="brush: html">&lt;fieldset&gt;
  &lt;legend&gt;What is your favorite meal?&lt;/legend&gt;
  &lt;ul&gt;
    &lt;li&gt;
      &lt;label for="soup"&gt;Soup&lt;/label&gt;
      &lt;input type="radio" checked id="soup" name="meal" value="soup"&gt;
    &lt;/li&gt;
    &lt;li&gt;
      &lt;label for="curry"&gt;Curry&lt;/label&gt;
      &lt;input type="radio" id="curry" name="meal" value="curry"&gt;
    &lt;/li&gt;
    &lt;li&gt;
      &lt;label for="pizza"&gt;Pizza&lt;/label&gt;
      &lt;input type="radio" id="pizza" name="meal" value="pizza"&gt;
    &lt;/li&gt;
  &lt;/ul&gt;
&lt;/fieldset&gt;</pre>

<p><img alt="Screenshots of radio buttons on several platforms." src="/files/4597/all-radio.png" style="height: 198px; width: 352px;"></p>

<h2 id="Buttons">Buttons</h2>

<p>Within HTML forms, there are three kinds of button:</p>

<dl>
 <dt>Submit</dt>
 <dd>Sends the form data to the server. For {{HTMLElement("button")}} elements, omitting the <code>type</code> attribute (or an invalid value of <code>type</code>) results in a submit button.</dd>
 <dt>Reset</dt>
 <dd>Resets all form widgets to their default values.</dd>
 <dt>Anonymous</dt>
 <dd>Buttons that have no automatic effect but can be customized using JavaScript code.</dd>
</dl>

<div class="note">
<p><strong>Note</strong>: You can find the examples from this section on GitHub as <a href="https://github.com/mdn/learning-area/blob/master/html/forms/native-form-widgets/button-examples.html">button-examples.html</a> (<a href="https://mdn.github.io/learning-area/html/forms/native-form-widgets/button-examples.html">see it live also</a>).</p>
</div>

<p>A button is created using a {{HTMLElement("button")}} element or an {{HTMLElement("input")}} element. It's the value of the {{htmlattrxref("type","input")}} attribute that specifies what kind of button is displayed:</p>

<h3 id="submit">submit</h3>

<pre class="brush: html">&lt;button type="submit"&gt;
    This a &lt;br&gt;&lt;strong&gt;submit button&lt;/strong&gt;
&lt;/button&gt;

&lt;input type="submit" value="This is a submit button"&gt;</pre>

<h3 id="reset">reset</h3>

<pre class="brush: html">&lt;button type="reset"&gt;
    This a &lt;br&gt;&lt;strong&gt;reset button&lt;/strong&gt;
&lt;/button&gt;

&lt;input type="reset" value="This is a reset button"&gt;</pre>

<h3 id="anonymous">anonymous</h3>

<pre class="brush: html">&lt;button type="button"&gt;
    This an &lt;br&gt;&lt;strong&gt;anonymous button&lt;/strong&gt;
&lt;/button&gt;

&lt;input type="button" value="This is an anonymous button"&gt;</pre>

<p>Buttons always behave the same whether you use a {{HTMLElement("button")}} element or an {{HTMLElement("input")}} element. There are, however, some notable differences:</p>

<ul>
 <li>As you can see from the examples, {{HTMLElement("button")}} elements let you use HTML content in their labels, which are inserted inside the opening and closing <code>&lt;button&gt;</code> tags. {{HTMLElement("input")}} elements on the other hand are empty elements; their labels are inserted inside <code>value</code> attributes, and therefore only accept plain text content.</li>
 <li>With {{HTMLElement("button")}} elements, it's possible to have a value different than the button's label (by setting it inside a <code>value</code> attribute). This isn't reliable in versions of Internet Explorer prior to IE 8.</li>
</ul>

<p><img alt="Screenshots of buttons on several platforms." src="/files/4599/all-buttons.png" style="height: 235px; width: 464px;"></p>

<p>Technically speaking, there is almost no difference between a button defined with the {{HTMLElement("button")}} element or the {{HTMLElement("input")}} element. The only noticeable difference is the label of the button itself. Within an {{HTMLElement("input")}} element, the label can only be character data, whereas in a {{HTMLElement("button")}} element, the label can be HTML, so it can be styled accordingly.</p>

<h2 id="Advanced_form_widgets">Advanced form widgets</h2>

<p>In this section we cover those widgets that let users input complex or unusual data. This includes exact or approximate numbers, dates and times, or colors.</p>

<div class="note">
<p><strong>Note</strong>: You can find the examples from this section on GitHub as <a href="https://github.com/mdn/learning-area/blob/master/html/forms/native-form-widgets/advanced-examples.html">advanced-examples.html</a> (<a href="https://mdn.github.io/learning-area/html/forms/native-form-widgets/advanced-examples.html">see it live also</a>).</p>
</div>

<h3 id="Numbers">Numbers</h3>

<p>Widgets for numbers are created with the {{HTMLElement("input")}} element, with its {{htmlattrxref("type","input")}} attribute set to the value <code>number</code>. This control looks like a text field but allows only floating-point numbers, and usually provides some buttons to increase or decrease the value of the widget.</p>

<p>It's also possible to:</p>

<ul>
 <li>Constrain the value by setting the {{htmlattrxref("min","input")}} and {{htmlattrxref("max","input")}} attributes.</li>
 <li>Specify the amount by which the increase and decrease buttons change the widget's value by setting the {{htmlattrxref("step","input")}} attribute.</li>
</ul>

<h4 id="Example">Example</h4>

<pre class="brush: html">&lt;input type="number" name="age" id="age" min="1" max="10" step="2"&gt;</pre>

<p>This creates a number widget whose value is restricted to any value between 1 and 10, and whose increase and decrease buttons change its value by 2.</p>

<p><code>number</code> inputs are not supported in versions of Internet Explorer below 10.</p>

<h3 id="Sliders">Sliders</h3>

<p>Another way to pick a number is to use a slider. Visually speaking, sliders are less accurate than text fields, therefore they are used to pick a number whose exact value is not necessarily important.</p>

<p>A slider is created by using the {{HTMLElement("input")}} with its {{htmlattrxref("type","input")}} attribute set to the value <code>range</code>. It's important to properly configure your slider; to that end, it's highly recommended that you set the {{htmlattrxref("min","input")}}, {{htmlattrxref("max","input")}}, and {{htmlattrxref("step","input")}} attributes.</p>

<h4 id="Example_2">Example</h4>

<pre class="brush: html">&lt;input type="range" name="beans" id="beans" min="0" max="500" step="10"&gt;</pre>

<p>This example creates a slider whose value may range between 0 and 500, and whose increment/decrement buttons change the value by +10 and -10.</p>

<p>One problem with sliders is that they don't offer any kind of visual feedback as to what the current value is. You need to add this yourself with JavaScript, but this is relatively easy to do. In this example we add an empty {{htmlelement("span")}} element, in which we will write the current value of the slider, updating it as it is changed.</p>

<pre class="brush: html">&lt;label for="beans"&gt;How many beans can you eat?&lt;/label&gt;
&lt;input type="range" name="beans" id="beans" min="0" max="500" step="10"&gt;
&lt;span class="beancount"&gt;&lt;/span&gt;</pre>

<p>This can be implemented using some simple JavaScript:</p>

<pre class="brush: js">var beans = document.querySelector('#beans');
var count = document.querySelector('.beancount');

count.textContent = beans.value;

beans.oninput = function() {
  count.textContent = beans.value;
}</pre>

<p>Here we store references to the range input and the span in two variables, then we immediately set the span's <code><a href="/en-US/docs/Web/API/Node/textContent">textContent</a></code> to the current <code>value</code> of the input. Finally, we set up an <code>oninput</code> event handler so that every time the range slider is moved, the span <code>textContent</code> is updated to the new input value.</p>

<p><code>range</code> inputs are not supported in versions of Internet Explorer below 10.</p>

<h3 id="Date_and_time_picker">Date and time picker</h3>

<p>Gathering date and time values has traditionally been a nightmare for web developers. HTML5 brings some enhancements here by providing a special control to handle this specific kind of data.</p>

<p>A date and time control is created using the {{HTMLElement("input")}} element and an appropriate value for the {{htmlattrxref("type","input")}} attribute, depending on whether you wish to collect dates, times, or both.</p>

<h4 id="datetime-local"><code>datetime-local</code></h4>

<p>This creates a widget to display and pick a date with time, but without any specific time zone information.</p>

<pre class="brush: html">&lt;input type="datetime-local" name="datetime" id="datetime"&gt;</pre>

<h4 id="month"><code>month</code></h4>

<p>This creates a widget to display and pick a month with a year.</p>

<pre class="brush: html">&lt;input type="month" name="month" id="month"&gt;</pre>

<h4 id="time"><code>time</code></h4>

<p>This creates a widget to display and pick a time value.</p>

<pre class="brush: html">&lt;input type="time" name="time" id="time"&gt;</pre>

<h4 id="week"><code>week</code></h4>

<p>This creates a widget to display and pick a week number and its year.</p>

<pre class="brush: html">&lt;input type="week" name="week" id="week"&gt;</pre>

<p>All date and time control can be constrained using the {{htmlattrxref("min","input")}} and {{htmlattrxref("max","input")}} attributes.</p>

<pre class="brush: html">&lt;label for="myDate"&gt;When are you available this summer?&lt;/label&gt;
&lt;input type="date" name="myDate" min="2013-06-01" max="2013-08-31" id="myDate"&gt;</pre>

<p>Warning — The date and time widgets don't have the deepest support. At the moment, Chrome, Edge, Firefox, and Opera support them well, but there is no support in Internet Explorer and Safari has patchy support.</p>

<h3 id="Color_picker">Color picker</h3>

<p>Colors are always a bit difficult to handle. There are many ways to express them: RGB values (decimal or hexadecimal), HSL values, keywords, etc. The color widget lets users pick a color in both textual and visual ways.</p>

<p>A color widget is created using the {{HTMLElement("input")}} element with its {{htmlattrxref("type","input")}} attribute set to the value <code>color</code>.</p>

<pre class="brush: html">&lt;input type="color" name="color" id="color"&gt;</pre>

<p>Warning — Color widget support it currently not very good. There is no support in Internet Explorer, and Safari currently doesn't support it either. The other major browsers do support it.</p>

<h2 id="Other_widgets">Other widgets</h2>

<p>There are a few other widgets that cannot be easily classified due to their very specific behaviors, but which are still very useful.</p>

<div class="note">
<p><strong>Note</strong>: You can find the examples from this section on GitHub as <a href="https://github.com/mdn/learning-area/blob/master/html/forms/native-form-widgets/other-examples.html">other-examples.html</a> (<a href="https://mdn.github.io/learning-area/html/forms/native-form-widgets/other-examples.html">see it live also</a>).</p>
</div>

<h3 id="File_picker">File picker</h3>

<p>HTML forms are able to send files to a server; this specific action is detailed in the article <a href="/en-US/docs/Learn/HTML/Forms/Sending_and_retrieving_form_data" title="/en-US/docs/HTML/Forms/Sending_and_retrieving_form_data">Sending and retrieving form data</a>. The file picker widget is how the user can choose one or more files to send.</p>

<p>To create a file picker widget, you use the {{HTMLElement("input")}} element with its {{htmlattrxref("type","input")}} attribute set to <code>file</code>. The types of files that are accepted can be constrained using the {{htmlattrxref("accept","input")}} attribute. In addition, if you want to let the user pick more than one file, you can do so by adding the {{htmlattrxref("multiple","input")}} attribute.</p>

<h4 id="Example_3">Example</h4>

<p>In this example, a file picker is created that requests graphic image files. The user is allowed to select multiple files in this case.</p>

<pre class="brush: html">&lt;input type="file" name="file" id="file" accept="image/*" multiple&gt;</pre>

<h3 id="Hidden_content">Hidden content</h3>

<p>It's sometimes convenient for technical reasons to have pieces of data that are sent with a form but not displayed to the user. To do this, you can add an invisible element in your form. Use an {{HTMLElement("input")}} with its {{htmlattrxref("type","input")}} attribute set to the value <code>hidden</code>.</p>

<p>If you create such an element, it's required to set its <code>name</code> and <code>value</code> attributes:</p>

<pre class="brush: html">&lt;input type="hidden" id="timestamp" name="timestamp" value="1286705410"&gt;</pre>

<h3 id="Image_button">Image button</h3>

<p>The <strong>image button</strong> control is one which is displayed exactly like an {{HTMLElement("img")}} element, except that when the user clicks on it, it behaves like a submit button (see above).</p>

<p>An image button is created using an {{HTMLElement("input")}} element with its {{htmlattrxref("type","input")}} attribute set to the value <code>image</code>. This element supports exactly the same set of attributes as the {{HTMLElement("img")}} element, plus all the attributes supported by other form buttons.</p>

<pre class="brush: html">&lt;input type="image" alt="Click me!" src="my-img.png" width="80" height="30" /&gt;</pre>

<p>If the image button is used to submit the form, this widget doesn't submit its value; instead the X and Y coordinates of the click on the image are submitted (the coordinates are relative to the image, meaning that the upper-left corner of the image represents the coordinate 0, 0). The coordinates are sent as two key/value pairs:</p>

<ul>
 <li>The X value key is the value of the {{htmlattrxref("name","input")}} attribute followed by the string "<em>.x</em>".</li>
 <li>The Y value key is the value of the {{htmlattrxref("name","input")}} attribute followed by the string "<em>.y</em>".</li>
</ul>

<p>So for example when you click on the image of this widget, you are sent to a URL like the following:</p>

<pre>http://foo.com?pos.x=123&amp;pos.y=456</pre>

<p>This is a very convenient way to build a "hot map". How these values are sent and retrieved is detailed in the <a href="/en-US/docs/Learn/HTML/Forms/Sending_and_retrieving_form_data" title="/en-US/docs/HTML/Forms/Sending_and_retrieving_form_data"><span>Sending and retrieving form data</span></a> article.</p>

<h3 id="Meters_and_progress_bars">Meters and progress bars</h3>

<p>Meters and progress bars are visual representations of numeric values.</p>

<h4 id="Progress">Progress</h4>

<p>A progress bar represents a value that changes over time up to a maximum value specified by the {{htmlattrxref("max","progress")}} attribute. Such a bar is created using a {{ HTMLElement("progress")}} element.</p>

<pre class="brush: html">&lt;progress max="100" value="75"&gt;75/100&lt;/progress&gt;</pre>

<p>This is for implementing anything requiring progress reporting, such as the percentage of total files downloaded, or the number of questions filled in on a questionnaire.</p>

<p>The content inside the {{HTMLElement("progress")}} element is a fallback for browsers that don't support the element and for assistive technologies to vocalize it.</p>

<h4 id="Meter">Meter</h4>

<p>A meter bar represents a fixed value in a range delimited by a {{htmlattrxref("min","meter")}} and a {{htmlattrxref("max","meter")}} value. This value is visualy rendered as a bar, and to know how this bar looks, we compare the value to some other set values:</p>

<ul>
 <li>The {{htmlattrxref("low","meter")}} and {{htmlattrxref("high","meter")}} values divide the range in three parts:
  <ul>
   <li>The lower part of the range is between the {{htmlattrxref("min","meter")}} and {{htmlattrxref("low","meter")}} values (including those values).</li>
   <li>The medium part of the range is between the {{htmlattrxref("low","meter")}} and {{htmlattrxref("high","meter")}} values (excluding those values).</li>
   <li>The higher part of the range is between the {{htmlattrxref("high","meter")}} and {{htmlattrxref("max","meter")}} values (including those values).</li>
  </ul>
 </li>
 <li>The {{htmlattrxref("optimum","meter")}} value defines the optimum value for the {{HTMLElement("meter")}} element. In conjuction with the {{htmlattrxref("low","meter")}} and {{htmlattrxref("high","meter")}} value, it defines which part of the range is prefered:
  <ul>
   <li>If the {{htmlattrxref("optimum","meter")}} value is in the lower part of the range, the lower range is considered to be the prefered part, the medium range is considered to be the average part and the higher range is considered to be the worst part.</li>
   <li>If the {{htmlattrxref("optimum","meter")}} value is in the medium part of the range, the lower range is considered to be an average part, the medium range is considered to be the prefered part and the higher range is considered to be average as well.</li>
   <li>If the {{htmlattrxref("optimum","meter")}} value is in the higher part of the range, the lower range is considered to be the worst part, the medium range is considered to be the average part and the higher range is considered to be the prefered part.</li>
  </ul>
 </li>
</ul>

<p>All browsers that implement the {{HTMLElement("meter")}} element use those values to change the color of the meter bar:</p>

<ul>
 <li>If the current value is in the prefered part of the range, the bar is green.</li>
 <li>If the current value is in the average part of the range, the bar is yellow.</li>
 <li>If the current value is in the worst part of the range, the bar is red.</li>
</ul>

<p>Such a bar is created using a {{HTMLElement("meter")}} element. This is for implementing any kind of meter, for example a bar showing total space used on a disk, which turns red when it starts to get full.</p>

<pre class="brush: html">&lt;meter min="0" max="100" value="75" low="33" high="66" optimum="50"&gt;75&lt;/meter&gt;</pre>

<p>The content inside the {{HTMLElement("meter")}} element is a fallback for browsers that don't support the element and for assistive technologies to vocalize it.</p>

<p>Support for progress and meter is fairly good — there is no support in Internet Explorer, but other browsers support it well.</p>

<h2 id="Conclusion">Conclusion</h2>

<p>As you'll have seen above, there are a lot of different types of available form elements — you don't need to remember all of these details at once, and can return to this article as often as you like to check up on details.</p>

<h2 id="See_also">See also</h2>

<p>To dig into the different form widgets, there are some useful external resources you should check out:</p>

<ul>
 <li><a href="http://wufoo.com/html5/" rel="external">The Current State of HTML5 Forms</a> by Wufoo</li>
 <li><a href="http://www.quirksmode.org/html5/inputs.html" rel="external">HTML5 Tests - inputs</a> on Quirksmode (also <a href="http://www.quirksmode.org/html5/inputs_mobile.html" rel="external">available for mobile</a> browsers)</li>
</ul>

<p>{{PreviousMenuNext("Learn/HTML/Forms/How_to_structure_an_HTML_form", "Learn/HTML/Forms/Sending_and_retrieving_form_data", "Learn/HTML/Forms")}}</p>



<h2 id="In_this_module">In this module</h2>

<ul>
 <li><a href="/en-US/docs/Learn/HTML/Forms/Your_first_HTML_form">Your first HTML form</a></li>
 <li><a href="/en-US/docs/Learn/HTML/Forms/How_to_structure_an_HTML_form">How to structure an HTML form</a></li>
 <li><a href="/en-US/docs/Learn/HTML/Forms/The_native_form_widgets">The native form widgets</a></li>
 <li><a href="/en-US/docs/Learn/HTML/Forms/Sending_and_retrieving_form_data">Sending form data</a></li>
 <li><a href="/en-US/docs/Learn/HTML/Forms/Form_validation">Form data validation</a></li>
 <li><a href="/en-US/docs/Learn/HTML/Forms/How_to_build_custom_form_widgets">How to build custom form widgets</a></li>
 <li><a href="/en-US/docs/Learn/HTML/Forms/Sending_forms_through_JavaScript">Sending forms through JavaScript</a></li>
 <li><a href="/en-US/docs/Learn/HTML/Forms/HTML_forms_in_legacy_browsers">HTML forms in legacy browsers</a></li>
 <li><a href="/en-US/docs/Learn/HTML/Forms/Styling_HTML_forms">Styling HTML forms</a></li>
 <li><a href="/en-US/docs/Learn/HTML/Forms/Advanced_styling_for_HTML_forms">Advanced styling for HTML forms</a></li>
 <li><a href="/en-US/docs/Learn/HTML/Forms/Property_compatibility_table_for_form_widgets">Property compatibility table for form widgets</a></li>
</ul>