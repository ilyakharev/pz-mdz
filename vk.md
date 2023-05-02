Формирование https запроса в вк:
* [ ] Получить `access_token`
  * Зайти на сайт `https://vkhost.github.io/`
  * Выполнить инструкции на сайте
* [ ] Выбрать метод на сайте `https://dev.vk.com/`.
  Например `https://dev.vk.com/method/friends.get`
  * В конструкторе примера на сайте выбрать необходимые параметры
* [ ] Ввести тип запроса `GET`
* [ ] Ввести адресс запроса
  * [ ] Ввести `https://api.vk.com/method/`
  * [ ] Ввсети необходимое название метода. Например `friends.get`
  * [ ] Ввести `?v=5.131&access_token=`
  * [ ] Ввсети `access_token`, полученный во втором пункте
  * [ ] Ввести все остальные параметры, которые мы вводили в конструкторе в таком формате
  `&параметр=значение`
    * Например: `&user_id=366600186&fields=city,dbate`
Пример запроса `GET https://api.vk.com/method/friends.get?v=5.131&access_token=vk1.a.WWWWWWWWWWWWWWWWWWWWWW&user_id=366600186&fields=city,dbate`
