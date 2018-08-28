# Kibana 설치하기

```bash
root@eafa6d164a7e:/# wget https://artifacts.elastic.co/downloads/kibana/kibana-6.4.0-amd64.deb
root@eafa6d164a7e:/# shasum -a 512 kibana-6.4.0-amd64.deb
root@eafa6d164a7e:/# sudo dpkg -i kibana-6.4.0-amd64.deb
```
`wget`으로 kibana 설치 파일을 다운로드 합니다.
> 다른 OS를 사용할 경우 [여기](https://www.elastic.co/guide/en/kibana/current/install.html)를 참고하세요.

```bash
root@5029351fa373:/# sudo update-rc.d kibana defaults 95 10
```
서버가 켜지거나 꺼지면 자동으로 kibana를 실행 또는 종료 시키도록 설정합니다.
- `sudo -i service kibana  start` 수동 실행
- `sudo -i service kibana  stop` 수동 종료

## 실행하기

우선 도커에 접속하기 위해 방화벽 설정을 해준다.
`sudo ufw status`
`