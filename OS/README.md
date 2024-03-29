# 프로세스와 스레드

## 프로세스

프로세스란 운영체제로부터 자원을 할당받은, 실행중인 프로그램이다.

### 프로세스 제어 블록 (Process Control Block, PCB)

운영체제가 프로세스를 관리하기위해 관련 정보를 저장하는 테이블 자료구조, 메모리 상에서 커널이 관리하는 커널 스페이스에 저장된다.
저장되는 정보는 운영체제마다 차이가 있지만 일반적으로는 다음과 같다.

- 프로세스 번호
- 프로세스 상태 (준비, 실행, 대기, 보류 등의 상태)
- 프로세스 스케줄링 정보 : 우선순위
- 프로그램 카운터 값 : 다음에 실행될 명령어 주소를 가지고 있다.
- 메모리 포인터 : 페이지 테이블 또는 세그먼트 테이블 등과 같은 정보를 포함
- 문맥 데이터 : 문맥 교환시에 CPU 레지스터 값들을 저장하는 영역
- 할당받은 자원들에 대한 목록 : 입출력 장치, 개방한 파일 등 할당받은 자원들의 정보
- 계정 정보 : CPU를 사용한 시간, 시간 제한 등에 대한 정보
- 입출력 정보 : 진행 중인 입출력 요구 등의 정보

### 프로세스는 언제 상태가 바뀌는지?

- 생성 : 사용자가 요청한 작업의 PCB가 만들어져, 준비 상태가 되기 전 잠시 거치는 상태
- 준비 : CPU를 할당받기를 기다리는 상태, CPU만 할당 받으면 바로 실행할 준비가 되어있는 상태
- 실행 : CPU를 할당받아 실행 중인 상태, 이때 CPU를 할당하는 것을 dispatch 라고 한다.
  실행상태의 CPU는 CPU 스케줄링 정책에 의해 CPU를 빼앗길 수 있는데 이 경우 준비상태로 바뀐다.
- 대기 : 프로세스가 실행되다가 입출력을 요청하거나 당장 확보할 수 없는 자원을 요청했을 때 CPU 를 양도하고 요청한 일이 완료되기를 기다리는 상태.
  요청한 일이 완료되면 다시 준비 상태가 된다.
- 보류 준비 : 준비상태에서 메모리 부족으로 당장 필요하지 않은 프로세스가 메모리 공간을 빼앗기고 디스크로 나가는 것
- 보류 대기 : 대기상태에서 메모리 부족으로 당장 필요하지 않은 프로세스가 메모리 공간을 빼앗기고 디스크로 나가는 것
- 종료 : 프로세스가 종료될 때 잠깐 거치는 상태, 이 상태의 프로세스는 할당되었던 모든 자원들이 회수되고 PCB만 커널에 남아있는 상태이다.
  운영체제가 시스템에 남겨져 있는 이 프로세스의 흔적들을 최종 정리 후 PCB를 삭제하면 프로세스가 완전히 사라지게 된다.

### 멀티 프로세스

CPU가 여러개 있어서 여러개의 프로세스를 동시에 처리할 수 있는 시스템

## 스레드

프로세스의 자원을 공유하는 스케줄링의 단위

스택과 스레드 제어 블록 (실행 중의 레지스터 값, 프로그램 카운터, 우선 순위, 스레드 상태 정보)은 스레드 각각 따로 가지지만

프로세스의 주소공간이나 열린 파일 같은 자원들은 서로 공유한다.

### 멀티 스레드

하나의 프로세스에 여러개의 스레드를 만들어 스레드 단위로 실행하는 것

멀티 스레딩을 하게 되면 하나의 프로세스 내에 여러개의 실행 단위들이 존재하여 작업의 수행에 필요한 자원들을 공유하기 때문에

자원의 생성과 관리가 중복되는 것을 줄일 수 있다.

### 스택을 스레드마다 독립적으로 할당하는 이유

스택은 호출한 함수의 정보, 매개변수, 지역변수 등이 저장되는 곳이다.

스레드가 독립적인 실행흐름을 가지려면 독립적인 함수호출이 가능해야하므로 스택을 따로 가져아한다.

### PC Register를 스레드마다 독립적으로 할당하는 이유

스래드들이 독립적인 실행흐름을 가지려면 현재 실행하는 코드의 위치들이 서로 다를 수 있다.

따라서 PC Register에 대한 정보를 따로 저장해 문맥이 교환 됐을 때 스레드의 실행흐름을 이어갈 수 있어야한다.

