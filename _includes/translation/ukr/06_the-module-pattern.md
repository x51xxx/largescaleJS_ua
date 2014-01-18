<!-- ##### Паттерн «Модуль» -->

Патерн «модуль» - це популярна реалізація патерну, інкапсуляція «приватної»
інформацію, стану і структури використовуючи замикання. Це дозволяє обертати
у модулі набір публічних і приватних методів і змінних, і запобігати
потраплянню цих змінних і методів у глобальний контекст, де вони можуть
конфліктувати з інтерфейсами інших розробників. Патерн «модуль» повертає
тільки публічну частину API, залишаючи все інше приватним, всередині замикань.

Це забезпечує чисте рішення для того, щоб, приховавши логіку від сторонніх
очей, виконувати важку роботу виключно через інтерфейс, який ви
визначте для використання в інших частинах вашої програми. цей патерн
схожий на самостійно викликаємі анонімні функцію ([IIFE] [3]),
за винятком того, що модуль повернуто не об'єкт, а функцію.

Потрібно зауважити, що в JavaScript немає справжньої приватності. На відміну
від деяких традиційних мов, JavaScript не має модифікаторів доступу.
Змінні технічно не можуть бути об'явлені як публічні або приватні, і
нам доводиться використовувати область видимості для того, щоб емулювати цю
концепцію. Завдяки замиканням, оголошені всередині модуля змінні і методи
доступні тільки зсередини цього модуля. Змінні і методи, оголошені всередині
об'єкта, що повертається модулем, будуть доступні всім.

Нижче ви можете побачити кошик покупок, реалізований за допомогою патерну «модуль».
Цей модуль знаходиться в глобальному об'єкті `basketModule`, і містить
все, що йому необхідно. Масив `basket`, що знаходиться всередині модуля приватний
і інші частини вашого додатки не можуть безпосередньо взаємодіяти з ним.
Масив `basket` існує всередині замикання, створеного модулем, і
взаємодіяти з ним можуть тільки методи, що знаходяться в тій же області
видимості (наприклад, `addItem()`, `getItem()`).

{% highlight javascript %}
var basketModule = (function() {
    var basket = []; //private
    return { //exposed to public
        addItem: function(values) {
            basket.push(values);
        },
        getItemCount: function() {
            return basket.length;
        },
        getTotal: function(){
           var q = this.getItemCount(),p=0;
            while(q--){
                p+= basket[q].price; 
            }
            return p;
        }
    }
}());
{% endhighlight %}

Всередині модуля, як ви помітили, ми повертаємо об'єкт. Цей об'єкт автоматично
присвоюється змінній `basketModule`, так що з ним можна взаємодіяти
наступним чином:

{% highlight javascript %}
//basketModule це об'єкт з властивостями, які можуть також бути і методами:
basketModule.addItem({item:'bread',price:0.5});
basketModule.addItem({item:'butter',price:0.3});

console.log(basketModule.getItemCount());
console.log(basketModule.getTotal());

// А наступний нижче код працювати не буде:
console.log(basketModule.basket);
// (повертається undefined тому, що не входить в об'єкт)
console.log(basket);
// (масив доступний тільки з замикання)
{% endhighlight %}


Методи вище, фактично, поміщені в неймспейс `basketModule`.

Історично, патерн «модуль» був розроблений у 2003 році групою людей, в число
яких входив [Richard Cornford][4]. Пізніше, цей патерн був популяризований
Дугласом Крокфордом в його лекціях, і відкритий заново в блозі YUI завдяки Eric
Miraglia.

Як щодо того, щоб подивитися на реалізацію «модуля» в різних бібліотеках
і фреймворках.

**Dojo** 

Dojo намагається забезпечувати поведінку схоже на класи, через `dojo.declare`,
який, крім створення «модулів», також використовується і для інших речей.
Давайте спробуємо, для прикладу, визначити `basket` як модуль всередині неймспейса
`store`:

{% highlight javascript %}
// традиційний спосіб
var store = window.store || {};
store.basket = store.basket || {};

// за допомогою dojo.setObject
dojo.setObject("store.basket.object", (function() {
    var basket = [];
    function privateMethod() {
        console.log(basket);
    }
    return {
        publicMethod: function(){
            privateMethod();
        }
    };
}()));
{% endhighlight %}

Кращого результату можна домогтися, використовуючи `dojo.provide` і міксини.

**YUI** 


Наступний код, здебільшого, заснований на прикладі реалізації патерну
«модуль» у фреймворку YUI, розробленим Eric Miraglia, але трохи більше
самодокументований.

{% highlight javascript %}
YAHOO.store.basket = function () {

    //приватная переменная:
    var myPrivateVar = "До мене можна отримати доступ тільки з YAHOO.store.basket.";

    //приватный метод:
    var myPrivateMethod = function () {
        YAHOO.log("Я доступний тільки при виклику з YAHOO.store.basket");
    }

    return {
        myPublicProperty: "Я - публічна властивість",
        myPublicMethod: function () {
            YAHOO.log("Я - публічна метод");

            //Будучи всередині кошика я можу отримати доступ до приватних змінних і методів:
            YAHOO.log(myPrivateVar);
            YAHOO.log(myPrivateMethod());

            //The native scope of myPublicMethod is store so we can
            //access public members using "this":
            YAHOO.log(this.myPublicProperty);
        }
    };

}();
{% endhighlight %}


**jQuery** 

There are a number of ways in which jQuery code unspecific to plugins can be
wrapped inside the module pattern. Ben Cherry previously suggested an 
implementation where a function wrapper is used around module definitions in the
event of there being a number of commonalities between modules.

<!-- #####
Існує ряд способів, за допомогою яких jQuery код неспецифічні для плагінів можуть бути
всередині, загорнутий шаблон модуля. Ben Cherry раніше запропонував
реалізація функції-оболонки використовується у всьому модулі визначень
подія, що існує ряд спільних рис між модулями.

Є цілий ряд способів, якими JQuery код неспецифічна плагінам можна обернути
усередині шаблону модуля. Ben Cherry раніше запропонував реалізацію,
коли функція-обгортка використовується навколо модуля
в разі там бути ряд спільних рис між модулями.
-->

У наступному прикладі, функція `library` використовується для декларації нової
бібліотеки автоматично, при її створенні(ie. модуля),
пов'язуючи виклик `init` з `document.ready`.

{% highlight javascript %}
function library(module) {
  $(function() {
    if (module.init) {
      module.init();
    }
  });
  return module;
}

var myLibrary = library(function() {
   return {
     init: function() {
       /*implementation*/
     }
   };
}());
{% endhighlight %}

{:class="message"}
**Посилання по темі:**

[Ben Cherry - The Module Pattern In-Depth][5]  
[John Hann - The Future Is Modules, Not Frameworks][6]  
[Nathan Smith - A Module pattern aliased window and document gist][7]  
[David Litmark - An Introduction To The Revealing Module Pattern][8]  


[3]: http://benalman.com/news/2010/11/immediately-invoked-function-expression/
[4]: http://groups.google.com/group/comp.lang.javascript/msg/9f58bd11bd67d937
[5]: http://www.adequatelygood.com/2010/3/JavaScript-Module-Pattern-In-Depth
[6]: http://lanyrd.com/2011/jsconf/sfgdk/
[7]: https://gist.github.com/274388
[8]: http://blog.davidlitmark.com/post/6009004931/an-introduction-to-the-revealing-module-pattern
