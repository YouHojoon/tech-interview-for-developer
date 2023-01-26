## [OS] Race Condition

공유 자원에 대해 여러 프로세스가 동시에 접근할 때, 결과값에 영향을 줄 수 있는 상태

> 동시 접근 시 자료의 일관성을 해치는 결과가 나타남

<br>

#### Race Condition이 발생하는 경우

1. ##### 커널 작업을 수행하는 중에 인터럽트 발생

   - 문제점 : 커널모드에서 데이터를 로드하여 작업을 수행하다가 인터럽트가 발생하여 같은 데이터를 조작하는 경우
   - 해결법 : 커널모드에서 작업을 수행하는 동안, 인터럽트를 disable 시켜 CPU 제어권을 가져가지 못하도록 한다.

2. ##### 프로세스가 'System Call'을 하여 커널 모드로 진입하여 작업을 수행하는 도중 문맥 교환이 발생할 때

   - 문제점 : 프로세스1이 커널모드에서 데이터를 조작하는 도중, 시간이 초과되어 CPU 제어권이 프로세스2로 넘어가 같은 데이터를 조작하는 경우 ( 프로세스2가 작업에 반영되지 않음 )
   - 해결법 : 프로세스가 커널모드에서 작업을 하는 경우 시간이 초과되어도 CPU 제어권이 다른 프로세스에게 넘어가지 않도록 함

3. ##### 멀티 프로세서 환경에서 공유 메모리 내의 커널 데이터에 접근할 때

   - 문제점 : 멀티 프로세서 환경에서 2개의 CPU가 동시에 커널 내부의 공유 데이터에 접근하여 조작하는 경우
   - 해결법 : 커널 내부에 있는 각 공유 데이터에 접근할 때마다, 그 데이터에 대한 lock/unlock을 하는 방법

   

### 대표적인 문제

#### Reader-Writer Probleme

데이터셋을 프로세스가 공유한다고 가정한다.
- Reader : 오지 데이터를 읽기만 한다.
- Writer : 읽기도 하고 데이터를 쓰기도 한다.

문제 : 복수의 Reader가 동시에 데이터셋에 접근할 수 있다.
- Writer가 접근할 때에는 혼자서만 접근할 수 있어야 한다.
- 만약 Writer가 데이터셋을 업데이트 하고 있는 와중에 Reader가 접근하면 다른 상태의 데이터셋을 읽을 것이기 때문이다.

Shared Data
- Datat set
- read_count, 초기값은 0
- rw_mutex, 초기값은 1
- mutex, 초기값은 1

Writer process
```c
do{
   wait(rw_mutex);
   /* writing process */
   signal(rw_mutex);
}while(true)
```

Reader process
```c
do{
   /* read_count에 접근한 프로세스가 있는 지 확인 */
   wait(mutex);
   read_count++;
   
   /* 내가 처음 읽기로 접근하느 프로세스라면? */
   if (read_count == 1)
      wait(rw_mutex); // Writer가 쓰기 중인지 확인한다.
 
   signal(mutex);
   
   /*  reading process */
   
   wait(mutex);
   read_count--;
   
   /* 내가 마지막인 Reader 프로세스라면?*/
   if (read_count == 0)
      signal(rw_mutex); // Writer가 쓰기 가능한 상태라고 알림.
   signal(mutex);
}while(true)
```

