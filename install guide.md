بناءً على تفضيلك لاستخدام Bash، إليك الخطوات المباشرة والنهائية لإعداد Dify على نظام Windows (باستخدام Git Bash أو WSL) بناءً على ملفات مشروعك:

fix the error with gemini is powerfull

1. إعداد Docker (Middleware)
Bash

cd docker
cp middleware.env.example middleware.env
docker compose -f docker-compose.yaml -f docker-compose.middleware.yaml up -d

2. إعداد الـ API (الخلفية)
Bash

cd ../api
cp .env.example .env and adjust some variables 
# هام: قم بتغيير القيمة في ملف .env يدوياً: DB_HOST=127.0.0.1

python -m venv venv
source venv/Scripts/activate  # استخدم bin/activate إذا كنت على WSL
pip install .

ثانياً: تشغيل الـ API (في مجلد api)
flask run --host=0.0.0.0 --port=5001 --debug
flask db upgrade


نافذة الـ Web:3
cd web
cp .env.example .env
pnpm install
pnpm run dev // if not working use >pnpm next dev

تحتاج دائماً إلى إبقاء 3 نوافذ مفتوحة:

1 Bash> cd api
source venv/Scripts/activate
flask run --host=0.0.0.0 --port=5001 --debug
Terminal 2 (الـ Worker):

2 Bash> cd api
source venv/Scripts/activate
celery -A app.celery worker -P gevent -c 1 -Q dataset,generation,mail,ops_trace,slow_fetch --loglevel INFO
Terminal 3 (الـ Web):

3 Bash> cd web
pnpm run dev
ملاحظة تقنية: تأكد دائماً من وجود المنافذ 5432, 5001, 3000 في حالة Forwarded داخل VS Code لضمان الاتصال السلس.

هل تود أن أقوم بكتابة ملف run.sh لتشغيل هذه النوافذ الثلاث بلمسة واحدة؟