### 프로세스 vs 스레드

멀티 스레딩을 하게 되면 하나의 프로세스 내에 여러개의 실행 단위들이 존재하며 작업의 수행에 필요한 자원들을 공유하기 때문에 자원의 생성과 관리가 중복되는 것을 막을 수 있다.

또한 문맥 교환 과정에서 자원을 새로 할당받는 일이 줄어들고 캐시 메모리를 초기화할 필요가 없기 때문에 오버헤드가 작다.

하지만 하나의 스레드에 문제가 생기면 전체 스레드에 문제가 생길 수 있다는 단점이 있다.

`단일 프로세서 멀티 스레드의 경우?`

### 스레드 동기화

한 프로세스 내의 스레드들은 그 프로세스의 주소공간과 자원들을 공유하기 때문에 특정 스레드가 변경시킨 내용이 다른 스레드에 영향을 미칠 수 있다.

따라서 오류를 야기할 수 있는 상호간섭, 데이터 파괴를 방지하기 위해 동기화가 요구된다.

<hr/>

# 스케줄러

스케줄러에는 단기스케줄러, 중기스케줄러, 장기스케줄러가 있다.

단기스케줄러는 CPU 스케줄러로, 준비 큐에 있는 프로세스 중 하나를 선택헤 CPU를 할당한다.

중기스케줄러는 메모리 공간이 확보되었을 때 보류상태에 있는 프로세스들 중 어떤 프로세스에게 메모리를 할당하여 준비 혹은 대기상태로 만들 것인지 정한다.

장기스케줄러는 프로세스가 되고자 하는 프로그램들 중 어떤 프로그램을 프로세스로 만들지 정한다.

## CPU 스케줄러 (단기 스케줄러)

준비 큐에 있는 프로세스 중 어떤 프로세스에 CPU를 할당할 것인지 정하는 스케줄러

아래와 같은 스케줄링 방식들이 사용될 수 있다.

### FCFS (FIFO)

먼저 준비 큐에 들어온 프로세스가 먼저 CPU를 할당받는 방식

비선점형

### SJF (SPN)

준비 큐에 있는 프로세스 중 가장 작업시간이 적은 프로세스에 CPU를 할당하는 방식

비선점형

- 기아상태에 빠질 수 있다.

### SRTF

준비 큐에 있는 프로세스 중 가장 작업시간이 적은 프로세스에 CPU를 할당하는 방식

선점형 : 새로운 프로세스가 들어올 때마다 계산한다.

- 기아상태에 빠질 수 있다.

### Priority

우선순위가 높은 프로세스를 먼저 CPU에 할당한다.

선점형과 비선점형 모두 가능하다.

- 기아상태에 빠질 수 있다.

- aging : wait time 이 길어지면 우선순위를 높이는 방식으로 기아상태를 어느정도 해결할 수 있다.

### Round Robin

준비큐의 프로세스에 일정한 시간만큼 CPU를 할당하는 방식

할당된 시간이 지나면 프로세스는 선점당하고 준비 큐의 가장 뒤로 들어간다.

- 한 프로세스에 할당되는 시간이 t, 준비큐의 프로세스가 n 개라고 한다면, 준비큐의 어떤 프로세스도 t\*(n - 1) 시간을 초과해서 기다리지 않는다.

- 따라서 기아상태에 빠지지 않는다.

- 응답시간이 빨라진다.

- 시간할당량이 너무 커지면 **FCFS** 와 같아지고, 시간할당량이 너무 작아지면 문맥교환에서 오버헤드가 커진다. 따라서 적당한 시간을 설정하는 것이 중요하다.

`적당한 할당 시간량 어떻게?`

### Virtual Round Robin

라운드 로빈에서 연산 위주의 프로세스가 입출력 위주의 프로세스보다 CPU 사용에 있어서 우대받는 것을 보완하기 위한 정책.

연산 위주는 주어진 시간 할당향을 모두 소진하고 큐의 맨 뒤로 돌아가는데 반해, 입출력 위주는 대부분 시간 할당량을 남긴 채 입출력을 발생시킨 다음 입출력이 완료되면 당시 남겨진 시간 할당량 부분을 보상받지 못한 채 큐의 맨 뒤로 들어감을 알수있다.

이 문제를 보완하기 위해서 입출력을 마친 프로세스가 들어가는 준비 큐를 따로 하나 더 두고 우선순위는 더 높게 하되, 이 큐에서 CPU를 받을 때는 이전 입출력을 발생시켰을 때 쓰지 못하고 남긴 시간 할당량만큼만 주도록 하는 방법이 있다.

