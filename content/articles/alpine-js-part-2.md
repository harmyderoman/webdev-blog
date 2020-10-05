---
title: Alpine.js — продолжаем знакомство
description: "Это вторая статья о перспективном фреймворке Alpine.js."
img: https://fonsekainnovations.com/app/uploads/2020/07/cover_d19731da547ed34f135d5d077ac73136_2000.jpg
alt: Alpine.js
author:
  name: Роман
  bio: Web-developer
  img: https://avatars2.githubusercontent.com/u/47144873?s=460&u=44b99fa1556ab16f32a934f331859f58f37a5825&v=4
tags:
  - web development
  - Alpine.js
  - JavaScript
---

Это вторая статья о перспективном фреймворке Alpine.js.
Первую можно прочитать по ссылке:
Alpine.js — легковесный фреймворк с удобным синтаксисом

Продолжаем знакомить сообщество с этим замечательным инструментом.
Полный код урока здесь.

##X-SHOW и X-CLOAK

    <div x-data="{ show: false }">
      <button x-on:click="show= ! show" x-text="show ? 'Скрыть' : 'Показать' ">
      </button>
      <p x-show="show" x-cloak>Привет!</p>
    </div>

В примере выше разберем сразу две новые директивы. X-show хоть и похожа по применению с x-if, но не стоит их путать. X-show не меняет древо страницы, а только управляет свойством display у элемента. То есть элемент будет отрендерен, но не показан.

X-cloak это директива, которая удаляется сразу после инициации скрипта. Казалось бы, какой в ней толк? Рассмотрим практическое применение. Когда мы запускаем код выше, на какое-то мгновение мы видим элемент с текстом «Привет!», несмотря на значение false в директиве x-show. Тут нам на помощь приходит x-cloak, нужно только прописать ему следующий стиль:

    [x-cloak] {
      display: none;
    }

Теперь элемент будет скрыт до начала работы скрипта.

Внезапное появление и исчезновение элемента не очень наглядно, поэтому для анимации этих событий используется модификатор .transition.

##X-SHOW.TRANSITION

Улучшим предыдущий код с помощью модификаторов:

    <div x-data="{ show: false }">
      <button x-on:click="show= ! show" x-text="show ? 'Скрыть': 'Показать' ">
      </button>
      <p x-show="show"> NO TRANSITION</p>
      <p x-show.transition="show">TRANSITION</p>
      <p x-show.transition.in.duration.1000ms="show">TRANSITION IN</p>
      <p x-show.transition.out.duration.1000ms="show">TRANSITION OUT</p>
      <p x-show.transition.scale.05.duration.1000ms="show">TRANSITION Scale</p>
      <p x-show.transition.opacity.in.duration.1000ms="show">TRANSITION Opacity</p>
      <p x-show.transition.in.duration.1000ms.out.duration.500ms="show">TRANSITION IN Duration</p>
    </div>

Разные вариации использования мы расположили друг под другом и увеличили время действия до 1 секунды с помощью цепочки модификаторов .in.duration.1000ms

По умолчанию transition (без других модификаторов) использует для анимации свойства opacity и scale ( scale: 0.95), временную функцию cubic-bezier(0.4, 0.0, 0.2, 1), 150 миллисекунд для анимации появления и 75 для исчезновения. Эти изменения малозаметны для глаза, поэтому для наглядности мы удлинили действие последующих скриптов.

Дополнительные модификаторы позволяют разнообразить и подчеркнуть работу анимации:

.in позволяет установить только анимацию появления;
.out — для анимации исчезновения;
.scale — изменение в размерах элемента;
.opacity — для анимации прозрачности;
.duration задает продолжительность в миллисекундах.

Для анимации появления и исчезновения элемента с директивой x-if используется специальная директива x-transition. Она предоставляет еще более гибкий функционал и работает также и на x-show. Её мы рассмотрим в следующий раз. Для нетерпеливых есть ссылка .

##X-INIT

X-init — директива, получающая блок кода, который вы хотите выполнить во время инициации компонента. Похожа на использование created во Vue.js. Основное применение это вычисление начальных значений в x-data. Рассмотрим ее на примере:

    <div
      x-data="{ posts: [] }"
      x-init="
        fetch('https://jsonplaceholder.typicode.com/albums/1/photos')
          .then((response) => {
            return response.json();
          })
          .then((data) => {
            posts = data
          });
        "
    >
      <template x-for="post in posts" :key="post.id">
        <div class="post">
          <h1 x-text="post.title"></h1>
          <img :src="post.thumbnailUrl" :alt="post.title" />
        </div>
      </template>
      <hr/>
    </div>

В x-data у нас есть объект posts, который по умолчанию не содержит ничего. В директиве x-init мы получаем данные с сервера с помощью функции fetch и после преобразования в json-формат присваиваем нашему объекту. Потом с помощью x-for можно отобразить полученные «посты» на странице.

##X-REF и \$REFS

Эта директива предоставляет доступ к нужному элементу в любой части страницы, в том числе и вне компонента. Все ссылки становятся доступными через глобальное поле \$refs. Продемонстрируем эту возможность на примере:

    <div x-data="{ }">
      <input type="text" x-ref="myInput"/>
      <button x-on:click="alert($refs.myInput.value)">Alert</button>
    <hr />
    </div>

Текст введенный в input с атрибутом x-ref=«myInput» будет доступен в \$refs.myInput.value

На этом наш сегодняшний урок закончен. Все примеры урока здесь.
Всем удачных проектов и до скорых встреч!
