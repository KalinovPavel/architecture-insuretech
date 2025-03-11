http {

    # 1) Определяем зону для хранения данных о количестве запросов
    # rate=10r/m означает лимит 10 запросов в минуту
    # perminute:10m — это имя и размер памяти, где Nginx хранит статистику
    limit_req_zone $binary_remote_addr zone=perminute:10m rate=10r/m;

    # Настройка upstream для балансировки нагрузки
    upstream backend_servers {
        server backend1.example.com;
        server backend2.example.com;
        server backend3.example.com;
    }

    server {
        listen 80;

        location / {

            # 2) Применяем rate limiting
            # Указываем зону 'perminute', burst — допускаемая "очередь" 
            # nodelay — значит не «задерживать», а сразу отклонять при превышении
            limit_req zone=perminute burst=5 nodelay;

            # 3) Возвращать 429, если лимит превышен
            limit_req_status 429;

            proxy_pass http://backend_servers;
        }
    }
}