### Multi-level Feedback Queue, MFQ : 다단계 피드백 큐

프로세스들의 CPU 요구량을 몰라도 짧은 프로세스들에게 유리하면서 입출력 프로세스를 우대할 수 있는 스케줄링 기법이다.

MFQ는 동적우선순위를 기반으로하는 선점 방식으로 진행된다. 우선순위 개수만큼의 큐가 있으며, 우선순위가 높을 수록 짧은 시간 할당량을 가진다.

새로운 프로세스는 최상위 단계의 준비 큐에 들어가 FCFS 순서로 CPU를 할당받아 실행되다가, 그 큐의 시간 할당량이 끝나면 한단계 아래의 준비 큐에 들어간다. 각 단계에서도 마찬가지로 다음 단계의 큐로 들어가게 되며 마지막 큐에서는 더 내려갈 큐가 없으므로 라운드로빈방식으로 실행될 것이다.

각 단계 큐의 프로세스는 상위 단계의 큐에 대기 중인 프로세스가 없어야만 실행할 수 있다.

어느 단계든 시간 할당량이 끝나기전에 입출력으로 CPU를 내놓게 되면 다시 준비 상태가 되었을때 한단계 위의 큐에 들어가게 함으로써 우선순위를 높여준다.

MFQ 기법은 여러가지 변형된 모양이 있는데, 각 단계에서 시간 할당량을 다 쓴 경우 그 단계에서 몇번의 순환 후 다음단계로 내려가는 방법, 입출력의 완료시 단계의 상승 폭을 더 주는 방법이 있다.

우선순위가 낮을 수록 시간 할당량을 늘려서 무한 대기를 방지하고자 하였으나 여전히 그 가능성이 남아있는데, Aging 기법을 활용하여 보완하는 방법도 있다.

<hr/>

# 프로세스 동기화

`추가`

<hr/>

# 메모리

## 가상 메모리

프로세스가 CPU를 할당받기 위해서는 메모리에 적재되어야한다.

하지만 메모리의 크기에는 한계가 있기 때문에 프로세스 전체의 크기가 메모리보다 큰 경우가 발생할 수 있다.

프로세스 크기가 메모리보다 작다고 해도 프로세스 전체를 메모리에 적재하면 메모리에 올라올 수 있는 프로세스의 수가 한정된다.

따라서 프로세스의 일부만 메모리에 적재하여 다중 프로그래밍 정도를 늘려, 사용자가 여러 프로그램을 동시에 실행하는 것처럼 느끼게 할 수 있다.

같은 크기로 나누면 페이징 기법이고 다른 크기로 나누면 세그먼트 기법이다.

### 주소의 매핑

가상 메모리에서 중요한 것은 주소의 매핑이다.

가상 메모리를 사용하면 프로그램에서 참조하는 주소가 실제 메모리에 있는 주소와 달라서, 메모리상의 주소로 변환이 필요하다.

만약 주소의 지정이 컴파일 시에 이루어진다면 프로그램이 참조하는 주소가 바로 실주소이기 때문에, 프로그램은 항상 메모리의 지정된 곳으로만 적재되어야한다.

가상 메모리 기법에서는 여러 프로세스의 일부를 메모리에 적재하므로 컴파일에서 주소의 지정이 이뤄져서는 안된다.

따라서 실행중인 프로그램 내에서 참조하는 주소는 가상주소를 사용하고, 주소의 매핑을 통해 물리주소를 찾아간다.

가상주소가 <3, 5> 라면 3번째 페이지(세그멘테이션)의 5번째 offset을 가리킨다는 뜻이다.

물리주소를 찾으려면 페이지(세그멘테이션)의 크기 \* 3 + 5 를 하면 된다.

## 페이징 기법

페이징을 위해서는 모든 프로세스들이 같은 크기의 조각들로 나뉘어야하는데 이떄 한 조각을 페이지라고 부른다.

메모리 역시 프레임이라는 페이지와 같은 크기로 나누어져 있으며 일련 번호가 있다.

한 프로세스의 전체 페이지는 디스크에 저장되고, 일부 페이지만 메모리에 적재되는데, 프로세스의 실행이 진행되는 과정에서 디스크와 메모리를 오가며 교체되는 단위가 페이지이며, 이것은 곧 사상의 단위가 된다.

