
## Async

Создание *асинхронной* функции:

```
async function asyncFn() {
    // ....
}

const asyncFn = async () => {
    // ....
}
```

Асинхронная функция ***Всегда*** возвращает промис

Пример:
```
const asyncFn = async () => {
    return 'Succes'
}

asyncFn()
```

![[Pasted image 20240812162433.png]]
Возвращает промис уже *исполненный*, то есть *resolved*


А при ошибке *reject*, который можно ловить с помощью *catch*

```
const asyncFn = async () => {
    throw new Error()
}

asyncFn()
.then((value) => console.log(value))
.catch((error) => console.error(error))
```

![[Pasted image 20240812164352.png]]

## Await

Внутри асинхронной функции можно ожидать результат промиса с помощью await. 
==Await используется всегда вместе с async==

```
const timePromise = () =>
    new Promise((resole, reject) =>
        setTimeout(() => resole(), 2000))

  

const asyncFn = async () => {
    console.log('Start function')
    await timePromise()
    console.log('Finish function')
}

asyncFn()
```

Функция timePromise служит как ожидание ответа от сервера, "ответ" приходит через 2 секунды. В асинхронной функции asyncFn для счета времени вызываем первый console.log, затем с помощью await мы ожидаем ответа "от сервера" при этом второй console.log не вызывается. Через 2 секунды получаем ответ, выводится console.log

Реальная работа async/await для получения данных с сервера с помощью fetch
```
const getData = async (url) => {
    res = await fetch(url)
    json = await res.json()
    return json
}

getData('https://jsonplaceholder.typicode.com/posts/1')
.then(data => console.log(data))
.catch(error => console.error(error))
```

Функция getData получает ссылку, через fetch получает данные и присваивается переменной res. Затем парсится через метод json и присваивается json и возвращается, при этом ни один из последовательных команд не начинается раньше из-за await, а также await позволяет сразу же работать со значением, как бы вместо .then, а для обработки ошибок надо использовать try catch блоки Пример:

```
const getData = async (url) => {
    res = await fetch(url)
    json = await res.json()
    return json
}

const url = 'https://jsonplaceholder.typicode.com/posts/1'

try {
    const data = await getData(url)
    console.log(data)
}
catch (error) {
    console.log(error.message)
}
```

При ошибке попадаем в блок catch

==Примечание: await без async можно использовать в консоли==