시작하기 전에 블로킹/논블로킹은 동기/비동기와 다르다는 것을 알고 지나가야 한다.

간단하게 비교하면

### 동기 & 비동기

**작업 완료**를 누가 신경 쓰는가?

> Synchronous : 작업을 동시에 수행하거나, 동시에 끝나거나, 끝나는 동시에 시작. ‘**호출한 함수**’가 스스로 신경쓴다.
>
> Asynchronous : 시작, 종료가 일치하지 않으며, 끝나는 동시에 시작을 하지 않음, ‘**호출된 함수 (callback 함수)**’ 가 신경쓰고, ‘호출한 함수’는 신경쓰지 않는다.

### 블로킹 & 논블로킹

호출되는 함수가 return 되는가? 호출한 함수가 **제어권**을 넘겨주는가?

<aside>
✔️ 제어권 : 자신(함수)의 코드를 실행할 권리 같은 것이다. 제어권을 가진 함수는 자신의 코드를 끝까지 실행한 후, 자신을 호출한 함수에게 돌려준다.

</aside>

> Blocking : ‘호출된 함수’가 자신의 작업을 모두 마칠 때까지 ‘호출한 함수’에게 제어권을 넘겨주지 않고 대기한다.
>
> Non-Blocking : ‘호출된 함수’에게 제어권이 넘어가지않고, ‘호출한 함수’가 제어권을 가지고 계속해서 다른 일을 한다.

동기/비동기, 블로킹/논블로킹의 판단 기준은 아래와 같다.

<aside>
🗣️ 동기/비동기 → 작업완료

블로킹/논블로킹 → 제어권

</aside>

## 💡동기/비동기

### 동기 (Synchronous)

요청과 결과가 동시에 일어난다. 어떤 작업에 대한 요청이 발생했을 때, 그 요청에 대한 응답을 받을 때까지 대기해야 한다. 작업에 대한 완료를 ‘호출한 함수’가 신경을 쓰고 있다.

