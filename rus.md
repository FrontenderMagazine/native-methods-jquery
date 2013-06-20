# Нативные эквиваленты функций jQuery

_**Дополнение**: многие спрашивали о поддержке браузерами методов, которые я
показал. Узнать о ней можно тут: [querySelector/querySelectorAll][1],
[classList][2], [getElementsByClassName][3], [createDocumentFragment][4]._

---

Если вы читали мою [последнюю статью][5], то знаете, что я в последнее
время много пишу на JavaScript, и не только для [Brackets][6]. Кроме того, я
провел серию сравнительных тестов производительности ([1][7], [2][8], [3][9])
популярных методов jQuery и их DOM-эквивалентов.

Да  я знаю, о чем вы думаете. Очевидно, что нативные методы JavaScript
быстрее, чем jQuery с её обратной совместимостью со старыми браузерами и
прочими функциями. Полностью согласен. Именно поэтому я бы не сказал, что это
анти-jQuery статья. Но если вы используете современные браузеры, то нативные,
написанные на C++ методы, которые они предоставляют, дадут вам в большинстве
случаев огромный прирост производительности.

Я думаю, что многие разработчики, даже не подозревают о том, что большинство
методов jQuery, которые они используют, имеют эквивалентные им нативные методы
JavaScript. При этом использование последних не всегда означает увеличение
объемов исходного кода. Далее на примерах я покажу некоторые из популярных
функций jQuery и их нативные эквиваленты.

## Селекторы

Возможность легко найти DOM-элементы - ключевая особенность библиотеки jQuery.
Вы можете передать ей CSS селектор, и она вернет все элементы DOM, которые ему
соответствуют. Чаще всего это можно сделать с помощью нативных методов,
написав то же количество кода.

Нативные эквиваленты выборки по селектору:

    //----Получить все элементы DIV на странице---------

      /* jQuery */
      $("div")

      /* нативный метод */
      document.getElementsByTagName("div")

    //----Получить все элементы с определенным CSS классом---------

      /* jQuery */
      $(".my-class")

      /* нативный метод */
      document.querySelectorAll(".my-class")

      /* нативный метод, который работает ЕЩЁ БЫСТРЕЕ */
      document.getElementsByClassName("my-class")

    //----Получить элементы, соответствующие CSS-селектору----------

      /* jQuery */
      $(".my-class li:first-child")

      /* нативный метод */
      document.querySelectorAll(".my-class li:first-child")

    //----Получить первый элемент DOM соответствующий определенному CSS селектору----

      /* jQuery */
      $(".my-class").get(0)

      /* нативный метод */
      document.querySelector(".my-class")

## Манипуляции с DOM

Еще одной областью, в которой часто используется jQuery, является работа с
DOM, вставка или удаление элементов. Чтобы сделать это правильно на
JavaScript, потребуется чуть больше кода, но вы всегда можете написать
вспомогательные функции при многократном использовании. Ниже приведен пример
вставки множества DOM-элементов в элемент body:

    //----Добавляем HTML элементы----

      /* jQuery */
      $(document.body).append("<div id='myDiv'><img src='im.gif'/></div>");

      /* ЖУТКИЙ нативный метод */
      document.body.innerHTML += "<div id='myDiv'><img src='im.gif'/></div>";

      /* нативный метод, который НАМНОГО ЛУЧШЕ */
      var frag = document.createDocumentFragment();

      var myDiv = document.createElement("div");
      myDiv.id = "myDiv";

      var im = document.createElement("img");
      im.src = "im.gif";

      myDiv.appendChild(im);
      frag.appendChild(myDiv);

      document.body.appendChild(frag);

    //----Добавляем HTML элементы перед остальными----

      // Тоже самое, что и выше, кроме последней строки
      document.body.insertBefore(frag, document.body.firstChild);

## CSS-классы

Используя jQuery очень просто добавить, удалить или проверить наличие класса
у элемента DOM. К счастью, все это так же просто сделать с помощью нативных
методов. Я сам о них недавно узнал. Спасибо вам, Chrome DevTools.

    // получаем ссылку на элемент DOM
    var el = document.querySelector(".main-content");

    //----Добавляем класс------

      /* jQuery */
      $(el).addClass("someClass");

      /* нативный метод */
      el.classList.add("someClass");

    //----Удаляем класс-----

      /* jQuery */
      $(el).removeClass("someClass");

      /* нативный метод */
      el.classList.remove("someClass");

    //----Проверяем наличие класса---

      /* jQuery */
      if($(el).hasClass("someClass"))

      /* нативный метод */
      if(el.classList.contains("someClass"))

## Изменение CSS-свойств

Необходимость программно установить или прочитать значение CSS-свойств,
используя JavaScript, возникает регулярно. При этом, гораздо быстрее задавать
свойства по одному, а не передавать их все в функцию CSS библиотеки jQuery. И,
да, это не потребует от вас написания большого количества дополнительного кода.

    // получаем ссылку на элемент DOM
    var el = document.querySelector(".main-content");

    //----Переопределить множество CSS-свойств----

    /* jQuery */
    $(el).css({
      background: "#FF0000",
      "box-shadow": "1px 1px 5px 5px red",
      width: "100px",
      height: "100px",
      display: "block"
    });

    /* нативный метод */
    el.style.background = "#FF0000";
    el.style.width = "100px";
    el.style.height = "100px";
    el.style.display = "block";
    el.style.boxShadow = "1px 1px 5px 5px red";

Помните, что jQuery удивительная библиотека, которая упрощает нашу жизнь. Но
вы всегда должны делать выбор в пользу DOM-методов, если это возможно в данном
случае. Это особенно важно, если вы используете jQuery внутри циклов или
таймеров.

Конечно, после того как я довольно долго крутился в среде разработчиков игр,
я стал слишком чувствителен в отношении вопросов производительности. Ведь там,
если ваша игра не работает на 60 FPS, то вы можете начинать искать себе работу
в сфере торговли. Так или иначе, надеюсь, что эта статья будет вам полезна!


[1]: http://caniuse.com/#search=queryselector
[2]: http://caniuse.com/#search=classlist
[3]: http://caniuse.com/#search=getelementsbyclassname
[4]: http://www.quirksmode.org/dom/w3c_core.html#miscellaneous
[5]: http://www.leebrimelow.com/responsive-design-with-adobe-brackets/
[6]: http://brackets.io/
[7]: http://jsperf.com/jquery-css-vs-native-dom
[8]: http://jsperf.com/jquery-vs-document-queryselector
[9]: http://jsperf.com/innertext-vs-fragment