운영체제는 가상주소를 실주소로 변환하기 위해 `프로세스당 하나의 페이지 테이블`을 만들어 두어야 하는데, 이것을 페이지 사상 테이블이라 부른다.

이 테이블의 크기는 해당 프로세스의 페이지 개수에 비례한다.

매핑을 위한 정보는 페이지당 하나씩 있어야 하므로 k 개의 페이지를 가지는 프로세스의 페이지 테이블을 k개의 엔트리로 구성된다.

한 프로세스에서 나뉘는 페이지들은 일련번호를 가지는데 페이지 테이블은 이 번호 순서대로 엔트리를 배치시킨다.

엔트리에 들어있는 정보 : 존재비트 (존재시 1), 존재시 적재되어 있는 프레임 번호, 존재하지 않을 시 페이지가 저장되어있는 디스크의 주소를 나타내는 필드

실행 시 참조되는 가상주소는 페이지 번호(p)와 페이지 내에서의 위치(d)로 표시된다.

페이지 테이블은 메모리의 커널 영역에 보관되며, 실행 중인 프로세스의 페이지 테이블 시작주소는 페이지 테이블 기준 레지스터에 들어있다.

먼저 기준 레지스터의 값에 p를 더해서 페이지 테이블에서 페이지 p의 사상 정보를 갖고 있는 엔트리를 찾은 후, 존재 비트를 확인한다.

존재 비트가 1일 경우 p가 적재되어 있는 프레임 번호를 알 수 있으므로, 이 번호 값에 페이지 크기를 곱하면 메모리에서 이 프레임의 시작주소를 얻을 수 있다.

프레임의 시작주소에 페이지 내의 위치 (d)를 더하면 프레임 내에서 접근해야 할 워드의 주소 즉, 실주소로 접근하게 되는 것이다.

존재 비트가 0인 경우 접근하고자 하는 페이지가 메모리에 없음을 의미하므로 먼저 디스크 주소로부터 이 페이지를 매모리에 적재한다.

그 다음 이 엔트리의 존재 비트를 1로 바꾸고, 적재된 프레임 번호를 기입한 후 매핑을 진행하면 실 주소를 얻을 수 있다.

### TLB의 사용

TLB = Translation Lookaside Buffer

가상 메모리 주소를 물리 메모리 주소로 바꾸는 속도를 높이기 위한 캐시이다.

연관메모리를 사용하기 때문에 한번에 검색할 수 있다.

찾고자하는 페이지 번호가 캐시에 있다면 hit, 프레임 번호를 알아내 (프레임 번호 \* 페이지 크기 + 페이지 내의 위치)로 메모리에 한번만 접근할 수 있다.

찾고자하는 페이지 번호가 캐시에 없다면 메모리의 페이지 테이블에 접근해 프레임 번호를 알아내고 다시 메모리에 접근한다. 메모리에 페이지가 없다면 디스크에 접근하는 과정이 추가된다. 또한 페이지 테이블 엔트리를 캐시에 올린다.

hit 비율이 높으면 속도가 비약적으로 올라가지만 hit 비율이 낮으면 오히려 cpu 성능이 떨어질 수 있다.

기본적으로 메모리에 두번 접근하는 시간에 캐시에 접근하는 시간 또한 추가되기 때문이다.

### 2단계 페이징

32비트의 주소공간을 가진다고 할 때, 페이지의 오프셋에 12비트를 할당한다고 하면 페이지의 크기는 2^12바이트 = 4Kbyte 가 된다.

그렇다면 메모리의 페이지 개수는 최대 2^20개 이므로 페이지 테이블의 엔트리 개수 또한 최대 2^20개가 될 것이다.

각 엔트리당 4Byte라고 해도 페이지 테이블의 크기는 4MByte가 된다. 이는 메우 큰 크기로 메모리에 저장하기에는 벅차다.

따라서 페이지 테이블을 나누어 일부만 메모리에 적재하는 방법을 생각해야한다.

20비트를 다시 10비트씩 나누어 뒤의 10비트는 페이지 테이블 주소공간, 앞의 10비트는 페이지 테이블을 위한 테이블(루트 테이블)의 주소공간으로 만든다.

p1(10 비트) / p2(10 비트) / d(12비트) : 이라는 주소가 있다고 할 때

우선 기준레지스터에 p1을 더해 루트테이블의 특정 엔트리를 찾는다. 해당 엔트리에는 해당 테이블이 메모리에 적재되어있는지 확인할 수 있는 비트와 페이지 테이블의 주소가 적혀있다.

