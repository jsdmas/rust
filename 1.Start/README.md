# rust 🦀

## rustup 도구 설치 명령어 - Linux || macOS

```bash
curl --proto "=https" --tlsv1.2 https://sh.rustup.rs -sSf | sh

# Rust is installed now. Great!

# install 확인 명령어
# runstc --version

# rust update
# rustup update

# rust delete
# rustup self uninstall

# rust doc (offline - local)
# rustup doc
```

### IDE extension

- rust-analyzer
  - 코드 완성
  - 실시간 오류 감지
  - 타입 추론 & 정보
  - 이동
  - 문서 및 참조
  - 코드 포맷팅
  - 리팩터링
  - LSP 기반 (언어 서버 프로토콜)
- rustfmt
  - 자동 포맷팅 도구

## main

```rust
fn main() {

}
```

- `main`함수는 특별한 함수로, 러스트 실행 프로그램에서 항상 가장 먼저 실행되는 함수를 뜻한다.
- 매개변수가 있을떄는 `()`안쪽에 작성한다.
- 함수 본문은 `{}`로 감싸진다.

### 컴파일과 실행은 별개의 과정이다.

러스트 프로그램을 실행하기 전, 아래처럼 `rustc` 명령어에 소스 파일명을 넘겨주어 컴파일 하는 과정이 있다.

```bash
rustrc main.rs
```

C나 C++ 의 `gcc`, `clang`사용 방법과 비슷하다.  
러스트는 소스 파일 컴파일에 성공하면 실행 가능한 바이너리를 만들어 낸다.

- ruby, python, javaScript 등 명령어 한 줄로 프로그램을 컴파일하고 실행할 수 있는 동적 프로그래밍 언어와 다르다.
  - 위의 언어들은 .rb, .py, .js 파일을 다른 곳에서 실행하려면 해당 언어의 구현체를 설치해야만 한다.
- 반면, 러스트는 AOT(ahead-of-time-compiled) 언어로, 컴파일과 실행이 별개인 대신 프로그램을 컴파일하여 만든 실행 파일을 러스트가 설치되지 않은 곳에서도 실행할 수 있다.

## Cargo

- 카고 (Cargo)는 러스트 빌드 시스템 및 패키지 매니저다.
- 코드 빌드나, 코드 작성에 필요한 외부 라이브러리를 다운로드 할 때나, 라이브러리를 제작할 때 겪는 귀찮은 일들을 상당수 줄여주는 도구이다.

```bash
# cargo 설치 확인
cargo --version
```

### 카고로 프로젝트 생성하기

```bash
cargo new hello_cargo
```

```toml
[package]
name = "hello_cargo"
version = "0.1.0"
edition = "2024"

[dependencies]
```

- Cargo.toml
  - 이 파일은 TOML(Tom's Obvious, Minimal Language) 포맷으로 되어있고, 이 포맷은 카고 설정에서 사용하는 포맷이다.
- `[package]` 라고 적힌 첫 번쨰 라인은 섹션 헤더로, 뒤에 패캐지 설정 구문들이 따라오는 걸 볼 수 있다.
- `[dependencies]` 는 프로젝트에서 사용하는 의존성 목록이다.
  - 러스트에서는 코드 패키지를 크레이트(crate)라고 부른다.

### 카고로 프로젝트 빌드하고 실행하기

```bash
cargo build
#    Compiling hello_cargo v0.1.0 (/Users/jangjinho/Documents/Rust/1.Start/hello_cargo)
#    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.39s
```

- 이 명령어는 현재 디렉터리가 아닌 target/debug/hello_cargo 에 실행 파일을 생성한다.
- 기본 빌드가 디버그 빌드기 때문에, 카고는 debug라는 디렉터리에 바이너리를 생성한다.

```bash
./target/debug/hello_cargo
# Hello, World!
```

- 처음 빌드를 수행한다면 Cargo.lock 파일이 생성되는데, 이 파일은 프로젝트에서 사용하는 의존성의 정확한 버전을 자동으로 기록하는 파일이다.
- `cargo build`로 빌드한 후 `./target/debug/hello_cargo` 명령어로 실행했지만, 컴파일과 실행을 한번에 진행하는 `cargo run` 명령어도 있다.

```bash
cargo run
#    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.02s
#     Running `target/debug/hello_cargo`
# Hello, world!
```

- `cargo run`을 사용하면 `cargo build` 실행 후 바이너리 경로를 입력해서 실행하는 것보다 편리하므로, 대부분의 개발자들이 `cargo run`을 이용한다.

출력 내용에 `hello_cargo`를 컴파일 중이라는 내용이 없다면 이는 카고가 파일 변경 사항이 없음을 알아채고 기존 바이너리를 그대로 실행했기 때문이다.  
소스 코드를 수정한 뒤 명령어를 다시 실행해 보면 다음과 같은 프로젝트를 다시 빌드한 후에 바이너리를 실행함을 알 수 있다.

```bash
cargo run
#   Compiling hello_cargo v0.1.0 (/Users/jangjinho/Documents/Rust/1.Start/hello_cargo)
#    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.47s
#     Running `target/debug/hello_cargo`
# Hello, world!
# Hello, world!!
```

카고에는 `cargo check` 라는 명령어도 있는데, 이는 실행 파일은 생성하지 않고 작성한 소스가 문제없이 컴파일되는지만 빠르게 확인하는 명령어이다.

```bash
cargo check
#    Checking hello_cargo v0.1.0 (/Users/jangjinho/Documents/Rust/1.Start/hello_cargo)
#    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.21s
```

- 실행 파일도 생성하지 않는 명령어가 왜 필요한가?
  - `cargo check`는 실행 파일을 생성하는 단계를 건너뛰기 때문에 `cargo build`보다 훨씬 빠르다.
  - 코드를 작성하는 동안 프로젝트가 컴파일되는지 지속적으로 검사하려면, `cargo check` 사용이 코드가 계속 컴파일되는지 확인하는 과정을 빠르게 해준다.

## 정리

- `cargo new`로 새 프로젝트 생성 가능
- `cargo build` 명령으로 프로젝트를 빌드 가능
- `cargo run` 명령어는 한 번에 프로젝트를 빌드하고 실행
- `cargo check` 명령으로 바이너리를 생성하지 않고 프로젝트의 에러를 체크할 수 있음
- 빌드로 만들어진 파일은 작성한 소스 코드와 뒤섞이지 않도록 target/debug 디렉터리에 저장됨
- 운영체제와 상관없이 같은 명령어를 사용한다.

## 릴리즈 빌드 생성

- `cargo build --release` 명령어를 사용해 릴리즈 빌드를 생성 가능.
- 일반 빌드와 차이점
  - target/debug 가 아닌 target/release 에 실행 파일이 생성된다는 점
  - 컴파일 시 최적화를 진행하여 컴파일이 오래 걸리는 대신 러스트 코드가 더 빠르게 작동하는 점
- 릴리즈 빌드가 더 빠르게 작동한다면, 왜 일반 빌드시에는 최적화를 진행하지 않는가?
  - 개발 중에는 빌드가 잦으며 작업의 흐름을 끊지 않기 위해 빌드 속도 또한 빠를수록 좋지만, 배포용 프로그램은 잦은 빌드가 필요 없으며 빌드 속도보단 프로그램의 작동 속도가 더 중요하기 때문.
  - 작성한 코드 작동 속도를 벤치마킹할 시에는 릴리즈 빌드를 기준으로 해야 한다.