![Untitled](https://user-images.githubusercontent.com/101804857/204139636-e5b8b22c-2d9a-4d8e-ab70-43671d6b499d.png)

Thread1, Thread2가 존재할 때 Thread1에서 처리하려고 했던 일을 Thread2에게 보낸 경우, Thread2가 해당 작업을 수행하는 동안 Thread1은 Thread2가 끝날 때까지 대기상태다.

![Untitled (1)](https://user-images.githubusercontent.com/101804857/204139662-889a86a3-9503-4085-9a18-122fa661bfc3.png)
Thread1이 작업1, 작업2, 작업3, 작업4를 가지고 있다.

![Untitled (2)](https://user-images.githubusercontent.com/101804857/204139696-edf5adcc-29ea-46b4-a6b7-a1f570a434e4.png)

Thread1이 작업을 Thread2에게 보낸다. Thread1은 Thread2의 작업이 끝난 후부터 2라는 작업을 처리할 수 있다.

![Untitled (3)](https://user-images.githubusercontent.com/101804857/204139723-d132f6d6-aabe-415e-83b6-6eb6c81d0466.png)

### 비동기 (Asynchronous)

요청과 결과가 동시에 일어나지 않는다. 작업에 대한 완료를 ‘호출 함수’가 아닌 ‘callback’이 신경쓴다.

![Untitled (4)](https://user-images.githubusercontent.com/101804857/204139764-0469a12b-8d86-4508-b67c-7f54070e0d67.png)

Thread1, Thread2가 존재할 때 Thread1에서 처리하려고 했던 일을 Thread2에게 보낸 경우, THread2가 해당 작업을 수행하는 동안 Thread1은 대기 없이 나머지 Task2, Task3를 실행한다.

![Thread1이 작업1, 작업2, 작업3, 작업4를 가지고 있다.](https://user-images.githubusercontent.com/101804857/204139780-ce391704-946a-4330-aed0-ba2ee060b04b.png)

Thread1이 작업1, 작업2, 작업3, 작업4를 가지고 있다.

![Untitled (6)](https://user-images.githubusercontent.com/101804857/204139882-23d03526-3354-4899-a0df-0e903f725bdc.png)

Thread1이 작업1을 Thread2에게 보낸다. Thread1은 Thread2의 작업의 완료 여부에 상관 없이 계속 실행한다.

![Untitled (7)](https://user-images.githubusercontent.com/101804857/204139916-e353272b-a906-4a11-8317-aeadd09c64b8.png)

## 💡블로킹/논블로킹

### 블로킹 (Blocking)

![Untitled (8)](https://user-images.githubusercontent.com/101804857/204139930-140705dd-da0f-47df-af24-717618c8be9b.png)

A함수가 실행될 때 제어권이 B함수로 넘어가게 되면 A함수는 제어권이 없는 상태로, B함수의 종료까지 대기 해야한다. 작업은 총 A함수, B함수 2개를 가진 상태이며, 다른 작업을 하는 동안 자신의 작업에 대한 제어권이 없을 때 다른 작업이 끝날 때까지 기다렸다가 자신의 작업을 멈췄던 부분부터 이어나간다.

![Untitled (9)](https://user-images.githubusercontent.com/101804857/204139953-17594977-9fa8-4a00-a072-1c691972fccd.png)

### 논블로킹 (Non-Blocking)

![Untitled (10)](https://user-images.githubusercontent.com/101804857/204139972-89f8b66d-cc13-4f28-a4c3-8ecc4ad44c41.png)

A함수가 실행되는 도중에 B함수가 호출되었다. A함수는 제어권을 B함수에 넘기지 않고 그대로 자신이 가지고 있으며 B함수의 완료 여부와 상관없이 작업을 계속 진행한다.

![Untitled (11)](https://user-images.githubusercontent.com/101804857/204139993-1e1681fe-71ad-4f78-a2e3-32acc99252ef.png)

## 💡조합

### 동기 (Synchronous) / 블로킹 (Blocking)

> 동기 (Synchronous) : 호출한 함수가 작업 완료 여부를 확인한다.
>
> 블로킹 (Blocking) : 호출된 함수 (task1)이 제어권을 가진다.

![Untitled (12)](https://user-images.githubusercontent.com/101804857/204140007-8f79f8c3-ac29-411f-8478-9be9972fc74f.png)

Thread1은 Task1이 끝날 때까지 아무것도 하지 못하고 대기해야 한다. 실행, 흐름이 순차적이기 때문에 프로그램을 제어하기 쉽다.

![Untitled (13)](https://user-images.githubusercontent.com/101804857/204140023-ac385c62-5485-4ad2-a582-312d015f3936.png)

**예제코드**

<center>
<img src="https://user-images.githubusercontent.com/101804857/204140665-666f6fe5-5417-4e90-875a-ee4c76400c67.png" width="80%">

<img src="https://user-images.githubusercontent.com/101804857/204140742-70e7adf7-f9e7-4242-b814-908fd926e44b.png" width="80%">
</center>

![Untitled (15)](https://user-images.githubusercontent.com/101804857/204140159-75a54cb3-07dc-4686-98b9-10c8fc8b0fd5.png)

모든 인형의 눈알을 다 붙히기 전까지 퇴근은 없다.

상위 프로세스인 boss 함수는 출근 작업을 수행한 뒤 하위 프로세스인 employee 함수에게 인형 눈알 붙히기 작업을 요청하고 있고, 이 인형 눈알 붙히기 작업이 완료되고나서야 boss 함수는 퇴근을 할 수 있다.

쉽게 말해서 작업을 시킨 놈인 상위 프로세스는 작업을 하는 놈인 하위 프로세스가 종료될 때까지 절대 퇴근할 수 없다.

**그렇다면 이 예제와 같이 동기적인 작업의 흐름을 유지하면서 employee 함수가 인형의 눈알을 붙히는 동안 boss 함수가 다른 일을 할 수 있을까 ?**

### 동기 (Synchronous) / 논블로킹 (Non-Blocking)

> 동기 (Synchronous) : 호출한 함수가 작업 완료 여부를 확인한다.
>
> 논블로킹 (Non-Blocking) : 호출한 함수가 제어권을 가진다.

물론 할 수 있다.

동기라는 것은 작업들이 순차적인 흐름을 가지고 있다는 것을 의미하기 때문에, 이 전제만 지켜진다면 나머지는 어떤 일이 발생하든 동기 방식이라는 것은 변하지 않는다.

**_그래서 동기 === 블로킹이라고 말할 수 없다._**

![Untitled (16)](https://user-images.githubusercontent.com/101804857/204140192-655518f6-5823-4f89-a22c-beec9a7e93b2.png)

Thread1은 task1의 완료 여부에 상관 없이 다른 작업을 진행 할 수 있다. 하지만 task1의 완료 여부를 지속적으로 확인한다.

![Untitled (17)](https://user-images.githubusercontent.com/101804857/204140211-e503f149-371e-40cc-accb-b9ae334a2cfb.png)

**예제코드**

<center>
<img src="https://user-images.githubusercontent.com/101804857/204140977-049ac8f8-f487-457b-b65f-0f738ffaa2ee.png" width="80%">

<img src="https://user-images.githubusercontent.com/101804857/204141010-c722d65a-47b6-4b11-9812-84be241ba81b.png" width="80%">
</center>

![Untitled (20)](https://user-images.githubusercontent.com/101804857/204140272-74bc8b8f-3224-4e77-8a4c-8463ee9a419c.png)

이 예제를 보면 상위 프로세스인 boss 함수는 출근한 후 하위 프로세스인 employee를 호출하여 인형 눈알 붙히기 작업을 시키고 주기적으로 이 작업이 끝났는지 검사하고 있다.

그리고 아직 작업이 끝나지 않았다면 자신 또한 유튜브 시청을 수행한다.

이 코드는 분명히 동기적인 흐름을 가지고 진행하지만 boss 함수 또한 중간중간 작업을 수행하고 있으므로 블로킹이 아니라 논블로킹 방식을 사용하고 있다.

동기 & 블로킹 방식과 마찬가지로 boss 함수는 employee 함수의 작업이 끝나기 전까지는 퇴근을 하지 않는다.

작업의 순서가 지켜지고 있다.

즉, 동기 방식이라는 것은 작업의 순차적인 흐름만 지켜진다면 블로킹이든 논블로킹이든 아무 상관 없다고 할 수 있다.

### 비동기 (Asynchronous) / 블로킹 (Blocking)

> 비동기 (Asynchronous) : Callback 함수가 작업 완료 여부를 확인한다.
>
> 블로킹 (Blocking) : 호출된 함수 (task1)이 제어권을 가진다.

![Untitled (21)](https://user-images.githubusercontent.com/101804857/204140284-3785c87f-80c2-4dd8-b343-d4d6c711b444.png)

Thread1이 task1의 작업 완료 여부를 신경쓰지 않으나, 작업이 완료될 때까지 아무것도 하지 못하는 대기 상태이다. 해당 경우는 비동기인데 굳이 블로킹인데, 이는 보통 비동기 + 논블로킹으로 작업을 시도했을 때 잘못된 경우 발생한다.

![Untitled (22)](https://user-images.githubusercontent.com/101804857/204140323-a57990da-bebb-43bf-adfe-ed1f5e931215.png)

### 비동기 (Asynchronous) / 논블로킹 (Non-Blocking)

> 비동기 (Asynchronous) : Callback 함수가 작업 완료 여부를 확인한다.
>
> 논블로킹 (Non-Blocking) : 호출된 함수가 제어권을 가진다.

![Untitled (23)](https://user-images.githubusercontent.com/101804857/204140339-64207044-9fce-4233-ac5b-b65a744cde8c.png)

Thread1이 task1의 완료 여부를 신경쓰지 않고 다른 자신의 작업을 진행한다. 성능과 자원 효율면에서 가장 우수하다.

![Untitled (24)](https://user-images.githubusercontent.com/101804857/204140349-a4ce0f7e-bfb1-4612-b8f6-7b1f68e367df.png)

비동기 방식이기 때문에 상위 프로세스는 하위 프로세스의 작업 완료 여부를 따로 신경쓰지 않는다.

이후 하위 프로세스의 작업이 종료되면 스스로 상위 프로세스에게 보고를 하든 아니면 다른 프로세스에게 일을 맡기든 할 것이다.

그리고 논블로킹 방식이기 때문에 상위 프로세스는 하위 프로세스에게 일을 맡기고 자신의 작업을 계속 수행할 수도 있다.

**예제코드**

<center>
<img src="https://user-images.githubusercontent.com/101804857/204141089-ac678cea-a6b5-4106-84bd-11e6cca38e55.png" width="80%">

<img src="https://user-images.githubusercontent.com/101804857/204141123-b6b77fb2-7966-4635-9e1d-76c00435b8a1.png" width="80%">
</center>

![Untitled (27)](https://user-images.githubusercontent.com/101804857/204140404-7d9a9d19-7ca4-4b60-a9f6-8d1535e6dd6b.png)

이 예제를 보면 boss 함수는 employee 함수에게 인형 눈알 100개를 붙히라고 지시한 후 바로 퇴근하였다.

상위 프로세스인 boss 함수는 employee 함수의 작업이 언제 끝나는지 관심이 없으며,

작업의 완료 신호는 콜백으로 넘겨진 눈알 결산 보고 작업이 대신 받아서 처리하고 있다.

<aside>
🗣️ 비동기 & 논블로킹 방식은 여러 개의 작업을 동시에 처리할 수 있는 부분에서 효율적으로 할 수 있지만, 너무 복잡하게 얽힌 비동기 처리 때문에 개발자가 어플리케이션의 흐름을 일기 어려워지는 등의 문제가 있을 수 있다.

Javascript에서 Promise나 async/await와 같은 문법을 사용하는 이유도 이런 비동기 처리의 흐름을 좀 더 명확하게 인지하고자 하는 노력이다.

</aside>