해당 페이지 테이블 주소에 p2를 더하면 페이지가 메모리에 적재되어있는지 확인할 수 있는 비트와 페이지가 몇번째 프레임인지 적혀있는 엔트리가 있다.

해당 엔트리에 적힌 프레임에 대한 정보로 (프레임 번호 \* 페이지(프레임)크기 + d) 를 계산하면 원하는 물리주소에 접근할 수 있다.

### 역 페이지 테이블 기법

역 페이지 테이블 기법은 프로세스당 페이지 테이블 하나를 두는 것이 아니라 메모리에 고정된 크기의 페이지 테이블 하나만 두는 것이다.

페이지 테이블의 엔트리 수는 메모리에서 프레임 수와 같으며 순서 또한 같다.

따라서 페이지 테이블에는 여러 프로세스의 페이지 정보가 같이 담기며 엔트리에는 적재여부, 프로세스 아이디(pid), 페이지 번호(p)가 담긴다.

가상주소가 pid/p/d 라고 할 때, 페이지 테이블을 순차적으로 순회하며 pid 와 p 가 일치하는 엔트리가 있는지 확인한다.

일치하는 엔트리가 i 번째 엔트리라면 헤당 페이지는 메모리 상에서 i 번째 프레임에 존재하므로

(i \* 페이지 크기 + d) 가 접근하고자 하는 물리주소가 된다.

역 페이지 테이블은 순회로 검색하므로 최악의 경우 끝까지 검색해봐야한다는 단점이 있다.

또한 역페이지 테이블에는 프레임마다 프로세스 아이디가 있으므로, 서로 다른 프로세스 아이디를 필요로하는 공유는 구현하기가 힘들다.

## 세그멘테이션 기법

페이징은 고정 크기의 페이지로 메모리를 관리함으로써 구현이 쉽다는 정점을 가지기 때문에 대부분의 시스템에서 채택되고 있다.

하지만, 프로그램의 마지막 페이지는 내부 단편화가 있게 되는 점도 간과할 수 없다.

또한 하나로 붙어 다녀야 효과적인 크기 (서브루틴, for문 또는 매트릭스 등)가 페이지 단위로 나누어져 일부가 적재되면 매핑시간이 길어지고 공유해야할 페이지가 많아져 관리하기 어려워질 수 있다.

그래서 물리적인 크기가 다르더라도 논리적인 단위별로 프로그램의 조각을 나누는 것이 세그맨테이션이다.

세그멘테이션 기법은 마찬가지로 세그멘테이션 테이블을 가지는데, 세그멘테이션 테이블의 엔트리에는 세그먼트의 시작주소와 세그먼트의 크기가 적혀있다.

세그멘트 시작주소에 오프셋 번호를 더하면 찾고자 하는 물리주소를 찾을 수 있다.

오프셋 번호가 세그먼트의 크기를 초과하면 트랩을 통해 프로세스의 실행을 중지시켜 메모리 보호를 할 수 있다.

세그멘테이션의 장점은 공유에서도 드러난다. 공유해야할 프로시저가 몇개의 페이지로 나누어진다면 여러개의 페이지를 각 프로세스의 테이블에서 같은 위치에 위치시켜야 하기 때문에 관리하기 힘들다. 또한 공유해야할 필요가 없는 부분도 같은 페이지로 만들어야한다. 하지만 세그멘테이션을 사용하면 공유해야할 프로시저만을 하나의 세그멘테이션으로 만들어 관리하기가 용이하다.

추가로 세그멘테이션 내부에 페이징을 사용하는 기법 또한 존재한다.

<hr/>

# 메모리 관리

## 적재 정책

실행에 필요한 페이지를 언제 적재할 것인가를 결정하는 정책이다.

- 요구적재
  페이지가 참조될 때 적재하는 기법. 즉 적재 요구가 있을 때 적재하는 기법이다.

- 예측적재
  예측을 통해 확률적으로 참조될 가능성이 높다고 판단되는 페이지를 미리 적재시키는 기법이다.

  예측이 잘 될 경우 페이지 부재 빈도를 낮출 수 있으나, 반대의 경우 예측을 위한 오버헤드와 참조되지 않을 페이지를 적재한 메모리 낭비가 발생한다.

  프로그램의 시작시점에서 당장 참조될 것으로 보이는 몇개의 페이지들을 적재시키는 경우에 고려해볼 수 있지만, 실험을 통해 요구적재가 더 낫다는 것이 밝혀저 예측 적재는 매우 드물게 일어난다.

## 배치 정책

