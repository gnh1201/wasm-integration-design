Note: This document is written in Korean.

## 문제 정의
이전 작업에서 PHP 4 세대(CGI-style)과 PHP 7 세대(OOP-style) 사이의 코딩 컨벤션(관습) 차이를 해소하기 위해 [ReasonableFramework](https://github.com/gnh1201/reasonableframework)를 출시하였다. 이것은 어중간한 세대를 구현하기 때문에 학문적 관점에선 비판받을 수 있으나 산업적 관점에서는 매우 다양한 실용적 효과를 보았다.

하지만 근본적인 해결책을 제시한다고 하기에는 부족한 점이 많았으며 한걸음 더 나아간 방법이 필요했다.

본 디자인에서는 WebAssembly을 채택하여 해당 문제에 대한 근본적인 해결로 나아감은 물론, PHP 기반의 아이디어가 다른 플랫폼에서도 사용될 가능성까지 확보한다.

## 제안 방법
PHP에서 WASM을 사용하는 공식적인 방법으로는 [wasmerio/wasmer-php](https://github.com/wasmerio/wasmer-php)가 있지만 모듈 컴파일과 `phpize`를 요구하기 때문에 공유 리눅스(Shared Linux)에서 사용할 수 없다.

Wordpress를 포함하여 절대 다수의 PHP 프로젝트가 공유 리눅스 환경에서 돌아가므로 실제로 많은 것이 제한되어 있다. 실례로, 한국 기준으로 어느 웹 호스팅사는 PHP 외 다른 프로세스로 부터 일어나는 스레드 분기(Thread Fork)에 제한을 두고 있어 WASM 컴파일러의 JIT(Just-In-Time)를 구현할 수 없었다.

  * 사례: https://github.com/bytecodealliance/wasm-micro-runtime/issues/802

이러한 제한된 리눅스에 어느정도 자유도를 부여하는 방법으로 잘 알려진 프로젝트로는 [BusyBox](https://busybox.net/)가 있으며, 이와 비슷하게 사용할 수 있는 가장 적합한 WASM 머신으로는 [iwasm](https://github.com/bytecodealliance/wasm-micro-runtime)이 있다. 머신 및 WASM 빌드 방법은 아래 링크에 소개되어 있다.

타겟 웹 호스팅에 맞게 정상적으로 iwasm을 컴파일했다면, PHP에서 쉘 명령을 허용하는 함수(exec, exec_shell 등)을 이용하여 WASM으로 작성된 함수를 호출할 수 있다.

  * 사례: https://github.com/gnh1201/reasonableframework/blob/master/helper/exectool.php

wasm으로 컴파일된 파일은 PHP 말고도 다른 플랫폼 기반의 프로젝트에서도 사용할 수 있다.

## 향후 계획
위 작업을 종합적으로 리뷰해보았을 때 WASM으로 워드프레스 플러그인을 개발하는 것을 실증할 수 있지 않을까 한다.
