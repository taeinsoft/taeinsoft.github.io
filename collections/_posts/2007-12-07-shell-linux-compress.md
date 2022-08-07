---
title: "[Shell] Linux에서 통합압축프로그램 사용"
categories:
  - Shell
tags:
  - Linux
  - Shell
  - Bash
date: 2007-12-07
last_modified_at: 2021-10-28
toc: false
---

리눅스 환경을 처음 접하게되면 가장 막히는 작업은 압축, 압축해제 명령어입니다. 필자만 그런건지는 잘 모르겠지만, 아무리 익혀도 잘 숙지가 되지 않습니다.

리눅스에서는 보통 tar를 이용하여 여러개의 파일을 묶고, bzip, gzip, ZIP, compress 등의 압축포멧으로 압축을 하게 되는데 압축하는 것은 본인이 자주 쓰는 형태로 압축하면 되지만 압축을 풀때는 각각의 명령어가 다르기 때문에 지속적으로 쓰지 않으면 잊어버리기 쉽습니다. 아래 소스코드는 학생시절에 만들었던 쉘 프로그램입니다.

이 프로그램을 이용하면 압축을 할때도, 압축을 풀때도 동일한 명령어를 입력해서 압축/압축해제를 할 수 있습니다. 압축 포멧에 따른 명령어를 따로 뒤져보지 않아도 되니, 그때 당시로서는 나름 편리하게 사용했었습니다.

쉘 환경은 bash이고, 프로그램의 간략한 사용법은 첨부파일로 올려놓았습니다.

[다운로드 : 통합압축프로그램 사용설명서.pdf](https://drive.google.com/file/d/10VAtbrzrzFi66XMwztcunah7SCUJLxAu/view?usp=sharing)

```sh
#!/bin/bash

#------------------------------------------------------------------
# Date       : 2007.12.07
# Blog       : 재미주의 프로그래밍[ http://taeinsoft.com ]
# Programmer : Taein
# Version    : 0.1
#------------------------------------------------------------------

echo -n "
통합 압축기 V.0.1.
"
#------------------------------------------------------------------
# 압축, 압축 해제 함수정의
#------------------------------------------------------------------
zip()
{
echo "ZIP형식으로 압축을 시작합니다."
exec zip -r $1.zip ${DIR}
}

unzip()
{
A=`ls -a *.zip 2> /dev/null`
T="$FILENAME.zip"
for x in `echo $A`
do
if [ $T = $x ]; then
echo -n "$x 라는 압축파일을 찾았습니다.
압축을 해제하시겠습니까?(Y/N):"
read F

            case $F in
            'n')
                exit
                ;;
            'N')
                exit
                ;;
            *)
                mkdir $FILENAME
                cd $FILENAME
                exec unzip ../$x
                exit
                ;;
            esac
        fi
    done
}

gunzip()
{
A=`ls -a *.tar.gz 2> /dev/null`
T="$FILENAME.tar.gz"
for x in `echo $A`
do
if [ $T = $x ]; then
echo -n "$x 라는 압축파일을 찾았습니다.
압축을 해제하시겠습니까?(Y/N):"
read F

            case $F in
            'n')
                exit
                ;;
            'N')
                exit
                ;;
            *)
                mkdir $FILENAME
                cd $FILENAME
                tar xvfz ../$x
                exit
                ;;
            esac
        fi
    done
}

gzip()
{
echo "gzip 형식으로 압축을 시작합니다."
tar cvfz $1.tar.gz ${DIR}
}

bunzip()
{
A=`ls -a *.tar.bz 2> /dev/null`
for x in `echo $A`
do
if [ "$FILENAME.tar.bz" = $x ]; then
echo -n "$x 라는 압축파일을 찾았습니다.
압축을 해제하시겠습니까?(Y/N):"
read F
case $F in
'n')
exit
;;
'N')
exit
;;
*)
mkdir $FILENAME
cd $FILENAME
tar xvfj ../$x
exit
;;
esac
fi
done
}

bzip()
{
echo "bzip 형식으로 압축을 시작합니다."
tar cvfj $1.tar.bz ${DIR}
}

uncompress()
{
A=`ls -a *.tar.Z 2> /dev/null`
for x in `echo $A`
do
if [ "$FILENAME.tar.Z" = $x ]; then
echo -n "$x 라는 압축파일을 찾았습니다.
압축을 해제하시겠습니까?(Y/N):"
read F
case $F in
'n')
exit
;;
'N')
exit
;;
*)
mkdir $FILENAME
cd $FILENAME
tar xvfZ ../$x
exit
;;
esac
fi
done
}

compress()
{
echo "compress 형식으로 압축을 시작합니다."
tar cvfZ $1.tar.Z ${DIR}
}

#------------------------------------------------------------------
# 메인 실행 부분
#------------------------------------------------------------------
if [ -z $1 ]; then
echo -n "
파일명 : "
read FILENAME
else
FILENAME=$1
fi

gunzip
bunzip
uncompress
unzip

if [ -z $2 ]; then
echo -n " 대상 디렉토리 : "
read DIR
else
DIR=$2
fi

echo -n "
현재 디렉토리를 압축합니다.
압축할 형태를 선택하세요.
기본 압축방식은 gzip입니다.

    1. gzip
    2. bzip2
    3. compress
    4. ZIP
    5. 취소
    Enter. 기본설정
 
    번호선택(1~5):"
read NUM

case $NUM in
1)
gzip ${FILENAME} ${DIR}
;;
2)
bzip $FILENAME
;;
3)
compress $FILENAME
;;
4)
zip $FILENAME
;;
5)
;;
*)
gzip $FILENAME
;;
esac
```