디스크로부터 가져온 페이지를 메모리의 어디에 적재할 것인가를 결정하는 정책이다.

페이징을 사용하는 시스템에서는 메모리가 페이지와 크기가 같은 프레임으로 이루어져있다고 배웠으므로, 빈 프레임만 발견되면 어떤 프레임에 적재하든 문제가 없고 따라서 배치 정책을 따로 신경 쓰지 않아도 된다.

하지만 세그먼테이션을 사용할 경우 세그먼트의 크기가 얼마든지 다를 수 있으므로 다양한 크기의 세그먼트를 수용할 수 있는 배치정책이 요구된다.

- 최초적합 (first-fit)

  free 리스트의 첫 노드부터 시작하여 제일 먼저 발견되는, 요구되는 크기보다 더 큰 빈 공간을 가지는 노드에 할당해주고 탐색을 마친다.

- 최적적합 (best-fit)

  free 리스트를 끝까지 탐색하여 요구되는 크기보다 더 크되, 그 차이가 제일 작은 노드를 찾아 할당해주는 방법이다.

- 최악적합 (worst-fit)

  free 리스트를 끝까지 탐색하여 요구되는 크기보다 더 크되, 그 차이가 제일 많이 나는 노드를 찾아 할당해주는 방법이다.

최초적합이 가장 효율적이다. 다만 시간이 지날수록 free 리스트의 앞부분이 매우 작아져서 탐색시간이 길어지는 문제가 발생한다.

이런 문제의 보완은 free 리스트를 순환 구조로 만든 다음, 할당이 가능한 노드가 선택될 때마다 헤더 포인터를 선택된 노드 다음으로 옮기게 하는 방법이 있다.

이 방법을 next-fit 이라고 부른다.

어떤 방법을 사용해도 결국 시간이 지나면 빈공간이 많아도 대부분 홀이라 크지 않은 프로세스의 적재 요구조차 수용할 수 없는 상황이 발생한다.

결국 작은 빈 공간들을 합쳐서 더 큰 빈공간을 만드는 작업이 요구된다.

- 인접한 빈공간의 병합

- 빈공간 전부의 통합

- 버디시스템

## 할당 정책

프로세스들에게 메모리를 얼마씩 줄 것인지를 결정하는 정책이다.

페이징의 경우 각 프로세스에게 메모리 프레임을 몇개씩 사용할 수 있도록 해줄 것인가와 같은 말이 될 것이다.

프로세스들에게 같은 개수든 서로 다른 개수든 시스템에서 정해놓은 개수의 프레임을 사용하도록 하고,

개수의 변동이 없도록 운영한다면 **고정할당** 이라고 부른다.

반면에 실행 도중 프로세스에 부여된 프레임의 수에 변동이 있도록 한다면 **가변할당**이라 부른다.

교체 정책에서 교체의 대상을 선택할 때 그 범위를 어디까지로 할 것인가는 할당 정책과 밀접한 관련이 있다.

해당 프로세스에게 할당된 프레임 중에서 교체될 페이지를 선택하면 지역교체라고 하고,

메모리의 모든 프레임이 대상이 되면 전역교체라고 하는데, 고정할당을 하게되면 전역교체는 불가능하다.

## 교체 정책

메모리에 빈 프레임이 없을 때 적재될 페이지를 위해 적재된 페이지 중 하나는 교체대상이 되어야한다.

이때 어떤 페이지를 교체할 것인가를 정하는 것이 교체 정책이다. 우선 고정할당의 경우에 대해 이야기한다.

### 최적 기법

현 시점에서 앞으로 가장 오랜시간 동안 참조되지 않을 페이지를 선택하여 교체하는 기법.

앞으로 어떤 페이지를 참조할 지 미리 알 수 없으므로 현실적으로 구현이 불가능하며 다른 기법들의 성능을 비교하기 위해 쓰인다.

### FIFO 기법

적재된지 가장 오래된 페이지를 교체하는 기법.

오래된 페이지를 알기 위해서는 시간이나 순서를 알아야한다.

시간을 기록해서 교체시 시간이 가장 오래된 페이지를 선택하는 방법, 큐를 사용하여 큐에서 맨 앞에 있는 페이지를 선택하는 방법이 있다.

FIFO는 페이지 부재를 줄이기 위해 프레임을 더 주었을 경우 오히려 부재율이 올라가는 FIFO 모순, Belady's Anomaly 가 발생할 수 있다.

### LRU 기법

