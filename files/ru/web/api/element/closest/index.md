---
title: Element.closest()
slug: Web/API/Element/closest
tags:
  - API
  - DOM
  - Element
  - Method
  - Reference
translation_of: Web/API/Element/closest
---
{{APIRef("DOM")}}

Метод **`Element.closest()`** возвращает ближайший родительский элемент (или сам элемент), который соответствует заданному CSS-селектору или null, если таковых элементов вообще нет.

## Синтаксис

```
var elt = element.closest(selectors);
```

- **`selectors` _-_ **строка, а точнее {{domxref("DOMString")}}`, `содержащая CSS-селектор, к примеру: _**"#id", ".class", "div"**..._
- Результат - элемент DOM ({{domxref("Element")}}), либо null.

## Исключения

- `SYNTAX_ERR`
  - : Указанный css-селектор не является допустимым _("/=21=1", "&@\*#", "%'54523" и т.п. приведут к ошибке)._

## Пример

```
<div id="block" title="Я - блок">
    <a href="#">Я ссылка в никуда</a>
    <a href="http://site.ru">Я ссылка на сайт</a>
    <div>
       <div id="too"></div>
    </div>
</div>
```

Думаю, стоит рассмотреть несколько примеров:

```js
var div = document.querySelector("#too"); //Это элемент от которого мы начнём поиск

div.closest("#block"); //Результат - самый первый блок древа выше
div.closest("div"); //Сам блок #too и будет результатом, так как он подходит под селектор "div"
div.closest("a"); //null - В предках #too нет ни одного тега "a"!
div.closest("div[title]") //#block - так как ближе нет блоков с атрибутом title.
```

## Полифил #1 (рекурсивный метод)

Для браузеров не поддерживающих Element.closest(), но позволяющих использовать element.matches() (или префиксный эквивалент) есть полифил:

```js
(function(ELEMENT) {
    ELEMENT.matches = ELEMENT.matches || ELEMENT.mozMatchesSelector || ELEMENT.msMatchesSelector || ELEMENT.oMatchesSelector || ELEMENT.webkitMatchesSelector;
    ELEMENT.closest = ELEMENT.closest || function closest(selector) {
        if (!this) return null;
        if (this.matches(selector)) return this;
        if (!this.parentElement) {return null}
        else return this.parentElement.closest(selector)
      };
}(Element.prototype));
```

## Полифил #2 (через цикл)

Тем не менее, если вам требуется поддержка IE 8, вы можете использовать следующий полифил. Имейте ввиду - этот способ позволяет использовать CSS селекторы только уровня 2.1 и может жутко тормозить.

```js
(function(e){
 e.closest = e.closest || function(css){
   var node = this;
   while (node) {
      if (node.matches(css)) return node;
      else node = node.parentElement;
   }
   return null;
 }
})(Element.prototype);
```

## Спецификация

| Specification                                                                                    | Status                           | Comment             |
| ------------------------------------------------------------------------------------------------ | -------------------------------- | ------------------- |
| {{SpecName('DOM WHATWG', '#dom-element-closest', 'Element.closest()')}} | {{Spec2('DOM WHATWG')}} | Initial definition. |

## Совместимость с браузерами

{{Compat}}

### Примечания совместимости

- В Edge `document.createElement(tagName).closest(tagName)` возвращает `null`, если элемент ещё не привязан в DOM.

## Смотрите также

- Интерфейс {{domxref("Element")}}.
- [Синтаксис селекторов](/ru/docs/Learn/CSS/Introduction_to_CSS/Selectors)
- Другие методы, принимающие селекторы: {{domxref("element.querySelector()")}} и {{domxref("element.matches()")}}.
