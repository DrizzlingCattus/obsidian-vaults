


```bash
ubuntu@ip-172-31-8-255:~/workspace$ top -b -n 1
top - 07:43:09 up 54 min,  1 user,  load average: 0.00, 0.01, 0.01
Tasks: 103 total,   1 running, 102 sleeping,   0 stopped,   0 zombie
%Cpu(s):  0.0 us,  0.0 sy,  0.0 ni,100.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
MiB Mem :    967.9 total,    173.9 free,    158.1 used,    636.0 buff/cache
MiB Swap:      0.0 total,      0.0 free,      0.0 used.    656.3 avail Mem

    PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND
      1 root      20   0  103748  12408   8148 S   0.0   1.3   0:04.64 systemd
      2 root      20   0       0      0      0 S   0.0   0.0   0:00.00 kthreadd
      3 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 rcu_gp
      4 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 rcu_par_gp
      5 root      20   0       0      0      0 I   0.0   0.0   0:00.09 kworker/0:0-cgroup_destroy
      6 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker/0:0H-events_highpri
      8 root      20   0       0      0      0 I   0.0   0.0   0:00.11 kworker/u30:0-events_unbound
      9 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 mm_percpu_wq
     10 root      20   0       0      0      0 S   0.0   0.0   0:00.00 rcu_tasks_rude_
     11 root      20   0       0      0      0 S   0.0   0.0   0:00.00 rcu_tasks_trace
     12 root      20   0       0      0      0 S   0.0   0.0   0:00.09 ksoftirqd/0
     13 root      20   0       0      0      0 I   0.0   0.0   0:00.41 rcu_sched
     14 root      rt   0       0      0      0 S   0.0   0.0   0:00.02 migration/0
     15 root     -51   0       0      0      0 S   0.0   0.0   0:00.00 idle_inject/0
```

- 시스템 구동시간 확인
- load average를 통한 시스템 과부화 정도 확인
- 현재 시스템에서 구동 중인 프로세스의 개수 확인 (Tasks row)
- CPU, MEM, SWAP 사용량
-    `PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND`  각각의 의미
	- PID, USER, CPU, MEM, COMMAND는 명확
	- PR : priority ~ 커널이 프로세스를 스케줄링할 때 사용하는 우선순위. 값이 낮을 수록 더 높은 우선순위.
	- NI :  nice value. PR을 얼마만큼 조절할 것인지 결정하는 계수. 기본 PR 값에 NI 값을 더해서 실제 PR값이 결정됨.
	- VIRT, RES, SHR : 프로세스가 사용하는 메모리 양. 프로세스 메모리 누수 판단에 사용.
	- S : status. 프로세스 상태. CPU 사용 중인지. I/O를 기다리는 중인지. 유휴 상태인지.

## VIRT, RES, SHR


```bash
45. VIRT  --  Virtual Memory Size (KiB)
           The  total  amount  of virtual memory used by the task.  It includes all code, data and shared libraries plus pages that have been swapped out and pages that have
           been mapped but not used.
```
실제로는 할당되지는 않음. 프로세스가 사용할 수 있는 가상 메모리 공간.

```bash
22. RES  --  Resident Memory Size (KiB)
           A subset of the virtual address space (VIRT) representing the non-swapped physical memory a task is currently using.  It is also the sum of  the  RSan,  RSfd  and
           RSsh fields.

           It  can include private anonymous pages, private pages mapped to files (including program images and shared libraries) plus shared anonymous pages.  All such mem‐
           ory is backed by the swap file represented separately under SWAP.

           Lastly, this field may also include shared file-backed pages which, when modified, act as a dedicated swap file and thus will never impact SWAP.

```
실제로 사용하고 있는 물리 메모리 공간. 메모리 점유율이 높은 프로세스를 찾기 위해서는 RES 영역이 높은 프로세스를 찾아야한다.

```bash
30. SHR  --  Shared Memory Size (KiB)
           A subset of resident memory (RES) that may be used by other processes.  It will include shared anonymous pages and shared file-backed  pages.   It  also  includes
           private pages mapped to files representing program images and shared libraries.
```
리눅스 프로세스들은 glibc라는 라이브러리를 참조하기에 각 프로세스 메모리 영역마다 올리는 것보단 SHR 영역에 동시에 참조할 수 있도록 했다.

![[Drawing-memory-structure.excalidra]]


memory commit
- 프로세스는 작업공간이 필요하고, 이 공은 메모리에 존재. 프로세스가 커널에 메모리 요청. 커널은 사용 가능한 메모리 영역을 주고, 할당은 하지 않고, 그냥 프로세스에게 어느정도 주었는지만 저장. 이 일련의 과정을 memory commit이라고 함.
- 그렇다면 VIRT를 무한대로 할당받을 수 있을까?
	- vm.overcommit_memory 파라미터에 따라 다르게 동작할 수 있다.
- 왜 이렇게 동작?
	- fork() 프로세스 콜을 위해서. fork() 시에 똑같은 프로세스를 생성하게 되는데, 이때 memory commit 없이 메모리를 똑같이 할당해버리면 물리 메모리가 넘칠 수 있음. COW(copy-on-write)를 통해 실제로 쓰기 작업이 발생한 후에 메모리 할당을 시작하게 한다.
- sar 도구를 통해 memory commit 상태를 확인할 수 있다.


process status

```bash
29. S  --  Process Status
           The status of the task which can be one of:
               D = uninterruptible sleep
               I = idle
               R = running
               S = sleeping
               T = stopped by job control signal
               t = stopped by debugger during trace
               Z = zombie
```


![[Pasted image 20220917173009.png]]


### 프로세스 우선순위

CPU 마다 Run Queue가 존재. Scheduler가 Run Queue에서 우선순위에 따라 하나씩 빼내고, Dispatcher에게 전달하면 CPU에게 할당되는 구조.

PR 값이 rt(real-time)인게 있는데, 이는 특정 시간안에 종료되어야하는 중요한 프로세스들이다. 커널에서 사용하는 데몬이 대상. RT 스케줄러의 적용을 받게되는 프로세스들은 CFS(completely fair scheduling) 스케줄러보다 더 먼저 실행된다.


### Load Average와 시스템 부하
TODO:
