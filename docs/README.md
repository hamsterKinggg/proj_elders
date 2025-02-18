# 初期架构设计
## SQL
考虑到并发需求选择mysql。
### user表：id(PK), device_id, device_model, last_connection

## server端
### server.py: 处理HTTP请求
    目前一句一句的发送请求以降低客户端嵌入式开发压力。考虑到并发需求所以使用faastAPI
### openai_api.py: 封装openai API
由于OpenAI API不具备记忆功能，为了模拟记忆对话历史的效果：
1. 根据每个用户ID，保存过去五句话的历史
2. 三句对话后提取一次关键词永久保存
3. 每次附带三句历史对话以及：“你是用户的长期助手，用户背景：健康状况…情感背景…个人偏好…”
4. 注意：为了防止GPT4本身提取关键词过慢、数据库要存储关键词太多的问题：
    - 用NLP预筛选，再让GPT确认，减轻关键词提取压力
    - 关键词加权，出现几次才添加
    - 关键词防止重复添加

## 关于requirements，这里再写一下
- requests
- socket编程（可能没用到）：socketio
- http相关：fastapi uvicorn sqlalchemy
- 数据库：mysql（brew）
- 语音识别： SpeechRecognition
- 语音合成： pyttsx3