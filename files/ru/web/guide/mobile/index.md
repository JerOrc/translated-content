---
title: Веб-разработка под мобильные устройства
slug: Web/Guide/mobile
tags:
  - Средний уровень
translation_of: Web/Guide/Mobile
---
На этой странице даётся обзор основных методов разработки веб-сайтов, корректно работающих на мобильных устройствах. Если вы ищете информацию о проекте Firefox OS от Mozilla, смотрите страницу [Firefox OS](/ru/docs/Archive/B2G_OS "Boot to Gecko"). Возможно вас также интересует подробная информация о [Firefox для Android](/ru/docs/Mozilla/Firefox_%D0%B4%D0%BB%D1%8F_Android "Firefox for Android").

Статья разбита на два раздела: [разработка под мобильные устройства](#разработка_под_мобильные_устройства) и [кросс-браузерная совместимость](#кросс_браузерная_вёрстка).
Смотрите также руководство от Jason Grlicky по [дружелюбность-к-мобильным](/ru/docs/Web_Development/Mobile/Mobile-friendliness) для веб-разработчиков.

## Разработка под мобильные устройства

По сравнению со стационарными компьютерами и ноутбуками у мобильных устройств гораздо больше различий в их аппаратной составляющей. Экраны мобильных устройств могут иметь как очень большое, так и очень маленькое разрешение. Помимо этого, они могут автоматически переключаться между вертикальной и горизонтальной ориентацией в момент, когда пользователь поворачивает устройство. Они обычно используют сенсорный экран для пользовательского ввода. Всевозможные API, например, геолокация или ориентация, которые либо не поддерживаются или не используются на на стационарных компьютерах, предоставляют мобильным устройствам пользователей дополнительные способы взаимодействия с вашим сайтом.

### Работа с маленькими экранами

[Адаптивный веб-дизайн](/ru/docs/Web/Guide/Responsive_design) – это термин, означающий набор методов, позволяющих вашему веб-сайту адаптировать разметку под среду, в которой он просматривается. Как правило, это изменяющиеся размеры и ориентация. Основные методы адаптивного веб-дизайна:

- обтекаемая разметка CSS, позволяющая плавно адаптировать страницу под изменяющиеся размеры окна
- использование [медиавыражений](/en/CSS/Media_queries "en/CSS/Media_queries"), подключающих стили по условию, соответственно, [ширине](/en/CSS/Media_queries#width "en/CSS/Media_queries#width") и [высоте](/en/CSS/Media_queries#height "en/CSS/Media_queries#height") экрана.

[Meta-тег viewport](/ru/docs/Mozilla/Mobile/Viewport_meta_tag "en/Mobile/Viewport_meta_tag") указывает браузеру, каким образом отображать ваш сайт в подходящем масштабе на устройстве пользователя.

### Работа с сенсорными экранами

Для использования сенсорного экрана вам понадобится работать с [DOM Touch events](/en/DOM/Touch_events "en/DOM/Touch_events"). Вы не сможете использовать псевдо класс [CSS :hover](/ru/docs/Web/CSS/:hover "En/CSS/:hover"), а при проектировании интерактивных элементов (к примеру, кнопок) нужно будет учитывать тот факт, что пальцы толще, чем указатели мыши. Дополнительные материалы в статье [проектирование под сенсорные экраны](https://web.archive.org/web/20150520130912/http://www.whatcreative.co.uk/blog/tips/designing-for-touch-screen/).

Вы можете использовать медиавыражение [-moz-touch-enabled](/en/CSS/Media_queries#-moz-touch-enabled "en/CSS/Media_queries#-moz-touch-enabled"), чтобы использовать нужные вам правила CSS на устройствах, поддерживающих обработку нажатий на экран.

### Оптимизация изображений

Для оптимизации скорости загрузки у пользователей мобильных устройств вы имеете возможность загружать различные версии изображений в соответствии со спецификациями экрана устройства. Описать правила загрузки можно в CSS при помощи медиавыражений [height](/en/CSS/Media_queries#height "en/CSS/Media_queries#height"), [width](/en/CSS/Media_queries#width "en/CSS/Media_queries#width"), и [pixel ratio](/en/CSS/Media_queries#-moz-device-pixel-ratio "en/CSS/Media_queries#-moz-device-pixel-ratio").

Также вы можете использовать свойства CSS чтобы применить визуальные эффекты типа [gradients](/en/CSS/Using_CSS_gradients "en/CSS/Using_CSS_gradients") и [shadows](/En/CSS/Box-shadow "En/CSS/Box-shadow") без использования изображений.

### API для мобильных устройств

Наконец, вы можете воспользоваться преимуществом новых возможностей, предлагаемых мобильными устройствами, такие как [orientation](/en/Detecting_device_orientation "en/Detecting_device_orientation") и [geolocation](/En/Using_geolocation "En/Using_geolocation").

## Кросс-браузерная вёрстка

### Пишите кросс-браузерный код

Чтобы создавать веб-сайты, которые будут работать приемлемо во всех мобильных браузерах:

- Старайтесь избегать использования стилей, специфических для браузеров, таких как свойства CSS с вендорными префиксами.
- Если всё же вам необходимо ими воспользоваться, убедитесь что другие браузеры применяют свои собственные версии этих свойств, и укажите их.
- Для браузеров, которые не поддерживают эти свойства, обеспечьте приемлемый упрощённый вариант.

Например, если вы указываете градиент в качестве фона для какого-либо текста, используя стиль с вендорным префиксом типа `-webkit-linear-gradient`, правильнее включить другие варианты с вендор-специфическими префиксами для свойства [linear-gradient](/en/CSS/linear-gradient "en/CSS/linear-gradient"). Если вы этого не делаете, по крайней мере удостоверьтесь, что фон, отображаемый по умолчанию, контрастирует с текстом: тогда страница, по крайней мере, будет корректно отображаться в браузере, который не воспринимает ваше `linear-gradient` правило.

Смотрите этот [список Gecko-специфических свойств](/en/CSS/CSS_Reference/Mozilla_Extensions "en/CSS/CSS_Reference/Mozilla_Extensions"), и этот список [WebKit-](/en/CSS/CSS_Reference/Webkit_Extensions "en/CSS/CSS_Reference/Webkit_Extensions")[специфических свойств](/en/CSS/CSS_Reference/Mozilla_Extensions "en/CSS/CSS_Reference/Mozilla_Extensions"), и [таблицу вендор-специфических свойств](http://peter.sh/experiments/vendor-prefixed-css-property-overview/) от Peter Beverloo.

Использование инструментов, наподобие [CSS Lint](http://csslint.net/) может помочь обнаружить подобные проблемы в коде, а такие препроцессоры, как [SASS](http://sass-lang.com/) и [LESS](http://lesscss.org/) могут помочь вам создавать кросс-браузерный код.

### Позаботьтесь об анализе user agent

Для веб-сайтов предпочтительнее обнаружить свойства, специфичные для устройства, такие как размеры экрана и наличие сенсорного экрана, используя описанные выше способы, и соответственно адаптироваться. Но иногда это непрактично, и веб-сайты прибегают к синтаксическому разбору строки user agent браузера чтобы различить десктопы, планшеты, и смартфоны, чтобы отправлять разный контент под каждый тип устройств.

Если вы это делаете, удостоверьтесь что ваш алгоритм точный, и вы не отправляете неверный тип контента на устройство из-за того что вы не знаете какой-то специфической для браузера user agent строки. Смотрите данное [руководство по использованию user agent строки чтобы определить тип устройства](/en/Browser_detection_using_the_user_agent#Mobile.2C_Tablet_or_Desktop "browser detection using the user agent").

### Проверяйте на многих браузерах

Проверяйте ваш веб-сайт на многих браузерах. Это означает тестирование на многих платформах — по крайней мере на iOS и Android.

- тестируйте под мобильный Safari на iPhone используя [iOS симулятор](https://developer.apple.com/devcenter/ios/index.action)
- тестируйте под Opera и Firefox используя [Android SDK](https://developer.android.com/sdk/index.html). Смотрите эти дополнительные инструкции для [запуска Firefox под Android используя эмулятор Android](https://wiki.mozilla.org/Mobile/Fennec/Android/Emulator).