참조된지 가장 오래된 페이지를 교채하는 기법이다. 최근에 자주 참조된 페이지가 앞으로도 자주 참조될 것이라는 판단에 근거하는 기법이다.

가장 오래된 페이지를 알기 위해서 시간을 기록하는 방법, 자료구조로 순서를 나타내는 방법이 있다.

시간 기록 기법을 사용하여 페이지들이 적재될 때의 시간을 기록한 후 이 페이지가 메모리에 있는 동안 참조될 때마다 가장 최근의 참조시간으로 갱신해놓으면, 가장 오래전 시간이 기록되어있는 페이지가 교체 대상이 될 것이다.

링크드리스트를 사용하면 노드의 위치가 상대적인 참조 순서를 나타내어 가장 앞에있는 페이지가 교체대상이 되도록 한다. 참조하는 경우 맨 뒤로 이동시켜 최근 참조 여부를 반영한다.

### Second-chance(Clock) 기법

FIFO의 변형이자 LRU에 근접하는 기법.

적재된 페이지에 참조비트를 두어 교체 대상인 페이지의 참조 비트가 0이면 바로 교체되고, 1로 되어있을 경우 비트를 0으로 만들면서 큐의 맨 뒤로 보냄으로써 메모리에 머무를 기회를 한번 더 주는 것이다.

### 개선된 Second-chance (NUR, Not Used Recently) 기법

Clock에 갱신 비트를 추가한 기법.

갱신 비트가 1이라는 말은 이 페이지가 적재 중 변경되었다는 것을 의미하고, 교체가 될 경우 변경된 내용으로 디스크에 기록을 해 주어야하는 부담이 있으므로 가급적 교체를 미루어 디스크에 대한 쓰기작업을 줄이고자 하는 의도이다.

참조 비트와 갱신 비트의 조합은 네 종류가 있게 되고 다음과 같이 운영된다.

첫 번째 단계로, 현재 포인터의 위치에서 포인터를 이동하며 참조와 갱신 비트가 모두 0인 페이지를 찾아 교체하고 다음 페이지로 포인터를 이동시킨다.

첫 번째 단계에서 그런 페이지를 찾지 못하면 두번째로 참조는 0 갱신은 1로 되어있는 페이지를 찾아 교체한다. 이때 포인터를 이동하면서 모든 프레임의 참조 비트를 0으로 바꾼다.

두 번째 단계에서도 해당 페이지를 찾지 못했다면 포인터는 제자리로 돌아와 있을 것이며, 모든 프레임의 참조 비트는 0이 되어있을 것이므로 다시 첫번째 단계를 시도하고 안되면 두번째 단계까지 시도해보면 교체 대상 페이지가 발견될 것이다.

이 때 교체의 대상은 참조와 갱신 비트의 짝이 (0,0)인 것부터 (0,1), (1,0), (1,1)의 순서로 채택된다.

### LFU(Least Frequently Used)와 MFU(Most Frequently Used) 기법

적재되어 있는 동안 참조된 횟수를 누적하여 기록한 후 그 값으로 교체 대상을 선택하는 기법이며,

LFU 는 많이 참조된 페이지는 앞으로도 참조될 확률이 높을 것이란 판단에 근거하여 값이 가장 작은 페이지를 선택한다.

반면에 MFU는 많이 참조된 페이지는 충분히 참조가 이루어졌으므로 더 이상 참조되지 않을 것이라는 판단에 근거하여 값이 가장 큰 페이지를 선택한다.

두 기법 모두 편향된시각에 근거함으로써 실제 구현되는 경우는 매우 드물다.

### 페이지 버퍼링 기법

이 기법은 단순한 교체 기법과 함께 사용되여 페이징의 성능을 향상시킬 수 있다.

적재가 가능한 가용 프레임 몇개를 풀로 만들어두고, 페이지 부재가 발생하면 풀에 바로 적재한다.

그리고 교체될 프레임을 선택하여 해당 프레임이 변경된 경우 디스크에 작업하고 프레임을 풀에 넣는다.

페이지를 바로 풀에 올림으로써 교체해야할 페이지를 디스크에 쓴다음에 페이지를 적재하는 시간적 순서를 지키지 않아서 빠른 응답을 할 수 있다는 장점이 있다.

또한 교체될 페이지들을 풀에 남겨두어 이 페이지가 다시 참조될 경우 바로 사용할 수 있다.

### Working set 이론 (가변 할당)

