# EXT4 & LFS Profiling

#### Purpose
To profile Ext4 and Nilfs2 on Linux Kernel 4.4 in order to figure out the difference between two file systems in the sequence of accessing disk block sectors when calling write system call<br>
For further information, check "Group 18 Report.pdf"

#### Result Preview
EXT4
![Result of EXT4](/ext4.jpg)
LFS
![Result of LFS](/nilfs2.jpg)

#### File description
/blk-core.c	원 경로는 $리눅스경로/block/ 이다. write/read 시 bio로 접근하는 함수인 submit_bio를 포함하기 때문에 해당 함수 안에 write 일 때만 접근 sector와 시간, 해당 bio의 block device의 super block의 파일시스템 타입을 버퍼에 기록하는 코드를 추가하였다. 시간값을 받아오기 위해 time.h 헤더 참조를 추가하였다.

/segbuf.c	원 경로는 $리눅스경로/fs/nilfs2/ 이다. Nilfs에서 사용하는 bio 대체 struct인 segment_buffer를 Ext4 파일시스템이 사용하기 위해 submit_bio 함수의 parameter에 segbuf를 넣는 역할이 포함된 파일이다. 따라서 submit_bio 전 해당 bio가 segbuf의 superblock을 가리키게 하는 코드를 추가하였다.

/Makefile	원 경로는 $리눅스경로 이다. 리눅스 커널 코드를 컴파일하기 위한 파일.

/hw1/hw1.c	원 경로는 $~/hw1/ 이다. hw1 이름의 모듈 코드 파일. 해당 모듈에 write가 발생할 때, 입력받은 값에 따라 커널의 버퍼값을 extern을 통해 받아와 로그에 기록하고, 같은 내용을 /tmp/result.csv 파일에 출력하고 해당 버퍼내용은 초기화시킨다.

/hw1/hw1.ko	원 경로는 $~/hw1/ 이다. hw1 이름의 모듈 이미지 파일. 컴파일 후 자동 생성된다.

/hw1/Makefile	원 경로는 $~/hw1/ 이다. 모듈 코드를 컴파일하기 위한 파일.
