리눅스에서 바로 되는 “정확한” 실행 방법 (백그라운드)
아래를 그대로 실행하세요.

cd /workspace/Delay_management_tool_0317_Original/DelayManagementTool
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
터미널/세션 1: Timing 서버 (5001)
nohup ./.venv/bin/streamlit run app.py \
  --server.address 0.0.0.0 \
  --server.port 5001 \
  > timing.log 2>&1 &
터미널/세션 2: Dashboard 서버 (5000)
핵심: TIMING_APP_URL을 127.0.0.1이 아닌 서버 IP로 지정

SERVER_IP=<서버의_실제_IP>   # 예: 10.10.10.25

nohup env \
  FLASK_SECRET_KEY='change-this-secret' \
  TIMING_APP_URL="http://${SERVER_IP}:5001" \
  ./.venv/bin/python app_unified.py \
  > dashboard.log 2>&1 &
바로 점검할 체크리스트
프로세스 확인

ps -ef | grep -E "streamlit run app.py|app_unified.py" | grep -v grep
로그 확인

tail -n 100 timing.log
tail -n 100 dashboard.log
브라우저 접속

http://<SERVER_IP>:5000

Timing 클릭 후 주소가 http://<SERVER_IP>:5001인지 확인

방화벽/보안그룹에서 5000, 5001 열려 있는지 확인