프로세스가 특정 시점에 집중적으로 참조하는 페이지들의 집합을 Working set 이라 하며, Working set을 메모리에 적재되도록 함으로써 페이지 부재를 최소화하는 방식. Working set이란 현 시점에서의 지역성을 표현하고 이 페이지의 집합은 앞으로도 당분간 집중적으로 참조될 것이라 보는 것이다.

working set 이론에서 윈도 크기를 정하는 것은 중요하다. 너무 크게 잡으면 지역성에 포함되지 않는 페이지도 Working set에 포함되어 다중 프로그래밍 정도를 유지하기 어려워진다. 반면에 너무 작게 잡으면 지역성에 포함되는 페이지들조차 Working set에서 빠져 페이지 부재로 인한 성능 저하가 발생하게 된다.

단점 : Working set이 언제나 그 시점의 지역성을 잘 표현하지는 못하며(지역성이 변화하는 구간에서), 최적의 윈도우 크기를 정하는 것이 어렵다. 매 페이지를 참조할 때마다 working set을 바꾸는 것도 상당한 부담이 될 수 있다.

<hr/>

# 디스크 스케줄링

디스크는 근본적으로 CPU나 메모리의 속도에 비하면 매우 느린 하드웨어이다. 대부분의 프로그램이 실행 중 디스크 입출력을 한다고 가정했을 때 전체 실행 시간의 대부분은 디스크의 속도에 의해 결정될 것이다. 따라서 한계를 가진 디스크에서 입출력 요구에 대한 처리 성능을 조금이라도 높여 시스템 성능을 개선해야할 것이다.

## FCFS 스케줄링

디스크 입출력 요청들이 도착한 순서대로 서비스하는 기법

공평하며 스케줄링으로 인한 오버헤드가 작다.

## SSTF 스케줄링 (Shortest Seek Time First)

현재 큐에 도착해있는 요구들 중 현재 가장 가까운 요구를 먼저 서비스한다.

단위 시간당 처리량이 극대화 되지만, 디스크 입출력 요구들에 대한 응답 시간의 예측성이 떨어진다.

만약 헤드에서 가까운 곳에 계속 새로운 요구가 들어온다면 그 구간에서 벗어나지 못해 무기한 연기가 나타날 수 있다.

## SCAN 스케줄링

SSTF 기법과 비슷한 형태로, 현재 헤드의 위치와 가장 가까운 위치에 대한 요구를 먼저 서비스하지만, 현재 헤드의 진행 방향으로만 입출력 요구들을 처리하며, 양 끝의 실린더에 도달했을 때만 방향을 전환하여 나머지 요구들을 처리하는 기법이다.

## LOOK 스케줄링

SCAN 기법과 같은 방식으로 운영되나, 헤드가 진행하는 도중 진행방향의 앞쪽으로 더 이상의 요구가 없으면 양끝의 실린더까지 진행하지 않고 그 자리에서 방향을 바꾸는 방식이다.

### SCAN과 LOOK의 단점

이 두 기법은 새로운 디스크 접근 요구가 입출력 헤드 진행 방향 앞쪽에서 발생하면 즉시 서비스하지만, 바로 뒤에서 발생하는 경우에는 헤드가 다시 되돌아 올 때까지 기다려야한다는 한계를 가진다.

또한 진행방향 앞쪽으로 계속 새로운 요구가 발생하면 반대 방향의 요구들의 대기시간은 점점 길어진다.

또한 양 끝에서 발생하는 입출력 요구는 디스크 중앙부분 실린더에 비해 서비스 기회를 적게 가지게된다는 단점도 갖는다.

## N-step SCAN 스케줄링

SCAN 기법과 같은 기법이지만 방향을 바꾸는 시점에서 큐에 대기중인 요구들만을 대상으로 서비스를 진행한다. 도중에 들어오는 요구사항에 대해서는 다음번 방향을 바꾼 후에 처리하도록 한다.

## C-SCAN 스케줄링

C-SCAN 스케줄링은 기본적으로 N-step SCAN 스케줄링이지만, 정해진 방향으로 헤드가 이동할 때에만 큐의 요구들을 처리한다.

양 끝 트랙과 중앙부분 트랙을 균등하게 서비스하여 응답시간의 편차를 최소화한다. 따라서 응답 시간에 대한 예측성이 매우 높다.

LOOK 처럼 진행방향에 요구가 없을 때 즉시 방향을 전환하는 방식을 C-LOOK 스케줄링이라고 한다.

일반적으로 SCAN(LOOK)방식이나 C-SCAN(C-LOOK) 기법을 많이 사용한다.
