# Chapter02. 파일 입출력

UNIX System에서는 거의 모든 것을 `File`로 표현합니다.

File은 Read하거나 Writ하기 전에 반드시 Open해야합니다.

## Linux Process가 열 수 있는 최대 File 개수

기본적으로 Linux Process가 열 수 있는 최대 File 개수는 1,024지만 `1,048,576`까지 설정이 가능합니다.

## File Descriptor

user area와 kernel area 모두에서 Process 내의 고유 식별자로 사용한다.

File을 열면 File Descriptor가 반환되고 이 File Descriptor를 관련 System Call의 첫 번째 인자로 넘겨 읽기, 쓰기 등을 수행한다.

Descriptor는 음수를 허용하지 않기 때문에 Error가 발생하는 경우 -1을 반환한다.

단순히 일반 파일만 나타내는 것이 아니라, File Descriptor는 장치 파일, 파이프, 디렉토리, 퓨텍스, FIFO, Socket 접근에도 사용되며 모든 것이 File이라는 UNIX 철학에 따라 읽고 쓸 수 있는 모든 것은 File Descriptor를 통해 접근할 수 있다.

### 명시적으로 닫지 않는 이상 살아있는 0, 1, 2

- 0: STDIN_FILENO (stdin)
- 1: STDOUT_FILENO (sterr)
- 2: STDERR_FILENO (stout)

사용자는 이런 표준 File Descriptor를 `redirect`하거나 `pipe`를 사용해서 프로그램의 출력을 다른 프로그램의 입력으로 redirect할 수도 있다.

## File Table

Kernel은 Process 별로 열린 File 목록을 관리하는 File Table을 가지고 있습니다.

File Table에는 Memory에 복사괸 inode를 가리키는 Pointer와 다양한 Metadata (File 위치와 접근 모드)가 포함되어 있습니다.

기본적으로 자식 프로세스는 부모 프로세스가 소유한 파일 테이블의 복사본을 상속 받는다.
