FROM ubuntu

RUN apt-get update
# -y는 nginx을 다운로드 받을 때, 용량 문제로 질문을 해서 Yes로 대답하는 옵션
RUN apt-get install -y nginx

WORKDIR /var/www/html

COPY ./webapp/index.html ./index.nginx-debian.html

# daemon off;는 nginx을 백그라운드가 아닌 포그라운드로 실행 시킴, 이래야지 컨테이너가 안 죽음
ENTRYPOINT ["nginx", "-g", "daemon off;"]