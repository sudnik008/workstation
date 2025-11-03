Нужно построить ssh-туннель до сервера

  

ssh -p 2002 spgai@45.89.227.47

пароль: Rjvveys26/4

  

И там развернут в докере postgres - порт 5432

Креды к постгресу:

  DB_USER: teleeng

  DB_PASS: teleeng

  DB_HOST: db

  DB_NAME: assistant

  DB_PORT: 5432

Подключись к базе

Далее тебе нужно две сущности DebugMessage, Log для получения информации о событиях / логах / технических событиях