# WebRTC

## Описание технологии

WebRTC (Web Real-Time Communication) - это открытый стандарт, который позволяет реализовать веб-приложения для обмена аудио, видео и данных в режиме реального времени между браузерами или между браузером и сервером без необходимости дополнительных плагинов или программного обеспечения

WebRTC использует технологии JavaScript API, которые могут быть использованы для создания веб-приложений, таких как видео-конференции, онлайн-игры, передача файлов и других приложений, где важна реальная передача данных в режиме реального времени

WebRTC поддерживается большинством современных браузеров, включая Google Chrome, Mozilla Firefox, Microsoft Edge и Safari. Он также может использоваться на мобильных устройствах с операционными системами Android и iOS. WebRTC стандартизирован W3C и IETF как открытый стандарт для услуг связи

## Основные методы

WebRTC (Web Real-Time Communication) представляет собой набор методов и протоколов для осуществления коммуникаций в режиме реального времени между веб-браузерами без необходимости установки дополнительного программного обеспечения. Основные методы WebRTC включают:

1. getUserMedia() - это API позволяет получить доступ к камере, микрофону и экрану пользователя. С его помощью можно захватывать аудио, видео и другие устройства ввода

```js
navigator.mediaDevices.getUserMedia({ video: true, audio: true })
  .then(function(stream) {
    // success, stream объект содержит медиапоток
  })
  .catch(function(error) {
    // ошибка
  });
  ```

2. RTCPeerConnection()- это основной элемент WebRTC, который отвечает за передачу потоков данных между пользователями по сети. Он обрабатывает подключение, отключение и переподключение к узлам веб-сокетов
```js
var peerConnection = new RTCPeerConnection();
```

3. RTCDataChannel() - позволяет обмениваться данными между двумя браузерами в режиме реального времени. Используется для передачи малых объемов данных, например, текстовых сообщений или информации о состоянии соединения
```js
// Создание WebRTC соединения
const peerConnection = new RTCPeerConnection();

// Создание RTCDataChannel
const dataChannel = peerConnection.createDataChannel('myDataChannel');

// Обработка открытия RTCDataChannel
dataChannel.onopen = function(event) {
  console.log('RTCDataChannel открыт');
};

// Обработка закрытия RTCDataChannel
dataChannel.onclose = function(event) {
  console.log('RTCDataChannel закрыт');
};

// Отправка сообщения через RTCDataChannel
dataChannel.send('Привет, друг!');
```

4. MediaStream() - это объект, который передает поток мультимедиа данных (аудио, видео) от устройства к браузеру. Позволяет передавать мультимедийные данные в режиме реального времени между браузерами
```js
navigator.getUserMedia = navigator.getUserMedia || navigator.webkitGetUserMedia || navigator.mozGetUserMedia;

if (navigator.getUserMedia) {
   navigator.getUserMedia({audio:true, video:true}, function(stream) {
      var video = document.querySelector('video');
      if ('srcObject' in video) {
         video.srcObject = stream;
      } else {
         video.src = window.URL.createObjectURL(stream);
      }
      video.onloadedmetadata = function(e) {
         video.play();
      };
   }, function(err) {
      console.log("The following error occurred: " + err.name);
   });
} else {
   console.log("getUserMedia not supported");
}
```

Этот код использует getUserMedia API для получения потока данных и затем отображает его на странице с помощью элемента `<video>`.

В случае, если браузер не поддерживает метод getUserMedia, код выведет сообщение об ошибке в консоль.

5. Signaling() - это не часть WebRTC, но синхронизирует узлы между собой. Он используется для установления соединения между двумя пользователями и обмена информации о конференциях или общем доступе к ресурсам. 
Вот пример кода сервера сигналинга на Node.js, который использует библиотеку Socket.IO для обмена сообщениями между двумя устройствами:
```js
// Инициализация сервера сигналинга
const app = require('express')();
const http = require('http').Server(app);
const io = require('socket.io')(http);
const port = 3000;

// Обработка подключения нового клиента
io.on('connection', (socket) => {
  console.log('a user connected');

  // Обработка отправки предложения
  socket.on('offer', (offer) => {
    console.log('offer received:', offer);

    // Отправка предложения другому клиенту
    socket.broadcast.emit('offer', offer);
  });

  // Обработка отправки ответа
  socket.on('answer', (answer) => {
    console.log('answer received:', answer);

    // Отправка ответа другому клиенту
    socket.broadcast.emit('answer', answer);
  });

  // Обработка отправки ICE-кандидата
  socket.on('candidate', (candidate) => {
    console.log('candidate received:', candidate);

    // Отправка ICE-кандидата другому клиенту
    socket.broadcast.emit('candidate', candidate);
  });

  // Обработка отключения клиента
  socket.on('disconnect', () => {
    console.log('user disconnected');
  });
});

// Запуск сервера сигналинга на порту 3000
http.listen(port, () => {
  console.log(`signaling server listening at http://localhost:${port}`);
});
```
В этом примере сервер сигналинга прослушивает подключения новых клиентов и обрабатывает сообщения, которые отправляются каждым клиентом. Когда клиент отправляет предложение, ответ или ICE-кандидат, сервер пересылает это сообщение другому клиенту. Обратите внимание, что этот пример только демонстрирует обмен сообщениями, а не реализует полноценное соединение между двумя устройствами

6. createOffer() и createAnswer() - это методы, которые создают предложение и ответ на соединение между двумя устройствами. Эти методы используются для установки соединения между двумя устройствами
```js
peerConnection.createOffer()
  .then(function(offer) {
    // success, offer содержит предложение
  })
  .catch(function(error) {
    // ошибка
  });


peerConnection.createAnswer()
  .then(function(answer) {
    // success, answer содержит ответ
  })
  .catch(function(error) {
    // ошибка
  });
```

7. setLocalDescription() и setRemoteDescription() - это методы, которые устанавливают локальное и удаленное описание соединения между двумя устройствами
```js
peerConnection.setLocalDescription(offer)
  .then(function() {
    // success, локальное описание установлено
  })
  .catch(function(error) {
    // ошибка
  });


peerConnection.setRemoteDescription(answer)
  .then(function() {
    // success, удаленное описание установлено
  })
  .catch(function(error) {
    // ошибка
  });
```

8. addIceCandidate() - это метод, который добавляет ICE-кандидата в соединение между двумя устройствами
```js
peerConnection.addIceCandidate(candidate)
  .then(function() {
    // success, ICE-кандидат добавлен
  })
  .catch(function(error) {
    // ошибка
  });
```

## Алгоритм создания простого приложения
Пример простого приложения на WebRTC может включать следующий функционал:

1. Создание соединения между двумя пользователями

2. Обмен сигналами для установления соединения

3. Передача потока аудио или видео от одного пользователя к другому

4. Возможность отправки текстовых сообщений

Для начала вы можете использовать библиотеки и фреймворки, такие как SimpleWebRTC, PeerJS, Google WebRTC API, и т.д. Они предоставляют инструменты для легкого и быстрого развертывания приложения на WebRTC.

Затем вы можете создать интерфейс пользователя с помощью HTML, CSS и Javascript. Например, у вас может быть кнопка "Подключиться", чтобы пользователь мог присоединиться к комнате с другим пользователем, и элемент управления потоков для просмотра и передачи аудио/видео.

Для обмена сигналами вы можете использовать сокеты, которые позволяют передавать сообщения между пользователями через сервер. Это позволяет пользователям установить соединение и обмениваться предварительными данными о медиа-сеансе.

Наконец, вы можете использовать WebRTC API для передачи потока аудио и видео между пользователями. В API есть методы для управления потоками, настройки качества, приватности и т.д.
